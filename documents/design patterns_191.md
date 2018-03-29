# design patterns
## factory methord

![=100%*](https://upload.wikimedia.org/wikipedia/commons/4/43/W3sDesign_Factory_Method_Design_Pattern_UML.jpg)
https://en.m.wikipedia.org/wiki/Factory_method_pattern
## abstract factory

![=100%*](https://upload.wikimedia.org/wikipedia/commons/a/aa/W3sDesign_Abstract_Factory_Design_Pattern_UML.jpg)
https://en.m.wikipedia.org/wiki/Abstract_factory_pattern

## bridge 
![=100%*](https://upload.wikimedia.org/wikipedia/commons/f/fd/W3sDesign_Bridge_Design_Pattern_UML.jpg)

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

## flyweight
![=100%*](https://upload.wikimedia.org/wikipedia/commons/4/4e/W3sDesign_Flyweight_Design_Pattern_UML.jpg)
https://en.wikipedia.org/wiki/Flyweight_pattern

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