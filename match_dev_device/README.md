# 使用Chrome浏览器和开发板进行蓝牙配对测试

* 测试工具/代码：https://googlechrome.github.io/samples/web-bluetooth/device-info-async-await.html?service=internet_protocol_support&allDevices=true
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/match_dev_device/%E7%A4%BA%E4%BE%8B.png)

# Web Bluetooth / Device Info (Async Await) Sample

JavaScript Snippet
``` js
async function onButtonClick() {
  let filters = [];

  let filterService = document.querySelector('#service').value;
  if (filterService.startsWith('0x')) {
    filterService = parseInt(filterService);
  }
  if (filterService) {
    filters.push({services: [filterService]});
  }

  let filterName = document.querySelector('#name').value;
  if (filterName) {
    filters.push({name: filterName});
  }

  let filterNamePrefix = document.querySelector('#namePrefix').value;
  if (filterNamePrefix) {
    filters.push({namePrefix: filterNamePrefix});
  }

  let options = {};
  if (document.querySelector('#allDevices').checked) {
    options.acceptAllDevices = true;
  } else {
    options.filters = filters;
  }

  try {
    log('Requesting Bluetooth Device...');
    log('with ' + JSON.stringify(options));
    const device = await navigator.bluetooth.requestDevice(options);

    log('> Name:             ' + device.name);
    log('> Id:               ' + device.id);
    log('> Connected:        ' + device.gatt.connected);
  } catch(error)  {
    log('Argh! ' + error);
  }
}
```
