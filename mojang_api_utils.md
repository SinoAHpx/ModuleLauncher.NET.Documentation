# MojangApiUtils

This utils class following [wiki.vg](https://wiki.vg/Mojang_API) to implement, yet some of APIs have not been covered.

All methods in this class is static.

## Get UUID by name

You can get player's UUID by their name (if specified player does not exist, an exception will be thrown):
```cs
var UUID = await MojangApiUtils.GetUuidByUsernameAsync("name");
```

Or you can get multiple UUIDs one time:

```cs
var UUIDs = await MojangApiUtils.GetUuidsByUsernamesAsync("name1", "name2");
```

?> parameters of `GetUuidsByUsernamesAsync` can be both `IEnumberable<string>` and `params string[]`

## Get name history by UUID

Needs no authentication, you can grab player's name history by their UUID:

```cs
var history = await MojangApiUtils.GetNameHistoryByUuidAsync("UUID");
```

?> return value of `GetNameHistoryByUuidAsync` is a tuple, which item1 is name, item2 is when changed to this name.

## Get profile by UUID

A player's profile contains their username, UUID and account assets(skins and capes). You can grab it by UUID without authentication:

```cs
var profile = await MojangApiUtils.GetProfileByUuidAsync("UUID");
```

## Get profile name change information

**Needs access token.**

This method returns a tuple, item1 is when's profile name changed, item2 is when's profile created, item3 is whether allowed to change name currently.

```cs
var changeInfo = await MojangApiUtils.GetProfileNameChangeInfoAsync("access token");
```

## Change username

**Needs access token.**

Unbelievable, but you can literally change your Minecraft name with code:

```cs
var newProfile = await MojangApiUtils.ChangeUsernameAsync("access token", "new name");
```

?> this method returns new Minecraft profile after name changed.

## Check if certain name is available to change to

**Needs access token.**

```cs
var isAvailable = await MojangApiUtils.CheckNameAvailabilityAsync("access token", "name");
```