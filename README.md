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
