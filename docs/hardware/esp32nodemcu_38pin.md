# ESP32 NodeMCU 38 pin

!!!note "Flash Memory"
    Get a version with at least 8 MB of flash memory to be able to use OTA updates.

!!!warning "UART0"
    Do **NOT** connect a peripheral to UART0 (GPIO1, GPIO3). [UART0 is
    used](limitations.md#using-uart0) by the ESP32's bootloader and by the
    firmware to print messages.

!!!warning "Input-Only Pins"
    Pins GPIO 34, 35, 36, and 39 are input-only pins and cannot be used as
    outputs. If you are not sure what that means, do not use these pins at all.

To use this board please refer to the [Device
Profile](../firmware/device_profiles.md) called `nodemcu_esp32.json` or
`blinkyparts_esp32.json`. Both are nicely compatible with the dev board. Open
the JSON file with a text editor and have a look at the specific pin numbers.

## Schematic

!!!info "Example"
    This is merely an example. You may need a different [RF
    module](inverter_overview.md) to support your inverter. The display and
    LEDs are optional. You may wire the components to other GPIOs, if you
    upload a matching [Device Profile](../firmware/device_profiles.md).

![Schematic](../assets/images/hardware/Wiring_ESP32NodeMCU_38pin_NRF24_Schematic.png)

## Symbolic View

!!!info "Example"
    This is merely an example. You may need a different [RF
    module](inverter_overview.md) to support your inverter. The display and
    LEDs are optional. You may wire the components to other GPIOs, if you
    upload a matching [Device Profile](../firmware/device_profiles.md).

![Symbolic](../assets/images/hardware/Wiring_ESP32NodeMCU_38pin_NRF24_Symbolic.png)
