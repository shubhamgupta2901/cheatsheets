## Background Processing in Android
  * [Threads And Handlers](#threads-and-handlers)

* Every Android app has a main thread (also called UI Thread) which is in charge of handling UI, if there is too much work happening on this thread, the app appears to hang or slow down, leading to an undesirable user experience. This leads to Application not Responding Error(ANR).

* Any long-running operations such as decoding a bitmap, accessing the disk, or performing network requests should be done on a separate background thread. In general, anything that takes more than a few milliseconds should be delegated to a background thread.

* This will improve the app performance by providing better user experience and save us from ANR. Also there are some operations like HTTP calls which are not allowed to be performed in the main thread.

### Threads And Runnables

* Android supports the usage of the ```Thread``` class to perform asynchronous processing. Android also supplies the ```java.util.concurrent``` package to perform something in the background. 

* 

* The problem with threads is if you need to update the user interface from a new Thread, you need to synchronize with the main thread i.e you will have to wait in the main thread for this new thread to complete its execution and join to main thread before we can update the user interface. 

* This issue can be solved using **Handlers**. A ```Handler``` object registers itself with the thread in which it is created. It provides a channel to send data to this thread, for example the main thread. 

* The data which can be posted via the ```Handler``` class can be an instance of the ```Message``` or the ```Runnable``` class. A ```Handler``` is particularly useful if you have to post data multiple times to the main thread.

* Android is a single threaded UI framework. There is one UI thread responsible for drawing on the canvas. **If you touch UI in any other thread, you meet ```CalledFromWrongThreadException```** ; Therefore some mechanism is needed to perform UI work in UI thread. Most of the developers have used Handler to accomplish that.
