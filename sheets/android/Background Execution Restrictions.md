1. [Background Execution Restrictions](#background-execution-restrictions)
    * [Background Service Limitations](#background-service-limitations)
    * [Broadcast Limitations](#broadcast-limitations)
3. [Job Intent Services](#job-intent-services)

### Background Execution Restrictions
* Whenever an app runs in the background, it consumes some of the device's limited resources, like RAM. This can result in an impaired user experience, especially if the user is using a resource-intensive app. 

* To improve the user experience, Android 8.0 (API level 26) imposes **limitations on what apps can do while running in the background**. 

* Many Android apps and services can be running simultaneously.If additional apps or services are running in the background, this places additional loads on the system, which could result in a poor user experience; (for example, the music app might be suddenly shut down in the background).

* To lower the chance of these problems, Android 8.0 places limitations on what apps can do while users aren't directly interacting with them. 

* Apps are restricted in two ways:
  * [Background Service Limitations](#background-service-limitations)
  * [Broadcast Limitations](#broadcast-limitations)

* In most cases, apps can work around these limitations by using ```JobScheduler``` jobs. This lets an app run tasks in background even when  it is not actively running. At the same time it allows the operating system to schedule jobs in a way
that doesn't affect the user experience. 

* Android 8.0 offers several improvements to ```JobScheduler``` that make it easier to replace services and broadcast receivers with scheduled jobs. 

#### Background Service Limitations

* Services running in the background can consume device resources, resulting in a worse user experience. To deal with this problem, the system applies a number of limitations on services.

* **These rules don't affect bound services in any way.** 

* An app is considered to be in the ***foreground*** if any of the following is true:
    * **It has a visible activity**, whether the activity is started or paused.
    * **It has a foreground service.**
    * **Another foreground app is connected to the app**, either by binding to one of its services or by making use of one of its content providers. For example, the app is in the foreground if another app binds to its:
       * IME
       * Wallpaper service
* If none of those conditions is true, the app is considered to be in the background.
      
* While an app is in the foreground, it can create and run both foreground and background services freely. When an app goes into the background, it has a window of several minutes in which it is still allowed to create and use services. At the end of that window, the app is considered to be idle. At this time, the system stops the app's background services, just as if the app had called the services' ```Service.stopSelf()``` methods.

* **Under certain circumstances, a background app is placed on a temporary whitelist for several minutes**. While an app is on the whitelist, it can launch services without limitation, and its background services are permitted to run. An app is placed on the whitelist when it handles a task that's visible to the user, such as:
  * Handling a high-priority Firebase Cloud Messaging (FCM) message.
  * Receiving a broadcast, such as an SMS/MMS message.
  * Executing a PendingIntent from a notification.
 
* Note: ```IntentService``` is a service, and is therefore subject to the new restrictions on background services. As a result, many apps that rely on ```IntentService``` do not work properly when targeting Android 8.0 or higher. For this reason, Android Support Library 26.0.0 introduces a new ```JobIntentService``` class, which provides the same functionality as ```IntentService``` but **uses jobs instead of services** when running on Android 8.0 or higher.


* In many cases, your app can replace background services with ```JobScheduler``` jobs. For example, CoolPhotoApp needs to check whether the user has received shared photos from friends, even if the app isn't running in the foreground. Previously, the app used a background service which checked with the app's cloud storage. To migrate to Android 8.0 (API level 26), the developer replaces the background service with a scheduled job, which is launched periodically, queries the server, then quits.

* Prior to Android 8.0, the usual way to create a foreground service was to create a background service, then promote that service to the foreground. **With Android 8.0,the system doesn't allow a background app to create a background service. For this reason, Android 8.0 introduces the new method ```startForegroundService()``` to start a new service in the foreground. After the system has created the service, the app has five seconds to call the service's ```startForeground()``` method to show the new service's user-visible notification. If the app does not call ```startForeground()``` within the time limit, the system stops the service and declares the app to be ANR.


