---
Title: Garage Door
---

### Garage Door Sensor

After installing the Hörmann ProMatic3 I wanted to also receive the status of the door to make sure it is closed at night or be alarmed when it is open when it should not be.
To my surprise this has been pretty straight forward.

Since the shelly comes with the 24v-60v DC option active by default there is nothing we need to change

#### **Hardware Setup**

- Connecting the power supply

    The ProMatic3 offers a direct power supply thru the board itself. Just connect

    **L(-) -> 20**

    **N(+) -> 5**

    <img src="images/ha_shelly_hoermann_1.jpg" width="50%" height="50%">

- Open / Close Status Information

    To also get a status about the current state of the gate you need to flip the the DL Button 2 to "ON". This is "OFF" by default
    DL Button 2 controls if the "End position message" should be triggered when the gate is closed
    Just connect the SW to the 0V terminal.

    <img src="images/ha_shelly_hoermann_3.jpg" width="50%" height="50%">

- Control the door

    I did not connect the O/I ports to control the gate with my shelly. If you also want to control the gate just connect the O/I ports of your shelly to the two terminal ports on the right (open in my picture). The order does not matter

#### **Homeassistant integration**

If you now connect your shelly to HA you will receive the status and can control your gate. There is only one issue. The information is somewhat misleading in HA since it will show "ON" for closed and "OFF" for open.
To fix that you can open up the Shelly App and change the setting "Reverse Input"

To get the right status ```OPEN / CLOSE``` you will need to convert the binary sensor of the shelly to something more meaningfull. As always there is more then one way to do this.

##### Option 1 Create a Custom Template

I also added a custom binary sensor to translate ON and OFF to OPEN and Close

```yaml
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

##### Option 2 Change Device Class

You can change the device class thru the ```configuration.yaml```

```yaml

homeassistant:
  customize:
    binary_sensor.shelly_garagentor_input: # change shelly class to garage door
      device_class: garage_door
      friendly_name: Garagentor Status

```