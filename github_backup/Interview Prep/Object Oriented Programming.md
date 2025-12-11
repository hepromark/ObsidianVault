### Encapsulation

**Def:** Bundling data & methods that operate on data into a single unit -> the object

**Purpose**: To hide internal state & force interaction to be performed through objects method (defined interface). Increases security & modularity

**Example**: Private variables with public set/get methods

### Abstraction


**Def:** Hiding complex implementation details, only showing essential features of objects

**Purpose**: Reduces complexity by exposing what is necessary to users

**Example**: `NonLinearOptim` class exposing a `solve()` method only, abstracting away internal implementations.

### Inheritance

**Def:** A child class inherits properties and behaviours from its parent class

**Purpose**: Reusable code, hierarchical relationship between classes

**Example**: `Truck` inherits from `Vehicles`

### Polymorphism

**Def:** Ability of diff objects to respond to same methods in diff ways

**Purpose**: Flexibility in code, allows similar classes to perform *similar* functions via the same method call

**Example**: `draw()` method on base class `Shape` is implemented differently in `Circle`, `Triangle`, and `Square` classes
