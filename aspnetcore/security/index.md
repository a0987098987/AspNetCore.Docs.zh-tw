---
title: ASP.NET Core 安全性概觀
author: rachelappel
description: 了解 ASP.NET Core 的驗證、授權和安全性基本概念。
ms.author: rachelap
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: a23d23cf1bf0503b59c6f5d962cecf89af37b4b3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278501"
---
# <a name="overview-of-aspnet-core-security"></a><span data-ttu-id="9b132-103">ASP.NET Core 安全性概觀</span><span class="sxs-lookup"><span data-stu-id="9b132-103">Overview of ASP.NET Core Security</span></span>

<span data-ttu-id="9b132-104">ASP.NET Core 可讓開發人員輕鬆設定應用程式的安全性並進行管理。</span><span class="sxs-lookup"><span data-stu-id="9b132-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="9b132-105">ASP.NET Core 包含針對認證、授權、資料保護、強制使用 SSL、應用程式密碼、防偽要求保護及 CORS 的管理功能。</span><span class="sxs-lookup"><span data-stu-id="9b132-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="9b132-106">這些安全性功能可讓您打造強固又安全的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9b132-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span>

## <a name="aspnet-core-security-features"></a><span data-ttu-id="9b132-107">ASP.NET Core 安全性功能</span><span class="sxs-lookup"><span data-stu-id="9b132-107">ASP.NET Core security features</span></span>

<span data-ttu-id="9b132-108">ASP.NET Core 提供許多工具和程式庫來保護應用程式的安全，包括內建的識別提供者，但您也可以使用協力廠商的識別服務 (如 Facebook、Twitter 或 LinkedIn)。</span><span class="sxs-lookup"><span data-stu-id="9b132-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="9b132-109">使用 ASP.NET Core 時，您可以輕鬆管理應用程式密碼，以便使用機密資訊而不在程式碼中公開這些資訊。</span><span class="sxs-lookup"><span data-stu-id="9b132-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span>

## <a name="authentication-vs-authorization"></a><span data-ttu-id="9b132-110">驗證與授權</span><span class="sxs-lookup"><span data-stu-id="9b132-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="9b132-111">驗證是一道程序，其會將使用者提供的認證與作業系統、資料庫、應用程式或資源中儲存的認證進行比對。</span><span class="sxs-lookup"><span data-stu-id="9b132-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="9b132-112">如果相符的話，使用者就能成功通過驗證，並可執行他們在授權程序期間取得授權的動作。</span><span class="sxs-lookup"><span data-stu-id="9b132-112">If they match, users authenticate successfully, and can then perform actions that they're authorized for, during an authorization process.</span></span> <span data-ttu-id="9b132-113">授權則是決定使用者得以執行哪些動作的程序。</span><span class="sxs-lookup"><span data-stu-id="9b132-113">The authorization refers to the process that determines what a user is allowed to do.</span></span>

<span data-ttu-id="9b132-114">您也可以將驗證想成是進入伺服器、資料庫、應用程式或資源這類空間的方法，而授權則表示使用者得以針對空間 (伺服器、資料庫或應用程式) 內哪些物件執行哪些動作。</span><span class="sxs-lookup"><span data-stu-id="9b132-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="9b132-115">軟體的常見弱點</span><span class="sxs-lookup"><span data-stu-id="9b132-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="9b132-116">ASP.NET Core 和 EF 包含的功能有助您保護應用程式的安全，並防範安全性缺口的發生。</span><span class="sxs-lookup"><span data-stu-id="9b132-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="9b132-117">下列連結清單可帶您前往如何避免 Web 應用程式中最常見資訊安全漏洞的技術詳細文件：</span><span class="sxs-lookup"><span data-stu-id="9b132-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* <span data-ttu-id="9b132-118">[Cross site scripting attacks](xref:security/cross-site-scripting) (跨網站指令碼攻擊)</span><span class="sxs-lookup"><span data-stu-id="9b132-118">[Cross-site scripting attacks](xref:security/cross-site-scripting)</span></span>
* <span data-ttu-id="9b132-119">[SQL Injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql) (SQL 插入式攻擊)</span><span class="sxs-lookup"><span data-stu-id="9b132-119">[SQL injection attacks](https://docs.microsoft.com/ef/core/querying/raw-sql)</span></span>
* <span data-ttu-id="9b132-120">[Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery) (跨網站偽造要求 (CSRF))</span><span class="sxs-lookup"><span data-stu-id="9b132-120">[Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery)</span></span>
* <span data-ttu-id="9b132-121">[Open redirect attacks](xref:security/preventing-open-redirects) (開啟重新導向攻擊)</span><span class="sxs-lookup"><span data-stu-id="9b132-121">[Open redirect attacks](xref:security/preventing-open-redirects)</span></span>

<span data-ttu-id="9b132-122">除此之外，還有許多弱點是您必須提防的。</span><span class="sxs-lookup"><span data-stu-id="9b132-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="9b132-123">如需詳細資訊，請參閱本文件的相關章節 (位於 *ASP.NET 安全性文件*)。</span><span class="sxs-lookup"><span data-stu-id="9b132-123">For more information, see the section in this document on *ASP.NET Security Documentation*.</span></span>

## <a name="aspnet-security-documentation"></a><span data-ttu-id="9b132-124">ASP.NET 安全性文件</span><span class="sxs-lookup"><span data-stu-id="9b132-124">ASP.NET Security Documentation</span></span>

*   [<span data-ttu-id="9b132-125">驗證</span><span class="sxs-lookup"><span data-stu-id="9b132-125">Authentication</span></span>](xref:security/authentication/index)
    *   [<span data-ttu-id="9b132-126">身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="9b132-126">Introduction to Identity</span></span>](xref:security/authentication/identity)
    *   [<span data-ttu-id="9b132-127">使用 Facebook、Google 和其他外部提供者啟用驗證</span><span class="sxs-lookup"><span data-stu-id="9b132-127">Enable authentication using Facebook, Google, and other external providers</span></span>](xref:security/authentication/social/index)
    *   [<span data-ttu-id="9b132-128">以 WS 同盟啟用驗證</span><span class="sxs-lookup"><span data-stu-id="9b132-128">Enable authentication with WS-Federation</span></span>](xref:security/authentication/ws-federation)
    * [<span data-ttu-id="9b132-129">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="9b132-129">Configure Windows Authentication</span></span>](xref:security/authentication/windowsauth)
    *   [<span data-ttu-id="9b132-130">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="9b132-130">Account confirmation and password recovery</span></span>](xref:security/authentication/accconfirm)
    *   [<span data-ttu-id="9b132-131">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="9b132-131">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
    *   [<span data-ttu-id="9b132-132">使用沒有身分識別的 Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="9b132-132">Use cookie authentication without Identity</span></span>](xref:security/authentication/cookie)
    *   [<span data-ttu-id="9b132-133">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9b132-133">Azure Active Directory</span></span>](xref:security/authentication/azure-active-directory/index)
        *   <span data-ttu-id="9b132-134">[Integrate Azure AD Into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/) (將 Azure AD 整合到 ASP.NET Core Web 應用程式)</span><span class="sxs-lookup"><span data-stu-id="9b132-134">[Integrate Azure AD into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)</span></span>
        *   <span data-ttu-id="9b132-135">[Call a ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/) (使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API)</span><span class="sxs-lookup"><span data-stu-id="9b132-135">[Call an ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)</span></span>
        *   <span data-ttu-id="9b132-136">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/) (使用 Azure AD 在 ASP.NET Core Web 應用程式中呼叫 Web API)</span><span class="sxs-lookup"><span data-stu-id="9b132-136">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)</span></span>
        *   [<span data-ttu-id="9b132-137">使用 Azure AD B2C 的 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9b132-137">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="9b132-138">使用 IdentityServer4 保護 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="9b132-138">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="9b132-139">授權</span><span class="sxs-lookup"><span data-stu-id="9b132-139">Authorization</span></span>](xref:security/authorization/index)
    *   [<span data-ttu-id="9b132-140">簡介</span><span class="sxs-lookup"><span data-stu-id="9b132-140">Introduction</span></span>](xref:security/authorization/introduction)
    *   [<span data-ttu-id="9b132-141">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="9b132-141">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="9b132-142">簡單授權</span><span class="sxs-lookup"><span data-stu-id="9b132-142">Simple authorization</span></span>](xref:security/authorization/simple)
    *   [<span data-ttu-id="9b132-143">角色型授權</span><span class="sxs-lookup"><span data-stu-id="9b132-143">Role-based authorization</span></span>](xref:security/authorization/roles)
    *   [<span data-ttu-id="9b132-144">宣告式授權</span><span class="sxs-lookup"><span data-stu-id="9b132-144">Claims-based authorization</span></span>](xref:security/authorization/claims)
    *   [<span data-ttu-id="9b132-145">原則式授權</span><span class="sxs-lookup"><span data-stu-id="9b132-145">Policy-based authorization</span></span>](xref:security/authorization/policies)
    *   [<span data-ttu-id="9b132-146">要求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="9b132-146">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
    *   [<span data-ttu-id="9b132-147">資源型授權</span><span class="sxs-lookup"><span data-stu-id="9b132-147">Resource-based authorization</span></span>](xref:security/authorization/resourcebased)
    *   [<span data-ttu-id="9b132-148">檢視型授權</span><span class="sxs-lookup"><span data-stu-id="9b132-148">View-based authorization</span></span>](xref:security/authorization/views)
    *   [<span data-ttu-id="9b132-149">以配置限制身分識別</span><span class="sxs-lookup"><span data-stu-id="9b132-149">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
*   [<span data-ttu-id="9b132-150">資料保護</span><span class="sxs-lookup"><span data-stu-id="9b132-150">Data protection</span></span>](xref:security/data-protection/index)
    *   [<span data-ttu-id="9b132-151">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="9b132-151">Introduction to data protection</span></span>](xref:security/data-protection/introduction)
    *   [<span data-ttu-id="9b132-152">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="9b132-152">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)
    *   [<span data-ttu-id="9b132-153">取用者 API</span><span class="sxs-lookup"><span data-stu-id="9b132-153">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)
        *   [<span data-ttu-id="9b132-154">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="9b132-154">Consumer APIs Overview</span></span>](xref:security/data-protection/consumer-apis/overview)
        *   [<span data-ttu-id="9b132-155">目的字串</span><span class="sxs-lookup"><span data-stu-id="9b132-155">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [<span data-ttu-id="9b132-156">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="9b132-156">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [<span data-ttu-id="9b132-157">雜湊密碼</span><span class="sxs-lookup"><span data-stu-id="9b132-157">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)
        *   [<span data-ttu-id="9b132-158">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="9b132-158">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [<span data-ttu-id="9b132-159">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="9b132-159">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [<span data-ttu-id="9b132-160">組態</span><span class="sxs-lookup"><span data-stu-id="9b132-160">Configuration</span></span>](xref:security/data-protection/configuration/index)
        *   [<span data-ttu-id="9b132-161">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="9b132-161">Configure data protection</span></span>](xref:security/data-protection/configuration/overview)
        *   [<span data-ttu-id="9b132-162">預設設定</span><span class="sxs-lookup"><span data-stu-id="9b132-162">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)
        *   [<span data-ttu-id="9b132-163">整個電腦的原則</span><span class="sxs-lookup"><span data-stu-id="9b132-163">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
        *   [<span data-ttu-id="9b132-164">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="9b132-164">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
    *   [<span data-ttu-id="9b132-165">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="9b132-165">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)
        *   [<span data-ttu-id="9b132-166">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="9b132-166">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)
        *   [<span data-ttu-id="9b132-167">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="9b132-167">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)
        *   [<span data-ttu-id="9b132-168">其他 API</span><span class="sxs-lookup"><span data-stu-id="9b132-168">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)
    *   [<span data-ttu-id="9b132-169">實作</span><span class="sxs-lookup"><span data-stu-id="9b132-169">Implementation</span></span>](xref:security/data-protection/implementation/index)
        *   [<span data-ttu-id="9b132-170">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="9b132-170">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [<span data-ttu-id="9b132-171">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="9b132-171">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)
        *   [<span data-ttu-id="9b132-172">內容標頭</span><span class="sxs-lookup"><span data-stu-id="9b132-172">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)
        *   [<span data-ttu-id="9b132-173">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="9b132-173">Key management</span></span>](xref:security/data-protection/implementation/key-management)
        *   [<span data-ttu-id="9b132-174">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="9b132-174">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)
        *   [<span data-ttu-id="9b132-175">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="9b132-175">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [<span data-ttu-id="9b132-176">金鑰的不變性和設定</span><span class="sxs-lookup"><span data-stu-id="9b132-176">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)
        *   [<span data-ttu-id="9b132-177">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="9b132-177">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)
        *   [<span data-ttu-id="9b132-178">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="9b132-178">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [<span data-ttu-id="9b132-179">相容性</span><span class="sxs-lookup"><span data-stu-id="9b132-179">Compatibility</span></span>](xref:security/data-protection/compatibility/index)
        *   [<span data-ttu-id="9b132-180">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="9b132-180">Replace <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
*   [<span data-ttu-id="9b132-181">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="9b132-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="9b132-182">在開發期間安全儲存應用程式祕密</span><span class="sxs-lookup"><span data-stu-id="9b132-182">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
*   [<span data-ttu-id="9b132-183">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="9b132-183">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
*   [<span data-ttu-id="9b132-184">強制使用 SSL</span><span class="sxs-lookup"><span data-stu-id="9b132-184">Enforce SSL</span></span>](xref:security/enforcing-ssl)
*   [<span data-ttu-id="9b132-185">防偽要求</span><span class="sxs-lookup"><span data-stu-id="9b132-185">Anti-Request Forgery</span></span>](xref:security/anti-request-forgery)
*   [<span data-ttu-id="9b132-186">防止開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="9b132-186">Prevent open redirect attacks</span></span>](xref:security/preventing-open-redirects)
*   [<span data-ttu-id="9b132-187">防止跨網站指令碼攻擊</span><span class="sxs-lookup"><span data-stu-id="9b132-187">Prevent Cross-Site Scripting</span></span>](xref:security/cross-site-scripting)
*   [<span data-ttu-id="9b132-188">啟用跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="9b132-188">Enable Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
*   [<span data-ttu-id="9b132-189">在應用程式間共用 Cookie</span><span class="sxs-lookup"><span data-stu-id="9b132-189">Share cookies among apps</span></span>](xref:security/cookie-sharing)
