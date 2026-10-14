# Changelog

## 2.0 · The Unfolding

<img alt="The Origami dashboard on two phones, one in light mode and one in dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/hero.webp">

Version 1.0 contained the theme and one long README. Version 2.0 adds the dashboard with its first pop-ups, the helpers, the font and the Bubble Card modules, all as separate files. A new README explains every part.

### New

- The phone dashboard is now one file, [`dashboard/dashboard.yaml`](dashboard/dashboard.yaml). It contains the greeting, notifications, weather, favorites, rooms, climate table and navigation bar.
- The helpers are now a package, [`packages/origami.yaml`](packages/origami.yaml). It contains the template sensors and scripts for room climate, averages, counters, the greeting, the active speaker and the pop-ups.
- The dashboard has pop-ups for a room, the TV and a fan, together with the scripts and helpers they use.
- Eleven modules give Bubble cards the Origami look. They are still experimental.
- The Montserrat font is included in `www/fonts`. It loads from your Home Assistant instead of from Google's servers.
- The new README explains the setup step by step. It has a screenshot of every part and lists the entities that each part uses.

### Theme

- The theme has a new set of tokens. Spacing and text sizes use the same names as Home Assistant, such as `ha-space-*` and `ha-font-size-*`.
- Home Assistant's named colors have new tones for light and dark mode. The icon shows the color, and the background behind it is a faint tint of the same color.
- Switches and other entities without a color of their own are now green instead of amber when they are on. Open windows are cyan.
- Shadows and fills are defined for each mode and mixed from a warm base color.

### Breaking changes

Some tokens from version 1.0 no longer exist. If your own cards use them, replace them with the tokens in this table.

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
| `shadow-color` | `shadow-tint`, as an RGB triplet such as `60, 50, 40` |
| `rise-offset` | No replacement |

### Upgrading

1. Update Origami in HACS and reload the themes.
2. To use the font, copy `www/fonts` to `/config/www/fonts` and add `/local/fonts/montserrat.css` as a **Stylesheet** resource.
3. To use the dashboard, follow the steps under [Setup](README.md#setup).
