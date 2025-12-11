*NVIDIA Omniverse*
- NVIDIA Omniverse a dev platform w/ SDKs, APIs, and microservices for 3D apps and services that uses OpenUSD and RTX
- Rendering is Omniverse RTX Renderer
- Physics is NVIDIA PhysX SDK (already in Omniverse)

*Isaac Sim* provides a framework & tools for specifically robotic workflows:
- Synthetic data gen
- Robot Learning
- Sensor simulation
- Integration w / AI and RL pipelines
![[Pasted image 20250704071925.png]]

*Isaac Lab* is builds on Issac Sim specifically for robotic learning (RL):
- APIs & ready to use examples for RL, imitation learning, etc.
- Integration w/ popular RL libraries
- Gives scalable parallel training across multiple GPUs and nodes
- Adds specific feature for robot learning: actuator dynamics, procedural terrain, human demos, etc.
- Open source interface for development, sharing, and bench marking of robot learning environments
- Replaces *Issac Gym*
