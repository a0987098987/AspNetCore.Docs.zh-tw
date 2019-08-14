# <a name="aspnet-core-authorization-sample"></a>ASP.NET Core 授權範例

此範例說明如何使用 Razor Pages 依慣例的授權。 這個範例會示範[Razor Pages 授權慣例](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization)主題中所述的功能。

此範例中的使用者授權使用在[不 ASP.NET Core 身分識別的情況下使用 cookie 驗證](https://docs.microsoft.com/aspnet/core/security/authentication/cookie)主題中所述的 cookie 驗證功能。 本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 身分識別的應用程式。 如需使用 ASP.NET Core 身分識別的相關資訊, 請參閱[ASP.NET Core 上的身分識別簡介](https://docs.microsoft.com/aspnet/core/security/authentication/identity)。

使用電子郵件地址 **maria.rodriguez@contoso.com** , 以任何密碼驗證使用者。 使用者會在`AuthenticateUser` *頁面/帳戶/登入. cshtml .cs*檔案的方法中進行驗證。 在真實世界的範例中, 使用者會針對資料庫進行驗證。

## <a name="examples-in-this-sample"></a>這個範例中的範例

| 功能 | 說明 |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | 將[AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)加入具有指定路徑的頁面。 |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | 將[AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)新增至資料夾中具有指定路徑的所有頁面。 |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | 將[AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)新增至具有指定路徑的頁面。 |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | 將[AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)新增至資料夾中具有指定路徑的所有頁面。 |
