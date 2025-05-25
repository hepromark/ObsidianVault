In F1Tenth Racing's context, we have some requirements for the motion planner:
- High responsiveness 
- Avoid obstacles (potentially dynamic for head to head racing)
- Motion constraints (some physical things that is impossible)

We could encode the configuration space w/ more parameters (ex: encoder speed, etc.) but that will raise dimensionality 


# RRT (Rapidly Exploring Random Trees)

#### Idea:
- Live in tree-space, want to setup a tree to explore the world
- We sample a random point in the space, if space is free drop a node
- Find the *nearest* node on current tree to the sampled point
	- Many ways to define *nearest*
- Expand the tree from nearest node -> sampled point
	- Via the "steer" function, again many ways to define steer
	- Operating in 3D world space, could be spline (not just straight edge between nodes)
- Check the expansion edge for collisions w/ obstacles
	- i.e. against the occupancy grid

#### Continuous vs. Discrete:
- Sampling & steering step happens in continuous
- Collision checking (occupancy grid) happens in discrete

Probabilistically Complete:
- Since probabilistic, no one to prove it will give a solution
- But as samples increase, prob approaches 1

Not optimal:
- Due to random sampling
- Variants of RRT (ex: RRT*) that considers the cost for a more optimal solution
- 