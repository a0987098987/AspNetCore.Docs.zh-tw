ASP.NET Core 身分識別會將使用者介面（UI）登入功能新增至 ASP.NET Core web 應用程式。 若要保護 web Api 和 Spa，請使用下列其中一項：

* [Azure Active Directory](/azure/api-management/api-management-howto-protect-backend-with-aad)
* [Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-custom-rest-api-netfw) （Azure AD B2C）]
* [IdentityServer4](https://identityserver.io)

IdentityServer4 是適用于 ASP.NET Core 3.0 的 OpenID Connect 和 OAuth 2.0 架構。 IdentityServer4 可啟用下列安全性功能：

* 驗證即服務（AaaS）
* 多個應用程式類型的單一登入/關閉（SSO）
* Api 的存取控制
* 同盟閘道

如需詳細資訊，請參閱[歡迎使用 IdentityServer4](http://docs.identityserver.io/en/latest/index.html)。