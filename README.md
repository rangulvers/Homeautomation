# Network and Homeautomation

Collection of scripts, tools, hardware and other elements used for our home automation setup

## Hardware

### Sensors and Automation
* Raspberry PI 4 to integrate more Zigbee devices  [link](https://www.amazon.de/Raspberry-Pi-ARM-Cortex-A72-Bluetooth-Micro-HDMI/dp/B07TC2BK1X/ref=sxts_sxwds-bia-wc-p13n1_0?adgrpid=70762780283&cv_ct_cx=raspberry+pi+4&dchild=1&gclid=Cj0KCQiAhs79BRD0ARIsAC6XpaVwlOQdd5mw_bwf6e5xMAyySUobhpqzcAI24BbjxlNVSjHTf2_POM8aArT7EALw_wcB&hvadid=352854576614&hvdev=c&hvlocphy=9041983&hvnetw=g&hvqmt=e&hvrand=3613656398060616438&hvtargid=kwd-297124344473&hydadcr=8207_1722838&keywords=raspberry+pi+4&pd_rd_i=B07TC2BK1X&pd_rd_r=a2d9be5f-034a-40e0-ac72-739b15f88466&pd_rd_w=NwjsC&pd_rd_wg=aMne9&pf_rd_p=62eb0a5a-7892-4776-9eb7-4ce13a045c59&pf_rd_r=NWRPERZQ99WGD72NXMGX&psc=1&qid=1605617162&quartzVehicle=812-409&replacementKeywords=raspberry+pi&sr=1-1-79e1db8b-ac0e-4e53-86a0-e4b4f9bb89cd&tag=googhydr08-21) running Homeassistant as the control center
* Shelly 1 [link](https://shelly.cloud/products/shelly-1-smart-home-automation-relay/)
* Shelly 2.5  [link](https://shelly.cloud/products/shelly-25-smart-home-automation-relay/)
* Sonoff Zigbee Brdige to add Zigbee devices [link](https://www.itead.cc/sonoff-zbbridge.html) 
* Sonoff Zigbee Wirless Switch
* Sonoff Zigbee Temperatue and humidity Sensor
* Sonoff Zigbee Motion Sensor
* Sonoff Zigbee Wireless door / window sensor
* Aqara Motion Sensor [link](https://www.aqara.com/eu/motion_sensor.html)
* Aqare Vibration Sensor [link](https://www.aqara.com/eu/vibration_sensor.html)

### Lights
* IKEA TRADFRI Starter Set [link](https://www.ikea.com/de/de/p/tradfri-set-mit-gateway-farb-und-weissspektrum-00406887/)
* IKEA TRADFRI Bulbs (many of them) [link](https://www.ikea.com/de/de/p/tradfri-led-leuchtmittel-e27-600-lm-kabellos-dimmbar-farb-und-weissspektrum-farb-und-weissspektrum-rund-opalweiss-00408612/)
* Lidl LED-Strip [link](https://www.lidl.de/de/livarno-lux-led-band-zigbee-smart-home-individuell-teilbar-selbsthaftend/p354570)
* Lidl Xmas Lights [link](https://www.lidl.de/de/melinera-lichterkette-zigbee-smart-home/p360021)
* Lidl Smartplug 

### Network
* Ubiquiti AP UAP-AC-LITE [link](https://www.amazon.de/gp/product/B016K4GQVG/ref=ox_sc_saved_image_1?smid=A3JWKAKR8XB7XF&psc=1)
* Ubiquite AP UAP-AC-PRO [link](https://www.amazon.de/gp/product/B016XYQ3WK/ref=ox_sc_saved_image_2?smid=A3JWKAKR8XB7XF&psc=1)
* Netgear 16-Port POE [link](https://www.amazon.de/Netgear-JGS516PE-100EUS-16-Port-ProSAFE-Managed/dp/B00F3XSLWI/ref=sr_1_5?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=2S0I128KF7QT1&dchild=1&keywords=netgear+poe+16+port&qid=1605617897&s=computers&sprefix=netgear+poe+16%2Ccomputers%2C174&sr=1-5)

### Cameras
* Reolink RLC-511w [link](https://reolink.com/de/product/rlc-511w/)


# Home Automations

Shellys have been a core component in my smart home setup. Controlling everything from lights, rbg strips and now also the door bell notification and garage door sensor

## Garage Door Sensor

After installing the Hörmann ProMatic3 I wanted to also retrieve the status of the door to make sure it is closed at night or be alarmed when it is opend when it should not be. 
To my suprise this has been pretty straight forward. 

Since the shelly comes with the 24v-60v DC option active by default there is nothing we need to change

### Hardware Setup
* Connecting the power supply

    The ProMatic3 offers a direct powersupply thru the board itself. Just connect 
    
    L(-) -> 20 
    
    N(+) -> 5
    
    ![](images/ha_shelly_hoermann_1.jpg)
    
* Status

    To also get a status about the current state of the gate you need to flip the the DL Button 2 to "ON". This is "OFF" by default
    DL Button 2 controlls if the "End position message" should be triggered when the gate is closed
    Just connect the SW to the 0V terminal. 
    
    ![](images/ha_shelly_hoermann_3.jpg)

* Controll

    I did not connect the O/I ports to actually controll the gate with my shelly. If you also want to controll the gate just conncet the O/I ports of your shelly to the two terminal ports on the right (open in my picture). The order does not matter

### Homeassistant integration
If you now connect your shelly to HA you will recieve the status and can controll your gate. There is only one issue. The information is somewhat missleading in HA since it will show "ON" for closed and "OFF" for open. 
To fix that you can open up the Shelly App and change the setting "Reverse Input"

I also added a custom binary sensor to translate ON and OFF to OPEN and Close

```
 - platform: template
    sensors:
      sensor_name:
        value_template: '{% if states.{sensor}.state %}
          {% if states.{sensor}.state == "on" %}
            Open
          {% else %}
            Closed
          {% endif %}
          {% else %}
          n/a
          {% endif %}'
        friendly_name: 'friendly sensor name'
```

![](images/ha_shelly_garage_door.png)


## Balter EVO-7M Doorbell 

After installing my Balter Doorbell system I realized that I the bell sound of both monitors in the living room and upstairs floor are not loud enough to notify me in my office located in the basement. After reading the documentation I realized that I can use the external bell output to trigger a shelly switch. 

This has to be connected to the main monitor of the system otherwise it won't work. 

![](images/ha_shelly_balter_doorbell.png)

In Homeassistant I also created a Node-Red flow to notify me when someone is at the door and to flash the lights in my office

![](images/ha_shelly_balter_doorbell_node_red.png)
