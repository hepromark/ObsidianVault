
## Applications of Humanoids AI

Functional humanoids:
- Social humanoids
	- Residential Assistance
	- Social services, activities, engagement
	- Educators / inquiries / Q&A positions
- Industrial humanoids
	- Manufacturing
- Research humanoids
	- HRI research
	- Space exploratory

## Functional & Non-functional Specifications
### Functional Specifications

Defines:
- how an AI humanoid looks like
- what it does & how it implements tasks
- what structures, senses, behaviours to implement

No humanoid satisfies all functional specs, but at least a sub-set such that they have mech/elec/bio designs with human-like features

### Nonfunctional Specifications

More like expectations of humanoid robots:
- Safe & secure actions
- Trustful & responsible for their actions
- Transparent & explanable outputs
- Empathetic & rational during teaming
- Compliant, legal and ethical

1. Human Likeness Satisfcation
2. Humanoid Capability Maturity
3. Humanoid Performance measurement
4. Humanoid impact estimation

# Technical System Design

## Mechanical Systems

Most important system
- dual role: enables practical interactions and research platform for bipedal locomotion / other research goals
- Important parameters: DOF, weight, size, power supply & sensors
- Manufacturing techniques: Laser cut, CNC, 3D printing

### Kinematics, Dynamics & Biomechanics

Humanoids are complicated: high DOF order, interconnected joints, non-linear attributes
- Kinematics & dynamics hard to solve
- No fixed reference points & collision potential during motion
- Classical methods (ex: analytical solutions to inverse kinematics) usually not good; difference in irl vs. theory
- Other avenues usually: neural networks, genetic algorithms, fuzzy logic

### Processing Units


## Control of Humanoids

### Different Control Methods

Traditional Methods: 
- ZMP-based, dynamical model-based
- Good for stability but doesn't apply to environments w/ obstacles

Optimization-based methods:
- PSO, CFO, MPC, TO
- Frames challenges as a real-time optimization
- Modern advances in MPC & neural nets makes this much better, enables things like:
- Contact planning, terrain adapation, balancing strategy, sensor integration, online perception

Model-based methods:
- Balance strategy, Real-time feedback, VGC
- Uses dynamic models of robots & environment to compute optimal control commands, but poor applicability

Bionic methods:
- CMAC, CPG

Learning-based control:
- Deep RL

### Control Domains

1. Locomotion Control
	- Bipedal motion is still a big challenge
	- Humanoids have legged movements, bipedal gait
	- Goal: omnidirectional walking on various speeds & terrains
	- Open-loop or close-loop walking engines
	- Big area of research
2. Balance and stability control
	- To use minimal energy during motion planning, balance & stability is important to consider
	- Research on balance control uses IMU & vision
	- Many stability criterias apply: 
		- Position of COM, COP
		- Zero Moment Points
3. Behavioural control
	- Autonomous, semi-autonomous & remote control methods
	- Complex tasks: path planning, footprint planning, nav
	- Navigating complex environments needs:
		- Self-collision detection
		- Path-planning
		- Obstacle avoidance

### Actuators

Electric actuators like brushed & brusheless DC motors good precise control & torque

Pneumatic actuators: muscular mechanisms, gives robot with lightweight & adaptable motion

Hydraulic actuators: good for demanding tasks, but needs fluid & intricate control systems

Cable-driven actuators:
- Expansive & agile motion
- Needs to consider friction & more complex kinematics








