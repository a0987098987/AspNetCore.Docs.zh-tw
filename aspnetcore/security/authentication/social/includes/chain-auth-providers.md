## <a name="multiple-authentication-providers"></a><span data-ttu-id="9de3c-101">多個驗證提供者</span><span class="sxs-lookup"><span data-stu-id="9de3c-101">Multiple authentication providers</span></span>

<span data-ttu-id="9de3c-102">當應用程式需要多個提供者時，請在 [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication) 之後鏈結提供者擴充方法：</span><span class="sxs-lookup"><span data-stu-id="9de3c-102">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
