F1Tenth Car uses **Planar Lidar**
- Distance to obstacle = (speed of light * time travelled) / 2
- Max range constrained by: Laser energy
	- As distances increase, signal-to-noise ratio falls
	- More and more noise until signal undetectable
- Energy of reflected ray depends on:
	- Divergence of laser
	- Bounce angle
	- Humidity
	- Reflectivity of target
	- Detector sensitivity

![[Pasted image 20250120234824.png]]

- Output is an array A[1080], where A[i] is the distance measurement of ith step
- `inf` and `nan` values represent ???


### Safety

How to quantify safety?
- We can have an indicator function: can output binary or continuous value
	- Modern day probably use Deep Learning, since more complex relationships needed (ex: moving fast not always dangerous, standing still but incoming car more scary)

#### Time-to-collision:
- TTC = time it would take for ego-vehicle and object to intercept, given that they maintain current movement (heading & velocity)

![[Pasted image 20250120235513.png]]

$\displaystyle TTC_i(t) = \frac{r_i(t)}{max(-\dot{r}_i(t), 0)}$
- TTC = Range between vehicle and object / rate of change of this range
- Only really defined when ego approaching object, if moving away TTC = inf

**Compute Range Rate**
- Range-rate = time derivative of distance = project velocity of vehicle onto vector between vehicle & object poses
$\displaystyle \dot{r_i} = v_xcos(\theta_i)$
- Assume that $v_x$ is forward speed in vehicle's reference frame, since we are not dealing w/ moving obstacles
- Theta is beam angle, i.e. angle between velocity vector and object


### Sample Interview Questions

1. Describe a feature which captures the relative pose of all vehicles in the vicinity of the car.

- Define a feature vector $\vec{v}$ containing relative poses of "all" vehicles in vicinity of the car:
	- $\vec{v}$ has some set max size, i.e. 5 cars -> 5 * 6 = 30 length
	- 4 because: [exists, x, y, vel x, vel y]
	- Can be in map-frame or relative frame
 - As an input to the neural net, if # of vehicles rises then its fine until limit is hit. 

- Data for the vehicle poses comes from 1) camera and 2) lidar. Detections *fused* together for one final prediction vector.