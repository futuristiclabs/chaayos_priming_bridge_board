# Chaayos Priming Bridge Board

Bridge board project (Atopile / `.ato`) for reading 4x non-contact liquid level sensors and exporting status over I2C to a host MCU.

## Project Goal

- Use four non-contact liquid level sensors mounted on process lines:
  - `WATER_1`
  - `WATER_2`
  - `MILK_1`
  - `MILK_2`
- Perform local signal conditioning / interface conversion on this bridge board.
- Send priming status for all four lines to the host machine over I2C.

## Sensor Interface (Per Channel)

Expected sensor-side wiring:

- `VCC`
- `GND`
- `SIG` (digital state)
- `MODE` / toggle line (if used for behavior inversion)

## Bridge Board Host Interface

Target host-side connector:

- `SCL`
- `SDA`
- `VCC`
- `GND`

Functional requirement: host side must implement a complete I2C bus (`SCL + SDA`) for status transfer.

## Priming Status Contract

Bridge board shall publish one digital priming status per channel:

- `WATER_1_PRIMED`
- `WATER_2_PRIMED`
- `MILK_1_PRIMED`
- `MILK_2_PRIMED`

Recommended host payload is a single status byte:

- bit0: `WATER_1_PRIMED`
- bit1: `WATER_2_PRIMED`
- bit2: `MILK_1_PRIMED`
- bit3: `MILK_2_PRIMED`
- bit4..bit7: reserved

## Implemented Schematic (Current)

Implemented in `main.ato` with the following architecture:

- I2C GPIO expander: `Texas Instruments TCA9534PWR` (`C783615`)
- Host connector: `JST B4B-XH-A` (4-pin)
- Sensor connectors: 4x `JST B4B-XH-A` (one per pipe)
- I2C pull-ups: 2x `4.7k` (0603)
- Decoupling capacitor: `100nF` (0603)

Host connector pin map:

- `P1 -> GND`
- `P2 -> SCL`
- `P3 -> SDA`
- `P4 -> VCC`

Sensor connector pin map (all 4 channels):

- `P1 -> VCC`
- `P2 -> GND`
- `P3 -> SIG`
- `P4 -> MODE` (currently tied to `GND`)

GPIO channel mapping:

- `P0 <- WATER_1 SIG`
- `P1 <- WATER_2 SIG`
- `P2 <- MILK_1 SIG`
- `P3 <- MILK_2 SIG`

I2C expander address straps:

- `A0 = GND`
- `A1 = GND`
- `A2 = GND`

## Local Sensor Schematic Assets

Saved in this repository:

- `docs/PRO RANGE-Y27C-Dx-NPN.pdf` (primary local datasheet, 10 pages)
- `docs/sensors/xkc_y25_connection_schematic.png` (sensor connection diagram)
- `docs/sensors/xkc_y25_pinout_board.png` (pinout/interface board reference)

Source reference page used:

- https://wiki.dfrobot.com/Non_Contact_Capacitive_Liquid_Level_Sensor_SKU_SEN0368

## Repo Structure (current)

- `main.ato` - top-level Atopile module entry
- `ato.yaml` - Atopile project/build config
- `layouts/` - PCB layout assets
- `docs/sensors/` - external sensor reference files

Created by Goutham Gandhi Nadendla <gouthamgandhi@ftlabs.io>
