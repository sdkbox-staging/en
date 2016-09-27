## 手工集成到 iOS
从包的 __plugins/ios__ 目录拖放下列 frameworks 到你的 Xcode 项目中, 添加时选中 `Copy items if needed`:

> sdkbox.framework

> PluginSdkboxGooglePlay.framework

> GoogleAppUtilities.framework

> GoogleAuthUtilities.framework

> GoogleNetworkingUtilities.framework

> GoogleOpenSource.framework

> GooglePlus.bundle

> GooglePlus.framework

> GoogleSignIn.bundle

> GoogleSignIn.framework

> GoogleSymbolUtilities.framework

> GoogleUtilities.framework

> gpg.bundle

> gpg.framework


上面的 frameworks依赖其他 frameworks. 你也需要添加一下系统 frameworks, 如果它们还未添加:

> AddressBook.framework

> AssetsLibrary.framework

> CoreData.framework

> CoreLocation.framework

> CoreMotion.framework

> CoreTelephony.framework

> CoreText.framework

> Foundation.framework

> MediaPlayer.framework

> QuartzCore.framework

> SafariServices

> Security.framework

> StoreKit

> Security.framework

> SystemConfiguration.framework

> libc++.dylib

> libz.dylib

添加一个 linker flag, 如果你设置需求为:
__Target -> Build Settings -> Linking -> Other Linker Flags__:

> -ObjC

### 代码修改

#### 为 GPG 设置一个 rootview controller:

更新 `proj.ios_mac/ios/RootViewController.h`
添加:

```
import <GoogleSignIn/GoogleSignIn.h>

// Change RootViewController class definition to:
@interface RootViewController : UIViewController<GIDSignInUIDelegate>
```

#### 设置 Google Play 登录回调

更新 `proj.ios_mac/ios/AppController.mm`

1. 添加这个方法:

```
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary *)options {
    return [[GIDSignIn sharedInstance] handleURL:url
                               sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
                                      annotation:options[UIApplicationOpenURLOptionsAnnotationKey]];
}
```

2. 在这个方法:
```
(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
```

添加调用 (在 return YES 之前):

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
