# Job Description

Possible Intern Projects:
- parallel/distributed computing
- task graph dependency tracking and management
- real-time game engines
- 3rd-party plugins
- 3D scene manipulation
- virtual characters
- CG pipeline tool development
- image and video processing
- machine learning
- 2D and 3D user interfaces

Requirements:
- Expert knowledge and experience in either C++ or Python, or both 
- Strong oral and written communication skills 
- Eagerness to learn and take on new challenges 
- Excellent academic track record 
- Experience with Unreal Engine, Unity or other game engines (optional) 
- Experience with SQL databases (optional) 
- Working knowledge of Python Django (optional) 
- Working knowledge of AngularJS, JavaScript, HTML5 and CSS3 (optional) 
- Working knowledge of Qt and PySide (optional)

# Study Concepts

### Task Scheduling

**Task Graph:** Nodes represent a task, edges represent dependencies
- Usually a DAG (Directed Acyclic Graph)
- ex Houdini nodes: geometric operation, shader compile, physics sim update, render pass

**Game Engine Scheduling Algorithms**
https://hal.science/hal-03580775/document
[[Game Engine Scheduling]]

### Physics Based Rendering (PBR)

PBR is a shading/rendering approach to simulate light interactions w/ surfaces based on real-world physics

1. BRDF: Bidirectional Reflectance Distribution Function
	- Mathemtical model of how light is reflected at a surface ponit
	- Ex: Microfacet Models - simulate reflection assuming a surface is made of tiny mirror-like facets w/ diff orientations
	

# Houdini-specific Stuff

Houdini:
- An offline VFX render engine that supports modular, node-based workflows
- Industry standard, essentially mandatory for any sorts of CGI

Houdini-engine:
- A unit / Unreal Engine plugin that lets you import Houdini stuff in Unit / UE
- Can import Houdini assets, manipulate the asset (and have it be saved in the node params)
- Just a C API

# Group Interview Notes

![[Pasted image 20250529101818.png]]

![[Pasted image 20250529102105.png]]

![[Pasted image 20250529102434.png]]


![[Pasted image 20250529102416.png]]

![[Pasted image 20250529102625.png]]

# Questions

1. You've been at SideFX for 20 years? What was that like, how did SideFX change in those times?
2. Did you do any other internships before coming to SideFX?
3. I know the VFX industry has been pretty down lately- coming down after covid + traditional movies also down, how does SideFX handle market changes?

Nvidia Omniverse Integration project?
- Synthetic data for AI/ML?
- 

shit i like:
- "Top 100 3D render montage"
- Grandpa Lacquer painting:
	- Blackboard + eggshells
	- Colour chalks used
	- Vanish applied + polishing

![[Pasted image 20250529150944.png]]

GA params:
- pop size
- crossover probability
- mtuation rate
- num generation
- selection methods
- 