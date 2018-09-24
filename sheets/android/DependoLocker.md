### Contents
* [Bluetooth and BLE](#bluetooth-and-ble)
* [ATT and GATT](#att-and-gatt)
* [TABLET MODE](#tablet-mode)
        

#### Bluetooth and BLE
* Bluetooth is a  wireless technology standard. It was developed as a way to exchange a lot of data over a short range without the need for wires.

* Bluetooth Low Energy Also known as Bluetooth 4.0. When talking about Bluetooth Low Energy vs. Bluetooth, the key difference is in BLE's low power consumption. Applications on BLE can run on a small battery for four to five years. It is vital for applications that only need to exchange small amounts of data periodically.

*  Unlike classic Bluetooth, **BLE remains in sleep mode constantly except for when a connection is initiated**.

* In summary, Bluetooth and Bluetooth Low Energy are used for very different purposes. Bluetooth can handle a lot of data, but consumes battery life quickly and costs a lot more. BLE is used for applications that do not need to exchange large amounts of data, and can therefore run on battery power for years at a cheaper cost.

#### ATT vs GATT

#### TABLET MODE

1. check for BLE support in the application.


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
 
 
