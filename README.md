## **👋 Welcome to Homio**

Homio is a clean, minimal, and fully YAML-based dashboard for Home Assistant that i build for a bit of fun. It's still work in progress but wanted to share it with you after receiving many requests for it. It’s built with tablets in mind — perfect for a wall-mounted screen — but it also works well on mobile thanks to its responsive layout.

Desktop/Tablet view

<img width="1512" alt="Screenshot 2025-06-11 at 22 04 05" src="https://github.com/user-attachments/assets/0c904899-f970-4fbe-9e13-64b069ffc126" />
<img width="1511" alt="Screenshot 2025-06-11 at 22 04 19" src="https://github.com/user-attachments/assets/36b5c8b3-1943-4d3b-9e3f-a066236cfec3" />
<img width="1483" alt="Screenshot 2025-06-11 at 22 09 22" src="https://github.com/user-attachments/assets/b2780ae6-e3f0-4ac8-a7c7-5f5c82e8b3d1" />


Mobile view

<img width="573" alt="Screenshot 2025-06-11 at 22 04 36" src="https://github.com/user-attachments/assets/9d4c8201-dd9d-4912-ba34-7ccf24d0a750" />
<img width="572" alt="Screenshot 2025-06-11 at 22 08 10" src="https://github.com/user-attachments/assets/5718134e-4ca3-4247-adf0-8a170d70cc6b" />




Everything is done in YAML to give you full control and make it easier to share, reuse, and tweak. I recommend to use the visual studio code editor plugin in home assistant to make coding that little bit easier.


## **✅ What You’ll Need**

Please make sure to make a full backup of your current home assistant state, its just best practice. Before jumping in, make sure you’ve got the basics covered:

1. Home Assistant in storage Mode
Even though Homio is written entirely in YAML, you should leave the Lovelace mode set to storage in configuration.yaml. This allows you to keep using the UI editor for other dashboards, while loading Homio as a YAML dashboard.

```text
lovelace:
  mode: storage
  dashboards:
    dashboard-homio:
      mode: yaml
      title: "homio"
      icon: mdi:star-plus-outline
      show_in_sidebar: true
      filename: dashboards/homio/homio.yaml
```

3. A Few Custom Cards
Homio uses a couple of custom cards.

**button-card by Romraider** — https://github.com/custom-cards/button-card  - This is the main building block of Homio. Install it via HCS

**layout-card by Thomas Loven** — https://github.com/thomasloven/lovelace-layout-card - You’ll need to actually use the slightly modified version included in this repo based on the layout-card by Thomas Loven **https://github.com/iamtherufus/Homio/blob/main/layout-card-modified.js** which supports some extra CSS properties. Dont install this card via HACS, grab it from the repo.

This card need to be installed in this location

**/www/community/layout-card-modified/layout-card-modified.js**

**my-slider-v2 by AnthonMS** - This is a fantastic light slider created by AnthonMS which is used for the homio_light card brightness slider. Full details on his configuration can be found here.
https://github.com/AnthonMS/my-cards/blob/main/docs/cards/slider-v2.md

This card need to be installed here

**/local/community/light-slider/my-slider-v2.js**


## 📁 Folder Structure

Everything lives under `/config` in your Home Assistant setup:

```text
/config
│
├── dashboards/
│   ├── homio/
│   │   └── homio.yaml                   # Main dashboard YAML file
│   │
│   ├── templates/
│   │   ├── button_cards/
│   │   │   ├── base/                    # Base templates like homio_default
│   │   │   └── cards/                   # Entity-specific cards like homio_light, homio_thermostat
│   │   └── includes/
│   │       ├── layouts/                # Layouts like homio_navigation.yaml
│   │       └── rooms/                  # Optional: room-specific cards
│
├── packages/
│   └── homio_helpers.yaml              # All required helpers (input_booleans, numbers, etc.)
│
├── themes/
│   └── homio/
│       └── homio.yaml                  # The Homio theme
│
├── www/
│   ├── community/
│   │   ├── button-card/
│   │   │   └── button-card.js          # Required button-card
│   │   ├── layout-card-modified/
│   │   │   └── layout-card-modified.js # Slightly tweaked layout-card
│   │   └── light-slider/
│   │       └── my-slider-v2.js         # Slider for lights and climate
│   └── images/
│       └── Homio/
│           ├── icons/                  # SVG icons for climate/mode visuals
│           │   ├── heating.svg
│           │   ├── increase.svg
│           │   ├── decrease.svg
│           │   └── power_off.svg
│           └── rooms/                  # Background images for rooms
│               ├── kitchen.jpg
│               ├── living_room.jpg
│               └── bedroom.jpg
│
├── sensors.yaml                         # Any custom sensors used by Homio
├── automations.yaml
├── scripts.yaml
├── scenes.yaml
└── configuration.yaml                   # Where includes are added


```

## **🖼️ Assets Setup – Images & Icons**

To make Homio look the way it’s intended, you’ll need to add your own room images and icons to the www folder in Home Assistant. These are used for things like room backgrounds and custom icons inside button cards. I dont use the built in mdi icons as i dont like them, i do use the material icons though but download them from google at the 100 weight as i feel they fitted my design better. I will include these in the repo and i plan to keep adding to them as well.

Material design icons: 

**https://fonts.google.com/icons?icon.query=light**

📁 Folder Structure
Inside your /config/www directory, create the following folders:

```
www/
└── images/
    └── Homio/
        ├── rooms/       ← Background images for rooms
        └── icons/       ← SVG icons used in entity cards

```

🖼️ Room Backgrounds
Place your .jpg files in:

```
/config/www/images/Homio/rooms/
```

Make sure the file names match what you use in the image: variable in the homio_room template (without the file extension). 

For example:

```
image: living_room  # Will load living_room.jpg
```

🧩 Icons
Put your .svg icon files here:

```
/config/www/images/Homio/icons/
```

These are used for visual cues like heating, doors, or lights. You can reference them with:

```
entity_picture: /local/images/Homio/icons/heating.svg
```

## **Layout Cards**

Homio uses a consistent layout across all dashboards powered by custom:layout-card (modified version required – see Setup Requirements). The layout file handles page sizing, grid setup, and responsive breakpoints.

## **homio_screen_layout**
Here’s what’s inside the main screen layout file. This is used for each room dasboard.

```
layout:
  grid-template-columns: 1fr
  grid-column-gap: 0px
  margin: 0
  height: 100vh
  position: relative
```

This layout ensures a single-column responsive grid and full-height display.

**Example**
Use the layout in your dashboard YAML like this:

```
- type: custom:grid-layout
  title: lounge
  path: lounge
  layout: !include /config/dashboards/templates/includes/layouts/homio_screen_layout.yaml
  cards:
    - type: custom:button-card
      template:
        - homio_room
      name: Lounge
      variables:
        image: lounge
        temperature: sensor.lounge_temperature
        humidity: sensor.lounge_humidity
        show_temp: true
        show_humid: true
```

## **homio_entity_layout**

The homio enitiy layout is a layout include file designed to make placing entity cards in a consistent, responsive layout easy. It handles spacing, responsive column counts, and layout switching for mobile views.

You don’t need to touch this file — just include it where you want a grid of homio_entity cards (or other custom buttons) to appear. However if you want vertical scroll on mobile for your entities make sure to uncomment the media query section in the below example

```
position: absolute
grid-auto-columns: 260px
grid-auto-flow: column
grid-column-gap: 5px
margin: 0 0 0 8vw
padding: 0
inset: auto 0 85px 0
scroll-snap-type: x mandatory
overflow-y: hidden
mediaquery:
  # FOR VERTICAL MOBILE SCROLLING UNCOMMENT ALL LINES
  # "(min-width: 769px) and (max-width: 1249px)":
  #   position: relative
  #   grid-auto-flow: row
  #   grid-auto-columns: none
  #   grid-template-columns: 1fr 1fr
  #   grid-column-gap: 5px
  #   grid-row-gap: 5px
  #   margin: 599px 4vw 0 4vw
  #   inset: auto
  # "(max-width: 768px)":
  #   position: relative
  #   grid-auto-flow: row
  #   grid-auto-columns: none
  #   grid-template-columns: 1fr
  #   grid-column-gap: 0
  #   grid-row-gap: 5px
  #   margin: 599px 4vw 0 4vw
  #   inset: auto
```

**Features**
Horizontal scroll layout on large screens

Responsive grid (2 columns) on tablets

Stacked layout (1 column) on mobile

Scroll snap and animation-friendly

Fully reusable via !include

**Example**

```
- type: custom:layout-card
  layout_type: custom:grid-layout
  layout: !include /config/dashboards/templates/includes/homio_entity_layout.yaml
  cards:
    - type: custom:button-card
      template: homio_entity
      entity: binary_sensor.front_door
      name: Front Door
      variables:
        icon: door
    - type: custom:button-card
      template: homio_entity
      entity: binary_sensor.windows
      name: Windows
      variables:
        icon: window
    - type: custom:button-card
      template: homio_entity
      entity: binary_sensor.motion
      name: Motion
      variables:
        icon: motion
```

## **homio_navigation**

This file builds the Homio Navigation Bar, automatically adapting to desktop and mobile devices. It includes the logo, navigation links, and current time display — all styled to match the Homio theme. You don’t need to touch this file

**File Structure Overview**

```
type: vertical-stack
cards:
  - type: conditional
    conditions: # Desktop navigation
      - condition: screen
        media_query: "(min-width: 1250px)"
    card:
      type: custom:layout-card
      layout_type: custom:grid-layout
      layout:
        grid-template-columns: max-content 1fr max-content
        margin: 0 8vw
        position: absolute
        inset: 60px 0 auto 0
      cards:
        - type: custom:button-card
          template: homio_logo
        - type: custom:layout-card
          layout_type: custom:grid-layout
          layout:
            grid-auto-flow: column
            place-content: center
            grid-column-gap: 50px
          cards: !include /config/dashboards/templates/includes/homio_navigation_list.yaml
        - type: custom:button-card
          template: homio_time

  - type: conditional
    conditions: # Mobile drawer
      - condition: screen
        media_query: "(max-width: 1249px)"
      - condition: state
        entity: input_boolean.homio_mobile_navigation
        state: "on"
    card:
      type: custom:layout-card
      layout_type: custom:grid-layout
      layout:
        grid-template-columns: 1fr
        height: 100vh
        width: 250px
        margin: 0
        padding: 0 0 0 60px
        inset: 0 auto auto 0
        background: rgba(255,255,255,0.1)
        backdrop-filter: blur(15px)
      cards:
        - type: custom:button-card
          template: homio_logo
        - type: custom:layout-card
          layout_type: custom:grid-layout
          layout:
            grid-auto-flow: row
            grid-row-gap: 30px
          cards: !include ../includes/homio_navigation_list.yaml
        - type: custom:button-card
          template: homio_time
```

## **homio_navigation_list**

This file contains the individual navigation buttons used in the top and side navigation bars. Each button links to a different Homio room/dashboard screen.

Keep it to 8 or fewer links for the best layout on larger screens. You only need to change the label and path to what ever your dashboard for that room is called

**Example**

```
- type: custom:button-card
  template:
    - homio_nav_button
  label: room1
  variables:
    path: /dashboard-homio/room1

- type: custom:button-card
  template:
    - homio_nav_button
  label: room2
  variables:
    path: /dashboard-homio/room2

- type: custom:button-card
  template:
    - homio_nav_button
  label: room3
  variables:
    path: /dashboard-homio/room3

- type: custom:button-card
  template:
    - homio_nav_button
  label: room4
  variables:
    path: /dashboard-homio/room4

- type: custom:button-card
  template:
    - homio_nav_button
  label: room5
  variables:
    path: /dashboard-homio/room5

- type: custom:button-card
  template:
    - homio_nav_button
  label: room6
  variables:
    path: /dashboard-homio/room6

- type: custom:button-card
  template:
    - homio_nav_button
  label: room7
  variables:
    path: /dashboard-homio/room7

- type: custom:button-card
  template:
    - homio_nav_button
  label: room8
  variables:
    path: /dashboard-homio/room8

```

## **Base cards**

A few base cards that you wont need to touch but just so you know what they are doing

## **homio_default**

This is the foundational button card template used across most components. It sets consistent styles like font, layout and background effects

```
homio_default:
  styles:
    card:
      - "--mdc-ripple-color": transparent
      - "-webkit-tap-highlight-color": transparent
      - position: relative
      - border: none
      - border-radius: 0
      - padding: 0
      - margin: 0
      - animation: fadeIn 0.3s ease-in forwards
  extra_styles: |
    @keyframes fadeIn {
      from {
        opacity: 0
      }
      to {
        opacity: 1
      }
    }

```

## **homio_entity**

This template extends homio_default and is used for entity status cards such as lights, climate status, water, motion, doors, windows, etc. It’s clean, responsive, and visually consistent across different types of entities.

```
homio_entity:
  template:
    - homio_default
  show_entity_picture: true
  entity_picture: |
    [[[
      return `/local/images/Homio/icons/${variables.icon}.svg`;
    ]]]
  styles:
    card:
      - height: 85px
      - scroll-snap-align: start
    img_cell:
      - justify-self: end
      - align-self: center
    icon:
      - width: 22px
      - height: 22px
```

## **Main Cards**

These are currently the cards I have setup, there are more to come in the future.

## **homio_room**

The homio room card acts as the top visual banner for each room or area on your dashboard. It typically includes a large background image, room name, temperature/humidity readouts, and optional motion detection feedback.

This card is intended to be used once per room dashboard, placed at the top for an immersive overview.

Make sure to use the template named homio_room for the custom button card as per the example below. The variables avilable on this card are in the table below.

### `Variables`

| Variable         | Default        | Description                                                                 |
|------------------|----------------|-----------------------------------------------------------------------------|
| `image`          | —              | Name of the background image (omit `.jpg`). Looks inside `/www/images/Homio/rooms/`. |
| `image_position` | `center center`| Optional background alignment of the image.                                |
| `show_motion`    | `false`        | Set to `true` to enable the motion detection banner.                        |
| `motion_sensor`  | `""`           | Entity ID of the motion binary sensor. Required if `show_motion` is `true`.|
| `show_temp`      | `false`        | Set to `true` to display the temperature summary field.                     |
| `temp_sensor`    | `""`           | Entity ID of the temperature sensor. Required if `show_temp` is `true`.     |
| `show_humid`     | `false`        | Set to `true` to display the humidity summary field.                        |
| `humid_sensor`   | `""`           | Entity ID of the humidity sensor. Required if `show_humid` is `true`.       |


**Example**

```
- type: custom:button-card
  template: homio_room
  name: Living Room
  variables:
    image: living_room
    image_position: center center
    show_motion: true
    motion_sensor: binary_sensor.living_room_motion
    show_temp: true
    temp_sensor: sensor.living_room_temperature
    show_humid: true
    humid_sensor: sensor.living_room_humidity
```

This will display a room card titled Living Room with:

Background image: /local/images/Homio/rooms/living_room.jpg

Motion banner when motion is detected.

Temperature and humidity status shown at the bottom.

## **homio_light**

The homio_light template extends homio_entity to provide dynamic control and status display for light entities. It includes a built-in brightness percentage readout and an animated slider (using my-slider-v2) for seamless brightness adjustment.

Make sure to use the template named homio_light for the custom button card as per the example below

**Features**
Shows brightness percentage when light is on.

Tap toggles light on only (off handled via hold).

Hold toggles light off.

Animated brightness slider appears only when light is on.

Example
```
- type: custom:button-card
  template: homio_light
  entity: light.kitchen_ceiling
  name: Kitchen
  variables:
    icon: ceiling
```

This will show:

A light icon (optional custom via icon variable),

"Brightness - 78%" when light is on,

"Off" when light is off,

A horizontal slider for brightness control when light is on.

my-slider-v2 must be installed via HACS or manually, and the resource must be included in your Lovelace configuration.

## **homio_thermostat**

The homio thermostat template brings smart control to your heating setup. It combines HVAC mode switching, target temperature setting, and a clean display layout using only button-card and layout-card components.

Make sure to use the template named homio_thermostat for the custom button card as per the example below

Required Helpers & Setup

input_boolean.homio_heating_control: Controls visibility of the temp controls

input_number.homio_thermostat_target_temperature: Stores the temperature to be sent in a helper which reacts much quicker on tap and only fires one service call to update the actual entity target temperature once the set button is triggered when you have the desired temperature.

Example

```
- type: custom:button-card
  template: homio_thermostat
  entity: climate.living_room_thermostat
  name: Heating
```

## **A full working example**

The dashboard is located here /config/dashboards/homio/homio.yaml

Make sure to add the below on the first line of any dashboard you create

**button_card_templates: !include_dir_merge_named /config/dashboards/templates/button_cards**


```
button_card_templates: !include_dir_merge_named /config/dashboards/templates/button_cards
views:
  - type: custom:grid-layout
    title: lounge
    path: lounge
    layout: !include /config/dashboards/templates/includes/homio_screen_layout.yaml
    cards:
      - type: custom:button-card
        variables:
          image: lounge
          image_position: center center
          show_humid: true
          humid_sensor: sensor.living_room_sensor_humidity
          show_temp: true
          temp_sensor: sensor.living_room_sensor_temperature
          show_motion: true
          motion_sensor: binary_sensor.hue_motion_sensor_living_room
        template:
          - homio_room
        name: Lounge Room
      - type: custom:layout-card
        layout_type: custom:grid-layout
        layout: !include /config/dashboards/templates/includes/homio_entity_layout.yaml
        cards:
          - type: custom:button-card
            variables:
              icon: window
            template:
              - homio_entity
            entity: binary_sensor.living_room_windows
            name: Windows
          - type: custom:button-card
            variables:
              icon: lamp
            template:
              - homio_light
            entity: light.hue_living_room_lamp
            name: Lamp
          - type: custom:button-card
            variables:
              icon: pendent
            template:
              - homio_light
            entity: light.hue_living_room_front
            name: Front Light
          - type: custom:button-card
            variables:
              icon: pendent
            template:
              - homio_light
            entity: light.hue_living_room_back
            name: Back Light
          - type: custom:button-card
            variables:
              icon: tv
            template:
              - homio_entity
            entity: media_player.living_room_samsung_tv
            name: Samsung TV
          - type: custom:button-card
            variables:
              icon: apple_tv
            template:
              - homio_entity
            entity: media_player.apple_tv
            name: Apple TV
```
