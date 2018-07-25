# <a name="cookie-sharing-sample-app"></a>Cookie 共用範例應用程式

此範例會說明在使用 cookie 驗證的三個應用程式之間共用的 cookie:

| 專案                             | 描述 |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core Razor 頁面應用程式，而不使用 ASP.NET Core 身分識別 |
| CookieAuthWithIdentity.Core         | 使用 ASP.NET Core 身分識別的 ASP.NET Core MVC 應用程式 |
| CookieAuthWithIdentity.NETFramework | 使用 ASP.NET Identity 的 ASP.NET Framework MVC 應用程式 |

指示：

1. 執行 CookieAuth.Core 應用程式。 註冊的使用者。 註冊使用者時，應用程式會驗證使用者。 將使用者登出。
1. 相同的瀏覽器工作階段中，執行 CookieAuthWithIdentity.Core 應用程式。 使用與核心應用程式註冊相同的使用者。 註冊使用者時，應用程式會驗證使用者。 將使用者登出。
1. 相同的瀏覽器工作階段中，執行 CookieAuthWithIdentity.NETFramework 應用程式。 使用與其他應用程式註冊相同的使用者。 註冊使用者時，應用程式會驗證使用者。 將使用者登出。
1. 三個應用程式的任何使用者登入。 驗證 cookie 會在應用程式間共用。 請注意，使用者會自動登入的其他兩個應用程式。
1. 從任何應用程式，將使用者登入。 請注意，使用者會自動登出其他兩個應用程式。

這個範例會示範中所述的功能[應用程式間共用 cookie](https://docs.microsoft.com/aspnet/core/security/cookie-sharing)主題。
