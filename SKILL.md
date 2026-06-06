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
  placement). See "Text rendering on canvas" below.
- Aliveness moves (walk cycle, idle bob, emotes, parallax, animated tiles, NPC
  steering) and a vetted asset/license catalog. See "Making the scene feel alive".

**Always invented per concept** (never inherited from an example):
- The situation and the "aha".
- The medium and the visual identity.
- Which actors exist, if any, and what they are (a cat, a sprite, a cursor, a
  glowing packet, nothing).
- Which of the borrowable techniques you use at all.
- Whether there is a sabotage toggle, a second actor, a builder, an export.

## Text rendering on canvas (no block model, so wrapping and clipping are manual)

Canvas 2D has no layout engine: `fillText` does not wrap, does not clip, and does
not measure the box for you. The three text bugs this family keeps hitting
(overflow past a bubble, two bubbles overlapping, animated content escaping a
panel) all come from treating canvas like the DOM. Apply these. The first and
third are confirmed against engine docs; the second is sound engineering with no
primary source, so treat it as a default, not gospel.

**Overflow: build the box FROM the text, never cram text into a fixed box.**
Wrap by measuring candidate lines with `ctx.measureText(s).width`, breaking before
a line exceeds the box width. Compute height from the vertical metrics, not a
guess: `lineH = m.fontBoundingBoxAscent + m.fontBoundingBoxDescent` (Baseline since
Aug 2023). `fillText`'s `maxWidth` only squashes glyphs, it does not wrap, so do
not rely on it. Confirmed: MDN measureText and Phaser's word-wrap source.
```js
function wrapText(ctx, text, maxW) {        // greedy word wrap, returns lines[]
  const words = text.split(' '); let line = ''; const lines = [];
  for (const w of words) {
    const t = line ? line + ' ' + w : w;
    if (ctx.measureText(t).width > maxW && line) { lines.push(line); line = w; }
    else line = t;
  }
  if (line) lines.push(line);
  return lines;                              // box height = lines.length*lineH + 2*pad
}
```
(`https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/measureText`)

**Containment: clip every panel that holds moving or possibly-overflowing content.**
A clip cannot be reverted directly, so the idiom is always save, clip, draw,
restore. This is the fix for scrolling fake-text on a dashboard, floating
harvest/damage numbers, any animated content that must stay inside its frame.
Confirmed: MDN clip. The DOM equivalent is `overflow:hidden` on a positioned
ancestor (confirmed: MDN overflow).
```js
ctx.save(); ctx.beginPath(); ctx.rect(px, py, pw, ph); ctx.clip();
drawScrollingStuff();            // nothing escapes the rect
ctx.restore();
```
(`https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/clip`)

**Two boxes overlapping: serialize, or place with a simple solver.** The cheapest
fix and the one this family already half-uses: show one bubble at a time and queue
the rest (a second actor's line waits its turn). When two must be visible, place
each above its owner, clamp into frame, and if it would collide with an
already-placed box, flip it below or push it down by the other box's height (AABB
test). Note: no primary source confirmed a dedicated speech-bubble placement
algorithm; the general label-de-overlap solvers that exist (D3-Labeler simulated
annealing, d3-force `forceCollide`) are overkill for a few runtime bubbles and
`forceCollide` does not even guarantee separation, so the AABB-flip-plus-queue
approach below is deliberate, not borrowed.
```js
function placeBubble(ax, ay, bw, bh, taken) {  // ax,ay = owner head; taken = placed boxes
  let x = ax - bw/2, y = ay - bh - 8;          // default above the head
  x = Math.max(4, Math.min(x, logicalW - bw - 4));
  if (y < 4) y = ay + 8;                        // no room above -> below
  for (const r of taken)                        // overlaps a placed box -> push down
    if (x < r.x+r.w && x+bw > r.x && y < r.y+r.h && y+bh > r.y) y = r.y + r.h + 6;
  taken.push({x, y, w: bw, h: bh});
  return {x, y};
}
```

Add to verification: run the LONGEST lines through every bubble and panel and
confirm nothing overflows and no two boxes overlap.

## Making the scene feel alive (assets and motion)

The default failure is a static scene: a character that slides without animating,
flat affect, a dead background. Aliveness is cheap if you reach for the right
move. All of the below is optional and per-concept, like every other borrowable
technique. Use `assets/` for real sprite art (the references already fail-soft on
missing PNGs); draw procedurally when you have no art.

**Asset catalog (record the license every time, it is the high-stakes part).**

| Source | License | Contents | Note |
|---|---|---|---|
| Liberated Pixel Cup (LPC) base + [Universal-LPC generator](https://github.com/LiberatedPixelCup/Universal-LPC-Spritesheet-Character-Generator) | **CC-BY-SA 3.0 / GPL 3.0, NOT CC0** (confirmed) | layered bodies, walk/run/jump cycles, clothing, hair, expressions, NPCs, tilesets, weapons, weather/magic effects; 500+ packs | MUST ship `CREDITS.TXT`; derivatives stay CC-BY-SA. Never treat as public domain. |
| [OGA Animated Emote Bubbles](https://opengameart.org/content/animated-emote-bubbles) | **CC0** (confirmed) | 16x16 PNG emote balloons (heart, exclaim, question, sleep, etc.) | Only pack confirmed CC0 in research. Attribution appreciated, not required. |
| Kenney.nl 2D packs | likely CC0, **unverified** | roguelike/RPG/platformer tiles + sprites | Verify on the pack page before use. |
| itch.io: Sprout Lands, Cup Nooble, Mystic Woods, Ninja Adventure, Pixel Frog (Pixel Adventure) | **unverified** | farming/RPG/platformer sets | Check each page; do not assume CC0. |

Rule: before bundling any asset, open its page, read the license, and if it is
LPC or any CC-BY/BY-SA pack, write a `CREDITS.txt` next to it. If you cannot
confirm the license, draw it procedurally instead.

**Walk cycle (confirmed: Godot, Phaser docs).** 4 to 8 frames at roughly 8 to 12
fps reads as alive; advance the frame index by DISTANCE TRAVELLED, not wall-clock,
and pick the sheet row by facing. Foot-anchor so the character does not float.
```js
const FRAMES = 6, STRIDE_PX = 10;                  // one frame per 10px walked
const frame = Math.floor(distanceWalked / STRIDE_PX) % FRAMES;
const row = {down:0, left:1, right:2, up:3}[facing];
```

**Idle and reaction (common practice, no single primary source).** Never leave a
standing character frozen: a 2 to 3 px sine bob (`y += Math.sin(t/400)*1.5`), an
occasional blink, a weight shift. On a landing or a hit, a quick squash-and-stretch
(scale y down/x up for 80ms, then overshoot back) sells impact.

**Articulate before you animate, and build ONE rig that drives every motion.**
A bob-in-place on a solid-block body does NOT read as alive: viewers call it
"jumping in place", because nothing swings. The fix is to give the figure
separate limbs (two legs, two arms drawn as `lineCap:"round"` strokes from hip
and shoulder to a foot/hand point) and drive their angle from a single phase
variable. Then walking, dancing, and idle are the SAME rig with a different
phase source, which is why one figure can do all three with no new art:
```js
let ph = 0;                                  // the one motion phase
if (moving)       ph = distanceWalked * Math.PI / STRIDE;   // gait: by distance
else if (dancing) ph = (t/(60000/BPM))*2*Math.PI + seedX;   // beat: by tempo + per-actor offset
// legs scissor on sin(ph); arms swing on -sin(ph) (opposition); feet anchor to
// the floor line (no bob) while the torso/head bob on |sin(ph)|.
```
Two things that matter: feet anchor to a fixed floor line so the body lifts off
them instead of the whole sprite sliding; and a per-actor phase offset
(`+ p.x*0.018`) makes a chorus ripple as a travelling wave instead of moving as
one clone. A character that is meant to walk must actually translate across the
floor (a real `walkTo` that accumulates `distanceWalked`), not teleport, or the
gait never engages. If the demo can host a "musical" mode, dancing is then free:
flip a `dancing` flag and the existing limbs follow the beat; held props (chorus
cards, pom-poms) just bob with the same phase. Differentiating figure types
(e.g. a skirt as a hip-to-knee trapezoid over the legs, longer hair as two side
panels by the jaw) is a few extra rects on the same rig, not a second sprite.

**Emotes (confirmed: Stardew event data).** A small fixed vocabulary covers most
emotional beats. Map an enum to a 16x16 emote sprite shown above the head, animate
a few frames, auto-dismiss after ~1.5s. Stardew's canon: heart, exclamation,
question, sleep, angry, sad, happy, blush. This is the expressive layer that was
missing.

**Living background (confirmed: Godot parallax, Tiled animated tiles).** Two cheap,
high-payoff moves: parallax (draw N background layers offset by `cameraX * scale`
with scale ~0.2 / 0.5 / 1.0 so far layers move slower) and animated tiles (water,
torches, grass sway as a short linear loop at ~10 fps, 100ms/frame). Ambient
particles (dust motes, fireflies, falling leaves) and a day-night colour overlay
add a lot for little code, but were not confirmed by a primary source, so treat
them as standard practice.
```js
for (const L of bgLayers)                          // parallax
  drawImage(L.img, -(cameraX * L.scale) % L.img.width, L.y);
```

**Autotiling for organic terrain (confirmed: Godot tilesets).** For grass/dirt/water
edges, compute a tile's variant from which neighbours share its terrain (a 4-bit
edge mask for simple cases, 8-bit corner+side for the full 47-blob set) and index a
lookup table. Pick 2x2 (cheapest), 3x3-minimal, or full 3x3 by how much art you have.

**Ambient NPC motion (confirmed: Nature of Code, Reynolds steering).** For NPCs that
wander believably instead of standing still: `steer = limit(desired - velocity,
maxForce)`. Wander = a random point on a circle projected ahead of the agent, with
the angle DRIFTING per frame (`theta += rand(-d, d)`) rather than re-rolled, so
motion is smooth not jittery.
```js
desired = setMag(sub(target, pos), maxSpeed);
const steer = limit(sub(desired, vel), maxForce);
vel = add(vel, steer); pos = add(pos, vel);
```
Wander alone, though, reads as aimless. To make agents feel like they are DOING
things, give them goal props and roles: a chased object (an item that relaunches
on catch), a batted object (a token with velocity, friction, wall-bounce that a
nearby agent knocks away and another returns), a sit-and-groom pose. Two gotchas
learned the hard way: (1) assign roles DETERMINISTICALLY every frame from
`floor(t/period)%n` and proximity, never by passing a "you're it" token between
agents on contact, because a dropped handoff leaves the whole crowd decaying to
plain wander and the activity silently never fires again; verify by sampling that
every intended mode actually occurs over a few seconds, not just at one instant.
(2) Confine roamers to an explicit field rect that excludes the panels/charts and
the bottom actor row, or they wander over content; clamp x and y and bounce the
heading off the walls.

**Data schema an LLM can emit (confirmed: Aseprite CLI, Stardew schedule data).**
When the scene is data-driven, the smallest workable contract is: a sprite atlas
PNG plus a JSON of named frame tags (`{name:"Walk", from:0, to:3}`, the Aseprite
`--data` format), an entity list of `{atlas, frameTags, pos, facing}`, and an
optional per-entity wander flag or a Stardew-style schedule string
(`<time> <x> <y> [facing] [animation]`). That is enough to compose a believable
animated scene without an engine.

Add to verification: confirm moving characters actually animate (limbs swing or
the frame advances with motion, not a sliding static sprite and not a bob on a
solid block), at least one idle/ambient motion is present so nothing is frozen,
every NPC activity mode actually fires over a few seconds (not just at one
instant), roamers stay inside their field and off the content, and every bundled
asset has a recorded license (and a CREDITS file where required). If a figure's
visibility is gated on a progress value (e.g. an agent-layer fade tied to a beat
that has not run yet), confirm it still shows when a later mode (the dance) turns
it on cold; gate explicit on-states at full opacity, not on the ramp.

## Visual identity

There is no mandatory palette. Choose a look that fits the concept and the
audience, and commit to it. The references show the range: warm Grand-Budapest
pastels with a single signature accent (the canvas builds), an editorial
cream-and-vermilion (the stack trace), a WW2 "We Can Do It" poster (the
skill-builder). Borrow a direction if it fits; invent one if it does not.

If the concept comes with brand constraints (a client colour, a workshop's
house style), honor those over any reference look.

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
