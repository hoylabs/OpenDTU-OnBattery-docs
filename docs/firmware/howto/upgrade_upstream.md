# Upgrade from OpenDTU

!!!note "Configuraton Backup"
    Download a backup of your `config.json` using the Web UI before switching
    between OpenDTU and OpenDTU-OnBattery.

## Partition Layout

When switching from OpenDTU to OpenDTU-OnBattery, performing an OTA
update will fail, as the OpenDTU-OnBattery firmware images are too large to fit
the upstream's partition layout. This project's partition layout for the ESP32
flash memory is different from the upstream project as of version 2024.08.18.
Follow the respective [upgrade guide](upgrade_8mb.md), which is also applicable
when upgrading from OpenDTU to OpenDTU-OnBattery.

Note that settings are preserved when following the upgrade guide.

In case you want to switch back to OpenDTU, you may do so by

* performing an OTA update using the respective OpenDTU (non-factory) firmware
  binary, unless you are using an OpenDTU-OnBattery variant without OTA
  support. This works as OpenDTU happily accepts to use the larger partitions.
* writing the OpenDTU factory image suitable for your board using a USB
  connection, which will reset the partition layout to the upstream version.

## Settings

In general, one can switch seamlessly from OpenDTU to OpenDTU-OnBattery, as the
parts and features inherited from the upstream project are kept compatible.
This is particularly true for the settings. Some settings are converted when
upgrading from OpenDTU.

Moving back to OpenDTU from OpenDTU-OnBattery is **not** seamless, so you
should download a backup of your `config.json` file from the Web UI **before**
upgrading to OpenDTU-OnBattery, in case you wish to return to OpenDTU.
