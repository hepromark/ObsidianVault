Gavriel State: transgaming ports -> nvidia startup acquired

USD - universal scene descriptor from pixar thats now being piggy backed off

Main:
1. Geomeric fabric: training wheels on controller (puts real time limits on controller we can send to controller)
	- Guarantees safety bounds
	- Imposes guidelines / join constraints

Wait isn't geometric fabric on the sim side (external to policy) why did it work in real

Newton:
- Open source
- GPU accelerated (physX is old, have new HW features now)
- Differentiable (for policy training, auto-tuning + system id)
- MuJoCo Support
- Supports OpenUSD
- Multiple Solvers:
	- MuJoCo Warp
	- Disney solver
	- Custom solver
- Collision libraries

Warp:
- Write GPU kernels in python (easy to write hardware accelerated code)

Ideas:
1. seperate physical models from numerical methods to solve it
2. flat data (arrays) > OOP
3. Avoid hidden state

Coupling:
- 1 way: Is it still parallelised? Since it's sequential
- 2 way coupling: have solvers iteratively loop, or something that does both at the same time
- Disney is doing both direct solving

Qs:
- Support for deformable / soft / etc. solids, not just hard models
- Is the fabric a 'model' w/ parameters?

