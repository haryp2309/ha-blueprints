# Home Assistant Blueprints

A collection of custom [Home Assistant Blueprints](https://www.home-assistant.io/docs/automation/using_blueprints/) for automations and scripts.

## Available Blueprints

### 1. Update Notifications (`automations/update_notifications.yaml`)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fharyp2309%2Fha-blueprints%2Fblob%2Fmain%2Fautomations%2Fupdate_notifications.yaml)

Automatically notifies selected mobile app devices (Android, iOS, macOS) when new updates are available for monitored `update` entities.

**Features:**
- **Multi-Device & Cross-Platform**: Select any number of Home Assistant Companion App devices (supports both Android channels/status bar icons and iOS grouping/thread-ids/images).
- **Concise Format**: Displays the entity's friendly name as the title and `installed_version → latest_version` as the message.
- **Smart Triggering**: Notifies when an update turns `on` or when an already-pending update receives an even newer version (`latest_version` bump), while ignoring `unavailable`/`unknown` transitions on restart.
- **Actionable Buttons**:
  - **Update**: Installs the update directly from the notification (with optional automatic backup if supported by the entity).
  - **Skip**: Skips the latest version.
  - **Release Notes**: Opens the changelog/release URL when provided by the update entity.
- **Live Activity / Live Update While Updating**: Uses `live_update: true`, `critical_text` (status bar chip / Dynamic Island), `persistent: true`, `alert_once: true` (silent in-place updates), and a live progress bar while installation is in progress, and automatically clears it from all devices once installed or skipped (including stale Core/OS notifications after a reboot).

---

### 2. Boosted Heating Sync (`automations/boosted_heating_sync.yaml`)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fharyp2309%2Fha-blueprints%2Fblob%2Fmain%2Fautomations%2Fboosted_heating_sync.yaml)

Synchronizes one or more target heaters with a master room thermostat by applying a proportional boosted temperature offset on top of a reference heater's `current_temperature`:

$$\text{Target} = \text{clamp}\!\left(\text{Heater}_{\text{current}} + \text{round}\!\left((\text{Master}_{\text{target}} - \text{Master}_{\text{current}}) \times \text{Multiplier}\right),\ \text{min\_temp},\ \text{max\_temp}\right)$$

**Features:**
- **Master Thermostat & Reference Heater**: Uses the temperature deficit/surplus from a master thermostat and adds the boosted offset to a selected reference heater's `current_temperature`.
- **Multi-Heater Support**: Updates any number of target `climate` entities.
- **Configurable Boost Multiplier**: Adjust the proportional gain (default `4`).
- **Min/Max Temperature Clamping**: Clamps calculated setpoints to each heater's `min_temp` and `max_temp` attributes so `climate.set_temperature` calls never fail out of bounds.
- **Loop & Traffic Protection**: Debounces changes to the reference heater's `current_temperature` and only sends `climate.set_temperature` when a heater's target temperature actually changes.

## Manual Import Instructions

1. In Home Assistant, go to **Settings** -> **Automations & Scenes** -> **Blueprints**.
2. Click **Import Blueprint** in the bottom right corner.
3. Paste the URL to the blueprint `.yaml` file from this repository and click **Preview**.
4. Click **Import Blueprint**.
