### Contents
 * [Inheritance](#inheritance)
 * [Object creation of Sub Class](#object-creation-of-sub-class)
 * [Constructors of Sub and Super Classes](#constructors-of-sub-and-super-classes)
 * [Types of Inheritance](#types-of-inheritance)
   * [Inheritance thorugh Classes](#inheritance-through-classes)
   * [Addition Inheritance through Interfaces](#additional-interfaces-through-interfaces)
 * [Multiple Inheritance in JAVA](#multiple-inheritance-in-java)
   * [Why multiple inheritance is not supported in java](#why-multiple-inheritance-is-not-supported-in-java)
   * [JAVA and Multiple Inheritance](#java-8-and-multiple-inheritance)
 
  
### Inheritance

* Inheritance is one of the important concepts in OOPS. It is a mechanism which allows one class to inherit the features of another class.

* Inheritance supports the concept of **reusability**, i.e. when we want to create a new class and there is already a class that includes some of the code that we want, we can derive our new class from the existing class. By doing this, we are reusing the fields and methods of the existing class.

* Another reason to use Inheritance is to achieve **run time polymorphism** by method overriding.

* **Super Class**: The class whose features are inherited is known as super class(or a base class or a parent class).

* **Sub Class**: The class that inherits the other class is known as sub class(or a derived class, extended class, or child class). The subclass can add its own fields and methods in addition to the superclass fields and methods.

* The keyword used for inheritance is **```extends```**.

```java
class child-class extends parent-class  
{  
   //methods and fields  
}  
```

* EXAMPLE:

```java
class Bicycle
{ 
    // the Bicycle class has two fields 
    public int gear; 
    public int speed; 
          
    // the Bicycle class has one constructor 
    public Bicycle(int gear, int speed) 
    { 
        this.gear = gear; 
        this.speed = speed; 
    } 
          
    // the Bicycle class has three methods 
    public void applyBrake(int decrement) 
    { 
        speed -= decrement; 
    } 
          
    public void speedUp(int increment) 
    { 
        speed += increment; 
    } 
      
    // toString() method to print info of Bicycle 
    public String toString()  
    { 
        return("No of gears are "+gear 
                +"\n"
                + "speed of bicycle is "+speed); 
    }  
} 
```

```java
class MountainBike extends Bicycle
{ 
      
    // the MountainBike subclass adds one more field 
    public int seatHeight; 
  
    // the MountainBike subclass has one constructor 
    public MountainBike(int gear,int speed, 
                        int startHeight) 
    { 
        // invoking base-class(Bicycle) constructor 
        super(gear, speed); 
        seatHeight = startHeight; 
    }  
          
    // the MountainBike subclass adds one more method 
    public void setHeight(int newValue) 
    { 
        seatHeight = newValue; 
    }  
      
    // overriding toString() method 
    // of Bicycle to print more info 
    @Override
    public String toString() 
    { 
        return (super.toString()+ 
                "\nseat height is "+seatHeight); 
    }
      
} 
```

```java
public class Test
{ 
    public static void main(String args[])  
    {
        MountainBike mb = new MountainBike(3, 100, 25); 
        System.out.println(mb.toString()); 
              
    } 
} 
```

OUTPUT

```java
No of gears are 3
speed of bicycle is 100
seat height is 25

```
* ```Bike``` is a class which defines a blueprint for bikes: they have gears and a speed. ```MountainBike``` is a special kind of Bike which also has gears and speed, along with it, it also has seatHeight. Rather than creating a new class with all these features, we can reuse the attributes of ```Bike``` in ```MountainBike``` by extending it, and only define new attributes. 

* In above program, when an object of ```MountainBike``` class is created, a copy of the all methods and fields of the superclass acquire memory inside this object. That is why, by using the object of the subclass we can also access the members of a superclass.


#### Object creation of Sub Class

* Before moving on to the concepts of Inheritance, it is important to understand how the objects of a child class are created in memory heap when they are instantiated.

* When a new object is created, there are three steps of object declaration and assignment: 
   * declare a reference variable,
   * create an object
   * and assign the object to the reference.

* So when an object of a subclass is created (because somebody said ```new```), the object gets space for all the instance variables - all the way up the inheritance tree.  When an object of a subclass is created, it's almost as though multiple objects materialize—the object being created and one object per each superclass. 

* But in reality only one object of superclass is being created, This object being created has layers of itself representing each superclass.

* **So when a new object is created, only one object gets space in the heap - the one which is created.But it contains all the parts of itself, and the super class parts of itself which it inherited. All instance variables from both class have to be here.** 

* For example if we create instance of a class ```Hippo``` which extends ```Animal```, it always has a parent, ```Object``` class. So, only one object of ```Hippo``` is created, but it contains all the parts of itself which it inherited.  

```java
public class Animal{
 Foo k;
 int x,v;
}

public class Hippo extends Animal{
 Foo X,Y;
}
```

![object creation](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/object%20creation.jpg)


#### Constructors of Sub and Super Classes

 * Creating an object of sub class also invokes the constructor of super class.
Example
```java
class Parent {
    String s;
 
    public Parent(){
    	System.out.println("Parent");
    }
}
 
public class Child extends Parent {
 
    public Child(){
    	System.out.println("Child");
    }
 
    public static void main(String[] args){
    	Sub s = new Child();
    }
}

```
Output of the above code is: 
```java
Parent
Child
```

* **When inheriting from another class, ```super()``` has to be called first in the constructor.** If not, the compiler will insert that call. This is why Parent constructor is also invoked when a Child object is created.

* After compiler inserts the ```Parent``` constructor in the ```Child``` constructor, the code looks like this:
```java
 public Sub(){
    	super();
    	System.out.println("Sub");
    }
```

* This evidently gives rise to a popular error. Since the child constructor implicitly calls the parent constructor. There is always a chance that the ```Parent``` class has not defined any constructor. In this case, compiler provides a default constructor to the ```Parent``` class and everything works fine. In cases when the ```Parent``` class defines a no-argument constructor, then also the code works fine, however, problem arises when the ```Parent``` class implements parameterized constructors, and no no-arg constructors. In this case the compiler does not provide any default constructor, which leads to compile time error: *"Implicit super constructor is undefined for default constructor. Must define an explicit constructor"*.


Example: 
```java
```java
class Parent {
    String s;
 
    public Parent(String s){
    	System.out.println(s);
    }
}
 
public class Child extends Parent {
 
    public Child(){
    	System.out.println("Child");
    }
 
    public static void main(String[] args){
    	Sub s = new Child();
    }
} 

```
* There are two ways you can neglect the problem:
  1. Implement a no-arg constructor in the ```Parent``` class.C
  2. You can explicitly call the parameterized constructor of ```Parent``` inside the ```Child``` class.  In this case we have to ensure that the call to ```Parent``` constructor is first thing we do in the constructor. We use ```super``` keyword for this purpose. 
  
 * call to constructor of ```Parent``` class is the first thing compiler does while constructing/initializing the ```Child``` object. If you want to explicitly invoke ```Parent``` class' constructor it needs to be the first statement in ```Child``` constructor. **i.e.```super()``` has to be first statement in a constructor.**  
 
 * The reason parent class' constructor needs to be called before the subclass' constructor is - it will ensure that if you call any methods of the Parent class in your Child constructor, the parent class has already been set up correctly. So it saves us from working with partially constructed objects.
 
 ```java
class Parent {
    String s;
 
    public Parent(String s){
    	System.out.println(s);
    }
}
 
public class Child extends Parent {
 
    public Child(){
    	super("Parent");
    	System.out.println("Child");
    }
 
    public static void main(String[] args){
    	Sub s = new Child();
    }
} 
 ```
  
* In brief, the rules is: child class constructor has to invoke parent class instructor, either explicitly by programmer or implicitly by compiler. For either way, the invoked parent constructor has to be defined. 

For example: This generates compile time error: *call to super must be first statement in constructor*
```java
public class MyClass {
    public MyClass(int x) {}
}

public class MySubClass extends MyClass {
    public MySubClass(int a, int b) {
        int c = a + b;
        super(c);  // COMPILE ERROR
    }
}
```
 ### Types of Inheritance
 
 #### Inheritance thorugh Classes
 
* **Single Inheritance** : In single inheritance, subclasses inherit the features of one superclass. In image below, the class A serves as a base class for the derived class B.

![single inheritance](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/single%20inheritance.png)

* **Multilevel Inheritance**: In Multilevel Inheritance, a class inherits features of a another class which itself has In below image, the class A serves as a base class for the derived class B, which in turn serves as a base class for the derived class C. In Java, a class cannot directly access the grandparent’s members.

![multilevel Inheritance](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/multilevel%20inheritance.png)

* **Hierarchical Inheritance**: In Hierarchical Inheritance, one class serves as a superclass (base class) for more than one sub class.In below image, the class A serves as a base class for the derived class B,C and D.

![hierarchical inheritance](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/heirarchial%20inheritance.png)

#### Addition Inheritance through Interfaces

* **Multiple Inheritance**:  In Multiple inheritance ,one class can have more than one superclass and inherit features from all parent classes. Please note that Java does not support multiple inheritance with classes. In java, we can achieve multiple inheritance only through Interfaces. In image below, Class C is derived from interface A and B.

![multiple inheritance](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/multiple%20inheritance.png)

* **Hybrid Inheritance**: It is a mix of two or more of the above types of inheritance. Since java doesn’t support multiple inheritance with classes, the hybrid inheritance is also not possible with classes. In java, we can achieve hybrid inheritance only through Interfaces.

![hybrid inheritance](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/hybrid%20inheritance.png)


### Multiple Inheritance in JAVA

* Multiple Inheritance is a feature of object oriented concept, where a class can inherit properties of more than one parent class. The problem occurs when there exist methods with same signature in both the super classes (and subclass). On calling the method, the compiler can not bind the method call to the method definition as there are more than one definition available. 

#### Why multiple inheritance is not supported in java

* Multiple inheritance is not supported in java to avoid ambiguity.

* Consider a scenario where Parent1, Parent2, and Test are three classes. The Test class inherits Parent1 and Parent2 classes. If Parent1 and Parent2 classes have the same method and you call it from child class object, there will be ambiguity to call the method of Parent1 or Parent2 class.

```java

// First Parent class 
class Parent1 
{ 
    void fun() 
    { 
        System.out.println("Parent1"); 
    } 
} 
  
// Second Parent Class 
class Parent2 
{ 
    void fun() 
    { 
        System.out.println("Parent2"); 
    } 
} 
  
// Error : Test is inheriting from multiple 
// classes 
class Test extends Parent1, Parent2 
{ 
   public static void main(String args[]) 
   { 
       Test t = new Test(); 
       t.fun(); 
   } 
} 
```
#### JAVA and Multiple Inheritance

* Before Java 8, interfaces only had abstract methods. The implementation of these methods has to be provided in a separate class. So, if a new method is to be added in an interface, then its implementation code has to be provided in the class implementing the same interface. 

* This way multiple inheritance could be achieved in JAVA through interfaces. Because even if a class is implementing two interfaces, and both the interfaces had same method, it will eventually need overriding it in the class, where its implementation can be provided. This would avoid the ambiguity and multiple inheritance could be achieved.

* Although it should be noted that if two interfaces have **same method name, parameters and return type**, only one implementation of both the methods is allowed in child class.This is because if a class implements two interfaces, and each interface defines a method that has identical signature,name and return type, then in effect there is only one method, and they are not distinguishable. 

```java
public interface Accessible {
    void sayHello();
}
```
```java
public interface Clickable {
    void sayHello();
}
```

```java
public class Test implements Accessible, Clickable {
    @Override
    public void sayHello() {
        System.out.println("Hello");

    }

    public static void main(String[] args) {
        Test test = new Test();
        test.sayHello();
    }
}

```

OUTPUT:
```java
Hello
```

* If the **method signature** of the two methods is different, child class will have to impelement both the methods separately:

```java
public interface Accessible {
    void sayHello(String name);
}
```

```java
public interface Clickable {
    void sayHello();
}
```

```java
public class Test implements Accessible, Clickable {
    @Override
    public void sayHello() {
        System.out.println("Hello");
    }

    @Override
    public void sayHello(String name) {
        System.out.println("Hello "+  name);
    }

    public static void main(String[] args) {
        Test test = new Test();
        test.sayHello();
        test.sayHello("Shubham");
    }
}
```

OUTPUT:
```java
Hello
Hello Shubham
```
* However if we wish to implement two interfaces with same methods having **same signature but different return types**, **then it is impossible to implement both the interface simultaneously.**

```java
public interface Clickable {
    int sayHello();
}
```

```java
public interface Accessible {
    void sayHello();
}
```

```java
public class Test implements Accessible, Clickable {
   @Override
    public void sayHello() {

    }

    @Override
    public int sayHello() {
        return 0;
    }
}
```

* **Compiler would not allow us to do that.** Because it still does not solve the issue of ambiguity. When a method will be called, since both the methods have same signatures, it would not be possible to bind the method call to method definition.

* With JAVA 8 however, new **default methods** have been introduced in interfaces. These methods are not abstract and have their default implementation in Interface only. If a class implementing the interface wants to override the definition of method provided in the interface, it is free to do so, otherwise a default implementation is already provided.

* This allows us to add new methods to the interfaces without breaking the existing implementation of these interfaces and maintain backward compatibility.

* So an interface with default method looks like this:

```java
public interface OldInterface {

     void existingMethod();

     default public void newDefaultMethod(){
         System.out.println("New default method is added in interface.");
     }

}
```

```java
public class OldInterfaceImplementation implements OldInterface {

    @Override
    public void existingMethod() {
        System.out.println("Existing Method");
    }

    public static void main(String[] args) {
        OldInterfaceImplementation impl = new OldInterfaceImplementation();
        impl.existingMethod();
        impl.newDefaultMethod();
    }
}
```

OUTPUT: 
```java
Existing Method
New default method is added in interface.
```
* The question arises when we want to implement two interfaces in our class and both of them have a default method with same name and signature. How does JAVA avoid ambiguity in those cases.

* **In case both the implemented interfaces contain deafult methods with same method signature, the implementing class must override the default method.** .While overriding either you can provide your own implementation, or use super keyword to use the implementation of one or both the interfaces.

```java
interface Accessible{
	void access();

	default void print(){
		System.out.println("print from Accessible");
	}
}
```

```java
interface Clickable{
	void click();

	default void print(){
		System.out.println("Print from Clickable");
	}
}
```

```java
public class Test implements Accessible,Clickable{
    @Override
    public void click() {

    }

    @Override
    public void access() {

    }

    @Override
    public void print() {
        System.out.println("Print from Test");
        Accessible.super.print();
        Clickable.super.print();

    }


    public static void main(String[] args) {
        Test test = new Test();
        test.print();
    }
}
```

OUTPUT:

```java
Print from Test
print from Accessible
Print from Clickable
```

