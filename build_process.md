<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. Build Process](#1-build-process)
  - [1.1 Configurations](#11-configurations)
    - [1.1.1 `VersionName` and `VersionCode`](#111-versionname-and-versioncode)
    - [1.1.2 `buildTypes`](#112-buildtypes)
    - [1.1.3 `productFlavors`](#113-productflavors)
    - [1.1.4 Using Manifest placeholders](#114-using-manifest-placeholders)
    - [1.1.5 Resource values](#115-resource-values)
    - [1.1.6 Removing unused resource files](#116-removing-unused-resource-files)
    - [1.1.7 Proguard](#117-proguard)
    - [1.1.8 Multidex](#118-multidex)
- [2. Distribution](#2-distribution)
  - [2.1 Google Play Alpha/Beta](#21-google-play-alphabeta)
  - [2.2 Testfairy](#22-testfairy)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 1. Build Process

As we use `gradle` as the main tool to manage dependencies, build and deploy our projects. Some types of configurations are very common.

## 1.1 Configurations

### 1.1.1 `VersionName` and `VersionCode`

We use generated `versionName` and `versionCode` based on `git` tags and commits:

```gradle
def getVersionCode = { ->
    try {
        def cmd = 'git rev-list --count HEAD'
        cmd.execute().text.trim().toInteger()
    } catch (ignored) {
        1
    }
}

def getVersionName = { ->
    try {
        def cmd = 'git describe --tags --abbrev=0'
        cmd.execute().text.trim()
    } catch (ignored) {
        "1.0"
    }
}

def completeVersion = getVersionName() + "." + getVersionCode()
```

And inside `android` configuration block:

```gradle
defaultConfig {
    versionCode getVersionCode() ?: 1
    versionName completeVersion ?: 1
}
```

To accomplish a good version naming, we recommend to create your tags as follow: `1.1`, `1.2`, `1.3` and so on...

### 1.1.2 `buildTypes`

Use of `buildTypes` to configure build based application wide parameters.

As part of `gradle`, is a good pratice to use `buildTypes` to configure proguard, minification and signing too.

```gradle
buildTypes {
    def BOOLEAN = "boolean"
    def TRUE = "true"
    def FALSE = "false"
    def STRING = "String"

    def USE_LOG = "USE_LOG"

    release {
        minifyEnabled true
        shrinkResources true

        signingConfig signingConfigs.release
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

        buildConfigField BOOLEAN, USE_LOG, FALSE
        // ...
    }

    debug {
        buildConfigField BOOLEAN, USE_LOG, TRUE
        // ...
    }
}
```

### 1.1.3 `productFlavors`

Use of `productFlavors` to configure build based environment wide parameters:

```gradle
productFlavors {
    def STRING = "String"

    def APP_API_DOMAIN = "younumber.herokuapp.com/v1/"
    def STAGING_APP_API_DOMAIN = "younumber-staging.herokuapp.com/v1/"

    def APP_API_SERVER_URL = "APP_SERVER_URL"
    
    def APP_API_PRODUCTION_SERVER_URL = "\"https://$APP_API_DOMAIN\""
    def APP_API_STAGING_SERVER_URL = "\"https://$STAGING_APP_API_DOMAIN\""
    
    def AMAZON_BUCKET_NAME = "AMAZON_BUCKET_NAME"

    production {
        buildConfigField STRING, APP_API_SERVER_URL, APP_API_PRODUCTION_SERVER_URL

        buildConfigField STRING, AMAZON_BUCKET_NAME, "\"PRODUCTION_BUCKET_NAME\""
    }

    staging {
        buildConfigField STRING, APP_API_SERVER_URL, APP_API_STAGING_SERVER_URL

        buildConfigField STRING, AMAZON_BUCKET_NAME, "\"STAGING_BUCKET_NAME\""
    }

    development {
        buildConfigField STRING, APP_API_SERVER_URL, APP_API_STAGING_SERVER_URL

        buildConfigField STRING, AMAZON_BUCKET_NAME, "\"DEVELOPMENT_BUCKET_NAME\""
    }
}
```

### 1.1.4 Using Manifest placeholders

Sometimes we need to change some properties, values or id's on AndroidManifest from our gradle build file.

You can declare an placeholder in the AndroidManifest with brackets notation, e.g: ```{variableName}```

An real use case may be:

````xml
<receiver android:name="com.helabs.example.GCMReceiver"
          android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />

        <category android:name="{appId}" />
    </intent-filter>
</receiver>
```

And the __appId__ binding and declaration as follow in your __build.gradle__ file:

```gradle
def final manifestPlaceholders = [
    "appId": "com.helabs.example"
];

defaultConfig {
    // ...
    manifestPlaceholders = manifestPlaceholders
}
```

If you want to configure different manifest placeholders for different configurations, such as build types or product flavors, we must explore more of groovy language:

```gradle
def final manifestPlaceholders = [
    "debug": [
        "appId": "com.helabs.example.debug"
    ],
    "release": [
        "appId": "com.helabs.example.release"
    ],
    "development": [
        "appId": "com.helabs.example.development"
    ],
    "production": [
        "appId": "com.helabs.example.production"
    ]
];

// Based on build types
debug {
    manifestPlaceholders = manifestPlaceholders.debug
}

release {
    manifestPlaceholders = manifestPlaceholders.release
}

// Or based on product flavors

productFlavors {
    development {
        manifestPlaceholders = manifestPlaceholders.development
    }
  
  // And so on...
}
````

### 1.1.5 Resource values

As an alternative to Manifest placeholders, we can generate some resource values dynamically right from our gradle build file. It has different use cases, but is very useful too.

A very common use case is to define the `facebook_app_id` resource, which can be different based on your need.

The syntax is the defined below:

```gradle
android {

    //...

    defaultConfig {
        // ...
        // A default value must be defined, or an error will occur
        resValue "string", "facebook_app_id", ""   
    }
    
    debug {
        resValue "string", "facebook_app_id", "xyz"
    }
    
    release {
        resValue "string", "facebook_app_id", "klmnop"
    }
```

We can access it's value using the common notation: ```@string/facebook_app_id```

### 1.1.6 Removing unused resource files

A simple tip, to reduce your apk size is to only include resources for languages that your app is translated. Some libraries included on the apk (in special google play services and support libraries) have resources for languages that the application is not expected to be localized, and usually increase the apk size for no reason.

On gradle build file, we can restrict what languages the resource files will be included on apk file using `resConfigs` property:

```gradle
android {
    // ...

    defaultConfig {
        //...

        resConfigs "en", "pt-rBR"
    }
```

### 1.1.7 Proguard

It's very important to use Proguard in release builds. Not just for drastic apk size and consequently dex count reductions, but for code obfuscation in case of decompilers usage. Using proguard __DOES NOT__ means that your application will be secured, but is better than nothing.

But is very important to heavly test the app after running proguard, or runtime errors may occur. If some library or piece of code use Reflection, some proguard rules may be added to prevent obfuscation of these used classes on Reflection.

To prevent any runtime error, it's recommended to starting using proguard on the beginning of the app development and test it frequently.

Many libraries packaged on `aar` files already include proguard rules, but sometimes we need to manually include such rules on our proguard rules file. If you use proguard on release builds, check out for proguard rules when including third-party libraries to your project.

A good resource for proguard rules is [android-proguard-snippets](https://github.com/krschultz/android-proguard-snippets).
Some rules may be outdated, so, the better source of updated rules are always the library repository.

### 1.1.8 Multidex

The process of import and use libraries is to easy when using gradle or maven, so, is very common to reach the 65k dexcount limit when developing complex apps. Just including the support libraries and other *must have libraries* we reach 30 ~ 40k dexcount pretty fast.

The easiest solution is to use MultiDex in the app. But for versions below 21, it heavly degrades the app build and startup time and often increase the apk size, not counting with other limitations that [Dalvik has](http://developer.android.com/intl/pt-br/tools/building/multidex.html#limitations). Which is bad for end users and must be used in the last case.

If you don't use Proguard for release builds, it can be the best solution for this case. But enabling proguard for debug builds  will slowdown build time and makes debug process very hard, which is not recommended :(

For this problem, the simplest solution is to enable Multidex in debug builds and disable it in release builds. But this slow down our development process, and thus, reducing productivity.

But the best solution for this case, is to define the `minSdkVersion` of development builds to 21 and use Multidex only in this build variant. The dex generated for ART is much faster than considering a compatible version for all devices, and will not cause any impact in development process. It's very recommended to read this [article](http://developer.android.com/intl/pt-br/tools/building/multidex.html#dev-build) to understand the implementation of this concept.

__OBS:__ Using 21 as our `minSdkVersion` for development builds, does not means that we must not test our apps in older versions in our development process.

# 2. Distribution

For production distribution, we often use Google Play Store.

But for in-development and tests, we have two main forms of distribution: using Google Play Store Alpha/Beta or TestFairy.

## 2.1 Google Play Alpha/Beta

## 2.2 Testfairy