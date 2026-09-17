# Visual density — the gate between "tasteful" and "premium"

Read this **after MASTER.md exists and before you build**. It is the step that was missing
when this pipeline produced pages the user rejected four times in a row as "too basic",
"too clean", "too simplistic" — while every individual choice in them was defensible.

## The typography trap

**Excellent type + generous whitespace + hairline rules is not visual richness.** It is a
beautifully set *document*. A premium landing page is a *built environment*.

This is the pipeline's characteristic failure, and it is seductive because every step passes:
the palette is considered, the type scale is right, the spacing is disciplined, the impeccable
pass comes back clean. The page is *tasteful and empty*. The user does not have the vocabulary
to say "insufficient visual device count"; they say **"it's too basic"**, and they are right.

A direction named "minimalism", "editorial", "Swiss", or "exaggerated minimalism" makes this
trap almost certain. Those are legitimate directions — but they describe **restraint in
composition**, not **absence of things**. Exaggerated minimalism at Apple still ships product
photography, depth, motion and material. Restraint means *few elements, enormous care*; it has
never meant *type on a background*.

> If you removed every heading and paragraph from your page, would anything be left?
> If the answer is "some rules and whitespace", you have built a document.

## The floor: ship at least SIX

Count the devices your page actually has. A rich page carries **six or more**. Under four, it
will read as basic no matter how good the typography is.

**Ambience and material** — the page has a surface, not a background colour
1. **Ambient light field** — large, soft, blurred colour lobes behind the whole document.
   Single highest-impact device on this list; a flat fill is the #1 tell of a thin page.
2. **Grain / noise / texture** overlaying the light so it never reads as a digital wash.
3. **Depth** — parallax, layers travelling at different rates, elements overlapping in z.

**Surfaces that react** — things behave like objects
4. **Cursor spotlight** on cards and panels — light follows the pointer.
5. **Tilt / perspective** on a product surface.
6. **Magnetic controls** — the primary action leans toward the cursor.

**Composition devices** — the page has shapes, not just a column
7. **Bento grid** — cells of genuinely different weights, at least one inverted or accented.
8. **Marquee / belt** — a strip travelling horizontally. Cheap to build, enormous presence.
9. **A full-bleed inverted section** that arrives as a panel rather than a hard cut.
10. **Card art** — every card carries a small piece of made geometry that *shows* its point.
    A card with a heading and a paragraph is a paragraph in a box.

**Figures and motion** — the page moves at moments that mean something
11. **Counters / odometers** — numbers that roll or count up when reached.
12. **Line-masked type reveals** — display type rising out of its own edge, never a fade-up.
13. **Spring arrivals** — physics, not authored durations.
14. **A live, playable element** — something the reader can actually operate.

**Never counts toward the six:** fade-up-on-scroll, hover colour changes, a drop shadow,
rounded corners. Those are hygiene.

## Calibrate before you build, not after

Name the bar out loud in one line, then list what those pages have that a type-only page does
not: Linear, Vercel, Stripe, Framer, Raycast, Clerk, Resend, Arc. Every one of them ships
ambient light, material depth, reactive surfaces, and at least one bespoke visual device per
section. **None of them is only type.**

If the project has real brand constraints (this product's warm paper and single accent, say),
you do not abandon them — you execute *those* constraints with density. Warm paper gets warm
light behind it. One accent colour becomes the spotlight, the counter, the marquee highlight
and the card art.

## Content density is a separate axis — do not confuse them

"Too bare" can mean two different things and they have different fixes:

- **Thin content** — the page does not say enough about the product. Fix: mine the actual
  codebase, config and docs for real capability. Concrete beats abstract every time — a real
  job cadence with hours in it beats the phrase "AI-powered", and a product's own verbatim
  copy beats anything you can write about it.
- **Thin visuals** — the page says plenty, in one uniform texture. Fix: the six above.

**Adding words does not fix thin visuals.** If the user says "too bare" and you respond with
more paragraphs, you will be told again. Ask which, or fix both.

## The gate

Before you write a line of component code, state in the run:

```
Visual devices planned: <list>          (must be >= 6)
Ambient light field:    yes / no        (if no — justify it, it is the biggest single win)
Per section:            what is the made thing here, besides type?
```

And after building, apply the real test — harsher than the AI-slop test, because a page can
pass that one by being empty:

> Screenshot it. Would a stranger believe this was made by a design-led company with a
> full-time designer? Or does it look like a very well-set README?

If a section contains only a heading, a paragraph and a rule, it has failed. Give it a made
thing or merge it into its neighbour.
