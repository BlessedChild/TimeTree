# TimeTree

# 开发环境

* 开发板：blue、wifi模块（乐鑫ESP-C3）、开发版（CPU、USB、RGB、一些最基础的元件）、可靠的可传输数据线。
* MacOS：13.2 (22D49)
* [开发板驱动（USB转串口）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
* [开发工具链esp-idf-v5.0.1](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html)
* [chrome蓝牙扫描](chrome://bluetooth-internals/#devices)

# 环境部署

1. 安装CH340开发板串口驱动，从开发板厂家官网下载：[开发板驱动（USB转串口）](https://www.wch.cn/downloads/CH34XSER_MAC_ZIP.html)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/USB%E8%BD%AC%E4%B8%B2%E5%8F%A3CH340.png)
2. 连接硬件（需要能支持数据传输的数据线）
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E5%BC%80%E5%8F%91%E6%9D%BF%E9%80%9A%E7%94%B5.png)
3. 安装驱动，“刷新”设备中心
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E5%88%B7%E6%96%B0%E8%AE%BE%E5%A4%87%E4%B8%AD%E5%BF%83.png)
4. 查看设备中心，可以看到开发板被正常识别出来
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E6%9F%A5%E7%9C%8B%E8%AE%BE%E5%A4%87%E4%B8%AD%E5%BF%83.png)
5. 参考芯片厂家提供的文档，安装开发OTA所需的[开发工具链esp-idf-v5.0.1](https://docs.espressif.com/projects/esp-idf/zh_CN/v5.0.1/esp32c3/get-started/linux-macos-setup.html)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7%E9%93%BEesp-idf-v5.0.1.png)
6. 
