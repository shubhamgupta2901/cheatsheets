

* Android offers the Service class to create application components that handle long-lived operations and include functionality that doesn’t require a user interface. 

* Although Services run without a dedicated UI, **they still execute in the main Thread of the applicationss process** — just like Activities and Broadcast Receivers.

* While Activities are started, stopped, and re-created regularly as part of their lifecycle, Services
are designed to be longer-lived — specifi cally, to perform ongoing and potentially time-consuming
operations.

* Services are started, stopped, and controlled from other application components, including Activities,
Broadcast Receivers, and other Services.

*   Android may
stop a Service prematurely to provide additional resources to a higher priority component(typically a foreground component). When that happens, your Service can be configured to restart automatically when
resources become available.

* If your Service is interacting directly with the user (for example, by playing music), it may be necessary
to increase its priority by labeling it as a foreground service. This will ensure that your
Service isn't terminated except in extreme circumstances, but it reduces the run time’s ability to
manage its resources, potentially degrading the overall user experience.


//write a code snippet to start a service

 * The onStartCommand method is called whenever the Service is started using startService, so
it may be executed several times within a Service’s lifetime. You should ensure that your Service
accounts for this.

* Services are launched on the main Application Thread, meaning that any processing done in the
onStartCommand handler will happen on the main GUI Thread. The standard pattern for implementing
a Service is to create and run a new Thread from onStartCommand to perform the processing
in the background, and then stop the Service when it’s been completed.

* RETURN TYPE: 
When we override the onStartCommand method.
Note that it returns a value that controls how the system will respond if the Service is restarted
after being killed by the run time. 

``java
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
 startBackgroundTask(intent, startId);
 return Service.START_STICKY;
}
```

*  it enables you to control the restart behavior
by returning one of the following Service constants:

* START_STICKY —  If you return this value, onStartCommand
will be called any time your Service restarts after being terminated by the run time. Note that
on a restart the Intent parameter passed in to onStartCommand will be null.
This mode typically is used for Services that handle their own states and that are explicitly
started and stopped as required (via startService and stopService). This includes
Services that play music or handle other ongoing background tasks.

* START_NOT_STICKY — 
Following termination by the run time, Services set to this mode restart only if there are
pending start calls. If no startService calls have been made since the Service was terminated,
the Service will be stopped without a call being made to onStartCommand.
This mode is ideal for Services that handle specifi c requests, particularly regular processing
such as updates or network polling. Rather than restarting the Service during a period
of resource contention, it’s often more prudent to let the Service stop and retry at the next
scheduled interval.

* START_REDELIVER_INTENT — In some circumstances you will want to ensure that the commands
you have requested from your Service are completed — for example when timeliness is important.
This mode is a combination of the fi rst two; if the Service is terminated by the run time, it
will restart only if there are pending start calls or the process was killed prior to its calling
stopSelf. In the latter case, a call to onStartCommand will be made, passing in the initial
Intent whose processing did not properly complete.

* Note that each mode requires you to explicitly stop your Service, through a call to stopService or
stopSelf, when your processing has completed. 

* The restart mode you specify in your onStartCommand return value will affect the parameter values
passed in to it on subsequent calls. Initially, the Intent will be the parameter you passed in to
startService to start your Service. After system-based restarts it will be either null, in the case of
START_STICKY mode, or the original Intent if the mode is set to START_REDELIVER_INTENT.
The flag parameter can be used to discover how the Service was started. In particular, you determine
if either of the following cases is true:
*  START_FLAG_REDELIVERY — Indicates that the Intent parameter is a redelivery caused by the
system run time’s having terminated the Service before it was explicitly stopped by a call to
stopSelf.
* START_FLAG_RETRY — Indicates that the Service has been restarted after an abnormal termination.
It is passed in when the Service was previously set to START_STICKY.

### Starting and Stopping Services
To start a Service, call startService. Much like Activities, you can either use an action to implicitly
start a Service with the appropriate Intent Receiver registered, or you can explicitly specify the
Service using its class. If the Service requires permissions that your application does not have,
the call to startService will throw a SecurityException.

```java
private void explicitStart() {
 // Explicitly start My Service
 Intent intent = new Intent(this, MyService.class);
 // TODO Add extras if required.
 startService(intent);
}
private void implicitStart() {
 // Implicitly start a music Service
 Intent intent = new Intent(MyMusicService.PLAY_ALBUM);
 intent.putExtra(MyMusicService.ALBUM_NAME_EXTRA, “United”); 
  intent.putExtra(MyMusicService.ARTIST_NAME_EXTRA, “Pheonix”);
 startService(intent); 
 }

```

Stopping a service:

```java
// Stop a service explicitly.
stopService(new Intent(this, MyService.class));

// Stop a service implicitly.
Intent intent = new Intent(MyMusicService.PLAY_ALBUM);
stopService(intent); 
```

* Calls to startService do not nest, so a single call to stopService will terminate the running
Service it matches, no matter how many times startService has been called.

* Self-Terminating Services

When your Service has completed the actions or processing for which it was started, you should terminate
it by making a call to stopSelf. You can call stopSelf either without a parameter to force
an immediate stop, or by passing in a startId value to ensure processing has been completed for
each instance of startService called so far.
By explicitly stopping the Service when your processing is complete, you allow the system to recover
the resources otherwise required to keep it running.

//TODO: Bind Service and Service Connection.

## Creating Foreground Services
 Android uses a dynamic
approach to managing resources that can result in your application’s components being terminated
with little or no warning. When calculating which applications and application components should be killed, Android assigns
running Services the second-highest priority. Only active, foreground Activities are considered a
higher priority. In cases where your Service is interacting directly with the user, it may be appropriate to lift its priority to the equivalent of a foreground Activity’s. You can do this by setting your Service to run in
the foreground by calling its startForeground method. Because foreground Services are expected to be interacting directly with the user (for example, by playing music), calls to startForeground must specify an ongoing Notifi cation. This notifi cation
will be displayed for as long as your Service is running in the foreground.


```java
private void startPlayback(String album, String artist) {
 int NOTIFICATION_ID = 1;
 // Create an Intent that will open the main Activity
 // if the notification is clicked.
 Intent intent = new Intent(this, MyActivity.class);
 PendingIntent pi = PendingIntent.getActivity(this, 1, intent, 0);
 // Set the Notification UI parameters
 Notification notification = new Notification(R.drawable.icon,
 “Starting Playback”, System.currentTimeMillis());
 notification.setLatestEventInfo(this, album, artist, pi);
 // Set the Notification as ongoing
 notification.flags = notification.flags |
 Notification.FLAG_ONGOING_EVENT;
 // Move the Service to the Foreground
 startForeground(NOTIFICATION_ID, notification);
}

```

* Moving a Service to the foreground effectively makes it impossible for the run
time to kill it in order to free resources. Having multiple unkillable Services running
simultaneously can make it extremely diffi cult for the system to recover
from resource-starved situations.
Use this technique only if it is necessary in order for your Service to function
properly, and even then keep the Service in the foreground only as long as absolutely
necessary.

* When your Service no longer requires foreground priority, you can move it back to the background,
and optionally remove the ongoing notifi cation using the stopForeground method, as shown in
Listing 9-10. The Notifi cation will be canceled automatically if your Service stops or is terminated. 
