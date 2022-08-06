# 首次启动

此处的启动程序只是一个快速入门，有关更详细的文档，请参阅[启动](/launch.md)。

## 获取 MinecraftEntry

参见[术语表](/terminology.md)所述， `MinecraftEntry` 是非常重要的，你可以用它来启动 Minecraft。


```cs
var minecraftResolver = new MinecraftResolver(@".minecraft direcotry");
var minecraft = minecraftResolver.GetMinecraft("Minecraft ID");
```

你可以使用命名空间：

```cs 
using ModuleLauncher.NET.Resources;
```

## 启动

身份验证、Java环境，是启动 Minecraft 所需的唯一两个值。


```cs
var process = await minecraft
    .WithAuthentication("AHpx")
    .WithJava("Some java exe file")
    .LaunchAsync();

// 如果你想检视Minecraft进程的输出信息
while (!process.ReadOutputLine().IsNullOrEmpty())
{
    Console.WriteLine(process.ReadOutputLine());
}
```