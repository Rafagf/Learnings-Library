# Clean Code Uncle Bob - Overview

**Table of Contents**
- [Chapter 2. Naming](#naming)
- [Chapter 3. Functions](#functions)
- [Chapter 4. Comments](#comments)
- [Chapter 5. Formatting](#formatting)
- [Chapter 6. Objects and Data Structures](#objects-and-data-structures)
- [Chapter 7. Error handling](#error-handling)
- [Chapter 8. Boundaries](#boundaries)
- [Chapter 9. Unit tests](#unit-tests)
- [Chapter 10. Classes](#classes)
- [Chapter 11. Systems](#systems)
- [Chapter 12. Emergence](#emergence)
- [Chapter 13. Concurrency](#concurrency)
- [Chapter 17. Smells and Heuristics](#smells)


<a name="naming"></a>
## Chapter 2. Naming
* Use intention-revealing names
* Avoid disinformation (i.e calling something accountList and then that being a map
* Use pronounceable and searchable names with the IDE
* Avoid prefixes and hungarian notation

<a name="functions"></a>
## Chapter 3: Functions
* Small and do one thing! (and do it well). They should also have one level of abstraction per function
* Switch statements are generally bad.
  * Are large and it easily grows.
  * They do more than one thing, so it breaks our first rule.
  * Violates the Single Responsibility Principle (SRP) because there is more than one reason to change the function.
  * Violates the Open Closed Principle (OCP) because it must change whenever new types are added. 
  * In general, we should bury switch statement inside a Factory class where no-one needs to know about it.
* Use descriptive names
* Should have ideally 0 parameters. 1 or 2 is fine, >=3 should be avoided where possible.
* Functions should never have side effects. We need to trust our functions!

<a name="comments"></a>
## Chapter 4: Comments
* In general, avoid comments. Code should explain itself without the need of comments.
* Comments don’t progress as code does, meaning they will be missleading at some point.
* If you leave a comment, make sure is crystal clear (no mumbling or moaning about something).
* Commented out code is noise. Delete it, it’s in your source code control.

<a name="formatting"></a>
## Chapter 5: Formatting
Do whatever you want but be consistent AND makes sure all your team uses the same formatting. That’s it.

<a name="objects-and-data-structures"></a>
## Chapter 6: Objects and Data Structures
* We don’t want to expose the details of our data. We want to express our data in abstract terms, hiding the implementation (we may change it later). 
* In general, setters should be avoided and properties should be created in an atomic way, all at the same time (i.e constructor) instead of separate setters
* **Objects hide their data behind abstractions and expose functions that operate on the data...**
* **And data structures expose their data and have no meaningful functions. They are opposite.**
* The classic example of procedural code (using data structures) vs object oriented.

```java 
public class Square {
  public Point topLeft;
  public double side;
}

public class Rectangle {
  public Point topLeft;
  public double height;
  public double width;
}

public class Circle {
  public Point center;
  public double radius;
}

public class Geometry {
  public double area (Object shape) {
    if (shape instanceOf square) {
      Square s = (Square) shape;
	    Return s.side * s.side;
    } else if (shape instanceOf Rectangle) {
      Rectangle r = (Rectangle) shape;
      Return r.height * r.width;
    }
...
```

If we want to extend this to also be able to calculate perimeter, **the shapes classes wouldn’t change!** On the other hand, if we add a new type, all the functions in **Geometry would need to change.**

Now let’s do a object-oriented solution using polymorphism. 

```java 
public class Square implements Shape {
  private Point topLeft;
  private double side;

  public double area() {
    return side*side;
  } 
}

public class Rectangle implements Shape {
  private Point topLeft;
  private double height; 
  private double width;
  
  public double area() {
    return height * witdth;
  }
}
```

No Geometry class needed as the functions live inside the objects. Now, if **we add a new shape, the existing functions would stay the same!** However, if we add a new function, all the shapes must be changed! 

These approaches are virtually opposites!

Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions. 

The complement is also true:

_Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change._ 


* A module should not know about the innards of the objects it manipulates. In simple words, the lay of Demeter says _talk to friends, not strangers_. A method should call its own functions or functions of its dependencies, but not functions on returned objects of these functions.

<a name="error-handling"></a>
## Chapter 7: Error Handling
* Use exceptions rather than return codes
* **Use unchecked exceptions over checked exceptions**
Checked exceptions are those where the signature of every method would list all the exceptions that it could pass to its caller. This violates the Open/Closed Principle. If you throw an exception and the catch is three levels above, you  must declare that exception in the signature of each method between you and the catch. *That means that a change at a low level will force signature changes on many higher levels.* 
* **In Java exceptions under Error and RuntimeException classes are unchecked exceptions, everything else under throwable is checked.**

<p align="center">
<img width="519" alt="Screenshot 2019-10-19 at 20 10 05" src="https://user-images.githubusercontent.com/6362660/67150228-b8309880-f2ac-11e9-98a8-25f6086fe9a2.png">
</>

* Don't return null when possible. Never pass null around as a parameter. 

<a name="boundaries"></a>
## Chapter 8. Boundaries
* Interfaces should hide implementation details.
* Write tests to make sure the 3rd party library that you’re using does what you expect from them. This will give you better sleep plus the possibility to update the library dependency without breaking functionality.

<a name="unit-tests"></a>
## Chapter 9. Unit tests
* Keep tests clean. They need to be maintainable! 
* One assert per test
* F.I.R.S.T: Fast (run quickly), Independent (should not depend on each other), Repeatable (in any environment), Self-Validating (boolean output, pass of fail), Timely (written in timely fashion).

<a name="classes"></a>
## Chapter 10. Classes
* Encapsulation. Our variables and utility functions need to be private (protected if they need to be accessed from our tests)
* Classes should be small and have ONE responsibility. They should not violate the Single Responsibility Principle and thus, have only one reason to change.
* Isolate from change using interfaces and hiding your implementations behind them.
* **Dependency Inversion Principle (DIP). Says that our classes should depend upon abstractions, not on concrete details**. The given example is pretty good. Instead of depending on a class called _TokyoStockExchange_, our _Portfolio_ class should depend on a _StockExchange_ interface. The _StockExchange_ interface represents the abstract concept of asking for the current price of a symbol. **The abstraction isolates all of the specific details of obtaining such a price, including from where the price is obtained.**


<a name="systems"></a>
## Chapter 11. Systems
* Separate constructing a system from using it.
* Software systems should separate the startup process, when the application objects are constructed and the dependencies are wired together, from the runtime logic that takes over after startup.
* Dependency injection help us to do so. **The main module or container will create all the needed objects by figuring out the dependencies**. By doing this, we release the objects to know how to instantiate themselves. 
* Systems need domain-specific languages. A good DSL minimizes the communication gap between a domain concept and the code that implements it.  

<a name="emergence"></a>
## Chapter 12. Emergence

4 rules to design good systems:
* Run all the tests. It should be easy to verify the behaviour of the system by running its tests. Also, the more tests we write the more we use principles like DIP, interfaces and abstraction to minimize coupling. Our code simply improves.
* No duplication.
* Code expressiveness.
* Minimal classes and methods

<a name="concurrency"></a>
## Chapter 13. Concurrency
* Concurrency is a decoupling strategy. It decouples what gets done from when it gets done.
* **Concurrency is hard** and it is very easy to write misleading code. Also, it does not always improve performance. It mainly works when there is wait time that can be shared between multiple threads or multiple processors.  
* Example of how concurrency can be tricky.
```java
public class X {
	private int lastIdUsed;

	public int getNextId() {
		return ++lastIdUsed;
	}
}
```

Let’s now create an X instance, set the lastIdUsed to 42 and then shared the instance between two threads. Let’s all the _getNextId()_ method from both threads. There are 3 possible outcomes:
1. Thread one gets value 43, thread two gets value 44, lastIdUsed is 44.
2. Thread one gets value 44, thread two gets value 43, lastIdUsed is 44.
3. Thread one gets value 43, thread two gets value 43, lastIdUsed is 43.

The surprising last result happens because there are many possible paths that the two threads can take through the one line of Java code (when this is translated into byte-code), some of these paths generating incorrect results. 
How many paths are there? To really answer this question, we need to understand what the Just-In-Time compiler does with the generated byte-code, and understand what the Java memory models considers to be atomic.
A quick answer, working just with the generated byte-code, is that there are 12.870 different possible execution paths for those 2 threads executing within the _getNextId()_ method. If _getNextId()_ is changed from int to long, then the number increases to 2704156 paths. 

_Quirky explanation_
```java
int foo;
foo = 0 -> atomic operation (i.e can’t be interrupted)
```

```java
long foo
foo = 0 -> only atomic in some processors. It means 2 32-bits operations are needed and they could be interleaved by other threads. 
```

Regardless of our _lastIdUsed_ being int or long in our example, incrementing is not an atomic operation. That means threads can step into each other in the process of incrementing the number, creating incorrect results.

* Some concurrency defense principles/recommendations:
1. SRP. Keep your concurrency-related code separate from other code.
2. Limit the scope of data. If possible, don’t share between threads. _Take data encapsulation to heart, severely limit the access of any data that may be shared._
3. Use copies of data whenever possible.
4. Threads should be as independent as possible. They should be able to run without knowing there are other threads running, and they shouldn’t share any data.

* Concurrent basic definitions
	* Bound resources -> Resources of a fixed size or number used in concurrent environments (i.e database connections).
	* Mutual exclusions -> Only one thread can access shared data or a shared resource at a time.
	* Starvation -> One thread or a group of threads is prohibited from proceeding for an excessively long time or forever.
	* Deadlock -> Two or more threads waiting for each other to finish.
	* Livelock -> Threads in a lockstep, each trying to do work but finding another in the way. 
	
* Executions models
	* Producer - Consumer
	One or more producer threads create some work and place it in a queue. One or more consumer threads acquire that work from the queue and complete it. The queue between the producers and consumers is a bound resource. This means producers must wait for free space in the queue before writing and consumers must wait until there is something in the queue to consume.
	* Readers - Writers
	When you have a shared resource that primarily serves as a source of information for readers, but which is ocassionally updated by witers, throughput is an issue. The challenge is to balance the needs of both readers and writers to satisfy correct operation, provide reasonable throughput and avoiding starvation. 
	* Dining philosophers
	Philisophers sitting around a circular table. A fork is placed to the left of each philosopher. There is food in the center of the table. The philosophers cannot eat unless they hold 2 forks, meaning they need to wait for the philosophers sitting next to them to be done. Replace philosophers with threads and forks with resources and this problem is similar to many enterprise applications in which processes compete for resources. 
	
* Random recommendations
	* Avoid using more than one synchronised method on a shared object to avoid bugs.
	* Keep your synchronized sections as small as possible. These sections create delays and overhead.
	* Writing shut down code is hard (resources depending on each other). Get it working early.
	* Write tests that have the potential to expose problems and then un them frequentely, with different configurations. If a test fail, don't ignore it just because the test pass on a subsequent run.
	* Do not try to chase down nonthreading bugs and threading bugs at the same time. Make sure your code works outside of threads.
	* Instrument your code concurrent ocde to force failures. You can do this hand-coded by using wait(), sleep(), yield() and priority(), which will affect the order of execution inside the code accessed by multiple threads.


<a name="smells"></a>
## Chapter 17. Smells and Heuristics
Some of them have been already mentioned in previous chapters. These are the others that caught my attention in this chapter:
* Environment
	* Build requires more than one step
	* Tests require more than one step
* Functions
	* Flag arguments arguments (single method functions receiving a boolean)
* General
	* Code at wrong level of abstractions. Example: we've got a base class and derivative classes. We want all the lower level concepts to be in derivatives and all the higher level concepts to be in the base class.
	* Base classes depending on their derivatives
	* Feature envy. The methods of a class should be interested in the variables and functions of the class they belong to, and not the variables and functions of other classes.
	* Inappropiate static. When in doubt, make the method non-static. If you really want a function to be static, make sure that there is no chance that you'll want it to behave polymorphically.
	* Avoid transitive navigation. The saw called Law of Demeter, or what other programmer call "Writing Shy Code". You don't want to have things like `a.getB().getC()`. Doing a change between B and C would mean you have to do changes in many places.
	

