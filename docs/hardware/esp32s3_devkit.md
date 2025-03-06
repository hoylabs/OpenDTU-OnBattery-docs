# ESP32-S3 DevKit

These boards are generally a good choice to run OpenDTU-OnBattery if you aspire
to assemble your own hardware components. Have a look at the [user
guide](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32s3/esp32-s3-devkitc-1/user_guide.html).

!!!note "Flash Memory"
    The original boards by Espressif as well as the imitations should all have
    at least 8 MB of flash memory, making it possible to use OTA updates.

!!!warning "Incompatibility with ESP32-DevKitC"
    The ESP32-S3-DevKitC boards are **not** compatible to the non-S3 versions.
    They cannot be used as drop-in upgrades in your existing hardware.
    The ESP32-S3-DevKitC boards have a higher pin count and incompatible pin
    assignments.

!!!warning "Reserved Pins"
    ESP32-S3 modules with 8 MB of PSRAM use an octal SPI interface. On these
    modules, pins GPIO 35, 36, and 37, which are usually wired to the board's
    pin header, are in use for the internal communication between ESP32-S3 and
    PSRAM. Therefore, these pins are **not** available for external use, even
    though they are wired to the board's pin header.

!!!warning "Avoid UART0"
    Avoid using UART0 (GPIO43, GPIO44) to connect a peripheral, as it is
    [reserved for the bootloader](limitations.md#using-uart0).

## Overview

The following is an overview of the original Espressif ESP32-S3 DevKit (USB-C
version) board. Many boards offered on well-known larger Chinese or local web
stores are usually merely compatible imitations.

![](../assets/images/hardware/espressif_esp32_s3_devkit_overview.jpg)

## Firmware

Use variant `generic_esp32s3_usb` on these boards. Three hardware UARTs will be
available. Console messages will appear on the USB connector which is wired to
the native USB stack of the ESP32-S3.
