 #### Type of dependencies that can be added in app's build.gradle:

```java
compile project(":mylibrary")
```

It is a dependency on a local library module.
It requires the build system to compile the library module with your app module and include the resulting AAR file in your APK.

```java
compile fileTree(dir: 'libs', include: ['*.jar'])
```
Local binary dependency.
Because Gradle reads paths relative to the build.gradle file, this tells the build system to add all JAR files inside your project's module_name/libs/ directory as dependencies.

```java
compile 'com.example.android:app-magic:12.3'
```
Dependency on a remote binary.
Shorthand for: 
```java
compile group: 'com.example.android', name: 'app-magic', version: '12.3'
```
 
#### Where does Android Studio fetch the library from?
Android Studio downloads the library from Maven Repository Server. (Apache Maven is a tools developed by Apache provides a file server to distribute the libraries). Basically there are just 2 standard servers used for host the libraries for Android such as jcenter and Maven Central.

* jCenter
jcenter is a Maven Repository hosted by bintray.com.To use jcenter in your project, you have to define the repository like below in project's build.gradle file:

```java
allprojects {
    repositories {
        jcenter()
    }
}
```

* Maven Central
Maven Central is a Maven Repository hosted by sonatype.org.To use Maven Central in your project, you have to define the repository like below in project's build.gradle file:

```java
allprojects {
    repositories {
        mavenCentral()
    }
}
```

Both jcenter and Maven Central are standard android library repositories but they are hosted at completely different place.

Apart from those two standard servers, we are also able to define the specific Maven Repository Server ourselves in case we use a library from some developers who want to host their libraries on their own server. 

Declare library like: 
```java
repositories {
    maven { url 'https://maven.fabric.io/public' }
}
```

Use library like:
```java
dependencies {
    compile 'com.crashlytics.sdk.android:crashlytics:2.2.4@aar'
}
```

#### Understanding jcenter and Maven Central
Both jcenter and Maven Central are the repositories having the same duty: Hosting Java/Android libraries. It is a developers' choice to upload their libraries to which one or may be both.
At first, Android Studio chose Maven Central as a default repository. Due to security concerns and its developerfriendly nature, Android Studio switched the default repository to jcenter instead. If you create a new project from latest version of Android Studio, jcenter() would be automatically defined instead of mavenCentral().jcenter delivers library through CDN which means that developer could enjoy the faster loading experience.

#### How does gradle pull a library from Repository?
When we write the following in a module's build.gradle:
```java
 compile 'com.example.android:app-magic:12.3'
 ```
Gradle is looking for three things: [GROUP_ID:ARTIFACT_ID:VERSION]
Here GROUP_ID is com.example.android
ARTIFACT_ID is app-magic
VERSION is 12.3
 
GROUP_ID defines the name of library's group.Generally we name it with developer's package name and then follow with the name of library's group.And then define the real name of the library in ARTIFACT_ID. For VERSION, there is nothing but a version number. Although it could be any text but I suggest to set it in x.y.z format.

Dependencies from Square look like this:
```java
dependencies {
  compile 'com.squareup:otto:1.3.7'
  compile 'com.squareup.picasso:picasso:2.5.2'
  compile 'com.squareup.okhttp:okhttp:2.4.0'
  compile 'com.squareup.retrofit:retrofit:1.9.0'
}
```

When we add dependencies like above,Gradle will ask Maven Repository Server that does the library exist if yes gradle will get a path of the requested library which mostly in the form of GROUP_ID/ARTIFACT_ID/VERSION_ID, for example, you could find library files of com.squareup.picasso:picasso:2.5.2 from http://jcenter.bintray.com/com/squareup/picasso/picasso/2.5.2 OR sonatype too.
And then Android Studio would download those files to our machine and compile with our project per our request.A library pulled from repository is nothing special but jar or aar files hosted on repository server. 
It is just like downloading those files ourselves, copy and compile everything along with our project. But with dependency system available on gradle, we don't have to do anything but just the compile statement.


#### Understanding JAR & AAR

* A JAR (Java ARchive) 
is a package file format typically used to aggregate many Java class files and associated metadata and resources (text, images, etc.) into one file for distribution.JAR files are archive files that include a Java-specific manifest file. They are built on the ZIP format and typically have a .jar file extension.

* An AAR(Android ARchive) 
file is developed on top of jar file. Unlike JAR files, AAR files can contain Android resources and a manifest file, which allows you to bundle in shared resources like layouts and drawables in addition to Java classes and methods. It was invented because something Android Library needs to be embedded with some Android-specific files like AndroidManifest.xml, Resources, Assets or JNI which are out of jar file's standard. Basically it is a normal zip file just like jar one but with different file structure. jar file is embedded inside aar file with classes.jar name.

When we create our own library in Android Studio, instead of compiling into an APK that runs on a device, an Android library compiles into an Android Archive (AAR) file that you can use as a dependency for an Android app module. 


#### How to upload your library to jcenter
Two step process: Create an AAR file and upload it to  http://jcenter.bintray.com repository.

![alt text](https://inthecheesefactory.com/uploads/source/jcenter/steps_1.png " ")

* Sign in to bintray, Create your new Repository <repo-name> of type Maven.No licenses are mandatory.
* Once you have a repository repo_name, Add a new package <package-name> inside your repository.
* License will be mandatory for creating a package, Apache-2.0 is ideal for Open Source projects as it allows others to reproduce and distribute copies of your Work with or without modifications.
* In version control, mention the remote repo of your source code. All other options are optional. 
* We now have our own maven repository on Bintray, and we will upload our library in this package. This is only the jcenter repo for our library. You may also need to upload it to Maven Center as lot of devs still use this repository.
* Create a new project in Android Studio. The application will hav two modules: app module, which will have the code to demonstrate the usage of library and library module which will contain the source code of library.
* At this stage we will have three gradle files in our project: top level build.gradle for our project, app module build.gradle and library module build.gradle.
* Having a VCS integration with your project at this stage will be a good idea.
* In your top level build.gradle, insert following dpendecies:

```java
dependencies {
        classpath 'com.android.tools.build:gradle:2.3.3'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:2.0'
      
    }

```
* In your local.properties, insert follwoing lines:
```java
bintray.user=YOUR_BINTRAY_USERNAME
bintray.apikey=YOUR_BINTRAY_API_KEY
```
* YOUR_BINTRAY_API_KEY can be fetched from your bintray profile's Edit Profile Section.
* In your library module's build.gradle file add the following block of code:

```java
ext {
    PUBLISH_GROUP_ID = 'com.achilles.tracker' // This should be your package name of library in Androoid Studio
    PUBLISH_ARTIFACT_ID = 'location-tracker' // This is mandatorily your package name in bintray's repo
    PUBLISH_VERSION = '1.0.0' //The version of library you ar at.
}
```

* At the very end of your module's build.gradle file add the following line: 

```java
apply from: 'https://raw.githubusercontent.com/blundell/release-android-library/master/android-release-aar.gradle'
```
* Everything is set now. In Android Studio, In Right side's Gradle navigation, Go to Your library's module. On expanding your library module, Go to Others->zip release. Let it build. 

* The output zip folder contains, AAR Files, JavaDoc ,POM files. TO find it, change your navigation view to Project, go to your library module->build. You will find a zip folder by your-package-name.x.y.z.zip file, which is your folder to be uploded to bin tray. 

* Once uploaded to bintray projects, you will need to add this to jcenter. It takes around 2-3 hours for your library to get approved, Once done others can use your library as a gradle dependency.

 
