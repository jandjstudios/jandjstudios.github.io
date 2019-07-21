---
title: datum, roboRIO, and Java
layout: single
read_time: false
comments: null
sidebar: false
author_profile: false
toc: true
toc_label: "contents"
toc_sticky: true
---

This tutorial shows how to use the [datum-Distance](/datum-Distance) sensor with the roboRIO and Java.  It covers configuring the sensor, setting up the build environment, and how to get the data from the sensor. 

## sensor configuration

Out of the box compact reports are disabled by default.  This is a great format for humans to read but not the best for robots to parse.  An example of what this looks like is shown below.  Take note of how the data packet is structured.  JSON uses a`"key":value`format.  That becomes important later.

```json
{
  "timestamp": 88.306,
  "distance": {
    "time": [88.3],
    "distance": [1794],
    "status": ["RANGE_VALID"],
    "signalRateReturn": [3.992188],
    "ambientRateReturn": [2.671875]
  }
}
```
Before plugging the datum sensor into the roboRIO check out the [getting started](/getting_started) guide.  This is a great introduction on how you can interact with the sensor directly.  We're going to start by changing the default report format.  

Plug the sensor into your computer, launch your favorite terminal program, and cut and paste the command below into the serial terminal and hit enter.  This command tells the sensor to return all the data on one line.

> set /config?compactReport=true

After entering the command the output looks like this.  Not quite as human readable but much easier to handle programmatically.

```json
{"timestamp":712.206,"distance":{"time":[712.2],"distance":[1783],"status":["RANGE_VALID"],"signalRateReturn":[3.445313],"ambientRateReturn":[2.71875]}}
```

This is also a great time to adjust the sample, data, and report rates.  Filters can be changed and units can be adjusted too. All the settings are saved on the sensor itself so you won't have to reconfigure it when it's plugged into the roboRIO.  There's more information about all of these parameters [here](/datum) and on the [product](/products) pages.

When you're done configuring the sensor unplug it from the computer and plug it into the roboRIO.  

## build environment

Before jumping into the code we need to make one change to the build environment first.  We're going to add the org.json [JSON-java](https://github.com/stleary/JSON-java) dependency to help with parsing the incoming data.  It's not strictly necessary but it makes working with the JSON data packets much easier.  Any parser such as GSON or Jackson could also be used and they will be featured in future tutorials.

### update build.gradle

To add the JSON-java dependency add the following line to the dependencies section of`build.gradle`.

> compile 'org.json:json:20180813'

Here's what the dependencies section looks like now.  Note that there may be other entries here depending on your specific build environment.

```java
// Defining my dependencies. In this case, WPILib (+ friends), and vendor libraries.
// Also defines JUnit 4.
dependencies {
    compile wpi.deps.wpilib()
    compile wpi.deps.vendor.java()
    compile 'org.json:json:20180813'
    nativeZip wpi.deps.vendor.jni(wpi.platforms.roborio)
    nativeDesktopZip wpi.deps.vendor.jni(wpi.platforms.desktop)
    testCompile 'junit:junit:4.12'
}
```
That's it for modifications to the build environment.  Next up is a simple example to show how to get data from the sensor.

## code

This example is built on the`TimedRobot`example included in the WPILib New Project Creator.  Any of the styles, Timed, Iterative, or Command, will work just fine but for this tutorial creating a new`TimedRobot`project will make it easier to follow along.  After you create the project open up`Robot.java`.  All the changes we need to make will be inside this file.

![alt text](/assets/images/posts/WPIlibNewProjectCreator.png){: .align-center}

There's just a few things we need to do to get data from the sensor.  First is importing the functions we'll be using, then creating and initializing the serial port, and finally reading in any data that's available and parsing the results.


### imports

Add the following lines at the beginning of the file.  This tells the compiler which packages and functions
we're going to using.

> org.json.JSONObject;  
> org.json.JSONArray;  
> org.json.JSONException;


```java
package frc.robot;

import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.PWMVictorSPX;
import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import edu.wpi.first.wpilibj.SerialPort;

import org.json.JSONObject;
import org.json.JSONArray;
import org.json.JSONException;
```

### instantiate the port

Next we need to create the variable to represent the serial port that data will be received on.  Add the line below at the beginning of the class definition.

> private SerialPort serialPort0;

Your project should look something like this.

```java
public class Robot extends TimedRobot {
  private final DifferentialDrive m_robotDrive
      = new DifferentialDrive(new PWMVictorSPX(0), new PWMVictorSPX(1));
  private final Joystick m_stick = new Joystick(0);
  private final Timer m_timer = new Timer();
  
  private SerialPort serialPort0;
```

### initialize the port

We need to initialize the serial port during`robotInit()`.  This tells the roboRIO what the name of the 
serial port is.  Note that it doesn't matter which physical port the sensor is plugged into.  They just get
named sequentially as kUSB, kUSB1, and kUSB2 if you have more than one.

Add the line below to the`roboInit()` function as shown.

> serialPort0 = new SerialPort(921600, SerialPort.Port.kUSB);

```java
/**
 * This function is run when the robot is first started up and should be
 * used for any initialization code.
 */
@Override
public void robotInit() {
  serialPort0 = new SerialPort(921600, SerialPort.Port.kUSB);
}
```  
### check for new data and parse the results

Next add the block of code below to`teleopPeriodic`.  This block of code grabs any data that's been sent to the serial port and stores it in`response`.  If there's data there the response is parsed during the creation of the`responseObject`.  Once the response has been successfully parsed we can extract the data from it.  The data is extracted using the keys in the JSON response.  The comments in the code block explain how this is done.

```java
  /**
   * This function is called periodically during teleoperated mode.
   */
  @Override
  public void teleopPeriodic() {
    m_robotDrive.arcadeDrive(0.0, 0.0);
    
    // Get any data that has been trasnmitted from the sensor
    String response = serialPort0.readString();

    // If data has been receive the length of the response will be greater than 1.
    if (response.length() > 1){
      try {
        // Data has been received, try parsing the response.
        JSONObject responseObject = new JSONObject(response);
        
        // Read the timestamp data from the response.
        double timestamp = responseObject.getDouble("timestamp");
        
        // The distance information is stored in a nested structure.  That has
        // to be extracted first, then the data arrays can be extracted next.
        JSONObject distanceObject = responseObject.getJSONObject("distance");
        JSONArray timeArray = distanceObject.getJSONArray("time");
        JSONArray distanceArray = distanceObject.getJSONArray("distance");

        // Grab the first values in the arrays.
        double time = timeArray.getDouble(0);
        double distance = distanceArray.getDouble(0);

        // Print out the values.
        System.out.print("timestamp: " + timestamp + "\t");
        System.out.print("time: " + time + "\t");
        System.out.println("distance: " + distance);           
      } catch (JSONException ex) {
        ex.printStackTrace();
      }
    }
  }
```
We're not doing any checking to make sure the communcation was complete so it's possible the parsing operation may fail.  If that happens it just creates an exception and we move on to the next transmission from the sensor.  Future tutorials will cover how to make this process more robust.

{::options parse_block_html="true" /}

<details><summary markdown="span">Click here for the full code listing.</summary>

```java
/*----------------------------------------------------------------------------*/
/* Copyright (c) 2017-2018 FIRST. All Rights Reserved.                        */
/* Open Source Software - may be modified and shared by FRC teams. The code   */
/* must be accompanied by the FIRST BSD license file in the root directory of */
/* the project.                                                               */
/*----------------------------------------------------------------------------*/

package frc.robot;

import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.PWMVictorSPX;
import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import edu.wpi.first.wpilibj.SerialPort;

import org.json.JSONObject;
import org.json.JSONArray;
import org.json.JSONException;

/**
 * The VM is configured to automatically run this class, and to call the
 * functions corresponding to each mode, as described in the TimedRobot
 * documentation. If you change the name of this class or the package after
 * creating this project, you must also update the manifest file in the resource
 * directory.
 */
public class Robot extends TimedRobot {
  private final DifferentialDrive m_robotDrive
      = new DifferentialDrive(new PWMVictorSPX(0), new PWMVictorSPX(1));
  private final Joystick m_stick = new Joystick(0);
  private final Timer m_timer = new Timer();
  
  private SerialPort serialPort0;

  /**
   * This function is run when the robot is first started up and should be
   * used for any initialization code.
   */
  @Override
  public void robotInit() {
    serialPort0 = new SerialPort(921600, SerialPort.Port.kUSB);
  }

  /**
   * This function is run once each time the robot enters autonomous mode.
   */
  @Override
  public void autonomousInit() {
    m_timer.reset();
    m_timer.start();
  }

  /**
   * This function is called periodically during autonomous.
   */
  @Override
  public void autonomousPeriodic() {
    // Drive for 2 seconds
    if (m_timer.get() < 2.0) {
      m_robotDrive.arcadeDrive(0.5, 0.0); // drive forwards half speed
    } else {
      m_robotDrive.stopMotor(); // stop robot
    }
  }

  /**
   * This function is called once each time the robot enters teleoperated mode.
   */
  @Override
  public void teleopInit() {
  }

  /**
   * This function is called periodically during teleoperated mode.
   */
  @Override
  public void teleopPeriodic() {
    m_robotDrive.arcadeDrive(0.0, 0.0);
    
    // Check to see if data has been trasnmitted from the sensor
    String response = serialPort0.readString();

    if (response.length() > 1){
      try {
        // Data has been received, try parsing the response.
        JSONObject responseObject = new JSONObject(response);
        
        // Read the timestamp data from the response.
        double timestamp = responseObject.getDouble("timestamp");
        
        // The distance information is stored in a nested structure.  That has
        // to be extracted first, then the data arrays can be extracted next.
        JSONObject distanceObject = responseObject.getJSONObject("distance");
        JSONArray timeArray = distanceObject.getJSONArray("time");
        JSONArray distanceArray = distanceObject.getJSONArray("distance");

        // Grab the first values in the arrays.
        double time = timeArray.getDouble(0);
        double distance = distanceArray.getDouble(0);

        // Print out the values.
        System.out.print("timestamp: " + timestamp + "\t");
        System.out.print("time: " + time + "\t");
        System.out.println("distance: " + distance);        

      } catch (JSONException ex) {
        ex.printStackTrace();
      }
    }
  }

  /**
   * This function is called periodically during test mode.
   */
  @Override
  public void testPeriodic() {
  }
}
``` 
</details>

## deploy the code

Now build and deploy the code to the roboRIO.  Open the console and you should see data streaming in like the example below.  If not you can always print the full`response`to make sure you're receiving data from the sensor.

```
 timestamp: 238.706	time: 238.675	distance: 1696.0 
 timestamp: 238.806	time: 238.775	distance: 1706.0 
 timestamp: 238.906	time: 238.875	distance: 1708.0 
 timestamp: 239.006	time: 238.975	distance: 1699.0 
 timestamp: 239.106	time: 239.075	distance: 1699.0 
 timestamp: 239.206	time: 239.175	distance: 1694.0 
 timestamp: 239.306	time: 239.275	distance: 1701.0 
 ```

