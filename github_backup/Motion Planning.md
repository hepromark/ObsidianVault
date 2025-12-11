### Overview

A computation problem to find a sequence of valid configurations that moves an object from start -> goal.

A basic motion planning problem is:
- compute a continuous path that connects starting config S and goal config G, while avoiding collisions with known obstacles.
- Both robot & obstacle is described in a 2D or 3D workspace.
- The motion is represented as a path in a configuration space.

### Describing the Problem

#### Work Space
- Just the 3D pose of the car (x,y,z,theta)
#### Configuration Space
- A configuration describes the pose of the robot
- The configuration space $C$ is the set of all possible configurations
#### Free Space
The set of configurations in $C$ that avoids collisions with obstacles is ca4 led the free space, $C_{free}$. 
- usually hard to explicitly compute the shape of $C_{free}$ 
- Testing whether a given configuration in $C_{free}$ is easier; ex: use [[forward kinematics]] and [[collision detection]]. 
#### Obstacle Space
Complement of free space $\complement C_{free}$ , where the configuration collides with obstacles.
- Invalid configurations that robot can't be in

Examples:

1. If the robot is a single point in 2D plane, then:
- the configuration can be represented by 2 parameters (x,y)
- $C$ is a plane

![[Pasted image 20250105063444.png]]


2. If the robot is a 2D shape that can translate & rotate:
- Workspace is a 2D plane still
- $C$ is the *special Euclidean group* $SE(2) = R^2 \times SO(2)$ 
- where $SO(2)$ is the special [[Orthogonal Group]] of 2D rotations
- $C$ is represented by 3 parameters $(x, y, \theta)$ 

![[Pasted image 20250105063503.png]]

3. If the robot is a solid 3D shape that can translate and rotate:
- Workspace is 3D
- $C$ is the special Euclidean group $SE(3) = R^3 \times SO(3)$ 
- $C$ requires 6 parameters: $(x, y, z)$ and Euler angles $(\alpha, \beta, \gamma)$ 

5. If the robot is a fixed-base manipulator with N revolute joints (no closed loops):
- C is N-dimensional

#### Target Space
A subspace of free space which denotes where we want the robot to move to.
- Global motion planning: target space observable by robot's sensors
- Local motion planning: robot can not observe the target space in some states. 
	- robot may go through several *virtual target spaces*, each located in the observable area around the robot.

### Algorithms

**Completeness**: Algorithms are complete if
- Terminate in finite time
- Returns solution if once exists / returns failure otherwise

**Optimal**: Algorithms are optimal if:
- Returns the minimum cost solution when a solution exists

There exists complete algorithms for motion planning, but:
- Not practical (ex: exponential time complexity)
- Real algorithms relaxes completeness guarantees somehow

The core idea is:
- We want to convert the *formal problem* into a *search problem*
- i.e. create representations of the actual problem, then use search as the last step to find a solution
- We want to "convert the *formal problem* into a *search problem*"


### Classical Motion Planning Algorithms

Low dimensional problems can be solved with grid-based algorithms or geometric algorithms.
High dimension problems may be computationally intractable:
- Potential-field algorithms effect but affected by local minima
- Sampling-based algorithms more promising (they are also currently considered SOTA) and apply well to higher (i.e. hundreds) dimensional problems
#### Grid-based & Geometric
- **A***: Classic graph search, good for low-dimensional, discrete spaces.
- **Dijkstra’s Algorithm**: Like A*, but without heuristics.
- **D*** and **D*-Lite**: Dynamic replanning, useful for environments that change.
- **Lattice Planner**: Discretizes the configuration space, often used for car-like robots.
#### Sampling-based
- **RRT (Rapidly-exploring Random Tree)**: Efficiently explores high-dimensional spaces, good for car-like robots.
- **RRT***: Asymptotically optimal version of RRT.
- **PRM (Probabilistic Roadmap)**: Good for multi-query problems in static environments.
#### Potential Field
- **Artificial Potential Fields**: Simple, but can get stuck in local minima.

### 2. Kinodynamic Motion Planning

- **Kinodynamic RRT / RRT***: Considers vehicle dynamics (acceleration, turning radius, etc.), crucial for realistic RC car motion.
- **Model Predictive Control (MPC)**: Optimizes a trajectory over a short horizon, considering dynamics and constraints.

### 3. Reinforcement Learning (RL)-based Motion Planning

#### Model-free RL
- **Deep Q-Networks (DQN)**: For discrete action spaces.
- **DDPG (Deep Deterministic Policy Gradient)**: For continuous control, suitable for steering/throttle.
- **SAC (Soft Actor-Critic)**: State-of-the-art for continuous control, stable and sample-efficient.
- **PPO (Proximal Policy Optimization)**: Robust, widely used in RL for robotics.

#### Model-based RL
- **MBPO (Model-Based Policy Optimization)**: Combines model learning with policy optimization for sample efficiency.

#### Imitation Learning
- **Behavioral Cloning**: Learn from expert demonstrations.
- **DAgger (Dataset Aggregation)**: Iteratively improves by querying an expert.

### 4. Hybrid Approaches
- **RL + Classical Planner**: Use RL for local planning or policy refinement, with a classical planner for global pathfinding.
- **Learning-based Sampling**: Use RL to bias sampling in RRT or PRM.

---

 Suggested Learning Path
1. **Understand classical planners**: A*, RRT, Lattice Planner.
2. **Implement a simple RRT or Lattice Planner** for your RC car’s kinematics.
3. **Study kinodynamic planning**: How to incorporate car dynamics.
4. **Learn RL basics**: DQN, DDPG, PPO, SAC.
5. **Try RL for simple navigation tasks** in simulation (e.g., OpenAI Gym’s CarRacing-v0).
6. **Combine RL with classical planning** for more robust solutions.



[[F1Tenth Motion Planning]]