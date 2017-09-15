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
# <a name="security"></a><span data-ttu-id="f7334-103">安全性</span><span class="sxs-lookup"><span data-stu-id="f7334-103">Security</span></span>

*   [<span data-ttu-id="f7334-104">驗證</span><span class="sxs-lookup"><span data-stu-id="f7334-104">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="f7334-105">身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="f7334-105">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="f7334-106">使用 Facebook、Google 和其他外部提供者啟用驗證</span><span class="sxs-lookup"><span data-stu-id="f7334-106">Enabling authentication using Facebook, Google and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="f7334-107">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="f7334-107">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="f7334-108">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="f7334-108">Account Confirmation and Password Recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="f7334-109">使用 SMS 的雙重關卡驗證</span><span class="sxs-lookup"><span data-stu-id="f7334-109">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="f7334-110">使用沒有 ASP.NET Core 身分識別的 Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="f7334-110">Using Cookie Authentication without ASP.NET Core Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="f7334-111">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f7334-111">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="f7334-112">將 Azure AD 整合到 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f7334-112">Integrating Azure AD Into an ASP.NET Core Web App</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="f7334-113">使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="f7334-113">Calling a ASP.NET Core Web API From a WPF Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="f7334-114">在使用 Azure AD 的 ASP.NET Core Web 應用程式中呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="f7334-114">Calling a Web API in an ASP.NET Core Web Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="f7334-115">使用 Azure AD B2C 的 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f7334-115">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="f7334-116">使用 IdentityServer4 保護 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="f7334-116">Securing ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="f7334-117">授權</span><span class="sxs-lookup"><span data-stu-id="f7334-117">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="f7334-118">簡介</span><span class="sxs-lookup"><span data-stu-id="f7334-118">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="f7334-119">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="f7334-119">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="f7334-120">簡單授權</span><span class="sxs-lookup"><span data-stu-id="f7334-120">Simple Authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="f7334-121">角色型授權</span><span class="sxs-lookup"><span data-stu-id="f7334-121">Role based Authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="f7334-122">宣告式授權</span><span class="sxs-lookup"><span data-stu-id="f7334-122">Claims-Based Authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="f7334-123">自訂以原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="f7334-123">Custom Policy-Based Authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="f7334-124">要求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="f7334-124">Dependency Injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="f7334-125">資源型授權</span><span class="sxs-lookup"><span data-stu-id="f7334-125">Resource Based Authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="f7334-126">檢視型授權</span><span class="sxs-lookup"><span data-stu-id="f7334-126">View Based Authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="f7334-127">以配置限制身分識別</span><span class="sxs-lookup"><span data-stu-id="f7334-127">Limiting identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="f7334-128">資料保護</span><span class="sxs-lookup"><span data-stu-id="f7334-128">Data Protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="f7334-129">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="f7334-129">Introduction to Data Protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="f7334-130">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="f7334-130">Getting Started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="f7334-131">取用者 API</span><span class="sxs-lookup"><span data-stu-id="f7334-131">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="f7334-132">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="f7334-132">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="f7334-133">目的字串</span><span class="sxs-lookup"><span data-stu-id="f7334-133">Purpose Strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="f7334-134">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="f7334-134">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="f7334-135">密碼雜湊</span><span class="sxs-lookup"><span data-stu-id="f7334-135">Password Hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="f7334-136">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="f7334-136">Limiting the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="f7334-137">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="f7334-137">Unprotecting payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="f7334-138">組態</span><span class="sxs-lookup"><span data-stu-id="f7334-138">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="f7334-139">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="f7334-139">Configuring Data Protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="f7334-140">預設設定</span><span class="sxs-lookup"><span data-stu-id="f7334-140">Default Settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="f7334-141">電腦全域原則</span><span class="sxs-lookup"><span data-stu-id="f7334-141">Machine Wide Policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="f7334-142">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="f7334-142">Non DI Aware Scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="f7334-143">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="f7334-143">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="f7334-144">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="f7334-144">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="f7334-145">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="f7334-145">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="f7334-146">其他 API</span><span class="sxs-lookup"><span data-stu-id="f7334-146">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="f7334-147">實作</span><span class="sxs-lookup"><span data-stu-id="f7334-147">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="f7334-148">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="f7334-148">Authenticated encryption details.</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="f7334-149">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="f7334-149">Subkey Derivation and Authenticated Encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="f7334-150">內容標頭</span><span class="sxs-lookup"><span data-stu-id="f7334-150">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="f7334-151">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="f7334-151">Key Management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="f7334-152">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="f7334-152">Key Storage Providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="f7334-153">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="f7334-153">Key Encryption At Rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="f7334-154">金鑰的不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="f7334-154">Key Immutability and Changing Settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="f7334-155">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="f7334-155">Key Storage Format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="f7334-156">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="f7334-156">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="f7334-157">相容性</span><span class="sxs-lookup"><span data-stu-id="f7334-157">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="f7334-158">共用應用程式之間的 Cookie</span><span class="sxs-lookup"><span data-stu-id="f7334-158">Sharing cookies between applications</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="f7334-159">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="f7334-159">Replacing <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="f7334-160">建立使用者資料受授權保護的應用程式</span><span class="sxs-lookup"><span data-stu-id="f7334-160">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="f7334-161">在開發期間安全儲存應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="f7334-161">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="f7334-162">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="f7334-162">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="f7334-163">強制執行 SSL</span><span class="sxs-lookup"><span data-stu-id="f7334-163">Enforcing SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="f7334-164">設定 HTTPS 進行開發</span><span class="sxs-lookup"><span data-stu-id="f7334-164">Setting up HTTPS for development</span></span>](https.md)
*   [<span data-ttu-id="f7334-165">防偽要求</span><span class="sxs-lookup"><span data-stu-id="f7334-165">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="f7334-166">防止開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="f7334-166">Preventing Open Redirect Attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="f7334-167">防止跨站台指令碼攻擊</span><span class="sxs-lookup"><span data-stu-id="f7334-167">Preventing Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="f7334-168">啟用跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="f7334-168">Enabling Cross-Origin Requests (CORS)</span></span>](cors.md)
