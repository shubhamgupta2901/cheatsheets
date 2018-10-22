 ### Contents
 * [Generic Class](#generic-class)
 * [Generic Methods](#generic-methods)
 * [Bounded Type Parameters](#bounded-type-parameters)
 * [Unbounded Wildcards](#unbounded-wildcards)
 * [Upper Bound Wildcards](#upper-bound-wildcards)
 * [Lower Bound Wildcards](#lower-bound-wildcards)
 * [Type Erasure](#type-erasure)
    
    
* There are two types of bugs in programs: 
    1. *Compile-time bugs*, which can be detected early on; we can use the compiler's error messages to figure out what the problem is and fix it, right then and there.
    2. *Runtime bugs*, they can be much more problematic; they don't always surface immediately, and when they do, it may be at a point in the program that is far removed from the actual cause of the problem.
    
* Generics add stability to our code by making more of our bugs detectable at compile time. 

* **Generics enable types (classes and interfaces) to be parameters when defining classes, interfaces and methods.** Type parameters provide a way for us to re-use the same code with different inputs. The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types.

* Code that uses generics has many benefits over non-generic code:
1. *Stronger type checks at compile time*: 
    A Java compiler applies strong type checking to generic code and issues errors if the code violates type safety. Fixing compile-time errors is easier than fixing runtime errors, which can be difficult to find.

2. *Elimination of casts*.
    The following code snippet without generics requires casting:
    
```java
    List list = new ArrayList();
    list.add("hello");
    String s = (String) list.get(0);
```
   
   When re-written to use generics, the code does not require casting:

```java
    List<String> list = new ArrayList<String>();
    list.add("hello");
    String s = list.get(0);   // no cast
```
3. *Enabling programmers to implement generic algorithms*: By using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.
    
   
    


  
