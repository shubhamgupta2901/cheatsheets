* Threads are the smallest sequence of instructions that can be managed independently by operating system. 
They run simultaneously within a process, and share resources such as memory etc.
Threads running in the same process can communicate with each other via shared objects or message passing.

* Android is a single threaded framework/platform.
When an application is launched, the system creates a thread of execution called main thread/ Ui thread to interact with users/ OS. 
This main thread  is responsible for drawing the user interface and how user is interacting with the interface.
This means that our application is assigned a single thread to interact with users and OS, - main thread/ UI thread. 
But we can spawn multiple threads to perform asynchronous and background tasks, which does not interrupt the main thread.

* All the applcation components (Activity, Service, BroadcastReceiver, Content Providers) instantiated in the same process/application created in the main thread only. So for example:
  * Start of an Activity
  * Execution of a Service
  * Receival of BroadcastReceiver (onReceive())
  * Querying a content provider
  * Responding to system callbacks for user events like onTouchListener, onKeyDown



#### 1. What are different android components?

Android app components are the building blocks for an app. Each component represents an entry points through which a user or OS can enter our application. They have their own lifecycles  They are broadly:
1. Activity
2. Services
3. Broadcast receviers
4. Content Providers.

Activity: 
(Runs on main thread)
Activities are one of the core android components. 
They serve as an entry point for user in our application. 
While multiple activities can be used to provide a cohesive user experience, each activity has an independent lifecycle.
This helps in rendering views and components on screen and govern how these views interact with each other and users.



Services:
1. Services are primary mechanism for background work in Android. 
2. They are designed to work without a user interface. 
3. Although Services run without a dedicated UI, they still execute in the *main Thread* of the application's process — just like other app components.
4. Examples: perform long-running operations like playing music in background or fetching data over internet without blocking user interaction.

Types of services: 
  1. Started Services: 
  * A started service is a service that has been started by another component like Acitivity or Broadcast receiver. They run continuously in the background until something explicitly tells the service to stop
  * **Foreground Service** : 
  1. When we perform background tasks,which the user is directly aware of we use Foreground services.
  2. Like playing music on the background and allowing controls to user via notification.
  3. In such scenarios we tell the system that we want to foreground with a notification.
  4. So the system understands it needs to keep the process on which this service is working alive at all costs, otherwise user will directly get affected.
  
  * ** Background Services**: 
  1. A regular background service is not something user is directly aware is running.
  2. Like syncing some data with backend in background.
  3. For such services system has greater liberty in managing its process.
  4. It may be allowed to kill and restart at some later point.
  
  
  2. Bound Services:  
  * A bound service behaves like  server in a client-server interface. 
  * It allows other app components like activities to bind to the service, send requests and receive response.
  * It lives only while it serves another application component and closes when there is no other component bound to it.
  * The component which starts this service can be from another process as well.
  * In such cases, the system knows there is a  dependency between two processes, so if process A is bound to a service in process B, it knows that it needs to keep process B (and its service) running for A. 
  
  3. Intent Services
    * Deprecated, should use JobIntent Services or WorkManager API.
    * IntentService is used to perform asynchronous requests using intents.
    * We can send multiple intents to the service to perform tasks.
    * Intent service uses a queue to process each intent one at a time.
    * It spawns a single worker thread that runs on background, to perform the task.
    * Once all the tasks are completed,it stops itself.
    
4. Hybrid Service
    * A hybrid service is a service that has the characteristics of a started service and a bound service.
    * It can be started by when a component binds to it or it may be started by some event. 
    * A client component may or may not be bound to the hybrid service. 
    * A hybrid service will keep running until it is explicitly told to stop, or until there are no more clients bound to it.
    * For example: A music player might find it useful to allow its service to run indefinitely and also provide binding. This way, an activity can start the service to play some music and the music continues to play even if the user leaves the application. Then, when the user returns to the application, the activity can bind to the service to regain control of playback.


**Broadcast Receivers**
1. Broadcast receivers are components that allows the app to register for system level and application level events.
2. Events can either be system level or can be created by us. like: 
```java
      Intent intent = new Intent();
      intent.setAction("com.journaldev.CUSTOM_INTENT");
      sendBroadcast(intent);
```
3. If the event for which the broadcast receiver has registered happens, the onReceive() method of the receiver is called by the Android system.
4. For example, we can register a broadcast receiver for ACTION_BOOT_COMPLETED, SMS_RECEIVED system event, so whenever the device is restarted, the system will broadcast the event, and we can recieve it in our app, and take action accordingly.
3. We can register a reciever via the AndroidManifest file or programmatically using Context.registerReceiver() method.
4.After the onReceive() of the receiver class has finished, the Android system is allowed to recycle the receiver.
5. Before API level 11, you could not perform any asynchronous operation in the onReceive() method, because once the onReceive() method had been finished, the Android system was allowed to recycle that component. If you have potentially long running operations, you should trigger a service instead. Since Android API 11 you can call the ```goAsync()``` method. 
6. **```goAsync()```**: We can use ```goAsync()``` to handoff the processing inside of your BroadcastReceiver's ```onReceive()``` method to another thread. The onReceive() method can then be finished there. The PendingResult is passed to the new thread and you have to call PendingResult.finish() to actually inform the system that this receiver can be recycled.
  
  ```
  final PendingResult result = goAsync();
Thread thread = new Thread() {
   public void run() {
      int i;
      // Do processing
      result.setResultCode(i);
      result.finish();
   }
};
thread.start();
  ```
* This method returns an object of the PendingResult type. The Android system considers the receiver as alive until you call the PendingResult.finish() on this object. As soon as that thread has completed, its task calls finish() to indicate to the Android system that this component can be recycled.


Creating a broadcast receiver:
```
public class MyReceiver extends BroadcastReceiver {
    public MyReceiver() {
    }
 
    @Override
    public void onReceive(Context context, Intent intent) {
      
        Toast.makeText(context, "Action: " + intent.getAction(), Toast.LENGTH_SHORT).show();
    }
}
```

Registering the BroadcastReceiver in AndroidManifest.xml: 
```
<receiver android:name=".ConnectionReceiver" >
             <intent-filter>
                 <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
             </intent-filter>
</receiver>
```

Registering Broadcast receiver programmatically:
```
IntentFilter filter = new IntentFilter();
intentFilter.addAction(getPackageName() + "android.net.conn.CONNECTIVITY_CHANGE");
 
MyReceiver myReceiver = new MyReceiver();
registerReceiver(myReceiver, filter);
```

Broadcasting custom event from app:
```
      Intent intent = new Intent();
      intent.setAction("com.journaldev.CUSTOM_INTENT");
      sendBroadcast(intent);
```

* Difference between registering broadcast receiver in Android Manifest vs registering them programmatically: When we declare Broadcast receivers programmatically, the broadcast are bound with the application. So when the application gets destroyed, the onRecive() method would not be called. Whereas if we register them in AndroidManifest, we onRecieve is called even when the application is not alive.

* Explicit and Implicit Broadcast receivers: 

Explicit Intents are used to call a particular component that you know of.
Implicit Intents are used when you don’t know the exact component to invoke.

Similarly, 

Explicit Broadcast Receivers are exclusive to your application. Only your application’s broadcast receiver will get triggered when the custom intent action you define, gets called.
Implicit Broadcast Receivers aren’t exclusive to your application. Actions such as ACTION_BOOT_COMPLETED or CONNECTIVITY_CHANGE are categorised in implicit broadcast receivers. This is because when these events happen, all the applications registered with the event will get the information.

* Handling Android Oreo Broadcast Receivers: 

 * Since Android Oreo, implicit broadcast receivers won’t work when registered in the AndroidManifest.xml. To use Implicit Receivers in your application, you need to define them programmatically in your code, using registerReceiver().
 * Using registerReceiver() we can programmatically register and unregisterReceiver() during the lifecycle of the activity. This way implicit receivers would only be called when our activity/application is alive and not at other times. Several Implicit Broadcasts are exempted and can be declared in the Manifest.

