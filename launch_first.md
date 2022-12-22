# First time launch

Launching procedures here are just for a very quick start, for a more detailed document, refer [Launch](/launch.md).

## Get MinecraftEntry

As we said in [Terminology](/terminology.md), `MinecraftEntry` is very import, so surely you can use it for launching Minecraft.

```cs
var minecraftResolver = new MinecraftResolver(@".minecraft direcotry");
var minecraft = minecraftResolver.GetMinecraft("Minecraft ID");
```

You may using namespace: 
```cs 
using ModuleLauncher.NET.Resources;
```

## Lauching

Authentication and Java, is the only two value that required for launching Minecraft.

```cs
var process = await minecraft
    .WithAuthentication("AHpx")
    .WithJava("Some java exe file")
    .LaunchAsync(PipeTarget.Null);

// if you would like to check out the Minecraft outputs
// you have to edit "PipeTarget.Null" to:
// e.g. delegate or whatever you want, refer to https://github.com/Tyrrrz/CliWrap#piping
    .LaunchAsync(PipeTarget.ToDelegate(s => { /* do something */ }))
```