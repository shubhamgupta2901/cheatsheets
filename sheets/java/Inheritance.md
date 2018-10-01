### Contents
 * [Inheritance](#inheritance)
 * [Object creation of Sub Class](#object-creation-of-sub-class)
 * [Constructors of Sub and Super Classes](#constructors-of-sub-and-super-classes)
 * [Types of Inheritance](#types-of-inheritance)
 
 
 #### Types of Inheritance
 
* **Single Inheritance** : In single inheritance, subclasses inherit the features of one superclass. In image below, the class A serves as a base class for the derived class B.

 ------------------------------------------------------------------------------------------------------------------------
  
### Inheritance

* Inheritance is one of the important concepts in OOPS. It is a mechanism which allows one class to inherit the features of another class.

* Inheritance supports the concept of **reusability**, i.e. when we want to create a new class and there is already a class that includes some of the code that we want, we can derive our new class from the existing class. By doing this, we are reusing the fields and methods of the existing class.

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




