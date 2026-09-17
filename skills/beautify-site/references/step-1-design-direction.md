# Step 1 — Design direction

Decide what the thing should look like, and write it down, before touching a single component.

**`ui-ux-pro-max` accelerates this step; it is not required.** If it is installed, run it (below). If it
is not, use the "Author it yourself" fallback at the bottom — the deliverable is identical either way:
a persisted `MASTER.md` that steps 2–4 read. Never block the run on a missing helper skill.

## Why this is first

Everything downstream reads the artifact this step produces. A component chosen without a design system is a guess; a component chosen against MASTER.md is a decision. Same for animation.

## What you need from the user

You usually only need three things, and you can often infer them from the codebase:

- **What it is** — product type and industry ("beauty spa booking site", "internal analytics dashboard").
- **Who it's for** — consumer, enterprise, technical.
- **Any hard constraints** — existing brand colors, a logo, a fixed font.

If the repo has a landing page, README, or `package.json`, read them and infer. Ask only what you genuinely cannot determine.

## Run the search

The skill lives at `~/.claude/skills/ui-ux-pro-max/`.

```bash
# On Windows use `python` — `python3` resolves to the Microsoft Store stub and fails.
cd ~/.claude/skills/ui-ux-pro-max
python scripts/search.py "<product_type> <industry> <keywords>" --design-system -p "<Project Name>"
```

Example:

```bash
python scripts/search.py "beauty spa wellness service" --design-system -p "Serenity Spa"
```

This returns a recommended **pattern** (page structure and conversion flow), **style** (visual language, light/dark support), **color tokens** with hex values, **typography**, and **accessibility risk flags**.

### Tune it when the default feels wrong

Three dials, each 1–10:

```bash
python scripts/search.py "<query>" --design-system --variance <1-10> --motion <1-10> --density <1-10>
```

- `--variance` — how far from convention.
- `--motion` — how animated the result should feel. **Carry this number into step 3.**
- `--density` — how much information per screen.

## Persist it — this is the handoff

Nothing downstream works without this. Always persist with an explicit project root:

```bash
python scripts/search.py "<query>" --design-system --persist -p "<Project Name>" --output-dir "<project-root>"
```

Writes:

- `design-system/<project-slug>/MASTER.md` — the Global Source of Truth: all design rules.
- `design-system/<project-slug>/pages/` — per-page overrides, created as needed.

For a page that legitimately deviates (a checkout that must be calmer than the marketing site):

```bash
python scripts/search.py "<query>" --design-system --persist -p "<Project>" --page "checkout" --output-dir "<project-root>"
```

Page rules override MASTER.md; MASTER.md governs everything else.

## Reuse, don't regenerate

Before running anything, look for an existing `design-system/*/MASTER.md`. A project has ONE direction. If it exists:

- Read it and build against it.
- Only add a `pages/<page>.md` override if this specific page genuinely differs.
- Regenerate the master **only** if the user explicitly asks for a new direction.

## Author it yourself — the fallback when ui-ux-pro-max is absent

No generator, same deliverable. Pick the direction deliberately, then write
`design-system/<project-slug>/MASTER.md` by hand with these sections:

- **Direction** — one sentence naming the aesthetic and what it is reacting against. Commit to ONE.
- **Palette** — surface, ink, muted ink, one accent, one secondary accent, plus a line/border colour.
  Tint your neutrals; never pure `#000` or `#fff`. Note which pairs are contrast-risky.
- **Type** — a display face and an interface face, the scale (clamp values), and the weights in play.
  Two families maximum, with a clean split of jobs.
- **Space** — the base unit and the section rhythm.
- **Motion level** — a number 1–10. **Carry it into step 3.**
- **Density** — how much information per screen.
- **Visual devices** — the list from step 1.5, named here. **Six minimum.** See below.
- **Rules that must not move** — reduced motion, focus ring, touch target, contrast floor.

Vary the direction every time. Reaching for the same palette and type pairing across projects is itself
an AI tell — see step 4.

## A direction is not finished until it names its THINGS

Palette, type and spacing describe how a page is *set*. They say nothing about what is actually
*on* it — and a MASTER.md that stops there reliably produces a beautifully typeset page the user
calls "too basic". This is the pipeline's most common outcome; do not let the direction leave
this step without an inventory.

So the last section of MASTER.md lists the **visual devices** this page will carry: the ambient
light, the textures, the reactive surfaces, the composition shapes, the figures.
[references/visual-density.md](visual-density.md) is the menu and the floor.

Write it as a commitment, not a wish:

```markdown
## Visual devices  (6 minimum — step 1.5)
1. Ambient light field — three warm lobes, honey/plum, blurred, slow drift
2. Grain over the light
3. Cursor spotlight on every card and tile
4. Bento grid, one tile inverted to near-black
5. Marquee, two strips counter-running
6. Counters that roll on reach
7. Card art — a made mark per card, never a paragraph in a box
```

**If the direction you chose seems to forbid this, you have misread it.** Minimal, editorial and
Swiss describe restraint in *composition* — few elements, enormous care. They have never meant
the absence of material. Apple is minimal and ships photography, depth and motion. If a project's
own design rules genuinely do forbid ornament, surface that conflict to the user rather than
silently shipping an empty page.

## Sibling skills worth pulling in — only if present

Optional accelerators. Check before invoking; skip silently if absent:

- `ui-styling` — shadcn/ui + Tailwind implementation, theming, dark mode.
- `design-system` — token architecture (primitive → semantic → component).
- `brand` — voice, identity, messaging, when the work is brand-level.
- `design` — logos, icons, corporate identity, social images.

## Before moving on

You have MASTER.md, you have read it, and you can state the chosen style, the color tokens, the type scale, and the motion level in one sentence. If you cannot, step 1 is not done — do not start step 2.

**And you can name six visual devices.** If the honest answer is "good typography and a lot of
whitespace", you have a direction but not a page. Go to step 1.5 and finish it — that gap is the
one this pipeline is worst at noticing on its own, because every other check will pass.
