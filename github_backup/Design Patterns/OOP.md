A programming paradigm based on concepts of “objects”, which can contain:

- Data → data in form of fields (attributes or properties)
- Code → procedures / methods
- OOP objects have methods which are attached to them, where this method can access and modify the object’s data fields

It organizes software design around data (objects) rather than functions or logic

- OOP focuses on objects that developers want to manipulate, instead of the logic needed to manipulate
- Common 1st step in OOP is to do data modelling → putting all the data dev wants to manipulate, put them in objects and design how they would interact with one another

Structures of OOP:

1. Classes
    a. user-defined data types, which are blueprints for individual objects
    b. Ex: `Person` class would have name, age, etc.
2. Objects
    a. specific **instances** of a class created with its own **specifically defined** data
    b. Ex: `person1` is a specific instance of the `Person` class, with name = “John” and age = 69
3. Methods
    a. Defined in the class template
    b. Functions that are defined inside a class
    c. Each method starts with a reference to an instance object
4. Attributes
    a. Defined in the class template and represent the state of an object
    b. Data for an object stored inside attribute fields

### Main Principles of OOP:

1. Encapsulation
    a. All important information contained inside an object, only some needed info exposed
    b. Implementation and state of each object held in the class
    c. Other objects can’t access to this class, or authority to make changes
    d. Outside users / objects can only call public methods
    e. Overall data hiding makes program more secure and avoids unintended data changes
2. Abstraction
    a. Objects hide unnecessary code implementations away from outsiders
    b. Only reveal internal stuff when it is relevant
    c. Developers can make changes to internal system, which won’t break any outside use → the internal systems were always hidden, as long as the function gives the same output to the same input everyone is happy
3. Inheritance
    a. Classes can reuse code from other classes
    b. Relationships and subclasses between objects can be found, devs can reuse common logic
    c. Forces thorough data analysis, and reduces development time through reusing code
4. Polymorphism
    a. Objects are designed to share behaviours and can take more than 1 form
    b. Program picks the which implementation to use for that particular object → ex: child and parent both implements the method `foo(int a)`, when `myChild.foo(1)` is called the child implementation is chosen
    c. Each of these classes / subclasses can provide its own implementation of methods
    d. Usually the most specific implementation (child > parent > grandparent) is chosen

Pros:

- **Modularity**: Encapsulation makes objects self-contained, troubleshooting easier
- **Reusability**: Code can be reused via inheritance
- Easily **upgradable**: Abstraction and encapsulation makes updating some parts easy without breaking others
- **Security**: Abstraction and encapsulation means complex code is hidden, software maintenance easier, and internet protocols are protected

Cons:

- Code can be more complex, harder to understand → debugging is harder
- Consume more resources (CPU & memory) than other paradigms
- Scalability vs. Simplicity → too many fragile / complex internal connections and inheritance can make development very difficult