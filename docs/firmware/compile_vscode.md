# Compile with Visual Studio Code

* Install [Visual Studio Code](https://code.visualstudio.com/download){target=_blank} (vscode).
* In vscode, install the [PlatformIO
  Extension](https://marketplace.visualstudio.com/items?itemName=platformio.platformio-ide){target=_blank}
* Install GIT and enable GIT in vscode -
  [Download](https://git-scm.com/downloads/){target=_blank} -
  [Instructions](https://www.jcchouinard.com/install-git-in-vscode/){target=_blank}
* Clone the [code repository](https://github.com/helgeerbe/OpenDTU-OnBattery).
  You really have to clone it, do not download and extract a source ZIP file.
  During the build process the GIT hash gets embedded into the firmware. If you
  download and extract the source ZIP file a build error will occur.
* In vscode, choose **File** --> **Open Folder** and select the previously
  downloaded source code (you have to select the folder which contains the
  `platformio.ini` and `platformio_override.ini` files).
* Adjust the COM port in the file `platformio_override.ini` for your
  USB-to-serial-converter. It occurs twice:
    * `upload_port`
    * `monitor_port`
* Select the arrow button in the blue bottom status bar (PlatformIO: Upload) to
  compile and upload the firmware. During the compilation, all required
  libraries are downloaded automatically.
* In Linux, if the upload fails with error messages "Could not open
  /dev/ttyUSB0, the port doesn't exist", you can check via ```ls -la
  /dev/tty*``` to which group your port belongs to, and then add your user this
  group via ```sudo adduser <yourusername> dialout``` (if you are using
  ```arch-linux``` use: ```sudo gpasswd -a <yourusername> uucp```, this method
  requires a logout/login of the affected user).
* There are two videos showing these steps:
    * [Git Clone and compilation](https://youtu.be/9cA_esv3zeA){target=_blank}
    * [Full installation and compilation](https://youtu.be/xs6TqHn7QWM){target=_blank}
