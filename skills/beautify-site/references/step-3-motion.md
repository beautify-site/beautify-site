# Step 3 — Motion (motion.dev)

Layer animation on last, once the layout and components are settled and correct.

## Why last

Motion amplifies whatever is underneath it. Animating an unresolved layout locks in the wrong thing and makes it harder to change. Get it right still, then make it move.

## Pull current docs — free, no auth

The docs index is:

```
https://motion.dev/llms.txt
```

Fetch it, then fetch the specific topic pages you need. **Do not use `llms.motion.dev`** — it does not resolve.

Topic pages the index covers:

| Need | Page |
|---|---|
| Core animation API | `motion.dev/docs/react-animation` |
| Enter/exit transitions | `motion.dev/docs/react-transitions`, `AnimatePresence` |
| Hover, tap, focus | `motion.dev/docs/react-gestures`, `motion.dev/docs/react-hover-animation` |
| Scroll-linked / scroll-triggered | `motion.dev/docs/react-scroll-animations`, `motion.dev/docs/react-use-scroll` |
| Layout shifts, shared element | `motion.dev/docs/react-layout-animations`, `motion.dev/docs/react-layout-group` |
| Spring physics | `motion.dev/docs/react-use-spring` |
| Drag | `motion.dev/docs/react-drag` |
| Reordering lists | `Reorder` |
| Variants, SVG, text | covered in the index |

Motion is the library formerly called **Framer Motion** — install `motion`, and treat older `framer-motion` snippets as renamed, not different.

It supports **React, plain JavaScript, and Vue**, so this step still works on non-React stacks.

Optional: a paid **Motion+** `/motion` skill gives agents 440+ examples and perf tooling. Mention it if the user wants depth; never require it.

## Take the motion level from MASTER.md

Step 1's `--motion` dial (1–10) is the budget. Honor it:

- **1–3** — state changes and focus only. Fades and 150ms transitions. Nothing decorative.
- **4–6** — the default. Entrance reveals on scroll, hover feedback, smooth layout transitions.
- **7–10** — expressive. Staggered reveals, spring physics, scroll-linked parallax, shared-element transitions.

If the design system says calm and you build a parallax showcase, you have broken the direction the user approved.

## Spend the budget on weight, not on entrances

**Fade-up-on-scroll is the house style of every generated landing page on the web.** It costs
nothing to write, which is exactly why it reads as free. A page whose entire motion budget is
`opacity: 0 → 1` plus `translateY(20px)` will be called "basic" no matter how well tuned the
easing is, and re-tuning the easing will not change that verdict.

Reach for the things that do not come free:

- **Springs, not durations.** Arrivals settle; they do not stop. Author stiffness and damping
  once, globally — a page where every element has its own bespoke easing reads as assembled.
- **Line-masked type.** Display type rising out of its own edge, split to the lines it
  *actually rendered on* (measure after layout; never guess the breaks). Not a fade.
- **Odometers.** A figure that rolls on a reel rather than teleporting from 6 to 5.
- **Depth.** Layers travelling at different rates so the page has a z-axis.
- **Velocity response.** How fast the reader scrolls deforms what they are looking at, a
  fraction — paper under acceleration behaves like paper. Keep it below the threshold of "did
  that move?"; past it, it reads as a rendering fault.
- **Something playable.** One element the reader can actually operate beats any amount of
  decoration, and it survives `prefers-reduced-motion` because it is an interaction rather
  than an ornament.

## The scroll budget — price every pinned section

Pinning is the most powerful device here and the easiest to make feel broken. A `position:
sticky` stage holds the page still while the reader scrolls; if too little changes while it
holds, they do not read "deliberate", they read **"the page is frozen"** — and from outside,
that is the same thing as a bug.

**Before shipping a pin, compute what it costs:**

```
scroll range = stage height − viewport height
cost per idea = scroll range ÷ number of states the stage moves through
```

- **Budget: roughly one viewport of scrolling per idea, and no more.** ~600–800px per state.
- A real failure from this pipeline: a three-card pinned deck at `300vh` cost **600px of scroll
  per word**, so the reader travelled 2,860px watching one unchanged card with the heading
  nailed to the top of the screen. Every mechanical test passed. It was still the worst thing
  on the page.
- **If a section cannot justify its hold, do not pin it.** Three cards you simply scroll past,
  each carrying its own art, beat a pinned sequence that makes the page feel stuck.
- Never pin content taller than the viewport — the bottom of it can never be reached.
- Never pin on a phone. Touch has its own momentum; a sticky stage on a short viewport traps
  content and fights the platform.

## Never hijack the wheel

Do not `preventDefault` a wheel event to ease the document toward your own target, however much
"weight" it promises. A trackpad does not send one large wheel event — it sends a burst of small
ones, roughly one a frame. Measured against that input, a lerped custom scroller moved the page
**102px for 1200px of delta**: it swallowed 92% of the reader's scrolling.

It also puts a hand-written loop in front of momentum, keyboard paging, find-in-page, the
scrollbar and anchor jumps — five things the browser already does better than any page can.

**Observe velocity from the native scroll position instead.** Every effect that wanted a custom
scroller works identically off the real one, at none of the cost.

## Two traps that pass every test and ship broken

**Animating a transform to `none`.** `none` carries no `scale`, and `scale` is the one transform
function whose identity is 1 rather than 0 — so `translateY(26px) scale(0.985) → none` resolves
scale to **0**. The element animates itself to literally zero size while still occupying its full
height in layout and reporting `opacity: 1`. `translate` and `rotate` survive it because their
identity *is* 0, which is why a headline animates correctly and hides the defect in everything
beside it. **Always name the end state explicitly, mirroring the function list you started from.**

**IntersectionObserver coalesces.** Delivery is asynchronous; if an element enters and leaves
between two deliveries — a fast flick past a section — the callback can never fire at all, and
that section is invisible for the rest of the session. A one-shot reveal makes it permanent.
**Add a scroll-idle sweep** that reveals anything sitting in the viewport without its callback.
It converts an invisible-content bug into an entrance that merely skipped its animation.

## What to animate, in priority order

1. **State feedback** — hover, focus, active, loading, success/error. This is the animation that earns its keep; do it first.
2. **Entrance** — content revealing on scroll or mount. Subtle, fast, once. Never re-trigger on every scroll pass.
3. **Continuity** — layout transitions and shared elements, so things move rather than jump.
4. **Expression** — hero flourishes, parallax. Only at high motion levels, only where it serves the story.

## Non-negotiable: reduced motion

Every animation must be reduced or disabled under `prefers-reduced-motion`. This is an accessibility requirement, not a nice-to-have — vestibular disorders make unwanted motion genuinely harmful.

- Use Motion's reduced-motion support (`useReducedMotion`) or a CSS media query.
- Reduced motion means the content still **arrives** — cut the movement, keep the opacity change or show the final state. Never leave an element invisible because its animation was skipped.

## Performance

- Animate `transform` and `opacity`. Avoid animating `width`, `height`, `top`, `left` — they force layout.
- Use Motion's layout animations rather than hand-animating position.
- Keep scroll handlers off the main thread where Motion offers a native-driven path.
- Continuous, always-running animation is a battery and CPU cost — bound it, or stop it offscreen.

## Before calling it done

- Every animation respects `prefers-reduced-motion`.
- Motion level matches MASTER.md.
- Nothing animates that does not need to.
- It runs at 60fps on a normal machine — check, do not assume.
- **Every pinned section is priced** — scroll range ÷ states, and it is inside the budget.
- **Nothing is hidden while it is on screen**, tested on a fast scroll rather than a slow one.
- **Real input was used.** If you touched scroll or pointer behaviour, drive it with real wheel
  and pointer events. `window.scrollTo` bypasses a wheel handler entirely — it will pass a
  whole suite over a scroller that eats 92% of the reader's input.
- **The resting state in CSS is the FINAL state**, and JS only ever *adds* entrance states.
  Build it that way and the page is complete with JS off, JS broken, or motion reduced — and no
  reveal bug can ever cost the reader content.
