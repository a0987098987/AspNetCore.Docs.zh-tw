---
title: "ASP.NET Core 安全性概觀"
author: rachelappel
description: "了解 ASP.NET Core 的驗證、授權和安全性基本概念。"
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: e03256d7b8b442569b0b0126983732c10817e20f
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="aspnet-core-security-overview"></a><span data-ttu-id="3d853-103">ASP.NET Core 安全性概觀</span><span class="sxs-lookup"><span data-stu-id="3d853-103">ASP.NET Core Security Overview</span></span>

<span data-ttu-id="3d853-104">ASP.NET Core 可讓開發人員輕鬆設定應用程式的安全性並進行管理。</span><span class="sxs-lookup"><span data-stu-id="3d853-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="3d853-105">ASP.NET Core 包含針對認證、授權、資料保護、強制使用 SSL、應用程式密碼、防偽要求保護及 CORS 的管理功能。</span><span class="sxs-lookup"><span data-stu-id="3d853-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="3d853-106">這些安全性功能可讓您打造強固又安全的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3d853-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span>

## <a name="aspnet-core-security-features"></a><span data-ttu-id="3d853-107">ASP.NET Core 安全性功能</span><span class="sxs-lookup"><span data-stu-id="3d853-107">ASP.NET Core security features</span></span>

<span data-ttu-id="3d853-108">ASP.NET Core 提供許多工具和程式庫來保護應用程式的安全，包括內建的識別提供者，但您也可以使用協力廠商的識別服務 (如 Facebook、Twitter 或 LinkedIn)。</span><span class="sxs-lookup"><span data-stu-id="3d853-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="3d853-109">使用 ASP.NET Core 時，您可以輕鬆管理應用程式密碼，以便使用機密資訊而不在程式碼中公開這些資訊。</span><span class="sxs-lookup"><span data-stu-id="3d853-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span>

## <a name="authentication-vs-authorization"></a><span data-ttu-id="3d853-110">驗證與授權</span><span class="sxs-lookup"><span data-stu-id="3d853-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="3d853-111">驗證是一道程序，其會將使用者提供的認證與作業系統、資料庫、應用程式或資源中儲存的認證進行比對。</span><span class="sxs-lookup"><span data-stu-id="3d853-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="3d853-112">如果相符的話，使用者就能成功通過驗證，並可執行他們在授權程序期間取得授權的動作。</span><span class="sxs-lookup"><span data-stu-id="3d853-112">If they match, users authenticate successfully, and can then perform actions that they're authorized for, during an authorization process.</span></span> <span data-ttu-id="3d853-113">授權則是決定使用者得以執行哪些動作的程序。</span><span class="sxs-lookup"><span data-stu-id="3d853-113">The authorization refers to the process that determines what a user is allowed to do.</span></span>

<span data-ttu-id="3d853-114">您也可以將驗證想成是進入伺服器、資料庫、應用程式或資源這類空間的方法，而授權則表示使用者得以針對空間 (伺服器、資料庫或應用程式) 內哪些物件執行哪些動作。</span><span class="sxs-lookup"><span data-stu-id="3d853-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="3d853-115">軟體的常見弱點</span><span class="sxs-lookup"><span data-stu-id="3d853-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="3d853-116">ASP.NET Core 和 EF 包含的功能有助您保護應用程式的安全，並防範安全性缺口的發生。</span><span class="sxs-lookup"><span data-stu-id="3d853-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="3d853-117">下列連結清單可帶您前往如何避免 Web 應用程式中最常見資訊安全漏洞的技術詳細文件：</span><span class="sxs-lookup"><span data-stu-id="3d853-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* <span data-ttu-id="3d853-118">[Cross site scripting attacks](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting) (跨網站指令碼攻擊)</span><span class="sxs-lookup"><span data-stu-id="3d853-118">[Cross-site scripting attacks](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)</span></span>
* <span data-ttu-id="3d853-119">[SQL Injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql) (SQL 插入式攻擊)</span><span class="sxs-lookup"><span data-stu-id="3d853-119">[SQL injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql)</span></span>
* <span data-ttu-id="3d853-120">[Cross-Site Request Forgery (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery) (跨網站偽造要求 (CSRF))</span><span class="sxs-lookup"><span data-stu-id="3d853-120">[Cross-Site Request Forgery (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)</span></span>
* <span data-ttu-id="3d853-121">[Open redirect attacks](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects) (開啟重新導向攻擊)</span><span class="sxs-lookup"><span data-stu-id="3d853-121">[Open redirect attacks](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)</span></span>

<span data-ttu-id="3d853-122">除此之外，還有許多弱點是您必須提防的。</span><span class="sxs-lookup"><span data-stu-id="3d853-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="3d853-123">如需詳細資訊，請參閱本文件的相關章節 (位於 *ASP.NET 安全性文件*)。</span><span class="sxs-lookup"><span data-stu-id="3d853-123">For more information, see the section in this document on *ASP.NET Security Documentation*.</span></span>

## <a name="aspnet-security-documentation"></a><span data-ttu-id="3d853-124">ASP.NET 安全性文件</span><span class="sxs-lookup"><span data-stu-id="3d853-124">ASP.NET Security Documentation</span></span>

*   [<span data-ttu-id="3d853-125">驗證</span><span class="sxs-lookup"><span data-stu-id="3d853-125">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="3d853-126">身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="3d853-126">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="3d853-127">使用 Facebook、Google 和其他外部提供者啟用驗證</span><span class="sxs-lookup"><span data-stu-id="3d853-127">Enable authentication using Facebook, Google, and other external providers</span></span>](authentication/social/index.md)
    *   [<span data-ttu-id="3d853-128">以 WS 同盟啟用驗證</span><span class="sxs-lookup"><span data-stu-id="3d853-128">Enable authentication with WS-Federation</span></span>](authentication/ws-federation.md)
    * [<span data-ttu-id="3d853-129">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="3d853-129">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="3d853-130">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="3d853-130">Account confirmation and password recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="3d853-131">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="3d853-131">Two-factor authentication with SMS</span></span>](authentication/2fa.md)
    *   [<span data-ttu-id="3d853-132">使用沒有身分識別的 Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="3d853-132">Use cookie authentication without Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="3d853-133">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3d853-133">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   <span data-ttu-id="3d853-134">[Integrate Azure AD Into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/) (將 Azure AD 整合到 ASP.NET Core Web 應用程式)</span><span class="sxs-lookup"><span data-stu-id="3d853-134">[Integrate Azure AD into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)</span></span>
        *   <span data-ttu-id="3d853-135">[Call a ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/) (使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API)</span><span class="sxs-lookup"><span data-stu-id="3d853-135">[Call an ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)</span></span>
        *   <span data-ttu-id="3d853-136">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/) (使用 Azure AD 在 ASP.NET Core Web 應用程式中呼叫 Web API)</span><span class="sxs-lookup"><span data-stu-id="3d853-136">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)</span></span>
        *   [<span data-ttu-id="3d853-137">使用 Azure AD B2C 的 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="3d853-137">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="3d853-138">使用 IdentityServer4 保護 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="3d853-138">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="3d853-139">授權</span><span class="sxs-lookup"><span data-stu-id="3d853-139">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="3d853-140">簡介</span><span class="sxs-lookup"><span data-stu-id="3d853-140">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="3d853-141">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="3d853-141">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="3d853-142">簡單授權</span><span class="sxs-lookup"><span data-stu-id="3d853-142">Simple authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="3d853-143">角色型授權</span><span class="sxs-lookup"><span data-stu-id="3d853-143">Role-based authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="3d853-144">宣告式授權</span><span class="sxs-lookup"><span data-stu-id="3d853-144">Claims-based authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="3d853-145">原則式授權</span><span class="sxs-lookup"><span data-stu-id="3d853-145">Policy-based authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="3d853-146">要求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="3d853-146">Dependency injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="3d853-147">資源型授權</span><span class="sxs-lookup"><span data-stu-id="3d853-147">Resource-based authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="3d853-148">檢視型授權</span><span class="sxs-lookup"><span data-stu-id="3d853-148">View-based authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="3d853-149">以配置限制身分識別</span><span class="sxs-lookup"><span data-stu-id="3d853-149">Limit identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="3d853-150">資料保護</span><span class="sxs-lookup"><span data-stu-id="3d853-150">Data protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="3d853-151">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="3d853-151">Introduction to data protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="3d853-152">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="3d853-152">Get started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="3d853-153">取用者 API</span><span class="sxs-lookup"><span data-stu-id="3d853-153">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="3d853-154">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="3d853-154">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="3d853-155">目的字串</span><span class="sxs-lookup"><span data-stu-id="3d853-155">Purpose strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="3d853-156">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="3d853-156">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="3d853-157">密碼雜湊</span><span class="sxs-lookup"><span data-stu-id="3d853-157">Password hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="3d853-158">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="3d853-158">Limit the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="3d853-159">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="3d853-159">Unprotect payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="3d853-160">組態</span><span class="sxs-lookup"><span data-stu-id="3d853-160">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="3d853-161">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="3d853-161">Configure data protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="3d853-162">預設設定</span><span class="sxs-lookup"><span data-stu-id="3d853-162">Default settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="3d853-163">整個電腦的原則</span><span class="sxs-lookup"><span data-stu-id="3d853-163">Machine-wide policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="3d853-164">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="3d853-164">Non DI-aware scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="3d853-165">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="3d853-165">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="3d853-166">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="3d853-166">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="3d853-167">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="3d853-167">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="3d853-168">其他 API</span><span class="sxs-lookup"><span data-stu-id="3d853-168">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="3d853-169">實作</span><span class="sxs-lookup"><span data-stu-id="3d853-169">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="3d853-170">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="3d853-170">Authenticated encryption details</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="3d853-171">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="3d853-171">Subkey derivation and authenticated encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="3d853-172">內容標頭</span><span class="sxs-lookup"><span data-stu-id="3d853-172">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="3d853-173">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="3d853-173">Key management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="3d853-174">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="3d853-174">Key storage providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="3d853-175">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="3d853-175">Key encryption at rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="3d853-176">金鑰的不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="3d853-176">Key immutability and changing settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="3d853-177">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="3d853-177">Key storage format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="3d853-178">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="3d853-178">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="3d853-179">相容性</span><span class="sxs-lookup"><span data-stu-id="3d853-179">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="3d853-180">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="3d853-180">Replace <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="3d853-181">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="3d853-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="3d853-182">在開發期間安全儲存應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="3d853-182">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="3d853-183">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="3d853-183">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="3d853-184">強制使用 SSL</span><span class="sxs-lookup"><span data-stu-id="3d853-184">Enforce SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="3d853-185">防偽要求</span><span class="sxs-lookup"><span data-stu-id="3d853-185">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="3d853-186">防止開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="3d853-186">Prevent open redirect attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="3d853-187">防止跨網站指令碼攻擊</span><span class="sxs-lookup"><span data-stu-id="3d853-187">Prevent Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="3d853-188">啟用跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="3d853-188">Enable Cross-Origin Requests (CORS)</span></span>](cors.md)
*   [<span data-ttu-id="3d853-189">在應用程式間共用 Cookie</span><span class="sxs-lookup"><span data-stu-id="3d853-189">Share cookies among apps</span></span>](cookie-sharing.md)
