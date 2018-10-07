### Contents
  * [Build Type and Product Flavour](#build-type-and-product-flavour)
  * []()
  
  
  
  
#### Build Type and Product Flavour

* **Build types** are for different builds of your application that are functionally similar.For example if we have a **debug** and **release** version of our app, then they're the same app, but the debug build type may contain debugging code,  more logging, etc., while the release build variant is optimized, resources are shrunk and possibly obfuscated via ProGuard. 

* build types in ```build.gradle```
```java
buildTypes {
        debug {
            applicationIdSuffix ".debug"
            debuggable true
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
```

* **Product Flavours:** With product flavors, the intent is that the app is functionally different in some way.Like a **free vs. a paid** version of your app. In such cases certain functionalities are different, hidden or added in product flavours.

```java
android {
    flavorDimensions "version"

    productFlavors {
        freeVersion {
            //select the dimension of flavor
            dimension "version"

            //configure applicationId for app published to Play store
            applicationId "com.pcc.flavors.free"

            //Configure this flavor specific app name published in Play Store
            resValue "string", "flavored_app_name", "Free Great App"

        }

        paidVersion {
            dimension "version"
            applicationId "com.pcc.flavors.paid"
            resValue "string", "flavored_app_name", "Paid Great App"
        }
    }
}
```

* The combination of Build Type and Flavor is known as **Build Variant**. For example, for above build types (debug and release) and product flavours (free and paid versions), build variants can be ```freeDebug```, ```freeRelease```, ```paidDebug```, ```paidRelease```.
