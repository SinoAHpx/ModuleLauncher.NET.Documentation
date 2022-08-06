# SkinUtils

管理皮肤。

此类中的所有方法：
- **需要Access Token**；
- 执行后返回新的 Minecraft 配置文件；
- 是异步方法；
- 应谨慎使用。

如果方法可以接受一个参数 `skinVariant`，一般该参数是对两个值的枚举：`Classic` （Steve 风格）和 `Slim`（Alex 风格）。

## 按外观文件 url 更改外观

更改皮肤，传递皮肤文件的 url。

*不推荐，很有可能失败。*


```cs
var rofile = await SkinUtils.ChangeSkinAsync("access token", "file url", SkinVariant.Slim);
```

## 按本地文件更改外观


```cs
var profile = await SkinUtils.ChangeSkinAsync("access token", new FileInfo("skin path"), SkinVariant.Classic);
```

## 重置外观

只需删除当前活动的皮肤并重置为 Steve。


```cs
var profile = await SkinUtils.ResetSkinAsync("access token");
```

## 隐藏斗篷

如果当前活动蒙皮具有斗篷，则隐藏。


```cs
var profile = await SkinUtils.HideCapeAsync("access token");
```

## 展示披肩

显示隐藏斗篷（如果有）。


```cs
var profile = await SkinUtils.ShowCapeAsync("access token", "cape id");
```

?> 你可以通过侧写来获取斗篷 id。