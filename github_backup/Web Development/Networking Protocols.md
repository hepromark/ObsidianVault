
### Communication Protocols

A communication protocol is system of rules that enables 2 or more devices to communicate.
- The protocol defines the rules, syntax, semantics, synchronization, and recovery methods for communication
- Protocols may be implemented by software or hardware or both

### MQTT
- Standards-based messaging protocol (set of rules) for machine-to-machine communication
- Supports device → cloud and cloud → device
- Is standard for IoT communication, because:
- Lightweight for microcontrollers, as light as 2 bytes & also small message headers
- Scalable to millions of IoT devices by design & minimal power usage
- Reliable: reduce time for IoT to connect to cloud & service levels:
    - at most once
    - at least once
    - exactly once
- Secured for easy encryption and auth with OAuth / etc.
- Works on the publisher/subscriber pattern to decouple message sender from reciever
- 3rd component (message broker) handles communication between Pubs and Subs
    - Space decoupling: pub and subs don’t share network location (ex: IPs or Ports)
    - Time decoupling: don’t run / have connectivity at the same time
    - Synchronization decoupling: pub and subs can send & receive without interrupting each other (ex: sub doesn’t have to wait until pub sends message)
- MQTT Clients: any device that runs MQTT library (can be pub or sub) → if it communicates w/ MQTT its a client
- MQTT Broker: backend system that handles messages between different clients:
    - auth for clients
    - passing messages to other systems
    - handling missed messages & client sessions
- MQTT Topic: keywords that the broker uses to filter messages for clients
    - organized hierarchically (like a file system)
- MQTT Publish: clients publish messages containing topic and data in byte format
    - client chooses data format → .txt, binary, XML or JSON
- MQTT Subscribe: clients send a subscribe message to MQTT broker with unique ID and a list of subscriptions