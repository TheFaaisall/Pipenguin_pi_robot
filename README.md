# 🐧 Monash University - Autonomous Mobile Robotics Portfolio
## Intelligent Robotics & Autonomous Path Planning (Penguin Pi Platform)

This repository showcases the system architecture, algorithmic design, and deployment methodologies for an intelligent mobile robotics project completed as part of my engineering degree at **Monash University**. 

> ⚠️ **Academic Integrity Notice:** In accordance with university policies, the complete underlying production source code is private and cannot be publicly distributed. This documentation serves as a professional portfolio demonstrating engineering competency in integrating artificial perception, strategic reasoning, and robotic action. Visual telemetry, simulation footage, and system architecture are documented below to demonstrate operational results.

---

## 🎯 Core Engineering Objectives & Competencies
The project required the end-to-end development of an intelligent agent capable of operating in unstructured, time-varying environments to fulfil physical navigation tasks. Key competencies demonstrated include:

* **Mechatronics System Integration:** Analysing physical robot structures, sensing modalities, and actuation constraints to build a cohesive hardware-software matrix.
* **Algorithmic Software Engineering:** Designing, debugging, and validating robust algorithms for autonomous robot navigation, localisation, and spatial awareness.
* **Full-Stack Robotics Deployment:** Integrating machine perception and motion planning into a functional, real-time system to execute complex physical tasks.

---

## 🛠️ Technical Stack 
* **Hardware Platform:** Penguin Pi Mobile Robot (Differential-Drive Chassis, Raspberry Pi Single Board Computer, Onboard RGB Camera, Embedded Custom I/O Board).
* **Languages & Frameworks:** Python, Linux (Raspbian/Ubuntu), Bash Scripting.
* **DevOps & Infrastructure:** Docker, Containerisation, Cross-Platform Architecture, Environment Reproducible Builds.
* **Robotics Core:** Path Planning, Obstacle Avoidance, Kinematics, Differential-Drive Systems, Odometry Tracking, Closed-Loop Control (PID).
* **Computer Vision & Perception:** Image Processing, Spatial Awareness, Sensor Data Fusion, Localisation.

---

## 🔌 Hardware Interface & Custom Python API

The **Penguin Pi** architecture features an independent **I/O board equipped with an embedded processor**. This board directly interfaces with the drive motors and manages a local user interface consisting of a 20x4 OLED display, 4 user pushbuttons, and status LEDs. 

To bridge the gap between low-level hardware abstraction and high-level algorithmic control, a custom **Object-Oriented Python API** was implemented. Below is a functional example illustrating how the high-level software layer interacts with the hardware platform over a network interface:

```python
import time
from penguin_pi import PiBot

# 1. Initialise the robot connection via its network IP address
robot = PiBot(ip_address='192.168.3.4')

try:
    # 2. Query system diagnostics (Embedded I/O Board telemetry)
    battery_voltage = robot.get_voltage()
    print(f"System Initialized. Battery Telemetry: {battery_voltage}V")
    
    # 3. Control local UI elements (LEDs and 20x4 OLED Display)
    robot.set_led(led_id=2, state=True)          # Turn on the green status LED
    robot.printf_oled("System Online\nRunning...") # Write data to the 20x4 OLED
    
    # 4. Stream and process perception data
    frame = robot.get_image()                     # Capture raw RGB camera frame
    print(f"Captured frame buffer with dimensions: {frame.shape}")

    # 5. Execute differential-drive motor commands
    # Set left and right wheel velocities to +20 and -20 respectively
    robot.set_velocity(left_speed=20, right_speed=-20)
    time.sleep(2.0)                               # Run trajectory for 2 seconds

finally:
    # 6. Safety Interlocking
    robot.stop()                                  # Force halt all motor actuation
    robot.printf_oled("Execution Terminated\nMotors Stopped.")
```

### API Architecture Highlights:
* **Encapsulation:** Abstracts complex network sockets, low-level register maps, and I/O polling loops behind clean, declarative Python methods.
* **Safety Assertions:** Features an explicit error-handling block ensuring the robot gracefully issues a hardware-level `stop()` command if the high-level Python script crashes or faces an unhandled exception.
* **Modern Style Guidelines:** Designed using clean, Pythonic `snake_case` methods to mirror modern enterprise software engineering standards.

---

## 📐 System Architecture & Methodology

### 📦 Containerised Development (Docker & Python)
To bridge the gap between simulation and hardware deployment on the physical Penguin Pi, a modern DevOps workflow was implemented:
* Utilised **Docker containers** to package the Python runtime, external libraries, and dependencies for target ARM architectures.
* Ensured strict environment reproducibility, completely eliminating deployment friction between the development host PC and the onboard Raspberry Pi hardware.

### 🗺️ Path Planning & Navigation
* Developed deterministic path planning algorithms to compute optimal, collision-free trajectories through cluttered areas.
* Translated global coordinate paths into local wheel velocities using differential-drive kinematics specific to the Penguin Pi motor layout.
* Implemented dynamic feedback control loops to handle wheel slippage and real-world environmental drift.

### 👁️ Machine Perception & Data Fusion
* Processed live video feeds from the Penguin Pi camera via an asynchronous Python pipeline to identify structural features.
* Combined vision data with kinematic odometry to improve localisation accuracy within the environment boundaries.

---

## 🎥 Media & System Demonstrations

### 💻 Physical Setup & Simulation Environment
<img src="images/penguin_pi_online.jpg" alt="Penguin Pi Robot Setup" width="600">

*Figure 1: Online reference image of the Penguin Pi mobile robot setup alongside the digital twin simulation environment used to benchmark path planning algorithms.*

### 🚀 Autonomous Execution & Telemetry
<img src="images/my_robot_run.gif" alt="Autonomous Path Planning Video" width="600">

*Figure 2: Recorded telemetry demonstrating real-time obstacle avoidance, autonomous path generation, and target acquisition on the physical Penguin Pi mobile robot.*
