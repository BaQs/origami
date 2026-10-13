# Theme

The theme is made of tokens. Home Assistant's surfaces read them, and the cards and modules read the same ones. Restyling means changing a few of them in [`origami.yaml`](../themes/origami.yaml).

## Colour

Colour means a device is on, and it sits in the glyph. The field behind a coloured glyph is `tint`, the same colour thinned out. Badges and slider fills use the colour at `fill-strength`.

Which colour a device gets is Home Assistant's own mapping, re-toned per mode. Lamps are amber, heating is deep orange, media is light blue, open windows are cyan, and switches and anything else that is simply on are green. To change a domain, add its `state-<domain>-active-color` line.

Amber is a light colour, and dark enough for a glyph on a light page it turns brown. For that the theme has `lamp-ink` on `lamp-field`, where in light mode the light sits in the field and the glyph stays ink.

## Tokens

| Token | |
| --- | --- |
| `ha-space-1` … `ha-space-5` | Spacing, 4 to 20 px. `card-padding`, `view-bleed` and the rest build on it. |
| `ha-card-border-radius` | Card corners, 22 px. `radius-inner` is derived from it, so inner corners follow. `radius-small` is 8 px. |
| `border-width`, `border-contrast` | Card outlines, off by default |
| `card-shadow`, `card-shadow-flat`, `shadow-elevated` | Elevation per mode. Flat is for cards in pop-ups. |
| `fill-subtle`, `fill-strong`, `fill-loud` | Neutral surfaces mixed from `fill-base`, so they stay warm |
| `fill-active`, `toggle-on` | Ink, for switches and emphasis |
| `tile-background`, `tile-chip-background` | The quiet tile and its raised chip |
| `opacity-disabled` … `opacity-strong` | 0.3, 0.45, 0.6, 0.8 |
| `control-height-mini` … `control-height-lg` | Fixed control heights, 32 to 64 px |
| `tile-size`, `tile-size-lg`, `dot-size` | Icon chips, 48 and 80 px, dots 12 px |
| `icon-size-2xs` … `icon-size-xl` | Icons, 14 to 48 px |
| `ha-font-size-xs` … `ha-font-size-xxl` | Type, 14 to 26 px |
| `primary-font-family` | Montserrat, weights 500 and 700 |
| `duration-fast`, `duration-normal`, `duration-slow`, `ease-standard` | Motion |
| `press-scale` | How far a button shrinks on press |
| `blur-soft`, `blur-strong` | Header, dialogs, navbar |
| `tint-strength`, `fill-strength` | How strong the tint and fills are, per mode |
| `amber-color`, `green-color`, … | Home Assistant's named colours, re-toned per mode |

Each mode sets its own surfaces, text colours, named colours and shadows. Paper Buttons Row and Bubble Card get their variables right before the modes. A few old names like `lamp-color` are still there for older cards and will go.
