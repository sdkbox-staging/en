
### 注册 Javascript 函数
在 cocos2d-x 使用它们之前，你需要注册所有的 Google Play Games JS 函数.

要做到这个:
* 修改 `./frameworks/runtime-src/Classes/AppDelegate.cpp` 包含下列头:
```cpp
#include "PluginSdkboxGooglePlayJS.hpp"
#include "PluginSdkboxGooglePlayJSHelper.h"
```

* 修改 `./frameworks/runtime-src/Classes/AppDelegate.cpp` 确保调用:
```cpp
    sc->addRegisterCallback(register_all_PluginSdkboxGooglePlayJS);
    sc->addRegisterCallback(register_all_PluginSdkboxGooglePlayJS_helper);
```

### 初始化 Google Play Games
Google Play 通过一个 `gpg.Builder` 对象初始化.
这是一个有效的复制和粘贴鉴定到库中:

```javascript

//Initialization
var config = new gpg.PlatformConfiguration();
config.SetClientID('777734739048-cdkbeieil19d6pfkavddrri5o19gk4ni.apps.googleusercontent.com');

new gpg.GameServices.Builder()
            .SetOnAuthActionStarted( function( result ) {
                // Auth started callback
            })
            .SetOnAuthActionFinished( function( result ) {
                // Auth finished callback
            })
            .SetLogging( gpg.LogLevel.INFO )	// Set Logging level
            .EnableSnapshots()					// Enable Snapshot (Saved Game) functionailty
            .Create( config, function( game_services ) {
                // 8
            } );
```

让我们一步步进入这个过程。

###初始化过程分级
建立一个 `gpg.PlatformConfiguration` 对象. 客户端 id 必须指定为你自己的应用程序，并且只在 iOS 中需要。

下一步是建立一个 GameServices 对象, 它提供所有 Google Play Game Services 的访问.


#### 授权回调
这有两个 GPG 授权回调: `AuthActionStarted` 和 `AuthActionFinished`

#### 授权开始

```javascript
//result.AuthOperation indicates if user wants sign in or sign out
.SetOnAuthActionStarted(
    function( result ) {
        cc.log('on auth action started: ' + result.AuthOperation);
    })
```

#### 授权完成

```javascript
.SetOnAuthActionFinished(
    function( result ) {
        cc.log('on auth action finished: ' + result.AuthOperation + ' ' + result.AuthStatus);
    })
```

#### 设置日志级别
设置日志级别是一个可选步骤, 但值得一提的是只有这个地方可以控制 GPG 的日志能力。


#### 允许保存的游戏
如果你的游戏预计保存游戏到云端, 这是一步将是强制性的，你必须在初始化过程中指定它。

#### 步骤 8
直到这一步骤之前，所有的代码都是设置验证的。
这个 `Create` 调用实际执行验证.
GPG 通过一个 `gpg.GameServices` 对象交互. 如果一切顺利的话，这个对象最终会被传递给 `Create` 方法的回调函数.

### 授权

所有谷歌玩游戏服务都需要用户登录。 所以经过 GPG 初始化，你应该尝试用下面的 API 登录：

```javascript
	game_services.StartAuthorizationUI();
```

### 成就

成就是一个很好的方式来增加用户在戏内的参与。 你可以在你的游戏中实现成就来鼓励用户探索的不同的游戏方式。 成就也可以是一个有趣的方式，让玩家彼此进行轻松的竞争。

例如，你可以解锁 `gpg.GameServices.Achievements.Unlock`, 增加 `gpg.GameServices.Achievements.Increment` 或者显示 `gpg.GameServices.Achievements.Reveal`.

更多信息请参考 [Achievements documentation](https://developers.google.com/games/services/common/concepts/achievements)

### 排行榜

排行榜是一个驱动玩家之间的竞争的有趣方式，无论是你最铁杆的粉丝（公共排行榜的榜首）和更多的休闲玩家（朋友之间比较彼此的进步）。

例如, 它让你访问元数据关于一个排行榜 `gpg.GameServices.Leaderboards.Fetch`, 页面和用户得分 `gpg.GameServices.Leaderboards.FetchScorePage`, 等等.

更多信息请参考 [Leaderboards documentation](https://developers.google.com/games/services/common/concepts/leaderboards)

### 保存的游戏

保存的游戏让你可以将游戏进度保存到 Google 的服务器. 你的游戏可以在任何设备上取得保存的游戏并让玩家继续游戏.
保存的游戏让玩家可以在多个设备间同步游戏数据.

更多信息请参考 [Saved Games documentation](https://developers.google.com/games/services/common/concepts/savedgames)

### 实时多人游戏

你的游戏可以在 Google Play 游戏服务里使用实时多人游戏 API 连接多个玩家到单个游戏会话，并且在连接的玩家间传输消息数据. 使用实时多人游戏 API 可以帮助简化你的开发，因为你可以从下列任务获得好处:

+ 建立和维护一个实时多人游戏房间(一个虚拟的结构，允许在同一个游戏会话里的玩家通过网络互相通讯，一个玩家可以直接发送数据给另一个玩家)的网络连接.
+ 提供一个玩家选择用户界面 (UI) 来邀请玩家加入房间, 为自动配对寻找随机玩家, 或者组合两种方式.
+ 在实时多人游戏生命期间存储游戏参与者的状态到 Google Play 游戏服务的服务器.
+ 发送房间邀请和更新给玩家. 玩家登陆的所有设备都会受到通知（除非已禁止）.

更多信息请参考 [Real-time Multiplayer documentation](https://developers.google.com/games/services/common/concepts/realtimeMultiplayer)

### 回合制多人游戏

在回合制多人游戏中，一个共享的游戏状态在多个玩家间传递, 并且同一时间只有一个玩家有修改共享数据的权利. 根据游戏确定的游戏顺序，玩家异步得到回合信息. 你的游戏可以是用下列任务:

+ 邀请一个玩家加入一个回合制游戏, 为自动配对搜索随机用户, 或者组合两种方式.Google Play 游戏服务允许一次比赛容纳最多八个参与者.
+ 存储参与者和比赛信息到 Google 的服务器，并且在比赛期间同步数据到所有参赛者。
+ 发送比赛邀请和更新给玩家. 玩家登陆的所有设备都会受到通知（除非已禁止）.

更多信息请参考 [Turn-based Multiplayer documentation](https://developers.google.com/games/services/common/concepts/turnbasedMultiplayer)

### 玩家统计

读取和设置玩家相关的数据.
例如, 关于当前已登录的玩家 `gpg.GameServices.Players.FetchSelf`, 或者任何识别的玩家 `gpg.GameServices.Players.Fetch`.
此外, 玩家统计也可以获取玩家的平均会话时长、每日活跃玩家数等信息. 更多信息请参考 [Player Stats documentation](https://developers.google.com/games/services/cpp/stats)


### 事件和任务

Google Play Games 事件服务允许你手机玩家在游戏过程中产生的时间，并存储到 Google 的服务器，用于游戏分析。你可以灵活地定义你的游戏应该收集的玩家数据，这可能包括一些指标，如：

+ 玩家使用一个特定的项目
+ 玩家达到一定级别
+ 玩家执行一些特定的游戏行动

你可以使用这些事件数据作为如何改进游戏的反馈。例如，你可以调整你的游戏中太难完成的内容。

例如，你可以接受一个任务 `gpg.GameServices.Quests.Accept` 或者完成一个里程碑 `gpg.GameServices.Quests.AcceptMilestone`.

更多信息请参考 [Events and Quests documentation](https://developers.google.com/games/services/common/concepts/quests)

### 临近连接
临近连接允许本地多人游戏和屏幕广播. 更多信息请参考 [Nearby documentation](https://developers.google.com/games/services/cpp/nearby)
