# taplink_ekvado0r

Link-in-bio page for photographer Katya Ekvador ([@ekvado0r](https://instagram.com/ekvado0r)) — live at [ekvado0rprice.online](https://ekvado0rprice.online/).

A static site served by GitHub Pages from the `main` branch.

## Layout

| Path | Contents |
| --- | --- |
| `index.html` | The entire page — markup, CSS and JS are inlined |
| `assets/` | Photos (WebP/JPEG), Open Graph preview, icons |
| `assets/fonts/` | woff2 fonts, subset to Cyrillic + Latin |
| `CNAME` | Custom domain binding |

The page makes **no external requests**. Fonts, images and icons are all local, so
nothing breaks if a third-party host goes down, and there is no build step — edit
`index.html` and push.

## Local preview

Relative paths need a real HTTP origin, so open it through a server rather than
as a `file://` URL:

```bash
python -m http.server 8099
```

Then visit http://localhost:8099.

## Fonts

Both faces are subset and converted to woff2, which is why they are small:

| Face | Used for | Size |
| --- | --- | --- |
| `eugusto.woff2` | Headings | 9.5 KB |
| `nintendo-ntlg-db.woff2` | Body text | 13.6 KB |

The body face is originally a Japanese font; roughly 6,900 of its 8,480 glyphs are
kanji and kana that this page never renders. Subsetting to Cyrillic, Latin, digits and
punctuation cut it from 2.4 MB to 13.6 KB. If you ever add characters outside that
range — the ruble sign `₽` is one, it is missing from the original font — they will
fall back to Georgia.

Note on licensing: the display face identifies itself as "Eugusto Free Personal Use",
and the body face is a Nintendo system font redistributed by a font aggregator. Worth
reviewing before any commercial use.

## Images

The gallery is a two-column grid. Each cell is 206 CSS px wide at the 460 px layout
width, and `calc(50vw - 24px)` below it — that is what the `sizes` attribute encodes.

Every photo comes from a 4–8 megapixel original, exported to WebP (quality 80) at
three widths, one per pixel density. The gallery shots are centre-cropped to 2:3; the
two soap-camera shots keep their native ratio, since each spans the full width and has
no neighbour to line up with.

| | 1x | 2x | 3x | Grid slot |
| --- | --- | --- | --- | --- |
| Six gallery photos | `206w` — 49 KB | `412w` — 160 KB | `618w` — 345 KB | 206 CSS px |
| Two soap photos | `424w` — 32 KB | `848w` — 135 KB | `1272w` — 408 KB | 424 CSS px |
| **Whole page, scrolled** | **81 KB** | **296 KB** | **753 KB** | |

The browser picks one per photo through `srcset`; verified against the server log with
a fresh profile at each density. Nothing is ever upscaled. Every photo is lazy-loaded,
so none of this lands on first paint — that stays at about 49 KB.

Click-to-enlarge loads a separate larger file: `-1200` for the gallery (2:3), and the
`-1272` grid image for the soap shots. While it downloads, the thumbnail already in
cache is shown stretched to the final geometry, so the view opens instantly and does
not jump when the full frame arrives.

When replacing a gallery photo, export all three widths plus the `-1200`, and keep the
exact 2:3 ratio — the grid relies on it to line rows up.

Still at the old 640 px export, no originals yet: `avatar.webp` and `og.jpg`.
