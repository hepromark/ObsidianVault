What is scheduling in computing?
- The action of assigning *resources* to perform *tasks*

### Overview

This is a class of [[Optimization Problem]] related to computer scheduling. 
- Inputs: List of tasks (what to do) and machines (things that does the tasks)
- Outputs: A schedule; assignments info of tasks to machines
- The output schedule should optimize a certain *objective function* 

### Problem Classes

There exists many different problems of optimal job scheduling; nature of jobs / machines/ restrictions / objective function. 

A common notation to describe the optimization problem:
- $\alpha$: Machine environment
- $\beta$: Task characteristic & constraints
- $\gamma$: Objective function

### $\alpha$ Machine Environments

In single-stage job scheduling:
1. Single machine scheduling: 
	1. 1 machine.
2. Identical machine scheduling: 
	1. $m$ identical parallel machines.
	2. Job $j$ takes $p_j$ on any machine.
3. Uniform-machine scheduling: 
	1. $m$ parallel machines with different speeds.
	2. Job $j$ takes $p_j/s_i$ time on machine $i$.  
4. Unrelated-machines scheduling: 
	1. $m$ parallel machines unrelated to each other. 
	2. Job $j$ takes $p_{ij}$ on machine $i$. 

In multi-stage job scheduling:
5. Open-shop problem: 
	- $m$ machines
	- Every job $j$ consists of $m$ operations $O_{ij}$ for $i$ from 1 to $m$ 
	- Operations can be scheduled in any order
	- Operation $O_{ij}$ must be processed for $p_{ij}$ time units on machine $i$
6. Flow-shop problem:
	- Same as open-shop, but operations must be processed in a *given* order
7. Job-shop problem:
	- $m$ machines
	- Every job $j$ consists of $n_j$ operations $O_{kj}$ for $i$ from 1 to $n$ 
	- operation processed given order
	- operation $O_{kj}$ must be processed for $p_{kj}$ units on a *dedicated machine* $\mu_{ij}$ 
	- $\mu_{kj} \neq \mu_{k{\prime}j}$ for $k \neq k{\prime}$  (meaning each machine must process at most 1 operation per job)

### $\beta$ Job characteristics

The job characteristics are specified by a set $\beta$ containing at most 6 elements:
1. Preemption
2. Precedence Relations
3. Release dates
4. Processing requirements
5. Batching

#### Time characteristics 
by modern convention, all processing times are assumed integers
1. Processing times equal 1 for all jobs: $p_i = 1$ or $p_{ij} = 1$
2. Processing times equal for all jobs: $p_i = p$ or $p_{ij} = p$
3. $r_j$ the release time before a job can be scheduled
4. $d_j$ the due date of a job. Usually used in the objective function to punish schedules with late jobs.
5. $\bar{d}_j$ the deadline (strict due date) of a job. All jobs must be completed before its deadline.
6. ${size}_j$ the number of machines that need to operate on the job at a time. 

#### Precedence Relations
A pair of 2 jobs may have a precedence relationship. This means that one job must be completed before the next job can start.
1. **prec**: No restrictions on precedence relations
2. **chains**: Each job preceded by at most 1 job and is predecessor to at most 1 job
3. **tree**: either an intree or an outree
	- **intree**: Each node is the predecessor of at most 1 job
	- **Outtree**: Each node is preceded by at most 1 job
4. **opposing forests**: 
