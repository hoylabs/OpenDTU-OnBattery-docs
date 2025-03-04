# Olimex ESP32-POE-ISO

!!!note "Flash Memory"
    Get a version with at least 8 MB of flash memory to be able to use OTA updates.

## Technical details

| Property     | Value                                                                                           |
|--------------|-------------------------------------------------------------------------------------------------|
| Name         | Olimex ESP32-POE-ISO                                                                            |
| Product Info | [Link](https://www.olimex.com/Products/IoT/ESP32/ESP32-POE/open-source-hardware){target=_blank} |

## Description

To use this board please refer to the [Device Profile](../firmware/device_profiles.md) called `olimex_esp32_poe.json`. Depending on the features (LEDs) or display type you need you can easily adjust the existing config. Open the JSON file with a text editor and have a look at the specific pin numbers.

## Schematic

!!!info "Example"
    This is merely an example. You may need a different [RF
    module](inverter_overview.md) to support your inverter. The display is
    optional. You may wire the components to other GPIOs, if you
    upload a matching [Device Profile](../firmware/device_profiles.md).

![Schematic](../assets/images/hardware/Wiring_OlimexPoeIso_NRF24_Schematic.png)

## Symbolic View

!!!info "Example"
    This is merely an example. You may need a different [RF
    module](inverter_overview.md) to support your inverter. The display is
    optional. You may wire the components to other GPIOs, if you
    upload a matching [Device Profile](../firmware/device_profiles.md).

![Symbolic](../assets/images/hardware/Wiring_OlimexPoeIso_NRF24_Symbolic.png)
