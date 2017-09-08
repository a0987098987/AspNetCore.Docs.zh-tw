---
title: "使用 IIS 模組與 ASP.NET Core"
author: guardrex
description: "說明 ASP.NET Core 應用程式的作用中和非使用中的 IIS 模組的參考文件。"
keywords: "ASP.NET Core iis、 模組、 反向 proxy"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: 4d9d3d863e62373716ecd81b9e1880145a127e9b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a><span data-ttu-id="75493-104">使用 IIS 模組與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75493-104">Using IIS Modules with ASP.NET Core</span></span>

<span data-ttu-id="75493-105">由[Luke Latham](https://github.com/GuardRex)</span><span class="sxs-lookup"><span data-stu-id="75493-105">By [Luke Latham](https://github.com/GuardRex)</span></span>

<span data-ttu-id="75493-106">由 IIS 所裝載 ASP.NET Core 應用程式中的反向 proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="75493-106">ASP.NET Core applications are hosted by IIS in a reverse-proxy configuration.</span></span> <span data-ttu-id="75493-107">原生 IIS 模組的某些和所有 IIS 管理模組並非可用來處理要求的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="75493-107">Some of the native IIS modules and all of the IIS managed modules are not available to process requests for ASP.NET Core apps.</span></span> <span data-ttu-id="75493-108">在許多情況下，ASP.NET Core 可提供替代性 IIS 原生和 managed 模組的功能。</span><span class="sxs-lookup"><span data-stu-id="75493-108">In many cases, ASP.NET Core offers an alternative to the features of IIS native and managed modules.</span></span>

## <a name="native-modules"></a><span data-ttu-id="75493-109">原生模組</span><span class="sxs-lookup"><span data-stu-id="75493-109">Native Modules</span></span>
<span data-ttu-id="75493-110">模組</span><span class="sxs-lookup"><span data-stu-id="75493-110">Module</span></span> | <span data-ttu-id="75493-111">.NET core 作用中</span><span class="sxs-lookup"><span data-stu-id="75493-111">.NET Core Active</span></span> | <span data-ttu-id="75493-112">ASP.NET Core 選項</span><span class="sxs-lookup"><span data-stu-id="75493-112">ASP.NET Core Option</span></span>
--- | :---: | ---
<span data-ttu-id="75493-113">**匿名驗證**</span><span class="sxs-lookup"><span data-stu-id="75493-113">**Anonymous Authentication**</span></span><br>`AnonymousAuthenticationModule` | <span data-ttu-id="75493-114">是</span><span class="sxs-lookup"><span data-stu-id="75493-114">Yes</span></span> | 
<span data-ttu-id="75493-115">**基本驗證**</span><span class="sxs-lookup"><span data-stu-id="75493-115">**Basic Authentication**</span></span><br>`BasicAuthenticationModule` | <span data-ttu-id="75493-116">是</span><span class="sxs-lookup"><span data-stu-id="75493-116">Yes</span></span> | 
<span data-ttu-id="75493-117">**用戶端憑證對應驗證**</span><span class="sxs-lookup"><span data-stu-id="75493-117">**Client Certification Mapping Authentication**</span></span><br>`CertificateMappingAuthenticationModule` | <span data-ttu-id="75493-118">是</span><span class="sxs-lookup"><span data-stu-id="75493-118">Yes</span></span> | 
<span data-ttu-id="75493-119">**CGI**</span><span class="sxs-lookup"><span data-stu-id="75493-119">**CGI**</span></span><br>`CgiModule` | <span data-ttu-id="75493-120">否</span><span class="sxs-lookup"><span data-stu-id="75493-120">No</span></span> | 
<span data-ttu-id="75493-121">**組態驗證**</span><span class="sxs-lookup"><span data-stu-id="75493-121">**Configuration Validation**</span></span><br>`ConfigurationValidationModule` | <span data-ttu-id="75493-122">是</span><span class="sxs-lookup"><span data-stu-id="75493-122">Yes</span></span> | 
<span data-ttu-id="75493-123">**HTTP 錯誤**</span><span class="sxs-lookup"><span data-stu-id="75493-123">**HTTP Errors**</span></span><br>`CustomErrorModule` | <span data-ttu-id="75493-124">否</span><span class="sxs-lookup"><span data-stu-id="75493-124">No</span></span> | [<span data-ttu-id="75493-125">狀態碼頁中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-125">Status Code Pages Middleware</span></span>](xref:fundamentals/error-handling#configuring-status-code-pages)
<span data-ttu-id="75493-126">**自訂記錄**</span><span class="sxs-lookup"><span data-stu-id="75493-126">**Custom Logging**</span></span><br>`CustomLoggingModule` | <span data-ttu-id="75493-127">是</span><span class="sxs-lookup"><span data-stu-id="75493-127">Yes</span></span> | 
<span data-ttu-id="75493-128">**預設文件**</span><span class="sxs-lookup"><span data-stu-id="75493-128">**Default Document**</span></span><br>`DefaultDocumentModule` | <span data-ttu-id="75493-129">否</span><span class="sxs-lookup"><span data-stu-id="75493-129">No</span></span> | [<span data-ttu-id="75493-130">預設的檔案中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-130">Default Files Middleware</span></span>](xref:fundamentals/static-files#serving-a-default-document)
<span data-ttu-id="75493-131">**摘要式驗證**</span><span class="sxs-lookup"><span data-stu-id="75493-131">**Digest Authentication**</span></span><br>`DigestAuthenticationModule` | <span data-ttu-id="75493-132">是</span><span class="sxs-lookup"><span data-stu-id="75493-132">Yes</span></span> | 
<span data-ttu-id="75493-133">**目錄瀏覽**</span><span class="sxs-lookup"><span data-stu-id="75493-133">**Directory Browsing**</span></span><br>`DirectoryListingModule` | <span data-ttu-id="75493-134">否</span><span class="sxs-lookup"><span data-stu-id="75493-134">No</span></span> | [<span data-ttu-id="75493-135">目錄瀏覽中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-135">Directory Browsing Middleware</span></span>](xref:fundamentals/static-files#enabling-directory-browsing)
<span data-ttu-id="75493-136">**動態壓縮**</span><span class="sxs-lookup"><span data-stu-id="75493-136">**Dynamic Compression**</span></span><br>`DynamicCompressionModule` | <span data-ttu-id="75493-137">是</span><span class="sxs-lookup"><span data-stu-id="75493-137">Yes</span></span> | [<span data-ttu-id="75493-138">回應壓縮中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-138">Response Compression Middleware</span></span>](xref:performance/response-compression)
<span data-ttu-id="75493-139">**追蹤**</span><span class="sxs-lookup"><span data-stu-id="75493-139">**Tracing**</span></span><br>`FailedRequestsTracingModule` | <span data-ttu-id="75493-140">是</span><span class="sxs-lookup"><span data-stu-id="75493-140">Yes</span></span> | [<span data-ttu-id="75493-141">ASP.NET Core 記錄</span><span class="sxs-lookup"><span data-stu-id="75493-141">ASP.NET Core Logging</span></span>](xref:fundamentals/logging#the-tracesource-provider)
<span data-ttu-id="75493-142">**檔案快取**</span><span class="sxs-lookup"><span data-stu-id="75493-142">**File Caching**</span></span><br>`FileCacheModule` | <span data-ttu-id="75493-143">否</span><span class="sxs-lookup"><span data-stu-id="75493-143">No</span></span> | [<span data-ttu-id="75493-144">回應的快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-144">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
<span data-ttu-id="75493-145">**HTTP 快取**</span><span class="sxs-lookup"><span data-stu-id="75493-145">**HTTP Caching**</span></span><br>`HttpCacheModule` | <span data-ttu-id="75493-146">否</span><span class="sxs-lookup"><span data-stu-id="75493-146">No</span></span> | [<span data-ttu-id="75493-147">回應的快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-147">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
<span data-ttu-id="75493-148">**HTTP 記錄**</span><span class="sxs-lookup"><span data-stu-id="75493-148">**HTTP Logging**</span></span><br>`HttpLoggingModule` | <span data-ttu-id="75493-149">是</span><span class="sxs-lookup"><span data-stu-id="75493-149">Yes</span></span> | [<span data-ttu-id="75493-150">ASP.NET Core 記錄</span><span class="sxs-lookup"><span data-stu-id="75493-150">ASP.NET Core Logging</span></span>](xref:fundamentals/logging)<br><span data-ttu-id="75493-151">實作： [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)， [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)， [NLog](https://github.com/NLog/NLog.Extensions.Logging)， [Serilog](https://github.com/serilog/serilog-framework-logging)</span><span class="sxs-lookup"><span data-stu-id="75493-151">Implementations: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-framework-logging)</span></span>
<span data-ttu-id="75493-152">**HTTP 重新導向**</span><span class="sxs-lookup"><span data-stu-id="75493-152">**HTTP Redirection**</span></span><br>`HttpRedirectionModule` | <span data-ttu-id="75493-153">是</span><span class="sxs-lookup"><span data-stu-id="75493-153">Yes</span></span> | [<span data-ttu-id="75493-154">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-154">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
<span data-ttu-id="75493-155">**IIS 用戶端憑證對應驗證**</span><span class="sxs-lookup"><span data-stu-id="75493-155">**IIS Client Certificate Mapping Authentication**</span></span><br>`IISCertificateMappingAuthenticationModule` | <span data-ttu-id="75493-156">是</span><span class="sxs-lookup"><span data-stu-id="75493-156">Yes</span></span> | 
<span data-ttu-id="75493-157">**IP 及網域限制**</span><span class="sxs-lookup"><span data-stu-id="75493-157">**IP and Domain Restrictions**</span></span><br>`IpRestrictionModule` | <span data-ttu-id="75493-158">是</span><span class="sxs-lookup"><span data-stu-id="75493-158">Yes</span></span> | 
<span data-ttu-id="75493-159">**ISAPI 篩選器**</span><span class="sxs-lookup"><span data-stu-id="75493-159">**ISAPI Filters**</span></span><br>`IsapiFilterModule` | <span data-ttu-id="75493-160">是</span><span class="sxs-lookup"><span data-stu-id="75493-160">Yes</span></span> | [<span data-ttu-id="75493-161">中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-161">Middleware</span></span>](xref:fundamentals/middleware)
<span data-ttu-id="75493-162">**ISAPI**</span><span class="sxs-lookup"><span data-stu-id="75493-162">**ISAPI**</span></span><br>`IsapiModule` | <span data-ttu-id="75493-163">是</span><span class="sxs-lookup"><span data-stu-id="75493-163">Yes</span></span> | [<span data-ttu-id="75493-164">中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-164">Middleware</span></span>](xref:fundamentals/middleware)
<span data-ttu-id="75493-165">**通訊協定支援**</span><span class="sxs-lookup"><span data-stu-id="75493-165">**Protocol Support**</span></span><br>`ProtocolSupportModule` | <span data-ttu-id="75493-166">是</span><span class="sxs-lookup"><span data-stu-id="75493-166">Yes</span></span> | 
<span data-ttu-id="75493-167">**要求篩選**</span><span class="sxs-lookup"><span data-stu-id="75493-167">**Request Filtering**</span></span><br>`RequestFilteringModule` | <span data-ttu-id="75493-168">是</span><span class="sxs-lookup"><span data-stu-id="75493-168">Yes</span></span> | [<span data-ttu-id="75493-169">URL 重寫中介軟體`IRule`</span><span class="sxs-lookup"><span data-stu-id="75493-169">URL Rewriting Middleware `IRule`</span></span>](xref:fundamentals/url-rewriting#irule-based-rule)
<span data-ttu-id="75493-170">**要求監視器**</span><span class="sxs-lookup"><span data-stu-id="75493-170">**Request Monitor**</span></span><br>`RequestMonitorModule` | <span data-ttu-id="75493-171">是</span><span class="sxs-lookup"><span data-stu-id="75493-171">Yes</span></span> | 
<span data-ttu-id="75493-172">**URL 重寫**</span><span class="sxs-lookup"><span data-stu-id="75493-172">**URL Rewriting**</span></span><br>`RewriteModule` | <span data-ttu-id="75493-173">Yes†</span><span class="sxs-lookup"><span data-stu-id="75493-173">Yes†</span></span> | [<span data-ttu-id="75493-174">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-174">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
<span data-ttu-id="75493-175">**伺服器端包含**</span><span class="sxs-lookup"><span data-stu-id="75493-175">**Server Side Includes**</span></span><br>`ServerSideIncludeModule` | <span data-ttu-id="75493-176">否</span><span class="sxs-lookup"><span data-stu-id="75493-176">No</span></span> | 
<span data-ttu-id="75493-177">**靜態壓縮**</span><span class="sxs-lookup"><span data-stu-id="75493-177">**Static Compression**</span></span><br>`StaticCompressionModule` | <span data-ttu-id="75493-178">否</span><span class="sxs-lookup"><span data-stu-id="75493-178">No</span></span> | [<span data-ttu-id="75493-179">回應壓縮中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-179">Response Compression Middleware</span></span>](xref:performance/response-compression)
<span data-ttu-id="75493-180">**靜態內容**</span><span class="sxs-lookup"><span data-stu-id="75493-180">**Static Content**</span></span><br>`StaticFileModule` | <span data-ttu-id="75493-181">否</span><span class="sxs-lookup"><span data-stu-id="75493-181">No</span></span> | [<span data-ttu-id="75493-182">靜態檔案中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-182">Static File Middleware</span></span>](xref:fundamentals/static-files)
<span data-ttu-id="75493-183">**權杖快取**</span><span class="sxs-lookup"><span data-stu-id="75493-183">**Token Caching**</span></span><br>`TokenCacheModule` | <span data-ttu-id="75493-184">是</span><span class="sxs-lookup"><span data-stu-id="75493-184">Yes</span></span> | 
<span data-ttu-id="75493-185">**URI 快取**</span><span class="sxs-lookup"><span data-stu-id="75493-185">**URI Caching**</span></span><br>`UriCacheModule` | <span data-ttu-id="75493-186">是</span><span class="sxs-lookup"><span data-stu-id="75493-186">Yes</span></span> | 
<span data-ttu-id="75493-187">**URL 授權**</span><span class="sxs-lookup"><span data-stu-id="75493-187">**URL Authorization**</span></span><br>`UrlAuthorizationModule` | <span data-ttu-id="75493-188">是</span><span class="sxs-lookup"><span data-stu-id="75493-188">Yes</span></span> | [<span data-ttu-id="75493-189">ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="75493-189">ASP.NET Core Identity</span></span>](xref:security/authentication/identity)
<span data-ttu-id="75493-190">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="75493-190">**Windows Authentication**</span></span><br>`WindowsAuthenticationModule` | <span data-ttu-id="75493-191">是</span><span class="sxs-lookup"><span data-stu-id="75493-191">Yes</span></span> | 

<span data-ttu-id="75493-192">†The URL Rewrite Module 的`isFile`和`isDirectory`無法搭配 ASP.NET Core 應用程式，因為變更[目錄結構](xref:hosting/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="75493-192">†The URL Rewrite Module's `isFile` and `isDirectory` do not work with ASP.NET Core applications due to the changes in [directory structure](xref:hosting/directory-structure).</span></span>

## <a name="managed-modules"></a><span data-ttu-id="75493-193">受管理的模組</span><span class="sxs-lookup"><span data-stu-id="75493-193">Managed Modules</span></span>
<span data-ttu-id="75493-194">模組</span><span class="sxs-lookup"><span data-stu-id="75493-194">Module</span></span> | <span data-ttu-id="75493-195">.NET core 作用中</span><span class="sxs-lookup"><span data-stu-id="75493-195">.NET Core Active</span></span> | <span data-ttu-id="75493-196">ASP.NET Core 選項</span><span class="sxs-lookup"><span data-stu-id="75493-196">ASP.NET Core Option</span></span>
--- | :---: | ---
<span data-ttu-id="75493-197">AnonymousIdentification</span><span class="sxs-lookup"><span data-stu-id="75493-197">AnonymousIdentification</span></span> | <span data-ttu-id="75493-198">否</span><span class="sxs-lookup"><span data-stu-id="75493-198">No</span></span> | 
<span data-ttu-id="75493-199">DefaultAuthentication</span><span class="sxs-lookup"><span data-stu-id="75493-199">DefaultAuthentication</span></span> | <span data-ttu-id="75493-200">否</span><span class="sxs-lookup"><span data-stu-id="75493-200">No</span></span> | 
<span data-ttu-id="75493-201">FileAuthorization</span><span class="sxs-lookup"><span data-stu-id="75493-201">FileAuthorization</span></span> | <span data-ttu-id="75493-202">否</span><span class="sxs-lookup"><span data-stu-id="75493-202">No</span></span> | 
<span data-ttu-id="75493-203">FormsAuthentication</span><span class="sxs-lookup"><span data-stu-id="75493-203">FormsAuthentication</span></span> | <span data-ttu-id="75493-204">否</span><span class="sxs-lookup"><span data-stu-id="75493-204">No</span></span> | [<span data-ttu-id="75493-205">Cookie 驗證中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-205">Cookie Authentication Middleware</span></span>](xref:security/authentication/cookie)
<span data-ttu-id="75493-206">OutputCache</span><span class="sxs-lookup"><span data-stu-id="75493-206">OutputCache</span></span> | <span data-ttu-id="75493-207">否</span><span class="sxs-lookup"><span data-stu-id="75493-207">No</span></span> | [<span data-ttu-id="75493-208">回應的快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-208">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
<span data-ttu-id="75493-209">設定檔</span><span class="sxs-lookup"><span data-stu-id="75493-209">Profile</span></span> | <span data-ttu-id="75493-210">否</span><span class="sxs-lookup"><span data-stu-id="75493-210">No</span></span> | 
<span data-ttu-id="75493-211">RoleManager</span><span class="sxs-lookup"><span data-stu-id="75493-211">RoleManager</span></span> | <span data-ttu-id="75493-212">否</span><span class="sxs-lookup"><span data-stu-id="75493-212">No</span></span> | 
<span data-ttu-id="75493-213">ScriptModule 4.0</span><span class="sxs-lookup"><span data-stu-id="75493-213">ScriptModule-4.0</span></span> | <span data-ttu-id="75493-214">否</span><span class="sxs-lookup"><span data-stu-id="75493-214">No</span></span> | 
<span data-ttu-id="75493-215">工作階段</span><span class="sxs-lookup"><span data-stu-id="75493-215">Session</span></span> | <span data-ttu-id="75493-216">否</span><span class="sxs-lookup"><span data-stu-id="75493-216">No</span></span> | [<span data-ttu-id="75493-217">工作階段中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-217">Session Middleware</span></span>](xref:fundamentals/app-state)
<span data-ttu-id="75493-218">UrlAuthorization</span><span class="sxs-lookup"><span data-stu-id="75493-218">UrlAuthorization</span></span> | <span data-ttu-id="75493-219">否</span><span class="sxs-lookup"><span data-stu-id="75493-219">No</span></span> | 
<span data-ttu-id="75493-220">UrlMappingsModule</span><span class="sxs-lookup"><span data-stu-id="75493-220">UrlMappingsModule</span></span> | <span data-ttu-id="75493-221">否</span><span class="sxs-lookup"><span data-stu-id="75493-221">No</span></span> | [<span data-ttu-id="75493-222">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="75493-222">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
<span data-ttu-id="75493-223">UrlRoutingModule 4.0</span><span class="sxs-lookup"><span data-stu-id="75493-223">UrlRoutingModule-4.0</span></span> | <span data-ttu-id="75493-224">否</span><span class="sxs-lookup"><span data-stu-id="75493-224">No</span></span> | [<span data-ttu-id="75493-225">ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="75493-225">ASP.NET Core  Identity</span></span>](xref:security/authentication/identity)
<span data-ttu-id="75493-226">WindowsAuthentication</span><span class="sxs-lookup"><span data-stu-id="75493-226">WindowsAuthentication</span></span> | <span data-ttu-id="75493-227">否</span><span class="sxs-lookup"><span data-stu-id="75493-227">No</span></span> | 

## <a name="iis-manager-application-changes"></a><span data-ttu-id="75493-228">IIS 管理員變更應用程式</span><span class="sxs-lookup"><span data-stu-id="75493-228">IIS Manager application changes</span></span>
<span data-ttu-id="75493-229">當您使用 IIS 管理員進行設定時，要直接變更*web.config*應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="75493-229">When you use IIS Manager to configure settings, you're directly changing the *web.config* file of the app.</span></span> <span data-ttu-id="75493-230">如果您部署您的應用程式，包括*web.config*，您使用 IIS 管理員所做的變更將會覆寫的已部署*web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="75493-230">If you deploy your app and include *web.config*, any changes you made with IIS Manger will be overwritten by the deployed *web.config file*.</span></span> <span data-ttu-id="75493-231">因此如果您變更伺服器的*web.config*檔案中，複製已更新*web.config*立即本機專案的檔案。</span><span class="sxs-lookup"><span data-stu-id="75493-231">Therefore if you make changes to the server's *web.config* file, copy the updated *web.config* file to your local project immediately.</span></span>

## <a name="disabling-iis-modules"></a><span data-ttu-id="75493-232">停用 IIS 模組</span><span class="sxs-lookup"><span data-stu-id="75493-232">Disabling IIS modules</span></span>
<span data-ttu-id="75493-233">如果您有想要停用應用程式的伺服器層級設定的 IIS 模組，您可以新增至與您*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="75493-233">If you have an IIS module configured at the server level that you would like to disable for an application, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="75493-234">請讓模組留在原處和停用它使用組態設定 （如果有的話），或將模組從應用程式中移除。</span><span class="sxs-lookup"><span data-stu-id="75493-234">Either leave the module in place and deactivate it using a configuration setting (if available) or remove the module from the app.</span></span>

### <a name="module-deactivation"></a><span data-ttu-id="75493-235">模組停用</span><span class="sxs-lookup"><span data-stu-id="75493-235">Module deactivation</span></span>
<span data-ttu-id="75493-236">許多模組提供可讓您停用它們，而不需移除應用程式的組態設定。</span><span class="sxs-lookup"><span data-stu-id="75493-236">Many modules offer a configuration setting that will allow you to disable them without removing them from the application.</span></span> <span data-ttu-id="75493-237">這是用來停用模組的最簡單且最快方式。</span><span class="sxs-lookup"><span data-stu-id="75493-237">This is the simplest and quickest way to deactivate a module.</span></span> <span data-ttu-id="75493-238">例如，如果您想要停用 IIS URL Rewrite 模組，請使用`<httpRedirect>`如下所示的項目。</span><span class="sxs-lookup"><span data-stu-id="75493-238">For example if you wish to disable the IIS URL Rewrite Module, use the `<httpRedirect>` element as shown below.</span></span> <span data-ttu-id="75493-239">如需有關如何停用模組組態設定的詳細資訊，請依照下列中的連結*子項目*區段[IIS `<system.webServer>` ](https://www.iis.net/configreference/system.webserver)。</span><span class="sxs-lookup"><span data-stu-id="75493-239">For more information on disabling modules with configuration settings, follow the links in the *Child Elements* section of [IIS `<system.webServer>`](https://www.iis.net/configreference/system.webserver).</span></span>

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a><span data-ttu-id="75493-240">模組移除</span><span class="sxs-lookup"><span data-stu-id="75493-240">Module removal</span></span>
<span data-ttu-id="75493-241">如果您選擇的設定中移除模組*web.config*，您必須解除鎖定該模組，並解除鎖定`<modules>`區段*web.config*第一次。</span><span class="sxs-lookup"><span data-stu-id="75493-241">If you opt to remove a module with a setting in *web.config*, you must unlock the module and unlock the `<modules>` section of *web.config* first.</span></span> <span data-ttu-id="75493-242">步驟如下所述：</span><span class="sxs-lookup"><span data-stu-id="75493-242">The steps are outlined below:</span></span>

1. <span data-ttu-id="75493-243">解除鎖定伺服器層級的模組。</span><span class="sxs-lookup"><span data-stu-id="75493-243">Unlock the module at the server level.</span></span> <span data-ttu-id="75493-244">在 IIS 管理員中的 IIS 伺服器上按一下**連線**[資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="75493-244">Click on the IIS server in the IIS Manager **Connections** sidebar.</span></span> <span data-ttu-id="75493-245">開啟**模組**中**IIS**區域。</span><span class="sxs-lookup"><span data-stu-id="75493-245">Open the **Modules** in the **IIS** area.</span></span> <span data-ttu-id="75493-246">按一下清單中的模組。</span><span class="sxs-lookup"><span data-stu-id="75493-246">Click on the module in the list.</span></span> <span data-ttu-id="75493-247">在**動作**提要欄位的右邊按一下**Unlock**。</span><span class="sxs-lookup"><span data-stu-id="75493-247">In the **Actions** sidebar on the right, click **Unlock**.</span></span> <span data-ttu-id="75493-248">解除鎖定數量的模組，當您規劃要移除與*web.config*更新版本。</span><span class="sxs-lookup"><span data-stu-id="75493-248">Unlock as many modules as you plan to remove with *web.config* later.</span></span>

2. <span data-ttu-id="75493-249">部署應用程式而沒有`<modules>`一節中*web.config*。</span><span class="sxs-lookup"><span data-stu-id="75493-249">Deploy your application without a `<modules>` section in *web.config*.</span></span> <span data-ttu-id="75493-250">如果您部署應用程式，但*web.config*包含`<modules>`區段，而不需要解除鎖定區段第一次在 IIS 管理員中，Configuration Manager 將會擲回例外狀況，當您嘗試解除鎖定的區段。</span><span class="sxs-lookup"><span data-stu-id="75493-250">If you deploy an app with a *web.config* containing the `<modules>` section without having unlocked the section first in the IIS Manager, the Configuration Manager will throw an exception when you try to unlock the section.</span></span> <span data-ttu-id="75493-251">因此，部署應用程式而沒有`<modules>`> 一節。</span><span class="sxs-lookup"><span data-stu-id="75493-251">Therefore, deploy your application without a `<modules>` section.</span></span>

3. <span data-ttu-id="75493-252">解除鎖定`<modules>`區段*web.config*。</span><span class="sxs-lookup"><span data-stu-id="75493-252">Unlock the `<modules>` section of *web.config*.</span></span> <span data-ttu-id="75493-253">在**連線**提要欄位中，按一下中的網站**網站**。</span><span class="sxs-lookup"><span data-stu-id="75493-253">In the **Connections** sidebar, click the website in **Sites**.</span></span> <span data-ttu-id="75493-254">在**管理**區域中，開啟**組態編輯器**。</span><span class="sxs-lookup"><span data-stu-id="75493-254">In the **Management** area, open the **Configuration Editor**.</span></span> <span data-ttu-id="75493-255">使用瀏覽控制項，來選取`system.webServer/modules`> 一節。</span><span class="sxs-lookup"><span data-stu-id="75493-255">Use the navigation controls to select the `system.webServer/modules` section.</span></span> <span data-ttu-id="75493-256">在**動作**[資訊看板]，在右側，按一下以**Unlock** > 一節。</span><span class="sxs-lookup"><span data-stu-id="75493-256">In the **Actions** sidebar on the right, click to **Unlock** the section.</span></span>

4. <span data-ttu-id="75493-257">此時，您可以在此處新增`<modules>`區段您*web.config*檔案搭配`<remove>`模組從應用程式中移除的項目。</span><span class="sxs-lookup"><span data-stu-id="75493-257">At this point, you will be able to add a `<modules>` section to your *web.config* file with a `<remove>` element to remove the module from the application.</span></span> <span data-ttu-id="75493-258">您可以加入多個`<remove>`移除多個模組的項目。</span><span class="sxs-lookup"><span data-stu-id="75493-258">You can add multiple `<remove>` elements to remove multiple modules.</span></span> <span data-ttu-id="75493-259">請記住，如果您進行*web.config*以立即進行中的專案在本機伺服器上的變更。</span><span class="sxs-lookup"><span data-stu-id="75493-259">Don't forget that if you make *web.config* changes on the server to make them immediately in the project locally.</span></span> <span data-ttu-id="75493-260">這種方式移除模組不會影響您使用與其他應用程式伺服器上的模組。</span><span class="sxs-lookup"><span data-stu-id="75493-260">Removing a module this way won't affect your use of the module with other apps on the server.</span></span>

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

<span data-ttu-id="75493-261">IIS 安裝使用預設安裝的模組中，您可以使用下列`<module>`區段，以移除預設的模組。</span><span class="sxs-lookup"><span data-stu-id="75493-261">For an IIS installation with the default modules installed, you can use the following `<module>` section to remove the default modules.</span></span>

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

<span data-ttu-id="75493-262">您也可以移除的 IIS 模組*Appcmd.exe*。</span><span class="sxs-lookup"><span data-stu-id="75493-262">You can also remove an IIS module with *Appcmd.exe*.</span></span> <span data-ttu-id="75493-263">提供`MODULE_NAME`和`APPLICATION_NAME`命令如下所示：</span><span class="sxs-lookup"><span data-stu-id="75493-263">Provide the `MODULE_NAME` and `APPLICATION_NAME` in the command shown below:</span></span>

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

<span data-ttu-id="75493-264">以下是如何移除`DynamicCompressionModule`從預設的網站：</span><span class="sxs-lookup"><span data-stu-id="75493-264">Here's how to remove the `DynamicCompressionModule` from the Default Web Site:</span></span>

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a><span data-ttu-id="75493-265">最小模組設定</span><span class="sxs-lookup"><span data-stu-id="75493-265">Minimal module configuration</span></span>
<span data-ttu-id="75493-266">只有執行 ASP.NET Core 應用程式所需的模組是匿名驗證模組和 ASP.NET 核心模組。</span><span class="sxs-lookup"><span data-stu-id="75493-266">The only modules required to run an ASP.NET Core application are the Anonymous Authentication Module and the ASP.NET Core Module.</span></span>

![IIS 管理員 中開啟模組顯示的最小模組設定](iis-modules/_static/modules.png)

## <a name="resources"></a><span data-ttu-id="75493-268">資源</span><span class="sxs-lookup"><span data-stu-id="75493-268">Resources</span></span>
* [<span data-ttu-id="75493-269">發行至 IIS</span><span class="sxs-lookup"><span data-stu-id="75493-269">Publishing to IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="75493-270">IIS 模組概觀</span><span class="sxs-lookup"><span data-stu-id="75493-270">IIS Modules Overview</span></span>](https://www.iis.net/learn/get-started/introduction-to-iis/iis-modules-overview)
* [<span data-ttu-id="75493-271">自訂的 IIS 7.0 角色和模組</span><span class="sxs-lookup"><span data-stu-id="75493-271">Customizing IIS 7.0 Roles and Modules</span></span>](https://technet.microsoft.com/library/cc627313.aspx)
* [<span data-ttu-id="75493-272">IIS`<system.webServer>`</span><span class="sxs-lookup"><span data-stu-id="75493-272">IIS `<system.webServer>`</span></span>](https://www.iis.net/configreference/system.webserver)
