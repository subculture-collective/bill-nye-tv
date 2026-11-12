# Bill Nye TV — Brand guide

Bill Nye TV: An independent archive of Bill Nye programs, with source links and scheduled replays.

Status: Proposed on November 12, 2026 after a review found the earlier Lab reel pack (cream and dark green, a flask in a film frame) unrelated to the live site. This pack follows the site's television identity instead. It is an agent proposal awaiting owner review; the owner has not decided whether the site header keeps the program's logo or uses this mark. Sample social layouts remain unapproved for publication.

## Mark

A television set on two feet; on its screen a ringed planet carrying a question mark. An original archive mark drawn from the site's own television and planet, not the program logo. It is an original geometric drawing in the maintained generator; no third-party logo or collection artwork is traced. `logos/mark.svg` is the colour version, `mark-mono-dark.svg` is one light colour for dark backgrounds and `mark-mono-light.svg` is one dark colour for light backgrounds.

- Clear space: one quarter of the mark's height on every side.
- Minimum size: 32 pixels wide on screen. The browser icon is a separately framed derivative for 16, 32 and 48 pixels. It has heavier lines and a solid planet without the question mark, which closes up below 32 pixels; `product/favicon.svg` and `favicon.ico` carry it.
- No gradients, rotation, outlines, shadows or recolouring beyond the three files above.

## Palette

| Role | Colour | Use | Contrast on background |
| --- | --- | --- | --- |
| Background | #15172b | Canvas | — |
| Text | #f6f0d8 | Copy and the mark | 15.45:1 |
| Accent | #ffe344 | Lit element and emphasis | 13.72:1 |
| Secondary | #29bde3 | Labels and supporting lines | 7.97:1 |

Site colours beyond the four roles, with the text pair each is used in:

| Token | Colour | Use | Contrast |
| --- | --- | --- | --- |
| surface | #22243a | Panels and cards on the canvas | text on surface: 13.30:1 |
| phosphor | #9bffd2 | Text on the television screen | phosphor on screen: 12.85:1 |
| screen | #0d2a22 | The television screen behind phosphor text | phosphor on screen: 12.85:1 |
| band-ink | #15172b | Text on the yellow band and yellow buttons | band-ink on accent: 13.72:1 |
| note-ink | #102639 | Text on a cyan note card | note-ink on secondary: 6.98:1 |

Text on the background must be at least 4.5:1 and the accent at least 3:1; the generator fails the build otherwise.

## Typography

- Display: Public Sans, weight 700, for the name and headlines.
- Body: Public Sans, regular, for descriptions.
- Label: Liberation Mono, regular, upper case with slight tracking, for small labels such as `BILL NYE TV / BY SUBCULT`.
- The site sets headings in Arial and labels and screen text in Courier New, both from the visitor's system; neither may be redistributed. Pack artwork uses Public Sans for Arial and Liberation Mono, which has Courier New's metrics, for Courier New. Both are under the SIL Open Font License 1.1.
- Spacing steps: 8, 16, 24, 32, 48, 64 and 96 pixels.

## Files

`logos/` holds the mark variants and the lockup; `product/` the mark copy, the 1440×900 hero and the 1200×630 link preview, both showing the television with phosphor text on its screen; `social/` the 1500×500 banner, 1080×1080 square and 512×512 avatar. Layout copy is the one-line description and is marked SAMPLE where it is a sample. Text in every SVG is converted to paths, and `fonts/` carries the fonts with their licences for new artwork.

Rebuild only this pack with `python3 studio/brand-tools/remaining_site_packs.py --only bill-nye-tv` from the repository root. Do not edit the PNG exports by hand.

## Product relationship

The product repository owns its interface and behavior. This pack supplies identity artwork, icons and link previews; it does not replace product controls, source notices, rights records or color semantics. Use the product's existing type stack in the application. Artwork uses the licensed bundled fonts.
