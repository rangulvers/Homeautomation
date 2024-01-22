---
Title: Automation
---

### Office Morning routine

My office is setup with

- Shelly 1 for the ceiling light
- Lidl LED Strip around a framed picture
- IKEA Light Bulb as a desk light
- OSRAM Smart Plug+ to control a standing light
- Aqara Motion sensor next to the door

When I walk into the office the motion sensor (PIR) is triggered and turns on all lights in my office. I also make use of the [FLUX](https://www.home-assistant.io/integrations/flux/) addon to generate a more natural light during the day.

The lights stay on as long I'm in my office. This is done via the access point that has a special WIFI just for my office. If my phone changes back to our normal WIFI when leaving the office all lights are turned off and the motion sensor returns to its default state.

I also added a the Sonoff SNZB-01 to control some custom light scenes.

- Single press  : cycle thru all scenes setup for my office. (Working, Hangout, Gaming, Reading)
- Double press  : toggle all lights on or off.
- Long press    : dim lights to 50%

See more [details](#zigbee-buttons-with-deconz-and-homeassistant) on how to use ZigBee Buttons with deConz and Homeassistant

### Alarm Mode and Presence Faker

#### **Alarm Mode**

One great use case for automation is the alarm mode. The mode turns on by itself when the house is empty based on the device trackers. If any of the motion, door, window or vibration sensors is triggered with the mode activated

- all lights in our house are turned on to full brightness
- the outdoor lights turn on as well and start blinking rapidly to drawn attention
- all cameras start recording
- a notification is send to my phone
- and to add a little bit of extra to it -> the smart speakers start playing a very unpleasant noice.

#### **Presence Faker**

To add a small layer of security I also added the [node-red-contrib-presence-faker](https://flows.nodered.org/node/node-red-contrib-presence-faker) node. This will turn on and off random lights in the house when we are away.
