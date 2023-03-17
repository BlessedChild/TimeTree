# Development environment

* Dev Board：blue、wifi（ESP32-C3）、board（CPU、USB、RGB、some basic meta electric unit）、data line
* MacOS：13.2 (22D49)
* [Driver for the dev board（USB to serial port）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
* [Document for thr OTA‘s dev（esp-idf-v5.0.1）](https://docs.espressif.com/projects/esp-idf/en/v5.0.1/esp32c3/get-started/linux-macos-setup.html#standard-toolchain-setup-for-linux-and-macos)
* [Package:Tool‘s chains for the OTA‘s dev（esp-idf-v5.0.1）](https://github.com/espressif/esp-idf/releases/tag/v5.0.1)
* [bluetooth scan tool（chrome://bluetooth-internals/#devices）](chrome://bluetooth-internals/#devices)

# Background of the project

![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/项目背景.png)

# executive plan

1. Programing OTA (Bluetooth firmware)
Pull examples from the manufacturer's warehouse to get familiar with them:
https://github.com/espressif/esp-idf/tree/v5.0.1/examples
Through some inquiry and study, we can understand the design of Bluetooth - that is divided into Client and Service mode, which can be compared with CS mode of general Internet application, in other words, Server provides Service. My expectation is that the computer can connect to the development board through Bluetooth, so the development board as a Service provider runs on the Server to provide service.

Next, I will pull an example of Service from the manufacturer's warehouse:
https://github.com/espressif/esp-idf/tree/v5.0.1/examples/bluetooth/bluedroid/ble/gatt_server
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/%E7%A4%BA%E4%BE%8B.png)

2. After determining the example, create a project and enter it at the command line:
``` shell
cd ~/esp
cp -r ~/esp/esp-idf/examples/bluetooth/bluedroid/ble/gatt_server .
```
How to check the name of the development board searched in the Bluetooth center? Looking at the .c file, you can see that:
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/c%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84.png)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/c%E6%96%87%E4%BB%B6.png)
The name of the development board in the. c code is "ESP_GATTS_DEMO"

3. Check the name of the development board and connect it to the computer:
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/%E8%BF%9E%E6%8E%A5%E5%BC%80%E5%8F%91%E6%9D%BF.png)
Command line input:
``` shell
ls /dev/cu.*
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/%E6%9F%A5%E8%AF%A2%E5%BC%80%E5%8F%91%E6%9D%BF%E5%90%8D%E7%A7%B0.png)
Name of development board at this time (in the command line):
/dev/cu.usbserial-1450
/dev/cu.wchusbserial1450

4. Compile & Flash OTA, command line input:
``` shell
idf.py set-target esp32c3
idf.py -p /dev/cu.usbserial-1410 flash monitor
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/OTA%E7%BC%96%E8%AF%91%E4%B8%AD.png)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/OTA%E7%BC%96%E8%AF%91%E5%AE%8C%E6%88%90.png)

5. I found the computer Bluetooth on Mac, but I still didn't find the name of the development board. After careful thinking about the logic, I speculated that ble5.0 could not be directly recognized by Mac and iPhone, or that the Bluetooth settings of the development board needed to be adjusted (you can enter the menu settings through idf.py menuconfig, which corresponds to the sdkconfig file in the file). A temporary way is to open the chrome browser and enter chrome://Bluetooth-Internals/# devices to search.
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/bluetooth-internals.png)
Then click "start scan" on the upper right, and the development board information is found in the list (ESP_GATTS_DEMO)：
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/bluetooth-scan.png)
Follow-up: After some inquiry and study, we can basically determine the reason why Mac and iPhone can't find the development board in the OS Bluetooth center, According to the manufacturer's documentation [explanation](https://docs.espressif.com/projects/espressif-esp-faq/zh_CN/latest/software-framework/ble-bt.html#bluetooth-le)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/macos-ble.png)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/macos-ble2.png)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/macos-ble3.png)
The above is the origin of the abbreviation "BLE"

6. Test: First, Open chrome and enter about://bluetooth-internals, and then you should arrived the page below
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/clickDevices.jpg)
Secondely，you should click "devices" button left，next，click “Start Scan” button top-right
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/clickDevices_1.jpg)
Thirdly, click “Inspect” button on the right in the option named “ESP_GATTS_DEMO”
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/clickDevices_2.jpg)
Finally, we can see the mac connect to the dev board, respone informations to mac on the page and outlog in the command line interface for the dev board, Client-Mac/Chrome:
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/clickDevices_3.jpg)
Server-connected:
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/connected.png)
Server-disconnected:
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/disconnected.png)

# Conclusion & Supplement

After research, I put forward a new scheme in this paper - chrome://bluetooth-internals/#devices
This scheme has the following advantages compared with the scheme published on the Internet:
1. The steps are simpler: to debug on a PC, just install a Chrome browser and enter the address chrome://Bluetooth-internal/# devices.
2. More extensibility: debugging programs can be programming directly through Javascript.
3. In essence, using chrome is also the way to install the app. The interface provided by Chrome encapsulates the Bluetooth interface of OS. In addition, installing WeChat app on the mobile phone and using some ble debugging applets/miniprograms (WeChat search "BLE debugging") can also achieve the same effect.
