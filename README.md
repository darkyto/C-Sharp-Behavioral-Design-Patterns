**Behavioral Design Patterns**
==============================

In software engineering, behavioral design patterns are design patterns that identify common communication patterns between objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.

 - Chain of responsibility
A way of passing a request between a chain of objects
 - Command
Encapsulate a command request as an object
 - Interpreter
A way to include language elements in a program
 - Iterator
Sequentially access the elements of a collection
 - Mediator
Defines simplified communication between classes
 - Memento
Capture and restore an object's internal state
 - Null Object
Designed to act as a default value of an object
 - Observer
A way of notifying change to a number of classes
 - State
Alter an object's behavior when its state changes
 - Strategy
Encapsulates an algorithm inside a class
 - Template method
Defer the exact steps of an algorithm to a subclass
 - Visitor
Defines a new operation to a class without change

**Chain of responsibility**
===============

 

 1. **Intends**
===========

 2. Avoid coupling the sender of a request to its receiver by giving more
   than one object a chance to handle the request. Chain the receiving
   objects and pass the request along the chain until an object handles
   it.

 3. Launch-and-leave requests with a single processing pipeline that
   contains many possible handlers.

 4. An object-oriented linked list with recursive traversal.

    
2. **Problem**
------------------

There is a potentially variable number of "handler" or "processing element" or "node" objects, and a stream of requests that must be handled. Need to efficiently process the requests without hard-wiring handler relationships and precedence, or request-to-handler mappings.

![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Chain_of_responsibility1-2x.png)

 

3. **Description**
================

Encapsulate the processing elements inside a "pipeline" abstraction; and have clients "launch and leave" their requests at the entrance to the pipeline.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Chain_of_responsibility_1-2x.png)
The pattern chains the receiving objects together, and then passes any request messages from object to object until it reaches an object capable of handling the message. The number and type of handler objects isn't known a priori, they can be configured dynamically. The chaining mechanism uses recursive composition to allow an unlimited number of handlers to be linked.

Chain of Responsibility simplifies object interconnections. Instead of senders and receivers maintaining references to all candidate receivers, each sender keeps a single reference to the head of the chain, and each receiver keeps a single reference to its immediate successor in the chain.

Make sure there exists a "safety net" to "catch" any requests which go unhandled.

Do not use Chain of Responsibility when each request is only handled by one handler, or, when the client object knows which service object should handle the request.

 4. **Structure**
---------------

The derived classes know how to satisfy Client requests. If the "current" object is not available or sufficient, then it delegates to the base class, which delegates to the "next" object, and the circle of life continues.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Chain_of_responsibility__-2x.png)
Multiple handlers could contribute to the handling of each request. The request can be passed down the entire length of the chain, with the last link being careful not to delegate to a "null next".

 5. **Example**
---------------

The Chain of Responsibility pattern avoids coupling the sender of a request to the receiver by giving more than one object a chance to handle the request. ATM use the Chain of Responsibility in money giving mechanism.
![enter image description here](https://sourcemaking.com/files/sm/images/patterns/Chain_of_responsibility_example.png)


**Check list**

 - The base class maintains a "next" pointer.
 - Each derived class implements its contribution for handling the
   request. If the request needs to be "passed on", then the derived
   class "calls back" to the base class, which delegates to the "next"
   pointer.
 - The client (or some third party) creates and links the chain (which
   may include a link from the last node to the root node).
 - The client "launches and leaves" each request with the root of the
   chain. Recursive delegation produces the illusion of magic.

**Rules of thumb**

 - Chain of Responsibility, Command, Mediator, and Observer, address how
   you can decouple senders and receivers, but with different
   trade-offs.

 - Chain of Responsibility passes a sender request along a chain of
   potential receivers.

 - Chain of Responsibility can use Command to represent requests as
   objects.

 - Chain of Responsibility is often applied in conjunction with
   Composite.

 - There, a component's parent can act as its successor.

    using System;

		  class MainApp
		  {
		    static void Main()
		    {
		      // Setup Chain of Responsibility 
		      Handler h1 = new ConcreteHandler1();
		      Handler h2 = new ConcreteHandler2();
		      Handler h3 = new ConcreteHandler3();
		      h1.SetSuccessor(h2);
		      h2.SetSuccessor(h3);
		
		      // Generate and process request 
		      int[] requests = {2, 5, 14, 22, 18, 3, 27, 20};
		
		      foreach (int request in requests)
		      {
		        h1.HandleRequest(request);
		      }
		
		      // Wait for user 
		      Console.Read();
		    }
		  }
		
		  // "Handler" 
		  abstract class Handler 
		  {
		    protected Handler successor;
		
		    public void SetSuccessor(Handler successor)
		    {
		      this.successor = successor;
		    }
		
		    public abstract void HandleRequest(int request);
		  }
		
		  // "ConcreteHandler1" 
		  class ConcreteHandler1 : Handler
		  {
		    public override void HandleRequest(int request)
		    {
		      if (request >= 0 && request < 10)
		      {
		        Console.WriteLine("{0} handled request {1}", 
		          this.GetType().Name, request);
		      }
		      else if (successor != null)
		      {
		        successor.HandleRequest(request);
		      }
		    }
		  }
		
		  // "ConcreteHandler2" 
		  class ConcreteHandler2 : Handler
		  {
		    public override void HandleRequest(int request)
		    {
		      if (request >= 10 && request < 20)
		      {
		        Console.WriteLine("{0} handled request {1}", 
		          this.GetType().Name, request);
		      }
		      else if (successor != null)
		      {
		        successor.HandleRequest(request);
		      }
		    }
		  }
		
		  // "ConcreteHandler3" 
		  class ConcreteHandler3 : Handler
		  {
		    public override void HandleRequest(int request)
		    {
		      if (request >= 20 && request < 30)
		      {
		        Console.WriteLine("{0} handled request {1}", 
		          this.GetType().Name, request);
		      }
		      else if (successor != null)
		      {
		        successor.HandleRequest(request);
		      }
		    }
		  }

Output
------
    ConcreteHandler1 handled request 2
	ConcreteHandler1 handled request 5
	ConcreteHandler2 handled request 14
	ConcreteHandler3 handled request 22
	ConcreteHandler2 handled request 18
	ConcreteHandler1 handled request 3
	ConcreteHandler3 handled request 27
	ConcreteHandler3 handled request 20


**Memento**
===============


 - **Intends**
===========
 - Without violating encapsulation, capture and externalize an object's
   internal state so that the object can be returned to this state
   later.
 - A magic cookie that encapsulates a "check point" capability.
 - Promote undo or rollback to full object status.

    
2. **Problem**
------------------

Need to restore an object back to its previous state (e.g. "undo" or "rollback" operations).
 

3. **Description**
------------------

The client requests a Memento from the source object when it needs to checkpoint the source object's state. The source object initializes the Memento with a characterization of its state. The client is the "care-taker" of the Memento, but only the source object can store and retrieve information from the Memento (the Memento is "opaque" to the client and all other objects). If the client subsequently needs to "rollback" the source object's state, it hands the Memento back to the source object for reinstatement.

An unlimited "undo" and "redo" capability can be readily implemented with a stack of Command objects and a stack of Memento objects.

The Memento design pattern defines three distinct roles:

 - Originator - the object that knows how to save itself.
 - Caretaker - the object that knows why and when the Originator needs
   to save and restore itself.
 - Memento - the lock box that is written and read by the Originator,
   and shepherded by the Caretaker.
   
4. **Structure**
---------------
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Memento-2x.png)
 5. **Example**
---------------

The Memento captures and externalizes an object's internal state so that the object can later be restored to that state. This pattern is common among do-it-yourself mechanics repairing drum brakes on their cars. The drums are removed from both sides, exposing both the right and left brakes. Only one side is disassembled and the other serves as a Memento of how the brake parts fit together. Only after the job has been completed on one side is the other side disassembled. When the second side is disassembled, the first side acts as the Memento.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Memento_example-2x.png)


**Check list**

 - Identify the roles of “caretaker” and “originator”.
 - Create a Memento class and declare the originator a friend.
 - Caretaker knows when to "check point" the originator.
 - Originator creates a Memento and copies its state to that Memento.
 - Caretaker holds on to (but cannot peek into) the Memento.
 - Caretaker knows when to "roll back" the originator.
 - Originator reinstates itself using the saved state in the Memento.

**Rules of thumb**

 - -Command and Memento act as magic tokens to be passed around and invoked at a later time. In Command, the token represents a request;
   in Memento, it represents the internal state of an object at a
   particular time. Polymorphism is important to Command, but not to
   Memento because its interface is so narrow that a memento can only be
   passed as a value.

 - Command can use Memento to maintain the state required for an undo
   operation.

 - Memento is often used in conjunction with Iterator. An Iterator can
   use a Memento to capture the state of an iteration. The Iterator
   stores the Memento internally.

	    using System;

		namespace DoFactory.GangOfFour.Memento.Structural
		{
		  // MainApp startup class for Structural 
		  // Memento Design Pattern.
		  class MainApp
		  {
		     // Entry point into console application.
		    static void Main()
		    {
		      Originator o = new Originator();
		      o.State = "On";
		
		      // Store internal state
		      Caretaker c = new Caretaker();
		      c.Memento = o.CreateMemento();
		
		      // Continue changing originator
		      o.State = "Off";
		
		      // Restore saved state
		      o.SetMemento(c.Memento);
		
		      // Wait for user
		      Console.ReadKey();
		    }
		  }
		
		  // The 'Originator' class
		  class Originator
		  {
		    private string _state;
		
		    // Property
		    public string State
		    {
		      get { return _state; }
		      set
		      {
		        _state = value;
		        Console.WriteLine("State = " + _state);
		      }
		    }
		
		    // Creates memento 
		    public Memento CreateMemento()
		    {
		      return (new Memento(_state));
		    }
		
		    // Restores original state
		    public void SetMemento(Memento memento)
		    {
		      Console.WriteLine("Restoring state...");
		      State = memento.State;
		    }
		  }
		
		  // The 'Memento' class
		  class Memento
		  {
		    private string _state;
		
		    // Constructor
		    public Memento(string state) {
		      this._state = state;
		    }
		
		    // Gets or sets state
		    public string State
		    {
		      get { return _state; }
		    }
		  }
		
		  // The 'Caretaker' class
		  class Caretaker
		  {
		    private Memento _memento;
		
		    // Gets or sets memento
		    public Memento Memento
		    {
		      set { _memento = value; }
		      get { return _memento; }
		    }
		  }
		}

Output
------
    State = On
	State = Off
	Restoring state:
	State = On
	

**Strategy**
===============


 - **Intends**
===========
 - Define a family of algorithms, encapsulate each one, and make them
   interchangeable. Strategy lets the algorithm vary independently from
   the clients that use it.
 - Capture the abstraction in an interface, bury implementation details
   in derived classes.

    
2. **Problem**
------------------

One of the dominant strategies of object-oriented design is the "open-closed principle".

Figure demonstrates how this is routinely achieved - encapsulate interface details in a base class, and bury implementation details in derived classes. Clients can then couple themselves to an interface, and not have to experience the upheaval associated with change: no impact when the number of derived classes changes, and no impact when the implementation of a derived class changes.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Strategy1-2x.png)
 A generic value of the software community for years has been, "maximize cohesion and minimize coupling". The object-oriented design approach shown in figure is all about minimizing coupling. Since the client is coupled only to an abstraction (i.e. a useful fiction), and not a particular realization of that abstraction, the client could be said to be practicing "abstract coupling" . an object-oriented variant of the more generic exhortation "minimize coupling".

A more popular characterization of this "abstract coupling" principle is "Program to an interface, not an implementation".

Clients should prefer the "additional level of indirection" that an interface (or an abstract base class) affords. The interface captures the abstraction (i.e. the "useful fiction") the client wants to exercise, and the implementations of that interface are effectively hidden.


   
4. **Structure**
---------------
The Interface entity could represent either an abstract base class, or the method signature expectations by the client. In the former case, the inheritance hierarchy represents dynamic polymorphism. In the latter case, the Interface entity represents template code in the client and the inheritance hierarchy represents static polymorphism.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Strategy_-2x.png)
 5. **Example**
---------------

A Strategy defines a set of algorithms that can be used interchangeably. Modes of transportation to an airport is an example of a Strategy. Several options exist such as driving one's own car, taking a taxi, an airport shuttle, a city bus, or a limousine service. For some airports, subways and helicopters are also available as a mode of transportation to the airport. Any of these modes of transportation will get a traveler to the airport, and they can be used interchangeably. The traveler must chose the Strategy based on tradeoffs between cost, convenience, and time.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Strategy_example1-2x.png)


**Check list**

 - Identify an algorithm (i.e. a behavior) that the client would prefer
   to access through a "flex point".
 - Specify the signature for that algorithm in an interface.
 - Bury the alternative implementation details in derived classes.
 - Clients of the algorithm couple themselves to the interface.

**Rules of thumb**

 - Strategy is like Template Method except in its granularity. State is
   like Strategy except in its intent.

 - Strategy lets you change the guts of an object. Decorator lets you
   change the skin.

 - State, Strategy, Bridge (and to some degree Adapter) have similar
   solution structures. They all share elements of the 'handle/body'
   idiom. They differ in intent - that is, they solve different
   problems.

 - Strategy has 2 different implementations, the first is similar to
   State. The difference is in binding times (Strategy is a bind-once
   pattern, whereas State is more dynamic).

 - Strategy objects often make good Flyweights.

		  using System;

		  class MainApp
		  {
		    static void Main()
		    {
		      Context context;
		
		      // Three contexts following different strategies 
		      context = new Context(new ConcreteStrategyA());
		      context.ContextInterface();
		
		      context = new Context(new ConcreteStrategyB());
		      context.ContextInterface();
		
		      context = new Context(new ConcreteStrategyC());
		      context.ContextInterface();
		
		      // Wait for user 
		      Console.Read();
		    }
		  }
		
		  // "Strategy" 
		  abstract class Strategy
		  {
		    public abstract void AlgorithmInterface();
		  }
		
		  // "ConcreteStrategyA" 
		  class ConcreteStrategyA : Strategy
		  {
		    public override void AlgorithmInterface()
		    {
		      Console.WriteLine(
		        "Called ConcreteStrategyA.AlgorithmInterface()");
		    }
		  }
		
		  // "ConcreteStrategyB" 
		  class ConcreteStrategyB : Strategy
		  {
		    public override void AlgorithmInterface()
		    {
		      Console.WriteLine(
		        "Called ConcreteStrategyB.AlgorithmInterface()");
		    }
		  }
		
		  // "ConcreteStrategyC" 
		  class ConcreteStrategyC : Strategy
		  {
		    public override void AlgorithmInterface()
		    {
		      Console.WriteLine(
		        "Called ConcreteStrategyC.AlgorithmInterface()");
		    }
		  }
		
		  // "Context" 
		  class Context
		  {
		    Strategy strategy;
		
		    // Constructor 
		    public Context(Strategy strategy)
		    {
		      this.strategy = strategy;
		    }
		
		    public void ContextInterface()
		    {
		      strategy.AlgorithmInterface();
		    }
		  }

Output
------
    Called ConcreteStrategyA.AlgorithmInterface()
	Called ConcreteStrategyB.AlgorithmInterface()
	Called ConcreteStrategyC.AlgorithmInterface()


**Iterator**
===============

 - **Intends**
-----------
 - Provide a way to access the elements of an aggregate object
   sequentially without exposing its underlying representation.
 - The C++ and Java standard library abstraction that makes it possible
   to decouple collection classes and algorithms.
 - Promote to "full object status" the traversal of a collection.
   Polymorphic traversal

    
2. **Problem**
------------------

Need to "abstract" the traversal of wildly different data structures so that algorithms can be defined that are capable of interfacing with each transparently.
 

3. **Description**
------------------

"An aggregate object such as a list should give you a way to access its elements without exposing its internal structure. Moreover, you might want to traverse the list in different ways, depending on what you need to accomplish. But you probably don't want to bloat the List interface with operations for different traversals, even if you could anticipate the ones you'll require. You might also need to have more than one traversal pending on the same list." And, providing a uniform interface for traversing many types of aggregate objects (i.e. polymorphic iteration) might be valuable.

The Iterator pattern lets you do all this. The key idea is to take the responsibility for access and traversal out of the aggregate object and put it into an Iterator object that defines a standard traversal protocol.

The Iterator abstraction is fundamental to an emerging technology called "generic programming". This strategy seeks to explicitly separate the notion of "algorithm" from that of "data structure". The motivation is to: promote component-based development, boost productivity, and reduce configuration management.

As an example, if you wanted to support four data structures (array, binary tree, linked list, and hash table) and three algorithms (sort, find, and merge), a traditional approach would require four times three permutations to develop and maintain. Whereas, a generic programming approach would only require four plus three configuration items.
   
4. **Structure**
---------------
The Client uses the Collection class’ public interface directly. But access to the Collection’s elements is encapsulated behind the additional level of abstraction called Iterator. Each Collection derived class knows which Iterator derived class to create and return. After that, the Client relies on the interface defined in the Iterator base class.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Iterator-2x.png)
 5. **Example**
---------------

The Iterator provides ways to access elements of an aggregate object sequentially without exposing the underlying structure of the object. Files are aggregate objects. In office settings where access to files is made through administrative or secretarial staff, the Iterator pattern is demonstrated with the secretary acting as the Iterator. Several television comedy skits have been developed around the premise of an executive trying to understand the secretary's filing system. To the executive, the filing system is confusing and illogical, but the secretary is able to access files quickly and efficiently.

On early television sets, a dial was used to change channels. When channel surfing, the viewer was required to move the dial through each channel position, regardless of whether or not that channel had reception. On modern television sets, a next and previous button are used. When the viewer selects the "next" button, the next tuned channel will be displayed. Consider watching television in a hotel room in a strange city. When surfing through channels, the channel number is not important, but the programming is. If the programming on one channel is not of interest, the viewer can request the next channel, without knowing its number.
![enter image description here](https://sourcemaking.com/files/v2/content/patterns/Iterator_example1-2x.png)

**Check list**

 - Add a `create_iterator()` method to the "collection" class, and grant
   the "iterator" class privileged access.
 - Design an "iterator" class that can encapsulate traversal of the
   "collection" class.
 - Clients ask the collection object to create an iterator object.
 - Clients use the `first()`, `is_done()`, `next(),` and `current_item()`
   protocol to access the elements of the collection class.

**Rules of thumb**

 - -The abstract syntax tree of Interpreter is a Composite (therefore Iterator and Visitor are also applicable).

 - Iterator can traverse a Composite. Visitor can apply an operation
   over a Composite.

 - Polymorphic Iterators rely on Factory Methods to instantiate the
   appropriate Iterator subclass.

 - Memento is often used in conjunction with Iterator. An Iterator can
   use a Memento to capture the state of an iteration. The Iterator
   stores the Memento internally.
   

		 using System;
		 using System.Collections;
		
		  class MainApp
		  {
		    static void Main()
		    {
		      ConcreteAggregate a = new ConcreteAggregate();
		      a[0] = "Item A";
		      a[1] = "Item B";
		      a[2] = "Item C";
		      a[3] = "Item D";
		
		      // Create Iterator and provide aggregate 
		      ConcreteIterator i = new ConcreteIterator(a);
		
		      Console.WriteLine("Iterating over collection:");
		 
		      object item = i.First();
		      while (item != null)
		      {
		        Console.WriteLine(item);
		        item = i.Next();
		      } 
		
		      // Wait for user 
		      Console.Read();
		    }
		  }
		
		  // "Aggregate" 
		  abstract class Aggregate
		  {
		    public abstract Iterator CreateIterator();
		  }
		
		  // "ConcreteAggregate" 
		  class ConcreteAggregate : Aggregate
		  {
		    private ArrayList items = new ArrayList();
		
		    public override Iterator CreateIterator()
		    {
		      return new ConcreteIterator(this);
		    }
		
		    // Property 
		    public int Count
		    {
		      get{ return items.Count; }
		    }
		
		    // Indexer 
		    public object this[int index]
		    {
		      get{ return items[index]; }
		      set{ items.Insert(index, value); }
		    }
		  }
		
		  // "Iterator" 
		  abstract class Iterator
		  {
		    public abstract object First();
		    public abstract object Next();
		    public abstract bool IsDone();
		    public abstract object CurrentItem();
		  }
		
		  // "ConcreteIterator" 
		  class ConcreteIterator : Iterator
		  {
		    private ConcreteAggregate aggregate;
		    private int current = 0;
		
		    // Constructor 
		    public ConcreteIterator(ConcreteAggregate aggregate)
		    {
		      this.aggregate = aggregate;
		    }
		
		    public override object First()
		    {
		      return aggregate[0];
		    }
		
		    public override object Next()
		    {
		      object ret = null;
		      if (current < aggregate.Count - 1)
		      {
		        ret = aggregate[++current];
		      }
		 
		      return ret;
		    }
		
		    public override object CurrentItem()
		    {
		      return aggregate[current];
		    }
		
		    public override bool IsDone()
		    {
		      return current >= aggregate.Count ? true : false ;
		    }
		  }
  
Output
------
    Iterating over collection:
	Item A
	Item B
	Item C
	Item D
	