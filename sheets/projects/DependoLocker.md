
### Contents
* [Bluetooth and BLE](#bluetooth-and-ble)
* [ATT and GATT](#att-and-gatt)
* [Configuration](#configuration)
* [Register a new Locker](#register-a-new-locker)
        

#### Bluetooth and BLE
* Bluetooth is a  wireless technology standard. It was developed as a way to exchange a lot of data over a short range without the need for wires.

* Bluetooth Low Energy Also known as Bluetooth 4.0. When talking about Bluetooth Low Energy vs. Bluetooth, the key difference is in BLE's low power consumption. Applications on BLE can run on a small battery for four to five years. It is vital for applications that only need to exchange small amounts of data periodically.

*  Unlike classic Bluetooth, **BLE remains in sleep mode constantly except for when a connection is initiated**.

* In summary, Bluetooth and Bluetooth Low Energy are used for very different purposes. Bluetooth can handle a lot of data, but consumes battery life quickly and costs a lot more. BLE is used for applications that do not need to exchange large amounts of data, and can therefore run on battery power for years at a cheaper cost.

#### ATT vs GATT

---------------------------------------------------------------------------------------------------------------------------
#### Hardware Configuration

* Every physical locker contains multiple compartments,Each compartments has multiples shelves. 

* Shelves come in three different sizes: small, medium, and large, user can select a shelf to put the shipments according to size of the shipments.

* Each compartment is configured with a unique BLE device, Hence every compartment can be uniquely identified with the mac address of the BLE device.

* **Every compartment has only one BLE device attached to it, and to open/close all the shelves in that compartment, we need to connect with the same BLE device in the compartment (and modify different bits to open/close different shelves in it.)**. 

* An android tablet is attached to the locker with this application installed in it. The tablet will take instruction from the user, connect with one of the BLE devices in the locker and then open/close the shelves using BLE communication with the locker.

#### How it works

* To book a locker, it can be searched from the android app through pin code of the area. Once the locker is selected and user books it (if it is empty?), user gets a passcode to access the locker, he can use this passcode to select a shelf and put his shipment. 
* The (same?) passcode can be shared with other people, if they want to get the shipment stored in the locker.


#### Representing Locker

* Locker Model looks like this:

```java
public class Locker {

    private String _id;
    private String android_id;
    private String locker_id;
    private String address;
    private String landmark;
    private String locality;
    private String city;
    private String state;
    private String countre;
    private int pincode;
    private double lat;
    private double lng;
    private List<Compartment> compartments;
    
}
```

* A ```Compartment``` looks like this:
```java
public class Compartment {
    private String _id;
    //Represents BLE Device's MAC Address
    private String compartment_id;
    private List<Shelf> shelves;
    private String number;
}

```

* And a shelf looks like this:
```java
public class Shelf {
    private String _id;
    private String number;
    private Float length;
    private Float breadth;
    private Float height;
    private String type;
    private String status;
    private String is_occupied;

}
```

#### Register a new Locker

* To create a new locker following points need understanding:

1) **Android Device ID**:  This returns a 64bit hex string that is **unique to each combination of app-signing key, user, and device**.
```Settings.Secure.getString(AppMain.getContext().getContentResolver(), Settings.Secure.ANDROID_ID)```
The value may change if a factory reset is performed on the device or if an APK signing key changes.

2) **Server Locker Id**: When a new locker is created in the web console, it is assigned a 10 digit locker_id which is unique. No two lockers can have same locker_id. 

* Once a locker is created, according to the hardware configuration of the actual locker, compartment and their shelves are created and assigned to the locker.

* While creating a ```Compartment```, it is provided with the **mac address** of the BLE device attached to it. 

* With above information, we have successfully registered a new locker in our server database.


#### Linking tablet with the locker

* The Locker also has an android tab attached to it, with Locker app installed in it. We need to map this android device to the locker.

* There can be three scenarios: 
  1. **A new Tab is being linked to a new locker**: 
  In this case, Android Id for the device is generated, and saved to Shared Preference. User is asked to insert the Server Locker Id in the application (which is provided to him when the locker is created in the server database).
  Once it is submitted, in the server side, Locker with this locker id is fetched, and the android id is saved in it, completing the mapping. Server returns the locker id back to the tab, which it saves in Shared Preference.
  Next time when the application is opened, android id and server locker id is fetched from shared preference, and we don't require to link this tab to server Locker.
 
  2. **An already linked tabled has to be linked with locker, because the app was reinstalled**: Again android id will be generated in the device, (which will most probably be same) and user will have to enter the locker id provided by server database. This new android id will be saved, replacing the previous one (in case the android id generated this time is different) in the locker, and linking will be complete.
  
  3. **A tab was already linked to the locker, but is being replaced with a new tab due to some error**: In this case too, since the server replaces the previous android id with new one, linking will not create any issues



#### TABLET MODE

1. Check for bluetooth support in the device. Availabe above API 18.

```java
public static boolean checkBleSupport(Context mContext) {

        if(android.os.Build.VERSION.SDK_INT < Build.VERSION_CODES.JELLY_BEAN_MR2)
            return false;

        final BluetoothManager bluetoothManager = (BluetoothManager) mContext.getSystemService(Context.BLUETOOTH_SERVICE);
        if (bluetoothManager.getAdapter() == null)
            return false;
        else return true;

    }

```

2. Request for runtime permissions if the device is running Android Marshmallow or above. Permissions required:

```ACCESS_FINE_LOCATION, ACCESS_COARSE_LOCATION, ACCESS_BLUETOOTH, ACCESS_BLUETOOTH_ADMIN, READ_SMS, RECEIVE_SMS```




3. 

4. **Server Device Id**: For every locker created, there will be a tablet device assigned to it, and will be coupled with it. 




* If the ```Build.VERSION.SDK_INT``` is 24 or greater, take  following runtime permissions.
 ```ACCESS_FINE_LOCATION, ACCESS_COARSE_LOCATION, ACCESS_BLUETOOTH, ACCESS_BLUETOOTH_ADMIN, READ_SMS, RECEIVE_SMS```
 
 * When all permissions are granted, check for the app flavour:
 
 
