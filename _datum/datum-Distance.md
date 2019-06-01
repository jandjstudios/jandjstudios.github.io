---
title: ""
#excerpt: "Foo Bar design system including logo mark, website design, and branding applications."
header:
  #image: /assets/images/datumLogo-small.png
  #teaser: /assets/images/foo-bar-identity-th.jpg
sidebar:
  #- title: "datum-Distance"
  - image: /assets/images/datum-Distance/datum-Distance Top.jpg/350x250
    image_alt: "datum-Distance"
  - title: "more information"
    text: "[schematic](/assets/pdfs/datum-Distance.pdf)"

#toc: true
---
![alt text](/assets/images/datumLogo-small.png) datum-Distance
===  

URI command syntax. JSON encapsulated data packets. The ideal distance sensor for your next IoT project.

The datum-Distance sensor combines the same SAMD21G18 microcontroller used on the Arduino Zero with the VL53LX1 distance sensor from ST Microelectronics to create the simplest, easiest to use distance sensor for your application.

The datum-Distance sensor emulates a serial port over a USB connection, presents the information and data stored on it in a JSON formatted packet, and processes URI style commands to change and retrieve its settings. The datum-Distance sensor fills the gap between a LEGO&reg; Mindstorms&reg; sensor and a breakout board.

***
- **datum**
   1. a piece of information
   1. a fixed starting point of a scale or operation

- **distance**
   1. an amount of space between two things or people

---
### **features**
  - Atmel SAMD21G18 microcontroller
  - ST Microelectronics VL53LX1 distance sensor
  - Onboard EEPROM settings storage
  - URI style command interface
  - JSON formatted data packets
  - Globally unique 64 bit UUID
  - Mounting holes in all four corners
  - 1.0" x 1.75" (25.4 cm x 44.4 cm)

---

Control the behavior of the sensor by using URI style commands right from your favorite terminal program.  Want to set the report rate to 10 Hz and send the reports automatically?  The following command does just that.

```json
set /config?reportRate=10&automaticReporting=true
```

Not sure what the settings for the datum-Distance sensor were?  The following command will retrieve them in a JSON formatted data packet.

```json
get /sensor/distance/config

{
  "distance": {
    "enabled": true,
    "units": "mm",
    "filterType": "none",
    "sampleRate": 10,
    "dataRate": 10
  }
}
```

The data from the datum-Distance sensor is also encapsulated in a JSON formatted packet.  This results in human readable text that is still easy to parse with your favorite programming language.  An example of the output is shown below.

```json
{
  "timestamp": 18785.05,
  "distance": {
    "time": [18779.6, 18780.6, 18781.6, 18782.6],
    "z": [1888, 1855, 1988, 1556]
  }
}
```

The datum-Distance sensor can do much more than just collect the data.  The measurement units can be customized to suit your application.  Distance data can be returned in mm, cm, inches, or feet.  The datum-Distance sensor does all the calculations for you.

It can also apply filters such as min, mix, mean, and RMS to the data stream.  This truly makes the datum-Distance sensor a smart sensor that goes far beyond what a breakout board can do.  The datum-Distance sensor is also programmed with an Arduino compatible bootloader allowing full customization of the firmware.


![alt text](/assets/images/datum-Distance/datum-Distance-Data1.png "J&J Studios ")
![alt text](/assets/images/datum-Distance/datum-Distance-Data2.png "J&J Studios ")

The datum-Distance sensor is just one of four sensors in the datum line currently available.  The other sensors, datum-Weather, datum-Light, and datum-IMU all feature the same easy to use, text based interface.  More information on the full spectrum of sensors is available [here](https://jandjstudios.github.io/datasheets/datumInformation.pdf).
