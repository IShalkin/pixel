---
name: pixel
description: "Turn a complex or abstract concept into ONE interactive, self-contained HTML page that teaches it by anchoring it in a simple, concrete SITUATION the audience steps through. The situation and the visual style are INVENTED fresh for each concept, never copied from an example. Use when the user wants to teach, demo, or explain something hard (an architecture, an agent loop, a protocol, a tradeoff, a workflow) in a way people can open and play with. Triggers: 'make a demo of X', 'turn this concept into a workshop explainer', 'show how Y works interactively', 'pixel-ify this', 'an interactive teaching page for Z'."
metadata:
  requires:
    bins: []
---

# pixel

Take one complex idea and make it land by showing it as a **simple, concrete
situation the audience can step through**. The output is a single self-contained
HTML page someone opens, clicks or walks through, and leaves understanding the
thing they did not understand before.

That is the whole skill. Everything else (pixel art or plain DOM, a cat or a
sprite or no character at all, a skill-builder, a ZIP export, a particular
palette) is a CHOICE you make to serve a particular situation. None of it is the
point, and none of it is mandatory.

## The one mistake this skill exists to prevent

Do not clone a reference and recolor it. The fastest way to ruin one of these is
to open `multi-agent-team.html`, keep its camera and its cats and its
skill-builder, and swap the labels. You get a page that is about the reference,
not about the concept, and stuffed with machinery the concept never asked for.

If you catch yourself keeping a feature (a character, a builder, a ZIP writer, a
sabotage toggle) because the example had it rather than because THIS concept
needs it, stop and take it out.

The references below are proof the method works across very different styles. A
PM's calendar day, an agent walking between desks, a five-layer stack trace, a
coloring-book of rules: same method, almost nothing in common visually. Study
them for HOW they teach. Invent your own situation and look.

## The method (this is the work)

### 0. Ask who it is for, before anything else.
Do NOT design in a vacuum. The same concept becomes a different page for a
first-year student, a domain expert, a mixed conference audience, or a presenter
driving it from a stage, and you cannot guess which from the concept alone. Ask
the user up front (one short question) at least:
- **Who is the audience and their level?** (novices needing analogies and expanded
  jargon, peers who want the real terms kept, a lay public wanting only the "aha".)
- **What is the setting?** (self-paced on their own screen, vs presented live from
  a stage, which wants bigger type, an autoplay-with-pause story, and fewer fiddly
  controls.) The deployed-page language question (see Localization) belongs here too.

The answer sets vocabulary (keep `SHH`/`idempotent`/`quorum` vs gloss every term),
depth (mechanism in full vs just the one surprise), reading level, pacing, and how
much interaction to expose. If the user says "doesn't matter" or gives a domain
(e.g. "internal Accenture, technical"), infer a sensible level from that and STATE
the assumption you are building to in one line, rather than silently defaulting.
Carry the chosen audience into every later step: the situation, the "aha" wording,
the caption copy, and the verification all get judged against THAT audience, not a
generic one.

### 1. Find the situation. This is where most of the thinking goes.
Brainstorm a concrete, familiar, small scene that makes the concept's core
tension VISIBLE. The concept is abstract; the situation is something a person can
picture. Ask:
- What is the one hard idea here? State it in a sentence.
- What everyday scene already has this same shape? (a day at work, a relay race,
  a kitchen, a customs line, an assembly line, a courtroom, a tide chart)
- What is the smallest version that still shows the tension? Cut everything else.

A good situation is simple enough to grasp in five seconds and rich enough that
the concept's surprise falls out of it naturally. If the situation needs a
paragraph of setup, it is the wrong situation.

### 2. Name the "aha".
The single thing the audience should viscerally feel by the end ("oh, THAT is why
you need the eval", "oh, the failure was upstream the whole time"). Everything in
the page serves that one moment. If a feature does not build toward it, drop it.

### 3. Choose the medium that shows THIS situation best.
Only now do you pick the form. Options, not a menu to default through:
- **Animated canvas** when the audience should WATCH something move through a
  process and feel its mood and rhythm (a character doing a loop, actors
  cooperating, a thing travelling a pipeline).
- **Interactive DOM/CSS** when the audience should TOGGLE things and watch
  outcomes recompute (rules on or off, layers failing, inputs changing a verdict).
- **Something else entirely** if that fits better. A scrubbable timeline, a
  diagram that fills in, a side-by-side diff that morphs. You are not limited to
  what the references happen to use.

State your pick and the one-line reason before building.

**Know when this skill is the wrong tool, and say so instead of forcing it.** The
method (a hand-drawn canvas or DOM scene, stepped through) is built for showing a
MECHANISM, a process with a shape and a mood. It is not a charting library and not a
3D engine, and several concept shapes will fight it. Recognize these at step 3 and
either escalate to the right tool or reframe the concept, rather than shipping a
fragile hand-rolled version:
- **Precise quantitative data on real axes.** If the "aha" rides on exact values,
  tick-marked axes, a nice-number scale, a log axis, or a distribution's true shape
  ("0.37 vs 0.41", "this is skewed"), you will hand-roll a buggy chart with no
  scale function. Reach for a real charting library (the `czechitas` skill loads
  Plotly for exactly this) and accept the one CDN dependency, or keep the quantity
  QUALITATIVE (a wash that is "more" or "less", not a number you must read off).
  A drawn field is honest only when the lesson is "which way it leans", not "by how
  much".
- **Dense graphs or networks.** The AABB label nudger handles a handful of labels;
  it has no leader lines and no force fallback, and there is no graph-layout code
  here (force-directed, hierarchical, DAG layering, edge routing). Five nodes you
  place by hand; fifty collapse into a hairball. If the concept IS a network, that
  is a layout-engine job, not this skill.
- **Genuinely 3D structure.** Canvas 2D only, no WebGL. A concept that is 3D in its
  bones (a folded protein, orbits, a 3D vector field, a rotating molecule) will come
  out as a hand-flattened projection that is both brittle and usually read wrong.
  Use a 2D SLICE or cross-section if one carries the idea (the neural-tube demo
  worked because a transverse slice was the honest view); otherwise this is the
  wrong medium.
- **Deep zoom over a huge coordinate space.** The logical canvas is one fixed size
  and the "camera" is a pan-focus, not a real zoomable viewport. A fractal, a
  genome browser, a map from country to street needs true zoom/level-of-detail,
  which this skill does not provide.
- **Continuous dynamics where the form IS the point.** "One action equals one step"
  is right for ordered events, but some concepts live in a continuous signal (a
  decaying oscillation, a waveform, phase, feedback ring). Forcing them into
  discrete Next-steps can destroy the very shape that is the "aha". Prefer a Play
  that runs the continuous motion with a Pause and a scrubber, and do not chop a
  smooth phenomenon into beats just to obey the rule.
- **Thousands of particles or per-pixel fields.** The pause-aware `tween` is for
  choreographed beats, not a live sim of thousands of particles or per-pixel work
  (reaction-diffusion, a full-resolution cellular automaton). Those need
  `ImageData`/typed arrays and a real rAF loop, which is outside what is documented
  here; expect a performance wall.
- **Heavy set math, formulas, code, or tables.** `fillText` is miserable for typeset
  formulas, matrices, syntax-highlighted code, or real tables. The DOM side is a
  verdict engine, not rich typography. If the content is fundamentally a typeset
  formula, that wants KaTeX/MathJax (a CDN dependency, in tension with the
  single-file rule) or it wants to be a different artifact.
The honest move when a concept lands here is to tell the user "this one is not a good
fit for a hand-drawn stepped scene because X; the better tool is Y", not to ship a
hand-rolled chart/graph/3D view that quietly misrepresents the data.

### 4. Make it steppable, never a movie.
One action equals one revealed step. Nothing important flashes by. The audience
controls the pace (Next, or a toggle, or a scrubber). This is non-negotiable
across every medium: the value is in seeing the mechanism unfold, not in watching
an animation finish.

**A self-playing STORY is usually the primary feature; build it first.** The thing
that makes these pages land is a narrative that visibly unfolds, an actor doing a
loop, a process running its course, a scene transforming beat by beat, and "the
audience controls the pace" means they can Play it, Pause on any beat, step it by
hand, and replay it, NOT that the only way to see anything is to click Next eight
times. The recurring failure (caught more than once) is to ship the manual
stepper alone and call it interactive: it reads as a controlled inspection, not a
story, and users rightly say "I expected it to animate / where is the plot."
So unless the concept is genuinely static, give it a real autoplay: a Play button
that walks the beats on its own, dwelling on each long enough to read, with a
Pause that truly freezes (use the abortable, elapsed-time `tween`/`wait` technique,
never raw `setTimeout`, so Pause does not fast-forward), and any manual action
(Next, a toggle, a drag) quietly pauses autoplay so the two never fight. A presenter
audience (see step 0) needs this even more: they Play, then pause to talk over each
beat. Build the playable story first, prove it carries the "aha" on its own, and
only then layer controls on top.

**Then prefer a manipulable model over a slideshow for the controls.** "Steppable"
is the floor, not the ceiling. If the concept has control variables (a threshold,
a dose, a rate, a gradient strength, the position of a source, a toggle that
recomputes an outcome), the strongest version ALSO lets the audience SET those and
watch the scene recompute live, so they can probe the rule by playing with it after
the story has shown it. Reserve linear Next-stepping for a genuinely ordered
sequence of events (a fold that has to happen before a tube can close, a request
falling through layers in order). The ideal shape is BOTH: a self-playing story as
the spine, plus a sandbox of real controls hanging off it, not one or the other.
The DOM verdict engine (toggle a rule, the verdict re-resolves) and a canvas demo
with sliders that move domain boundaries in real time are the same idea in two
media. A useful test while designing: name the two or three variables a domain
expert would reach for to probe this idea, and make at least one of them something
the audience controls directly, not just a value the narration changes for them.

### 5. Add contrast only if the concept has a "what goes wrong".
If the lesson includes a guardrail or a failure mode, let the audience break it
and watch the bad outcome the lesson warned about reappear. This is powerful but
optional. A concept with no failure dimension does not need it, and forcing it in
(as a "sabotage toggle" because the examples had one) is the clone mistake again.

### 6. Add a take-home artifact only if it fits.
If the concept is about producing a real thing (a config, a document), let the
audience build and download it. If it is not, skip it. The PM demo's
skill-builder + ZIP export belongs to demos ABOUT authoring skills. It does not
belong in a demo about, say, consensus protocols.

## The references: situations, not templates

All in [`reference/`](reference/). Read the one or two whose METHOD-MOVE is
closest to your concept, to see how they made an abstraction concrete. Do not
copy them.

| Reference | The situation it invented | The method-move worth stealing (the idea, not the code) |
|---|---|---|
| `multi-agent-team.html` | a product team's working day, reinvented by per-person agents | many actors cooperating on shared systems; zooming the "camera" onto whoever is acting so attention never splits |
| `canvas-sprite-stations.html` | a character physically walking desk to desk to do one job | a single actor doing a full loop you can follow step by step; a second character who grades the result |
| `canvas-accenture-slide.html` | an agent assembling one branded slide, checked by a critic | the payoff is a real artifact that visibly degrades when a guardrail is removed |
| `dom-stack-trace.html` | one request falling through five named layers | a linear cascade where a failure upstream visibly skips everything downstream |
| `dom-skill-builder.html` | a coloring-book of rules you switch on and off | toggling rules and watching a verdict recompute live; the abstract rule set made tactile |

What these share is ONLY the method: a simple concrete scene, stepped through, that
makes one hard idea obvious. They do not share a style, a character, a palette, or
a feature set, and your build does not have to share any of those either.

## What is genuinely reusable vs. what is always fresh

**Borrowable techniques** (lift the code when, and only when, your chosen medium
actually uses them):
- Canvas crispness: 2x DPR backing store, `ctx.scale(2,2)`,
  `imageSmoothingEnabled=false`, logical-coordinate shadow size.
- Pause-aware, abortable timing (`ABORTED` sentinel, `tween`/`wait` that account
  for elapsed time) so Pause freezes instead of fast-forwarding. Use this instead
  of raw `setTimeout` whenever a canvas demo can be paused.
- One-speech-bubble-at-a-time discipline; a "camera" that focuses the actor.
- Sprite walking between fixed stations; a procedurally drawn second actor.
- DOM verdict engine: a pure `run`/`evaluate` returning typed statuses with
  max-severity resolution and downstream cascade; `state` + `setState` re-render;
  CSS keyframes/transitions for flow; an inline `?test=1` assertion harness.
- A dependency-free store-only ZIP writer (CRC-32) for multi-file downloads.
- Manual canvas text handling (wrap from `measureText`, clip per panel, bubble
  placement). See [`reference/technique-canvas-text.md`](reference/technique-canvas-text.md).
- Aliveness moves (walk cycle, idle bob, emotes, parallax, animated tiles, NPC
  steering) and a vetted asset/license catalog. See [`reference/technique-aliveness.md`](reference/technique-aliveness.md).

**Always invented per concept** (never inherited from an example):
- The situation and the "aha".
- The medium and the visual identity.
- Which actors exist, if any, and what they are (a cat, a sprite, a cursor, a
  glowing packet, nothing).
- Which of the borrowable techniques you use at all.
- Whether there is a sabotage toggle, a second actor, a builder, an export.

## Borrowable-technique deep-dives (read on demand, not every time)

Two techniques have enough hard-won detail that they live in their own files under
[`reference/`](reference/), so this method file stays short. Open the matching file
ONLY when your chosen medium actually uses it; otherwise skip both.

- **Text rendering on canvas** -> [`reference/technique-canvas-text.md`](reference/technique-canvas-text.md).
  Read it when an animated canvas draws text (speech bubbles, labels, panels of
  moving content). Canvas 2D has no layout engine, so wrapping, clipping, and
  de-overlapping boxes are all manual. Headline gotchas: build the box FROM the
  text with `measureText` (never cram text into a fixed box); `save/clip/draw/restore`
  every panel of moving content; show one bubble at a time or place with the AABB
  flip-plus-queue solver. Plain-DOM pages need none of this.
- **Making the scene feel alive (assets and motion)** -> [`reference/technique-aliveness.md`](reference/technique-aliveness.md).
  Read it when the page has characters or a scene that should feel animated (an
  actor doing a loop, a crowd, a living background). Covers the licensed asset
  catalog (record every license; LPC needs a `CREDITS.txt`), the one-rig
  walk/dance/idle figure, emotes, parallax, autotiling, and goal-driven NPC
  steering. A DOM verdict page or a static diagram needs none of this.

Each file ends with the verification items specific to that technique; fold those
into your final check when you used the technique.

## Visual identity

There is no mandatory palette. Choose a look that fits the concept and the
audience, and commit to it. The references show the range: warm Grand-Budapest
pastels with a single signature accent (the canvas builds), an editorial
cream-and-vermilion (the stack trace), a WW2 "We Can Do It" poster (the
skill-builder). Borrow a direction if it fits; invent one if it does not.

If the concept comes with brand constraints (a client colour, a workshop's
house style), honor those over any reference look.

**Draw the field, do not just label it.** If the concept IS a gradient, a flow, a
pressure, a force, a field, anything continuous that varies across space, then that
quantity is the hero of the page and it must be VISIBLE as a thing on the canvas, a
wash / heatmap / arrow whose length tracks magnitude / particle density, not merely
a text label ("BMP source") pointing at empty space. The recurring failure (caught
on the neural-tube demo) was a page whose entire point was "two opposing gradients
overlap across the same cells" while neither gradient was actually rendered, only
named. The fix that made it land: paint each gradient as a vertical wash behind the
actors, and tie its opacity to the live source state so toggling a source off or
dialing a slider visibly drains its field. When the field is the concept, make the
field something the eye can see change.

But a drawn field can lie more convincingly than a label, so encode it honestly.
Opacity-as-magnitude is perceptually NON-linear (the eye does not read twice the
alpha as twice the value), so use it for "more here, less there", not for a quantity
someone is meant to compare by amount; if the amount matters, that is the
quantitative case above, give it a scale and a legend. Two semi-transparent washes
overlapping produce a THIRD colour that the eye reads as a third category, not as a
sum, so when two fields cross, make the overlap legible on purpose (a deliberate
blend, or keep them on separate visual channels) rather than hoping it reads as
addition. Pick a colour-blind-safe ramp and a sequential ramp for a one-directional
quantity vs a diverging ramp for something with a meaningful midpoint, and if the
colour or opacity carries meaning at all, give the page a legend that says what the
colour means. A field the eye can see change is the goal; a field the eye
mis-reads is worse than an honest label.

**Fill the frame, do not leave the hero in a corner.** A canvas that is mostly empty
paper with the subject crammed into one quadrant reads as unfinished, not as
minimal. Either center and size the subject to use the space, or put the empty
region to work with something that also teaches (the gradient wash above, a
persistent axis with live magnitude arrows, a legend, a small inset). Empty space is
fine only when it is deliberate breathing room around a centered subject, not a
gap the eye reads as "missing".

## Localization

The page's language is independent of the chat language. Ask which language the
deployed page should be in; default to English. Czechitas workshop pages ship a
Czech UI even when we are talking in English or Russian; code identifiers and
comments stay English; numbers use the right locale (`toLocaleString('cs-CZ')`).

## Generation procedure

1. **Ask who it is for and in what setting** (method step 0), then **brainstorm the
   situation and the "aha"** for THAT audience, before any code or any reference.
   Write the audience, the situation, and the "aha" down in one or two lines. If
   they are weak, the page will be weak no matter how polished.
2. **Pick the medium (method step 3)** and say it to the user in one line with the
   reason.
3. **Read at most the one or two references** whose method-move matches, for ideas
   on making it concrete. Then close them. Do not build from a copy.
4. **Start from an empty file** named for the concept. Bring in borrowable
   techniques deliberately, one at a time, only as the medium needs them.
5. **Build in this order, testing after each chunk**: identity and layout, the
   situation and its actors/elements, the stepping mechanism, the content of each
   step, then optional contrast, optional artifact, then all chrome copy.
6. **Verify** (below) before declaring done.
7. **Hand off by absolute path** so the user can open it. Output is a file only;
   do NOT auto-deploy to GitHub Pages. If the user later wants it published, that
   is a separate, opt-in step (the personal-GitHub + Pages REST flow is documented
   in the `czechitas` skill); mention it exists, do not perform it unasked.

## Taste rules (hard constraints)

- **Zero em-dashes, zero en-dashes.** Use commas, periods, or "to" for ranges.
  Grep the output for the em-dash (U+2014) and en-dash (U+2013); there must be none.
- **No emoji in copy or UI.** Geometric control glyphs (a Play triangle) are fine.
- Instructional/caption text stays plain and concrete; personality lives in the
  situation, not in the labels.
- Single self-contained file: inline CSS and JS, no build, no framework. External
  deps only when the chosen look truly needs them (a Google Fonts link, or local
  fail-soft sprite PNGs). Prefer zero CDNs when offline-safety matters.

## Verification (every time, before saying it is done)

1. Serve and open. From the file's dir:
   `"/c/Program Files/Python313/python" -m http.server 8772` then open
   `http://localhost:8772/<file>.html`. (Python is at that full path; bare
   `python` is not on PATH. Playwright at :9222 is unreliable here;
   `cmd.exe //c start "" "<abs path>"` opens the default browser as a fallback.)
2. Step through the WHOLE situation: nothing flashes by, the audience controls the
   pace, the "aha" actually lands at the end, no console errors. If there is a
   contrast/sabotage control, confirm it changes the outcome. If there is a
   verdict engine, confirm `?test=1` prints all-passed. Unless the concept is
   genuinely static, confirm there is a self-playing story: Play walks the beats on
   its own, Pause truly freezes on a beat (does not fast-forward when resumed), a
   manual action pauses autoplay instead of fighting it, and the "aha" lands from
   Play alone without the user having to click Next. Then confirm the audience can
   also set at least one control variable themselves and the scene recomputes live,
   unless the concept is a genuinely linear sequence with no control variables.
   Shipping only a Next button on a concept that has a story or a knob is the
   failure this checks for: add the autoplay, add the knob.
3. Grep the file for the em-dash (U+2014) and en-dash (U+2013) (must be zero) and
   for stray emoji.
4. If the page language is not English, confirm every visible string is translated
   and numbers use the right locale.
5. Sanity-check the deliverable against the concept: does it teach THIS idea, or
   did a reference's machinery leak in? Remove anything that does not serve the
   "aha".
6. Sanity-check against the AUDIENCE you asked about: is the vocabulary, depth, and
   pacing right for them (jargon glossed for novices vs kept for peers), and does
   the setting fit (large type and an autoplay-with-pause story for a live
   presenter, calmer self-paced controls for a solo reader)? If you never asked who
   it is for, that is a process miss, not a style nitpick.
7. Look at a rendered screenshot, not just the code. If the concept is a
   gradient / flow / field, confirm that quantity is actually DRAWN (a wash, arrows,
   density) and not just labeled, and that it visibly changes when its source is
   toggled or dialed. Confirm the frame is filled: the subject is centered or the
   empty region does real work, not the hero stranded in a corner. Confirm no two
   on-canvas labels overlap and that symmetric elements are labeled symmetrically (a
   thing on the left and an identical thing on the right should not have one named
   and the other bare).
8. If you used either borrowable-technique deep-dive, run its own verification list
   too (the last paragraph of [`reference/technique-canvas-text.md`](reference/technique-canvas-text.md):
   longest lines through every bubble/panel, no overflow, no two boxes overlapping;
   and of [`reference/technique-aliveness.md`](reference/technique-aliveness.md):
   moving characters actually animate, at least one idle/ambient motion, every NPC
   mode fires over a few seconds, roamers stay off the content, every bundled asset
   has a recorded license).

## What this skill is NOT

- Not a template to clone. It is a method for inventing a fresh situation per
  concept.
- Not locked to pixel art, to a character, or to any one feature (skill-builder,
  ZIP, sabotage toggles). Those are options, used only when the concept calls.
- Not a slide generator and not a deployer. One interactive HTML page, written to
  disk; publishing is a separate opt-in step.

## Anti-patterns

- Cloning a reference and recoloring it. (The single worst failure.)
- Carrying over a character, a builder, a ZIP export, or a sabotage toggle because
  an example had it, not because the concept needs it.
- Picking the medium before finding the situation, or skipping the situation and
  jumping to "make a pixel demo".
- A demo that plays as a movie instead of being stepped by the audience.
- Raw `setTimeout` in a pausable canvas demo (breaks Pause and Reset); use the
  abortable `tween`/`wait` technique instead.
- More than one speech bubble on screen at once in a canvas demo.
- Auto-deploying to GitHub Pages without being asked.
- Em-dashes, en-dashes, or emoji. Ever.
