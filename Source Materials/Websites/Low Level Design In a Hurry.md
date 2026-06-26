Tags:
- [[Low Level Design]]
---
## Delivery Framework
- **requirements**
    - primary capabilities
    - state transitions
    - error handling
    - what's in scope
- **entities & relationships**
    - entities: meaningful nouns in the domain
    - relationships: how they interact
- **class design**
    - for each entity: define state and behaviour
    - look at requirements to determine state each entity needs (attributes), then determine what does entities need to do (methods)
- **implementation**
    - implement the major methods in pseudocode
    - start with happy path, then dive into edge cases
    - verify by stepping through the logic
- **(optional) extensibility**

## Design Principles
- 3 most important principles
    - **Keep It Simple, Stupid**: only add complexity when the simple solution stops working
    - **Don't Repeat Yourself**: extract _conceptually_ similar code into its own function
    - **You Aren't Gonna Need It**: _think_ ahead, don't _build_ ahead; Design to be extensible, but don't implement the extensions unless required
- SOLID
    - **Single Responsibility Principle**: Class should have 1 reason to change
    - **Open/Closed Principle**: Should be able to add new behaviour without changing existing code. Usually involves implementing a common interface
    - **Liskov Substitution Principle**: Should be able to substitute the subclass anywhere the parent is used. Common code smell is `if isinstance(child, Parent)` (suggests child can't be subbed in for parent)
    - **Interface Segregation Principle**: Don't force classes to implement methods they don't need, split the interfaces up
    - **Dependency Inversion Principle**: Let higher-level classes (e.g. business logic) define an interface and let the lower level classes conform to that. Ensure lower level depends on higher level, not the other way around
## OOP Principles
- **Encapsulation**: hide state, expose behaviour
- **Abstraction**: hide the _how_, expose only the _what_
- **Polymorphism**: instead of checking types, let each object handle itself
    - gives more extensibility (OCP) at the cost of being harder to understand initially
- **Inheritance**: Only use this when sharing a stable implementation where everything needs to be shared. Default to composition instead

## Design Patterns (the actually important ones)
- **Factory**: Handles creation of the correct type of object
    - e.g. `AnimalFactory` that takes in `species: str` and returns an instance of the appropriate subtype of `Animal`
- **Builder**: Create complex object with many optional parts, step by step
    - nested class that contains an instance of the main class (with default values)
    - each method sets a field of that instance
    - a `build()` method returns that instance
- **Singleton**: ensure only 1 instance exists
    - e.g. DB connection, config
    - for other, normal cases, prefer to just explicitly pass shared objects through constructors
- **Decorator**: Add behaviour to an object without changing class / adding subtypes
    - implements some interface I and takes in an instance satisfying I in the constructor
    - each method can then use the internal instance's method, with additional logic
    - good for layering multiple additional behaviours (e.g. timing, logging, caching, etc.) without having to define subclasses for every combination
- **Facade**: orchestrator that hides complexity of multiple classes
- **Strategy**: Replace conditional logic with polymorphism
- **Observer**: Let multiple objects subscribe to events and get notified when they happen
    - observers implement a handler method
    - the subject maintains a list of observers and exposes methods to subscribe and unsubscribe
    - when an event happens in the subject, it loops through its list of observers and calls their handler method
- **State Machine**: Handle the behaviour of states and their transitions
    - Given a class C with states and transitions, define a CState interface containing methods representing actions that could cause state transitions
    - Give C a CState instance as a field
    - The methods should take in an instance of C and update its internal CState according to the domain rules
    - implement a concrete CState class for each actual state, defining the changes and the state transitions for each action

---
Source: https://www.hellointerview.com/learn/low-level-design/in-a-hurry/introduction
