# Technique: text rendering on canvas

A borrowable-technique deep-dive for the `pixel` skill. Read this only when your
chosen medium is an animated canvas that draws text (speech bubbles, labels,
panels of moving content). If your page is plain DOM, you do not need any of this.

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

The label de-overlap idea generalizes beyond bubbles: when several static labels
sit near each other (source markers, axis caps), measure each box from its text and
nudge it along a preferred direction away from already-placed boxes for this frame.
A handful of labels is fine; if you find yourself needing leader lines or a force
solver for dozens of labels, that is the "dense graphs or networks" wrong-tool case
in SKILL.md, not this skill.

**Verification for this technique:** run the LONGEST lines through every bubble and
panel and confirm nothing overflows and no two boxes overlap.
