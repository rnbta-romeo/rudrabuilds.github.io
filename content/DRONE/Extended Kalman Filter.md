# Extended Kalman Filter (EKF)

The **Extended Kalman Filter (EKF)** is an advanced version of the standard Kalman Filter used for estimating the state of **non-linear systems**.

It is widely used in:

- Robotics
- Drones
- Aerospace systems
- GPS navigation
- Autonomous vehicles

---

# Why Do We Need EKF?

The standard Kalman Filter works only for **linear systems**.

But most real-world systems are actually **non-linear**.

For example:

- Drone motion
- Robot rotation
- Aircraft dynamics
- Radar tracking

If we directly apply a normal Kalman Filter to a non-linear system, the estimates become inaccurate and may eventually diverge.

The EKF solves this problem by:

> Converting the non-linear system into an approximately linear system around the current estimate.

This process is called **linearization**.

---

# Linear vs Non-Linear Systems

Understanding this difference is the key to understanding EKF.

---

# 1. Linear System

A system is linear if:

- Variables are not multiplied together
- No powers like $x^2$
- No trigonometric functions like $\sin(x)$
- The graph is a straight line

General form:

$$
y = mx + c
$$

Example:

$$
y = 2x + 3
$$

If:

$$
x = 1
$$

then:

$$
y = 5
$$

If:

$$
x = 2
$$

then:

$$
y = 7
$$

The relationship changes uniformly.

---

## Example: Linear Motion

Suppose a car moves with constant velocity:

$$
x_{k+1} = x_k + vt
$$

Where:

- $x_k$ = current position
- $v$ = constant velocity
- $t$ = time step

This is linear because the variables are not squared or inside functions.

---

# 2. Non-Linear System

A system becomes non-linear if it contains:

- Powers like $x^2$
- Products like $xy$
- Trigonometric functions
- Exponentials
- Logarithms

Example:

$$
y = x^2 + 3
$$

or

$$
y = \sin(x)
$$

The output no longer changes uniformly.

---

## Example: Drone Rotation

Suppose a drone rotates with angle $\theta$:

$$
x_{k+1} = x_k + v\cos(\theta)t
$$

$$
y_{k+1} = y_k + v\sin(\theta)t
$$

This is non-linear because of:

$$
\sin(\theta), \cos(\theta)
$$

A standard Kalman Filter cannot directly handle this properly.

This is where EKF becomes useful.

---

# Core Idea of EKF

EKF approximates a non-linear system as linear **only near the current estimate**.

It uses:

- Taylor Series Approximation
- Jacobian Matrices

to convert the system into a local linear model.

---

# Non-Linear System Equations

Instead of matrix equations used in normal Kalman Filters, EKF uses non-linear functions.

---

# 1. State Transition Equation

Describes how the state changes over time.

$$
\dot{x} = f(x, u)
$$

Where:

- $x$ = state vector
- $u$ = control input
- $f$ = non-linear state function

---

## Example

For a moving robot:

$$
x_{k+1} =
\begin{bmatrix}
x_k + v\cos(\theta)t \\
y_k + v\sin(\theta)t \\
\theta_k + \omega t
\end{bmatrix}
$$

Where:

- $v$ = velocity
- $\omega$ = angular velocity

This model is non-linear because of sine and cosine terms.

---

# 2. Measurement Equation

Relates sensor measurements to the system state.

$$
z = h(x, u)
$$

Where:

- $z$ = sensor measurements
- $h$ = non-linear measurement function

---

## Example

A radar measuring distance:

$$
z = \sqrt{x^2 + y^2}
$$

This is non-linear because of:

$$
\sqrt{x^2 + y^2}
$$

---

# Linearization Using Jacobians

EKF linearizes the system using **Jacobians**.

A Jacobian is a matrix of partial derivatives.

It tells us how the non-linear function changes near the current estimate.

---

# State Jacobian

$$
F_k = \frac{\partial f}{\partial x}
$$

This linearizes the state transition function.

---

## Example

Suppose:

$$
f(x) =
\begin{bmatrix}
x + v\cos(\theta)t \\
y + v\sin(\theta)t
\end{bmatrix}
$$

Then the Jacobian becomes:

$$
F =
\begin{bmatrix}
1 & 0 & -v\sin(\theta)t \\
0 & 1 & v\cos(\theta)t \\
0 & 0 & 1
\end{bmatrix}
$$

---

# Measurement Jacobian

$$
H_k = \frac{\partial h}{\partial x}
$$

This linearizes the measurement function.

---

## Example

For:

$$
h(x,y) = \sqrt{x^2 + y^2}
$$

The Jacobian is:

$$
H =
\begin{bmatrix}
\frac{x}{\sqrt{x^2+y^2}} &
\frac{y}{\sqrt{x^2+y^2}}
\end{bmatrix}
$$

---

# EKF Algorithm

The EKF works in two repeating stages:

1. Prediction
2. Correction (Update)

---

# 1. Prediction Step

The filter predicts the next state before receiving sensor data.

---

## State Prediction

$$
\hat{x}_{k|k-1} = f(\hat{x}_{k-1}, u_k)
$$

Where:

- $\hat{x}_{k|k-1}$ = predicted state
- $f$ = non-linear transition function

---

## Covariance Prediction

$$
P_{k|k-1}
=
F_{k-1} P_{k-1} F_{k-1}^T + Q_{k-1}
$$

Where:

- $P$ = covariance matrix
- $Q$ = process noise covariance
- $F$ = state Jacobian

This predicts uncertainty in the estimate.

---

# 2. Correction (Update) Step

Now sensor measurements are used to correct the prediction.

---

# Kalman Gain

$$
K_k
=
P_{k|k-1} H_k^T
(H_k P_{k|k-1} H_k^T + R_k)^{-1}
$$

Where:

- $K_k$ = Kalman Gain
- $R_k$ = measurement noise covariance

The Kalman Gain decides:

> How much should we trust the sensor vs the prediction?

---

# State Update

$$
\hat{x}_k
=
\hat{x}_{k|k-1}
+
K_k
\left(
y_k - h(\hat{x}_{k|k-1})
\right)
$$

Where:

- $y_k$ = actual measurement
- $h(\hat{x}_{k|k-1})$ = predicted measurement

The difference:

$$
y_k - h(\hat{x}_{k|k-1})
$$

is called the **innovation** or **residual**.

It measures prediction error.

---

# Covariance Update

$$
P_k
=
(I - K_k H_k)P_{k|k-1}
$$

This updates the uncertainty after correction.

---

# Simple Intuition

EKF continuously performs:

1. Predict the future state
2. Measure the real world
3. Compare prediction and reality
4. Correct the estimate
5. Repeat

The important difference is:

> EKF handles non-linearity using local linear approximations.

---

# Limitations of EKF

## 1. Approximation Errors

EKF uses only a first-order approximation.

If the system is highly non-linear, accuracy decreases.

---

## 2. Jacobians Are Difficult

For complex systems, deriving Jacobians can become mathematically complicated.

---

## 3. Filter Divergence

Poor approximation can make the filter unstable.

---

## 4. Gaussian Assumption

EKF assumes:

- Gaussian noise
- Single-peaked distributions

It does not work well for multi-modal probability distributions.

---

# Alternatives to EKF

## Unscented Kalman Filter (UKF)

Uses sigma points instead of Taylor approximation.

Usually more accurate than EKF.

---

## Particle Filter

Uses many random samples (particles).

Works for highly complex systems.

---

# Summary

The Extended Kalman Filter:

- Extends the Kalman Filter to non-linear systems
- Uses Jacobians for local linearization
- Predicts and corrects estimates repeatedly
- Is widely used in robotics and navigation

EKF is one of the most important estimation techniques in modern engineering.