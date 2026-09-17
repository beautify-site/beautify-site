# Step 2 — Components and templates (21st.dev)

Find the best-fitting **free** component or template and adapt it to MASTER.md.

## Read MASTER.md first

Before searching, you must be able to state: the style, the primary/surface colors, the type pairing, and the density. You search *for those*, not for "a nice hero".

A component chosen without this is a guess. That is the whole reason this step is second.

## Setup — the CLI, not the MCP

The `21st` CLI is the supported path. It is one global install plus a browser login; there is no API key to copy or store.

```bash
npm i -g @21st-dev/cli
21st login          # opens the browser, saves the token locally
```

- `21st login` is **interactive** — it opens a browser for the human to sign in. Never try to run it unattended; ask the user to run it and wait.
- For scripts/CI only, the token can come from the `TWENTYFIRST_TOKEN` environment variable instead.
- Verify with `21st whoami`. If it prints "Not logged in", stop and ask the user to run `21st login` — do not try to work around it.

**Search requires login.** Only `21st logo` works signed-out. If the user is not signed in, say so plainly and fall back to composing from the design system (see below) rather than stalling.

## Free-tier discipline — this matters

Check the budget before spending it:

```bash
21st usage          # account tier + remaining free quota
```

Exactly ONE thing is metered on the free tier: **`21st get` — component code, 2 per day.**
Everything else is uncapped. Verified by running each command with the counter at 0/2.

| Capability | Cost | Use it? |
|---|---|---|
| `21st search` (any `--type`) | **Free, uncapped** | Yes — your primary tool. |
| `21st get <id>` | Free but **capped 2/day** — this is THE meter | Sparingly. Spend it on the single best match. |
| `21st theme <id>` | **Free, uncapped** — works at 0/2 | Yes — seeds MASTER.md tokens. |
| `21st logo` | **Free, uncapped**, no login | Yes, when a logo is needed. |
| Templates | **Free, uncapped** — not served by `get` at all | Yes — see below. |
| `21st generate` | **Paid AI credits** | Only with explicit, per-use user approval. |

**Never run `21st generate` without asking.** The brief is a *free* component. Search-and-adapt is nearly always enough: take a free component and restyle it to MASTER.md rather than paying to generate one that would still need restyling.

### When the user wants MULTIPLE sections, reach for a template

Three sections via `21st get` = two days of quota. A template is a whole multi-section page
(hero + benefits + CTA) for **zero** quota, and it arrives coherent rather than assembled.

```bash
21st search "landing page" --type template --free --limit 12 --json
```

`21st get <template-id>` **does not work** — it answers `No 21st component found for id <n>`
because templates live in a separate id space. Each JSON row carries `price: 0` and a `url`
to its 21st page; the code is fetched from there (typically the author's public repo), which
is why it never touches the component meter.

If the 2/day meter is spent and the user needs sections, say so and offer the template lane —
do not stall, and do not offer to pay.

## The search loop

```bash
# Search by design intent, not by name. --free enforces the brief.
21st search "minimal editorial pricing table light" --type c --free --limit 5

# --type c = component · theme · template
21st search "saas landing" --type template --free --limit 5

# Read the real code before you commit to a candidate
21st get <id>
```

1. **Query with the style and purpose from MASTER.md** — "minimal editorial pricing table light mode", not "pricing".
2. **Always pass `--free`.** The brief says free; the flag enforces it rather than relying on judgement.
3. **Shortlist 2–3 candidates**, then `21st get <id>` each. Judge against MASTER.md: does its visual language match the chosen style, or fight it?
4. **Prefer a `template`** when rebuilding a whole page — it gives coherent structure. Prefer a `component` when slotting into an existing layout.
5. **Adapt, always.** A pulled component carries its own colors, radii, and spacing. Rewrite those to MASTER.md's tokens. An unadapted component is exactly why sites look assembled from parts.
6. **Keep the accessibility.** These are usually Radix-based — do not strip ARIA, focus management, or keyboard handling while restyling.

Useful extras: `21st theme <id>` prints a theme's CSS variables, which can seed MASTER.md's tokens rather than fight them.

## When nothing free fits

Say so plainly, then compose from the design system instead:

- Build the component from MASTER.md's tokens and the `ui-styling` sibling skill (shadcn/ui + Tailwind).
- Do **not** force a component whose visual language contradicts the design system — that is worse than building it.
- Do **not** silently escalate to paid generation. Offer it with the cost stated and let the user choose.

## Non-React stacks

21st.dev is React/Tailwind-centric. For Vue, Svelte, Astro, or plain JS:

- Still search — use the results as **design reference** (structure, spacing, states).
- Hand-build the equivalent in the target framework against MASTER.md.
- Do not install React components into a non-React project.

## Before moving on

The component is in place, restyled to MASTER.md, keyboard-accessible, and it renders. Motion comes next — not before.
