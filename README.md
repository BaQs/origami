# ◪ Origami

A chill Home Assistant theme, a set of Bubble Card modules made for it, and the dashboard built from both.

<img width="760" alt="The Origami dashboard on a phone, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/dashboard.webp">

- Light and dark mode
- Soft, warm surfaces, and colour only where something is on
- Content that only shows up while it matters
- Nothing jumps while the dashboard loads

> [!NOTE]
> This is a personal dashboard that has grown over years. Some things are done in very specific ways, mostly to avoid layout shifts. Take what you like.

**Basics** · [Setup](#setup) · [Dashboard](#dashboard) · [Helpers](#helpers)<br>
**Reference** · [Bubble modules](#bubble-modules) · [Patterns](#patterns) · [Theme](#theme)

## Setup

<details>
<summary><b>1. Theme</b></summary>

<br>

The theme is what makes everything look right, so it comes first. Install it through HACS to get updates.

[![Open your Home Assistant instance and open this repository inside HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme)

1. The button opens this repository in your HACS. Confirm with **Add**, then **Download**.
2. Reload the themes with the `frontend.reload_themes` action, or restart.
3. Open your profile and pick **Origami** as theme. **Auto** follows your device's dark mode, and most phones can switch that at sunset.

<details>
<summary>Without the button, or without HACS</summary>

<br>

**HACS, by hand:** open HACS, then the three dots, **Custom repositories**. Add `https://github.com/hazymorning/origami` with the type **Theme** and download Origami.

**Without HACS:** copy `themes/origami.yaml` into the `themes` folder of your config, then reload the themes. Without a `themes` folder yet, create it next to `configuration.yaml`, add this once and restart:

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

</details>

</details>

<details>
<summary><b>2. Font</b></summary>

<br>

Origami uses [Montserrat](https://fonts.google.com/specimen/Montserrat) in the weights 500 and 700. Host it on your Home Assistant, so it loads instantly and without an outside server. A font that arrives late redraws all text, and that is a layout shift.

1. Copy the folder [`www/fonts`](www/fonts) from this repository to `/config/www/fonts`.
2. Go to **Settings > Dashboards**, open the three dots and choose **Resources**. Turn on advanced mode in your profile if it's missing.
3. Add `/local/fonts/montserrat.css` with the type **Stylesheet**, then reload the page.

The folder holds the two weights as `.woff2`, their `@font-face` rules and the font's license.

</details>

<details>
<summary><b>3. Cards</b></summary>

<br>

All from HACS:

| Card | For |
| --- | --- |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | Favorites, section headers and all cards in the pop-ups |
| [Bubble Card Tools](https://github.com/Clooos/Bubble-Card-Tools) | Stores the modules as files. An integration, add it under **Settings > Devices & services**. |
| [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) | Greeting, server status, climate table and room cards |
| [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) | The bar at the bottom |
| [Origami Header](https://github.com/hazymorning/origami_header) | The fold-out menu instead of the header. Loads as an extra module, see its readme. |
| [Origami Notifications](https://github.com/hazymorning/origami_notifications) | Notifications next to the greeting |
| [Origami Weather](https://github.com/hazymorning/origami_weather) | The weather card |

The screenshots come from Home Assistant 2026.10 with Bubble Card 3.4.1, Bubble Card Tools 1.1.1, Paper Buttons Row 2.3.1 and Navbar Card 1.7.0.

</details>

<details>
<summary><b>4. Bubble modules</b></summary>

<br>

Copy the files from [`bubble_card/modules`](bubble_card/modules) to `/config/bubble_card/modules/` and reload. More in [Bubble modules](#bubble-modules).

</details>

<details>
<summary><b>5. Helpers</b></summary>

<br>

The cards read a few template sensors, two toggles and a speaker picker. They come as one [package](packages/origami.yaml).

1. Copy `packages/origami.yaml` to `/config/packages/`.
2. Add this once to `configuration.yaml`, if packages aren't on yet:

   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```

3. Rename the entities in the file to yours and restart.

What each sensor does is in [Helpers](#helpers).

</details>

<details>
<summary><b>6. Dashboard</b></summary>

<br>

Create a new dashboard and open it. Choose **Edit dashboard**, then **Raw configuration editor** from the three dots, and paste [`dashboard/dashboard.yaml`](dashboard/dashboard.yaml). Then replace the example entities with yours.

From then on Origami Header replaces the dashboard's header. Edit mode is in its menu, which **More** in the navbar opens, or add `?edit=1` to the dashboard's address.

</details>

## Dashboard

One sections view: a single column on phones, two on larger screens. Each part below lists what it reads, the whole thing is in [`dashboard/dashboard.yaml`](dashboard/dashboard.yaml).

<details>
<summary><b>Before you copy</b></summary>

<br>

All entity IDs are English examples, rename them to yours. Cards that show a state get it from a [helper](#helpers) or a module, so they stay simple.

Some ways of building a dashboard add lag on the first load, where you can watch the layout build itself, card-mod or template cards for example. Everything here avoids that, which sometimes leads to unusual solutions. More in [Patterns](#patterns).

</details>

### Greeting and menu

Home Assistant's header is gone. [Origami Header](https://github.com/hazymorning/origami_header) replaces it with a menu that folds out from the top, opened from the navbar. The page starts with the next event, a greeting and the notifications.

<img width="800" alt="The next event, the greeting and the notifications card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/header.webp">

<img width="800" alt="The fold-out menu with the server status and four buttons" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/menu.webp">

<details>
<summary><b>How it works</b></summary>

<br>

- The greeting row is a grid card with two columns: a paper-buttons-row with the next calendar entry and the greeting, and [Origami Notifications](https://github.com/hazymorning/origami_notifications). The notifications turn through what needs attention, every 3 seconds. A tap opens the list.
- The menu holds a server card and four buttons: files, automations, the sidebar and edit mode. The card is one paper-buttons-row. Its chip is green when all is good, accent coloured with updates, orange or red when the disk, memory, processor or the last backup need a look. The text says what, and how long the server has been up.
- The navbar's **More** opens the menu with a `fire-dom-event` action.

</details>

<details>
<summary><b>What it needs</b></summary>

<br>

| Entity | For |
| --- | --- |
| `calendar.household` | The next entry above the greeting |
| `sensor.greetings` | The greeting, see [Helpers](#helpers) |
| `weather.home` | Rain ahead in the notifications |
| `sensor.system_monitor_*` | Processor, memory, disk and last boot from the [System Monitor](https://www.home-assistant.io/integrations/systemmonitor/) integration |
| `sensor.backup_last_successful_automatic_backup` | From the Backup integration |

The notifications list a few more entities: weather warnings, the calendar, a timer, a countdown, the vacuum and tonight's dinner.

</details>

### Weather

An [Origami Weather](https://github.com/hazymorning/origami_weather) card. The sky follows the weather and the sun, the house shows the room temperatures, and a room with an open window gets a window icon.

<img width="800" alt="The weather card with the house in daylight and at night" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/weather.webp">

<details>
<summary><b>How it works</b></summary>

<br>

- Top left is the condition and the temperature on the balcony, from a real sensor rather than the forecast.
- Bottom left is today's chance of rain from the daily forecast.
- The house in the screenshots is the card's demo house. The dashboard this comes from has a drawing of the actual home instead, with a lit window per room while its lights are on. The [Origami Weather readme](https://github.com/hazymorning/origami_weather#layouts) explains how to make one.
- The temperatures are buttons placed on the house. The bedroom only shows up while its window is open, with a window icon.

</details>

<details>
<summary><b>What it needs</b></summary>

<br>

| Entity | For |
| --- | --- |
| `weather.home` | Condition, sky and the daily forecast |
| `sensor.climate_balcony` | Outside temperature, attribute `temperature` |
| `sensor.climate_<room>` | Room temperatures on the house |
| `binary_sensor.bedroom_window` | The window icon |

</details>

### Favorites

Shortcuts that only exist while they matter. A [Header](bubble_card/README.md#header) row and [Tiles](bubble_card/README.md#tiles), both Bubble Card.

<img width="800" alt="Six favorite tiles in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/favorites.webp">

<details>
<summary><b>How it works</b></summary>

<br>

- Each tile is a sub-button of one Bubble card, and Bubble's visibility conditions decide when it exists. Hidden tiles leave no gap.
- The oven only shows while it's on, and always comes first (`order: -2`). The vacuum moves to the front while it cleans, and its progress becomes the name.
- The fan shows up in warm weather or while it runs. Music shows up late in the evening.
- The dinner tile shows in the afternoon and evening, with the recipe photo as its icon.
- Bath and printer show the minutes left while they run, counted in the browser from an `input_datetime`.
- A tile's colour is Home Assistant's state colour while the entity is on, or set per tile.

</details>

<details>
<summary><b>What it needs</b></summary>

<br>

Your own devices. The example uses `sensor.oven`, `vacuum.robot` with `number.robot_cleaning_progress`, `fan.bedroom`, `sensor.dinner` with a `recipe.image` attribute (like [Tandoor](https://tandoor.dev)), `media_player.bedroom`, `input_boolean.bath_time` with `input_datetime.bath_time_end` and `switch.printer` with `input_datetime.printer_end`.

</details>

### Rooms

A header with the home's average and the open windows, a climate table that folds out under it, and a card per room.

<img width="800" alt="The rooms header, the open climate table and six room cards" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/rooms.webp">

<details>
<summary><b>How it works</b></summary>

<br>

- The header is a Bubble separator with the [Header](bubble_card/README.md#header) module in chip style. The window chip turns cyan while a window is open. A tap on a chip or the arrow toggles `input_boolean.room_climate_card`, and the table is a card with a visibility condition on it.
- The table is one paper-buttons-row shaped into a grid of six columns with fixed row heights. Per room: a comfort dot and the name, the window, the thermostat's target, the temperature and the humidity.
- The thermostat tints while it heats, an open window tints cyan. An arrow marks the warmest, coldest, most humid and driest room.
- Rooms without a thermostat or window contact leave those cells empty, like the office. The front door sits in the spare column of the first row.
- The zebra stripes are one gradient on the row background, sized to the row height.
- Each room card is a paper-buttons-row. The icon chip lights up while the room's lights are on, the dot shows the comfort colour. The whole card opens the room's pop-up, the window icon has its own tap.
- The first four rooms always show. **All rooms** toggles `input_boolean.all_rooms` for the rest.
- On larger screens a status row with the people at home, the front door and the open windows sits above the header.

</details>

<details>
<summary><b>What it needs</b></summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.climate_<room>` | Per room: `temperature`, `humidity`, `temp_rank`, `humidity_rank`, `color_code` |
| `sensor.climate_home` | `temperature_average` and `windows_open` for the header, `front_door_open` for the status row |
| `binary_sensor.<room>_window` | Window contacts |
| `climate.<room>` | Thermostats, target and `hvac_action` |
| `light.<room>` | The lit chip on the room cards |
| `input_boolean.room_climate_card`, `input_boolean.all_rooms` | The two toggles |

All sensors and both toggles are in the [helpers](#helpers).

</details>

### Navbar

A [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) at the bottom: music, TV, lights, favorites and the menu. What plays sits above it.

<img width="800" alt="The navbar with the now playing card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar.webp">

<details>
<summary><b>How it works</b></summary>

<br>

- Navbar Card's templates are JavaScript, not Jinja. Inside `[[[ ]]]` it's `states['sensor.x'].state`, not `states('sensor.x')`.
- A route is highlighted in its domain's colour while something is on: media light blue, lights amber. A field of tint sits behind the glyph, like the icon chips in the modules.
- The badges count playing speakers and lights that are on. Each badge cuts a small gap into the route's field.
- Holding **Lights** opens the scenes. The active scene is amber.
- Above the bar, Navbar Card's media player shows the speaker that matters right now, picked by `sensor.active_speaker`. A link badge on the cover marks a group, and a hold on **Music** plays or pauses.
- On larger screens the bar sits at the right edge.

</details>

<details>
<summary><b>What it needs</b></summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.active_speaker_count`, `sensor.lights_on_count` | The badges |
| `sensor.active_speaker` | The now playing card, see [Helpers](#helpers) |
| `media_player.living_room_tv`, `media_player.tv_ump` | The TV route, and a universal media player for the TV |
| `light.all_lights`, `script.lights_toggle` | The first button in the scenes switches all lights |
| `scene.living_room_*`, `sensor.living_room_scene` | The scenes and the active one |

</details>

<details>
<summary><b>Scenes</b></summary>

<br>

<img width="600" alt="The scenes that open from the lights route" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar-scenes.webp">

</details>

<details>
<summary><b>On a larger screen</b></summary>

<br>

<img width="800" alt="The dashboard on a desktop screen" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/desktop.webp">

</details>

## Helpers

Template sensors do the thinking, the cards only show the result. All of them and the toggles are in [`packages/origami.yaml`](packages/origami.yaml).

| Helper | Gives | Read by |
| --- | --- | --- |
| `sensor.climate_<room>` | Rounded temperature and humidity, rank, comfort colour | Climate table, room cards, weather |
| `sensor.climate_temperature_peaks`, `sensor.climate_humidity_peaks` | Warmest and most humid room, the lowest as attribute | The room sensors |
| `sensor.temperature_average`, `sensor.humidity_average` | Averages over all rooms | `sensor.climate_home` |
| `sensor.climate_home` | Averages, open windows, front door | Rooms header, status row |
| `sensor.windows_open_count` | Open windows | `sensor.climate_home`, status row |
| `sensor.greetings` | The greeting | Greeting |
| `sensor.lights_on_count`, `sensor.active_speaker_count` | Counters | Navbar badges |
| `sensor.active_target`, `sensor.active_speaker` | The speaker that matters and what it plays | Navbar |
| `input_boolean.room_climate_card`, `input_boolean.all_rooms` | Toggles | Rooms |

<details>
<summary><b>Room climate sensor</b></summary>

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
            {% if t < 16 %}var(--blue-color)
            {% elif t < 19 %}var(--cyan-color)
            {% elif t < 24 %}var(--green-color)
            {% elif t < 27 %}var(--orange-color)
            {% else %}var(--red-color){% endif %}
```

- A missing source gives `?` instead of an empty value. A card that shows `?` keeps its size, an empty one collapses and jumps when the value arrives.
- The comfort colour names the theme's colours, so it fits both modes.
- The peaks sensors return `none` while all rooms read the same. Without that, two rooms at the same temperature would fight over the arrow.

</details>

<details>
<summary><b>Active speaker</b></summary>

<br>

The navbar doesn't watch every speaker, it reads one sensor that already picked the interesting one. `sensor.active_target` decides which: a playing speaker, then the TV or Bluetooth, then a paused one, then the one in `input_select.selected_speaker`. `sensor.active_speaker` prepares what the card shows:

| Attribute | |
| --- | --- |
| `chip` | `playing_art`, `playing_plain`, `paused_art`, `paused_plain`, `tv_art`, `tv_plain` or `off` |
| `title` | `Title \| Artist`, never empty and never stale |
| `speaker` | Short name, with a count while grouped, like `Living +1` |
| `group` | `yes` or `no` |
| `artwork` | The cover or TV artwork, `none` without |

It's written for LinkPlay speakers, where each player has Music Assistant (`_mass`) and DLNA (`_dlna`) twins, and a `sensor.tv_source` with title and artwork while the TV plays over optical. Leave out what you don't have.

</details>

## Bubble modules

Eleven modules give Bubble Card the Origami look. Base goes on every button and media player card first, then the module for its type. The files are in [`bubble_card/modules`](bubble_card/modules), options and examples in the [module reference](bubble_card/README.md).

<img width="800" alt="A pop-up built from the modules, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/module-popup.webp">

| Module | For |
| --- | --- |
| [Base](bubble_card/README.md#base) | Frame, type and colour of every card, with compact rows, a pill switch, grouped keys and quiet tiles |
| [Light](bubble_card/README.md#light), [Fan](bubble_card/README.md#fan), [Climate](bubble_card/README.md#climate) | Lights, fans with speed keys, thermostats |
| [Media](bubble_card/README.md#media), [Speaker Select](bubble_card/README.md#speaker-select) | Speakers and TVs, speaker rows for a music pop-up |
| [Slider](bubble_card/README.md#slider), [Button](bubble_card/README.md#button) | A slider on its own, rows and grids of buttons |
| [Header](bubble_card/README.md#header), [Tiles](bubble_card/README.md#tiles), [Popup](bubble_card/README.md#popup) | Section headers, tile grids, pop-ups |

## Patterns

The same few ideas repeat through every card.

<details>
<summary><b>The row is the card</b></summary>

<br>

Paper Buttons Row draws no card by itself. Give the row a background, radius and padding through `styles` and it becomes a card. Turn on `display: grid` and it becomes a layout. The room cards, the climate table and the server card are each one row.

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
<summary><b>Dumb cards, smart sensors</b></summary>

<br>

Cards don't calculate. A sensor turns everything into one word, and the card maps the word to a look. The climate table reads `temp_rank` and maps it to an arrow:

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

Change the logic in the sensor and every card follows.

</details>

<details>
<summary><b>No layout shifts</b></summary>

<br>

This is behind most unusual choices here.

- **Fixed heights.** Rows, tiles and cards have set heights, a long text never pushes anything.
- **States swap colours, not sizes.** A tile that turns on changes its colour, never its size.
- **Helpers always return a value.** `?` instead of an empty string, and `| default(..., true)` where a value can be missing.
- **Elements that come and go** either leave no gap, like hidden tiles, or keep their box and scale, like the group badge.
- **Values in the browser.** The modules take JavaScript in `${ }`, which is there on the first frame. Jinja waits for the server.
- **Animations only on user action.** The climate table fades in when it's opened. Nothing animates on page load.
- **A local font.** A font that loads late reflows all text.
- **Grid instead of stack cards** where the dashboard needs a wrapper. A card placed directly in the section loads fastest.

</details>

<details>
<summary><b>Whole card tap targets</b></summary>

<br>

Instead of making every part tappable, one invisible button stretches over the whole card and catches the tap. Everything else gets `pointer-events: none`. A single element can take taps again with `pointer-events: all`, like the window icon on the room cards.

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
<summary><b>Visibility instead of logic</b></summary>

<br>

What only matters sometimes only exists sometimes. The favorites are sub-buttons with Bubble's visibility conditions, the extra rooms and the climate table are cards with a visibility condition on a toggle. No helper decides what shows, and a hidden tile leaves no gap.

</details>

<details>
<summary><b>Sorting with order</b></summary>

<br>

Grid items sort with plain CSS `order`. The oven goes first with `order: -2`, the vacuum follows while it cleans:

```yaml
origami_tiles:
  order:
    cleaning: -1
```

</details>

<details>
<summary><b>Zebra stripes</b></summary>

<br>

Striped rows without extra elements: one repeating gradient on the table's background, sized to the fixed row height. More rooms and the stripes still match, because both read the same variable.

```yaml
styles:
  grid-template-rows: repeat(6, var(--control-height-icon))
  background-image: >-
    repeating-linear-gradient(to bottom, transparent, transparent var(--control-height-icon),
    var(--fill-subtle) var(--control-height-icon), var(--fill-subtle) calc(var(--control-height-icon) * 2))
```

</details>

<details>
<summary><b>Morphing with scale</b></summary>

<br>

To show and hide an element without moving anything, scale it instead of removing it. Its box stays, and with a transition it pops in and out. CSS can't read states, so the navbar's subtitle template writes the group state into a variable, and the badge scales with it:

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

## Theme

The theme is made of tokens, Home Assistant's surfaces read them, and the cards and modules read the same ones. Restyling means changing a few of them in `origami.yaml`.

<details>
<summary><b>Colour</b></summary>

<br>

Colour means a device is on, and it sits in the glyph. The field behind a coloured glyph is `tint`, the same colour thinned out. Badges and slider fills use the colour at `fill-strength`.

Which colour a device gets is Home Assistant's own mapping, re-toned per mode. Lamps are amber, heating is deep orange, media is light blue, open windows are cyan, and switches and anything else that is simply on are green. To change a domain, add its `state-<domain>-active-color` line.

Amber is a light colour, and dark enough for a glyph on a light page it turns brown. For that the theme has `lamp-ink` on `lamp-field`, where in light mode the light sits in the field and the glyph stays ink.

</details>

<details>
<summary><b>Tokens</b></summary>

<br>

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

</details>

## Status

- [x] Theme, setup, dashboard and helpers
- [x] Bubble modules
- [ ] Pop-ups: rooms, music, TV, favorites, lights and the rest
