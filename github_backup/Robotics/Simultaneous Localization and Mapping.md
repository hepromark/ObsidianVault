**Definition:** 
Computation problem of **constructing / updating a map** of an unknown environment while simultaneously keeping **track of an agent's location** in it 
- Seems to be chicken or egg problem, but is actually solvable
- Usually algorithms are not optimal (hard problem) but are 'good enough' for the application

**Mathematical Formal Definition**
![[Pasted image 20250520194720.png]]

### Mapping
1. Grid Maps
Discretize the physical space into small grids.

2. Topological Maps
Represents the environments' topology (ex: representing a building as nodes) instead of modelling the physical space

Some modern applications (ex: self-driving cars) uses extremely good pre-mapping -> don't need to do online Mapping at all.
### Localization
The process of localization against the existing / known map

Heavy use of sensors:
- Usually different types of sensors that are (statistically) independent
- SLAM algorithm depends on the sensor choice
- High information sensors, ex: cameras
- Low information sensors, ex: WiFi signal strength, Light-sensors at RR

# Algorithms

Ranging from less complex to more complex (context of robots)

When without a map:
1. Encoder or IMU Dead Reckoning
2. Encoder & IMU simple fusing (integration then average)
3. Encoder & IMU Extended Kalman Filter
4. Lidar feature extraction + estimate motion from diff between frames
5. Lidar-based SLAM
	- Hector SLAM
	- LOAM

When with a map (race time):
1. Scan match to a starting scan
2. Adaptive Monte Carlo Localization (i.e. Particle Filtering)

[[Scan Matching]]
[[Loop Closure]]
[[Pose Graph Optimization]]
[[Bundle Adjustment]]



---
# F1Tenth SLAM
Commands:
`ros2 launch slam_toolbox online_sync_launch.py params_file:=./src/mapper_params_online_async2.yaml use_sim_time:=true`

`sudo apt-get -y install ros-humble-slam-toolbox && sudo apt-get -y install x11-apps && sudo apt-get -y install ros-humble-rviz2 && sudo apt-get install x11-xserver-utils`



