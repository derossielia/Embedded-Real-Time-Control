**Embedded Real-Time Control (ERTC) - Lab Projects**

This repository contains the C language projects and source code developed for the Embedded Real-Time Control (ERTC) course (DEI - Unipd).
The software is designed to interface with the sensors and actuators of a TurtleBot rover, utilizing an STM32F767 microcontroller based on the ARM Cortex-M7 architecture.

**Tech Stack & Tools**

Language: C   
IDE: STM32CubeIDE   
Hardware Configuration: STM32CubeMX for graphical pinout configuration, clock tree setup, and automatic initialization code generation.   
Libraries: STM32 HAL (Hardware Abstraction Layer).  

**Repository Structure**

Lab 0: GPIO and Interrupts
This module introduces fundamental Input/Output operations and asynchronous event handling.   Configured GPIO pins in output mode to drive LEDs (e.g., pin PE5).   Configured GPIO pins in input mode to read button states (e.g., pin PF8).   Implemented hardware interrupts using the EXTI controller and NVIC to detect button presses without continuous CPU polling.   
Lab 1: I2C Bus, Line Sensor, and Keypad
This module focuses on synchronous serial communication and advanced timing mechanisms.   Initialized and communicated over the I2C bus with two SX1509 I/O expanders (slave addresses 0x3E and 0x3F).   Integrated a line sensor and a 16-key keypad, handling external interrupts on pin PF4 triggered by key presses.   Redirected the standard printf system call via ITM to display debug logs directly in the STM32CubeIDE console.   Configured a hardware timer (TIM6) to generate periodic interrupts.   

**Build and Execution Instructions**

Clone this repository to your local machine.
Open STM32CubeIDE and import the project by selecting File -> Import project from File System.   
Build the project using the IDE's compiler tools.
Flash the generated binary onto the STM32F767 microcontroller and use the debug perspective to monitor variables and ITM console output.
