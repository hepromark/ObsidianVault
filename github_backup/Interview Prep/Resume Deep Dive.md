### 1. Vehicle Dynamics Control Intern | Tesla
Themes: embedded systems, control theory, safety-critical systems

**Plausibility Check**
You developed real-time sensor data plausibility checks. Walk me through a specific scenario where a sensor fails but looks like valid data (e.g., a bias shift or a stuck value). How specifically did your physical model differentiate between a high-dynamic vehicle maneuver and a sensor fault?
- To be honest, I haven't really seen that scenario where sensor fails -> still "looks" like real data
- My work is sensor data plausibility within the controller ECU, so it's fairly "high" up on the pipeline
	- There manufacturer has their own data filtering / correction already
	- Bias shift is handled on by supplier, their ECUs correct for that
- I degrade sensors that are 'obviously' dead: like an IMU saying the car is rotating quickly when none of the wheels & internal parameters correlate that 
- My model differentiated by using a bicycle model & statistical modelling. Essentially, we have a defined variance of sensor values computed for a certain car state for a signal. If the signal is beyond this for more than X seconds, the sensor gets degraded
- **TODO: learn the control hand-off** I'm guessing it's like every control action stabilizes the car further, i.e. if control dies at any point it would still be more stable than before

**Data Pipeline**
You mentioned creating DAGs (Directed Acyclic Graphs) for time-series data. Why a DAG? How did you handle out-of-order data ingestion or timestamp drift across the fleet? If the query graph failed on a specific node, how did the system recover?
- DAG because it's naturally the process for triaging
- This DAG operates on existing / not fresh data (few days old, there's guaranteed non-rewrite)
- **TODO: Learn and explain how the FM vs. FF process works, inorder to explain how the DAG worked**

**Controller Optimization**
You improved the dynamics controller to fix top failure cases. Pick one specific failure case. Was it an instability in the control loop or a logic error? How did you isolate it in a fleet of 2 million vehicles without complete telemetry for every second of driving?
- I used the DAG to automatically triage cases where the controller failed in the fleet
- Some errors:
	- Thresholding rate check too strict
	- State machine poor 'turn off for 2000km' behaviour
- The telemetry system passively takes in data and sends it to Tesla servers. Every ~20 hours batches get processed into smaller tables to more efficient querying. 
### **2. Simulation & Optimization Intern | Rapyuta Robotics**

**The "Spatial-Temporal Grid" Drill**
You achieved a 12x speedup using spatial-temporal grids. Explain the data structure you used for the grid. How did you handle agents moving between grid cells in a discrete timeframe? Did you encounter 'teleporting' issues where an agent moves through an occupied cell between ticks? How did you solve that?"
- Let's just consider 1 'floor' to simplify explanation: it's x, y, (a 2D) numpy array where each element is either None (so no robots) or 'Occupied' which is the corresponding contiguous time frame & robot ID that occupies it
- Since the order processing is sequential, we can schedule each robot path at a 'global' level by sending a trajectory through that array
- Any subsequent trajectories will NOT be conflicting because we just make sure we don't occupy the grid at the same time that some other robot occupies it

**The "Genetic Algorithm"**
You used custom Genetic Algorithms for storage placement. How did you encode the 'genome' for a warehouse layout? What was your fitness function, and specifically, how did you weigh density against throughput? How did you prevent the algorithm from converging on a local maximum too early?
- The overall 'layout' is fixed, genetic algorithm operates on how special objects are placed within the warehouse layout
- Genome is basically python lists containing locations (x,y,z) of elevators, pickup/dropof tiles, etc.
- Fitness function was the estimated throughput per hour of this structure on a large & randomly generated item list
- "how to prevent algorithm from converging too early" -> large number of starting individuals, high mutation rate, and we lowered the cutoff so that not too many individuals will be discarded from the population pool. 

### **3. Robotics Planning Intern | Rapyuta Robotics**

**The "Django Scaling" Drill:**
"You scaled the database to handle 10x more robots ($6\rightarrow60$). What exactly was the bottleneck? Was it Python's GIL, database lock contention, or query inefficiency? When you implemented 'caches'8, what was your invalidation strategy? If a robot's state changed, how did you ensure the planner didn't read stale cache data?"
- It was mainly query inefficiency & poor choices/bugs in the schema
	- An example bug: we had an self-referencing attribute in the django model (so I want to list all Tasks but inside the Task for some reason it had a dynamic field that requires reading all tasks) so that O(n) call become O(n^2). This wasn't caught since we only tested on small number of Tasks, but when task counts went to like 10k+ in prod our server died
- Caches were for map updates -> robots ping server for map updates whenever possible
	- Added caching and instead of sending entire map, just send diffs on the existing map (versioned diff'ing system)
	- In some cases where big changes occured, the entire map was resent but that is rare
	- Stale maps versions are ignored by robots


**The "Task Tree" Drill:**
"You generated backend task trees optimized for parallel execution. How did you detect dependencies between tasks? If Robot A's task failed, how did the tree propagate that failure to Robot B, which was waiting on Robot A? Did you use a standard graph traversal algorithm, and if so, what was the time complexity?"
- Doubly-linked tree structure, dependencies are communicated by 1) upstream tasks dependent on downstream finishing and 2) 'hard coded' tree layers, i.e. movement tasks vs. "item devlivery" higher level task
- Tree propagate failure by traversing this tree -> goes up to and marks parent tasks as failed, then logic that cancels lower leveling tasks with failed parents
	- O(n) in number of related tasks (could be alot could bejust the parent)

### **5. Computer Vision Researcher | VIP Lab**
**The "Lazy Loading" Drill:**
"You optimized inference by creating lazy loading to cache/load latent feature matrices. Explain the memory access pattern. Did this introduce latency spikes during the 'load' phase? How did you balance VRAM usage against the PCI-E bandwidth cost of constantly moving data in and out of the GPU?"
- bandwidth cost weren't considered by me, this was done offline on datasets. Main constraint was running out of memory, memory loading speed isn't too much of a problem due to inefficencies else where

**The "ViT Architecture" Drill:**
"You used a ViT-based architecture for 3D reconstruction. Why a Vision Transformer? How does the self-attention mechanism help with 3D reconstruction compared to a standard CNN? specifically, how did you handle the quadratic complexity of attention if you were dealing with high-res construction site images?"        
- VIT mainly to abuse the rich pre-training
- **TODO: find out why ViT are actually good (why Mast3r used them) and how did the resolution work?**
- Resolution was just cropped down to 480 by 480

### **6. STM32 Custom Operating System**
**The "Malloc" Drill:**
"You implemented a custom free list allocator with coalescence. Walk me through the coalescence logic. When you free a block, how do you mathematically check the adjacent memory addresses to see if they are free? How do you handle the header overhead to ensure you aren't overwriting the next block's metadata?"
- Simple algorithm that I came up with:
	- Assume state is already as condensed as possible (i.e. no 2 unallocated block next to each other)
	- Free a block at some location -> call 'coalescence()' on this existing block that will combine this block and next block together if possible
	- Call 'coalescence()' one more time on the block before it
- Header overhead also easy-> each metadata holds the size of this block, so via memory address addition 

**The "Context Switch" Drill:**
"You implemented pre-emptive EDF scheduling. Explain exactly what happens to the stack pointer and the program counter during a context switch on the ARM assembly level. How did you ensure the scheduler itself didn't consume too many cycles, causing the system to miss the deadlines you were trying to enforce?"        
- #TODO: review stack pointer and program counter during context switch for ARM assembly

### **7. WARG (Drone Clustering)**

**Focus:** Unsupervised Learning, Probabilistic Modeling.
**The "Gaussian Mixture" Drill:**
"You used a Variational Gaussian Mixture Model (GMM). Why Variational? How did you determine the number of components (clusters) dynamically?
- #TODO: what's difference between GMM vs. VGMM?
- Implement a simple k-means vs. GMM vs. VGMM in python so that I'm not just a sci-kit learn monkey