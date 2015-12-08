# Chronometro
<img src="https://github.com/dilah/chronometro/blob/master/stopwatch-34108_640.png" width="100">

The *Chronometro* plugin is a benchmarking plugin for Android Studio. 

It provides the information on loading times of any kind of functionality that may be found in the Android application.
Benchmarking is not a trivial task on Android when taking into account the life cycle of activities and fragments. Thus, this plugin appears to be very handy when one needs to track the completion of the network calls in relation to that.

The plugin provides the annotation that is used to mark the methods that are going to be tracked. At the same time the checkpoints are provided if one would like to have a report on loading time in a current point of execution without stopping the timer.

*Chronometro*  is based on Aspect Oriented Programming for Android and uses the [AspectJ library](https://eclipse.org/aspectj/).

*Chronometro*  finds its inspiration in:
* Jake Wharton's plugin [Hugo](https://github.com/JakeWharton/hugo);
* Fernando Cejas guide on [AAOP](http://fernandocejas.com/2014/08/03/aspect-oriented-programming-in-android/);
* [Bintray and packaging](http://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en) by Sittiphol Phanvilai 


```java
@LogUILoadingTime(state = LogUILoadingTime.START, name = "Fragment1")
@Override
public void onAttach(Activity activity) {
  super.onAttach(activity);
  // some code here
}
    
@LogUILoadingTime(state = LogUILoadingTime.END, name = "Fragment1")
@Override
public void onDataSuccess(Data data) {
  // some code here
}
```
    
Example of result logs:
```java
Performance Measure Loading Times --> Fragment1 started
Performance Measure Loading Times --> Fragment1 created --> [2000ms]
```

One may define several timers y providing a different name value in the annotation.
Here are the two states that are available for getting the loading time right before the method runs 
(LogUILoadingTime.CHECKPOINT_START) 
and one right after 
(LogUILoadingTime.CHECKPOINT_END)

```java    
// logging the time until after the view has been created
@LogUILoadingTime(state = LogUILoadingTime.CHECKPOINT_END, name = "Fragment1")
@Override
public void onViewCreated(View view, @Nullable Bundle savedInstanceState) {
  super.onViewCreated(view, savedInstanceState);
  // some code here
}
    
// logging the time until before the fetching of data
@LogUILoadingTime(state = LogUILoadingTime.CHECKPOINT_START, name = "Fragment1")
@Override
private void fetchData() {
  // some code here
}
```
    
Example of logs at the checkpoints:
```java
  Performance Measure Loading Times --> Fragment1 checkpoint after method onViewCreated --> [68ms]
  Performance Measure Loading Times --> Fragment1 checkpoint before method fetchData --> [108ms]
```
  
To integrate the plugin into your project, please add the following code to your Gradle file:
```groovy
buildscript {
  repositories {
    mavenCentral()
  }

dependencies {
  classpath 'se.tv4:chronometro-plugin:0.4.0'
  }
}

apply plugin: 'com.android.application'
apply plugin: 'se.tv4.benchmark'
```

# License
```java
Copyright 2015, TV4 AB.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```