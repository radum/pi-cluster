# Setup Home Assistant

> TODO: This is still a WIP document and just shows my initial setup.

Steps:

**Step 1**

Install HA on PI.

Addons used:

```
Mosquitto broker
Node-RED
Visual Studio Code
Zigbee2mqtt
```

**Step 2**

Install zigbee2mqtt.io addons and use the Slae.sh stick to connect to devices

- https://medium.com/@fedorets.alex/hass-io-zigbee2mqtt-io-the-simplest-way-b7375c9fba22
- https://slae.sh/projects/cc2652/ - Has important info on how to find the device on Linux 

For example:

```
Another thing we need to take care of, is to use the "long" serial-port names!
Do NOT use the syntax like /dev/ttyUSBx.
Instead use the one like /dev/serial/by-id/usb-Silicon_Labs_slae.sh_cc2652rb_stick_-_slaesh_s_iot_stuff_00_12_4B_00_21_A8_E3_95-if00-port0!
```

**Step 3**

Add Node-Red with these modules:

```
  - node-red-dashboard
  - node-red-node-random
  - node-red-node-openweathermap
  - node-red-contrib-simple-message-queue
  - node-red-contrib-stoptimer
  - node-red-contrib-weekday
  - node-red-contrib-traffic
```

**Step 4**

Example on how to send mobile notifications:

- https://community.home-assistant.io/t/ios-notify/125782
