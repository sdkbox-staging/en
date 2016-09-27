
##配置 Google Play Services##

当前配置过程是在 Lua 中完成并传递到 Google Play Services. 这个对象在 ```gpg``` Lua 名字空间可用.

要建立一个游戏服务实例，你需要传递一个包含 ```ClientID``` 的表格，这个值在 Google Play Console 中.

```
    local config = {ClientID="..."}
    gpg:CreateGameServices(config)
```

注意 ```ClientID``` 使用的是从 Google Play Console 转发的版本. 这里还有一个保留的版本用在 plist 中.确保你有正确的版本，否则初始化会失败。

##回调##

大部分 Google Play Services 方法通过一个回调来返回结果。这主要是因为异步操作的结果只能在调用后提供。

支持两种类型的回调。类方法回调和函数回调 (包括 lambda 函数)

###类方法示例###

```
ExampleClass:CallbackMethod(result)
    -- use result here
end

local someClass = ExampleClass:new()

gpg:MethodWithCallback({someClass, ExampleClass.CallbackMethod})
```

注意类方法示例里，你必须传递类实例和方法.

###Lambda 函数示例###

```
gpg:MethodWithCallback(function(result)
    -- use result here
end)
```

##授权##

在使用任何 Google Play Services 之前你必须登录. 如果你之前登录过，sdkbox 会尝试自动登录. 你会得到同样的事件，通常来说你是自动登录的。

要开始登录过程，你调用下面的方法并传入回调来接受响应.

```
gpg:StartAuthorizationUI(function(result)
    if result.authStatus == gpg.AuthStatus.VALID then
        -- successfully authenticated
    end
end)
```


##任务 API##

###开始之前###

确保检查 Google Play Services Quests [文档](https://developers.google.com/games/services/common/concepts/quests) 明确如何设置任务并在 API 中引用.

在你游戏开始访问事件和任务之前，你必须首先在 [Google Play Developer Console](https://play.google.com/apps/publish/) 里定义它们。

###提交一个事件###

你发送一个事件到事件服务让它知道发生了什么事. 这不会有返回结果，所以不需要回调.

```
gpg.Events:Increment("<event id>")
```

### 接收一个事件###

要接收事件的计数，使用一个 *Fetch* 方法.

```
gpg.Events:Fetch("<event id>", function(result)
	-- use result.count here
end)

-- or

gpg.Events:FetchAll(function(results)
    for k,v in pairs(results.data) do
        -- k is the event id
        -- use v.count here
    end
end)
```

完整的回调结果成员可以在本文档的相关部分找到。

###显示任务###

Google Play Services 提供一个 UI 来选择任务, 或者你可以提供你自己的 UI 并从回调中使用任务数据.

你可以显示所有可用的任务或者单个任务 UI。

```
gpg.Quests:ShowUI("<quest id>", function(result)
    -- use result.quest here
end)

-- or

gpg.Quests:ShowAllUI(function(result)
    -- use result.quest here
end)
```

###处理接受任务###

如果你的游戏使用内置的任务 UI，回调结果会包含一个有效的 Quest 对象，你可以使用 ```quest.valid()``` 测试它。或者你可以使用自己的 UI，然后调用下列方法来接受任务。

```
gpg.Quests:Accept("<quest id>", function(result)
    -- use result.quest here
end)
```

###处理任务完成###

在用户接受一个任务后，你发送关于任务进度的事件到任务服务.

一旦任务的所有要求都满足，你可以通过内置 UI 或者自己的 UI 索取奖励。

你可以调用 claim 方法.

```
gpg.Quests:ClaimMilestone("<milestone id>", function(result)
    if result.status == gpg.ClaimMilestoneResponse.VALID
        -- use result.milestone.completionRewardData here
    end
end)
```

##玩家统计##

关于玩家统计的完整描述以及如何使用，请参考[这里](https://developers.google.com/games/services/cpp/stats)

###取得当前已登录玩家的统计信息###

你可以像这样取得统计信息.

```
gpg.Stats:FetchForPlayer(function(result)
	if result.status == gpg.FetchForPlayerResponse.VALID then
	    -- use PlayerStats here
	end
end)
```

## 成就

完整文档请参考 [achievements](https://developers.google.com/games/services/common/concepts/achievements)

###状态

成就可以隐藏、已透露和已解锁。

成就可以指定为标准的或渐进的。一般来说，一个渐进的成就涉及到一个玩家在一段较长的时间内逐步取得成就的进展.

### 显示 UI
```
    gpg.Achievements:ShowAllUI(function(result)
		-- handle the result here
    end)
```

### 读取所有成就
```
	gpg.Achievements:FetchAll(nil, function(result)
	    log:d(log:to_str(result))
	end)
```

### 读取指定的成就
```
	gpg.Achievements:Fetch('CgkI6KjppNEWEAIQBQ', nil, function(result)
	    log:d(log:to_str(result))
	end)
```

### 增加成就
```
   gpg.Achievements:Increment('CgkI6KjppNEWEAIQBQ')
```

### 解锁成绩
```
   gpg.Achievements:Unlock('CgkI6KjppNEWEAIQBQ')
```

### 暴露成就
```
	gpg.Achievements:Reveal('CgkI6KjppNEWEAIQBQ')
```

## 排行榜

完整文档请参考 [here](https://developers.google.com/games/services/common/concepts/leaderboards)

### 显示指定排行榜
```
    gpg.Leaderboards:ShowUI("achievement id")
```

### 显示所有排行榜
```
    gpg.Leaderboards:ShowAllUI()
```

### 提交分数
```
    gpg.Leaderboards:SubmitScore("achievement id", score, "meta data", function(result)
    end)
```

### 取得成就的概要
```
    gpg.Leaderboards:FetchAllScoreSummaries("achievement id", data source, function(result)
    end)
```

### 取得所有成就
```
    gpg.Leaderboards:FetchAll(datasource, function(result)
    end)
```

### 取得得分页
```
    gpg.Leaderboards:FetchScorePage("achievement id", datasource, time, timespan, collection, maxitmes, function(result)
    end)
```

### 取得下一个得分页
```
    gpg.Leaderboards:FetchNextScorePage(datasource, max items, function(result)
    end)
```


