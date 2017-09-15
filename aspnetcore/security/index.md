---
title: "安全性"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: e8045b8804d1e7915cd81d697d43a173e33a7119
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="security"></a>安全性

*   [驗證](authentication/index.md)
    *   [身分識別簡介](authentication/identity.md)
    *   [使用 Facebook、Google 和其他外部提供者啟用驗證](authentication/social/index.md)
    * [使用 Windows 驗證](authentication/windowsauth.md)
    *   [帳戶確認和密碼復原](authentication/accconfirm.md)
    *   [使用 SMS 的雙重關卡驗證](authentication/2fa.md) 
    *   [使用沒有 ASP.NET Core 身分識別的 Cookie 驗證](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [將 Azure AD 整合到 ASP.NET Core Web 應用程式](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [在使用 Azure AD 的 ASP.NET Core Web 應用程式中呼叫 Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [使用 Azure AD B2C 的 ASP.NET Core Web 應用程式](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [使用 IdentityServer4 保護 ASP.NET Core 應用程式](https://identityserver4.readthedocs.io)
*   [授權](authorization/index.md)
    *   [簡介](authorization/introduction.md)
    *   [建立使用者資料受授權保護的應用程式](xref:security/authorization/secure-data)
    *   [簡單授權](authorization/simple.md)
    *   [角色型授權](authorization/roles.md)
    *   [宣告式授權](authorization/claims.md)
    *   [自訂以原則為基礎的授權](authorization/policies.md)
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
        *   [電腦全域原則](data-protection/configuration/machine-wide-policy.md)
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
        *   [共用應用程式之間的 Cookie](data-protection/compatibility/cookie-sharing.md)
        *   [取代 ASP.NET 中的 <machineKey>](data-protection/compatibility/replacing-machinekey.md)
*   [建立使用者資料受授權保護的應用程式](xref:security/authorization/secure-data)
*   [在開發期間安全儲存應用程式密碼](app-secrets.md)
*   [Azure Key Vault 組態提供者](key-vault-configuration.md)
*   [強制執行 SSL](enforcing-ssl.md)
*   [設定 HTTPS 進行開發](https.md)
*   [防偽要求](anti-request-forgery.md)
*   [防止開啟重新導向攻擊](preventing-open-redirects.md)
*   [防止跨站台指令碼攻擊](cross-site-scripting.md)
*   [啟用跨原始來源要求 (CORS)](cors.md)
