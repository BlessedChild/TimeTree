# TimeTree

# 开发环境

* 开发板：blue、wifi模块（乐鑫ESP-C3）、开发版（CPU、USB、RGB、一些最基础的元件）、可靠的可传输数据线。
* MacOS：13.2 (22D49)
* [开发板驱动（USB转串口）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
* [开发工具链esp-idf-v5.0.1](https://github.com/espressif/esp-idf/releases/tag/v5.0.1)
* [蓝牙扫描工具chrome://bluetooth-internals/#devices](chrome://bluetooth-internals/#devices)

# 环境部署

1. 安装CH340开发板串口驱动，从开发板厂家官网下载：[开发板驱动（USB转串口）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/USB%E8%BD%AC%E4%B8%B2%E5%8F%A3CH340.png)
2. 连接硬件（需要能支持数据传输的数据线）
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E5%BC%80%E5%8F%91%E6%9D%BF%E9%80%9A%E7%94%B5.png)
3. 安装驱动，“刷新”设备中心
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E5%88%B7%E6%96%B0%E8%AE%BE%E5%A4%87%E4%B8%AD%E5%BF%83.png)
4. 查看设备中心，可以看到开发板被正常识别出来
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E6%9F%A5%E7%9C%8B%E8%AE%BE%E5%A4%87%E4%B8%AD%E5%BF%83.png)
5. 参考芯片厂家提供的[文档](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html)，安装开发OTA所需的依赖：cmake、ninja、dfu-util、ccache、XCode命令行工具
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/安装依赖.png)
6. 参考芯片厂家提供的[文档](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html)，安装开发OTA所需的IDE，即[开发工具链esp-idf-v5.0.1](https://github.com/espressif/esp-idf/releases/tag/v5.0.1)。因为一些原因，git clone会出现不完整的情况，所以直接通过前面的链接下载，最终ESP-IDF 将下载至 ~/esp/esp-idf
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/开发工具链esp-idf-v5.0.1.png)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E8%8E%B7%E5%8F%96ESP-IDF.png)
7. 参考芯片厂家提供的[文档](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html)，设置工具，除了 ESP-IDF 本身，您还需要为支持 ESP32-C3 的项目安装 ESP-IDF 使用的各种工具，比如编译器、调试器、Python 包等。输入：
``` shell
cd ~/esp/esp-idf
./install.sh esp32c3
```
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/设置工具.png)
8. 参考芯片厂家提供的[文档](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html)，设置环境变量，刚刚安装的工具尚未添加至 PATH 环境变量，无法通过“命令窗口”使用这些工具。因此，必须设置一些环境变量。这可以通过 ESP-IDF 提供的另一个脚本进行设置，在需要运行 ESP-IDF 的终端窗口运行以下命令：
``` shell
. $HOME/esp/esp-idf/export.sh
```
注意，命令开始的 “.” 与路径之间应有一个空格！
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/设置环境变量.png)
