### 复制文件
从包中 `plugin/android/libs` 复制下列 __jar__ 文件到你项目的 __<project_root>/libs__ 目录.

> PluginGoogleAnalytics.jar

> sdkbox.jar


* 如果你使用 cocos2d-x 复制 __jar__ 文件到:

	Android command-line:
	```
	cocos2d/cocos/platform/android/java/libs
	```

	Android Studio:
	```
	cocos2d/cocos/platform/android/libcocos2dx/libs
	```

* 如果你使用 cocos2d-js 或者 lua 复制 __jar__ 文件到:

	Android command-line:
	```
	frameworks/cocos2d-x/cocos/platform/android/java/libs
	```

	Android Studio:
	```
	frameworks/cocos2d-x/cocos/platform/android/libcocos2dx/libs
	```

* 如果你使用 cocos2d-x 预编译库，复制 __jar__ 文件到:

	Android command-line:
	```
	<project_root>/libs
	```

<<[../../shared/copy_jni_lib.md]


### 编辑 `AndroidManifest.xml`
在 __application tag__ 添加下列权限:
```xml
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
```

还有少量的附件 meta-data 需要添加:
```xml
<meta-data android:name="com.google.android.gms.version"
    android:value="@integer/google_play_services_version" />
<meta-data android:name="com.google.android.gms.games.APP_ID"
    android:value="@string/google_app_id" />
```

确保添加 `<string name="google_app_id">777734739048</string>` 到文件 `res/values/string.xml` 中。将值修改为你自己的 App Id。

### 编辑 `Android.mk`

编辑 `<project_root>/jni/Android.mk`:

#### GPG 依赖

安装一个名为 `gpg` 的目录到你的 `proj.android` 目录。下载地址 https://developers.google.com/games/services/downloads/gpg-cpp-sdk.v2.1.zip

添加这个到你的 `android.mk` 文件

```
# right after: include $(CLEAR_VARS)
LOCAL_MODULE := libgpg
LOCAL_SRC_FILES := ../gpg/lib/gnustl/$(TARGET_ARCH_ABI)/libgpg.a
include $(PREBUILT_STATIC_LIBRARY)

# in local c includes block
LOCAL_C_INCLUDES += ../gpg/include/

# in local whole static libs block
LOCAL_WHOLE_STATIC_LIBRARIES += gpg-1

# in imports block
$(call import-module, ../gpg)
```

#### 其他 mk 文件步骤

添加附加的 __LOCAL_WHOLE_STATIC_LIBRARIES__:
```
LOCAL_WHOLE_STATIC_LIBRARIES += PluginGoogleAnalytics
LOCAL_WHOLE_STATIC_LIBRARIES += sdkbox
```

添加调用到:
```
$(call import-add-path,$(LOCAL_PATH))
```
在任何 __import-module__ 指令之前.

添加附加的 __import-module__ 指定在最后:
```
$(call import-module, ./sdkbox)
$(call import-module, ./pluginsdkboxgoogleplay)
```

  __注意:__ 如果你使用预编译库，重要的是确保这些指令在已有的 `$(call import-module,./prebuilt-mk)` 指令之前.

### 修改 `Application.mk` (仅限 Cocos2d-x v3.0 到 v3.2)
修改 `<project_root>/jni/Application.mk` 确保 __APP_STL__ 正确定义. 如果 `Application.mk` 包含 `APP_STL := c++_static`, 应该修改为:
```
APP_STL := gnustl_static
```
