# Tuning
There are 3 main ways to tune a PID:
1. Manual Tuning
Requires a lot of human involvement and general rules of thumbs, less rigorous math:
- Set I and D to 0
- Increase (P) gain until loop oscillates at a constant amplitude
- Halve the P and then increase Integral (I) until the stead-state error is eliminated at a desired rate
- Increase D term to minimize overshoot and quickly dampen the oscillations

2. Tuning Heuristics
Uses existing formulas to set the P, I, and D values to, depending on the oscillation period of when just P term

**Ziegler-Nichols**
1. Turn of I & D components
2. Slowly increase P until process oscillates
	- Gain = Ultimate Gain
	- Period of oscillation = Ultimate Period
	
![[Pasted image 20250525145440.png]]

**Åström-Hägglund**
1. Turn off I & D
2. Selecting 2 opposing output values (ex: 100% cool and 100% heat)
3. Every time process variable hits setpoint, switch to other value
4. Calculate ultimate gain $K_u$ = $4d / \pi a$ where d is amplitude of output, and a is amplitude of process 
5. Same as Ziegler-Nichols for $T_u$ and rest of variables

# PID Improvement techniques
Low-pass filtering:
- A filter that lets low frequency signals through but blocks high frequency signals above the set threshold
- To filter out large spikes in error that will explode the D term

Anti-windup:
- Issue with the integral term, is the source of the “over-shooting”
- As the system gets closer to the setpoint, the error in the I term is still being accumulated
- This leads to the I term causing the controller to overshoot since once the system is AT the setpoint the accumulated error is still > 0
- Some ways to migitage:
	1. Clamping technique → clamps the I term to a max/min value realistic to the physical system
	2. Stop I term from increasing once output is at maximum control value

# Sample C Code
```C
// Define PID struct
typdef struct {
	float Kp;
	float Ki;
	float Kd;

	float prev_error;
	float accum_error;
} PIDController;

// Init
void PID_init(PIDController pid, float Kp, flat Ki, flat Kd) {
	pid->Kp = Kp;
	pid->Ki = Ki;
	pid->Kd = Kd;

	pid->prev_error = 0;
	pid->accum_error = 0;
}

float PID_loop(PIDController pid, float setpoint, float measurement, float dt) {
	float error = setpoint - measurement;
	float output = 0;
	
	// Proportional
	float p_output += error * pid->Kp;	

	// Derivative
	float d_output = (pid->prev_error - error) / dt * pid->Kd;
	prev_error = error;

	// Integral
	pid->accum_error += error * dt;
	float i_output = pid->accum_error * pid->Ki;

	// Total
	output = p_output + d_output + i_output;
}

// Main
int main() {
	PIDController pid;
	PID_init(pid);

	float setpoint = 10;
	float variable = 0;
	float dt = 0.1;

	for (int i = 0; i < 100; i++) {
		float output = PID_loop(pid, setpoint, variable, dt);

		// Apply output and re-measure process variable
		variable += output * dt;
	}

	return 0;
}
```
