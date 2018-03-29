# design patterns
[[TOC]]
## Creatinal
### abstract factory

![=100%*](https://upload.wikimedia.org/wikipedia/commons/a/aa/W3sDesign_Abstract_Factory_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Abstract_factory_pattern

The Abstract Factory design pattern solves problems like: 

* How can an application be independent of how its objects are created?
* How can a class be independent of how the objects it requires are created?
* How can families of related or dependent objects be created?

The Abstract Factory design pattern describes how to solve such problems:

* Encapsulate object creation in a separate (factory) object. That is, define an interface (AbstractFactory) for creating objects, and implement the interface.
* A class delegates object creation to a factory object instead of creating objects directly.

### builder
![=100%*](https://upload.wikimedia.org/wikipedia/commons/8/87/W3sDesign_Builder_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Builder_pattern

The Builder design pattern solves problems like:

* How can a class (the same construction process) create different representations of a complex object?
* How can a class that includes creating a complex object be simplified?

The Builder design pattern describes how to solve such problems:

* Encapsulate creating and assembling the parts of a complex object in a separate Builder object.
* A class delegates object creation to a Builder object instead of creating the objects directly.

### factory methord

![=100%*](https://upload.wikimedia.org/wikipedia/commons/4/43/W3sDesign_Factory_Method_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Factory_method_pattern

The Factory Method design pattern solves problems like: 

* How can an object be created so that subclasses can redefine which class to instantiate?
* How can a class defer instantiation to subclasses?

The Factory Method design pattern describes how to solve such problems:

* Define a separate operation (factory method) for creating an object.
* Create an object by calling a factory method.

### prototype

![=100%*](https://upload.wikimedia.org/wikipedia/commons/c/c4/W3sDesign_Prototype_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Prototype_pattern

The Prototype design pattern solves problems like:

* How can objects be created so that which objects to create can be specified at run-time?
* How can dynamically loaded classes be instantiated?

The Prototype design pattern describes how to solve such problems:

* Define a Prototype object that returns a copy of itself.
* Create new objects by copying a Prototype object.


### singleton
The singleton design pattern solves problems like:

* How can it be ensured that a class has only one instance?
* How can the sole instance of a class be accessed easily?
* How can a class control its instantiation?
* How can the number of instances of a class be restricted?

The singleton design pattern describes how to solve such problems:

* Hide the constructor of the class.
* Define a public static operation (getInstance()) that returns the sole instance of the class.
```csharp
public class Singleton
{
   private static Singleton instance = new Singleton();

   private Singleton() {}

   public static Singleton Instance
   {
      get 
      {
         return instance;
      }
   }
}
```

https://en.wikipedia.org/wiki/Singleton_pattern
## Structural

### adapter

![=100%*](https://upload.wikimedia.org/wikipedia/commons/e/e5/W3sDesign_Adapter_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Adapter_pattern

The Adapter design pattern solves problems like: 

* How can a class be reused that does not have an interface that a client requires?
* How can classes that have incompatible interfaces work together?
* How can an alternative interface be provided for a class?

The Adapter design pattern describes how to solve such problems:

* Define a separate Adapter class that converts the (incompatible) interface of a class (Adaptee) into another interface (Target) clients require.
* Work through an Adapter to work with (reuse) classes that do not have the required interface.

### bridge 
![=100%*](https://upload.wikimedia.org/wikipedia/commons/f/fd/W3sDesign_Bridge_Design_Pattern_UML.jpg)

The Bridge pattern is an application of the old advice, "prefer composition over inheritance".  
decouple an abstraction from its implementation so that the two can vary independently. The bridge pattern can also be thought of as two layers of abstraction.

https://en.wikipedia.org/wiki/Bridge_pattern

Before:

                         ----Shape---
                        /            \
               Rectangle              Circle
              /         \            /      \
      BlueRectangle  RedRectangle BlueCircle RedCircle

After:

                ----Shape---                        Color
               /            \                       /   \
      Rectangle(Color)   Circle(Color)           Blue   Red

> Before is `4`, after is `2 * 2`

What problems can the Bridge design pattern solve? 

* An abstraction and its implementation should be defined and extended independently from each other.
* A compile-time binding between an abstraction and its implementation should be avoided so that an implementation can be selected at run-time. When using subclassing, different subclasses implement an abstract class in different ways. But an implementation is bound to the abstraction at compile-time and can't be changed at run-time.

What solution does the Bridge design pattern describe?

* Separate an abstraction (Abstraction) from its implementation (Implementor) by putting them in separate class hierarchies.
* Implement the Abstraction in terms of (by delegating to) an Implementor object.

### composite

![=100%*](https://upload.wikimedia.org/wikipedia/commons/6/65/W3sDesign_Composite_Design_Pattern_UML.jpg)
![=100%*](https://upload.wikimedia.org/wikipedia/commons/3/39/W3sDesign_Composite_Design_Pattern_Type_Safety_UML.jpg)
What problems can the Composite design pattern solve?

* A part-whole hierarchy should be represented so that clients can treat part and whole objects uniformly.
* A part-whole hierarchy should be represented as tree structure.
When defining Part objects and Whole objects that act as containers for Part objects, clients must treat them separately, which complicates client code.

What solution does the Composite design pattern describe?

* Define a unified Component interface for both part (Leaf) objects and whole (Composite) objects.
* Individual Leaf objects implement the Component interface directly, and Composite objects forward requests to their child components.

### decorator
![=100%*](https://upload.wikimedia.org/wikipedia/commons/8/83/W3sDesign_Decorator_Design_Pattern_UML.jpg)

What problems can the Decorator design pattern solve? 

* Responsibilities should be added to (and removed from) an object dynamically at run-time.
* A flexible alternative to subclassing for extending functionality should be provided.
When using subclassing, different subclasses extend a class in different ways. But an extension is bound to the class at compile-time and can't be changed at run-time.

What solution does the Decorator design pattern describe?

* Define Decorator objects that implement the interface of the extended (decorated) object (Component) transparently by forwarding all requests to it and perform additional functionality before/after forwarding a request.


### proxy

![=100%*](https://upload.wikimedia.org/wikipedia/commons/6/6e/W3sDesign_Proxy_Design_Pattern_UML.jpg)
What problems can the Proxy design pattern solve? 

* The access to an object should be controlled.
* Additional functionality should be provided when accessing an object.

When accessing sensitive objects, for example, it should be possible to check that clients have the needed access rights.

What solution does the Proxy design pattern describe?

* Define a separate Proxy object that can be used as substitute for another object (Subject) and kkimplements additional functionality to control the access to this subject.
### facade
![=100%*](https://upload.wikimedia.org/wikipedia/commons/9/96/W3sDesign_Facade_Design_Pattern_UML.jpg)

What problems can the Facade design pattern solve?

* To make a complex subsystem easier to use, a simple interface should be provided for a set of interfaces in the subsystem.
* The dependencies on a subsystem should be minimized.

Clients that access a complex subsystem directly refer to (depend on) many different objects having different interfaces (tight coupling), which makes the clients hard to implement, change, test, and reuse.

What solution does the Facade design pattern describe?

* Define a Facade object that implements a simple interface in terms of (by delegating to) the interfaces in the subsystem and
* may perform additional functionality before/after forwarding a request.

### flyweight
![=100%*](https://upload.wikimedia.org/wikipedia/commons/4/4e/W3sDesign_Flyweight_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Flyweight_pattern

What problems can the Flyweight design pattern solve?

* Large numbers of objects should be supported efficiently.
* Creating large numbers of objects should be avoided.

What solution does the Flyweight design pattern describe?

* Define Flyweight objects that store intrinsic (invariant) state that can be shared and provide an interface through which extrinsic (variant) state can be passed in.
```csharp
using System.Collections.Concurrent;
using System.Collections.Generic;
using System.Threading;

public interface ICoffeeFlavourFactory {
    CoffeeFlavour GetFlavour(string flavour);
}

public class ReducedMemoryFootprint : ICoffeeFlavourFactory {
    private readonly object _cacheLock = new object();
    private readonly IDictionary<string, CoffeeFlavour> _cache = new Dictionary<string, CoffeeFlavour>();

    public CoffeeFlavour GetFlavour(string flavour) {
        if (_cache.ContainsKey(flavour)) return _cache[flavour];
        var coffeeFlavour = new CoffeeFlavour(flavour);
        ThreadPool.QueueUserWorkItem(AddFlavourToCache, coffeeFlavour);
        return coffeeFlavour;
    }

    private void AddFlavourToCache(object state) {
        var coffeeFlavour = (CoffeeFlavour)state;
        if (!_cache.ContainsKey(coffeeFlavour.Flavour)) {
            lock (_cacheLock) {
                if (!_cache.ContainsKey(coffeeFlavour.Flavour)) _cache.Add(coffeeFlavour.Flavour, coffeeFlavour);
            }
        }
    }
}

public class MinimumMemoryFootprint : ICoffeeFlavourFactory {
    private readonly ConcurrentDictionary<string, CoffeeFlavour> _cache = new ConcurrentDictionary<string, CoffeeFlavour>();

    public CoffeeFlavour GetFlavour(string flavour) {
        return _cache.GetOrAdd(flavour, flv => new CoffeeFlavour(flv));
    }
}
```


examples: Windows WPF user event handle, bubble up the tree.
## Behavioral
### chain of responsibility
![=100%*](https://upload.wikimedia.org/wikipedia/commons/6/6a/W3sDesign_Chain_of_Responsibility_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern

What problems can the Chain of Responsibility design pattern solve? 

* Coupling the sender of a request to its receiver should be avoided.
* It should be possible that more than one receiver can handle a request.

What solution does the Chain of Responsibility design pattern describe?

* Define a chain of receiver objects having the responsibility, depending on run-time conditions, to either handle a request or forward it to the next receiver on the chain (if any).

### command
![=100%*](https://upload.wikimedia.org/wikipedia/commons/c/c8/W3sDesign_Command_Design_Pattern_UML.jpg)

What problems can the Command design pattern solve?

* Coupling the invoker of a request to a particular request should be avoided. That is, hard-wired requests should be avoided.
* It should be possible to configure an object (that invokes a request) with a request.

Implementing (hard-wiring) a request directly into a class is inflexible because it couples the class to a particular request at compile-time, which makes it impossible to specify a request at run-time.

What solution does the Command design pattern describe?

* Define separate (command) objects that encapsulate a request.
* A class delegates a request to a command object instead of implementing a particular request directly.

### interpreter
![=100%*](https://upload.wikimedia.org/wikipedia/commons/3/33/W3sDesign_Interpreter_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Interpreter_pattern

What problems can the Interpreter design pattern solve? 

* A grammar for a simple language should be defined
* so that sentences in the language can be interpreted.

What solution does the Interpreter design pattern describe?

* Define a grammar for a simple language by defining an Expression class hierarchy and implementing an interpret() operation.
* Represent a sentence in the language by an abstract syntax tree (AST) made up of Expression instances. Interpret a sentence by calling interpret() on the AST.

### Iterator pattern

![=100%*](https://upload.wikimedia.org/wikipedia/commons/c/c5/W3sDesign_Iterator_Design_Pattern_UML.jpg)

What problems can the Iterator design pattern solve?

* The elements of an aggregate object should be accessed and traversed without exposing its representation (data structures).
* New traversal operations should be defined for an aggregate object without changing its interface.

Defining access and traversal operations in the aggregate interface is *inflexible* because it commits the aggregate to particular access and traversal operations and makes it impossible to add new operations later without having to change the aggregate interface.

What solution does the Iterator design pattern describe?

* Define a separate (iterator) object that encapsulates accessing and traversing an aggregate object.
* Clients use an iterator to access and traverse an aggregate without knowing its representation (data structures).

Different iterators can be used to access and traverse an aggregate in different ways.
New access and traversal operations can be defined independently by defining new iterators.

### mediator
![=100%*](https://upload.wikimedia.org/wikipedia/commons/9/92/W3sDesign_Mediator_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Mediator_pattern

What solution does the Mediator design pattern describe?
* Define a separate (mediator) object that encapsulates the interaction between a set of objects.
* Objects delegate their interaction to a mediator object instead of interacting with each other directly.

What problems can the Mediator design pattern solve? 
* Tight coupling between a set of interacting objects should be avoided.
* It should be possible to change the interaction between a set of objects independently.

### memento

![=100%*](https://upload.wikimedia.org/wikipedia/commons/3/38/W3sDesign_Memento_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Memento_pattern

What problems can the Memento design pattern solve?

* The internal state of an object should be saved externally so that the object can be restored to this state later.
* The object's encapsulation must not be violated.
The problem is that a well designed object is encapsulated so that its representation (data structure) is hidden inside the object and can't be accessed from outside the object.

What solution does the Memento design pattern describe?

Make an object (originator) itself responsible for

saving its internal state to a (memento) object and
restoring to a previous state from a (memento) object.
Only the originator that created a memento is allowed to access it.

A client (caretaker) can request a memento from the originator (to save the internal state of the originator) and pass a memento back to the originator (to restore to a previous state).
This enables to save and restore the internal state of an originator without violating its encapsulation.

### observer

![=100%*](https://upload.wikimedia.org/wikipedia/commons/0/01/W3sDesign_Observer_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Observer_pattern

What problems can the Observer design pattern solve?

* A one-to-many dependency between objects should be defined without making the objects tightly coupled.
* It should be ensured that when one object changes state an open-ended number of dependent objects are updated automatically.
* It should be possible that one object can notify an open-ended number of other objects.

Defining a one-to-many dependency between objects by defining one object (subject) that updates the state of dependent objects directly is inflexible because it commits (tightly couples) the subject to particular dependent objects. Tightly coupled objects are hard to implement, change, test, and reuse because they refer to and know about (how to update) many different objects with different interfaces.

What solution does the Observer design pattern describe?
* Define Subject and Observer objects
* so that when a subject changes state, all registered observers are notified and updated automatically.

The sole responsibility of a subject is to maintain a list of observers and to notify them of state changes by calling their update() operation.
The responsibility of observers is to register (and unregister) themselves on a subject (to get notified of state changes) and to update their state (synchronize their state with subject's state) when they are notified.
This makes subject and observers loosely coupled. Subject and observers have no explicit knowledge of each other. Observers can be added and removed independently at run-time.
This notification-registration interaction is also known as publish-subscribe.

### state

![=100%*](https://upload.wikimedia.org/wikipedia/commons/e/ec/W3sDesign_State_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/State_pattern

What problems can the State design pattern solve? 

* An object should change its behavior when its internal state changes.
* State-specific behavior should be defined independently. That is, new states should be added and the behavior of existing states should be changed independently.

Implementing state-specific behavior directly within a class is inflexible because it commits the class to a particular behavior and makes it impossible to add a new state or change the behavior of an existing state later independently from (without changing) the class.

What solution does the State design pattern describe?

* Define separate (state) objects that encapsulate state-specific behavior for each state. That is, define an interface (State) for performing state-specific behavior, and define classes that implement the interface for each state.
* A class delegates state-specific behavior to its current state object instead of implementing state-specific behavior directly.

This makes a class independent of how state-specific behavior is implemented. New states can be added by defining new state classes.
A class can change its behavior at run-time by changing its current state object.


### visitor
![=100%*](https://upload.wikimedia.org/wikipedia/commons/0/00/W3sDesign_Visitor_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Visitor_pattern

What problems can the Visitor design pattern solve? 
* It should be possible to define a new operation for (some) classes of an object structure without changing the classes.

What solution does the Visitor design pattern describe?

* Define a separate (visitor) object that implements an operation to be performed on elements of an object structure.
* Clients traverse the object structure and call a dispatching operation accept(visitor) on an element — that "dispatches" (delegates) the request to the "accepted visitor object". The visitor object then performs the operation on the element ("visits the element").