<div align="center">

# AI Educademy

### Multilingual AI and software engineering education, from absolute zero to interview ready

[![Website](https://img.shields.io/badge/Website-aieducademy.org-6366f1?style=for-the-badge&logo=vercel&logoColor=white)](https://aieducademy.org)
[![npm](https://img.shields.io/npm/v/@ai-educademy/ai-ui-library?style=for-the-badge&color=f59e0b&label=UI%20Library)](https://www.npmjs.com/package/@ai-educademy/ai-ui-library)
[![Licence](https://img.shields.io/badge/Licence-MIT-10b981?style=for-the-badge)](https://github.com/ai-educademy/ai-platform/blob/main/LICENSE)

**Learn AI from zero. No coding, maths, or jargon required to start.**

Every lesson is available in 11 languages: English, French, German, Spanish, Portuguese, Dutch, Hindi, Telugu, Japanese, Chinese, and Arabic.

</div>

## Mission

AI education should be accessible, practical, and available in the learner's own language. AI Educademy starts from the basics and builds towards production AI, engineering craft, and career readiness.

Every programme opens with a free first lesson so learners can judge the teaching before paying. Full access is available through Pro.

## Learning tracks

Fifteen programmes are grouped into three tracks:

| Track | Programmes |
|-------|------------|
| Understanding AI | AI Seeds, AI Sprouts, AI Branches, AI Canopy, AI Forest |
| Craft and Engineering | AI Sketch, AI Chisel, AI Craft, AI Polish, AI Masterpiece |
| Career Ready | Interview Launchpad, Behavioural Mastery, Technical Interviews, AI and ML Interviews, Offer and Beyond |

## Pricing

The first lesson of every programme is free, in every language. Full access to all fifteen programmes is available through Pro:

| Plan | Price |
|------|-------|
| Monthly | £3.99/month |
| Annual | £29.99/year |
| Lifetime | £49.99 |

Subscriptions pay for hosting, translation, and new content. They keep the product advert free.

## Repository map

| Repo | Role | Description |
|------|------|-------------|
| [`ai-platform`](https://github.com/ai-educademy/ai-platform) | App shell | Next.js 16, React 19, i18n, auth, payments, routing, and deployment for [aieducademy.org](https://aieducademy.org) |
| [`ai-courses`](https://github.com/ai-educademy/ai-courses) | Public content | Free first lessons and the public blog in 11 languages |
| `ai-courses-pro` | Pro content | Private subscriber lessons for paid plans |
| [`ai-ui-library`](https://github.com/ai-educademy/ai-ui-library) | Design system | Shared React components on [npm](https://www.npmjs.com/package/@ai-educademy/ai-ui-library) |

## Architecture

```text
ai-platform
  Next.js app shell, auth, payments, i18n, programme registry
  |
  +-- ai-ui-library
  |     Shared React components and design tokens
  |
  +-- ai-courses
  |     Free first lessons and blog content
  |
  +-- ai-courses-pro
        Private subscriber lessons
```

## Get started

Visit [aieducademy.org](https://aieducademy.org) and take the first lesson free. It takes about 10 minutes.

## Contributing

We welcome focused contributions:

- Improve a translation
- Fix a typo or explanation
- Add a practical example
- Improve accessibility
- Build a reusable UI component
- Fix product defects in the platform

## Automation

This org runs an agent fleet. Each repo owns its local agents, and [`ai-educademy/.github`](https://github.com/ai-educademy/.github) owns the org level command layer. Read [FLEET.md](FLEET.md) for the fleet roles, guardrails, and kill switch.

## Built by

[@rameshreddy-adutla](https://github.com/rameshreddy-adutla)

<div align="center">

**Star the repos if you believe AI education should reach everyone, in their own language.**

</div>
