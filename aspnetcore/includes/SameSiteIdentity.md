ASP.NET Core 身分[識別](xref:security/authentication/identity)主要不會受到[SameSite cookie](xref:security/samesite)的影響，除了像是或整合等 advanced 案例 `IFrames` `OpenIdConnect` 。

使用時 `Identity` ，請***不要***新增任何 cookie 提供者或呼叫 ` services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)` ，而 `Identity` 是會負責處理。