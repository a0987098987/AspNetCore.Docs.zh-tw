---
title: 與 ASP.NET Core 搭配運作的 IIS 模組
author: rick-anderson
description: 探索 ASP.NET Core 應用程式的使用中和非使用中 IIS 模組，管理 IIS 模組的方式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 124f2a629ebd14210cb21351a720e007bba48f02
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404011"
---
# <a name="iis-modules-with-aspnet-core"></a><span data-ttu-id="35e5d-103">與 ASP.NET Core 搭配運作的 IIS 模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-103">IIS modules with ASP.NET Core</span></span>

<span data-ttu-id="35e5d-104">您無法使用部分原生 IIS 模組與所有 IIS 受控模組來處理 ASP.NET Core 應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="35e5d-104">Some of the native IIS modules and all of the IIS managed modules aren't able to process requests for ASP.NET Core apps.</span></span> <span data-ttu-id="35e5d-105">在許多情況下，ASP.NET Core 都提供由 IIS 原生與受控模組處理之情節的替代方案。</span><span class="sxs-lookup"><span data-stu-id="35e5d-105">In many cases, ASP.NET Core offers an alternative to the scenarios addressed by IIS native and managed modules.</span></span>

## <a name="native-modules"></a><span data-ttu-id="35e5d-106">原生模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-106">Native modules</span></span>

<span data-ttu-id="35e5d-107">下表指出可搭配 ASP.NET Core 應用程式和 ASP.NET Core 模組運作的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-107">The table indicates native IIS modules that are functional with ASP.NET Core apps and the ASP.NET Core Module.</span></span>

| <span data-ttu-id="35e5d-108">模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-108">Module</span></span> | <span data-ttu-id="35e5d-109">對 ASP.NET Core 應用程式有作用</span><span class="sxs-lookup"><span data-stu-id="35e5d-109">Functional with ASP.NET Core apps</span></span> | <span data-ttu-id="35e5d-110">ASP.NET Core 選項</span><span class="sxs-lookup"><span data-stu-id="35e5d-110">ASP.NET Core Option</span></span> |
| --- | :---: | --- |
| <span data-ttu-id="35e5d-111">**匿名驗證**</span><span class="sxs-lookup"><span data-stu-id="35e5d-111">**Anonymous Authentication**</span></span><br>`AnonymousAuthenticationModule`                                  | <span data-ttu-id="35e5d-112">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-112">Yes</span></span> | |
| <span data-ttu-id="35e5d-113">**基本驗證**</span><span class="sxs-lookup"><span data-stu-id="35e5d-113">**Basic Authentication**</span></span><br>`BasicAuthenticationModule`                                          | <span data-ttu-id="35e5d-114">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-114">Yes</span></span> | |
| <span data-ttu-id="35e5d-115">**用戶端憑證對應驗證**</span><span class="sxs-lookup"><span data-stu-id="35e5d-115">**Client Certification Mapping Authentication**</span></span><br>`CertificateMappingAuthenticationModule`      | <span data-ttu-id="35e5d-116">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-116">Yes</span></span> | |
| <span data-ttu-id="35e5d-117">**CGI**</span><span class="sxs-lookup"><span data-stu-id="35e5d-117">**CGI**</span></span><br>`CgiModule`                                                                           | <span data-ttu-id="35e5d-118">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-118">No</span></span>  | |
| <span data-ttu-id="35e5d-119">**設定驗證**</span><span class="sxs-lookup"><span data-stu-id="35e5d-119">**Configuration Validation**</span></span><br>`ConfigurationValidationModule`                                  | <span data-ttu-id="35e5d-120">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-120">Yes</span></span> | |
| <span data-ttu-id="35e5d-121">**HTTP 錯誤**</span><span class="sxs-lookup"><span data-stu-id="35e5d-121">**HTTP Errors**</span></span><br>`CustomErrorModule`                                                           | <span data-ttu-id="35e5d-122">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-122">No</span></span>  | [<span data-ttu-id="35e5d-123">狀態碼頁面中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-123">Status Code Pages Middleware</span></span>](xref:fundamentals/error-handling#usestatuscodepages) |
| <span data-ttu-id="35e5d-124">**自訂記錄**</span><span class="sxs-lookup"><span data-stu-id="35e5d-124">**Custom Logging**</span></span><br>`CustomLoggingModule`                                                      | <span data-ttu-id="35e5d-125">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-125">Yes</span></span> | |
| <span data-ttu-id="35e5d-126">**預設文件**</span><span class="sxs-lookup"><span data-stu-id="35e5d-126">**Default Document**</span></span><br>`DefaultDocumentModule`                                                  | <span data-ttu-id="35e5d-127">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-127">No</span></span>  | [<span data-ttu-id="35e5d-128">預設檔案中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-128">Default Files Middleware</span></span>](xref:fundamentals/static-files#serve-a-default-document) |
| <span data-ttu-id="35e5d-129">**摘要式驗證**</span><span class="sxs-lookup"><span data-stu-id="35e5d-129">**Digest Authentication**</span></span><br>`DigestAuthenticationModule`                                        | <span data-ttu-id="35e5d-130">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-130">Yes</span></span> | |
| <span data-ttu-id="35e5d-131">**目錄瀏覽**</span><span class="sxs-lookup"><span data-stu-id="35e5d-131">**Directory Browsing**</span></span><br>`DirectoryListingModule`                                               | <span data-ttu-id="35e5d-132">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-132">No</span></span>  | [<span data-ttu-id="35e5d-133">目錄瀏覽中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-133">Directory Browsing Middleware</span></span>](xref:fundamentals/static-files#enable-directory-browsing) |
| <span data-ttu-id="35e5d-134">**動態壓縮**</span><span class="sxs-lookup"><span data-stu-id="35e5d-134">**Dynamic Compression**</span></span><br>`DynamicCompressionModule`                                            | <span data-ttu-id="35e5d-135">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-135">Yes</span></span> | [<span data-ttu-id="35e5d-136">回應壓縮中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-136">Response Compression Middleware</span></span>](xref:performance/response-compression) |
| <span data-ttu-id="35e5d-137">**失敗的要求追蹤**</span><span class="sxs-lookup"><span data-stu-id="35e5d-137">**Failed Requests Tracing**</span></span><br>`FailedRequestsTracingModule`                                     | <span data-ttu-id="35e5d-138">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-138">Yes</span></span> | [<span data-ttu-id="35e5d-139">ASP.NET Core 記錄</span><span class="sxs-lookup"><span data-stu-id="35e5d-139">ASP.NET Core Logging</span></span>](xref:fundamentals/logging/index#tracesource-provider) |
| <span data-ttu-id="35e5d-140">**檔案快取**</span><span class="sxs-lookup"><span data-stu-id="35e5d-140">**File Caching**</span></span><br>`FileCacheModule`                                                            | <span data-ttu-id="35e5d-141">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-141">No</span></span>  | [<span data-ttu-id="35e5d-142">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-142">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="35e5d-143">**HTTP 快取**</span><span class="sxs-lookup"><span data-stu-id="35e5d-143">**HTTP Caching**</span></span><br>`HttpCacheModule`                                                            | <span data-ttu-id="35e5d-144">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-144">No</span></span>  | [<span data-ttu-id="35e5d-145">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-145">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="35e5d-146">**HTTP 記錄**</span><span class="sxs-lookup"><span data-stu-id="35e5d-146">**HTTP Logging**</span></span><br>`HttpLoggingModule`                                                          | <span data-ttu-id="35e5d-147">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-147">Yes</span></span> | [<span data-ttu-id="35e5d-148">ASP.NET Core 記錄</span><span class="sxs-lookup"><span data-stu-id="35e5d-148">ASP.NET Core Logging</span></span>](xref:fundamentals/logging/index) |
| <span data-ttu-id="35e5d-149">**HTTP 重新導向**</span><span class="sxs-lookup"><span data-stu-id="35e5d-149">**HTTP Redirection**</span></span><br>`HttpRedirectionModule`                                                  | <span data-ttu-id="35e5d-150">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-150">Yes</span></span> | [<span data-ttu-id="35e5d-151">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-151">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="35e5d-152">**HTTP 追蹤**</span><span class="sxs-lookup"><span data-stu-id="35e5d-152">**HTTP Tracing**</span></span><br>`TracingModule`                                                              | <span data-ttu-id="35e5d-153">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-153">Yes</span></span> | |
| <span data-ttu-id="35e5d-154">**IIS 用戶端憑證對應驗證**</span><span class="sxs-lookup"><span data-stu-id="35e5d-154">**IIS Client Certificate Mapping Authentication**</span></span><br>`IISCertificateMappingAuthenticationModule` | <span data-ttu-id="35e5d-155">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-155">Yes</span></span> | |
| <span data-ttu-id="35e5d-156">**IP 及網域限制**</span><span class="sxs-lookup"><span data-stu-id="35e5d-156">**IP and Domain Restrictions**</span></span><br>`IpRestrictionModule`                                          | <span data-ttu-id="35e5d-157">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-157">Yes</span></span> | |
| <span data-ttu-id="35e5d-158">**ISAPI 篩選器**</span><span class="sxs-lookup"><span data-stu-id="35e5d-158">**ISAPI Filters**</span></span><br>`IsapiFilterModule`                                                         | <span data-ttu-id="35e5d-159">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-159">Yes</span></span> | [<span data-ttu-id="35e5d-160">中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-160">Middleware</span></span>](xref:fundamentals/middleware/index) |
| <span data-ttu-id="35e5d-161">**ISAPI**</span><span class="sxs-lookup"><span data-stu-id="35e5d-161">**ISAPI**</span></span><br>`IsapiModule`                                                                       | <span data-ttu-id="35e5d-162">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-162">Yes</span></span> | [<span data-ttu-id="35e5d-163">中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-163">Middleware</span></span>](xref:fundamentals/middleware/index) |
| <span data-ttu-id="35e5d-164">**通訊協定支援**</span><span class="sxs-lookup"><span data-stu-id="35e5d-164">**Protocol Support**</span></span><br>`ProtocolSupportModule`                                                  | <span data-ttu-id="35e5d-165">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-165">Yes</span></span> | |
| <span data-ttu-id="35e5d-166">**要求篩選**</span><span class="sxs-lookup"><span data-stu-id="35e5d-166">**Request Filtering**</span></span><br>`RequestFilteringModule`                                                | <span data-ttu-id="35e5d-167">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-167">Yes</span></span> | [<span data-ttu-id="35e5d-168">URL 重寫中介軟體`IRule`</span><span class="sxs-lookup"><span data-stu-id="35e5d-168">URL Rewriting Middleware `IRule`</span></span>](xref:fundamentals/url-rewriting#irule-based-rule) |
| <span data-ttu-id="35e5d-169">**要求監視器**</span><span class="sxs-lookup"><span data-stu-id="35e5d-169">**Request Monitor**</span></span><br>`RequestMonitorModule`                                                    | <span data-ttu-id="35e5d-170">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-170">Yes</span></span> | |
| <span data-ttu-id="35e5d-171">**URL 重新寫入**&#8224;</span><span class="sxs-lookup"><span data-stu-id="35e5d-171">**URL Rewriting**&#8224;</span></span><br>`RewriteModule`                                                      | <span data-ttu-id="35e5d-172">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-172">Yes</span></span> | [<span data-ttu-id="35e5d-173">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-173">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="35e5d-174">**伺服器端包含**</span><span class="sxs-lookup"><span data-stu-id="35e5d-174">**Server-Side Includes**</span></span><br>`ServerSideIncludeModule`                                            | <span data-ttu-id="35e5d-175">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-175">No</span></span>  | |
| <span data-ttu-id="35e5d-176">**靜態壓縮**</span><span class="sxs-lookup"><span data-stu-id="35e5d-176">**Static Compression**</span></span><br>`StaticCompressionModule`                                              | <span data-ttu-id="35e5d-177">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-177">No</span></span>  | [<span data-ttu-id="35e5d-178">回應壓縮中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-178">Response Compression Middleware</span></span>](xref:performance/response-compression) |
| <span data-ttu-id="35e5d-179">**靜態內容**</span><span class="sxs-lookup"><span data-stu-id="35e5d-179">**Static Content**</span></span><br>`StaticFileModule`                                                         | <span data-ttu-id="35e5d-180">否</span><span class="sxs-lookup"><span data-stu-id="35e5d-180">No</span></span>  | [<span data-ttu-id="35e5d-181">靜態檔案中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-181">Static File Middleware</span></span>](xref:fundamentals/static-files) |
| <span data-ttu-id="35e5d-182">**權杖快取**</span><span class="sxs-lookup"><span data-stu-id="35e5d-182">**Token Caching**</span></span><br>`TokenCacheModule`                                                          | <span data-ttu-id="35e5d-183">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-183">Yes</span></span> | |
| <span data-ttu-id="35e5d-184">**URI 快取**</span><span class="sxs-lookup"><span data-stu-id="35e5d-184">**URI Caching**</span></span><br>`UriCacheModule`                                                              | <span data-ttu-id="35e5d-185">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-185">Yes</span></span> | |
| <span data-ttu-id="35e5d-186">**URL 授權**</span><span class="sxs-lookup"><span data-stu-id="35e5d-186">**URL Authorization**</span></span><br>`UrlAuthorizationModule`                                                | <span data-ttu-id="35e5d-187">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-187">Yes</span></span> | <span data-ttu-id="35e5d-188">[ASP.NET CoreIdentity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="35e5d-188">[ASP.NET Core Identity](xref:security/authentication/identity)</span></span> |
| <span data-ttu-id="35e5d-189">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="35e5d-189">**Windows Authentication**</span></span><br>`WindowsAuthenticationModule`                                      | <span data-ttu-id="35e5d-190">是</span><span class="sxs-lookup"><span data-stu-id="35e5d-190">Yes</span></span> | |

<span data-ttu-id="35e5d-191">&#8224;由於[目錄結構](xref:host-and-deploy/directory-structure)變更的緣故，因此「URL 重寫模組」的 `isFile` 和 `isDirectory` 比對類型對 ASP.NET Core 應用程式沒有作用。</span><span class="sxs-lookup"><span data-stu-id="35e5d-191">&#8224;The URL Rewrite Module's `isFile` and `isDirectory` match types don't work with ASP.NET Core apps due to the changes in [directory structure](xref:host-and-deploy/directory-structure).</span></span>

## <a name="managed-modules"></a><span data-ttu-id="35e5d-192">受控模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-192">Managed modules</span></span>

<span data-ttu-id="35e5d-193">當應用程式集區的 .NET CLR 版本已設定為 [沒有 Managed 程式碼]\*\*\*\* 時，受控模組對所裝載的 ASP.NET Core 應用程式「沒有」\*\* 作用。</span><span class="sxs-lookup"><span data-stu-id="35e5d-193">Managed modules are *not* functional with hosted ASP.NET Core apps when the app pool's .NET CLR version is set to **No Managed Code**.</span></span> <span data-ttu-id="35e5d-194">ASP.NET Core 在數種案例中都有提供中介軟體替代方案。</span><span class="sxs-lookup"><span data-stu-id="35e5d-194">ASP.NET Core offers middleware alternatives in several cases.</span></span>

| <span data-ttu-id="35e5d-195">模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-195">Module</span></span>                  | <span data-ttu-id="35e5d-196">ASP.NET Core 選項</span><span class="sxs-lookup"><span data-stu-id="35e5d-196">ASP.NET Core Option</span></span> |
| ----------------------- | ------------------- |
| <span data-ttu-id="35e5d-197">AnonymousIdentification</span><span class="sxs-lookup"><span data-stu-id="35e5d-197">AnonymousIdentification</span></span> | |
| <span data-ttu-id="35e5d-198">DefaultAuthentication</span><span class="sxs-lookup"><span data-stu-id="35e5d-198">DefaultAuthentication</span></span>   | |
| <span data-ttu-id="35e5d-199">FileAuthorization</span><span class="sxs-lookup"><span data-stu-id="35e5d-199">FileAuthorization</span></span>       | |
| <span data-ttu-id="35e5d-200">FormsAuthentication</span><span class="sxs-lookup"><span data-stu-id="35e5d-200">FormsAuthentication</span></span>     | [<span data-ttu-id="35e5d-201">Cookie 驗證中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-201">Cookie Authentication Middleware</span></span>](xref:security/authentication/cookie) |
| <span data-ttu-id="35e5d-202">OutputCache</span><span class="sxs-lookup"><span data-stu-id="35e5d-202">OutputCache</span></span>             | [<span data-ttu-id="35e5d-203">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-203">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="35e5d-204">設定檔</span><span class="sxs-lookup"><span data-stu-id="35e5d-204">Profile</span></span>                 | |
| <span data-ttu-id="35e5d-205">RoleManager</span><span class="sxs-lookup"><span data-stu-id="35e5d-205">RoleManager</span></span>             | |
| <span data-ttu-id="35e5d-206">ScriptModule-4.0</span><span class="sxs-lookup"><span data-stu-id="35e5d-206">ScriptModule-4.0</span></span>        | |
| <span data-ttu-id="35e5d-207">工作階段</span><span class="sxs-lookup"><span data-stu-id="35e5d-207">Session</span></span>                 | [<span data-ttu-id="35e5d-208">工作階段中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-208">Session Middleware</span></span>](xref:fundamentals/app-state) |
| <span data-ttu-id="35e5d-209">UrlAuthorization</span><span class="sxs-lookup"><span data-stu-id="35e5d-209">UrlAuthorization</span></span>        | |
| <span data-ttu-id="35e5d-210">UrlMappingsModule</span><span class="sxs-lookup"><span data-stu-id="35e5d-210">UrlMappingsModule</span></span>       | [<span data-ttu-id="35e5d-211">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="35e5d-211">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="35e5d-212">UrlRoutingModule-4.0</span><span class="sxs-lookup"><span data-stu-id="35e5d-212">UrlRoutingModule-4.0</span></span>    | <span data-ttu-id="35e5d-213">[ASP.NET CoreIdentity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="35e5d-213">[ASP.NET Core Identity](xref:security/authentication/identity)</span></span> |
| <span data-ttu-id="35e5d-214">WindowsAuthentication</span><span class="sxs-lookup"><span data-stu-id="35e5d-214">WindowsAuthentication</span></span>   | |

## <a name="iis-manager-application-changes"></a><span data-ttu-id="35e5d-215">IIS 管理員應用程式變更</span><span class="sxs-lookup"><span data-stu-id="35e5d-215">IIS Manager application changes</span></span>

<span data-ttu-id="35e5d-216">使用「IIS 管理員」來進行設定時，會變更應用程式的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="35e5d-216">When using IIS Manager to configure settings, the *web.config* file of the app is changed.</span></span> <span data-ttu-id="35e5d-217">如果部署應用程式並包含 *web.config*，則所部署的 *web.config* 檔案會覆寫使用「IIS 管理員」來進行的所有變更。</span><span class="sxs-lookup"><span data-stu-id="35e5d-217">If deploying an app and including *web.config*, any changes made with IIS Manager are overwritten by the deployed *web.config* file.</span></span> <span data-ttu-id="35e5d-218">對伺服器的 *web.config* 檔案進行變更後，請立即將伺服器上已更新的 *web.config* 檔案複製到本機專案。</span><span class="sxs-lookup"><span data-stu-id="35e5d-218">If changes are made to the server's *web.config* file, copy the updated *web.config* file on the server to the local project immediately.</span></span>

## <a name="disabling-iis-modules"></a><span data-ttu-id="35e5d-219">停用 IIS 模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-219">Disabling IIS modules</span></span>

<span data-ttu-id="35e5d-220">如果在必須針對應用程式停用的伺服器層級設定了 IIS 模組，只要在應用程式的 *web.config* 檔案中新增設定，即可停用該模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-220">If an IIS module is configured at the server level that must be disabled for an app, an addition to the app's *web.config* file can disable the module.</span></span> <span data-ttu-id="35e5d-221">請將模組留在原處，然後使用組態設定 (如果有的話) 來停用它，或是從應用程式移除模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-221">Either leave the module in place and deactivate it using a configuration setting (if available) or remove the module from the app.</span></span>

### <a name="module-deactivation"></a><span data-ttu-id="35e5d-222">模組停用</span><span class="sxs-lookup"><span data-stu-id="35e5d-222">Module deactivation</span></span>

<span data-ttu-id="35e5d-223">許多模組都有提供可將模組停用而無須從應用程式中移除的組態設定。</span><span class="sxs-lookup"><span data-stu-id="35e5d-223">Many modules offer a configuration setting that allows them to be disabled without removing the module from the app.</span></span> <span data-ttu-id="35e5d-224">這是停用模組的最簡便快速方式。</span><span class="sxs-lookup"><span data-stu-id="35e5d-224">This is the simplest and quickest way to deactivate a module.</span></span> <span data-ttu-id="35e5d-225">例如，使用 *web.config* 中的 `<httpRedirect>` 元素，即可停用「HTTP 重新導向模組」：</span><span class="sxs-lookup"><span data-stu-id="35e5d-225">For example, the HTTP Redirection Module can be disabled with the `<httpRedirect>` element in *web.config*:</span></span>

```xml
<configuration>
  <system.webServer>
    <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="35e5d-226">如需使用配置設定停用模組的詳細資訊，請遵循[ \<system.webServer> IIS](/iis/configuration/system.webServer/)的 [*子項目*] 區段中的連結。</span><span class="sxs-lookup"><span data-stu-id="35e5d-226">For more information on disabling modules with configuration settings, follow the links in the *Child Elements* section of [IIS \<system.webServer>](/iis/configuration/system.webServer/).</span></span>

### <a name="module-removal"></a><span data-ttu-id="35e5d-227">模組移除</span><span class="sxs-lookup"><span data-stu-id="35e5d-227">Module removal</span></span>

<span data-ttu-id="35e5d-228">如果選擇透過 *web.config* 中的設定來移除模組，請先將模組解除鎖定，以及將 *web.config* 的 `<modules>` 區段解除鎖定：</span><span class="sxs-lookup"><span data-stu-id="35e5d-228">If opting to remove a module with a setting in *web.config*, unlock the module and unlock the `<modules>` section of *web.config* first:</span></span>

1. <span data-ttu-id="35e5d-229">將伺服器層級的模組解除鎖定。</span><span class="sxs-lookup"><span data-stu-id="35e5d-229">Unlock the module at the server level.</span></span> <span data-ttu-id="35e5d-230">選取「IIS 管理員」[連線]\*\*\*\* 資訊看板中的 IIS 伺服器。</span><span class="sxs-lookup"><span data-stu-id="35e5d-230">Select the IIS server in the IIS Manager **Connections** sidebar.</span></span> <span data-ttu-id="35e5d-231">開啟 [IIS]\*\*\*\* 區域中的 [模組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="35e5d-231">Open the **Modules** in the **IIS** area.</span></span> <span data-ttu-id="35e5d-232">選取清單中的模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-232">Select the module in the list.</span></span> <span data-ttu-id="35e5d-233">在右邊的 [動作]\*\*\*\* 資訊看板上，選取 [解除鎖定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="35e5d-233">In the **Actions** sidebar on the right, select **Unlock**.</span></span> <span data-ttu-id="35e5d-234">若模組的動作項目顯示為**鎖定**，就代表該模組已經解除鎖定，且不需要任何動作。</span><span class="sxs-lookup"><span data-stu-id="35e5d-234">If the action entry for the module appears as **Lock**, the module is already unlocked, and no action is required.</span></span> <span data-ttu-id="35e5d-235">將您打算稍後從 *web.config* 移除的模組都解除鎖定。</span><span class="sxs-lookup"><span data-stu-id="35e5d-235">Unlock as many modules as you plan to remove from *web.config* later.</span></span>

2. <span data-ttu-id="35e5d-236">在web.config中部署應用程式，而不使用 `<modules>` 區段。 *web.config*如果應用程式是以包含區段的*web.config*來部署 `<modules>` ，但未先在 IIS 管理員中解除鎖定區段，則在嘗試解除鎖定區段時，Configuration Manager 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="35e5d-236">Deploy the app without a `<modules>` section in *web.config*. If an app is deployed with a *web.config* containing the `<modules>` section without having unlocked the section first in the IIS Manager, the Configuration Manager throws an exception when attempting to unlock the section.</span></span> <span data-ttu-id="35e5d-237">因此，請在沒有 `<modules>` 區段的情況下部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="35e5d-237">Therefore, deploy the app without a `<modules>` section.</span></span>

3. <span data-ttu-id="35e5d-238">解除鎖定 `<modules>` *web.config*的區段。在 [**連接**] 提要欄位中，選取 [**網站**] 中的網站。</span><span class="sxs-lookup"><span data-stu-id="35e5d-238">Unlock the `<modules>` section of *web.config*. In the **Connections** sidebar, select the website in **Sites**.</span></span> <span data-ttu-id="35e5d-239">在 [管理]\*\*\*\* 區域中，開啟 [設定編輯器]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="35e5d-239">In the **Management** area, open the **Configuration Editor**.</span></span> <span data-ttu-id="35e5d-240">使用導覽控制項來選取 `system.webServer/modules` 區段。</span><span class="sxs-lookup"><span data-stu-id="35e5d-240">Use the navigation controls to select the `system.webServer/modules` section.</span></span> <span data-ttu-id="35e5d-241">在右邊的 [動作]\*\*\*\* 資訊看板上，選取將區段 [解除鎖定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="35e5d-241">In the **Actions** sidebar on the right, select to **Unlock** the section.</span></span> <span data-ttu-id="35e5d-242">若模組區段的動作項目顯示為**鎖定區段**，就代表該模組區段已經解除鎖定，且不需要任何動作。</span><span class="sxs-lookup"><span data-stu-id="35e5d-242">If the action entry for the module section appears as **Lock Section**, the module section is already unlocked, and no action is required.</span></span>

4. <span data-ttu-id="35e5d-243">將 `<modules>` 區段新增至具有 `<remove>` 元素的應用程式本機 *web.config* 檔案，以從應用程式移除該模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-243">Add a `<modules>` section to the app's local *web.config* file with a `<remove>` element to remove the module from the app.</span></span> <span data-ttu-id="35e5d-244">新增多個 `<remove>` 元素以移除多個模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-244">Add multiple `<remove>` elements to remove multiple modules.</span></span> <span data-ttu-id="35e5d-245">如果已在伺服器上進行 *web.config* 變更，請立即在本機對專案的 *web.config* 檔案進行相同的變更。</span><span class="sxs-lookup"><span data-stu-id="35e5d-245">If *web.config* changes are made on the server, immediately make the same changes to the project's *web.config* file locally.</span></span> <span data-ttu-id="35e5d-246">使用此方法移除模組不會影響模組與伺服器上其他應用程式的搭配使用。</span><span class="sxs-lookup"><span data-stu-id="35e5d-246">Removing a module using this approach doesn't affect the use of the module with other apps on the server.</span></span>

   ```xml
   <configuration>
    <system.webServer>
      <modules>
        <remove name="MODULE_NAME" />
      </modules>
    </system.webServer>
   </configuration>
   ```

<span data-ttu-id="35e5d-247">若要使用 *web.config* 對 IIS Express 新增或移除模組，請修改 *applicationHost.config* 以解除鎖定 `<modules>` 區段：</span><span class="sxs-lookup"><span data-stu-id="35e5d-247">In order to add or remove modules for IIS Express using *web.config*, modify *applicationHost.config* to unlock the `<modules>` section:</span></span>

1. <span data-ttu-id="35e5d-248">開啟 *{APPLICATION ROOT}\\.vs\config\applicationhost.config*。</span><span class="sxs-lookup"><span data-stu-id="35e5d-248">Open *{APPLICATION ROOT}\\.vs\config\applicationhost.config*.</span></span>

1. <span data-ttu-id="35e5d-249">找出 IIS 模組的 `<section>` 元素，並將 `overrideModeDefault` 從 `Deny` 變更為 `Allow`：</span><span class="sxs-lookup"><span data-stu-id="35e5d-249">Locate the `<section>` element for IIS modules and change `overrideModeDefault` from `Deny` to `Allow`:</span></span>

   ```xml
   <section name="modules"
            allowDefinition="MachineToApplication"
            overrideModeDefault="Allow" />
   ```

1. <span data-ttu-id="35e5d-250">找出 `<location path="" overrideMode="Allow"><system.webServer><modules>` 區段。</span><span class="sxs-lookup"><span data-stu-id="35e5d-250">Locate the `<location path="" overrideMode="Allow"><system.webServer><modules>` section.</span></span> <span data-ttu-id="35e5d-251">對於您要移除的任何模組，請將 `lockItem` 從 `true` 變更為 `false`。</span><span class="sxs-lookup"><span data-stu-id="35e5d-251">For any modules that you wish to remove, set `lockItem` from `true` to `false`.</span></span> <span data-ttu-id="35e5d-252">以下為將 CGI 模組解除鎖定的範例：</span><span class="sxs-lookup"><span data-stu-id="35e5d-252">In the following example, the CGI Module is unlocked:</span></span>

   ```xml
   <add name="CgiModule" lockItem="false" />
   ```

1. <span data-ttu-id="35e5d-253">在將 `<modules>` 區段及個別模組解除鎖定後，您可任意使用應用程式的 *web.config* 檔案新增或移除 IIS 模組，以在 IIS Express 上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="35e5d-253">After the `<modules>` section and individual modules are unlocked, you're free to add or remove IIS modules using the app's *web.config* file for running the app on IIS Express.</span></span>

<span data-ttu-id="35e5d-254">您也可以使用 *Appcmd.exe* 來移除 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-254">An IIS module can also be removed with *Appcmd.exe*.</span></span> <span data-ttu-id="35e5d-255">請在命令中提供 `MODULE_NAME` 和 `APPLICATION_NAME`：</span><span class="sxs-lookup"><span data-stu-id="35e5d-255">Provide the `MODULE_NAME` and `APPLICATION_NAME` in the command:</span></span>

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

<span data-ttu-id="35e5d-256">例如，從預設網站中移除 `DynamicCompressionModule`：</span><span class="sxs-lookup"><span data-stu-id="35e5d-256">For example, remove the `DynamicCompressionModule` from the Default Web Site:</span></span>

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a><span data-ttu-id="35e5d-257">最基本的模組設定</span><span class="sxs-lookup"><span data-stu-id="35e5d-257">Minimum module configuration</span></span>

<span data-ttu-id="35e5d-258">執行 ASP.NET Core 應用程式只需「匿名驗證模組」和「ASP.NET Core 模組」這兩個模組。</span><span class="sxs-lookup"><span data-stu-id="35e5d-258">The only modules required to run an ASP.NET Core app are the Anonymous Authentication Module and the ASP.NET Core Module.</span></span>

<span data-ttu-id="35e5d-259">「URI 快取模組」(`UriCacheModule`) 可讓 IIS 快取 URL 層級的網站設定。</span><span class="sxs-lookup"><span data-stu-id="35e5d-259">The URI Caching Module (`UriCacheModule`) allows IIS to cache website configuration at the URL level.</span></span> <span data-ttu-id="35e5d-260">如果沒有此模組，IIS 就必須針對每個要求都讀取並剖析設定，即使是重複要求相同的 URL 時也一樣。</span><span class="sxs-lookup"><span data-stu-id="35e5d-260">Without this module, IIS must read and parse configuration on every request, even when the same URL is repeatedly requested.</span></span> <span data-ttu-id="35e5d-261">針對每個要求都剖析設定會導致效能大幅降低。</span><span class="sxs-lookup"><span data-stu-id="35e5d-261">Parsing the configuration every request results in a significant performance penalty.</span></span> <span data-ttu-id="35e5d-262">*雖然不一定要有「URI 快取模組」，所裝載的 ASP.NET Core 應用程式就能執行，但建議您為所有 ASP.NET Core 部署都啟用「URI 快取模組」。*</span><span class="sxs-lookup"><span data-stu-id="35e5d-262">*Although the URI Caching Module isn't strictly required for a hosted ASP.NET Core app to run, we recommend that the URI Caching Module be enabled for all ASP.NET Core deployments.*</span></span>

<span data-ttu-id="35e5d-263">「HTTP 快取模組」(`HttpCacheModule`) 會實作 IIS 輸出快取，也會實作用來將項目快取至 HTTP.sys 快取中的邏輯。</span><span class="sxs-lookup"><span data-stu-id="35e5d-263">The HTTP Caching Module (`HttpCacheModule`) implements the IIS output cache and also the logic for caching items in the HTTP.sys cache.</span></span> <span data-ttu-id="35e5d-264">如果沒有此模組，就不會再以核心模式快取內容，而且會忽略快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="35e5d-264">Without this module, content is no longer cached in kernel mode, and cache profiles are ignored.</span></span> <span data-ttu-id="35e5d-265">移除「HTTP 快取模組」通常會對效能和資源使用情況造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="35e5d-265">Removing the HTTP Caching Module usually has adverse effects on performance and resource usage.</span></span> <span data-ttu-id="35e5d-266">*雖然不一定要有「HTTP 快取模組」，所裝載的 ASP.NET Core 應用程式就能執行，但建議您為所有 ASP.NET Core 部署都啟用「HTTP 快取模組」。*</span><span class="sxs-lookup"><span data-stu-id="35e5d-266">*Although the HTTP Caching Module isn't strictly required for a hosted ASP.NET Core app to run, we recommend that the HTTP Caching Module be enabled for all ASP.NET Core deployments.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35e5d-267">其他資源</span><span class="sxs-lookup"><span data-stu-id="35e5d-267">Additional resources</span></span>

* [<span data-ttu-id="35e5d-268">IIS 架構簡介：IIS 中的模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-268">Introduction to IIS Architectures: Modules in IIS</span></span>](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [<span data-ttu-id="35e5d-269">IIS 模組概觀</span><span class="sxs-lookup"><span data-stu-id="35e5d-269">IIS Modules Overview</span></span>](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [<span data-ttu-id="35e5d-270">自訂 IIS 7.0 角色和模組</span><span class="sxs-lookup"><span data-stu-id="35e5d-270">Customizing IIS 7.0 Roles and Modules</span></span>](https://technet.microsoft.com/library/cc627313.aspx)
* [<span data-ttu-id="35e5d-271">IIS\<system.webServer></span><span class="sxs-lookup"><span data-stu-id="35e5d-271">IIS \<system.webServer></span></span>](/iis/configuration/system.webServer/)
