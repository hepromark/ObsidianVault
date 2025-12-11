### Assignment #1

What are the five different methods for I/O synchronization? Briefly describe the behaviour of each I/O synchronization method

1. Blind Cycle - no sync attempt
2. Periodic polling - regular intervals of polls
3. Occasional polling - polling of status when convenient to do so
4. Tight polling - Constant polling until status changes
5. Interrupts - Status change results in interrupt to CPU

 Blind synchronisation
	- Producer generates generates new data on its clock
	- Consumer samples on same edge of its clock
	- No knowledge of data validity

Synchronous Sync
	- Prod generates on falling edge of shared clock
	- Con samples on rising edge of shared clock

Asynchronous Sync
	- Prod generates on falling edge of its own clock
	- Exists a Data Valid signal to let consumer know when data is available
	- Cons sample on rising edge of data valid signal

5. Which method of synchronization has the lowest latency from the perspective of a single device?

Tight polling - 1 less context switch than ISR, and least time between polls

6. When is it appropriate for interrupt sync to be used?

CPU needs to have other tasks to do. Otherwise, overhead over tight polling is uneccessary. 

### Assignment 2

2. Why is MAR write-only from CPU side & read-only from system side?

Because CPU only needs to write an address to MAR for the memory to respond to it. Likewise, system can't dictate what address CPU wants to read from, so it's read-only from system side. 

MDR both direction because CPU can both do a write to memory or read from memory operation. In the write case, the system must be able to read from the MDR to update. In the read case, the system must be able to write onto the MDR at which point the CPU reads it.

3. Reading from or writing to memory is much slower than a CPU internal bus cycle. Two approaches to solving this problem were discussed in the course notes: synchronously or asynchronously. What is one benefit and one drawback of each approach?

**Sync**
Benefit: Consistent transaction time.
Drawback: Must allow time for slowest possible transaction always

**Async**
Benefit: No unnecessary waiting when a traction finishes faster
Drawback: More control signals (the data valid line) needed

### Assignment #3

2. When is it not appropriate to use a push-pull driver?

When the push-pull driver is not the only driver that is writing to the line

3. Can a bus conflict occur if only tristate drivers are utilized?

Yes, the tristate drivers are still active low / active high, so if 2 tristate drivers don't have the correct enable signal setup a conflict can still occur

4. Can bus conflict w/ just open-drain or just open-source drivers?

No. Only 0 or Z, no combination of these on a line will result in conflict.

5. Why are open-drain / open-source drivers not normally used?

Slow windup times (ex: Active HIGH requires a resistor, rise time of signal will be slower than the active HIGH of a tristate)

### Assignment #4

1. Definitions

a. Skew Time: On a single data transfer process, some lines will be be slower than others. T_skew is the difference between fastest and slowest lines

b. Propagation Time: Time taken for a signal to travel along a line from instance of being sent -> first seen by the receiver. Comes from parasitic resistances / capacitance in the line. 

c. Setup Time: D-flip-flops, the min amount of time prior to active edge of clock signal where inputs are stable to ensure correct register behaviour

d. Hold Time: D-flip-flops, the amount of time after active clock signal that all inputs should be stable to ensure correct behaviour

3. Compare and contrast synchronous vs. async bus transfers

Synchronous bus transfer:
- All share the same clock
- Sender & receiver agree on 