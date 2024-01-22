---
title: Energy Monitoring
---

## Smart Meter Energy Monitoring

### Volkszähler Setup

First you need to have a way to collect the readings from you energy meter. You can follow this guide on how to connect your Energy Meter with Volkszähler

[Smart Meter Setup](https://github.com/rangulvers/smart_meter_setup)

### Homeassistant integration

If you have your Volkszähler integration up and running it is time to connect the instance to your homeassistant. For this we will setup the Volkszähler MQTT Homeassistant integration.

Add the following lines to your vzlogger.conf

````yaml
    "mqtt": {
        "enabled": true,  // enable mqtt client. needs host and port as well
        "host": "192.168.xx.xxx", // IP of your MQTT Broker
        "port": 1883, // 1883 for unencrypted, 8883 enc, 8884 enc cert needed,
        "cafile": "", // optional file with server CA
        "capath": "", // optional path for server CAs. see mosquitto.conf. Specify only cafile or capath
        "certfile": "", // optional file for your client certificate (e.g. client.crt)
        "keyfile": "", // optional path for your client certficate private key (e.g. client.key)
        "keypass": "", // optional password for your private key
        "keepalive": 30, // optional keepalive in seconds.
        "topic": "vzlogger/data", // optional topic dont use $ at start and no / at end
        "user": "mqtt_user", // optional user name for the mqtt server
        "pass": "mqtt_password", // optional password for the mqtt server
        "retain": true, // optional use retain message flag
        "rawAndAgg": true, // optional publish raw values even if agg mode is used
        "qos": 0, // optional quality of service, default is 0
        "timestamp": true // optional whether to include a timestamp in the payload
    },
````

Since I am running Homeassistant I am using the [Mosquitto Broker](https://github.com/home-assistant/addons/tree/master/mosquitto) to work with all MQTT messages

Next you need to configure the sensors within Homeassistant to map Volkszähler entities. Open up the ```` /config/configuration.yaml ```` and enter the one sensor for each entity you want to map

````yaml
sensor: 
  - platform: mqtt
    state_topic: "vzlogger/data/chn1/raw" 
    name: "ezh_Leistung_1670"
    unit_of_measurement: "W"
    device_class: power
    state_class: measurement
    value_template: "{{ value_json.value }}"
    
  - platform: mqtt
    state_topic: "vzlogger/data/chn0/agg" 
    name: "ezh_Bezug_180"
    unit_of_measurement: "kWh"  
    device_class: energy
    state_class: measurement
    value_template: "{{ (value_json.value / 1000) | round(4) }}"

````

Your meter information should now showup in your Homeassistant

![image](https://user-images.githubusercontent.com/5235430/131532612-fb492e99-a9f2-4710-b953-f20105f40912.png)

![image](https://user-images.githubusercontent.com/5235430/131532655-6b0db4ab-4321-486a-af40-78660300f391.png)

And thanks to the new energy function you can use those information to monitor you daily energy usage

![image](https://user-images.githubusercontent.com/5235430/131532831-898098d3-fd5f-4dbe-bdcc-3e983cc6df0c.png)

