# <a name="aspnet-core-authorization-sample"></a>ASP.NET Core 授權範例

這個範例會說明使用 Razor Pages 授權慣例。 這個範例會示範中所述的功能[Razor Pages 授權慣例](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization)主題。

在此範例中的使用者授權會使用 cookie 驗證中所述的功能[使用沒有 ASP.NET Core 身分識別的 cookie 驗證](https://docs.microsoft.com/aspnet/core/security/authentication/cookie)主題。 如需使用 ASP.NET Core 身分識別資訊，請參閱<xref:security/authentication/identity>。

當執行範例，請使用電子郵件地址**maria.rodriguez@contoso.com**來驗證使用者。

## <a name="examples-in-this-sample"></a>這個範例中的範例

| 功能 | 描述 |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | 新增[AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)頁面使用指定的路徑。 |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | 新增[AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有具有指定路徑的資料夾中的頁面。 |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | 新增[AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)頁面，以使用指定的路徑。 |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | 新增[AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)所有具有指定路徑的資料夾中的頁面。 |
