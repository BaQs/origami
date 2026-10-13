# Changelog

## 2.0 · The Unfolding

<img alt="The Origami dashboard on two phones, one in light and one in dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/hero.webp">

1.0 was the theme and one very long README. 2.0 unfolds it into files you can use: the dashboard, its helpers, the font and Bubble Card modules, with a new README that walks through every part.

### New

- **Dashboard.** The phone dashboard in one file, [`dashboard/dashboard.yaml`](dashboard/dashboard.yaml): greeting and notifications, weather, favorites, rooms, a climate table and a navbar.
- **Helpers.** The template sensors behind it as a package, [`packages/origami.yaml`](packages/origami.yaml): room climate, averages, counters, the greeting and the active speaker.
- **Bubble Card modules.** Eleven modules that give Bubble Card the Origami look. Still experimental.
- **Montserrat included.** The font ships in `www/fonts` and loads from your Home Assistant instead of Google's servers.
- **New README.** Setup step by step, a screenshot of every part, and what each part uses.

### Theme

- New token set. Spacing and text sizes use Home Assistant's own names, like `ha-space-*` and `ha-font-size-*`.
- Home Assistant's named colours, re-toned for light and dark. Colour sits in the icon, on a tint of the same colour.
- Switches and anything else that is simply on are green instead of amber. Open windows are cyan.
- Shadows and fills per mode, mixed from a warm base.

### Breaking changes

Some 1.0 tokens are gone. If your own cards use them, switch to these:

| 1.0 | 2.0 |
| --- | --- |
| `border-radius`, `br-inner`, `br-circle` | `ha-card-border-radius`, `radius-inner`, `radius-circle` |
| `br-pill` | `ha-border-radius-pill`, from Home Assistant |
| `gap-xs`, `gap`, `gap-lg`, `section-gap` | `ha-space-1`, `ha-space-2`, `ha-space-3`, `ha-space-5` |
| `card-padding-lg` | `ha-space-4` |
| `font-weight-regular`, `font-weight-medium`, `font-weight-bold`, `font-weight-heavy` | `ha-font-weight-medium`, `ha-font-weight-bold` |
| `font-size-title`, `font-size-display` | `ha-font-size-xl`, `ha-font-size-3xl` |
| `text-color`, `card-background` | `primary-text-color`, `ha-card-background` |
| `fill-accent-quiet` | `accent-color-background` |
| `shadow-color` | `shadow-tint`, as an RGB triplet like `60, 50, 40` |
| `rise-offset` | removed |

### Upgrading

1. Update Origami in HACS and reload themes.
2. For the font, copy `www/fonts` to `/config/www/fonts` and add `/local/fonts/montserrat.css` as a **Stylesheet** resource.
3. For the dashboard, follow [Setup](README.md#setup).
