<img alt="Origami, a chill and clean dashboard for Home Assistant, on two phones in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/hero.webp">

<a href="https://github.com/hazymorning/origami/releases/latest"><img alt="Latest release" src="https://img.shields.io/github/v/release/hazymorning/origami?label=Release&color=6A8778&labelColor=181715"></a>
<a href="https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme"><img alt="HACS custom repository" src="https://img.shields.io/badge/HACS-Custom-6A8778?labelColor=181715"></a>

Origami is a Home Assistant theme with a matching phone dashboard. The dashboard is built mainly with [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) and [Bubble Card](https://github.com/Clooos/Bubble-Card), and everything else uses what Home Assistant already offers, such as themes, template sensors and scripts. The repository also contains modules that give Bubble cards the Origami look.

| Part | Description |
| --- | --- |
| Theme | The theme uses muted, warm colors in light and dark mode. Strong colors are used mainly for devices that are on. It works with any dashboard. |
| Dashboard | The dashboard shows the weather, favorites, rooms and a navigation bar. Its cards keep their size while the page loads. |
| Bubble Card modules | The modules give Bubble cards the Origami look. They are still experimental. The [module reference](bubble_card/README.md) explains them. |

> [!NOTE]
> The dashboard comes from a real home, so all entities are examples. Replace them with your own and remove the parts you do not need. Some cards are built in an unusual way so that nothing moves while the page loads. The [Techniques](#techniques) section explains how. The desktop layout is not finished yet.

[Setup](#setup) · [Dashboard](#dashboard) · [Pop-ups](#pop-ups) · [Helpers](#helpers) · [Techniques](#techniques) · [Theme](#theme) · [Troubleshooting](#troubleshooting)

## Setup

<details>
<summary><b>Theme only</b></summary>

<br>

1. Add these lines to `configuration.yaml` and restart Home Assistant. If `frontend:` is already in the file, add only the `themes` line under it.

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
   ```

2. Open Origami in HACS. If HACS asks you to add the repository, select **Add**. Then select **Download**.

   [![Open your Home Assistant instance and open this repository inside HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme)

3. Run [`frontend.reload_themes`](https://my.home-assistant.io/redirect/developer_call_service/?service=frontend.reload_themes) and select **Origami** in your [profile](https://my.home-assistant.io/redirect/profile/).
4. To use the font, copy [`www/fonts`](https://github.com/hazymorning/origami/tree/main/www/fonts) to `/config/www/fonts`. Then add `/local/fonts/montserrat.css` as a **Stylesheet** under [Resources](https://my.home-assistant.io/redirect/lovelace_resources/). If you just created the `www` folder, restart Home Assistant.

</details>

<details>
<summary><b>Full dashboard</b></summary>

<br>

The dashboard has been tested with Home Assistant 2026.10. You need [HACS](https://hacs.xyz) and access to the `/config` folder, for example through the Samba share or the Studio Code Server app.

<details>
<summary>1. Copy three folders</summary>

<br>

To download this repository, select **Code**, then **Download ZIP**. Copy these folders into `/config`.

| Folder | Contents |
| --- | --- |
| `www/fonts` | The Montserrat font |
| `bubble_card/modules` | The Bubble Card modules |
| `packages` | The helpers, with template sensors, toggles, a speaker selection, a timer, scripts and an automation |

</details>

<details>
<summary>2. Edit configuration.yaml</summary>

<br>

Add these lines and restart Home Assistant. If `homeassistant:` or `frontend:` is already in the file, add only the indented line under the existing key.

```yaml
homeassistant:
  packages: !include_dir_named packages

frontend:
  themes: !include_dir_merge_named themes
```

</details>

<details>
<summary>3. Install the cards from HACS</summary>

<br>

Open each repository in HACS. If HACS asks you to add the repository, select **Add**. Then select **Download**.

| Repository | Used for | Link |
| --- | --- | --- |
| [Origami](https://github.com/hazymorning/origami) | The theme | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme) |
| [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) | The greeting, the server card, the room cards, the climate table and the buttons in the pop-ups | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=jcwillox&repository=lovelace-paper-buttons-row&category=plugin) |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | The favorites, the section headers and the pop-ups | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card&category=plugin) |
| [Bubble Card Tools](https://github.com/Clooos/Bubble-Card-Tools) | Loading the Bubble Card modules | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card-Tools&category=integration) |
| [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) | The navigation bar at the bottom | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=joseluis9595&repository=lovelace-navbar-card&category=plugin) |
| [Origami Notifications](https://github.com/hazymorning/origami_notifications) | The notifications next to the greeting | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_notifications&category=plugin) |
| [Origami Weather](https://github.com/hazymorning/origami_weather) | The weather card | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_weather&category=plugin) |

Restart Home Assistant again. Then [add Bubble Card Tools](https://my.home-assistant.io/redirect/config_flow_start/?domain=bubble_card_tools) as an integration.

</details>

<details>
<summary>4. Select the theme and add the font</summary>

<br>

1. Select **Origami** as the theme in your [profile](https://my.home-assistant.io/redirect/profile/).
2. Add `/local/fonts/montserrat.css` as a **Stylesheet** under [Resources](https://my.home-assistant.io/redirect/lovelace_resources/).

</details>

<details>
<summary>5. Install Origami Header (optional)</summary>

<br>

Origami Header removes the Home Assistant header without any flicker while the page loads. In its place, a menu slides in from the top of the screen when you tap **More** in the navigation bar. Origami Header is still experimental, and the dashboard also works without it.

1. [Open Origami Header in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_header&category=plugin). If HACS asks you to add the repository, select **Add**. Then select **Download**. Origami Header requires Home Assistant 2026.9 or newer.
2. Add the `extra_module_url` lines under the existing `frontend:` key in `configuration.yaml`. Then restart Home Assistant.

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
     extra_module_url:
       - /hacsfiles/origami_header/origami-header.js
   ```

To edit the dashboard, use the edit button in the menu or add `?edit=1` to the address.

</details>

<details>
<summary>6. Add the dashboard</summary>

<br>

1. Open [Dashboards](https://my.home-assistant.io/redirect/lovelace_dashboards/) and select **Add dashboard**, then **New dashboard from scratch**. Open the new dashboard.
2. In the three-dot menu, select **Edit dashboard**, then **Raw configuration editor**.
3. Replace everything in the editor with the contents of [`dashboard.yaml`](dashboard/dashboard.yaml).
4. If you do not use Origami Header, delete the card under `# Menu (Origami Header)` and the route under `# Opens the Origami Header menu`.
5. Select **Save**.

</details>

<details>
<summary>7. Replace the example entities</summary>

<br>

The dashboard uses entities from one home. In the raw configuration editor, press Ctrl+F (⌘F on a Mac) to find an entity such as `light.living_room`, and replace it with your own. Do the same in the pop-ups at the end of the file. Delete the parts that do not apply to your home, such as a tile, a room or a navigation bar button. Each part described under [Dashboard](#dashboard) has a list of the entities it uses.

Then replace the same entities in `/config/packages/origami.yaml`. These are the temperature and humidity sensors, window contacts, front door, lights, speakers, scenes and fan. The dashboard reads the helpers that this file creates, such as `sensor.climate_living_room`. If you rename a helper, rename it in both files. Restart Home Assistant when you are done.

</details>

</details>

## Dashboard

The dashboard is a single sections view for phones. In [`dashboard.yaml`](dashboard/dashboard.yaml), each part starts with a comment such as `# Favorites`. Some buttons open a [pop-up](#pop-ups). The repository contains the pop-ups for the living room, the TV and the fan. Other pop-ups, such as those for music, lights and the remaining rooms, are not included yet, so their buttons do nothing.

<img width="760" alt="The Origami dashboard on a phone, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/dashboard.webp">

### Greeting and notifications

The greeting changes with the time of day, and the next calendar event appears above it. The Origami Notifications card is next to the greeting.

<img width="800" alt="The next event, the greeting and the notifications card" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/header.webp">

<details>
<summary>How it works</summary>

<br>

- The greeting and the event line above it form a single Paper Buttons Row. The event line shows the current or next event from `calendar.household`, and its text comes from the calendar's `message` attribute. To use another entity, change the entity and remove `attribute: message`. You can also delete the event line and keep only the greeting.
- [Origami Notifications](https://github.com/hazymorning/origami_notifications) shows Home Assistant's notifications, repairs and updates without any configuration. Each entity that you add to the card appears only when it is relevant, for example a timer while it runs. In this example, the card watches weather warnings, a calendar, a pizza timer, the oven, the vacuum and the dinner sensor.
- `hide_when_empty` is set to `false`, so the card also shows the weather and upcoming events after its entries. When there are no entries, the card shows only the weather and the events. With the default value `true`, the card is hidden when there is nothing to show.
- When the card has several entries, it switches to the next one every 3 seconds. Tap the card to open the full list.

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `sensor.greetings` | The greeting, from the [helpers](#helpers) |
| `calendar.household` | The event above the greeting. The notifications card shows events one day in advance. |
| `weather.home` | Rain, snow, thunderstorms and frost in the forecast, and the current weather |
| `sensor.dwd_*`, `timer.pizza`, `sensor.oven_ready_in`, `vacuum.robot`, `sensor.dinner` | These are examples. The card accepts almost any entity, as its [README](https://github.com/hazymorning/origami_notifications) explains. |

All of these entities are optional.

</details>

### Menu

The menu requires the optional [Origami Header](#setup), which removes the Home Assistant header. Origami Header and its menu are still experimental. **More** in the navigation bar opens the menu from the top of the screen.

<img width="800" alt="The menu opened from More in the navigation bar, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/menu.webp">

<details>
<summary>How it works</summary>

<br>

- The menu is an Origami Header card with `mode: menu`, and you decide what it contains. This example contains a server card and buttons for the File editor app, automations, the sidebar and edit mode. If you do not use the File editor app, change or delete the **Files** button.
- The server card shows a status, the uptime, the processor load and the memory use. Tap the processor or memory value to open its details. Tap anywhere else on the card to open the server pop-up, which is not included yet.
- The status consists of an icon and a short message. The icon shows the overall state, and the message gives the reason, for example **Backup due**.

  | Icon | Meaning |
  | --- | --- |
  | Green check | Everything is fine. |
  | Arrow in the accent color | Updates are available. |
  | Orange | The disk is more than 80% full, the memory use is above 85%, the processor load is above 80%, the last automatic backup is older than 3 days, or System Monitor sends no data. |
  | Red | The disk is more than 90% full, the memory use or processor load is above 95%, or the last automatic backup is older than 7 days. |

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `sensor.system_monitor_*` | The processor load, memory use, disk use and uptime, from [System Monitor](https://www.home-assistant.io/integrations/systemmonitor/). Without these sensors, the status shows **No data**. |
| `sensor.backup_last_successful_automatic_backup` | The backup check. Without this sensor, the card does not check backups. |

</details>

### Weather

The weather card is an [Origami Weather](https://github.com/hazymorning/origami_weather) card. The card has many options that this example does not use, such as an animated sky, forecasts, gauges and your own images. This example uses a simple setup with one color for each weather condition.

<img width="800" alt="The weather card in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/weather.webp">

<details>
<summary>Configuration</summary>

<br>

- `background_mode` is set to `color`, and `background_thresholds` sets a color for each condition in light and dark mode.
- The sun, moon, clouds, stars, birds, balloons and planes are turned off. Rain, snow, hail and thunderstorms still appear, because `precipitation_effects` is on by default.
- The house is the card's demo house. The Origami Weather README explains how to create a picture of [your own home](https://github.com/hazymorning/origami_weather#layouts) in the same style.
- The top-left corner shows the next sunset, or the next sunrise while the sun is down. The bottom-left corner shows the temperature and condition from the weather entity. To use your own temperature sensor, set it as the `entity` of the button that shows the temperature, and delete `attribute: temperature`.

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `weather.home` | The condition, color and temperature |
| `sun.sun` | The next sunset or sunrise |

</details>

### Favorites

The favorites are tiles for devices and tasks that are relevant at the moment. Most tiles appear only at certain times or in certain states. The tiles use Bubble Card with the [Header](bubble_card/README.md#header) and [Tiles](bubble_card/README.md#tiles) modules.

<img width="800" alt="Six favorite tiles in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/favorites.webp">

<details>
<summary>How it works</summary>

<br>

- All tiles are sub-buttons of one Bubble card. A tile with visibility conditions appears only while its conditions are met. A hidden tile leaves no gap.
- In this example, the oven tile is always first and appears while `sensor.oven` is not `Off`. The fan tile appears while the fan runs or when the living room or bedroom is warmer than 24°. Music appears from 22:00 to 4:00, dinner from 14:00 to 21:00 with a photo of the recipe, and the bath from 11:00 to 23:00 and whenever it runs.
- The vacuum and printer tiles are always visible. While the vacuum is cleaning, its tile shows the progress and moves to the front. The bath and printer tiles show the remaining minutes while they run.
- Tap the fan, bath or printer tile to turn it on or off. The music, oven and vacuum tiles open pop-ups, and the dinner tile opens Tandoor. Hold a tile to open the details for its entity.
- To add a tile, copy an existing tile and change its entity and visibility conditions.

</details>

<details>
<summary>Entities</summary>

<br>

| Tile | Entities |
| --- | --- |
| Oven | `sensor.oven` |
| Vacuum | `vacuum.robot`, `number.robot_cleaning_progress` |
| Fan | `fan.bedroom`, `sensor.living_room_temperature`, `sensor.bedroom_temperature` |
| Dinner | `sensor.dinner` with a `recipe.image` attribute, for example from [Tandoor](https://tandoor.dev) |
| Music | `media_player.bedroom` |
| Bath | `input_boolean.bath_time`, `input_datetime.bath_time_end` |
| Printer | `switch.printer`, `input_datetime.printer_end` |

All of these entities are examples. If your vacuum has no progress entity, delete the `name` template from the vacuum tile, or the tile shows **Error**.

</details>

### Rooms

Each room has a card. The first four rooms are always visible, and **All rooms** shows the others.

<img width="800" alt="The rooms header and four room cards" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/rooms.webp">

<details>
<summary>How it works</summary>

<br>

- Each room card is a single Paper Buttons Row. Tap anywhere on the card to open the room's [pop-up](#room).
- The icon is colored while the room's light is on.
- Below the name, a colored dot shows the room's temperature, from blue when it is cold to red when it is hot. A window icon follows if the room has a window contact. The temperature and humidity come last.
- The rooms header shows the average temperature and the number of open windows. Tap one of these values or the arrow to open or close the climate table.
- To add a room, copy a card and change its entities, name, icon and pop-up. Add a `sensor.climate_<room>` for the room in `packages/origami.yaml`, and add the room to the lists in that file. The rooms after the fourth are conditional cards, which appear while `input_boolean.all_rooms` is on.

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `light.<room>` | The icon color |
| `sensor.climate_<room>` | The dot color, temperature and humidity, from the [helpers](#helpers) |
| `binary_sensor.<room>_window` | The window icon. In the living room, this is a balcony door. |
| `sensor.climate_home` | The average temperature and the open windows in the header |
| `input_boolean.all_rooms` | The **All rooms** button |

</details>

### Climate table

The climate table shows all rooms at once. It opens under the rooms header.

<img width="800" alt="The rooms header with the climate table open" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/climate.webp">

<details>
<summary>How it works</summary>

<br>

- Tap the arrow or one of the values in the rooms header to open or close the table.
- Each row shows one room with its colored dot, name, window, thermostat, temperature and humidity. The living room row also shows the front door.
- An open window or door turns cyan, and a thermostat that is heating turns deep orange. Arrows mark the warmest and coldest rooms and the most and least humid rooms.
- Tap a room to open its thermostat. Tap a window or the door to open its contact sensor.
- The table is a single Paper Buttons Row with a grid layout. Keep the room names short so that the table fits on a phone. To add a room, copy a row and increase the 6 in `repeat(6, …)`.

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `sensor.climate_<room>` | The temperature, humidity, arrows and dot color |
| `binary_sensor.<room>_window`, `binary_sensor.front_door` | The windows and the door |
| `climate.<room>` | The target temperature and whether the thermostat is heating |
| `input_boolean.room_climate_card` | Whether the table is open |

If a room has no thermostat or window contact, delete that button. The cell stays empty, as in the office row.

</details>

### Navigation bar

The navigation bar is a [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) with buttons for music, the TV, the lights, the favorites and the menu. A card above the bar shows what is playing.

<img width="800" alt="The navigation bar with the card that shows what is playing" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar.webp">

<details>
<summary>How it works</summary>

<br>

- The music button turns light blue while a speaker is playing, and the TV button turns light blue while the TV is on. The lights button turns amber while a light is on. Badges on the music and lights buttons show how many speakers are playing and how many lights are on.
- Hold **Music** to play or pause the active speaker, unless the speaker is playing TV audio. Tap **TV** to open the [TV pop-up](#tv), or hold it to open the TV entity's controls.
- Hold **Lights** to open a menu. The menu contains the living room scenes and a button that turns all lights on or off. The active scene is amber.
- **More** opens the Origami Header menu. Administrators see a dot on this button while updates are available.
- The card above the bar shows either the TV or the most relevant speaker, which comes from `sensor.active_speaker`.
- In Navbar Card's `[[[ ]]]` templates, read a state with `states['sensor.x'].state`. The `states('sensor.x')` function only works in Jinja and in the `${ }` templates of the Bubble Card modules.

<img width="600" alt="The light scenes, which open when you hold Lights" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/navbar-scenes.webp">

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `sensor.active_speaker_count`, `sensor.lights_on_count` | The numbers on the badges, from the [helpers](#helpers) |
| `sensor.active_speaker` | The card above the bar, from the helpers |
| `media_player.living_room_tv`, `media_player.tv_ump` | The TV button and the TV in the card above the bar |
| `light.all_lights`, `script.lights_toggle` | Turning all lights on or off |
| `scene.living_room_*`, `sensor.living_room_scene` | The scenes and the active scene |

Except for the helpers, all of these entities are examples. Delete the buttons and scenes that you do not need.

</details>

## Pop-ups

The dashboard contains three [Bubble Card](https://github.com/Clooos/Bubble-Card) pop-ups for a room, the TV and the fan. They are at the end of [`dashboard.yaml`](dashboard/dashboard.yaml), under `# Pop-ups`. A pop-up opens when a button navigates to its hash, such as `#popup_tv`. To close a pop-up, tap the close button or swipe down. The scripts and helpers that the pop-ups use are in the [package](packages/origami.yaml).

### Room

Each room can have the same kind of pop-up, which opens from its room card. From top to bottom, the pop-up shows the media in the room, the lights and the climate. The living room pop-up is the example to copy for other rooms.

<img width="800" alt="The living room pop-up in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/popup-room.webp">

<details>
<summary>How it works</summary>

<br>

- The pop-up's hash is `#popup_living_room`, which is the living room card's navigation path.
- The media card shows the TV while it is on, and the speaker at all other times. Tap the TV card to open the TV pop-up. Smaller controls for the room go under the media card. In this example, a smart plug appears as a compact row with a switch.
- The lights header shows how many lights in the room's light group are on. The power button on the **All** card turns the whole group on or off, and the arrow next to it shows the scenes. A tap on the card opens the group's details. The scene that was turned on last is highlighted.
- The climate header shows the temperature and humidity, with a dot in the room's comfort color. Tap the fan card to open the [fan pop-up](#fan). The plus and minus buttons change the thermostat's target temperature by half a degree.
- **Everything off** turns off the lights, fans, media players and switches in the room's [area](https://www.home-assistant.io/docs/organizing/areas/). The list of these domains is in `script.room_off`.
- When you close the pop-up, the scenes are hidden again.

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `media_player.living_room_tv`, `media_player.living_room_tv_cast` | The TV card, which the [TV](#tv) section explains |
| `media_player.living_room` | The speaker and both volume sliders |
| `switch.living_room_console` | The smart plug |
| `light.living_room` | The room's light group. The header counts the lights in this group. |
| `scene.living_room_*`, `sensor.living_room_scene` | The scenes and the scene that was turned on last |
| `input_boolean.living_room_scenes` | Whether the scenes are visible |
| `sensor.climate_living_room` | The temperature, humidity and comfort color |
| `fan.living_room`, `climate.living_room` | The fan and the thermostat |
| `script.thermostat_step`, `script.room_off` | The thermostat buttons and **Everything off** |

All of these entities are examples. The scripts, the toggle and `sensor.living_room_scene` are in the package, and the sensor contains the list of scenes.

To create a pop-up for another room, copy the living room pop-up and give it the hash that the room card opens, such as `#popup_bedroom`. Then change the name, the entities and the `area` of **Everything off**. The new room's scenes need their own toggle and scene sensor. You can also remove the scenes. A different fan needs its own fan pop-up.

</details>

### TV

The TV pop-up is a remote control for the TV. It opens from **TV** in the navigation bar or from the TV card in the room pop-up.

<img width="800" alt="The TV pop-up in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/popup-tv.webp">

<details>
<summary>How it works</summary>

<br>

- The top card shows the TV, its current program and its power button. A separate speaker plays the TV's sound, and the card's slider sets the speaker's volume. A blurred picture of the current program appears behind the pop-up.
- The input button sends the `TV_INPUT` command to the TV. The **TV** button switches the speaker to its TV input, and it is filled while the speaker uses that input.
- The minus and plus buttons change the volume by 1%. A double tap changes it by 5%, and holding the button changes it by 10%. Tap the number to mute the speaker.
- The app buttons scroll sideways, and the running app's button is filled. Tap an app to start it with `media_player.select_source`.
- The remote has a back button, a home button and a round pad with an OK button. Hold an arrow button to repeat its command.

The commands, such as `DPAD_UP`, `DPAD_CENTER` and `TV_INPUT`, are for the [Android TV Remote](https://www.home-assistant.io/integrations/androidtv_remote/) integration. Other remotes use different command names. The app buttons need your TV's source names.

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `media_player.living_room_tv` | The TV and its apps, from Android TV Remote |
| `remote.living_room_tv` | The remote, from Android TV Remote |
| `media_player.living_room_tv_cast` | The current program and its picture, from Google Cast |
| `media_player.living_room` | The speaker that plays the TV's sound |
| `script.volume` | The volume steps and mute, from the package |

In this example, the speaker's TV input is called `Optical In`. If you do not have a separate speaker, point the volume buttons and the slider to the TV and delete the **TV** button.

</details>

### Fan

The fan pop-up controls one fan. It sets the speed, the mode and a timer, and turns oscillation on or off. The pop-up opens from the fan card in the room pop-up.

<img width="800" alt="The fan pop-up in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/popup-fan.webp">

<details>
<summary>How it works</summary>

<br>

- The title shows the fan's name. Both the power button and the fan button turn the fan on and off, and the fan icon spins while the fan is running.
- The arrow buttons turn the fan's head with `fan.set_direction`. This works with fans that use the direction setting to turn the head, such as Xiaomi fans. The middle button turns oscillation on or off.
- The slider can set any speed. The leaf button selects the `nature` preset, and the wind button selects `normal`. Buttons 1, 2 and 3 set the speed to 33, 66 and 100%.
- The timer button shows the timer row, which has the options Off, 1 hour, 3 hours and 5 hours. When the timer ends, an automation in the package turns the fan off.
- Small templates in the buttons decide which button is filled, for example the button for the fan's current speed range.

</details>

<details>
<summary>Entities</summary>

<br>

| Entity | Purpose |
| --- | --- |
| `fan.living_room` | The fan, which is an example |
| `timer.living_room_fan` | The timer, together with the automation that turns the fan off |
| `input_boolean.fan_popup_timer` | Whether the timer row is visible |
| `script.fan_oscillation` | Turning oscillation on or off |

The timer, the toggle, the script and the automation are in the package. To use your own fan, replace `fan.living_room` in the pop-up and in the automation. If your fan uses other preset names, change `nature` and `normal`. You can delete the buttons that your fan does not support, such as the arrows. A second fan needs its own copy of the pop-up with its own hash, timer, toggle and automation.

</details>

## Helpers

Most of the logic is in template sensors, so the cards usually only display a result. All helpers are in [`packages/origami.yaml`](packages/origami.yaml), together with the scripts that the pop-ups use. To use the helpers in your home, make these changes.

- Copy one `Climate | <room>` block for each room and set its two source sensors.
- The peak and average sensors build sensor names from a list of rooms, such as `sensor.<room>_temperature`. Name your sensors in the same way, or change these templates.
- Add your lights, window contacts and speakers to the counters, to `sensor.active_target`, to `sensor.active_speaker` and to `input_select.selected_speaker`.
- Add your scenes to `sensor.living_room_scene`, and add your fan to the **Living room fan timer** automation.
- Assign your devices to [areas](https://www.home-assistant.io/docs/organizing/areas/) so that **Everything off** in a room pop-up can find them.

| Helper | Purpose |
| --- | --- |
| `sensor.climate_<room>` | A room's rounded temperature and humidity, rank and comfort color |
| `sensor.climate_temperature_peaks`, `sensor.climate_humidity_peaks` | The warmest room and the most humid room. The coldest and driest rooms are attributes. |
| `sensor.temperature_average`, `sensor.humidity_average`, `sensor.climate_home` | The averages, the open windows and the front door |
| `sensor.windows_open_count`, `sensor.lights_on_count`, `sensor.active_speaker_count` | The number of open windows, lights that are on and speakers that are playing |
| `sensor.greetings` | The greeting |
| `sensor.active_target`, `sensor.active_speaker` | The most relevant speaker and what it is playing |
| `input_boolean.room_climate_card`, `input_boolean.all_rooms`, `input_select.selected_speaker` | The toggles for the climate table and the extra rooms, and the speaker to show when nothing is playing |
| `sensor.living_room_scene`, `input_boolean.living_room_scenes` | The scene that was turned on last, and the toggle for the scenes in the room pop-up |
| `timer.living_room_fan`, `input_boolean.fan_popup_timer` | The fan pop-up's timer and the toggle for its timer row |
| `script.fan_oscillation`, `script.volume`, `script.thermostat_step`, `script.room_off` | The actions for the pop-up buttons |
| The **Living room fan timer** automation | Turning the fan off when its timer ends |

<details>
<summary>Room climate sensor</summary>

<br>

Each room has one climate sensor. The state is the temperature rounded to one decimal place, and the cards read the attributes.

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

- If the source sensor is missing, the temperature and humidity show `?`, so the card keeps its size. The comfort dot then uses the color for 20°.
- The comfort color goes from blue at 12° to red above 32°. The color is slightly transparent so that it works in both modes.
- The peak sensors return `none` when fewer than two rooms report a value or when all values are equal. In that case, no room gets an arrow.

</details>

<details>
<summary>Active speaker</summary>

<br>

`sensor.active_target` selects the speaker to show. It prefers a speaker that is playing, then a speaker on a TV input or on Bluetooth, then a paused speaker, and finally `input_select.selected_speaker`. If the playing speaker is part of a group, the sensor uses the group leader. `sensor.active_speaker` has the same state and adds what the speaker is playing. The navigation bar reads this second sensor.

| Attribute | Content |
| --- | --- |
| `chip` | `playing_art`, `playing_plain`, `paused_art`, `paused_plain`, `tv_art`, `tv_plain` or `off`. `_art` means that a picture is available. For music, this is the cover from Music Assistant. For the TV input, it is the `artwork` attribute of `sensor.tv_source`. |
| `title` | While music is playing, the title is `Title \| Artist`. Otherwise, it is a short label such as `Bluetooth`, `TV`, `Paused` or `Nothing playing`, so it is never empty. |
| `speaker` | The speaker's short name, with a count while it is grouped, such as `Living +1` |
| `group` | `yes` or `no` |
| `artwork` | The cover or the TV picture, or `none` |

The example uses LinkPlay speakers. Each speaker also has a player in Music Assistant, such as `media_player.<name>_mass`. This player provides the title and cover while the speaker is playing from Music Assistant. The Music Assistant player also reports `playing` while the speaker plays over Bluetooth or from one of its own streaming sources, so the sensor ignores the player in these cases. Without a Music Assistant player, the sensor uses the speaker's own title and cover, and `chip` never uses `playing_art` or `paused_art`.

The sensor finds the TV input and Bluetooth by name in the speaker's `source`. It treats optical, coaxial, HDMI and line inputs as TV inputs. Other brands may use different names. `sensor.tv_source` is optional. Its `display` and `artwork` attributes provide the name and picture of what is playing on the TV input.

</details>

## Techniques

These techniques appear in several places in the dashboard.

<details>
<summary>Paper Buttons Row as a card</summary>

<br>

Paper Buttons Row has no background when you use it as a card. To make it look like a card, give the row a background, a corner radius and padding. With `display: grid`, you can arrange the buttons in rows and columns, as in the climate table. The room cards, the climate table and the server card are each a single Paper Buttons Row.

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
<summary>Logic in sensors</summary>

<br>

Where possible, a sensor turns numbers into a single word, and the card changes its look based on that word. In this example from the climate table, a room gets an arrow when it is the warmest or the coldest room.

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

- Cards have fixed heights, so long text never moves other elements.
- State changes affect colors, but never sizes.
- The room helpers return `?` instead of an empty value.
- Elements that appear and disappear either take no space while they are hidden or keep their space and scale in and out.
- Templates run in the browser where possible. The `${ }` templates in the Header and Tiles modules and the `[[[ ]]]` templates in Navbar Card render in the first frame. Jinja templates have to wait for the server.
- The font loads from Home Assistant itself. A font that loads late makes all text redraw.
- The main view uses grid cards instead of stack cards where it needs a wrapper.

</details>

<details>
<summary>One tap target for each card</summary>

<br>

Give the row `position: relative`. An invisible button covers the whole row and receives every tap. All other buttons get `pointer-events: none`.

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
<summary>Hidden until needed</summary>

<br>

The favorites use Bubble Card's visibility conditions. The extra rooms and the climate table appear while a toggle is on. Hidden elements leave no gap.

</details>

<details>
<summary>Sorting tiles</summary>

<br>

The tiles are sorted with the CSS `order` property. The oven comes first with `order: -2`, and the vacuum follows while it is cleaning.

```yaml
origami_tiles:
  order:
    cleaning: -1
```

</details>

<details>
<summary>Striped rows</summary>

<br>

The climate table's background is a single repeating gradient, which is as tall as two table rows. In `repeat(6, …)`, the 6 is the number of rooms.

```yaml
styles:
  grid-template-rows: repeat(6, var(--control-height-icon))
  background-image: >-
    repeating-linear-gradient(to bottom, transparent, transparent var(--control-height-icon),
    var(--fill-subtle) var(--control-height-icon), var(--fill-subtle) calc(var(--control-height-icon) * 2))
```

</details>

<details>
<summary>Scaling instead of hiding</summary>

<br>

To show and hide an element without moving anything else, scale it instead of removing it. CSS cannot read entity states, so a Navbar Card template writes the group state into a CSS variable.

```js
document.documentElement.style.setProperty('--navbar-player-group', a.group === 'yes' ? '1' : '0');
```

The group badge on the cover scales with this variable.

```css
.media-player::before,
.media-player::after {
  scale: var(--navbar-player-group, 0);
  transition: scale var(--duration-normal) var(--ease-standard);
}
```

</details>

## Theme

The theme is a set of tokens. Home Assistant reads these tokens, and so do the cards and modules. To change the style, copy [`origami.yaml`](themes/origami.yaml) to a new file in `/config/themes`, such as `my-origami.yaml`. Give the theme in the copy a new name by changing `Origami:` at the top, and then edit its tokens. Do not edit the original file, because a HACS update replaces it.

<details>
<summary>Color</summary>

<br>

Color mostly shows that something is on. The icon is colored, and the area behind it uses `tint`, which mixes the same color into the card background. The mix is 14% in light mode and 4% in dark mode. `tint` uses `currentColor`, so set `color` on the same element.

Home Assistant decides which color a device gets, for example deep orange for heating and light blue for media. Origami adjusts the tone of these colors and changes a few of them. Switches and other entities without a color of their own are green instead of amber when they are on, and open windows are cyan. Paused media players and people who are away use the secondary text color, and closed covers and disarmed alarm panels use the accent color.

To change a color, add `state-<domain>-active-color` to your copy of the theme. Domains with a color for each state need `state-<domain>-<state>-color`, such as `state-climate-heat-color`.

</details>

<details>
<summary>Paper Buttons Row</summary>

<br>

The name Origami comes from Paper Buttons Row, because origami is folded paper. Paper Buttons Row has its own color variables, `pbs-button-*`, and the theme sets them to the text color. Entity buttons that are on still use their state color. To turn this off, set `active: []`, as in the room cards. The `rgb` variables need a triplet such as `var(--rgb-primary-text-color)`, because a hex value breaks the color.

</details>

<details>
<summary>Tokens</summary>

<br>

| Token | Purpose |
| --- | --- |
| `ha-space-1` … `ha-space-5` | Spacing from 4 to 20 px |
| `ha-card-border-radius` | The card corner radius, 22 px. `radius-inner` is this value divided by 1.3, about 17 px. `radius-small` is 8 px. |
| `border-width`, `border-contrast` | Card outlines, which are off by default |
| `card-shadow`, `card-shadow-flat`, `shadow-elevated` | Shadows for each mode. `card-shadow-flat` only has the small contact shadow, and the card above the navigation bar uses `shadow-elevated`. |
| `fill-subtle`, `fill-strong`, `fill-loud` | Neutral surfaces, mixed from `fill-base` so that they stay warm |
| `fill-contrast` | The strength of `fill-subtle`, 8% in light mode and 5% in dark mode |
| `fill-active`, `toggle-on` | The text color at 85% on the card background, for switch tracks and active buttons |
| `tile-background`, `tile-chip-background` | A tile's background and its icon's background |
| `opacity-disabled`, `opacity-quiet`, `opacity-muted`, `opacity-strong` | Opacity levels of 0.3, 0.45, 0.6 and 0.8 |
| `control-height-mini`, `-icon`, `-row`, `-list`, `-sm`, `control-height`, `-lg` | Fixed control heights of 32, 40, 44, 48, 52, 56 and 64 px |
| `tile-size`, `tile-size-lg`, `dot-size` | Icon backgrounds of 48 and 80 px, and dots of 12 px |
| `icon-size-2xs` … `icon-size-xl` | Icon sizes from 14 to 48 px |
| `ha-font-size-xs` … `ha-font-size-xl` | Text sizes from 14 to 22 px |
| `primary-font-family` | Montserrat in weights 500 and 700 |
| `duration-fast`, `duration-normal`, `duration-slow`, `ease-standard` | Animation timing |
| `press-scale` | How much a button shrinks when you press it |
| `blur-soft`, `blur-strong` | The blur behind the header, dialogs and the navigation bar |
| `tint-strength`, `fill-strength` | The amount of state color in `tint` (14% in light mode, 4% in dark mode) and in colored fills such as sliders (32% and 30%) |
| `amber-color`, `green-color`, … | Home Assistant's named colors, with adjusted tones |

</details>

## Troubleshooting

<details>
<summary>Origami is not in the theme list</summary>

<br>

Home Assistant only loads themes when `configuration.yaml` contains the `themes` line. Add the line, restart Home Assistant and select Origami in your profile.

</details>

<details>
<summary>HACS cannot find Origami</summary>

<br>

Origami is not in the default HACS store. Use the **Open in HACS** button under [Setup](#setup), which adds the repository for you.

</details>

<details>
<summary>A card shows "Custom element doesn't exist"</summary>

<br>

The card is not installed, or the browser is still using an old version of the page. Install the card and reload the page. In the Companion app, select **Reset frontend cache** in the app settings.

</details>

<details>
<summary>The font does not change</summary>

<br>

In a browser, open `/local/fonts/montserrat.css` on your Home Assistant server, for example `http://homeassistant.local:8123/local/fonts/montserrat.css`. The file should contain two `@font-face` rules. If the file does not open, check that the font files are in `/config/www/fonts`, and restart Home Assistant if you just created the `www` folder. If the file opens, check that it is under Resources as a **Stylesheet** and that Origami is your theme. Then reload the page.

</details>

<details>
<summary>Bubble cards look plain</summary>

<br>

The modules are not loaded. Check that Bubble Card Tools is added as an integration and that the module files are in `/config/bubble_card/modules/`. Then reload the page.

</details>

<details>
<summary>A tap does not open a pop-up</summary>

<br>

The pop-up must be in the same view as the button, and its `hash` must match the button's `navigation_path`, such as `#popup_tv`. The included pop-ups are at the end of `dashboard.yaml`, under `# Pop-ups`. Bubble Card must also be installed.

</details>

<details>
<summary>A value shows a question mark</summary>

<br>

A helper cannot find its source sensor. Replace the example sensors in `/config/packages/origami.yaml` with your own. Then reload **Template entities** under [YAML configuration reloading](https://my.home-assistant.io/redirect/server_controls/).

</details>

<details>
<summary>Loading the font from Google Fonts</summary>

<br>

Instead of copying the font files, you can add `https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap` as the stylesheet. In that case, the font loads from Google's servers. It may arrive after the page, and then all text redraws.

</details>

## Status

- [x] Theme, dashboard and helpers
- [x] Bubble Card modules, which are still experimental
- [x] Pop-ups for a room, the TV and the fan
- [ ] More pop-ups, such as those for music, lights, favorites and the server
- [ ] Desktop layout

## License

Origami is licensed under [AGPL-3.0](LICENSE). The Montserrat files in `www/fonts` remain under their own [SIL Open Font License](www/fonts/OFL.txt).
