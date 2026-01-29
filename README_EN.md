# Time Pattern Trigger Scene/Script Blueprint

Home Assistant automation blueprints that automatically execute Scenes and Scripts at configured time intervals (seconds, minutes, hours).

## Features

- **Multiple Trigger Frequencies**: Supports seconds, minutes, and hours time patterns
- **Mixed Entity Selection**: Single selector supports both Scenes and Scripts
- **Global Conditions**: Set additional trigger conditions
- **Time Restrictions**: Limit execution to specific time periods and weekdays
- **Execution Notifications**: Send persistent notifications and notify.notify after completion

## Versions

### Simple Version (`time_pattern_trigger_things_simple.yaml`)

Supports only seconds interval triggers, suitable for frequent execution scenarios.

| Setting | Description | Default |
|---------|-------------|---------|
| Interval Seconds | time_pattern format, e.g., `/15` = every 15 seconds | `/15` |

### Combined Version (`time_pattern_trigger_things_complex.yaml`)

Supports seconds, minutes, and hours triggers simultaneously. Executes when any condition is met.

| Setting | Description | Default |
|---------|-------------|---------|
| Interval Seconds | time_pattern format, e.g., `/10` = every 10 seconds | `/15` |
| Interval Minutes | time_pattern format, e.g., `/5` = every 5 minutes | `*` |
| Interval Hours | time_pattern format, e.g., `/2` = every 2 hours | `*` |

> **Note**: Use `*` to disable that frequency.

## time_pattern Format Reference

| Format | Description | Example |
|--------|-------------|---------|
| `/N` | Trigger every N units | `/15` = every 15 seconds/minutes/hours |
| `*` | Disable this trigger | No trigger |
| `N` | Trigger at the Nth unit | `30` = at 30 seconds/minutes |

## Requirements

- Home Assistant 2024.1 or newer
- Pre-created Scenes or Scripts to execute

## Installation

### Method 1: One-Click Import (Recommended)

Click the buttons below to import blueprints directly:

**Simple Version (Seconds Trigger):**

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FWOOWTECH%2FHA_blueprint_light_loop%2Fblob%2Fmain%2Ftime_pattern_trigger_things_simple.yaml)

**Combined Version (Seconds/Minutes/Hours):**

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FWOOWTECH%2FHA_blueprint_light_loop%2Fblob%2Fmain%2Ftime_pattern_trigger_things_complex.yaml)

### Method 2: Manual URL Import

1. Go to **Settings** > **Automations & Scenes** > **Blueprints**
2. Click **Import Blueprint** in the bottom right corner
3. Paste the following URL:

**Simple Version:**
```
https://github.com/WOOWTECH/HA_blueprint_light_loop/blob/main/time_pattern_trigger_things_simple.yaml
```

**Combined Version:**
```
https://github.com/WOOWTECH/HA_blueprint_light_loop/blob/main/time_pattern_trigger_things_complex.yaml
```

### Method 3: Manual File Copy

1. Copy the blueprint files to Home Assistant's `config/blueprints/automation/` directory
2. Reload blueprints or restart Home Assistant

```bash
# Create directory
mkdir -p /config/blueprints/automation/woowtech/

# Copy files
cp time_pattern_trigger_things_simple.yaml /config/blueprints/automation/woowtech/
cp time_pattern_trigger_things_complex.yaml /config/blueprints/automation/woowtech/
```

## Configuration

### Frequency Settings

Configure the trigger time intervals using time_pattern format.

### Target Entities

Select the Scenes and/or Scripts to execute. Multiple selection is supported, and both types can be mixed.

### Global Conditions (Optional)

Set additional trigger conditions, for example:
- Execute only when a sensor is in a specific state
- Execute only when a switch is on

### Time/Weekday Conditions (Optional)

Limit automation to specific time periods and weekdays:
- **Start Time**: Default 00:00:00
- **End Time**: Default 23:59:59
- **Active Weekdays**: Default all selected

### Notification Settings (Optional)

Whether to send notifications after execution:
- **Send Notification**: Yes/No
- **Title**: Leave empty for auto-generation
- **Message**: Supports Jinja templates, leave empty for auto-generation

## Usage Examples

### Example 1: Execute Scene Every 30 Seconds

Using Simple Version:
- Interval Seconds: `/30`
- Target Entities: Select the scene to execute

### Example 2: Execute Script Every 5 Minutes

Using Combined Version:
- Interval Seconds: `*` (disabled)
- Interval Minutes: `/5`
- Interval Hours: `*` (disabled)
- Target Entities: Select the script to execute

### Example 3: Execute Every Hour on the Hour

Using Combined Version:
- Interval Seconds: `*` (disabled)
- Interval Minutes: `*` (disabled)
- Interval Hours: `/1`
- Target Entities: Select scenes and scripts to execute

## FAQ

### Q: Why isn't my automation triggering?

1. Check if time conditions match the current time
2. Check if weekday conditions include today
3. Check if global conditions pass
4. Verify time_pattern format is correct

### Q: Can I execute multiple Scenes and Scripts simultaneously?

Yes. Target entities support multiple selection and will execute all selected entities in sequence.

### Q: Will multiple triggers in the Combined Version execute repeatedly?

Yes. If both seconds and minutes triggers are set, when both conditions are met (e.g., on the minute), it may trigger twice in quick succession. It's recommended to enable only one frequency based on your needs.

### Q: How do I disable a trigger frequency?

Set that frequency to `*` to disable it.

## Technical Details

- Uses `homeassistant.turn_on` service to uniformly execute Scenes and Scripts
- Supports `condition` selector for global conditions
- Automation mode: Simple version uses `single`, Combined version uses `restart`

## Author

WOOW TECH CO., LTD.

## License

MIT License
