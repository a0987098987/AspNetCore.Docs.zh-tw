---
title: ASP.NET Core 安全性概觀
author: tdykstra
description: 了解 ASP.NET Core 的驗證、授權和安全性基本概念。
ms.author: tdykstra
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: 3a1c1ea1ad28fccbe5ae91b0be193938b095f60b
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41746075"
---
# <a name="overview-of-aspnet-core-security"></a>ASP.NET Core 安全性概觀

ASP.NET Core 可讓開發人員輕鬆設定應用程式的安全性並進行管理。 ASP.NET Core 包含針對認證、授權、資料保護、強制使用 SSL、應用程式密碼、防偽要求保護及 CORS 的管理功能。 這些安全性功能可讓您打造強固又安全的 ASP.NET Core 應用程式。

## <a name="aspnet-core-security-features"></a>ASP.NET Core 安全性功能

ASP.NET Core 提供許多工具和程式庫來保護應用程式的安全，包括內建的識別提供者，但您也可以使用協力廠商的識別服務 (如 Facebook、Twitter 或 LinkedIn)。 使用 ASP.NET Core 時，您可以輕鬆管理應用程式密碼，以便使用機密資訊而不在程式碼中公開這些資訊。

## <a name="authentication-vs-authorization"></a>驗證與授權

驗證是一道程序，其會將使用者提供的認證與作業系統、資料庫、應用程式或資源中儲存的認證進行比對。 如果相符的話，使用者就能成功通過驗證，並可執行他們在授權程序期間取得授權的動作。 授權則是決定使用者得以執行哪些動作的程序。

您也可以將驗證想成是進入伺服器、資料庫、應用程式或資源這類空間的方法，而授權則表示使用者得以針對空間 (伺服器、資料庫或應用程式) 內哪些物件執行哪些動作。

## <a name="common-vulnerabilities-in-software"></a>軟體的常見弱點

ASP.NET Core 和 EF 包含的功能有助您保護應用程式的安全，並防範安全性缺口的發生。 下列連結清單可帶您前往如何避免 Web 應用程式中最常見資訊安全漏洞的技術詳細文件：

* [Cross site scripting attacks](xref:security/cross-site-scripting) (跨網站指令碼攻擊)
* [SQL Injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql) (SQL 插入式攻擊)
* [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery) (跨網站偽造要求 (CSRF))
* [Open redirect attacks](xref:security/preventing-open-redirects) (開啟重新導向攻擊)

除此之外，還有許多弱點是您必須提防的。 如需詳細資訊，請參閱 *ASP.NET Core 安全性文件*上本文件的這個章節。

## <a name="aspnet-core-security-documentation"></a>ASP.NET Core 安全性文件

*   [驗證](xref:security/authentication/index)
    *   [身分識別簡介](xref:security/authentication/identity)
    *   [使用 Facebook、Google 和其他外部提供者啟用驗證](xref:security/authentication/social/index)
    *   [以 WS 同盟啟用驗證](xref:security/authentication/ws-federation)
    * [使用 Windows 驗證](xref:security/authentication/windowsauth)
    *   [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
    *   [使用 SMS 的雙因素驗證](xref:security/authentication/2fa)
    *   [使用沒有身分識別的 Cookie 驗證](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integrate Azure AD Into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/) (將 Azure AD 整合到 ASP.NET Core Web 應用程式)
        *   [Call a ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/) (使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API)
        *   [Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/) (使用 Azure AD 在 ASP.NET Core Web 應用程式中呼叫 Web API)
        *   [使用 Azure AD B2C 的 ASP.NET Core Web 應用程式](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [使用 IdentityServer4 保護 ASP.NET Core 應用程式](https://identityserver4.readthedocs.io)
*   [授權](xref:security/authorization/index)
    *   [簡介](xref:security/authorization/introduction)
    *   [建立使用者資料受授權保護的應用程式](xref:security/authorization/secure-data)
    *   [簡單授權](xref:security/authorization/simple)
    *   [角色型授權](xref:security/authorization/roles)
    *   [宣告式授權](xref:security/authorization/claims)
    *   [原則式授權](xref:security/authorization/policies)
    *   [要求處理常式中的相依性插入](xref:security/authorization/dependencyinjection)
    *   [資源型授權](xref:security/authorization/resourcebased)
    *   [檢視型授權](xref:security/authorization/views)
    *   [以配置限制身分識別](xref:security/authorization/limitingidentitybyscheme)
*   [資料保護](xref:security/data-protection/index)
    *   [資料保護簡介](xref:security/data-protection/introduction)
    *   [資料保護 API 使用者入門](xref:security/data-protection/using-data-protection)
    *   [取用者 API](xref:security/data-protection/consumer-apis/index)
        *   [取用者 API 概觀](xref:security/data-protection/consumer-apis/overview)
        *   [目的字串](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [目的階層和多租用戶](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [雜湊密碼](xref:security/data-protection/consumer-apis/password-hashing)
        *   [限制受保護承載的存留期](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [取消索引鍵已撤銷之承載的保護](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [組態](xref:security/data-protection/configuration/index)
        *   [設定資料保護](xref:security/data-protection/configuration/overview)
        *   [預設設定](xref:security/data-protection/configuration/default-settings)
        *   [整個電腦的原則](xref:security/data-protection/configuration/machine-wide-policy)
        *   [非 DI 感知案例](xref:security/data-protection/configuration/non-di-scenarios)
    *   [擴充性 API](xref:security/data-protection/extensibility/index)
        *   [核心加密擴充性](xref:security/data-protection/extensibility/core-crypto)
        *   [金鑰管理擴充性](xref:security/data-protection/extensibility/key-management)
        *   [其他 API](xref:security/data-protection/extensibility/misc-apis)
    *   [實作](xref:security/data-protection/implementation/index)
        *   [已驗證的加密詳細資料](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [子機碼衍生和驗證的加密](xref:security/data-protection/implementation/subkeyderivation)
        *   [內容標頭](xref:security/data-protection/implementation/context-headers)
        *   [金鑰管理](xref:security/data-protection/implementation/key-management)
        *   [金鑰儲存體提供者](xref:security/data-protection/implementation/key-storage-providers)
        *   [待用時加密金鑰](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [金鑰的不變性和設定](xref:security/data-protection/implementation/key-immutability)
        *   [金鑰儲存體格式](xref:security/data-protection/implementation/key-storage-format)
        *   [暫時資料保護提供者](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [相容性](xref:security/data-protection/compatibility/index)
        *   [取代 ASP.NET 中的 <machineKey>](xref:security/data-protection/compatibility/replacing-machinekey)
*   [建立使用者資料受授權保護的應用程式](xref:security/authorization/secure-data)
*   [在開發期間安全儲存應用程式祕密](xref:security/app-secrets)
*   [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)
*   [強制使用 SSL](xref:security/enforcing-ssl)
*   [防偽要求](xref:security/anti-request-forgery)
*   [防止開啟重新導向攻擊](xref:security/preventing-open-redirects)
*   [防止跨網站指令碼攻擊](xref:security/cross-site-scripting)
*   [啟用跨原始來源要求 (CORS)](xref:security/cors)
*   [在應用程式間共用 Cookie](xref:security/cookie-sharing)
