# Overview

Communication protocols handle:
- Data formatting
- Timing & sync
- Error detection & correction
- Device addressing
- Access control (on shared lines)

**Types of protocols**
1. Serial Protocols
	Close communication between chips or modules on the same board/system.

2. Network Protocols 
	Long-distance, multi-device communication

# Serial Protocols

A way to transmit data (one bit at a time) sequentially, over a communication line.
- Direct comm between 2 devices -> point to point
- Few wires / lines needed
- Simpler to design into embedded systems
- Good at short - med distances
- Ex: handles comm between MCUs, sensors, and actuators

### UART - Universal Asynchronous Receiver/Transmitter

![[Pasted image 20250527102458.png]]

**Overview**
- Simple and cheap way for 2 embedded devices to communicate
- Asynchronous
- 1 master -> 1 slave

**Features**
- Baud rate defines speed: 9600, 115200, etc.
- Simple & widely supported
- Common for debugging, consoles, Bluetooth, GPS modules

`[Start Bit] [Data Bits] [Optional Parity Bit] [Stop Bit]`
- Start Bit - Held at high when not transmitting data. Pull down to LOW for 1 clock cycle before transmitting happens. Receiver starts reading after the LOW is detected (line sampled according to baud rate)
- Data Bits - 5 to 8 bits long of data, LSB first
- Parity Bit - Even or odd, as an error checker
- Stop Bits - Pull up to HIGH to signal end of transfer

**Wires**
- TX (Transmit)
- RX (Receive)
- optional GND for a shared ref voltage
- Each TX connects to RX on the other device, and vice versa

**Example**
- Console debug from micro-controller to PC terminal

### SPI - Serial Peripheral Interface

![[Pasted image 20250527145123.png]]

**Overview**
- More expensive but faster & more flexible communication
- Synchronous - shared clock
- Full duplex - data set & received simulatenously
- Supports 1 master -> multiple slaves (master is device that generates clock signal)

**Features**
- Fast transfer ~10Mbps range

**Wires**
- Needs more wires than UART or I2C -> usually 4 wires
- Clock
- Chip Select (CS)
- Master out slave in (MOSI)
- Master in slave out (MISO)


**Example**
- Master sends clock signal and selects target slave by CS signal
- Both MOSI and MISO lines are used simultaneously
- Clock edge syncs the shifting and sampling of data (rise or fall works)
- Ex: Flash memory comm, or high-speed sensor

### I2C - Inter-integrated Circuit

**Overview**
I2C was designed for short-distance comm between multple chips inside 1 device.

**Features**
- Synchronous Port structure (so shared clock)
- 2 wires only, so easier for stuff like PCB routing esp. w/ many peripherals

**Characteristics**
- Generally a cheaper way than SPI to do multi-master / multi-slave
- I2C bus uses 2 lines, SDA and SCL to transfer info between devices connected to the bus
- Synchronous, but only half-duplex
- Multi master, multi slave possible
- Each device has unique address (like IP addr)
- Slower than SPI

**Wires**
- SDA (Serial Data)
- SCL (Serial Clock)

**Example**
![[Pasted image 20250525124922.png]]


|           | UART                  | I2C                                           | SPI                                             |
| --------- | --------------------- | --------------------------------------------- | ----------------------------------------------- |
| Wires     | 2 -> TX, RX           | 2 -> SDA, SCL but multiple devices on these 2 | 4+ (MOSI, MISO, SCLK, Csx)                      |
| Speed     | ~ 1 Mbps              | 400 khz -> 3.4 Mhz                            | > 100 Mbps                                      |
| # Devices | 1 to 1                | Many (with addressing)                        | Many (but need 1 CSlines per master-slave pair) |
| Duplex    | Full, Async           | Half, Sync                                    | Full, Sync                                      |
| Use Case  | Debug, GPS, Bluetooth | Sensors                                       | High speed sensors, flash drives                |
| Cost      | Low                   | Medium                                        | High                                            |

What type of drivers are used for I2C and why?
- Uses Open Drain drivers, since multiple devices share a line

How is bit sync achieved in I2C?
- A dedicated clock line used- bit rate & phase info known from the clock signal

How is byte sync achieved in I2C?
- Shared clock only pulses when data/address are being sent on the SDA line
- Otherwise clock idles HIGH

How is block sync achieved in I2C?
- Out of band signalling (since block info is not inside msg): 
	- Block start: idle high SCL + SDA low
	- Block end: idle high SCL + SDA high

# Networked Protocols

### CAN - Controller Area Network

A standard protocol for in-vehicle communication for automotive industry. It is a real-time & fault-tolerant communication protocol designed for microcontrollers & embedded devices.
- No host computer needed
- Essentially the nervous system of the car that enables comm

**ECU**
- Electric Control Units that controls certain functionalities (ex: brakes, engines, etc.)
- any ECU on the CAN bus can broadcast info or sensor data

![[Pasted image 20250525130948.png]]
1. Microcontrolelr
	- Interprets incoming CAN msgs
	- Decides what to transmit
2. CAN controller
	- Controller typically integrated on MCU & ensures all comms adhere to CAN protocol
	- Handles encoding, error detection, arbitration, etc. to abstract the Microcontroller
3. CAN transceiver
	- Connects CAN controller to physical CAN wires
	- Converts controller data into differential signals for the bus
	- Electrical protection

**Benefits of the CAN bus**
- Simple & low cost -> all ECUs communicate via a single CAN system
- Easy access -> 1 point of entry to communicate with all network ECUs for easby debug, logging, configs
- Extremely robust to electrical disturbances & interference
- Priority -> CAN frames has ID based priority for important components to get bus access

**CAN types**
![[Pasted image 20250525131514.png]]

**CAN Frame**
- Standard CAN frame has 11 bits ID
- SOF: Start of Frame bit (dominant 0) to signal other nodes that CAN node wants to talk
- ID: Lower IDs have higher priority
- RTR: Is a node sending data or requesting dedicated data from another node
- Control: Specifies control info, ex: length of data in bytes
- Data: Actual bytes being transformed, which includes CAN signals to be decoded for info
- CRC: Cyclic Redundancy check for data integrity
- ACK: If node has acknowledged and recieved data correctly
- EOF: End of frame marks end of CAN frame

![[Pasted image 20250525131924.png]]

**Example**
- ECU wants to write to the CAN-bus
- Tries to transmit its ID to the CAN-bus lines
	- Will be overpowered by higher priority IDs (lower voltages)
	- Knows it won arbitration if its ID can be read back from the lines
- Once won, it transmits the frame

### LIN - Local Interconnect Network
