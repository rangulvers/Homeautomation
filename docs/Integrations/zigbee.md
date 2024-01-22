---
Title: Zigbee
---

### ZigBee Buttons with deConz and Homeassistant


Adding lights or outlets to your installation is pretty easy, but what about button and switches? It is almost as easy but takes a couple of steps to get everything working as you wish.

#### Add your buttons and switches

thru the phoscon app by clicking on ```switches``` and then on ```Add new switch```

<img src="images/ha_button_switch.png" width="50%" height="50%">

1. The pairing will start and you will need to set your switch into pairing mode.

2. After the pairing is done, go back into your HA view and look for the switch. It is important to note that a switch will not show any state change that you can use.

3. To still be able to use the switch you need to react to the event of the switch send to the event bus. To find the event go the the developer tools and listen to the ```deconz_event```. This should look something like this

```json
{
    "event_type": "deconz_event",
    "data": {
        "id": "office_work_switch",
        "unique_id": "00:12:4b:00:22:29:41:b0",
        "event": 1004,
        "device_id": "edaab2525312c6e5d15851af28328106"
    },
    "origin": "LOCAL",
    "time_fired": "2021-01-20T08:40:59.909382+00:00",
    "context": {
        "id": "37db6e5c382a00cd12232ceac7970f3c",
        "parent_id": null,
        "user_id": null
    }
}
```

As you can see the event shows up with ``` "event": 1004 ```. You will have to note down the numbers to move forward
For the Sonoff SNZB-01 will give you the following events

- 1002 -> Single Press
- 1003 -> Long Press
- 1004 -> double Press

5. You can now create your automation based on those information. Since I use more than one button and wanted to manage all actions in one view, I'm using node-red for this.

Create a "events: all" node and under event type select the ```deconz_event```.
Next add a switch node to seperate the events by device id
Now you can add another switch node after on each output to define the different button actions.

<img src="images/ha_button_node_red.png" width="400" height="400">