---
title: "ASP.NET Core 安全性概觀 | Microsoft Docs"
author: rachelappel
description: "了解 ASP.NET Core 的驗證、授權和安全性基本概念"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: f6a1f32c1edd098b0782fd066d8e32f09952a9b7
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-security-overview"></a>ASP.NET Core 安全性概觀

ASP.NET Core 可讓開發人員輕鬆設定應用程式的安全性並進行管理。 ASP.NET Core 包含針對認證、授權、資料保護、強制使用 SSL、應用程式密碼、防偽要求保護及 CORS 的管理功能。 這些安全性功能可讓您打造強固又安全的 ASP.NET Core 應用程式。 

## <a name="aspnet-core-security-features"></a>ASP.NET Core 安全性功能

ASP.NET Core 提供許多工具和程式庫來保護應用程式的安全，包括內建的識別提供者，但您也可以使用協力廠商的識別服務 (如 Facebook、Twitter 或 LinkedIn)。 使用 ASP.NET Core 時，您可以輕鬆管理應用程式密碼，以便使用機密資訊而不在程式碼中公開這些資訊。 

## <a name="authentication-vs-authorization"></a>驗證與授權

驗證是一道程序，其會將使用者提供的認證與作業系統、資料庫、應用程式或資源中儲存的認證進行比對。 如果相符的話，使用者就能成功通過驗證，並可執行他們在授權程序期間取得授權的動作。 授權則是決定使用者得以執行哪些動作的程序。 

您也可以將驗證想成是進入伺服器、資料庫、應用程式或資源這類空間的方法，而授權則表示使用者得以針對空間 (伺服器、資料庫或應用程式) 內哪些物件執行哪些動作。

## <a name="common-vulnerabilities-in-software"></a>軟體的常見弱點

ASP.NET Core 和 EF 包含的功能有助您保護應用程式的安全，並防範安全性缺口的發生。 下列連結清單可帶您前往如何避免 Web 應用程式中最常見資訊安全漏洞的技術詳細文件：

* [Cross site scripting attacks](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting) (跨網站指令碼攻擊)
* [SQL Injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql) (SQL 插入式攻擊)
* [Cross-Site Request Forgery (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery) (跨網站偽造要求 (CSRF))
* [Open redirect attacks](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects) (開啟重新導向攻擊)

除此之外，還有許多弱點是您必須提防的。 如需詳細資訊，請參閱本文件的相關章節 (位於 *ASP.NET 安全性文件*)。 

## <a name="aspnet-security-documentation"></a>ASP.NET 安全性文件

*   [驗證](authentication/index.md)
    *   [身分識別簡介](authentication/identity.md)
    *   [使用 Facebook、Google 和其他外部提供者啟用驗證](authentication/social/index.md)
    * [使用 Windows 驗證](authentication/windowsauth.md)
    *   [帳戶確認和密碼復原](authentication/accconfirm.md)
    *   [使用 SMS 的雙因素驗證](authentication/2fa.md) 
    *   [使用沒有身分識別的 Cookie 驗證](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrate Azure AD Into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/) (將 Azure AD 整合到 ASP.NET Core Web 應用程式)
        *   [Call a ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/) (使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API)
        *   [Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/) (使用 Azure AD 在 ASP.NET Core Web 應用程式中呼叫 Web API)
        *   [使用 Azure AD B2C 的 ASP.NET Core Web 應用程式](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [使用 IdentityServer4 保護 ASP.NET Core 應用程式](https://identityserver4.readthedocs.io)
*   [授權](authorization/index.md)
    *   [簡介](authorization/introduction.md)
    *   [建立使用者資料受授權保護的應用程式](xref:security/authorization/secure-data)
    *   [簡單授權](authorization/simple.md)
    *   [角色型授權](authorization/roles.md)
    *   [宣告式授權](authorization/claims.md)
    *   [原則式授權](authorization/policies.md)
    *   [要求處理常式中的相依性插入](authorization/dependencyinjection.md)
    *   [資源型授權](authorization/resourcebased.md)
    *   [檢視型授權](authorization/views.md)
    *   [以配置限制身分識別](authorization/limitingidentitybyscheme.md)
*   [資料保護](data-protection/index.md)
    *   [資料保護簡介](data-protection/introduction.md)
    *   [資料保護 API 使用者入門](data-protection/using-data-protection.md)
    *   [取用者 API](data-protection/consumer-apis/index.md)
        *   [取用者 API 概觀](data-protection/consumer-apis/overview.md)
        *   [目的字串](data-protection/consumer-apis/purpose-strings.md)
        *   [目的階層和多租用戶](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [密碼雜湊](data-protection/consumer-apis/password-hashing.md)
        *   [限制受保護承載的存留期](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [取消索引鍵已撤銷之承載的保護](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [組態](data-protection/configuration/index.md)
        *   [設定資料保護](data-protection/configuration/overview.md)
        *   [預設設定](data-protection/configuration/default-settings.md)
        *   [整個電腦的原則](data-protection/configuration/machine-wide-policy.md)
        *   [非 DI 感知案例](data-protection/configuration/non-di-scenarios.md)
    *   [擴充性 API](data-protection/extensibility/index.md)
        *   [核心加密擴充性](data-protection/extensibility/core-crypto.md)
        *   [金鑰管理擴充性](data-protection/extensibility/key-management.md)
        *   [其他 API](data-protection/extensibility/misc-apis.md)
    *   [實作](data-protection/implementation/index.md)
        *   [已驗證的加密詳細資料](data-protection/implementation/authenticated-encryption-details.md)
        *   [子機碼衍生和驗證的加密](data-protection/implementation/subkeyderivation.md)
        *   [內容標頭](data-protection/implementation/context-headers.md)
        *   [金鑰管理](data-protection/implementation/key-management.md)
        *   [金鑰儲存體提供者](data-protection/implementation/key-storage-providers.md)
        *   [待用時加密金鑰](data-protection/implementation/key-encryption-at-rest.md)
        *   [金鑰的不變性和變更設定](data-protection/implementation/key-immutability.md)
        *   [金鑰儲存體格式](data-protection/implementation/key-storage-format.md)
        *   [暫時資料保護提供者](data-protection/implementation/key-storage-ephemeral.md)
    *   [相容性](data-protection/compatibility/index.md)
        *   [應用程式間共用 Cookie](data-protection/compatibility/cookie-sharing.md)
        *   [取代 ASP.NET 中的 <machineKey>](data-protection/compatibility/replacing-machinekey.md)
*   [建立使用者資料受授權保護的應用程式](xref:security/authorization/secure-data)
*   [在開發期間安全儲存應用程式密碼](app-secrets.md)
*   [Azure Key Vault 組態提供者](key-vault-configuration.md)
*   [強制使用 SSL](enforcing-ssl.md)
*   [防偽要求](anti-request-forgery.md)
*   [防止開啟重新導向攻擊](preventing-open-redirects.md)
*   [防止跨網站指令碼攻擊](cross-site-scripting.md)
*   [啟用跨原始來源要求 (CORS)](cors.md)
