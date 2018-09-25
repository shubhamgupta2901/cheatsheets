### Contents
* [Bluetooth and BLE](#bluetooth-and-ble)
* [ATT and GATT](#att-and-gatt)
* [Configuration](#configuration)
* [Create a new locker](#create-a-new-locker)
        

#### Bluetooth and BLE
* Bluetooth is a  wireless technology standard. It was developed as a way to exchange a lot of data over a short range without the need for wires.

* Bluetooth Low Energy Also known as Bluetooth 4.0. When talking about Bluetooth Low Energy vs. Bluetooth, the key difference is in BLE's low power consumption. Applications on BLE can run on a small battery for four to five years. It is vital for applications that only need to exchange small amounts of data periodically.

*  Unlike classic Bluetooth, **BLE remains in sleep mode constantly except for when a connection is initiated**.

* In summary, Bluetooth and Bluetooth Low Energy are used for very different purposes. Bluetooth can handle a lot of data, but consumes battery life quickly and costs a lot more. BLE is used for applications that do not need to exchange large amounts of data, and can therefore run on battery power for years at a cheaper cost.

#### ATT vs GATT

---------------------------------------------------------------------------------------------------------------------------
#### Configuration

* Every physical locker contains multiple compartments,Each compartments has multiples shelves. 
* Shelves come in three different sizes: small, medium, and large, user can select a shelf to put the shipments according to size of the shipments.
* Each compartment is configured with a unique BLE device, Hence every compartment can be uniquely identified with the mac address of the BLE device.
* To book a locker, it can be searched from the android app through pin code of the area. Once the locker is selected and user books it (if it is empty?), user gets a passcode to access the locker, he can use this passcode to select a shelf and put his shipment. 
* The (same?) passcode can be shared with other people, if they want to get the shipment stored in the locker.

#### Create a new locker

* To create a new locker following points need understanding:

1) **Android Device ID**:  This returns a 64bit hex string that is **unique to each combination of app-signing key, user, and device**.
```Settings.Secure.getString(AppMain.getContext().getContentResolver(), Settings.Secure.ANDROID_ID)```
The value may change if a factory reset is performed on the device or if an APK signing key changes.

2) **Server Locker Id**: For every physical locker, we create a server Locker which looks like this: 

```ruby

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

```



Configuration: There 

* A Locker consists of multiple compartments. Each compartment has a BLE device attached to it.
* Each compartment has multiple shelves. 
* To open a shelf, we need to connect the user device to the compartment BLE device, Once the connection is established, by manipulating the bits, we can open the particular locker.
* Two flavours of same application: ```TABLET_MODE``` and ```CUSTOMER_MODE```.




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
 
 
