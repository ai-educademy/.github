<div align="center">

# 🎓 AI Educademy

### Multilingual AI and software engineering education, from absolute zero to interview ready

[![Website](https://img.shields.io/badge/Website-aieducademy.org-6366f1?style=for-the-badge&logo=vercel&logoColor=white)](https://aieducademy.org)
[![npm](https://img.shields.io/npm/v/@ai-educademy/ai-ui-library?style=for-the-badge&color=f59e0b&label=UI%20Library)](https://www.npmjs.com/package/@ai-educademy/ai-ui-library)
[![License](https://img.shields.io/badge/License-MIT-10b981?style=for-the-badge)](https://github.com/ai-educademy/ai-platform/blob/main/LICENSE)
[![Stars](https://img.shields.io/github/stars/ai-educademy?style=for-the-badge&color=ff9f0a)](https://github.com/ai-educademy)

---

**Learn AI from zero. No coding, no maths, no jargon required to start.**

🌍 Every lesson available in **11 languages**: English · Français · Deutsch · Español · Português · Nederlands · हिन्दी · తెలుగు · 日本語 · 中文 · العربية

</div>

## 🌱 Our Mission

AI education should be **accessible** and **available in your language**. Too many learners, especially in the Global South, are left behind because quality AI resources are English-only or assume prior technical knowledge.

AI Educademy starts from the very basics and builds understanding step by step, using everyday language and real-world analogies. Every programme opens with a free lesson so you can judge the teaching before you pay anything.

## 🗺️ Learning Tracks

Fifteen programmes across three tracks, each running from level 1 to level 5.

### 🌳 Understanding AI
```
🌱 AI Seeds       →  Absolute beginners, zero experience
🌿 AI Sprouts     →  Foundations and core concepts
🌳 AI Branches    →  Applied AI and real-world use cases
🏕️ AI Canopy      →  Advanced topics and specialisation
🌲 AI Forest      →  Expert level, research and contribution
```

### 🔨 Craft and Engineering
```
✏️ AI Sketch       →  Data structures and problem solving basics
🪨 AI Chisel       →  Intermediate algorithms and patterns
🔨 AI Craft        →  System design and architecture
💎 AI Polish       →  Optimisation and advanced techniques
🏆 AI Masterpiece  →  Interview-ready mastery
```

### 🎯 Career Ready
```
🚀 Interview Launchpad   →  How hiring actually works, and how to prepare
🗣️ Behavioral Mastery    →  Stories, structure and signal
⚙️ Technical Interviews  →  Coding rounds and live problem solving
🤖 AI & ML Interviews    →  ML system design and applied AI rounds
🏁 Offer & Beyond        →  Negotiation, levelling and the first 90 days
```

## 💷 Pricing

The first lesson of every programme is free, in every language. Full access to all fifteen programmes is **£3.99/month** or **£29.99/year**.

Subscriptions pay for hosting, translation and new content. They are the reason this can exist without ads or selling anyone's data.

## 📦 Repository Map

| Repo | Role | Description |
|------|------|-------------|
| [`ai-platform`](https://github.com/ai-educademy/ai-platform) | 🌐 App Shell | Next.js 16, React 19, i18n, auth, payments, routing. The deployed site |
| [`ai-courses`](https://github.com/ai-educademy/ai-courses) | 📚 Free Content | MDX lessons for the free programmes and the blog, in all 11 languages |
| `ai-courses-pro` | 🔒 Premium Content | Subscriber lesson content. Private |
| [`ai-ui-library`](https://github.com/ai-educademy/ai-ui-library) | 🎨 Design System | Shared components on [npm](https://www.npmjs.com/package/@ai-educademy/ai-ui-library). Button, Card, ThemeToggle, animations |

### Architecture

```
┌────────────────────────────────────────────────────────┐
│                     ai-platform                         │
│            Next.js 16 app shell (Vercel)                │
│   routing · auth · i18n · payments · program registry   │
├──────────────────┬─────────────────────────────────────┤
│  ai-ui-library   │   ai-courses  +  ai-courses-pro      │
│  (npm package)   │  ┌───────────────────────────────┐   │
│                  │  │  Understanding AI              │   │
│  Button · Card   │  │  🌱 Seeds → 🌿 Sprouts →      │   │
│  Badge · Theme   │  │  🌳 Branches → 🏕️ Canopy →    │   │
│  Animations      │  │  🌲 Forest                     │   │
│                  │  ├───────────────────────────────┤   │
│                  │  │  Craft & Engineering           │   │
│                  │  │  ✏️ Sketch → 🪨 Chisel →       │   │
│                  │  │  🔨 Craft → 💎 Polish →        │   │
│                  │  │  🏆 Masterpiece                │   │
│                  │  ├───────────────────────────────┤   │
│                  │  │  Career Ready                  │   │
│                  │  │  🚀 Launchpad → 🗣️ Behavioral →│   │
│                  │  │  ⚙️ Technical → 🤖 AI/ML →     │   │
│                  │  │  🏁 Offer & Beyond             │   │
│                  │  └───────────────────────────────┘   │
└──────────────────┴─────────────────────────────────────┘
```

## 🚀 Get Started

Visit **[aieducademy.org](https://aieducademy.org)** and take the first lesson free. It takes 10 minutes.

## 🤝 Contributing

We welcome contributions from everyone:

- 🌍 **Improve a translation**. Lessons marked `machineTranslated` need a native-speaker pass
- ✍️ **Write lessons**. Share your AI or coding knowledge
- 🧩 **Build components**. Contribute to the UI library
- 💻 **Code**. Features, fixes, new programmes
- 📣 **Share**. Tell your friends, classmates and communities

## 🤖 Automation

This org runs an agent fleet. Each repo has its own agents, and the
[`ai-educademy/.github`](https://github.com/ai-educademy/.github) repo runs the
org-level command layer above them: a daily Fleet Chief that reports fleet
health, a weekly Fleet Auditor that checks the agents' own configuration, and a
plain, auditable cross-repo merge authority. Read [FLEET.md](FLEET.md) for what
each agent does, how the guardrails work, and where the kill switch is.

## 👨‍💻 Built By

[**@rameshreddy-adutla**](https://github.com/rameshreddy-adutla) · Tech Lead, 14 years experience · London

---

<div align="center">

**⭐ Star our repos if you believe AI education should reach everyone, in their own language**

</div>
