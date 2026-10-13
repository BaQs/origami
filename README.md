# ◪ Origami

<a href="https://github.com/hazymorning/origami/releases/latest"><img alt="Latest release" src="https://img.shields.io/github/v/release/hazymorning/origami?label=Release&color=6A8778&labelColor=181715"></a>
<a href="https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme"><img alt="HACS custom repository" src="https://img.shields.io/badge/HACS-Custom-6A8778?labelColor=181715"></a>

A calm theme for Home Assistant, and the phone dashboard I built with it, mostly from [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) and [Bubble Card](https://github.com/Clooos/Bubble-Card). Plus Bubble Card modules in the same style.

<img alt="The Origami dashboard on two phones, one in light and one in dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/hero.webp">

| | |
| --- | --- |
| **Theme** | Soft, warm surfaces in light and dark. Colour mostly marks what is on. Works with any dashboard. |
| **Dashboard** | Weather, favorites, rooms and a navbar, made for phones. Nothing jumps around while it loads. |
| **Bubble modules** | The theme's look for Bubble Card. Still experimental. |

> [!NOTE]
> This is my own dashboard, grown over years. The entities are from my home: swap them for yours and leave out what you don't need. Some parts are built in unusual ways so nothing jumps while the page loads, see [Under the hood](#under-the-hood). A desktop layout is still in the works.

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
4. For the font, copy [`www/fonts`](https://github.com/hazymorning/origami/tree/main/www/fonts) to `/config/www/fonts` and add `/local/fonts/montserrat.css` as **Stylesheet** in [Resources](https://my.home-assistant.io/redirect/lovelace_resources/). Restart once if `www` is new.

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
| `packages` | The helpers: template sensors, two toggles and a speaker select |

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
<summary>5. Optional: Origami Header</summary>

<br>

It removes Home Assistant's header completely, with no flicker while the page loads. In its place you get a menu that opens from the top when you tap **More** in the navbar. It's a bit experimental, but no other way I've found hides the header this cleanly. The dashboard works fine without it.

1. [Open it in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_header&category=plugin), confirm with **Add** if asked, then **Download**. It needs Home Assistant 2026.9 or newer.
2. Add the `extra_module_url` lines under the `frontend:` you already have in `configuration.yaml`, then restart:

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
     extra_module_url:
       - /hacsfiles/origami_header/origami-header.js
   ```

Edit mode is then in the menu, or add `?edit=1` to the address.

</details>

<details>
<summary>6. Add the dashboard</summary>

<br>

1. Open [Dashboards](https://my.home-assistant.io/redirect/lovelace_dashboards/), **Add dashboard**, **New dashboard from scratch**, and open it.
2. From the three dots, choose **Edit dashboard**, then **Raw configuration editor**.
3. Replace everything with [`dashboard.yaml`](dashboard/dashboard.yaml).
4. Without Origami Header, delete the card under `# Menu (Origami Header)` and the route under `# Opens the Origami Header menu`.
5. Save.

</details>

<details>
<summary>7. Make it yours</summary>

<br>

The dashboard is set up for my home. In the raw editor, Ctrl+F (⌘F on a Mac) finds an entity like `light.living_room`, so you can replace it with yours. Delete what you don't have, like a tile, a room or a navbar route. Each part below lists the entities it uses.

Then change the same entities in `/config/packages/origami.yaml`: temperature and humidity sensors, windows, the front door, lights and speakers. The helpers it creates, like `sensor.climate_living_room`, are read by the dashboard, so rename those in both files or not at all. Restart.

</details>

</details>

## Dashboard

One sections view, made for phones. Each part starts with a comment in [`dashboard.yaml`](dashboard/dashboard.yaml), like `# Favorites`. Some taps open a pop-up, for example a tap on a room card. The pop-ups aren't in this repository yet, so those taps do nothing for now.

<img width="760" alt="The Origami dashboard on a phone, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/dashboard.webp">

### Greeting and notifications

A greeting that changes with the time of day, with the next calendar event above it. Next to it is the Origami Notifications card.

<img width="800" alt="The next event, the greeting and the notifications card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/header.webp">

<details>
<summary>How it works</summary>

<br>

- The greeting and the line above it are a Paper Buttons Row. The line shows the current or next event of `calendar.household`, from its `message` attribute. For another entity, change the entity and remove `attribute: message`. Or delete the line and keep only the greeting.
- [Origami Notifications](https://github.com/hazymorning/origami_notifications) shows Home Assistant's notifications, repairs and updates without any setup. Each entity you add shows only while it matters, like a timer while it runs. Here it watches weather warnings, a calendar, a pizza timer, the oven, the vacuum and dinner.
- Because `hide_when_empty` is `false` here, the weather and the next events turn by after the entries, and fill the card when nothing is going on. With `true`, the card's default, it hides when there is nothing to show.
- With several entries, it switches between them every 3 seconds. A tap opens the list.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.greetings` | The greeting, from the [helpers](#under-the-hood) |
| `calendar.household` | The event above the greeting. The notifications show it from a day ahead. |
| `weather.home` | Rain, snow, thunder and frost ahead, and the current weather |
| `sensor.dwd_*`, `timer.pizza`, `sensor.oven_ready_in`, `vacuum.robot`, `sensor.dinner` | Examples. The card takes almost any entity, see its [README](https://github.com/hazymorning/origami_notifications) |

All of them are optional.

</details>

### Menu

Needs the optional [Origami Header](#setup). Home Assistant's header is gone, and **More** in the navbar opens this menu from the top of the screen.

<img width="800" alt="Tap More in the navbar and the menu opens from the top, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/menu.webp">

<details>
<summary>How it works</summary>

<br>

- The menu is an Origami Header card with `mode: menu`. What goes in it is up to you. Here it's a server card and buttons for the File editor app, automations, the sidebar and edit mode. Without the File editor app, change or delete **Files**.
- The server card shows a status, the uptime, CPU and RAM. A tap on CPU or RAM opens its details, a tap anywhere else opens the server pop-up.
- The chip next to the status shows how things are, and the text says why, like **Backup due**:

  | Chip | When |
  | --- | --- |
  | Green check | All good |
  | Arrow in the accent colour | Updates are waiting |
  | Orange | Disk over 80 %, RAM over 85 %, CPU over 80 %, the last automatic backup older than 3 days, or no System Monitor data |
  | Red | Disk over 90 %, RAM or CPU over 95 %, the backup older than 7 days |

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

<img width="800" alt="The weather card in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/weather.webp">

<details>
<summary>How it's set up here</summary>

<br>

- `background_mode: color`, with a colour for each condition in light and dark mode, set in `background_thresholds`.
- Sun, moon, clouds, stars, birds, balloons and planes are off. Rain, snow, hail and thunderstorms still show, because `precipitation_effects` is on by default.
- The house is the card's demo house. Its README shows how to get [your own home](https://github.com/hazymorning/origami_weather#layouts) in the same style.
- Top left is the next sunset, or the next sunrise while the sun is down. Bottom left are the temperature and the condition of the weather entity. For a temperature sensor of your own, make it the button's `entity` and delete `attribute: temperature`.

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

Tiles for what matters right now. Most of them only show at certain times or states. Made with Bubble Card and the [Header](bubble_card/README.md#header) and [Tiles](bubble_card/README.md#tiles) modules.

<img width="800" alt="Six favorite tiles in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/favorites.webp">

<details>
<summary>How it works</summary>

<br>

- All tiles are sub-buttons of one Bubble card. A tile with visibility conditions shows only while they hold, and a hidden tile leaves no gap.
- In my home, the oven shows while `sensor.oven` isn't `Off` and always comes first. The fan shows while it runs, or when the living room or bedroom is above 24°. Music shows from 22:00 to 4:00, dinner from 14:00 to 21:00 with the recipe photo, and the bath from 11:00 to 23:00 and whenever it runs.
- The vacuum and the printer always show. The vacuum shows its progress while it cleans and moves up. Bath and printer show the minutes left while they run.
- A tap toggles the fan, the bath and the printer. Music, oven and vacuum open pop-ups, dinner opens Tandoor. Holding a tile opens its entity.
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

All examples from my home. Without a progress entity, delete the vacuum's `name` template, or the tile reads **Error**.

</details>

### Rooms

A card for each room. The first four always show, **All rooms** shows the rest.

<img width="800" alt="The rooms header and four room cards" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/rooms.webp">

<details>
<summary>How it works</summary>

<br>

- Each room card is one Paper Buttons Row. A tap anywhere on it opens the room's pop-up.
- The icon lights up while the room's light is on.
- Below the name: a dot coloured by the room's temperature, from blue when cold to red when hot. Then a window icon if the room has a window contact, and temperature and humidity.
- The rooms header shows the average temperature and how many windows are open. A tap on either, or on the arrow, opens or closes the climate table.
- For another room, copy a card and change its entities, name, icon and pop-up. Add a `sensor.climate_<room>` for it in `packages/origami.yaml` and add the room to the lists there. Rooms after the fourth are conditional cards that show while `input_boolean.all_rooms` is on.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `light.<room>` | The icon |
| `sensor.climate_<room>` | Dot colour, temperature and humidity, from the [helpers](#under-the-hood) |
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
- One line per room: the coloured dot and name, window, thermostat, temperature and humidity. The living room also has the front door.
- An open window or door turns cyan, a thermostat that heats turns deep orange. Arrows mark the warmest and coldest room, and the most and least humid.
- A tap opens the room's thermostat. Window and door open their contact.
- It's one Paper Buttons Row laid out as a grid. Keep the room names short, so it fits on a phone. For another room, copy a line and raise the 6 in `repeat(6, …)`.

</details>

<details>
<summary>What it uses here</summary>

<br>

| Entity | For |
| --- | --- |
| `sensor.climate_<room>` | Temperature, humidity, arrows and dot colour |
| `binary_sensor.<room>_window`, `binary_sensor.front_door` | Windows and the door |
| `climate.<room>` | Target temperature and heating |
| `input_boolean.room_climate_card` | Open or closed |

For a room without a thermostat or window contact, delete that button. The cell stays empty, like in the office line.

</details>

### Navbar

A [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) with music, TV, lights, favorites and the menu. What's playing shows above it.

<img width="800" alt="The navbar with the now playing card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar.webp">

<details>
<summary>How it works</summary>

<br>

- Music and TV turn light blue while a speaker plays or the TV is on, Lights turns amber while a light is on. Badges on Music and Lights count them.
- Hold **Music** to play or pause the active speaker, unless it plays the TV. Hold **TV** for the TV's controls.
- Hold **Lights** for a menu: all lights on or off, and the living room scenes. The active scene is amber.
- **More** opens the menu from Origami Header. Admins see a dot on it while updates are waiting.
- The card above shows the speaker that matters right now, from `sensor.active_speaker`, or the TV.
- In Navbar Card's `[[[ ]]]` templates, read a state with `states['sensor.x'].state`. `states('sensor.x')` only works in Jinja and in the Bubble modules' `${ }`.

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

All examples from my home, apart from the helpers. Delete the routes and scenes you don't need.

</details>

## Bubble modules

Eleven modules give Bubble Card the Origami look. They're still experimental. Options and examples are in the [module reference](bubble_card/README.md).

<img width="800" alt="A pop-up built from the modules, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/module-popup.webp">

<details>
<summary>All modules</summary>

<br>

| Module | For |
| --- | --- |
| [Base](bubble_card/README.md#base) | Frame, text and colour for button and media player cards |
| [Light](bubble_card/README.md#light), [Fan](bubble_card/README.md#fan), [Climate](bubble_card/README.md#climate) | Lights, fans with speed keys, thermostats |
| [Media](bubble_card/README.md#media), [Speaker Select](bubble_card/README.md#speaker-select) | Speakers and TVs, speaker rows for a music pop-up. Written for LinkPlay speakers. |
| [Slider](bubble_card/README.md#slider), [Button](bubble_card/README.md#button) | A slider on its own, rows and grids of buttons |
| [Header](bubble_card/README.md#header), [Tiles](bubble_card/README.md#tiles), [Popup](bubble_card/README.md#popup) | Section headers, tile grids, pop-ups |

</details>

## Under the hood

<details>
<summary><b>Helpers</b></summary>

<br>

Most of the logic is in template sensors, so most cards only show a result. They're all in [`packages/origami.yaml`](packages/origami.yaml). To use them in your home:

- Copy one `Climate | <room>` block per room and set its two source sensors.
- The peaks and averages build sensor names from a list of rooms, like `sensor.<room>_temperature`. Name your sensors that way, or change those templates.
- Put your lights, window contacts and speakers into the counters, `sensor.active_target`, `sensor.active_speaker` and `input_select.selected_speaker`.

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

- If the source sensor is missing, temperature and humidity show `?`, so the card keeps its size. The comfort dot then shows the colour for 20°.
- The comfort colour goes from blue at 12° to red above 32°, a little transparent so it works in both modes.
- The peaks sensors return `none` when fewer than two rooms report or all read the same. Then no room gets an arrow.

</details>

<details>
<summary>Active speaker</summary>

<br>

`sensor.active_target` picks the speaker to show: one that plays, then one on a TV input or Bluetooth, then a paused one, then `input_select.selected_speaker`. If the playing speaker is grouped, the group leader wins. `sensor.active_speaker` has the same state and adds what it plays. The navbar reads that one.

| Attribute | |
| --- | --- |
| `chip` | `playing_art`, `playing_plain`, `paused_art`, `paused_plain`, `tv_art`, `tv_plain` or `off`. `_art` means there is a Music Assistant cover. |
| `title` | `Title \| Artist` while music plays, otherwise a short text like `Bluetooth`, `TV`, `Paused` or `Nothing playing`. Never empty. |
| `speaker` | Short name, with a count while grouped, like `Living +1` |
| `group` | `yes` or `no` |
| `artwork` | The cover or TV artwork, `none` without |

My speakers are LinkPlay speakers, each with a Music Assistant twin (`media_player.<name>_mass`). The twin gives title and cover while the speaker plays from Music Assistant. Music Assistant also reports playing while the speaker plays Bluetooth or one of its own streaming sources, so the twin is ignored then. Without a twin, the speaker's own title and cover are used, and `chip` stays `_plain`.

The TV input and Bluetooth are found by name in the speaker's `source`: optical, coax, HDMI or line in for the TV. Other brands may name them differently. `sensor.tv_source` is optional, its `display` and `artwork` attributes name and show what plays on the TV input.

</details>

</details>

<details>
<summary><b>Patterns</b></summary>

<br>

A few ideas that come up again and again in the dashboard.

<details>
<summary>Paper Buttons Row as a card</summary>

<br>

Used as a card, Paper Buttons Row has no background of its own. Give the row a background, radius and padding and it looks like a card. With `display: grid` you can place the buttons in rows and columns, like in the climate table. The room cards, the climate table and the server card are each one row.

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

Where it can, a sensor turns the numbers into one word, and the card picks a look for that word:

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
- **Room helpers return `?`** instead of an empty value.
- **Things that appear and disappear** either take no space while hidden, or keep their space and scale in and out.
- **Browser templates where possible.** `${ }` in the Header and Tiles modules and `[[[ ]]]` in Navbar Card show on the first frame. Jinja templates wait for the server.
- **A local font.** A font that loads late redraws all text.
- **Grid cards instead of stack cards** where a wrapper is needed.

</details>

<details>
<summary>One tap target per card</summary>

<br>

Give the row `position: relative`. An invisible button covers it and catches the tap, and the other buttons get `pointer-events: none`.

```yaml
type: custom:paper-buttons-row
styles:
  position: relative
base_config:
  styles:
    button:
      pointer-events: none
buttons:
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

One repeating gradient on the table's background, sized to the fixed row height. The 6 is the number of rooms.

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
.media-player::before,
.media-player::after {
  scale: var(--navbar-player-group, 0);
  transition: scale var(--duration-normal) var(--ease-standard);
}
```

</details>

</details>

<details>
<summary><b>Theme</b></summary>

<br>

The theme is a set of tokens. Home Assistant reads them, and so do the cards and modules. To restyle, change a few of them in [`origami.yaml`](themes/origami.yaml). A HACS update replaces that file, so keep your changes in a copy of the theme with another name.

<details>
<summary>Colour</summary>

<br>

Colour mostly means something is on. It goes into the icon, and the field behind it, `tint`, is that colour mixed into the card background: 14 % in light mode, 4 % in dark mode. `tint` uses `currentColor`, so set `color` on the same element.

Which colour a device gets is Home Assistant's own choice, like deep orange for heating or light blue for media. Origami re-tones those colours and changes two of them: switches and anything else that is simply on are green instead of amber, and open windows are cyan.

To change a colour, add `state-<domain>-active-color`. Domains with a colour per state need `state-<domain>-<state>-color`, like `state-climate-heat-color`.

</details>

<details>
<summary>Paper Buttons Row</summary>

<br>

Origami is named after Paper Buttons Row: origami is folded paper. Paper Buttons Row has its own colour variables, `pbs-button-*`, and the theme points them at the text colour. Entity buttons that are on still take their state colour, `active: []` turns that off, like in the room cards. The `rgb` ones need a triplet like `var(--rgb-primary-text-color)`, a hex value breaks the colour.

</details>

<details>
<summary>Tokens</summary>

<br>

| Token | |
| --- | --- |
| `ha-space-1` … `ha-space-5` | Spacing, 4 to 20 px |
| `ha-card-border-radius` | Card corners, 22 px. `radius-inner` is that divided by 1.3, about 17 px. `radius-small` is 8 px. |
| `border-width`, `border-contrast` | Card outlines, off by default |
| `card-shadow`, `card-shadow-flat`, `shadow-elevated` | Shadows per mode, flat for cards in pop-ups |
| `fill-subtle`, `fill-strong`, `fill-loud` | Neutral surfaces, mixed from `fill-base` so they stay warm |
| `fill-contrast` | How strong `fill-subtle` is, 8 % light and 5 % dark |
| `fill-active`, `toggle-on` | Text colour at 85 % on the card background, for switch tracks and emphasis |
| `tile-background`, `tile-chip-background` | The quiet tile and its raised icon chip |
| `opacity-disabled`, `opacity-quiet`, `opacity-muted`, `opacity-strong` | 0.3, 0.45, 0.6, 0.8 |
| `control-height-mini`, `-icon`, `-row`, `-list`, `-sm`, `control-height`, `-lg` | Fixed control heights: 32, 40, 44, 48, 52, 56 and 64 px |
| `tile-size`, `tile-size-lg`, `dot-size` | Icon chips, 48 and 80 px, dots 12 px |
| `icon-size-2xs` … `icon-size-xl` | Icons, 14 to 48 px |
| `ha-font-size-xs` … `ha-font-size-xxl` | Text, 14 to 26 px |
| `primary-font-family` | Montserrat, weights 500 and 700 |
| `duration-fast`, `duration-normal`, `duration-slow`, `ease-standard` | Motion |
| `press-scale` | How far a button shrinks when pressed |
| `blur-soft`, `blur-strong` | Header, dialogs, navbar |
| `tint-strength`, `fill-strength` | How much state colour goes into `tint` (14 % light, 4 % dark) and into coloured fills like slider fills (32 %, 30 %) |
| `amber-color`, `green-color`, … | Home Assistant's named colours, re-toned |

</details>

</details>

## Help

<details>
<summary>Origami isn't in the theme list</summary>

<br>

Home Assistant only loads themes when `configuration.yaml` has the `themes` line. Add it, restart, then pick Origami in your profile.

</details>

<details>
<summary>HACS can't find Origami</summary>

<br>

It isn't in the HACS store. Use the **Open in HACS** button under [Setup](#setup), it adds the repository for you.

</details>

<details>
<summary>A card says "Custom element doesn't exist"</summary>

<br>

The card isn't installed, or the browser still runs the old page. Install it and reload. In the Companion app, use **Reset frontend cache** in its settings.

</details>

<details>
<summary>The font doesn't change</summary>

<br>

Open `/local/fonts/montserrat.css` on your Home Assistant, like `http://homeassistant.local:8123/local/fonts/montserrat.css`. It should show two `@font-face` rules. If not, check that the files are in `/config/www/fonts`, and restart once if `www` is new. If it does, check that the file is in Resources as **Stylesheet** and that Origami is your theme, then reload.

</details>

<details>
<summary>Bubble cards look plain</summary>

<br>

The modules aren't loaded. Check that Bubble Card Tools is added as an integration and that the files are in `/config/bubble_card/modules/`, then reload.

</details>

<details>
<summary>A value shows ?</summary>

<br>

A helper can't find its source sensor. Replace the example sensors in `/config/packages/origami.yaml` with yours, then reload **Template entities** under [YAML configuration reloading](https://my.home-assistant.io/redirect/server_controls/).

</details>

<details>
<summary>Linking the font instead of copying it</summary>

<br>

Add `https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap` as the stylesheet instead. There are no files to copy, but the font then comes from Google's servers. It can arrive after the page, and then all text redraws.

</details>

## Status

- [x] Theme, dashboard and helpers
- [x] Bubble modules, still experimental
- [ ] Pop-ups: rooms, music, TV, favorites, lights and the rest
- [ ] Desktop layout

## License

[AGPL-3.0](LICENSE). The Montserrat files in `www/fonts` keep their own [SIL Open Font License](www/fonts/OFL.txt).
