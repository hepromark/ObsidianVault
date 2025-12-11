# Introduction to RL

### About
Reinforcement is the science of decision making
- Machine Learning - CS
- Optimal Control - Engineering
- Reward System - Neuroscience
- Classical Conditioning - Psychology
- Operations Research - Math
- Bounded Rationality - Economics

Characteristics of RL:
- RL is different from other ML paradigms
- no supervisor, only a reward signal
- Feedback is delayed, not instantaneous
- time matters- data is sequential, not "independent instantaneous data"
- dynamic system, agent is moving through world
- Agent's actions affect the subsequent data it receives

Examples of RL:
- Playing backgammon and beating world champions
- Manage investment portfolios
- Control power stations
- Make humanoid robots walk
- Play many different Atari games better than humans

### Reinforcement Learning Problem

Rewards:
- Reward $R_t$ is a scalar feedback signal
- Indicates how well agent is doing at step t
- Agent's job is to maximise cumulative reward
- RL based on reward hypothesis

**Reward Hypothesis**: All goals can be described by the maximisation of expected cumulative reward
- Fundamental idea behind RL

Examples of rewards:
- Fly stun in helicopter: + for following trajectory, - for crashing
- Defeat world champion: +/- for winning or losing a game
- Manage an investment portfolio: $ money

Sequential Decision Making:
- Goal: select actions to maximise total future reward
- Actions may have long term consequences
- Reward may be delayed
- Can't be greedy (less now -> more later)

Agent and Environment:
- Observation $O_t$
- Reward $R_t$
- Action $A_t$

There is an observation - action - reward loop
- At each step, agent:
	- Executes $A_t$
	- receives $O_t$
	- receives $R_t$
- At each step, environment:
	- Receives $A_t$
	- Emits $O_t$
	- Emits $R_t$

History and State
- History $H_t$ is the sequence of observations, actions, and rewards so far
- $H_t = A_1, O_1, R_1, ..., A_t, O_t, R_t$
- All observables variables up to time t
- i.e. the sensorimotor streams of a robot / embodied agent
- Agent selects actions based on history
- Environment select observation depends on history
- But not very useful since it's enormous, too big
- Typically *state* is the information used to determine what happens next

Formally, state is just a function of history:
- $S_t = f(H_t)$ 

*Environment State*: $S^e_t$ is the environment's private representation 
- Usually invisible to agent
- More like a formalism, we can't really design with this information
![[Pasted image 20250627124944.png]]

*Agent state*: $S_t^a$ is the agent's internal state representation
- Whatever info agent uses to pick the next action
- RL agorithms use this information
- Can be any function of history: $S_t^a = f(H_t)$ 

*Information State:* i.e. a Markov state, contains all useful info from history
- A state $S_t$ is *Markov* if and only if:
	$\mathbb{P}[S_{t+1} \mid St] = \mathbb{P}[S_{t+1} \mid S_1, ..., S_t]$
- The future only depends on present, not past
- State fully characterizes the distribution of the future

*Fully Observable Environment*: Agent directly observers the state
- $O_t = S^a_t = S^e_t$
- Known as **Markov Decision Process**

*Partially Observable Environment*: Agent indirectly observes environment
- Robot has camera vision not known absolute position
- Trading agent only sees current prices
- Poker agent only observes public cards
- Agent state != environment state
- **Partially Observable MDP**
- Agent must construct its own state representation:
	- Contain entire history $S_t^a = H_t$
	- Or have beliefs of environment state: 
	- Or recurrent neural network
![[Pasted image 20250627132901.png]]

### Inside an RL Agent

RL agent may include 1 or more of these components:
- Policy: agent's behaviour function
- Value function: how good is each state and/or action
- Model: agent's representation of the environment

**Policy**:
- Agent's behaviour
- Maps from state -> action
- A deterministic policy: $a = \pi (s)$
- A stochastic policy: $pi(a \mid s) = \mathbb{P}[A = a \mid S=s]$

**Value function**:
- A prediction of expected future reward
- We need because need to make choices that has the highest expected future reward
- $v_{\pi}(s) = E_\pi[R_t + \gamma R_{t+1} + \gamma ^2 R_{t+2} + ... \mid S_t=s]$
- Value function depends on the current policy $\pi$
- Can have a discount $\gamma$ to prioritize current rewards

**Model**:
- Predicts what environment will do next
- Transitions: $P$ predicts next state (ex: using dynamics)
- Rewards: $R$ predicts next (immediate) reward

Categorising RL agents:

1. Value Based if:
	- No Policy (implicit, just look at $v_\pi(s)$ and act greedily)
	- Value function

2. Policy based if:
	- Policy
	- No value function

3. Actor critic:
	- Policy
	- Value function

4. Model Free RL
	- Policy and/or Value Function
	- No Model
	- Don't try to explicitly define the dynamics of the environment

5. Model Based RL
	- Policy and/or Value function
	- Model
	- Do planning with the model

![[Pasted image 20250627135354.png]]

### Problems within Reinforcement Learning

Two fundamental problems in sequential decision making:
1. Reinforcement Learning:
	 - Environment is unknown
	 - Agent interacts w/ the environment
	 - Agent improves its policy

2. Planning:
	 - A model of the environment is known
	 - Agent performs computations with its model (without any external interaction)
	 - Agent improves policy
	 - Ex: tree search

Exploration and Exploitation:
- RL is like trial-and-error learning
- Agent should discover a good policy AND shouldn't lose too much reward along the way
- Exploration finds more information about the environemtn

Prediction and Control:
- Prediction: evaluate the future (given a policy)
- Control: optimise the future (find the best policy)

# Lecture 2: Markov Decision Process

### Markov Processes

Markov Decision Processes formally describe an environment for RL
- Def: A sequence of random states with the Markov Property
- Environment is *fully observable*
- Current *state* fully describes the process
- Almost all RL problems can be formalised as MDPs (ex: optimal control)
	- Continous MDPs
	- Partially observable problems can be convereted MDPs
	- "Bandits" are MDPs with one state

Markov Property:
	$\mathbb{P}[S_{t+1} \mid St] = \mathbb{P}[S_{t+1} \mid S_1, ..., S_t]$
- State completely characterizes everything we need to know from the history
- "Throw away" states previous

State Transition Matrix:
- Starting in some state $S$ and successor state $S'$, the *state transition probability* is defined by:
	$P_{ss'} = \mathbb{P}[S_{t+1} = s' \mid S_t = s]$
- matrix $P$ defines transition prob from all states to all other successor states

Markov Process is defined by tuple $(S,P)$

### Markov Reward Processes

A Markov chain with values
#### Definition
A *Markov Reward Process* is a tuple $(S, P, R, \gamma)$
- $S$ is a finite set of states
- $P$ is the transition prob matrix
- $R$ is a reward function, $R_s = E[R_{t+1} \mid S_t = s]$
- $\gamma$ is decay weight

#### Definition
The return $G_t$ is the total discounted reward from time-step $t$
	$G_t = R_{t+1} + \gamma R_{t+2} + ... = \sum_{k}^{\infty}{\gamma ^k R_{t+k+1}}$
- Discount $gamma$ is the presnet value of future rewards
- $\gamma$ between 0 -> 1, 0 is max short term and 1 long term
- Value of reward R after k+1 time-steps is $\gamma ^k R$

#### Definition
Value function $v(s)$ gives the long-term value of state $s$, which is the *expected return* starting from state s
	$v(s) = \mathbb{E}[G_t \mid S_t=s]$
- The state transitions can be random, but $v(s)$ is defined as the **expected return** so it's rigidly defined to a real scalar value

Bellman Equation for MRPs
Value function can be decomposed into 2 parts:
- Immediate reward $R_{t+1}$
- Discounted value of sucessor state $\gamma v(S_{t+1})$

Bellman Equation in Matrix Form:
- Bellman equation can be expressed in matricies:
	$v = R + \gamma Pv$
- where v is a column vector w/ 1 entry per state
![[Pasted image 20250628012747.png]]
- we can solve the Bellman equation directly since it's a linear equation:
$v = (I - \gamma P)^{-1} R$
- $O(n^3)$ for $n$ states -> not that good for large markov decision processes
- Large MRPs have many iterative methods:
1. Dynamic Programming
2. Monte-Carlo Evaluation
3. Temporal-Difference learning

### Markov Decision Process

The MDP is a Markov reward process *with decisions*. It is an **environment** in which all states are markov.
#### Definition
*Markov Decision Process* is a tuple $(S, A, P, R, \gamma)$ ( **Actions** is new)
- $P$ is a probability transition matrix:
	- $P^a_{ss'} = \mathbb{P}[S_{t+1}=s' \mid S_t=s, A_t=a]$


**Policies**
#### Definition
A policy $\pi$ is a distribution over actions given states:
	$\pi(a \mid s) = \mathbb{P}[A_t=a \mid S_t=s]$
- Fully defines the behaviour of an agent
- MDP policies depends on the current state (not just history)
- i.e. Policy is *stationary* (time independent)
	$A_t

We can always recover Markov Reward Process from the MDP:
- If we have some policy, we can draw the sequence of states from following this process is a markov chain
- No matter what policy chosen, the policy defines a markov chain
- The sequence of states / rewards we see is the Markov Reward Process

**Value Function**
#### Definition
The state-value function  $v_\pi(s)$ of an MDP is the *expected return* starting from state $s$ following policy $\pi$
	$v_\pi(s) = \mathbb{E_\pi}[G_t \mid S_t = s]$ 

#### Definition
The action-value function $q_\pi(s, a)$ is the expected return starting from state $s$, taking action $a$, *and then* following policy $\pi$
	$q_\pi(s,a) = \mathbb{E}_\pi[G_t \mid S_t =s, A_t =a]$

**Bellman Expectation Equation**
The state-value function can again be decomp. into immedate reward + discounted value of sucessor state
![[Pasted image 20250628214247.png]]

There's also a way to flatten MPDs -> Markov Reward Processes:
- Form matrix of state transition matrices

### Optimal Value Function
#### Definition
Optimal state-value function $v_*(s)$ is the *maximum value function* over all policies
	$v_*(s) = \max_\pi v_\pi(s)$
- This says what the maximum possible reward we can extract from the system (not what the best policy is)

#### Definition
Optimal action-value function $q_*(s,a)$ is the maximum action-value function over all policies
	$q_*(s,a) = \max_\pi(s,a)$ 
- Essentially 'solved' if we know this function -> we just need to pick the action $a$ where this function is the highest

Optimal Policy:
Define a partial ordering over policies:
	$\pi \geq \pi' \text{ if } v_\pi(s) \geq v_{\pi'}(s), \forall s$
- one policy is better than another, if the value function of the 1st policy is >= value function of the 2nd policy in all states

**Theorem:** 
- There exists an optimal policy $\pi_*$ that is better than or equal to all other policies
- There can be more than 1 optimal policy
- All optimal policies achieve the optimal value function: $v_{\pi_*} = v_*(s)$
- All optimal policies achieve the optimal action-value function, $q_{\pi_*}(s,a) = q_*(s,a)$

**Finding an Optimal Policy**
An optimal policy can be found by maximising over $q_*(s,a)$:
![[Pasted image 20250628222625.png]]
- There is always a deterministic optimal policy for any MDP
- If we know $q_*(s,a)$  

Bellman Optimality Equation for $V^*$ 
- Max over actions, and average over states
![[Pasted image 20250628224103.png]]

# Lecture 3: Planning by Dynamic Programming
### Introduction

What is dynamic programming?
- Dynamic: sequential or temporal component to the problem
- Programming: Optimising a "program" -> a policy
- A method to solve complex problems

We need 2 properties for DP to work:
1. Optimal substructure:
	Principle of optimality applies- the optimal solution to sub-problems informs the optimal solution to the main problem
2. Over-lapping subproblems:
	Subproblems recur many times
	We can cache those solution + get efficient divide-and-conquer approach

MDPs satisfy both of the above properties:
- Bellman equation gives recursive decomposition (optimal substructure)
- The value function *is the cache* 

Planning by Dynamic Programming:
- DP assumes full knowledge of the MDP
- The reward / transition structure is fully known, now just solve
- 2 special cases of planning:
	*Prediction*: (MDP + policy $\pi$) or (MRP) -> value function $v_\pi$
	*Control*: MDP $(S, A, P, R, \gamma)$ -> optimal value function $v_*$ + optimal policy $\pi$

Dynamic programming is used to solve many other problems:
- Scheduling algorithms
- String algorithms
- Graph algorithms (ex: shortest path)
- Graphical models
- Bioinformatics (ex: lattice models)

### Policy Evaluation

Known MDP + known policy, how good is this policy?
- Problem: evaluate given a policy $\pi$
- Solution: iterative application of Bellman expectation backup

We iteratively update $v$
$v_1 \rightarrow v_2 \rightarrow ... \rightarrow v_\pi$

Method 1: *synchronous backups*
- At each iteration $k + 1$
- For all states $s \in S$
- Update $v_{k+1}(s) \text{ from } v_k(s')$
- where $s'$ is a sucessor state of $s$
Iterative vector equation:
	$\vec{v}^{k+1} = \vec{R}^\pi + \gamma \vec{P}^\pi \vec{v}^k$ (*reminder: Bellman equation has $v^{k+1}=v^k$ )*
- This is guaranteed converge to a $\vec{v}$ 

Note: Any value function can be used to compute a better value function via greedy wrt the original value function

### Policy Iteration

*How do we make our policies better?*
- Given a policy $\pi$ 
	- Evaluate the policy $\pi$ by computing value function $v_\pi(s)$
		$v_\pi(s) = \mathbb{E}[R_{t+1} + \gamma R_{t+2} + ... \mid S_t = s]$
	- Improve the policy by acting greedily wrt respect to $v_\pi$
		$\pi'=  \text{greedy}(v_\pi)$

In general, we iterate through this policy improvement (evaluate + improve loop)
- **Always converges to optimal policy** $\pi$

Formally:
1. Consider a deterministic policy, $a = \pi(s)$
2. We can *improve* the policy by acting greedily
	$\pi'(s) = \text{argmax }q_\pi(s,a)$ over $a \in A$ 
3. This improves the value from any state $s$ over one step
	![[Pasted image 20250629134538.png]]
	- Following the greedy policy for 1 step then follow $\pi$ for the remaining steps, we are guaranteed to get the >= following policies for all steps
		- Comes from the definition of argmax, since $\pi'(s)$ is >= previous $\pi(s)$ 
	- The proof continues by iteratively doing this (following greedy for 2 steps >= 1 steps -> ... -> following greedy for all steps > following for 1 steps > following for 0 steps)
4. If improvements stop, Bellman optimality equation has been satisfied
	![[Pasted image 20250629135533.png]]
	- i.e. $v_*(s) = v_\pi(s) = \text{max}q_\pi(s,a) \text{ over } a \in A$ for all $s \in S$
	- 

Modified Policy Iteration:
- Idea: stopping early (when greedy policy doesn't change from iteration to iteration)

### Value Iteration

**Principle of Optimality**:
Any optimal policy can be subdivided into 2 components:
1. An optimal first action $A_*$
2. Followed by an optimal policy from successor state $S'$

![[Pasted image 20250629173302.png]]

Assume $v_*(s')$ is given:
- Do a one-step lookahead tree w/ Bellman optimality equation
![[Pasted image 20250629173516.png]]
- Idea of value iteration: apply this iteratively (numerically solve for the $v_*$)
- Intuition: start at the end of the problem (final rewards) and work backwards
	- Still works with loopy, stochastic MDPs

![[Pasted image 20250629193802.png]]

### Extensions to Dynamic Programming

DP methods described so far are synchronous backups (all states are backed up in parallel):
- Async DP backs up states individually, in any order
- For each selected state, apply the appropriate backup
- Reduce computation
- Guaranteed to converge if all states continue to be selected

3 Simple Ideas for async DP:
1. In-place dynamic programming
	- Instead of storing 2 vectors $v_{new}$ and $v_{old}$, just use the same $v(s)$ and update in-place
2.  Prioritised sweeping
	- Use the magnitude of the Bellman error to guide state selection
	- Backup state with the largest remaining Bellman error
	- Update Bellman error of affected states after each backup
	- Requires knowledge of reverse dynamics
3. Real-time dynamic programming
	- Idea: only concerned about states that are relevant to agent
	- Use agent's experience to guide the selection of states

Drawbacks:
- DP uses full-width backups
- For each backup (sync or async):
	- Every sucessor state and action is considered
	- Using knowledge of the MDP transitions and reward function
- DP is effective of medium-sized problems (million of states)
- For large problems DP suffers the *curse of dimensionality*
	- Number of states $n = |N|$   
- We can just do **sampling** to break the curse & do model-free RL

# Lecture 4: Model-Free Prediction

So far, we have assumed that the MDP is known. Now, methods for unknown environments.

### Introduction

This lecture covers the prediction step for Model-Free problems, where the *MDP is unknown*. 
- MDP is usually unknown in the real-world for practical & interesting applications

### Monte-Carlo Learning

MC methods learn directly from episodes of experience
MC is model-free: no knowledge of MDP transitions or rewards
MC learns from *complete* episodes: no bootstrapping
MC uses the simplest possible idea: value = mean return
Caveat: can only apply MC to episodic MDPs
- All episodes must terminate

 **Monte-Carlo Policy Evaluation**
Goal: learn $v_\pi$ from episodes of experience under policy $\pi$ 
	$S_1, A_1, R_2, ..., S_k$ ~ $\pi$ 
- (recall) *Return* is the total discounted reward
- (recall) value function is the expected return
- Monte-Carlo policy evaluation uses empirical mean return instead of expected return 

**Approach 1: First-Visit Monte-Carlo Policy Evaluation**
To evaluate states $s$:
- Define $N$ and $S$
- For each episodes:
	- The *1st time-step t* that state $s$ is visited in an episode:
	- Increment counter $N(s) \leftarrow N(s) + 1$
	- Increase total return $S(s) \leftarrow S(s) + G_t$
- Value is estimated by mean return $V(s) = S(s)/N(s)$
- By the rules of large numbers, $V(s) \rightarrow v_\pi(s)$ as $N(s) \rightarrow \infty$  

**Approach 2: Every-visit Monte-Carlo Policy**
- Same as before, but counter and total return updated on every-visit of the state, not just 1st time per episode

Incremental Mean:
- The mean of a sequence can be computed incrementally
- Gives the same result as the mean computed with entire number sequence
	![[Pasted image 20250629223405.png]]
- Essentially a prediction (current mean) and an update (difference between current mean and observation, scaled by $1/k$)

Incremental Monte-Carlo updates:
- Update $V(s)$ incrementaly after each episode
	$N(S_t) \leftarrow N(S_t) + 1$
	$V(S_t) \leftarrow V(S_t) + \frac{1}{N(S_t)}(G_t - V(S_t))$ #important
- In stationary problems, it can be useful to track a running mean (a constant stepsize $\alpha$), essentially a moving average instead of mean
### Temporal-Difference Learning

What are TD methods?
- Learn directly from episodes of experience
- TD is *model-free*: no knowledge of MDP needed
- TD learns from incomplete episodes, by *bootstrapping*
- TD updates a guess towards a guess

![[Pasted image 20250712121825.png]]

MC vs. TD:
- *Before final outcome*: TD can learn online after every step - MC must wait until end of episode before return known
- *Without final outcome*: TD can learn from incomplete / continuing sequences - MC only works from complete & episodic environments
- MC has good convergence properties -> not sensitive to initial value, simple to understand / use
- TD has low variance, some bias -> TD(0) converges to $v_\pi(s)$ Usually more efficient than MC, but sensitive to initial value

Bias / Variance Trade-off:
- Return $G_t$ is the an unbiased estimate of $v_\pi$ 
- True TD is also unbiased estimate of $v_\pi(S_{t})$ if $v_\pi(S_{t+1})$
- But actual TD target is a *biased* estimate of $v_\pi(S_t)$ 
	- But has less variance (noise)
	- $G_t$ depends on *many random actions, transitions, rewards*
	- TD target depends on *one* random action, transition, reward

**Bootstrapping and Sampling**
Bootstrapping: updates involves an estimate (use our own value function as a target)
- MC does not bootstrap
- DP bootstraps (exhaustive tree search doesn't)
- TD bootstraps

Sampling: updates samples an expectation
- MC samples
- DP does not sample
- TD samples

![[Pasted image 20250712153205.png]]

### $TD(\lambda)$ 

At $TD(0)$
- Temporal Difference from `1` step in the future, i.e. update current state's value function using next `1` state reward + value function at that state

At $TD(1)$
- Temporal Difference from `1` step in the future, i.e. update current state's value function using next discounted `n` state reward + value function at that state `n+1`

At $TD(\infty)$ 
- Just Monte Carlo Learning

### Backward View $TD(\lambda)$

Eligibility Traces:
- A credit assignment problem: did A or B cause outcome C?
- *Frequency Heuristic*: Assign credit to most frequent states
- *Recency heuristic*: Assign credit to most recent states
- Eligibility traces combine both of these heuristics

![[Pasted image 20250712210000.png]]
- Jump + exponential decay every time a state is stopped

Procedure:
- Keep eligibility trace for every state $s$
- Update value $V(s)$ for every state $s$
	$\delta(t) = R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$  <- error from TD(0)
	$V(s) \leftarrow V(s) + \alpha \delta_t E_t(s)$ <- value function update proportional to Eligibility + error

TD($\lambda$)
- gamma = 1: Backward view = both Exactly equal to TD(0)
- gamma = 1: Backward view = forward view TD($\lambda$)

# Lecture 5: Model-Free Control

### Introduction

Why should we care about these problems?
- Almost all interesting problems have underlying MDPs:
	- MDP is unknown or
	- MDP is known is big to be useful

**On-policy** learning:
- "Learn on the job"
- Learn about policy $\pi$ from experience sampled from $\pi$

**Off-policy** learning:
- "Look over someone's shoulder"
- Learn about policy $\pi$ from experience sampled from $\mu$ 

What's wrong with just 'naively' using Monte-Carlo policy eval + Greedy policy improvement in an iterative loop to solve control?
1. To act greedily wrt policy function, we still need to know the underlying dynamics $P$. Essentially needs to 'roll' the model forward to find the action to take
![[Pasted image 20250712214924.png]]

Alternative: Use action-value because Q doesn't depend on the model
![[Pasted image 20250712215030.png]]

2. Exploration issue: acting greedily wrt policy function, means that state space never gets explored

Greedy Exploration:
- Simplest idea to ensure continual exploration
- All $m$ actions tried w/ non-zero probability
- With probability 1 - epsilon: choose greedy
- With probability epsilon: choose action at random
![[Pasted image 20250712220023.png]]

**Theorem**: For any $\epsilon$-greedy policy $\pi$, the $\epsilon$-greedy policy $\pi'$ with respect to $q_\pi$ **is an improvement**
- Over 1 step, its trivial that the $\epsilon$-greedy >= original policy
- By telescoping & bellman equation, over all steps: $\epsilon$-greedy >= original policy

### On-Policy Monte-Carlo Control

**Monte-Carlo Policy Iteration**
- Policy evaluation: Monte-Carlo policy evaluation
- Policy improvement: $\epsilon$-greedy policy improvement  

Idea:
- We don't have to fully evaluate the policy each time (before running improvement steps)
- Evaluate the policy upon each episode (so approach $q_\pi$) but then greedily improve the policy

Definition: *Greedy in the Limit with Infinite Exploration (GLIE)*
- All state-action pairs are explored infinitely many times
	$N_k(s,a)$ = inf as k increases 
- The policy converges on a greedy policy (eventually)
![[Pasted image 20250716194205.png]]
Example:
- Can achieve this by choosing a $\epsilon$-greedy policy and decay $\epsilon$ over time

**GLE Monte-Carlo Control**:
- Sample kth episode using policy $\pi$: generates state, action reward, etc..
- Do incremental update to $Q(S_t, A_t)$ 
![[Pasted image 20250716194429.png]]
- Improve the policy based on new action-value function
	$\epsilon \leftarrow 1/k$
	$\pi \rightarrow \epsilon$-greedy(Q)
- In practice, since $\pi$ changes on every episode, we only really need $Q$ and $\pi$ is implicitly defined

This our first full solution!
- Putting it in any MDP and letting it run, will get a solution
- GLE Monte-Carlo control converges to the optimal action-value function:
	$Q(s,a) -> q_*(s,a)$ 


### On-Policy TD Learning

MC vs. TD:
- TD has:
	- Lower variance
	- Online
	- Incomplete sequences

Natural Idea:
- Use TD instead of MC in our control loop (TD to $Q(s,a)$)
- Apply $\epsilon$-greedy policy improvement
- Update the value function greedily after just one step of data (instead of 1 episode)

Algorithm name: *SARSA*
- $(S,A)$ -> sample environment (gives reward $R$) -> State $S'$ -> sample action from policy -> $A'$

	$Q(S,A) \leftarrow Q(S,A) + \alpha(R + \gamma Q(S',A') - Q(S,A))$
- Move Q value in direction of error between TD target and Q value
- Then act $\epsilon$-greedy wrt Q again
- Q values implictly represent our policy

**SARSA Algorithm**

1. Initialize $Q(s,a)$ (this is a lookup table for now) for all states and actions. Set $Q(terminal-state, all-actions$ ) = 0
2. Repeat for each episode:
	1. Initialize state S
	2. Choose A from S using policy derived from Q (greedily)
	3. Repeat for each step in episode:
		1. Take action A, observe R, S'
		2. Choose A' from S' using policy derived from Q (greedy policy improvement)
		3. $Q(S,A) \leftarrow Q(S,A) + \alpha(R + \gamma Q(S',A') - Q(S,A))$ (re-evaluate policy)
		4. $S \leftarrow S'$, $A \leftarrow A'$
		until S is terminal

### Off-Policy Learning

Everything above: policy we're following is the policy we're learning about

Idea:
- Follow some policy $\mu$ that we pick actions from in the environment
- Evaluate some other policy $\pi$ to compute $v_\pi$ or $q_\pi(s,a)$ 

Why off-policy?
1. Learn from observation or other agents
2. Re-use experience generated from old policies. Ex: using general policy iteration, but we should be able to reuse old data to evaluate current policy.
3. Learning about *optimal* policy while following *exploratory* polices
4. Learn about *multiple* polices while following *one* policy

Importance Sampling:
- Estimate the expectation of a different distribution
![[Pasted image 20250717032128.png]]

We can then importance sample across the entire trajectory
For Monte-Carlo:
- Use returns generated from $\mu$ to evaluate $\pi$ 
- Weight return $G_t$ is modified according to similarities between policies
- In practice, this is useless because after so many steps, the odds of the optimal policy having the same trajectory as the exploratory policy is vanishingly small
	- i.e. "The paths are too different for us to learn anything about policy of interest"

For TD:
- Bootstrapping over 1 step, so much more viable
- Use TD targets from $\mu$ to evaluate $\pi$
- Weigh the TD target by importance sampling
- Only 1 single importance sampling correction
- Slightly increases variance

**Q-Learning**

Consider off-policy learning of action-values $Q(s,a)$
- No importance sampling is required
- Next action is chosen using behaviour policy $A_{t+1}$ from $\mu$
- But also consider alternative successor action $A'$ from target policy $\pi$
- Update $Q(S_t, A_t)$ towards values of alternative action
- 


### Summary