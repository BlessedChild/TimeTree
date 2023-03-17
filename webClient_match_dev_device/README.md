# Bluetooth pairing test using Chrome browser and development board

* Simple one:

Firstly, Open chrome and enter about://bluetooth-internals, and then you should arrived the page below
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/ota_ble_gatt_server/clickDevices.jpg)
Secondely，you should click "devices" button left，next，click “Start Scan” button top-right
![avatar](https://github.com/BlessedChild/TimeTree/sources/ota_ble_gatt_server/clickDevices_1.jpg)
Thirdly, click “Inspect” button on the right in the option named “ESP_GATTS_DEMO”
![avatar](https://github.com/BlessedChild/TimeTree/sources/ota_ble_gatt_server/clickDevices_2.jpg)
Finally, we can see the mac connect to the dev board, respone informations to mac on the page and outlog in the command line interface for the dev board, Client-Mac/Chrome:
![avatar](https://github.com/BlessedChild/TimeTree/sources/ota_ble_gatt_server/clickDevices_3.jpg)
Server-connected:
![avatar](https://github.com/BlessedChild/TimeTree/sources/ota_ble_gatt_server/connected.png)
Server-disconnected:
![avatar](https://github.com/BlessedChild/TimeTree/sources/ota_ble_gatt_server/disconnected.png)

* Test tool/code：https://googlechrome.github.io/samples/web-bluetooth/device-info-async-await.html?service=internet_protocol_support&allDevices=true
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
