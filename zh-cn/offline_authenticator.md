# 离线身份验证器

这里没啥可说的，通常这个类是不用的，你可以简单地用表示用户名的字符串实例化本类。

## Authenticate


```cs
var authenticator =  new OfflineAuthenticator("Name");
var result = authenticator.Authenticate();
```