# Patterns

Most of the dashboard is Paper Buttons Row, folded into cards and layouts, hence the name. The same few ideas repeat through every card.

[The row is the card](#the-row-is-the-card) · [Dumb cards, smart sensors](#dumb-cards-smart-sensors) · [No layout shifts](#no-layout-shifts) · [Whole card tap targets](#whole-card-tap-targets) · [Visibility instead of logic](#visibility-instead-of-logic) · [Sorting with order](#sorting-with-order) · [Zebra stripes](#zebra-stripes) · [Morphing with scale](#morphing-with-scale)

## The row is the card

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

## Dumb cards, smart sensors

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

## No layout shifts

This is behind most unusual choices here.

- **Fixed heights.** Rows, tiles and cards have set heights, a long text never pushes anything.
- **States swap colours, not sizes.** A tile that turns on changes its colour, never its size.
- **Helpers always return a value.** `?` instead of an empty string, and `| default(..., true)` where a value can be missing.
- **Elements that come and go** either leave no gap, like hidden tiles, or keep their box and scale, like the group badge.
- **Values in the browser.** The modules take JavaScript in `${ }`, which is there on the first frame. Jinja waits for the server.
- **Animations only on user action.** The climate table fades in when it's opened. Nothing animates on page load.
- **A local font.** A font that loads late reflows all text.
- **Grid instead of stack cards** where the dashboard needs a wrapper. A card placed directly in the section loads fastest.

## Whole card tap targets

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

## Visibility instead of logic

What only matters sometimes only exists sometimes. The favorites are sub-buttons with Bubble's visibility conditions, the extra rooms and the climate table are cards with a visibility condition on a toggle. No helper decides what shows, and a hidden tile leaves no gap.

## Sorting with order

Grid items sort with plain CSS `order`. The oven goes first with `order: -2`, the vacuum follows while it cleans:

```yaml
origami_tiles:
  order:
    cleaning: -1
```

## Zebra stripes

Striped rows without extra elements: one repeating gradient on the table's background, sized to the fixed row height. More rooms and the stripes still match, because both read the same variable.

```yaml
styles:
  grid-template-rows: repeat(6, var(--control-height-icon))
  background-image: >-
    repeating-linear-gradient(to bottom, transparent, transparent var(--control-height-icon),
    var(--fill-subtle) var(--control-height-icon), var(--fill-subtle) calc(var(--control-height-icon) * 2))
```

## Morphing with scale

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
