# LottieExample
Lottie是Airbnb开源的一个支持 Android、iOS 以及 ReactNative，利用Json文件的方式快速实现动画效果的库。简单点说就是Json文件记录动画路径，Android、iOS 以及 ReactNative解析展现出来。

Lottie项目地址：[https://github.com/airbnb/lottie-react-native](https://link.jianshu.com/?t=https://github.com/airbnb/lottie-android)

Lottie如何使用？

一、Lottie能干什么？

在回答Lottie能干什么之前，我们先想下如下的动画如何实现？

![Example1.gif](http://upload-images.jianshu.io/upload_images/1780580-86be7d816f1cc355.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Example2.gif](http://upload-images.jianshu.io/upload_images/1780580-e3bb75ed39d306a8.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Example3.gif](http://upload-images.jianshu.io/upload_images/1780580-b0dd45aa46e6c453.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

大概有几种方式（针对Android开发的，本人是Android开发小白）:

使用帧动画。这种方式固然可行，但是一个需要动画添加很多张图片，势必会导致apk体积变大，并且还要根据不同的尺寸进行适配。

用 Gif。但是使用 Gif 占用空间较大，而且需要为各种屏幕尺寸、分辨率做适配，并且Android本是不支持gif直接展示的。

用代码加图片辅助。如之前写的[仿照驾校一点通欢迎页](http://blog.csdn.net/zhang58246500/article/details/45248701)，这种方式繁琐并且每更新一次都需要重新写很多代码。

Android 5.x 之后提供了对 SVG 的支持，通过 VectorDrawable、AnimatedVectorDrawable 的结合可以实现一些稍微复杂的动画，但是问题和前2个类似。

另外就是React-Native官方的动画让我看得很蛋疼，可能我比较菜吧

综述以上的问题，有没有什么方式是即可以方便的实现动画效果，又可以不用考虑适配的问题，而且Android、ios还可以兼容呢? 答案是确定的，那就是Lottie

**Lottie就是支持Android, iOS, 和React Native,并且只需简单的代码就可以实现复杂动画效果的库**

那对于程序原来说有什么局限呢，那就是Json文件从何而来，一个路径就是自己搞一个Adobe 的 After Effects(简称 AE)工具文件拖进去，另一个途径就是直接给你们设计师美工用AE给你们生成一个Json文件。软件安装具体可参考[airbnb.io/lottie/after-effects/getting-started.html](http://airbnb.io/lottie/after-effects/getting-started.html)

* * *

官方API： [github.com/airbnb/lottie-react-native/blob/master/docs/api.md](https://github.com/airbnb/lottie-react-native/blob/master/docs/api.md)

官方文档介绍：[airbnb.io/lottie/](http://airbnb.io/lottie/)

官方LottieFiles：[www.lottiefiles.com/](https://www.lottiefiles.com/)

下面是程序员关注的操作（[具体可以看官方介绍](http://airbnb.io/lottie/react-native/react-native.html)）

以下是根据官方介绍简单翻译过来的

#### 入门：

通过使用yarn或npm安装节点模块来开始使用Lottie：
```
pod 'lottie-ios', :path => '../node_modules/lottie-ios'
pod 'lottie-react-native', :path => '../node_modules/lottie-react-native'
```
### iOS版：
如果你在iOS上使用CocoaPods，你可以在你的Podfile：
```
pod 'lottie-ios', :path => '../node_modules/lottie-ios'
pod 'lottie-react-native', :path => '../node_modules/lottie-react-native'
```
如果你不在iOS上使用CocoaPods，你可以使用react-native link：
```
react-native link lottie-ios
react-native link lottie-react-native
```
之后，打开Xcode项目配置并添加Lottie.frameworkas Embedded Binaries。
### Android版：
对于android，你也可以react-native link：
```
react-native link lottie-react-native
```
或者你可以手动添加它：
```
settings.gradle 文件添加：

include ':lottie-react-native'
project(':lottie-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/lottie-react-native/lib/android')

build.gradle 文件添加：

dependencies {
  ...
  compile project(':lottie-react-native')
  ...
}
```
Lottie需要Android支持库版本26.如果您正在使用该react-native init 模板，您可能仍然在使用23.要更改此设置，只需转到该块内android/app/build.gradle的compileSdkVersion选项android并将其更改为
```
android {
    compileSdkVersion 26 // <-- update this to 26
    // ...
```
您还必须添加LottiePackage到getPackages()您的ReactApplication
```
@Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          ...
          new LottiePackage()
      );
    }
  };
```
有了这个改变，你应该准备好去。

Lottie的动画进度可以控制一个Animated值：
```
import React from 'react';
import { Animated } from 'react-native';
import LottieView from 'lottie-react-native';

export default class BasicExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      progress: new Animated.Value(0),
    };
  }

  componentDidMount() {
    Animated.timing(this.state.progress, {
      toValue: 1,
      duration: 5000,
    }).start();
  }

  render() {
    return (
      <LottieView source={require('../path/to/animation.json')} progress={this.state.progress} />
    );
  }
}
```
此外，有一个迫切的API有时更简单。
```
import React from 'react';
import LottieView from 'lottie-react-native';

export default class BasicExample extends React.Component {
  componentDidMount() {
    this.animation.play();
    // Or set a specific startFrame and endFrame with:
    this.animation.play(30, 120);
  }

  render() {
    return (
      <LottieView
        ref={animation => {
          this.animation = animation;
        }}
        source={require('../path/to/animation.json')}
      />
    );
  }
}
```
实践中遇到的问题，IOS按步骤来操作没什么问题，主要是Android手动处理的有点多，先看错误

1、用react-native run-android 时报的错误
```
FAILURE: Build failed with an exception.

* What went wrong:
A problem occurred configuring project ':app'.
> Could not resolve all dependencies for configuration ':app:_debugApk'.
   > A problem occurred configuring project ':lottie-react-native'.
      > Could not resolve all dependencies for configuration ':lottie-react-native:_debugPublishCopy'.
         > Could not find com.android.support:appcompat-v7:26.1.0.
           Searched in the following locations:
               file:/Users/lihong/Library/Android/sdk/extras/android/m2repository/com/android/support/appcompat-v7/26.1.0/appcompat-v7-26.1.0.pom
               file:/Users/lihong/Library/Android/sdk/extras/android/m2repository/com/android/support/appcompat-v7/26.1.0/appcompat-v7-26.1.0.jar
               file:/Users/lihong/WebstormProjects/LottieExample/android/sdk-manager/com/android/support/appcompat-v7/26.1.0/appcompat-v7-26.1.0.jar
           Required by:
               LottieExample:lottie-react-native:unspecified > com.airbnb.android:lottie:2.2.5

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

BUILD FAILED

Total time: 20.289 secs
Could not install the app on the device, read the error above for details.
Make sure you have an Android emulator running or a device connected and have
set up your Android development environment:
https://facebook.github.io/react-native/docs/android-setup.html
```
心中是捉急呀，没办法只能用Android Studio打开看看了。
先看我配置过的app下的build.gradle
```
android {
    compileSdkVersion 26
    buildToolsVersion '26.0.2'

    defaultConfig {
        applicationId "com.lottieexample"
        minSdkVersion 16
        targetSdkVersion 26
        versionCode 1
        versionName "1.0"
        ndk {
            abiFilters "armeabi-v7a", "x86"
        }
    }
    splits {
        abi {
            reset()
            enable enableSeparateBuildPerCPUArchitecture
            universalApk false  // If true, also generate a universal APK
            include "armeabi-v7a", "x86"
        }
    }
    buildTypes {
        release {
            minifyEnabled enableProguardInReleaseBuilds
            proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
        }
    }
    // applicationVariants are e.g. debug, release
    applicationVariants.all { variant ->
        variant.outputs.each { output ->
            // For each separate APK per architecture, set a unique version code as described here:
            // http://tools.android.com/tech-docs/new-build-system/user-guide/apk-splits
            def versionCodes = ["armeabi-v7a":1, "x86":2]
            def abi = output.getFilter(OutputFile.ABI)
            if (abi != null) {  // null for the universal-debug, universal-release variants
                output.versionCodeOverride =
                        versionCodes.get(abi) * 1048576 + defaultConfig.versionCode
            }
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }
}

dependencies {
    compile project(':lottie-react-native')
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.android.support:appcompat-v7:26.1.0'
    compile 'com.android.support:support-annotations:26.1.0'
    // From node_modules
    compile 'com.facebook.react:react-native:+'
}
```
主要是配置:
```
compileSdkVersion 26
buildToolsVersion '26.0.2'

targetSdkVersion 26

compile 'com.android.support:appcompat-v7:26.1.0'
```
重新编译报错如下：
```
Unable to resolve dependency for ':app@debug/compileClasspath': Could not resolve com.android.support:appcompat-v7:26.1.0.

Could not resolve com.android.support:appcompat-v7:26.1.0.
Required by:
    project :app
    project :app > project :lottie-react-native > com.airbnb.android:lottie:2.2.5
 > Could not resolve com.android.support:appcompat-v7:26.1.0.
    > Could not get resource 'https://maven.google.com/com/android/support/appcompat-v7/26.1.0/appcompat-v7-26.1.0.pom'.
          > Could not HEAD 'https://maven.google.com/com/android/support/appcompat-v7/26.1.0/appcompat-v7-26.1.0.pom'.
                   > Remote host closed connection during handshake
                               > SSL peer shut down incorrectly
```
个人觉得是要翻墙才可以下载:appcompat-v7:26.1.0，瞬间无奈中，望Android大神指教一哈。

OK！先不纠结Android的。后期再好好琢磨琢磨，好歹IOS能搞起来，先看看效果图
![Lottie.gif](http://upload-images.jianshu.io/upload_images/1780580-be4f228858873b5a.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后项目地址（希望大家给star）有问题及时沟通：
  
