# Head First Design Patterns Overview

It's important to point out this won't include any personal opinion, but just a plain overview of what's described in the book. 

**Table of Contents**
- [Chapter 1. Intro to Design Patterns](#intro-to-design-patterns)
- [Chapter 2. The Observer Pattern](#the-observer-pattern)
- [Chapter 3. The Decorator Pattern](#the-decorator-pattern)
- [Chapter 4. The Factory Pattern](#the-factory-pattern)
- [Chapter 5. The Singleton Pattern](#the-singleton-pattern)
- [Chapter 6. The Command Pattern](#the-command-pattern)
- [Chapter 7. The Adapter and Facade Patterns](#the-adapter-pattern)
- [Chapter 8. The Template Method Pattern](#the-template-method-pattern)
- [More coming soon]()

<a name="intro-to-design-patterns"></a>
## Chapter 1. Intro to Design Patterns 
* Inheritance can sucks, specially when you have many subclasses extending from a base class.
* Principle: Program to an interface, not to an implementation. Interface meaning the “Java interface” or a supertype class (interface or abstract class). Example of this is:
```java
Animal animal = new Dog();
animal.makeSound();
//or even better
a = getAnimal();
a.makeSound();
//Both are much better than programming to an implementation:
Dog d = new Dog();
d.bark();
```
* _Principle_: **Favors composition over inheritance**. Do this creating “Behaviours” classes. For a Duck, you can create a “Flying” behaviour (interface with a Fly method) that multiple classes will implement (FlyingWithWings, FlyingNoWay, etc). Then, you can use an instance of the Flying behaviour in your abstract base class, and then every concrete implementation of that abstract class will decide which “Flying” behaviour will use. This way, an abstract base class like Duck has no code at all for flying. It **delegates** this to different classes, and Duck subclasses only need to worry about deciding which behaviour need to use (and change it at runtime using setters!)

<a name="the-observer-pattern"></a>
## Chapter 2. The Observer Pattern
* Publisher (Subject, Observable) + Subscribers (Observers) = Observer pattern
* _Principle_: **Strive for loosely coupled designs between object that interact**. Observer are loosely coupled in that the observable knows nothing about them, other than that they implement the Observer interface.
* 2 ways of receiving notifications: push (you pass the data inside the notify method) or pull (the notify method just notify the observer that there is a new update and then the observer can do subscriber.getX()). Pull is considered more “correct”.

<a name="the-decorator-pattern"></a>
## Chapter 3. The Decorator Pattern 
* Attaches additional responsibilities to an object dynamically. Provide a flexible alternative to subclassing for extending functionality. In other words, **instead of using inheritance as form of extension, it uses composition & delegation**. 
* Decorators have the same supertype as the objects the decorate (can be through extending the supertype or implementing its interface).
* Decorators wrap the object the want to extend and add the functionality (you can wrap a component with any number of decorators). 
* The decorators add its own behaviour either before and/or after delegating to the object it decorates to do the rest of the job.
* _Principle_: **Open-Closed. Classes should be open for extension but closed for modification.**
* Example:
```java
Beverage beverage = new Espresso();
System.out.println(beverage.getDescription() + “ “ + beverage.cost()
```
This will print something like “Espresso description 2£”.

```java
Beverage beverage2 = new DarkRoast();
beverage2 = new Chocolate(beverage2); //We add chocolate
beverage2 = new Chocolate(beverage2); //We add more chocolate
beverage2 = new Whip(beverage2); //We add whip
System.out.println(beverage2.getDescription() + “ “ + beverage.cost()
```
This will print something like “Dark roast description Chocolate Chocolate Whip 5£”, being the price the sum of every beverage/decorator.

<a name="the-factory-pattern"></a>
## Chapter 4. The Factory Pattern 
* **We want to encapsulate what changes**. Creation code (instantiating different types of objects) usually leads to changes. If we have the classic Pizza example that creates CheesePizza, ClaimPizza, VeggiePizza, etc. every time we add/remove a Pizza we will need to modify code. We want this code to be as encapsulated as possible, so the clients don’t care.
* **Factories are a powerful technique for coding to abstractions, not concrete classes.**
* **Simple Factory** _pattern_: a class with a createObject method that takes a parameter and based on that parameter will create a concrete object of the supertype class. Example:
```java
public Pizza createPizza(String type) {
	if (type.equals(“cheese”) {
		return new CheesePizza();
	} else if (type.equals(“pepperoni”){
		return new PepperoniPizza();
	}
        ...
}
```
* **Factory method pattern**: encapsulates object creation by letting subclasses classes decide what objects to create. The creator class (PizzaStore for example) will be abstract and have an abstract method (let’s call it createPizza()) that every Subclass (NYPizzaStore) will implement the way the like (creating pizzas such as NYStyleCheesePizza(), that will extend/implement the supertype Pizza class). This way, the creator class knows nothing about how the Pizzas are created and only need to worry about use them.
* _Principle_: **Dependency Inversion. Depend upon abstractions. Do not depend upon concrete class.** This principle guides us to avoid dependencies on concrete types and to strive for abstractions. 
* **Abstract Factory pattern**: provides an interface for creating families of related or dependent objects without specifying their concrete classes. 
	Example:
```java
public interface PizzaIngredientFactory {
	public Dough createDough();
	public Sauce createSauce();
	public Cheese createCheese();
	public Clams createClam()
	…
}

public class NYPizzaIngredientFactory implements PizzaIngredientFactory {
	public Dough createDough() {
		return new ThinCrustDough();
	}
	
	public createSauce() {
		return new MariranaSauce();
	}
	
	public createClam() {
		return new FreshClams(); //(Chicago for example would return FrozenClam();
	}
}
...
```

This way, every factory can returns whatever they want, but the PizzaStore just need “a” PizzaIngredientFactory. This factory would be used like this:

```java 
public class ClaimPizza extends Pizza {
	PizzaIngredientFactory ingredientFactory;
	
	public ClaimPizza(PizzaIngredientFactory ingredientFactory) {
		this.ingredientFactory = ingredientFactory;
	}

	void prepare() {
		dough = ingredientFactory.createDough();
		sauce = ingredientFactory.createSauce();
		cheese = ingredientFactory.createCheese();
		…
	}
}
```

This way, we can create a ClaimPizza. We only need to know the set of products/ingredientss we need (dough, sauce, cheese, etc). ClaimPizza doesn’t care about the the real implementation of these products are (what cheese type, what dough type…), because the Factory is the one who cares about that (using object composition, that’s why we need to provide the factory in the constructor). 

* Some differences/similarities between **Factory method** and **abstract factory**:
	* Both encapsulate object creation to keep applications loosely coupled and less dependant on implementations. 
	* **Factory method creates objects through inheritance** (using subclasses that to do the creation). Clients only need to know the abstract type, the subclasses worry about the concrete type. 
	* **Abstract factory does it through object composition**. Provides an abstract type (Factory) for creating a family of products. Subclasses of this type define how those products are produced. 
	* Abstract factory is good to create set of families that need to work together. This means adding a new member to this family (product, ingredient…) would involve changing the interface and therefore changing every implementation, which could be a big deal. This doesn’t happen for the Factory method as it only need method.
	* Sometimes concrete implementations or abstract factories uses factory methods to create their own classes. 

<a name="the-singleton-pattern"></a>
## Chapter 5. The Singleton Pattern 
* Ensures you have at most one instance of a class in your application.
* Provides global access point to that instance.
* Careful on multi-threading environments, as you could end up with 2 singleton instances. Solutions:
	* Synchronizing getInstance() -> straightforward and effective, but will decrease performance by a factor of 100.
	* Eagerly instantiation instead of lazily 
	* Double-checked locking -> synchronizing only the creation block code of the getInstance method. 

<a name="the-command-pattern"></a>
## Chapter 6. The Command Pattern 
* Decouples an object making a request from **the one that knows how to perform it**.
* A command objects encapsulate a **receiver with an action** (or a set of actions)

```java
public class RemoteControl {
		
		Command [] onCommands;
		Command [] offComands;
		…
		public void setCommand(int slot, Command onCommand, Command offCommand) {
			onCommand[slot] = onCom;
			offCommand[slot] = offCom;
		}
		
		public void onButtonWasPushed(int slot) {
			onCommands[slot].execute();
		}
}

public class LightOffCommand implements Command {
	
	Light light;
	public LightOffCommand(Light light) {
		this.light = light;
	}

	@Override
	public void execute() {
		light.off();
	}
}
...

RemoteControl remoteControl = new RemoteControl();
Light livingRoomLight = new Light(“Living Room”);
GarageDoor garageDoor = new GarageDoor(“”);
LightOnCommand livingRoomLightOn = new LightOnCommand(livingRoomLight);
LightOffCommand livingRoomLightOff = new LightOffCommand(livingRoomLight);
...
remoteControl.setCommand(0, livingRoomLightOn, livingRoomLightOff)
...
remoteControl.onButtonWasPushed(0) // lights on!
```

* To avoid overhead, lambda expressions can be used. Instead of creating X classes that implements the Command interface (which only contains one method, execute), we can just use lambda. 

```java 
remoteControl.setCommand(0, () -> livingRoomLight.on(); }, () -> livingRoomLight.off()); } );
```

<a name="the-adapter-pattern"></a>
## Chapter 7. The Adapter and Facade patterns
* **Adapter pattern**: Converts the interface of a class into another interface client expect. **Lets classes work together that couldn’t otherwise because of incompatible interfaces**.
* When you need to use an existing class and its interface is not the one you need, use an adapter.
* Using an adapter means you don’t need to change any of the 2 classes; all the code changes stay in the adapter class, which it’s a big win. 
* There are 2 types: **object** and **class** adapters. Object adapter are achieved by means of composition (composing the adaptee) and class adapters are by means of multiple inheritance. Example of object adapter:
```java
public interface Duck {
	public void quack();
	public void fly();
}

public interface Turkey {
	public void gobble();
	public void fly();
}

public void TurkeyAdapter implements Duck {
	Turkey turkey;

	public TurkeyAdapter(Turkey turkey) {
		this.turkey = turkey;
	}

	public void quack() {
		turkey.gobble();
	}

	public void fly() {
		for (int i = 0; i < 5; i++) {
			turkey.fly();
		}
	}
}

public class DuckTestDrive {
	pubic static void main(String [] args) {
		MallardDuck duck = new MallardDuck();	
        WildTurkey turkey = new WildTurkey();
		Duck turkeyAdapter = new TurkeyAdapter(turkey);

		testDuck(duck);
		testDuck(turkeyAdapter);
	}

	static void testDuck(Duck duck) {
		duck.quack();
		duck.fly();
	}
}

```


* **Facade pattern**: provides **a unified interface to a set of interfaces in a subsystem**. Facade defines a higher level interface that makes the subsystem easier to use. 
* When you need to simplify and unify a large interface or complex set of interfaces, use a facade.

	_Example: watching a movie on our home theathre_
   
   _Steps:_ 
   
	_1. Turn on the popcorn popper (popper.on())_ 

	_2. Start the popper popping (popper.pop())_
    
	_3. Dim the lights (lights.dim(10))_ 
    
	_4. Put the screen down (screen.down())_ 
    
	_5. Turn the projector on (projector.on())_ 
    
	_6. Set the project input to DVD (projector.setInput(dvd))_
	...

	_Using the facade pattern:_
    
    ```java
    public class HomeTheatreFacade {
		Amplifiear amp;
		Tuner tuner;
		DvdPlayer dvd;
		Screen screen;
		PopcornPopper popper;
		…

		public HomeTheathreFacade(Amplifier amp, Tuner tuner…) {
			this.amp = amp;
			this.tuner = tuner;
			…
		}

		public void watchMovie(String movie) {
			popper.off();
			popper.on();
			screen.up();
			projector.off();
			...
		}
	}
    ```
* An adapter wraps an object to change its interface, a decorator wraps an object to add new behaviours and responsibilities, and a face “wraps” a set of objects to simplify.
* _Principle_: Principle of Least Known, also known as _Law or Demeter_ or talk only to your friends. Be careful of the number of classes your object interacts with and also how it comes to interact with those classes. This reduces the dependencies between objects, although it results in more wrapper classes being written to handle these.

<a name="the-template-method-pattern"></a>
## Chapter 8. The Template Method Pattern
* A "template method" defines the steps of an algorithm, deferring to subclasses for the implementation of some of these steps, while others are implemented in the superclass.
* The template method's abstract class may define concrete methods, abstract methods or hooks. Hooks are method that do nothing or default behaviour in the abstract class, but may be overriden in the subclass.
* To prevent subclasses from changing the algorithm in the template method, declare the template method as final.
* _Principle_: The Holliwood Principle. It guides us to put decision making in high-level modules that can decide how and when to call low-level modules. Basically, a high-level module must not depend on low-level modules. "You don't call me, I'll call you".

_Example: Making coffee or tea using a template method_
```java
public abstract class CaffeineBeverage {
    final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }
    //The implementation of these methods are unknown
    abstract void brew();
    abstract void addCondiments();
    
    void boilWater() {
        System.out.println("Boiling water");
    }
    
    void pourInCup() {
        System.out.println("Pouring into cup");
    }
}

public class Tea extends CaffeineBeverage {
    public void brew() {
        System.out.println("Steeping the tea");
    }
    
    public void addCondiments() {
        System.out.println("Adding Lemon")
    }
}

public class Coffee extends CaffeineBeverage {
    public void brew() {
        System.out.println("Dripping Coffee through filter");
    }
    
    public void addCondiments() {
        System.out.println("Adding Sugar and Milk");
    }
}
```




