# Home Assistant Blueprints – Tesla Automations

**Namespace:** `tesla_automation`  
**Repository:** `urbanbur/ha-blueprints-tesla`

---

## 📌 Overview

This repository hosts **Home Assistant blueprints** for automating Tesla vehicles using the **Tesla Fleet** integration.

### ✅ Included Blueprint
**Tesla Morning Preheat (Time + Weekday, if Zone=Target & Plugged-in)**  
Automatically starts **Tesla climate preconditioning** at a scheduled time on selected weekdays **only if**:
- The Tesla’s **location (device_tracker)** equals a **configurable target zone** (default: `home`)
- The **charger is connected** (`Connected` state)

**Optional features**
- Wake the car first (if asleep)
- Run only when the **outdoor temperature is below** a set threshold
- Set a **cabin temperature setpoint**
- **Auto-stop** climate after N minutes
- Send a **notification**

---

## 🔗 Import into Home Assistant

1. Open **Settings → Automations & Scenes → Blueprints → Import Blueprint**.
2. Paste this RAW URL:

```
https://raw.githubusercontent.com/urbanbur/ha-blueprints-tesla/main/blueprints/automation/tesla_automation/tesla_morning_preheat_time_location_plugged.yaml
```

3. Click **Import** and create an automation from the blueprint.

---

## 🛠 Requirements

- **Home Assistant** (2024.12+ recommended)
- **Tesla Fleet** integration configured (OAuth + virtual key enrolled)
- Entities available in HA:
  - `device_tracker.<your_tesla>` — **state equals zone name** (e.g., `home`, `work`, `garage`)
  - `sensor.tesla_charger_state` — must read `Connected`
  - `button.auto_conditioning_start` — starts climate preconditioning

**Optional entities**
- `button.wake_up` — wakes the car if asleep
- `button.auto_conditioning_stop` — stops climate
- `climate.<your_tesla>` — climate entity to set a cabin temperature
- `sensor.<outdoor_temperature>` — any outside temp sensor in °C

> **Tip:** Check **Settings → Areas & Zones → Zones** to confirm the exact **zone names** you intend to use.

---

## ⚙️ Inputs (explained)

| Input                          | Description                                                                 |
|-------------------------------|-----------------------------------------------------------------------------|
| **Preheat time**             | Time-of-day trigger (e.g., `07:00`)                                        |
| **Days to run**              | Weekday selection                                                          |
| **Tesla device tracker**     | `device_tracker` entity for Tesla (state = zone name)                      |
| **Target zone name**         | Zone name to match (default: `home`)                                       |
| **Charger state sensor**     | Must equal `Connected`                                                     |
| **Outdoor temp sensor**      | Optional sensor for outside temperature                                    |
| **Temp threshold (°C)**      | Only run if outdoor temp is below this value                               |
| **Wake button**              | Optional button to wake the car before starting                            |
| **Perform wake first**       | Boolean: wake car before preconditioning                                   |
| **Wake wait timeout (s)**    | How long to wait after wake for car to confirm presence                    |
| **Climate start button**     | Button to start climate preconditioning                                    |
| **Climate stop button**      | Optional button to stop climate after N minutes                            |
| **Climate entity**           | Optional climate entity to set cabin temperature                           |
| **Cabin setpoint (°C)**      | Desired cabin temperature                                                  |
| **Stop after (minutes)**     | Auto-stop climate after this duration (requires stop button)               |
| **Notify service**           | Optional notification service (e.g., `notify.mobile_app_my_phone`)         |

---

## ✅ Example Use Case

- **Goal:** Warm up your Tesla at 07:00 on weekdays when it’s cold.
- **Conditions:** Car is at home, plugged in, outdoor temp < 5 °C.
- **Actions:** Wake car → start climate → set cabin to 21 °C → stop after 30 minutes → send notification.

---

## 🔍 How It Works

1. **Trigger:** Time + weekday.
2. **Conditions:**  
   - Tesla location = `target_zone` (default `home`)  
   - Charger state = `Connected`  
   - Optional: outdoor temp below threshold  
3. **Actions:**  
   - Optionally wake the car  
   - Start climate preconditioning  
   - Optionally set cabin temperature  
   - Optionally auto-stop after N minutes  
   - Optionally notify  

---

## 🧪 Troubleshooting

- **Automation didn’t run:**  
  - Confirm weekday selection includes today.  
  - Check Tesla tracker state matches `target_zone`.  
  - Ensure charger state = `Connected`.  
  - Verify outdoor temp threshold if used.

- **Wake didn’t work:**  
  - Confirm `button.wake_up` exists and works via Developer Tools → Services.  
  - Increase wake wait timeout to 90–120 s if needed.

---

## 🗂 Repository Layout

```
urbanbur/ha-blueprints-tesla
└─ blueprints/
   └─ automation/
      └─ tesla_automation/
         └─ tesla_morning_preheat_time_location_plugged.yaml
```

---

## 📜 License

MIT License – see [LICENSE](LICENSE) for details.

---

## 🔗 Quick Import Link

```
https://raw.githubusercontent.com/urbanbur/ha-blueprints-tesla/main/blueprints/automation/tesla_automation/tesla_morning_preheat_time_location_plugged.yaml
```

---

## 💡 Notes

- This blueprint leverages the **Tesla Fleet** integration’s entities (e.g., `button.auto_conditioning_start`, `button.wake_up`) and the `device_tracker` **zone** reporting.  
- For consistent results on cold mornings, consider enabling **wake** first.  
- Keep the `tesla_automation` namespace for future Tesla blueprints (arrival preheat, smart departure, charge windows, sentry mode control, etc.).
