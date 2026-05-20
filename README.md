# ESP32-2432S028 ESPHome Config

ESPHome configuration for the ESP32-2432S028 (CYD — Cheap Yellow Display, 2.8").

Displays current time and weather temperature, with two large touch buttons to control a gate or garage door via Home Assistant.

## Display Layout

```
┌──────────────────────────────────┐
│  13:45                  -2°C     │
├──────────────────────────────────┤
│                                  │
│   [ OPEN ]          [ CLOSE ]    │
│                                  │
└──────────────────────────────────┘
```

## Features

- **Time** — synced via SNTP (Europe/Warsaw timezone)
- **Temperature** — pulled from a Home Assistant weather entity
- **OPEN button** — calls `cover.open_cover` on your HA entity
- **CLOSE button** — calls `cover.close_cover` on your HA entity
- Buttons publish binary sensors to Home Assistant for automation use

## Setup

### 1. Fonts

Copy both font files into a `fonts/` folder next to this config:

- `fonts/Arimo-Regular.ttf`
- `fonts/materialdesignicons-webfont.ttf`

### 2. Secrets

Create a `secrets.yaml` file (excluded from git) with the following keys:

```yaml
api_key: <base64 32-byte key>   # generate with ESPHome or openssl
ota_password: "yourpassword"
wifi_ssid: "YourSSID"
wifi_password: "YourWiFiPassword"
ap_password: "FallbackPassword"
```

### 3. Configuration

Edit the `substitutions` block at the top of `ESP32-2432S028.yaml`:

```yaml
substitutions:
  weather_entity: weather.forecast_home   # your HA weather entity
  gate_entity: cover.gate                 # your HA cover/gate entity
```

To find your weather entity: **Home Assistant → Developer Tools → States**, search for `weather.`.

### 4. Flash

```bash
esphome run ESP32-2432S028.yaml
```

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-WROVER (esp32dev) |
| Display | ST7789V 2.8" 320×240 (ILI9XXX driver) |
| Touch | XPT2046 resistive |
| Backlight | GPIO21 (LEDC PWM) |
| RGB LED | GPIO04 (R), GPIO16 (G), GPIO17 (B) |

## Credits

- Original 12-button Stream Deck config: Aaron Stewart [@makeitworktech](https://github.com/makeitworktech)
- Button fix: Larry Goodale @TTechGuy
