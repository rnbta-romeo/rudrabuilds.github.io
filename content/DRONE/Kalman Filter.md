
## The Problem

Imagine you're trying to track a moving car in thick fog.

You have two sources of information:

1. A mathematical model (physics)
2. A noisy sensor

The model predicts where the car should be.

The sensor tells you where it thinks the car is.

Unfortunately, both can be wrong.

The Kalman Filter combines them intelligently to obtain the best estimate.

---

## What Is The State?

The **state** is simply the information needed to describe the system.

For a moving object:

$x = [\text{position}, \text{velocity}]^T$

For a drone:

$x = [x,y,z,v_x,v_y,v_z]^T$

The goal of the Kalman Filter is to estimate the state.

---

## Why Not Just Use Sensors?

Sensors contain noise.

Examples:

- GPS drifts
- Accelerometers are noisy
- Gyroscopes drift over time

Using sensor data directly leads to inaccurate estimates.

---

## Why Not Just Use Physics?

The mathematical model is never perfect.

Examples:

- Wind gusts
- Changing battery voltage
- Unmodelled disturbances

Using only the model also leads to errors.

---

## The Core Idea

A Kalman Filter continuously answers:

> How much should I trust the model?
>
> How much should I trust the sensor?

---

# Mathematical Model

The system evolves according to:

$$
x_k = A x_{k-1} + B u_{k-1} + w_{k-1}
$$

where:

- $x_k$ = current state
- $A$ = system dynamics matrix
- $u_k$ = control input
- $B$ = control matrix
- $w_k$ = process noise

Think of this equation as:

> New State = Predicted State + Control Effect + Unknown Disturbances

---

## Process Noise

The term $w_k$ represents things that happen in reality but are not included in the model.

Examples:

- Wind
- Vibrations
- Modelling errors

We assume:

$w_k \sim \mathcal N(0,Q)$

where $Q$ is called the **process noise covariance**.

### Intuition

Small $Q$:

> I trust my model.

Large $Q$:

> I do not trust my model very much.

---

## Sensor Measurements

Sensors provide measurements according to:

$$
z_k = Hx_k + v_k
$$

where:

- $z_k$ = measurement
- $H$ = observation matrix
- $v_k$ = measurement noise

Think:

> Measurement = Reality + Sensor Error

---

## Measurement Noise

We assume:

$v_k \sim \mathcal N(0,R)$

where $R$ is the **measurement noise covariance**.

### Intuition

Small $R$:

> Reliable sensor.

Large $R$:

> Unreliable sensor.

---

# Uncertainty

The Kalman Filter doesn't just estimate the state.

It also estimates how uncertain it is.

This uncertainty is stored in the covariance matrix $P$.

Example:

Estimate A: $100 \pm 0.01$

Estimate B: $100 \pm 20$

Both estimates are 100, but the confidence is very different.

That confidence is represented by $P$.

---

# Step 1: Prediction

Using the model, predict the next state.

$$
\hat{x}_{k|k-1}
=
A\hat{x}_{k-1|k-1}
+
Bu_{k-1}
$$

Interpretation:

> Where do I think the system will be before seeing the new measurement?

---

## Predicting Uncertainty

The uncertainty is also propagated forward:

$$
P_{k|k-1}
=
A P_{k-1|k-1} A^T
+
Q
$$

Notice the $+Q$.

Every prediction introduces additional uncertainty.

---

# Step 2: Compare Prediction With Measurement

Suppose:

- Prediction = 15 m
- Sensor = 13 m

Difference = 2 m.

This difference is called the **innovation**.

$$
y_k
=
z_k
-
H\hat{x}_{k|k-1}
$$

Interpretation:

> Innovation = Measurement − Prediction

A large innovation means the prediction was inaccurate.

---

# Kalman Gain

The Kalman Gain determines how much the filter should trust the measurement.

$$
K_k
=
P_{k|k-1}
H^T
(HP_{k|k-1}H^T + R)^{-1}
$$

You do not need to memorize this equation.

The important idea is:

- Large $K$ → trust measurement more
- Small $K$ → trust prediction more

---

# State Update

The estimate is corrected using the innovation.

$$
\hat{x}_{k|k}
=
\hat{x}_{k|k-1}
+
K_k y_k
$$

Interpretation:

> New Estimate = Prediction + Correction

---

# Covariance Update

After receiving a measurement, uncertainty decreases.

$$
P_{k|k}
=
(I-K_kH)
P_{k|k-1}
$$

This means the filter becomes more confident after incorporating sensor information.

---

# Effect of Q

$Q$ controls trust in the model.

If $Q$ increases:

- Predicted uncertainty increases
- Kalman Gain increases
- Measurements are trusted more

Summary:

$Q \uparrow \Rightarrow K \uparrow$

---

# Effect of R

$R$ controls trust in the sensor.

If $R$ increases:

- Sensor becomes less reliable
- Kalman Gain decreases
- Model is trusted more

Summary:

$R \uparrow \Rightarrow K \downarrow$

---

# Drone Example

Prediction comes primarily from:

- IMU
- Motion equations

Correction comes from:

- GPS
- Barometer
- Magnetometer

The drone continuously performs:

```text
Predict
   ↓
Measure
   ↓
Correct
   ↓
Repeat
```

hundreds of times per second.

---

# One-Sentence Summary

A Kalman Filter is a mathematical method that continuously combines a noisy prediction and a noisy measurement, weighting each according to how much it trusts them.