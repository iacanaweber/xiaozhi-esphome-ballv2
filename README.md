# xiaozhi-esphome-ballv2

ESPHome configuration for the **Spotpear Ball V2** — a round 240×240 display powered by an ESP32-S3, showing time, weather, and temperature data from Home Assistant.

## What it does

- Displays the current time (24h) synced from Home Assistant
- Shows a weather-condition background image (15 conditions supported)
- Shows MAX / CUR / MIN temperatures from Home Assistant sensors
- Refreshes every minute, and also on any sensor/weather update

![sunny](img/sunny.png)

## Hardware

| Component | Details |
|-----------|---------|
| MCU | ESP32-S3 (16 MB flash, Octal PSRAM 80 MHz) |
| Display | GC9A01A — 240×240 round LCD |
| Interface | SPI (40 MHz data rate) |

### Pin mapping (Ball V2)

| Signal | GPIO |
|--------|------|
| SPI CLK | 4 |
| SPI MOSI | 2 |
| LCD CS | 5 |
| LCD DC | 47 |
| LCD RESET | 38 |
| Backlight | 42 |

## Requirements

- [ESPHome](https://esphome.io/) (with Home Assistant integration)
- Home Assistant with the following entities configured:

| Entity | Purpose |
|--------|---------|
| `sensor.sensor_cobertura_temperatura` | Current temperature |
| `input_number.cobertura_temp_min_hoje` | Daily minimum temperature |
| `input_number.cobertura_temp_max_hoje` | Daily maximum temperature |
| `weather.forecast_casa` | Weather condition |

> You can change all entity IDs in the `substitutions` block at the top of `config_ball_v2.yaml`.

## Getting started

1. Clone this repository.
2. Create a `secrets.yaml` next to the config file with your Wi-Fi credentials:
   ```yaml
   wifi_ssid: "YourSSID"
   wifi_password: "YourPassword"
   ```
3. Flash the device:
   ```bash
   esphome run config_ball_v2.yaml
   ```
4. Add the device to Home Assistant via the ESPHome integration.

## Supported weather conditions

The background image changes automatically based on the Home Assistant weather state:

`clear-night` · `cloudy` · `exceptional` · `fog` · `hail` · `lightning` · `lightning-rainy` · `partlycloudy` · `pouring` · `rainy` · `snowy` · `snowy-rainy` · `sunny` · `windy` · `windy-variant`

## Customization

All key parameters are defined in the `substitutions` block:

```yaml
substitutions:
  name: esphome-web-deba28
  friendly_name: "Spotpear Clock"

  # Pins
  backlight_output_pin: "42"
  # ...

  # Home Assistant entities
  temp_atual_entity: "sensor.sensor_cobertura_temperatura"
  temp_min_entity:   "input_number.cobertura_temp_min_hoje"
  temp_max_entity:   "input_number.cobertura_temp_max_hoje"
  weather_entity:    "weather.forecast_casa"
```

## Fallback access point

If Wi-Fi is unavailable the device broadcasts a hotspot named **Ball v2 Hotspot** for configuration via the captive portal.
