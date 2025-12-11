### Overview

Control theory deals with the control of dynamical systems in engineered processes & machines.

Objective is to develop a model / algorithm that *controls* the system inputs to drive the system to a desired state, while minimizing delay, overshoot, or steady-state error & ensuring a level of control stability. 

To achieve the objective, a *controller* is needed:
- monitors controlled process variable PV
- compares PV with the setpoint SP
- the error (difference between PV and SP) applied as feedback inside the controller to generate a control action to bring PV back to setpoint

# Basics
### Open vs. Closed loop control

Fundamentally there are 2 types of control loops: 
1. Open-loop control (feedfoward)
2. Closed-loop control (feedback)

In open-loop control:
- Control action is independent from the PV
- Ex: turning a boiler on then off for a set period of time

In closed-loop control:
- Control action dependent on PV
- Ex: having a thermostat 
- Common beginner control system with closed loop is a [[PID]]. 

# Intermediate
[[Intermediate Control Theory]]
System Modelling

Stability & Frequency Response Analysis

State-space & Pole Placement

MPC
### Things to study:
- Open loop vs. feedback vs. feedfoward
- Linear, time invariant definition
- State spaces
- Mass-spring damper -> state space conversions
- Block diagrams
	- For bicycle model
- Cruise control example
	- Note: bigger gears = slower speed = more torque
	- Smaller gears = faster speed = less torque
- Lyapunov stability definition
- Routh-Hurwitz Criterion

