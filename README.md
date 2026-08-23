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

Every photo is capped at 640 px on its long side — that is the resolution Taplink
exported, and no larger original exists in this repository's history. On a phone with a
2x or 3x display these files are already being upscaled, so **the gallery must never
render a photo larger than roughly 206 CSS px wide**, or the softness becomes obvious.

That constraint is why the gallery is a plain two-column grid with no row spans: a cell
taller than ~309 px forces the browser to stretch a 640 px image past 1:1.

To go further, replace the files with 2x exports (roughly 854 × 1280) and add
`srcset`/`sizes` to each `<img>`.
