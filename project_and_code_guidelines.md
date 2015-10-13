<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [TL;DR](#tldr)
- [0. Language](#0-language)
- [1. Project guidelines](#1-project-guidelines)
  - [1.1 Project structure](#11-project-structure)
  - [1.2 Package structure](#12-package-structure)
  - [1.3 File naming](#13-file-naming)
    - [1.3.1 Class files](#131-class-files)
    - [1.3.1 Resources files](#131-resources-files)
      - [1.3.1.1 Drawable files](#1311-drawable-files)
      - [1.3.1.2 Layout files](#1312-layout-files)
      - [1.3.1.3 Avoid deep layout hierarchy](#1313-avoid-deep-layout-hierarchy)
      - [1.3.1.4 Use designtime attributes](#1314-use-designtime-attributes)
      - [1.3.1.5 Styles](#1315-styles)
      - [1.3.1.6 Menu files](#1316-menu-files)
      - [1.3.1.7 Values files](#1317-values-files)
  - [1.4 XML style rules](#14-xml-style-rules)
    - [1.4.1 Use self closing tags](#141-use-self-closing-tags)
    - [1.4.2 Resources naming](#142-resources-naming)
      - [1.4.2.1 ID naming](#1421-id-naming)
      - [1.4.2.2 Strings](#1422-strings)
      - [1.4.2.3 Styles and Themes](#1423-styles-and-themes)
    - [1.4.3 Attributes ordering](#143-attributes-ordering)
  - [1.5 Dependencies](#15-dependencies)
- [2 Code guidelines](#2-code-guidelines)
  - [2.1 Java language rules](#21-java-language-rules)
    - [2.1.1 Don't ignore exceptions](#211-dont-ignore-exceptions)
    - [2.1.2 Don't catch generic exception](#212-dont-catch-generic-exception)
    - [2.1.3 Don't use finalizers](#213-dont-use-finalizers)
    - [2.1.4 Fully qualify imports](#214-fully-qualify-imports)
  - [2.2 Java style rules](#22-java-style-rules)
    - [2.2.1 Fields definition and naming](#221-fields-definition-and-naming)
    - [2.2.3 Treat acronyms as words](#223-treat-acronyms-as-words)
    - [2.2.4 Use spaces for indentation](#224-use-spaces-for-indentation)
    - [2.2.5 Use standard brace style](#225-use-standard-brace-style)
    - [2.2.6 Use standard Java annotations](#226-use-standard-java-annotations)
    - [2.2.7 Limit variable scope](#227-limit-variable-scope)
    - [2.2.8 Order import statements](#228-order-import-statements)
    - [2.2.9 Logging guidelines](#229-logging-guidelines)
    - [2.2.10 Class member ordering](#2210-class-member-ordering)
    - [2.2.11 Parameter ordering in methods](#2211-parameter-ordering-in-methods)
    - [2.2.12 String constants, naming and values](#2212-string-constants-naming-and-values)
    - [2.2.14 Arguments in Fragments and Activities](#2214-arguments-in-fragments-and-activities)
    - [2.2.15 Line length limit](#2215-line-length-limit)
      - [2.2.15.1 Line-wrapping strategies](#22151-line-wrapping-strategies)
    - [2.2.16 RxJava chains styling](#2216-rxjava-chains-styling)
  - [2.3 Tests style rules](#23-tests-style-rules)
    - [2.3.1 Unit tests](#231-unit-tests)
    - [2.3.2 Espresso tests](#232-espresso-tests)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

######Legend:
 - ![Required](./assets/required.png) means that a section is required.
 - ![Optional](./assets/optional.png) means that a section is optional.

# TL;DR

The most important sections are:

 - [1.3 File naming](#13-file-naming)
 - [1.4.2 Resources naming](#142-resources-naming)
 - [1.5 Dependencies](#15-dependencies)
 - [2.2 Java style rules](#22-java-style-rules)

# 0. Language
![Required](./assets/required.png)

All code must be written in english language. It's a sin if you write half portuguese and half english code. This rule doesn't apply to application strings that are visible to the users.

# 1. Project guidelines

## 1.1 Project structure 

New projects should follow the Android Gradle project structure that is defined on the [Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure).

Old structure:

```
old-structure
├─ assets
├─ libs
├─ res
├─ src
│  └─ com/futurice/project
├─ AndroidManifest.xml
├─ build.gradle
├─ project.properties
└─ proguard-rules.pro
```

New structure:

```
new-structure
├─ library-foobar
├─ app
│  ├─ libs
│  ├─ src
│  │  ├─ androidTest
│  │  │  └─ java
│  │  │     └─ com/futurice/project
│  │  └─ main
│  │     ├─ java
│  │     │  └─ com/futurice/project
│  │     ├─ res
│  │     └─ AndroidManifest.xml
│  ├─ build.gradle
│  └─ proguard-rules.pro
├─ build.gradle
└─ settings.gradle
```

## 1.2 Package structure
![Required](./assets/required.png)

```
java
└─ br/com/packagename
   ├─ fragment
   ├─ activity
   ├─ dialog
   ├─ adapter
   ├─ service/controller
   ├─ api (for any api that you consume)
   │  ├─ youtube (optional)
   │  ├─ amazon (optional)
   │  ├─ twitter (optional)
   │  └─ instagram (optional)
   ├─ receiver
   ├─ model
   ├─ rx/bus/dagger/etc... (or any package for components created for any sdk/framework or architecture library) 
   ├─ view (for any Custom View that receives any model object and bind the data to it's children, e.g: UserAvatarView, PostView, etc...)
   ├─ widget (for any widget that can be reused, e.g: FancyButton, CustomFAB, etc...)
   └─ util
```

## 1.3 File naming

### 1.3.1 Class files
![Required](./assets/required.png)

Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase). 

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### 1.3.1 Resources files
![Required](./assets/required.png)

Resources file names are written in __lowercase_underscore__. 

#### 1.3.1.1 Drawable files
![Required](./assets/required.png)
 
Naming conventions for drawables:

| Asset Type   | Prefix            |		Example                  |
|------------- | ----------------- |---------------------------- |
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	           | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          | 
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	           | `ic_star.png`               |
| Menu         | `menu_	`          | `menu_submenu_bg.9.png`     |
| Notification | `notification_`   | `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

Naming conventions for icons (taken from [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):

| Asset Type                      | Prefix             | Example                      |
| ------------------------------- | ------------------ | ---------------------------- | 
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

Naming conventions for selector states:

| State	       | Suffix          | Example                     |
|------------- |---------------- |---------------------------- |
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.3.1.2 Layout files
![Required](./assets/required.png)

Layout files should match the name of the Android components that they are intended for but moving the top level component name to the beginning. For example, if we are creating a layout for the `SignInActivity`, the name of the layout file should be `activity_sign_in.xml`.

| Component         | Class Name             | Layout Name                                        |
| ----------------- | ---------------------- | -------------------------------------------------- |
| Activity          | `UserProfileActivity`  | `activity_user_profile.xml`                        |
| Fragment          | `SignUpFragment`       | `fragment_sign_up.xml`                             |
| Dialog            | `ChangePasswordDialog` | `dialog_change_password.xml`                       |
| ListView item     | ---                    | `list_item_person.xml`                             |
| RecyclerView item | ---                    | `list_item_person.xml`                             |
| Pager item        | ---                    | `pager_item_person.xml`                            |
| GridView item     | ---                    | `grid_item_person.xml`                             |
| Partial layout    | ---                    | `include_stats_bar.xml` or `partial_stats_bar.xml` |
| Custom View layout| `UserView`             | `view_user.xml`                                    |

#### 1.3.1.3 Avoid deep layout hierarchy
![Optional](./assets/optional.png)

Avoid a deep hierarchy of views. Sometimes you might be tempted to just add yet another LinearLayout, to be able to accomplish an arrangement of views. This kind of situation may occur:

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >

    <RelativeLayout
        ...
        >

        <LinearLayout
            ...
            >

            <LinearLayout
                ...
                >

                <LinearLayout
                    ...
                    >
                </LinearLayout>

            </LinearLayout>

        </LinearLayout>

    </RelativeLayout>
    
</LinearLayout>
```

Even if you don't witness this explicitly in a layout file, it might end up happening if you are inflating (in Java) views into other views.

A couple of problems may occur. You might experience performance problems, because there are is a complex UI tree that the processor needs to handle. Another more serious issue is a possibility of StackOverflowError.

Therefore, try to keep your views hierarchy as flat as possible: learn how to use RelativeLayout, how to optimize your layouts and to use the <merge> tag.

Another recomendation is to __NOT USE__ `RelativeLayout` as an wrapper to add some padding or backgroud or anything. He is very complex just to wrap a view (2 layout passes to render content). Use `FrameLayout` instead.

#### 1.3.1.4 Use designtime attributes
![Optional](./assets/optional.png)

Rather than hard coding `android:text`, consider using [Designtime](http://tools.android.com/tips/layout-designtime-attributes) attributes (e.g: `tools:text`, `tools:background`, `tools:minHeight`, `tools:src` and etc...) available for Android Studio. There are some limitations, but major attributes can be used. It's very helpfull for prototyping and layout.

#### 1.3.1.5 Styles
![Required](./assets/required.png)

As a rule of thumb, attributes `android:layout_****` should be defined in the layout XML, while other attributes `android:****` should stay in a style XML. This rule has exceptions, but in general works fine. The idea is to keep only layout (positioning, margin, sizing) and content attributes in the layout files, while keeping all appearance details (colors, padding, font) in styles files.

The exceptions are:

- `android:id` should obviously be in the layout files
- `android:orientation` for a LinearLayout normally makes more sense in layout files
- `android:text` should be in layout files because it defines content
- Sometimes it will make sense to make a generic style defining `android:layout_width` and `android:layout_height` but by default these should appear in the layout files

Use styles. Almost every project needs to properly use styles, because it is very common to have a repeated appearance for a view. At least you should have a common style for most text content in the application, for example:

```xml
<style name="ContentText">
    <item name="android:textSize">@dimen/font_normal</item>
    <item name="android:textColor">@color/basic_black</item>
</style>
```

Applied to `TextView`:

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/price"
    style="@style/ContentText"
    />
```

You probably will need to do the same for buttons, but don't stop there yet. Go beyond and move a group of related and repeated `android:****` attributes to a common style.

Split a large style file into other files. You don't need to have a single `styles.xml` file. Android SDK supports other files out of the box, there is nothing magical about the name styles, what matters are the XML tags `<style>` inside the file. Hence you can have files `styles.xml`, `styles_home.xml`, `styles_item_details.xml`, `styles_forms.xml`. Unlike resource directory names which carry some meaning for the build system, filenames in `res/values` can be arbitrary.

We highly suggest a hierarchy naming convention, just like:

```xml
<style name="BaseAppTheme" parent="@style/Theme.AppCompat.Light.NoActionBar">
    <item name="colorAccent">@color/accent</item>
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primary_dark</item>
</style>

<style name="AppTheme" parent="@style/BaseAppTheme">
    <!-- Any other rule that can be overrided on other qualifier -->
</style>

<style name="Card" parent="@style/CardView">
    <!-- Any rule for AppTheme.Card -->
</style>

<style name="CardRed" parent="@style/AppTheme.Card">
    <item name="cardBackground">@color/md_red_500</item>
</style>
```

__or maybe__ with underscore's

```xml
<style name="Base_AppTheme" parent="@style/Theme.AppCompat.Light.NoActionBar">
    <item name="colorAccent">@color/accent</item>
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primary_dark</item>
</style>

<style name="AppTheme" parent="@style/Base_AppTheme">
    <!-- Any other rule that can be overrided on other qualifier -->
</style>

<style name="Card" parent="@style/CardView">
    <!-- Any rule for AppTheme.Card -->
</style>

<style name="Card_Red" parent="@style/Card">
    <item name="cardBackground">@color/md_red_500</item>
</style>
```

__Be careful__ with dot notation as separators, they mean implicity inheritance for styles, this can lead to some mistakes:

```xml
<style name="Card" parent="@style/CardView">
    <!-- Any rule for AppTheme.Card -->
</style>

<style name="Card.Red">
    <item name="cardBackground">@color/md_red_500</item>
</style>

<!-- is the same than -->

<style name="CardRed" parent="@style/Card">
    <item name="cardBackground">@color/md_red_500</item>
</style>

<!-- but is different than -->

<style name="Card.Red" parent="@style/CardBlue">
    <item name="cardBackground">@color/md_red_500</item>
</style>
```

The attribute `parent` means explicity inheritance, and `.` means implicitly inheritance. And explicitly inheritance always is selected over implicitly, there is __no merge__ between attributes.

This video [Using Styles and Themes Without Going Crazy](https://www.youtube.com/watch?v=Jr8hJdVGHAk) of Daniel Lew will help alot with some tips about the power of styles.

#### 1.3.1.6 Menu files
![Required](./assets/required.png)

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be use in the `UserActivity`, then the name of the file should be `activity_user.xml`

A good practise is to not include the word `menu` as part of the name because these files are already located in directory called menu.

#### 1.3.1.7 Values files
![Required](./assets/required.png)

Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

## 1.4 XML style rules

### 1.4.1 Use self closing tags
![Optional](./assets/optional.png)

When an XML element doesn't have any content, you __must__ use self closing tags.

This is good:

```xml
<TextView
	android:id="@+id/text_view_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```
        
This is __bad__ :

```xml
<!-- Don't do this! -->
<TextView
    android:id="@+id/text_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```

### 1.4.2 Resources naming
![Required](./assets/required.png)

Resource IDs and names are written in __lowercase_underscore__

For all sections below, you can use an sufix or a prefix. But keep in mind to follow the convention for all project duration.

#### 1.4.2.1 ID naming
![Required](./assets/required.png)

IDs should be sufixed/prefixed with the acronym of the element in lowercase underscore. For example:

| Element            | Sufix             |
| -----------------  | ----------------- |
| `TextView`         | `_tv`             |
| `EditText`         | `_edt`            |
| `TextInputLayout`  | `_til`            |
| `ImageView`        | `_iv`             | 
| `Button`           | `_btn`            |
| `Menu`             | `_menu`           |

Image view example:

```xml
<ImageView
    android:id="@+id/profile_iv"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu example:

```xml
<menu>
	<item
        android:id="@+id/menu_done"
        android:title="Done" />
</menu>
```

#### 1.4.2.2 Strings
![Optional](./assets/optional.png)

String names start with a prefix that indentifies the section they belong to. For example `registration_email_hint` or `registration_name_hint`. If a string __doesn't belong__ to any section then you should follow the rules below:


| Prefix             | Description                           |
| ------------------ | ------------------------------------- |
| `error_`           | An error message                      |
| `msg_`             | A regular information message         |       
| `title_`           | A title, i.e. a dialog title          | 
| `action_`          | An action such as "Save" or "Create"  |


#### 1.4.2.3 Styles and Themes
![Required](./assets/required.png)

Unless the rest of resources, style names are written in __UpperCamelCase__.

### 1.4.3 Attributes ordering
![Optional](./assets/optional.png)

As a general rule you should try to group similar attributes together. A good way of ordering the most common attributes is:

1. View Id
2. Style
3. Layout width and layout height
4. Other layout attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically

## 1.5 Dependencies
![Required](./assets/required.png)

Try to avoid Maven dynamic dependency resolution, such as `io.reactivex:rxjava:1.0.+` as this may result in different in unstable builds or subtle, untracked differences in behavior between builds. But __never__ use dependency like `io.reactivex:rxjava:+` or `io.reactivex:rxjava:2.+`, they are too much instable. Dependencies like `io.reactivex:rxjava:1.0.+` as a little less problematic, but is not recommended.

The use of static versions such as `2.1.1` helps create a more stable, predictable and repeatable development environment. The process to update these versions may be boring, going one on one to your dependencies to find the latest version. But this process makes your app much more stable as said before.

# 2 Code guidelines
![Optional](./assets/optional.png)

## 2.1 Java language rules

### 2.1.1 Don't ignore exceptions

You must never do the following:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_While you may think that your code will never encounter this error condition or that it is not important to handle it, ignoring exceptions like above creates mines in your code for someone else to trip over some day. You must handle every Exception in your code in some principled way. The specific handling varies depending on the case._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

See alternatives [here](https://source.android.com/source/code-style.html#dont-ignore-exceptions).
	
### 2.1.2 Don't catch generic exception

You should not do this:

```java
try {
    someComplicatedIOFunction();        // may throw IOException 
    someComplicatedParsingFunction();   // may throw ParsingException 
    someComplicatedSecurityFunction();  // may throw SecurityException 
    // phew, made it all the way 
} catch (Exception e) {                 // I'll just catch all exceptions 
    handleError();                      // with one generic handler!
}
```

See the reason why and some alternatives [here](https://source.android.com/source/code-style.html#dont-catch-generic-exception)

### 2.1.3 Don't use finalizers

_We don't use finalizers. There are no guarantees as to when a finalizer will be called, or even that it will be called at all. In most cases, you can do what you need from a finalizer with good exception handling. If you absolutely need it, define a `close()` method (or the like) and document exactly when that method needs to be called. See `InputStream` for an example. In this case it is appropriate but not required to print a short log message from the finalizer, as long as it is not expected to flood the logs._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers))

### 2.1.4 Fully qualify imports

This is bad: `import foo.*;`

This is good: `import foo.Bar;`

See more info [here](https://source.android.com/source/code-style.html#fully-qualify-imports)

## 2.2 Java style rules 

### 2.2.1 Fields definition and naming 

Fields should be defined at the __top of the file__ and they should follow the naming rules listed below.

* Private, non-static field names start with __m__.
* Private, static field names start with __s__.
* Other fields start with a lower case letter.
* Static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.

Example:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```
	
### 2.2.3 Treat acronyms as words

| Good             | Bad              |
| ---------------- | ---------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` | 
| `getCustomerId`  | `getCustomerID`  | 
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.4 Use spaces for indentation

Use __4 space__ idents for blocks:

```java
if (x == 1) {
    x++;
}
```

Use __8 space__ idents for line wraps:

```java
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```

### 2.2.5 Use standard brace style

Braces go on the same line as the code before them.

```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

Braces around the statements are required unless the condition and the body fit on one line. 

If the condition and the body fit on one line and that line is shorter than the max line length, then do __not__ use braces e.g.

```java
if (condition) body();
```

This is __bad__:

```java
if (condition)
    body();  // bad!
```
        
### 2.2.6 Use standard Java annotations

According to the Android code style guide, the standard practices for some of the predefined annotations in Java are:

* `@Override`: The @Override annotation __must be used__ whenever a method overrides the declaration or implementation from a super-class. For example, if you use the @inheritdocs Javadoc tag, and derive from a class (not an interface), you must also annotate that the method @Overrides the parent class's method.

* `@SuppressWarnings`: The @SuppressWarnings annotation should only be used under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the @SuppressWarnings annotation must be used, so as to ensure that all warnings reflect actual problems in the code.

More information about annotations guidelines can be found [here](http://source.android.com/source/code-style.html#use-standard-java-annotations).


### 2.2.7 Limit variable scope 

_The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable._ 

_Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### 2.2.8 Order import statements 

If you are using an IDE such as Android Studio, you don't have to worry about this because your IDE is already obeying these rules. If not, have a look below.

The ordering of import statements is:

1. Android imports
2. Imports from third parties (com, junit, net, org)
3. java and javax
4. Same project imports 

To exactly match the IDE settings, the imports should be:

* Alphabetical within each grouping, with capital letters before lower case letters (e.g. Z before a).
* There should be a blank line between each major grouping (android, com, junit, net, org, java, javax).

More info [here](https://source.android.com/source/code-style.html#limit-variable-scope)

### 2.2.9 Logging guidelines

Use the logging methods provided by the `Log` class to print out error messages or other information that may be useful for developers to identifiy issues:

* `Log.v(String tag, String msg)` (verbose)
* `Log.d(String tag, String msg)` (debug)
* `Log.i(String tag, String msg)` (information)
* `Log.w(String tag, String msg)` (warning)
* `Log.e(String tag, String msg)` (error)

As a general rule, we use the class name as tag and we define it as a `static final` field at the top of the file. For example:

```java
public class MyClass {
    private static final String TAG = "MyClass";
    
    public myMethod() {
        Log.e(TAG, "My error message");
    }
}
```
	
VERBOSE and DEBUG logs __must__ be disable on relase builds. It is also recommendable to disable INFORMATION, WARNING and ERROR logs but you may want to keep them enable if you think they may be useful to identify issues on release builds. If you decide to leave them enable, you have to make sure that they are not leaking private information such as email addresses, user ids, etc. 

To only show logs on debug builds:

```java
if (BuildConfig.DEBUG) Log.d(TAG, "The value of x is " + x);
```
	
### 2.2.10 Class member ordering 

There is no single correct solution for this but using a __logical__ and __consistent__ order will improve code learnability and readability. It is recommendable to use the following order:

1. Constants 
2. Fields 
3. Constructors 
4. Override methods and callbacks (public or private)
5. Public methods
6. Private methods
7. Inner classes or interfaces

Example:

```java
public class MainActivity extends Activity {

    private String mTitle;
    private TextView mTextViewTitle;
    
    @Override 
    public void onCreate() {
        ...
    }
    
    public void setTitle(String title) {
    	mTitle = title;
    }
    
    private void setUpView() {
        ...
    }
    
    static class AnInnerClass {
    
    }

} 
```

If your class is extending and __Android component__ such as an Activity or a Fragment, it is a good practise, __but not obligatory__, to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle	
    @Override 
    public void onCreate() {} 
    
    @Override 
    public void onResume() {}
    
    @Override 
    public void onPause() {}
    
    @Override 
    public void onDestory() {}

}
```

### 2.2.11 Parameter ordering in methods

When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the __Context__ must be the __first__ parameter.

The opposite case are __callback__ interfaces that should always be the __last__ parameter.

Examples:

```java
// Context always go first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.12 String constants, naming and values

Many elements of the Android SDK such as `SharedPreferences`, `Bundle` or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicaded below.

| Element            | Field Name Prefix |
| ------------------ | ----------------- |
| SharedPreferences  | `PREF_`           |
| Bundle             | `BUNDLE_`         | 
| Fragment Arguments | `ARGUMENT_`       |   
| Intent Extra       | `EXTRA_`          |
| Intent Action      | `ACTION_`         |

Note that the arguments of a Fragment - `Fragment.getArguments()` - are also a Bundle. However, because this is a quite common use of Bundles, we define a different prefix for them. 

Example:

```java
// Note the value of the field is the same as the name to avoid duplication issues
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Intent-related items use full package name as value
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.14 Arguments in Fragments and Activities

When data is passed into an `Activity `or `Fragment` via `Intents` or a `Bundles`, the keys for the different values __must__ follow the rules described in the section above.

When an `Activity` or `Fragment` expect arguments, it should provide a `static public` method that facilitates the creation of the `Fragment` or `Intent`.

In the case of Activities the method is usually called `getStartIntent()`

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

For Fragments it's named `newInstance()` and it handles the creation of the Fragment with the right arguments. 

```java
public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment;
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args)
	return fragment;
}
```

__Note 1__: these methods should go at the top of the class before `onCreate()`

__Note 2__: if we provide the methods described above, the keys for extras and arguments should be `private` because there is not need for them to be exposed outside the class. 

### 2.2.15 Line length limit

Code lines should not exceed __100 characters__. If the line is longer than this limit there are usually two options to reduce its length:

* Extract a local variable or method (Preferable).
* Apply line-wrapping to divide a single line into multiple ones. 

There are two __exceptions__ where is possible to have lines longer than 100:

* Lines that are not possible to split, e.g. long URLs in comments.
* `package` and `import` statements. 

#### 2.2.15.1 Line-wrapping strategies

There isn't an exact formula that explains how to line-wrap and quite often different solutions are valid. However there are a few rules that can be applied to common cases.

__Method chain case__ 

When multiple methods are chained in the same line - for example when using Builders - every call to a method should go in its own line, breaking the line before the `.`

```java
Glide.with(context).load("http://ribot.co.uk/images/sexyjoe.jpg").into(imageView);
```

```java
Glide.with(context)
        .load("http://ribot.co.uk/images/sexyjoe.jpg")
        .into(imageView);
```

__Long parameters case__

When a method has many parameters or its parameters are very long we should break the line after every comma `,`

```java
loadPicture(context, "http://ribot.co.uk/images/sexyjoe.jpg", mImageViewProfilePicture, clickListener, "Title of the picture");
```

```java
loadPicture(context,
        "http://ribot.co.uk/images/sexyjoe.jpg",
        mImageViewProfilePicture,
        clickListener,
        "Title of the picture");
```

### 2.2.16 RxJava chains styling 

Rx chains of operators require line-wrapping. Every operator must go in a new line and the line should be broken before the `.`

```java
public Observable<Location> syncLocations() {
    return mDatabaseHelper.getAllLocations()
            .concatMap(new Func1<Location, Observable<? extends Location>>() {
                @Override
                 public Observable<? extends Location> call(Location location) {
                     return mConcurService.getLocation(location.id);
                 }
            })
            .retry(new Func2<Integer, Throwable, Boolean>() {
                 @Override
                 public Boolean call(Integer numRetries, Throwable throwable) {
                     return throwable instanceof RetrofitError;
                 }
            });
}
```

## 2.3 Tests style rules
![Optional](./assets/optional.png)

### 2.3.1 Unit tests 

The test classes should match the name of the class that the tests are targeting followed by `Test`. For example, If we create a test class that contains test for the `DataManager`, we should name it `DataManagerTest`.

The name of the tests must start with `should` followed by the expected behaviour. For example:

- `shouldLoadUserData()`
- `shouldThrowExceptionWhenLoadingUser()`

### 2.3.2 Espresso tests

Every Espresso test class must target an Activity, therefore the name should match the name of the targeted Activity followed by `Test`, e.g. `SignInActivityTest`

When using the Espresso api is a common practise to place chained method in new lines. 

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```
