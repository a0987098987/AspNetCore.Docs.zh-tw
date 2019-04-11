## <a name="multiple-authentication-providers"></a>多個驗證提供者

當應用程式需要多個提供者時，請在 [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication) 之後鏈結提供者擴充方法：

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
