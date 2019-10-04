# <a name="additional-claims-sample-app"></a>其他宣告範例應用程式

範例應用程式會示範如何：

* 從 Google 取得使用者名字和姓氏，並使用 Google 提供的值來儲存名稱宣告。
* 將 Google 存取權杖儲存在使用者的 `AuthenticationProperties` 中。

若要使用範例應用程式：

1. 註冊應用程式，並取得有效的用戶端識別碼和用戶端密碼以進行 Google 驗證。 如需詳細資訊，請參閱[Google external login 設定](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins)。
1. 將用戶端識別碼和用戶端秘密提供給[GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)中的應用程式（`Startup.ConfigureServices`）。
1. 執行應用程式並要求 [我的宣告] 頁面。 當使用者未登入時，應用程式會重新導向至 Google。 使用 Google 登入。 Google 會將使用者重新導向回到應用程式（`/MyClaims`）。 使用者已通過驗證，並已載入 [我的宣告] 頁面。 指定的名稱和姓氏宣告會出現在**使用者宣告**底下，並具有 Google 所提供的值。 存取權杖會顯示在 [**驗證屬性**] 之下。
