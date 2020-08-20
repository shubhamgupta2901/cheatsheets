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
