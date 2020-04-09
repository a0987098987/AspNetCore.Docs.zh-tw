ASP.NET核心識別將使用者介面 (UI) 登錄功能添加到ASP.NET核心 Web 應用。 要保護 Web API 和 SPA,請使用以下之一:

* [Azure Active Directory](/azure/api-management/api-management-howto-protect-backend-with-aad)
* [Azure 活動目錄 B2C(Azure](/azure/active-directory-b2c/active-directory-b2c-custom-rest-api-netfw) AD B2C)*
* [IdentityServer4](https://identityserver.io)

標識伺服器4 是一個 OpenID 連接和 OAuth 2.0 框架,用於ASP.NET核心 3.0。 識別Server4支援以下安全功能:

* 為服務身份驗證 (AaaS)
* 跨多個應用程式類型的單一登入 /關閉 (SSO)
* API 的存取控制
* 聯合閘道

有關詳細資訊,請參閱[歡迎使用識別伺服器4](http://docs.identityserver.io/en/latest/index.html)。