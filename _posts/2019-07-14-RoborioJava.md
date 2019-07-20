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

This tutorial shows how to use the [datum-Distance](/datum/datum-Distance) sensor with the roboRIO using Java.  It covers configuring the sensor, setting up the build environment, and how to get the data from the sensor. 

## sensor configuration

Out of the box compact reports are disabled by default.  This is a great format for humans to read but not the best for robots to parse.  An example of what they look like is shown below.

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
Before plugging the datum sensor into the roboRIO check out the [getting started](/getting_started) guide.  This is a great introduction on how you can interact with the sensor directly.  We're going to start by changing the default report format.  Cut and paste the command below into the serial terminal and hit enter.

> set /config?compactReport=true

After entering the command the output looks like this.  Not quite as human readable but much easier to handle programmatically.

```json
{"timestamp":712.206,"distance":{"time":[712.2],"distance":[1783],"status":["RANGE_VALID"],"signalRateReturn":[3.445313],"ambientRateReturn":[2.71875]}}
```

This is also a great time to adjust the sample, data, and report rates.  Filters can be changed and units can be adjusted too. All the settings are saved on the sensor itself so you won't have to reconfigure it when it's plugged into the roboRIO.  There's more information about all of these parameters [here](/datum) and on the product pages.

Now unplug the sensor from the computer and plug it into the roboRIO.  Before jumping into the code there a couple things to do in the build environment first.  They aren't strictly necessary but it makes parsing the JSON data packets much easier.

## build environment

There 

This tutorial will use the 

### update build.gradle

Add the following line to the dependencies section of `build.gradle`.

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
## code

### imports

Add the following lines at the beginning of the file.

```java
import org.json.JSONObject;
import org.json.JSONArray;
import org.json.JSONException;
```

### initialize the serial port

Add a line to initialize the serial port.

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

Next add the block of code to teleopPeriodic.

```java
  /**
   * This function is called periodically during teleoperated mode.
   */
  @Override
  public void teleopPeriodic() {
    m_robotDrive.arcadeDrive(0.0, 0.0);
    
    String response = serialPort0.readString();

    if (response.length() > 1){
      try {
        JSONObject responseObject = new JSONObject(response);
        JSONObject distanceObject = responseObject.getJSONObject("distance");
        JSONArray distanceArray = distanceObject.getJSONArray("distance");
        double distance = distanceArray.getDouble(0);
        double timestamp = responseObject.getDouble("timestamp");
        System.out.print("timestamp: " + timestamp + "\t");
        System.out.println("distance: " + distance);
      } catch (JSONException ex) {
        ex.printStackTrace();
      }
    }
  }
```
Now build and deploy the code to the roboRIO.  Open the console 

```
 timestamp: 101839.806	distance: 1707.0 
 timestamp: 101839.906	distance: 1712.0 
 timestamp: 101840.006	distance: 1710.0 
 timestamp: 101840.106	distance: 1708.0 
 timestamp: 101840.206	distance: 1708.0 
 timestamp: 101840.306	distance: 1711.0 
 timestamp: 101840.406	distance: 1710.0 
 timestamp: 101840.506	distance: 1707.0 
 timestamp: 101840.606	distance: 1708.0 
 timestamp: 101840.706	distance: 1709.0 
 timestamp: 101840.806	distance: 1710.0 
 ```

{::options parse_block_html="true" /}

<details><summary markdown="span">Click for full code listing.</summary>

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
  private int counter = 0;

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
    
    String response = serialPort0.readString();

    if (response.length() > 1){
      try {
        JSONObject responseObject = new JSONObject(response);
        JSONObject distanceObject = responseObject.getJSONObject("distance");
        JSONArray distanceArray = distanceObject.getJSONArray("distance");
        double distance = distanceArray.getDouble(0);
        double timestamp = responseObject.getDouble("timestamp");
        System.out.print("timestamp: " + timestamp + "\t");
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
