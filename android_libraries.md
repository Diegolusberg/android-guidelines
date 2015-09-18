# Android Libraries

There are a great variety of libraries to speed up the development. Some of then that we most use are:

### BAAS
- [Parse](https://www.parse.com/) is a simple solution as a Back-end for short projects.
- [Firebase](https://www.firebase.com/) for realtime communication.

### Architecture
- [RxJava](https://github.com/ReactiveX/RxJava) & [RxAndroid](https://github.com/ReactiveX/RxAndroid) for Reactive programming.
  - Rxjava is a library for Reactive Programming, in other words, handling asynchronous events. It is a powerful and promising paradigm, which can also be confusing since it's so different. We recommend to take some caution before using this library to architect the entire application. There are some projects done by us using RxJava, if you need help talk to one of these people: Timo Tuominen, Olli Salonen, Andre Medeiros, Mark Voit, Antti Lammi, Vera Izrailit, Juha Ristolainen. We have written some blog posts on it: [[1]](http://blog.futurice.com/tech-pick-of-the-week-rx-for-net-and-rxjava-for-android), [[2]](http://blog.futurice.com/top-7-tips-for-rxjava-on-android), [[3]](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754), [[4]](http://blog.futurice.com/android-development-has-its-own-swift).
  - If you have no previous experience with Rx, start by applying it only for responses from the API. Alternatively, start by applying it for simple UI event handling, like click events or typing events on a search field. If you are confident in your Rx skills and want to apply it to the whole architecture, then write Javadocs on all the tricky parts. Keep in mind that another programmer unfamiliar to RxJava might have a very hard time maintaining the project. Do your best to help them understand your code and also Rx.

- [Otto](http://square.github.io/otto/) to reduce coupling with component communications without going crazy with Listeners and Callback hell.
- [AndroidAnnotations](http://androidannotations.org/) to reduce the boilerplate code. Some of useful use cases are:
  - A powerful dependency injection for: views, contexts, events handlers, resources values, beans.
  - Some convenience annotations to run code on Background and in Main Thread. __But be aware about framework components (Activities, Fragments, Service and etc...) lifecycle__. During long tasks, some components may be destroyed while the task is running, so check for state before bind and result at any UI component.

   One dangerous gotcha about Background processing is scope. When you run an Background task using `@Background`, the task hold an reference to the parent Context (Check the generated code). Think about rotating the screen many times running N background tasks. Be aware of that before abusing it...
   A simple solution to this problem is to using this for short tasks. And delegate long tasks to `Services` or using and combination of `Thread Scheduler` or maybe `AsyncTasks` with `Event buses`. 
- [Dagger 2](http://google.github.io/dagger/)
  - If you don't need all magic that AA gives, Dagger 2 can be a simple solution to dependency injection without reflection.

### Networking
- [Okhttp](http://square.github.io/okhttp/)
- [Retrofit 1 and 2](https://github.com/square/retrofit)
- [Picasso](http://square.github.io/picasso/)
- [Glide](https://github.com/bumptech/glide)
- [Gson](https://code.google.com/p/google-gson/)

### Debugging tools
- [LeakCanary](https://github.com/square/leakcanary) to keep tracking over any memory leak that can be happening.

### Views
- [Butterknife](http://jakewharton.github.io/butterknife/) as a lightweight solution for views, resources and events handlers dependency injection.

### Support Libraries
- [Support v4](https://developer.android.com/tools/support-library/features.html#v4)
  - Specially some elements:
    - `Fragment`
    - `FragmentActivity`
    - `ActivityCompat` (specially for `Activity` and `Fragment` Transitions and Runtime Permissions compatibility)
    - `ContextCompat`
    - `DrawableCompat`
- [Multi Dex](https://developer.android.com/tools/support-library/features.html#multidex)
- [AppCompat v7](https://developer.android.com/tools/support-library/features.html#v7)
  - Some elements:
    - `AppCompatActivity`
    - `AlertDialog`
    - All `AppCompat*` widgets created to allow tinting.
  - [CardView](https://developer.android.com/tools/support-library/features.html#v7-cardview)
  - [RecyclerView](https://developer.android.com/tools/support-library/features.html#v7-recyclerview)
  - [Preferences](https://developer.android.com/tools/support-library/features.html#v7-preference)
  - [Palette](https://developer.android.com/tools/support-library/features.html#v7-palette)
- [Support Annotations](https://developer.android.com/tools/support-library/features.html#annotations)
  - Some annotations are pretty hand to prevent some mistakes:
    - `@NonNull`
    - `@Nullable`
    - `@StringRes` and all the `Res` family
    - `@CheckResult` and `@CallSuper` are very important
    - `@RequiresPermission` for the new Android M runtime permissions model.
    - `@StringDef` and all the `Def` famility to reduce `enum` usage.
    - And much more...
- [Design Library](https://developer.android.com/tools/support-library/features.html#design)
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

### Utilities
- [JodaTime](https://github.com/dlew/joda-time-android) to handle datime and time parsing and some temporal operations, without going crazy with broken Java 6 Calendar and Date API's.

For a list of other useful libraries visit [android arsenal](http://android-arsenal.com). Do also ask other Android devs here what libraries we’ve used recently as for a lot of things, what’s best changes very quickly.

If you have some interest in tests, take a look at these Standard Test Libraries

 - [Espresso](https://developer.android.com/training/testing/ui-testing/espresso-testing.html)
 - [Robolectric](http://robolectric.org/)
 - [Mockito](https://github.com/mockito/mockito)
 - JUnit 
