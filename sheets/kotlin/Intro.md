## Kotlin

* It's a dialect of Java.

#### Hello World

```
fun main(args: Array<String>){
    println("Hello World")
}
```
Output:
```
Hello World
```

* Inside the file, we have a function - a block of code. use keyword ```fun```. The name of function is ```main``` and we pass set of arguments.




### Variables
* Variables are container of data. We user keyword ```var``` to declare a variable, We can change value of ```var``` during execution of program, but we can not change variable type. Example:
```
var name = "Shubham"
var age = 27
age = 28
```

* Changing type of variables
```
 var firstName = "Shubham"
 println(firstName)
 firstName = 23 // Compile type error: The integer literal does not conform to expected type String
```

* We can also use keyword ```val``` to declare a variable,(we need to assign values to ```val``` during declaration) we can not change the value of ```val```.
```
val age = 27
age = 28 //Val cannot be reassigned
```

### Data types in Kotlin:
* **Char and String**
* ```Char``` is a single character, and ```String```  is collection of ```Char```.
* Char is surrounded by single quotes: ``` 'c'```, String is surrounded by double quotes: ```"Shubham"```
* 

* **Number**
* Number includes JAVA's 
  Regular: byte (8 bit), short(16 bit), int(32 bit), long(64 bit)
  Decimal: float, double
  
* Kotlin implicitly assigns a type by looking at the value, To get the data-type Kotlin has assigned to a variable we can use ```variableName::class.java```. Eg:
```
var price   = 29.99f
println(price::class.java)
```
OUTPUT:
```
float
```
* Implicit vs Explicit variable types: When we create a variable, the systems automatically decides an appropiate data type depending on the value assigned to it.
* We can explicitly instruct Kotlin to assign different type to the variable. This is known as explicit declaration of variable data type.
```java
val cats = 5 //int
val byteCats: Byte = 4 //Byte
```
* **Boolean**

### Nullability

* Kotlin provides some functionality to make sure that, Null Pointer Exceptions can be avoided. NPE occurs when we try to perform some kind of operation on variable that has null values.


