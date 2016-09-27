

### 初始化 Google Play Games
在你代码的适当位置初始化插件. 我们推荐在 `AppDelegate::applicationDidFinishLaunching()` 或者 `AppController:didFinishLaunchingWithOptions()` 里做初始化. 确保包含了适当的头文件:
```cpp
AppDelegate::applicationDidFinishLaunching()
{
     sdkbox::PluginSdkboxGooglePlay::init();
}
```

### 授权
所有 Google Play Game 服务都需要用户登录, 确保在使用任何服务前请求用户登录. 更多信息参考 [官方文档](https://developers.google.com/games/services/cpp/GettingStartedNativeClient#concepts)

### 成就
要使成就功能可以工作, 确保你连接 Game Services 到你的游戏, 也确保游戏处于 "Published" 状态. 更多信息参考 [achievements documentation](https://developers.google.com/games/services/common/concepts/achievements)

### 排行榜
要使排行榜功能可以工作, 确保你连接 Game Services 到你的游戏, 也确保游戏处于 "Published" 状态. 更多信息参考 [leaderboards documentation](https://developers.google.com/games/services/common/concepts/leaderboards)

### 保存的游戏
Saved Games 特征需要在初始化阶段调用 `EnableSnapshots()` 打开，也需要在 Google Play Developer Console 中允许这个功能. 更多信息参考 [Saved Games documentation](https://developers.google.com/games/services/common/concepts/savedgames)

### 实时多人游戏
要使实时多人游戏功能可以工作, 确保你连接 Game Services 到你的游戏, 也确保游戏处于 "Published" 状态. 更多信息参考 [Real-time Multiplayer documentation](https://developers.google.com/games/services/common/concepts/realtimeMultiplayer)

### 回合多人游戏
要使回合多人游戏功能可以工作, 确保你连接 Game Services 到你的游戏, 也确保游戏处于 "Published" 状态. 更多信息参考 [Turn-based Multiplayer documentation](https://developers.google.com/games/services/common/concepts/turnbasedMultiplayer)

### 事件和任务
要使事件和任务功能可以工作, 确保你连接 Game Services 到你的游戏, 也确保游戏处于 "Published" 状态. 更多信息参考 [Events and Quests documentation](https://developers.google.com/games/services/common/concepts/quests)

### 玩家统计
玩家统计添加有用的分析数据到你的游戏. 更多信息参考 [Player Stats documentation](https://developers.google.com/games/services/cpp/stats) 

### 临近连接
临近连接允许本地多人游戏和屏幕广播. 更多信息参考 [Nearby documentation](https://developers.google.com/games/services/cpp/nearby)
