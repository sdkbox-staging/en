### 修改 __Cocos2dxActivity.java__
* 如果你使用 cocos2d-x 源代码, 假定你在 __proj.android__ 目录中, __Cocos2dxActivity.java__ 的位置是:

    ```
    ../../cocos2d-x/cocos/platform/android/java/src/org/cocos2dx/
    lib/Cocos2dxActivity.java
    ```

* 如果你使用预编译的 cocos2d-x，假定你在 `proj.android` 目录中, __Cocos2dxActivity.java__ 的位置是:

    ```
    ./src/org/cocos2dx/lib/Cocos2dxActivity.java
    ```

  __说明:__ 当使用 Cocos2d-x 源代码, 不同版本的 __Cocos2dxActivity.java__ 在不同的位置. 找到这个位置的途径之一是查看 __proj.android/project.properties__. 例如:
  ```
  android.library.reference.1=../../cocos2d-x/cocos/platform/android/java
  ```

在这个例子里, __Cocos2dxActivity.java__ 应该位于:

```
../../cocos2d-x/cocos/platform/android/java/src/org/cocos2dx/lib/Cocos2dxActivity.java
```

* 修改 __Cocos2dxActivity.java__ 增加下列 imports:
```java
import android.content.Intent;
import com.sdkbox.plugin.SDKBox;
```

* 第二, 修改 __Cocos2dxActivity.java__ 并编辑 `onCreate(final Bundle savedInstanceState)` 函数增加一个调用 `SDKBox.init(this);`. 这个调用的位置很重要. 它必须在调用 `onLoadNativeLibraries();` 之后. 例如:
```java
onLoadNativeLibraries();
SDKBox.init(this);
```

* 最后, 我们需要插入适当的 __overrides__ 代码. 这里有一些规则.
    * 如果下列方法没有定义, __添加它__.

    * 如果方法已经定义，增加调用到 `SDKBox` 到 __同名的__ 以后函数.
```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
          if(!SDKBox.onActivityResult(requestCode, resultCode, data)) {
            super.onActivityResult(requestCode, resultCode, data);
          }
    }
    @Override
    protected void onStart() {
          super.onStart();
          SDKBox.onStart();
    }
    @Override
    protected void onStop() {
          super.onStop();
          SDKBox.onStop();
    }
    @Override
    protected void onResume() {
          super.onResume();
          SDKBox.onResume();
    }
    @Override
    protected void onPause() {
          super.onPause();
          SDKBox.onPause();
    }
    @Override
    public void onBackPressed() {
          if(!SDKBox.onBackPressed()) {
            super.onBackPressed();
          }
    }
```
