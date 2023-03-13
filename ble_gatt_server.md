# 开发环境

* 开发板：blue、wifi模块（乐鑫ESP-C3）、开发版（CPU、USB、RGB、一些最基础的元件）、可靠的可传输数据线。
* MacOS：13.2 (22D49)
* [开发板驱动（USB转串口）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
* [开发工具链esp-idf-v5.0.1](https://github.com/espressif/esp-idf/releases/tag/v5.0.1)
* [蓝牙扫描工具chrome://bluetooth-internals/#devices](chrome://bluetooth-internals/#devices)

# 项目背景

![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/项目背景.png)

# 实施

1. 编写OTA(蓝牙固件)
从厂家仓库中拉取示例来熟悉一下：
https://github.com/espressif/esp-idf/tree/v5.0.1/examples
这里通过一些查询和学习，可以这么理解蓝牙的设计，蓝牙分为主（Client）从（Service）模式，可以跟类比于一般互联网应用的CS模式，即Server提供Service服务。
我的预期是让电脑可以通过蓝牙连接开发版，那么开发板作为提供服务的一方，上面跑的即是Server提供服务（Service）。

接下来，我将在厂家仓库中拉取Service的示例：
https://github.com/espressif/esp-idf/tree/v5.0.1/examples/bluetooth/bluedroid/ble/gatt_server
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ble_gatt_server/%E7%A4%BA%E4%BE%8B.png)

2. 确定好示例后，来创建工程，在命令行中输入：
``` shell
cd ~/esp
cp -r ~/esp/esp-idf/examples/bluetooth/bluedroid/ble/gatt_server .
```
如何查看开发板在蓝牙中心被搜索到的名字？查看.c文件，可以看到
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/c文件路径.png)
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/c文件.png)

3. 查询开发板名称，把开发板接到电脑上
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/连接开发板.png)
命令行输入：
``` shell
ls /dev/cu.*
```
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/查询开发板名称.png)
此时的开发板名称（命令行中）：
/dev/cu.usbserial-1450
/dev/cu.wchusbserial1450

4. 编译&烧录OTA，命令行输入：
``` shell
idf.py set-target esp32c3
idf.py -p /dev/cu.usbserial-1410 flash monitor
```
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/OTA编译中.png)
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/OTA编译完成.png)

5. 发现Mac上电脑蓝牙还是没有发现开发板的名字，仔细思考了一下逻辑，推测可能是ble5.0低功耗不能被Mac、iPhone直接识别或者是开发板的蓝牙设置有一些需要调整（可以通过idf.py menuconfig进入菜单设置，其对应文件中的sdkconfig文件），临时的一个办法可以打开chrome浏览器，输入chrome://bluetooth-internals/#devices进行搜索
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/bluetooth-internals.png)
然后点击右上方“start scan”，在列表中找到了开发板信息了（ESP_GATTS_DEMO）
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/bluetooth-scan.png)
后续：经过一番查询和学习，基本可以确定Mac和iPhone无法在OS蓝牙中心发现开发板的原因了，根据厂家文档的[解释](https://docs.espressif.com/projects/espressif-esp-faq/zh_CN/latest/software-framework/ble-bt.html#bluetooth-le)
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/macos-ble.png)
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/macos-ble2.png)
![avatar](https://github.com/BlessedChild/TimeTree/tree/main/sources/ble_gatt_server/macos-ble3.png)

# 结论&补充

经过研究，我在本文中提出一种新的方案-chrome://bluetooth-internals/#devices
该方案对比网上已公开的方案具有以下优势：
1. 步骤更简洁：在PC上进行调试，只需安装Chrome浏览器并输入地址chrome://bluetooth-internals/#devices
2. 拓展性更高：可通过Javascript直接编写调试程序
3. 本质上使用Chrome也是安装app的方式，chrome底层提供的接口对OS蓝牙接口进行了封装，另外，在手机上安装微信app并通过一些BLE调试小程序（微信搜索“ble调试”）也可以实现同样的效果
