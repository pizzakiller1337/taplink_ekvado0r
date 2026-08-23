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

The six gallery photos come from 4–8 megapixel originals, centre-cropped to 2:3 and
exported to WebP (quality 80) at three widths, one per pixel density:

| Width | Serves | Total for six photos |
| --- | --- | --- |
| `206w` | 1x displays | 49 KB |
| `412w` | 2x displays | 160 KB |
| `618w` | 3x displays | 345 KB |

The browser picks one per photo through `srcset`; verified against the server log at
each density. Nothing in the gallery is ever upscaled. All six are lazy-loaded, so none
of this lands on first paint.

When replacing a photo, export it at all three widths and keep the exact 2:3 ratio —
the grid relies on it to line rows up. Anything wider than 618 px is wasted: no cell
ever renders larger than that.

Still at the old 640 px export, no originals available yet: `soap-01`, `soap-02`,
`avatar.webp` and `og.jpg`. The two soap-camera shots are meant to look lo-fi, so
they are less of a problem there.
