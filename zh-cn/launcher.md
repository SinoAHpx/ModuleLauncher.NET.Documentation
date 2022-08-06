# 启动

在这之前，我们假设你在一个现有的.minecraft目录中启动一个典型Minecraft版本。如果你没有，敬请参阅 [Downloads]() 一节。

ModuleLauncher 提供了两种启动风格：
- 传统方法：实例化 `Launcher` 类并配置启动偏好设置，然后调用 `LaunchAsync` 方法。
- 方法链：你不需要显式初始化属 `Launcher` 的对象，你可以从 `MinecraftEntry` 开始配置启动偏好设置，链的末端也是 `LaunchAsync` 方法。

## 传统方式

在这里，我们显式赋值了所有属性，但实际上最重要的是其中两个：

- `Authentication`：Minecraft 玩家信息。值得注意的是，这个属性的实际类型是 `AuthenticateResult`，它有一个字符串的隐式转换器，所以这里我们只传递一个字符串即可。
- `Javas`：`MinecraftJava` 的列表。表示 Minecraft 的 Java 运行时环境。


```cs
var launcher = new Launcher
{
    MinecraftResolver = @"some .minecraft path",
    LauncherConfig = new LauncherConfig
    {
        Javas = new List<MinecraftJava>
        {
            new MinecraftJava
            {
                Executable = new FileInfo(@"path of java exe file"),
                Version = 17
            }
        },
        LauncherName = "name of your launcher",
        MaxMemorySize = 1024,
        MinMemorySize = 512,
        Authentication = "you",
        WindowHeight = 900,
        WindowWidth = 1600,
        Fullscreen = true
    }
};

var process = await launcher.LaunchAsync("minecraft id");
```

`LaunchAsync` 方法是一个异步的方法，所以你必须使用一个 `await` 修饰符来正确地调用它。它返回一个 `Process` 实例，显然是 Minecraft 的进程。你可以拿它来搞搞事儿，比如读一下它的输出：


```cs
while (!process.ReadOutputLine().IsNullOrEmpty())
{
    Console.WriteLine(process.ReadOutputLine());
}
```


?> `ReadOutputLine` 此处是由库 `Manganese` 提供的。

## 方法链

这种方法比上面的方法简单得多，但相应地可能缺少一些特性功能。而且它对于Null值是安全的，你可以方便地在 GUI 应用程序中使用它。


```cs
var minecraftResolver = new MinecraftResolver(@".minecraft direcotry");
var minecraft = minecraftResolver.GetMinecraft("Minecraft ID");

var process = await minecraft
    .WithAuthentication("AHpx")
    .WithJava("Some java exe file")
    .LaunchAsync();
```

!> `WithJava` 和 `WithJavas` 只需要 java exe 文件的路径，但可能只适用于 Windows 。