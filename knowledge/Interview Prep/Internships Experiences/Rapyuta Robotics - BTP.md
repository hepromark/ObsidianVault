I developed a Python component for Rapyuta's ASRS system that schedules item bin movements for the storage system to work. This component interacts with the system's world model database, which I also work on occasionally.

### Bin Task Planner
#### Overview
This component lives between the high level Warehouse Management System and lower level robot / elevator / path planning components.
- It consumes 'orders' from the WMS and turns these into movement tasks
- Ex: An order for bananas -> it creates a task for a bin with bananas to come to human
#### Interactions
BTP is a standalone component that interacts with data from the world model database ("OWM"). 
- Bin Tasks exists as objects inside the world model
- Downstream / lower level component interacts completes these Bin Tasks and updates fields in corresponding world model objects

"Why isn't BTP combined inside the world model?"
- Initial design philosophy was to have WM code be as simple as possible -> just a simple 'source of truth' that other components interact with
- At time of internship completion, I participated in the re-design of the WM to now absorb the Task Planner to make all actions atomic

#### Pros and Cons of BTP <-> World Model Architecture
Pros
1. Easy to develop - no knowledge of Django or DB libraries required ->`GET` calls to the DB, operate on local data, then `POST` or `PATCH` it back.
3. Both BTP and world model can be developed in parallel since logic isn't inter-twined, allows more people work without conflicts.

Cons
1. Race conditions
2. Scaling local replica: To operate quickly and reduce DB calls, a local replica of data is needed. The local replica is hard to scale.
3. Requirement creep: As operations / requirements gets more complicated, intermediate structs are needed to store relationships. These structs are then stored inside the database (since too much information for local memory) which: 
	1) abuses the 'WM' because there is component-specific data on the central SOT 
	2) complicates debugging with large amounts of relationships / ownerships.
4. Breakdown of large tasks into smaller tasks makes end-to-end optimization methods unviable. Instead of tackling the optimization problem holistically (multi-stage scheduling problem), the broken-down nature of the database object means that multiple single-stage optimization problems are present instead. 
#### Scheduling Problem
[[Optimal Job Scheduling]]

[[Distributed Computing]]

### Fallback Behaviours

1. Task Retry Controller
	- If a task starts execution but robot fails before picking up item bins, send out another task
	- Mitigates non-critical robot errors
	- Least expensive recovery

2.  Dynamic End Location Fallbacks
	- If a robot realizes its end location is unavailable
	- Asks planner for a new location

3. Dead bins
	- Sometimes, a robot dies while still carrying a bin. Makes bin inaccessible
	- Essentially blacklists this bin against future queries until robot is online again
	- Re-enable after lower level components say its ok

4. Bin Disabling
	- Some bin (looks fine) but tasks keep failing when interacting with this bin
	- Unknown error, so after a while disables this bin so robots can't try to touch
	- Re-enables after some timeout

5. Bin overflow
