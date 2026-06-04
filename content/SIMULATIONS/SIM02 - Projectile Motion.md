#### Objective
I made this simple simulation in Octave to simulate the trajectory of a projectile. It takes the velocity i.e. $v_{0}$ and the angle of elevation, $\theta$, as an inputs to show the path travelled by the object.

#### Physics Model
A 2D motion in physics is dealt by breaking it down into two motions in 1 dimension that is the vertical motion in the $y$ axis and the horizontal motion in $x$ axis using the equations of kinematics under constant gravitational acceleration. Thus we get:
- $x(t) = v_x t$ 
- $y(t) = v_y t - \frac{1}{2}gt^2$ 

where $v_x$ and $v_y$ are the horizontal and vertical component of the velocity vector.

#### Simulation
We first initialize the physical parameters that is v_0 and \theta. Then we run a loop that - 
1) computes projectile position
2) renders trajectory and projectile
3) updates display and clears the previous path
4) repeats until the object hits the ground

#### Examples 
Here is what we get with the input $v_0 = 20,  \theta=45\degree$ 
![[Pasted image 20260602132707.png]]

Lets see $\theta = 55\degree$ 
![[Pasted image 20260603130032.png]]

#### Error Note
I faced an infinite loop issue that made the animation never stop. This was because I used the vector incorrectly in the position calculation. The problematic code was - 
```Octave
x = v_x * t(1:i);
y = v_y * t(1:i) - 0.5*g*t(1:i).^2;
```

**t(1:i)** produce vector instead of a scalar positions causing the loop termination condition to behave incorrectly. The fix for this was -
```Octave
x = v_x * t(i);
y = v_y * t(i) - 0.5*g*t(i).^2;
```

#### Future Improvements
1) Add air resistance
2) Add bounce when colliding with ground (see [[SIM03 - Bouncing Ball]] )
3) Make a gif from octave
4) Add a noisy sensor and KF estimation (see [[SIM04 - Projectile State Estimation]])