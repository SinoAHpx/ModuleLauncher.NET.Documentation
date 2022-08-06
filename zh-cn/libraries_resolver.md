# 库解析程序

`LibrariesResolver` 同时提供静态方法和实例方法，你不需要实例化 `LibrariesResolver` 如果你可以方便地获取到 `MinecraftEntry`。

## 获取 Libraries

用途 `LibrariesResolver`：


```cs
var libraries = LibrariesResolver.GetLibraries(minecraft);
```

使用扩展方法 `MinecraftEntry`：


```cs
var libraries = minecraft.GetLibraries();
```

## 用法

这是一个单一的 `LibraryEntry`：

- `File`： `FileInfo` 对象。指示库文件。
- `IsNative`：布尔值。指示条目是否是需要提取的 Native库（当然，你不必手动提取）。
- `Type`：枚举对象。指示此 Minecraft 条目属于什么类型，列表中可能会有不同类型的库，因为 Minecraft 的 Forge 或 Fabric json 缺少一些库。
- `RelativeUrl`：库的相对 url，可以手动组合得到下载 url。

你可以抓取下载网址：


```cs
var url = libraryEntry.GetDownloadUrl();
```

或指定其他下载源：


```cs
var url = libraryEntry.GetDownloadUrl(DownloadSource.Bmcl);
```