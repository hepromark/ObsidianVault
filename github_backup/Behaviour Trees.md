### Overview

Behaviour Trees are a *Task Switching Structure* that, when given a set of available actions, decides *what* action and *when* to switch.
Use cases are similar to FSMs, but behaviour tree offers distinct advantages in some scenarios.

Behaviour Trees activates every 'tick':
- A tick starts at the root of the tree and propagates down the tree
- Interior nodes are logical operators (conditions), which determines which branch the ticking goes down
- Leaf nodes are action/behaviour nodes, which does some task.
- Subsequent ticks propagates down the conditions, changing executing leaf nodes if the conditions change.
### Defining Properties

1. Modularity
- The tree and node style data structure is highly modular.
- Similar nodes can be based off of the same 'basic' behaviour.

2. Hierarchical
- The tree design is inherently hierarchical: entire subtrees can be abstracted away if needed.
- Varying levels of details depending on max depth from the root.
- When developing any particular nodes, usually we don't care about if the remaining tree is implemented:
	- As long as root & child nodes mocked, behaviour will work
	- Un-implemented subtrees can easily be mocked

3. Interrupts / Fallback Handling
- The tree 'ticking' has built-in handling of interrupts / fallbacks.
- When operating conditions change, tree easily switches behaviour (execution path).
- "Interrupts" is just a re-pathing of the ticking.
- "Fallbacks" means leaf nodes earlier (more left) is ticked instead of the current node due to changing conditions.

### Tree Components

Fallback `?`:
- Ticks all its children until one return `SUCCESS` 
	- If at least one child returns `SUCCESS`, Fallback returns `SUCCESS`
	- If all children return `FAILURE`, fallback returns `FAILURES`
- Logical `OR` of the child nodes.

Sequence `->`:
- Ticks all its children until one return `FAILURE`
	- If all children returns `SUCCESS`, 
- Logical `AND` of the child nodes.

![[Pasted image 20250108161802.png]]
### Fixed vs. Utility Trees

In fixed (traditional) behaviour trees, the ordering of sibling nodes in a tree is **important and fixed**.
- Ordering matters for sequences: condition checks usually first, and then behaviour that would affect that condition
- Ordering matters for fallbacks: 'priority' of the sibling nodes, ex: eat apple before drinking water or vice versa?

Fixed order drawbacks:
- Inability to pick the 'better' behaviour at runtime; different scenarios cause different actions to be more beneficial

If the 'utility' of each option can be measured: 
- we can pick the highest utility
- condition nodes can be ignored since we'll factor conditions into utility

Utility for an action:
- Fast is good -> 1 / completion time
- Cheap is good -> 1 / cost
- Success is good -> P(success)
- Reward is good -> solve tree search problem

Aggregating utility up a tree:
- Cost-based utility: 
	- Fallback nodes: smallest child
	- Sequence nodes: sum of child nodes
- Success:
	- Fallback nodes: $\text{Product of } p(s_i)$ where each child node has $p(s_i)$ success probability
	- Sequence: 1 - product of ($1 - p(s_i)$)

### Designing Behaviour Trees

4 Design criteria for a bug-free behaviour tree:

1. Action achieves post-condition in a finite time
	- Ex: move to object satisfies robot near object

2. Action does not violate key previously achieved sub-goals
	- Ex: move to object doesn't violate in-safe-area
	- If these sub-goals are violated, the tree has to 're-tick' these earlier behaviours to make subgoal achieved again
	- May cause a infinite loop of sub-goal violation

3. Action does not violate conditions NEEDED for earlier actions
4. Action does not invoke previously failed subtree

### Finite State Machines Comparison

Why BTs over FSMs?
- BTs are more modular, logic can be re-used easier.
- Abstraction (and thus collaborative work) can be done easier.
- BTs scale better; extra nodes in the tree impact only the sub-tree containing it (behaviour is still very similar to before adding).
	- For FSMs, new nodes mean potentially new connections to every existing node.

### RL in Behaviour Trees
   
- [ ] Finish this section

End-to-end reinforcement learning behaviour model:
- Nearly optimal performance in certain scenarios
- No hand-tuning / manual design needed
- No safety or convergence GUARANTEEs 

BTs:
- Modular, hierarchical
- Transparent logic that can be reasoned about
- Safety GUARANTEEs
- Convergence GUARANTEEs

How to learn a BT?
1. Learn the topology of the space(ex: w/ genetic algorithms)
2. Replace a subtree with RL (use modularity) policy
3. Learn interior nodes (not a subtree, but just 1 node)

