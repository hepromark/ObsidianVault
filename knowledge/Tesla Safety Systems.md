### TCS - Traction Control System

**How it works**
- Prevents wheel slip during acceleration
- Detects if a wheel is spinning faster than others, then counter it:
	- Reduce engine power
	- Braking the spinning wheel
- Default ON unless going on special terrain where wheels expected to slip

**Sub-modes (Model Y)**
- Slippery surface: distributes traction evenly across all tires to provide more traction & stability in snow/ice/etc.
- Off-road Assist: more gradual torque from motors, enables scenarios where one side of vehicle loses traction

**Sub-modes (Model 3)**
- Slip-start: Wheels can slip

### ABS - Anti-lock Braking System

**How It Works**
- Prevents wheel from locking up during hard braking and sliding
- Sliding friction < rolling + regular brakes
- ABS controller reads all wheel speed sensors
	- if one wheel slows down much more than other wheels, ABS activates and reduces brake pressure to wheel
	- Pulsing up & down of ABS activating to reduce brake pressure, and the driver holding down to increase brake pressure

### ESC - Electronic Stability Control

**How It works**
- Looks at wheel speeds during steering to reduce understeering / oversteering
- ESC uses ABS hardware (ex: individual brake actuators) and TCS logic (ex: modulate torque)

**Example Activation**

1. Oversteer w/ rear of car swinging out during a right turn
	- Yaw sensor detects car rotating more than expected
	- ESC calculates *oversteering*
	- ESC uses ABS to brake front-left wheel
		- Counteracting torque pulls car back
		- May reduce rear wheel power w/ TCS

2. Understeer while turning right
	- Yaw sensor detects car rotating less than expected
	- ESC uses ABS to brake the right front wheel to pivot the car into the turn
	- Use TCS to reduce power to front wheels & restore grip

### Regenerative Braking

While car moving and foot off accelerator:
- Regenerative braking slows down vehicle & feeds surplus power back into battery


### Random Tidbits
> Installing winter tires with aggressive compound and tread design may result in temporarily-reduced regenerative braking power. However, your vehicle is designed to continuously recalibrate itself, and after changing tires it will increasingly restore regenerative braking power after some straight-line accelerations. For most drivers this occurs after a short period of normal driving, but drivers who normally accelerate lightly may need to use slightly harder accelerations while the recalibration is in progress. Touch Service > Wheel & Tire > Tires to select winter tires and quicken this process.

