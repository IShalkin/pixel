# assets

Optional sprite art the `pixel` skill can use when a demo wants real pixel
characters instead of procedurally drawn ones. Drawing procedurally is always
the zero-dependency, zero-license-risk default; this folder is for when you
deliberately want hand-drawn LPC art.

## lpc/

A small starter set from the Liberated Pixel Cup collection, pulled via the
[Universal LPC Spritesheet Character Generator](https://github.com/LiberatedPixelCup/Universal-LPC-Spritesheet-Character-Generator).

```
lpc/
  body/  male_walk.png  male_idle.png  female_walk.png  female_idle.png
  hair/  plain_walk.png plain_idle.png
  CREDITS.txt
```

Each PNG is a standard LPC sheet: 64x64 per frame, walk rows ordered
up / left / down / right, 9 frames per row. The hair layer shares the body
grid, so you draw the body frame then the matching hair frame on top.

## License (read before using)

LPC art is **NOT CC0**. It is OGA-BY 3.0 / CC-BY-SA 3.0 / GPL 3.0, which means:

- attribution is required (see `lpc/CREDITS.txt`), and
- derivatives are **share-alike**: anything you ship using these must be
  released under compatible terms.

If you do not want share-alike obligations, do not use these files. Draw the
characters procedurally instead, which is what the reference demos do.
