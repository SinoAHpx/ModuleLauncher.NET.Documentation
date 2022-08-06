# LibrariesResolver

`LibrariesResolver` provides both static method and instanced method, you don't need to instance `LibrariesResolver` if you are convenient to get `MinecraftEntry`.

## Get libraries

Use `LibrariesResolver`:

```cs
var libraries = LibrariesResolver.GetLibraries(minecraft);
```

Use extension method for `MinecraftEntry`:

```cs
var libraries = minecraft.GetLibraries();
```

## Consume

This is a single `LibraryEntry`:

- `File`: a `FileInfo` object of the library file.
- `IsNative`: indicating if the entry is a native library, which needs to be extacted (of course you don't have to do it manually).
- `Type`: what type of Minecraft this entry belongs to, there's might be different kind of libraries in a lis since a Forge or Fabric json of Minecraft missing some libraries.
- `RelativeUrl`: a relative url of the library, you can combine it manually to get download url.

You can grab download url:

```cs
var url = libraryEntry.GetDownloadUrl();
```

Or specify other download source:

```cs
var url = libraryEntry.GetDownloadUrl(DownloadSource.Bmcl);
```