## Contents
 * [Why Background Processing](#why-background-processing)
 * [Problems with Threads in Android](#problems-with-threads-in-android)
 * [Communicate with the UI thread](#communicate-with-the-ui-thread)
 * [Loopers](#loopers)
 * [Handlers](#handlers)
 * [HandlerThreads](#handlerthreads)
 


 ------------------------------------------------------------------
 ### Why Background Processing

* Threads are the smallest sequence of instructions that can be managed independently by operating system. They are units of computation that run simultaneously within a process.They usually reside in a process and share resources such as memory (but have independent call stacks). Threads running in the same process can communicate with each other via shared objects or message passing. 

* Android also supports the usage of the ```Thread``` class to perform asynchronous processing. 

* In Android, when an application is launched, the system creates a thread of execution called **main**. This main thread (also called the UI thread) is responsible for drawing the user interface.  It is the thread responsible for what the user gets the see on the screen and interacts with.

* **All the applcation components (Activity, Service, BroadcastReceiver, Content Providers) instantiated in the same process/application created in the main thread only.** So for example:
  * Start of an Activity
  * Execution of a Service
  * Receival of BroadcastReceiver (onReceive())
  * Querying a content provider
  * Responding to system callbacks for user events like onTouchListener, onKeyDown

* All these and more,are processed on the main thread. *This is why long running operations (like network access, database queries, complicated calculations, accessing big files or any task that could possibly take more than 5 seconds) should not be executed on the main thread which will cause it to delay in drawing the UI leading to an unresponsive laggy application.* It will show the **Application Not Responding (ANR)** dialog.This could lead to users uninstalling your app and submitting low ratings and negative feedbacks on the play store.

* Hence, all sorts of intensive operations should be done in Background/Worker threads. 

 ### Problems with Threads in Android
 
 * Android is a single threaded UI framework. There is one UI thread responsible for drawing on the canvas. **If we touch UI in any other thread, we will meet ```CalledFromWrongThreadException```**. So no other thread can directly handle UI. Therefore some mechanism is needed to perform UI work in UI thread. 
 
 * **If we need to update the UI from another UI Thread, we need to synchronize with the UI thread**. We will need to joing the thread to UI Thread using ```Thread.join()``` to know that the task assigned was completed or not. It gets even trickier if we want to do some work in background thread partially, and update the results in the UI Thread. Because of these restrictions and complexity we need additional constructed classes in Android to handle concurrency in comparison with standard Java.
 
 * In addition, Java threads are one-time use only and die after executing its run method.

 ### Communicate with the UI thread
 
 * **Handler** is part of Android's framework which can be used to manage threads. **Handlers allow communicating back with UI thread from another background thread**.
 
 * Basically, Hanlders along with Loopers implement a common concurrency pattern known as **Pipeline Thread Pattern**. Here's how it works:
  1. The Pipeline Thread holds a queue of tasks which are just some units of work that can be executed or processed.
  2. Other threads can safely push new tasks into the Pipeline Thread’s queue at any time.
  3. The Pipeline Thread processes the queued tasks one after another. If there are no tasks queued, it blocks until a task appears in the queue.
  
 ** **Looper turns our UI thread into a Pipeline Thread and Handler gives us a mechanism to push tasks into the UI thread from any other threads.**
 
 * Here is how Android implements the Pipeline Thread Pattern to solve the communication issue between threads:
  1. The UI Thread maintains a MessageQueue which contains a list of tasks(```Messages``` and ```Runnables``` : they are set of executable code) to be performed on this thread.
  2. The UI Thread also has a Handler. Any thread which has a reference of this handler can use it to add tasks to the MessageQueue of UI Thread (This can be done from any thread, but they need to be part of same process).
  3. The Looper in UI Thread polls the MessageQueue to look for the next task in the queue. As soon as a message is encountered, it is forwarded to its respective handler to execute it in the UI Thread. 
 
* EXAMPLE: Consider a situation where a background thread is performing a time consuming operation. Everytime some part of the operation is completed it has to update the UI Thread about the progress of task. UI Thread has a progress bar which it would like to update so that the user using the application is aware about the progress of background task.

* We can use Handler to facilitate the communication between UI Thread and the Background Thread.

```java
public class DemoActivity extends AppCompatActivity {

    private ProgressBar progressBar;
    private Button button;
    private Handler handler;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_demo);
        bindViews();
        initializeHandler();
        setOnClickListeners();
    }

    private void bindViews() {
        progressBar = findViewById(R.id.progressBar1);
        progressBar.setMax(10);
        button = findViewById(R.id.button1);
    }

    private void initializeHandler() {
        /**Initialized in the main thread, the handler belongs to this thread.
         Any thread that has reference of this handler can use it to call sendMessage with a message,
         That message will be enqueued in the MessageQueue of this thread. And once the turns comes for this
         method, it will be executed by the handler in this thread by calling handle Message.**/
        handler = new Handler() {
            @Override
            public void handleMessage(Message msg) {
                progressBar.setProgress(msg.arg1);
            }
        };

    }

    private void setOnClickListeners() {
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                new BackgroundThread(handler).start();
            }
        });
    }

}
```

```java
public class BackgroundThread extends Thread {
    private  Handler handler;

    BackgroundThread(Handler handler){
        //receives the reference of handler from the main thread.
        this.handler = handler;
    }
    @Override
    public void run() {
        super.run();
        for (int i = 0; i<10; i++){

            SystemClock.sleep(500);
            /**Creates a new message every time and sends it to the UI Thread. It will be executed in
            the UI thread, when it will be dequeued from the Message Queue from the Looper.
            Note a new message instance is created every time, and not reused to avoid runtime error.**/
            Message message = Message.obtain();
            message.arg1 = i+1;
            handler.sendMessage(message);
        }
    }
}
```

* This is how we communicate with UI Thread:
 1. UI Thread starts a background thread to perform a background operation.
 2. Background Thread starts the background operation, it wants to update the UI Thread regarding the progress of the task.
 3. It gets the UI Thread's Handler reference and sends messages periodically using ```sendMessage()```.This message will go inside the MessageQueue of the main thread.
 4. Looper in the main thread keeps polling the message queue to find if theres a task to be performed. 
 5. When this task added by background thread is dequeued, the Looper will open the message and forward this message to its handler.
 6. Handler object has method handleMessage() in which whatever code we have written will be executed in the UI Thread.
 7. So a handler will take a message from another thread, enqueue it in the main thread's Message Queue, and when time comes, Looper will dispatch this task to handler asking it to execute it in the main thread.
 
 MODIFICATION IN ABOVE CODE:
 
 * In above code the handler is not created as a static class, If it is not static, it will have a reference to our Activity object. Handler objects for the same thread all share a common Looper object, which they post messages to and read from. As messages contain target Handler, as long as there are messages with target handler in the message queue, the handler cannot be garbage collected. If handler is not static, our Activity cannot be garbage collected, even after being destroyed.This may lead to memory leaks, for some time at least - as long as the messages stay in the queue. 
 
 ```java
 public class DemoActivity extends AppCompatActivity {

    private ProgressBar progressBar;
    private Button button;
    private ProgressHandler handler;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_demo);
        bindViews();
        handler = new ProgressHandler(progressBar);
        setOnClickListeners();
    }

    private void bindViews() {
        progressBar = findViewById(R.id.progressBar1);
        progressBar.setMax(10);
        button = findViewById(R.id.button1);
    }

    private void setOnClickListeners() {
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                new BackgroundThread(handler).start();
            }
        });
    }

    static class ProgressHandler extends Handler{
        ProgressBar progressBar;
        public ProgressHandler(ProgressBar progressBar) {
            this.progressBar = progressBar;
        }

        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            progressBar.setProgress(msg.arg1);
        }
    }
    
}
 ```

 ### Loopers
 
  *  **Looper** is a class that keeps a thread alive, loops through the **MessageQueue** of its thread, and sends messages to the corresponding handler to process.
  
  * The Looper is named so because it implements the loop – takes a task from the queue, executes it (or delegates it to someone to execute it), and then take next task and so on. ```Looper.loop()``` has an infinite for loop being run checking for the next message. **It is a blocking method ensuring that the ```run()``` method blocks and is not finished***.So basically a Looper uses a synchronized MessageQueue that’s used to process Messages placed on the queue. It implements a Thread-specific event loop that waits for and dispatches Messages to handlers. 
  
* By default, a thread does not have a message loop associated with it, hence doesn’t have a Looper either. To create a Looper for a thread and dedicate that thread to process messages serially from a message loop, you can use the Looper class.

```java
class LooperThread extends Thread {
    public Handler mHandler;
 
    @Override
    public void run() {
        // Initialize the current thread as a Looper
        // (this thread can have a MessageQueue now)
        Looper.prepare();
 
        mHandler = new Handler() {
            @Override
            public void handleMessage(Message msg) {
                // process incoming messages here
            }
        };
 
        // Run the message queue in this thread
        Looper.loop();
    }
}
```

Starting a thread still remains the same:

```java
LooperThread looperThread = new LooperThread();
looperThread.start();
```
* Since we have created a handler inside the thread, any thread which has its reference can  can send Messages or post Runnables.These will go inside the message queue of this thread, and looper will dequeue them and dispatch them to handlers. 

* **A thread can have only one associated Looper and hence a single message queue.** So if multiple thread tries to send messages to a particular Looper thread then all of them will be processed sequentially. Trying to setting up another Looper will throw a runtime error with a message like this java.lang.RuntimeException: Only one Looper may be created per thread.

* There are 2 methods to terminate a Looper:
 1. ```Looper.quit()``` – Terminates the Looper without processing any more messages in the MessageQueue. Any attempt to post messages to the queue will fail once the Looper is asked to quit. For example, the ```Handler.sendMessage()``` or ```Handler.post()``` dispatching methods will return false.
 2. ```Looper.quitSafely()``` – Similar to the previous version but makes sure that the pending messages that are due to be delivered are handled. Messages with due times in the future won’t be delivered before the Looper quits though.
 
 * Terminating the Looper lets the thread resume running the method which had invoked the call to ```Looper.loop()```. The old Looper or even a new Looper cannot be started anymore. So the thread can no longer enqueue and handle messages. ```Looper.prepare()``` called again should throw a ```RuntimeException``` whereas ```Looper.loop()``` called again will block but no messages will be dispatched from the queue.
 
 * **The UI thread is the only thread that is associated with a Looper by default before the application components are initialized**. There are a few differences (or maybe specialities that it has) between it and other application thread Loopers: 
   * It cannot be terminated with ```Looper.quit()```. A ```RuntimeException``` is thrown if that is tried.
   * It is accessible from everywhere through the ```Looper.getMainLooper()``` method.
   * It is associated with the UI thread via ```Looper.prepareMainLooper()``` and can be done only once per application. Trying to call this in some other Thread to attach the main looper will throw an exception.


 ### Handlers
 
* A Handler defines a set of methods using which we can post/send (as well as remove) and process Message (data message) and Runnable (task message) objects in the MessageQueue associated with the Thread-specific Looper. 

* Each Handler instance is actually bound to the thread (and hence the message queue associated with the thread) in which it is created. It delivers Message and Runnable objects to that associated MessageQueue and executes them when they get dispatched by the Looper.

* **A Handler has to be bound to a Looper else they cannot function**. A Looper is strictly required for it to connect to a queue to insert messages and receive messages from the Looper. 

* When creating a Handler object you’ve to pass the Looper object as the first argument to the constructor explicitly. If not then it binds to the Looper of the current thread. If the current thread doesn’t have a Looper then a RuntimeException will be thrown.

* Multiple Handlers doesn’t ensure concurrent execution as the Messages/Runnables will be posted in the queue from which they’re processed sequentially.

* As seen above Handlers are quite handy when we need communication between two threads. Handler does it by passing the message of a thread to another thread's message queue.We can also enqueue a Runnable to the MessageQueue using Handlers:

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("My Runnable");
    }
};
 
Handler handler = new Handler();
handler.post(r);
```

* The Handler posts the Runnable in the MessageQueue and once the Looper gets to it, the Runnable is executed.Here we have posted a runnable from same thread. We can also post runnables from other threads (which have reference to this handler) to this thread. The runnable will be delivered from that thread to this thread, and will enqueue in the MessageQueue. 

* There’s  also ```postDelayed()``` and ```sendMessageDelayed()``` using which you can schedule the processing of Runnable and Message objects to a later time.

* So handlers have two primary jobs:
  1. To enqueue an action to be performed on a different thread than the one we are using.
  2. To schedule messages and runnables to be executed at some point in future.(In same thread or another, depending where the handler was instantiated)

* A very important thing to remember is that **only Message objects are allowed in the MessageQueue**. Hence Runnables sent through the post* methods and what integers sent through sendEmptyMessage* are wrapped into Message objects.

```java
public class DemoActivity extends AppCompatActivity {

    private ProgressBar progressBar;
    private Button button;
    private ProgressHandler handler;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_demo);
        bindViews();
        handler = new ProgressHandler(progressBar);
        setOnClickListeners();
    }

    private void bindViews() {
        progressBar = findViewById(R.id.progressBar1);
        progressBar.setMax(10);
        button = findViewById(R.id.button1);
    }

    private void setOnClickListeners() {
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                new BackgroundThread(handler,progressBar).start();
            }
        });
    }

    static class ProgressHandler extends Handler{
        ProgressBar progressBar;
        public ProgressHandler(ProgressBar progressBar) {
            this.progressBar = progressBar;
        }

        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            progressBar.setProgress(msg.arg1);
        }
    }

}
```

```java
public class BackgroundThread extends Thread {
    private  Handler handler;
    private  ProgressBar progressBar;

    BackgroundThread(Handler handler, ProgressBar progressBar){
        //receives the reference of handler from the main thread.
        this.handler = handler;
        this.progressBar = progressBar;
    }
    @Override
    public void run() {
        super.run();
        for (int i = 0; i<10; i++){
            SystemClock.sleep(500);
            final int finalI = i+1;
            handler.post(new Runnable() {
                @Override
                public void run() {
                   progressBar.setProgress(finalI );
                }
            });
        }
    }
}
```

 #### HandlerThreads
 
 * **HandlerThread is a class for starting a new thread that has a Looper (hence an associated MessageQueue) attached** so that one doesn’t have to go through creating a Thread and calling ```Looper.prepare()```, ```Looper.loop()```, etc. in that. 

* Once a HandlerThread is started, it sets up queuing through a Looper and MessageQueue and waits for incoming messages to process:

```java
HandlerThread handlerThread = new HandlerThread("HandlerThread");
handlerThread.start();
 
// Create a handler attached to the HandlerThread's Looper
mHandler = new Handler(handlerThread.getLooper()) {
    @Override
    public void handleMessage(Message msg) {
        // Process messages here
    }
};
 
// We can now send messages from other threads, which have reference to this handler, using mHandler.sendMessage(), it will be enqueued in handlerThread's MessageQueue
```

* There's only one associated MessageQueue on the thread, hence execution is guaranteed to be sequential and therefore thread-safe. Behind the scenes, **HandlerThread guarantees no race condition between the Looper creation and sending messages by making ```handlerThread.getLooper()``` a blocking call until the it is ready to receive messages**. This is an important reason why HandlerThread should be used over manual setup with Looper.prepare(), Looper.loop(), Looper.quit(), etc.

* The ```HandlerThread.onLooperPrepared()``` method can be used to execute some sort of setup before the Looper loops, like creating a Handler that will be associated with the HandlerThread. This method is invoked on the background thread when the Looper is prepared (after the Looper.prepare() call).

```java
private class MyHandlerThread extends HandlerThread {

    Handler handler;

    public MyHandlerThread(String name) {
        super(name);
    }

    @Override
    protected void onLooperPrepared() {
        handler = new Handler(getLooper()) {
            @Override
            public void handleMessage(Message msg) {
                // process incoming messages here
                // this will run in non-ui/background thread
            }
        };
    }
}
```

* You generally need a thread attached with a Looper when you want sequential execution of tasks without race conditions and keep a thread alive even after a particular task is completed so that it can be reused so that you don’t have to create new thread instances.

* Also starting the same thread object again raises IllegalThreadStateException with a Thread already started message.


  




 
