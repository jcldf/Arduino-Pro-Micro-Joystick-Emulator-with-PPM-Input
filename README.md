# Arduino Pro Micro Joystick Emulator with PPM Input

This project transforms an Arduino Pro Micro into a USB joystick that can receive inputs from either physical buttons and analog sticks or a PPM signal from an RC transmitter, such as the Flysky FS-i6. The Arduino emulates a game controller on your computer, allowing you to control PC games or simulations using your RC transmitter or a custom-built controller.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Pin Configuration](#pin-configuration)
- [Code Explanation](#code-explanation)
  - [Libraries and Definitions](#libraries-and-definitions)
  - [Global Variables](#global-variables)
  - [Pin Setup](#pin-setup)
  - [Joystick Initialization](#joystick-initialization)
  - [Setup Function](#setup-function)
  - [Main Loop](#main-loop)
  - [Reading Inputs and Updating Joystick State](#reading-inputs-and-updating-joystick-state)
  - [Mapping Stick Values](#mapping-stick-values)
  - [Adjustments Function](#adjustments-function)
  - [PPM Signal Processing](#ppm-signal-processing)
- [Usage](#usage)
  - [Hardware Setup](#hardware-setup)
  - [Software Setup](#software-setup)
  - [Uploading the Code](#uploading-the-code)
  - [Testing](#testing)
  - [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview
This code allows an Arduino Pro Micro to emulate a USB joystick by reading inputs from:
- Physical buttons and analog sticks connected directly to the Arduino.
- PPM signal from an RC receiver connected to the Arduino, allowing the use of an RC transmitter as a game controller.

You can switch between input methods by setting the `usePPM` variable in the code.

## Features
- Emulates a standard USB gamepad with 14 buttons and 4 axes.
- Supports input from RC transmitters via PPM signal.
- Allows customization of button and axis mappings.
- Provides serial output for debugging and monitoring PPM signals.

## Hardware Requirements
- Arduino Pro Micro (5V version recommended)
- PPM-compatible RC receiver (e.g., FS-A8S) if using PPM input
- RC transmitter (e.g., Flysky FS-i6) bound to the receiver
- Physical buttons and analog sticks (optional if not using PPM input)
- Connecting wires
- Power source (the Arduino can power the receiver if it operates at 5V)

## Software Requirements
- Arduino IDE
- Joystick Library for Arduino by Matthew Heironimus
  - Install via Arduino Library Manager (Tools > Manage Libraries and search for "Joystick")

## Pin Configuration

### PPM Input
- `ppmInputPin`: Pin 3 (INT0 on Pro Micro)

### Buttons
| Button         | Pin |
|----------------|-----|
| Triangle       | 3*  |
| Circle         | 4   |
| Square         | 5   |
| Cross          | 6   |
| D-Pad Up       | 7   |
| D-Pad Down     | 8   |
| D-Pad Left     | 9   |
| D-Pad Right    | 10  |
| L1             | 14  |
| R1             | 16  |
| Select         | 18  |
| Start          | 19  |
| Home           | 20  |

\* Note: `triangleButtonPin` shares the same pin as `ppmInputPin` but is only used when `usePPM` is set to false.

### Analog Sticks
| Stick       | X Axis Pin | Y Axis Pin |
|-------------|------------|------------|
| Left Stick  | A3         | A4         |
| Right Stick | A5         | A6         |

## Code Explanation

### Libraries and Definitions
```cpp
#include <Joystick.h>
Joystick.h: This library allows the Arduino to emulate a USB joystick.

cpp
Copiar código
const boolean usePPM = true;
#define RC_CHANS 5
usePPM: Determines whether to use PPM input (true) or physical buttons and analog sticks (false).
RC_CHANS: Number of RC channels to read from the PPM signal.

Global Variables
cpp
Copiar código
volatile uint16_t rcValue[RC_CHANS] = {1502, 1502, 1502, 1502, 1502};
volatile boolean newDataReceived = false;
rcValue: Stores the latest PPM channel values.
newDataReceived: Flag indicating that new PPM data has been received.

Channel Enumeration
Assigns indices to each channel for easy reference:

cpp
Copiar código
enum {
  PITCH,
  YAW,
  THROTTLE,
  ROLL,
  AUX1
};
Pin Setup
cpp
Copiar código
const int ppmInputPin = 3; // INT0 on Pro Micro
ppmInputPin: The pin used for PPM input. On the Pro Micro, pin 3 corresponds to INT0.

cpp
Copiar código
const int triangleButtonPin = 3; // Same as ppmInputPin if not using PPM
const int circleButtonPin = 4;
// ... (other button pins)
Button Pins: Assigns pins to each button. If usePPM is false, the triangle button uses ppmInputPin.

cpp
Copiar código
const int leftStickXPin = A3;
const int leftStickYPin = A4;
const int rightStickXPin = A5;
const int rightStickYPin = A6;
Analog Stick Pins: Assigns analog input pins to the X and Y axes of the left and right sticks.

Joystick Initialization
cpp
Copiar código
Joystick_ Joystick(JOYSTICK_DEFAULT_REPORT_ID,
  JOYSTICK_TYPE_GAMEPAD,
  14, // Number of buttons
  0,  // Number of HAT switches
  true,  // Include X axis
  true,  // Include Y axis
  true,  // Include Z axis
  true,  // Include Rx axis
  false, // Include Ry axis
  false, // Include Rz axis
  false, // Include rudder
  false, // Include throttle
  false, // Include accelerator
  false, // Include brake
  false  // Include steering
);
Initializes the joystick with specified parameters, emulating a gamepad with 14 buttons and 4 axes.

Setup Function
Initializes serial communication and joystick emulation.

cpp
Copiar código
void setup() {
  Serial.begin(115200);
  setupPins();
  Joystick.begin();
}
Main Loop
Reads inputs, updates joystick state, and prints channel values when new PPM data is received.

cpp
Copiar código
void loop() {
  readInputsAndUpdateJoystick();
}
Reading Inputs and Updating Joystick State
Handles button states, analog sticks, and PPM input.

cpp
Copiar código
void readInputsAndUpdateJoystick() {
  // ...
}
Mapping Stick Values
Converts RC channel values to a range suitable for joystick axes.

cpp
Copiar código
byte stickValue(int rcVal) {
  return map(constrain(rcVal - 1000, 0, 1000), 0, 1000, 0, 255);
}
Adjustments Function
Applies fine adjustments to the RC channel values.

cpp
Copiar código
uint16_t adjust(uint16_t diff, uint8_t chan) { /*...*/ }
PPM Signal Processing
Interrupt service routine for decoding the PPM signal.

cpp
Copiar código
void rxInt(void) { /*...*/ }
Usage
Hardware Setup
PPM Input: Connect the PPM output of your RC receiver to ppmInputPin (Pin 3) on the Arduino. Ensure power and ground connections.
Buttons and Analog Sticks: Connect buttons and analog sticks to specified pins.
Software Setup
Install the Joystick Library via Arduino Library Manager.
Set usePPM to true if using PPM input or false to use physical inputs.
Uploading the Code
Connect the Arduino Pro Micro.
Select the correct board and port in the Arduino IDE.
Upload the code.
Testing
Open Serial Monitor at 115200 baud to check PPM values.
Verify joystick functionality in game controller settings on your computer.
Troubleshooting
Joystick Not Recognized: Ensure the Joystick library is installed and the Arduino is detected.
No PPM Data Received: Verify connections and PPM settings.
Axes Not Centered or Incorrect Range: Adjust the stickValue() function or the adjust() function.
License
This project is open-source and available under the MIT License.
