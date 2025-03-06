# Compile on CLI

## Step by Step

1. Install [PlatformIO Core](https://platformio.org/install/cli){target=_blank} (PIO).
2. Install the prerequisites to be able to [build the
   WebApp](compile_webapp.md#install-prerequisites).
3. Clone the [source code repository](https://github.com/hoylabs/OpenDTU-OnBattery).
   You really have to clone the source code repository. Do not download and
   extract a source ZIP file. During the build process the Git hash is embedded
   into the firmware. If you merely download and extract the source ZIP file a
   build error will occur, as no Git status can be determined.
4. Adjust the serial port in file `platformio_override.ini` to your setup. The
   setting occurs twice:
    * `upload_port`
    * `monitor_port`
5. Build the firmware: `platformio run -e generic_esp32s3` (chose a suitable PIO environment)
6. Upload to ESP32 module: `platformio run -e generic_esp32s3 -t upload`

## Other Popular PlatformIO Tasks

* Clean the sources:  `platformio run -e generic_esp32s3 -t clean`
* Erase flash: `platformio run -e generic_esp32s3 -t erase`
