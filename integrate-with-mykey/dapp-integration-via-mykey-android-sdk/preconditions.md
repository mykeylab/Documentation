# 环境准备

对于Android用户，按照以下步骤集成:

## 1. 添加aar文件

在以下项目链接中，下载MYKEYWalletLib.aar文件，复制到到你dapp模块的libs目录下 [https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android](https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android)

## 2. 添加libs

在dapp模块的build.gradle文件的空白处添加如下代码：

```java
repositories {
    flatDir {
        dirs 'libs'
    }
}
```

## 3. 添加jni配置

在dapp模块的build.gradle文件android下添加Jni文件夹配置

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

## 4. 添加mykey项目依赖

在dapp模块的build.gradle文件中添加依赖

```java
dependencies{
    implementation(name: 'MYKEYWalletLib', ext: 'aar')
    implementation "com.alibaba:fastjson:1.1.70.android"
}
```

## 5. 添加mykey activity

复制下面的代码到你的AndroidManifest.xml，并设置符合你包名或规则的scheme、host和path

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

此设置会为你生成一个供MYKEY调用的深度链接，在MYKEY初始化时会用到，[init](https://github.com/mykeylab/Documentation/blob/master/Chinese/MYKEY_ANDROID_SDK.md#init)[initSimple](https://github.com/mykeylab/Documentation/blob/master/Chinese/MYKEY_ANDROID_SDK.md#initSimple)。

## 6. 添加混淆配置

```java
-keep class com.mykey.sdk**{*;}
-dontwarn com.mykey.sdk**

-keep class go**{*;}
-dontwarn go**

-keep class mykeycore**{*;}
-dontwarn mykeycore**
```

