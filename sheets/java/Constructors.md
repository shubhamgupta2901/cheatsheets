### Contents
 * [Constructors](#constructors)
 * [How does constructors work](#how-does-constructors-work)
 * [Types of constructors](#types-of-constructors)
 * [Constructor Overloading](#constructor-overloading)
 * [Constructor Chaining](#constructor-chaining)
 * [Private constructors](#private-constructors)
 * [Constructors of Sub and Super Classes](#constructors-of-sub-and-super-classes)
 * [Constructors in Abstract classes](#constructors-in-abstract-classes) 
 * [Static constructors ?](#static-constructors-?)
 * [Constructors in Abstract Classes](#constructors-in-abstract-classes)
 * [Summary](#summary)
 
 
  ## Constructors
  * Constructor is block of code(similar to method) which is used to initialize an object. It is called when an instance of object is created (using ```new``` ) and memory is allocated to object.
  * The constructor name matches with the class name and it doesn’t have a return type.
   
   #### How does constructors work
   A default constructor in a class looks like this. A default constructor is a constructor which does not take any parameter.
  
  ```java
   public class Hello {
   String name;
   
   // Constructor
   Hello(){
      this.name = "Shubham";
   }
   
   public static void main(String[] args) {
      Hello obj = new Hello();
      System.out.println(obj.name);
   }
}
  ```
   When we create object of the above class :
  ```java
   Hello obj = new Hello(); 
  ```
  OUTPUT of ```main()```
  ```java
  Shubham
  ```
  The ```new``` keyword here creates a new object of class ```Hello``` by allocating memory. ```obj``` is then assigned the value of this memory's reference.
  The constructor is invoked in the object to initialize the values of this newly created object. So once the constructor is called the value of ```name``` in for object ```obj``` will be ```Shubham```.
  
  #### Types of Constructors 
  * **Default Constructor**  If we do not implement any constructor in our class, Java inserts a default constructor to the code in our behalf. This initializes the object's variable with default values of their respective data types.
  * Default constructors are not visible in the source code, as they are inserted at the time of compilation when you have not provided any constructor explicitly.
  * If you provide a constructor to your, then the default constructor is not generated by the compiler.
  * Consider the following example: 
 ```java 
  public class Hello {
   String name;
}
 ```
  Since no other constructor is provided in the class, at the time of compilation, a default constructor is provided by the compiler. So the class essentially looks something like this; 
  ```java 
  public class Employee {
   String name;
   int credits;
   boolean isActive;
   
   Employee(){
    name = null;
    credits = 0; 
    isActive = false;
   }
}
 ```
 * The default constructor is the no-argument constructor automatically generated unless you define another constructor. It initialises any uninitialised fields to their default values.
 
 * *Why default constructor?* 
 
 * **No-argument constructor** A no-argument constructor is similar to the default constructor in a sense that it also does not take any parameter but unlike default constructor, it is explicitly defined by user.
 * Example: 
```java 
  public class Employee {
   String name;
   int credits;
   boolean isActive;
   
   Employee(){
    credits = 25; 
    isActive = true;
   }
}
```
 In the above example, use of no-argument constructor can be seen, you want all the objects created to be initialised with default values, which are different than the data type provided values. So when the objects of ```Employee``` class is created, all of them will have 25 credits and will be active, when they are initialized.
 
 * NOTE: The compiler automatically provides a no-argument, default constructor for any class without constructors. This default constructor will call the no-argument constructor of the superclass. In this situation, the compiler will complain if the superclass doesn't have a no-argument constructor so you must verify that it does. If your class has no explicit superclass, then it has an implicit superclass of Object, which does have a no-argument constructor.
 
 * **Parameterized Constructors** Constructors which take parameters are called parameterized constructor. They are used to provide different values to the distinct objects at the time of initialization.

 
 Example: 
 
 ```java
 public class Employee {

   int empId;  
   String empName;  
	    
   //parameterized constructor with two parameters
   Employee(int id, String name){  
       this.empId = id;  
       this.empName = name;  
   }  
   void info(){
        System.out.println("Id: "+empId+" Name: "+empName);
   }  
	   
   public static void main(String args[]){  
	  Employee obj1 = new Employee(10245,"Chaitanya");  
	  Employee obj2 = new Employee(92232,"Negan");  
	  obj1.info();  
	  obj2.info();  
   }  
}
 ```
 Output:
```java
Id: 10245 Name: Chaitanya
Id: 92232 Name: Negan
```

#### Constructor Overloading 

* Just like methods, constructors can also be overloaded. Since constructors don't have a return type, we can overloading is obtained solely on basis of difference in types and number of arguments.
So a class can have as many constructors (with different signatures) as you want.
 ```java
 class Example
{
      private int var;
      //no arg constructor
      public Example()
      {
             this.var = 10;
      }
      //parameterized constructor
      public Example(int num)
      {
             this.var = num;
      }
      public int getValue()
      {
              return var;
      }
      public static void main(String args[])
      {
              Example2 obj = new Example2();
              Example2 obj2 = new Example2(100);
              System.out.println("var is: "+obj.getValue());  //Output var is: 0
              System.out.println("var is: "+obj2.getValue()); // Output var is: 100
      }
}
 ```
 
 In the above example, we have two constructors, a default constructor and a parameterized constructor. When we do not pass any parameter while creating the object using new keyword then default constructor is invoked, however when you pass a parameter then parameterized constructor that matches with the passed parameters list gets invoked.(Compile time polymorphism or static binding.)

* *C++'s copy constructor is a parameterized constructor in Java.* There is no copy constructor in java. But, we can copy the values of one object to another like copy constructor in C++.
```java
class Student6{  
    int id;  
    String name;  
    Student6(int i,String n){  
    id = i;  
    name = n;  
    }  
      
    Student6(Student6 s){  
    id = s.id;  
    name =s.name;  
    }  
    void display(){System.out.println(id+" "+name);}  
   
    public static void main(String args[]){  
    Student6 s1 = new Student6(111,"Karan");  
    Student6 s2 = new Student6(s1);  
    s1.display();  
    s2.display();  
   }  
}  

```

Output of the above code: 
```java
111 Karan
111 Karan
```

* **When you only implement parameterized constructor?** When you don’t implement any constructor in your class, compiler inserts the default constructor into your code, however when you implement any constructor (in above example I have implemented parameterized constructor with int parameter), then you don’t receive the default constructor by compiler into your code.

Example: 
```java
class Example3
{
      private int var;
      public Example3(int num)
      {
             var=num;
      }
      public int getValue()
      {
              return var;
      }
      public static void main(String args[])
      {
              Example3 myobj = new Example3();
              System.out.println("value of var is: "+myobj.getValue());
      }
}
```

The above code generates compile time error, as it is not able to find the constructor with no arguments.If we remove the parameterized constructor from the above code then the program would run fine, because then compiler would insert the default constructor into your code.

#### Constructor Chaining

* **We can chain constructors.** When A constructor calls another constructor of same class then this is called constructor chaining. 
* This can be achieved by using ```this``` keyword.If you want to invoke a parameterized constructor then do it like this: ```this(parameter list)```.
Example: 
```java
class Employee
{   
    public String empName;
    public int empSalary;
    public String address;

    //default constructor of the class
    public Employee()
    {
    	//this will call the constructor with String param
        this("Chaitanya");
    }

    public Employee(String name)
    {
    	//call the constructor with (String, int) param
    	this(name, 120035);
    }
    public Employee(String name, int sal)
    {
    	//call the constructor with (String, int, String) param
    	this(name, sal, "Gurgaon");
    }
    public Employee(String name, int sal, String addr)
    {
    	this.empName=name;
    	this.empSalary=sal;
    	this.address=addr;
    }

    void disp() {
    	System.out.println("Employee Name: "+empName);
    	System.out.println("Employee Salary: "+empSalary);
    	System.out.println("Employee Address: "+address);
    }
    public static void main(String[] args)
    {
        Employee obj = new Employee();
        obj.disp();
    }
}

```
Output: 
```java
Employee Name: Chaitanya
Employee Salary: 120035
Employee Address: Gurgaon
```
* The real purpose of Constructor Chaining is that you can pass parameters through a bunch of different constructors, but only have the initialization done in a single place. This allows you to maintain your initializations from a single location, while providing multiple constructors to the user. If we don’t chain, and two different constructors require a specific parameter, you will have to initialize that parameter twice, and when the initialization changes, you’ll have to change it in every constructor, instead of just the one.

#### Private Constructors

* A Constructor, like a method can have any access modifier. However, if the constructor is private, it can not be invoked from anywhere outside the class. This means the object creation of this class can only happen from inside the class, and not from anywhere else.

* The use of private constructors is to serve **Singleton classes**. A singleton class is one which limits the number of objects creation to one. Using private constructor we can ensure that no more than one object can be created at a time. By providing a private constructor you prevent class instances from being created in any place other than this very class. We will see in the below example how to use private constructor for limiting the number of objects for a singleton class.

* Example: 
```java
public class SingleTonClass {
   //Static Class Reference
   private static SingleTonClass obj=null;
   private SingleTonClass(){
      /*Private Constructor will prevent 
       * the instantiation of this class directly*/
   }
   public static SingleTonClass objectCreationMethod(){
	/*This logic will ensure that no more than
	 * one object can be created at a time */
	if(obj==null){
	    obj= new SingleTonClass();
	}
        return obj;
   }
   public void display(){
	System.out.println("Singleton class Example");
   }
  
}
public class Driver {
public static void main(String args[]){
	//Object cannot be created directly due to private constructor 
        //This way it is forced to create object via our method where
        //we have logic for only one object creation
	SingleTonClass myobject= SingleTonClass.objectCreationMethod();
	myobject.display();
   }
}
```

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

* When inheriting from another class, super() has to be called first in the constructor. If not, the compiler will insert that call. This is why super constructor is also invoked when a Sub object is created.
* This doesn't create two objects, only one ```Child``` object. The reason to have super constructor called is that if super class could have private fields which need to be initialized by its constructor.
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
 * call to constructor of ```Parent``` class is the first thing compiler does while constructing/initializint the ```Child``` object.If you want to explicitly invoke ```Parent``` class' constructor it needs to be the first statement in ```Child``` constructor. **i.e.```super()``` has to be first statement in a constructor.** The parent class' constructor needs to be called before the subclass' constructor. This will ensure that if you call any methods on the parent class in your constructor, the parent class has already been set up correctly. So it saves us from working with partially constructed objects.
 
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
#### Static constructors ?
* A constructor can not be marked static. You will get *modifier static not allowed here*.
* It’s actually pretty simple to understand – Everything that is marked static belongs to the class only, for example static   method cannot be inherited in the sub class because they belong to the class in which they have been declared.

* Static methods belong to a class, not an object. Similary for static variables (which can only be accessed by static methods), only one copy exists for the entire class, as opposed to a member variable, which is created once for each object created from a class. They are used to store data for the class. On the other hand, object methods are meant to operate on the data associated with a single instance of a class, an object. Constructors are the code that is used to initialize an object and set it's data to an initial state. They are executed immediately (and automatically) after the memory has been allocated to store a new object. Even if you do not explicitly define a constructor, a kind of "default constructor" is executed in order to map the object's member variables and the object's method code to the new object.So logically it does not make sense for a constructor to be static.

* Another reasoning, Since each constructor is being called by its subclass during creation of the object of its subclass, so if you mark constructor as static the subclass will not be able to access the constructor of its parent class because it is marked static and thus belong to the class only. This will violate the whole purpose of inheritance concept and that is reason why a constructor cannot be static.
Example: 
```java
public class StaticDemo
{
     public StaticDemo()
     {
         /*Constructor of this class*/
         System.out.println("StaticDemo");
     }
}
public class StaticDemoChild extends StaticDemo
{
     public StaticDemoChild()
     {
          /*By default super() is hidden here */
          System.out.println("StaticDemoChild");
     }
     public void display()
     {
          System.out.println("Just a method of child class");
     }
     public static void main(String args[])
     {
          StaticDemoChild obj = new StaticDemoChild();
          obj.display();
     }
}
```

Output:
```java
StaticDemo
StaticDemoChild
Just a method of child class
```

* When we created the object of child class, it first invoked the constructor of parent class and then the constructor of it’s own class. It happened because the new keyword creates the object and then invokes the constructor for initialization, since every child class constructor by default has super() as first statement which calls it’s parent class’s constructor. The statement super() is used to call the parent class(base class) constructor.This is the reason why constructor cannot be static – Because if we make them static they cannot be called from child class thus object of child class cannot be created.

* However **static blocks** do provide an alternative to static constructors.

#### Constructors in Abstract Classes

* **Abstract class can have constructors** and it is defined and behaves just like any other class's constructor. **Except that abstract classes can't be directly instantiated**, only extended, so the use is therefore always from a subclass's constructor.

 * Consider this:
 ```java
 abstract class Product { 
    int multiplyBy;
    public Product( int multiplyBy ) {
        this.multiplyBy = multiplyBy;
    }

    public int mutiply(int val) {
       return multiplyBy * val;
    }
}

class TimesTwo extends Product {
    public TimesTwo() {
        super(2);
    }
}

class TimesWhat extends Product {
    public TimesWhat(int what) {
        super(what);
    }
}
 
 ```
 
* The superclass ```Product``` is abstract and has a constructor. The concrete class ```TimesTwo``` has a constructor that just hardcodes the value 2. The concrete class ```TimesWhat``` has a constructor that allows the caller to specify the value. Abstract constructors will frequently be used to enforce class constraints or invariants such as the minimum fields required to setup the class.
* You would define a constructor in an abstract class if you are in one of these situations:
  1)  you want to perform some initialization (to fields of the abstract class) before the instantiation of a subclass actually takes place.
  2)  you have defined final fields in the abstract class but you did not initialize them in the declaration itself; in this case, you MUST have a constructor to initialize these fields


#### Summary
* Constructors are not methods and they don’t have any return type.
* Constructor name should match with class name .
* A constructor in Java doesn't actually "build" the object, it is used to initialize fields. Once the memory allocation for the object is done using ```new``` operator, constructor will initialize the fields for that object.
* If you don’t implement any constructor within the class, compiler will do it for you.
* A constructor can also invoke another constructor of the same class – By using this(). If you want to invoke a parameterized constructor then do it like this: this(parameter list).
* * If a class doesn’t have a no-arg(default) constructor then compiler would not insert a default constructor in the class as it does in normal scenario.
*  **Since constructors do not need any object reference to be called, only new operator, so they can be called in static methods as well.** Although constructors are non-static, and belong to instance of class.
* Every class has a constructor whether it’s a normal class or a abstract class. Abstract class can have constructor and it gets invoked when a class, which extends it, is instantiated. (i.e. object creation of concrete class).
* When a child class' constructor is invoked, the first thing it does is invoke the default constructor of parent class (unless theres a explicit call to parent class' constructor, in which case it should be first thing you do in child class' constructor). Remember constructors are only used for initializing the fields of class. So memory is only allocated for child class's object, but the constructor call to parent ensures that all the fileds of parent clss are initialized and are ready to be used by child class.
* Constructor can use any access specifier, they can be declared as private also. Private constructors are possible in java but there scope is within the class only. So new objects can only be created from inside the class if we have private constructors.
* **Like constructors method can also have name same as class name, but still they have return type, though which we can identify them that they are methods not constructors* Constructors are not methods and they don’t have any return type.**
* this() and super() should be the first statement in the constructor code. If you don’t mention them, compiler does it for you accordingly.
* Constructor overloading is possible but **overriding is not possible**. Which means we can have overloaded constructor in our class but we can’t override a constructor.
* Constructors can not be inherited.
* Interfaces do not have constructors.
* Constructors cannot be abstract, final, static and synchronised while methods can be.
