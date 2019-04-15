# bugasura-perf-sdk

### Integration

Appachhi SDK is build with two binaries (SDK and Plugin)

#### SDK
It does much of the tasks like monitoring various metric and saving the information to the database. You can find details [here](https://github.com/appachhi/android-sdk).
In order to integrate the SDK to an existing project, you can add the `sdk.aar` into the `src/main/libs/` directory in your `app` module. 

Also update the project level `build.gradle` file  as follows

```gradle
allprojects {
    repositories {
        google()
        jcenter()
        flatDir {
            dirs 'src/main/libs'
        }
    }
}
```

#### Plugin
In order to add  integrate the plugin, you need to add the `plugin.jar` file in top level `libs` directory.If folder doesn't exist,then create one.

Now in you project level `build.gradle` add the following lines

```gradle
buildscript {
    dependencies {
        classpath files("libs/plugin.jar")
    }
}
```
Update the app level `build.gradle` as follow

```gradle
apply plugin: 'com.appachhi.plugin'
```

### Other information

- SDK will automatically be initialized as soon as the app launches and it will start collecting data

- In order to gather information regarding method trace, you can annotate a method as show below
```java
    @Trace(name = "Your Trace Name")
    public void foo(){
    // Any code
    }
```
- As soon as the app launches, you will be asked to grant permission to show an overlay. If you allow then an overlay will be show where you can see the captured data

- All the data are currently saved in database too 
