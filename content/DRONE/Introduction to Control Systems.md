A **control system** is a device or group of devices used to **manage, command, direct, or regulate** the behavior of another system.

### 1. Basic Definitions

#### System
A **system** is anything that:
1. Takes an input
2. Processes it
3. Produces an output
Example:
- Input → joystick command
- System → drone
- Output → drone movement

#### Plant / Process
The **plant** is the actual object being controlled.
Example:
In drones:
- Motors
- Propellers
- Drone body dynamics

together form the plant.

#### Signal
A **signal** carries information.
Examples:
- Sensor readings
- GPS coordinates
- Motor commands
- Gyroscope values

Signals usually vary with time.

### 2. Open-Loop vs Closed-Loop Systems

#### Open-Loop System
In an open-loop system:
- Output is not measured
- No feedback exists
- System cannot correct errors

The workflow looks like this - 
```text
Input → Controller → System → Output
```

Example:
A drone spins motors for 5 seconds regardless of:
- wind disturbance
- battery condition
- actual lift-off

#### Closed-Loop System (Feedback System)

A closed-loop system continuously checks the output and corrects errors.

1. Desired output is given
2. Sensors measure actual output
3. Difference is called error
4. Controller reduces the error

Example: Drone Attitude Hold

Suppose:
- Desired roll angle = 0°
- Wind tilts drone by 10°

Sensors detect tilt and the controller adjusts motor speeds to restore balance. It is self correcting and more accurate.

### 3. Feedback in Control Systems

Feedback means taking part of the output and feeding it back into the system input.

**Negative Feedback:** Subtracts the error from the input. It reduces overall error and stabilizes the system.
**Positive Feedback:** Adds the error back in the input. Rarely used in drones.

### 4. Feedback Control System Components

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

#### Comparator
Calculates error:
$$
e(t) = r(t) - y(t)
$$

Where:
- $r(t)$ = desired output
- $y(t)$ = actual output
- $e(t)$ = error

#### Controller $C(s)$
Determines how to reduce error. Some common controllers are -
- P Controller
- PI Controller
- PID Controller

#### Actuator
Converts electrical signals into physical movement. 

#### Plant $G(s)$
The system being controlled. Like the drone itself.

#### Sensor
Measures system output. For example -
- IMU
- Gyroscope
- Accelerometer
- GPS
- Barometer

### 5. System Representation Methods

There are various methods which is used to represent the model mathematically.

#### Differential Equation Representation
Represents system behavior in the time domain.
Example:

$$
m\ddot{x} + b\dot{x} + kx = F(t)
$$

Where:
- \(m\) = mass
- \(b\) = damping
- \(k\) = stiffness

This DE represents the motion of a spring block system.

#### Transfer Function Representation
But differential equations are hard to solve, so we convert them into algebraic function for the easy of solving.
General Form:
$$
G(s) = \frac{Output(s)}{Input(s)}
$$


# State Space Representation

Represents systems using:
- State variables
- Matrices
For example:
$$
\dot{x} = f(x, u)
$$


### 6. Stability

A control system must remain stable. If a bounded input produces an unbounded output then the
system is unstable.

**Types of Stability:**

1. Stable: Output settles properly.
2. Asymptotically Stable: Output gradually reaches equilibrium.
3. Marginally Stable: Oscillates continuously without increasing.
4. Unstable: Oscillations grow over time.


