#### What is Control?
The usage of feedback & algorithms in engineered systems

#### What is feedback?
A situation where two or more dynamical systems are connected together, st each system impacts the other and their dynamics are strongly coupled
- Individual reasoning is hard since 1st system impacts 2nd, and etc.
- Better to think about them as a whole

#### What is Feedfoward?
Feedback is reactive- error happens, control happens to correct for it. We can instead take action *before* error happens:
- Shapes response to command signals
- Requires good process models 
- Tries to match current output -> command

#### Positive Feedback
An increase in some variable / signal leads to that same quantity being further increased through its dynamics
- Usually undesirable in engineering systems, negative feedback more common
- Ex: in nature very useful

### State Space Models

The current controls models take from mechanics + electrical modelling

### Block Diagrams

### Dynamic Behaviours

**Finite Escape Time**: Solution goes to infinity within a finite amount of time
**Non-unique Solution**: Same initial condition may lead to different system behaviours

**Equilibrium Point**: A stationary condition for condition for dynamics
- ${dx}/{dt} = F(x)$ has stationary state $x_e$ if $F(x_e) = 0$
- They define points of constant operating conditions
- 0, 1, or many equil points

**Stable**: A solution is stable if if other solutions that start near a stay very close to a for all time

**Locally Stable in radius r:** All solutions within r stay stable
**Globally stable**: All solutions are stable

note:
- Ask chat what other ways to approach the bicycle model project (ex: dynamics POV)
- How to control that car (feedforward vs. feedback)
- Revise MPC
### Linear Systems

Can completely characterize a linear system using 2 concepts:
1. Matrix exponential
2. Convolution equation

If both linear + time-invariant:
- The response to an arbitrary input is completely characterized by response to step inputs / response to short "impulses"

### State Feedback

### Output Feedback

### Transfer Functions

### Frequency Domain Analysis

### Frequency Domain Design

### Modelling Uncertainty

