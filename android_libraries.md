<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Android Libraries](#android-libraries)
    - [BAAS](#baas)
    - [Architecture](#architecture)
    - [Networking](#networking)
    - [Database](#database)
    - [Debugging tools](#debugging-tools)
    - [Views](#views)
    - [Support Libraries](#support-libraries)
    - [Date and Time](#date-and-time)
    - [Typefaces](#typefaces)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

###### Legend:
 - All libraries marked with ![Recommended](./assets/recommended.png) are recommended among the similars, because they have been or are used in our projects. Nothing prevents you from using others alternatives, but is too much easy to ask for help with already known libraries.

# Android Libraries

Adding a third-party library to your project, think that you are doing a high commitment with this library. So, think twice before adding it and starting using. Some advices to choose a good library:
 
 - Open-source
   - Open-source libraries has too many advantages over closed ones to list here.
 - Focused to resolve a unique problem
   - Try to avoid all-in-one libraries, instead, choose smaller libraries to resolve each problem. Choosing a bloated library, if you face any problem, will be harder to fix or replace it.
 - Lower dex count
   - If you can't decide what library choose with a set of libraries, try to compare the dexcount of them. Because it's easy to include libraries to your project, the dexcount of your app can increase very fast, and can be a problem in a long term vision. When choosing a library, use this [tool](http://www.methodscount.com) as a comparator before including it in your project
 - Look at some Github metrics:
   - Opened Issues number (Closed issues number is a good tip too)
   - Number of stars
   - The library owner, look to peoples and companies known on the Android Community
   - Number of releases
   - Last commit date is a good indicative of active development
 - Well known libraries
 - Automated tests is a good indicator to libraries with a good quality

There are a great variety of libraries to speed up the development. Some of then that we most use are:

### BAAS
- ![Recommended](./assets/recommended.png) [Parse](https://www.parse.com/) is a simple solution as a Back-end for short projects.
- [Firebase](https://www.firebase.com/) for realtime storage, communication and synchronization.

### Architecture
- [RxJava](https://github.com/ReactiveX/RxJava) & [RxAndroid](https://github.com/ReactiveX/RxAndroid) for Reactive programming.
  - Rxjava is a library for Reactive Programming, in other words, handling asynchronous events. It is a powerful and promising paradigm, which can also be confusing since it's so different. We recommend to take some caution before using this library to architect the entire application. There are some projects done by us using RxJava and RxSwift, if you need help talk to one of these people: __Wakim Jraige__, __Marcilio Junior__ and __Vinicius Soares__.
  
  - If you have no previous experience with Rx, start by applying it only for responses from the API. Alternatively, start by applying it for simple UI event handling, like click events or typing events on a search field. If you are confident in your Rx skills and want to apply it to the whole architecture, then write Javadocs on all the tricky parts. Keep in mind that another programmer unfamiliar to RxJava might have a very hard time maintaining the project. Do your best to help them understand your code and also Rx.

- ![Recommended](./assets/recommended.png) [Otto](http://square.github.io/otto/) to reduce coupling with component communications without going crazy with Listeners and Callback hell.
- ![Recommended](./assets/recommended.png) [AndroidAnnotations](http://androidannotations.org/) to reduce the boilerplate code. Some of useful use cases are:
  - A powerful dependency injection for: views, contexts, events handlers, resources values, beans.
  - Some convenience annotations to run code on Background and in Main Thread. __But be aware about framework components (Activities, Fragments, Service and etc...) lifecycle__. During long tasks, some components may be destroyed while the task is running, so check for state before bind and result at any UI component.

   One dangerous gotcha about Background processing is scope. When you run an Background task using `@Background`, the task hold an reference to the parent Context (Check the generated code). Think about rotating the screen many times running N background tasks. Be aware of that before abusing it...
   A simple solution to this problem is to using this for short tasks. And delegate long tasks to `Services` or using and combination of `Thread Scheduler` or maybe `AsyncTasks` with `Event buses`. 
- [Dagger 2](http://google.github.io/dagger/)
  - If you don't need all magic that AA provides, Dagger 2 can be a simple solution to dependency injection without reflection.

### Networking
- ![Recommended](./assets/recommended.png) [Okhttp](http://square.github.io/okhttp/)
- ![Recommended](./assets/recommended.png) [Retrofit 1 and 2](https://github.com/square/retrofit)
- [Picasso](http://square.github.io/picasso/)
- ![Recommended](./assets/recommended.png) [Glide](https://github.com/bumptech/glide)
- ![Recommended](./assets/recommended.png) [Gson](https://code.google.com/p/google-gson/)

### Database
- ![Recommended](./assets/recommended.png) [Active Android](https://github.com/pardom/ActiveAndroid) is a well know ORM with a powerfull API. 
- [Sugar ORM](https://github.com/satyan/sugar) is a lighweight ORM with basic API and a smaller dex count.

### Debugging tools
- ![Recommended](./assets/recommended.png) [LeakCanary](https://github.com/square/leakcanary) to keep tracking over any memory leak that can be happening.
- ![Recommended](./assets/recommended.png) [Stetho](http://facebook.github.io/stetho/) is a great tool to debug some informations about app in browser, an experience closer than debugging the front-end of a web application.

  The best part of __Stetho__ is the ability to inspect the full application backstack view hierarchy, in a xml format and read-only access to views properties, and all activity's properties. It also suport the Network request inspection, for both UrlConnection and OkHttp. Another great feature is the hability to inspect SQLite databases, with the possibility to perform any query to read or manipulate the data if needed.

### Views
- [Butterknife](http://jakewharton.github.io/butterknife/) as a lightweight solution for views, resources and events handlers dependency injection.

### Support Libraries
- ![Recommended](./assets/recommended.png) [Support v4](https://developer.android.com/tools/support-library/features.html#v4)
  - Specially some elements:
    - `Fragment`
    - `FragmentActivity`
    - `ActivityCompat` (specially for `Activity` and `Fragment` Transitions and Runtime Permissions compatibility)
    - `ContextCompat`
    - `DrawableCompat`
- ![Recommended](./assets/recommended.png) [Multi Dex](https://developer.android.com/tools/support-library/features.html#multidex)
- ![Recommended](./assets/recommended.png) [AppCompat v7](https://developer.android.com/tools/support-library/features.html#v7)
  - Some elements:
    - `AppCompatActivity`
    - `AlertDialog`
    - All `AppCompat*` widgets created to allow tinting.
  - [CardView](https://developer.android.com/tools/support-library/features.html#v7-cardview)
  - [RecyclerView](https://developer.android.com/tools/support-library/features.html#v7-recyclerview)
  - [Preferences](https://developer.android.com/tools/support-library/features.html#v7-preference)
  - [Palette](https://developer.android.com/tools/support-library/features.html#v7-palette)
- ![Recommended](./assets/recommended.png) [Support Annotations](https://developer.android.com/tools/support-library/features.html#annotations)
  - Some annotations are pretty hand to prevent some mistakes:
    - `@NonNull`
    - `@Nullable`
    - `@StringRes` and all the `Res` family
    - `@CheckResult` and `@CallSuper` are very important
    - `@RequiresPermission` for the new Android M runtime permissions model.
    - `@StringDef` and all the `Def` famility to reduce `enum` usage.
    - And much more...
- ![Recommended](./assets/recommended.png) [Design Library](https://developer.android.com/tools/support-library/features.html#design)
  - Some great elements inside:
    - `FAB`
    - `AppBar`
    - `TabLayout`
    - `CoordinatorLayout` and `Behaviors API`
    - `Snackbar`
    - `TextInputLayout`
    - `SwitchCompat`
- [Custom Tabs](https://developer.android.com/tools/support-library/features.html#custom-tabs)
  - Custom tabs allows you to display any url using the Chrome 45+ engine.
  - Better than displaying an WebView inside your app, but an fallback must be supplied because Custom Tabs only works with Chrome 45+.
  - You can accelerate the page loading, its pretty handy.
  - Some sources:
    - https://developer.chrome.com/multidevice/android/customtabs
    - https://github.com/GoogleChrome/custom-tabs-client

### Date and Time
- [JodaTime](https://github.com/dlew/joda-time-android) to handle datime and time parsing and some temporal operations, without going crazy with broken Java 6 Calendar and Date API's for better Date/Time manipulation.
- ![Recommended](./assets/recommended.png) [ThreeTenABP](https://github.com/JakeWharton/ThreeTenABP) It's backport of JSR-310 (the new Java 8 Date API) to Android, as an alternative to __JodaTime__, designed for Android. Have a smaller method count (5054 of Joda Time vs 3265 of 310ABP) and an optimized timezone data loading. 

### Typefaces
- [Calligraphy](https://github.com/chrisjenx/Calligraphy) is a great library to handle the boilerplate and provides an automatic text typeface configuration for all views of a layout. It creates a wrapper over the __LayoutInflater__ to intercept View inflation. Yes, it use's Reflection to change the typeface of TextView's, but you may think about it before using.

---

For a list of other useful libraries visit [android arsenal](http://android-arsenal.com). Do also ask other Android devs here what libraries we’ve used recently as for a lot of things, what’s best changes very quickly.

If you have some interest in tests, take a look at these Standard Test Libraries

 - [Espresso](https://developer.android.com/training/testing/ui-testing/espresso-testing.html)
 - [Robolectric](http://robolectric.org/)
 - [Mockito](https://github.com/mockito/mockito)
 - JUnit
