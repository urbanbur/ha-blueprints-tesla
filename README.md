# Home Assistant Blueprints – Tesla Automations

**Namespace:** `tesla_automation`  
**Repository:** `urbanbur/ha-blueprints-tesla`

---

## 📌 Overview

This repository hosts **Home Assistant blueprints** for automating Tesla vehicles using the **Tesla Fleet** integration.

### ✅ Included blueprint
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
