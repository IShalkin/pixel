# Technique: making the scene feel alive (assets and motion)

A borrowable-technique deep-dive for the `pixel` skill. Read this only when your
page has characters or a scene that should feel animated and alive (an actor doing
a loop, a crowd, a living background). All of it is optional and per-concept; a DOM
verdict page or a static diagram needs none of it.

The default failure is a static scene: a character that slides without animating,
flat affect, a dead background. Aliveness is cheap if you reach for the right
move. Use `assets/` for real sprite art (the references already fail-soft on
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

**Verification for this technique:** confirm moving characters actually animate
(limbs swing or the frame advances with motion, not a sliding static sprite and not
a bob on a solid block), at least one idle/ambient motion is present so nothing is
frozen, every NPC activity mode actually fires over a few seconds (not just at one
instant), roamers stay inside their field and off the content, and every bundled
asset has a recorded license (and a CREDITS file where required). If a figure's
visibility is gated on a progress value (e.g. an agent-layer fade tied to a beat
that has not run yet), confirm it still shows when a later mode (the dance) turns
it on cold; gate explicit on-states at full opacity, not on the ramp.
