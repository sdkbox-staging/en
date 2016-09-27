[&#8249; Google Play Games Services Doc Home](./)

<h1>Google Play Games Services 集成说明</h1>
<<[../../shared/-VERSION-/version.md]

##集成
打开一个终端并且使用下面的命令安装 SDKBOX Google Play Games Services 插件。确定 SDKBOX installer 已经正确安装.
```bash
$ sdkbox import googleplay
```

##安装之后

### Android

#### 更新 AndroidManifest.xml

添加这个 meta-data tag 到你的 `AndroidManifest.xml`

```
<meta-data android:name="com.google.android.gms.games.APP_ID"
    android:value="@string/google_app_id" />
```

#### 更新 string.xml

添加这个到 `proj.android/res/values/string.xml`

```
<string name="google_app_id">777734739048</string>
```

用你的 App Id 替换 `google_app_id` 值。

### iOS

#### 为 GPG 选择一个 rootview controller:

更新 `proj.ios_mac/ios/RootViewController.h`
添加这个:

```
#import <GoogleSignIn/GoogleSignIn.h>

// Change RootViewController class definition to:
@interface RootViewController : UIViewController<GIDSignInUIDelegate>
```

#### 设置 Google Play 登录回调

更新 `proj.ios_mac/ios/AppController.mm`

1. 添加这个方法：

```
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary *)options {
    return [[GIDSignIn sharedInstance] handleURL:url
                               sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
                                      annotation:options[UIApplicationOpenURLOptionsAnnotationKey]];
}
```

2. 在这个方法中:
```
(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
```

添加调用（在 return Yes 之前）:

```
    // _viewController could also be named
    //  viewController, depending of the project type.
    [GIDSignIn sharedInstance].uiDelegate = _viewController;
```

#### 添加 URL 类型

在 **你的工程 > Info > URL Types** 中添加下列 URL 类型：

+ URL 1:

    + Identifier: `com.google.ReverseClientId`
    + Url schemes: `com.googleusercontent.apps.777734739048-cdkbeieil19d6pfkavddrri5o19gk4ni`
    > (use this as sample, or put your very own application’s url scheme)

+ URL 2:

    + Identifier: `com.google.BundleId`
    + URL schemes: `com.sdkbox.gpg`
    > (use this as sample or put your own application’s bundle id)

#### 更多信息

更多信息请参考[官方文档](https://developers.google.com/games/services/cpp/gettingStartedIOS)

<<[../../shared/notice.md]

##使用

### 要求

你必须在 [Google Play Developer console](https://play.google.com/apps/publish) 创建应用程序，并且在控制台允许和配置所有服务。

* 请根据 [setup guide](https://developers.google.com/games/services/console/enabling) 设置 Google Play Games Services.
* 设置后，请根据 [config guide](https://developers.google.com/games/services/console/configuring) 允许不同的 Games Services.

> 备注： Google Play Games Services 默认使用你的 release keystore, 如果要用 debug 设置测试你的游戏, 请链接一个附加 app [debug keystore](http://stackoverflow.com/questions/17612928/should-i-use-debug-keystore-with-google-play-game-services-during-development)

<<[usage.md]

<<[api-reference.md]

##手工集成

如果 *SDKBOX Installer* __没有完成安装__ , 可以手工集成 SDKBOX. 如果安装成功，请 __不要__ 完成这个文档的任何内容。这不需要。

这些步骤很少需要. 如果你自己做这些步骤，请重复检查与核对。


<<[manual_ios.md]

<<[../../shared/manual_integration_android_and_android_studio.md]

<<[manual_android.md]

<<[extra-step.md]

<<[../../shared/manual_integration_google_play_step.md]

<<[proguard.md]


