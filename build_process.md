<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. Build Process](#1-build-process)
  - [1.1 Configurations](#11-configurations)
    - [1.1.1 `VersionName` and `VersionCode`](#111-versionname-and-versioncode)
    - [1.1.2 `buildTypes`](#112-buildtypes)
    - [1.1.3 `productFlavors`](#113-productflavors)
- [2. Distribution](#2-distribution)
  - [2.1 Google Play Alpha/Beta](#21-google-play-alphabeta)
  - [2.2 Testfairy](#22-testfairy)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 1. Build Process

As we use `gradle` as the main tool to manage dependencies, build and deploy our projects. Some types of configurations are very common.

## 1.1 Configurations

### 1.1.1 `VersionName` and `VersionCode`

We use generated `versionName` and `versionCode` based on `git` tags and commits:

```
def getVersionCode = { ->
    try {
        def stdout = new ByteArrayOutputStream()
        exec {
            commandLine 'git', 'rev-list', '--count', 'HEAD'
            standardOutput = stdout
        }
        return Integer.parseInt(stdout.toString().trim())
    } catch (s) {
        println s
        return 1
    }
}

def getVersionName = { ->
    try {
        def stdout = new ByteArrayOutputStream()
        exec {
            commandLine 'git', 'describe', '--tags', '--abbrev=0'
            standardOutput = stdout
        }
        return stdout.toString().trim()
    } catch (s) {
        println s
        return "1.0"
    }
}

def completeVersion = getVersionName() + "." + getVersionCode()
```

And inside `android` configuration block:

```
defaultConfig {
    versionCode getVersionCode() ?: 1
    versionName completeVersion ?: 1
}
```

To accomplish a good version naming, we recommend to create your tags as follow: `1.1`, `1.2`, `1.3` and so on...

### 1.1.2 `buildTypes`

Use of `buildTypes` to configure build based application wide parameters.

As part of `gradle`, is a good pratice to use `buildTypes` to configure proguard, minification and signing too.

```
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

```
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

# 2. Distribution

For production distribution, we often use Google Play Store.

But for in-development and tests, we have two main forms of distribution: using Google Play Store Alpha/Beta or TestFairy.

## 2.1 Google Play Alpha/Beta

## 2.2 Testfairy