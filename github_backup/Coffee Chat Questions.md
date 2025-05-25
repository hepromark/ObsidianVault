#### Skills Matching:
- Planning algorithms
- Non-convex optim / Genetic Algorithms
- Multi-Agent Robotic Systems
- Robotic Storage Systems
- Scaling startups
- Autonomy


Actual questions I have:
1. How diverse, in-terms of educational backgrounds, are the engineers in Robotics? From my experience talking to people in other software fields, it seems like the educational barrier to entry for the robotics industry is much higher. A lot of positions (fulltime & internships) I want to work in has positioned filled by Master's and pHD. In your experience, am I, as an undergraduate intern, locked out of most of the available position due to lack of higher degrees?
2. Can you tell me if your past job experiences have funnelled you towards this current position, or did you arrive here somewhat unexpectedly? From your linkedin work history, it seems that ... . I've come to gain experience in several adjacent fields to motion planning / controls / autonomy, but I'm wary that my lack of specialized expertise would stop me from getting [insert role]. 

3 bullet points:
```
- Created a system to algorithmically explore & test new changes to my startup's product without having to deploying to production nor running any extensive dockerized simulations.

- The simulations that my startup used is heavy and slow, requiring Quality engineers to setup and run test cases for- making it unfriendly for developers who want to verify "intuitive optimizations".

- My system is a lightweight python sim that discretizes actions the storage robots can make at any time step and exposes all important decision-making logic as simple interfaces which can be easily modified. It uses a modified A* for robotic planning, with a global brute-force planner using dynamic programming to simulate "optimal conditions" under any new parameters given.

- I directly integrated all new features that aims to improve system performance into my lightweight sim, resulting in 5 major system improvements being greenlit for development and several proposed changes being stopped for re-evaluation.

- Led a mechanic project group to go beyond expectations by implementing closed-loop control & vision systems

- The course project expected a simple drawing robot that takes simple directional commands and draws straight lines. 

- I designed & implemented a PID control for the motors, a more generalized movement system that allowed for curved lines using splines & a velocity motion profile.

- Scaled Task Planner component for my startup's robotic storage system massively and raised system reliability, making it deployable to our first customers.

- The Task Planner reactively schedules storage tasks for our robots to fulfill customer item orders, but it did not account for potential real-world failures (ex: a robot unexpectedly fails, an item is damaged and a replacement is needed, robot collisions, etc.) 

- I designed & implemented new fallback behaviours for mitigate these cases- which allowed for a higher number of concurrently operating robots due to less catastrophic robot failures (from 6 to 60) and a longer operational time (from several hours to 2 or 3 days without human intervention). 
```
