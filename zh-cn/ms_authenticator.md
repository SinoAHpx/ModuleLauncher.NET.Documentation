# Microsoft 身份验证器

在 2020 年底，Mojang 开始将传统 Yggdrasil 身份验证方案的旧 Mojang 帐户迁移到使用 OAuth 身份验证方案的新 Microsoft 帐户。显然，大多数 Minecraft 玩家都做了这种迁移，这就是为什么这个库不提供旧的 yggdrasil 实现。

## 配置客户端 id 和重定向 url

Microsoft 身份验证（也称为 OAuth 2.0） `redirect url` 需要 `client id` 完成一个授权，在这个库中，实际上你不需要在 Azure 中注册一个应用程序 [wiki.vg](https://wiki.vg/Microsoft_Authentication_Scheme)，因为我们可以使用 Mojang 的客户端 ID。当然了，如果你愿意，也可以提供你自己的。

### 使用 Mojang 的ID

你只需初始化 MicrosoftAuthenticator 的实例，如下所示。


```cs
var authenticator = new MicrosoftAuthenticator();
```

### 使用你自己的ID

我们不谈如何在 Azure 中注册一个应用程序，如果你想选择这种方式，我确信你有这个能力。

你可以指定 Azure 客户端 id 和重定向 url，但不需要客户端密码：


```cs
var authenticator = new MicrosoftAuthenticator
{
    ClientId = "homo-114514",
    RedirectUrl = "xxxx"
};
```

## 取得 `Code`

除了客户端 id 和重定向 url 之外，最后需要的是 `code`。ModuleLauncher 在 `MicrosoftAuthenticator` 提供了一个属性 `LoginUrl` 。打开此 url 并完成登录过程后，它将重定向到如下 url：

```
https://login.live.com/oauth20_desktop.srf?code=M.R3_BAY.76e1c5a6-d22d-bc6e-c027-3c8a6c1d4e17&lc=1033
```

?> 在这里， `https://login.live.com/oauth20_desktop.srf` 是 `RedirectUrl` 属性的默认值。

这个 `code` url 中的参数就是我们想要的代码，库提供了一个字符串扩展方法 `ExtractCode` 来提取代码参数。

## 执行身份验证

若要执行，只要从 `MicrosoftAuthenticator` 实例调用 `AuthenticateAsync` 即可。

```cs
var result = await authenticator.AuthenticateAsync();
```

此方法的返回值是一个， `AuthenticateResult` 它包含：

- Name（名称）
- UUID（唯一标识符）
- AccessToken（访问授权密钥）
- RefreshToken（刷新密钥）
- ExpireIn（过期时间）

如你所见，有一个 `ExpireIn` 属性，它是 `TimeSpan`，指出旧的存取 Token 过期后，你应该在何时调用 `RefreshAuthenticateAsync` 来取得新的 AccessToken。