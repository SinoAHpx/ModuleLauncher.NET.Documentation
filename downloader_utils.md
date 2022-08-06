# DownloaderUtils

`DownloadUtils` does not provide download service.

## Get download url

Get download url, this is main usage of this class.

All of `MinecraftEntry`(vanilla only), `LibraryEntry`, `AssetEntry` can invoke an extension method: `GetDownloadUrl`, which can accept `DownloadSource` enum to indicate download mirrors.

```cs
var mcUrl = minecraftEntry.GetDownloadUrl(DownloadSource.Bmcl);
var libUrl = libraryEntry.GetDownloadUrl();
var assetUrl = assetEntry.GetDownloadUrl();
```

### ResolveDownloadSource

In a GUI application, this might be convenient, this method convert a string value like "default" to `DownloadSource` enum:

```cs
var source = "bmcl".ResolveDownloadSource();
var source = "default".ResolveDownloadSource();
var source = "mcbbs".ResolveDownloadSource();
```

## Get remote Miencraft

`RemoteMinecraftEntry` is contrast to `MinecraftEntry`, it represents an entry in the [launcher metadata manifest](http://launchermeta.mojang.com/mc/game/version_manifest_v2.json).

A single `RemoteMinecraftEntry`:

- Id: Minecraft ID.
- Url: url of Minecraft json file.
- ReleaseTime: when does this Minecraft be released.
- Sha1: sha1 hash of Minecraft json.
- Type: Minecraft json type, it's an enum, can be `Release`, `Snapshot`, `OldAlpha` and `OldBeta`.

Get all of them:

```cs
var remoteMinecrafts = await DownloaderUtils.GetRemoteMinecraftsAsync();
```

?> when you firstly invoked `GetRemoteMinecraftsAsync`, result will be stored in a cache variable to improve performance, you can refresh the cache variable by invoking `RefreshRemoteMinecraftsCacheAsync`.

Get single one by id:

```cs
var remoteMinecraft = await DownloaderUtils.GetRemoteMinecraftAsync("1.19");
```

Or you can filter remote Minecrafts to specified types:

```cs
var remoteMinecrafts = await DownloaderUtils
    .GetRemoteMinecraftsAsync()
    .FilterAsync(MinecraftJsonType.OldBeta | MinecraftJsonType.OldAlpha);
```

?> `FilterAsync` also has a synchronous overload, which is an extension method of `List<RemoteMinecraftEntry>` instead of `Task<List<RemoteMinecraftEntry>>`.

## Convert remote Minecraft to local Minecraft

Local Minecraft is represented by `MinecraftEntry`, so essentially it's a convert from `RemoteMinecraftEntry` to `MinecraftEntry`. The library provides two ways to do this.

### ResolveLocalEntryAsync

You need a `RemoteMinecraftEntry` and a `MinecraftResolver` instance to invoke this method:

```cs
var minecraftEntry = await remoteMinecraftEntry.ResolveLocalEntryAsync(minecraftResolver);
```

### GetRemoteMinecraftAndToLocalAsync

It's an extension method on `MinecraftResovler`, accepts a string Minecraft ID. As method signature said, it gets `RemoteMinecraftEntry` and invoke `ResolveLocalEntryAsync`.

```cs
var minecraft = await mcResolver.GetRemoteMinecraftAndToLocalAsync("id");
```

## Validate if a xxEntry needs to re-download

All resouces of vanilla Minecraft provides sha1 checksum, which can be used to check if downloaded file if corrupted, so that you can perform re-download.

Same as `GetDownloadUrl`, there's an extension method `ValidateChecksum` apply for all the resource entries:

```cs
minecraftEntry.ValidateChecksum();
assetEntry.ValidateChecksum();
libraryEntry.ValidateChecksum();
```

If return value if true, means corresponding file is fine, doesn't need to re-download. Otherwise, means it might be corrupted, you should download it again.