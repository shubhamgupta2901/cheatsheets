* In 2017 Lifecycle Library was announced.
* Lifecycle Library is a set of libraries for avoiding memory leaks and solving common android lifecycle challanges.
* Lifecycle library is part of jetpack and includes new integrations with data binding.
* Lifecycle library's View Model class:

* A ViewModel holds our app's UI data while surviving configuration changes.
* Rotating your phone is considered a configuration change, Configuration changes cause the whole activity to be destroyed and recreated again.
* If we don't properly save and restore that data, we may lose the data in such event, and we may end up with wierd UI bugs and crashes.
* That's where ViewModel classes are important, they survive configuration changes. So instead of putting all our UI data, we can put it in ViewModel instead.
* This helps with configuration change, but in general it is also a good software design.
* One Common pitfall while designing an android app is putting a lot of logic inside the activity, like:
  * Drawing UI Components
  * Saving and restoring UI 
  * Data loading via netork and database
  * Processing data
  * Handling UI interactions
* This creates a large unmaintainable class, and violates the single responsibility principle, we can use View Model to easily divide that responsibility.
* View Model will be responsible for holding the data that we are gonna show in the UI, and the activity is only responsible for knowing how to draw that data to the screen, and receiving user interaction(but not for processing them).
* If app loads and stores data, we can use the repostory pattern.

### Pitfall
* Never pass context to ViewModels, this includes activity, fragments and views. Since View Models can outlive our specific activity and fragment lifecycles.
* Lets say we store the instance of activity in our ViewModel, when we rotate the screen that activity is destroyed. We now have a ViewModel holding the reference of destroyed activity.And this is a memory leak.
* So if we want to use context, we must the ones that will outlive viewmodel.We can also use AndroidViewModel subclass that has the reference to our activity.


### ViewModels can not replace onSavedInstance
* ViewModels do not survive process shutdown on resource restrictions. onSavedInstances are created to handle such death scenarios.
* View Models are meant for storing large amount of data, but onSavedInstance not so much.
* onSavedInstance requires serialization.
