# 其他工具类

## CommonUtils

### GetJavaExecutableVersion

获取 Java 可执行文件的版本，可能仅适用于 Windows。

### GetSha1

获取文件的 sha1。

## LauncherUtils

提供方法链风格的方式来启动 Minecraft，详情敬请参阅 [Launcher](/launcher.md)。

## AssetsResolverUtils

### RefreshAssetIndexAsync

刷新资产文件，因为说不准某天它会被更新。


```cs
await minecraftEntry.RefreshAssetIndexAsync();
```

### MapAssetsAsync 和 MapAssets

在老版 Minecrafts 中（资产索引是 legacy.json），资产应该映射到具有原始名称的 assets/virtual/legacy，而不是直接使用 sha1 作为名称。而 1.6 版之前的 Minecrafts（资产索引为 1.6.json 之前的版本），资产将映射到. Minecrafts/resources。

这个方法会做这些工作，当你用 ModuleLauncher 启动 Minecraft 时，它会被自动调用。

## MinecraftUtils

### GetMinecraftType

获取 Minecraft 类型，可以是 `Vanilla`、 `Forge`、 `OptiFine`、 `LiteLoader` 和 `Fabric`。

未知类型的 Minecraft 将被视为 `Vanilla` 。

### ExtractNativesAsync

从原生库中提取原生文件，此方法将在启动 Minecraft 时自动调用。