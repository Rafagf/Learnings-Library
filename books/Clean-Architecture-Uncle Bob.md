# Clean-Architecture-Book-Overview
Overview of Uncle's Bob book on Architecture.

**Table of Contents**
- [Chapter 1. What is Design and Architecture](#design-and-architecture)
- [Chapter 2. A tale of two values](#tale-of-two-values)
- [Chapter 3. Paradigm Overview](#paradigm-overview)
- [Chapter 4. Structured Programming](#structured-programming)


<a name="design-and-architecture"></a>
## Chapter 1. What is Design and Architecture
* What is the difference between boths? You could say _Design_ takes care of the low-level details, whereas _Architecture_ cares about high-level details. In practice, they both need each other and we could assert there's no difference between both of them.
* Developers's productivity will decrease with the pass of the time, despite adding more developers to the project. The only way to avoid this is very well summed up on this quote: "_The only way to go fast, is to go well_"
* Developers usually think the answer to problems is to start over from scratch and redesign the whole system. This is just overconfidence taking over before they end up doing a similar mess to the original problem. The only way to avoid these situations is for organisations to take quality of its software archicture seriously. But we first need to understand what good software architecture is. 


<a name="#tale-of-two-values"></a>
## Chapter 2. A tale of two values
* Every software system proides 2 different values to the stakeholders: _Behaviour_ and _Structure_. _Behaviour_ being what the software does, or in other words, how it makes money for the stakeholders. Many programmers sadly believe this is mainly their job. _Structure_ defines the shape of the software, how easy/hard is to fulfil its desired _Behaviour_ 
* Despite what some may think, _Structure_ should be more important than _Behaviour_ in the long run. This is the only way we have to make sure the _Behaviour_ of the software can progress. Developers need to make sure the sense of urgency always lives on the side of _Structure_, as business managers will always try to put it on the side of _Behaviour_.
* **Fight for Architecture**: "If architecture comes last, the system will become ever more costly to develop, and eventually change will become impossible (...). If that happens, it means the development team did not fight hard enough for what they knew was needed."

<a name="#paradigm-overview"></a>
## Chapter 3. Paradigm Overview
* **Structured programming**: _goto_ statements are harmful. Replacing these with if/then/else, do/while/until and functional decomposotion (being able to create functional "modules", like functions).
* **Object-Oriented programming**: moved the function call stack frame to the heap, allowing local variables declared by a function to exist long after the function returned. In other words, the function became a constructor for a **class**, the local variables became **instance variables** and the nested methods became methods.
* **Functional programming**: the foundational notion of Functional Programming is **mutability**, that is, the notion that values of symbols do not change. This effectively means that a functional language has no assignment statement. 
* What does this have to do with good Architecture? We use **polymorphism** (object-oriented programming) as the mechanism to cross architectural bounderies; we use functional programming to impose discipline on the location and access of data; and we use structured programming as the algorithmic foundation of our modules. 

<a name="#structured-programming"></a>
## Chapter 4. Structured Programming
* It explains why the _goto_ statement wasn't great to write scalable software.
* Functional decomposition: structured programming forces us to recursuvely decompose a program into a set of small provable functions.

