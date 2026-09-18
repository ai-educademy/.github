# The AI Educademy agent fleet

This repo (`ai-educademy/.github`) runs the org-level command layer for the
whole agent fleet: the "manager of managers". Each product repo has its own fleet
of agents doing the work (a `pr-doctor`, a `bug-sweeper`, content, SEO,
accessibility, test and security agents, and a per-repo `fleet-manager.yml` that
auto-merges their PRs when CI is green). This layer sits above all of that. It
watches the whole fleet, audits its configuration, and holds the only cross-repo
merge authority.

If you are reading this because something is going wrong right now, start here.

## Stop everything: the kill switch

There is one lever that stands the entire org fleet down.

Set the **repository variable** `FLEET_AUTOMERGE` to `off` in this `.github`
repo:

```
gh variable set FLEET_AUTOMERGE --body off --repo ai-educademy/.github
```

While it is `off`, `org-fleet-manager.yml` merges nothing on any repo. It is the
first thing that workflow checks, and it exits cleanly with a note in the run
summary. To re-enable, set it to anything else or delete it:

```
gh variable set FLEET_AUTOMERGE --body on --repo ai-educademy/.github
# or
gh variable delete FLEET_AUTOMERGE --repo ai-educademy/.github
```

This only stops automated merging. It does not stop the agents from running and
opening issues or PRs. To silence an individual agent, disable its workflow in
the repo it lives in (`gh workflow disable <name> --repo ai-educademy/<repo>`),
or set its schedule off. The Fleet Chief will notice and report a silent agent,
so expect a note in the daily report if you do this.

## What runs here

| Workflow | Type | Schedule | Cost cap | What it does |
|---|---|---|---|---|
| `fleet-chief.md` | Agentic (Copilot) | Daily | 8000 credits/day | Surveys all five repos and reports fleet health: failing agents, stuck PRs, issue backlog, cost, consistency drift, guardrail integrity, and the Stripe payment path. Reads and reasons only. Opens one consolidated issue labelled `fleet-health`, plus separate `URGENT` issues for genuinely urgent findings. |
| `fleet-auditor.md` | Agentic (Copilot) | Weekly (Monday) | 8000 credits/day | Audits the fleet's own configuration across all five repos: over-broad permissions, missing caps or timeouts, `strict` off, unpinned actions, markdown and lock files out of step, unknown secrets, wide network allowlists. Opens one issue per finding, labelled `fleet-audit`. |
| `org-fleet-manager.yml` | Plain YAML | Every 30 min | n/a | The actual cross-repo merge authority. Merges genuinely-ready agent PRs across all five repos, within strict guardrails and merge caps. Never a model. |
| `fleet-dispatch.yml` | Plain YAML | Manual only | n/a | Lets the owner fire any named agent at any named repo on demand, for example pointing `pr-doctor` at one stuck PR. |

The Fleet Chief and Fleet Auditor have judgement but no authority: they cannot
merge, approve, push or edit code anywhere. Authority lives only in
`org-fleet-manager.yml`, which has no judgement: it is deliberately dumb,
auditable shell logic.

## Why merge authority is plain YAML and never a model

Merging is the one action that spends real money (the `ai-platform` repo is the
revenue path) and that can widen the fleet's own privileges (a merged change to
`.github/workflows/` changes what every agent is allowed to do). A language model
can be prompt-injected through a PR title or a diff, and can convince itself a
change is "probably fine". So the merge decision is made by boring shell logic a
human can audit line by line. The agentic workflows have no merge safe-output at
all, by design. They structurally cannot merge.

If you are ever tempted to make the merge logic "smarter" by handing it to an
agent: don't. The whole point is that it cannot be talked into anything.

## The merge guardrails

`org-fleet-manager.yml` will only merge a PR when every one of these holds. Each
skip is logged with its exact reason to the run summary, which is the audit trail
for automated merges.

1. **Kill switch off.** `FLEET_AUTOMERGE != off`.
2. **Author is a trusted bot**, one of `github-actions[bot]`, `app/github-actions`,
   `dependabot[bot]`, `app/dependabot`. Never a human's PR, however green.
3. **Does not touch `.github/workflows/`** in any repo. An agent must not be able
   to widen its own permissions or disable its own guardrails through a PR. Such
   PRs are labelled `needs-human` and left for a person.
4. **Does not touch `src/app/api/stripe/`** in `ai-platform`. That is the payment
   path, and a wrong merge costs real money. Labelled `needs-human`.
5. **Not a draft.**
6. **No hold labels**: `do-not-merge`, `wip`, `needs-human`.
7. **Review decision is not `CHANGES_REQUESTED`.**
8. **Git mergeable state is `MERGEABLE`** (no conflicts).
9. **Checks are genuinely green**: zero failing, zero pending, and at least one
   check has reported. Zero checks reported is treated as "not ready", never as
   "nothing failed".
10. **Required checks present and green by name.** In `ai-platform` those are
    exactly `build` and `smoke-test` (matched tolerantly, so `CI / build` and
    `CI/build (pull_request)` both count). `e2e-tests` also runs but is not
    required.

There are also merge caps: at most 5 merges per repo and 10 in total per run, so
a haywire fleet can only do bounded damage between runs. A `concurrency` group
stops overlapping runs.

## The cross-repo token: FLEET_PAT

`GITHUB_TOKEN` is repo-scoped and cannot reach sibling repos. Anything that works
across all five repos needs a **fine-grained Personal Access Token** (or a GitHub
App token) stored as the secret **`FLEET_PAT`** in this `.github` repo. The
existing `SUBMODULE_PAT` in `ai-platform` follows the same pattern.

Every workflow here fails loudly with an actionable message if `FLEET_PAT` is
absent, rather than half-working in silence.

### Creating FLEET_PAT (owner, one-time manual step)

Create a **fine-grained PAT** at
`https://github.com/settings/personal-access-tokens/new`:

- **Resource owner**: the `ai-educademy` organisation.
- **Repository access**: select all five repos (`ai-platform`, `ai-ui-library`,
  `ai-courses`, `ai-courses-pro`, `.github`), or "All repositories" in the org.
- **Repository permissions**:
  - **Pull requests: Read and write** (so `org-fleet-manager` can merge, and
    `fleet-chief` can read PRs).
  - **Contents: Read-only** (reading files and workflow contents; merging a PR
    uses the pull-requests permission, not contents write).
  - **Actions: Read and write** (read for run history in `fleet-chief`; write so
    `fleet-dispatch` can trigger workflows in sibling repos).
  - **Issues: Read-only** (the agents open issues in `.github` via their own
    scoped token; they only need to read issues elsewhere).
  - **Metadata: Read-only** (mandatory, added automatically).
- **Organisation permissions**: none required.

Then store it as the secret:

```
gh secret set FLEET_PAT --repo ai-educademy/.github
# paste the token when prompted
```

Set a calendar reminder for the token's expiry. When it lapses, every cross-repo
workflow here will fail loudly, which is the intended behaviour, but you will
need to rotate the token to restore the fleet.

## Firing an agent on demand

Use `fleet-dispatch.yml` from the Actions tab (or the CLI). Pick the repo, give
the workflow file name (for example `pr-doctor.lock.yml`), and optionally a PR
number to pass through:

```
gh workflow run fleet-dispatch.yml --repo ai-educademy/.github \
  -f repo=ai-platform -f workflow=pr-doctor.lock.yml -f pr=42
```

## Adding a new agent to this layer

1. Write a new `*.md` agentic workflow in `.github/workflows/`, using the same
   frontmatter shape as `fleet-chief.md`: `engine: copilot` with
   `copilot-requests: write` (no API key needed), a `max-daily-ai-credits` cap,
   `timeout-minutes`, `strict: true`, a `network.allowed` list, and a
   `FLEET_PAT` preflight step if it needs to reach other repos.
2. Keep permissions as narrow as it needs. The Fleet Auditor will flag you if you
   over-grant.
3. Compile with `gh aw compile`. Commit both the `.md` and the generated
   `.lock.yml`, plus `.github/aw/` and `.gitattributes`.
4. Never give an agent a merge safe-output. Merge authority stays in
   `org-fleet-manager.yml` alone.
5. Document it in the table above.

## British English and house style

Everything here is written in British English. There are no em dashes or en
dashes anywhere in the fleet, on purpose.
