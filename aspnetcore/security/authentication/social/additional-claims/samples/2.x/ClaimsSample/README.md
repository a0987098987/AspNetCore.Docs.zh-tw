# <a name="additional-claims-sample-app"></a>額外的宣告範例應用程式

範例應用程式示範如何：

* 從 Google 取得使用者的名字和姓氏，並使用由 Google 提供的值來儲存名稱宣告。
* 將 Google 存取權杖儲存在使用者的`AuthenticationProperties`。

若要使用範例應用程式：

1. 註冊應用程式，並取得有效的用戶端識別碼和 Google 驗證的用戶端祕密。 如需詳細資訊，請參閱 < [Google 外部登入設定](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins)。
1. 提供用戶端識別碼和應用程式中的用戶端祕密[GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)的`Startup.ConfigureServices`。
1. 執行應用程式，並要求我宣告頁面。 當使用者未登入時，應用程式重新導向至 Google。 使用 Google 登入。 Google 使用者重新導向回到應用程式 (`/MyClaims`)。 使用者經過驗證，而且我的宣告頁面載入。 名字和姓氏的宣告會出現下**使用者宣告**由 Google 提供的值。 下方顯示的存取權杖**驗證屬性**。
