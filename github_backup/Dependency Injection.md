### Overview
Dependency Injection (DI) is a design pattern
- Manages how components in a system get objects (dependencies) they need to function
- Not actually hard coding dependencies themselves

**Idea**
- Instead of class creating / finding dependencies, they are injected from outside
- Makes code more modular, testable
- Can switch out dependencies without changing the class that uses it

**Example of DI**
```python
class Engine:
    pass

class Car:
    def __init__(self, engine):
        self.engine = engine  # Engine is injected externally
```

