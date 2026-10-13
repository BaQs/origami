# ◪ Origami

A chill Home Assistant theme, a set of Bubble Card modules made for it, and the dashboard built from both.

<img width="760" alt="The Origami dashboard on a phone, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/dashboard.webp">

| | |
| --- | --- |
| **Theme** | Soft, warm surfaces in light and dark, and colour only where something is on. Works with any dashboard. |
| **Dashboard** | Weather, favorites, rooms and a navbar, made for phones. Nothing jumps while it loads. |
| **Bubble modules** | The theme's look for Bubble Card. Still experimental. |

> [!NOTE]
> This is a personal dashboard that has grown over years. Some things are done in very specific ways, mostly to avoid layout shifts. A desktop layout is still in the works. Take what you like.

[Theme only](#theme-only) · [Dashboard](#dashboard) · [Help](#help) · [Reference](#reference)

## Theme only

1. Add this to `configuration.yaml` and restart Home Assistant. If there is a `frontend:` already, add the `themes` line below it.

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
   ```

2. Open the theme in HACS, confirm with **Add** if asked, then **Download**.

   [![Open your Home Assistant instance and open this repository inside HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme)

3. Run [`frontend.reload_themes`](https://my.home-assistant.io/redirect/developer_call_service/?service=frontend.reload_themes), then pick **Origami** in your [profile](https://my.home-assistant.io/redirect/profile/). **Auto** follows your device's dark mode.

4. For the full look, add the font: copy the folder [`www/fonts`](https://github.com/hazymorning/origami/tree/main/www/fonts) to `/config/www/fonts`, then add `/local/fonts/montserrat.css` as a **Stylesheet** in [Resources](https://my.home-assistant.io/redirect/lovelace_resources/).

## Dashboard

Tested with Home Assistant 2026.10. You need [HACS](https://hacs.xyz) and access to your `/config` folder, for example through the Samba share or Studio Code Server app.

### 1. Copy three folders

Download this repository (**Code**, then **Download ZIP**) and copy these folders into `/config`:

| Folder | What it is |
| --- | --- |
| `www/fonts` | The font |
| `bubble_card/modules` | The Bubble modules |
| `packages` | The helper sensors the cards read |

### 2. Edit configuration.yaml

Add this and restart. If `homeassistant:` or `frontend:` is already there, add the lines below it.

```yaml
homeassistant:
  packages: !include_dir_named packages

frontend:
  themes: !include_dir_merge_named themes
```

### 3. Install from HACS

Open each one, confirm with **Add** if asked, then **Download**.

| | For | |
| --- | --- | --- |
| [Origami](https://github.com/hazymorning/origami) | The theme | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme) |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | Favorites and section headers | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card&category=plugin) |
| [Bubble Card Tools](https://github.com/Clooos/Bubble-Card-Tools) | Loads the Bubble modules | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card-Tools&category=integration) |
| [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) | Greeting, climate table and room cards | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=jcwillox&repository=lovelace-paper-buttons-row&category=plugin) |
| [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) | The bar at the bottom | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=joseluis9595&repository=lovelace-navbar-card&category=plugin) |
| [Origami Notifications](https://github.com/hazymorning/origami_notifications) | Notifications next to the greeting | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_notifications&category=plugin) |
| [Origami Weather](https://github.com/hazymorning/origami_weather) | The weather card | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_weather&category=plugin) |

Restart once more, then [add Bubble Card Tools](https://my.home-assistant.io/redirect/config_flow_start/?domain=bubble_card_tools) as an integration.

### 4. Theme and font

- In your [profile](https://my.home-assistant.io/redirect/profile/), pick **Origami** as theme.
- In [Resources](https://my.home-assistant.io/redirect/lovelace_resources/), add `/local/fonts/montserrat.css` as **Stylesheet**.

### 5. Add the dashboard

1. Open [Dashboards](https://my.home-assistant.io/redirect/lovelace_dashboards/), **Add dashboard**, **New dashboard from scratch**, then open the new dashboard.
2. From the three dots, choose **Edit dashboard**, then **Raw configuration editor**.
3. Replace everything with [`dashboard.yaml`](dashboard/dashboard.yaml) and save.

Not using Origami Header (below)? Before saving, delete the card under `# Fold-out menu` and the route under `# Opens the Origami Header menu`.

### 6. Use your entities

The dashboard uses examples like `light.living_room`. In the raw editor, Ctrl+F (⌘F on a Mac) finds and replaces them. [Dashboard parts](docs/dashboard.md) lists what each part reads. The helpers use the same names, so rename them in `/config/packages/origami.yaml` too, then restart.

<details>
<summary><b>Optional: Origami Header</b></summary>

<br>

[Origami Header](https://github.com/hazymorning/origami_header) hides Home Assistant's header and turns it into a menu that folds out from the top, opened by **More** in the navbar. It's a bit experimental, but it's the only way I've found to hide the header completely, without a flicker. The dashboard works fine without it.

1. [Open it in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_header&category=plugin) and choose **Download**. It needs Home Assistant 2026.9 or newer.
2. Add the `extra_module_url` lines to the `frontend:` part of `configuration.yaml` and restart:

   ```yaml
   frontend:
     extra_module_url:
       - /hacsfiles/origami_header/origami-header.js
   ```

Edit mode is then in its menu, or add `?edit=1` to the dashboard's address.

</details>

## Help

<details>
<summary>HACS doesn't offer the theme</summary>

<br>

HACS only handles themes once Home Assistant loads them. Add the `themes` line to `configuration.yaml`, as in [Theme only](#theme-only), and restart.

</details>

<details>
<summary>A card says "Custom element doesn't exist"</summary>

<br>

The card isn't installed, or the browser still runs the old page. Install it from step 3 and reload. In the Companion app, use **Reset frontend cache** in its settings.

</details>

<details>
<summary>The font doesn't change</summary>

<br>

Open `/local/fonts/montserrat.css` on your Home Assistant's address, it should show two `@font-face` rules. If it doesn't, check that the files are in `/config/www/fonts`, and restart once if `www` is new. Then reload the page.

</details>

<details>
<summary>Bubble cards look plain</summary>

<br>

The modules aren't loaded. Check that Bubble Card Tools is added as an integration and that the files are in `/config/bubble_card/modules/`, then reload the page.

</details>

<details>
<summary>A value shows ?</summary>

<br>

A helper can't find its source sensor. Rename the entities in `/config/packages/origami.yaml` to yours, then reload **Template entities** under [YAML configuration reloading](https://my.home-assistant.io/redirect/server_controls/).

</details>

<details>
<summary>Linking the font instead of copying it</summary>

<br>

Add `https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap` as the stylesheet instead. No files to copy, but the font then loads from Google every time and arrives a moment late, which redraws all text.

</details>

## Reference

- [Dashboard parts](docs/dashboard.md): how each part works and which entities it reads
- [Helpers](docs/helpers.md): the template sensors behind the cards
- [Bubble modules](bubble_card/README.md): options and examples for all eleven modules
- [Patterns](docs/patterns.md): the ideas that repeat through every card
- [Theme](docs/theme.md): colour and tokens

## Status

- [x] Theme, dashboard and helpers
- [x] Bubble modules, still experimental
- [ ] Pop-ups: rooms, music, TV, favorites, lights and the rest
- [ ] Desktop layout
