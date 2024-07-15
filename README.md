# DARKNESS.DETECTOR
## Description
The Darkness Detector is a basic project that uses an LDR (Light Dependent Resistor) and an Arduino Uno to detect the absence of light. When the ambient light level drops below a certain threshold, an LED turns on, indicating darkness. 
## Materials Required

### Hardware Requirement
- Arduino Uno
- LDR (Light Dependent Resistor)
- 10kΩ Resistor
- LED
- 270Ω Resistor
- Breadboard
- Jumper wires

### Software
- Arduino IDE

## Theory

### LDR (Light Dependent Resistor)
An LDR (Light Dependent Resistor) is a resistor whose resistance decreases with increasing incident light intensity. It is used in light-sensing circuits. Below is an image of an LDR:

![Ldr](https://github.com/Abhishek1problemsolver/Darkness_detector/assets/121240970/1944c083-bcb3-4a1d-b27e-fe5611f4a856)


### Arduino Uno
The Arduino Uno has several digital and analog pins used for input and output operations. Below is an image of the Arduino pin description:

![arduino_](https://github.com/Abhishek1problemsolver/Darkness_detector/assets/121240970/e325d4e1-c7fa-443f-83da-68d7a4017925)


### 74HC595 8-bit Parallel Shift Register
The 74HC595 is an 8-bit shift register with a serial input and a parallel output. It is used for expanding the number of I/O pins of a microcontroller. Below is an image of the 74HC595 shift register:

![74HC595-Shift-Register](https://github.com/Abhishek1problemsolver/Darkness_detector/assets/121240970/58e3d88f-8a64-43f8-82f8-cccda105d071)


## Circuit Diagram

### Prototype Image
![circuit_](https://github.com/Abhishek1problemsolver/Darkness_detector/assets/121240970/7b22fca7-1518-4c4f-826c-250babbe2751)


### Real Image from Project
![IMG20240321235450_BURST000_COVER](https://github.com/Abhishek1problemsolver/Darkness_detector/assets/121240970/ea435b78-1cc1-4334-8e80-f42df5e1e993)


## Working
The Darkness Detector project utilizes an LDR (Light Dependent Resistor) to sense ambient light levels and an Arduino Uno to process the sensor data and control an LED. Here’s a detailed explanation of how it works:

### Principle
An LDR changes its resistance based on the light intensity it is exposed to. In bright light, the resistance of the LDR is low, and in darkness, the resistance is high. By measuring the voltage across the LDR, we can determine the ambient light level.

### Setup and Components
- **LDR**: Connected in a voltage divider configuration with a fixed resistor (10kΩ). The junction of the LDR and the resistor is connected to an analog input pin (A0) of the Arduino to read the voltage level, which varies with light intensity.
- **LED**: Connected to one of the output pins of the 74HC595 shift register. The shift register is controlled by the Arduino and expands its output pins.

### Circuit Description
1. **LDR and Resistor**: The LDR and a 10kΩ resistor form a voltage divider. One end of the LDR is connected to 5V, the other end to the analog input pin A0, and the 10kΩ resistor connects this junction to ground (GND).
2. **Shift Register (74HC595)**: The shift register receives serial data from the Arduino and provides parallel outputs. The data pins (DS, ST_CP, and SH_CP) of the 74HC595 are connected to digital pins of the Arduino.
3. **LED and Resistor**: The LED is connected to one of the output pins of the 74HC595 through a 220Ω current-limiting resistor. The cathode of the LED is connected to GND.

### Steps
1. **Reading LDR Value**: The Arduino reads the analog voltage from the LDR using the `analogRead` function.
2. **Comparison with Threshold**: The read value is compared with a predefined threshold. If the value is below the threshold, it indicates low light levels (darkness).
3. **Controlling the LED via Shift Register**: Based on the comparison, the Arduino sends a signal to the 74HC595 shift register to set the output pin connected to the LED either HIGH (turning the LED on) or LOW (turning the LED off).

## Arduino IDE Code
```cpp
int lightpin = 0; 
int datapin = 4;
int clockpin = 6;
int latchpin = 5;

int leds = 0;



void setup() {
  pinMode(datapin, OUTPUT);
  pinMode(clockpin, OUTPUT); 
  pinMode(latchpin, OUTPUT);
}

void loop() {
  int reading = 1023 - analogRead(lightpin);
  int numLEDlit = reading / 57;
  if (numLEDlit > 8)
    numLEDlit = 8;

  leds = 0;

  for (int i = 0; i < numLEDlit; i++) {
    leds = leds + (1 << i);
  }
  updateshiftRegister();
  
}

void updateshiftRegister() {
  digitalWrite(latchpin, LOW);
  shiftOut(datapin, clockpin, LSBFIRST, leds); 
  digitalWrite(latchpin, HIGH); 
}

```
## video

[![Darkness Detector](https://github.com/Abhishek1problemsolver/Darkness_detector/assets/121240970/d712c99b-ae81-4ff0-82fe-073254f67c51)](https://drive.google.com/file/d/1zGaIQl206SzrmY0VrdrH1joacs8Q75ke/view?usp=drivesdk)

click on the image to see project in action

## Conclusion
The darkness detector works by continuously monitoring the ambient light level through the LDR. When the light level drops below the threshold, indicating darkness, the Arduino turns on the LED by controlling the 74HC595 shift register. This setup allows for efficient use of Arduino pins, enabling the control of multiple LEDs or other outputs if needed.

## Features
- Detects ambient light levels
- Turns on an LED when it gets dark
- Adjustable light sensitivity threshold
- Uses a 74HC595 shift register to expand output capabilities
- Simple and easy to build
- Low-cost components

## Uses
- Automating lighting systems
- Night-time security lighting
- Light-based alarms
- Educational projects to understand the working of LDRs, shift registers, and Arduino
