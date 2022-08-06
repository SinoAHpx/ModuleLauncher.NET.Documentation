# SkinUtils

Manage your skin.
All methods in this class:
- **Need access toekn**.
- Return the new Minecraft profile after changing.
- Is asynchronous method.
- Should be carefully used.

If method can accept a parameter `skinVariant`, the parameter is an enum of two value: `Classic` (Steve style) and `Slim` (Alex style).

## Change skin by skin file url

Change skin, passing the url of skin file. *Not recommended, very a chance to be failed.*

```cs
var rofile = await SkinUtils.ChangeSkinAsync("access token", "file url", SkinVariant.Slim);
```

## Change skin by local file

```cs
var profile = await SkinUtils.ChangeSkinAsync("access token", new FileInfo("skin path"), SkinVariant.Classic);
```

## Reset skin

Just remove currently active skin and reset to Steve.

```cs
var profile = await SkinUtils.ResetSkinAsync("access token");
```

## Hide cape

Hide if currently active skin have a cape.

```cs
var profile = await SkinUtils.HideCapeAsync("access token");
```

## Show cape

Show hidden cape if available.

```cs
var profile = await SkinUtils.ShowCapeAsync("access token", "cape id");
```

?> you can grab cape id by getting profile.