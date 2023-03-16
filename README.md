# Introduction

This is a solution for communicating with Bluetooth/WIFI components through the web interface

# Development environment

* Dev Board：blue、wifi（ESP32-C3）、board（CPU、USB、RGB、some basic meta electric unit）、data line
* MacOS：13.2 (22D49)
* [Driver for the dev board（USB to serial port）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
* [Document for thr OTA‘s dev（esp-idf-v5.0.1）](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#standard-toolchain-setup-for-linux-and-macos)
* [Package:Tool‘s chains for the OTA‘s dev（esp-idf-v5.0.1）](https://github.com/espressif/esp-idf/releases/tag/v5.0.1)
* [bluetooth scan tool（chrome://bluetooth-internals/#devices）](chrome://bluetooth-internals/#devices)

# Background of the project

![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/项目背景.png)

# Development environment deployment

1. Install CH340 development board serial port driver，download from development board manufacturer official website：[Driver for the dev board（USB to serial port）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/USB%E8%BD%AC%E4%B8%B2%E5%8F%A3CH340.png)

2. Connecting hardware（requires supportting by the data line）
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E5%BC%80%E5%8F%91%E6%9D%BF%E9%80%9A%E7%94%B5.png)

3. Install the driver and "refresh" the equipment center
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E5%88%B7%E6%96%B0%E8%AE%BE%E5%A4%87%E4%B8%AD%E5%BF%83.png)

4. Looking at the equipment center, you can see that the development board is recognized normally, so as to confirm that the drive works normally
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E6%9F%A5%E7%9C%8B%E8%AE%BE%E5%A4%87%E4%B8%AD%E5%BF%83.png)

5. Install the dependencies needed to develop OTA：cmake、ninja、dfu-util、ccache、XCode command line tool, refer to the [document](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html) provided by the chip manufacturer
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/安装依赖.png)

6. Install the IDE required for OTA development, namely [development tool chain esp-idf-v5.0.1](https://github.com/espressif/esp-idf/releases/tag/v5.0.1)，[document](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html#esp-idf) provided by the chip manufacturer.Because of some reasons, "git clone" will be incomplete, so download it directly through the previous link, and finally ESP-IDF will be downloaded to ~/esp/esp-idf
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/开发工具链esp-idf-v5.0.1.png)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E8%8E%B7%E5%8F%96ESP-IDF.png)

7. Set up the tools，[document](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html#get-started-set-up-tools) provided by the chip manufacturer，Aside from the ESP-IDF, we also need to install the tools used by ESP-IDF, such as the compiler, debugger, Python packages, etc, for projects supporting ESP32-C3：
``` shell
cd ~/esp/esp-idf
./install.sh esp32c3
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/设置工具.png)

8. Set up the environment variables，[document](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#step-4-set-up-the-environment-variables) provided by the chip manufacturer, The installed tools are not yet added to the PATH environment variable. To make the tools usable from the command line, some environment variables must be set. ESP-IDF provides another script which does that：
``` shell
. $HOME/esp/esp-idf/export.sh
```
Note the space between the leading dot and the path!
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/设置环境变量.png)

9. OTA burning test，[document](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#start-a-project) provided by the chip manufacturer, we can start with get-started/hello_world project from examples directory in ESP-IDF, copy the project get-started/hello_world to ~/esp directory：
``` shell
cd ~/esp
cp -r $IDF_PATH/examples/get-started/hello_world .
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/OTA烧录测试.png)

10. Query the interface name of the dev board，[document](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/establish-serial-connection.html) provided by the chip manufacturer, the dev board is connected to the mac, and the command line is input：
``` shell
ls /dev/cu.*
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/查询开发板名称.png)
The dev board name at this time：
/dev/cu.usbserial-1450
/dev/cu.wchusbserial1450

11. Configure OTA project, [document](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#configure-your-project) provided by the chip manufacturer, Navigate to your hello_world directory, set ESP32-C3 as the target, and run the project configuration utility menuconfig
``` shell
cd ~/esp/hello_world
idf.py set-target esp32c3
idf.py menuconfig
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/menucongfig.png)
Set Flash Size to 4MB
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/menucongfig-4MB.png)

12. Build&Compile OTA project, [document](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#build-the-project) provided by the chip manufacturer, Run command below will compile the application and all ESP-IDF components, then it will generate the bootloader, partition table, and application binaries
``` shell
idf.py build
```
OTA Compiling...
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/OTA编译中.png)
OTA Compiled!
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/OTA编译完成.png)

13. Flash OTA onto the device, [document](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#flash-onto-the-device) provided by the chip manufacturer, flash the binaries that you just built (bootloader.bin, partition-table.bin and hello_world.bin) onto your ESP32-C3 board by running:
``` shell
idf.py -p /dev/cu.usbserial1450 flash
```
Flashing...
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/OTA烧录到设备过程.png)
Flashed!
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/OTA烧录到设备完成.png)

14. Restart the dev board, Monitor the Output, [document](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#monitor-the-output) provided by the chip manufacturer, To check if “hello_world” is indeed running, type idf.py -p PORT monitor (Do not forget to replace PORT with your serial port name):
``` shell
idf.py -p /dev/cu.usbserial-1450 monitor
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/OTA监控开发板输出.png)
You will see the warn prompt here. Just set the Flash Size to 4MB according to step 11.
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/OTA监控开发板输出.png)
To exit IDF monitor use the shortcut Ctrl+].
