Sources:
- https://medium.com/bytebytego-system-design-alliance/the-architects-blueprint-understanding-software-styles-and-patterns-with-cheatsheet-5c1f5fd55bbd

# Architectural Styles

Architectural Styles: 
- High level strategies / abstract frameworks for a whole *family* of systems
- Ex: Layered, Event-Driven, Micro services

Architectural Patterns:
- More concrete & specific to a particular problem / module within the system
- Provides a structured solution to architectural issues
- Ex: Model-View-Controller (MVC), Publisher-Subscriber, and Serverless

> Architectural styles describe the **overarching structure of the system**, while architectural patterns **address specific design problems** that might arise within this structure.

![[Pasted image 20250529095934.png]]

### Layered Architectural Style

One of the most common arch patterns, used in web-apps and enterprise apps
**Principles:**
- Separates concerns into distinct layers
- ex: Presentation, business logic, data storage

**Strengths:
- Easy to understand, test & maintain
- Independent layers

**Weaknesses**:
- Performance overhead
- Multi-layer changes hard to implement

**Applications:**
- Web apps, enterprise apps

**Anti-patterns**:
- Circular dependencies
- Skipping layers

#### Examples
1. Layered pattern
	- Each layer has specific responsibility
	- Communicate with adjacent layers only
![[Pasted image 20250529121755.png]]	
2. Onion Pattern
	- Separation of concern
	- Concentric layers, outer layers depends on inner layers but not vice versa
	- Changes on UI, infra, external services should have no change on business logic
	- Good testability, maintainability
	
![[Pasted image 20250529121811.png]]

### Component-Based Architecture Style
Separation of concerns regarding wide-range functionalities in a system

**Principles:** System is made up of a loosely-coupled, reusable components
**Strengths:** High reusability and flexibility to move components around
**Weaknesses**: Interactions between component difficult
**Applications:** Distributed systems, web apps, desktop apps
**Anti-patterns**: Overly large components, redundant components

#### Object-Oriented Pattern
- Objects contains data and defines how to operate on that data

#### Microkernel Pattern
Separates a minimal functional core (microkernel) from extended functionalities:
- Other features are implemented as plugins to the microkernel
- Core doesn't change while system extended
![[Pasted image 20250529122728.png]]

#### Plug-in Pattern
- Adding new functionality to app by adding new modules or plugins
- New modules integrated via a standard interface

### Service-Orientated Architectural Style

