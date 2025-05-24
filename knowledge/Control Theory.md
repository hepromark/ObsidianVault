### Overview

Control theory deals with the control of dynamical systems in engineered processes & machines.

Objective is to develop a model / algorithm that *controls* the system inputs to drive the system to a desired state, while minimizing delay, overshoot, or steady-state error & ensuring a level of control stability. 

To achieve the objective, a *controller* is needed:
- monitors controlled process variable PV
- compares PV with the set point SP
- the error (SP -PV) applied as feedback inside the controller to generate a control action to bring PV back to setpoint

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
