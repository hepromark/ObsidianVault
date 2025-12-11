Source: https://hal.science/hal-03580775/document

$P|prec, d_j = d|T_{max}$
- Precedence constraints, different processing times, same due date
- NP - Hard problem
- Modelled w/ stochastic processing times (time-aware algs mostly dependent on measurements from previous frames to est. current frame behaviour)

**Notation:**
- $T_j$ tardiness
- $P_j$ - stochastic processing times
- $F$ - total number of frames
- $f \in [1, F]$ - a single frame
- $T^{f}_{max}$ - maximum tardiness of frame $f$

**3 Possible Optimisation Metrics to Minimise**
1. Slowest Frame SF - frame w/ highest tardiness
2. Delayed Frames DF - Number of frames with tardiness > 0
3. Cumulative Slowdown CS - Total tardiness across all frames

### Exploring List Scheduling Algorithms

Generally, List Scheduling is:
- Greedy algorithm for identical-machines scheduling
- Input: list of jobs to be executed on machines
- Alg repeatedly executes:
	- Takes in highest priority job
	- Find machine available for job exec- no available machine, then execute next job
- Performance guarantees:
	- O(n) in number of jobs
- Adaptive to # of resources available

This paper's algorithms use the following strategy:
1. Macro-scheduler takes highest priority task and calls micro-scheduler
2. Micro-scheduler calls one of its sub-tasks

**Game Engine Baseline**:
- First In First Out (FIFO)

