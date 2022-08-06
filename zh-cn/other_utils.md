# Other utils

## CommonUtils

### GetJavaExecutableVersion

Get version of Java executable file, perhaps only works on Windows.

### GetSha1

Get sha1 of a file.

## LauncherUtils

Provide method-chain style way to launch Minecraft, see [Launcher](/launcher.md).

## AssetsResolverUtils

### RefreshAssetIndexAsync

Refresh assets file since it might be updated someday.

```cs
await minecraftEntry.RefreshAssetIndexAsync();
```

### MapAssetsAsync & MapAssets

In legacy Minecrafts (assets index is legacy.json), assets should be mapped to assets/virtual/legacy with original name instead of directly using sha1 as name.
And pre-1.6 Minecrafts (assets index is pre-1.6.json), assets will be mapped to .minecraft/resources.

This method will do these jobs, and when you launch Minecraft with ModuleLauncher, it will be automatically invoked.

## MinecraftUtils

### GetMinecraftType

Get Minecraft type, which can be `Vanilla`, `Forge`, `OptiFine`, `LiteLoader`, and `Fabric`.

Unknown type of Minecraft will be considered as vanilla.

### ExtractNativesAsync

Extract natives files from natives libraries, this method will be automatically invoked when launching Minecraft.