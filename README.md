[![Version](https://img.shields.io/github/v/release/joseluis9595/lovelace-navbar-card)](#)
[![Last commit](https://img.shields.io/github/last-commit/joseluis9595/lovelace-navbar-card)](#)
![Downloads](https://img.shields.io/github/downloads/joseluis9595/lovelace-navbar-card/total)

# Navbar Card
![navbar-card](https://github.com/user-attachments/assets/df2a9a5d-51ec-4786-8f54-36ece2aa6f9a)

Navbar Card is a custom Lovelace card designed to simplify navigation within your Home Assistant dashboard, heavily inspired by the great work of [Adaptive Mushroom](https://community.home-assistant.io/t/adaptive-mushroom/640308). It provides a sleek, responsive navigation bar that displays as a full-width bar at the bottom on mobile devices. On desktop, it adapts into a flexible container that can be positioned on any side of the screen (top, bottom, left, or right) adjusting its orientation to fit seamlessly.

## 🚀 Installation
### Via HACS (recommended)
1. Go to HACS in Home Assistant.
2. On the top right, click "Custom repositories".
3. Enter the repository URL: https://github.com/joseluis9595/lovelace-navbar-card.git
4. Search for "Navbar Card".
5. Click Install!

### Manual
1. Download [navbar-card.js](https://github.com/joseluis9595/lovelace-navbar-card/releases/latest/download/navbar-card.js) from the latest release.
2. Move this file to home assistant's `<config>/www` folder.
3. In home assistant, go to `Settings > Dashboards`.
4. On the top right corner, click `Resources`.
5. Click `Add resource`.
6. Fill URL with: `/local/navbar-card.js`.
7. Select `JavaScript module` and `Create`.
8. Go to your dashboard, refresh your page and add your new navbar-card!


---
## ⚙️ Configuration

### Main options

| Name      | Type                | Default    | Description                                           |
|-----------|---------------------|------------|-------------------------------------------------------|
| `routes`  | [Routes](#routes)   | `Required` | Defines the array of routes to be shown in the navbar |
| `desktop` | [Desktop](#desktop) | -          | Configuration options specific to desktop mode        |
| `mobile`  | [Mobile](#mobile)   | -          | Configuration options specific to mobile mode         |


### Routes

Routes represents an array of clickable icons that redirects to a given path. Each item in the array should contain the following configuration:

| Name            	| Type            	| Default    	| Description                                                     	|
|-----------------	|-----------------	|------------	|-----------------------------------------------------------------	|
| `url`           	| string          	| `Required` 	| The path to a lovelace view                                     	|
| `icon`          	| string          	| `Required` 	| Material icon to display as this entry icon                     	|
| `icon_selected` 	| string          	| -          	| Icon to be displayed when `url` matches the current browser url 	|
| `badge`         	| [Badge](#badge) 	| -          	| Badge configuration                                             	|
| `label`         	| string           	| -          	| Label to be displayed under the given route if `show_labels` is true                                    |

#### Badge

Configuration to display a small badge on any of the navbar items.

![navbar-card-badge](https://github.com/user-attachments/assets/5f548ce3-82b5-422f-a084-715bc73846b0)


| Name       	| Type        	| Default 	| Description                                                     	|
|------------	|-------------	|---------	|-----------------------------------------------------------------	|
| `template` 	| JS template 	| -       	| Boolean template indicating whether to display the badge or not 	|
| `color`    	| string      	| red     	| Background color of the badge                                   	|


### Desktop

Specific configuration for desktop mode.

| Name          | Type                                   | Default  | Description                                                                |
|---------------|----------------------------------------|----------|----------------------------------------------------------------------------|
| `show_labels` | boolean                                | `false`  | Whether or not to display labels under each route                          |
| `min_width`   | number                                 | `768`    | Screen size from which the navbar will be displayed as its desktop variant |
| `position`    | `top` \| `bottom` \| `left` \| `right` | `bottom` | Position of the navbar on desktop devices                                  |


### Mobile

Specific configuration for mobile mode.

| Name          | Type    | Default | Description                                       |
|---------------|---------|---------|---------------------------------------------------|
| `show_labels` | boolean | `false` | Whether or not to display labels under each route |

---
## Example Configuration
Basic example:
```yaml
type: custom:navbar-card
desktop:
  position: left
  min_width: 768
mobile:
  show_labels: true
routes:
  - url: /lovelace/home
    label: Home
    icon: mdi:home-outline
    icon_selected: mdi:home-assistant
  - url: /lovelace/devices
    label: Devices
    icon: mdi:devices
  - url: /lovelace/weather
    label: Weather
    icon: mdi:thermometer
  - url: /lovelace/control
    label: Control
    icon: mdi:creation-outline
    icon_selected: mdi:creation
  - url: /lovelace/system
    label: System
    icon: mdi:information-outline
    icon_selected: mdi:information
    badge:
      template: states['binary_sensor.docker_hub_update_available'].state === 'on'
      color: var(--primary-color)
```

---
## 🖌️ Styling (Optional)

### Padding 

If you’re using the Navbar Card, you might notice it could collide with other cards on your dashboard. A simple way to fix this is by adding some padding to your Home Assistant views. The easiest way to do that is by using [card-mod](https://github.com/thomasloven/lovelace-card-mod) with a [custom theme](https://www.home-assistant.io/integrations/frontend/#themes).

Here’s an example of how you can tweak your theme to adjust the layout for both desktop and mobile:


```yaml
your_theme:
  card-mod-root-yaml: |
    .: |
      /* Add padding to the left (or other sides, depending on your navbar position) for desktop screens */
      @media (min-width: 768px) {
        hui-sections-view {
          padding-left: 100px !important;
        }
      }

      /* Add bottom padding for mobile screens to prevent cards from overlapping with the navbar *
      @media (max-width: 767px) {
        hui-sections-view:after {
          content: "";
          display: block;
          height: 80px;
          width: 100%;
          background-color: transparent; 
        }
      }
```

### Hiding native tabs

Another useful styling detail, is removing the native ha-tabs element on the top of the screen. We want to hide the ha-tabs element, but keep the edit, search and assist buttons visible.
To do so, once again, using card-mod and custom themes is quite easy:

```yaml
your_theme:
  app-header-background-color: transparent
  app-header-text-color: var(--primary-text-color)

  card-mod-root-yaml: |
    .: |
      ha-tabs {
        pointer-events: none;
        opacity: 0;
      }
```
