# ◪ Origami

<a href="https://github.com/hazymorning/origami/releases/latest"><img alt="Latest release" src="https://img.shields.io/github/v/release/hazymorning/origami?label=Release&color=6A8778&labelColor=181715"></a>
<a href="https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme"><img alt="HACS custom repository" src="https://img.shields.io/badge/HACS-Custom-6A8778?labelColor=181715"></a>

A calm theme for Home Assistant, and the phone dashboard I built with it from [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) and [Bubble Card](https://github.com/Clooos/Bubble-Card). Plus Bubble Card modules in the same style.

<img alt="Four phones with the Origami dashboard, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/hero.webp">

| | |
| --- | --- |
| **Theme** | Soft, warm surfaces in light and dark. Colour only shows where something is on. Works with any dashboard. |
| **Dashboard** | Weather, favorites, rooms and a navbar, made for phones. Nothing jumps around while it loads. |
| **Bubble modules** | The theme's look for Bubble Card. Still experimental. |

> [!NOTE]
> This is my own dashboard, grown over years. The entities are from my home: swap them for yours and leave out what you don't need. Some things are done in very specific ways, mostly so nothing jumps while the page loads. A desktop layout is still in the works.

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

2. Open Origami in HACS, confirm with **Add** if asked, then **Download**.

   [![Open your Home Assistant instance and open this repository inside HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme)

3. Run [`frontend.reload_themes`](https://my.home-assistant.io/redirect/developer_call_service/?service=frontend.reload_themes) and pick **Origami** in your [profile](https://my.home-assistant.io/redirect/profile/).
4. For the font, copy [`www/fonts`](https://github.com/hazymorning/origami/tree/main/www/fonts) to `/config/www/fonts` and add `/local/fonts/montserrat.css` as **Stylesheet** in [Resources](https://my.home-assistant.io/redirect/lovelace_resources/).

</details>

<details>
<summary><b>Whole dashboard</b></summary>

<br>

Tested with Home Assistant 2026.10. You need [HACS](https://hacs.xyz) and access to `/config`, for example through the Samba share or the Studio Code Server app.

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
| [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) | Greeting, server card, room cards and climate table | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=jcwillox&repository=lovelace-paper-buttons-row&category=plugin) |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | Favorites and section headers | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card&category=plugin) |
| [Bubble Card Tools](https://github.com/Clooos/Bubble-Card-Tools) | Loads the Bubble modules | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card-Tools&category=integration) |
| [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) | The bar at the bottom | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=joseluis9595&repository=lovelace-navbar-card&category=plugin) |
| [Origami Notifications](https://github.com/hazymorning/origami_notifications) | The notifications next to the greeting | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_notifications&category=plugin) |
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

If you skip Origami Header, delete the card under `# Menu (Origami Header)` and the route under `# Opens the Origami Header menu` first.

</details>

<details>
<summary>6. Make it yours</summary>

<br>

The dashboard is set up for my home. In the raw editor, Ctrl+F (⌘F on a Mac) finds an entity like `light.living_room`, so you can replace it with yours. Delete what you don't have, like a tile, a room or a navbar route. Each part below lists the entities it uses.

Then put your sensors into `/config/packages/origami.yaml` and restart.

</details>

<details>
<summary>Optional: Origami Header</summary>

<br>

It removes Home Assistant's header completely, with no flicker while the page loads. In its place you get a menu that opens from the top when you tap **More** in the navbar. It's a bit experimental, but no other way I've found hides the header this cleanly. The dashboard works fine without it.

1. [Open it in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_header&category=plugin) and choose **Download**. It needs Home Assistant 2026.9 or newer.
2. Add the `extra_module_url` lines to `configuration.yaml` and restart:

   ```yaml
   frontend:
     extra_module_url:
       - /hacsfiles/origami_header/origami-header.js
   ```

Edit mode is then in the menu, or add `?edit=1` to the address.

</details>

</details>

## Dashboard

One sections view, made for phones. Each part starts with a comment in [`dashboard.yaml`](dashboard/dashboard.yaml), like `# Favorites`. Some taps open a pop-up, like a room card. The pop-ups aren't in this repository yet, so those taps do nothing for now.

<img width="760" alt="The Origami dashboard on a phone, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/dashboard.webp">

### Greeting and notifications

A greeting that changes with the time of day, with the next calendar event above it. Next to it is the Origami Notifications card.

<img width="800" alt="The next event, the greeting and the notifications card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/header.webp">

<details>
<summary>How it works</summary>

<br>

- The greeting and the line above it are a Paper Buttons Row. The line shows the next event of `calendar.household`. Any other entity works there too, or delete it and keep only the greeting.
- [Origami Notifications](https://github.com/hazymorning/origami_notifications) shows Home Assistant's notifications, repairs and updates without any setup. Each entity you add shows only while it matters, like a timer while it runs. Here it watches weather warnings, a calendar, a pizza timer, the oven, the vacuum and dinner.
- When nothing is going on, it shows the weather and the next events instead. With `hide_when_empty: true` it hides.
- With several entries, it switches between them every 3 seconds. A tap opens the list.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.greetings` | The greeting, from the [helpers](#under-the-hood) |
| `calendar.household` | The next event above the greeting |
| `weather.home` | Rain ahead, and the weather when nothing is going on |
| `sensor.dwd_*`, `timer.pizza`, `sensor.oven_ready_in`, `vacuum.robot`, `sensor.dinner` | Examples. The card takes almost any entity, see its [README](https://github.com/hazymorning/origami_notifications) |

All of them are optional.

</details>

### Menu

Needs the optional [Origami Header](#setup). Home Assistant's header is gone, and **More** in the navbar opens this menu from the top of the screen.

<img width="800" alt="Tap More in the navbar and the menu opens from the top, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/menu.webp">

<details>
<summary>How it works</summary>

<br>

- The menu is an Origami Header card with `mode: menu`. What goes in it is up to you. Here it's a server card and buttons for the file editor, automations, the sidebar and edit mode.
- The server card shows a status, the uptime, CPU and RAM.
- The status is **All good** in green, or the number of updates in the accent colour. It turns orange or red when the disk, memory or CPU run high, or when the last automatic backup is more than 3 days old.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.system_monitor_*` | CPU, memory, disk and uptime, from [System Monitor](https://www.home-assistant.io/integrations/systemmonitor/). Without it, the status says **No data**. |
| `sensor.backup_last_successful_automatic_backup` | The backup check. Without it, backups aren't checked. |

</details>

### Weather

An [Origami Weather](https://github.com/hazymorning/origami_weather) card. It can show a lot more than this, like an animated sky, forecasts, gauges or your own pictures. Here it's set up calm, with one colour per condition.

<img width="800" alt="The weather card in daylight and at night" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/weather.webp">

<details>
<summary>How it's set up here</summary>

<br>

- `background_mode: color`, with a colour for each condition in light and dark mode, set in `background_thresholds`.
- Sun, moon, clouds, stars, birds, balloons and planes are off. Rain and snow still show.
- The house is the card's demo house. Its README shows how to get [your own home](https://github.com/hazymorning/origami_weather#layouts) in the same style.
- Top left is the next sunset, or the next sunrise while the sun is down. Bottom left are the temperature and the condition. Any temperature sensor works there too.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `weather.home` | Condition, colour and temperature |
| `sun.sun` | Next sunset or sunrise |

</details>

### Favorites

Tiles that only show up while they matter. Made with Bubble Card and the [Header](bubble_card/README.md#header) and [Tiles](bubble_card/README.md#tiles) modules.

<img width="800" alt="Six favorite tiles in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/favorites.webp">

<details>
<summary>How it works</summary>

<br>

- All tiles belong to one Bubble card. Each tile has its own visibility conditions, and a hidden tile leaves no gap.
- In my home, the oven shows while it's on and always comes first. The fan shows while it runs, or when the living room or bedroom gets warmer than 24°. Music shows from 22:00 to 4:00, dinner from 14:00 to 21:00 with the recipe photo, and the bath from 11:00 to 23:00.
- The vacuum and the printer always show. The vacuum shows its progress while it cleans and moves up. Bath and printer show the minutes left while they run.
- For your own tile, copy one and change its entity and visibility.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Tile | Entities |
| --- | --- |
| Oven | `sensor.oven` |
| Vacuum | `vacuum.robot`, `number.robot_cleaning_progress` |
| Fan | `fan.bedroom`, `sensor.living_room_temperature`, `sensor.bedroom_temperature` |
| Dinner | `sensor.dinner` with a `recipe.image` attribute, like from [Tandoor](https://tandoor.dev) |
| Music | `media_player.bedroom` |
| Bath | `input_boolean.bath_time`, `input_datetime.bath_time_end` |
| Printer | `switch.printer`, `input_datetime.printer_end` |

All examples from my home.

</details>

### Rooms

A card for each room. The first four always show, **All rooms** shows the rest.

<img width="800" alt="The rooms header and four room cards" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/rooms.webp">

<details>
<summary>How it works</summary>

<br>

- Each room card is one Paper Buttons Row. A tap anywhere on it opens the room's pop-up.
- The icon lights up while the room's light is on.
- Below the name: a dot in the room's comfort colour, a window icon if the room has a window contact, then temperature and humidity.
- The rooms header shows the average temperature and how many windows are open. A tap on either, or on the arrow, opens the climate table.
- For another room, copy a card and change its entities. Rooms after the fourth are conditional cards that show while `input_boolean.all_rooms` is on.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `light.<room>` | The icon |
| `sensor.climate_<room>` | Comfort colour, temperature and humidity, from the [helpers](#under-the-hood) |
| `binary_sensor.<room>_window` | The window icon. The living room has a balcony door. |
| `sensor.climate_home` | Average and open windows in the header |
| `input_boolean.all_rooms` | **All rooms** |

</details>

### Climate table

All rooms in one table. It opens under the rooms header.

<img width="800" alt="The rooms header with the climate table open" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/climate.webp">

<details>
<summary>How it works</summary>

<br>

- The arrow or a chip in the rooms header opens and closes it.
- One line per room: comfort dot and name, window, thermostat, temperature and humidity. The living room also has the front door.
- An open window turns cyan, a thermostat that heats turns orange. Arrows mark the warmest and coldest room, and the most and least humid.
- A tap opens the room's thermostat. Window and door open their contact.
- It's one Paper Buttons Row laid out as a grid. Keep the room names short, so it fits on a phone.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.climate_<room>` | Temperature, humidity, arrows and comfort colour |
| `binary_sensor.<room>_window`, `binary_sensor.front_door` | Windows and the door |
| `climate.<room>` | Target temperature and heating |
| `input_boolean.room_climate_card` | Open or closed |

A room without a thermostat or window contact leaves that cell empty, like the office.

</details>

### Navbar

A [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) with music, TV, lights, favorites and the menu. What's playing shows above it.

<img width="800" alt="The navbar with the now playing card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar.webp">

<details>
<summary>How it works</summary>

<br>

- A button takes on its colour while something is on. Music and Lights show how many speakers play and how many lights are on.
- Hold **Music** to play or pause the active speaker, hold **TV** for the TV's controls.
- Hold **Lights** to turn all lights on or off, or to pick a scene. The active scene is amber.
- **More** opens the menu from Origami Header. Admins see a dot on it while updates are waiting.
- The card above shows the speaker that matters right now, picked by `sensor.active_speaker`, or the TV.
- Navbar Card templates are JavaScript: `states['sensor.x'].state`, not `states('sensor.x')`.

<img width="600" alt="The light scenes, opened by holding Lights" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar-scenes.webp">

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.active_speaker_count`, `sensor.lights_on_count` | The counts, from the [helpers](#under-the-hood) |
| `sensor.active_speaker` | The card above, from the helpers |
| `media_player.living_room_tv`, `media_player.tv_ump` | The TV button and the TV in the card above |
| `light.all_lights`, `script.lights_toggle` | All lights on or off |
| `scene.living_room_*`, `sensor.living_room_scene` | The scenes and which one is active |

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

Template sensors do the work, the cards only show the result. They're all in [`packages/origami.yaml`](packages/origami.yaml). Put in your own source sensors, and your rooms in the lists for the peaks, averages and counters.

| Helper | Gives |
| --- | --- |
| `sensor.climate_<room>` | Rounded temperature and humidity, rank, comfort colour |
| `sensor.climate_temperature_peaks`, `sensor.climate_humidity_peaks` | The warmest and most humid room, the lowest as attribute |
| `sensor.temperature_average`, `sensor.humidity_average`, `sensor.climate_home` | Averages, open windows, front door |
| `sensor.windows_open_count`, `sensor.lights_on_count`, `sensor.active_speaker_count` | Counters |
| `sensor.greetings` | The greeting |
| `sensor.active_target`, `sensor.active_speaker` | The speaker that matters and what it plays |
| `input_boolean.room_climate_card`, `input_boolean.all_rooms`, `input_select.selected_speaker` | Toggles, and the speaker to show when nothing plays |

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

- If the source sensor is missing, the values are `?`, so the card keeps its size.
- The comfort colour goes from blue at 12° to red above 32°, a little transparent so it works in both modes.
- The peaks sensors return `none` while all rooms read the same, so no room gets an arrow.

</details>

<details>
<summary>Active speaker</summary>

<br>

The navbar reads one sensor that has already picked the speaker to show: one that plays, then one on the TV input or Bluetooth, then a paused one, then `input_select.selected_speaker`. If the playing speaker is grouped, the group leader wins.

| Attribute | |
| --- | --- |
| `chip` | `playing_art`, `playing_plain`, `paused_art`, `paused_plain`, `tv_art`, `tv_plain` or `off` |
| `title` | `Title \| Artist`, never empty and never out of date |
| `speaker` | Short name, with a count while grouped, like `Living +1` |
| `group` | `yes` or `no` |
| `artwork` | The cover or TV artwork, `none` without |

My speakers are LinkPlay speakers, each with a Music Assistant twin (`media_player.<name>_mass`). Without a twin, the speaker's own title and cover are used, so other speakers work too. Music Assistant also reports playing while the speaker plays Bluetooth or another source of its own, so the twin only counts without one. `sensor.tv_source` is optional, it names what the TV plays over optical.

</details>

</details>

<details>
<summary><b>Patterns</b></summary>

<br>

A few ideas that come up again and again in the dashboard.

<details>
<summary>Paper Buttons Row as a card</summary>

<br>

Used as a card, Paper Buttons Row has no background of its own. Give the row a background, radius and padding and it looks like a card. With `display: grid` it becomes a layout. The room cards, the climate table and the server card are each one row.

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
<summary>Logic in sensors, not in cards</summary>

<br>

The cards don't calculate. A sensor turns the numbers into one word, and the card picks a look for that word:

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
- **States change colours, not sizes.**
- **Helpers always return a value**, `?` instead of an empty string.
- **Things that come and go** leave no gap, or keep their space and scale.
- **Values in the browser.** JavaScript in `${ }` is there on the first frame, Jinja templates wait for the server.
- **A local font.** A font that loads late redraws all text.
- **Grid cards instead of stack cards** where a wrapper is needed.

</details>

<details>
<summary>One tap target per card</summary>

<br>

An invisible button covers the whole card and catches the tap. Everything else in the card gets `pointer-events: none`.

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
<summary>Show only what matters</summary>

<br>

Favorites use Bubble Card's visibility conditions. The extra rooms and the climate table show while a toggle is on. Nothing hidden leaves a gap.

</details>

<details>
<summary>Sorting tiles</summary>

<br>

Tiles sort with plain CSS `order`. The oven goes first with `order: -2`, the vacuum follows while it cleans:

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
<summary>Show and hide with scale</summary>

<br>

To show and hide something without moving anything else, scale it instead of removing it. CSS can't read entity states, so a navbar template writes the group state into a variable:

```js
document.documentElement.style.setProperty('--navbar-player-group', a.group === 'yes' ? '1' : '0');
```

The group badge on the cover scales with it:

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

The theme is a set of tokens. Home Assistant reads them, and so do the cards and modules. To restyle, change a few of them in [`origami.yaml`](themes/origami.yaml).

<details>
<summary>Colour</summary>

<br>

Colour means something is on. It goes into the icon, and the field behind it, `tint`, is the same colour, much lighter. Which colour a device gets is Home Assistant's own choice, like deep orange for heating or light blue for media, re-toned for each mode. Origami changes a few: amber is only for lights, switches and anything else that is simply on are green, open windows are cyan. To change a domain, add its `state-<domain>-active-color` line.

In light mode, amber is too pale for an icon on white. So lights use `lamp-ink` on `lamp-field` there.

</details>

<details>
<summary>Paper Buttons Row</summary>

<br>

The name is a play on Paper Buttons Row. It has its own colour variables, `pbs-button-*`. The theme points them at the text colour, so a row is plain until you give it colours in its styles. The `rgb` ones need a triplet like `var(--rgb-primary-text-color)`, a hex value breaks the colour.

</details>

<details>
<summary>Tokens</summary>

<br>

| Token | |
| --- | --- |
| `ha-space-1` … `ha-space-5` | Spacing, 4 to 20 px |
| `ha-card-border-radius` | Card corners, 22 px. `radius-inner` follows it, `radius-small` is 8 px. |
| `border-width`, `border-contrast` | Card outlines, off by default |
| `card-shadow`, `card-shadow-flat`, `shadow-elevated` | Shadows per mode, flat for cards in pop-ups |
| `fill-subtle`, `fill-strong`, `fill-loud` | Neutral surfaces, mixed from `fill-base` so they stay warm |
| `fill-active`, `toggle-on` | Text colour, for switches and emphasis |
| `tile-background`, `tile-chip-background` | The quiet tile and its raised icon chip |
| `opacity-disabled` … `opacity-strong` | 0.3, 0.45, 0.6, 0.8 |
| `control-height-mini` … `control-height-lg` | Fixed control heights, 32 to 64 px |
| `tile-size`, `tile-size-lg`, `dot-size` | Icon chips, 48 and 80 px, dots 12 px |
| `icon-size-2xs` … `icon-size-xl` | Icons, 14 to 48 px |
| `ha-font-size-xs` … `ha-font-size-xxl` | Text, 14 to 26 px |
| `primary-font-family` | Montserrat, weights 500 and 700 |
| `duration-fast`, `duration-normal`, `duration-slow`, `ease-standard` | Motion |
| `press-scale` | How far a button shrinks when pressed |
| `blur-soft`, `blur-strong` | Header, dialogs, navbar |
| `tint-strength`, `fill-strength` | How strong tint and fills are, per mode |
| `amber-color`, `green-color`, … | Home Assistant's named colours, re-toned per mode |

</details>

</details>

## Help

<details>
<summary>HACS doesn't offer the theme</summary>

<br>

HACS can only install themes once Home Assistant loads them from the `themes` folder. Add the `themes` line to `configuration.yaml` and restart.

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

A helper can't find its source sensor. Put your entities into `/config/packages/origami.yaml`, then reload **Template entities** under [YAML configuration reloading](https://my.home-assistant.io/redirect/server_controls/).

</details>

<details>
<summary>Linking the font instead of copying it</summary>

<br>

Add `https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap` as the stylesheet instead. There are no files to copy, but the font then loads from Google every time and arrives a moment late, which redraws all text.

</details>

## Status

- [x] Theme, dashboard and helpers
- [x] Bubble modules, still experimental
- [ ] Pop-ups: rooms, music, TV, favorites, lights and the rest
- [ ] Desktop layout

## License

[AGPL-3.0](LICENSE). The Montserrat files in `www/fonts` keep their own [SIL Open Font License](www/fonts/OFL.txt).
