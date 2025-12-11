### Job Description
- Ability to read electrical schematics and debug electronics hardware using equipment such as multimeters, oscilloscopes, signal analyzers, hardware debuggers, etc
- Prior experience working on robotic systems
- Experience working on embedded systems. Knowledge of NVidia Jetson based architectures is a plus!

Sample Intern Projects:
- Work on a stereo matching algorithm for a sensor pair
- Write a pipeline to fuse infrared images with EO images
- Develop an AI model to detect buoys
- Write a AI detection system for radar
- Develop and train an AI model to detect vessels, buoys, swimmers
- Develop a real time 360 degree top down stitching algorithm
- Project AR information into a camera stream

### Resume Points
1. CV Researcher
2. F1Tenth stack
	1. Autonomous software stack + decision making
	2. Sensor fusion for IMU + Encoders 
3. Working w/ Jetson on WARG drone

### Technical Concepts
1. Computer Vision
	1. Stereo Matching: 2 images + matching
	2. Sensor Fusion: between cameras, lidars & other sensors
	3. Object Detection Models: 
		1. YOLO models, MobileNets, etc.
		2. TensorRT & ONNX deployment on Jetsons
	4. Image stitching

2. Embedded Systems
	1. Sensor Interfacing
	2. Protocols -> [[Embedded Protocols]]

3. SLAM methods
	1. EKF to fuse GPS, IMU, etc. -> [[Simultaneous Localization and Mapping]]
	2. Marine-specific SLAM methods
	
### Interviewer Notes - April Blaylock

Background:
- Robotic Developer -> Unmanned Aerial Eng
- Vision System Arch -> Sr. Principal Aerial Systems Arch
- Staff Autonomy Systems Arch 
- Analyst?
- Co-Founder enVgo

Personality
- Passionate about Autonomy
	- Vietnam pollution & how it can be mitigated w/ autonomous vehicles
	
- Architectural level experience
	- Say how I enjoy system design / architecture over the implementation
	- I designed systems in my last startup internship
	- Joined F1Tenth to be able to define the architecture & software direction
	


### Marine Industry

**4 levels of autonomy**
1. Ships w/ automated process & decision support. Some operations may be automated & unsupervised, but human control can be taken

2. Remotely controlled ship w/ people onboard. Controlled and operated remotely, but human onboard control can still be taken

3. Remotely controlled ship without seafarers on board.

4. Fully autonomous ship. Operating system of ship able to make decisions and determine actions by itself.


**Sonars**
- Essentially underwater lidar in-terms of data type & operating principle
- Gives direction + range (depthmap)
- Uses sound waves instead of laser
- Used to measure height from ocean floor, mapping surround area, or foward-facing sonars for obstacle avoidance



