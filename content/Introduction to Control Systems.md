# Introduction to Control Systems

A **control system** is a device or group of devices used to **manage, command, direct, or regulate** the behavior of another system.

Control systems are widely used in:
- Drones
- Aircraft
- Robotics
- Automobiles
- Industrial automation
- Spacecraft

In drones, control systems help maintain:
- Stability
- Orientation
- Altitude
- Navigation
- Autonomous flight

---

# 1. Basic Definitions

## System
A **system** is anything that:
1. Takes an input
2. Processes it
3. Produces an output

### Example
- Input → joystick command
- System → drone
- Output → drone movement

---

## Plant / Process
The **plant** is the actual object being controlled.

### Example
In drones:
- Motors
- Propellers
- Drone body dynamics

together form the plant.

---

## Signal
A **signal** carries information.

### Examples
- Sensor readings
- GPS coordinates
- Motor commands
- Gyroscope values

Signals usually vary with time.

---

# 2. Manual vs Automatic Control

## Manual Control
The human directly controls the system.

### Example
A pilot manually flying a drone using a remote controller.

### Characteristics
- Depends on human reaction time
- Less accurate
- Fatigue affects performance

---

## Automatic Control
The system controls itself using:
- Sensors
- Controllers
- Algorithms

### Example
Drone altitude hold mode.

### Advantages
- Faster response
- Better accuracy
- Continuous correction
- Reduced human workload

---

# 3. Open-Loop vs Closed-Loop Systems

---

# Open-Loop System

In an open-loop system:
- Output is not measured
- No feedback exists
- System cannot correct errors

## Block Diagram

```text
Input → Controller → System → Output
```

## Example
A drone spins motors for 5 seconds regardless of:
- wind disturbance
- battery condition
- actual lift-off

### Advantages
- Simple
- Cheap
- Easy to design

### Disadvantages
- No correction mechanism
- Less accurate
- Sensitive to disturbances

---

# Closed-Loop System (Feedback System)

A closed-loop system continuously checks the output and corrects errors.

## Working Principle
1. Desired output is given
2. Sensors measure actual output
3. Difference is called error
4. Controller reduces the error

---

## Closed-Loop Block Diagram

```text
           +----------------------+
           |                      |
           |      Feedback        |
           |                      |
Input → (+) → Controller → Plant → Output
          ↑                       |
          |_______________________|
```

---

## Example: Drone Attitude Hold

Suppose:
- Desired roll angle = 0°
- Wind tilts drone by 10°

Sensors detect tilt and the controller adjusts motor speeds to restore balance.

---

## Advantages
- Self-correcting
- More accurate
- Better stability
- Handles disturbances

---

# 4. Feedback in Control Systems

Feedback means:
> Taking part of the output and feeding it back into the system input.

---

# Negative Feedback

Most commonly used in engineering.

It:
- reduces error
- stabilizes the system

## Example
If drone altitude becomes too high:
- controller reduces thrust

If altitude becomes too low:
- controller increases thrust

This maintains stable altitude.

---

# Positive Feedback

Output reinforces the input.

Usually:
- amplifies errors
- causes instability

## Example
Microphone squealing near speakers.

Rarely used in drones.

---

# 5. Feedback Control System Components

```text
Reference Input
       ↓
   Comparator
       ↓
 Controller C(s)
       ↓
   Actuator
       ↓
   Drone G(s)
       ↓
    Output
       ↓
    Sensor
       ↓
   Feedback
```

---

## Comparator

Calculates error:

\[
e(t) = r(t) - y(t)
\]

Where:
- \(r(t)\) = desired output
- \(y(t)\) = actual output
- \(e(t)\) = error

---

## Controller \(C(s)\)

Determines how to reduce error.

### Common Controllers
- P Controller
- PI Controller
- PID Controller

---

## Actuator

Converts electrical signals into physical movement.

### Example
Drone motors.

---

## Plant \(G(s)\)

The system being controlled.

### Example
The drone itself.

---

## Sensor

Measures system output.

### Examples
- IMU
- Gyroscope
- Accelerometer
- GPS
- Barometer

---

# 6. System Representation Methods

Engineers use mathematics to model systems.

---

# Differential Equation Representation

Represents system behavior in the time domain.

## Example

\[
m\ddot{x} + b\dot{x} + kx = F(t)
\]

Where:
- \(m\) = mass
- \(b\) = damping
- \(k\) = stiffness

---

# Transfer Function Representation

Uses Laplace Transform.

## General Form

\[
G(s) = \frac{Output(s)}{Input(s)}
\]

### Advantages
- Easier analysis
- Easier stability study
- Useful for controller design

---

# State Space Representation

Represents systems using:
- State variables
- Matrices

Widely used in:
- Robotics
- Drones
- Spacecraft
- Modern control systems

---

# 7. Stability

A control system must remain stable.

---

# BIBO Stability

BIBO means:

> Bounded Input → Bounded Output

If a bounded input produces an unbounded output:
- system is unstable

---

## Stable Drone
Returns to normal after disturbance.

## Unstable Drone
Oscillates uncontrollably or crashes.

---

# Types of Stability

## 1. Stable
Output settles properly.

## 2. Asymptotically Stable
Output gradually reaches equilibrium.

## 3. Marginally Stable
Oscillates continuously without increasing.

## 4. Unstable
Oscillations grow over time.

---

# 8. Important Performance Terms

---

# Transient Response

Initial behavior immediately after input changes.

### Example
Drone suddenly commanded to climb.

---

# Steady-State Response

Final behavior after the system settles.

### Example
Drone maintaining constant altitude.

---

# Overshoot

Amount by which output exceeds target value.

### Example
Desired altitude = 10 m  
Drone rises to 12 m first.

---

# Settling Time

Time required for output to remain near desired value.

Lower settling time means:
- faster stabilization

---

# Damping

Controls oscillations.

- Low damping → oscillatory
- High damping → sluggish response

---

# Robustness

Ability to handle:
- wind
- disturbances
- sensor noise
- parameter changes

---

# Sensitivity

Measures how much output changes due to disturbances.

Lower sensitivity is preferred.

---

# 9. Drone Applications of Control Systems

---

# 1. Attitude / Orientation Control

Controls:
- Roll
- Pitch
- Yaw

Uses:
- IMU
- Gyroscope
- Accelerometer

Usually implemented using PID controllers.

---

# 2. Position / Navigation Control

Uses:
- GPS
- Magnetometer
- Visual odometry

Functions:
- waypoint navigation
- path following

---

# 3. Stability Augmentation

Counters disturbances using fast feedback loops.

---

# 4. Velocity Control

Maintains desired speed and direction.

---

# 5. Obstacle Avoidance & Path Planning

Uses:
- Cameras
- LiDAR
- Sensor fusion

### Common Algorithms
- Artificial Potential Fields
- Model Predictive Control (MPC)
- Reactive obstacle avoidance

---

# 6. Payload Stabilization

Keeps cameras or sensors stable during flight.

### Example
Gimbal stabilization.

---

# 7. Mission Management

Handles:
- autonomous missions
- route execution
- task scheduling

---

# 8. Auto Takeoff and Landing

Drone autonomously:
- takes off
- stabilizes
- lands safely

---

# 9. Formation Control

Multiple drones coordinate together.

### Methods
- Consensus algorithms
- Distributed control

### Applications
- Drone swarms
- Surveillance
- Drone light shows

---

# Final Intuition

A control system continuously tries to minimize the difference between:
- Desired output
- Actual output

using:
- Sensors
- Feedback
- Controllers
- Actuators

This feedback loop allows drones to fly stably, accurately, and autonomously.