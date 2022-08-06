# MojangApiUtils

本实用类遵循 [wiki.vg](https://wiki.vg/Mojang_API) 来实现，但是部分 API 的实现还没有被涵盖在内。

此类所有方法均是静态的。

## 按名称获取 UUID

你可以通过玩家的名字获取他们的 UUID（如果指定的玩家不存在，将抛出异常）：

```cs
var UUID = await MojangApiUtils.GetUuidByUsernameAsync("name");
```

或者，你可以一次获得多个 UUID：


```cs
var UUIDs = await MojangApiUtils.GetUuidsByUsernamesAsync("name1", "name2");
```

?> `GetUuidsByUsernamesAsync` 的参数，可以是 `IEnumberable<string>` 也可以是 `params string[]`

## 按 UUID 获取名称历史记录

无需身份验证，你可以通过玩家的 UUID 获取其姓名历史记录：


```cs
var history = await MojangApiUtils.GetNameHistoryByUuidAsync("UUID");
```

?> `GetNameHistoryByUuidAsync` 的返回值是一个元组，其中 item1 是名称，item2 是更改为该名称的时间。

## 按 UUID 获取配置文件

玩家的个人资料包含他们的用户名、UUID 和帐户资产（皮肤和披风）。你可以通过 UUID 获取它而无需身份验证：

```cs
var profile = await MojangApiUtils.GetProfileByUuidAsync("UUID");
```

## 获取配置文件名称更改信息

**需要访问令牌。**

该方法返回一个元组，item1 是用户的配置文件名称更改的时间，item2 是用户的配置文件创建的时间，item3 是当前是否允许更改名称。

```cs
var changeInfo = await MojangApiUtils.GetProfileNameChangeInfoAsync("access token");
```

## 更改用户名

**需要访问令牌。**

难以置信，但你就是可以用代码改变你的 Minecraft 名称：

```cs
var newProfile = await MojangApiUtils.ChangeUsernameAsync("access token", "new name");
```

?> 此方法在名称更改后返回新的 Minecraft 配置文件。

## 检查是否可以更改某个名称

**需要访问令牌。**

```cs
var isAvailable = await MojangApiUtils.CheckNameAvailabilityAsync("access token", "name");
```