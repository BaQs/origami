# ◪ Origami

A chill Home Assistant theme, a set of Bubble Card modules made for it, and the dashboard built from both.

<img width="760" alt="The Origami dashboard on a phone, in light and dark mode" src="https://raw.githubusercontent.com/hazymorning/origami/main/images/dashboard.webp">

- Light and dark mode
- Soft, warm surfaces, and colour only where something is on
- Content that only shows up while it matters
- Nothing jumps while the dashboard loads

> [!NOTE]
> This is a personal dashboard that has grown over years. Some things are done in very specific ways, mostly to avoid layout shifts. Take what you like.

[Theme only](#theme-only) · [Whole dashboard](#whole-dashboard) · [Troubleshooting](#troubleshooting) · [Reference](#reference)

## Theme only

[![Open your Home Assistant instance and open this repository inside HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami&category=theme)

1. The button opens this repository in your HACS. Confirm with **Add** if asked, then **Download**.
2. Restart Home Assistant, or run the action [`frontend.reload_themes`](https://my.home-assistant.io/redirect/developer_call_service/?service=frontend.reload_themes).
3. Open your [profile](https://my.home-assistant.io/redirect/profile/) and pick **Origami** as theme. **Auto** follows your device's dark mode.

The theme is made for its font, Montserrat. [Step 2](#whole-dashboard) of the whole dashboard adds it.

<details>
<summary>Without the button, or without HACS</summary>

<br>

**HACS, by hand:** open HACS, then the three dots, **Custom repositories**. Add `https://github.com/hazymorning/origami` with the type **Theme** and download Origami.

**Without HACS:** copy [`themes/origami.yaml`](themes/origami.yaml) into the `themes` folder of your config, then reload the themes. Without a `themes` folder yet, create it next to `configuration.yaml`, add this once and restart:

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

</details>

## Whole dashboard

You need Home Assistant 2026.9 or newer, [HACS](https://hacs.xyz), and a way to edit files in `/config`, like the File editor or Studio Code Server app. Go through the steps in order.

**1. Theme**, as in [Theme only](#theme-only).

<details>
<summary><b>2. Font</b></summary>

<br>

Origami uses [Montserrat](https://fonts.google.com/specimen/Montserrat) in the weights 500 and 700, served by your own Home Assistant. That way it's there instantly, even offline. A font that arrives late redraws all text, and that is a layout shift.

1. Copy the folder [`www/fonts`](https://github.com/hazymorning/origami/tree/main/www/fonts) to `/config/www/fonts`. If `/config/www` is new, restart Home Assistant once.
2. Open [Resources](https://my.home-assistant.io/redirect/lovelace_resources/), add `/local/fonts/montserrat.css` with the type **Stylesheet**, and reload the page.

The folder holds the two weights, their `@font-face` rules and the font's license.

<details>
<summary>Link it from Google Fonts instead</summary>

<br>

Add `https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap` as the stylesheet. No files to copy, but the font then loads from Google every time and shows up a moment late.

</details>

</details>

<details>
<summary><b>3. Cards</b></summary>

<br>

Each link opens the card in your HACS. Confirm with **Add** if asked, then **Download**.

| Card | For | |
| --- | --- | --- |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | Favorites, section headers and the pop-ups | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card&category=plugin) |
| [Bubble Card Tools](https://github.com/Clooos/Bubble-Card-Tools) | Keeps the Bubble modules as files | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card-Tools&category=integration) |
| [Paper Buttons Row](https://github.com/jcwillox/lovelace-paper-buttons-row) | Greeting, server card, climate table and room cards | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=jcwillox&repository=lovelace-paper-buttons-row&category=plugin) |
| [Navbar Card](https://github.com/joseluis9595/lovelace-navbar-card) | The bar at the bottom | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=joseluis9595&repository=lovelace-navbar-card&category=plugin) |
| [Origami Header](https://github.com/hazymorning/origami_header) | The fold-out menu instead of the header | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_header&category=plugin) |
| [Origami Notifications](https://github.com/hazymorning/origami_notifications) | Notifications next to the greeting | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_notifications&category=plugin) |
| [Origami Weather](https://github.com/hazymorning/origami_weather) | The weather card | [Open in HACS](https://my.home-assistant.io/redirect/hacs_repository/?owner=hazymorning&repository=origami_weather&category=plugin) |

Two of them need one more step:

- **Bubble Card Tools** is an integration. Restart, then [add it](https://my.home-assistant.io/redirect/config_flow_start/?domain=bubble_card_tools).
- **Origami Header** loads as an extra module. Add this to `configuration.yaml` and restart. If there is a `frontend:` already, add the two lines below it.

  ```yaml
  frontend:
    extra_module_url:
      - /hacsfiles/origami_header/origami-header.js
  ```

The screenshots come from Home Assistant 2026.10 with Bubble Card 3.4.1, Bubble Card Tools 1.1.1, Paper Buttons Row 2.3.1 and Navbar Card 1.7.0.

</details>

<details>
<summary><b>4. Bubble modules</b></summary>

<br>

1. Copy the files from [`bubble_card/modules`](https://github.com/hazymorning/origami/tree/main/bubble_card/modules) to `/config/bubble_card/modules/`.
2. Reload the page. The modules show up in the Bubble Card editor under **Modules**.

They are still experimental. The [module reference](bubble_card/README.md) has the details, and a way to import them without copying files.

</details>

<details>
<summary><b>5. Helpers</b></summary>

<br>

The cards read a few template sensors, two toggles and a speaker picker. They come as one package.

1. Copy [`packages/origami.yaml`](packages/origami.yaml) to `/config/packages/`.
2. Add this to `configuration.yaml`, unless you use packages already. If there is a `homeassistant:` already, add the `packages` line below it.

   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```

3. Rename the entities in the file to yours and restart.

[Helpers](docs/helpers.md) explains each sensor.

</details>

<details>
<summary><b>6. Dashboard</b></summary>

<br>

1. Open [Dashboards](https://my.home-assistant.io/redirect/lovelace_dashboards/), **Add dashboard**, **New dashboard from scratch**.
2. Open the new dashboard. From the three dots, choose **Edit dashboard**, then **Raw configuration editor**.
3. Replace everything with [`dashboard/dashboard.yaml`](dashboard/dashboard.yaml) and save.

From now on Origami Header hides the header. Edit mode is in its menu, which **More** in the navbar opens, or add `?edit=1` to the dashboard's address.

</details>

<details>
<summary><b>7. Your entities</b></summary>

<br>

The dashboard uses example entities like `light.living_room`. Swap them for yours:

1. In the raw configuration editor, Ctrl+F (⌘F on a Mac) opens search and replace. **replace all** changes every use of an entity at once.
2. [Dashboard parts](docs/dashboard.md) lists what each part reads. In the YAML, each part starts with a comment like `# Favorites`.
3. Don't need a part? Delete it in edit mode, like any other card.

The helpers use the same example names, so rename them in `packages/origami.yaml` as well.

</details>

## Troubleshooting

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

A helper can't find its source sensor. Rename the entities in `packages/origami.yaml` to yours, then reload **Template entities** under [YAML configuration reloading](https://my.home-assistant.io/redirect/server_controls/).

</details>

<details>
<summary>The edit button is gone</summary>

<br>

Origami Header replaces the header, and edit mode is in its menu. **More** in the navbar opens it. Or add `?edit=1` to the dashboard's address.

</details>

## Reference

- [Dashboard parts](docs/dashboard.md): how each part works and which entities it reads
- [Helpers](docs/helpers.md): the template sensors behind the cards
- [Bubble modules](bubble_card/README.md): options and examples for all eleven modules
- [Patterns](docs/patterns.md): the ideas that repeat through every card
- [Theme](docs/theme.md): colour and tokens

## Status

- [x] Theme, setup, dashboard and helpers
- [x] Bubble modules, still experimental
- [ ] Pop-ups: rooms, music, TV, favorites, lights and the rest
