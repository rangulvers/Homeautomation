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
