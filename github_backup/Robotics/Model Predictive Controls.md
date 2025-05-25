### Overview
Control technique where control actions minimize a cost function for a constrained dynamical system over a finite, receding, horizon. It usually contains:

- An internal dynamic model of the plant / process
- A cost function J over the receding horizon
- An optimization algorithm minimizing the cost function _J_ with control input _u_

![[Pasted image 20250107224518.png]]

Simplified breakdown of what happens at each timestep:
- MPC controller receives or estimates current state of the plant
- MPC controller calculates a sequence of control actions that minimize the cost over the chosen time horizon by:
	- Solving a constrained optimization problem based on an internal plant model
	- Internal plant model depends on current state & physical equations that describes the plant behaviour
- MPC controller sends out the first control output of the 'optimal control' list
- Controller repeats last 3 steps at each time-step (recalculates the remaining items of the optimal controls list, only ever output the first element)

Watonomous Car Example:
- The car’s change in state (and hence future state) can be defined as a function of
    - The current car state (position, velocity, acceleration, etc…) at this `t0`
    - A control input at this at this `t0`
- For MPC team, this function is actually a sum of two functions where:
    - `f_F` is the “First principles” model using traditional dynamics equations
    - `f_D` is the data-driven model using neural networks and real training data
	
	![[Untitled (5).png]]
	
- Constraining conditions (max speed on this road, max acceleration possible for this car, the curb, lanes, etc…) considered inside the Loss Function
    - Loss Function is hand engineered by us the team members
    - We want high loss when car does bad stuff, low loss when car moves the way we want to
    - This loss function is slightly different depending on the car’s current state, so every-time the car state changes this function needs to be optimized again differently
- This Loss Function is minimized via finding the “best” control input `u`
    - Ex: We want to get from A → B 2 meters away
    - The most optimal input at `t0` is to accelerate because it minimizes the cost function via reducing the distance

When cost function is quadratic (i.e. Squared loss) & plant is linear without constraints & horizon tends to infinity:
- MPC equivalent to a linear-quadratic regulator (LQR) control

### WATOnomous MPC Design Choices
Why use a MPC control algorithm instead of simple PIDs?
- MPC has ability to “anticipate future events” & take control actions accordingly
- PIDs bad at large time delays & high-order dynamics

Non-linear MPC:
- MPC that uses non-linear model to directly control the application
- Empirical data fit (i.e. neural net) or high-fidelity dynamic model
- Non-linear model can be linearized to get a Kalman filter or specify a model for linear MPC

What we do at MPC:
- Implementing MPC using a bicycle model as dynamics model to represent our system
- Using a data-driven model (neural net) trained using physical parameters of the car
- We currently working on implementing [[Kinematics Bicycle Model]] in sims & collecting data for the data-driven model