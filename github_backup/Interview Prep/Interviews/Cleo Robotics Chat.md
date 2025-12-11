### Cleo Robotics

Robot company w/ 1 product: Dronut

![[Pasted image 20251102124035.png]]

![[Pasted image 20251102124053.png]]

### Interviewer: CTO Simon Crzarnota
##### Builder Nation Interview
- Always like making computers think
- Coding long time now
- Spent a year working at Cisco internship-> 1 year
	- In cybersecurity team, build & testing for commerical / military use
	- Software but not hardware for some reason
- Personal interest in hardware
- Taking things apart, etc.
- Want to do something on his own (founder grindset)
- Was demotivated by lack of impact in corporate company
- Want to sell to potential interns: big impact at small company
- Love being able to forge own path
- Why Cleo Robotics? Why a drone?
	- Born & raised in Poland
	- **Came to Canada at 10 (immigrant like me yay)**
	- Lived in Calgary, Alberta
- Worked for a bit at oil/gas company
- Co-founder went out into field to do pipeline inspection
	- Dangerous visual inspection
	- Protective clothing / risky fumes / etc.
	- Why not use a drone?
- Most drones have exposed propellers, make them very dangerous to operate
	- "Build the perfect drone possible"
	- Safe, small, efficient, capable
	- 1st iteration of the Dronut that was very small 
- Dronut:
	- Co-axle ductive fan (2 propellers stacked ontop of each other)
	- Both are enclosed in the shell of the dronut
	- All electronics / power systems within the shell
- Challenges they have:
	- CEO & Co-Founder had no aero dynamics experience
	- Need to learn actuators & 
- **Every he learns he has self-learnt anyways** -> humble, loves to learn
- Ductive fans technology is not new
	- Notoriously difficult to control
	- Patented: thrust vectoring technology (I'm working on torque vectoring)
	- Came up with: uses flaps that are retractable to direct airflow and vector thrust
- They didn't want to over-complicate things
- They want to make this drone as *similar to a quad-copter as possible*
- Use as many existing solutions as possible
- Created a translation between dronut actuator <-> quadcopter actuator
- 4 generations:
	- 1st gen: too small to fly flast
	- 2nd gen: too small to get strong compute
	- 3rd gen: optimizing size, to find sweet spot between performance and efficiency
- No plans to go into consumer market
	- Price point still too high
	- Platform itself is super stable
	- On a technical/hardware level it's very doable, but market isn't available
- 2 main use cases
- 1st use case:
	- Indoor inspection, confined space inspection (powerplants, warehouses, manufacturing plants)
	- Flying *inside* a piece of equipment for inspection
- 2nd use case:
	- ISR -> Intelligence, Surveillance, Recon
	- Law enforcement deployment, etc.
	- Easy to fly, so get videos on the inside
- Best ideal use case:
	- Not trying to build for any single use-case
	- "Ultimate data collection platform" that is easy & intelligent to use



Good questions:
- Does the drone need any special protection to work in these hazardous environments?
- Is performance degraded when the drone is outside?
- How complicated was the control / how hard was the autonomy?
-
- **How loud is the dronut?** -> promo videos all have music

Personal Question: What is the one advice for new/young entrepreneur coming out of college?
Answer:
- Lots of startup founders have engineering background, want to build
- But it's not just building -> need to do other stuff like talking to investor, marketing, etc.
- Lots of time not building & not engineering
- Watch-out for everything else that is not just the fun shiny stuff

# Friday Morning Conversation

### About Me
- born and grew up in Vietnam
- Always liked maths and physics, was 1 english channel in Vietnam was National Geographic
- Moved to Canada at 12
- Started university as MECH ENG, but realized I really really enjoyed coding as well (extension of math) -> AutoCAD & CAD design / etc. wasn't my cup of tea
	- Self-teaching a lot of programming, CV, C++, ML, worked on a module for a drone team
- Worked on robotics last year in Japan, was for a mid-size startup that did warehousing. 
- Had these "load carrying roombas" that moved stuff around their special warehouse
- Wrote planning algorithms for them
- Some more recent stuff:
	- Want to 'validate' knowledge; did the same design team as Rowan (where I met him) -> exposed to system design, controls, state estimation, etc. 
	- Computer Vision research to do mapping using cameras on drones
- I'm at Tesla- working on controls 
- overall a lot of problems that the team needs to be solved.
	- Ex: Fleet analytics & bulk data pipelines and analysis
	- Ex: Improving Anomaly Detection ML models
	- Ex: Improving controller up-time
	- Ex: Test automation using High-Fidelity-Physics-Models

Questions:
- What are your plans for the drone? Are the autonomy use cases == manual use cases?
- Due to how it's shaped/built, okay-ish wind resistance -> sticking to indoor use cases?
- How loud is it?
 - The hardware seems super impressive (compard to competitor platforms as well) -> any thoughts on essentially selling it as a platform for others to do compute on?

# Technical

### Resume

Biggest problem right now:
1. if he asks to "show code" it's a little cooked currently
- [ ] CV Code is shitty -> need to fix ASAP
- [ ] Need to read & understand the F1Tenth code base + finish the diagram
2. Too surface level knowledge of ROS
- [ ] Revise ROS2 definitions & potential architectures

**Tesla**
> Developing real-time sensor data plausibility checks using theoretical & simulated guarantees, raising system safety.
- Statistical confidence intervals, if >3 sigma away (99.7%) then degrade sensor trust
- Bicycle model on these existing signals to get baseline for^

**CV Researcher**
> Utilized Open3D & PCL libraries to extract metrics compute loss between model output & baseline lidar results.
- The main problem is: it's VERY expensive to get points which are adjacent to each other
- Use K-D trees, which are essentially a way to query the "closest" neighbours neighbours in O(1) time
- So sample a subset of the cloud, pick a distance ex: 1m, count how many in 1m, then compute avg density

**Autonomous Racing Team Lead**
> Led the design & implementation of pure pursuit control, lattice motion planner, particle filter localization, and EKF.
- Basically this is what devised as the "most simple" architecture possible to autonomy
- We use the nav2 library to PCL, we implemented pure pursuit control & lattice planner & EKF by ourselves (mainly because I thought it'll be a good opportunity for the members to learn)








### Misc Research Notes
![[Pasted image 20251102122750.png]]


