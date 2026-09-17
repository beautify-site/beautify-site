# Step 4 — Impeccable pass (mandatory)

Strip every AI tell from the **built artifact**, not from the plan. This step is never skipped.

**Self-contained.** The blocklist and the mechanical sweep below are the complete standard. The
`impeccable` skill enriches this pass when installed, but is never required to run it.

## Why it comes last

Steps 1–3 make the page good. This step makes it not-obviously-machine-made. Those are different jobs, and the second one can only be done against real rendered output — a tell you can only see once the thing exists.

The test that decides it:

> **If you showed this to someone and said "an AI made this," would they believe you instantly?**

If yes, it is not finished. A distinctive interface makes people ask *how was this made*, not *which AI made this*.

## Method

1. **Take the blocklist below as the source of truth.** It is complete and self-contained — this step needs no other skill installed.

   *Optional enrichment:* if `~/.claude/skills/impeccable/SKILL.md` exists, read it fresh first (the `<absolute_bans>` block, the `DO NOT` lines under Visual Details / Motion / Interaction, and *The AI Slop Test*) and fold anything extra into the sweep. If it is absent, say nothing and proceed — the pass still runs at full strength.
2. **Grep the built artifact** for the mechanical bans (commands below). Grep catches what the eye skips.
3. **Look at it.** Screenshot the rendered page at desktop and phone width and actually read the image. Several tells — and every rendering bug — are invisible to both grep and a passing test suite.
4. **Fix by restructuring.** A banned pattern in a nicer colour is still the banned pattern. Change the element's structure.
5. **Re-verify** in a real browser, then report what was found and removed.

## The mechanical sweep

```bash
F=<the built file or src dir>

# BAN 1 — side-stripe accents (border-left/right wider than 1px)
grep -rnE "border-(left|right):\s*([2-9]|[1-9][0-9])" "$F"

# BAN 2 — gradient text
grep -rnE "background-clip:\s*text|-webkit-background-clip:\s*text" "$F"

# BAN 3 — eyebrows / kickers / overlines above headings
grep -rniE "eyebrow|kicker|overline|class=\"label[- \"]" "$F"
grep -rnE "text-transform:\s*uppercase" "$F"     # letterspaced caps label is the usual tell

# BAN 4 — trailing periods on headings (incl. before a <br>)
grep -rnoE "<h[1-6][^>]*>[^<]*\.(\s*</h[1-6]>|\s*<br)" "$F"

# BAN 5 — fake terminal / code-block / IDE mock
grep -rniE "class=\"term|terminal|\.mock|window-dot|traffic-light|<pre|<code" "$F"

# Softer tells worth a look
grep -rnE "backdrop-filter|cubic-bezier\([^)]*1\.[0-9]" "$F"   # glass everywhere; bounce/elastic easing
grep -rniE "#000\b|#fff\b|#ffffff|#000000" "$F"                # untinted pure black / white
grep -rnE "transition:[^;]*\b(width|height|padding|margin)\b" "$F"  # animating layout properties
```

## The blocklist

**Hard bans — match and refuse:**

| Ban | Pattern | Rewrite as |
|---|---|---|
| Gradient text | `background-clip: text` over any gradient | One solid colour. Emphasise with weight, size, or a different colour — never gradient fill. |
| Side-stripe accent | `border-left`/`border-right` > 1px on cards, list items, callouts, alerts | A different structure entirely: full border, background tint, leading number or icon, or no indicator at all. Not `box-shadow: inset`. |
| Eyebrow / kicker / overline | Any small label above a heading — letterspaced uppercase mono especially — plus announcement pills floating above a hero headline | Delete it. Fold anything load-bearing into the heading. The heading carries the section on its own. |
| Trailing period on a heading | A full stop ending any `h1`–`h6` or heading line, including before a `<br>` | Remove it. Headings are labels, not sentences. Punctuation between two genuine clauses stays; the terminal stop goes. |
| Fake terminal / code-block / IDE mock | Chrome-dot window, monospace lines typing themselves, mock dashboard screenshot, mock chat window | Invent a visual specific to what the product actually does. A window full of fake logs is the dev-tool landing-page cliché. |

**Also strip:**

- Decorative glassmorphism used everywhere rather than purposefully
- Rounded rectangles with generic drop shadows — safe, forgettable, could be any AI output
- Sparklines as decoration — tiny charts that look sophisticated and convey nothing
- Modals used because they were easy
- Bounce or elastic easing — dated and tacky; real objects decelerate smoothly
- Animating `width` / `height` / `padding` / `margin` — transform and opacity only
- Every button styled primary — hierarchy needs ghost and text variants
- Redundant headers and intros that restate the heading
- Pure `#000` or `#fff` — always tint; neither appears in nature

## The trap that keeps catching people

**A text-based check cannot see a visual bug.** Two real cases, both of which passed every automated check:

- Character-splitting a headline for a stagger animation moved the glyphs outside the box painting the gradient fill — **the entire phrase rendered invisible**, while `textContent` read back perfectly.
- Wrapping words in `overflow-hidden` mask spans clipped the spaces between them, so the headline rendered as `Wearenot heretomove`. Every text assertion passed, because the space was in the DOM.

So: **screenshot it and read the image.** Assert on structure (computed styles, rendered box dimensions, opacity) rather than on `textContent`, which lies.

## What to report

Name what you found and what you did. Concrete, per hit:

> Found 2 AI tells: gradient text on the hero headline (BAN 2) and on the closing CTA. Both rewritten as solid colour with weight contrast carrying the emphasis. Re-verified in Chromium at 1440 and 390 — no errors, no overflow, headline renders in full.

If the audit genuinely comes back clean, say so plainly and name what you checked. Never claim a pass you did not run.

## Do NOT

- **Do not skip this because the design already looks good.** That is exactly when tells survive.
- **Do not soften a banned pattern** — a thinner stripe or a subtler gradient is still the ban.
- **Do not trust a green test suite** as evidence the page renders correctly. Look at it.
- **Do not strip character from the design** in the name of this pass. The goal is removing *templated* choices, not removing boldness. Distinctive stays; generic goes.
