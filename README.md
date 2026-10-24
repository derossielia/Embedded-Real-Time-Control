# Embedded Real-Time Control Systems (ERTC) — Course Laboratory Repository

This repository contains the software projects, lab assignments, and real-time control implementations developed for the **Embedded Real-Time Control Systems (ERTC)** university course at the **University of Padova (Department of Information Engineering - DEI)**.

The course covers theoretical foundations and hands-on laboratory experiences on embedded microcontroller architectures, real-time operating systems (RTOS), hardware peripherals, serial communication protocols, and closed-loop motor control algorithms. All firmware is designed to run on a **TurtleBot** mobile robot powered by an **STM32F767ZI** microcontroller (ARM Cortex-M7 core @ 216 MHz).


---

## Tech Stack &amp; Toolchain

* **Target Microcontroller:** STMicroelectronics **STM32F767ZI** (ARM Cortex-M7 with FPU &amp; MPU, 2 MB Flash, 512 KB SRAM)
* **Programming Language:** C (ISO C99 / C11, standard `stdint.h`)
* **IDE &amp; Development Environment:** **STM32CubeIDE v1.17** (Eclipse-based GCC cross-compilation toolchain)
* **Hardware Configuration Tool:** **STM32CubeMX** (`.ioc` pinout, clock tree, and peripheral initialization generator)
* **Software Abstraction Layer:** **STM32 Hardware Abstraction Layer (HAL)**
* **Real-Time Operating System:** **FreeRTOS**
* **Debugging &amp; Tracing:** **Serial Wire Viewer (SWV)** / **ITM (Instrumentation Trace Macrocell)** for real-time `printf` output over SWO port

---

##  Repository Structure

```
.
├── .gitignore                      # Excludes build outputs (Debug/, .elf, .hex) and IDE user settings
├── README.md                       # Repository overview and documentation
├── Lab00_GPIO_Interrupts/          # Lab 0: Basic GPIO driver, button input, LED toggling &amp; EXTI interrupts
│   ├── Core/
│   │   ├── Inc/                    # Header files (main.h, stm32f7xx_it.h)
│   │   └── Src/                    # C source files (main.c, stm32f7xx_it.c)
│   ├── Drivers/                    # STM32 HAL &amp; CMSIS drivers
│   └── Lab00_GPIO_Interrupts.ioc   # CubeMX peripheral configuration
├── Lab01_I2C_Keypad_LineSensor/    # Lab 1: I2C bus communication, SX1509 expanders, keypad &amp; timer sampling
│   ├── Core/Inc/print_utils.h      # ITM_SendChar redirect for printf debug logs
│   └── Core/Inc/SX1509_Utils.h     # SX1509 I2C register definitions &amp; initialization
├── Lab02_Motor_Characterization/   # Lab 2: Open-loop motor control &amp; system response identification
├── Lab03_ClosedLoop_PI_Control/    # Lab 3: Discrete-time PI motor speed controller
├── Lab04_LineFollower_System/      # Lab 4: Integrated autonomous line follower robot
└── Lab05_LineFollower_RTOS/        # Lab 5: Concurrent line follower using FreeRTOS tasks &amp; scheduling

```

---

##  Lab Modules Overview

### **Lab 0: GPIOs and External Hardware Interrupts**

* Configured GPIO pins as digital outputs (e.g., LED1 on `PE5`) and digital inputs with resistor pull-downs (e.g., User Button on `PF8`).
* Implemented asynchronous event handling using the **External Interrupt Controller (EXTI)** and **Nested Vectored Interrupt Controller (NVIC)** to replace inefficient CPU polling.
* Handled interrupt callbacks via `HAL_GPIO_EXTI_Callback()` to toggle system state.

### **Lab 1: I2C Communication, Line Sensor, Matrix Keypad &amp; Timers**

* Configured I2C bus (`I2C1`) to interface with two **SX1509 I/O expanders** (Slave addresses `0x3E` for Pololu QTR reflectance line sensor, `0x3F` for 16-key matrix keypad).
* Redirected C standard library `printf()` to SWV **ITM Stimulus Port 0** via custom `_write()` implementation in `print_utils.h`.
* Implemented dual sensing modes: periodic polling and timer-driven interrupts using hardware timer **TIM6** for 100 ms sampling rate.

### **Lab 2: Open-Loop Control &amp; Motor Characterization**

* Measured DC motor dynamic response using PWM signals and encoder feedback.
* Derived motor transfer function parameters for control system design.

### **Lab 3: Closed-Loop Motor Control (PI Controller)**

* Implemented a discrete-time Proportional-Integral (PI) speed controller in C.
* Regulated motor PWM duty cycle based on real-time quadrature encoder velocity feedback.

### **Lab 4: Autonomous Line Follower Integration**

* Fused reflectance line sensor array readings with motor control loops to implement closed-loop line tracking logic.

### **Lab 5: Real-Time Operating System (FreeRTOS)**

* Restructured line follower application into concurrent tasks (Sensor Reading, Control Calculation, Motor Drive, Safety Monitoring).
* Managed task priorities, synchronization primitives, and deterministic execution deadlines using FreeRTOS.

---

## ⚙️ How to Build, Flash, and Debug

### **1\. Prerequisites**

* Install **STM32CubeIDE v1.17** (matching the university lab PC environment).
* Connect the STM32F767ZI target board / TurtleBot via ST-LINK USB cable.

### **2\. Importing a Project into STM32CubeIDE**

1. Clone this repository to your local machine.
2. Launch STM32CubeIDE v1.17.
3. Navigate to **File &gt; Import... &gt; General &gt; Existing Projects into Workspace**.
4. Select the directory of a specific lab project (e.g., `Lab01_I2C_Keypad_LineSensor`).
5. **Important:** Do **NOT** check *"Copy projects into workspace"*. This ensures changes stay tracked inside your Git working directory.

### **3\. Building &amp; Flashing**

1. **Build:** Click the **Hammer** icon in the top toolbar (or press `Ctrl+B`) to invoke the GCC ARM cross-compiler.
2. **Flash Firmware:** Click the green **Play** button to upload the compiled ELF binary onto the board.
3. **Debug:** Click the **Bug** icon to open the debug perspective, step through instructions, set breakpoints, and inspect CPU registers.

### **4\. Enabling Real-Time printf Logs (SWV ITM)**

1. Open **Run &gt; Debug Configurations...** \-&gt; Select project -&gt; **Debugger** tab.
2. Check **Enable** under **Serial Wire Viewer (SWV)** and verify the Core Clock matches system clock (e.g., `96.0` MHz).
3. Start debugging session, then open **Window &gt; Show View &gt; SWV &gt; SWV ITM Data Console**.
4. Click the **Configure Trace** gear icon and enable **ITM Stimulus Port 0**.
5. Click **Start Trace** (red record button) and resume execution.

---

## License &amp; Acknowledgments

* **Academic Project:** Developed as part of the **Embedded Real-Time Control Systems (ERTC)** course at the **University of Padova (UNIPD)**.
* **Hardware Architecture:** STMicroelectronics STM32F767ZI, TurtleBot mobile robotics platform designed by Dr. Riccardo Antonello.