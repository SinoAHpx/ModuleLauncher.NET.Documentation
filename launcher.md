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

var result = await launcher.LaunchAsync("minecraft id", PipeTarget.Null);
```

`LaunchAsync` method is an asynchronous method as you can see, so you have to use an `await` modifier to corretly invoke it. It returns a `CommandResult` instance, is certainly the process of Minecraft, you can do something with it, for an example, get its exit code:

```cs
Console.WriteLine(result.ExitCode));
```

In the version above `4.0.7`, ModuleLauncher uses `CliWrap` instead of native `Process`, so you have to make some change to read the Minecraft output lines, just change `PipeTarget.Null` above to whatever you want that [CliWrap](https://github.com/Tyrrrz/CliWrap#piping) supported. For example, use an `Action` to receive the output:

```cs
var process = await minecraft
    .WithAuthentication("AHpx")
    .WithJava("Some java exe file")
    .LaunchAsync(PipeTarget.ToDelegate(s => { /* do something */ }));
```

## Method chain

This way is much simpler than above one, but correspondingly may missing some features. And it's safe for null values, you can conveniently use it in a GUI application.

```cs
var minecraftResolver = new MinecraftResolver(@".minecraft direcotry");
var minecraft = minecraftResolver.GetMinecraft("Minecraft ID");

var process = await minecraft
    .WithAuthentication("AHpx")
    .WithJava("Some java exe file")
    .LaunchAsync(PipeTarget.Null);
```

!> `WithJava` and `WithJavas` procedures that only need file path of java exe file may only work on Windows.

!> To read the output, it is same as above, just pass a `PipeTarget` instance