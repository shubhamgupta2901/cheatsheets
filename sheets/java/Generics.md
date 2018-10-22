 ### Contents
 * [Generic Classes](#generic-class)
 * [Multiple Type Parameters](#multiple-type-parameters)
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
    
   
#### Generic Classes

* Consider a ```Box``` class that operates on objects of any type. It needs only to provide two methods: ```set()```, which adds an object to the box, and ```get()```, which retrieves it: 

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

* Since its methods accept or return an Object, we are free to pass in whatever you want, provided that it is not one of the primitive types. There is no way to verify, at compile time, how the class is used. One part of the code may place an ```Integer``` in the box and expect to get ```Strings``` out of it resulting in a runtime error.

* Something like this:

```java
public static void main(String[] args) {
      Box box = new Box();
      box.setItem(10);
      String s = (String) box.getItem();
      System.out.println(s);
   }
```
* This will generate the following error:
```Exception in thread "main" java.lang.ClassCastException: java.base/java.lang.Integer cannot be cast to java.base/java.lang.String```

* We can use generic class instead, with the following syntax:

```java
class name<T1, T2, ..., Tn> { /* ... */ }
```

* To update the Box class to use generics, you create a generic type declaration by changing the code ```public class Box``` to ```public class Box<T>```. This introduces the type variable, ```T```, that can be used anywhere inside the class.

```java
public class Box<T> {

    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return this.item;
    }
}
```

* A type variable can be any non-primitive type you specify: any class type, any interface type, any array type, or even another type variable.This same technique can be applied to create generic interfaces.

* It can be used in the following way:

```java
public class App {

    public static void main(String[] args) {
      Box<Integer> integerBox = new Box<>();
      integerBox.setItem(10);
      Integer integerItem = integerBox.getItem();
      System.out.println(integerItem);

      Box<String> stringBox = new Box<>();
      stringBox.setItem("Shubham");
      String stringItem = stringBox.getItem();
      System.out.println(stringItem);
    }
}
```
OUTPUT:
```java
10
Shubham
```

* Although the object of ```Box``` should be instantiated the following way:

```java
 Box<Integer> integerBox = new Box<Integer>();
 ```
but since Java SE 7 and later, we can replace the type arguments required to invoke the constructor of a generic class with an empty set of type arguments (<>) as long as the compiler can determine, or infer, the type arguments from the context. For example, you can create an instance of Box<Integer> with the following statement:

```java
Box<Integer> integerBox = new Box<>();
```
This is called **Type Inference**.

#### Multiple Type Parameters

*  A generic class can have multiple type parameters. Something like this: 

```java
public interface Pair<K,V> {
    K getKey();
    V getValue();
}
```

```java
public class OrderedPair<K,V> implements Pair<K,V> {

    private  K key;
    private  V value;

    public OrderedPair(K key, V value){
        this.key = key;
        this.value = value;
    }

    @Override
    public K getKey() {
        return key;
    }

    @Override
    public V getValue() {
        return value;
    }
}
```

* 


  
