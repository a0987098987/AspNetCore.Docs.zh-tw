---
title: "使用 IIS 模組與 ASP.NET Core"
author: guardrex
description: "探索使用中和非使用中的 IIS 模組 ASP.NET Core 應用程式，以及如何管理 IIS 模組。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: a6610e33abdc3eafb5908728b3299e95e6e7183f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a><span data-ttu-id="ad334-103">使用 IIS 模組與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad334-103">Using IIS Modules with ASP.NET Core</span></span>

<span data-ttu-id="ad334-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ad334-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ad334-105">IIS 裝載 ASP.NET Core 應用程式中的反向 proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="ad334-105">ASP.NET Core apps are hosted by IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="ad334-106">某些原生 IIS 模組及其所有的受管理的 IIS 模組不是可用來處理要求的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ad334-106">Some of the native IIS modules and all of the IIS managed modules aren't available to process requests for ASP.NET Core apps.</span></span> <span data-ttu-id="ad334-107">在許多情況下，ASP.NET Core 可提供替代性 IIS 原生和 managed 模組的功能。</span><span class="sxs-lookup"><span data-stu-id="ad334-107">In many cases, ASP.NET Core offers an alternative to the features of IIS native and managed modules.</span></span>

## <a name="native-modules"></a><span data-ttu-id="ad334-108">原生模組</span><span class="sxs-lookup"><span data-stu-id="ad334-108">Native modules</span></span>

| <span data-ttu-id="ad334-109">Module</span><span class="sxs-lookup"><span data-stu-id="ad334-109">Module</span></span> | <span data-ttu-id="ad334-110">.NET core 作用中</span><span class="sxs-lookup"><span data-stu-id="ad334-110">.NET Core Active</span></span> | <span data-ttu-id="ad334-111">ASP.NET Core Option</span><span class="sxs-lookup"><span data-stu-id="ad334-111">ASP.NET Core Option</span></span> |
| ------ | :--------------: | ------------------- |
| <span data-ttu-id="ad334-112">**匿名驗證**</span><span class="sxs-lookup"><span data-stu-id="ad334-112">**Anonymous Authentication**</span></span><br>`AnonymousAuthenticationModule` | <span data-ttu-id="ad334-113">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-113">Yes</span></span> | |
| <span data-ttu-id="ad334-114">**基本驗證**</span><span class="sxs-lookup"><span data-stu-id="ad334-114">**Basic Authentication**</span></span><br>`BasicAuthenticationModule` | <span data-ttu-id="ad334-115">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-115">Yes</span></span> | |
| <span data-ttu-id="ad334-116">**用戶端憑證對應驗證**</span><span class="sxs-lookup"><span data-stu-id="ad334-116">**Client Certification Mapping Authentication**</span></span><br>`CertificateMappingAuthenticationModule` | <span data-ttu-id="ad334-117">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-117">Yes</span></span> | |
| <span data-ttu-id="ad334-118">**CGI**</span><span class="sxs-lookup"><span data-stu-id="ad334-118">**CGI**</span></span><br>`CgiModule` | <span data-ttu-id="ad334-119">否</span><span class="sxs-lookup"><span data-stu-id="ad334-119">No</span></span> | |
| <span data-ttu-id="ad334-120">**組態驗證**</span><span class="sxs-lookup"><span data-stu-id="ad334-120">**Configuration Validation**</span></span><br>`ConfigurationValidationModule` | <span data-ttu-id="ad334-121">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-121">Yes</span></span> | |
| <span data-ttu-id="ad334-122">**HTTP 錯誤**</span><span class="sxs-lookup"><span data-stu-id="ad334-122">**HTTP Errors**</span></span><br>`CustomErrorModule` | <span data-ttu-id="ad334-123">否</span><span class="sxs-lookup"><span data-stu-id="ad334-123">No</span></span> | [<span data-ttu-id="ad334-124">狀態碼頁中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-124">Status Code Pages Middleware</span></span>](xref:fundamentals/error-handling#configuring-status-code-pages) |
| <span data-ttu-id="ad334-125">**自訂記錄**</span><span class="sxs-lookup"><span data-stu-id="ad334-125">**Custom Logging**</span></span><br>`CustomLoggingModule` | <span data-ttu-id="ad334-126">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-126">Yes</span></span> | |
| <span data-ttu-id="ad334-127">**預設文件**</span><span class="sxs-lookup"><span data-stu-id="ad334-127">**Default Document**</span></span><br>`DefaultDocumentModule` | <span data-ttu-id="ad334-128">否</span><span class="sxs-lookup"><span data-stu-id="ad334-128">No</span></span> | [<span data-ttu-id="ad334-129">預設的檔案中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-129">Default Files Middleware</span></span>](xref:fundamentals/static-files#serve-a-default-document) |
| <span data-ttu-id="ad334-130">**摘要式驗證**</span><span class="sxs-lookup"><span data-stu-id="ad334-130">**Digest Authentication**</span></span><br>`DigestAuthenticationModule` | <span data-ttu-id="ad334-131">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-131">Yes</span></span> | |
| <span data-ttu-id="ad334-132">**目錄瀏覽**</span><span class="sxs-lookup"><span data-stu-id="ad334-132">**Directory Browsing**</span></span><br>`DirectoryListingModule` | <span data-ttu-id="ad334-133">否</span><span class="sxs-lookup"><span data-stu-id="ad334-133">No</span></span> | [<span data-ttu-id="ad334-134">目錄瀏覽中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-134">Directory Browsing Middleware</span></span>](xref:fundamentals/static-files#enable-directory-browsing) |
| <span data-ttu-id="ad334-135">**動態壓縮**</span><span class="sxs-lookup"><span data-stu-id="ad334-135">**Dynamic Compression**</span></span><br>`DynamicCompressionModule` | <span data-ttu-id="ad334-136">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-136">Yes</span></span> | [<span data-ttu-id="ad334-137">回應壓縮中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-137">Response Compression Middleware</span></span>](xref:performance/response-compression) |
| <span data-ttu-id="ad334-138">**追蹤**</span><span class="sxs-lookup"><span data-stu-id="ad334-138">**Tracing**</span></span><br>`FailedRequestsTracingModule` | <span data-ttu-id="ad334-139">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-139">Yes</span></span> | [<span data-ttu-id="ad334-140">ASP.NET Core 記錄</span><span class="sxs-lookup"><span data-stu-id="ad334-140">ASP.NET Core Logging</span></span>](xref:fundamentals/logging/index#the-tracesource-provider) |
| <span data-ttu-id="ad334-141">**檔案快取**</span><span class="sxs-lookup"><span data-stu-id="ad334-141">**File Caching**</span></span><br>`FileCacheModule` | <span data-ttu-id="ad334-142">否</span><span class="sxs-lookup"><span data-stu-id="ad334-142">No</span></span> | [<span data-ttu-id="ad334-143">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-143">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="ad334-144">**HTTP 快取**</span><span class="sxs-lookup"><span data-stu-id="ad334-144">**HTTP Caching**</span></span><br>`HttpCacheModule` | <span data-ttu-id="ad334-145">否</span><span class="sxs-lookup"><span data-stu-id="ad334-145">No</span></span> | [<span data-ttu-id="ad334-146">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-146">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="ad334-147">**HTTP 記錄**</span><span class="sxs-lookup"><span data-stu-id="ad334-147">**HTTP Logging**</span></span><br>`HttpLoggingModule` | <span data-ttu-id="ad334-148">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-148">Yes</span></span> | [<span data-ttu-id="ad334-149">ASP.NET Core 記錄</span><span class="sxs-lookup"><span data-stu-id="ad334-149">ASP.NET Core Logging</span></span>](xref:fundamentals/logging/index)<br><span data-ttu-id="ad334-150">實作： [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)， [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)， [NLog](https://github.com/NLog/NLog.Extensions.Logging)， [Serilog](https://github.com/serilog/serilog-extensions-logging)</span><span class="sxs-lookup"><span data-stu-id="ad334-150">Implementations: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)</span></span>
| <span data-ttu-id="ad334-151">**HTTP 重新導向**</span><span class="sxs-lookup"><span data-stu-id="ad334-151">**HTTP Redirection**</span></span><br>`HttpRedirectionModule` | <span data-ttu-id="ad334-152">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-152">Yes</span></span> | [<span data-ttu-id="ad334-153">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="ad334-154">**IIS 用戶端憑證對應驗證**</span><span class="sxs-lookup"><span data-stu-id="ad334-154">**IIS Client Certificate Mapping Authentication**</span></span><br>`IISCertificateMappingAuthenticationModule` | <span data-ttu-id="ad334-155">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-155">Yes</span></span> | |
| <span data-ttu-id="ad334-156">**IP 及網域限制**</span><span class="sxs-lookup"><span data-stu-id="ad334-156">**IP and Domain Restrictions**</span></span><br>`IpRestrictionModule` | <span data-ttu-id="ad334-157">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-157">Yes</span></span> | |
| <span data-ttu-id="ad334-158">**ISAPI 篩選器**</span><span class="sxs-lookup"><span data-stu-id="ad334-158">**ISAPI Filters**</span></span><br>`IsapiFilterModule` | <span data-ttu-id="ad334-159">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-159">Yes</span></span> | [<span data-ttu-id="ad334-160">中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-160">Middleware</span></span>](xref:fundamentals/middleware/index) |
| <span data-ttu-id="ad334-161">**ISAPI**</span><span class="sxs-lookup"><span data-stu-id="ad334-161">**ISAPI**</span></span><br>`IsapiModule` | <span data-ttu-id="ad334-162">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-162">Yes</span></span> | [<span data-ttu-id="ad334-163">中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-163">Middleware</span></span>](xref:fundamentals/middleware/index) |
| <span data-ttu-id="ad334-164">**通訊協定支援**</span><span class="sxs-lookup"><span data-stu-id="ad334-164">**Protocol Support**</span></span><br>`ProtocolSupportModule` | <span data-ttu-id="ad334-165">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-165">Yes</span></span> | |
| <span data-ttu-id="ad334-166">**要求篩選**</span><span class="sxs-lookup"><span data-stu-id="ad334-166">**Request Filtering**</span></span><br>`RequestFilteringModule` | <span data-ttu-id="ad334-167">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-167">Yes</span></span> | [<span data-ttu-id="ad334-168">URL 重寫中介軟體 `IRule`</span><span class="sxs-lookup"><span data-stu-id="ad334-168">URL Rewriting Middleware `IRule`</span></span>](xref:fundamentals/url-rewriting#irule-based-rule) |
| <span data-ttu-id="ad334-169">**要求監視器**</span><span class="sxs-lookup"><span data-stu-id="ad334-169">**Request Monitor**</span></span><br>`RequestMonitorModule` | <span data-ttu-id="ad334-170">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-170">Yes</span></span> | |
| <span data-ttu-id="ad334-171">**URL 重寫**</span><span class="sxs-lookup"><span data-stu-id="ad334-171">**URL Rewriting**</span></span><br>`RewriteModule` | <span data-ttu-id="ad334-172">Yes&#8224;</span><span class="sxs-lookup"><span data-stu-id="ad334-172">Yes&#8224;</span></span> | [<span data-ttu-id="ad334-173">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-173">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="ad334-174">**伺服器端包含**</span><span class="sxs-lookup"><span data-stu-id="ad334-174">**Server-Side Includes**</span></span><br>`ServerSideIncludeModule` | <span data-ttu-id="ad334-175">否</span><span class="sxs-lookup"><span data-stu-id="ad334-175">No</span></span> | |
| <span data-ttu-id="ad334-176">**靜態壓縮**</span><span class="sxs-lookup"><span data-stu-id="ad334-176">**Static Compression**</span></span><br>`StaticCompressionModule` | <span data-ttu-id="ad334-177">否</span><span class="sxs-lookup"><span data-stu-id="ad334-177">No</span></span> | [<span data-ttu-id="ad334-178">回應壓縮中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-178">Response Compression Middleware</span></span>](xref:performance/response-compression) |
| <span data-ttu-id="ad334-179">**靜態內容**</span><span class="sxs-lookup"><span data-stu-id="ad334-179">**Static Content**</span></span><br>`StaticFileModule` | <span data-ttu-id="ad334-180">否</span><span class="sxs-lookup"><span data-stu-id="ad334-180">No</span></span> | [<span data-ttu-id="ad334-181">靜態檔案中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-181">Static File Middleware</span></span>](xref:fundamentals/static-files) |
| <span data-ttu-id="ad334-182">**權杖快取**</span><span class="sxs-lookup"><span data-stu-id="ad334-182">**Token Caching**</span></span><br>`TokenCacheModule` | <span data-ttu-id="ad334-183">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-183">Yes</span></span> | |
| <span data-ttu-id="ad334-184">**URI 快取**</span><span class="sxs-lookup"><span data-stu-id="ad334-184">**URI Caching**</span></span><br>`UriCacheModule` | <span data-ttu-id="ad334-185">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-185">Yes</span></span> | |
| <span data-ttu-id="ad334-186">**URL 授權**</span><span class="sxs-lookup"><span data-stu-id="ad334-186">**URL Authorization**</span></span><br>`UrlAuthorizationModule` | <span data-ttu-id="ad334-187">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-187">Yes</span></span> | [<span data-ttu-id="ad334-188">ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="ad334-188">ASP.NET Core Identity</span></span>](xref:security/authentication/identity) |
| <span data-ttu-id="ad334-189">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="ad334-189">**Windows Authentication**</span></span><br>`WindowsAuthenticationModule` | <span data-ttu-id="ad334-190">[是]</span><span class="sxs-lookup"><span data-stu-id="ad334-190">Yes</span></span> | |

<span data-ttu-id="ad334-191">&#8224;URL Rewrite Module 的`isFile`和`isDirectory`相符項目類型不適用於 ASP.NET Core 應用程式，因為已變更的[目錄結構](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="ad334-191">&#8224;The URL Rewrite Module's `isFile` and `isDirectory` match types don't work with ASP.NET Core apps due to the changes in [directory structure](xref:host-and-deploy/directory-structure).</span></span>

## <a name="managed-modules"></a><span data-ttu-id="ad334-192">受管理的模組</span><span class="sxs-lookup"><span data-stu-id="ad334-192">Managed modules</span></span>

| <span data-ttu-id="ad334-193">Module</span><span class="sxs-lookup"><span data-stu-id="ad334-193">Module</span></span>                  | <span data-ttu-id="ad334-194">.NET core 作用中</span><span class="sxs-lookup"><span data-stu-id="ad334-194">.NET Core Active</span></span> | <span data-ttu-id="ad334-195">ASP.NET Core Option</span><span class="sxs-lookup"><span data-stu-id="ad334-195">ASP.NET Core Option</span></span> |
| ----------------------- | :--------------: | ------------------- |
| <span data-ttu-id="ad334-196">AnonymousIdentification</span><span class="sxs-lookup"><span data-stu-id="ad334-196">AnonymousIdentification</span></span> | <span data-ttu-id="ad334-197">否</span><span class="sxs-lookup"><span data-stu-id="ad334-197">No</span></span>               | |
| <span data-ttu-id="ad334-198">DefaultAuthentication</span><span class="sxs-lookup"><span data-stu-id="ad334-198">DefaultAuthentication</span></span>   | <span data-ttu-id="ad334-199">否</span><span class="sxs-lookup"><span data-stu-id="ad334-199">No</span></span>               | |
| <span data-ttu-id="ad334-200">FileAuthorization</span><span class="sxs-lookup"><span data-stu-id="ad334-200">FileAuthorization</span></span>       | <span data-ttu-id="ad334-201">否</span><span class="sxs-lookup"><span data-stu-id="ad334-201">No</span></span>               | |
| <span data-ttu-id="ad334-202">FormsAuthentication</span><span class="sxs-lookup"><span data-stu-id="ad334-202">FormsAuthentication</span></span>     | <span data-ttu-id="ad334-203">否</span><span class="sxs-lookup"><span data-stu-id="ad334-203">No</span></span>               | [<span data-ttu-id="ad334-204">Cookie 驗證中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-204">Cookie Authentication Middleware</span></span>](xref:security/authentication/cookie) |
| <span data-ttu-id="ad334-205">OutputCache</span><span class="sxs-lookup"><span data-stu-id="ad334-205">OutputCache</span></span>             | <span data-ttu-id="ad334-206">否</span><span class="sxs-lookup"><span data-stu-id="ad334-206">No</span></span>               | [<span data-ttu-id="ad334-207">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-207">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="ad334-208">設定檔</span><span class="sxs-lookup"><span data-stu-id="ad334-208">Profile</span></span>                 | <span data-ttu-id="ad334-209">否</span><span class="sxs-lookup"><span data-stu-id="ad334-209">No</span></span>               | |
| <span data-ttu-id="ad334-210">RoleManager</span><span class="sxs-lookup"><span data-stu-id="ad334-210">RoleManager</span></span>             | <span data-ttu-id="ad334-211">否</span><span class="sxs-lookup"><span data-stu-id="ad334-211">No</span></span>               | |
| <span data-ttu-id="ad334-212">ScriptModule-4.0</span><span class="sxs-lookup"><span data-stu-id="ad334-212">ScriptModule-4.0</span></span>        | <span data-ttu-id="ad334-213">否</span><span class="sxs-lookup"><span data-stu-id="ad334-213">No</span></span>               | |
| <span data-ttu-id="ad334-214">工作階段</span><span class="sxs-lookup"><span data-stu-id="ad334-214">Session</span></span>                 | <span data-ttu-id="ad334-215">否</span><span class="sxs-lookup"><span data-stu-id="ad334-215">No</span></span>               | [<span data-ttu-id="ad334-216">工作階段中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-216">Session Middleware</span></span>](xref:fundamentals/app-state) |
| <span data-ttu-id="ad334-217">UrlAuthorization</span><span class="sxs-lookup"><span data-stu-id="ad334-217">UrlAuthorization</span></span>        | <span data-ttu-id="ad334-218">否</span><span class="sxs-lookup"><span data-stu-id="ad334-218">No</span></span>               | |
| <span data-ttu-id="ad334-219">UrlMappingsModule</span><span class="sxs-lookup"><span data-stu-id="ad334-219">UrlMappingsModule</span></span>       | <span data-ttu-id="ad334-220">否</span><span class="sxs-lookup"><span data-stu-id="ad334-220">No</span></span>               | [<span data-ttu-id="ad334-221">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="ad334-221">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="ad334-222">UrlRoutingModule-4.0</span><span class="sxs-lookup"><span data-stu-id="ad334-222">UrlRoutingModule-4.0</span></span>    | <span data-ttu-id="ad334-223">否</span><span class="sxs-lookup"><span data-stu-id="ad334-223">No</span></span>               | [<span data-ttu-id="ad334-224">ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="ad334-224">ASP.NET Core Identity</span></span>](xref:security/authentication/identity) |
| <span data-ttu-id="ad334-225">WindowsAuthentication</span><span class="sxs-lookup"><span data-stu-id="ad334-225">WindowsAuthentication</span></span>   | <span data-ttu-id="ad334-226">否</span><span class="sxs-lookup"><span data-stu-id="ad334-226">No</span></span>               | |

## <a name="iis-manager-application-changes"></a><span data-ttu-id="ad334-227">IIS 管理員變更應用程式</span><span class="sxs-lookup"><span data-stu-id="ad334-227">IIS Manager application changes</span></span>

<span data-ttu-id="ad334-228">當使用 IIS 管理員來設定設定值， *web.config*應用程式的檔案變更時。</span><span class="sxs-lookup"><span data-stu-id="ad334-228">When using IIS Manager to configure settings, the *web.config* file of the app is changed.</span></span> <span data-ttu-id="ad334-229">如果部署應用程式，包括*web.config*，使用 IIS 管理員進行任何變更會覆寫的已部署*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="ad334-229">If deploying an app and including *web.config*, any changes made with IIS Manager are overwritten by the deployed *web.config* file.</span></span> <span data-ttu-id="ad334-230">如果變更伺服器的*web.config*檔案中，複製已更新*web.config*立即的本機專案在伺服器上的檔案。</span><span class="sxs-lookup"><span data-stu-id="ad334-230">If changes are made to the server's *web.config* file, copy the updated *web.config* file on the server to the local project immediately.</span></span>

## <a name="disabling-iis-modules"></a><span data-ttu-id="ad334-231">停用 IIS 模組</span><span class="sxs-lookup"><span data-stu-id="ad334-231">Disabling IIS modules</span></span>

<span data-ttu-id="ad334-232">如果必須停用應用程式中，新增至應用程式的伺服器層級設定的 IIS 模組*web.config*檔案可以停用模組。</span><span class="sxs-lookup"><span data-stu-id="ad334-232">If an IIS module is configured at the server level that must be disabled for an app, an addition to the app's *web.config* file can disable the module.</span></span> <span data-ttu-id="ad334-233">請讓模組留在原處和停用它使用組態設定 （如果有的話），或將模組從應用程式中移除。</span><span class="sxs-lookup"><span data-stu-id="ad334-233">Either leave the module in place and deactivate it using a configuration setting (if available) or remove the module from the app.</span></span>

### <a name="module-deactivation"></a><span data-ttu-id="ad334-234">模組停用</span><span class="sxs-lookup"><span data-stu-id="ad334-234">Module deactivation</span></span>

<span data-ttu-id="ad334-235">許多模組提供可讓它們不會從應用程式移除此模組會停用的組態設定。</span><span class="sxs-lookup"><span data-stu-id="ad334-235">Many modules offer a configuration setting that allows them to be disabled without removing the module from the app.</span></span> <span data-ttu-id="ad334-236">這是用來停用模組的最簡單且最快方式。</span><span class="sxs-lookup"><span data-stu-id="ad334-236">This is the simplest and quickest way to deactivate a module.</span></span> <span data-ttu-id="ad334-237">例如，IIS URL Rewrite Module 可以停用與 **\<httpRedirect >**中的項目*web.config*:</span><span class="sxs-lookup"><span data-stu-id="ad334-237">For example, the IIS URL Rewrite Module can be disabled with the **\<httpRedirect>** element in *web.config*:</span></span>

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="ad334-238">如需有關如何停用模組組態設定的詳細資訊，請依照下列中的連結*子項目*區段[IIS \<system.webServer >](/iis/configuration/system.webServer/)。</span><span class="sxs-lookup"><span data-stu-id="ad334-238">For more information on disabling modules with configuration settings, follow the links in the *Child Elements* section of [IIS \<system.webServer>](/iis/configuration/system.webServer/).</span></span>

### <a name="module-removal"></a><span data-ttu-id="ad334-239">模組移除</span><span class="sxs-lookup"><span data-stu-id="ad334-239">Module removal</span></span>

<span data-ttu-id="ad334-240">如果選擇移除模組中的設定與*web.config*、 模組解除鎖定和解除鎖定**\<模組 >**區段*web.config*第一次：</span><span class="sxs-lookup"><span data-stu-id="ad334-240">If opting to remove a module with a setting in *web.config*, unlock the module and unlock the **\<modules>** section of *web.config* first:</span></span>

1. <span data-ttu-id="ad334-241">解除鎖定伺服器層級的模組。</span><span class="sxs-lookup"><span data-stu-id="ad334-241">Unlock the module at the server level.</span></span> <span data-ttu-id="ad334-242">選取 IIS 伺服器上 IIS Manager 中**連線**[資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="ad334-242">Select the IIS server in the IIS Manager **Connections** sidebar.</span></span> <span data-ttu-id="ad334-243">開啟**模組**中**IIS**區域。</span><span class="sxs-lookup"><span data-stu-id="ad334-243">Open the **Modules** in the **IIS** area.</span></span> <span data-ttu-id="ad334-244">在清單中選取的模組。</span><span class="sxs-lookup"><span data-stu-id="ad334-244">Select the module in the list.</span></span> <span data-ttu-id="ad334-245">在**動作**在右側，資訊看板選取**Unlock**。</span><span class="sxs-lookup"><span data-stu-id="ad334-245">In the **Actions** sidebar on the right, select **Unlock**.</span></span> <span data-ttu-id="ad334-246">解除鎖定數量的模組，當您規劃要移除*web.config*更新版本。</span><span class="sxs-lookup"><span data-stu-id="ad334-246">Unlock as many modules as you plan to remove from *web.config* later.</span></span>

1. <span data-ttu-id="ad334-247">部署應用程式不含**\<模組 >**一節中*web.config*。如果應用程式部署與*web.config*包含**\<模組 >**區段，而不需要解除鎖定區段第一次在 IIS 管理員中，Configuration Manager 就會擲回例外狀況當嘗試解除鎖定的區段。</span><span class="sxs-lookup"><span data-stu-id="ad334-247">Deploy the app without a **\<modules>** section in *web.config*. If an app is deployed with a *web.config* containing the **\<modules>** section without having unlocked the section first in the IIS Manager, the Configuration Manager throws an exception when attempting to unlock the section.</span></span> <span data-ttu-id="ad334-248">因此，部署應用程式不含**\<模組 >** > 一節。</span><span class="sxs-lookup"><span data-stu-id="ad334-248">Therefore, deploy the app without a **\<modules>** section.</span></span>

1. <span data-ttu-id="ad334-249">解除鎖定**\<模組 >**區段*web.config*。在**連線**提要欄位中，選取的網站**網站**。</span><span class="sxs-lookup"><span data-stu-id="ad334-249">Unlock the **\<modules>** section of *web.config*. In the **Connections** sidebar, select the website in **Sites**.</span></span> <span data-ttu-id="ad334-250">在**管理**區域中，開啟**組態編輯器**。</span><span class="sxs-lookup"><span data-stu-id="ad334-250">In the **Management** area, open the **Configuration Editor**.</span></span> <span data-ttu-id="ad334-251">使用瀏覽控制項，來選取`system.webServer/modules`> 一節。</span><span class="sxs-lookup"><span data-stu-id="ad334-251">Use the navigation controls to select the `system.webServer/modules` section.</span></span> <span data-ttu-id="ad334-252">在**動作**來選取在右側，資訊看板**Unlock** > 一節。</span><span class="sxs-lookup"><span data-stu-id="ad334-252">In the **Actions** sidebar on the right, select to **Unlock** the section.</span></span>

1. <span data-ttu-id="ad334-253">此時， **\<模組 >**區段可以新增至*web.config*檔案搭配**\<移除 >**從模組移除的項目應用程式。</span><span class="sxs-lookup"><span data-stu-id="ad334-253">At this point, a **\<modules>** section can be added to the *web.config* file with a **\<remove>** element to remove the module from the app.</span></span> <span data-ttu-id="ad334-254">多個**\<移除 >**項目可以加入要移除多個模組。</span><span class="sxs-lookup"><span data-stu-id="ad334-254">Multiple **\<remove>** elements can be added to remove multiple modules.</span></span> <span data-ttu-id="ad334-255">如果*web.config*伺服器上進行變更，立即進行相同的變更，在專案的*web.config*在本機檔案。</span><span class="sxs-lookup"><span data-stu-id="ad334-255">If *web.config* changes are made on the server, immediately make the same changes to the project's *web.config* file locally.</span></span> <span data-ttu-id="ad334-256">這種方式移除模組不會影響其他應用程式伺服器上之模組的使用。</span><span class="sxs-lookup"><span data-stu-id="ad334-256">Removing a module this way won't affect the use of the module with other apps on the server.</span></span>

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

<span data-ttu-id="ad334-257">IIS 安裝使用預設安裝的模組，請使用下列**\<模組 >**區段，以移除預設的模組。</span><span class="sxs-lookup"><span data-stu-id="ad334-257">For an IIS installation with the default modules installed, use the following **\<module>** section to remove the default modules.</span></span>

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

<span data-ttu-id="ad334-258">IIS 模組也可以移除與*Appcmd.exe*。</span><span class="sxs-lookup"><span data-stu-id="ad334-258">An IIS module can also be removed with *Appcmd.exe*.</span></span> <span data-ttu-id="ad334-259">提供`MODULE_NAME`和`APPLICATION_NAME`命令中：</span><span class="sxs-lookup"><span data-stu-id="ad334-259">Provide the `MODULE_NAME` and `APPLICATION_NAME` in the command:</span></span>

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

<span data-ttu-id="ad334-260">例如，移除`DynamicCompressionModule`從預設的網站：</span><span class="sxs-lookup"><span data-stu-id="ad334-260">For example, remove the `DynamicCompressionModule` from the Default Web Site:</span></span>

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a><span data-ttu-id="ad334-261">最小模組設定</span><span class="sxs-lookup"><span data-stu-id="ad334-261">Minimum module configuration</span></span>

<span data-ttu-id="ad334-262">只有執行 ASP.NET Core 應用程式所需的模組是匿名驗證模組和 ASP.NET 核心模組。</span><span class="sxs-lookup"><span data-stu-id="ad334-262">The only modules required to run an ASP.NET Core app are the Anonymous Authentication Module and the ASP.NET Core Module.</span></span>

![IIS 管理員 中開啟模組顯示的最小模組設定](modules/_static/modules.png)

## <a name="additional-resources"></a><span data-ttu-id="ad334-264">其他資源</span><span class="sxs-lookup"><span data-stu-id="ad334-264">Additional resources</span></span>

* [<span data-ttu-id="ad334-265"> Windows 上使用 IIS 的主機</span><span class="sxs-lookup"><span data-stu-id="ad334-265">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="ad334-266">IIS 架構簡介： 在 IIS 中的模組</span><span class="sxs-lookup"><span data-stu-id="ad334-266">Introduction to IIS Architectures: Modules in IIS</span></span>](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [<span data-ttu-id="ad334-267">IIS 模組概觀</span><span class="sxs-lookup"><span data-stu-id="ad334-267">IIS Modules Overview</span></span>](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [<span data-ttu-id="ad334-268">自訂的 IIS 7.0 角色和模組</span><span class="sxs-lookup"><span data-stu-id="ad334-268">Customizing IIS 7.0 Roles and Modules</span></span>](https://technet.microsoft.com/library/cc627313.aspx)
* [<span data-ttu-id="ad334-269">IIS `<system.webServer>`</span><span class="sxs-lookup"><span data-stu-id="ad334-269">IIS `<system.webServer>`</span></span>](/iis/configuration/system.webServer/)
