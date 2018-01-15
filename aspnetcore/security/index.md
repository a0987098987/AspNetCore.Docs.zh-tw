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
# <a name="aspnet-core-security-overview"></a><span data-ttu-id="46fe6-103">ASP.NET Core 安全性概觀</span><span class="sxs-lookup"><span data-stu-id="46fe6-103">ASP.NET Core Security Overview</span></span>

<span data-ttu-id="46fe6-104">ASP.NET Core 可讓開發人員輕鬆設定應用程式的安全性並進行管理。</span><span class="sxs-lookup"><span data-stu-id="46fe6-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="46fe6-105">ASP.NET Core 包含針對認證、授權、資料保護、強制使用 SSL、應用程式密碼、防偽要求保護及 CORS 的管理功能。</span><span class="sxs-lookup"><span data-stu-id="46fe6-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="46fe6-106">這些安全性功能可讓您打造強固又安全的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="46fe6-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span> 

## <a name="aspnet-core-security-features"></a><span data-ttu-id="46fe6-107">ASP.NET Core 安全性功能</span><span class="sxs-lookup"><span data-stu-id="46fe6-107">ASP.NET Core security features</span></span>

<span data-ttu-id="46fe6-108">ASP.NET Core 提供許多工具和程式庫來保護應用程式的安全，包括內建的識別提供者，但您也可以使用協力廠商的識別服務 (如 Facebook、Twitter 或 LinkedIn)。</span><span class="sxs-lookup"><span data-stu-id="46fe6-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="46fe6-109">使用 ASP.NET Core 時，您可以輕鬆管理應用程式密碼，以便使用機密資訊而不在程式碼中公開這些資訊。</span><span class="sxs-lookup"><span data-stu-id="46fe6-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span> 

## <a name="authentication-vs-authorization"></a><span data-ttu-id="46fe6-110">驗證與授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="46fe6-111">驗證是一道程序，其會將使用者提供的認證與作業系統、資料庫、應用程式或資源中儲存的認證進行比對。</span><span class="sxs-lookup"><span data-stu-id="46fe6-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="46fe6-112">如果相符的話，使用者就能成功通過驗證，並可執行他們在授權程序期間取得授權的動作。</span><span class="sxs-lookup"><span data-stu-id="46fe6-112">If they match, users authenticate successfully, and can then perform actions that they are authorized for, during an authorization process.</span></span> <span data-ttu-id="46fe6-113">授權則是決定使用者得以執行哪些動作的程序。</span><span class="sxs-lookup"><span data-stu-id="46fe6-113">The authorization refers to the process that determines what a user is allowed to do.</span></span> 

<span data-ttu-id="46fe6-114">您也可以將驗證想成是進入伺服器、資料庫、應用程式或資源這類空間的方法，而授權則表示使用者得以針對空間 (伺服器、資料庫或應用程式) 內哪些物件執行哪些動作。</span><span class="sxs-lookup"><span data-stu-id="46fe6-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="46fe6-115">軟體的常見弱點</span><span class="sxs-lookup"><span data-stu-id="46fe6-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="46fe6-116">ASP.NET Core 和 EF 包含的功能有助您保護應用程式的安全，並防範安全性缺口的發生。</span><span class="sxs-lookup"><span data-stu-id="46fe6-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="46fe6-117">下列連結清單可帶您前往如何避免 Web 應用程式中最常見資訊安全漏洞的技術詳細文件：</span><span class="sxs-lookup"><span data-stu-id="46fe6-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* <span data-ttu-id="46fe6-118">[Cross site scripting attacks](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting) (跨網站指令碼攻擊)</span><span class="sxs-lookup"><span data-stu-id="46fe6-118">[Cross-site scripting attacks](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)</span></span>
* <span data-ttu-id="46fe6-119">[SQL Injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql) (SQL 插入式攻擊)</span><span class="sxs-lookup"><span data-stu-id="46fe6-119">[SQL injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql)</span></span>
* <span data-ttu-id="46fe6-120">[Cross-Site Request Forgery (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery) (跨網站偽造要求 (CSRF))</span><span class="sxs-lookup"><span data-stu-id="46fe6-120">[Cross-Site Request Forgery (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)</span></span>
* <span data-ttu-id="46fe6-121">[Open redirect attacks](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects) (開啟重新導向攻擊)</span><span class="sxs-lookup"><span data-stu-id="46fe6-121">[Open redirect attacks](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)</span></span>

<span data-ttu-id="46fe6-122">除此之外，還有許多弱點是您必須提防的。</span><span class="sxs-lookup"><span data-stu-id="46fe6-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="46fe6-123">如需詳細資訊，請參閱本文件的相關章節 (位於 *ASP.NET 安全性文件*)。</span><span class="sxs-lookup"><span data-stu-id="46fe6-123">For more information, see the section in this document on *ASP.NET Security Documentation*.</span></span> 

## <a name="aspnet-security-documentation"></a><span data-ttu-id="46fe6-124">ASP.NET 安全性文件</span><span class="sxs-lookup"><span data-stu-id="46fe6-124">ASP.NET Security Documentation</span></span>

*   [<span data-ttu-id="46fe6-125">驗證</span><span class="sxs-lookup"><span data-stu-id="46fe6-125">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="46fe6-126">身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="46fe6-126">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="46fe6-127">使用 Facebook、Google 和其他外部提供者啟用驗證</span><span class="sxs-lookup"><span data-stu-id="46fe6-127">Enable authentication using Facebook, Google, and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="46fe6-128">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="46fe6-128">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="46fe6-129">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="46fe6-129">Account confirmation and password recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="46fe6-130">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="46fe6-130">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="46fe6-131">使用沒有身分識別的 Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="46fe6-131">Use cookie authentication without Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="46fe6-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="46fe6-132">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   <span data-ttu-id="46fe6-133">[Integrate Azure AD Into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/) (將 Azure AD 整合到 ASP.NET Core Web 應用程式)</span><span class="sxs-lookup"><span data-stu-id="46fe6-133">[Integrate Azure AD into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)</span></span>
        *   <span data-ttu-id="46fe6-134">[Call a ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/) (使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API)</span><span class="sxs-lookup"><span data-stu-id="46fe6-134">[Call an ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)</span></span>
        *   <span data-ttu-id="46fe6-135">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/) (使用 Azure AD 在 ASP.NET Core Web 應用程式中呼叫 Web API)</span><span class="sxs-lookup"><span data-stu-id="46fe6-135">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)</span></span>
        *   [<span data-ttu-id="46fe6-136">使用 Azure AD B2C 的 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="46fe6-136">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="46fe6-137">使用 IdentityServer4 保護 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="46fe6-137">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="46fe6-138">授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-138">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="46fe6-139">簡介</span><span class="sxs-lookup"><span data-stu-id="46fe6-139">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="46fe6-140">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="46fe6-140">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="46fe6-141">簡單授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-141">Simple authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="46fe6-142">角色型授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-142">Role-based authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="46fe6-143">宣告式授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-143">Claims-based authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="46fe6-144">原則式授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-144">Policy-based authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="46fe6-145">要求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="46fe6-145">Dependency injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="46fe6-146">資源型授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-146">Resource-based authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="46fe6-147">檢視型授權</span><span class="sxs-lookup"><span data-stu-id="46fe6-147">View-based authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="46fe6-148">以配置限制身分識別</span><span class="sxs-lookup"><span data-stu-id="46fe6-148">Limit identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="46fe6-149">資料保護</span><span class="sxs-lookup"><span data-stu-id="46fe6-149">Data protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="46fe6-150">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="46fe6-150">Introduction to data protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="46fe6-151">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="46fe6-151">Get started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="46fe6-152">取用者 API</span><span class="sxs-lookup"><span data-stu-id="46fe6-152">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="46fe6-153">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="46fe6-153">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="46fe6-154">目的字串</span><span class="sxs-lookup"><span data-stu-id="46fe6-154">Purpose strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="46fe6-155">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="46fe6-155">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="46fe6-156">密碼雜湊</span><span class="sxs-lookup"><span data-stu-id="46fe6-156">Password hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="46fe6-157">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="46fe6-157">Limit the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="46fe6-158">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="46fe6-158">Unprotect payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="46fe6-159">組態</span><span class="sxs-lookup"><span data-stu-id="46fe6-159">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="46fe6-160">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="46fe6-160">Configure data protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="46fe6-161">預設設定</span><span class="sxs-lookup"><span data-stu-id="46fe6-161">Default settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="46fe6-162">整個電腦的原則</span><span class="sxs-lookup"><span data-stu-id="46fe6-162">Machine-wide policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="46fe6-163">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="46fe6-163">Non DI-aware scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="46fe6-164">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="46fe6-164">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="46fe6-165">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="46fe6-165">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="46fe6-166">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="46fe6-166">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="46fe6-167">其他 API</span><span class="sxs-lookup"><span data-stu-id="46fe6-167">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="46fe6-168">實作</span><span class="sxs-lookup"><span data-stu-id="46fe6-168">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="46fe6-169">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="46fe6-169">Authenticated encryption details</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="46fe6-170">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="46fe6-170">Subkey derivation and authenticated encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="46fe6-171">內容標頭</span><span class="sxs-lookup"><span data-stu-id="46fe6-171">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="46fe6-172">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="46fe6-172">Key management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="46fe6-173">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="46fe6-173">Key storage providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="46fe6-174">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="46fe6-174">Key encryption at rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="46fe6-175">金鑰的不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="46fe6-175">Key immutability and changing settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="46fe6-176">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="46fe6-176">Key storage format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="46fe6-177">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="46fe6-177">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="46fe6-178">相容性</span><span class="sxs-lookup"><span data-stu-id="46fe6-178">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="46fe6-179">應用程式間共用 Cookie</span><span class="sxs-lookup"><span data-stu-id="46fe6-179">Share cookies between apps</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="46fe6-180">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="46fe6-180">Replace <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="46fe6-181">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="46fe6-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="46fe6-182">在開發期間安全儲存應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="46fe6-182">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="46fe6-183">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="46fe6-183">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="46fe6-184">強制使用 SSL</span><span class="sxs-lookup"><span data-stu-id="46fe6-184">Enforce SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="46fe6-185">防偽要求</span><span class="sxs-lookup"><span data-stu-id="46fe6-185">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="46fe6-186">防止開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="46fe6-186">Prevent open redirect attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="46fe6-187">防止跨網站指令碼攻擊</span><span class="sxs-lookup"><span data-stu-id="46fe6-187">Prevent Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="46fe6-188">啟用跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="46fe6-188">Enable Cross-Origin Requests (CORS)</span></span>](cors.md)
