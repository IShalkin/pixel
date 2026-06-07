# pixel

A skill for Claude Code that turns one complex or abstract concept into a single
self-contained, interactive HTML page that teaches it through a concrete
SITUATION the audience steps through. Method-first: the audience, the situation,
and the visual style are worked out fresh for each concept, never cloned from an
example.

The output is one HTML file you open in a browser. It has a self-playing story
you can Play, Pause, and replay, plus controls the audience can manipulate to
probe the idea themselves. No build step, no framework, no dependencies.

## What you can build with it

You describe a hard idea and who it is for; the skill produces the page. Examples
of the kind of thing it makes:

- "Show how an agent loop calls tools, reads results, and decides to stop."
- "Explain how one request falls through five service layers and where it fails."
- "Teach why a retry storm forms and how a circuit breaker stops it."
- "Walk a workshop through how a data pipeline drops malformed rows."

## Use it

This is a Claude Code skill (it also works in compatible skill hosts).

1. Put this folder where Claude Code looks for skills:
   ```
   ~/.claude/skills/pixel/        (macOS / Linux)
   %USERPROFILE%\.claude\skills\pixel\   (Windows)
   ```
   so that `~/.claude/skills/pixel/SKILL.md` exists.
2. In Claude Code, invoke it by name with `/pixel`, or just ask in plain words,
   for example: "make an interactive page that explains X to first-year students."
3. The skill will ask who the audience is and in what setting (a solo reader vs a
   presenter on a stage), then build the page and hand you the file path to open.

You do not need the `assets/` folder to use the skill. Characters are drawn
procedurally by default; sprite art is opt-in (see Licensing).

## Layout

```
SKILL.md      the skill itself (the full method and the borrowable techniques)
reference/    five worked example pages (study the METHOD, do not clone them),
              plus two technique deep-dives read on demand:
              technique-canvas-text.md (canvas text wrap/clip/de-overlap) and
              technique-aliveness.md (assets, walk/dance rig, NPC motion)
assets/       optional LPC sprite art; procedural drawing is the default
```

## The reference pages

Five finished pages that prove the method works across very different looks and
subjects. Read one or two whose approach is closest to your concept, for ideas on
making an abstraction concrete, then build your own. Do not copy them.

| Page | The situation it invented | Live preview |
|---|---|---|
| `multi-agent-team.html` | a product team's working day, reinvented by per-person agents | [open](https://htmlpreview.github.io/?https://github.com/IShalkin/pixel/blob/main/reference/multi-agent-team.html) |
| `canvas-sprite-stations.html` | a character walking desk to desk to do one job, graded by a second actor | [open](https://htmlpreview.github.io/?https://github.com/IShalkin/pixel/blob/main/reference/canvas-sprite-stations.html) |
| `canvas-accenture-slide.html` | an agent assembling one branded slide, checked by a critic | [open](https://htmlpreview.github.io/?https://github.com/IShalkin/pixel/blob/main/reference/canvas-accenture-slide.html) |
| `dom-stack-trace.html` | one request falling through five named layers | [open](https://htmlpreview.github.io/?https://github.com/IShalkin/pixel/blob/main/reference/dom-stack-trace.html) |
| `dom-skill-builder.html` | a coloring-book of rules you switch on and off | [open](https://htmlpreview.github.io/?https://github.com/IShalkin/pixel/blob/main/reference/dom-skill-builder.html) |

(The previews render through htmlpreview.github.io. You can also clone the repo
and open any file directly in a browser.)

## What the skill covers

The full method lives in [`SKILL.md`](SKILL.md). In short, it walks you through:

- **Ask who it is for first.** Audience and setting set the vocabulary, depth, and
  pacing; the skill does not design in a vacuum.
- **Find the situation and name the "aha"** before any code.
- **Build the self-playing story as the primary feature,** then add a sandbox of
  controls the audience can manipulate. Not a movie, not a bare slideshow.
- **Draw the field, do not just label it.** When the concept is a gradient, a
  flow, or a field, render that quantity as something the eye can see change, not
  a text label pointing at empty space.
- **Know when this is the wrong tool.** A hand-drawn stepped scene is for showing a
  mechanism. For precise quantitative charts, dense graphs and networks, true 3D,
  or thousands of particles, the skill tells you to reach for the right tool (a
  charting library, a layout engine) or reframe the concept, rather than ship a
  fragile hand-rolled version.
- **Borrowable techniques** to lift only when your page needs them: crisp canvas
  setup, pause-aware abortable timing, manual canvas text wrapping and clipping,
  a one-rig walk/dance figure, emotes, parallax, goal-driven NPC steering, a DOM
  verdict engine, and a dependency-free ZIP writer.
- **A verification checklist** to run before calling a page done.

## Licensing note

`SKILL.md` and the `reference/` pages are this project's own work. The sprites
under `assets/lpc/` are Liberated Pixel Cup art and are **NOT CC0**: they carry
OGA-BY 3.0 / CC-BY-SA 3.0 / GPL 3.0 with required attribution and share-alike.
Because they are bundled here, treat this repository as share-alike where those
files are concerned, and keep `assets/lpc/CREDITS.txt` with them. If you want no
license obligations, ignore `assets/` and draw characters procedurally, which is
what the reference demos do.
