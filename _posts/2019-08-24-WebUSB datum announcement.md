---
title: WebUSB and the datum sensors!
layout: single
read_time: false
sidebar: false
author_profile: false
toc: false
toc_label: "contents"
toc_sticky: true
comments: true
---

Adafruit recently announced that they added [WebUSB support to the TinyUSB core][1].  Well that USB core is now at the heart of all datum sensors!  

WebUSB is the perfect match for the JSON encapsulated data packets returned by each of the datum sensors.  No more messing around with baud rates, parity, or stop bits.  Plug in one of the datum sensors, hit connect on our new [console](/console/), and start interacting with your sensor immediately!

The URI command syntax is also perfect for the browser environment.  The same HTTP methods behind every web page out there are used to control the datum sensors.  Use GET commands to retrieve more information about your datum sensor.  Change the sample rate, units, and other settings using PUT or PATCH commands. 

Upgrading your datum sensor to the new USB core couldn't be easier.  Download the version 1.1 firmware from the [product](/products/) pages, double click the reset button, and drag and drop the uf2 file onto the datum-Boot drive.  That's all it takes to install the latest firmware!

[1]: https://blog.adafruit.com/2019/07/30/webusb-is-here-tinyusb-now-has-webusb-support-at-adafruit-tinyusb-tinyusb-webusb-chrome-googlechrome-adafruit-reillyeon-arduino/



