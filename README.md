<div align="center">

# 🌦️ Weather Neon Card

**A neon weather card for Home Assistant with a live WebGL sky: rain on the glass, drifting fog, frost, falling snow, lightning, and a real moon.**

[![HACS Custom][hacs-badge]][hacs-url]
[![Release][release-badge]][release-url]
[![Validate][validate-badge]][validate-url]
[![License: MIT][license-badge]][license-url]

[![Open your Home Assistant instance and open this repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=cerealkiller57540&repository=weather-neon-card&category=plugin)

<img src="https://raw.githubusercontent.com/cerealkiller57540/weather-neon-card/main/images/scenes.jpg?v=3.3.1" alt="Weather Neon Card in eight conditions: frost on a sunny freezing day, cloudy, light rain, heavy rain, snow with frost, fog, thunderstorm at night with the moon, and a clear night with an aurora" width="900">

</div>

The card reads your `weather.*` entity and turns the condition into a scene. Rain runs down the glass and blurs what is behind it, fog drifts in layers in front of the text, frost grows from the edges when it freezes, snow falls behind the glass, and lightning branches across the sky. At night the sky goes dark and the moon shows its real phase for today. Below the scene: five forecast tiles with a min/max temperature bar, wind, and optional air-quality and pollen badges.

## 🥚 Easter eggs

<img src="https://raw.githubusercontent.com/cerealkiller57540/weather-neon-card/main/images/et.gif" alt="A bicycle silhouette crossing the full moon at night, slowed down three times" width="520">

- **Full moon, clear night**: a familiar bicycle crosses the moon once when the card appears, and again every time you tap the moon. *(Slowed down ×3 above.)*
- **New moon, clear night**: an aurora lights up the sky. Only on the five or so darkest nights of the month, never otherwise.

Both follow the real moon phase. To see them without waiting, set `fx_et_toujours: true` or `fx_aurore_toujours: true` (demo modes, not meant to stay on).

## ✨ Features

- **Two cards in one install**
  - `weather-neon-card-webgl`: the WebGL sky and glass effects (recommended).
  - `weather-neon-card`: the same layout with lighter canvas and CSS effects.
- **Animated neon SVG icons**, and an accent colour that follows the condition.
- **Forecast tiles**, daily or hourly, through `weather.get_forecasts`, with a min/max bar coloured by temperature.
- **Day and night from the sun**, not from the provider's fixed time slots, optionally refined by a light sensor (`lux_entity`).
- **Weather warnings** (`alert_entity`, e.g. a Météo-France vigilance sensor) and **Atmo France** air-quality and pollen badges.
- **Glitch the cat** pops out of the forecast divider from time to time.
- **Visual editor** with grouped sections; every effect has its own slider and a "force visible" switch to tune it in any weather.
- **One WebGL context** shared by every layer and every instance, kept alive when you change views: it stays well under the 8-context limit of Android WebViews. Pauses when off-screen, falls back to canvas rendering without WebGL, and scales the whole block down on narrow columns instead of squeezing it.

## 📦 Installation

### HACS (recommended)

1. Click the **Open in HACS** button above, or add this repository as a custom repository in HACS (category **Dashboard**): `https://github.com/cerealkiller57540/weather-neon-card`.
2. Download **Weather Neon Card**.
3. Reload your browser.

HACS registers one resource, `weather-neon-card.js`. It loads the WebGL variant on its own, so **do not** add `weather-neon-card-webgl.js` as a second resource.

### Manual

1. Copy both files from [`dist/`](dist) to `config/www/weather-neon-card/`.
2. Add a dashboard resource: URL `/local/weather-neon-card/weather-neon-card.js`, type **JavaScript module**.

## 🚀 Usage

```yaml
type: custom:weather-neon-card-webgl
entity: weather.home
forecast_type: daily
forecast_count: 5
```

With every optional sensor:

```yaml
type: custom:weather-neon-card-webgl
entity: weather.home
name: Home
alert_entity: sensor.<dept>_weather_alert
lux_entity: sensor.outdoor_illuminance
air_entity: sensor.atmo_france_qualite_globale_<zone>
pollen_entity: sensor.atmo_france_qualite_globale_pollen_<zone>
```

## ⚙️ Options

**Card**

| Option | Default | Description |
|---|---|---|
| `entity` | **required** | A `weather.*` entity |
| `name` / `show_name` | cleaned `friendly_name` / `true` | Place label under the temperature |
| `forecast_type` | `daily` | `daily`, `hourly` or `twice_daily` (if your integration supports it) |
| `forecast_count` | `5` | Number of forecast tiles |
| `alert_entity` | — | Weather warning sensor (Météo-France vigilance) |
| `sun_entity` | `sun.sun` | Sunrise and sunset, and the day/night fallback |
| `night_from_sun` | `true` | Decide day/night from the sun; `false` keeps the raw condition |
| `lux_entity` | — | Light sensor, checked first for day/night |
| `wind_entity`, `rain_chance_entity`, `snow_chance_entity` | auto | Detected from the weather entity name when left empty |
| `show_humidity` / `show_wind` / `show_pressure` | `true` | Detail badges |
| `show_aside` | `true` | Right column: sunrise, sunset, gusts |
| `show_atmo` | `true` | Atmo France badges |
| `air_entity` (+ `air_entity_next`), `pollen_entity` (+ `pollen_entity_next`) | — | Air quality and pollen, today and tomorrow |
| `reactive_bg` | `false` | Gradient background from the weather instead of the theme |
| `mood_accent` | `true` | Accent colour follows the condition |
| `glitch` | `true` | Glitch the cat |
| `particles` | `true` | All atmospheric effects |
| `neon_fx` | `true` | Scanlines and glitched temperature |
| `frost` / `frost_below` | `true` / `3` | Frost below this temperature (°C) |
| `orbitron` | `false` | Orbitron font for temperature and days |

**WebGL card only**

| Option | Default | Description |
|---|---|---|
| `sky` / `fx_gl` | `true` | The sky layer / the glass post-process |
| `sky_opacite` | `0.55` | Master volume of clouds, haze and glow |
| `fx_pluie`, `fx_brouillard`, `fx_givre`, `fx_neige`, `fx_chaleur` | `0.70`, `0.80`, `1.00`, `0.75`, `0.95` | Rain, fog, frost, snow and heat-haze strength |
| `fx_pluie_toujours`, `fx_brouillard_toujours`, `fx_givre_toujours`, `fx_neige_toujours`, `fx_vent_toujours`, `fx_chaleur_toujours` | `false` | Force one effect on, whatever the weather (for tuning) |
| `fx_lune` / `fx_lune_taille` | `true` / `96` | Photographic moon with its real phase, size in px |
| `fx_aurore` / `fx_aurore_toujours` | `true` / `false` | Aurora easter egg / demo mode |
| `fx_et` / `fx_et_toujours` | `true` / `false` | Full-moon easter egg / demo mode |
| `largeur_ref` | `380` | Width the card is drawn at; narrower columns scale the whole block |

More than a hundred other `sky_*`, `fx_*` and `fogx_*` settings (cloud scale, drop size, frost branches, snow depth, moon relief…) are easiest to tune from the visual editor, where each is a slider with its range.

## ❓ FAQ

**Which weather integrations work?** Any `weather.*` entity. The card was built with Météo-France, whose extra sensors (rain chance, warnings) it picks up automatically. Other integrations simply show fewer badges.

**Why is it night on the card when my provider says "sunny"?** Some providers switch to night on fixed time slots. The card follows the sun (and your light sensor, if set). Set `night_from_sun: false` to keep the provider's condition.

**Some cards go blank on my Android phone.** Android WebViews keep at most 8 WebGL contexts per page and drop the oldest one. This card uses a single context for all its layers and all its instances, and keeps it across view changes. If you run many other WebGL cards on one view, use `weather-neon-card` on some of them.

**Which languages are supported?** English and French (conditions, days, alerts and the editor). Names that come from your sensors are shown as they are. The editor and the card texts follow your Home Assistant language: French if it is French, English otherwise. Reload the page after changing the language. Every option can also be set in YAML.

**Which theme is in the screenshots?** Neo Tokyo, the author's own dark theme (not published). The card works with any theme. The weather data in the screenshots is made up.

## 🌃 More neon cards

This card is part of a family. See the full collection at [**Home-Assistant-Neon-Cards**](https://github.com/cerealkiller57540/Home-Assistant-Neon-Cards).

---

## 🐾 Support this project

If you enjoy these cards, please consider donating to **Quatre Pattes**, an animal rescue organization.

[![Sauver des animaux](https://img.shields.io/badge/🐾%20Sauver%20des%20animaux-Faire%20un%20don-ff69b4?style=for-the-badge)](https://don.quatre-pattes.org/s/?_jtsuid=70083177244599792679303)

> 💛 No need to support me — just help the animals. Thank you!

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-card`
3. Commit and push
4. Open a Pull Request

---

## 📄 License

[MIT License][license-url]

[hacs-badge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[hacs-url]: https://hacs.xyz
[release-badge]: https://img.shields.io/github/v/release/cerealkiller57540/weather-neon-card?style=for-the-badge
[release-url]: https://github.com/cerealkiller57540/weather-neon-card/releases
[validate-badge]: https://img.shields.io/github/actions/workflow/status/cerealkiller57540/weather-neon-card/validate.yml?branch=main&label=HACS&style=for-the-badge
[validate-url]: https://github.com/cerealkiller57540/weather-neon-card/actions/workflows/validate.yml
[license-badge]: https://img.shields.io/github/license/cerealkiller57540/weather-neon-card?style=for-the-badge
[license-url]: LICENSE
