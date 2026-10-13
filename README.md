<img alt="Origami, a calm theme and dashboard for Home Assistant" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/hero.webp">

<p>
<a href="https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme"><img height="28" alt="HACS: Custom" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/badge-hacs.webp"></a>
<a href="https://www.home-assistant.io"><img height="28" alt="Home Assistant 2026.10" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/badge-ha.webp"></a>
</p>

A calm Home Assistant theme, and a phone dashboard folded out of [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) and [Bubble Card](https://github.com/Clooos/Bubble-Card). Plus a set of Bubble Card modules made for the theme.

| | |
| --- | --- |
| **Theme** | Soft, warm surfaces in light and dark, and colour only where something is on. Works with any dashboard. |
| **Dashboard** | Weather, favorites, rooms and a navbar for phones. Nothing jumps while it loads. |
| **Bubble modules** | The theme's look for Bubble Card. Still experimental. |

> [!NOTE]
> This is a personal dashboard that has grown over years. Some things are done in very specific ways, mostly to avoid layout shifts. A desktop layout is still in the works. Take what you like.

[Setup](#setup) · [Dashboard](#dashboard) · [Bubble modules](#bubble-modules) · [Under the hood](#under-the-hood) · [Help](#help)

## Setup

<details>
<summary><b>Theme only</b></summary>

<br>

1. Add this to `configuration.yaml` and restart. If there is a `frontend:` already, add the `themes` line below it.

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
   ```

2. Open the theme in HACS, confirm with **Add** if asked, then **Download**.

   [![Open your Home Assistant instance and open this repository inside HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme)

3. Run [`frontend.reload_themes`](https://my.home-assistant.io/redirect/developer_call_service/?service=frontend.reload_themes) and pick **Origami** in your [profile](https://my.home-assistant.io/redirect/profile/).
4. For the full look, copy [`www/fonts`](https://github.com/hazymorning/origami/tree/main/www/fonts) to `/config/www/fonts` and add `/local/fonts/montserrat.css` as **Stylesheet** in [Resources](https://my.home-assistant.io/redirect/lovelace_resources/).

</details>

<details>
<summary><b>Whole dashboard</b></summary>

<br>

Tested with Home Assistant 2026.10. You need [HACS](https://hacs.xyz) and access to `/config`, for example through the Samba share or Studio Code Server app.

<details>
<summary>1. Copy three folders</summary>

<br>

Download this repository (**Code**, **Download ZIP**) and copy into `/config`:

| Folder | |
| --- | --- |
| `www/fonts` | The font |
| `bubble_card/modules` | The Bubble modules |
| `packages` | The helper sensors |

</details>

<details>
<summary>2. Edit configuration.yaml</summary>

<br>

Add this and restart. If `homeassistant:` or `frontend:` is already there, add the lines below it.

```yaml
homeassistant:
  packages: !include_dir_named packages

frontend:
  themes: !include_dir_merge_named themes
```

</details>

<details>
<summary>3. Install from HACS</summary>

<br>

Open each one, confirm with **Add** if asked, then **Download**.

| | For | |
| --- | --- | --- |
| [Origami](https://github.com/hazymorning/origami) | The theme | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme) |
| [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) | Greeting, menu, climate table and room cards | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=jcwillox&repository=lovelace-paper-buttons-row&category=plugin) |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | Favorites and section headers | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card&category=plugin) |
| [Bubble Card Tools](https://github.com/Clooos/Bubble-Card-Tools) | Loads the Bubble modules | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card-Tools&category=integration) |
| [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) | The bar at the bottom | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=joseluis9595&repository=lovelace-navbar-card&category=plugin) |
| [Origami Notifications](https://github.com/hazymorning/origami_notifications) | Notifications next to the greeting | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_notifications&category=plugin) |
| [Origami Weather](https://github.com/hazymorning/origami_weather) | The weather card | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_weather&category=plugin) |

Restart once more, then [add Bubble Card Tools](https://my.home-assistant.io/redirect/config_flow_start/?domain=bubble_card_tools) as an integration.

</details>

<details>
<summary>4. Theme and font</summary>

<br>

- In your [profile](https://my.home-assistant.io/redirect/profile/), pick **Origami** as theme.
- In [Resources](https://my.home-assistant.io/redirect/lovelace_resources/), add `/local/fonts/montserrat.css` as **Stylesheet**.

</details>

<details>
<summary>5. Add the dashboard</summary>

<br>

1. Open [Dashboards](https://my.home-assistant.io/redirect/lovelace_dashboards/), **Add dashboard**, **New dashboard from scratch**, and open it.
2. From the three dots, choose **Edit dashboard**, then **Raw configuration editor**.
3. Replace everything with [`dashboard.yaml`](dashboard/dashboard.yaml) and save.

Without Origami Header, delete the card under `# Fold-out menu` and the route under `# Opens the Origami Header menu` first.

</details>

<details>
<summary>6. Use your entities</summary>

<br>

The dashboard uses examples like `light.living_room`. In the raw editor, Ctrl+F (⌘F on a Mac) finds and replaces them. Each part below lists what it reads. Rename the same entities in `/config/packages/origami.yaml`, then restart.

</details>

<details>
<summary>Optional: Origami Header</summary>

<br>

It hides Home Assistant's header and turns it into a menu that folds out from the top, opened by **More** in the navbar. It's a bit experimental, but it's the only way I've found to hide the header completely, without a flicker. The dashboard works fine without it.

1. [Open it in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_header&category=plugin) and choose **Download**. It needs Home Assistant 2026.9 or newer.
2. Add the `extra_module_url` lines to `configuration.yaml` and restart:

   ```yaml
   frontend:
     extra_module_url:
       - /hacsfiles/origami_header/origami-header.js
   ```

Edit mode is then in its menu, or add `?edit=1` to the address.

</details>

</details>

## Dashboard

One sections view, made for phones. Each part starts with a comment in [`dashboard.yaml`](dashboard/dashboard.yaml), like `# Favorites`.

<img width="760" alt="The Origami dashboard on a phone, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/dashboard.webp">

### Greeting and menu

The next event, a greeting and the notifications. With the optional Origami Header, **More** in the navbar slides a menu down from the top, over the dashboard.

<img width="800" alt="The next event, the greeting and the notifications card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/header.webp">

<img width="800" alt="Tap More in the navbar and the menu slides down from the top, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/menu.webp">

<details>
<summary>How it works</summary>

<br>

- The greeting is a Paper Buttons Row with the next calendar entry above it.
- [Origami Notifications](https://github.com/hazymorning/origami_notifications) turns through what needs attention. A tap opens the list.
- The menu holds a server card and buttons for files, automations, the sidebar and edit mode. **More** in the navbar opens it.
- The server chip is green when all is good, accent coloured with updates, and orange or red when disk, memory, processor or backup need a look.

</details>

<details>
<summary>What it needs</summary>

<br>

| Entity | For |
| --- | --- |
| `calendar.household` | The next entry |
| `sensor.greetings` | The greeting |
| `weather.home` | Rain ahead in the notifications |
| `sensor.system_monitor_*` | Processor, memory, disk and last boot, from [System Monitor](https://www.home-assistant.io/integrations/systemmonitor/) |
| `sensor.backup_last_successful_automatic_backup` | The last backup |

</details>

### Weather

An [Origami Weather](https://github.com/hazymorning/origami_weather) card with a calm colour per condition.

<img width="800" alt="The weather card in daylight and at night" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/weather.webp">

<details>
<summary>How it works</summary>

<br>

- Each condition has its own colour for light and dark, set in `background_thresholds`. No sun, moon, clouds or other effects.
- Top left is the next sunset, or sunrise while the sun is down.
- The temperature comes from the weather entity. For an outdoor sensor of your own, give that text an `entity` instead of the `attribute`.
- The house sits 20 px from the top, right and bottom. It's the card's demo house, a picture of your own home needs its own offsets, see [layouts](https://github.com/hazymorning/origami_weather#layouts).

</details>

<details>
<summary>What it needs</summary>

<br>

| Entity | For |
| --- | --- |
| `weather.home` | Condition, colour and temperature |
| `sun.sun` | Next sunset or sunrise |

</details>

### Favorites

Shortcuts that only show up while they matter, made with Bubble Card's [Header](bubble_card/README.md#header) and [Tiles](bubble_card/README.md#tiles) modules.

<img width="800" alt="Six favorite tiles in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/favorites.webp">

<details>
<summary>How it works</summary>

<br>

- Each tile is a sub-button of one Bubble card. Visibility conditions decide when it shows, and hidden tiles leave no gap.
- The oven comes first while it's on. The vacuum moves up while it cleans and shows its progress.
- The fan shows in warm weather, music late in the evening, dinner in the afternoon with the recipe photo.
- Bath and printer count down the minutes left.

</details>

<details>
<summary>What it needs</summary>

<br>

| Tile | Entities |
| --- | --- |
| Oven | `sensor.oven` |
| Vacuum | `vacuum.robot`, `number.robot_cleaning_progress` |
| Fan | `fan.bedroom` |
| Dinner | `sensor.dinner` with a `recipe.image` attribute, like from [Tandoor](https://tandoor.dev) |
| Music | `media_player.bedroom` |
| Bath | `input_boolean.bath_time`, `input_datetime.bath_time_end` |
| Printer | `switch.printer`, `input_datetime.printer_end` |

</details>

### Rooms

A card per room. The first four always show, **All rooms** shows the rest.

<img width="800" alt="The rooms header and four room cards" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/rooms.webp">

<details>
<summary>How it works</summary>

<br>

- Each room card is one Paper Buttons Row, and the whole card opens the room's pop-up.
- The chip lights up while the room's lights are on, the dot shows the comfort colour.
- The window icon only shows with a window contact, and has its own tap.
- The header shows the home's average and the open windows.

</details>

<details>
<summary>What it needs</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.climate_<room>` | Temperature, humidity and comfort colour per room |
| `sensor.climate_home` | Average and open windows in the header |
| `binary_sensor.<room>_window` | Window contacts |
| `light.<room>` | The lit chip |
| `input_boolean.all_rooms` | **All rooms** |

The sensors and toggles are in the [helpers](#under-the-hood).

</details>

### Climate table

Every room's climate at a glance. It folds out under the rooms header.

<img width="800" alt="The rooms header with the climate table open" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/climate.webp">

<details>
<summary>How it works</summary>

<br>

- A tap on a chip or the arrow in the rooms header opens and closes it.
- The table is one Paper Buttons Row as a six-column grid: comfort dot and name, window, thermostat, temperature, humidity. Keep the names short, so it fits on a phone.
- A heating thermostat and an open window tint. Arrows mark the warmest, coldest, most humid and driest room.
- The zebra stripes are one gradient, sized to the fixed row height.

</details>

<details>
<summary>What it needs</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.climate_<room>` | Temperature, humidity, rank and comfort colour per room |
| `binary_sensor.<room>_window` | Window contacts |
| `climate.<room>` | Thermostats |
| `input_boolean.room_climate_card` | Opens the table |

</details>

### Navbar

Music, TV, lights, favorites and the menu, with what plays right above.

<img width="800" alt="The navbar with the now playing card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar.webp">

<details>
<summary>How it works</summary>

<br>

- A [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card). Routes light up in their domain's colour while something is on, badges count playing speakers and lights.
- Holding **Lights** opens the scenes, the active one is amber.
- The now playing card shows the speaker that matters right now, picked by `sensor.active_speaker`.
- Navbar Card templates are JavaScript: `states['sensor.x'].state`, not `states('sensor.x')`.

<img width="600" alt="The scenes that open from the lights route" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar-scenes.webp">

</details>

<details>
<summary>What it needs</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.active_speaker_count`, `sensor.lights_on_count` | The badges |
| `sensor.active_speaker` | The now playing card |
| `media_player.living_room_tv`, `media_player.tv_ump` | The TV route and its now playing card |
| `light.all_lights`, `script.lights_toggle` | The all lights button in the scenes |
| `scene.living_room_*`, `sensor.living_room_scene` | The scenes and the active one |

</details>

## Bubble modules

Eleven modules give Bubble Card the Origami look. They're still experimental. Options and examples are in the [module reference](bubble_card/README.md).

<img width="800" alt="A pop-up built from the modules, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/module-popup.webp">

<details>
<summary>All modules</summary>

<br>

| Module | For |
| --- | --- |
| [Base](bubble_card/README.md#base) | Frame, type and colour of every card |
| [Light](bubble_card/README.md#light), [Fan](bubble_card/README.md#fan), [Climate](bubble_card/README.md#climate) | Lights, fans with speed keys, thermostats |
| [Media](bubble_card/README.md#media), [Speaker Select](bubble_card/README.md#speaker-select) | Speakers and TVs, speaker rows for a music pop-up |
| [Slider](bubble_card/README.md#slider), [Button](bubble_card/README.md#button) | A slider on its own, rows and grids of buttons |
| [Header](bubble_card/README.md#header), [Tiles](bubble_card/README.md#tiles), [Popup](bubble_card/README.md#popup) | Section headers, tile grids, pop-ups |

</details>

## Under the hood

<details>
<summary><b>Helpers</b></summary>

<br>

Template sensors do the thinking, the cards only show the result. They're all in [`packages/origami.yaml`](packages/origami.yaml).

| Helper | Gives |
| --- | --- |
| `sensor.climate_<room>` | Rounded temperature and humidity, rank, comfort colour |
| `sensor.climate_temperature_peaks`, `sensor.climate_humidity_peaks` | The warmest and most humid room, the lowest as attribute |
| `sensor.temperature_average`, `sensor.humidity_average`, `sensor.climate_home` | Averages, open windows, front door |
| `sensor.windows_open_count`, `sensor.lights_on_count`, `sensor.active_speaker_count` | Counters |
| `sensor.greetings` | The greeting |
| `sensor.active_target`, `sensor.active_speaker` | The speaker that matters and what it plays |
| `input_boolean.room_climate_card`, `input_boolean.all_rooms`, `input_select.selected_speaker` | Toggles and the fallback speaker |

<details>
<summary>Room climate sensor</summary>

<br>

One per room. The state is the temperature with one decimal, the attributes are what the cards read.

```yaml
template:
  - sensor:
      - name: Climate | Living room
        unique_id: climate_living_room
        icon: mdi:thermometer
        state: >
          {% set t = states('sensor.living_room_temperature') | float(none) %}
          {{ t | round(1) if t is not none else '?' }}
        attributes:
          temperature: >
            {% set t = states('sensor.living_room_temperature') | float(none) %}
            {{ t | round(0) | int if t is not none else '?' }}
          humidity: >
            {% set h = states('sensor.living_room_humidity') | float(none) %}
            {{ h | round(0) | int if h is not none else '?' }}
          temp_rank: >
            {{ 'warmest' if is_state('sensor.climate_temperature_peaks', 'living_room')
               else 'coldest' if is_state_attr('sensor.climate_temperature_peaks', 'lowest', 'living_room')
               else 'normal' }}
          humidity_rank: >
            {{ 'most_humid' if is_state('sensor.climate_humidity_peaks', 'living_room')
               else 'driest' if is_state_attr('sensor.climate_humidity_peaks', 'lowest', 'living_room')
               else 'normal' }}
          color_code: >
            {% set t = states('sensor.living_room_temperature') | float(20) %}
            {% set scale = [(12, '114, 143, 168'), (15, '124, 151, 169'), (17, '139, 162, 168'),
               (18.5, '154, 174, 163'), (22, '136, 163, 126'), (24, '153, 166, 119'),
               (25, '174, 167, 111'), (26, '187, 159, 104'), (27, '192, 147, 95'),
               (28, '191, 138, 95'), (29, '187, 126, 93'), (30.5, '179, 115, 92'),
               (32, '170, 106, 90'), (99, '156, 96, 86')] %}
            rgba({{ (scale | selectattr(0, '>=', t) | first)[1] }}, 0.7)
```

- A missing source gives `?`, so the card keeps its size instead of collapsing.
- The comfort colour runs from blue at 12° to red above 32°, with some transparency for both modes.
- The peaks sensors return `none` while all rooms read the same, so two rooms don't fight over the arrow.

</details>

<details>
<summary>Active speaker</summary>

<br>

The navbar reads one sensor that already picked the interesting speaker: a playing one, then the TV or Bluetooth, then a paused one, then `input_select.selected_speaker`. A playing group leader wins over its members.

| Attribute | |
| --- | --- |
| `chip` | `playing_art`, `playing_plain`, `paused_art`, `paused_plain`, `tv_art`, `tv_plain` or `off` |
| `title` | `Title \| Artist`, never empty and never stale |
| `speaker` | Short name, with a count while grouped, like `Living +1` |
| `group` | `yes` or `no` |
| `artwork` | The cover or TV artwork, `none` without |

Written for LinkPlay speakers with a Music Assistant twin (`media_player.<name>_mass`). Music Assistant also reports playing for the speaker's own sources like Bluetooth, so its title and cover only count without one. Other speakers use their own. `sensor.tv_source` is optional and names what the TV plays over optical.

</details>

</details>

<details>
<summary><b>Patterns</b></summary>

<br>

Paper Buttons Row and Bubble Card get folded into cards and layouts. The same few ideas repeat everywhere.

<details>
<summary>The row is the card</summary>

<br>

Paper Buttons Row draws no card by itself. Give the row a background, radius and padding and it becomes a card. Add `display: grid` and it becomes a layout. The room cards, the climate table and the server card are each one row.

```yaml
type: custom:paper-buttons-row
styles:
  padding: var(--ha-space-4)
  background-color: var(--fill-subtle)
  border-radius: var(--ha-card-border-radius)
buttons:
  - entity: light.living_room
```

</details>

<details>
<summary>Dumb cards, smart sensors</summary>

<br>

Cards don't calculate. A sensor turns everything into one word, and the card maps the word to a look:

```yaml
- entity: sensor.climate_living_room
  icon: mdi:thermometer
  name:
    attribute: temperature
    postfix: °
  state:
    attribute: temp_rank
  state_icons:
    warmest: mdi:arrow-up
    coldest: mdi:arrow-down
```

</details>

<details>
<summary>No layout shifts</summary>

<br>

- **Fixed heights.** A long text never pushes anything.
- **States swap colours, not sizes.**
- **Helpers always return a value**, `?` instead of an empty string.
- **Elements that come and go** leave no gap, or keep their box and scale.
- **Values in the browser.** JavaScript in `${ }` is there on the first frame, Jinja waits for the server.
- **Animations only on user action**, never on page load.
- **A local font**, because a font that loads late reflows all text.
- **Grid instead of stack cards** where a wrapper is needed.

</details>

<details>
<summary>Whole card tap targets</summary>

<br>

One invisible button stretches over the whole card and catches the tap. Everything else gets `pointer-events: none`, and single elements can take taps again with `pointer-events: all`.

```yaml
- entity: light.living_room
  layout: name
  name: false
  tap_action:
    action: navigate
    navigation_path: '#popup_living_room'
  styles:
    button:
      position: absolute
      inset: 0px
      z-index: 3
      pointer-events: all
      border-radius: var(--ha-card-border-radius)
```

</details>

<details>
<summary>Visibility instead of logic</summary>

<br>

What only matters sometimes only exists sometimes. Favorites use Bubble's visibility conditions, the extra rooms and the climate table a visibility condition on a toggle. A hidden tile leaves no gap.

</details>

<details>
<summary>Sorting with order</summary>

<br>

Grid items sort with plain CSS `order`. The oven goes first with `order: -2`, the vacuum follows while it cleans:

```yaml
origami_tiles:
  order:
    cleaning: -1
```

</details>

<details>
<summary>Zebra stripes</summary>

<br>

One repeating gradient on the table's background, sized to the fixed row height:

```yaml
styles:
  grid-template-rows: repeat(6, var(--control-height-icon))
  background-image: >-
    repeating-linear-gradient(to bottom, transparent, transparent var(--control-height-icon),
    var(--fill-subtle) var(--control-height-icon), var(--fill-subtle) calc(var(--control-height-icon) * 2))
```

</details>

<details>
<summary>Morphing with scale</summary>

<br>

To show and hide something without moving anything, scale it instead of removing it. The navbar writes the group state into a variable, and the badge scales with it:

```js
document.documentElement.style.setProperty('--navbar-player-group', a.group === 'yes' ? '1' : '0');
```

```css
.media-player::before {
  scale: var(--navbar-player-group, 0);
  transition: scale var(--duration-normal) var(--ease-standard);
}
```

</details>

</details>

<details>
<summary><b>Theme</b></summary>

<br>

The theme is made of tokens. Home Assistant reads them, and so do the cards and modules. Restyling means changing a few of them in [`origami.yaml`](themes/origami.yaml).

<details>
<summary>Colour</summary>

<br>

Colour means a device is on, and it sits in the glyph. The field behind it, `tint`, is the same colour thinned out. Which colour a device gets is Home Assistant's own mapping, re-toned per mode: lamps amber, heating deep orange, media light blue, open windows cyan, anything else that is on green. To change a domain, add its `state-<domain>-active-color` line.

Amber dark enough for a glyph on a light page turns brown, so lamps use `lamp-ink` on `lamp-field` instead.

</details>

<details>
<summary>Paper Buttons Row</summary>

<br>

Paper Buttons Row is where the name comes from: origami is folding paper. It has its own colour variables, `pbs-button-*`. The theme points them at the text colour, so a row starts out in ink, and colour comes from the styles you give it. The `rgb` ones need a triplet like `var(--rgb-primary-text-color)`, a hex value breaks the colour.

</details>

<details>
<summary>Tokens</summary>

<br>

| Token | |
| --- | --- |
| `ha-space-1` … `ha-space-5` | Spacing, 4 to 20 px |
| `ha-card-border-radius` | Card corners, 22 px. `radius-inner` follows it, `radius-small` is 8 px. |
| `border-width`, `border-contrast` | Card outlines, off by default |
| `card-shadow`, `card-shadow-flat`, `shadow-elevated` | Elevation per mode, flat for cards in pop-ups |
| `fill-subtle`, `fill-strong`, `fill-loud` | Neutral surfaces, mixed from `fill-base` so they stay warm |
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
| `tint-strength`, `fill-strength` | How strong tint and fills are, per mode |
| `amber-color`, `green-color`, … | Home Assistant's named colours, re-toned per mode |

</details>

</details>

## Help

<details>
<summary>HACS doesn't offer the theme</summary>

<br>

HACS only handles themes once Home Assistant loads them. Add the `themes` line to `configuration.yaml` and restart.

</details>

<details>
<summary>A card says "Custom element doesn't exist"</summary>

<br>

The card isn't installed, or the browser still runs the old page. Install it and reload. In the Companion app, use **Reset frontend cache** in its settings.

</details>

<details>
<summary>The font doesn't change</summary>

<br>

Open `/local/fonts/montserrat.css` on your Home Assistant's address, it should show two `@font-face` rules. If not, check that the files are in `/config/www/fonts`, and restart once if `www` is new.

</details>

<details>
<summary>Bubble cards look plain</summary>

<br>

The modules aren't loaded. Check that Bubble Card Tools is added as an integration and that the files are in `/config/bubble_card/modules/`, then reload.

</details>

<details>
<summary>A value shows ?</summary>

<br>

A helper can't find its source sensor. Rename the entities in `/config/packages/origami.yaml` to yours, then reload **Template entities** under [YAML configuration reloading](https://my.home-assistant.io/redirect/server_controls/).

</details>

<details>
<summary>Linking the font instead of copying it</summary>

<br>

Add `https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap` as the stylesheet instead. No files to copy, but the font loads from Google every time and arrives a moment late, which redraws all text.

</details>

## Status

- [x] Theme, dashboard and helpers
- [x] Bubble modules, still experimental
- [ ] Pop-ups: rooms, music, TV, favorites, lights and the rest
- [ ] Desktop layout

## License

[AGPL-3.0](LICENSE). The Montserrat files in `www/fonts` keep their own [SIL Open Font License](www/fonts/OFL.txt).
