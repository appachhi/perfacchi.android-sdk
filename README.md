# Perfachhi Performance SDK

A simple and easy to use performance monitoring and instrumentation library for
Android

Following functionalities are supported
- [CPU Usage](#cpu-usage)
- [Memory Usage](#memory-usage)
- [GC Run Information](#gc-run)
- [Network Usage](#network-usage)
- [FPS](#fps)
- [Memory Leak Information](#memory-leak)
- [Screen Transition Details](#screen-transition)
- [Method Tracing](#method-tracing)
- [Network Call Details](#network-call)

# Download

Add the following snippet in your project level `build.gradle` file

```gradle
buildscript {
    repositories {
        maven {url 'https://jitpack.io'}
    }
    dependencies {
        classpath 'com.github.appachhi.android-sdk:appachhiplugin:v0.1-alpha"
    }
}

allprojects {
    repositories {
       maven {url 'https://jitpack.io'}
    }
}
```
Add the following snippet in your app level `build.`

```gradle
dependencies {
    implementation "com.github.appachhi.android-sdk:appachhisdk:v0.1-alpha"
}

```


## CPU Usage
---
SDK computes the cpu usage for the device as well as the for app.The value will range from 0 - 100 % where 0 means cpu usage is least and 100 means cpu usage is at max. It is automatically computed where the SDK is integrated with the app. This is enabled for devices below Android Oreo(Android 8.0)

## Memory Usage
---
SDK computes the detailed memory usage for the app as well the device. Computed values are in bytes.Some value are available only after Android M(Android 6.0). This is also computed automatically once the SDK is integrated

Following information are extracted as part of memory usage:-
- Threshold Memory
- Total PSS Memory
- Total Private Dirty Memory
- Total Share Dirty Memory
- Java Heap Memory
- Native Heap
- Code Memory (Android M and above)
- Stack Memory (Android M and above)
- Graphic Memory (Android M and above)
- Other Memory (Android M and above)
- System Resource Memory (Android M and above)
- Total Swap Memory (Android M and above)

## GC Run
---
SDK  gives detailed analysis of the GC run. This is computed automatically once the SDK is integrated.

Following information are extracted aspart of GC Run :-
-  Reason for running GC
-  Name of the GC
-  Object Freed
-  Object Freed Size
-  AllocSpace Object Free
-  AllocSpace Object Fred Size
-  Large Object Free Percentage
-  Large Object Total Size
-  Pause Time for GC(Main Thread Pause)
-  GC Running Time

## Network Usage
--
SDK computes the network usage for the app happened during a single session. This can contain data sent over a http connection or web sockets. Values are in bytes

Following information are extracted as part of Network Usage :-
-  Data Sent
-  Data Received

## FPS
--
Frame per second is a very important metric in Android. Ideally an good app should be able to render 60 time per seccond.If It goes below 60 fps, the UI will feel janky. SDK computes this automatically once it is integrated with the app. The values can be anything between 0 to 60.

## Memory Leak
---
Memory leak occurs when developer create a memory in heap and forget to delete it. Memory leak is very important factor for crash in any app. An good performning app should have ver less or absolutely no memory leak.

Follwoing information are extracted as part of Memory Leak :-
- className(Name of the class which is leaked)
- Leak Trace(Trace showing which line in the source caused a leak)

## Screen Transition
---
Screen transition is the time take to switch from one screen to another.SDK computes this in an automatic mode as well as manual mode

### Automatic
It is as simple as annotating the activity for which screen transition time
needs to be computed.For this to work, you need to make sure you have added the `appachhiplugin` to your classpath

```java
import com.appachhi.sdk.instrument.transition.AutoActivityTrace;

@AutoActivityTrace
class MyActivity extends AppCompatActivity{
    // Your code
}
```

### Manual(Recommended)

As of now this is the recommended approach because of limitation of automatic
method. We will be improving it over the period of time.
```java
import com.appachhi.sdk.instrument.transition.ScreenTransitionManager;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // Default way
        ScreenTransitionManager.getInstance().beginTransition(this);

        // If there is mutliple instance of the activity running at the
        // same time or if you want to give a custom name
        ScreenTransitionManager.getInstance().beginTransition(this,"Your Screen Name");

        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    protected void onResume() {
        super.onResume();

        // Default way
        ScreenTransitionManager.getInstance().endTransition(this);

        // Use this if you have  give the custom name as shown above.Also keep
        // in mind to give the same name which was given during transition start
        ScreenTransitionManager.getInstance().endTransition(this,"Your Screen Name");
    }
}

```

## Method Tracing
---
Method trace is feature where developer can easily compute the time taken by the method to execute. This can be done by annotating any method as show.This will only work if `appachhiplugin` is added to your classpath

```java
import com.appachhi.sdk.instrument.trace.Trace;

public class MyClass{
     @Trace(name = "Trace Name")
     public void myMethod(){
       //  Implementation
     }
}
```
## Network Call
---
Network call detail are automaticallu computed by the SDK. 
It will extract following information for any network :-
- url 
- Http Method
- Content Type(As per response header)
- Request Content Length
- Response Code
- Duration
- ThreadName(Thread on which request was executed)

*Note* - Currently only OkHttp client is supported for automatically capturing the Network call details
