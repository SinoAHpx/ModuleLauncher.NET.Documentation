# DownloadUtils

`DownloadUtils` 不实现下载功能。

## 获取下载 url

获取下载网址，即该类的主要用法。

所有的 `MinecraftEntry`（仅限 vanilla）、 `LibraryEntry`、 `AssetEntry` 都可以调用扩展方法`GetDownloadUrl`。

它可以接受传入的 `DownloadSource` 枚举来指定下载镜像。


```cs
var mcUrl = minecraftEntry.GetDownloadUrl(DownloadSource.Bmcl);
var libUrl = libraryEntry.GetDownloadUrl();
var assetUrl = assetEntry.GetDownloadUrl();
```

### 解析下载源

这可能对于GUI程序很方便，此方法将字符串值（如“default”）转换为 `DownloadSource` 枚举：


```cs
var source = "bmcl".ResolveDownloadSource();
var source = "default".ResolveDownloadSource();
var source = "mcbbs".ResolveDownloadSource();
```

## 获取远程 Miencraft

`RemoteMinecraftEntry` 与 `MinecraftEntry` 相反，它表示 [启动程序元数据清单](http://launchermeta.mojang.com/mc/game/version_manifest_v2.json)。

单个 `RemoteMinecraftEntry`：

- Id：指示Minecraft ID。
- Url：指示Minecraft Json文件的 URL。
- ReleaseTime：指示Minecraft发布时间。
- Sha1：指示Minecraft Json的sha1哈希值。
- Type：指示Minecraft Json 类型。它是一个枚举，可以是 `Release`， `Snapshot`， `OldAlpha` 和 `OldBeta`。

获取所有这些文件：

```cs
var remoteMinecrafts = await DownloaderUtils.GetRemoteMinecraftsAsync();
```

?> 当你第一次调用 `GetRemoteMinecraftsAsync` 时，结果会储存在缓存中以改善效能，你可以调用 `RefreshRemoteMinecraftsCacheAsync` 来刷新缓存。

给定 ID 以获取单个实例：

```cs
var remoteMinecraft = await DownloaderUtils.GetRemoteMinecraftAsync("1.19");
```

或者你可以以指定地类型筛选远程 Minecraft：


```cs
var remoteMinecrafts = await DownloaderUtils
    .GetRemoteMinecraftsAsync()
    .FilterAsync(MinecraftJsonType.OldBeta | MinecraftJsonType.OldAlpha);
```

?> `FilterAsync` 也有同步的重载方法，这是 `List<RemoteMinecraftEntry>` 的扩展方法，而不是 `Task<List<RemoteMinecraftEntry>>` 的。

## 将远程 Minecraft 转换为本地 Minecraft

本地 Minecraft 由 `MinecraftEntry` 描述，所以本质上它是从 `RemoteMinecraftEntry` 转换至 `MinecraftEntry`。该库提供了两种方法来完成此操作。

### ResolveLocalEntryAsync

你需要各属 `RemoteMinecraftEntry`、`MinecraftResolver` 的两个实例来调用这个方法：


```cs
var minecraftEntry = await remoteMinecraftEntry.ResolveLocalEntryAsync(minecraftResolver);
```

### GetRemoteMinecraftAndToLocalAsync

这是一个 `MinecraftResovler` 的扩展方法，接受一个字符串 Minecraft ID。正如方法签名，它获取 `RemoteMinecraftEntry` 并调用 `ResolveLocalEntryAsync`。


```cs
var minecraft = await mcResolver.GetRemoteMinecraftAndToLocalAsync("id");
```

## 验证 xxEntry 是否需要重新下载

vanilla Minecraft 的所有资源都提供 sha1 校验，它可以用来检查下载的文件是否损坏，以便你可以执行重新下载。

类似于 `GetDownloadUrl`，有一个扩展方法 `ValidateChecksum` 适用于所有资源条目：


```cs
minecraftEntry.ValidateChecksum();
assetEntry.ValidateChecksum();
libraryEntry.ValidateChecksum();
```

如果返回值为 true，则表示对应文件没有问题，不需要重新下载；否则，意味着它可能已损坏，你应该重新下载它。