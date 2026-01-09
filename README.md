
# Tesla Morning Preheat (Stop on Cable Disconnect or Timeout)

## Overview
This Home Assistant blueprint automates **Tesla climate preconditioning** on weekday mornings. It starts the HVAC at a scheduled time **only if**:
- The car is in the specified zone (default: `Home`).
- The charging cable is **Connected**.

It **snapshots the original climate state** before preheating and **restores it** when:
- The cable is unplugged before the countdown ends, **or**
- The countdown expires.

## Features
- **Scheduled start**: Runs at a chosen time on selected weekdays.
- **Zone check**: Ensures the car is at `Home` (or your chosen zone).
- **Charger check**: Requires charger state = `Connected`.
- **Climate restore**: Uses `scene.create` and `scene.turn_on` to revert to the original HVAC mode and setpoint.
- **Timeout**: Stops and restores after `stop_after_minutes`.

## Inputs
| Input | Description | Default |
|---|---|---|
| `preheat_time` | Time to start preheating | — |
| `weekdays` | Days to run | Mon–Fri |
| `tesla_location_tracker` | Device tracker for Tesla (reports zone name) | — |
| `target_zone` | Zone name to match | `Home` |
| `charger_state_sensor` | Sensor showing `Connected` when plugged | — |
| `climate_entity` | Tesla climate entity (e.g., `climate.model_3`) | — |
| `cabin_setpoint` | Target cabin temperature (°C) | 21 |
| `stop_after_minutes` | Countdown before restoring climate | 30 |

## How It Works
1. At `preheat_time`, checks:
   - Day is in `weekdays`.
   - Car is in `target_zone`.
   - Charger state = `Connected`.
2. Snapshots current climate state.
3. Turns on climate and sets `cabin_setpoint`.
4. Waits for **either**:
   - Charger state changes away from `Connected` (cable unplugged), **or**
   - Timeout expires.
5. Restores original climate state via `scene.turn_on`.

## Installation
1. In Home Assistant:
   - Go to **Settings → Blueprints → Import Blueprint**.
   - Upload the file `https://raw.githubusercontent.com/urbanbur/ha-blueprints-tesla/main/blueprints/automation/tesla_automation/tesla_morning_preheat_time_location_plugged.yaml`
2. Create an automation from the blueprint and fill in your entity IDs.

## Best Practices
- Use a Tesla integration that exposes a **`climate`** entity (e.g., Tesla Fleet, Tessie, Teslemetry).
- Confirm that your **charger state sensor** reports the string `Connected` when the cable is plugged.
- If you run multiple Teslas, create separate automations per vehicle.

## Troubleshooting
- **Automation doesn’t start**: Verify the device tracker shows `Home` (or your `target_zone`) and charger sensor is `Connected`.
- **Restore fails**: Ensure `scene.create` ran successfully; you should see a temporary `scene.tesla_preheat_before` entity.
- **Entity IDs**: Use **Developer Tools → States** to find exact entity IDs.

## License
You may use and modify this blueprint for personal and educational purposes.
