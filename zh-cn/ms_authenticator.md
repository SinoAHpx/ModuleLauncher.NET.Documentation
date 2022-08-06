# MicrosoftAuthenticator

At the end of 2020, Mojang started migrate old Mojang accounts that use legacy Yggdrasil authentication scheme to new Microsoft accounts with OAuth authentication shceme. And apprently most of Minecraft players have done this migration, this is why this library does not provide the old yggdrasil implementation.

## Configure client id and redirect url

Microsoft authentication (aka OAuth 2.0) needs `redirect url` and `client id` to complete a authorization, in this library, actually you don't need to register an application in Azure as [wiki.vg](https://wiki.vg/Microsoft_Authentication_Scheme) said, because we can use Mojang's client id. You can also provide your own if you would love to.

### Use Mojang's

Nothing you need to provide, but just initialize an instance of MicrosoftAuthenticator like this.

```cs
var authenticator = new MicrosoftAuthenticator();
```

### Use your own

We don't talk about how to register an application in Azure, if you want to choose this way, I do believe you have such an ability.

You can specify your azure client id and redirect url, but client secret is not required:

```cs
var authenticator = new MicrosoftAuthenticator
{
    ClientId = "homo-114514",
    RedirectUrl = "xxxx"
};
```

## Get `Code`

Besides client id and redirect url, last thing you need is the `code`. ModuleLauncher provides a property `LoginUrl` in the `MicrosoftAuthenticator`, open this url and complete login in procedures, and it will redirect to a url like:
```
https://login.live.com/oauth20_desktop.srf?code=M.R3_BAY.76e1c5a6-d22d-bc6e-c027-3c8a6c1d4e17&lc=1033
```

?> In here, `https://login.live.com/oauth20_desktop.srf` is the default value of `RedirectUrl` property in the library.

The parameter `code` in this url is the code we want, and the library provides a string extension method `ExtractCode` to extract the code parameter.

## Execute the authentication

To execute, just invoke `AuthenticateAsync` of a MicrosoftAuthenticator instance.
```cs
var result = await authenticator.AuthenticateAsync();
```

Return value of this method is an `AuthenticateResult`, it contains:

- Name
- UUID
- AccessToken
- RefreshToken
- ExpireIn

As you can see, there's an `ExpireIn` property, and it's a `TimeSpan`, indicating when you should invoke `RefreshAuthenticateAsync` to get a new access token since old one is expired.