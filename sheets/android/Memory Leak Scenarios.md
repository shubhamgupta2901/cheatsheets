### Contents
* [Memory Leaks](#memory-leaks)
* [Why memory leaks are bad](#why-memory-leaks-are-bad)
* [Memory Leak Common Scenarios](#memory-leak-common-scenarios)
* [Leak Activity to Static Reference](#leak-activity-to-static-reference)
     * [Leak Activity to a static view](#leak-activity-to-a-static-view)
     * [Leak Activity to a static variable](#leak-activity-to-a-static-variable)
     * [Leak Activity to a singleton object](#leak-activity-to-a-singleton-object)
     * [Leak Activity to a static instance of a inner class of the activity](#leak-activity-to-a-static-instance-of-a-inner-class-of-the-activity)
* [Leak Activity to Long Running Operations](#leak-activity-to-long-running-operations)
     * [Leak Activity to a thread](#) 
     * [Leak Activity to a handler](#)
     * [Leak Activity to an AsyncTask](#)
     * [Leak Activity to Listeners](#)
* [Leak Worker Threads](#leak-worker-threads)

    

-------------------    

#### Memory Leaks 

* Since Java provides garbage collection, we only care about creating object. We don't care about cleaning up the objects from memory heap because that is done by garbage collector, but it can only collect objects which has no live strong reference or it's not reachable from any thread. **If an object, which should be collected but still lives in memory due to unintentional strong reference then it's known as memory leak in Java**. These cases generally lead to situation where  there is no memory space for creating new object in Heap, even after garbage collection.

### Why memory leaks are bad

* Less usable memory would be available when memory leak happens. As a result, **Android system will trigger more frequent GC events**. GC events are stop-the-world events. It means when GC happens, the rendering of UI and processing of events will stop. Users may feel slowness in application.

* When your app has memory leaks, it cannot claim memory from unused objects. As a result, it will ask the operating system for more memory. When the operating system can no longer allocate more memory for your app, app user will get an **out-of-memory crash**.

* Memory leak issues are mostly hard to find in QA/testing. They are hard to reproduce.

### Memory Leaks Common Scenarios

* There are some common patterns which lead to majority of memory leaks in android. These are:
   * **Leak Activity to Static Reference** : A static reference lives as long as your app is in memory. An activity has lifecycles which are usually destroyed and re-created multiple times during you app’s lifecycle. If you reference an activity directly or indirectly from a static reference, the activity would not be garbage collected after it is destroyed.
      1. Leak Activity to a static view
      2. Leak Activity to a static variable
      3. Leak Activity to a singleton object
      4. Leak Activity to a static instance of a inner class of the activity 
   * **Leak Activity to Long Running Operations**
      1. Leak Activity to a thread 
      2. Leak Activity to a handler
      3. Leak Activity to an AsyncTask
      4. Leak Activity to Listeners
   * **Leak The Worker Thread Itself**
      
#### Memory Leak Scenario 1

* This scenario is the **typical case of providing activity level context to classes which are independent of this activity's lifecycle**, and may live even after the activity is destroyed.

* Consider the following Singleton class:

```java
public class Singleton {
    private Context context;
    private static Singleton singletonInstance;

    private Singleton(Context context) {
        this.context = context;
    }

    public static Singleton getSingletonInstance(Context context) {
        if(singletonInstance == null)
            return new Singleton(context);
        return singletonInstance;
    }
}
```

* Consider this following code where an Activity requires the ```singletonInstance``` which it does in the ```onCreate()```. 

```java
public class MainActivity extends AppCompatActivity {
    private Singleton singletonInstance;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        singletonInstance = Singleton.getSingletonClassInstance(this);
    }
}
```

* This Activity requires the instance of ```Singleton``` and gets it by providing it its own Context. Note that the context it has passed is Activity level context. It is only available in an activity and is tied to the lifecycle of an activity. 

* When ```onDestroy()``` is called in this activity, due to some confiuguration change like change in screen orientation or simply when this activity needs to be shut down, this singleton class will hold the reference to this activity via its context. So even when this activity is finished the ```Singleton``` object will have the isntance of the context provided through activity, hence activity will not be garbage collected resulting in memory leaks. 

* The solution to such problems are to use Application Level Context.This context is tied to the lifecycle of an application. The application context can be used when you are passing a context beyond the scope of an activity.

* Example: If you have to create a singleton object for your application and that object needs a context, always pass the application context. Because if you pass the activity context here, it will lead to the memory leak as it will keep the reference to the activity and activity will not be garbage collected.

* Similarly when we have to initialize a library in an activity, always pass the application context, not the activity context.So that the context the library has is independent of the activity. Otherwise even when the activity is finished the library object will have the instance of context provided through activity, hence activity will not be garbage collected and memory leaks may occur.

* The activity context should be used when you are passing the context in the scope of an activity or you need the context whose lifecycle is attached to the current context.

#### Memory Leak Scenario 2




