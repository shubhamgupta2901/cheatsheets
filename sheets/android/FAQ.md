### Contents
  * [Modules](modules)
  * [Building android app](#building-android-app)
  * [Java Bytecode and Dalvik bytecode](#java-bytecode-and)
  * [Build Type and Product Flavour](#build-type-and-product-flavour)
  * [Android Manifest](#android-manifest)
  * [build.gradle]
  
#### Java Bytecode and Dalvik Bytecode
* Each Java class is compiled into Java bytecode. In result, a .class file is created; Java bytecode which is a set of instructions for the Java virtual machine

#### Modules
* A module is a collection of source files that allow us to divide our project into seperate units of functionality. One project can have one or many modules and one module may use another module as a dependency. Each module can be independently built, tested, and debugged.

* Additional modules are often useful when creating code libraries within your own project.

* **Android app module**:  When you create a new project, the default module name is "app".This provides a container for our app's source code, resource files, and app level settings such as the module-level build file and Android Manifest file.

* The ```settings.gradle``` file, located in the root project directory, tells Gradle which modules it should include when building your app. 

```java
include ':app', ':uicomponent-materialdialogs', ':uicomponent-conversationwindow', ':uicomponent-waitingdots', ':uicomponent-textdrawable', ':uicomponent-progresswheel', ':uicomponent-shimmer', ':uicomponent-photoview'
```

#### Build Process in android

* By build process we mean that using certain tool which will convert our **android project into an Android Application Package(APK)**.APKs are packages that are readable by android devices, and once we install them we can use the application.
* Normally our android project contains following files: 
  1. App Module:
    * Source Code
    * Resource Files
    * AIDL Files
  2. Dependencies:
    * Local library modules
    * AAR Libraries (Android Archives)
    * JAR Libraries (Java Archives)

* Build Process: 
  1. First step involves compiling the **resources folder** (/res) using the AAPT (android asset packaging tool) tool. These are compiled to a single class file called **R.java**. This is a class that just contains constants. 
  2. The compilers convert the **source code** into DEX (Dalvik Executable) files, which has the Dalvik bytecode(Different from Java bytecode) that runs on Android devices. Output is **classes.dex**.
  3. Same two steps are also implemented for dependencies.
  4. Once this is done we have DEX File(s) and Compiled Resources.
  5. Android APK Packager takes these files, signs it using a debug or release keystore, optimizes the app to use less memory and creates an APK File.

* The final step involves the android apkbuilder which takes all the input and builds the apk (android packaging key) file.
  
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

#### Android Manifest
* The manifest file describes essential information about your app to the Android build tools, the Android operating system, and Google Play.

* Essential information includes: 
 * **Application package name**:
 * **The components of the app**: which include all activities, services, broadcast receivers, and content providers.
 * **The permissions that the app needs**: 
 * **The hardware and software features the app requires**:

* Every android project must have an ```AndroidManifest.xml``` file (with precisely that name) at the root of the project source set. 

* Every module has one Android Manifest file.
