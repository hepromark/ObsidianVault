### Overview
ROS2 is an open-source software framework to help build robotics applications. 
- Acts as middleware
- Allows communication between processes

### Features
1. Nodes
2. Topics
3. Services

4. Actions

Made for long running tasks
- 3 parts: goal, feedback, and result

Similar to services, but actions are pre-emptable
- Provides steady feedback (instead of services with a single response)
- Client-server model
- Action client node sends a goal to an "action server" node that acknowledges the goal and return a stream of feedback

Ex: navigation scenario, action goal tells robot to travel to a position, robot continually send updates along the way + final result msg once its arrived
	
6. Parameters

### Data Distribution Service

### ROS vs ROS2
ROS2 is newer:
- DDS - higher efficiency, reliability, low latency, and scalability
- Native MRS support
- DDS-based security (more secure)
- Lifecycle management

### Scale-able alternatives
ROS2 has issues in real production & at large scale
1. Massive node & topic counts across many robots is hard to manage
	- Network saturation
	- Startup timing issues
2. Real-Time constraints not fully met
	- ROS2 itself supports real-time, but actual performance depends on the different packages, settings, and architecture design
	- Not all packages are built for real-time determinism
3. Difficult Security
	- Security is not plug & play
	- must enforce secure communications across the ROS2 system by writing low level configs
4. Poor DevOps integration