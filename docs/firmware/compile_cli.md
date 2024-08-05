# Compile on the command line with PlatformIO Core

* Install [PlatformIO Core](https://platformio.org/install/cli){target=_blank}
* Clone this repository (you really have to clone it, don't just download the ZIP file. During the build process the git hash gets embedded into the firmware. If you download the ZIP file a build error will occur)
* Adjust the COM port in the file "platformio_override.ini". It occurs twice:
    * upload_port
    * monitor_port
* build: `platformio run -e generic_esp32s3`
* upload to esp module: `platformio run -e generic_esp32s3 -t upload`
* other options:
    * clean the sources:  `platformio run -e generic_esp32s3 -t clean`
    * erase flash: `platformio run -e generic_esp32s3 -t erase`