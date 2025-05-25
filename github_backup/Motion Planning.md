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

==**Completeness**==: Algorithms are complete if
- Terminate in finite time
- Returns solution if once exists / returns failure otherwise

==**Optimal**:== Algorithms are optimal if:
- Returns the minimum cost solution when a solution exists

There exists complete algorithms for motion planning, but:
- Not practical (ex: exponential time complexity)
- Real algorithms relaxes completeness guarantees somehow

==The core idea is:==
- We want to convert the *formal problem* into a *search problem*
- i.e. create representations of the actual problem, then use search as the last step to find a solution
- We want to "convert the *formal problem* into a *search problem*"


Low dimensional problems can be solved with grid-based algorithms or geometric algorithms.
- [[RRT]] 

High dimension problems may be computationally intractable:
- Potential-field algorithms effect but affected by local minima
- Sampling-based algorithms more promising (they are also currently considered SOTA) and apply well to higher (i.e. hundreds) dimensional problems

[[F1Tenth Motion Planning]]