# Helpers

Template sensors do the thinking, the cards only show the result. All of them and the toggles are in [`packages/origami.yaml`](../packages/origami.yaml), set up in [steps 1 and 2](../README.md#dashboard) of the readme.

| Helper | Gives | Read by |
| --- | --- | --- |
| `sensor.climate_<room>` | Rounded temperature and humidity, rank, comfort colour | Climate table, room cards |
| `sensor.climate_temperature_peaks`, `sensor.climate_humidity_peaks` | Warmest and most humid room, the lowest as attribute | The room sensors |
| `sensor.temperature_average`, `sensor.humidity_average` | Averages over all rooms | `sensor.climate_home` |
| `sensor.climate_home` | Averages, open windows, front door | Rooms header, status row |
| `sensor.windows_open_count` | Open windows | `sensor.climate_home`, status row |
| `sensor.greetings` | The greeting | Greeting |
| `sensor.lights_on_count`, `sensor.active_speaker_count` | Counters | Navbar badges |
| `sensor.active_target`, `sensor.active_speaker` | The speaker that matters and what it plays | Navbar |
| `input_boolean.room_climate_card`, `input_boolean.all_rooms` | Toggles | Rooms |
| `input_select.selected_speaker` | The speaker to fall back to | `sensor.active_target` |

## Room climate sensor

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

- A missing source gives `?` instead of an empty value. A card that shows `?` keeps its size, an empty one collapses and jumps when the value arrives.
- The comfort colour is a muted scale from blue at 12° to red above 32°. Its transparency lets it sit well in both modes.
- The peaks sensors return `none` while all rooms read the same. Without that, two rooms at the same temperature would fight over the arrow.

## Active speaker

The navbar doesn't watch every speaker, it reads one sensor that already picked the interesting one. `sensor.active_target` decides which: a playing speaker, then the TV or Bluetooth, then a paused one, then the one in `input_select.selected_speaker`. A playing group leader wins over its members. `sensor.active_speaker` prepares what the card shows:

| Attribute | |
| --- | --- |
| `chip` | `playing_art`, `playing_plain`, `paused_art`, `paused_plain`, `tv_art`, `tv_plain` or `off` |
| `title` | `Title \| Artist`, never empty and never stale |
| `speaker` | Short name, with a count while grouped, like `Living +1` |
| `group` | `yes` or `no` |
| `artwork` | The cover or TV artwork, `none` without |

It's written for LinkPlay speakers with a Music Assistant twin (`media_player.<name>_mass`). Music Assistant also reports playing for the speaker's own sources, like Bluetooth or Spotify, so its title and cover only count while none of them is on. Other speakers fall back to their own title and cover.

`sensor.tv_source` is optional. While a speaker plays the TV over optical, its `display` and `artwork` attributes say what's on. Without it the card shows TV.
