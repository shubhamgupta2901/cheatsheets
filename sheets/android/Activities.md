## Contents
* [Activity Lifecycle](#activity-lifecycle)
* [Difference between onCreate and onStart](#difference-between-oncreate-and-onstart)
* [Scenario where onDestroy is called without onPause or onStop](#scenario-where-onDestroy-is-called-without-onPause-or-onStop)
* [Configuration Changes](#configuration-changes)
* [Tasks and BackStack](#tasks-and-backstacks)
* [Launch Modes](#launch-modes)
    * [Standard](#standard)
    * [SingleTop](#singletop)
    * [SingleTask](#singletask)
    * [SingleInstance](#singleinstance)
    

#### Launch Modes

* Launch modes allow us to define **how a new instance of an activity is associated with the current task**. We can define different launch modes in two ways:
    * **using manifest file**: When you declare an activity in your manifest file, you can specify how the activity should associate with tasks when it starts.
    * **using intent flags**: When you call ```startActivity()```, you can include a flag in the Intent that declares how (or whether) the new activity should associate with the current task.
* Some launch modes available for the manifest file are not available as flags for an intent and, likewise, some launch modes available as flags for an intent cannot be defined in the manifest.
* if ```ActivityA``` starts ```ActivityB```, ```ActivityB``` can define in its manifest how it should associate with the current task and ```ActivityA``` can also request how ```ActivityB``` should associate with current task using intent flags. If both activities define how ```ActivityB``` should be associated with a task, then ```ActivityA's``` request (as defined in the intent) is honored over ```ActivityB's``` request (as defined in its manifest).

##### [Standard](#standard) 
* It is the **default** launchMode. It creates a new instance of an activity in the task from which it was started. Multiple instances of the activity can be created and multiple instances can be added to the same or different tasks. 
Eg: Suppose there is an activity stack of A -> B -> C. 
Now if we launch B again with the launch mode as "standard", the new stack will be A -> B -> C -> B.
    
##### [SingleTop](#SingleTop)
* It is the same as the standard, except if there is a previous instance of the activity that exists in the top of the stack, then it will not create a new instance but rather send the intent to the existing instance of the activity.
Eg: Suppose there is an activity stack of A -> B. 
Now if we launch C with the launch mode as "singleTop", the new stack will be A -> B -> C as usual. 
Now if there is an activity stack of A -> B -> C. 
If we launch C again with the launch mode as "singleTop", the new stack will still be A -> B -> C.

* For example, suppose a task's back stack consists of root activity A with activities B, C, and D on top (the stack is A-B-C-D; D is on top). An intent arrives for an activity of type D. If D has the default "standard" launch mode, a new instance of the class is launched and the stack becomes A-B-C-D-D. However, if D's launch mode is "singleTop", the existing instance of D receives the intent through ```onNewIntent()```, because it's at the top of the stack—the stack remains A-B-C-D. However, if an intent arrives for an activity of type B, then a new instance of B is added to the stack, even if its launch mode is "singleTop"

##### [SingleTask](#SingleTask)
* The system creates a new task and instantiates the activity at the root of the new task.
* However, if an instance of the activity already exists in a separate task, the system routes the intent to the existing instance through a call to its ```onNewIntent()``` method, rather than creating a new instance.
* Eg: Suppose there is an activity stack of A -> B -> C -> D. 
Now if we launch D with the launch mode as "singleTask", the new stack will be A -> B -> C -> D as usual.
* Now if there is an activity stack of A -> B -> C -> D. 
If we launch activity B again with the launch mode as "singleTask", the new activity stack will be A -> B. Activities C and D will be destroyed.

##### [SingleInstance](#SingleInstance)

---------

#### Activity Lifecycle
* When the user interacts with the application, the operating system has to create, stop and resume and destroy activities in the app. Because of this the state of activity keeps changing. The ```Activity``` class provides callbacks to the instance of activity, which allows the activity to know that a state has changed. It allows the activity to know that the system is creating it, stoping it, resuming it or destroying it.

* Each callback allows us to perform specific work that's appropriate to a given change of state. Doing the right work at the right time and handling transitions properly make your app more robust and performant.

* To navigate transitions between stages of the activity lifecycle, the Activity class provides a core set of six callbacks: ```onCreate(), onStart(), onResume(), onPause(), onStop(), and onDestroy()```. The system invokes each of these callbacks as an activity enters a new state.

![Activity Lifecycle](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/activity_lifecycle.png)

##### ```onCreate()```
* This callback is fired when the system first creates the activity and the activity enters the *Created* state. 

* In the ```onCreate()``` method, we perform basic application startup logic that should happen only once for the entire life of the activity. For example, your implementation of ```onCreate()``` might bind the layout xml file to the activity, bind data to lists and instantiate some class-scope variables. 

* This method receives the parameter ```savedInstanceState```, which is a ```Bundle``` object containing the activity's previously saved state. If the activity has never existed before, the value of the ```Bundle``` object is null.

* The following example of the ```onCreate()``` method shows fundamental setup for the activity, such binding an xml layout to the activity, defining member variables, and configuring some of the UI. In this example, the XML layout file is specified by passing file’s resource ID ```R.layout.main_activity``` to ```setContentView()```.

* **Why would you do the ```setContentView()``` in ```onCreate()``` of Activity class?** : ```setContentView()``` is a heavy operation because it binds an xml layout file to the activity. Since ```onCreate()``` is called only once during the activity lifecycle when it is created, it would be appropiate to do it here, rather than ```onStart()``` or ```onResume()``` which are called multiple times during the lifecycle of the activity.


```java
TextView mTextView;

// some transient state for the activity instance
String mGameState;

@Override
public void onCreate(Bundle savedInstanceState) {
    // call the super class onCreate to complete the creation of activity like
    // the view hierarchy
    super.onCreate(savedInstanceState);

    // recovering the instance state
    if (savedInstanceState != null) {
        mGameState = savedInstanceState.getString(GAME_STATE_KEY);
    }

    // set the user interface layout for this activity
    // the layout file is defined in the project res/layout/main_activity.xml file
    setContentView(R.layout.main_activity);

    // initialize member TextView so we can manipulate it later
    mTextView = (TextView) findViewById(R.id.text_view);
}

// This callback is called only when there is a saved instance that is previously saved by using
// onSaveInstanceState(). We restore some state in onCreate(), while we can optionally restore
// other state here, possibly usable after onStart() has completed.
// The savedInstanceState Bundle is same as the one used in onCreate().
@Override
public void onRestoreInstanceState(Bundle savedInstanceState) {
    mTextView.setText(savedInstanceState.getString(TEXT_VIEW_KEY));
}

// invoked when the activity may be temporarily destroyed, save the instance state here
@Override
public void onSaveInstanceState(Bundle outState) {
    outState.putString(GAME_STATE_KEY, mGameState);
    outState.putString(TEXT_VIEW_KEY, mTextView.getText());

    // call superclass to save any view hierarchy
    super.onSaveInstanceState(outState);
}
```
* Your activity does not reside in the *Created* state. After the ```onCreate()``` method finishes execution, the activity enters the *Started* state, and the system calls the ```onStart()``` and ```onResume()``` methods in quick succession. 

##### ```onStart()```

* When the activity enters the *Started* state, the system invokes this callback. The ```onStart()``` call makes the activity visible to the user, as the app prepares for the activity to enter the foreground and become interactive. For example, this method is where the app initializes the code that maintains the UI.

* The ```onStart()``` method completes very quickly and the activity does not stay resident in the *Started* state. Once this callback finishes, the activity enters the *Resumed* state, and the system invokes the ```onResume()``` method. 

##### ```onResume()```

* When the activity enters the *Resumed* state, it comes to the foreground, and then the system invokes the ```onResume()``` callback. **This is the state in which the app interacts with the user**. The app stays in this *RUNNING* state until something happens to take focus away from the app. Such an event might be, for instance, receiving a phone call, the user’s navigating to another activity, or the device screen’s turning off.

* When an interruptive event occurs, the activity enters the *Paused* state, and the system invokes the ```onPause()``` callback.

* If the activity returns to the *Resumed* state from the *Paused* state, the system once again calls ```onResume()``` method. For this reason, you should implement ```onResume()``` to initialize components that you release during ```onPause()```, and perform any other initializations that must occur each time the activity enters the Resumed state.

##### ```onPause()```
* The system calls this method as the first indication that the user is leaving your activity (though it does not always mean the activity is being destroyed); it indicates that the activity is no longer in the foreground (though it may still be visible if the user is in multi-window mode). 
* In multi-window mode, your activity may be fully visible even when it is in the Paused state. For example, when the user is in multi-window mode and taps the other window that does not contain your activity, your activity will move to the Paused state. 
* Use the ```onPause()``` method to pause or adjust operations that should not continue (or should continue in moderation) while the ```Activity``` is in the Paused state, and that you expect to resume shortly. There are several reasons why an activity may enter this state. For example:
 * In Android 7.0 (API level 24) or higher, multiple apps run in multi-window mode. Because only one of the apps (windows) has focus at any time, the system pauses all of the other apps.
 * A new, semi-transparent activity (such as a dialog) opens. As long as the activity is still partially visible but not in focus, it remains paused.

* You can also use the ```onPause()``` method to release system resources, handles to sensors (like GPS), or any resources that may affect battery life while your activity is paused and the user does not need them. However, as mentioned above in the ```onResume()``` section, a *Paused* activity may still be fully visible if in multi-window mode. As such, you should consider using ```onStop()``` instead of ```onPause()``` to fully release or adjust UI-related resources and operations to better support multi-window mode.

* ```onPause()``` execution is very brief, and does not necessarily afford enough time to perform save operations. For this reason, you should not use ```onPause()``` to save application or user data, make network calls, or execute database transactions; such work may not complete before the method completes. Instead, you should perform heavy-load shutdown operations during ```onStop()```.

* Completion of the ```onPause()``` method does not mean that the activity leaves the *Paused* state. Rather, the activity remains in this state until either the activity resumes or becomes completely invisible to the user. If the activity resumes, the system once again invokes the ```onResume()``` callback. If the activity returns from the Paused state to the *Resumed* state, the system keeps the Activity instance resident in memory, recalling that instance when the system invokes ```onResume()```. In this scenario, you don’t need to re-initialize components that were created during any of the callback methods leading up to the *Resumed* state. If the activity becomes completely invisible, the system calls ```onStop()```.

##### ```onStop()```

* When your activity is no longer visible to the user, it has entered the Stopped state, and the system invokes the ```onStop()``` callback. This may occur, for example, when a newly launched activity covers the entire screen. The system may also call ```onStop()``` when the activity has finished running, and is about to be terminated.

* In the ```onStop()``` method, the app should release or adjust resources that are not needed while the app is not visible to the user. For example, your app might pause animations or switch from fine-grained to coarse-grained location updates. Using ```onStop()``` instead of ```onPause()``` ensures that UI-related work continues, even when the user is viewing your activity in multi-window mode.

* You should also use ```onStop()``` to perform relatively CPU-intensive shutdown operations. For example, if you can't find a more opportune time to save information to a database, you might do so during ```onStop()```. 

* When your activity enters the *Stopped* state, the Activity object is kept resident in memory: It maintains all state and member information, but is not attached to the window manager. When the activity resumes, the activity recalls this information. You don’t need to re-initialize components that were created during any of the callback methods leading up to the *Resumed* state. The system also keeps track of the current state for each View object in the layout, so if the user entered text into an ```EditText``` widget, that content is retained so you don't need to save and restore it.

* Note: Once your activity is stopped, the system might destroy the process that contains the activity if the system needs to recovery memory. Even if the system destroys the process while the activity is stopped, the system still retains the state of the View objects (such as text in an EditText widget) in a Bundle (a blob of key-value pairs) and restores them if the user navigates back to the activity.

* From the *Stopped* state, the activity either comes back to interact with the user, or the activity is finished running and goes away. If the activity comes back, the system invokes ```onRestart()```. If the Activity is finished running, the system calls ```onDestroy()```. 

##### ```onDestroy()```

* onDestroy() is called before the activity is destroyed. The system invokes this callback either because:
    1.the activity is finishing (due to the user completely dismissing the activity or due to finish() being called on the activity), or
    2.the system is temporarily destroying the activity due to a configuration change (such as device rotation or multi-window mode)

* If the activity is finishing, ```onDestroy()``` is the final lifecycle callback the activity receives. If ```onDestroy()``` is called as the result of a configuration change, the system immediately creates a new activity instance and then calls ```onCreate()``` on that new instance in the new configuration.

* The ```onDestroy()``` callback should release all resources that have not yet been released by earlier callbacks such as ```onStop()```.

#### Difference between onCreate and onStart
* **```onCreate()```**: The ```onCreate()``` is called only once during the activity lifecycle, either when the application starts, or when the activity has been destroyed and then recreated, for example during a configuration change.

* **```onStart()```**: The ```onStart()``` method can be called multiple times during the lifecycle of the activity. It is called whenever the activity becomes visible to the user.


#### Scenario where onDestroy is called without onPause or onStop

* ```onPause()``` and ```onStop()``` will not be invoked if ```finish()``` is called from within the ```onCreate()``` method. This might occur, for example, if you detect an error during ```onCreate()``` and call ```finish()``` as a result. In such a case, though, any cleanup you expected to be done in ```onPause()``` and ```onStop()``` will not be executed.

```java
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        finish();
    }

```

### Configuration Changes

* Any configuration change will cause Android to restart your Activity. A configuration change might be 
    * the device rotating (because now we have a different screen layout to draw upon), 
    *  a language switch (because we need to re-write all those strings, which may need more room now OR it could be the scary RTL switch!), 
    * or even keyboard availability. 
* What’s happening here is that the system is trying to be helpful and reload your app with the correct resources. When such configuration changes happen, Android usually destroys your application's existing Activities and Fragments and recreates them. Android does this so that your application can reload resources based on the new configuration. 

* When it destroys your Activities and Fragments it will end up creating new instances of them which will wipe out all of your member variables. To work around this, Android gives you the opportunity to save your app’s state before destroying your Activities and Fragments, and the opportunity to restore your state when recreating them.

TODO: savedInstanceState
