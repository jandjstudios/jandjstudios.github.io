---
title: "datum-Weather"
excerpt: "Foo Bar design system including logo mark, website design, and branding applications."
header:
  #image: /assets/images/foo-bar-identity.jpg
  #teaser: /assets/images/foo-bar-identity-th.jpg
sidebar:
  - title: "datum-Distance"
    image: /assets/images/datum-Distance.jpg
    image_alt: "datum-Distance"
    text: "Designer, Front-End Developer"
  - title: "Responsibilities"
    text: "Reuters try PR stupid commenters should isn't a business model"
gallery:
  - url: /assets/images/datum-Distance.jpg
    image_path: assets/images/datum-Distance.jpg
    alt: "placeholder image 1"
  - url: /assets/images/datum-Weather.jpg
    image_path: assets/images/datum-Weather.jpg
    alt: "placeholder image 2"
---


![alt text](https://jandjstudios.github.io/images/datumLogo-small.png "J&J Studios LLC")  datum-Distance
---
***
#### **datum**
   1. a piece of information
   1. a fixed starting point of a scale or operation

The datum-Distance sensor combines the same SAMD21G18 microcontroller used on the Arduino Zero with the RFD77402 ToF distance sensor from RF Digital to create the simplest, easiest to use distance sensor for your application.

The datum-Distance sensor emulates a serial port over a USB connection, presents the information and data stored on it in a JSON formatted packet, and processes URI style commands to change and retrieve its settings. The datum-Distance sensor fills the gap between a LEGO&reg; Mindstorms&reg; sensor and a breakout board.

Control the behavior of the sensor by using URI style commands right from your favorite terminal program.  Want to set the report rate to 10 Hz and send the reports automatically?  The following command does just that.

```JSON
set /config?reportRate=10&automaticReporting=true
```

Not sure what the settings for the datum-Distance sensor were?  The following command will retrieve them in a JSON formatted data packet.

```JSON
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

``` JSON
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

The datum-Distance sensor is just one of four sensors in the datum line currently available.  The other sensors, datum-Weather, datum-Light, and datum-IMU all feature the same easy to use, text based interface.  More information on the full spectrum of sensors is available [here](https://jandjstudios.github.io/datasheets/datumInformation.pdf).

#### **Features**
   - URI style command interface
   - JSON formatted data packets
   - Onboard EEPROM stores settings
   - Globally unique 64 bit UUID
   - Mounting holes in all four corners
   - 1.0" x 1.75" (25.4 cm x 44.4 cm)

![alt text](https://jandjstudios.github.io/images/J&J Studios LLC Logo-128x128.png "J&J Studios LLC")
