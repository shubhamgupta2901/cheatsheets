## Contents
* [Android Lifecycles](#activity-lifecycle)
   * [Activity Lifecycle](#activity-lifecycle)
   * [Lifecycle Scenarios](#lifecycle-scenarios)
      * [Single Activity](#single-activity)
      * [Multiple Activities](#multiple-activities)
      * [Fragments](#fragments)
      * [ViewModels, Translucent Activities and Launch Modes](#view-models-translucent-activities-launch-modes)
* [Tasks and BackStack](#tasks-and-backstack)
* [Launch Modes](#launch-modes)
    * [Standard](#standard)
    * [SingleTop](#singletop)
    * [SingleTask](#singletask)
    * [SingleInstance](#singleinstance)
    

## [Android Lifecycles](#android-lifecycles)

* When users interact with an app they might rotate the screen, respond to a notification, or switch to another task.
* The idea is they should be able to continue using the app seamlessly after such an event.
* During such events the components go through **transition in their states**.
* Components have their own lifecycle, and when such transition happens, the system notifies the components via the callback methods.

### Activity Lifecycle

* The Activity class provides a core set of six callbacks: ```onCreate(), onStart(), onResume(), onPause(), onStop(), and onDestroy()```. The system invokes each of these callbacks as an activity enters a new state.

![Activity Lifecycle](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_1.png)

##### ```onCreate()```
* This callback is fired when the system first creates the activity and the activity enters the *Created* state. 

* In the ```onCreate()``` method, we perform basic application startup logic that should happen only once for the entire life of the activity. For example, your implementation of ```onCreate()``` might bind the layout xml file to the activity, bind data to lists and instantiate some class-scope variables. 

* This method receives the parameter ```savedInstanceState```, which is a ```Bundle``` object containing the activity's previously saved state. 
If the activity has never existed before, the value of the ```Bundle``` object is null. 
But if the 1. system had closed the process on which this acitivity resided, or 2. when system destroys an activity to recreate it due to configuration change (same bundle which is passed to ```onRestoreInstanceState()```, is passed to ```onCreate()``` as well ), it returns the previous state via this bundle. 

##### ```onStart()```

* System invokes this callback, when the activity starts becoming visible to the user. Now the  app prepares for the activity to enter the foreground and become interactive. This method is where the app initializes the code that maintains the UI.

* The ```onStart()``` method completes very quickly and the activity does not stay resident in this state. Once this callback finishes, the activity enters the *Resumed* state, and the system invokes the ```onResume()``` method. 

##### ```onResume()```

* When the activity comes to the foreground,and becomes completely interactive with user, the system invokes the ```onResume()``` callback. 

* The app stays in this *RUNNING* state until something happens to take focus away from the app. Such an event might be, for instance, receiving a phone call, the user’s navigating to another activity, or the device screen’s turning off.

* When an interruptive event occurs, the system invokes the ```onPause()``` callback. If the activity comes to foreground again, the system once again calls ```onResume()``` method. For this reason, we should use ```onResume()``` to  perform initializations that must occur each time the activity enters the Resumed state, or initialize components that were released during ```onPause()```.

##### ```onPause()```
* The system calls this method when the activity is no longer in foreground, or has lost focus. This is the first indication that the user is leaving our activity (though it does not always mean the activity is being destroyed).

* This does not mean that the activity is no longer visible, it may still be visible, it just has lost focus.

*  There are several reasons why an activity may enter this state. For example:
  * In Android 7.0 (API level 24) or higher, multiple apps run in multi-window mode. Because only one of the apps (windows) has focus at any time, the system pauses all of the other apps.
  * A new, semi-transparent activity (such as a dialog) opens. As long as the activity is still partially visible but not in focus, it remains paused.

* You can also use the ```onPause()``` method to release system resources, handles to sensors (like GPS), or any resources that may affect battery life while your activity is paused and the user does not need them. However, as mentioned above in the ```onResume()``` section, a *Paused* activity may still be fully visible if in multi-window mode. As such, you should consider using ```onStop()``` instead of ```onPause()``` to fully release or adjust UI-related resources and operations to better support multi-window mode.

* ```onPause()``` execution is very brief, and does not necessarily afford enough time to perform save operations. For this reason, you should not use ```onPause()``` to save application or user data, make network calls, or execute database transactions; such work may not complete before the method completes. Instead, you should perform heavy-load shutdown operations during ```onStop()```.

* After completion of the ```onPause()``` method the activity resumes(onResume) or becomes completely invisible(onStop) to the user. If the activity resumes, the system once again invokes the ```onResume()``` callback. If the activity resumes again, the system keeps the Activity instance resident in memory, recalling that instance when the system invokes ```onResume()```. In this scenario, you don’t need to re-initialize components that were created during any of the callback methods leading up to the *Resumed* state. If the activity becomes completely invisible, the system calls ```onStop()```.

##### ```onStop()```

* When your activity is no longer visible to the user, it has entered the Stopped state, and the system invokes the ```onStop()``` callback. This may occur, for example, when a newly launched activity covers the entire screen. 

* In the ```onStop()``` method, the app should release or adjust resources that are not needed while the app is not visible to the user. For example, your app might pause animations or switch from fine-grained to coarse-grained location updates. Using ```onStop()``` instead of ```onPause()``` ensures that UI-related work continues, even when the user is viewing your activity in multi-window mode.

* You should also use ```onStop()``` to perform relatively CPU-intensive shutdown operations. For example, if you can't find a more opportune time to save information to a database, you might do so during ```onStop()```. 

* When your activity enters the *Stopped* state, the Activity object is kept resident in memory: It maintains all state and member information, but is not attached to the window manager. When the activity resumes, the activity recalls this information. You don’t need to re-initialize components that were created during any of the callback methods leading up to the *Resumed* state. 

* The system also keeps track of the current state for each View object in the layout, so if the user entered text into an ```EditText``` widget, that content is retained so you don't need to save and restore it.

* Note: Once your activity is stopped, the system might destroy the process that contains the activity if the system needs to recovery memory. Even if the system destroys the process while the activity is stopped, the system still retains the state of the View objects (such as text in an EditText widget) in a Bundle (a blob of key-value pairs) and restores them if the user navigates back to the activity.

* From the *Stopped* state, the activity either comes back to interact with the user, or the activity is finished running and goes away. If the activity comes back, the system invokes ```onRestart()```. If the Activity is finished running, the system calls ```onDestroy()```. 

##### ```onDestroy()```

* onDestroy() is called before the activity is destroyed. The system invokes this callback either because:
    1.the activity is finishing (due to the user completely dismissing the activity or due to finish() being called on the activity), or
    2.the system is temporarily destroying the activity due to a configuration change (such as device rotation or multi-window mode)

* If the activity is finishing, ```onDestroy()``` is the final lifecycle callback the activity receives. If ```onDestroy()``` is called as the result of a configuration change, the system immediately creates a new activity instance and then calls ```onCreate()``` on that new instance in the new configuration.

* The ```onDestroy()``` callback should release all resources that have not yet been released by earlier callbacks such as ```onStop()```.

### [Lifecycle Scenarios](#lifecycle-scenarios)

#### [Single Activity](#single-activity)

#### Single-activity application: application started, finished and restarted by the user:

* Triggered by:
   * The user presses the Back button, or
   * The Activity.finish() method is called
   
  ![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_2.png)

* Managing State:
   * onSaveInstanceState is not called (since the activity is finished, you don’t need to save state)
   * onCreate doesn’t have a Bundle when the app is reopened, because the activity was finished and the state doesn’t need to be restored.
   
#### Single-activity application: User navigates away from application
* Triggered by:
   * The user presses the Home button
   * The user switches to another app (via Overview menu, from a notification, accepting a call, etc.)

In this scenario the system will stop the activity, but won’t immediately finish it.

![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_3.png)
* Managing State:
   * onSaveInstanceState is called, system uses onSaveInstanceState to save the app state in case the system kills the app’s process later on (see below).
   * If the process isn’t killed, the activity instance is kept resident in memory, retaining all state. When the activity comes back to the foreground, the activity recalls this information. You don’t need to re-initialize components that were created earlier.

#### Single-activity application: Configuration changes
* Triggered by:
  * Configuration changes, like a rotation, language change
  * User resizes the window in multi-window mode

![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_4.png)

* Managing State:
  * The activity is completely destroyed, but the state is saved and restored for the new instance.
  * The Bundle in onCreate and onRestoreInstanceState is the same.
  
#### Single-activity application: App is paused by the system
* Triggered by:
  * Enabling Multi-window mode (API 24+) and losing the focus
  * Another app partially covers the running app (a purchase dialog, a runtime permission dialog, a third-party login dialog…)
  * An intent chooser appears, such as a share dialog

![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_5.png)

* This scenario doesn’t apply to:
  * Dialogs in the same app. Showing an AlertDialog or a DialogFragment won’t pause the underlying activity.
  * Notifications. User receiving a new notification or pulling down the notification bar won’t pause the underlying activity.
  
#### [Multiple Activities](#multiple-activities)
Note: Grouped events that appear side by side run in parallel, so the order of calls among parallel groups of events is not guaranteed. However, order inside a group is guaranteed.

#### Navigating between activities

* Scenario: Activity1 starts, it starts Activity2, then we press back button on Activity2.
* When a new activity is started, Activity2 is STOPPED (but not destroyed), after that when the Back button is pressed, Activity2 is destroyed and finished.

![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_6.png)

* Managing state
  * **onSaveInstanceState is called, but onRestoreInstanceState is not**. This is because if there is a configuration change when the second activity is active, the first activity will be destroyed and recreated only when it’s back in focus. That’s why saving an instance of the state is important. If the system kills the app process to save resources, this is another scenario in which the state needs to be restored.
  
#### Multiple Activities: Activities in the back stack with configuration changes

* Scenario: Activity1 starts, it starts Activity2. When on Activity2, the device is rotated.

![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_7.png)

* Managing state
  * Saving state is not only important for the activity in the foreground. All activities in the stack need to restore state after a configuration change to recreate their UI. Also, the system can kill your app’s process at almost any time so you should be prepared to restore state in any situation.

#### Multiple Activities: App’s process is killed

* Scenario: Activity1 starts -> it starts Activity2 -> App goes on Background -> Android System kills in background -> App comes to foreground -> Back pressed from Activity 2 

![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/lifecycle_8.png)

* Managing state
  * Note that the state of the full back stack is saved but, in order to efficiently use resources, activities are only restored when they are recreated.

#### [Fragments](#fragments)
#### [ViewModels, Translucent Activities and Launch Modes](#view-models-translucent-activities-launch-modes)

#### [Tasks and BackStack](#tasks-and-backstack)

A task is a collection of activities, and it uses a Back Stack to arrange them(activities) with the order in which they were opened.  
Each task has its own back stack in addition to some other information and/or data.
Each app has its own one or more tasks.

0. So when we tap the launcher icon for our app, the system is looking for a previously existing task (determined by the Intent and Activity it points to) to resume — getting us back to exactly where we were. If no existing task is found, then a new task is created with our newly launched activity as the base activity on the task’s back stack.
1. Further, when we start a new activity using ```startActivity()```, we are pushing a new activity onto our task, causing the previous Activity to be paused and stopped.
2. If we press the back button, By default it will pop the stack, calling finish() on the topmost activity, destroying it and removing it from the back stack 
3. And we are taken back to the previous activity. This repeats until there’s nothing left in the back stack and you’re back at the launcher.


The same principles apply to fragments as well:
1. When we provide a fragment transaction to add, replace, or remove a fragment from your UI, you can use addToBackStack() to effectively add the FragmentTransaction to the back stack.
2. Then when the user hits the back button, instead of activity being popped,the fragment transaction is reversed. Only when there are no more fragment transactions, the activity is finished.

#### Launch Modes

* Launch modes allow us to define **how a new instance of an activity is associated with the current task**. We can define different launch modes in two ways:
    * **using manifest file**: When you declare an activity in your manifest file, you can specify how the activity should associate with tasks when it starts.
    * **using intent flags**: When you call ```startActivity()```, you can include a flag in the Intent that declares how (or whether) the new activity should associate with the current task.
* Some launch modes available for the manifest file are not available as flags for an intent and, likewise, some launch modes available as flags for an intent cannot be defined in the manifest.
* if ```ActivityA``` starts ```ActivityB```, ```ActivityB``` can define in its manifest how it should associate with the current task and ```ActivityA``` can also request how ```ActivityB``` should associate with current task using intent flags. If both activities define how ```ActivityB``` should be associated with a task, then ```ActivityA's``` request (as defined in the intent) is honored over ```ActivityB's``` request (as defined in its manifest).

##### [Standard](#standard) 
* It is the **default** launchMode. It creates a new instance of an activity in the task from which it was started. Multiple instances of the activity can be created and multiple instances can be added to the same or different tasks.

* Scenario 1: 
Current Stack: A->B
New Action: C was launched
Now: A -> B -> C 
What happened: Since the launch mode was standard, new instance of C was created and was placed in the task.

* Scenario 2: 
Current Stack: A->B->C
New Action: we launch B again with the launch mode as "standard"
Now: A -> B -> C -> B.
What happened: Since the launch mode was standard, new instance of B was created and was placed in the task, even though B already exists in stack.

    
##### [SingleTop](#SingleTop)
* It is the same as the standard, except if there is a previous instance of the activity that exists in the top of the stack, then it will not create a new instance but rather send the intent to the existing instance of the activity.

* Scenario 1: 
Current Stack: A->B
New Action: C was launched
Now: A -> B -> C 
What happened: New instance of C was created and was placed in the task.

* Scenario 2: 
Current Stack: A->B->C
New Action: we launch B again with the launch mode as "singleTop"
Now: A -> B -> C -> B.
What happened: Since the launch mode was standard, new instance of B was created and was placed in the task, even though B already exists in stack.

* Scenario 3: 
Current Stack: A->B->C
New Action: we launch C again with the launch mode as "singletop"
Now: A -> B -> C.
What happened: C was launched again with the launch mode as "singleTop", the new stack will still be A -> B -> C. The existing instance of C receives the intent through ```onNewIntent()```, because it's at the top of the stack


##### [SingleTask](#SingleTask)
* The system creates a new task and instantiates the activity at the root of the new task.
* However, if an instance of the activity already exists in a separate task, the system routes the intent to the existing instance through a call to its ```onNewIntent()``` method, rather than creating a new instance.

* Scenario 1: 
Current Stack: A->B
New Action: C is launched with Single Task.
Now: A->B  || C 
What happened: (New taks was created, and C was placed at the root of new task) (|| indicates new task)

* Scenario 2: 
Current Stack: A->B->C->D
New Action: C is launched with Single Task
Now: A->B->C 
What Happened: C already existed in seperate task, the system does not create new instance, it delivers intent through C's onNewIntent, and D is destroyed.

* Scenario 3: 
Current Stack: A->B->C
New Action: C is launched with Single Task
Now: A->B->C 
What Happened: C already existed in seperate task, the system does not create new instance, it delivers intent through C's onNewIntent, No activity on top of C to be destroyed.

* Scenario 3: 
Current Stack: A->B->C(Was launched with single task)->D
New Action: D is launched without launch mode
Now: A->B-> || C->D
What Happened: C had created a new task, D was created and placed in C's new task.


##### [SingleInstance](#SingleInstance)

* Scenario 1:
Current Stack: A->B->C
New Action: D is started with launch Mode: singleInstance
Now: A->B->C || D
What Happened: When E is started with single Instance new task for it is created, and it is placed in the root of new task.


* Scenario 2:
Current Stack: A->B->C || D (D was started with singleInstance)
New Action: E is started from D
Now: D || A->B->C->E
What Happened:  Android launches activity E into the task containing A->B->C (it cannot launch it into the task containing D because D is defined as "singleInstance", so it launches it into a task that has the same "taskAffinity", in this case the task containing A->B->C). To do that, Android brings the task containing A->B->C to the front. Now you have 2 tasks: The task containing A->B->C-> in the front, and the second one containing D below that.

Another Action: Press Back Button
What Happened: it finishes activity E and returns to the activity below that in the task, namely C. You still have 2 tasks: The one containing C in the front, and the one containing D below that.

Anther Action: Pressing Back key when there were two tasks: D || A
Now you press the BACK key again. This finishes activity A (and thereby finishes the task that held A) and brings the previous task in the task stack to the front, namely the task containing D. You now have 1 task: the task containing D.


* Scenario 3:
Current Stack: D || A->B->C->E (D was started with single Instance)
New Action: D is started from E again
Now: A->B->C->E || D 
What Happened: D's task comes in foreground, existing instance of D gets data through onNewIntent(bundle)

