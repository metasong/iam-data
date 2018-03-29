# design patterns

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

## bridge 
![=100%*](https://upload.wikimedia.org/wikipedia/commons/f/fd/W3sDesign_Bridge_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Bridge_pattern
What problems can the Bridge design pattern solve? 

* An abstraction and its implementation should be defined and extended independently from each other.
* A compile-time binding between an abstraction and its implementation should be avoided so that an implementation can be selected at run-time.

What solution does the Bridge design pattern describe?

* Separate an abstraction (Abstraction) from its implementation (Implementor) by putting them in separate class hierarchies.
* Implement the Abstraction in terms of (by delegating to) an Implementor object.
## visitor
![=100%*](https://upload.wikimedia.org/wikipedia/commons/0/00/W3sDesign_Visitor_Design_Pattern_UML.jpg)

https://en.wikipedia.org/wiki/Visitor_pattern

What problems can the Visitor design pattern solve? 
* It should be possible to define a new operation for (some) classes of an object structure without changing the classes.

What solution does the Visitor design pattern describe?

* Define a separate (visitor) object that implements an operation to be performed on elements of an object structure.
* Clients traverse the object structure and call a dispatching operation accept(visitor) on an element — that "dispatches" (delegates) the request to the "accepted visitor object". The visitor object then performs the operation on the element ("visits the element").
## interpreter
![=100%*](https://upload.wikimedia.org/wikipedia/commons/3/33/W3sDesign_Interpreter_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Interpreter_pattern

What problems can the Interpreter design pattern solve? 

* A grammar for a simple language should be defined
* so that sentences in the language can be interpreted.

What solution does the Interpreter design pattern describe?

* Define a grammar for a simple language by defining an Expression class hierarchy and implementing an interpret() operation.
* Represent a sentence in the language by an abstract syntax tree (AST) made up of Expression instances. Interpret a sentence by calling interpret() on the AST.
## flyweight
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
## chain of responsibility
![=100%*](https://upload.wikimedia.org/wikipedia/commons/6/6a/W3sDesign_Chain_of_Responsibility_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern

What problems can the Chain of Responsibility design pattern solve? 

* Coupling the sender of a request to its receiver should be avoided.
* It should be possible that more than one receiver can handle a request.

What solution does the Chain of Responsibility design pattern describe?

* Define a chain of receiver objects having the responsibility, depending on run-time conditions, to either handle a request or forward it to the next receiver on the chain (if any).

examples: Windows WPF user event handle, bubble up the tree.
## mediator
![=100%*](https://upload.wikimedia.org/wikipedia/commons/9/92/W3sDesign_Mediator_Design_Pattern_UML.jpg)

What solution does the Mediator design pattern describe?
* Define a separate (mediator) object that encapsulates the interaction between a set of objects.
* Objects delegate their interaction to a mediator object instead of interacting with each other directly.

What problems can the Mediator design pattern solve? 
* Tight coupling between a set of interacting objects should be avoided.
* It should be possible to change the interaction between a set of objects independently.