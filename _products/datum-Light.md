---
permalink: /datum/datum-Light/
title: ""
excerpt: "Foo Bar design system including logo mark, website design, and branding applications."
header:
  #image: /assets/images/foo-bar-identity.jpg
  #teaser: /assets/images/foo-bar-identity-th.jpg
layout: single

sidebar:
  - image: /assets/images/datum-Light/datum-Light Top.jpg
    image_alt: "datum-Light"
  - nav: lightInfo
#toc: true
---

# Available now on [GroupGets](https://store.groupgets.com/collections/datum-sensors/products/datum-light)! 
{: .text-center}

![alt text](/assets/images/datumLogo-small.png) datum-Light
==  
---

**specifications**

  - Ambient Light Sensing
  - RGB Color Sensing
  - Proximity Sensing
  - Maximum Report Rate: 100 Hz
  - Sensor Type: USB
  - Dimensions: 1.0" x 1.75" (25.4 mm x 44.4 mm)
  - Mounting holes: 4 x 4-40 holes, 0.75" x 1.5" on center (19.05 mm x 38.1 mm)
  
---

**features**
  - Atmel SAMD21G18 microcontroller
  - Broadcom APDS-9960 light sensor
  - Onboard EEPROM settings storage
  - URI style command interface
  - JSON formatted data packets
  - Globally unique 64 bit UUID
  - UF2 Bootloader

---

- **datum**
   1. a piece of information
   1. a fixed starting point of a scale or operation

- **light**
   1. the natural agent that stimulates sight and makes things visible

---

The datum-Light sensor combines the same SAMD21G18 microcontroller used on the Arduino Zero with the APDS-9960 light sensor from Broadcom to create the simplest, easiest to use light sensor for your application.

The datum-Light sensor emulates a serial port over a USB connection, presents the information and data stored on it in a JSON formatted packet, and processes URI style commands to change and retrieve its settings. The datum-Light sensor fills the gap between a LEGO&reg; Mindstorms&reg; sensor and a breakout board.

Control the behavior of the sensor by using URI style commands right from your favorite terminal program.  Want to set the report rate to 10 Hz and send the reports automatically?  The following command does just that.

```json
set /config?reportRate=10&automaticReporting=true
```

Not sure what the settings for the datum-Light sensor were?  The following command will retrieve them in a JSON formatted data packet.

```json
get /sensor/light/config

{
  "color": {
    "enabled": true,
    "units": "counts",
    "gain": 16,
    "integrationCycles": 64,
    "filterType": "mean",
    "sampleRate": 20,
    "dataRate": 10
  },
  "proximity": {
    "enabled": true,
    "units": "counts",
    "gain": 1,
    "LEDstrength": "25 mA",
    "filterType": "mean",
    "sampleRate": 20,
    "dataRate": 10
  }
}
```

The data from the datum-Light sensor is also encapsulated in a JSON formatted packet.  This results in human readable text that is still easy to parse with your favorite programming language.  An example of the output is shown below.

```json
{
  "timestamp": 84.804,
  "color": {
    "time": [84.675, 84.775],
    "ambient": [40, 40],
    "red": [19, 19],
    "green": [19, 19],
    "blue": [18, 18]
  },
  "proximity": {
    "time": [84.678, 84.778],
    "proximity": [7, 7]
  }
}
```


The datum-Light sensor can do much more than just collect the data.  The measurement units can be customized to suit your application.  light data can be returned in counts or normalized.  The datum-Light sensor does all the calculations for you.

It can also apply filters such as min, mix, mean, and RMS to the data stream.  This truly makes the datum-Light sensor a smart sensor that goes far beyond what a breakout board can do.  The datum-Light sensor is also programmed with an Arduino compatible bootloader allowing full customization of the firmware.


![alt text](/assets/images/datum-Light/datum-Light-Data1.png)
![alt text](/assets/images/datum-Light/datum-Light-Data2.png)

The datum-Light sensor is just one of the sensors in the datum line currently available.  The other sensors, datum-Weather, datum-Light, and datum-IMU all feature the same easy to use, text based interface.  More information on the full spectrum of sensors is available [here](/datum/).
