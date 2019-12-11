# Preconditions

For Android users, please check the following steps:

### 1. Add aar file

Download 'MYKEYWalletLib.aar' from following link, copy to libs directory of your app module

[https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android](https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android)

### 2. Add libs

Add following code to file build.gradle

```java
repositories {
    flatDir {
        dirs 'libs'
    }
}
```

### 3. Add Jni configuration

In file build.gradle, add config for Jni directory

```java
android {
...
    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }
    defaultConfig {
        ndk {
        abiFilters "armeabi-v7a"
        }
    }
}
```

### 4. Add MYKEY dependencies

Add following dependency in file build.gradle

```java
dependencies{
    implementation(name: 'MYKEYWalletLib', ext: 'aar')
    implementation "com.alibaba:fastjson:1.1.70.android"
}
```

### 5. Add MYKEY activity

Copy following code to AndroidManifest.xml, and set the callback deeplink, composed by scheme、host and path

```java
<activity android:name="com.mykey.sdk.connect.scheme.callback.MYKEYCallbackActivity">
    <intent-filter>
        <data
            android:scheme="customscheme"
            android:host="customhost"
            android:path="/custompath"/>
        <data/>

        <category android:name="android.intent.category.DEFAULT"/>
        <action android:name="android.intent.action.VIEW"/>
        <category android:name="android.intent.category.BROWSABLE"/>
    </intent-filter>
</activity>
```

This configuration will generate a deeplink for MYKEY callback, which will be used in MYKEK SDK initlization, [initSimple](../../dive-into-mykey/classes-and-methods.md#initsimplerequest)。

### 6. Proguard rules

```java
-keep class com.mykey.sdk**{*;}
-dontwarn com.mykey.sdk**

-keep class go**{*;}
-dontwarn go**

-keep class mykeycore**{*;}
-dontwarn mykeycore**
```

