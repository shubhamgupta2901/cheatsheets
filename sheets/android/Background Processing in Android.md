## Contents
 * [Why Background Processing](#why-background-processing)
 * [Threads And Runnable](#threads-and-runnable)
 * [Problems with Threads in Android](#problems-with-threads-in-android)
 * [Handlers](#handlers)
 
 

 ------------------------------------------------------------------
 ### Why Background Processing

* Every Android app has a main thread (also called UI Thread) which is in charge of handling UI, if there is too much work happening on this thread, the app appears to hang or slow down, leading to an undesirable user experience. This leads to **Application Not Responding** Error(ANR).

* Any long-running operations such as decoding a bitmap, accessing the disk, or performing network requests should be done on a separate background thread. In general, anything that takes more than a few milliseconds should be delegated to a background thread.

* This will improve the app performance by providing better user experience and save us from ANR. Also there are some operations like HTTP calls which are not allowed to be performed in the main thread.

### Threads And Runnables

* Android supports the usage of the ```Thread``` class to perform asynchronous processing. Android also supplies the ```java.util.concurrent``` package to perform something in the background. 

* This issue can be solved using **Handlers**. A ```Handler``` object registers itself with the thread in which it is created. It provides a channel to send data to this thread, for example the main thread. 

* The data which can be posted via the ```Handler``` class can be an instance of the ```Message``` or the ```Runnable``` class. A ```Handler``` is particularly useful if you have to post data multiple times to the main thread.

 ### Problems with Threads in Android
 
 * Android is a single threaded UI framework. There is one UI thread responsible for drawing on the canvas. **If we touch UI in any other thread, we will meet ```CalledFromWrongThreadException```**. So no other thread can directly handle UI. Therefore some mechanism is needed to perform UI work in UI thread. 
 
 * **If we need to update the UI from another UI Thread, we need to synchronize with the UI thread**. Because of this restrictions and complexity we need additional constructed classes in Android to handle concurrency in comparison with standard Java.
 
 * In addition, Java threads are one-time use only and die after executing its run method.

 
 ### Handlers 
 
 * Handler is part of Android's framework which can be used to manage threads. **Handlers allow communicating back with UI thread from another background thread**.
 
 * Every class has its own 
 
 
*  Looper is a worker that keeps a thread alive, loops through MessageQueue and sends messages to the corresponding handler to process.
 
 
