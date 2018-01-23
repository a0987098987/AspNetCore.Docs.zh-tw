# <a name="cookie-sharing-sample-app"></a>Cookie 共用範例應用程式

範例將說明在使用 cookie 驗證的三個應用程式之間共用的 cookie:

| 專案                             | 描述 |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core 2.0 Razor 頁面應用程式，而不需要使用 ASP.NET Core 身分識別 |
| CookieAuthWithIdentity.Core         | 使用 ASP.NET Core 身分識別的 ASP.NET Core 2.0 MVC 應用程式 |
| CookieAuthWithIdentity.NETFramework | 使用 ASP.NET Identity 的 ASP.NET Framework 4.6.1 MVC 應用程式 |

指示：

1. 執行 CookieAuth.Core 應用程式。 註冊的使用者。 註冊使用者時，應用程式會驗證使用者。 登出使用者。
1. 在相同的瀏覽器工作階段執行 CookieAuthWithIdentity.Core 應用程式。 所用的核心應用程式註冊相同的使用者。 註冊使用者時，應用程式會驗證使用者。 登出使用者。
1. 在相同的瀏覽器工作階段執行 CookieAuthWithIdentity.NETFramework 應用程式。 使用與其他應用程式註冊相同的使用者。 註冊使用者時，應用程式會驗證使用者。 登出使用者。
1. 登入三個應用程式的任何使用者。 應用程式之間共用的驗證 cookie。 請注意，使用者會自動登入其他兩個應用程式。
1. 登出使用者從任何應用程式。 請注意，使用者會自動登出其他兩個應用程式。

這個範例會示範中所述的功能[共用應用程式之間的 cookie](https://docs.microsoft.com/aspnet/core/security/data-protection/compatibility/cookie-sharing)主題。
