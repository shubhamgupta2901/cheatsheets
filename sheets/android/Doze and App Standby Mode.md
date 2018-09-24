### Contents
  * [Doze Mode](#doze-mode)



* Starting from API  23, Android introduces two power-saving features that extend battery life for users by managing how apps behave when a device is not connected to a power source. 
 
 * **Doze** reduces battery consumption by postponing background CPU and network activity for apps when the device is unused for long periods of time. **App Standby** postpones background network activity for apps with which the user has not recently interacted.
 
 * Doze and App Standby manage the behavior of all apps running on Android Marshmallow or higher, regardless whether the apps are targeting API level 23 or not. 
 
 ### Doze Mode
 
 * **What is idle state?** When an Android device is left in idle state, it will first dim the screen, then turn off the screen, and finally turn off the CPU. This prevents the device battery from quickly being drained or simply idle state is when you are not using device.
 
 * **What is Doze mode?**: If a user leaves a device unplugged and stationary for a period of time, with the screen off, the device enters Doze mode.It attempts to conserve battery by restricting apps' access to network and CPU-intensive services.
 
* Periodically, the system exits Doze for a brief time to let apps complete their postponed tasks. During this **maintenance window**, the system runs all pending syncs, jobs, and alarms, and lets apps access the network.

![Doze mode illustration](https://github.com/shubhamgupta2901/cheatsheets/blob/master/assets/doze.png) 

* After each maintenance window, the system again enters Doze, suspending network access and postponing jobs, syncs, and alarms. Over time, the system schedules maintenance windows less and less frequently, helping to reduce battery consumption in cases of longer-term inactivity when the device is not connected to a charger.

* As soon as the user wakes the device by moving it, turning on the screen, or connecting a charger, the system exits Doze and all apps return to normal activity.


* **Restrictions imposed by Doze Mode**:
  *  Network access is suspended.
  * The system ignores wake locks.
  *  Standard AlarmManager alarms (including setExact() and setWindow()) are deferred to the next maintenance window.
  * The system does not perform Wi-Fi scans.
  * The system does not allow sync adapters to run.
  * The system does not allow JobScheduler to run.
