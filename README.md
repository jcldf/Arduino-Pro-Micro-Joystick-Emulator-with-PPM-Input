# Arduino Pro Micro Joystick Emulator with PPM Input

This project transforms an Arduino Pro Micro into a USB joystick that can receive inputs from either physical buttons and analog sticks or a PPM signal from an RC transmitter, such as the Flysky FS-i6. The Arduino emulates a game controller on your computer, allowing you to control PC games or simulations using your RC transmitter or a custom-built controller.

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

###Usage
##Hardware Setup
PPM Input: Connect the PPM output of your RC receiver to ppmInputPin (Pin 3) on the Arduino. Ensure power and ground connections.
Buttons and Analog Sticks: Connect buttons and analog sticks to specified pins.
##Software Setup
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
