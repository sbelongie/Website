# ESPHome Bluetooth Proxy ESP32-Solo YAML

ESPHome YAML Configuration to create an ESP32-Solo (Single Core) Home Assistant Bluetooth Active Proxy.  In layman's terms, it is a remote network attached Bluetooth adapter Home Assistant can use to communicate with Bluetooth devices.  For instance, put one in your backyard to pick up temperature sensors in the pool and greenhouse; list of [Bluetooth Compatible Devices](/wiki/ha/bluetooth-compatible-devices)   

This file can be used as a base to adapt to other ESP32-Solo Devices such as [Linkind Switches/Dimmers Gen1](https://1b8d781e8c5fc5437e2f6f4d44a644d6.blogspot.com/p/linkind-esp32-smart-switch-how-to-flash.html), [Shelly Plus 1](/2021/12/01/diy-smart-garage-opener-local-shelly-plus), and more.

## The YAML

```yaml
substitutions:
  display_name: esp32solo-btproxy

esphome:
  name: ${display_name}

esp32:
  board: esp32dev
  framework:
    type: esp-idf
    sdkconfig_options:
      CONFIG_FREERTOS_UNICORE: y
      CONFIG_ESP_TASK_WDT_TIMEOUT_S: "10"

logger:
api:
ota:

button:
  - platform: safe_mode
    name: ${display_name} (Safe Mode)

# Change the WiFi config to a non-static IP if needed
wifi:
  ssid: !secret wifi_myssid
  password: !secret wifi_mypass
  manual_ip:
    static_ip: !secret ip_esp32solo_btproxy
    gateway: !secret ip_gateway
    subnet: !secret ip_subnet
    dns1: !secret ip_dns1

esp32_ble_tracker:
  scan_parameters:
# Adjust timing if the defaults do not work in your environment
#    interval: 1100ms
#    window: 1100ms
    active: true

bluetooth_proxy:
  active: true
```