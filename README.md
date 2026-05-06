# 🏠 Home Assistant M.A.C.S. Dashboard

https://github.com/user-attachments/assets/1954b7b9-e99b-45d3-94ab-533569a17c86

*__Read this in:__ English | [Français](README.fr.md)*

A feature-rich Home Assistant dashboard designed for **tablets** in **landscape orientation only**. Built with a collection of Lovelace custom cards, it offers a polished, kiosk-ready experience.

> 🚧 **Work In Progress** — This dashboard is still under active development. Expect changes, missing documentation, and incomplete features.

> ⚠️ **This dashboard is not beginner-friendly.** You will need to adapt entity IDs, card positions, and dimensions to match your own setup and screen resolution/ratio.

---

## ✨ Features

- Landscape-only layout optimized for wall-mounted tablets
- Clean look via a custom Home Assistant theme (hides UI chrome, provides styling)
- **Two versions available:**
  - **Fully Kiosk Browser version** — uses two tabs to switch between views without reloading dashboards
  - **Standard Lovelace navigation version** — uses classic Lovelace view switching
- Motion detection history with per-camera filtering
- Fuel price display from nearby stations
- Alarm panel, media player, weather, calendar, graphs, and more

---

## 📋 Prerequisites

- [Home Assistant](https://www.home-assistant.io/) (recent version recommended)
- [HACS](https://hacs.xyz/) installed
- Basic knowledge of Lovelace YAML configuration
- *(Optional)* [Fully Kiosk Browser](https://www.fully-kiosk.com/) for the two-tab version

---

## 📦 Required Plugins

All plugins below are available via HACS unless otherwise noted.

### Standard HACS Repositories

| Plugin | Description |
|---|---|
| [alarmo-card](https://github.com/nielsfaber/alarmo-card) | Alarm panel card for Alarmo integration |
| [state-switch](https://github.com/thomasloven/lovelace-state-switch) | Show/hide cards based on entity state or hash |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | Minimalist card with pop-up support |
| [Button Card](https://github.com/custom-cards/button-card) | Highly customizable button card |
| [Calendar Card Pro](https://github.com/alexpfau/calendar-card-pro) | Advanced calendar card |
| [Clock Weather Card](https://github.com/pkissling/clock-weather-card) | Combined clock and weather forecast card |
| [Embedded View Card](https://github.com/redkanoon/embedded-view-card) | Embed a Lovelace view inside another |
| [M.A.C.S.](https://github.com/glyndavidson/MACS) | Mood-Aware Character SVG — animated companion |
| [Mini Graph Card](https://github.com/kalkih/mini-graph-card) | Compact graph card for sensor history |
| [Plotly Graph Card](https://github.com/dbuezas/lovelace-plotly-graph-card) | Powerful graph card using Plotly |
| [Prix Carburant](https://github.com/Aohzan/hass-prixcarburant) | French fuel price integration |
| [Restriction Card](https://github.com/iantrich/restriction-card) | Add restrictions/protection to any card |
| [Simple Swipe Card](https://github.com/nutteloost/simple-swipe-card) | Touch swipe between cards |
| [Spook](https://github.com/frenck/spook) | Extended service calls, used in automations |
| [Waterfall History Card](https://github.com/sxdjt/horizontal-waterfall-history-card) | Horizontal history visualization |

### Custom Repositories (add manually in HACS)

These repositories are not in the default HACS catalog and must be added as **custom repositories**:

| Plugin | Description |
|---|---|
| [AlertTicker-Card](https://github.com/djdevil/AlertTicker-Card) | Scrolling alert ticker card |
| [Camera Detection Slider](https://github.com/Arubinu/camera-detection-slider) | View the latest detections from your cameras |
| [FileTrack](https://github.com/TheScubaDiver/FileTrack) | Sensor tracking files in a folder (used for motion detection history) |
| [Hash Timer Card](https://github.com/Arubinu/hash-timer-card) | Card that acts as a smart router |
| [Mediocre Media Player Cards](https://github.com/antontanderup/mediocre-hass-media-player-cards) | Improved media player cards |

### Styling: card-mod or uix (pick one)

For card styling, you need **one** of the following. Both work — choose based on your preference:

| Plugin | Notes |
|---|---|
| [lovelace-card-mod](https://github.com/thomasloven/lovelace-card-mod) | The classic, widely used card styling plugin |
| [uix](https://github.com/Lint-Free-Technology/uix) | Uses card-mod internally; maintained as a higher-level abstraction |

---

## ⚙️ Configuration

### Fuel Prices (`configuration.yaml`)

Add the following to your `configuration.yaml`. Station IDs can be found on the [official French fuel price website](https://www.prix-carburants.gouv.fr) — open a station's detail page and take the last digits from the URL.

```yaml
sensor:
  - platform: prix_carburant
    stations:
      - 00000000  # Replace with your station ID
      - 00000000  # Replace with your station ID
```

### Motion Detection History with FileTrack (`configuration.yaml`)

Duplicate the sensor block for each camera you want to track. The `filter` field supports wildcards to match filenames.

```yaml
filetrack:
  sensors:
    - name: 'Camera 1 FileTrack'
      folder: /config/www/media/Recordings
      filter: 'Camera 1*'
      sort: date
```

### Allow External Access to Recordings (`configuration.yaml`)

Required for FileTrack to access files and for recordings to be accessible outside your local network:

```yaml
homeassistant:
  allowlist_external_dirs:
    - /config/www/media/Recordings

  media_dirs:
    recordings: /media/Recordings
```

> 📁 **Note:** The `/media/Recordings` path is exposed via a **Network Storage** share added directly from Home Assistant's interface (**Settings → System → Storage → Add network storage**). Without this step, the folder will not be visible to Home Assistant or FileTrack.

---

## 🤖 Included Automations

The following automation files are included in the repository and require specific configuration.

### `interaction.yaml` — M.A.C.S. Emotion on Motion Detection

Triggers a random emotion animation on the [M.A.C.S.](https://github.com/glyndavidson/MACS) character when motion is detected in front of the tablet. Requires:

- [Spook](https://github.com/frenck/spook) for the extended service calls used in the automation
- A Fully Kiosk Browser device declared in Home Assistant integrations
- The binary sensor `binary_sensor.kiosk` linked to that Fully Kiosk device (update the entity ID to match your own)

### `letters.yaml` — Mailbox Alert

Displays an alert when mail is detected in the mailbox. Requires creating the following helper manually in Home Assistant:

**Settings → Devices & Services → Input → Add input → Text**

```
input_text.boite_aux_lettre_alerte
```

### `home.yaml` — Auto-Return to Main View

Automatically navigates back to the main dashboard view after **1 minute of inactivity**. No additional configuration needed beyond updating the dashboard/view target to match your setup.

---

## 📐 Adapting to Your Screen

This dashboard was built for a specific screen size and ratio. You **will** need to adjust:

- **Entity IDs** — replace all entities with your own
- **Positions and dimensions** — card positions (especially for absolute-positioned layouts) depend on your screen resolution and ratio
- **Theme settings** — configure the theme to match your preferences
- **Dashboard URL slug** — the dashboard must be created with the slug `m-a-c-s` (set when creating the dashboard in Home Assistant) to avoid having to update the URL references throughout the YAML

---

## 🖥️ Fully Kiosk Browser Version

If you use [Fully Kiosk Browser](https://www.fully-kiosk.com/), the two-tab version allows switching between dashboard views **without reloading** them. This results in smoother transitions and no loading delay when switching views.

### Home Assistant Integration

Install the **Fully Kiosk Browser** integration in Home Assistant (**Settings → Devices & Services → Add integration → Fully Kiosk Browser**). This exposes your tablet as a device with motion detection sensors, screen control services, and more — required for the `interaction.yaml` automation.

### Two-Tab Setup

In the Fully Kiosk Browser settings, configure your two dashboard URLs in the **Start URL** field by entering each URL on a separate line. Fully Kiosk will load them as two independent tabs, keeping both dashboards alive in memory simultaneously.

Configure the appropriate permissions (screen always on, autostart on boot, etc.) to ensure the tablet stays live.

### JavaScript Injection — Mobile DevTools

Tablet browsers do not expose a developer panel. You can inject [Eruda](https://github.com/liriliri/eruda), a mobile-friendly developer console, directly via the **Fully Kiosk Browser administration interface** (Advanced Web Settings → Inject JavaScript). This simulates the browser developer tools and is very useful for debugging your dashboard:

```javascript
var script = document.createElement('script');
script.src = 'https://cdn.jsdelivr.net/npm/eruda';
script.onload = function() { eruda.init(); };
document.head.appendChild(script);
```

---

## 🧩 Per-User Customization with Browser Mod

[Browser Mod](https://github.com/thomasloven/hass-browser_mod) is a highly recommended companion integration that allows per-device and per-user customization directly from Home Assistant. It is installed as a **custom HACS repository**.

Among other things, it lets you:

- **Set a default dashboard** per user or per browser/device
- **Hide the sidebar** (left navigation menu) for a cleaner kiosk experience
- **Hide the Lovelace header/title bar** to maximize screen space
- **Manage user permissions** at the browser level

This is particularly useful for wall-mounted tablets where you want a full-screen, distraction-free display without having to rely solely on Fully Kiosk Browser for UI control.

---

## 🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first.

---

## 📄 License

[MIT](LICENSE)
