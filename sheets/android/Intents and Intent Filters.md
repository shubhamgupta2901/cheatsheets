### Contents
  * [Intents](#intents)
  * [Intent Types](#intent-types)

### Intents

* An Intent is a messaging object that help in communication between two app components. We can use them to request an action from another app component. Three fundamental use cases of intents:

**Starting an activity**: 
* We can start a new instance of an Activity by passing an Intent to ```startActivity()```. The Intent describes the activity to start and carries any necessary data.

```java
public void launchSecondActivity(View view) {
       Log.d(LOG_TAG, "Button clicked!");

       Intent intent = new Intent(this, SecondActivity.class);
       String message = mMessageEditText.getText().toString();

       intent.putExtra(EXTRA_MESSAGE, message);
       startActivity(intent);
}
```

* If we want to receive a result from the activity when it finishes, call ```startActivityForResult()```. Your activity receives the result as a separate Intent object in your activity's ```onActivityResult()``` callback. 

* We do need to pass an additional integer argument to the ```startActivityForResult()``` method.This is a "request code" that identifies your request. When you receive the result Intent, the callback provides the same request code so that your app can properly identify the result and determine how to handle it.

```java
static final int PICK_CONTACT_REQUEST = 1;  // The request code
...
private void pickContact() {
    Intent pickContactIntent = new Intent(Intent.ACTION_PICK, Uri.parse("content://contacts"));
    pickContactIntent.setType(Phone.CONTENT_TYPE); // Show user only contacts w/ phone numbers
    startActivityForResult(pickContactIntent, PICK_CONTACT_REQUEST);
}
```

* When the user is done with the subsequent activity and returns, the system calls your activity's onActivityResult() method. This method includes three arguments: request code you passed to ```startActivityForResult()```, A result code specified by the second activity(RESULT_OK or RESULT_CANCELED) and an Intent that carries the result data.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // Check which request we're responding to
    if (requestCode == PICK_CONTACT_REQUEST) {
        // Make sure the request was successful
        if (resultCode == RESULT_OK) {
            // The user picked a contact.
            // The Intent's data Uri identifies which contact was selected.

            // Do something with the contact here (bigger example below)
        }
    }
}
```

**Starting a service**

* A Service is a component that performs operations in the background without a user interface. With API level 21 and later, you can start a service with JobScheduler. For earlier versions, you can start a service by using methods of the Service class. You can start a service to perform a one-time operation (such as downloading a file) by passing an Intent to ```startService()```. The Intent describes the service to start and carries any necessary data.

* If the service is designed with a client-server interface, you can bind to the service from another component by passing an Intent to ```bindService()```. 

**Delivering a broadcast**

* A broadcast is a message that any app can receive. The system delivers various broadcasts for system events, such as when the system boots up or the device starts charging. You can deliver a broadcast to other apps by passing an Intent to ```sendBroadcast()``` or sendOrderedBroadcast().

### Intent Types

* **Explicit Intents** specify which application will satisfy the intent, by supplying either the target app's package name or a fully-qualified component class name. We typically use an explicit intent to start a component in your own app, because you know the class name of the activity or service you want to start. For example, you might start a new activity within your app in response to a user action, or start a service to download a file in the background.

* **Implicit intents** do not name a specific component, but instead declare a general action to perform, which allows a component from another app to handle it.

* When you use an implicit intent, the Android system finds the appropriate component to start by comparing the contents of the intent to the intent filters declared in the manifest file of other apps on the device. If the intent matches an intent filter, the system starts that component and delivers it the Intent object. If multiple intent filters are compatible, the system displays a dialog so the user can pick which app to use.

* An **intent filter** is an expression in an app's manifest file that specifies the type of intents that the component would like to receive. For instance, by declaring an intent filter for an activity, you make it possible for other apps to directly start your activity with a certain kind of intent. Likewise, if you do not declare any intent filters for an activity, then it can be started only with an explicit intent.
