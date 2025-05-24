### Tuning
There are 3 main ways to tune a PID:
1. Manual Tuning
Requires a lot of human involvement and general rules of thumbs, less rigorous math:
- Set I and D to 0
- Increase (P) gain until loop oscillates at a constant amplitude
- Halve the P and then increase Integral (I) until the stead-state error is eliminated at a desired rate
- Increase D term to minimize overshoot and quickly dampen the oscillations

2. Tuning Heuristics
Uses existing formulas to set the P, I, and D values to, depending on the oscillation period of when just P term

### PID Improvement techniques
Low-pass filtering:
- A filter that lets low frequency signals through but blocks high frequency signals above the set threshold
- To filter out large spikes in error that will explode the D term

Anti-windup:
- Issue with the integral term, is the source of the “over-shooting”
- As the system gets closer to the setpoint, the error in the I term is still being accumulated
- This leads to the I term causing the controller to overshoot since once the system is AT the setpoint the accumulated error is still > 0
- Clamping technique → clamps the I term to a max/min value realistic to the physical system