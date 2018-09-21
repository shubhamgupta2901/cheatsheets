* A Locker consists of multiple compartments. Each compartment has a BLE device attached to it.
* Each compartment has multiple shelves. 
* To open a shelf, we need to connect the user device to the compartment BLE device, Once the connection is established, by manipulating the bits, we can open the particular locker.
* Two flavours of same application: ```TABLET_MODE``` and ```CUSTOMER_MODE```.

* Check if the device has BLE support
```java
private boolean hasBLESupport() {
        // Use this check to determine whether BLE is supported on the device.
        if (!getPackageManager().hasSystemFeature(
                PackageManager.FEATURE_BLUETOOTH_LE)) {
            return false;
        }
        // Initializes a Blue tooth adapter.
        final BluetoothManager bluetoothManager = (BluetoothManager) getSystemService(Context.BLUETOOTH_SERVICE);
        if (bluetoothManager.getAdapter() == null)
            return false;
        return true;
    }
```

* If the ```Build.VERSION.SDK_INT``` is 24 or greater, take  following runtime permissions.
 ```ACCESS_FINE_LOCATION, ACCESS_COARSE_LOCATION, ACCESS_BLUETOOTH, ACCESS_BLUETOOTH_ADMIN, READ_SMS, RECEIVE_SMS```
 
 * When all permissions are granted, check for the app flavour:
 
 
