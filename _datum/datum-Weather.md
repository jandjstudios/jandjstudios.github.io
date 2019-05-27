---
title: ""
#excerpt: "Foo Bar design system including logo mark, website design, and branding applications."
header:
  #image: /assets/images/datum-Weather-small.jpg
  #teaser: /assets/images/foo-bar-identity-th.jpg

sidebar:
  #- title: "datum-Weather"
  - image: /assets/images/datum-Weather-small.jpg
    image_alt: "[datum-Weather](/assets/images/datum-Weather.jpg)"
    nav: weatherInfo
    #text: "datum-Weather"
  #- title: "firmware"

---
![alt text](/assets/images/datumLogo-small.png "J&J Studios LLC")  datum-Weather

---
#### **datum**
   1. a piece of information
   1. a fixed starting point of a scale or operation

#### **weather**
   1. the state of the atmosphere at a place and time as regards heat, dryness, sunshine, wind, rain, etc.

---
#### **features**
  - Atmel SAMD21G18 microcontroller
  - Bosch Sensortec BME280 environmental sensor
  - Onboard EEPROM settings storage
  - URI style command interface
  - JSON formatted data packets
  - Globally unique 64 bit UUID
  - Mounting holes in all four corners
  - 1.0" x 1.75" (25.4 cm x 44.4 cm)


  The datum-Weather sensor combines the same SAMD21G18 microcontroller used on the Arduino Zero with the BME280 environmental sensor from Bosch Sensortec to create the simplest, easiest to use weather sensor for your application.

  The datum-Weather sensor emulates a serial port over a USB connection, presents the information and data stored on it in a JSON formatted packet, and processes URI style commands to change and retrieve its settings. The datum-Weather sensor fills the gap between a LEGO&reg; Mindstorms&reg; sensor and a breakout board.

  Control the behavior of the sensor by using URI style commands right from your favorite terminal program.  Want to set the report rate to 10 Hz and send the reports automatically?  The following command does just that.

  ``` http
  set /config?reportRate=10&automaticReporting=true
  ```

  Not sure what the settings for the temperature sensor were?  The following command will retrieve them in a JSON formatted data packet.

  ``` json
  get /sensor/temperature/config

  {
    "enabled": true,
    "units": "C",
    "filterType": "mean",
    "sampleRate": 5,
    "dataRate": 1
  }
  ```

  The data from the datum-Weather sensor is also encapsulated in a JSON formatted packet.  This results in human readable text that is still easy to parse with your favorite programming language.  An example of the output is shown below.

  ```json
  {
    "timestamp": 352.001,
    "temperature": {
      "time": [350.6, 351.6],
      "temperature": [26.63574, 26.63965]
    },
    "humidity": {
      "time": [348, 349],
      "humidity": [34.05078, 34.04102]
    },
    "pressure": {
      "time": [348, 349],
      "pressure": [14.45278, 14.45191]
    },
    "altitude": {
      "time": [348, 349],
      "altitude": [140.4521, 140.9053]
    }
  }
  ```

  The datum-Weather sensor can do much more than just collect the data.  The measurement units can be customized to suit your application.  Temperature data can be returned in degrees Farenheit or Celsius.  Altitude could be in meters or feet.  The datum-Weather sensor does all the calculations for you.

  It can also apply filters such as min, mix, mean, and RMS to the data stream.  This truly makes the datum-Weather sensor a smart sensor that goes far beyond what a breakout board can do.  The datum-Weather sensor is also programmed with an Arduino compatible bootloader allowing full customization of the firmware.

  The datum-Weather sensor is just one of four sensors in the datum line currently available.  The other sensors, datum-Light, datum-Distance, and datum-IMU all feature the same easy to use, text based interface.  More information on the full spectrum of sensors is available [here](https://jandjstudios.github.io/datasheets/datumInformation.pdf).
