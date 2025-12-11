Timeline
- University of Washington in ME
- Tesla 2015 - 2024 (on Chassis Controls)
	- Integration -> CC -> senior -> staff
	- Wrote a bunch of the state estimation docs
	- Presumably a Controls / Firmware guy?
	- Ask Jack about him on Monday
- Some gap between Tesla - Mytra ?
- Mytra: 2024 - 2025

### Questions

Personal / past career
1. Degree in ME? Did you have view on programming or fell into it?

Tesla
1. You haven't left for too long, but currently for the most part CC & Tesla vehicle firmware is pretty stable -> no direct pressure for the 'next big thing'. I personally love the big projects / new hard stuff / things to do, what did you think about that shift? Did you notice the change?
2. Why did you move?

### Mytra
1. What part of the system are you working on at Mytra?

Technical
1. Very interesting wheel design! RR had mecanum wheels (https://en.wikipedia.org/wiki/Mecanum_wheel) to just do xy movement. Mytra does xyz movement directly via like 2 sets of 4 wheels each? 
2. How does robot know where it is on structure? How does it recognize & path plan with the other robots?
3. What's the current system reliability / bottle neck? What is the critical problem that you're solving
4. Connectivity / networking constraints? 1 contiguous unit so hypothetically it could get very deep right?
5. Speed / safety tests? 3000lbs pallets high up travelling at speed going wrong = not good?
6. You guys advertise "AI"- like is there actually an ML system running in the back for path planning / task assignment / etc. or is that incoming

![[Pasted image 20251122130524.png]]
7. How important is throughput? At RR with their use case, matching 600 lines per Man Hour very important goal. Fundamentally bigger scale for Mytra, but item pickup is much slower due to items picked up by forklift operators?
8. Centralized or decentralized compute? Onsite servers? Links with connectivity question

Potential problems I see:
7. Bot dies in the middle of the structure -> how do you retrieve it?
8. It's very expensive


### Rapyuta Robotics

RR vs. Mytra:
- Generally cheaper/smaller scale- plastic structure, 2D robots + lifts built into the structure
- Targeted lower mass & consumer items, ex: stationary, toys, etc.
- Mutable contents in each item bin (ex: put in more pencils, take out apples, etc.)
- Generally could be used as entire storage & retrieval stack end to end (from input truck to shipping truck)

What I did:
Main:
1. Robotic task planning. Integrated the WMS to our world model (a DB) and the DB to our path planner. 
2. Simulation & structure optimization. SimPy-based simulation system, we were running into throughput (algorithmic) constraints. 
Peripheral:
3. Talked to people about path planning, networking, etc.
Why not RR:
4. Managers (like 3) were very 'complacent' / had a lot of job security
5. Loved people I was working with, but the 1 layer above I don't like the environment they created (1/3rd of team left and then project got restructured)



