# AssetsResolver

Same as `LibrariesResovler`, the `AssetsResolver` helps you getting minecraft assets file.

## Get assets

Use `AssetsResolver`:

```cs
var assets = AssetsResolver.GetAssets(minecraft);
```

Or use asynchronous one, which will automatically download missing asset index file:

```cs
var assets = await AssetsResolver.GetAssetsAsync(minecraft);
```

?> `AssetsResolver` also have extension methods on `MinecraftEntry`

## Consume

This is a single `AssetEntry`:

- `File`: a `FileInfo` object of the assets file.
- `RelativeUrl`: a relative url of the asset, you can combine it manually to get download url.
- `IsLegacy`: indicating this entry is for legacy Minecrafts like `1.7.2`.
- `MapToResource`: indicating this entry should be mapped to `.minecraft/resources`, only for Minecrafts prior `1.6`.
- `Hash`: sha1 hash of the entry, in newer versions of Minecraft, this is the file name of an asset.

You can grab download url:

```cs
var url = assetEntry.GetDownloadUrl();
```

Or specify other download source:

```cs
var url = assetEntry.GetDownloadUrl(DownloadSource.Bmcl);
```