#### What does this simulation do?
Estimate the height and velocity of a falling object using - 
- A physics model
- A noisy sensor
- A [[Kalman Filter]]

#### The Physics Model
The motion of an object freely falling under the influence of gravity is simply governed by the equations of kinematics i.e. -
1) $v_{final} = v_{initial} + a t$ 
2) $h_{final} = h_{initial} + vt$ 

and that's it! These two equations will be our model that tells us where the object ideally should be.

#### The Noisy Sensor
The sensors do not give 100% accurate data, there is always some discrepancies that are associated with it called noise. We assume that the noise is Gaussian for the sake of simplicity.
The plot below shows the true height according to our physics model and we also plot the faulty sensor data along side it. Notice the variation in both the curves - its HUGE! 
![[Pasted image 20260601122717.png|697]]

### The Kalman Filter
Here comes the magical part - the [[Kalman Filter]]. It strikes a very fine balance between a) how much should the model be trusted and b) how much should the sensor be trusted. 
It works in the following steps -
1) **Step 1 - Prediction**: calculates where the object should be according to the physics model. Lets call this $a.$
2) **Step 2 - Sensor:** the sensor data arrives and gives the perceived height of the object. Lets call this $b.$ The filter calculates the difference between the data in step 1 and 2, this difference is called the innovation $y$ i.e. $y = b - a$
3) **Step 3 - Kalman Gain:** it measures how much of the difference, that is, the innovation should I incorporate in my correction. Lets call this $K.$
4) **Step 4 - Correction:** after the Kalman gain is calculates, it updates the estimated prediction accounting for the corrections. If the current estimate is $x$ then after correction it becomes $x = x + K(b - a)$ 

This is how all those messy sensor inputs, coupled with the predictions from the physics model gives us a near perfect estimation of where the object is located in space and time. We get the following plot after the KF calculations -
![[Pasted image 20260601125821.png]]

> [!Note]
> I have used [Octave](https://www.octave.org) for the simulation and the complete source code can be found [here](https://github.com/rudraBuilds/Simulations/blob/main/freefall_state_estimation.m)
