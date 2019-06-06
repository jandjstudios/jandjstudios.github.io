---
title: ""
#excerpt: "Foo Bar design system including logo mark, website design, and branding applications."
header:
  #image: /assets/images/datum-Distance-small.jpg
  #teaser: /assets/images/foo-bar-identity-th.jpg

sidebar:
  #- title: "datum-IMU"
  - image: /assets/images/datum-IMU/datum-IMU Top.jpg
    image_alt: "datum-IMU"
    #text: "Designer, Front-End Developer"
  - title: "More Information"
    text: "[schematic](/assets/pdfs/datum-IMU.pdf)"
#toc: true
---
![alt text](/assets/images/datumLogo-small.png) datum-IMU
===  

---
- **datum**
   1. a piece of information
   1. a fixed starting point of a scale or operation

- **inertia**
   1. a property of matter by which it continues in its existing state of rest or uniform motion in a straight line, unless that state is changed by an external force

---
**features**

  - Atmel SAMD21G18 microcontroller
  - ST Microelectronics LSM9DS1 IMU
  - Onboard EEPROM settings storage
  - URI style command interface
  - JSON formatted data packets
  - Globally unique 64 bit UUID
  - Mounting holes in all four corners
  - 1.0" x 1.75" (25.4 cm x 44.4 cm)

---

URI command syntax. JSON encapsulated data packets. The ideal IMU for your next IoT or robotics project.

The datum-IMU sensor combines the same SAMD21G18 microcontroller used on the Arduino Zero with the LSM9DS1 IMU sensor from ST Microelectronics to create the simplest, easiest to use IMU sensor for your application.

The datum-IMU sensor emulates a serial port over a USB connection, presents the information and data stored on it in a JSON formatted packet, and processes URI style commands to change and retrieve its settings. The datum-IMU sensor fills the gap between a LEGO&reg; Mindstorms&reg; sensor and a breakout board.

Control the behavior of the sensor by using URI style commands right from your favorite terminal program.  Want to set the report rate to 10 Hz and send the reports automatically?  The following command does just that.

```json
set /config?reportRate=10&automaticReporting=true
```

Not sure what the settings for the datum-IMU sensor were?  The following command will retrieve them in a JSON formatted data packet.

```json
get /sensor/IMU/config

{
  "IMU": {
    "enabled": true,
    "units": "mm",
    "filterType": "none",
    "sampleRate": 10,
    "dataRate": 10
  }
}
```

The data from the datum-IMU sensor is also encapsulated in a JSON formatted packet.  This results in human readable text that is still easy to parse with your favorite programming language.  An example of the output is shown below.

```json
{
  "timestamp": 18785.05,
  "IMU": {
    "time": [18779.6, 18780.6, 18781.6, 18782.6],
    "z": [1888, 1855, 1988, 1556]
  }
}
```

The datum-IMU sensor can do much more than just collect the data.  The measurement units can be customized to suit your application.  IMU data can be returned in mm, cm, inches, or feet.  The datum-IMU sensor does all the calculations for you.

It can also apply filters such as min, mix, mean, and RMS to the data stream.  This truly makes the datum-IMU sensor a smart sensor that goes far beyond what a breakout board can do.  The datum-IMU sensor is also programmed with an Arduino compatible bootloader allowing full customization of the firmware.


![alt text](/assets/images/datum-IMU/datum-IMU-Data1.png "J&J Studios ")
![alt text](/assets/images/datum-IMU/datum-IMU-Data2.png "J&J Studios ")

The datum-IMU sensor is just one of four sensors in the datum line currently available.  The other sensors, datum-Weather, datum-Light, and datum-IMU all feature the same easy to use, text based interface.  More information on the full spectrum of sensors is available [here](https://jandjstudios.github.io/datasheets/datumInformation.pdf).