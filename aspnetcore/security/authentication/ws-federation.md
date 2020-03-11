---
title: 在 ASP.NET Core 中使用 WS-同盟來驗證使用者
author: chlowell
description: 本教學課程示範如何在 ASP.NET Core 應用程式中使用 WS-同盟。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: d82421a14ede6cb6b01ef59f233bb2eba6b56aec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655425"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>在 ASP.NET Core 中使用 WS-同盟來驗證使用者

本教學課程示範如何讓使用者使用 WS-同盟驗證提供者（例如 Active Directory 同盟服務（ADFS）或[Azure Active Directory](/azure/active-directory/) （AAD））進行登入。 它會使用[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)中所述的 ASP.NET Core 2.0 範例應用程式。

針對 ASP.NET Core 2.0 應用程式，WS-同盟支援是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)提供。 WsFederation。 此元件是從 Owin 進行移植。 [WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)並共用其中許多元件的機制。 不過，這些元件的差異在於幾個重要的方式。

根據預設，新的中介軟體：

* 不允許未經要求的登入。 WS-同盟通訊協定的這項功能很容易遭受 XSRF 攻擊。 不過，您可以使用 [`AllowUnsolicitedLogins`] 選項來啟用它。
* 不會檢查每個表單張貼的登入訊息。 只會檢查 `CallbackPath` 的要求是否有登入。 `CallbackPath` 預設為 `/signin-wsfed`，但可透過[WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性加以變更。 藉由啟用[SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests)選項，可以與其他驗證提供者共用此路徑。

## <a name="register-the-app-with-active-directory"></a>向 Active Directory 註冊應用程式

### <a name="active-directory-federation-services"></a>Active Directory Federation Services

* 從 ADFS 管理主控台開啟伺服器的 [**新增信賴憑證者信任]** 。

![新增信賴憑證者信任嚮導：歡迎使用](ws-federation/_static/AdfsAddTrust.png)

* 選擇手動輸入資料：

![新增信賴憑證者信任嚮導：選取資料來源](ws-federation/_static/AdfsSelectDataSource.png)

* 輸入信賴憑證者的 [顯示名稱]。 此名稱對 ASP.NET Core 應用程式而言並不重要。

* [AspNetCore，WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)缺少權杖加密的支援，因此請勿設定權杖加密憑證：

![新增信賴憑證者信任嚮導：設定憑證](ws-federation/_static/AdfsConfigureCert.png)

* 使用應用程式的 URL，啟用 WS-同盟被動式通訊協定的支援。 確認此應用程式的埠是否正確：

![新增信賴憑證者信任嚮導：設定 URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> 這必須是 HTTPS URL。 IIS Express 可以在開發期間裝載應用程式時提供自我簽署憑證。 Kestrel 需要手動設定憑證。 如需詳細資訊，請參閱[Kestrel 檔](xref:fundamentals/servers/kestrel)。

* 在嚮導的其餘部分按 [**下一步**]，然後在結尾處**關閉**。

* ASP.NET Core 身分識別需要**名稱識別碼**宣告。 從 [編輯宣告**規則**] 對話方塊新增一個：

![編輯宣告規則](ws-federation/_static/EditClaimRules.png)

* 在 [**新增轉換宣告規則] 嚮導**中，將預設的 [**傳送 LDAP 屬性**] 保留為已選取，然後按 **[下一步]** 。 新增規則，將**SAM-Account-Name** LDAP 屬性對應至**名稱識別碼**傳出宣告：

![新增轉換宣告規則嚮導：設定宣告規則](ws-federation/_static/AddTransformClaimRule.png)

* 在 **編輯宣告規則** 視窗中，按一下 **完成** > **確定** 。

### <a name="azure-active-directory"></a>Azure Active Directory

* 流覽至 AAD 租使用者的 [應用程式註冊] 分頁。 按一下 [**新增應用程式註冊**]：

![Azure Active Directory：應用程式註冊](ws-federation/_static/AadNewAppRegistration.png)

* 輸入應用程式註冊的名稱。 這對 ASP.NET Core 應用程式來說並不重要。
* 在 [登**入 url**] 中輸入應用程式接聽的 url：

![Azure Active Directory：建立應用程式註冊](ws-federation/_static/AadCreateAppRegistration.png)

* 按一下 [**端點**]，並記下 [**同盟元資料檔案**URL]。 這是 WS-同盟中介軟體的 `MetadataAddress`：

![Azure Active Directory：端點](ws-federation/_static/AadFederationMetadataDocument.png)

* 流覽至新的應用程式註冊。 按一下 [**設定**] > [**屬性**]，並記下 [**應用程式識別碼 URI**]。 這是 WS-同盟中介軟體的 `Wtrealm`：

![Azure Active Directory：應用程式註冊屬性](ws-federation/_static/AadAppIdUri.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>使用不含 ASP.NET Core 身分識別的 WS-同盟

您可以使用 WS-同盟中介軟體而不需要身分識別。 例如：
::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon21.cs?name=snippet)]
::: moniker-end

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>新增 WS-同盟做為 ASP.NET Core 身分識別的外部登入提供者

* 新增對 AspNetCore 的相依性。 [WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)至專案。
* 將 WS-同盟新增至 `Startup.ConfigureServices`：

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup21.cs?name=snippet)]
::: moniker-end

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>使用 WS-同盟進行登入

流覽至應用程式，然後按一下導覽標頭中的 [**登入**] 連結。 有一個選項可使用 WsFederation： ![登入頁面登入](ws-federation/_static/WsFederationButton.png)

使用 ADFS 做為提供者時，按鈕會重新導向至 ADFS 登入頁面： ![ADFS 登入頁面](ws-federation/_static/AdfsLoginPage.png)

使用 Azure Active Directory 做為提供者時，按鈕會重新導向至 AAD 登入頁面： ![AAD 登入頁面](ws-federation/_static/AadSignIn.png)

成功登入新的使用者會重新導向至應用程式的使用者註冊頁面： ![註冊 頁面](ws-federation/_static/Register.png)