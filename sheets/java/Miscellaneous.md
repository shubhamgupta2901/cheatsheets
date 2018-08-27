## Contents
* [Strong and Weak Reference](#strong-and-weak-reference)

### Strong and Weak Reference
* Objects in Java are created on the heap. When garbage collection occurs, all the unreferenced objects are removed, while all the referenced objects still live in the heap.
* But in JAVA not all the referenced objects are equally strong. Some of them can be knocked out of the heap without their knowledge even when they are not candidates for garbage collection. These objects are perfectly good objects which are being referenced and used, yet gc takes the liberty to remove them from heap. These kind of objects are called **weak references**.
* Consider the following code:
```java Widget w = new Widget(); ```

Here a ```Widget``` object is created in heap and ```w``` has its reference.As long as the variable ```w``` is active, the object which it points to will not be garbage collected.
* Such a kind reference is called **Strong reference**. The object referenced by such variables can not be garbage collected till they are active.
* However, Java also has an alternate type of reference - a weak reference. Objects referenced by them may be garbage collected -- if the JVM runs short of memory -- even when they are being used. Creating a weak reference is a way of telling the JVM: *I am creating this object as a weak reference. Even though I need it, feel free to garbage collect it if you run out of memory. I know this object can be GC'd any time and am prepared to deal with it.*

* Suppose that the garbage collector determines at a certain point in time that an object is weakly reachable. At that time it will atomically clear all weak references to that object and all weak references to any other weakly-reachable objects from which that object is reachable through a chain of strong and soft references. At the same time it will declare all of the formerly weakly-reachable objects to be finalizable. At the same time or at some later time it will enqueue those newly-cleared weak references that are registered with reference queues.

* For making an object weak reference, We use Java's ```WeakReference``` class.The object which needs to be weakly referenced is wrapped inside a ```WeakReference``` object.

* Follwing code creates a weak reference to the String "abc:

```java
WeakReference<String> wr = new WeakReference<String>(new String("abc"));
```
* We can get the String back by using the ```get``` method as shown below. We can never assume that the object we are trying to get actually exists.

* * Here's how you would create a simple cache of Widget objects using weak references:
```java
String s = wr.get();
if(s != null ) {
    // great the weak ref has not been garbage collected
} else {
    // oops the weak ref was garbage collected... now I will have to create another one
}
```
* The most common use for weak references are read-only caches, where losing an object is not disastrous. It just means we have to recreate the object, usually back from the database.

```java
public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("How many objects do you want to create?");
        int count = sc.nextInt();

        Map<Integer, WeakReference<Widget>> weakWidgetsMap = new HashMap<Integer, WeakReference<Widget>>();
        System.out.println("Creating "+count+" widgets as weak reference");

        for (int i = 0; i<count ; i++){
            weakWidgetsMap.put(i, new WeakReference<Widget>(new Widget(i)));
        }

        for (int i = 0; i< count; i++){
            WeakReference<Widget> weakReferenceWidget = weakWidgetsMap.get(i);
            Widget widget = weakReferenceWidget.get();
            if (widget == null)
                System.out.println("Weak Widget"+ i +" has been garbage collected.");
            else System.out.println("Weak widget"+ i + "is still around. Safe to use.");
        }
    }


}

class Widget{
    private byte[] buff;
    private  int id;

    public Widget(int id) {
        this.id = id;
        this.buff = new byte[1024*1000];
    }

}
```


