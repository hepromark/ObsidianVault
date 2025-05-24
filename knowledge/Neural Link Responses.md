- Created an inhouse discrete event simulation system that outperforms all the other options my startup had to test algorithms & code changes

- I used dynamic programming & spatial temporal grids to efficiently simulate warehouse robot movements

- Hundreds of times faster than the dockerized full simulation, this exposes an API for our developers to plug and play different path algorithms and warehouse layouts to check performance

- Led a mechanical project group to go beyond expectations by implementing closed-loop control & vision systems

- The course project expected a simple drawing robot that takes simple directional commands and draws straight lines. 

- I designed & implemented a PID control for the motors, a more generalized movement system that allowed for curved lines using splines & a velocity motion profile.

- I created an image pipeline in python to convert digital pixel images into a set of drawable contours for the robot using a sequence of edge detections, filtering, and contour detection/pruning which resulted in the robot being able to draw any digital image. 

- Scaled Task Planner component for my startup's robotic storage system massively and raised system reliability, making it deployable to our first customers.

- The Task Planner reactively schedules storage tasks for our robots to fulfill customer item orders, but it did not account for potential real-world failures (ex: a robot unexpectedly fails, an item is damaged and a replacement is needed, robot collisions, etc.) 

- I designed & implemented new fallback behaviours for mitigate these cases- which allowed for a higher number of concurrently operating robots due to less catastrophic robot failures (from 6 to 60) and a longer operational time (from several hours to 2 or 3 days without human intervention). 
