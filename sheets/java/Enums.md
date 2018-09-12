## Contents
* [Enums in java](#enums-in-java)
* [Enums with constructors](#enums-with-constructors)
* [Enum with customized value](#enum-with-customized-value)

#### Enums in java

* A Java enum type is a special kind of Java class used to define collections of constants. 
* First line inside enum should be list of constants and then other things like methods, variables and constructor can be put. They look like this: 
```java
public class Test 
{ 
    enum Color 
    { 
        RED, GREEN, BLUE; 
    } 
  
    // Driver method 
    public static void main(String[] args) 
    { 
        Color c1 = Color.RED; 
        System.out.println(c1); 
    } 
}
```
OUTPUT:
```java
RED
```

* enums internally implemented by using Class. 
For example: 
```java
public enum Color {
    RED, GREEN, BLUE
}
```
is internally converted to 
```java
class Color
{
     public static final Color RED = new Color();
     public static final Color BLUE = new Color();
     public static final Color GREEN = new Color();
}
```

* Since they are internally reperesented as class, we can not create another class in a package with same name.
* All enums implicitly extend ```java.lang.Enum class```, so it can not extend any other class. However it can implement any number of interfaces.
* Every enum constant is always implicitly ```public static final```. Since it is ```static```, we can access it by using enum Name.
* **We can declare ```main()``` method inside enum. Hence we can invoke enum directly from the Command Prompt**.
For example:
```java
public enum Color {

    RED, GREEN, BLUE;

    public static void main(String[] args) {
        Color color = Color.BLUE;
        System.out.println(color.toString());
    }
}
```
OUTPUT: 
```java
BLUE
```
* **```toString()```**: This method is overridden in ``` java.lang.Enum class```. It returns enum constant name.
* **```valueOf()```** returns the enum constant of the specified string value, if exists.
* **```values()```** method can be used to return all values present inside enum.
* **```ordinal()```** Order is important in enums.By using ```ordinal()``` method, each enum constant index can be found, just like array index.

For example: 

```java
public class Test {

    enum Color {
        RED, GREEN, BLUE;
    }

    public static void main(String[] args) {
        Color[] colors = Color.values();
        for (Color color : colors) {
            System.out.println(color.ordinal() + ": " + color.toString());
        }
    }
}
```
OUTPUT:

```java
0: RED
1: GREEN
2: BLUE
```
* **methods in enum should always have a body,we can not have abstract methods in enums.**

#### Enums with constructors

* Each enum constant represents an object of that enum.
* enum can contain constructor and it is executed separately for each enum constant at the time of enum class loading.
* Constructors in enum have private access, even when we create our own constructors in enum, access modifiers are not allowed.
* This means we can't invoke enum constructor directly (We can't create enum objects explicitly).

EXAMPLE:

```java
public enum Color {

    RED, GREEN, BLUE;

    Color() {
        System.out.println("Constructor called for : "+ this.toString());
    }
}
```

```java
public class Test {

    public static void main(String[] args) {
        Color color = Color.BLUE;
    }
}
```

```OUTPUT
Constructor called for : RED
Constructor called for : GREEN
Constructor called for : BLUE
```

### Enum with customized value

* enum can contain constructor and it is executed separately for each enum constant at the time of enum class loading.
* By default enums have their own string values butwe can also assign some custom values to enums.
```java
enum  Fruits
{
    APPLE(“RED”), BANANA(“YELLOW”), GRAPES(“GREEN”);
}
```
* For enums with custom values, following points are to be kept in mind:
  * We have to create parameterized constructor for this enum class. Why? Because as we know that enum class's object can't be create explicitly so for initializing we use parameterized constructor. 
  * And the constructor cannot be the public or protected it must have private or default modifiers. Why? if we create public or protected, it will allow initializing more than one objects. This is totally against enum concept.
  * We have to create one getter method to get the value of enums.
  Example:
  
  ```java
  public enum Color {
    //This will call enum constructor with one string argument
    RED("#FF0000"), GREEN("#00FF00"), BLUE("#0000FF");

    //Declaring private variable for getting color code.
    private String colorCode;

    // enum constructor : can not be public or protected.
    Color(String colorCode){
        this.colorCode = colorCode;
        System.out.println("Constructor called for "+ this.toString());
    }
    //getter method
    public String getColorCode() {
        return colorCode;
    }
}
```

```java
public class Test {

    public static void main(String[] args) {
        Color[] colors = Color.values();
        for (Color color : colors) {
            System.out.println(color.toString() + ": " + color.getColorCode());
        }
    }
}

```

OUTPUT:

```java
Constructor called for RED
Constructor called for GREEN
Constructor called for BLUE
RED: #FF0000
GREEN: #00FF00
BLUE: #0000FF
```

NOTE: **If you want, you can also provide one or more constructor to your Enum as it also supports constructor overloading like normal Java classes.**
