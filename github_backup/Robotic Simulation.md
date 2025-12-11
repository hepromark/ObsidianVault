# Why use Simulators?

**Cheap, fast, and safe testing environment to develop in**
Cheap:
- Cheaper to crash simulation robots
- No need for SREs / QAs / people to manually setup robots for a big test
Fast:
- Enables faster running than real time, discrete time etc.
- Lower setup times
Safe:
- Can simulate dangerous / abnormal conditions
- Failure testing without risks
Consistency:
- Replicable and reproducible testing conditions for bugs / algorithm verification. 

Can use it rapid iterative testing (ex: a black box optimization function) for development.

**Synthetic Data**
- Generate large volumes of training data cheaply

Usually, robotic simulators interface at the ROS layer:
![[Pasted image 20250630230520.png]]

# How do Physics Simulators Work?

These engines simulate complex dynamics without needing custom equations of motions for each robot configurations by representing robots as assemblies of rigid bodies connected by joints.

### Fundamental Concepts

At the core of physics simulation are math models that characterises movements and interactions

1. Generic Rigid-Body Framework
	Modern physics engines treat all objects as rigid bodies, characterized by mass, inertia tensors, and geometry.
	Uses Newton-Euler equations to describe translational and rotational motion of rigid bodies under applied forces/torques

2. Constraints and Joints instead of analytical equations of motions
	Engines define robotic assemblies of multiple rigid bodies connected by joints and constraints
	Doesn't use robot-specific analytical models

	The engine then sets up a large system of equations representing:
	- Motion of each body
	- Constraints imposed by joints and contacts (ex: wheels on ground, limbs connected to torso, etc.)

3. Numerical Integration Methods
	Physics engines uses numerical integration techniques to solve DEs governing dynamics
	Classical RK4 methods possible, but modern engines use semi-implicit or sympletic integrators
	Optimized for speed in real-time and stability
	
	Common techniques:
	- Explicit Euler / Semi-explicit Euler
	- Verlet integration or sympletic methods
	- Constraint solvers: ex. Projected Gauss-Seidel
	
	Modern engines usually mix and match these.

4. Defining Robot Models with URDF
	How do we pass in robot-specific info (parameters) to simulators?
	Usually via *URDFs* - Unified Robot Description Format, an XML based language
	
	Links: Define the rigid bodies, incl. geometry, mass, inertia
	Joints: Specify the connections between links, including joint type, axis of rotation, translation, limits, etc.
	Sensors and Actuators: Describe additional components and their properties

### Examples of Physics Simulators
[[Gazebo]] -> Open source robotic sim that integrates with ROS. Uses physics engines (ex: Open Dynamics Engine) to simulate interactions

[[Issac Lab]] -> Developed by NVIDIA, is a high-performance simulator designed for learning. Uses GPU to do physics sims, to do massively parallelised number of environments.

[[AirSim]] -> Built on UE for drones / autonomous vehicles

[[MuJoCo]]

[[PyBullet]]

[[OpenAI Gym]]
