# Kalman Filtering

## Core Idea

In many engineering systems (drones, aircraft, robots, missiles), the true internal state cannot be measured directly.

Instead, we have:

1. A mathematical model that predicts how the system evolves.
2. Sensors that provide noisy and indirect observations.

The Kalman Filter combines both to produce the best estimate of the system state under linear-Gaussian assumptions.

---

# System Model

## Continuous-Time System

State Equation:

$$
\dot{x}=f(x,u)
$$

Measurement Equation:

$$
z=h(x,u)
$$

where:

- $x$ = state vector
- $u$ = control input
- $z$ = measurement vector

Example:

$$
x=
\begin{bmatrix}
\text{position}\\
\text{velocity}
\end{bmatrix}
$$

---

## Linearized System

After linearization around an operating point:

$$
\dot{x}=Ax+Bu
$$

$$
z=Hx
$$

where:

- $A$ = state transition matrix
- $B$ = control matrix
- $H$ = observation matrix

---

# Discrete-Time Kalman Filter Model

The Kalman Filter operates in discrete time.

## Process Equation

$$
x_k = A x_{k-1} + B u_{k-1} + w_{k-1}
$$

## Measurement Equation

$$
z_k = H x_k + v_k
$$

where:

- $x_k$ = state vector
- $u_k$ = control input
- $z_k$ = measurement

Process noise:

$$
w_k \sim \mathcal N(0,Q)
$$

Measurement noise:

$$
v_k \sim \mathcal N(0,R)
$$

---

# Meaning of Q and R

## Process Noise Covariance Q

Represents uncertainty in the mathematical model.

Sources:

- Wind disturbances
- Unmodeled dynamics
- Parameter errors
- Numerical approximations

Large $Q$:

> "I don't trust my model."

Small $Q$:

> "My model is highly accurate."

---

## Measurement Noise Covariance R

Represents sensor uncertainty.

Sources:

- GPS noise
- Accelerometer noise
- Gyroscope noise
- Quantization errors

Large $R$:

> "Sensor readings are unreliable."

Small $R$:

> "Sensor readings are trustworthy."

---

# State Estimate and Covariance

Kalman Filtering estimates two quantities:

State estimate:

$$
\hat{x}
$$

Uncertainty of estimate:

$$
P
$$

where

$$
P=
\text{Covariance of estimation error}
$$

---

## Interpretation of P

Suppose

$$
\hat{x}=100
$$

This estimate is meaningless without uncertainty.

Examples:

$$
100 \pm 0.01
$$

or

$$
100 \pm 20
$$

Both have the same estimate but very different confidence.

The covariance matrix $P$ quantifies that confidence.

---

# Kalman Filter Cycle

At every timestep:

```text
Predict
   ↓
Measure
   ↓
Update
   ↓
Repeat
```

---

# Step 1: Prediction (Time Update)

Use the system model to predict the next state.

## State Prediction

$$
\hat{x}_{k|k-1}
=
A\hat{x}_{k-1|k-1}
+
Bu_{k-1}
$$

Meaning:

> Predict where the system should be before seeing the new measurement.

---

## Covariance Prediction

$$
P_{k|k-1}
=
A P_{k-1|k-1} A^T
+
Q
$$

Meaning:

- Transform old uncertainty forward.
- Add process uncertainty.

Since uncertainty accumulates over time:

$$
P_{k|k-1}
>
P_{k-1|k-1}
$$

typically.

---

# Step 2: Update (Measurement Correction)

A new measurement arrives.

Kalman Filter compares:

Predicted measurement:

$$
H\hat{x}_{k|k-1}
$$

with actual measurement:

$$
z_k
$$

---

# Innovation

Innovation is the prediction error.

$$
y_k
=
z_k
-
H\hat{x}_{k|k-1}
$$

Interpretation:

$$
\text{Innovation}
=
\text{Measurement}
-
\text{Prediction}
$$

---

### Small Innovation

Prediction was accurate.

---

### Large Innovation

Prediction was inaccurate.

---

# Innovation Covariance

Measures uncertainty in the innovation.

$$
S_k
=
H P_{k|k-1} H^T
+
R
$$

Large $S_k$ means the innovation is expected to vary significantly.

---

# Kalman Gain

The most important equation.

0

The Kalman Gain determines:

> How much should the filter trust the measurement versus the prediction?

---

## Interpretation

### Large Gain

$$
K_k \uparrow
$$

Trust measurement more.

Apply a large correction.

---

### Small Gain

$$
K_k \downarrow
$$

Trust model more.

Apply a small correction.

---

# State Update

Correct the prediction using the innovation.

$$
\hat{x}_{k|k}
=
\hat{x}_{k|k-1}
+
K_k y_k
$$

Expanded:

$$
\hat{x}_{k|k}
=
\hat{x}_{k|k-1}
+
K_k
\left(
z_k
-
H\hat{x}_{k|k-1}
\right)
$$

Interpretation:

$$
\text{New Estimate}
=
\text{Prediction}
+
\text{Correction}
$$

---

# Covariance Update

After incorporating the measurement:

$$
P_{k|k}
=
(I-K_kH)
P_{k|k-1}
$$

Meaning:

After receiving information from sensors, uncertainty decreases.

Therefore:

$$
P_{k|k}
<
P_{k|k-1}
$$

typically.

---

# Effect of Process Noise Q

Recall:

$$
P_{k|k-1}
=
A P_{k-1|k-1} A^T
+
Q
$$

Increasing $Q$:

$$
Q \uparrow
$$

causes

$$
P_{k|k-1}\uparrow
$$

which causes

$$
K_k \uparrow
$$

Result:

> Trust measurements more.

---

Summary:

$$
Q\uparrow
\Rightarrow
P\uparrow
\Rightarrow
K\uparrow
$$

---

# Effect of Measurement Noise R

Kalman Gain contains:

$$
(HP_{k|k-1}H^T + R)^{-1}
$$

Increasing $R$:

$$
R\uparrow
$$

causes

$$
K_k\downarrow
$$

Result:

> Trust measurements less.

---

Summary:

$$
R\uparrow
\Rightarrow
K\downarrow
$$

---

# Drone Example

Consider a quadrotor.

State vector:

$$
x=
\begin{bmatrix}
x\\
y\\
z\\
v_x\\
v_y\\
v_z
\end{bmatrix}
$$

Sensors:

- IMU
- GPS
- Magnetometer
- Barometer

Prediction:

- Uses equations of motion.
- Integrates IMU measurements.

Correction:

- GPS corrects position.
- Barometer corrects altitude.
- Magnetometer corrects heading.

This continuous fusion process allows the drone to estimate its true state.

---

# Why Kalman Filters Work

A Kalman Filter continuously balances:

1. What physics predicts.
2. What sensors observe.

using uncertainty information.

The entire filter can be summarized as:

> Predict using physics, compare with measurements, then blend the two according to their respective uncertainties.

---

# Limitations of Kalman Filters

1. Assumes linear dynamics.
2. Assumes Gaussian noise.
3. Requires a reasonably accurate system model.
4. Sensitive to incorrect tuning of $Q$ and $R$.
5. Performance depends on initial estimates.
6. Computational cost increases for large state vectors.

---

# Extensions

## Extended Kalman Filter (EKF)

Used for nonlinear systems.

Linearizes the system around the current estimate.

Widely used in drones and robotics.

---

## Unscented Kalman Filter (UKF)

Avoids explicit linearization.

Uses sigma points to propagate uncertainty.

More accurate than EKF for strongly nonlinear systems.

---

## Error-State Kalman Filter (ESKF)

Most common in modern drone flight controllers.

Tracks only estimation errors.

Provides improved numerical stability for attitude estimation.

Used in PX4 and ArduPilot.