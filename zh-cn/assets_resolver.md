# AssetsResolver

 类似于`LibrariesResovler`， `AssetsResolver` 帮助你获得 Minecraft 资产文件。

## 获取Assets

`AssetsResolver` 用法：


```cs
var assets = AssetsResolver.GetAssets(minecraft);
```

或者使用异步方法，它将自动下载缺少的资产索引文件：


```cs
var assets = await AssetsResolver.GetAssetsAsync(minecraft);
```

?> `AssetsResolver` 对 `MinecraftEntry` 有上述扩展方法。

## 用法

单个 `AssetEntry` 实例的属性包括：

- `File`： `FileInfo` assets文件的对象。
- `RelativeUrl`：assets的相对 url，你可以手动组合它以获得下载 url。
- `IsLegacy`：表示此条目用于旧版 Minecraft（例如`1.7.2`） 。
- `MapToResource`：表示此条目应映射到 `.minecraft/resources` ，仅适用于之前的 Minecrafts `1.6`。
- `Hash`：sha1 条目的哈希值，在较新版本的 Minecraft 中，这是资源的文件名。

你可以抓取下载网址：

```cs
var url = assetEntry.GetDownloadUrl();
```

或指定其他下载源：


```cs
var url = assetEntry.GetDownloadUrl(DownloadSource.Bmcl);
```