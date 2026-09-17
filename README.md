<div align="center">

# Beautify Site

**Make any website look professionally designed: bold, coherent, and free of the tell-tale signs of AI-generated design.**

A skill for [Claude Code](https://code.claude.com/docs/en/skills) that redesigns a page the way a designer would. It picks a direction first, fits components to it, adds motion, then audits the result.

[![Made in Omniscio](https://img.shields.io/badge/made_in-Omniscio-06070B.svg)](https://omniscio.com)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code skill](https://img.shields.io/badge/Claude_Code-skill-D97757.svg)](https://code.claude.com/docs/en/skills)
[![Claude Code plugin](https://img.shields.io/badge/Claude_Code-plugin-D97757.svg)](https://code.claude.com/docs/en/plugins)
[![React + Tailwind](https://img.shields.io/badge/React_%2B_Tailwind-full_support-38B2AC.svg)](#works-with)

</div>

---

> **Made in [Omniscio](https://omniscio.com).** Beautify Site was built in Omniscio, a desktop app for managing many AI coding sessions from one place, and it comes included with the app.

## Use it in Omniscio

Beautify Site is built into Omniscio 0.1.105 and later, so there's nothing extra to install.

1. **Get Omniscio.** Download it from [omniscio.com](https://omniscio.com).
2. **Turn the skill on.** Open **Settings → Features** and switch on **Enable Beautify Site skill**. It's off by default.
3. **Restart Omniscio.** The skill is added the next time the app starts.
4. **Ask for it.** In any session, ask for a better-looking page in plain words, like *"make this landing page look better"*. There's no button to press. The skill starts on its own.

Not using Omniscio? [Install it in Claude Code](#getting-started) instead.

## Overview

Ask an AI to make a page look better and you usually get one of two results. Either the page is stitched together from nice-looking parts that don't belong together, or it's tasteful but empty and still reads as generic.

Beautify Site prevents both by enforcing the order a designer works in. It settles the design direction first and saves it as a design system. Components and motion then have to follow that system. The run ends with a mandatory audit that removes the patterns people now recognize as machine-made.

Once it's installed, just ask Claude Code for a better-looking page:

- "Make this landing page look better"
- "This site looks generic. Make it look expensive."
- "Redesign this UI"

## Features

- **Direction before components.** Settles one visual direction and saves it as `design-system/<slug>/MASTER.md`, the contract every later step follows.
- **A visual-density gate.** Names at least six visual devices before building anything, so the page never reads as "too basic".
- **Components that fit.** Searches [21st.dev](https://21st.dev) for free components and templates, then restyles them to the design system, or builds them by hand.
- **Purposeful motion.** Adds animation with [Motion](https://motion.dev), within a scroll budget and with full reduced-motion support.
- **A mandatory AI-tell pass.** Audits the built page and removes giveaways such as gradient text, side-stripe accents, eyebrow labels and fake terminal mockups.
- **Bold by default.** Designs extravagantly unless you ask for something minimal, and never trades away accessibility to do it.
- **Verified before it's called done.** Checks contrast, keyboard access, reduced motion and mobile overflow, then screenshots the rendered page.
- **Runs anywhere.** Every helper tool is optional and has a built-in fallback, so it works on a fresh machine.
- **Never spends money silently.** Paid AI generation always needs your explicit approval.

## How it works

| Step | What happens | Result |
|---|---|---|
| **1. Direction** | Settles on one bold visual direction | `design-system/<slug>/MASTER.md` |
| **1.5 Density** | Names at least six visual devices before building | A device list the build must deliver |
| **2. Components** | Finds free components that fit the direction and adapts them | Components restyled to the design system |
| **3. Motion** | Adds purposeful animation and prices every pinned section in scroll distance | Motion that follows the design system |
| **4. Impeccable pass** | Audits the built page and strips every AI tell | A clean, verified page |

The steps never run out of order. Choosing components before the design system exists is exactly the failure this skill is built to prevent.

## Works with

| Stack | Support |
|---|---|
| React + Tailwind | Every step at full strength |
| Vue, Svelte, Astro, plain JS | Direction and motion at full strength. Components are hand-built, using 21st.dev as a reference. |
| SwiftUI, Flutter, Compose | Design direction only. Use the platform's own animation system. |

## Optional helpers

Nothing here is required. If a helper is missing, the skill tells you and uses its fallback.

| Helper | Speeds up | Fallback |
|---|---|---|
| [ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) skill (needs Python) | Choosing the design direction | Writes the design system itself |
| [Impeccable](https://github.com/pbakaus/impeccable) skill | The final AI-tell audit | Uses its own built-in blocklist |
| [21st.dev](https://21st.dev) CLI, signed in | Finding components | Builds components by hand |

Motion's documentation is free and needs no account.

## Getting started

These steps are for Claude Code on its own. In Omniscio, just [switch it on](#use-it-in-omniscio).

### Prerequisites

- [Claude Code](https://code.claude.com/docs/en/skills)
- A website or app to improve, or a new one to design

### Install as a plugin (recommended)

In Claude Code, run:

```text
/plugin marketplace add beautify-site/beautify-site
/plugin install beautify-site@beautify-site
```

Or from your terminal:

```bash
claude plugin marketplace add beautify-site/beautify-site
claude plugin install beautify-site@beautify-site
```

### Install manually

Copy the skill folder into your personal skills folder.

**macOS / Linux**

```bash
git clone https://github.com/beautify-site/beautify-site.git
mkdir -p ~/.claude/skills
cp -r beautify-site/skills/beautify-site ~/.claude/skills/
```

**Windows (PowerShell)**

```powershell
git clone https://github.com/beautify-site/beautify-site.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse beautify-site\skills\beautify-site "$HOME\.claude\skills\"
```

Start a new Claude Code session and the skill is ready.

### Use it

Open your project in Claude Code and ask for a better-looking page. The skill will:

1. Tell you which steps will run at full strength and which will use a fallback.
2. Ask what you're going for, then write the design system.
3. Build, animate, audit and verify the page, then report what it checked.

## Project structure

```text
beautify-site/
├── .claude-plugin/
│   ├── marketplace.json          # lets Claude Code install this repo as a plugin
│   └── plugin.json               # plugin details
├── skills/
│   └── beautify-site/
│       ├── SKILL.md              # the pipeline, preflight checks, verification and rules
│       └── references/
│           ├── step-1-design-direction.md
│           ├── visual-density.md
│           ├── step-2-components.md
│           ├── step-3-motion.md
│           └── step-4-impeccable.md
├── LICENSE
└── README.md
```

## License

[MIT](LICENSE) © 2026 Beautify Site contributors
