# Launch

In here, we assuming you'll launch launchable(you've launched it by other launchers before) Minecraft in a existing .minecraft directory. If you don't have one, refer to [Downloads]() section.

ModuleLauncher provices two styles for launching:
- Traiditional way: contruct a `Launcher` object and assign values to the properties, then invoke the `LaunchAsync` method.
- Method chain: you don't have to intiailize a `Launcher` object explicitly, you start with `MinecraftEntry`, and configure values you need, end of the chain is also the `LaunchAsync` method.

## Traditional way

We provided all properties here, but actually only two of them are required:

- `Authentication`: Minecraft player information. Notably actual type of this property is `AuthenticateResult`, it has an implicit converter for strings, so here we simply pass a string.
- `Javas`: a list of `MinecraftJava`, which represents the java runtimes for Minecraft.

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

`LaunchAsync` method is an asynchronous method as you can see, so you have to use an `await` modifier to corretly invoke it. It returns a `Process` instance, is certainly the process of Minecraft, you can do something with it, for an example, read its output:

```cs
while (!process.ReadOutputLine().IsNullOrEmpty())
{
    Console.WriteLine(process.ReadOutputLine());
}
```


?> `ReadOutputLine` here is provided by the library `Manganese`, you may using namespaces or use native .NET library instead.

## Method chain

This way is much simpler than above one, but correspondingly may missing some features. And it's safe for null values, you can conveniently use it in a GUI application.

```cs
var minecraftResolver = new MinecraftResolver(@".minecraft direcotry");
var minecraft = minecraftResolver.GetMinecraft("Minecraft ID");

var process = await minecraft
    .WithAuthentication("AHpx")
    .WithJava("Some java exe file")
    .LaunchAsync();
```

!> `WithJava` and `WithJavas` procedures that only need file path of java exe file may only work on Windows.