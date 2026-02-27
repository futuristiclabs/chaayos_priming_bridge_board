# AGENTS.md

This file defines how coding agents should work in this repository.

## Project Context

- Project type: Atopile (`.ato`) hardware design project.
- Objective: build a bridge PCB for 4x non-contact liquid level sensors.
- Sensor assignment: `WATER_1`, `WATER_2`, `MILK_1`, `MILK_2`.
- Bridge output: functional I2C interface to upstream host machine.
- Working repo root: `chaayos_priming_bridge_board/`.

## Engineering Intent

- Capture each sensor digital status reliably.
- Aggregate/report status over I2C.
- Keep sensor-side and host-side connectors clearly separated in net naming.

## Interface Requirements

### Sensor side (4 channels)

Per sensor, assume:

- `VCC`
- `GND`
- `SIG`
- `MODE` (optional, depending on operating mode)

### Host side

I2C output to main controller:

- `SCL`
- `SDA` (mandatory)
- `VCC`
- `GND`

Do not ship a host connector definition that omits `SDA`.

## Reporting Contract

Expose 4 priming bits to the host over I2C:

- bit0: `WATER_1_PRIMED`
- bit1: `WATER_2_PRIMED`
- bit2: `MILK_1_PRIMED`
- bit3: `MILK_2_PRIMED`

## Files Agents Should Touch

- Primary design: `main.ato`
- Build config: `ato.yaml`
- Documentation: `README.md`
- Sensor references: `docs/sensors/`

## Reference Assets

Local sensor reference diagrams:

- `docs/PRO RANGE-Y27C-Dx-NPN.pdf` (primary datasheet)
- `docs/sensors/xkc_y25_connection_schematic.png`
- `docs/sensors/xkc_y25_pinout_board.png`

Upstream source reference:

- https://wiki.dfrobot.com/Non_Contact_Capacitive_Liquid_Level_Sensor_SKU_SEN0368

## Working Rules

- Prefer minimal, traceable changes.
- Do not rename or delete existing project files unless required.
- Keep all new hardware assumptions explicit in docs.
- Validate `main.ato` syntax after edits.
