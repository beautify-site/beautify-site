---
name: beautify-site
description: Use when the user wants a website, page, or UI to look better — "make this site look better", "beautify my landing page", "redesign this UI", "make it look professional", "this page looks bland/generic/cheap", "polish the design", "make it look expensive", "make it extravagant". Runs one ordered pipeline — pin a design direction, fit components to it, layer motion, then strip every AI tell in a mandatory final pass. Bold and extravagant by default. Fully self-contained — every helper tool is optional with a documented fallback. Produces a persisted design-system MASTER.md plus implemented, accessibility-checked, AI-tell-free UI.
---

# Beautify Site

Turn a bland or generic interface into a deliberate, coherent one — by deciding the design direction FIRST, then choosing components that fit it, adding motion, and finally stripping every AI tell.

## Ambition — bold by default

**Design extravagantly unless the user asked for restraint.** Safe and tasteful is the failure mode. Commit hard to one direction and execute it with precision — canvas/WebGL, scroll-linked motion, sticky-pinned sequences, cursor-reactive surfaces, staggered orchestration, custom cursors, live counters, grain, parallax depth. Vary the direction every single time; converging on a house style is its own AI tell.

Dial back ONLY when the user explicitly asks for minimal/quiet, or the project's own documented design system says so and the user has not overridden it. When a project's rules conflict with this default, **surface the conflict and let the user choose** — never silently pick restraint.

Bold is an aesthetic budget, not an accessibility one. Reduced motion, focus rings, touch targets, contrast, and no-horizontal-overflow are never traded away.

## When to use

- "Make this site/page/app look better" · "beautify my landing page" · "redesign this UI"
- "This looks generic / bland / cheap / AI-generated" · "make it look professional"
- Any request to improve the visual quality of an existing or new interface.

**Not for:** a single isolated tweak ("make this button blue"), a pure copy/wording fix, or a performance problem. Those are one-off edits — just do them.

## The order is the whole point

```
1.  DIRECTION   →  design-system/<slug>/MASTER.md  (ui-ux-pro-max if present, else by hand)
1.5 DENSITY     →  name >= 6 visual devices BEFORE building  (references/visual-density.md)
2.  COMPONENTS  →  chosen to FIT MASTER.md         (21st.dev if signed in, else hand-built)
3.  MOTION      →  layered on, honoring MASTER.md  (motion.dev docs — free, no auth)
4.  IMPECCABLE  →  every AI tell stripped from the BUILT artifact  (blocklist is inline)
```

**Never reorder these.** Picking a component before the design system exists is the exact failure this skill prevents: you get a page assembled from good-looking parts that do not belong together. MASTER.md is the contract — steps 2 and 3 both read it and conform to it.

Each step gates the next. If step 1 produces nothing usable, do not proceed to step 2.

## The failure this skill will most likely hand you

Not ugliness. **Emptiness.** A page where every individual choice is defensible — considered
palette, correct type scale, disciplined spacing, a clean impeccable pass — and the whole thing
still reads as a beautifully set README rather than a product page.

It happens because *taste* is what this pipeline optimises for, and taste is not density. The
user will not say "insufficient visual device count". They will say **"too basic"**, "too
clean", "too simplistic" — and they will be right, and no amount of re-tuning the palette will
answer it.

**Step 1.5 exists to stop that and it is mandatory.** Read
[references/visual-density.md](references/visual-density.md), count your devices, and name them
in the run *before* you build. Six is the floor. A direction called minimal, editorial, Swiss or
"exaggerated minimalism" does not lower it — those words describe restraint in *composition*,
never absence of *things*.

**Also learn the distinction it draws.** "Too bare" means either thin content or thin visuals,
and they have opposite fixes. Answering thin visuals with more paragraphs is the single most
reliable way to be told the same thing twice.

**Step 4 is not optional and never skipped** — not because the work "already looks good", not because time is short, not because the user seemed happy. A design that has not been through the impeccable pass is not finished.

## Preflight — check before you start, degrade with a clear next action

Run these checks first and tell the user what you found. Never die mid-run on a missing dependency.

**Nothing here is a hard dependency.** Every helper below is an accelerator with a documented fallback,
so this skill runs standalone on a fresh machine with no other skill installed.

| Check | How | If missing |
|---|---|---|
| `ui-ux-pro-max` (optional) | `ls ~/.claude/skills/ui-ux-pro-max/SKILL.md` | Author MASTER.md yourself — [step 1 "Author it yourself"](references/step-1-design-direction.md). Same deliverable, no blocker. Offer the install (`npx ui-ux-pro-max-cli@latest init --ai claude --global`) but do not wait on it. |
| `impeccable` (optional) | `ls ~/.claude/skills/impeccable/SKILL.md` | Step 4's own blocklist is the complete standard — [step 4](references/step-4-impeccable.md). The pass runs at full strength either way. |
| Python (only for ui-ux-pro-max) | `python --version` (**not** `python3` on Windows — that hits the Store stub) | Use the step 1 fallback. Never install Python yourself. |
| 21st CLI installed + signed in | `21st whoami` | `npm i -g @21st-dev/cli`, then ask the user to run `21st login` (interactive browser sign-in — never run it unattended). Step 2 degrades to hand-building — see [references/step-2-components.md](references/step-2-components.md). |
| Target stack | Read `package.json` / config | React + Tailwind = full support. Otherwise see "Non-React stacks". |
| Existing design system | Look for `design-system/*/MASTER.md` | Reuse it instead of regenerating — a project has ONE direction. |

State plainly which steps will run at full strength and which will degrade, then proceed with what works.
**A missing helper never stops the run** — it only changes how a step is performed.

## Step 1 — Design direction (required)

Establish the visual direction and persist it. This is the only step that is never optional.

→ [references/step-1-design-direction.md](references/step-1-design-direction.md)

## Step 1.5 — Visual density (required, and the one most often skipped)

Count the made things your page will contain, before you build any of them. Under six it will
read as basic however good the typography is.

→ [references/visual-density.md](references/visual-density.md)

## Step 2 — Components and templates (free-first)

Find the best-fitting **free** component or template and adapt it to MASTER.md. Search is free; installs are rate-limited; AI generation costs money — never spend without asking.

→ [references/step-2-components.md](references/step-2-components.md)

## Step 3 — Motion (last)

Add purposeful animation using Motion, pulling current docs from `https://motion.dev/llms.txt`. Free, no auth.

→ [references/step-3-motion.md](references/step-3-motion.md)

## Step 4 — Impeccable pass (MANDATORY — strip every AI tell)

The design is built. Now prove it does not read as machine-made. Audit the **built artifact** against the `impeccable` skill's bans and remove every hit by restructuring — never by softening.

The two hard bans that fail the pass outright: **gradient text** (`background-clip: text` over any gradient) and **side-stripe accents** (`border-left`/`border-right` > 1px on a card, list item, callout, or alert).

→ [references/step-4-impeccable.md](references/step-4-impeccable.md)

## Non-React stacks

Say this plainly rather than half-delivering:

- **React + Tailwind** — all three steps at full strength.
- **Vue / Svelte / Astro / plain JS** — steps 1 and 3 work fully (Motion supports JS and Vue). Step 2 degrades: 21st.dev is React/Tailwind-centric, so use its results as *design reference* and hand-build the component to MASTER.md instead of installing.
- **Native (SwiftUI, Flutter, Compose)** — step 1 only; ui-ux-pro-max has stack guidance for these. Skip 21st.dev and Motion; use the platform's own animation system.

## Verify before calling it done

Not optional. Check and report each:

1. **Matches MASTER.md** — colors, type, spacing, and density actually follow the persisted system. Any deviation is deliberate and stated.
2. **Device count** — list the visual devices you actually shipped. Under six, go back to step 1.5 rather than to the user.
3. **Contrast** — text meets 4.5:1 (3:1 for large text). MASTER.md flags the risky combinations; check them. **Measure it through a canvas, not by parsing `getComputedStyle`** — modern browsers return `oklch()` verbatim, and reading those three numbers as RGB scores every pair at ~1.0, so a perfect palette reports as a total failure. Sanity-check your ruler on black-on-white first; it must come back 21.
4. **Keyboard** — every interactive element is reachable and has a visible focus state.
5. **Reduced motion** — every animation is disabled or reduced under `prefers-reduced-motion`.
6. **It actually renders** — build or run it, then **screenshot it and read the image**. A passing test suite is not evidence that a page renders correctly; text-based checks cannot see a visual bug.
7. **No horizontal overflow** at 390px, and **nothing hidden while it is on screen** — the property to assert is not "every reveal fired" but "nothing in the viewport is invisible once its animation settles". Check it in the worst case, not the happy path.
8. **Scroll cost** — see step 3. No section may cost multiple screens of scrolling for one idea.
9. **Real input** — if you touched scrolling or pointer behaviour, exercise it with real wheel and pointer events. `window.scrollTo` bypasses a wheel handler completely and will pass a suite over code that is totally broken.
10. **The impeccable pass ran** — name the tells found and removed, or state plainly that the audit came back clean and what you checked.

Report what you verified with evidence, not "should work".

## Do NOT

- **Do not pick a component before MASTER.md exists.** That is the failure mode this skill exists to prevent.
- **Do not spend money silently.** `21st generate` needs paid credits — always ask first, and prefer free search-and-adapt with `--free`.
- **Do not run `21st login` yourself** — it opens a browser for the human to sign in. Ask them.
- **Do not force a bad match.** If nothing free fits, say so and compose from the design system instead.
- **Do not use `python3` on Windows** — it resolves to the Microsoft Store stub and fails. Use `python`.
- **Do not abort because a helper skill is missing.** Every one has a fallback — use it and say which path you took.
- **Do not use `llms.motion.dev`** — it does not resolve. The docs index is `https://motion.dev/llms.txt`.
- **Do not install software on the user's machine** (Python, package managers). Ask them.
- **Do not print, echo, or hardcode the 21st.dev API key.**
- **Do not regenerate a design system** a project already has — reuse and extend it.
- **Do not animate for its own sake.** Motion serves comprehension; noise is a downgrade.
- **Do not add AI attribution** to any file, comment, or commit.
- **Do not skip step 4** — not because the design looks good, not because the user seemed happy, not because time is short.
- **Do not ship gradient text or a side-stripe accent.** They are the two hard bans and they fail the pass outright.
- **Do not soften a banned pattern instead of restructuring it.** A subtler gradient is still gradient text.
- **Do not default to restraint** because a project's design docs are cautious — surface the conflict and let the user decide.
- **Do not confuse "no AI tells" with "no personality."** Strip the templated, keep the bold.
- **Do not ship a page made only of type, rules and whitespace.** That is the emptiness failure above, and it is this pipeline's most likely output. Six devices, minimum.
- **Do not treat a section as done if it is a heading, a paragraph and a rule.** Give it a made thing or fold it into its neighbour.
- **Do not hijack the wheel.** Never `preventDefault` a wheel event to ease the document toward your own scroll target. Measured: a trackpad flick of 1200px over 640ms moved such a page **102px** — it ate 92% of the input — and it also puts a hand-written loop in front of momentum, keyboard paging, find-in-page, the scrollbar and anchor jumps. Weight is not worth breaking scrolling for; observe velocity from the native scroll position instead.
- **Do not pin a section without pricing it.** A `position: sticky` stage that costs 600px of scroll per idea reads as a frozen page, not as design. See step 3's scroll budget.
- **Do not animate a transform to the keyword `none`.** `none` carries no `scale`, and scale is the one transform function whose identity is 1 rather than 0 — so `scale(0.98) → none` resolves to `scale(0)` and the element animates itself to literally zero size while still occupying layout and reporting `opacity: 1`. Name every end state explicitly and mirror the function list you started from.
- **Do not verify motion by whether it fired.** Verify what it costs the reader: how far they scroll, how long they wait, whether anything is invisible while on screen.
