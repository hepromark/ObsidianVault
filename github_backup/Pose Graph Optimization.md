### Overview

PGO is a nonlinear least-squares optimization problem that operates on the nodes to minimize an edge error function in a pose graph.
- Nodes = robot poses at different time stamps
- Edges = relative pose measurements between two poses

**Goal:** 
- Find best global config of poses that agrees with the edges (as much as possible)

**Edges contains:**
1. Relative pose
	- Describe a relative transformation between 2 poses (nodes)
	- $z_{ij}​=[Δx,Δy,Δθ]$ describes edge between nodes $i$ and $j$
2. Information matrix $\Omega_{ij}$
	- The confidence (inverse of covariance matrix) in this relative pose


### Mathematical Formulation
![[Pasted image 20250522185652.png]]

Essentially minimizing least squares error over all **measured relative pose** vs **predicted relative pose** 