#### Objective 
To make a ball in free-fall bounce upon hitting the ground.
#### Physics Model
We use the equations of motion under the influence of gravity to model the motion.
- $v = v_0 - g t$
- $h = h_0 + v t$

#### Simulation 
1) We update the velocity at every small time interval $dt$, and use that to find the height displaced during that interval. 
2) We run this loop till the time the height reaches zero. After that we make the following changes to the initial parameters:
   - change the direction of height opposite to the previous direction $\rightarrow$ $h = - h$
   - reduce magnitude of velocity and change its direction opposite to the initial direction. $\rightarrow$ $v = -0.5v$ 
   These two changes add that bouncing effect when the ball hits the ground.
![[Recording_05_06_2026__08_26_pm.mp4]]
