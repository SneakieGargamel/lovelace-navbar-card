
# Navbar Card
![navbar-card](https://github.com/user-attachments/assets/df2a9a5d-51ec-4786-8f54-36ece2aa6f9a)

Navbar Card is a custom Lovelace card that simplifies navigation within your Home Assistant dashboard. This card provides a sleek, responsive navigation bar that appears at the bottom of the screen on mobile devices and on the side for desktop users.

[![Version](https://img.shields.io/github/v/release/joseluis9595/lovelace-navbar-card)](#)
[![Last commit](https://img.shields.io/github/last-commit/joseluis9595/lovelace-navbar-card)](#)
![Downloads](https://img.shields.io/github/downloads/joseluis9595/lovelace-navbar-card/total)

## 🚀 Installation
### Via HACS (recommended)
1. Go to HACS in Home Assistant.
2. Search for "Navbar Card".
3. Click Install!

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

| Name              | Type              | Default    | Description                                                                |
| ----------------- | ----------------- | ---------- | -------------------------------------------------------------------------- |
| `routes`          | [Routes](#routes) | `Required` | Defines the array of routes to be shown in the navbar                      |
| `desktopMinWidth` | number            | `768`      | Screen size from which the navbar will be displayed as its desktop variant |

### Routes

Routes represents an array of clickable icons that redirects to a given path. Each item in the array should contain the following configuration:

| Name            	| Type            	| Default    	| Description                                                     	|
|-----------------	|-----------------	|------------	|-----------------------------------------------------------------	|
| `url`           	| string          	| `Required` 	| The path to a lovelace view                                     	|
| `icon`          	| string          	| `Required` 	| Material icon to display as this entry icon                     	|
| `icon_selected` 	| string          	| -          	| Icon to be displayed when `url` matches the current browser url 	|
| `badge`         	| [Badge](#badge) 	| -          	| Badge configuration                                             	|

### Badge

Configuration to display a small badge on any of the navbar items.

![navbar-card-badge](https://github.com/user-attachments/assets/5f548ce3-82b5-422f-a084-715bc73846b0)


| Name       	| Type        	| Default 	| Description                                                     	|
|------------	|-------------	|---------	|-----------------------------------------------------------------	|
| `template` 	| JS template 	| -       	| Boolean template indicating whether to display the badge or not 	|
| `color`    	| string      	| red     	| Background color of the badge                                   	|


---
## Example Configuration
```yaml
type: custom:navbar-card
routes:
  - url: /lovelace/home
    icon: mdi:home-outline
    icon_selected: mdi:home-assistant
  - url: /lovelace/devices
    icon: mdi:devices
  - url: /lovelace/temperature
    icon: mdi:thermometer
  - url: /lovelace/control
    icon: mdi:creation-outline
    icon_selected: mdi:creation
  - url: /lovelace/system
    icon: mdi:information-outline
    icon_selected: mdi:information
    badge:
      template: states['binary_sensor.docker_hub_update_available'].state === 'on'
      color: var(--primary-color)
```
