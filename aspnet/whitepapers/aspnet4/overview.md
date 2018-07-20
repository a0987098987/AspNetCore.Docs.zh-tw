---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 和 Visual Studio 2010 網頁程式開發概觀 |Microsoft Docs
author: rick-anderson
description: 本文件會包含在.net Framework 4 和 Visual Studio 2010 中的 asp.net 提供許多新功能的概觀。
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 5c7aa95b18bc0a97f42cc981476c110830286fa5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829039"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a><span data-ttu-id="04082-103">ASP.NET 4 和 Visual Studio 2010 網頁程式開發概觀</span><span class="sxs-lookup"><span data-stu-id="04082-103">ASP.NET 4 and Visual Studio 2010 Web Development Overview</span></span>
====================
> <span data-ttu-id="04082-104">本文件會包含在.net Framework 4 和 Visual Studio 2010 中的 asp.net 提供許多新功能的概觀。</span><span class="sxs-lookup"><span data-stu-id="04082-104">This document provides an overview of many of the new features for ASP.NET that are included in the.NET Framework 4 and in Visual Studio 2010.</span></span>
> 
> [<span data-ttu-id="04082-105">下載此白皮書</span><span class="sxs-lookup"><span data-stu-id="04082-105">Download This Whitepaper</span></span>](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


<span data-ttu-id="04082-106">**內容**</span><span class="sxs-lookup"><span data-stu-id="04082-106">**Contents**</span></span>

<span data-ttu-id="04082-107">**[Core Services](#0.2__Toc253429238 "_Toc253429238")**</span><span class="sxs-lookup"><span data-stu-id="04082-107">**[Core Services](#0.2__Toc253429238 "_Toc253429238")**</span></span>  
[<span data-ttu-id="04082-108">Web.config 檔案重構</span><span class="sxs-lookup"><span data-stu-id="04082-108">Web.config File Refactoring</span></span>](#0.2__Toc253429239 "_Toc253429239")  
[<span data-ttu-id="04082-109">可延伸的輸出快取</span><span class="sxs-lookup"><span data-stu-id="04082-109">Extensible Output Caching</span></span>](#0.2__Toc253429240 "_Toc253429240")  
[<span data-ttu-id="04082-110">自動啟動 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="04082-110">Auto-Start Web Applications</span></span>](#0.2__Toc253429241 "_Toc253429241")  
[<span data-ttu-id="04082-111">永久重新導向頁面</span><span class="sxs-lookup"><span data-stu-id="04082-111">Permanently Redirecting a Page</span></span>](#0.2__Toc253429242 "_Toc253429242")  
[<span data-ttu-id="04082-112">壓縮工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="04082-112">Shrinking Session State</span></span>](#0.2__Toc253429243 "_Toc253429243")  
[<span data-ttu-id="04082-113">展開的可允許的 Url 範圍</span><span class="sxs-lookup"><span data-stu-id="04082-113">Expanding the Range of Allowable URLs</span></span>](#0.2__Toc253429244 "_Toc253429244")  
[<span data-ttu-id="04082-114">可延伸的要求驗證</span><span class="sxs-lookup"><span data-stu-id="04082-114">Extensible Request Validation</span></span>](#0.2__Toc253429245 "_Toc253429245")  
[<span data-ttu-id="04082-115">物件快取物件和快取的擴充性</span><span class="sxs-lookup"><span data-stu-id="04082-115">Object Caching and Object Caching Extensibility</span></span>](#0.2__Toc253429246 "_Toc253429246")  
[<span data-ttu-id="04082-116">可延伸的 HTML、 URL 和 HTTP 標頭的編碼方式</span><span class="sxs-lookup"><span data-stu-id="04082-116">Extensible HTML, URL, and HTTP Header Encoding</span></span>](#0.2__Toc253429247 "_Toc253429247")  
[<span data-ttu-id="04082-117">在單一背景工作處理序中的個別應用程式的效能監視</span><span class="sxs-lookup"><span data-stu-id="04082-117">Performance Monitoring for Individual Applications in a Single Worker Process</span></span>](#0.2__Toc253429248 "_Toc253429248")  
[<span data-ttu-id="04082-118">Multi-Targeting</span><span class="sxs-lookup"><span data-stu-id="04082-118">Multi-Targeting</span></span>](#0.2__Toc253429249 "_Toc253429249")

<span data-ttu-id="04082-119">**[Ajax](#0.2__Toc253429250 "_Toc253429250")**</span><span class="sxs-lookup"><span data-stu-id="04082-119">**[Ajax](#0.2__Toc253429250 "_Toc253429250")**</span></span>  
[<span data-ttu-id="04082-120">包括在 Web Form 和 MVC 的 jQuery</span><span class="sxs-lookup"><span data-stu-id="04082-120">jQuery Included with Web Forms and MVC</span></span>](#0.2__Toc253429251 "_Toc253429251")  
[<span data-ttu-id="04082-121">內容傳遞網路支援</span><span class="sxs-lookup"><span data-stu-id="04082-121">Content Delivery Network Support</span></span>](#0.2__Toc253429252 "_Toc253429252")  
[<span data-ttu-id="04082-122">ScriptManager 明確指令碼</span><span class="sxs-lookup"><span data-stu-id="04082-122">ScriptManager Explicit Scripts</span></span>](#0.2__Toc253429253 "_Toc253429253")

<span data-ttu-id="04082-123">**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**</span><span class="sxs-lookup"><span data-stu-id="04082-123">**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**</span></span>  
[<span data-ttu-id="04082-124">設定使用 Page.MetaKeywords 和 Page.MetaDescription 屬性的中繼標籤</span><span class="sxs-lookup"><span data-stu-id="04082-124">Setting Meta Tags with the Page.MetaKeywords and Page.MetaDescription Properties</span></span>](#0.2__Toc253429257 "_Toc253429257")  
[<span data-ttu-id="04082-125">啟用個別的控制項檢視狀態</span><span class="sxs-lookup"><span data-stu-id="04082-125">Enabling View State for Individual Controls</span></span>](#0.2__Toc253429258 "_Toc253429258")  
[<span data-ttu-id="04082-126">變更瀏覽器能力</span><span class="sxs-lookup"><span data-stu-id="04082-126">Changes to Browser Capabilities</span></span>](#0.2__Toc253429259 "_Toc253429259")  
[<span data-ttu-id="04082-127">ASP.NET 4 中的路由</span><span class="sxs-lookup"><span data-stu-id="04082-127">Routing in ASP.NET 4</span></span>](#0.2__Toc253429260 "_Toc253429260")  
[<span data-ttu-id="04082-128">設定用戶端識別碼</span><span class="sxs-lookup"><span data-stu-id="04082-128">Setting Client IDs</span></span>](#0.2__Toc253429261 "_Toc253429261")  
[<span data-ttu-id="04082-129">資料控制項中選取的保存資料列</span><span class="sxs-lookup"><span data-stu-id="04082-129">Persisting Row Selection in Data Controls</span></span>](#0.2__Toc253429262 "_Toc253429262")  
[<span data-ttu-id="04082-130">ASP.NET 的 Chart 控制項</span><span class="sxs-lookup"><span data-stu-id="04082-130">ASP.NET Chart Control</span></span>](#0.2__Toc253429263 "_Toc253429263")  
[<span data-ttu-id="04082-131">篩選與 QueryExtender 控制項的資料</span><span class="sxs-lookup"><span data-stu-id="04082-131">Filtering Data with the QueryExtender Control</span></span>](#0.2__Toc253429264 "_Toc253429264")  
[<span data-ttu-id="04082-132">Html 編碼的程式碼運算式</span><span class="sxs-lookup"><span data-stu-id="04082-132">Html Encoded Code Expressions</span></span>](#0.2__Toc253429265 "_Toc253429265")  
[<span data-ttu-id="04082-133">專案範本變更</span><span class="sxs-lookup"><span data-stu-id="04082-133">Project Template Changes</span></span>](#0.2__Toc253429266 "_Toc253429266")  
[<span data-ttu-id="04082-134">CSS 改良</span><span class="sxs-lookup"><span data-stu-id="04082-134">CSS Improvements</span></span>](#0.2__Toc253429267 "_Toc253429267")  
[<span data-ttu-id="04082-135">隱藏 div 周圍隱藏欄位的項目</span><span class="sxs-lookup"><span data-stu-id="04082-135">Hiding div Elements Around Hidden Fields</span></span>](#0.2__Toc253429268 "_Toc253429268")  
[<span data-ttu-id="04082-136">樣板化控制項呈現的外部資料表</span><span class="sxs-lookup"><span data-stu-id="04082-136">Rendering an Outer Table for Templated Controls</span></span>](#0.2__Toc253429269 "_Toc253429269")  
[<span data-ttu-id="04082-137">ListView 控制項的增強功能</span><span class="sxs-lookup"><span data-stu-id="04082-137">ListView Control Enhancements</span></span>](#0.2__Toc253429270 "_Toc253429270")  
[<span data-ttu-id="04082-138">CheckBoxList 和 RadioButtonList 控制項的增強功能</span><span class="sxs-lookup"><span data-stu-id="04082-138">CheckBoxList and RadioButtonList Control Enhancements</span></span>](#0.2__Toc253429271 "_Toc253429271")  
[<span data-ttu-id="04082-139">功能表控制項改良</span><span class="sxs-lookup"><span data-stu-id="04082-139">Menu Control Improvements</span></span>](#0.2__Toc253429272 "_Toc253429272")  
[<span data-ttu-id="04082-140">精靈和 CreateUserWizard 控制項 56</span><span class="sxs-lookup"><span data-stu-id="04082-140">Wizard and CreateUserWizard Controls 56</span></span>](#0.2__Toc253429273 "_Toc253429273")

<span data-ttu-id="04082-141">**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**</span><span class="sxs-lookup"><span data-stu-id="04082-141">**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**</span></span>  
[<span data-ttu-id="04082-142">區域支援</span><span class="sxs-lookup"><span data-stu-id="04082-142">Areas Support</span></span>](#0.2__Toc253429275 "_Toc253429275")  
[<span data-ttu-id="04082-143">資料註解屬性驗證支援</span><span class="sxs-lookup"><span data-stu-id="04082-143">Data-Annotation Attribute Validation Support</span></span>](#0.2__Toc253429276 "_Toc253429276")  
[<span data-ttu-id="04082-144">樣板化 Helper</span><span class="sxs-lookup"><span data-stu-id="04082-144">Templated Helpers</span></span>](#0.2__Toc253429277 "_Toc253429277")

<span data-ttu-id="04082-145">**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**</span><span class="sxs-lookup"><span data-stu-id="04082-145">**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**</span></span>  
[<span data-ttu-id="04082-146">對於現有的專案中啟用動態資料</span><span class="sxs-lookup"><span data-stu-id="04082-146">Enabling Dynamic Data for Existing Projects</span></span>](#0.2__Toc253429279 "_Toc253429279")  
[<span data-ttu-id="04082-147">宣告式 DynamicDataManager 控制項語法</span><span class="sxs-lookup"><span data-stu-id="04082-147">Declarative DynamicDataManager Control Syntax</span></span>](#0.2__Toc253429280 "_Toc253429280")  
[<span data-ttu-id="04082-148">實體範本</span><span class="sxs-lookup"><span data-stu-id="04082-148">Entity Templates</span></span>](#0.2__Toc253429281 "_Toc253429281")  
[<span data-ttu-id="04082-149">新的欄位範本 Url 和電子郵件地址</span><span class="sxs-lookup"><span data-stu-id="04082-149">New Field Templates for URLs and E-mail Addresses</span></span>](#0.2__Toc253429282 "_Toc253429282")  
[<span data-ttu-id="04082-150">建立與 DynamicHyperLink 控制項的連結</span><span class="sxs-lookup"><span data-stu-id="04082-150">Creating Links with the DynamicHyperLink Control</span></span>](#0.2__Toc253429283 "_Toc253429283")  
[<span data-ttu-id="04082-151">支援的資料模型中的繼承</span><span class="sxs-lookup"><span data-stu-id="04082-151">Support for Inheritance in the Data Model</span></span>](#0.2__Toc253429284 "_Toc253429284")  
[<span data-ttu-id="04082-152">支援多對多關聯性 (Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="04082-152">Support for Many-to-Many Relationships (Entity Framework Only)</span></span>](#0.2__Toc253429285 "_Toc253429285")  
[<span data-ttu-id="04082-153">新的屬性，以控制顯示，並支援列舉</span><span class="sxs-lookup"><span data-stu-id="04082-153">New Attributes to Control Display and Support Enumerations</span></span>](#0.2__Toc253429286 "_Toc253429286")  
[<span data-ttu-id="04082-154">增強的篩選器支援</span><span class="sxs-lookup"><span data-stu-id="04082-154">Enhanced Support for Filters</span></span>](#0.2__Toc253429287 "_Toc253429287")

<span data-ttu-id="04082-155">**[Visual Studio 2010 Web 開發改善](#0.2__Toc253429288 "_Toc253429288")**</span><span class="sxs-lookup"><span data-stu-id="04082-155">**[Visual Studio 2010 Web Development Improvements](#0.2__Toc253429288 "_Toc253429288")**</span></span>  
[<span data-ttu-id="04082-156">改良 CSS 相容性</span><span class="sxs-lookup"><span data-stu-id="04082-156">Improved CSS Compatibility</span></span>](#0.2__Toc253429289 "_Toc253429289")  
[<span data-ttu-id="04082-157">HTML 和 JavaScript 程式碼片段</span><span class="sxs-lookup"><span data-stu-id="04082-157">HTML and JavaScript Snippets</span></span>](#0.2__Toc253429290 "_Toc253429290")  
[<span data-ttu-id="04082-158">JavaScript IntelliSense 的增強功能</span><span class="sxs-lookup"><span data-stu-id="04082-158">JavaScript IntelliSense Enhancements</span></span>](#0.2__Toc253429291 "_Toc253429291")

<span data-ttu-id="04082-159">**[Web 與 Visual Studio 2010 的應用程式部署](#0.2__Toc253429292 "_Toc253429292")**</span><span class="sxs-lookup"><span data-stu-id="04082-159">**[Web Application Deployment with Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**</span></span>  
[<span data-ttu-id="04082-160">Web 封裝</span><span class="sxs-lookup"><span data-stu-id="04082-160">Web Packaging</span></span>](#0.2__Toc253429293 "_Toc253429293")  
[<span data-ttu-id="04082-161">Web.config Transformation</span><span class="sxs-lookup"><span data-stu-id="04082-161">Web.config Transformation</span></span>](#0.2__Toc253429294 "_Toc253429294")  
[<span data-ttu-id="04082-162">資料庫部署</span><span class="sxs-lookup"><span data-stu-id="04082-162">Database Deployment</span></span>](#0.2__Toc253429295 "_Toc253429295")  
[<span data-ttu-id="04082-163">單鍵發行為 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="04082-163">One-Click Publish for Web Applications</span></span>](#0.2__Toc253429296 "_Toc253429296")  
[<span data-ttu-id="04082-164">Resources</span><span class="sxs-lookup"><span data-stu-id="04082-164">Resources</span></span>](#0.2__Toc253429297 "_Toc253429297")

<span data-ttu-id="04082-165">**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**</span><span class="sxs-lookup"><span data-stu-id="04082-165">**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**</span></span>

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a><span data-ttu-id="04082-166">核心服務</span><span class="sxs-lookup"><span data-stu-id="04082-166">Core Services</span></span>

<span data-ttu-id="04082-167">ASP.NET 4 導入了一些改善核心 ASP.NET 服務，例如輸出快取和工作階段狀態儲存體的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-167">ASP.NET 4 introduces a number of features that improve core ASP.NET services such as output caching and session-state storage.</span></span>

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a><span data-ttu-id="04082-168">重構的 Web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="04082-168">Web.config File Refactoring</span></span>

<span data-ttu-id="04082-169">`Web.config`檔案，其中包含組態，如已加入新的功能，例如 Ajax，移轉過去的幾個版本的.NET framework 的 Web 應用程式已經大幅變得的路由，以及與 IIS 7 整合。</span><span class="sxs-lookup"><span data-stu-id="04082-169">The `Web.config` file that contains the configuration for a Web application has grown considerably over the past few releases of the .NET Framework as new features have been added, such as Ajax, routing, and integration with IIS 7.</span></span> <span data-ttu-id="04082-170">這使得它難設定，或啟動新的 Web 應用程式，而不需要 Visual Studio 之類的工具。</span><span class="sxs-lookup"><span data-stu-id="04082-170">This has made it harder to configure or start new Web applications without a tool like Visual Studio.</span></span> <span data-ttu-id="04082-171">在蒐羅 NET Framework 4 中，主要的組態項目都已移至`machine.config`檔案和應用程式現在會繼承這些設定。</span><span class="sxs-lookup"><span data-stu-id="04082-171">In .the NET Framework 4, the major configuration elements have been moved to the `machine.config` file, and applications now inherit these settings.</span></span> <span data-ttu-id="04082-172">這可讓`Web.config`為空白或包含只是下列幾行，後者會指定適用於 Visual Studio 應用程式的目標 framework 版本的 ASP.NET 4 應用程式中的檔案：</span><span class="sxs-lookup"><span data-stu-id="04082-172">This allows the `Web.config` file in ASP.NET 4 applications either to be empty or to contain just the following lines, which specify for Visual Studio what version of the framework the application is targeting:</span></span>

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a><span data-ttu-id="04082-173">可延伸的輸出快取</span><span class="sxs-lookup"><span data-stu-id="04082-173">Extensible Output Caching</span></span>

<span data-ttu-id="04082-174">自 ASP.NET 1.0 發行時，輸出快取已啟用開發人員在記憶體中儲存的頁面、 控制項和 HTTP 回應所產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="04082-174">Since the time that ASP.NET 1.0 was released, output caching has enabled developers to store the generated output of pages, controls, and HTTP responses in memory.</span></span> <span data-ttu-id="04082-175">在後續 Web 要求，ASP.NET 可以提供內容更快速地從記憶體，而不重新產生的輸出，從零開始擷取產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="04082-175">On subsequent Web requests, ASP.NET can serve content more quickly by retrieving the generated output from memory instead of regenerating the output from scratch.</span></span> <span data-ttu-id="04082-176">不過，此方法有一項限制，產生的內容一律儲存在記憶體中，而且發生高流量的伺服器，在輸出快取所耗用的記憶體可以從 Web 應用程式的其他部分的記憶體需求與競爭。</span><span class="sxs-lookup"><span data-stu-id="04082-176">However, this approach has a limitation — generated content always has to be stored in memory, and on servers that are experiencing heavy traffic, the memory consumed by output caching can compete with memory demands from other portions of a Web application.</span></span>

<span data-ttu-id="04082-177">ASP.NET 4 中加入的擴充點的輸出快取，可讓您設定一或多個自訂輸出快取提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-177">ASP.NET 4 adds an extensibility point to output caching that enables you to configure one or more custom output-cache providers.</span></span> <span data-ttu-id="04082-178">輸出快取提供者可以使用任何儲存機制，來保存的 HTML 內容。</span><span class="sxs-lookup"><span data-stu-id="04082-178">Output-cache providers can use any storage mechanism to persist HTML content.</span></span> <span data-ttu-id="04082-179">這讓您能夠建立自訂輸出快取提供者，針對不同的持續性機制，包括本機或遠端磁碟，雲端儲存體，以及分散式快取引擎。</span><span class="sxs-lookup"><span data-stu-id="04082-179">This makes it possible to create custom output-cache providers for diverse persistence mechanisms, which can include local or remote disks, cloud storage, and distributed cache engines.</span></span>

<span data-ttu-id="04082-180">您可以建立自訂輸出快取提供者做為衍生自新的類別*System.Web.Caching.OutputCacheProvider*型別。</span><span class="sxs-lookup"><span data-stu-id="04082-180">You create a custom output-cache provider as a class that derives from the new *System.Web.Caching.OutputCacheProvider* type.</span></span> <span data-ttu-id="04082-181">接著，您可以設定中的提供者`Web.config`使用新的檔案*提供者*子區段*outputCache*項目，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-181">You can then configure the provider in the `Web.config` file by using the new *providers* subsection of the *outputCache* element, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample2.xml)]

<span data-ttu-id="04082-182">根據預設，ASP.NET 4 中，所有的 HTTP 回應，在轉譯頁面和控制項在前一個範例中，所示，使用記憶體中輸出快取中，其中*defaultProvider*屬性設為 AspNetInternalProvider。</span><span class="sxs-lookup"><span data-stu-id="04082-182">By default in ASP.NET 4, all HTTP responses, rendered pages, and controls use the in-memory output cache, as shown in the previous example, where the *defaultProvider* attribute is set to AspNetInternalProvider.</span></span> <span data-ttu-id="04082-183">您可以變更用於 Web 應用程式，藉由指定不同的提供者名稱的預設輸出快取提供者*defaultProvider*。</span><span class="sxs-lookup"><span data-stu-id="04082-183">You can change the default output-cache provider used for a Web application by specifying a different provider name for *defaultProvider*.</span></span>

<span data-ttu-id="04082-184">此外，您可以選取每個控制項，且每個要求不同的輸出快取提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-184">In addition, you can select different output-cache providers per control and per request.</span></span> <span data-ttu-id="04082-185">選擇不同的輸出快取提供者，針對不同的 Web 使用者控制項的最簡單方式是使用新因此以宣告方式執行*providerName*屬性中的控制項指示詞，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-185">The easiest way to choose a different output-cache provider for different Web user controls is to do so declaratively by using the new *providerName* attribute in a control directive, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample3.aspx)]

<span data-ttu-id="04082-186">指定不同的輸出快取提供者針對 HTTP 要求需要多一點的工作。</span><span class="sxs-lookup"><span data-stu-id="04082-186">Specifying a different output cache provider for an HTTP request requires a little more work.</span></span> <span data-ttu-id="04082-187">而不是以宣告方式指定提供者，您會覆寫新*GetOuputCacheProviderName*方法中的`Global.asax`以程式設計方式指定要用於特定的要求哪一個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-187">Instead of declaratively specifying the provider, you override the new *GetOuputCacheProviderName* method in the `Global.asax` file to programmatically specify which provider to use for a specific request.</span></span> <span data-ttu-id="04082-188">下列範例顯示如何執行這項工作。</span><span class="sxs-lookup"><span data-stu-id="04082-188">The following example shows how to do this.</span></span>

[!code-csharp[Main](overview/samples/sample4.cs)]

<span data-ttu-id="04082-189">加入至 ASP.NET 4 的輸出快取提供者擴充性之後中,，您現在可以針對您的網站，以嘗試更積極與更聰明的輸出快取策略。</span><span class="sxs-lookup"><span data-stu-id="04082-189">With the addition of output-cache provider extensibility to ASP.NET 4, you can now pursue more aggressive and more intelligent output-caching strategies for your Web sites.</span></span> <span data-ttu-id="04082-190">比方說，現在正是可以快取的 「 前 10 個 」 頁面的網站，以在記憶體中，在快取取得較低的流量，在磁碟的頁面時。</span><span class="sxs-lookup"><span data-stu-id="04082-190">For example, it is now possible to cache the "Top 10" pages of a site in memory, while caching pages that get lower traffic on disk.</span></span> <span data-ttu-id="04082-191">或者，您可以快取呈現的頁面上，每個不同的組合，但使用分散式快取，以便卸載來自前端 Web 伺服器的記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="04082-191">Alternatively, you can cache every vary-by combination for a rendered page, but use a distributed cache so that the memory consumption is offloaded from front-end Web servers.</span></span>

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a><span data-ttu-id="04082-192">自動啟動 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="04082-192">Auto-Start Web Applications</span></span>

<span data-ttu-id="04082-193">載入大量資料，或執行昂貴的初始設定處理提供服務的第一個要求之前，需要一些 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-193">Some Web applications need to load large amounts of data or perform expensive initialization processing before serving the first request.</span></span> <span data-ttu-id="04082-194">在舊版 ASP.NET 中，這些情況下您必須想出 「 喚醒 」 的 ASP.NET 應用程式，然後再執行期間的初始化程式碼的自訂方法*應用程式\_負載*方法中的`Global.asax`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-194">In earlier versions of ASP.NET, for these situations you had to devise custom approaches to "wake up" an ASP.NET application and then run initialization code during the *Application\_Load* method in the `Global.asax` file.</span></span>

<span data-ttu-id="04082-195">新的擴充性功能，稱為*自動啟動*直接位址這種情況下可當 ASP.NET 4 執行的 Windows Server 2008 R2 上的 IIS 7.5。</span><span class="sxs-lookup"><span data-stu-id="04082-195">A new scalability feature named *auto-start* that directly addresses this scenario is available when ASP.NET 4 runs on IIS 7.5 on Windows Server 2008 R2.</span></span> <span data-ttu-id="04082-196">自動啟動功能提供啟動應用程式集區，初始化 ASP.NET 應用程式，然後接受 HTTP 要求的控制的的方法。</span><span class="sxs-lookup"><span data-stu-id="04082-196">The auto-start feature provides a controlled approach for starting up an application pool, initializing an ASP.NET application, and then accepting HTTP requests.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="04082-197">適用於 IIS 7.5 的 IIS 應用程式準備模組</span><span class="sxs-lookup"><span data-stu-id="04082-197">IIS Application Warm-Up Module for IIS 7.5</span></span>
> 
> <span data-ttu-id="04082-198">IIS 小組已發行應用程式準備模組的第一個 beta 版測試版本 for IIS 7.5。</span><span class="sxs-lookup"><span data-stu-id="04082-198">The IIS team has released the first beta test version of the Application Warm-Up Module for IIS 7.5.</span></span> <span data-ttu-id="04082-199">這可讓您更容易，比先前所述的應用程式正在準備。</span><span class="sxs-lookup"><span data-stu-id="04082-199">This makes warming up your applications even easier than previously described.</span></span> <span data-ttu-id="04082-200">而不需要撰寫自訂程式碼，您可以指定要執行的 Web 應用程式接受來自網路的要求資源的 Url。</span><span class="sxs-lookup"><span data-stu-id="04082-200">Instead of writing custom code, you specify the URLs of resources to execute before the Web application accepts requests from the network.</span></span> <span data-ttu-id="04082-201">此準備，就會發生在 IIS 服務的啟動過程 (如果您已設定為 IIS 應用程式集區*AlwaysRunning*) 和 IIS 工作者處理序回收時。</span><span class="sxs-lookup"><span data-stu-id="04082-201">This warm-up occurs during startup of the IIS service (if you configured the IIS application pool as *AlwaysRunning*) and when an IIS worker process recycles.</span></span> <span data-ttu-id="04082-202">期間回收，舊的 IIS 工作者處理序會繼續執行要求，直到新產生的背景工作處理序完全準備就緒，以便應用程式體驗不中斷或其他問題，因為 unprimed 快取。</span><span class="sxs-lookup"><span data-stu-id="04082-202">During recycle, the old IIS worker process continues to execute requests until the newly spawned worker process is fully warmed up, so that applications experience no interruptions or other issues due to unprimed caches.</span></span> <span data-ttu-id="04082-203">請注意，此模組的運作方式與任何版本的 ASP.NET 中，從 2.0 版開始。</span><span class="sxs-lookup"><span data-stu-id="04082-203">Note that this module works with any version of ASP.NET, starting with version 2.0.</span></span>
> 
> <span data-ttu-id="04082-204">如需詳細資訊，請參閱 < [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) IIS.net 網站上。</span><span class="sxs-lookup"><span data-stu-id="04082-204">For more information, see [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) on the IIS.net Web site.</span></span> <span data-ttu-id="04082-205">如需說明如何使用 「 準備 」 功能的逐步解說，請參閱 < [Getting Started with IIS 7.5 的應用程式準備模組](https://www.iis.net/learn/manage)IIS.net 網站上。</span><span class="sxs-lookup"><span data-stu-id="04082-205">For a walkthrough that illustrates how to use the warm-up feature, see [Getting Started with the IIS 7.5 Application Warm-Up Module](https://www.iis.net/learn/manage) on the IIS.net Web site.</span></span>


<span data-ttu-id="04082-206">若要使用自動啟動功能，IIS 管理員，請設定為自動啟動使用中的下列設定的 IIS 7.5 中的應用程式集區`applicationHost.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="04082-206">To use the auto-start feature, an IIS administrator sets an application pool in IIS 7.5 to be automatically started by using the following configuration in the `applicationHost.config` file:</span></span>

[!code-xml[Main](overview/samples/sample5.xml)]

<span data-ttu-id="04082-207">因為單一應用程式集區可以包含多個應用程式，您會指定個別的應用程式使用中的下列設定為自動啟動`applicationHost.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="04082-207">Because a single application pool can contain multiple applications, you specify individual applications to be automatically started by using the following configuration in the `applicationHost.config` file:</span></span>

[!code-xml[Main](overview/samples/sample6.xml)]

<span data-ttu-id="04082-208">IIS 7.5 的 IIS 7.5 伺服器會是冷啟動時，或個別的應用程式集區回收時，使用中的資訊`applicationHost.config`檔案，以判斷哪一個 Web 應用程式必須自動啟動。</span><span class="sxs-lookup"><span data-stu-id="04082-208">When an IIS 7.5 server is cold-started or when an individual application pool is recycled, IIS 7.5 uses the information in the `applicationHost.config` file to determine which Web applications need to be automatically started.</span></span> <span data-ttu-id="04082-209">每個應用程式標示為自動啟動，IIS7.5 傳送要求至 ASP.NET 4，以啟動應用程式狀態，此時應用程式暫時不接受 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="04082-209">For each application that is marked for auto-start, IIS7.5 sends a request to ASP.NET 4 to start the application in a state during which the application temporarily does not accept HTTP requests.</span></span> <span data-ttu-id="04082-210">處於此狀態時，ASP.NET 會具現化所定義的型別*serviceAutoStartProvider*屬性 （如上例所示），然後呼叫到它的公用進入點。</span><span class="sxs-lookup"><span data-stu-id="04082-210">When it is in this state, ASP.NET instantiates the type defined by the *serviceAutoStartProvider* attribute (as shown in the previous example) and calls into its public entry point.</span></span>

<span data-ttu-id="04082-211">使用必要的項目點建立受管理的自動啟動類型藉由實作*IProcessHostPreloadClient*介面，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-211">You create a managed auto-start type with the necessary entry point by implementing the *IProcessHostPreloadClient* interface, as shown in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample7.cs)]

<span data-ttu-id="04082-212">程式碼會在您初始化之後*預先載入*方法和方法傳回時，ASP.NET 應用程式已準備好處理要求。</span><span class="sxs-lookup"><span data-stu-id="04082-212">After your initialization code runs in the *Preload* method and the method returns, the ASP.NET application is ready to process requests.</span></span>

<span data-ttu-id="04082-213">搭配 IIS.5 和 ASP.NET 4 自動啟動，您現在已有妥善定義的方法執行耗費資源的應用程式初始化之前處理第一個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="04082-213">With the addition of auto-start to IIS .5 and ASP.NET 4, you now have a well-defined approach for performing expensive application initialization prior to processing the first HTTP request.</span></span> <span data-ttu-id="04082-214">例如，您可以使用新的自動啟動功能，來初始化應用程式，並接著通知負載平衡器的應用程式已初始化並準備好接受 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="04082-214">For example, you can use the new auto-start feature to initialize an application and then signal a load-balancer that the application was initialized and ready to accept HTTP traffic.</span></span>

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a><span data-ttu-id="04082-215">永久重新導向頁面</span><span class="sxs-lookup"><span data-stu-id="04082-215">Permanently Redirecting a Page</span></span>

<span data-ttu-id="04082-216">它是常見的做法，在經過一段時間，可能會導致累積的過時的連結，在搜尋引擎中移動頁面和其他內容周圍的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-216">It is common practice in Web applications to move pages and other content around over time, which can lead to an accumulation of stale links in search engines.</span></span> <span data-ttu-id="04082-217">在 ASP.NET 中，開發人員有傳統上由舊 Url 的要求，使用來*Response.Redirect*方法，以將要求轉送到新的 URL。</span><span class="sxs-lookup"><span data-stu-id="04082-217">In ASP.NET, developers have traditionally handled requests to old URLs by using by using the *Response.Redirect* method to forward a request to the new URL.</span></span> <span data-ttu-id="04082-218">不過，*重新導向*方法發出 HTTP 302 已找到 （暫時重新導向） 回應，導致額外 HTTP 來回行程當使用者嘗試存取舊的 Url。</span><span class="sxs-lookup"><span data-stu-id="04082-218">However, the *Redirect* method issues an HTTP 302 Found (temporary redirect) response, which results in an extra HTTP round trip when users attempt to access the old URLs.</span></span>

<span data-ttu-id="04082-219">加入新的 ASP.NET 4 *RedirectPermanent* helper 方法，可讓您輕鬆地發出 HTTP 301 已永久移動回應，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-219">ASP.NET 4 adds a new *RedirectPermanent* helper method that makes it easy to issue HTTP 301 Moved Permanently responses, as in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample8.cs)]

<span data-ttu-id="04082-220">搜尋引擎和辨識永久重新導向其他使用者代理程式會將儲存新的內容，這樣會消除不必要的來回行程由暫時重新導向的瀏覽器相關聯的 URL。</span><span class="sxs-lookup"><span data-stu-id="04082-220">Search engines and other user agents that recognize permanent redirects will store the new URL that is associated with the content, which eliminates the unnecessary round trip made by the browser for temporary redirects.</span></span>

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a><span data-ttu-id="04082-221">壓縮工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="04082-221">Shrinking Session State</span></span>

<span data-ttu-id="04082-222">ASP.NET 提供兩個工作階段狀態儲存在 Web 伺服陣列的預設選項： 叫用跨處理序工作階段狀態伺服器中，工作階段狀態提供者和工作階段狀態提供者在 Microsoft SQL Server 資料庫中儲存資料。</span><span class="sxs-lookup"><span data-stu-id="04082-222">ASP.NET provides two default options for storing session state across a Web farm: a session-state provider that invokes an out-of-process session-state server, and a session-state provider that stores data in a Microsoft SQL Server database.</span></span> <span data-ttu-id="04082-223">由於這兩個選項需要將儲存 Web 應用程式的背景工作處理序以外的狀態資訊，工作階段狀態，就必須傳送到遠端存放裝置之前，序列化。</span><span class="sxs-lookup"><span data-stu-id="04082-223">Because both options involve storing state information outside a Web application's worker process, session state has to be serialized before it is sent to remote storage.</span></span> <span data-ttu-id="04082-224">根據多少開發人員會將儲存在工作階段狀態的資訊，序列化的資料可以成長的大小相當大。</span><span class="sxs-lookup"><span data-stu-id="04082-224">Depending on how much information a developer saves in session state, the size of the serialized data can grow quite large.</span></span>

<span data-ttu-id="04082-225">ASP.NET 4 引進新的壓縮選項，讓這兩種跨處理序工作階段狀態提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-225">ASP.NET 4 introduces a new compression option for both kinds of out-of-process session-state providers.</span></span> <span data-ttu-id="04082-226">當*compressionEnabled*下列範例所示的組態選項設定為 *，則為 true*，ASP.NET 會壓縮 （和解壓縮） 使用.NET Framework序列化工作階段狀態*System.io.compression.gzipstream 就*類別。</span><span class="sxs-lookup"><span data-stu-id="04082-226">When the *compressionEnabled* configuration option shown in the following example is set to *true*, ASP.NET will compress (and decompress) serialized session state by using the .NET Framework *System.IO.Compression.GZipStream* class.</span></span>

[!code-xml[Main](overview/samples/sample9.xml)]

<span data-ttu-id="04082-227">加上以新屬性的簡單`Web.config`檔案中，使用多餘的 CPU 週期，在 Web 伺服器上的應用程式可以實現大幅縮減大小序列化工作階段狀態資料。</span><span class="sxs-lookup"><span data-stu-id="04082-227">With the simple addition of the new attribute to the `Web.config` file, applications with spare CPU cycles on Web servers can realize substantial reductions in the size of serialized session-state data.</span></span>

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a><span data-ttu-id="04082-228">展開可允許的 Url 的範圍</span><span class="sxs-lookup"><span data-stu-id="04082-228">Expanding the Range of Allowable URLs</span></span>

<span data-ttu-id="04082-229">ASP.NET 4 引進了新的選項，擴充應用程式 Url 的大小。</span><span class="sxs-lookup"><span data-stu-id="04082-229">ASP.NET 4 introduces new options for expanding the size of application URLs.</span></span> <span data-ttu-id="04082-230">舊版 ASP.NET 的受限 URL 路徑的長度為 260 個字元，根據 NTFS 檔案路徑限制。</span><span class="sxs-lookup"><span data-stu-id="04082-230">Previous versions of ASP.NET constrained URL path lengths to 260 characters, based on the NTFS file-path limit.</span></span> <span data-ttu-id="04082-231">在 ASP.NET 4 中，您可以選擇以增加 （或減少） 為適用於您的應用程式，此限制使用兩個新*httpRuntime*組態屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-231">In ASP.NET 4, you have the option to increase (or decrease) this limit as appropriate for your applications, using two new *httpRuntime* configuration attributes.</span></span> <span data-ttu-id="04082-232">下列範例會示範這些新屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-232">The following example shows these new attributes.</span></span>

[!code-xml[Main](overview/samples/sample10.xml)]

<span data-ttu-id="04082-233">若要允許長或短的路徑 （不包含通訊協定、 伺服器名稱，以及查詢字串之 url 的部分），修改*[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* 屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-233">To allow longer or shorter paths (the portion of the URL that does not include protocol, server name, and query string), modify the *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* attribute.</span></span> <span data-ttu-id="04082-234">若要允許長或短的查詢字串，修改的值*[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* 屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-234">To allow longer or shorter query strings, modify the value of the *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* attribute.</span></span>

<span data-ttu-id="04082-235">ASP.NET 4 也可讓您設定的 URL 字元檢查所使用的字元。</span><span class="sxs-lookup"><span data-stu-id="04082-235">ASP.NET 4 also enables you to configure the characters that are used by the URL character check.</span></span> <span data-ttu-id="04082-236">當 ASP.NET 在 URL 的路徑部分中找到無效的字元時，它會拒絕要求，並發出 HTTP 400 錯誤。</span><span class="sxs-lookup"><span data-stu-id="04082-236">When ASP.NET finds an invalid character in the path portion of a URL, it rejects the request and issues an HTTP 400 error.</span></span> <span data-ttu-id="04082-237">在舊版 ASP.NET 中，URL 字元檢查已限制為一組固定的字元。</span><span class="sxs-lookup"><span data-stu-id="04082-237">In previous versions of ASP.NET, the URL character checks were limited to a fixed set of characters.</span></span> <span data-ttu-id="04082-238">在 ASP.NET 4 中，您可以自訂的一組使用新的有效字元*requestPathInvalidChars*屬性*httpRuntime*組態項目，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-238">In ASP.NET 4, you can customize the set of valid characters using the new *requestPathInvalidChars* attribute of the *httpRuntime* configuration element, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample11.xml)]

<span data-ttu-id="04082-239">根據預設， <em>requestPathInvalidChars</em>屬性定義為無效的八個字元。</span><span class="sxs-lookup"><span data-stu-id="04082-239">By default, the <em>requestPathInvalidChars</em> attribute defines eight characters as invalid.</span></span> <span data-ttu-id="04082-240">(在字串指派給<em>requestPathInvalidChars</em>預設<em>，</em>小於 (&lt;)、 大於 (&gt;)，和連字號 (&amp;) 字元編碼，因為`Web.config`檔案是 XML 檔案。)視需要您可以自訂的一組無效的字元。</span><span class="sxs-lookup"><span data-stu-id="04082-240">(In the string that is assigned to <em>requestPathInvalidChars</em> by default<em>,</em>the less than (&lt;), greater than (&gt;), and ampersand (&amp;) characters are encoded, because the `Web.config` file is an XML file.) You can customize the set of invalid characters as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="04082-241">請注意，ASP.NET 4 一律會拒絕包含字元 0x00 到 0x1F ASCII 範圍中的 URL 路徑，因為這些是在 IETF 的 RFC 2396 定義的無效的 URL 字元 ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt))。</span><span class="sxs-lookup"><span data-stu-id="04082-241">Note ASP.NET 4 always rejects URL paths that contain characters in the ASCII range of 0x00 to 0x1F, because those are invalid URL characters as defined in RFC 2396 of the IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)).</span></span> <span data-ttu-id="04082-242">Windows Server 版本上執行 IIS 6 或更新版本中，http.sys 通訊協定的裝置驅動程式會自動拒絕 Url 具有這些字元。</span><span class="sxs-lookup"><span data-stu-id="04082-242">On versions of Windows Server that run IIS 6 or higher, the http.sys protocol device driver automatically rejects URLs with these characters.</span></span>


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a><span data-ttu-id="04082-243">可延伸的要求驗證</span><span class="sxs-lookup"><span data-stu-id="04082-243">Extensible Request Validation</span></span>

<span data-ttu-id="04082-244">ASP.NET 要求驗證跨網站指令碼 (XSS) 攻擊中，搜尋傳入 HTTP 要求資料的常用的字串。</span><span class="sxs-lookup"><span data-stu-id="04082-244">ASP.NET request validation searches incoming HTTP request data for strings that are commonly used in cross-site scripting (XSS) attacks.</span></span> <span data-ttu-id="04082-245">如果找不到可能的 XSS 字串，則要求驗證旗標的可疑的字串，並傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="04082-245">If potential XSS strings are found, request validation flags the suspect string and returns an error.</span></span> <span data-ttu-id="04082-246">只有在找到最常見的字串，用於 XSS 攻擊時，才，內建的要求驗證就會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="04082-246">The built-in request validation returns an error only when it finds the most common strings used in XSS attacks.</span></span> <span data-ttu-id="04082-247">先前嘗試以進行更積極的 XSS 驗證卻造成太多誤判。</span><span class="sxs-lookup"><span data-stu-id="04082-247">Previous attempts to make the XSS validation more aggressive resulted in too many false positives.</span></span> <span data-ttu-id="04082-248">不過，客戶可以更積極，或反之可能會想要刻意放寬 XSS 要求驗證檢查的特定頁面，或針對特定類型的要求。</span><span class="sxs-lookup"><span data-stu-id="04082-248">However, customers might want request validation that is more aggressive, or conversely might want to intentionally relax XSS checks for specific pages or for specific types of requests.</span></span>

<span data-ttu-id="04082-249">在 ASP.NET 4 中，要求驗證功能已擴充，讓您可以使用自訂要求驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="04082-249">In ASP.NET 4, the request validation feature has been made extensible so that you can use custom request-validation logic.</span></span> <span data-ttu-id="04082-250">若要擴充要求驗證，您可以建立衍生自新的類別*System.Web.Util.RequestValidator*類型，以及您設定應用程式 (在*httpRuntime*區段`Web.config`檔案) 使用自訂的型別。</span><span class="sxs-lookup"><span data-stu-id="04082-250">To extend request validation, you create a class that derives from the new *System.Web.Util.RequestValidator* type, and you configure the application (in the *httpRuntime* section of the `Web.config` file) to use the custom type.</span></span> <span data-ttu-id="04082-251">下列範例示範如何設定自訂要求驗證類別：</span><span class="sxs-lookup"><span data-stu-id="04082-251">The following example shows how to configure a custom request-validation class:</span></span>

[!code-xml[Main](overview/samples/sample12.xml)]

<span data-ttu-id="04082-252">新*requestValidationType*屬性需要指定類別，可提供自訂要求驗證的標準.NET Framework 型別識別項字串。</span><span class="sxs-lookup"><span data-stu-id="04082-252">The new *requestValidationType* attribute requires a standard .NET Framework type identifier string that specifies the class that provides custom request validation.</span></span> <span data-ttu-id="04082-253">針對每個要求，ASP.NET 會叫用自訂的型別，來處理內送的 HTTP 要求資料的每個片段。</span><span class="sxs-lookup"><span data-stu-id="04082-253">For each request, ASP.NET invokes the custom type to process each piece of incoming HTTP request data.</span></span> <span data-ttu-id="04082-254">傳入的 URL，所有的 HTTP 標頭 （cookie 和自訂標頭），實體是可供用來檢查由自訂要求驗證類別類似下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-254">The incoming URL, all the HTTP headers (both cookies and custom headers), and the entity body are all available for inspection by a custom request validation class like that shown in the following example:</span></span>

[!code-csharp[Main](overview/samples/sample13.cs)]

<span data-ttu-id="04082-255">其中您不想檢查傳入的 HTTP 資料片段的情況下，要求驗證類別可切換回讓只是呼叫執行的 ASP.NET 預設要求驗證*基底。IsValidRequestString。*</span><span class="sxs-lookup"><span data-stu-id="04082-255">For cases where you do not want to inspect a piece of incoming HTTP data, the request-validation class can fall back to let the ASP.NET default request validation run by simply calling *base.IsValidRequestString.*</span></span>

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a><span data-ttu-id="04082-256">物件快取物件和快取的擴充性</span><span class="sxs-lookup"><span data-stu-id="04082-256">Object Caching and Object Caching Extensibility</span></span>

<span data-ttu-id="04082-257">ASP.NET 已自其第一個版本中，包含了功能強大的記憶體內部物件快取 (*System.Web.Caching.Cache*)。</span><span class="sxs-lookup"><span data-stu-id="04082-257">Since its first release, ASP.NET has included a powerful in-memory object cache (*System.Web.Caching.Cache*).</span></span> <span data-ttu-id="04082-258">您已這麼受歡迎，非 Web 應用程式中將它使用快取實作。</span><span class="sxs-lookup"><span data-stu-id="04082-258">The cache implementation has been so popular that it has been used in non-Web applications.</span></span> <span data-ttu-id="04082-259">不過，還是有點不便 Windows Form 或 WPF 應用程式包含的參考`System.Web.dll`只是為了要能夠使用 ASP.NET 物件快取。</span><span class="sxs-lookup"><span data-stu-id="04082-259">However, it is awkward for a Windows Forms or WPF application to include a reference to `System.Web.dll` just to be able to use the ASP.NET object cache.</span></span>

<span data-ttu-id="04082-260">若要讓快取適用於所有的應用程式，.NET Framework 4 引進了新的組件、 新的命名空間、 某些基底類型和具體快取實作。</span><span class="sxs-lookup"><span data-stu-id="04082-260">To make caching available for all applications, the .NET Framework 4 introduces a new assembly, a new namespace, some base types, and a concrete caching implementation.</span></span> <span data-ttu-id="04082-261">新`System.Runtime.Caching.dll`組件包含在新的快取 API *System.Runtime.Caching*命名空間。</span><span class="sxs-lookup"><span data-stu-id="04082-261">The new `System.Runtime.Caching.dll` assembly contains a new caching API in the *System.Runtime.Caching* namespace.</span></span> <span data-ttu-id="04082-262">命名空間包含類別的兩個核心集：</span><span class="sxs-lookup"><span data-stu-id="04082-262">The namespace contains two core sets of classes:</span></span>

- <span data-ttu-id="04082-263">提供的基礎建置的自訂快取實作的任何類型的抽象型別。</span><span class="sxs-lookup"><span data-stu-id="04082-263">Abstract types that provide the foundation for building any type of custom cache implementation.</span></span>
- <span data-ttu-id="04082-264">具體記憶體內部物件快取實作 ( *System.Runtime.Caching.MemoryCache*類別)。</span><span class="sxs-lookup"><span data-stu-id="04082-264">A concrete in-memory object cache implementation (the *System.Runtime.Caching.MemoryCache* class).</span></span>

<span data-ttu-id="04082-265">新*MemoryCache*類別根據密切 ASP.NET 快取，以及它與 ASP.NET 共用許多內部快取引擎邏輯。</span><span class="sxs-lookup"><span data-stu-id="04082-265">The new *MemoryCache* class is modeled closely on the ASP.NET cache, and it shares much of the internal cache engine logic with ASP.NET.</span></span> <span data-ttu-id="04082-266">雖然快取的公用 Api，在*System.Runtime.Caching*如果您曾經使用過 ASP.NET 已更新為支援的自訂快取，開發*快取*物件，您會發現在熟悉的概念新的 Api。</span><span class="sxs-lookup"><span data-stu-id="04082-266">Although the public caching APIs in *System.Runtime.Caching* have been updated to support development of custom caches, if you have used the ASP.NET *Cache* object, you will find familiar concepts in the new APIs.</span></span>

<span data-ttu-id="04082-267">新的深入討論*MemoryCache*類別和支援的基底的 Api 需要整份文件。</span><span class="sxs-lookup"><span data-stu-id="04082-267">An in-depth discussion of the new *MemoryCache* class and supporting base APIs would require an entire document.</span></span> <span data-ttu-id="04082-268">不過，下列範例可讓您了解新的快取 API 的運作方式。</span><span class="sxs-lookup"><span data-stu-id="04082-268">However, the following example gives you an idea of how the new cache API works.</span></span> <span data-ttu-id="04082-269">此範例針對所撰寫的 Windows Forms 應用程式，而不需要任何相依性上`System.Web.dll`。</span><span class="sxs-lookup"><span data-stu-id="04082-269">The example was written for a Windows Forms application, without any dependency on `System.Web.dll`.</span></span>

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a><span data-ttu-id="04082-270">可延伸的 HTML、 URL 和 HTTP 標頭的編碼方式</span><span class="sxs-lookup"><span data-stu-id="04082-270">Extensible HTML, URL, and HTTP Header Encoding</span></span>

<span data-ttu-id="04082-271">在 ASP.NET 4 中，您可以建立自訂編碼常式，如下列的一般文字編碼工作：</span><span class="sxs-lookup"><span data-stu-id="04082-271">In ASP.NET 4, you can create custom encoding routines for the following common text-encoding tasks:</span></span>

- <span data-ttu-id="04082-272">HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="04082-272">HTML encoding.</span></span>
- <span data-ttu-id="04082-273">URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="04082-273">URL encoding.</span></span>
- <span data-ttu-id="04082-274">HTML 屬性編碼。</span><span class="sxs-lookup"><span data-stu-id="04082-274">HTML attribute encoding.</span></span>
- <span data-ttu-id="04082-275">輸出 HTTP 標頭的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="04082-275">Encoding outbound HTTP headers.</span></span>

<span data-ttu-id="04082-276">您可以建立自訂編碼器，藉由衍生自新*System.Web.Util.HttpEncoder*類型，然後設定 ASP.NET 使用中的自訂型別*httpRuntime*一節`Web.config`檔案，做為下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-276">You can create a custom encoder by deriving from the new *System.Web.Util.HttpEncoder* type and then configuring ASP.NET to use the custom type in the *httpRuntime* section of the `Web.config` file, as shown in the following example:</span></span>

[!code-xml[Main](overview/samples/sample15.xml)]

<span data-ttu-id="04082-277">設定自訂編碼器之後，ASP.NET 會自動呼叫的自訂編碼實作公用方法的編碼方式時*System.Web.HttpUtility*或*System.Web.HttpServerUtility*類別則稱為。</span><span class="sxs-lookup"><span data-stu-id="04082-277">After a custom encoder has been configured, ASP.NET automatically calls the custom encoding implementation whenever public encoding methods of the *System.Web.HttpUtility* or *System.Web.HttpServerUtility* classes are called.</span></span> <span data-ttu-id="04082-278">這可讓建立自訂編碼器，會實作積極的字元編碼，而 Web 開發小組的其餘部分會繼續使用公用 Api 的編碼方式的 ASP.NET Web 開發小組的一個部分。</span><span class="sxs-lookup"><span data-stu-id="04082-278">This lets one part of a Web development team create a custom encoder that implements aggressive character encoding, while the rest of the Web development team continues to use the public ASP.NET encoding APIs.</span></span> <span data-ttu-id="04082-279">藉由集中設定中的自訂編碼器*httpRuntime*項目，您可保證所有文字編碼的呼叫，從公用 Api 的編碼方式的 ASP.NET 會路由都傳送到自訂編碼器。</span><span class="sxs-lookup"><span data-stu-id="04082-279">By centrally configuring a custom encoder in the *httpRuntime* element, you are guaranteed that all text-encoding calls from the public ASP.NET encoding APIs are routed through the custom encoder.</span></span>

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a><span data-ttu-id="04082-280">在單一背景工作處理序中的個別應用程式的效能監視</span><span class="sxs-lookup"><span data-stu-id="04082-280">Performance Monitoring for Individual Applications in a Single Worker Process</span></span>

<span data-ttu-id="04082-281">為了增加可以在單一伺服器裝載的網站數目，許多主機服務提供者會在單一背景工作處理序中執行多個 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-281">In order to increase the number of Web sites that can be hosted on a single server, many hosters run multiple ASP.NET applications in a single worker process.</span></span> <span data-ttu-id="04082-282">不過，如果多個應用程式使用單一共用的背景工作處理序，很難識別個別應用程式發生問題的伺服器系統管理員。</span><span class="sxs-lookup"><span data-stu-id="04082-282">However, if multiple applications use a single shared worker process, it is difficult for server administrators to identify an individual application that is experiencing problems.</span></span>

<span data-ttu-id="04082-283">ASP.NET 4 會運用 CLR 所導入新資源監視功能。</span><span class="sxs-lookup"><span data-stu-id="04082-283">ASP.NET 4 leverages new resource-monitoring functionality introduced by the CLR.</span></span> <span data-ttu-id="04082-284">若要啟用這項功能，您可以新增下列 XML 組態程式碼片段`aspnet.config`組態檔。</span><span class="sxs-lookup"><span data-stu-id="04082-284">To enable this functionality, you can add the following XML configuration snippet to the `aspnet.config` configuration file.</span></span>

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> <span data-ttu-id="04082-285">請注意`aspnet.config`檔案位於.NET Framework 安裝所在的目錄。</span><span class="sxs-lookup"><span data-stu-id="04082-285">Note The `aspnet.config` file is in the directory where the .NET Framework is installed.</span></span> <span data-ttu-id="04082-286">它不是`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-286">It is not the `Web.config` file.</span></span>


<span data-ttu-id="04082-287">當*Appdomainresourcemonitoring>* 已啟用功能、 兩個新的效能計數器位於 [ASP.NET 應用程式] 的效能分類：*受控處理器時間百分比*和*Managed 記憶體使用*。</span><span class="sxs-lookup"><span data-stu-id="04082-287">When the *appDomainResourceMonitoring* feature has been enabled, two new performance counters are available in the "ASP.NET Applications" performance category: *% Managed Processor Time* and *Managed Memory Used*.</span></span> <span data-ttu-id="04082-288">這兩個這些效能計數器來追蹤估計的 CPU 時間和受管理的記憶體使用率的個別的 ASP.NET 應用程式使用新的 CLR 應用程式定義域資源管理功能。</span><span class="sxs-lookup"><span data-stu-id="04082-288">Both of these performance counters use the new CLR application-domain resource management feature to track estimated CPU time and managed memory utilization of individual ASP.NET applications.</span></span> <span data-ttu-id="04082-289">如此一來，ASP.NET 4 中，以系統管理員現在可以在單一背景工作處理序中執行的個別應用程式的資源耗用量的更細微檢視。</span><span class="sxs-lookup"><span data-stu-id="04082-289">As a result, with ASP.NET 4, administrators now have a more granular view into the resource consumption of individual applications running in a single worker process.</span></span>

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a><span data-ttu-id="04082-290">多目標</span><span class="sxs-lookup"><span data-stu-id="04082-290">Multi-Targeting</span></span>

<span data-ttu-id="04082-291">您可以建立以特定版本的.NET framework 為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-291">You can create an application that targets a specific version of the .NET Framework.</span></span> <span data-ttu-id="04082-292">在 ASP.NET 4 中中的新屬性*編譯*項目`Web.config`檔案可讓您以.NET Framework 4 及更新版本為目標。</span><span class="sxs-lookup"><span data-stu-id="04082-292">In ASP.NET 4, a new attribute in the *compilation* element of the `Web.config` file lets you target the .NET Framework 4 and later.</span></span> <span data-ttu-id="04082-293">如果您明確的目標.NET Framework 4，，和包含選擇性的項目中`Web.config`檔案的項目如*system.codedom*，這些項目必須是正確的.NET Framework 4。</span><span class="sxs-lookup"><span data-stu-id="04082-293">If you explicitly target the .NET Framework 4, and if you include optional elements in the `Web.config` file such as the entries for *system.codedom*, these elements must be correct for the .NET Framework 4.</span></span> <span data-ttu-id="04082-294">(如果未明確目標.NET Framework 4，目標架構會推斷中的項目和缺少`Web.config`檔案。)</span><span class="sxs-lookup"><span data-stu-id="04082-294">(If you do not explicitly target the .NET Framework 4, the target framework is inferred from the lack of an entry in the `Web.config` file.)</span></span>

<span data-ttu-id="04082-295">下列範例示範使用*targetFramework*屬性中*編譯*項目`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-295">The following example shows the use of the *targetFramework* attribute in the *compilation* element of the `Web.config` file.</span></span>

[!code-xml[Main](overview/samples/sample17.xml)]

<span data-ttu-id="04082-296">請注意下列有關特定版本的.NET framework 為目標：</span><span class="sxs-lookup"><span data-stu-id="04082-296">Note the following about targeting a specific version of the .NET Framework:</span></span>

- <span data-ttu-id="04082-297">在.NET Framework 4 應用程式集區中，ASP.NET 建置系統會假設為.NET Framework 4 做為目標如果`Web.config`檔案不包含*targetFramework*屬性或`Web.config`遺漏的檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-297">In a .NET Framework 4 application pool, the ASP.NET build system assumes the .NET Framework 4 as a target if the `Web.config` file does not include the *targetFramework* attribute or if the `Web.config` file is missing.</span></span> <span data-ttu-id="04082-298">（您可能需要對您的應用程式，讓它在.NET Framework 4 下執行的程式碼的變更）。</span><span class="sxs-lookup"><span data-stu-id="04082-298">(You might have to make coding changes to your application to make it run under the .NET Framework 4.)</span></span>
- <span data-ttu-id="04082-299">若您納入*targetFramework*屬性，而如果*system.codeDom*項目定義在`Web.config`檔案，此檔案必須包含正確的項目，適用於.NET Framework 4。</span><span class="sxs-lookup"><span data-stu-id="04082-299">If you do include the *targetFramework* attribute, and if the *system.codeDom* element is defined in the `Web.config` file, this file must contain the correct entries for the .NET Framework 4.</span></span>
- <span data-ttu-id="04082-300">如果您使用*aspnet\_編譯器*命令來先行編譯您的應用程式 （例如在建置環境），您必須使用正確的版本*aspnet\_編譯器*目標 framework 的命令。</span><span class="sxs-lookup"><span data-stu-id="04082-300">If you are using the *aspnet\_compiler* command to precompile your application (such as in a build environment), you must use the correct version of the *aspnet\_compiler* command for the target framework.</span></span> <span data-ttu-id="04082-301">使用.NET Framework 2.0 （%windir%\microsoft.net\framework\v2.0.50727) 的出貨的編譯器編譯的.NET Framework 3.5 和更早版本。</span><span class="sxs-lookup"><span data-stu-id="04082-301">Use the compiler that shipped with the .NET Framework 2.0 (%WINDIR%\Microsoft.NET\Framework\v2.0.50727) to compile for the .NET Framework 3.5 and earlier versions.</span></span> <span data-ttu-id="04082-302">使用.NET Framework 4 的附隨的編譯器來編譯應用程式建立使用該架構或更新版本。</span><span class="sxs-lookup"><span data-stu-id="04082-302">Use the compiler that ships with the .NET Framework 4 to compile applications created using that framework or using later versions.</span></span>
- <span data-ttu-id="04082-303">在執行階段，編譯器會使用最新的 framework 組件安裝在電腦上 (因此在 GAC 中)。</span><span class="sxs-lookup"><span data-stu-id="04082-303">At run time, the compiler uses the latest framework assemblies that are installed on the computer (and therefore in the GAC).</span></span> <span data-ttu-id="04082-304">如果 framework 稍後進行更新 （例如，假設版本 4.1 安裝），您可以使用功能較新的 framework 版本中，即使*targetFramework*屬性以較低的版本為目標（例如 4.0)。</span><span class="sxs-lookup"><span data-stu-id="04082-304">If an update is made later to the framework (for example, a hypothetical version 4.1 is installed), you will be able to use features in the newer version of the framework even though the *targetFramework* attribute targets a lower version (such as 4.0).</span></span> <span data-ttu-id="04082-305">(不過，在設計階段於 Visual Studio 2010 或當您使用*aspnet\_編譯器*命令時，使用較新的 framework 的功能將會導致編譯器錯誤)。</span><span class="sxs-lookup"><span data-stu-id="04082-305">(However, at design time in Visual Studio 2010 or when you use the *aspnet\_compiler* command, using newer features of the framework will cause compiler errors).</span></span>

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a><span data-ttu-id="04082-306">Ajax</span><span class="sxs-lookup"><span data-stu-id="04082-306">Ajax</span></span>

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a><span data-ttu-id="04082-307">包括在 Web Form 和 MVC 的 jQuery</span><span class="sxs-lookup"><span data-stu-id="04082-307">jQuery Included with Web Forms and MVC</span></span>

<span data-ttu-id="04082-308">Web Form 和 MVC 的 Visual Studio 範本包含開放原始碼 jQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="04082-308">The Visual Studio templates for both Web Forms and MVC include the open-source jQuery library.</span></span> <span data-ttu-id="04082-309">當您建立新的網站或專案時，會建立包含下列 3 個檔案的指令碼資料夾：</span><span class="sxs-lookup"><span data-stu-id="04082-309">When you create a new website or project, a Scripts folder containing the following 3 files is created:</span></span>

- <span data-ttu-id="04082-310">jQuery-1.4.1.js – 人類看得懂，unminified jQuery 程式庫版本。</span><span class="sxs-lookup"><span data-stu-id="04082-310">jQuery-1.4.1.js – The human-readable, unminified version of the jQuery library.</span></span>
- <span data-ttu-id="04082-311">-jQuery 14.1.min.js 的 – 縮短的 jQuery 程式庫版本。</span><span class="sxs-lookup"><span data-stu-id="04082-311">jQuery-14.1.min.js – The minified version of the jQuery library.</span></span>
- <span data-ttu-id="04082-312">jQuery-1.4.1-vsdoc.js – jQuery 程式庫的 Intellisense 文件檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-312">jQuery-1.4.1-vsdoc.js – The Intellisense documentation file for the jQuery library.</span></span>

<span data-ttu-id="04082-313">開發應用程式時包含 jQuery unminified 的版本。</span><span class="sxs-lookup"><span data-stu-id="04082-313">Include the unminified version of jQuery while developing an application.</span></span> <span data-ttu-id="04082-314">包含的實際執行應用程式的 jQuery 縮短的版本。</span><span class="sxs-lookup"><span data-stu-id="04082-314">Include the minified version of jQuery for production applications.</span></span>

<span data-ttu-id="04082-315">例如，下列的 Web Form 頁面會說明如何使用 jQuery，以及在若要變更 ASP.NET TextBox 控制項設為黃色，當他們有焦點時的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="04082-315">For example, the following Web Forms page illustrates how you can use jQuery to change the background color of ASP.NET TextBox controls to yellow when they have focus.</span></span>

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a><span data-ttu-id="04082-316">內容傳遞網路支援</span><span class="sxs-lookup"><span data-stu-id="04082-316">Content Delivery Network Support</span></span>

<span data-ttu-id="04082-317">Microsoft Ajax 內容傳遞網路 (CDN) 可讓您輕鬆地將 ASP.NET Ajax 和 jQuery 指令碼新增至您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-317">The Microsoft Ajax Content Delivery Network (CDN) enables you to easily add ASP.NET Ajax and jQuery scripts to your Web applications.</span></span> <span data-ttu-id="04082-318">例如，您可以在其中開始使用 jQuery 程式庫新增`<script>`您指向 Ajax.microsoft.com，像這樣的頁面的標記：</span><span class="sxs-lookup"><span data-stu-id="04082-318">For example, you can start using the jQuery library simply by adding a `<script>` tag to your page that points to Ajax.microsoft.com like this:</span></span>

[!code-html[Main](overview/samples/sample19.html)]

<span data-ttu-id="04082-319">利用 Microsoft Ajax CDN，您可以大幅改善 Ajax 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="04082-319">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="04082-320">Microsoft Ajax CDN 的內容會在位於世界各地的伺服器上快取。</span><span class="sxs-lookup"><span data-stu-id="04082-320">The contents of the Microsoft Ajax CDN are cached on servers located around the world.</span></span> <span data-ttu-id="04082-321">此外，Microsoft Ajax CDN 可讓瀏覽器針對位於不同網域中的網站重複使用快取的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-321">In addition, the Microsoft Ajax CDN enables browsers to reuse cached JavaScript files for Web sites that are located in different domains.</span></span>

<span data-ttu-id="04082-322">Microsoft Ajax 內容傳遞網路支援 SSL (HTTPS)，萬一您需要提供使用 Secure Sockets Layer 的網頁。</span><span class="sxs-lookup"><span data-stu-id="04082-322">The Microsoft Ajax Content Delivery Network supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="04082-323">無法使用 CDN 時，請實作後援。</span><span class="sxs-lookup"><span data-stu-id="04082-323">Implement a fallback when the CDN is unavailable.</span></span> <span data-ttu-id="04082-324">測試此後援。</span><span class="sxs-lookup"><span data-stu-id="04082-324">Test the fallback.</span></span>

<span data-ttu-id="04082-325">若要深入了解 Microsoft Ajax CDN，請瀏覽下列網站：</span><span class="sxs-lookup"><span data-stu-id="04082-325">To learn more about the Microsoft Ajax CDN, visit the following website:</span></span>

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

<span data-ttu-id="04082-326">ASP.NET ScriptManager 支援 Microsoft Ajax CDN。</span><span class="sxs-lookup"><span data-stu-id="04082-326">The ASP.NET ScriptManager supports the Microsoft Ajax CDN.</span></span> <span data-ttu-id="04082-327">只要設定一個屬性，加入 EnableCdn 屬性中，您可以從 CDN 擷取所有 ASP.NET framework JavaScript 檔案：</span><span class="sxs-lookup"><span data-stu-id="04082-327">Simply by setting one property, the EnableCdn property, you can retrieve all ASP.NET framework JavaScript files from the CDN:</span></span>

[!code-aspx[Main](overview/samples/sample20.aspx)]

<span data-ttu-id="04082-328">您將設定加入 EnableCdn 屬性值為 true 之後，ASP.NET 架構會擷取所有 ASP.NET framework JavaScript 檔案從 CDN 包括用於驗證和 UpdatePanel 的所有 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-328">After you set the EnableCdn property to the value true, the ASP.NET framework will retrieve all ASP.NET framework JavaScript files from the CDN including all JavaScript files used for validation and the UpdatePanel.</span></span> <span data-ttu-id="04082-329">設定這個屬性會對您的 web 應用程式的效能造成重大的影響。</span><span class="sxs-lookup"><span data-stu-id="04082-329">Setting this one property can have a dramatic impact on the performance of your web application.</span></span>

<span data-ttu-id="04082-330">您可以使用 WebResource 屬性，為您自己的 JavaScript 檔案設定的 CDN 路徑。</span><span class="sxs-lookup"><span data-stu-id="04082-330">You can set the CDN path for your own JavaScript files by using the WebResource attribute.</span></span> <span data-ttu-id="04082-331">新的 CdnPath 屬性會指定 CDN 加入 EnableCdn 屬性設定為值 true 時所使用的路徑：</span><span class="sxs-lookup"><span data-stu-id="04082-331">The new CdnPath property specifies the path to the CDN used when you set the EnableCdn property to the value true:</span></span>

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a><span data-ttu-id="04082-332">ScriptManager 明確指令碼</span><span class="sxs-lookup"><span data-stu-id="04082-332">ScriptManager Explicit Scripts</span></span>

<span data-ttu-id="04082-333">在過去，如果您使用 ASP.NET ScriptManger 然後您必須將整個的整合型 ASP.NET Ajax 程式庫。</span><span class="sxs-lookup"><span data-stu-id="04082-333">In the past, if you used the ASP.NET ScriptManger then you were required to load the entire monolithic ASP.NET Ajax Library.</span></span> <span data-ttu-id="04082-334">利用新的 ScriptManager.AjaxFrameworkMode 屬性，您可以控制完全載入的 ASP.NET Ajax 程式庫的元件，並載入 ASP.NET Ajax 程式庫所需的元件。</span><span class="sxs-lookup"><span data-stu-id="04082-334">By taking advantage of the new ScriptManager.AjaxFrameworkMode property, you can control exactly which components of the ASP.NET Ajax Library are loaded and load only the components of the ASP.NET Ajax Library that you need.</span></span>

<span data-ttu-id="04082-335">ScriptManager.AjaxFrameworkMode 屬性可以設定為下列值：</span><span class="sxs-lookup"><span data-stu-id="04082-335">The ScriptManager.AjaxFrameworkMode property can be set to the following values:</span></span>

- <span data-ttu-id="04082-336">已啟用-指定 ScriptManager 控制項，會自動包含 MicrosoftAjax.js 指令碼檔案，也就是結合的指令碼檔案的每個核心架構指令碼 （舊版行為）。</span><span class="sxs-lookup"><span data-stu-id="04082-336">Enabled -- Specifies that the ScriptManager control automatically includes the MicrosoftAjax.js script file, which is a combined script file of every core framework script (legacy behavior).</span></span>
- <span data-ttu-id="04082-337">停用-指定已停用所有的 Microsoft Ajax 指令碼功能和，ScriptManager 控制項未參考任何指令碼自動。</span><span class="sxs-lookup"><span data-stu-id="04082-337">Disabled -- Specifies that all Microsoft Ajax script features are disabled and that the ScriptManager control does not reference any scripts automatically.</span></span>
- <span data-ttu-id="04082-338">明確-指定您要明確地加入至您的頁面所需要的個別架構核心指令碼檔案的指令碼參考，並將每個指令碼檔所需的相依性的參考。</span><span class="sxs-lookup"><span data-stu-id="04082-338">Explicit -- Specifies that you will explicitly include script references to individual framework core script file that your page requires, and that you will include references to the dependencies that each script file requires.</span></span>

<span data-ttu-id="04082-339">比方說，如果您將加入 AjaxFrameworkMode 屬性設定為明確的值然後您可以指定特定 ASP.NET Ajax 元件指令碼，您需要：</span><span class="sxs-lookup"><span data-stu-id="04082-339">For example, if you set the AjaxFrameworkMode property to the value Explicit then you can specify the particular ASP.NET Ajax component scripts that you need:</span></span>

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a><span data-ttu-id="04082-340">Web Form</span><span class="sxs-lookup"><span data-stu-id="04082-340">Web Forms</span></span>

<span data-ttu-id="04082-341">Web Form ASP.NET 1.0 發行以來已在 ASP.NET 中的核心功能。</span><span class="sxs-lookup"><span data-stu-id="04082-341">Web Forms has been a core feature in ASP.NET since the release of ASP.NET 1.0.</span></span> <span data-ttu-id="04082-342">針對 ASP.NET 4，包括下列這方面已許多增強功能：</span><span class="sxs-lookup"><span data-stu-id="04082-342">Many enhancements have been in this area for ASP.NET 4, including the following:</span></span>

- <span data-ttu-id="04082-343">若要設定的能力*meta*標記。</span><span class="sxs-lookup"><span data-stu-id="04082-343">The ability to set *meta* tags.</span></span>
- <span data-ttu-id="04082-344">進一步控制檢視狀態的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="04082-344">More control over view state.</span></span>
- <span data-ttu-id="04082-345">使用瀏覽器功能的簡單方法。</span><span class="sxs-lookup"><span data-stu-id="04082-345">Easier ways to work with browser capabilities.</span></span>
- <span data-ttu-id="04082-346">支援使用 ASP.NET Web form 的路由。</span><span class="sxs-lookup"><span data-stu-id="04082-346">Support for using ASP.NET routing with Web Forms.</span></span>
- <span data-ttu-id="04082-347">更充分掌控產生識別碼。</span><span class="sxs-lookup"><span data-stu-id="04082-347">More control over generated IDs.</span></span>
- <span data-ttu-id="04082-348">保存資料控制項中選取的資料列的能力。</span><span class="sxs-lookup"><span data-stu-id="04082-348">The ability to persist selected rows in data controls.</span></span>
- <span data-ttu-id="04082-349">更充分掌控中轉譯的 HTML *FormView*並*ListView*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-349">More control over rendered HTML in the *FormView* and *ListView* controls.</span></span>
- <span data-ttu-id="04082-350">篩選資料來源控制項的支援。</span><span class="sxs-lookup"><span data-stu-id="04082-350">Filtering support for data source controls.</span></span>

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a><span data-ttu-id="04082-351">設定使用 Page.MetaKeywords 和 Page.MetaDescription 屬性的中繼標籤</span><span class="sxs-lookup"><span data-stu-id="04082-351">Setting Meta Tags with the Page.MetaKeywords and Page.MetaDescription Properties</span></span>

<span data-ttu-id="04082-352">ASP.NET 4 中加入兩個屬性，以*網頁*類別*MetaKeywords*並*MetaDescription*。</span><span class="sxs-lookup"><span data-stu-id="04082-352">ASP.NET 4 adds two properties to the *Page* class, *MetaKeywords* and *MetaDescription*.</span></span> <span data-ttu-id="04082-353">這兩個屬性，代表對應*meta*標記儲存在您的網頁，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-353">These two properties represent corresponding *meta* tags in your page, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample23.aspx)]

<span data-ttu-id="04082-354">這兩個屬性的運作方式相同方式頁面*Title*屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-354">These two properties work the same way that the page's *Title* property does.</span></span> <span data-ttu-id="04082-355">它們會遵循下列規則：</span><span class="sxs-lookup"><span data-stu-id="04082-355">They follow these rules:</span></span>

1. <span data-ttu-id="04082-356">如果有任何*中繼*標記中*標頭*符合屬性名稱的項目 (也就是名稱 = 「 關鍵字 」 *Page.MetaKeywords*和名稱 ="description"，如*Page.MetaDescription*，尚未設定這些屬性的意義)，則*meta*呈現時，將頁面加入標記。</span><span class="sxs-lookup"><span data-stu-id="04082-356">If there are no *meta* tags in the *head* element that match the property names (that is, name="keywords" for *Page.MetaKeywords* and name="description" for *Page.MetaDescription*, meaning that these properties have not been set), the *meta* tags will be added to the page when it is rendered.</span></span>
2. <span data-ttu-id="04082-357">如果已有*meta*具有這些名稱的標記，這些屬性會做為取得和設定的現有標籤內容的方法。</span><span class="sxs-lookup"><span data-stu-id="04082-357">If there are already *meta* tags with these names, these properties act as get and set methods for the contents of the existing tags.</span></span>

<span data-ttu-id="04082-358">您可以在執行階段，可讓您從資料庫或其他來源取得內容，並可讓您設定以動態方式來描述項目的標記來設定這些屬性是特定的頁面。</span><span class="sxs-lookup"><span data-stu-id="04082-358">You can set these properties at run time, which lets you get the content from a database or other source, and which lets you set the tags dynamically to describe what a particular page is for.</span></span>

<span data-ttu-id="04082-359">您也可以設定*關鍵字*並*描述*中的屬性 *@ Page*指示詞頂端的 Web Form 網頁的標記，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-359">You can also set the *Keywords* and *Description* properties in the *@ Page* directive at the top of the Web Forms page markup, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample24.aspx)]

<span data-ttu-id="04082-360">這會覆寫*meta*標記內容 （如果有的話） 中的頁面已經宣告。</span><span class="sxs-lookup"><span data-stu-id="04082-360">This will override the *meta* tag contents (if any) already declared in the page.</span></span>

<span data-ttu-id="04082-361">描述的內容*meta*標記用於改善搜尋列出 Google 中的預覽。</span><span class="sxs-lookup"><span data-stu-id="04082-361">The contents of the description *meta* tag are used for improving search listing previews in Google.</span></span> <span data-ttu-id="04082-362">(如需詳細資訊，請參閱 <<c0> [ 改善程式碼片段與中繼描述改造](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html)Google 網站管理員中心部落格上。)Google 和 Windows Live Search 不會使用關鍵字的連接內容的任何項目，但其他搜尋引擎可能。</span><span class="sxs-lookup"><span data-stu-id="04082-362">(For details, see [Improve snippets with a meta description makeover](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) on the Google Webmaster Central blog.) Google and Windows Live Search do not use the contents of the keywords for anything, but other search engines might.</span></span> <span data-ttu-id="04082-363">如需詳細資訊，請參閱 < [Meta 關鍵字建議](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php)搜尋引擎指南網站上。</span><span class="sxs-lookup"><span data-stu-id="04082-363">For more information, see [Meta Keywords Advice](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) on the Search Engine Guide Web site.</span></span>

<span data-ttu-id="04082-364">這些新的屬性是一個簡單的功能，但它們會儲存您的需求，將這些手動新增或撰寫您自己的程式碼，以建立*meta*標記。</span><span class="sxs-lookup"><span data-stu-id="04082-364">These new properties are a simple feature, but they save you from the requirement to add these manually or from writing your own code to create the *meta* tags.</span></span>

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a><span data-ttu-id="04082-365">啟用個別的控制項檢視狀態</span><span class="sxs-lookup"><span data-stu-id="04082-365">Enabling View State for Individual Controls</span></span>

<span data-ttu-id="04082-366">根據預設，檢視狀態被啟用頁面上，即使您不需要應用程式，每個頁面上的控制項可能儲存檢視狀態的結果。</span><span class="sxs-lookup"><span data-stu-id="04082-366">By default, view state is enabled for the page, with the result that each control on the page potentially stores view state even if it is not required for the application.</span></span> <span data-ttu-id="04082-367">檢視狀態資料包含在標記中，會產生頁面，並增加傳送到用戶端的頁面和回傳它所需的時間量。</span><span class="sxs-lookup"><span data-stu-id="04082-367">View state data is included in the markup that a page generates and increases the amount of time it takes to send a page to the client and post it back.</span></span> <span data-ttu-id="04082-368">儲存更多超出所需的檢視狀態，可能會導致效能大幅降低。</span><span class="sxs-lookup"><span data-stu-id="04082-368">Storing more view state than is necessary can cause significant performance degradation.</span></span> <span data-ttu-id="04082-369">在舊版 ASP.NET 中，開發人員可以停用個別的控制項檢視狀態，以減少頁面大小，但必須進行明確的個別控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-369">In earlier versions of ASP.NET, developers could disable view state for individual controls in order to reduce page size, but had to do so explicitly for individual controls.</span></span> <span data-ttu-id="04082-370">在 ASP.NET 4 中，Web 伺服器控制項包括*ViewStateMode*屬性，可讓您依預設停用檢視狀態，然後再將它啟用只針對需要在頁面中的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-370">In ASP.NET 4, Web server controls include a *ViewStateMode* property that lets you disable view state by default and then enable it only for the controls that require it in the page.</span></span>

<span data-ttu-id="04082-371">*ViewStateMode*屬性會有三個值的列舉：*已啟用*，*已停用*，以及*繼承*。</span><span class="sxs-lookup"><span data-stu-id="04082-371">The *ViewStateMode* property takes an enumeration that has three values: *Enabled*, *Disabled*, and *Inherit*.</span></span> <span data-ttu-id="04082-372">*已啟用*啟用檢視狀態，該控制項，而設定為任何子控制項*繼承*或者，有任何設定。</span><span class="sxs-lookup"><span data-stu-id="04082-372">*Enabled* enables view state for that control and for any child controls that are set to *Inherit* or that have nothing set.</span></span> <span data-ttu-id="04082-373">*已停用*停用檢視狀態，並*繼承*指定控制項使用*ViewStateMode*設定從父控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-373">*Disabled* disables view state, and *Inherit* specifies that the control uses the *ViewStateMode* setting from the parent control.</span></span>

<span data-ttu-id="04082-374">下列範例示範如何*ViewStateMode*屬性雖然有效。</span><span class="sxs-lookup"><span data-stu-id="04082-374">The following example shows how the *ViewStateMode* property works.</span></span> <span data-ttu-id="04082-375">標記和程式碼中的下列網頁的控制項包含值*ViewStateMode*屬性：</span><span class="sxs-lookup"><span data-stu-id="04082-375">The markup and code for the controls in the following page includes values for the *ViewStateMode* property:</span></span>

[!code-aspx[Main](overview/samples/sample25.aspx)]

<span data-ttu-id="04082-376">如您所見，程式碼會停用 PlaceHolder1 控制項的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="04082-376">As you can see, the code disables view state for the PlaceHolder1 control.</span></span> <span data-ttu-id="04082-377">子 label1 控制項繼承這個屬性值 (*繼承*的預設值*ViewStateMode* 。 控制項)，因此可以節省任何檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="04082-377">The child label1 control inherits this property value (*Inherit* is the default value for *ViewStateMode* for controls.) and therefore saves no view state.</span></span> <span data-ttu-id="04082-378">在 PlaceHolder2 控制項中， *ViewStateMode*設為*已啟用*，因此 label2 繼承這個屬性，並儲存檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="04082-378">In the PlaceHolder2 control, *ViewStateMode* is set to *Enabled*, so label2 inherits this property and saves view state.</span></span> <span data-ttu-id="04082-379">第一次載入頁面時，*文字*屬性*標籤*控制項設定為"[DynamicValue]"的字串。</span><span class="sxs-lookup"><span data-stu-id="04082-379">When the page is first loaded, the *Text* property of both *Label* controls is set to the string "[DynamicValue]".</span></span>

<span data-ttu-id="04082-380">這些設定的效果是當頁面載入第一次，在瀏覽器中顯示下列輸出：</span><span class="sxs-lookup"><span data-stu-id="04082-380">The effect of these settings is that when the page loads the first time, the following output is displayed in the browser:</span></span>

<span data-ttu-id="04082-381">已停用 `: [DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="04082-381">Disabled `: [DynamicValue]`</span></span>

<span data-ttu-id="04082-382">已啟用：`[DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="04082-382">Enabled:`[DynamicValue]`</span></span>

<span data-ttu-id="04082-383">之後回傳，不過，會顯示下列輸出：</span><span class="sxs-lookup"><span data-stu-id="04082-383">After a postback, however, the following output is displayed:</span></span>

<span data-ttu-id="04082-384">已停用 `: [DeclaredValue]`</span><span class="sxs-lookup"><span data-stu-id="04082-384">Disabled `: [DeclaredValue]`</span></span>

<span data-ttu-id="04082-385">已啟用：`[DynamicValue]`</span><span class="sxs-lookup"><span data-stu-id="04082-385">Enabled:`[DynamicValue]`</span></span>

<span data-ttu-id="04082-386">Label1 控制項 (其*ViewStateMode*值設定為*停用*) 不會保留它已設定為在程式碼中的值。</span><span class="sxs-lookup"><span data-stu-id="04082-386">The label1 control (whose *ViewStateMode* value is set to *Disabled*) has not preserved the value that it was set to in code.</span></span> <span data-ttu-id="04082-387">不過，控制 label2 (其*ViewStateMode*值設定為*已啟用*) 已保留其狀態。</span><span class="sxs-lookup"><span data-stu-id="04082-387">However, the label2 control (whose *ViewStateMode* value is set to *Enabled*) has preserved its state.</span></span>

<span data-ttu-id="04082-388">您也可以設定*ViewStateMode*中 *@ Page*指示詞，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-388">You can also set *ViewStateMode* in the *@ Page* directive, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample26.aspx)]

<span data-ttu-id="04082-389">*網頁*類別是只是另一個控制項，它會作為網頁中的所有其他控制項的父控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-389">The *Page* class is just another control; it acts as the parent control for all the other controls in the page.</span></span> <span data-ttu-id="04082-390">預設值*ViewStateMode*是*已啟用*的執行個體*頁面*。</span><span class="sxs-lookup"><span data-stu-id="04082-390">The default value of *ViewStateMode* is *Enabled* for instances of *Page*.</span></span> <span data-ttu-id="04082-391">因為控制項預設為*繼承*，控制項就會繼承*已啟用*屬性值，除非您設定*ViewStateMode*頁面或控制項的層級。</span><span class="sxs-lookup"><span data-stu-id="04082-391">Because controls default to *Inherit*, controls will inherit the *Enabled* property value unless you set *ViewStateMode* at page or control level.</span></span>

<span data-ttu-id="04082-392">值*ViewStateMode*屬性會決定是否檢視狀態會維護才*EnableViewState*屬性設定為*true*。</span><span class="sxs-lookup"><span data-stu-id="04082-392">The value of the *ViewStateMode* property determines if view state is maintained only if the *EnableViewState* property is set to *true*.</span></span> <span data-ttu-id="04082-393">如果*EnableViewState*屬性設定為*false*，將無法保持檢視狀態即使*ViewStateMode*設定為*已啟用*。</span><span class="sxs-lookup"><span data-stu-id="04082-393">If the *EnableViewState* property is set to *false*, view state will not be maintained even if *ViewStateMode* is set to *Enabled*.</span></span>

<span data-ttu-id="04082-394">適合用於這項功能是使用*ContentPlaceHolder*中，您可以設定的主版頁面的控制項*ViewStateMode*來*已停用*主要頁面上，然後再啟用它為個別*ContentPlaceHolder*又包含需要控制項的控制項檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="04082-394">A good use for this feature is with *ContentPlaceHolder* controls in master pages, where you can set *ViewStateMode* to *Disabled* for the master page and then enable it individually for *ContentPlaceHolder* controls that in turn contain controls that require view state.</span></span>

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a><span data-ttu-id="04082-395">變更瀏覽器功能</span><span class="sxs-lookup"><span data-stu-id="04082-395">Changes to Browser Capabilities</span></span>

<span data-ttu-id="04082-396">ASP.NET 會判斷使用者正在使用利用稱為瀏覽您的網站的瀏覽器的功能*瀏覽器功能*。</span><span class="sxs-lookup"><span data-stu-id="04082-396">ASP.NET determines the capabilities of the browser that a user is using to browse your site by using a feature called *browser capabilities*.</span></span> <span data-ttu-id="04082-397">瀏覽器功能由*HttpBrowserCapabilities*物件 (由*Request.Browser*屬性)。</span><span class="sxs-lookup"><span data-stu-id="04082-397">Browser capabilities are represented by the *HttpBrowserCapabilities* object (exposed by the *Request.Browser* property).</span></span> <span data-ttu-id="04082-398">例如，您可以使用*HttpBrowserCapabilities*來判斷型別和目前的瀏覽器版本是否支援特定版本的 JavaScript 物件。</span><span class="sxs-lookup"><span data-stu-id="04082-398">For example, you can use the *HttpBrowserCapabilities* object to determine whether the type and version of the current browser supports a particular version of JavaScript.</span></span> <span data-ttu-id="04082-399">或者，您可以使用*HttpBrowserCapabilities*物件，以判斷要求是否來自行動裝置。</span><span class="sxs-lookup"><span data-stu-id="04082-399">Or, you can use the *HttpBrowserCapabilities* object to determine whether the request originated from a mobile device.</span></span>

<span data-ttu-id="04082-400">*HttpBrowserCapabilities*物件由一組的瀏覽器定義檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-400">The *HttpBrowserCapabilities* object is driven by a set of browser definition files.</span></span> <span data-ttu-id="04082-401">這些檔案包含特定的瀏覽器功能資訊。</span><span class="sxs-lookup"><span data-stu-id="04082-401">These files contain information about the capabilities of particular browsers.</span></span> <span data-ttu-id="04082-402">在 ASP.NET 4 中，已更新這些瀏覽器定義檔案包含近期推出的瀏覽器和 Google Chrome、 研究等影片 BlackBerry smartphone 和 Apple iPhone 中的裝置的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="04082-402">In ASP.NET 4, these browser definition files have been updated to contain information about recently introduced browsers and devices such as Google Chrome, Research in Motion BlackBerry smartphones, and Apple iPhone.</span></span>

<span data-ttu-id="04082-403">下列清單會顯示新的瀏覽器定義檔案：</span><span class="sxs-lookup"><span data-stu-id="04082-403">The following list shows new browser definition files:</span></span>

- <span data-ttu-id="04082-404">*blackberry.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-404">*blackberry.browser*</span></span>
- <span data-ttu-id="04082-405">*chrome.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-405">*chrome.browser*</span></span>
- <span data-ttu-id="04082-406">*Default.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-406">*Default.browser*</span></span>
- <span data-ttu-id="04082-407">*firefox.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-407">*firefox.browser*</span></span>
- <span data-ttu-id="04082-408">*gateway.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-408">*gateway.browser*</span></span>
- <span data-ttu-id="04082-409">*generic.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-409">*generic.browser*</span></span>
- <span data-ttu-id="04082-410">*ie.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-410">*ie.browser*</span></span>
- <span data-ttu-id="04082-411">*iemobile.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-411">*iemobile.browser*</span></span>
- <span data-ttu-id="04082-412">*iphone.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-412">*iphone.browser*</span></span>
- <span data-ttu-id="04082-413">*opera.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-413">*opera.browser*</span></span>
- <span data-ttu-id="04082-414">*safari.browser*</span><span class="sxs-lookup"><span data-stu-id="04082-414">*safari.browser*</span></span>

#### <a name="using-browser-capabilities-providers"></a><span data-ttu-id="04082-415">使用瀏覽器功能的提供者</span><span class="sxs-lookup"><span data-stu-id="04082-415">Using Browser Capabilities Providers</span></span>

<span data-ttu-id="04082-416">在 ASP.NET 3.5 版 Service Pack 1，您可以定義可透過下列方式在瀏覽器的功能：</span><span class="sxs-lookup"><span data-stu-id="04082-416">In ASP.NET version 3.5 Service Pack 1, you can define the capabilities that a browser has in the following ways:</span></span>

- <span data-ttu-id="04082-417">在電腦層級中，您需要建立或更新`.browser`下列資料夾中的 XML 檔案：</span><span class="sxs-lookup"><span data-stu-id="04082-417">At the computer level, you create or update a `.browser` XML file in the following folder:</span></span>

- [!code-console[Main](overview/samples/sample27.cmd)]

- <span data-ttu-id="04082-418">您定義的瀏覽器功能之後，您執行下列命令從 Visual Studio 命令提示字元以重建瀏覽器的功能組件，並將它新增至 GAC:</span><span class="sxs-lookup"><span data-stu-id="04082-418">After you define the browser capability, you run the following command from the Visual Studio Command Prompt in order to rebuild the browser capabilities assembly and add it to the GAC:</span></span>

- [!code-console[Main](overview/samples/sample28.cmd)]

- <span data-ttu-id="04082-419">在您建立個別的應用程式，如`.browser`應用程式的檔案`App_Browsers`資料夾。</span><span class="sxs-lookup"><span data-stu-id="04082-419">For an individual application, you create a `.browser` file in the application's `App_Browsers` folder.</span></span>

<span data-ttu-id="04082-420">這些方法需要您變更 XML 檔案和電腦層級變更，您必須重新啟動應用程式之後執行 aspnet\_regbrowsers.exe 程序。</span><span class="sxs-lookup"><span data-stu-id="04082-420">These approaches require you to change XML files, and for computer-level changes, you must restart the application after you run the aspnet\_regbrowsers.exe process.</span></span>

<span data-ttu-id="04082-421">ASP.NET 4 包含功能，稱為*瀏覽器功能的提供者*。</span><span class="sxs-lookup"><span data-stu-id="04082-421">ASP.NET 4 includes a feature referred to as *browser capabilities providers*.</span></span> <span data-ttu-id="04082-422">顧名思義，這可讓您建置接著可讓您的提供者會使用您自己的程式碼來判斷瀏覽器功能。</span><span class="sxs-lookup"><span data-stu-id="04082-422">As the name suggests, this lets you build a provider that in turn lets you use your own code to determine browser capabilities.</span></span>

<span data-ttu-id="04082-423">在實務上，開發人員通常不會定義自訂的瀏覽器功能。</span><span class="sxs-lookup"><span data-stu-id="04082-423">In practice, developers often do not define custom browser capabilities.</span></span> <span data-ttu-id="04082-424">瀏覽器檔案是難更新程序相當複雜，更新為與之 XML 語法的`.browser`檔案可以是複雜，無法使用，並定義。</span><span class="sxs-lookup"><span data-stu-id="04082-424">Browser files are hard to update, the process for updating them is fairly complicated, and the XML syntax for `.browser` files can be complex to use and define.</span></span> <span data-ttu-id="04082-425">什麼會讓這個程序更容易是如果有個常見的瀏覽器定義語法，或包含最新的瀏覽器的定義或甚至是這類資料庫的 Web 服務的資料庫。</span><span class="sxs-lookup"><span data-stu-id="04082-425">What would make this process much easier is if there were a common browser definition syntax, or a database that contained up-to-date browser definitions, or even a Web service for such a database.</span></span> <span data-ttu-id="04082-426">新的瀏覽器功能提供者功能會讓這些案例可能和實務的第三方開發人員。</span><span class="sxs-lookup"><span data-stu-id="04082-426">The new browser capabilities providers feature makes these scenarios possible and practical for third-party developers.</span></span>

<span data-ttu-id="04082-427">有兩種主要的方法，使用新的 ASP.NET 4 的瀏覽器功能提供者功能： 擴充 ASP.NET 瀏覽器功能定義功能，或完全取代它。</span><span class="sxs-lookup"><span data-stu-id="04082-427">There are two main approaches for using the new ASP.NET 4 browser capabilities provider feature: extending the ASP.NET browser capabilities definition functionality, or totally replacing it.</span></span> <span data-ttu-id="04082-428">如何取代的功能，以及如何加以擴充，則下列各節會說明第一次。</span><span class="sxs-lookup"><span data-stu-id="04082-428">The following sections describe first how to replace the functionality, and then how to extend it.</span></span>

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a><span data-ttu-id="04082-429">取代 ASP.NET 瀏覽器功能的功能</span><span class="sxs-lookup"><span data-stu-id="04082-429">Replacing the ASP.NET Browser Capabilities Functionality</span></span>

<span data-ttu-id="04082-430">若要完全取代 ASP.NET 瀏覽器功能定義功能，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="04082-430">To replace the ASP.NET browser capabilities definition functionality completely, follow these steps:</span></span>

1. <span data-ttu-id="04082-431">建立提供者類別衍生自*HttpCapabilitiesProvider*它會覆寫*GetBrowserCapabilities*方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-431">Create a provider class that derives from *HttpCapabilitiesProvider* and that overrides the *GetBrowserCapabilities* method, as in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    <span data-ttu-id="04082-432">在此範例中的程式碼會建立新*HttpBrowserCapabilities*物件，指定只將名為瀏覽器，並將該功能設定為 MyCustomBrowser 的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-432">The code in this example creates a new *HttpBrowserCapabilities* object, specifying only the capability named browser and setting that capability to MyCustomBrowser.</span></span>
2. <span data-ttu-id="04082-433">註冊應用程式提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-433">Register the provider with the application.</span></span> 

    <span data-ttu-id="04082-434">若要使用的提供者與應用程式，您必須新增*提供者*屬性設定為*browserCaps*一節中`Web.config`或`Machine.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-434">In order to use a provider with an application, you must add the *provider* attribute to the *browserCaps* section in the `Web.config` or `Machine.config` files.</span></span> <span data-ttu-id="04082-435">(您也可以定義中的提供者屬性*位置*針對特定的目錄，在應用程式，例如特定的行動裝置所需的資料夾中的項目。)下列範例示範如何設定*提供者*組態檔中的屬性：</span><span class="sxs-lookup"><span data-stu-id="04082-435">(You can also define the provider attributes in a *location* element for specific directories in application, such as in a folder for a specific mobile device.) The following example shows how to set the *provider* attribute in a configuration file:</span></span>

    [!code-xml[Main](overview/samples/sample30.xml)]

    <span data-ttu-id="04082-436">若要註冊新的瀏覽器功能定義的另一種方式是使用程式碼，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-436">Another way to register the new browser capability definition is to use code, as shown in the following example:</span></span>

    [!code-csharp[Main](overview/samples/sample31.cs)]

    <span data-ttu-id="04082-437">此程式碼必須執行*應用程式\_開始*事件的`Global.asax`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-437">This code must run in the *Application\_Start* event of the `Global.asax` file.</span></span> <span data-ttu-id="04082-438">對任何變更*BrowserCapabilitiesProvider*類別必須在應用程式中的任何程式碼執行時，若要確定快取仍處於有效狀態的已解析之前進行*HttpCapabilitiesBase*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-438">Any change to the *BrowserCapabilitiesProvider* class must occur before any code in the application executes, in order to make sure that the cache remains in a valid state for the resolved *HttpCapabilitiesBase* object.</span></span>

#### <a name="caching-the-httpbrowsercapabilities-object"></a><span data-ttu-id="04082-439">快取 HttpBrowserCapabilities 物件</span><span class="sxs-lookup"><span data-stu-id="04082-439">Caching the HttpBrowserCapabilities Object</span></span>

<span data-ttu-id="04082-440">上述範例中有一個問題，也就是執行程式碼會叫用自訂提供者以取得每次*HttpBrowserCapabilities*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-440">The preceding example has one problem, which is that the code would run each time the custom provider is invoked in order to get the *HttpBrowserCapabilities* object.</span></span> <span data-ttu-id="04082-441">這可能會多次發生在每個要求。</span><span class="sxs-lookup"><span data-stu-id="04082-441">This can happen multiple times during each request.</span></span> <span data-ttu-id="04082-442">在範例中，提供者的程式碼無法做太多事。</span><span class="sxs-lookup"><span data-stu-id="04082-442">In the example, the code for the provider does not do much.</span></span> <span data-ttu-id="04082-443">不過，如果您的自訂提供者中的程式碼執行中的重要工作以取得*HttpBrowserCapabilities*物件，這可能會影響效能。</span><span class="sxs-lookup"><span data-stu-id="04082-443">However, if the code in your custom provider performs significant work in order to get the *HttpBrowserCapabilities* object, this can affect performance.</span></span> <span data-ttu-id="04082-444">為了避免這種情況，您可以快取*HttpBrowserCapabilities*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-444">To prevent this from happening, you can cache the *HttpBrowserCapabilities* object.</span></span> <span data-ttu-id="04082-445">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="04082-445">Follow these steps:</span></span>

1. <span data-ttu-id="04082-446">建立衍生自類別*HttpCapabilitiesProvider*，在下列範例類似：</span><span class="sxs-lookup"><span data-stu-id="04082-446">Create a class that derives from *HttpCapabilitiesProvider*, like the one in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    <span data-ttu-id="04082-447">在範例中，程式碼產生快取索引鍵呼叫自訂的 BuildCacheKey 方法，而且它會藉由呼叫自訂的 GetCacheTime 方法取得的快取的時間長度。</span><span class="sxs-lookup"><span data-stu-id="04082-447">In the example, the code generates a cache key by calling a custom BuildCacheKey method, and it gets the length of time to cache by calling a custom GetCacheTime method.</span></span> <span data-ttu-id="04082-448">然後程式碼新增解決*HttpBrowserCapabilities*至快取的物件。</span><span class="sxs-lookup"><span data-stu-id="04082-448">The code then adds the resolved *HttpBrowserCapabilities* object to the cache.</span></span> <span data-ttu-id="04082-449">物件可以擷取從快取和重複使用在上進行的後續要求使用的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-449">The object can be retrieved from the cache and reused on subsequent requests that make use of the custom provider.</span></span>
2. <span data-ttu-id="04082-450">在前述程序中所述，則您可以向應用程式提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-450">Register the provider with the application as described in the preceding procedure.</span></span>

#### <a name="extending-aspnet-browser-capabilities-functionality"></a><span data-ttu-id="04082-451">擴充 ASP.NET 瀏覽器功能的功能</span><span class="sxs-lookup"><span data-stu-id="04082-451">Extending ASP.NET Browser Capabilities Functionality</span></span>

<span data-ttu-id="04082-452">上一節說明如何建立新*HttpBrowserCapabilities* ASP.NET 4 中的物件。</span><span class="sxs-lookup"><span data-stu-id="04082-452">The previous section described how to create a new *HttpBrowserCapabilities* object in ASP.NET 4.</span></span> <span data-ttu-id="04082-453">您也可以藉由新增新的瀏覽器功能定義於已經在 ASP.NET 中，擴充 ASP.NET 瀏覽器功能的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-453">You can also extend the ASP.NET browser capabilities functionality by adding new browser capabilities definitions to those that are already in ASP.NET.</span></span> <span data-ttu-id="04082-454">您可以不使用 XML 的瀏覽器定義來這樣做。</span><span class="sxs-lookup"><span data-stu-id="04082-454">You can do this without using the XML browser definitions.</span></span> <span data-ttu-id="04082-455">下列程序顯示如何。</span><span class="sxs-lookup"><span data-stu-id="04082-455">The following procedure shows how.</span></span>

1. <span data-ttu-id="04082-456">建立衍生自類別*HttpCapabilitiesEvaluator*它會覆寫*GetBrowserCapabilities*方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-456">Create a class that derives from *HttpCapabilitiesEvaluator* and that overrides the *GetBrowserCapabilities* method, as shown in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    <span data-ttu-id="04082-457">第一次，此程式碼會嘗試識別瀏覽器使用 ASP.NET 瀏覽器功能的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-457">This code first uses the ASP.NET browser capabilities functionality to try to identify the browser.</span></span> <span data-ttu-id="04082-458">不過，如果瀏覽器不會識別根據在要求中所定義的資訊 (亦即，如果*瀏覽器*屬性*HttpBrowserCapabilities*物件是 「 Unknown 」 字串)，程式碼會呼叫自訂提供者 (MyBrowserCapabilitiesEvaluator) 來識別瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="04082-458">However, if no browser is identified based on the information defined in the request (that is, if the *Browser* property of the *HttpBrowserCapabilities* object is the string "Unknown"), the code calls the custom provider (MyBrowserCapabilitiesEvaluator) to identify the browser.</span></span>
2. <span data-ttu-id="04082-459">在上述範例中所述，則您可以向應用程式提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-459">Register the provider with the application as described in the previous example.</span></span>

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a><span data-ttu-id="04082-460">擴充瀏覽器功能的功能加入到現有的功能定義的新功能</span><span class="sxs-lookup"><span data-stu-id="04082-460">Extending Browser Capabilities Functionality by Adding New Capabilities to Existing Capabilities Definitions</span></span>

<span data-ttu-id="04082-461">除了建立自訂瀏覽器的定義提供者，並以動態方式建立新的瀏覽器的定義，您可以擴充現有的瀏覽器定義，使用額外的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-461">In addition to creating a custom browser definition provider and to dynamically creating new browser definitions, you can extend existing browser definitions with additional capabilities.</span></span> <span data-ttu-id="04082-462">這可讓您使用的定義，很接近您想要但缺少只有少數的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-462">This lets you use a definition that is close to what you want but lacks only a few capabilities.</span></span> <span data-ttu-id="04082-463">若要這麼做，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="04082-463">To do this, use the following steps.</span></span>

1. <span data-ttu-id="04082-464">建立衍生自類別*HttpCapabilitiesEvaluator*它會覆寫*GetBrowserCapabilities*方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-464">Create a class that derives from *HttpCapabilitiesEvaluator* and that overrides the *GetBrowserCapabilities* method, as shown in the following example:</span></span> 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    <span data-ttu-id="04082-465">範例程式碼擴充現有的 ASP.NET *HttpCapabilitiesEvaluator*類別，並取得*HttpBrowserCapabilities*符合目前的要求定義，使用下列程式碼的物件:</span><span class="sxs-lookup"><span data-stu-id="04082-465">The example code extends the existing ASP.NET *HttpCapabilitiesEvaluator* class and gets the *HttpBrowserCapabilities* object that matches the current request definition by using the following code:</span></span>

    [!code-csharp[Main](overview/samples/sample35.cs)]

    <span data-ttu-id="04082-466">程式碼則可以新增或修改此瀏覽器的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-466">The code can then add or modify a capability for this browser.</span></span> <span data-ttu-id="04082-467">有兩種方式可以指定新的瀏覽器功能：</span><span class="sxs-lookup"><span data-stu-id="04082-467">There are two ways to specify a new browser capability:</span></span>

    - <span data-ttu-id="04082-468">將索引鍵/值組加入*IDictionary*所公開的物件*功能*屬性*HttpCapabilitiesBase*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-468">Add a key/value pair to the *IDictionary* object that is exposed by the *Capabilities* property of the *HttpCapabilitiesBase* object.</span></span> <span data-ttu-id="04082-469">在上述範例中，程式碼會加入一項功能使用的值為多點觸控 *，則為 true*。</span><span class="sxs-lookup"><span data-stu-id="04082-469">In the previous example, the code adds a capability named MultiTouch with a value of *true*.</span></span>
    - <span data-ttu-id="04082-470">設定現有的屬性*HttpCapabilitiesBase*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-470">Set existing properties of the *HttpCapabilitiesBase* object.</span></span> <span data-ttu-id="04082-471">在上述範例中，程式碼會設定*框架*屬性設 *，則為 true*。</span><span class="sxs-lookup"><span data-stu-id="04082-471">In the previous example, the code sets the *Frames* property to *true*.</span></span> <span data-ttu-id="04082-472">這個屬性是直接存取子*IDictionary*所公開的物件*功能*屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-472">This property is simply an accessor for the *IDictionary* object that is exposed by the *Capabilities* property.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="04082-473">請注意此模型的任何屬性會套用*HttpBrowserCapabilities*，包括控制項配接器。</span><span class="sxs-lookup"><span data-stu-id="04082-473">Note This model applies to any property of *HttpBrowserCapabilities*, including control adapters.</span></span>
2. <span data-ttu-id="04082-474">先前的程序中所述，則您可以向應用程式提供者。</span><span class="sxs-lookup"><span data-stu-id="04082-474">Register the provider with the application as described in the earlier procedure.</span></span>

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a><span data-ttu-id="04082-475">ASP.NET 4 中的路由</span><span class="sxs-lookup"><span data-stu-id="04082-475">Routing in ASP.NET 4</span></span>

<span data-ttu-id="04082-476">ASP.NET 4 新增內建支援使用 Web form 的路由。</span><span class="sxs-lookup"><span data-stu-id="04082-476">ASP.NET 4 adds built-in support for using routing with Web Forms.</span></span> <span data-ttu-id="04082-477">路由可讓您設定應用程式以接受要求不會對應至實體檔案的 Url。</span><span class="sxs-lookup"><span data-stu-id="04082-477">Routing lets you configure an application to accept request URLs that do not map to physical files.</span></span> <span data-ttu-id="04082-478">相反地，您可以使用路由定義的使用者有意義，而且可以幫助您的應用程式的搜尋引擎最佳化 (SEO) 的 Url。</span><span class="sxs-lookup"><span data-stu-id="04082-478">Instead, you can use routing to define URLs that are meaningful to users and that can help with search-engine optimization (SEO) for your application.</span></span> <span data-ttu-id="04082-479">例如，顯示產品類別目錄中現有的應用程式頁面的 URL 看起來可能如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-479">For example, the URL for a page that displays product categories in an existing application might look like the following example:</span></span>

[!code-console[Main](overview/samples/sample36.cmd)]

<span data-ttu-id="04082-480">藉由使用路由，您可以設定應用程式接受下列的 URL，來呈現相同的資訊：</span><span class="sxs-lookup"><span data-stu-id="04082-480">By using routing, you can configure the application to accept the following URL to render the same information:</span></span>

[!code-console[Main](overview/samples/sample37.cmd)]

<span data-ttu-id="04082-481">路由已可開始使用 ASP.NET 3.5 SP1。</span><span class="sxs-lookup"><span data-stu-id="04082-481">Routing has been available starting with ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="04082-482">(如需如何使用 ASP.NET 3.5 SP1 中的路由的範例，請參閱文章[使用路由與 WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "這個項目的標題。")</span><span class="sxs-lookup"><span data-stu-id="04082-482">(For an example of how to use routing in ASP.NET 3.5 SP1, see the entry [Using Routing With WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Title of this entry.")</span></span> <span data-ttu-id="04082-483">Phil Haack 的部落格。）不過，ASP.NET 4 包含一些功能，使您更輕鬆地使用路由，包括下列：</span><span class="sxs-lookup"><span data-stu-id="04082-483">on Phil Haack's blog.) However, ASP.NET 4 includes some features that make it easier to use routing, including the following:</span></span>

- <span data-ttu-id="04082-484">*PageRouteHandler*類別，這是簡單的 HTTP 處理常式時定義的路由使用。</span><span class="sxs-lookup"><span data-stu-id="04082-484">The *PageRouteHandler* class, which is a simple HTTP handler that you use when you define routes.</span></span> <span data-ttu-id="04082-485">類別會將資料傳遞至要求會路由傳送至頁面。</span><span class="sxs-lookup"><span data-stu-id="04082-485">The class passes data to the page that the request is routed to.</span></span>
- <span data-ttu-id="04082-486">新的屬性*HttpRequest.RequestContext*並*Page.RouteData* (這是 proxy *HttpRequest.RequestContext.RouteData*物件)。</span><span class="sxs-lookup"><span data-stu-id="04082-486">The new properties *HttpRequest.RequestContext* and *Page.RouteData* (which is a proxy for the *HttpRequest.RequestContext.RouteData* object).</span></span> <span data-ttu-id="04082-487">這些屬性可讓您更輕鬆地傳遞來自路由的存取資訊。</span><span class="sxs-lookup"><span data-stu-id="04082-487">These properties make it easier to access information that is passed from the route.</span></span>
- <span data-ttu-id="04082-488">下列新運算式產生器，是定義於*System.Web.Compilation.RouteUrlExpressionBuilder*並*System.Web.Compilation.RouteValueExpressionBuilder*:</span><span class="sxs-lookup"><span data-stu-id="04082-488">The following new expression builders, which are defined in *System.Web.Compilation.RouteUrlExpressionBuilder* and *System.Web.Compilation.RouteValueExpressionBuilder*:</span></span>
- <span data-ttu-id="04082-489">*RouteUrl*，以提供簡單的方法，來建立對應至 ASP.NET 伺服器控制項中的路由 URL 的 URL。</span><span class="sxs-lookup"><span data-stu-id="04082-489">*RouteUrl*, which provides a simple way to create a URL that corresponds to a route URL within an ASP.NET server control.</span></span>
- <span data-ttu-id="04082-490">*RouteValue*，以提供簡單的方法來擷取資訊，從*RouteContext*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-490">*RouteValue*, which provides a simple way to extract information from the *RouteContext* object.</span></span>
- <span data-ttu-id="04082-491">*RouteParameter*類別，可讓您更輕鬆地將中所包含的資料傳遞*RouteContext*的查詢，資料來源控制項的物件 (類似[ *FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).</span><span class="sxs-lookup"><span data-stu-id="04082-491">The *RouteParameter* class, which makes it easier to pass data contained in a *RouteContext* object to a query for a data source control (similar to [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).</span></span>

#### <a name="routing-for-web-forms-pages"></a><span data-ttu-id="04082-492">Web Form 網頁的路由</span><span class="sxs-lookup"><span data-stu-id="04082-492">Routing for Web Forms Pages</span></span>

<span data-ttu-id="04082-493">下列範例示範如何定義使用新的 Web Form 的路由*MapPageRoute*方法*路由*類別：</span><span class="sxs-lookup"><span data-stu-id="04082-493">The following example shows how to define a Web Forms route by using the new *MapPageRoute* method of the *Route* class:</span></span>

[!code-csharp[Main](overview/samples/sample38.cs)]

<span data-ttu-id="04082-494">ASP.NET 4 引進*MapPageRoute*方法。</span><span class="sxs-lookup"><span data-stu-id="04082-494">ASP.NET 4 introduces the *MapPageRoute* method.</span></span> <span data-ttu-id="04082-495">下列範例相當於先前的範例所示的 SearchRoute 定義，但改用*PageRouteHandler*類別。</span><span class="sxs-lookup"><span data-stu-id="04082-495">The following example is equivalent to the SearchRoute definition shown in the previous example, but uses the *PageRouteHandler* class.</span></span>

[!code-csharp[Main](overview/samples/sample39.cs)]

<span data-ttu-id="04082-496">此範例中的程式碼對應至實體頁面的路由 (在第一個路由中，以`~/search.aspx`)。</span><span class="sxs-lookup"><span data-stu-id="04082-496">The code in the example maps the route to a physical page (in the first route, to `~/search.aspx`).</span></span> <span data-ttu-id="04082-497">第一個路由定義也會指定應該從 URL 擷取具名 searchterm 參數，然後傳遞至頁面。</span><span class="sxs-lookup"><span data-stu-id="04082-497">The first route definition also specifies that the parameter named searchterm should be extracted from the URL and passed to the page.</span></span>

<span data-ttu-id="04082-498">*MapPageRoute*方法支援下列方法多載：</span><span class="sxs-lookup"><span data-stu-id="04082-498">The *MapPageRoute* method supports the following method overloads:</span></span>

- <span data-ttu-id="04082-499">*MapPageRoute 字串 routeName、 字串 routeUrl、 字串 physicalFile (bool checkPhysicalUrlAccess）*</span><span class="sxs-lookup"><span data-stu-id="04082-499">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*</span></span>
- <span data-ttu-id="04082-500">*MapPageRoute 字串 routeName、 字串 routeUrl、 字串 physicalFile、 bool checkPhysicalUrlAccess (RouteValueDictionary 預設值）*</span><span class="sxs-lookup"><span data-stu-id="04082-500">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*</span></span>
- <span data-ttu-id="04082-501">*MapPageRoute 字串 routeName、 字串 routeUrl、 字串 physicalFile、 bool checkPhysicalUrlAccess、 RouteValueDictionary 預設值 (RouteValueDictionary 條件約束）*</span><span class="sxs-lookup"><span data-stu-id="04082-501">*MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*</span></span>

<span data-ttu-id="04082-502">*CheckPhysicalUrlAccess*參數會指定路由是否應該檢查路由傳送至實體頁面的安全性權限 (在此情況下，search.aspx) 以及傳入 URL 的權限 （在此情況下，搜尋/ {searchterm})。</span><span class="sxs-lookup"><span data-stu-id="04082-502">The *checkPhysicalUrlAccess* parameter specifies whether the route should check the security permissions for the physical page being routed to (in this case, search.aspx) and the permissions on the incoming URL (in this case, search/{searchterm}).</span></span> <span data-ttu-id="04082-503">如果值*checkPhysicalUrlAccess*是*false*，要檢查的傳入 URL 的權限。</span><span class="sxs-lookup"><span data-stu-id="04082-503">If the value of *checkPhysicalUrlAccess* is *false*, only the permissions of the incoming URL will be checked.</span></span> <span data-ttu-id="04082-504">在 定義這些權限`Web.config`檔案中，使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="04082-504">These permissions are defined in the `Web.config` file using settings such as the following:</span></span>

[!code-xml[Main](overview/samples/sample40.xml)]

<span data-ttu-id="04082-505">在範例組態中，存取被拒的實體頁面`search.aspx`除外身為系統管理員角色中的所有使用者。</span><span class="sxs-lookup"><span data-stu-id="04082-505">In the example configuration, access is denied to the physical page `search.aspx` for all users except those who are in the admin role.</span></span> <span data-ttu-id="04082-506">當*checkPhysicalUrlAccess*參數設為 *，則為 true* （此為其預設值），只有系統管理員的使用者允許存取 URL /search/ {searchterm}，因為實體頁面 search.aspx限制為該角色中的使用者。</span><span class="sxs-lookup"><span data-stu-id="04082-506">When the *checkPhysicalUrlAccess* parameter is set to *true* (which is its default value), only admin users are allowed to access the URL /search/{searchterm}, because the physical page search.aspx is restricted to users in that role.</span></span> <span data-ttu-id="04082-507">如果*checkPhysicalUrlAccess*設為*false*和此站台已在上述範例所示，允許所有已驗證的使用者存取 URL /search/ {searchterm}。</span><span class="sxs-lookup"><span data-stu-id="04082-507">If *checkPhysicalUrlAccess* is set to *false* and the site is configured as shown in the previous example, all authenticated users are allowed to access the URL /search/{searchterm}.</span></span>

#### <a name="reading-routing-information-in-a-web-forms-page"></a><span data-ttu-id="04082-508">讀取 Web Forms 網頁中的路由資訊</span><span class="sxs-lookup"><span data-stu-id="04082-508">Reading Routing Information in a Web Forms Page</span></span>

<span data-ttu-id="04082-509">在 Web Form 實體頁面的程式碼中，您可以存取路由已從 URL 擷取的資訊 (或加入另一個物件，以其他資訊*RouteData*物件) 使用兩個新屬性： *HttpRequest.RequestContext*並*Page.RouteData*。</span><span class="sxs-lookup"><span data-stu-id="04082-509">In the code of the Web Forms physical page, you can access the information that routing has extracted from the URL (or other information that another object has added to the *RouteData* object) by using two new properties: *HttpRequest.RequestContext* and *Page.RouteData*.</span></span> <span data-ttu-id="04082-510">(*Page.RouteData*包裝*HttpRequest.RequestContext.RouteData*。)下列範例示範如何使用*Page.RouteData*。</span><span class="sxs-lookup"><span data-stu-id="04082-510">(*Page.RouteData* wraps *HttpRequest.RequestContext.RouteData*.) The following example shows how to use *Page.RouteData*.</span></span>

[!code-csharp[Main](overview/samples/sample41.cs)]

<span data-ttu-id="04082-511">程式碼會擷取傳遞給 searchterm 參數，如稍早範例路由中所定義的值。</span><span class="sxs-lookup"><span data-stu-id="04082-511">The code extracts the value that was passed for the searchterm parameter, as defined in the example route earlier.</span></span> <span data-ttu-id="04082-512">請考慮下列的要求 URL:</span><span class="sxs-lookup"><span data-stu-id="04082-512">Consider the following request URL:</span></span>

[!code-console[Main](overview/samples/sample42.cmd)]

<span data-ttu-id="04082-513">此提出要求時，會以"scott"這個字`search.aspx`頁面。</span><span class="sxs-lookup"><span data-stu-id="04082-513">When this request is made, the word "scott" would be rendered in the `search.aspx` page.</span></span>

#### <a name="accessing-routing-information-in-markup"></a><span data-ttu-id="04082-514">存取在標記中的路由資訊</span><span class="sxs-lookup"><span data-stu-id="04082-514">Accessing Routing Information in Markup</span></span>

<span data-ttu-id="04082-515">上一節中所述的方法顯示如何在 Web Forms 網頁中的程式碼取得路由資料。</span><span class="sxs-lookup"><span data-stu-id="04082-515">The method described in the previous section shows how to get route data in code in a Web Forms page.</span></span> <span data-ttu-id="04082-516">您也可以使用相同的資訊可讓您存取在標記中的運算式。</span><span class="sxs-lookup"><span data-stu-id="04082-516">You can also use expressions in markup that give you access to the same information.</span></span> <span data-ttu-id="04082-517">運算式產生器是功能強大且簡潔的方式來使用宣告式程式碼。</span><span class="sxs-lookup"><span data-stu-id="04082-517">Expression builders are a powerful and elegant way to work with declarative code.</span></span> <span data-ttu-id="04082-518">(如需詳細資訊，請參閱文章[Express 自行使用自訂運算式產生器](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx)Phil Haack 的部落格上。)</span><span class="sxs-lookup"><span data-stu-id="04082-518">(For more information, see the entry [Express Yourself With Custom Expression Builders](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) on Phil Haack's blog.)</span></span>

<span data-ttu-id="04082-519">ASP.NET 4 包含兩個新的運算式產生器，Web Form 的路由。</span><span class="sxs-lookup"><span data-stu-id="04082-519">ASP.NET 4 includes two new expression builders for Web Forms routing.</span></span> <span data-ttu-id="04082-520">下列範例示範如何使用它們。</span><span class="sxs-lookup"><span data-stu-id="04082-520">The following example shows how to use them.</span></span>

[!code-aspx[Main](overview/samples/sample43.aspx)]

<span data-ttu-id="04082-521">在範例中， *RouteUrl*運算式用來定義為基礎的路由參數的 URL。</span><span class="sxs-lookup"><span data-stu-id="04082-521">In the example, the *RouteUrl* expression is used to define a URL that is based on a route parameter.</span></span> <span data-ttu-id="04082-522">這可讓您不必使用硬式編碼到標記的完整 URL，並可讓您稍後變更 URL 結構，而不需要此連結的任何變更。</span><span class="sxs-lookup"><span data-stu-id="04082-522">This saves you from having to hard-code the complete URL into the markup, and lets you change the URL structure later without requiring any change to this link.</span></span>

<span data-ttu-id="04082-523">根據先前定義的路由，則此標記會產生下列 URL:</span><span class="sxs-lookup"><span data-stu-id="04082-523">Based on the route defined earlier, this markup generates the following URL:</span></span>

[!code-console[Main](overview/samples/sample44.cmd)]

<span data-ttu-id="04082-524">ASP.NET 會自動運作出正確的路由 （也就是它會產生正確的 URL） 根據輸入參數。</span><span class="sxs-lookup"><span data-stu-id="04082-524">ASP.NET automatically works out the correct route (that is, it generates the correct URL) based on the input parameters.</span></span> <span data-ttu-id="04082-525">您也可以包含在運算式中，可讓您指定要使用的路由的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="04082-525">You can also include a route name in the expression, which lets you specify a route to use.</span></span>

<span data-ttu-id="04082-526">下列範例示範如何使用*RouteValue*運算式。</span><span class="sxs-lookup"><span data-stu-id="04082-526">The following example shows how to use the *RouteValue* expression.</span></span>

[!code-aspx[Main](overview/samples/sample45.aspx)]

<span data-ttu-id="04082-527">包含此控制項的網頁執行時，值"scott"會顯示在標籤中。</span><span class="sxs-lookup"><span data-stu-id="04082-527">When the page that contains this control runs, the value "scott" is displayed in the label.</span></span>

<span data-ttu-id="04082-528">*RouteValue*運算式可簡化在標記中，使用路由資料，而且還能避免需要使用更複雜的 Page.RouteData["x"] 在標記中的語法。</span><span class="sxs-lookup"><span data-stu-id="04082-528">The *RouteValue* expression makes it simple to use route data in markup, and it avoids having to work with the more complex Page.RouteData["x"] syntax in markup.</span></span>

#### <a name="using-route-data-for-data-source-control-parameters"></a><span data-ttu-id="04082-529">使用路由資料的資料來源控制項參數</span><span class="sxs-lookup"><span data-stu-id="04082-529">Using Route Data for Data Source Control Parameters</span></span>

<span data-ttu-id="04082-530">*RouteParameter*類別可讓您指定作為資料來源控制項中的查詢的參數值的路由資料。</span><span class="sxs-lookup"><span data-stu-id="04082-530">The *RouteParameter* class lets you specify route data as a parameter value for queries in a data source control.</span></span> <span data-ttu-id="04082-531">它[運作方式就像是](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)類別，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-531">It [works much like the](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) class, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample46.aspx)]

<span data-ttu-id="04082-532">在此情況下，路由參數 searchterm 的值將用於@companyname中的參數<em>選取</em>陳述式。</span><span class="sxs-lookup"><span data-stu-id="04082-532">In this case, the value of the route parameter searchterm will be used for the @companyname parameter in the <em>Select</em> statement.</span></span>

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a><span data-ttu-id="04082-533">設定用戶端識別碼</span><span class="sxs-lookup"><span data-stu-id="04082-533">Setting Client IDs</span></span>

<span data-ttu-id="04082-534">新*ClientIDMode*屬性解決長久以來的問題，在 ASP.NET 中，也就是如何控制項建立*識別碼*它們所呈現的項目屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-534">The new *ClientIDMode* property addresses a long-standing issue in ASP.NET, namely how controls create the *id* attribute for elements that they render.</span></span> <span data-ttu-id="04082-535">了解*識別碼*呈現項目的屬性是很重要，如果您的應用程式包含用戶端指令碼會參考這些項目。</span><span class="sxs-lookup"><span data-stu-id="04082-535">Knowing the *id* attribute for rendered elements is important if your application includes client script that references these elements.</span></span>

<span data-ttu-id="04082-536">*識別碼*呈現 Web 伺服器控制項的 HTML 中的屬性就會產生根據*ClientID*控制項的屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-536">The *id* attribute in HTML that is rendered for Web server controls is generated based on the *ClientID* property of the control.</span></span> <span data-ttu-id="04082-537">直到 ASP.NET 4 中，產生的演算法*識別碼*屬性從*ClientID*屬性已經以串連命名的容器 （如果有的話） 的識別碼，並在重複 （作為中控制項的情況下資料控制項），將前置詞和序號。</span><span class="sxs-lookup"><span data-stu-id="04082-537">Until ASP.NET 4, the algorithm for generating the *id* attribute from the *ClientID* property has been to concatenate the naming container (if any) with the ID, and in the case of repeated controls (as in data controls), to add a prefix and a sequential number.</span></span> <span data-ttu-id="04082-538">雖然這有一定的頁面中的控制項 Id 是唯一的此演算法已經產生的控制不是可預測，並因此很難進行用戶端指令碼中參考的識別碼。</span><span class="sxs-lookup"><span data-stu-id="04082-538">While this has always guaranteed that the IDs of controls in the page are unique, the algorithm has resulted in control IDs that were not predictable, and were therefore difficult to reference in client script.</span></span>

<span data-ttu-id="04082-539">新*ClientIDMode*屬性可讓您指定更精確的用戶端識別碼產生方式的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-539">The new *ClientIDMode* property lets you specify more precisely how the client ID is generated for controls.</span></span> <span data-ttu-id="04082-540">您可以設定*ClientIDMode*任何控制項，包含頁面的屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-540">You can set the *ClientIDMode* property for any control, including for the page.</span></span> <span data-ttu-id="04082-541">可能的設定如下所示：</span><span class="sxs-lookup"><span data-stu-id="04082-541">Possible settings are the following:</span></span>

- <span data-ttu-id="04082-542">*AutoID* – 這相當於產生演算法*ClientID*舊版 ASP.NET 中使用的屬性值。</span><span class="sxs-lookup"><span data-stu-id="04082-542">*AutoID* – This is equivalent to the algorithm for generating *ClientID* property values that was used in earlier versions of ASP.NET.</span></span>
- <span data-ttu-id="04082-543">*靜態*– 這會指定*ClientID*值將會是相同的識別碼，而不需要串連父命名容器的識別碼。</span><span class="sxs-lookup"><span data-stu-id="04082-543">*Static* – This specifies that the *ClientID* value will be the same as the ID without concatenating the IDs of parent naming containers.</span></span> <span data-ttu-id="04082-544">這可用於 Web 使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-544">This can be useful in Web user controls.</span></span> <span data-ttu-id="04082-545">因為 Web 使用者控制項可能位於不同的頁面上，並在不同的容器控制項，所以很難撰寫使用控制項的用戶端指令碼*AutoID*演算法因為您無法預測的 ID 值將會.</span><span class="sxs-lookup"><span data-stu-id="04082-545">Because a Web user control can be located on different pages and in different container controls, it can be difficult to write client script for controls that use the *AutoID* algorithm because you cannot predict what the ID values will be.</span></span>
- <span data-ttu-id="04082-546">*可預測*– 此選項主要是針對使用重複的範本的資料控制項中使用。</span><span class="sxs-lookup"><span data-stu-id="04082-546">*Predictable* – This option is primarily for use in data controls that use repeating templates.</span></span> <span data-ttu-id="04082-547">它會控制的命名容器，但產生的識別碼屬性*ClientID*值並沒有包含"ctlxxx 」 等的字串。</span><span class="sxs-lookup"><span data-stu-id="04082-547">It concatenates the ID properties of the control's naming containers, but generated *ClientID* values do not contain strings like "ctlxxx".</span></span> <span data-ttu-id="04082-548">此設定可搭配*ClientIDRowSuffix*控制項的屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-548">This setting works in conjunction with the *ClientIDRowSuffix* property of the control.</span></span> <span data-ttu-id="04082-549">您設定*ClientIDRowSuffix*資料欄位的名稱和該欄位的值的屬性產生時，可作為尾碼*ClientID*值。</span><span class="sxs-lookup"><span data-stu-id="04082-549">You set the *ClientIDRowSuffix* property to the name of a data field, and the value of that field is used as the suffix for the generated *ClientID* value.</span></span> <span data-ttu-id="04082-550">通常您會使用為資料錄的主索引鍵*ClientIDRowSuffix*值。</span><span class="sxs-lookup"><span data-stu-id="04082-550">Typically you would use the primary key of a data record as the *ClientIDRowSuffix* value.</span></span>
- <span data-ttu-id="04082-551">*繼承*– 此設定是控制項的預設行為; 它會指定控制項的識別碼產生作業是其父代相同。</span><span class="sxs-lookup"><span data-stu-id="04082-551">*Inherit* – This setting is the default behavior for controls; it specifies that a control's ID generation is the same as its parent.</span></span>

<span data-ttu-id="04082-552">您可以設定*ClientIDMode*頁面層級的屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-552">You can set the *ClientIDMode* property at the page level.</span></span> <span data-ttu-id="04082-553">這會定義預設值*ClientIDMode*本頁中的所有控制項的值。</span><span class="sxs-lookup"><span data-stu-id="04082-553">This defines the default *ClientIDMode* value for all controls in the current page.</span></span>

<span data-ttu-id="04082-554">預設值*ClientIDMode*頁面層級的值是*AutoID*，和預設*ClientIDMode*控制層級的值是*繼承*.</span><span class="sxs-lookup"><span data-stu-id="04082-554">The default *ClientIDMode* value at the page level is *AutoID*, and the default *ClientIDMode* value at the control level is *Inherit*.</span></span> <span data-ttu-id="04082-555">如此一來，如果您未設定這個屬性隨處在程式碼中，所有控制項將會都預設為*AutoID*演算法。</span><span class="sxs-lookup"><span data-stu-id="04082-555">As a result, if you do not set this property anywhere in your code, all controls will default to the *AutoID* algorithm.</span></span>

<span data-ttu-id="04082-556">您設定頁面層級值 *@ Page*指示詞，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-556">You set the page-level value in the *@ Page* directive, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample47.aspx)]

<span data-ttu-id="04082-557">您也可以設定*ClientIDMode*組態檔，在電腦 （電腦） 層級或應用程式層級中的值。</span><span class="sxs-lookup"><span data-stu-id="04082-557">You can also set the *ClientIDMode* value in the configuration file, either at the computer (machine) level or at the application level.</span></span> <span data-ttu-id="04082-558">這會定義預設值*ClientIDMode*設定應用程式中的所有頁面中的所有控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-558">This defines the default *ClientIDMode* setting for all controls in all pages in the application.</span></span> <span data-ttu-id="04082-559">如果您在電腦層級設定值，它會定義預設值*ClientIDMode*該電腦上設定的所有網站。</span><span class="sxs-lookup"><span data-stu-id="04082-559">If you set the value at the computer level, it defines the default *ClientIDMode* setting for all Web sites on that computer.</span></span> <span data-ttu-id="04082-560">下列範例所示*ClientIDMode*組態檔中設定：</span><span class="sxs-lookup"><span data-stu-id="04082-560">The following example shows the *ClientIDMode* setting in the configuration file:</span></span>

[!code-xml[Main](overview/samples/sample48.xml)]

<span data-ttu-id="04082-561">如前文所述，windows 7 *ClientID*屬性衍生自命名的容器控制項的父代。</span><span class="sxs-lookup"><span data-stu-id="04082-561">As noted earlier, the value of the *ClientID* property is derived from the naming container for a control's parent.</span></span> <span data-ttu-id="04082-562">在某些情況下，例如當您使用主版頁面中，控制項可以得到識別碼類似下列中呈現 HTML:</span><span class="sxs-lookup"><span data-stu-id="04082-562">In some scenarios, such as when you are using master pages, controls can end up with IDs like those in the following rendered HTML:</span></span>

[!code-html[Main](overview/samples/sample49.html)]

<span data-ttu-id="04082-563">即使*輸入*標記中所顯示的項目 (從*TextBox*控制項) 是只有兩個命名容器頁面中的深度 (巢狀*ContentPlaceholder*控制項)，由於處理主版頁面的方式，最終結果會是如下所示的控制項識別碼：</span><span class="sxs-lookup"><span data-stu-id="04082-563">Even though the *input* element shown in the markup (from a *TextBox* control) is only two naming containers deep in the page (the nested *ContentPlaceholder* controls), because of the way master pages are processed, the end result is a control ID like the following:</span></span>

[!code-console[Main](overview/samples/sample50.cmd)]

<span data-ttu-id="04082-564">此識別碼保證是唯一在頁面中，但不必要地長，不適用於大部分用途。</span><span class="sxs-lookup"><span data-stu-id="04082-564">This ID is guaranteed to be unique in the page, but is unnecessarily long for most purposes.</span></span> <span data-ttu-id="04082-565">假設您想減少轉譯的識別碼，長度或需要更充分掌控所產生的識別碼。</span><span class="sxs-lookup"><span data-stu-id="04082-565">Imagine that you want to reduce the length of the rendered ID, and to have more control over how the ID is generated.</span></span> <span data-ttu-id="04082-566">（例如，您想要消除 「 ctlxxx"前置詞。）若要這麼做最簡單方式是藉由設定*ClientIDMode*屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-566">(For example, you want to eliminate "ctlxxx" prefixes.) The easiest way to achieve this is by setting the *ClientIDMode* property as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample51.aspx)]

<span data-ttu-id="04082-567">在此範例中， *ClientIDMode*屬性設定為*靜態*的最外層*NamingPanel*項目，並設定為*可預測*為內部*NamingControl*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-567">In this sample, the *ClientIDMode* property is set to *Static* for the outermost *NamingPanel* element, and set to *Predictable* for the inner *NamingControl* element.</span></span> <span data-ttu-id="04082-568">這些設定會導致下列標記 （頁面和主版頁面的其餘部分會假設相同，如先前範例所示）：</span><span class="sxs-lookup"><span data-stu-id="04082-568">These settings result in the following markup (the rest of the page and the master page is assumed to be the same as in the previous example):</span></span>

[!code-html[Main](overview/samples/sample52.html)]

<span data-ttu-id="04082-569">*靜態*設定已重設所有控制項最外層命名階層的效果*NamingPanel*項目，以及消除*ContentPlaceHolder*並*MasterPage* Id 的來源產生的識別碼。</span><span class="sxs-lookup"><span data-stu-id="04082-569">The *Static* setting has the effect of resetting the naming hierarchy for any controls inside the outermost *NamingPanel* element, and of eliminating the *ContentPlaceHolder* and *MasterPage* IDs from the generated ID.</span></span> <span data-ttu-id="04082-570">(*名稱*呈現項目的屬性會受到影響，事件會保留一般的 ASP.NET 功能，讓檢視狀態，依此類推。)重設命名階層架構的副作用是，即使您移動的標記*NamingPanel*到不同的項目*ContentPlaceholder*控制項，轉譯的用戶端識別碼維持不變。</span><span class="sxs-lookup"><span data-stu-id="04082-570">(The *name* attribute of rendered elements is unaffected, so the normal ASP.NET functionality is retained for events, view state, and so on.) A side effect of resetting the naming hierarchy is that even if you move the markup for the *NamingPanel* elements to a different *ContentPlaceholder* control, the rendered client IDs remain the same.</span></span>

> [!NOTE]
> <span data-ttu-id="04082-571">請注意，則您必須確定呈現的控制項識別碼都是唯一的。</span><span class="sxs-lookup"><span data-stu-id="04082-571">Note It is up to you to make sure that the rendered control IDs are unique.</span></span> <span data-ttu-id="04082-572">如果他們不這樣做，可能會中斷任何需要對個別的 HTML 項目，例如用戶端的唯一識別碼的功能*document.getElementById*函式。</span><span class="sxs-lookup"><span data-stu-id="04082-572">If they are not, it can break any functionality that requires unique IDs for individual HTML elements, such as the client *document.getElementById* function.</span></span>


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a><span data-ttu-id="04082-573">建立資料繫結控制項中的可預測的用戶端識別碼</span><span class="sxs-lookup"><span data-stu-id="04082-573">Creating Predictable Client IDs in Data-Bound Controls</span></span>

<span data-ttu-id="04082-574">*ClientID*所產生舊的演算法所資料繫結清單控制項中的控制項可以是值，且不是真的可預測。</span><span class="sxs-lookup"><span data-stu-id="04082-574">The *ClientID* values that are generated for controls in a data-bound list control by the legacy algorithm can be long and are not really predictable.</span></span> <span data-ttu-id="04082-575">*ClientIDMode*功能可協助您將更多控制透過這些方式 Id 的產生。</span><span class="sxs-lookup"><span data-stu-id="04082-575">The *ClientIDMode* functionality can help you have more control over how these IDs are generated.</span></span>

<span data-ttu-id="04082-576">在下列範例中的標記包括*ListView*控制項：</span><span class="sxs-lookup"><span data-stu-id="04082-576">The markup in the following example includes a *ListView* control:</span></span>

[!code-aspx[Main](overview/samples/sample53.aspx)]

<span data-ttu-id="04082-577">在上述範例中， *ClientIDMode*並*RowClientIDRowSuffix*標記中設定屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-577">In the previous example, the *ClientIDMode* and *RowClientIDRowSuffix* properties are set in markup.</span></span> <span data-ttu-id="04082-578">*ClientIDRowSuffix*屬性只能用於資料繫結控制項，其行為的差異根據哪一個控制項使用。</span><span class="sxs-lookup"><span data-stu-id="04082-578">The *ClientIDRowSuffix* property can be used only in data-bound controls, and its behavior differs depending on which control you are using.</span></span> <span data-ttu-id="04082-579">這些差異︰</span><span class="sxs-lookup"><span data-stu-id="04082-579">The differences are these:</span></span>

- <span data-ttu-id="04082-580">*GridView*控制項 — 您可以指定一或多個資料行名稱在資料來源，在執行階段建立用戶端識別碼一起使用。</span><span class="sxs-lookup"><span data-stu-id="04082-580">*GridView* control — You can specify the name of one or more columns in the data source, which are combined at run time to create the client IDs.</span></span> <span data-ttu-id="04082-581">例如，如果您設定*RowClientIDRowSuffix* "ProductName ProductId"，來呈現項目的的格式如下所示的控制項 Id:</span><span class="sxs-lookup"><span data-stu-id="04082-581">For example, if you set *RowClientIDRowSuffix* to "ProductName, ProductId", control IDs for rendered elements will have a format like the following:</span></span>

- [!code-console[Main](overview/samples/sample54.cmd)]

- <span data-ttu-id="04082-582">*ListView*控制項 — 您可以指定單一資料行中的資料來源，會附加至用戶端識別碼。</span><span class="sxs-lookup"><span data-stu-id="04082-582">*ListView* control — You can specify a single column in the data source that is appended to the client ID.</span></span> <span data-ttu-id="04082-583">例如，如果您設定*ClientIDRowSuffix* "ProductName"，以呈現的控制項識別碼的格式如下所示：</span><span class="sxs-lookup"><span data-stu-id="04082-583">For example, if you set *ClientIDRowSuffix* to "ProductName", the rendered control IDs will have a format like the following:</span></span>

- [!code-console[Main](overview/samples/sample55.cmd)]

- <span data-ttu-id="04082-584">在此情況下尾端 1 被衍生自目前資料項目的產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="04082-584">In this case the trailing 1 is derived from the product ID of the current data item.</span></span>

- <span data-ttu-id="04082-585">*Repeater*控制項，此控制項不支援*ClientIDRowSuffix*屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-585">*Repeater* control— This control does not support the *ClientIDRowSuffix* property.</span></span> <span data-ttu-id="04082-586">在  *Repeater*控制項中，目前的資料列的索引使用。</span><span class="sxs-lookup"><span data-stu-id="04082-586">In a *Repeater* control, the index of the current row is used.</span></span> <span data-ttu-id="04082-587">當您使用 ClientIDMode = 「 預測 」 *Repeater*控制項，用戶端就能產生識別碼的格式如下：</span><span class="sxs-lookup"><span data-stu-id="04082-587">When you use ClientIDMode="Predictable" with a *Repeater* control, client IDs are generated that have the following format:</span></span>

- [!code-console[Main](overview/samples/sample56.cmd)]

- <span data-ttu-id="04082-588">結尾 0 為目前的資料列的索引。</span><span class="sxs-lookup"><span data-stu-id="04082-588">The trailing 0 is the index of the current row.</span></span>

<span data-ttu-id="04082-589">*FormView*並*DetailsView*控制項不會顯示多個資料列，因此它們不支援*ClientIDRowSuffix*屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-589">The *FormView* and *DetailsView* controls do not display multiple rows, so they do not support the *ClientIDRowSuffix* property.</span></span>

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a><span data-ttu-id="04082-590">在資料控制項中的保存資料列選取範圍</span><span class="sxs-lookup"><span data-stu-id="04082-590">Persisting Row Selection in Data Controls</span></span>

<span data-ttu-id="04082-591">*GridView*並*ListView*控制項可以讓使用者選取一個資料列。</span><span class="sxs-lookup"><span data-stu-id="04082-591">The *GridView* and *ListView* controls can let users select a row.</span></span> <span data-ttu-id="04082-592">在舊版 ASP.NET 中，選取具有已根據頁面上的資料列索引。</span><span class="sxs-lookup"><span data-stu-id="04082-592">In previous versions of ASP.NET, selection has been based on the row index on the page.</span></span> <span data-ttu-id="04082-593">比方說，如果您選取第 1 頁上的第三個項目，然後移至第 2 頁時，就會選取該頁面上的第三個項目。</span><span class="sxs-lookup"><span data-stu-id="04082-593">For example, if you select the third item on page 1 and then move to page 2, the third item on that page is selected.</span></span>

<span data-ttu-id="04082-594">一開始只能在.NET Framework 3.5 SP1 中的動態資料專案中支援保存的選取項目。</span><span class="sxs-lookup"><span data-stu-id="04082-594">Persisted selection was initially supported only in Dynamic Data projects in the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="04082-595">啟用這項功能時，目前選取的項目根據項目的資料索引鍵。</span><span class="sxs-lookup"><span data-stu-id="04082-595">When this feature is enabled, the current selected item is based on the data key for the item.</span></span> <span data-ttu-id="04082-596">這表示如果您選取第 1 頁的第三個資料列，並移至第 2 頁，不會選取在第 2 頁。</span><span class="sxs-lookup"><span data-stu-id="04082-596">This means that if you select the third row on page 1 and move to page 2, nothing is selected on page 2.</span></span> <span data-ttu-id="04082-597">當您移回至第 1 頁時，第三個資料列仍然會選取狀態。</span><span class="sxs-lookup"><span data-stu-id="04082-597">When you move back to page 1, the third row is still selected.</span></span> <span data-ttu-id="04082-598">現在支援保存的選取*GridView*並*ListView*使用的所有專案中的控制項*EnablePersistedSelection*屬性中所示下列範例：</span><span class="sxs-lookup"><span data-stu-id="04082-598">Persisted selection is now supported for the *GridView* and *ListView* controls in all projects by using the *EnablePersistedSelection* property, as shown in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a><span data-ttu-id="04082-599">ASP.NET 的 Chart 控制項</span><span class="sxs-lookup"><span data-stu-id="04082-599">ASP.NET Chart Control</span></span>

<span data-ttu-id="04082-600">ASP.NET*圖表*控制項擴充.NET Framework 中的資料視覺效果供應項目。</span><span class="sxs-lookup"><span data-stu-id="04082-600">The ASP.NET *Chart* control expands the data-visualization offerings in the .NET Framework.</span></span> <span data-ttu-id="04082-601">使用*圖表*控制項，您可以建立有直覺式且引人注目的圖表進行複雜的統計或財務分析的 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="04082-601">Using the *Chart* control, you can create ASP.NET pages that have intuitive and visually compelling charts for complex statistical or financial analysis.</span></span> <span data-ttu-id="04082-602">ASP.NET*圖表*控制項以.NET Framework 3.5 SP1 版中導入成 附加元件和是.NET Framework 4 版本的一部分。</span><span class="sxs-lookup"><span data-stu-id="04082-602">The ASP.NET *Chart* control was introduced as an add-on to the .NET Framework version 3.5 SP1 release and is part of the .NET Framework 4 release.</span></span>

<span data-ttu-id="04082-603">控制項包含下列功能：</span><span class="sxs-lookup"><span data-stu-id="04082-603">The control includes the following features:</span></span>

- <span data-ttu-id="04082-604">35 種不同的圖表類型。</span><span class="sxs-lookup"><span data-stu-id="04082-604">35 distinct chart types.</span></span>
- <span data-ttu-id="04082-605">不限的數目的圖表區域、 標題、 圖例和附註。</span><span class="sxs-lookup"><span data-stu-id="04082-605">An unlimited number of chart areas, titles, legends, and annotations.</span></span>
- <span data-ttu-id="04082-606">各種不同的所有圖表項目的外觀設定。</span><span class="sxs-lookup"><span data-stu-id="04082-606">A wide variety of appearance settings for all chart elements.</span></span>
- <span data-ttu-id="04082-607">大部分的圖表類型支援 3d。</span><span class="sxs-lookup"><span data-stu-id="04082-607">3-D support for most chart types.</span></span>
- <span data-ttu-id="04082-608">可以自動配合資料點周圍的智慧型資料標籤。</span><span class="sxs-lookup"><span data-stu-id="04082-608">Smart data labels that can automatically fit around data points.</span></span>
- <span data-ttu-id="04082-609">帶狀線、 刻度斷層和對數縮放比例。</span><span class="sxs-lookup"><span data-stu-id="04082-609">Strip lines, scale breaks, and logarithmic scaling.</span></span>
- <span data-ttu-id="04082-610">超過 50 種財務和統計公式可供資料分析與轉換。</span><span class="sxs-lookup"><span data-stu-id="04082-610">More than 50 financial and statistical formulas for data analysis and transformation.</span></span>
- <span data-ttu-id="04082-611">簡單繫結和操作的圖表資料。</span><span class="sxs-lookup"><span data-stu-id="04082-611">Simple binding and manipulation of chart data.</span></span>
- <span data-ttu-id="04082-612">常見的資料格式，例如日期、 時間和貨幣的支援。</span><span class="sxs-lookup"><span data-stu-id="04082-612">Support for common data formats such as dates, times, and currency.</span></span>
- <span data-ttu-id="04082-613">支援互動性和事件驅動的自訂，包括用戶端按一下事件使用 Ajax。</span><span class="sxs-lookup"><span data-stu-id="04082-613">Support for interactivity and event-driven customization, including client click events using Ajax.</span></span>
- <span data-ttu-id="04082-614">狀態管理。</span><span class="sxs-lookup"><span data-stu-id="04082-614">State management.</span></span>
- <span data-ttu-id="04082-615">二進位資料流。</span><span class="sxs-lookup"><span data-stu-id="04082-615">Binary streaming.</span></span>

<span data-ttu-id="04082-616">下圖顯示範例的財務 ASP.NET 的 Chart 控制項所產生的圖表。</span><span class="sxs-lookup"><span data-stu-id="04082-616">The following figures show examples of financial charts that are produced by the ASP.NET Chart control.</span></span>

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

<span data-ttu-id="04082-617">圖 2: ASP.NET 圖表控制項範例</span><span class="sxs-lookup"><span data-stu-id="04082-617">Figure 2: ASP.NET Chart control examples</span></span>

<span data-ttu-id="04082-618">如需詳細範例，示範如何使用 ASP.NET 的 Chart 控制項中，下載範例程式碼上[之 Microsoft Chart Controls 的範例環境](https://go.microsoft.com/fwlink/?LinkId=128300)MSDN 網站上的頁面。</span><span class="sxs-lookup"><span data-stu-id="04082-618">For more examples of how to use the ASP.NET Chart control, download the sample code on the [Samples Environment for Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) page on the MSDN Web site.</span></span> <span data-ttu-id="04082-619">您可以找到更多社群內容範例在[Chart 控制項論壇](https://go.microsoft.com/fwlink/?LinkId=128713)。</span><span class="sxs-lookup"><span data-stu-id="04082-619">You can find more samples of community content at the [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713).</span></span>

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a><span data-ttu-id="04082-620">圖表控制項加入 ASP.NET 網頁</span><span class="sxs-lookup"><span data-stu-id="04082-620">Adding the Chart Control to an ASP.NET Page</span></span>

<span data-ttu-id="04082-621">下列範例示範如何新增*圖表*ASP.NET 網頁使用標記的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-621">The following example shows how to add a *Chart* control to an ASP.NET page by using markup.</span></span> <span data-ttu-id="04082-622">在範例中，*圖表*控制項產生靜態的資料點的直條圖。</span><span class="sxs-lookup"><span data-stu-id="04082-622">In the example, the *Chart* control produces a column chart for static data points.</span></span>

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a><span data-ttu-id="04082-623">使用 3d 圖表</span><span class="sxs-lookup"><span data-stu-id="04082-623">Using 3-D Charts</span></span>

<span data-ttu-id="04082-624">*圖表*控制項包含*ChartAreas*集合，其中可以包含*ChartArea*定義特性的圖表區域的物件。</span><span class="sxs-lookup"><span data-stu-id="04082-624">The *Chart* control contains a *ChartAreas* collection, which can contain *ChartArea* objects that define characteristics of chart areas.</span></span> <span data-ttu-id="04082-625">例如，若要使用 3d 圖表區域中，使用*的 [area3dstyle]* 屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-625">For example, to use 3-D for a chart area, use the *Area3DStyle* property as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample59.aspx)]

<span data-ttu-id="04082-626">下圖顯示的四個數列的 3d 圖表*列*圖表類型。</span><span class="sxs-lookup"><span data-stu-id="04082-626">The figure below shows a 3-D chart with four series of the *Bar* chart type.</span></span>

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

<span data-ttu-id="04082-627">圖 3: 3d 橫條圖</span><span class="sxs-lookup"><span data-stu-id="04082-627">Figure 3: 3-D Bar Chart</span></span>

#### <a name="using-scale-breaks-and-logarithmic-scales"></a><span data-ttu-id="04082-628">使用刻度斷層和對數標尺</span><span class="sxs-lookup"><span data-stu-id="04082-628">Using Scale Breaks and Logarithmic Scales</span></span>

<span data-ttu-id="04082-629">刻度斷層和對數標尺是其他兩種方式可以加入至圖表的複雜度。</span><span class="sxs-lookup"><span data-stu-id="04082-629">Scale breaks and logarithmic scales are two additional ways to add sophistication to the chart.</span></span> <span data-ttu-id="04082-630">這些功能專屬於每個圖表區域中的座標軸。</span><span class="sxs-lookup"><span data-stu-id="04082-630">These features are specific to each axis in a chart area.</span></span> <span data-ttu-id="04082-631">例如，若要使用這些功能主要 Y 軸的圖表區域上，使用*AxisY.IsLogarithmic*並*ScaleBreakStyle*中的屬性*ChartArea*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-631">For example, to use these features on the primary Y axis of a chart area, use the *AxisY.IsLogarithmic* and *ScaleBreakStyle* properties in a *ChartArea* object.</span></span> <span data-ttu-id="04082-632">下列程式碼片段示範如何使用主要 Y 軸的刻度斷層。</span><span class="sxs-lookup"><span data-stu-id="04082-632">The following snippet shows how to use scale breaks on the primary Y axis.</span></span>

[!code-aspx[Main](overview/samples/sample60.aspx)]

<span data-ttu-id="04082-633">下圖顯示具有啟用刻度斷層的 Y 軸。</span><span class="sxs-lookup"><span data-stu-id="04082-633">The figure below shows the Y axis with scale breaks enabled.</span></span>

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

<span data-ttu-id="04082-634">圖 4： 刻度斷層</span><span class="sxs-lookup"><span data-stu-id="04082-634">Figure 4: Scale Breaks</span></span>

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a><span data-ttu-id="04082-635">QueryExtender 控制項使用的篩選資料</span><span class="sxs-lookup"><span data-stu-id="04082-635">Filtering Data with the QueryExtender Control</span></span>

<span data-ttu-id="04082-636">建立資料導向的網頁開發人員的常見工作是篩選資料。</span><span class="sxs-lookup"><span data-stu-id="04082-636">A very common task for developers who create data-driven Web pages is to filter data.</span></span> <span data-ttu-id="04082-637">這過去來執行建置*其中*子句中的資料來源控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-637">This traditionally has been performed by building *Where* clauses in data source controls.</span></span> <span data-ttu-id="04082-638">這種方法可能很複雜，在某些情況下*其中*語法不會讓您充分利用基礎資料庫的完整功能。</span><span class="sxs-lookup"><span data-stu-id="04082-638">This approach can be complicated, and in some cases the *Where* syntax does not let you take advantage of the full functionality of the underlying database.</span></span>

<span data-ttu-id="04082-639">若要讓篩選更容易，新*QueryExtender* ASP.NET 4 中已加入控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-639">To make filtering easier, a new *QueryExtender* control has been added in ASP.NET 4.</span></span> <span data-ttu-id="04082-640">這個控制項可以加入至*EntityDataSource*或是*LinqDataSource*控制項，以篩選這些控制項所傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="04082-640">This control can be added to *EntityDataSource* or *LinqDataSource* controls in order to filter the data returned by these controls.</span></span> <span data-ttu-id="04082-641">因為*QueryExtender*控制項要依賴 LINQ、 將資料傳送至頁面上，會導致非常有效率的作業之前在資料庫伺服器上套用篩選。</span><span class="sxs-lookup"><span data-stu-id="04082-641">Because the *QueryExtender* control relies on LINQ, the filter is applied on the database server before the data is sent to the page, which results in very efficient operations.</span></span>

<span data-ttu-id="04082-642">*QueryExtender*控制項支援各種不同的篩選選項。</span><span class="sxs-lookup"><span data-stu-id="04082-642">The *QueryExtender* control supports a variety of filter options.</span></span> <span data-ttu-id="04082-643">下列各節說明這些選項，並提供範例，示範如何使用它們。</span><span class="sxs-lookup"><span data-stu-id="04082-643">The following sections describe these options and provide examples of how to use them.</span></span>

#### <a name="search"></a><span data-ttu-id="04082-644">搜尋</span><span class="sxs-lookup"><span data-stu-id="04082-644">Search</span></span>

<span data-ttu-id="04082-645">搜尋選項時， *QueryExtender*控制項中指定的欄位執行搜尋。</span><span class="sxs-lookup"><span data-stu-id="04082-645">For the search option, the *QueryExtender* control performs a search in specified fields.</span></span> <span data-ttu-id="04082-646">在下列範例中，控制項使用的 TextBoxSearch 控制項和其內容的搜尋中所輸入的文字`ProductName`並`Supplier.CompanyName`傳回的資料中的資料行*LinqDataSource*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-646">In the following example, the control uses the text that is entered in the TextBoxSearch control and searches for its contents in the `ProductName` and `Supplier.CompanyName` columns in the data that is returned from the *LinqDataSource* control.</span></span>

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a><span data-ttu-id="04082-647">範圍</span><span class="sxs-lookup"><span data-stu-id="04082-647">Range</span></span>

<span data-ttu-id="04082-648">範圍選項類似於 [搜尋] 選項，但指定一組值來定義範圍。</span><span class="sxs-lookup"><span data-stu-id="04082-648">The range option is similar to the search option, but specifies a pair of values to define the range.</span></span> <span data-ttu-id="04082-649">在下列範例中， *QueryExtender*控制搜尋`UnitPrice`傳回的資料中的資料行*LinqDataSource*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-649">In the following example, the *QueryExtender* control searches the `UnitPrice` column in the data returned from the *LinqDataSource* control.</span></span> <span data-ttu-id="04082-650">範圍是讀取自 TextBoxFrom 和 TextBoxTo 控制項在頁面上。</span><span class="sxs-lookup"><span data-stu-id="04082-650">The range is read from the TextBoxFrom and TextBoxTo controls on the page.</span></span>

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a><span data-ttu-id="04082-651">PropertyExpression</span><span class="sxs-lookup"><span data-stu-id="04082-651">PropertyExpression</span></span>

<span data-ttu-id="04082-652">屬性的 [運算式] 選項可讓您定義的屬性值的比較。</span><span class="sxs-lookup"><span data-stu-id="04082-652">The property expression option lets you define a comparison to a property value.</span></span> <span data-ttu-id="04082-653">如果運算式評估為 *，則為 true*，會傳回正在檢查的資料。</span><span class="sxs-lookup"><span data-stu-id="04082-653">If the expression evaluates to *true*, the data that is being examined is returned.</span></span> <span data-ttu-id="04082-654">在下列範例中， *QueryExtender*控制項來比較中的資料篩選資料`Discontinued`從 CheckBoxDiscontinued 控制項在頁面上的資料行的值。</span><span class="sxs-lookup"><span data-stu-id="04082-654">In the following example, the *QueryExtender* control filters data by comparing the data in the `Discontinued` column to the value from the CheckBoxDiscontinued control on the page.</span></span>

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a><span data-ttu-id="04082-655">CustomExpression</span><span class="sxs-lookup"><span data-stu-id="04082-655">CustomExpression</span></span>

<span data-ttu-id="04082-656">最後，您可以指定要搭配使用的自訂運算式*QueryExtender*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-656">Finally, you can specify a custom expression to use with the *QueryExtender* control.</span></span> <span data-ttu-id="04082-657">此選項可讓您呼叫的函式中定義自訂篩選器邏輯的頁面。</span><span class="sxs-lookup"><span data-stu-id="04082-657">This option lets you call a function in the page that defines custom filter logic.</span></span> <span data-ttu-id="04082-658">下列範例示範如何以宣告方式指定 使用自訂的運算式*QueryExtender*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-658">The following example shows how to declaratively specify a custom expression in the *QueryExtender* control.</span></span>

[!code-aspx[Main](overview/samples/sample64.aspx)]

<span data-ttu-id="04082-659">下列範例示範自訂函式所叫用*QueryExtender*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-659">The following example shows the custom function that is invoked by the *QueryExtender* control.</span></span> <span data-ttu-id="04082-660">在此情況下，而不是使用包含的資料庫查詢*其中*子句，將程式碼使用 LINQ 查詢來篩選資料。</span><span class="sxs-lookup"><span data-stu-id="04082-660">In this case, instead of using a database query that includes a *Where* clause, the code uses a LINQ query to filter the data.</span></span>

[!code-csharp[Main](overview/samples/sample65.cs)]

<span data-ttu-id="04082-661">下列範例將示範中所用的只有一個運算式*QueryExtender*控制項一次。</span><span class="sxs-lookup"><span data-stu-id="04082-661">These examples show only one expression being used in the *QueryExtender* control at a time.</span></span> <span data-ttu-id="04082-662">不過，您可以包含多個運算式內*QueryExtender*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-662">However, you can include multiple expressions inside the *QueryExtender* control.</span></span>

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a><span data-ttu-id="04082-663">Html 編碼的程式碼運算式</span><span class="sxs-lookup"><span data-stu-id="04082-663">Html Encoded Code Expressions</span></span>

<span data-ttu-id="04082-664">有些的 ASP.NET 網站 （特別是使用 ASP.NET MVC) 依賴使用`<%` =  `expression %>` （通常稱為 「 程式碼區塊 」） 的語法，將一些文字寫入回應。</span><span class="sxs-lookup"><span data-stu-id="04082-664">Some ASP.NET sites (especially with ASP.NET MVC) rely heavily on using `<%`= `expression %>` syntax (often called "code nuggets") to write some text to the response.</span></span> <span data-ttu-id="04082-665">當您使用程式碼運算式時，很容易忘了將 HTML 編碼的文字，如果文字是來自使用者輸入，它可以保留頁面開啟 （跨網站指令碼） 的 XSS 攻擊。</span><span class="sxs-lookup"><span data-stu-id="04082-665">When you use code expressions, it is easy to forget to HTML-encode the text, If the text comes from user input, it can leave pages open to an XSS (Cross Site Scripting) attack.</span></span>

<span data-ttu-id="04082-666">ASP.NET 4 引進下列新的程式碼運算式語法：</span><span class="sxs-lookup"><span data-stu-id="04082-666">ASP.NET 4 introduces the following new syntax for code expressions:</span></span>

[!code-aspx[Main](overview/samples/sample66.aspx)]

<span data-ttu-id="04082-667">此語法會使用 HTML 編碼預設，寫入回應時。</span><span class="sxs-lookup"><span data-stu-id="04082-667">This syntax uses HTML encoding by default when writing to the response.</span></span> <span data-ttu-id="04082-668">這個新的運算式有效地將資料轉譯為下列：</span><span class="sxs-lookup"><span data-stu-id="04082-668">This new expression effectively translates to the following:</span></span>

[!code-aspx[Main](overview/samples/sample67.aspx)]

<span data-ttu-id="04082-669">例如， &lt;%: 要求 ["u"] %&gt;執行值的 HTML 編碼*要求 ["u"]*。</span><span class="sxs-lookup"><span data-stu-id="04082-669">For example, &lt;%: Request["UserInput"] %&gt; performs HTML encoding on the value of *Request["UserInput"]*.</span></span>

<span data-ttu-id="04082-670">這項功能的目標是讓舊語法的所有執行個體取代新語法，因此，您不強制每個步驟，決定要使用哪一個。</span><span class="sxs-lookup"><span data-stu-id="04082-670">The goal of this feature is to make it possible to replace all instances of the old syntax with the new syntax so that you are not forced to decide at every step which one to use.</span></span> <span data-ttu-id="04082-671">不過，一些情況下所輸出的文字是 html 或已編碼，在此情況下這可能會導致重複編碼。</span><span class="sxs-lookup"><span data-stu-id="04082-671">However, there are cases in which the text being output is meant to be HTML or is already encoded, in which case this could lead to double encoding.</span></span>

<span data-ttu-id="04082-672">針對這些情況下，ASP.NET 4 引進了新的介面*IHtmlString*的具象實作，以及*HtmlString*。</span><span class="sxs-lookup"><span data-stu-id="04082-672">For those cases, ASP.NET 4 introduces a new interface, *IHtmlString*, along with a concrete implementation, *HtmlString*.</span></span> <span data-ttu-id="04082-673">這些類型的執行個體可讓您表示，傳回的值是已正確編碼 （或否則檢查） 顯示為 HTML，並，因此值不應該經過 HTML 編碼一次。</span><span class="sxs-lookup"><span data-stu-id="04082-673">Instances of these types let you indicate that the return value is already properly encoded (or otherwise examined) for displaying as HTML, and that therefore the value should not be HTML-encoded again.</span></span> <span data-ttu-id="04082-674">例如，下列不應該 （和不） 編碼的 HTML:</span><span class="sxs-lookup"><span data-stu-id="04082-674">For example, the following should not be (and is not) HTML encoded:</span></span>

[!code-aspx[Main](overview/samples/sample68.aspx)]

<span data-ttu-id="04082-675">ASP.NET MVC 2 的 helper 方法已在已更新為使用這種新語法，使它們不雙重編碼，但只當您執行 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="04082-675">ASP.NET MVC 2 helper methods have been updated to work with this new syntax so that they are not double encoded, but only when you are running ASP.NET 4.</span></span> <span data-ttu-id="04082-676">這種新語法無法運作時執行的應用程式使用 ASP.NET 3.5 SP1。</span><span class="sxs-lookup"><span data-stu-id="04082-676">This new syntax does not work when you run an application using ASP.NET 3.5 SP1.</span></span>

<span data-ttu-id="04082-677">請注意這不保證 XSS 攻擊的保護。</span><span class="sxs-lookup"><span data-stu-id="04082-677">Keep in mind that this does not guarantee protection from XSS attacks.</span></span> <span data-ttu-id="04082-678">例如，使用不在引號中的屬性值的 HTML 可以包含仍然容易受到影響的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="04082-678">For example, HTML that uses attribute values that are not in quotation marks can contain user input that is still susceptible.</span></span> <span data-ttu-id="04082-679">請注意，ASP.NET 控制項和 ASP.NET MVC 協助程式的輸出一律包含屬性值引號括起來，這是建議的方法。</span><span class="sxs-lookup"><span data-stu-id="04082-679">Note that the output of ASP.NET controls and ASP.NET MVC helpers always includes attribute values in quotation marks, which is the recommended approach.</span></span>

<span data-ttu-id="04082-680">同樣地，此語法不會執行 JavaScript 的編碼，例如當您建立 JavaScript 字串，根據使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="04082-680">Likewise, this syntax does not perform JavaScript encoding, such as when you create a JavaScript string based on user input.</span></span>

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a><span data-ttu-id="04082-681">專案範本的變更</span><span class="sxs-lookup"><span data-stu-id="04082-681">Project Template Changes</span></span>

<span data-ttu-id="04082-682">在舊版 ASP.NET 中，當您使用 Visual Studio 來建立新的網站專案或 Web 應用程式專案，產生的專案包含只 Default.aspx 頁面上，預設值`Web.config`檔案，而`App_Data`資料夾，如下列所示圖：</span><span class="sxs-lookup"><span data-stu-id="04082-682">In earlier versions of ASP.NET, when you use Visual Studio to create a new Web Site project or Web Application project, the resulting projects contain only a Default.aspx page, a default `Web.config` file, and the `App_Data` folder, as shown in the following illustration:</span></span>

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

<span data-ttu-id="04082-683">Visual Studio 也支援空白網站專案型別，不包含任何檔案根本，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="04082-683">Visual Studio also supports an Empty Web Site project type, which contains no files at all, as shown in the following figure:</span></span>

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

<span data-ttu-id="04082-684">結果是針對初學者，有極少的指引如何建置生產 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-684">The result is that for the beginner, there is very little guidance on how to build a production Web application.</span></span> <span data-ttu-id="04082-685">因此，ASP.NET 4 將介紹三個新的範本、 一個用於空的 Web 應用程式專案，以及分別指派給 Web 應用程式和網站專案。</span><span class="sxs-lookup"><span data-stu-id="04082-685">Therefore, ASP.NET 4 introduces three new templates, one for an empty Web application project, and one each for a Web Application and Web Site project.</span></span>

#### <a name="empty-web-application-template"></a><span data-ttu-id="04082-686">空的 Web 應用程式範本</span><span class="sxs-lookup"><span data-stu-id="04082-686">Empty Web Application Template</span></span>

<span data-ttu-id="04082-687">顧名思義，空白 Web 應用程式範本是精簡的 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="04082-687">As the name suggests, the Empty Web Application template is a stripped-down Web Application project.</span></span> <span data-ttu-id="04082-688">選取此專案範本從 Visual Studio [新增專案] 對話方塊中，在下圖所示：</span><span class="sxs-lookup"><span data-stu-id="04082-688">You select this project template from the Visual Studio New Project dialog box, as shown in the following figure:</span></span>

[![](overview/_static/image7.png)](overview/_static/image6.png)

<span data-ttu-id="04082-689">([按一下以檢視完整大小的影像](overview/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="04082-689">([Click to view full-size image](overview/_static/image8.png))</span></span>

<span data-ttu-id="04082-690">當您建立空白的 ASP.NET Web 應用程式時，Visual Studio 會建立下列資料夾配置：</span><span class="sxs-lookup"><span data-stu-id="04082-690">When you create an Empty ASP.NET Web Application, Visual Studio creates the following folder layout:</span></span>

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

<span data-ttu-id="04082-691">這是從舊版 ASP.NET 中，有一個例外狀況類似的空白網站配置。</span><span class="sxs-lookup"><span data-stu-id="04082-691">This is similar to the Empty Web Site layout from earlier versions of ASP.NET, with one exception.</span></span> <span data-ttu-id="04082-692">在 Visual Studio 2010 中，空的 Web 應用程式和空白網站專案都包含下列最小`Web.config`檔案，其中包含 Visual Studio 用來識別專案目標 framework 的資訊：</span><span class="sxs-lookup"><span data-stu-id="04082-692">In Visual Studio 2010, Empty Web Application and Empty Web Site projects contain the following minimal `Web.config` file that contains information used by Visual Studio to identify the framework that the project is targeting:</span></span>

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

<span data-ttu-id="04082-693">如果沒有這麼做*targetFramework*屬性，若要開啟較舊的應用程式時，保留相容性目標.NET Framework 2.0 的 Visual Studio 預設值。</span><span class="sxs-lookup"><span data-stu-id="04082-693">Without this *targetFramework* property, Visual Studio defaults to targeting the .NET Framework 2.0 in order to preserve compatibility when opening older applications.</span></span>

#### <a name="web-application-and-web-site-project-templates"></a><span data-ttu-id="04082-694">Web 應用程式和網站專案範本</span><span class="sxs-lookup"><span data-stu-id="04082-694">Web Application and Web Site Project Templates</span></span>

<span data-ttu-id="04082-695">其他兩個新專案範本所隨附的 Visual Studio 2010 包含重大變更。</span><span class="sxs-lookup"><span data-stu-id="04082-695">The other two new project templates that are shipped with Visual Studio 2010 contain major changes.</span></span> <span data-ttu-id="04082-696">下圖顯示當您建立新的 Web 應用程式專案建立專案配置。</span><span class="sxs-lookup"><span data-stu-id="04082-696">The following figure shows the project layout that is created when you create a new Web Application project.</span></span> <span data-ttu-id="04082-697">（網站專案的版面配置是幾乎完全相同）。</span><span class="sxs-lookup"><span data-stu-id="04082-697">(The layout for a Web Site project is virtually identical.)</span></span>

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

<span data-ttu-id="04082-698">專案包含不建立在舊版本中的檔案數目。</span><span class="sxs-lookup"><span data-stu-id="04082-698">The project includes a number of files that were not created in earlier versions.</span></span> <span data-ttu-id="04082-699">此外，新的 Web 應用程式專案已使用 基本成員資格功能，可讓您快速地開始在保護新的應用程式的存取。</span><span class="sxs-lookup"><span data-stu-id="04082-699">In addition, the new Web Application project is configured with basic membership functionality, which lets you quickly get started in securing access to the new application.</span></span> <span data-ttu-id="04082-700">加入此項目，因為`Web.config`檔案為新的專案包含用來設定成員資格、 角色和設定檔的項目。</span><span class="sxs-lookup"><span data-stu-id="04082-700">Because of this inclusion, the `Web.config` file for the new project includes entries that are used to configure membership, roles, and profiles.</span></span> <span data-ttu-id="04082-701">下列範例所示`Web.config`新的 Web 應用程式專案檔。</span><span class="sxs-lookup"><span data-stu-id="04082-701">The following example shows the `Web.config` file for a new Web Application project.</span></span> <span data-ttu-id="04082-702">(在此情況下， *roleManager*已停用。)</span><span class="sxs-lookup"><span data-stu-id="04082-702">(In this case, *roleManager* is disabled.)</span></span>

[![](overview/_static/image13.png)](overview/_static/image12.png)

<span data-ttu-id="04082-703">([按一下以檢視完整大小的影像](overview/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="04082-703">([Click to view full-size image](overview/_static/image14.png))</span></span>

<span data-ttu-id="04082-704">專案也包含第二個`Web.config`檔案中`Account`目錄。</span><span class="sxs-lookup"><span data-stu-id="04082-704">The project also contains a second `Web.config` file in the `Account` directory.</span></span> <span data-ttu-id="04082-705">第二個組態檔可用來安全存取的 ChangePassword.aspx 頁面非登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="04082-705">The second configuration file provides a way to secure access to the ChangePassword.aspx page for non-logged in users.</span></span> <span data-ttu-id="04082-706">下列範例顯示的第二個內容`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-706">The following example shows the contents of the second `Web.config` file.</span></span>

![](overview/_static/image15.png)

<span data-ttu-id="04082-707">建立新的專案範本中預設的頁面也會包含比在舊版中的其他內容。</span><span class="sxs-lookup"><span data-stu-id="04082-707">The pages created by default in the new project templates also contain more content than in previous versions.</span></span> <span data-ttu-id="04082-708">專案包含預設主版頁面和 CSS 檔案，並預設網頁 (Default.aspx) 時，已設定為使用主版頁面上，依預設。</span><span class="sxs-lookup"><span data-stu-id="04082-708">The project contains a default master page and CSS file, and the default page (Default.aspx) is configured to use the master page by default.</span></span> <span data-ttu-id="04082-709">結果是，當您第一次執行 Web 應用程式或網站上，預設 （首頁） 網頁已經功能。</span><span class="sxs-lookup"><span data-stu-id="04082-709">The result is that when you run the Web application or Web site for the first time, the default (home) page is already functional.</span></span> <span data-ttu-id="04082-710">事實上，它是類似於您啟動新的 MVC 應用程式時，您會看到預設頁面。</span><span class="sxs-lookup"><span data-stu-id="04082-710">In fact, it is similar to the default page you see when you start up a new MVC application.</span></span>

[![](overview/_static/image17.png)](overview/_static/image16.png)

<span data-ttu-id="04082-711">([按一下以檢視完整大小的影像](overview/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="04082-711">([Click to view full-size image](overview/_static/image18.png))</span></span>

<span data-ttu-id="04082-712">這些變更的專案範本的目的是要提供有關如何開始建置新的 Web 應用程式的指引。</span><span class="sxs-lookup"><span data-stu-id="04082-712">The intention of these changes to the project templates is to provide guidance on how to start building a new Web application.</span></span> <span data-ttu-id="04082-713">語意不正確、 嚴格的 XHTML 1.0 相容標記與指定使用 CSS 的版面配置，將範本中的頁面會代表建置 ASP.NET 4 Web 應用程式的最佳作法。</span><span class="sxs-lookup"><span data-stu-id="04082-713">With semantically correct, strict XHTML 1.0-compliant markup and with layout that is specified using CSS, the pages in the templates represent best practices for building ASP.NET 4 Web applications.</span></span> <span data-ttu-id="04082-714">預設頁面也會有兩欄版面配置，您可以輕鬆地自訂。</span><span class="sxs-lookup"><span data-stu-id="04082-714">The default pages also have a two-column layout that you can easily customize.</span></span>

<span data-ttu-id="04082-715">例如，想像一下，新的 Web 應用程式想要變更的某些色彩，並插入您的公司標誌來取代我的 ASP.NET 應用程式標誌。</span><span class="sxs-lookup"><span data-stu-id="04082-715">For example, imagine that for a new Web Application you want to change some of the colors and insert your company logo in place of the My ASP.NET Application logo.</span></span> <span data-ttu-id="04082-716">若要這樣做，您會建立新的目錄下`Content`來儲存您的標誌影像：</span><span class="sxs-lookup"><span data-stu-id="04082-716">To do this, you create a new directory under `Content` to store your logo image:</span></span>

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

<span data-ttu-id="04082-717">若要將影像加入至頁面中，您再開啟`Site.Master`檔案中，尋找 我的 ASP.NET 應用程式文字的已定義，其中，並將它取代為*映像*項目其*src*屬性設定為新的標誌映像，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-717">To add the image to the page, you then open the `Site.Master` file, find where the My ASP.NET Application text is defined, and replace it with an *image* element whose *src* attribute is set to the new logo image, as in the following example:</span></span>

[![](overview/_static/image21.png)](overview/_static/image20.png)

<span data-ttu-id="04082-718">([按一下以檢視完整大小的影像](overview/_static/image22.png))</span><span class="sxs-lookup"><span data-stu-id="04082-718">([Click to view full-size image](overview/_static/image22.png))</span></span>

<span data-ttu-id="04082-719">然後，您可以進入 Site.css 檔案，並修改 CSS 類別定義，以變更頁面背景色彩，以及的標頭。</span><span class="sxs-lookup"><span data-stu-id="04082-719">You can then go into the Site.css file and modify CSS class definitions to change the background color of the page as well as that of the header.</span></span>

<span data-ttu-id="04082-720">這些變更的結果是，您可以顯示自訂的首頁上，使用少量的工作：</span><span class="sxs-lookup"><span data-stu-id="04082-720">The result of these changes is that you can display a customized home page with very little effort:</span></span>

[![](overview/_static/image24.png)](overview/_static/image23.png)

<span data-ttu-id="04082-721">([按一下以檢視完整大小的影像](overview/_static/image25.png))</span><span class="sxs-lookup"><span data-stu-id="04082-721">([Click to view full-size image](overview/_static/image25.png))</span></span>

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a><span data-ttu-id="04082-722">CSS 增強功能</span><span class="sxs-lookup"><span data-stu-id="04082-722">CSS Improvements</span></span>

<span data-ttu-id="04082-723">ASP.NET 4 中重點之一就是工作的協助呈現 HTML 相容的最新的 HTML 標準。</span><span class="sxs-lookup"><span data-stu-id="04082-723">One of the major areas of work in ASP.NET 4 has been to help render HTML that is compliant with the latest HTML standards.</span></span> <span data-ttu-id="04082-724">這包括 ASP.NET Web 伺服器控制項的 CSS 樣式的使用方式的變更。</span><span class="sxs-lookup"><span data-stu-id="04082-724">This includes changes to how ASP.NET Web server controls use CSS styles.</span></span>

#### <a name="compatibility-setting-for-rendering"></a><span data-ttu-id="04082-725">轉譯的相容性設定</span><span class="sxs-lookup"><span data-stu-id="04082-725">Compatibility Setting for Rendering</span></span>

<span data-ttu-id="04082-726">根據預設，Web 應用程式或 Web 站台為目標的.NET Framework 4 中，當*controlRenderingCompatibilityVersion*屬性*頁*元素設定為"4.0"。</span><span class="sxs-lookup"><span data-stu-id="04082-726">By default, when a Web application or Web site targets the .NET Framework 4, the *controlRenderingCompatibilityVersion* attribute of the *pages* element is set to "4.0".</span></span> <span data-ttu-id="04082-727">此項目會定義在電腦層級`Web.config`檔案，並根據預設會套用到所有的 ASP.NET 4 應用程式：</span><span class="sxs-lookup"><span data-stu-id="04082-727">This element is defined in the machine-level `Web.config` file and by default applies to all ASP.NET 4 applications:</span></span>

[!code-xml[Main](overview/samples/sample69.xml)]

<span data-ttu-id="04082-728">值*controlRenderingCompatibility*是一個字串，可讓潛在的新版本定義在未來的版本。</span><span class="sxs-lookup"><span data-stu-id="04082-728">The value for *controlRenderingCompatibility* is a string, which allows potential new version definitions in future releases.</span></span> <span data-ttu-id="04082-729">在目前的版本中，此屬性支援下列值：</span><span class="sxs-lookup"><span data-stu-id="04082-729">In the current release, the following values are supported for this property:</span></span>

- <span data-ttu-id="04082-730">"3.5".</span><span class="sxs-lookup"><span data-stu-id="04082-730">"3.5".</span></span> <span data-ttu-id="04082-731">這個設定表示舊版轉譯和標記。</span><span class="sxs-lookup"><span data-stu-id="04082-731">This setting indicates legacy rendering and markup.</span></span> <span data-ttu-id="04082-732">控制項所呈現的標記是 100%相容，以及設定*xhtmlConformance*屬性會接受。</span><span class="sxs-lookup"><span data-stu-id="04082-732">Markup rendered by controls is 100% backward compatible, and the setting of the *xhtmlConformance* property is honored.</span></span>
- <span data-ttu-id="04082-733">"4.0".</span><span class="sxs-lookup"><span data-stu-id="04082-733">"4.0".</span></span> <span data-ttu-id="04082-734">如果屬性具有這項設定，ASP.NET Web 伺服器控制項執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="04082-734">If the property has this setting, ASP.NET Web server controls do the following:</span></span>
- <span data-ttu-id="04082-735">*XhtmlConformance*屬性一律會視為"Strict"。</span><span class="sxs-lookup"><span data-stu-id="04082-735">The *xhtmlConformance* property is always treated as "Strict".</span></span> <span data-ttu-id="04082-736">如此一來，控制項呈現的 XHTML 1.0 Strict 的標記。</span><span class="sxs-lookup"><span data-stu-id="04082-736">As a result, controls render XHTML 1.0 Strict markup.</span></span>
- <span data-ttu-id="04082-737">停用非輸入控制項不再轉譯不正確的樣式。</span><span class="sxs-lookup"><span data-stu-id="04082-737">Disabling non-input controls no longer renders invalid styles.</span></span>
- <span data-ttu-id="04082-738">*div*隱藏的欄位前後的項目現在的樣式，因此它們不會干擾使用者建立 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="04082-738">*div* elements around hidden fields are now styled so they do not interfere with user-created CSS rules.</span></span>
- <span data-ttu-id="04082-739">功能表控制項呈現語意正確且符合協助工具指導方針的標記。</span><span class="sxs-lookup"><span data-stu-id="04082-739">Menu controls render markup that is semantically correct and compliant with accessibility guidelines.</span></span>
- <span data-ttu-id="04082-740">驗證控制項不會轉譯內嵌樣式。</span><span class="sxs-lookup"><span data-stu-id="04082-740">Validation controls do not render inline styles.</span></span>
- <span data-ttu-id="04082-741">控制先前呈現的框線 ="0"(衍生自 ASP.NET 控制項*表格*控制項，以及 ASP.NET*映像*控制項) 不再呈現此屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-741">Controls that previously rendered border="0" (controls that derive from the ASP.NET *Table* control, and the ASP.NET *Image* control) no longer render this attribute.</span></span>

#### <a name="disabling-controls"></a><span data-ttu-id="04082-742">停用控制項</span><span class="sxs-lookup"><span data-stu-id="04082-742">Disabling Controls</span></span>

<span data-ttu-id="04082-743">在 ASP.NET 3.5 SP1 和舊版中，架構會呈現*停用*屬性 (attribute) 的 HTML 標記中任何控制其*已啟用*屬性設定為*false*。</span><span class="sxs-lookup"><span data-stu-id="04082-743">In ASP.NET 3.5 SP1 and earlier versions, the framework renders the *disabled* attribute in the HTML markup for any control whose *Enabled* property set to *false*.</span></span> <span data-ttu-id="04082-744">不過，根據 HTML 4.01 規格，只有*輸入*項目應具有此屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-744">However, according to the HTML 4.01 specification, only *input* elements should have this attribute.</span></span>

<span data-ttu-id="04082-745">在 ASP.NET 4 中，您可以設定*controlRenderingCompatabilityVersion* "3.5"，如下列範例所示的屬性：</span><span class="sxs-lookup"><span data-stu-id="04082-745">In ASP.NET 4, you can set the *controlRenderingCompatabilityVersion* property to "3.5", as in the following example:</span></span>

[!code-xml[Main](overview/samples/sample70.xml)]

<span data-ttu-id="04082-746">您可以建立標記*標籤*如下所示，它會停用控制項的控制項：</span><span class="sxs-lookup"><span data-stu-id="04082-746">You might create markup for a *Label* control like the following, which disables the control:</span></span>

[!code-aspx[Main](overview/samples/sample71.aspx)]

<span data-ttu-id="04082-747">*標籤*控制項會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="04082-747">The *Label* control would render the following HTML:</span></span>

[!code-html[Main](overview/samples/sample72.html)]

<span data-ttu-id="04082-748">在 ASP.NET 4 中，您可以設定*controlRenderingCompatabilityVersion*設為"4.0"。</span><span class="sxs-lookup"><span data-stu-id="04082-748">In ASP.NET 4, you can set the *controlRenderingCompatabilityVersion* to "4.0".</span></span> <span data-ttu-id="04082-749">在此情況下，只會控制轉譯的*輸入*項目會呈現*停用*屬性時的控制項*已啟用*屬性設定為*false*.</span><span class="sxs-lookup"><span data-stu-id="04082-749">In that case, only controls that render *input* elements will render a *disabled* attribute when the control's *Enabled* property is set to *false*.</span></span> <span data-ttu-id="04082-750">不會轉譯 HTML 的控制項*輸入*項目改為轉譯*類別*參考可用來定義了停用控制項的 CSS 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-750">Controls that do not render HTML *input* elements instead render a *class* attribute that references a CSS class that you can use to define a disabled look for the control.</span></span> <span data-ttu-id="04082-751">例如，*標籤*先前範例所示的控制項就會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="04082-751">For example, the *Label* control shown in the earlier example would generate the following markup:</span></span>

[!code-html[Main](overview/samples/sample73.html)]

<span data-ttu-id="04082-752">這個控制項指定類別的預設值為"aspNetDisabled"。</span><span class="sxs-lookup"><span data-stu-id="04082-752">The default value for the class that specified for this control is "aspNetDisabled".</span></span> <span data-ttu-id="04082-753">不過，您可以變更此預設值，藉由設定靜態*DisabledCssClass*靜態屬性*WebControl*類別。</span><span class="sxs-lookup"><span data-stu-id="04082-753">However, you can change this default value by setting the static *DisabledCssClass* static property of the *WebControl* class.</span></span> <span data-ttu-id="04082-754">對控制項開發人員，要使用特定控制項的行為也可以定義使用*SupportsDisabledAttribute*屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-754">For control developers, the behavior to use for a specific control can also be defined using the *SupportsDisabledAttribute* property.</span></span>

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a><span data-ttu-id="04082-755">隱藏 div 周圍隱藏欄位的項目</span><span class="sxs-lookup"><span data-stu-id="04082-755">Hiding div Elements Around Hidden Fields</span></span>

<span data-ttu-id="04082-756">ASP.NET 2.0 和更新版本的呈現系統特定隱藏的欄位 (例如*隱藏*用來儲存檢視狀態資訊的項目) 內*div*為了符合 XHTML 標準的項目。</span><span class="sxs-lookup"><span data-stu-id="04082-756">ASP.NET 2.0 and later versions render system-specific hidden fields (such as the *hidden* element used to store view state information) inside *div* element in order to comply with the XHTML standard.</span></span> <span data-ttu-id="04082-757">不過，這可能會造成問題時的 CSS 規則會影響*div*頁面上的項目。</span><span class="sxs-lookup"><span data-stu-id="04082-757">However, this can cause a problem when a CSS rule affects *div* elements on a page.</span></span> <span data-ttu-id="04082-758">例如，可能會導致頁面中出現一個像素列大約隱藏*div*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-758">For example, it can result in a one-pixel line appearing in the page around hidden *div* elements.</span></span> <span data-ttu-id="04082-759">在 ASP.NET 4 中， *div*括住 ASP.NET 產生的隱藏的欄位的項目新增 CSS 類別參考，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-759">In ASP.NET 4, *div* elements that enclose the hidden fields generated by ASP.NET add a CSS class reference as in the following example:</span></span>

[!code-html[Main](overview/samples/sample74.html)]

<span data-ttu-id="04082-760">然後，您可以定義 CSS 類別，僅適用於*隱藏*由 ASP.NET，產生如下列範例所示的項目：</span><span class="sxs-lookup"><span data-stu-id="04082-760">You can then define a CSS class that applies only to the *hidden* elements that are generated by ASP.NET, as in the following example:</span></span>

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a><span data-ttu-id="04082-761">樣板化控制項呈現的外部資料表</span><span class="sxs-lookup"><span data-stu-id="04082-761">Rendering an Outer Table for Templated Controls</span></span>

<span data-ttu-id="04082-762">根據預設，下列的 ASP.NET Web 伺服器控制項支援範本自動包裝在外部資料表，可用來套用內嵌樣式：</span><span class="sxs-lookup"><span data-stu-id="04082-762">By default, the following ASP.NET Web server controls that support templates are automatically wrapped in an outer table that is used to apply inline styles:</span></span>

- <span data-ttu-id="04082-763">*FormView*</span><span class="sxs-lookup"><span data-stu-id="04082-763">*FormView*</span></span>
- <span data-ttu-id="04082-764">*登入*</span><span class="sxs-lookup"><span data-stu-id="04082-764">*Login*</span></span>
- <span data-ttu-id="04082-765">*PasswordRecovery*</span><span class="sxs-lookup"><span data-stu-id="04082-765">*PasswordRecovery*</span></span>
- <span data-ttu-id="04082-766">*ChangePassword*</span><span class="sxs-lookup"><span data-stu-id="04082-766">*ChangePassword*</span></span>
- <span data-ttu-id="04082-767">*精靈*</span><span class="sxs-lookup"><span data-stu-id="04082-767">*Wizard*</span></span>
- <span data-ttu-id="04082-768">*CreateUserWizard*</span><span class="sxs-lookup"><span data-stu-id="04082-768">*CreateUserWizard*</span></span>

<span data-ttu-id="04082-769">名為的新屬性*RenderOuterTable*已新增至可讓外部資料表從標記中移除這些控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-769">A new property named *RenderOuterTable* has been added to these controls that allows the outer table to be removed from the markup.</span></span> <span data-ttu-id="04082-770">例如，請考慮下列的範例*FormView*控制項：</span><span class="sxs-lookup"><span data-stu-id="04082-770">For example, consider the following example of a *FormView* control:</span></span>

[!code-aspx[Main](overview/samples/sample76.aspx)]

<span data-ttu-id="04082-771">此標記會轉譯到頁面上，其中包含 HTML 表格的下列輸出：</span><span class="sxs-lookup"><span data-stu-id="04082-771">This markup renders the following output to the page, which includes an HTML table:</span></span>

[!code-html[Main](overview/samples/sample77.html)]

<span data-ttu-id="04082-772">若要防止資料表所呈現，您可以設定*FormView*控制項的*RenderOuterTable*屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-772">To prevent the table from being rendered, you can set the *FormView* control's *RenderOuterTable* property, as in the following example:</span></span>

[!code-aspx[Main](overview/samples/sample78.aspx)]

<span data-ttu-id="04082-773">前一個範例會呈現下列的輸出，無需*表格*， *tr*，並*td*項目：</span><span class="sxs-lookup"><span data-stu-id="04082-773">The previous example renders the following output, without the *table*, *tr*, and *td* elements:</span></span>

> <span data-ttu-id="04082-774">內容</span><span class="sxs-lookup"><span data-stu-id="04082-774">Content</span></span>


<span data-ttu-id="04082-775">這項增強功能可讓您更輕鬆地利用 CSS，控制項內容的樣式因為沒有非預期的標記控制項所呈現。</span><span class="sxs-lookup"><span data-stu-id="04082-775">This enhancement can make it easier to style the content of the control with CSS, because no unexpected tags are rendered by the control.</span></span>

> [!NOTE]
> <span data-ttu-id="04082-776">請注意這項變更會停用支援自動格式化函式，在 Visual Studio 2010 設計工具中，因為目前已不再*資料表*可以主控樣式屬性的自動格式化選項所產生的項目。</span><span class="sxs-lookup"><span data-stu-id="04082-776">Note This change disables support for the auto-format function in the Visual Studio 2010 designer, because there is no longer a *table* element that can host style attributes that are generated by the auto-format option.</span></span>


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a><span data-ttu-id="04082-777">ListView 控制項的增強功能</span><span class="sxs-lookup"><span data-stu-id="04082-777">ListView Control Enhancements</span></span>

<span data-ttu-id="04082-778">*ListView*控制項已在 ASP.NET 4 中使用的工作變得更容易。</span><span class="sxs-lookup"><span data-stu-id="04082-778">The *ListView* control has been made easier to use in ASP.NET 4.</span></span> <span data-ttu-id="04082-779">較早版本的控制項需要您指定包含具有已知識別碼的伺服器控制項的版面配置範本</span><span class="sxs-lookup"><span data-stu-id="04082-779">The earlier version of the control required that you specify a layout template that contained a server control with a known ID.</span></span> <span data-ttu-id="04082-780">下列標記示範如何使用的典型範例*ListView* ASP.NET 3.5 中的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-780">The following markup shows a typical example of how to use the *ListView* control in ASP.NET 3.5.</span></span>

[!code-aspx[Main](overview/samples/sample79.aspx)]

<span data-ttu-id="04082-781">在 ASP.NET 4 中， *ListView*控制項不需要的版面配置範本。</span><span class="sxs-lookup"><span data-stu-id="04082-781">In ASP.NET 4, the *ListView* control does not require a layout template.</span></span> <span data-ttu-id="04082-782">在上述範例中所顯示的標記，都可以使用下列標記取代：</span><span class="sxs-lookup"><span data-stu-id="04082-782">The markup shown in the previous example can be replaced with the following markup:</span></span>

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a><span data-ttu-id="04082-783">CheckBoxList 和 RadioButtonList 控制項的增強功能</span><span class="sxs-lookup"><span data-stu-id="04082-783">CheckBoxList and RadioButtonList Control Enhancements</span></span>

<span data-ttu-id="04082-784">在 ASP.NET 3.5 中，您可以指定版面配置*CheckBoxList*並*RadioButtonList*使用下列兩項設定：</span><span class="sxs-lookup"><span data-stu-id="04082-784">In ASP.NET 3.5, you can specify layout for the *CheckBoxList* and *RadioButtonList* using the following two settings:</span></span>

- <span data-ttu-id="04082-785">*流程*。</span><span class="sxs-lookup"><span data-stu-id="04082-785">*Flow*.</span></span> <span data-ttu-id="04082-786">控制項呈現*跨越*包含其內容的項目。</span><span class="sxs-lookup"><span data-stu-id="04082-786">The control renders *span* elements to contain its content.</span></span>
- <span data-ttu-id="04082-787">*資料表*。</span><span class="sxs-lookup"><span data-stu-id="04082-787">*Table*.</span></span> <span data-ttu-id="04082-788">控制項呈現*資料表*項目來包含其內容。</span><span class="sxs-lookup"><span data-stu-id="04082-788">The control renders a *table* element to contain its content.</span></span>

<span data-ttu-id="04082-789">下列範例會顯示這些控制項的每個標記。</span><span class="sxs-lookup"><span data-stu-id="04082-789">The following example shows markup for each of these controls.</span></span>

[!code-aspx[Main](overview/samples/sample81.aspx)]

<span data-ttu-id="04082-790">根據預設，控制項會呈現 HTML 如下所示：</span><span class="sxs-lookup"><span data-stu-id="04082-790">By default, the controls render HTML similar to the following:</span></span>

[!code-html[Main](overview/samples/sample82.html)]

<span data-ttu-id="04082-791">因為這些控制項包含的項目清單，來呈現語意正確的 HTML，其應該會轉譯它們使用 HTML 清單的內容 (*li*) 項目。</span><span class="sxs-lookup"><span data-stu-id="04082-791">Because these controls contain lists of items, to render semantically correct HTML, they should render their contents using HTML list (*li*) elements.</span></span> <span data-ttu-id="04082-792">這可讓您更輕鬆讀取網頁使用輔助技術，並讓您更輕鬆地使用 CSS 樣式控制項的使用者。</span><span class="sxs-lookup"><span data-stu-id="04082-792">This makes it easier for users who read Web pages using assistive technology, and makes the controls easier to style using CSS.</span></span>

<span data-ttu-id="04082-793">在 ASP.NET 4 中， *CheckBoxList*並*RadioButtonList*控制項支援下列新值*RepeatLayout*屬性：</span><span class="sxs-lookup"><span data-stu-id="04082-793">In ASP.NET 4, the *CheckBoxList* and *RadioButtonList* controls support the following new values for the *RepeatLayout* property:</span></span>

- <span data-ttu-id="04082-794">*OrderedList* – 內容會轉譯成*li*內的項目*ol*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-794">*OrderedList* – The content is rendered as *li* elements within an *ol* element.</span></span>
- <span data-ttu-id="04082-795">*UnorderedList* – 內容會轉譯成*li*內的項目*ul*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-795">*UnorderedList* – The content is rendered as *li* elements within a *ul* element.</span></span>

<span data-ttu-id="04082-796">下列範例示範如何使用這些新的值。</span><span class="sxs-lookup"><span data-stu-id="04082-796">The following example shows how to use these new values.</span></span>

[!code-aspx[Main](overview/samples/sample83.aspx)]

<span data-ttu-id="04082-797">上述標記會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="04082-797">The preceding markup generates the following HTML:</span></span>

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> <span data-ttu-id="04082-798">請注意是否您設定*RepeatLayout*要*OrderedList*或*UnorderedList*，則*Flow*屬性無法再使用，並將如果屬性已設定您的標記或程式碼中，會擲回執行階段例外狀況。</span><span class="sxs-lookup"><span data-stu-id="04082-798">Note If you set *RepeatLayout* to *OrderedList* or *UnorderedList*, the *RepeatDirection* property can no longer be used and will throw an exception at run time if the property has been set within your markup or code.</span></span> <span data-ttu-id="04082-799">屬性會有任何值，因為這些控制項的視覺化版面配置改為使用 CSS 定義。</span><span class="sxs-lookup"><span data-stu-id="04082-799">The property would have no value because the visual layout of these controls is defined using CSS instead.</span></span>


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a><span data-ttu-id="04082-800">功能表控制項的改善</span><span class="sxs-lookup"><span data-stu-id="04082-800">Menu Control Improvements</span></span>

<span data-ttu-id="04082-801">ASP.NET 4 以前* 功能表*呈現一系列 HTML 表格的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-801">Before ASP.NET 4, the *Menu* control rendered a series of HTML tables.</span></span> <span data-ttu-id="04082-802">這變得更難套用 CSS 樣式以外設定內嵌屬性，且也不符合協助工具標準。</span><span class="sxs-lookup"><span data-stu-id="04082-802">This made it more difficult to apply CSS styles outside of setting inline properties and was also not compliant with accessibility standards.</span></span>

<span data-ttu-id="04082-803">在 ASP.NET 4 中，控制項現在使用的未排序的清單和清單項目所組成的語意標記的 HTML 呈現。</span><span class="sxs-lookup"><span data-stu-id="04082-803">In ASP.NET 4, the control now renders HTML using semantic markup that consists of an unordered list and list elements.</span></span> <span data-ttu-id="04082-804">下列範例示範針對 ASP.NET 網頁中的標記* 功能表*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-804">The following example shows markup in an ASP.NET page for the *Menu* control.</span></span>

[!code-aspx[Main](overview/samples/sample85.aspx)]

<span data-ttu-id="04082-805">當頁面呈現時，控制項就會產生下列 HTML ( *onclick*為了清楚起見已省略的程式碼):</span><span class="sxs-lookup"><span data-stu-id="04082-805">When the page renders, the control produces the following HTML (the *onclick* code has been omitted for clarity):</span></span>

[!code-html[Main](overview/samples/sample86.html)]

<span data-ttu-id="04082-806">除了轉譯改進鍵盤瀏覽功能表的已改善使用焦點管理。</span><span class="sxs-lookup"><span data-stu-id="04082-806">In addition to rendering improvements, keyboard navigation of the menu has been improved using focus management.</span></span> <span data-ttu-id="04082-807">當* 功能表*控制項取得焦點時，您可以使用方向鍵來瀏覽項目。</span><span class="sxs-lookup"><span data-stu-id="04082-807">When the *Menu* control gets focus, you can use the arrow keys to navigate elements.</span></span> <span data-ttu-id="04082-808">* 功能表*控制項現在也將可存取豐富的網際網路應用程式 (ARIA) 角色和屬性下列[wing](http://www.w3.org/TR/wai-aria-practices/#menu "功能表 ARIA 指導方針")改善存取範圍。</span><span class="sxs-lookup"><span data-stu-id="04082-808">The *Menu* control also now attaches accessible rich internet applications (ARIA) roles and attributes follo[wing the](http://www.w3.org/TR/wai-aria-practices/#menu "Menu ARIA guidelines")for improved accessibility.</span></span>

<span data-ttu-id="04082-809">功能表控制項的樣式會呈現的樣式區塊頂端的頁面上，而不是依照呈現的 HTML 項目中。</span><span class="sxs-lookup"><span data-stu-id="04082-809">Styles for the menu control are rendered in a style block at the top of the page, rather than in line with the rendered HTML elements.</span></span> <span data-ttu-id="04082-810">如果您想要全權掌控控制項樣式時，您可以設定的新*IncludeStyleBlock*屬性設*false*，在此情況下不會發出樣式區塊。</span><span class="sxs-lookup"><span data-stu-id="04082-810">If you want to take full control over the styling for the control, you can set the new *IncludeStyleBlock* property to *false*, in which case the style block is not emitted.</span></span> <span data-ttu-id="04082-811">使用這個屬性的一個方式是使用 Visual Studio 設計工具中的自動格式化功能，來設定功能表的外觀。</span><span class="sxs-lookup"><span data-stu-id="04082-811">One way to use this property is to use the auto-format feature in the Visual Studio designer to set the appearance of the menu.</span></span> <span data-ttu-id="04082-812">您可以執行的頁面、 開啟網頁原始檔，並接著將呈現的樣式區塊複製到外部 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-812">You can then run the page, open the page source, and then copy the rendered style block to an external CSS file.</span></span> <span data-ttu-id="04082-813">在 Visual Studio 中，復原樣式設定和設定*IncludeStyleBlock*要*false*。</span><span class="sxs-lookup"><span data-stu-id="04082-813">In Visual Studio, undo the styling and set *IncludeStyleBlock* to *false*.</span></span> <span data-ttu-id="04082-814">結果是功能表外觀，定義在外部樣式表中使用樣式。</span><span class="sxs-lookup"><span data-stu-id="04082-814">The result is that the menu appearance is defined using styles in an external style sheet.</span></span>

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a><span data-ttu-id="04082-815">精靈和 CreateUserWizard 控制項</span><span class="sxs-lookup"><span data-stu-id="04082-815">Wizard and CreateUserWizard Controls</span></span>

<span data-ttu-id="04082-816">ASP.NET*精靈*並*CreateUserWizard*控制項都支援可讓您定義其轉譯 HTML 的範本。</span><span class="sxs-lookup"><span data-stu-id="04082-816">The ASP.NET *Wizard* and *CreateUserWizard* controls support templates that let you define the HTML that they render.</span></span> <span data-ttu-id="04082-817">(*CreateUserWizard*衍生自*精靈*。)下列範例顯示完整的樣板化的標記*CreateUserWizard*控制項：</span><span class="sxs-lookup"><span data-stu-id="04082-817">(*CreateUserWizard* derives from *Wizard*.) The following example shows the markup for a fully templated *CreateUserWizard* control:</span></span>

[!code-aspx[Main](overview/samples/sample87.aspx)]

<span data-ttu-id="04082-818">控制項會呈現 HTML 如下所示：</span><span class="sxs-lookup"><span data-stu-id="04082-818">The control renders HTML similar to the following:</span></span>

[!code-html[Main](overview/samples/sample88.html)]

<span data-ttu-id="04082-819">在 ASP.NET 3.5 SP1 中，雖然您可以變更範本內容中，您仍有受限制的控制權的輸出*精靈*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-819">In ASP.NET 3.5 SP1, although you can change the template contents, you still have limited control over the output of the *Wizard* control.</span></span> <span data-ttu-id="04082-820">在 ASP.NET 4 中，您可以建立*LayoutTemplate*範本和 insert*預留位置*控制項 （使用保留的名稱），指定您要如何*精靈控制項*來呈現。</span><span class="sxs-lookup"><span data-stu-id="04082-820">In ASP.NET 4, you can create a *LayoutTemplate* template and insert *PlaceHolder* controls (using reserved names) to specify how you want the *Wizard control* to render.</span></span> <span data-ttu-id="04082-821">下列範例會示範這個：</span><span class="sxs-lookup"><span data-stu-id="04082-821">The following example shows this:</span></span>

[!code-aspx[Main](overview/samples/sample89.aspx)]

<span data-ttu-id="04082-822">此範例包含下列名為中的預留位置*LayoutTemplate*項目：</span><span class="sxs-lookup"><span data-stu-id="04082-822">The example contains the following named placeholders in the *LayoutTemplate* element:</span></span>

- <span data-ttu-id="04082-823">*headerPlaceholder* – 在執行階段，將會取代運算式的內容*HeaderTemplate*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-823">*headerPlaceholder* – At run time, this is replaced by the contents of the *HeaderTemplate* element.</span></span>
- <span data-ttu-id="04082-824">*sideBarPlaceholder* – 在執行階段，將會取代運算式的內容*SideBarTemplate*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-824">*sideBarPlaceholder* – At run time, this is replaced by the contents of the *SideBarTemplate* element.</span></span>
- <span data-ttu-id="04082-825">*wizardStepPlaceHolder* – 在執行階段，將會取代運算式的內容*WizardStepTemplate*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-825">*wizardStepPlaceHolder* – At run time, this is replaced by the contents of the *WizardStepTemplate* element.</span></span>
- <span data-ttu-id="04082-826">*navigationPlaceholder* – 在執行階段，這會取代您已定義的任何瀏覽範本。</span><span class="sxs-lookup"><span data-stu-id="04082-826">*navigationPlaceholder* – At run time, this is replaced by any navigation templates that you have defined.</span></span>

<span data-ttu-id="04082-827">在範例中，會使用預留位置的標記會轉譯下列 HTML （而不需要在範本中實際定義的內容）：</span><span class="sxs-lookup"><span data-stu-id="04082-827">The markup in the example that uses placeholders renders the following HTML (without the content actually defined in the templates):</span></span>

[!code-html[Main](overview/samples/sample90.html)]

<span data-ttu-id="04082-828">只是現在沒有使用者定義的 html*跨越*項目。</span><span class="sxs-lookup"><span data-stu-id="04082-828">The only HTML that is now not user-defined is a *span* element.</span></span> <span data-ttu-id="04082-829">(我們預期這在未來版本中，甚至*跨越*項目不會呈現出來。)這可讓您完整控制所產生的幾乎所有內容*精靈*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-829">(We anticipate that in future releases, even the *span* element will not be rendered.) This now gives you full control over virtually all the content that is generated by the *Wizard* control.</span></span>

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a><span data-ttu-id="04082-830">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="04082-830">ASP.NET MVC</span></span>

<span data-ttu-id="04082-831">ASP.NET MVC 是在 2009 年 3 月，做為附加元件架構引進到 ASP.NET 3.5 SP1。</span><span class="sxs-lookup"><span data-stu-id="04082-831">ASP.NET MVC was introduced as an add-on framework to ASP.NET 3.5 SP1 in March 2009.</span></span> <span data-ttu-id="04082-832">Visual Studio 2010 包含 ASP.NET MVC 2，其中包含新特色與功能。</span><span class="sxs-lookup"><span data-stu-id="04082-832">Visual Studio 2010 includes ASP.NET MVC 2, which includes new features and capabilities.</span></span>

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a><span data-ttu-id="04082-833">區域支援</span><span class="sxs-lookup"><span data-stu-id="04082-833">Areas Support</span></span>

<span data-ttu-id="04082-834">區域可讓您群組控制器和檢視表到大型應用程式相對的獨立於其他區段的區段。</span><span class="sxs-lookup"><span data-stu-id="04082-834">Areas let you group controllers and views into sections of a large application in relative isolation from other sections.</span></span> <span data-ttu-id="04082-835">每個區域可以實作為個別的 ASP.NET MVC 專案，然後主應用程式參考。</span><span class="sxs-lookup"><span data-stu-id="04082-835">Each area can be implemented as a separate ASP.NET MVC project that can then be referenced by the main application.</span></span> <span data-ttu-id="04082-836">這有助於管理複雜度，當您建置大型應用程式，並讓多個小組一起工作，在單一應用程式更容易。</span><span class="sxs-lookup"><span data-stu-id="04082-836">This helps manage complexity when you build a large application and makes it easier for multiple teams to work together on a single application.</span></span>

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a><span data-ttu-id="04082-837">資料註解屬性驗證支援</span><span class="sxs-lookup"><span data-stu-id="04082-837">Data-Annotation Attribute Validation Support</span></span>

<span data-ttu-id="04082-838">*DataAnnotations*屬性可以讓您附加至模型的驗證邏輯使用中繼資料屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-838">*DataAnnotations* attributes let you attach validation logic to a model by using metadata attributes.</span></span> <span data-ttu-id="04082-839">*DataAnnotations* ASP.NET 3.5 SP1 中的 ASP.NET Dynamic Data 中導入的屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-839">*DataAnnotations* attributes were introduced in ASP.NET Dynamic Data in ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="04082-840">這些屬性已整合到預設模型繫結器，並提供中繼資料導向的方式，來驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="04082-840">These attributes have been integrated into the default model binder and provide a metadata-driven means to validate user input.</span></span>

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a><span data-ttu-id="04082-841">樣板化 Helper</span><span class="sxs-lookup"><span data-stu-id="04082-841">Templated Helpers</span></span>

<span data-ttu-id="04082-842">樣板化 helper 可讓您自動產生關聯的編輯，並顯示與資料類型的範本。</span><span class="sxs-lookup"><span data-stu-id="04082-842">Templated helpers let you automatically associate edit and display templates with data types.</span></span> <span data-ttu-id="04082-843">比方說，您可以使用範本協助程式，來指定日期選擇器 UI 項目，如自動呈現*System.DateTime*值。</span><span class="sxs-lookup"><span data-stu-id="04082-843">For example, you can use a template helper to specify that a date-picker UI element is automatically rendered for a *System.DateTime* value.</span></span> <span data-ttu-id="04082-844">這是類似於 ASP.NET Dynamic Data 中的欄位範本。</span><span class="sxs-lookup"><span data-stu-id="04082-844">This is similar to field templates in ASP.NET Dynamic Data.</span></span>

<span data-ttu-id="04082-845">*Html.EditorFor*並*Html.DisplayFor* helper 方法轉譯標準的資料類型以及複雜的物件，具有多個屬性，具有內建支援。</span><span class="sxs-lookup"><span data-stu-id="04082-845">The *Html.EditorFor* and *Html.DisplayFor* helper methods have built-in support for rendering standard data types as well as complex objects with multiple properties.</span></span> <span data-ttu-id="04082-846">它們也可讓您套用資料註解屬性，例如自訂轉譯*DisplayName*並*ScaffoldColumn*來*ViewModel*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-846">They also customize rendering by letting you apply data-annotation attributes like *DisplayName* and *ScaffoldColumn* to the *ViewModel* object.</span></span>

<span data-ttu-id="04082-847">通常您會想要自訂 UI 協助程式更進一步的輸出，並擁有完整控制權產生的內容。</span><span class="sxs-lookup"><span data-stu-id="04082-847">Often you want to customize the output from UI helpers even further and have complete control over what is generated.</span></span> <span data-ttu-id="04082-848">*Html.EditorFor*並*Html.DisplayFor* helper 方法支援此使用樣板化機制，可讓您定義可以覆寫的外部範本和控制項轉譯的輸出。</span><span class="sxs-lookup"><span data-stu-id="04082-848">The *Html.EditorFor* and *Html.DisplayFor* helper methods support this using a templating mechanism that lets you define external templates that can override and control the output rendered.</span></span> <span data-ttu-id="04082-849">範本可以個別轉譯的類別。</span><span class="sxs-lookup"><span data-stu-id="04082-849">The templates can be rendered individually for a class.</span></span>

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a><span data-ttu-id="04082-850">Dynamic Data</span><span class="sxs-lookup"><span data-stu-id="04082-850">Dynamic Data</span></span>

<span data-ttu-id="04082-851">動態資料引進在 2008 年發行的.NET Framework 3.5 SP1。</span><span class="sxs-lookup"><span data-stu-id="04082-851">Dynamic Data was introduced in the .NET Framework 3.5 SP1 release in mid-2008.</span></span> <span data-ttu-id="04082-852">這項功能提供了許多增強功能，來建立資料驅動應用程式，包括下列：</span><span class="sxs-lookup"><span data-stu-id="04082-852">This feature provides many enhancements for creating data-driven applications, including the following:</span></span>

- <span data-ttu-id="04082-853">快速建置資料導向網站 RAD 體驗。</span><span class="sxs-lookup"><span data-stu-id="04082-853">A RAD experience for quickly building a data-driven Web site.</span></span>
- <span data-ttu-id="04082-854">資料模型中定義的條件約束為基礎的自動驗證。</span><span class="sxs-lookup"><span data-stu-id="04082-854">Automatic validation that is based on constraints defined in the data model.</span></span>
- <span data-ttu-id="04082-855">能夠輕鬆地變更針對欄位中所產生的標記*GridView*並*DetailsView*使用動態資料專案一部分的欄位範本的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-855">The ability to easily change the markup that is generated for fields in the *GridView* and *DetailsView* controls by using field templates that are part of your Dynamic Data project.</span></span>

> [!NOTE]
> <span data-ttu-id="04082-856">請注意，如需詳細資訊，請參閱 <<c0> [ 動態資料文件](https://msdn.microsoft.com/library/cc488545.aspx)MSDN Library 中。</span><span class="sxs-lookup"><span data-stu-id="04082-856">Note For more information, see the [Dynamic Data documentation](https://msdn.microsoft.com/library/cc488545.aspx) in the MSDN Library.</span></span>


<span data-ttu-id="04082-857">針對 ASP.NET 4 中，動態資料已經過增強，讓開發人員快速建置資料導向網站的更強大。</span><span class="sxs-lookup"><span data-stu-id="04082-857">For ASP.NET 4, Dynamic Data has been enhanced to give developers even more power for quickly building data-driven Web sites.</span></span>

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a><span data-ttu-id="04082-858">對於現有的專案中啟用動態資料</span><span class="sxs-lookup"><span data-stu-id="04082-858">Enabling Dynamic Data for Existing Projects</span></span>

<span data-ttu-id="04082-859">隨附於.NET Framework 3.5 SP1 的動態資料功能帶入新的功能，如下所示：</span><span class="sxs-lookup"><span data-stu-id="04082-859">Dynamic Data features that shipped in the .NET Framework 3.5 SP1 brought new features such as the following:</span></span>

- <span data-ttu-id="04082-860">欄位樣板 – 這些會提供資料類型為基礎的範本資料繫結控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-860">Field templates – These provide data-type-based templates for data-bound controls.</span></span> <span data-ttu-id="04082-861">欄位範本會提供更簡單的方式，來自訂比使用範本欄位的每個欄位的資料控制項的外觀。</span><span class="sxs-lookup"><span data-stu-id="04082-861">Field templates provide a simpler way to customize the look of data controls than using template fields for each field.</span></span>
- <span data-ttu-id="04082-862">驗證-動態資料可讓您在資料類別上使用屬性來指定必要的欄位、 範圍檢查、 型別檢查、 使用規則運算式比對的模式的常見案例的驗證和自訂驗證。</span><span class="sxs-lookup"><span data-stu-id="04082-862">Validation – Dynamic Data lets you use attributes on data classes to specify validation for common scenarios like required fields, range checking, type checking, pattern matching using regular expressions, and custom validation.</span></span> <span data-ttu-id="04082-863">資料控制項，強制執行驗證。</span><span class="sxs-lookup"><span data-stu-id="04082-863">Validation is enforced by the data controls.</span></span>

<span data-ttu-id="04082-864">不過，這些功能有下列需求：</span><span class="sxs-lookup"><span data-stu-id="04082-864">However, these features had the following requirements:</span></span>

- <span data-ttu-id="04082-865">資料存取層，就必須根據 Entity Framework 或 LINQ to SQL。</span><span class="sxs-lookup"><span data-stu-id="04082-865">The data-access layer had to be based on Entity Framework or LINQ to SQL.</span></span>
- <span data-ttu-id="04082-866">唯一的資料來源支援這些功能的控制項*EntityDataSource*或是*LinqDataSource*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-866">The only data source controls supported for these features were the *EntityDataSource* or *LinqDataSource* controls.</span></span>
- <span data-ttu-id="04082-867">功能需要 Web 專案以支援此功能所需的所有檔案使用的動態資料或動態資料實體範本所建立的。</span><span class="sxs-lookup"><span data-stu-id="04082-867">The features required a Web project that had been created using the Dynamic Data or Dynamic Data Entities templates in order to have all the files that were required to support the feature.</span></span>

<span data-ttu-id="04082-868">在 ASP.NET 4 中的動態資料支援，主要目的是啟用任何 ASP.NET 應用程式的動態資料的新功能。</span><span class="sxs-lookup"><span data-stu-id="04082-868">A major goal of Dynamic Data support in ASP.NET 4 is to enable the new functionality of Dynamic Data for any ASP.NET application.</span></span> <span data-ttu-id="04082-869">下列範例顯示可以利用動態資料功能的現有網頁中的控制項的標記。</span><span class="sxs-lookup"><span data-stu-id="04082-869">The following example shows markup for controls that can take advantage of Dynamic Data functionality in an existing page.</span></span>

[!code-aspx[Main](overview/samples/sample91.aspx)]

<span data-ttu-id="04082-870">中頁面的程式碼，您必須加入下列的程式碼，若要讓這些控制項的動態資料支援：</span><span class="sxs-lookup"><span data-stu-id="04082-870">In the code for the page, the following code must be added in order to enable Dynamic Data support for these controls:</span></span>

[!code-csharp[Main](overview/samples/sample92.cs)]

<span data-ttu-id="04082-871">當*GridView*控制項處於編輯模式、 動態的資料會自動驗證輸入的資料是正確的格式。</span><span class="sxs-lookup"><span data-stu-id="04082-871">When the *GridView* control is in edit mode, Dynamic Data automatically validates that the data entered is in the proper format.</span></span> <span data-ttu-id="04082-872">如果不存在，則會顯示一則錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="04082-872">If it is not, an error message is displayed.</span></span>

<span data-ttu-id="04082-873">這項功能也提供其他好處，例如能夠指定預設值插入模式。</span><span class="sxs-lookup"><span data-stu-id="04082-873">This functionality also provides other benefits, such as being able to specify default values for insert mode.</span></span> <span data-ttu-id="04082-874">沒有動態的資料，來實作的預設值 欄位中，您必須附加至事件，找不到控制項 (使用*FindControl*)，並設定其值。</span><span class="sxs-lookup"><span data-stu-id="04082-874">Without Dynamic Data, to implement a default value for a field, you must attach to an event, locate the control (using *FindControl*), and set its value.</span></span> <span data-ttu-id="04082-875">在 ASP.NET 4 中， *EnableDynamicData*呼叫支援第二個參數，可讓您傳遞，在物件上的任何欄位的預設值，在此範例中所示：</span><span class="sxs-lookup"><span data-stu-id="04082-875">In ASP.NET 4, the *EnableDynamicData* call supports a second parameter that lets you pass default values for any field on the object, as shown in this example:</span></span>

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a><span data-ttu-id="04082-876">宣告式 DynamicDataManager 控制項語法</span><span class="sxs-lookup"><span data-stu-id="04082-876">Declarative DynamicDataManager Control Syntax</span></span>

<span data-ttu-id="04082-877">*DynamicDataManager*控制項已經過增強，以便您可以設定以宣告方式，如同在 ASP.NET 中，而不是只在程式碼中大部分的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-877">The *DynamicDataManager* control has been enhanced so that you can configure it declaratively, as with most controls in ASP.NET, instead of only in code.</span></span> <span data-ttu-id="04082-878">標記*DynamicDataManager*控制項看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04082-878">The markup for the *DynamicDataManager* control looks like the following example:</span></span>

[!code-aspx[Main](overview/samples/sample94.aspx)]

<span data-ttu-id="04082-879">這個標記可讓 GridView1 控制項中所參考的動態資料行為*度*一節*DynamicDataManager*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-879">This markup enables Dynamic Data behavior for the GridView1 control that is referenced in the *DataControls* section of the *DynamicDataManager* control.</span></span>

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a><span data-ttu-id="04082-880">實體範本</span><span class="sxs-lookup"><span data-stu-id="04082-880">Entity Templates</span></span>

<span data-ttu-id="04082-881">實體範本提供自訂的資料版面配置，而不需要您建立自訂頁面的新方式。</span><span class="sxs-lookup"><span data-stu-id="04082-881">Entity templates offer a new way to customize the layout of data without requiring you to create a custom page.</span></span> <span data-ttu-id="04082-882">頁面上，使用範本*FormView*控制項 (而非*DetailsView*控制項，在舊版的動態資料的頁面範本中所使用) 和*DynamicEntity*要呈現實體範本的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-882">Page templates use the *FormView* control (instead of the *DetailsView* control, as used in page templates in earlier versions of Dynamic Data) and the *DynamicEntity* control to render Entity templates.</span></span> <span data-ttu-id="04082-883">這可讓您更充分掌控 Dynamic Data 所呈現的標記。</span><span class="sxs-lookup"><span data-stu-id="04082-883">This gives you more control over the markup that is rendered by Dynamic Data.</span></span>

<span data-ttu-id="04082-884">下列清單顯示新的專案目錄配置，其中包含實體範本：</span><span class="sxs-lookup"><span data-stu-id="04082-884">The following list shows the new project directory layout that contains entity templates:</span></span>

[!code-console[Main](overview/samples/sample95.cmd)]

<span data-ttu-id="04082-885">`EntityTemplate`目錄包含如何顯示的資料模型物件的範本。</span><span class="sxs-lookup"><span data-stu-id="04082-885">The `EntityTemplate` directory contains templates for how to display data model objects.</span></span> <span data-ttu-id="04082-886">根據預設，物件會使用呈現`Default.ascx`範本，可看起來像是所建立的標記的標記*DetailsView* ASP.NET 3.5 SP1 中的 Dynamic Data 所使用的控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-886">By default, objects are rendered by using the `Default.ascx` template, which provides markup that looks just like the markup created by the *DetailsView* control used by Dynamic Data in ASP.NET 3.5 SP1.</span></span> <span data-ttu-id="04082-887">下列範例顯示的標記`Default.ascx`控制項：</span><span class="sxs-lookup"><span data-stu-id="04082-887">The following example shows the markup for the `Default.ascx` control:</span></span>

[!code-aspx[Main](overview/samples/sample96.aspx)]

<span data-ttu-id="04082-888">若要變更整個網站的外觀及操作，可以編輯預設範本。</span><span class="sxs-lookup"><span data-stu-id="04082-888">The default templates can be edited to change the look and feel for the entire site.</span></span> <span data-ttu-id="04082-889">範本顯示、 編輯和插入作業。</span><span class="sxs-lookup"><span data-stu-id="04082-889">There are templates for display, edit, and insert operations.</span></span> <span data-ttu-id="04082-890">新的範本可以加入根據資料物件的名稱以變更的外觀與風格只是一種類型的物件。</span><span class="sxs-lookup"><span data-stu-id="04082-890">New templates can be added based on the name of the data object in order to change the look and feel of just one type of object.</span></span> <span data-ttu-id="04082-891">例如，您可以新增下列範本：</span><span class="sxs-lookup"><span data-stu-id="04082-891">For example, you can add the following template:</span></span>

[!code-console[Main](overview/samples/sample97.cmd)]

<span data-ttu-id="04082-892">範本可能包含下列標記：</span><span class="sxs-lookup"><span data-stu-id="04082-892">The template might contain the following markup:</span></span>

[!code-aspx[Main](overview/samples/sample98.aspx)]

<span data-ttu-id="04082-893">新的實體範本時，會顯示在頁面上，使用新*DynamicEntity*控制項。</span><span class="sxs-lookup"><span data-stu-id="04082-893">The new entity templates are displayed on a page by using the new *DynamicEntity* control.</span></span> <span data-ttu-id="04082-894">在執行階段，此控制項被取代的實體範本的內容。</span><span class="sxs-lookup"><span data-stu-id="04082-894">At run time, this control is replaced with the contents of the entity template.</span></span> <span data-ttu-id="04082-895">下列標記示範*FormView*控制中`Detail.aspx`使用的實體範本的頁面範本。</span><span class="sxs-lookup"><span data-stu-id="04082-895">The following markup shows the *FormView* control in the `Detail.aspx` page template that uses the entity template.</span></span> <span data-ttu-id="04082-896">請注意*DynamicEntity*在標記中的項目。</span><span class="sxs-lookup"><span data-stu-id="04082-896">Notice the *DynamicEntity* element in the markup.</span></span>

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a><span data-ttu-id="04082-897">新的欄位範本 Url 和電子郵件地址</span><span class="sxs-lookup"><span data-stu-id="04082-897">New Field Templates for URLs and Email Addresses</span></span>

<span data-ttu-id="04082-898">ASP.NET 4 引進了兩個新的內建欄位範本，`EmailAddress.ascx`和`Url.ascx`。</span><span class="sxs-lookup"><span data-stu-id="04082-898">ASP.NET 4 introduces two new built-in field templates, `EmailAddress.ascx` and `Url.ascx`.</span></span> <span data-ttu-id="04082-899">這些範本用於標示欄位*EmailAddress*或是*Url*使用*DataType*屬性。</span><span class="sxs-lookup"><span data-stu-id="04082-899">These templates are used for fields that are marked as *EmailAddress* or *Url* with the *DataType* attribute.</span></span> <span data-ttu-id="04082-900">針對*EmailAddress*物件時，欄位會顯示為超連結建立利用*mailto:* 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="04082-900">For *EmailAddress* objects, the field is displayed as a hyperlink that is created by using the *mailto:* protocol.</span></span> <span data-ttu-id="04082-901">當使用者按一下連結時，它會開啟使用者的電子郵件用戶端的連線，並建立基本架構的訊息。</span><span class="sxs-lookup"><span data-stu-id="04082-901">When users click the link, it opens the user's email client and creates a skeleton message.</span></span> <span data-ttu-id="04082-902">物件的型別為*Url*會顯示為一般的超連結。</span><span class="sxs-lookup"><span data-stu-id="04082-902">Objects typed as *Url* are displayed as ordinary hyperlinks.</span></span>

<span data-ttu-id="04082-903">下列範例會顯示欄位會標示。</span><span class="sxs-lookup"><span data-stu-id="04082-903">The following example shows how fields would be marked.</span></span>

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a><span data-ttu-id="04082-904">建立與 DynamicHyperLink 控制項的連結</span><span class="sxs-lookup"><span data-stu-id="04082-904">Creating Links with the DynamicHyperLink Control</span></span>

<span data-ttu-id="04082-905">動態資料會使用.NET Framework 3.5 SP1，來控制存取網站時，請參閱 < 終端使用者的 Url 中加入新的路由功能。</span><span class="sxs-lookup"><span data-stu-id="04082-905">Dynamic Data uses the new routing feature that was added in the .NET Framework 3.5 SP1 to control the URLs that end users see when they access the Web site.</span></span> <span data-ttu-id="04082-906">新*DynamicHyperLink*控制項可讓您輕鬆建置動態資料站台中的網頁的連結。</span><span class="sxs-lookup"><span data-stu-id="04082-906">The new *DynamicHyperLink* control makes it easy to build links to pages in a Dynamic Data site.</span></span> <span data-ttu-id="04082-907">下列範例示範如何使用*DynamicHyperLink*控制項：</span><span class="sxs-lookup"><span data-stu-id="04082-907">The following example shows how to use the *DynamicHyperLink* control:</span></span>

[!code-aspx[Main](overview/samples/sample101.aspx)]

<span data-ttu-id="04082-908">此標記會建立指向的 「 清單 」 頁面的連結`Products`資料表中所定義的路由為基礎`Global.asax`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-908">This markup creates a link that points to the List page for the `Products` table based on routes that are defined in the `Global.asax` file.</span></span> <span data-ttu-id="04082-909">控制會自動使用動態資料頁面為基礎的預設資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="04082-909">The control automatically uses the default table name that the Dynamic Data page is based on.</span></span>

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a><span data-ttu-id="04082-910">支援的資料模型中的繼承</span><span class="sxs-lookup"><span data-stu-id="04082-910">Support for Inheritance in the Data Model</span></span>

<span data-ttu-id="04082-911">Entity Framework 和 LINQ to SQL 支援繼承其資料模型中。</span><span class="sxs-lookup"><span data-stu-id="04082-911">Both the Entity Framework and LINQ to SQL support inheritance in their data models.</span></span> <span data-ttu-id="04082-912">這個範例可能是資料庫具有`InsurancePolicy`資料表。</span><span class="sxs-lookup"><span data-stu-id="04082-912">An example of this might be a database that has an `InsurancePolicy` table.</span></span> <span data-ttu-id="04082-913">它可能也包含`CarPolicy`並`HousePolicy`具有相同的欄位的資料表，`InsurancePolicy`然後新增更多的欄位。</span><span class="sxs-lookup"><span data-stu-id="04082-913">It might also contain `CarPolicy` and `HousePolicy` tables that have the same fields as `InsurancePolicy` and then add more fields.</span></span> <span data-ttu-id="04082-914">動態資料已經過修改，以了解資料模型中的繼承的物件，以及繼承的資料表支援 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="04082-914">Dynamic Data has been modified to understand inherited objects in the data model and to support scaffolding for the inherited tables.</span></span>

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a><span data-ttu-id="04082-915">支援多對多關聯性 (Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="04082-915">Support for Many-to-Many Relationships (Entity Framework Only)</span></span>

<span data-ttu-id="04082-916">Entity Framework 提供豐富的支援，藉由在上公開為集合關聯性的資料表之間的多對多關聯性*實體*物件。</span><span class="sxs-lookup"><span data-stu-id="04082-916">The Entity Framework has rich support for many-to-many relationships between tables, which is implemented by exposing the relationship as a collection on an *Entity* object.</span></span> <span data-ttu-id="04082-917">新`ManyToMany.ascx`和`ManyToMany_Edit.ascx`欄位範本已新增至可顯示和編輯多對多關聯性中涉及的資料。</span><span class="sxs-lookup"><span data-stu-id="04082-917">New `ManyToMany.ascx` and `ManyToMany_Edit.ascx` field templates have been added to provide support for displaying and editing data that is involved in many-to-many relationships.</span></span>

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a><span data-ttu-id="04082-918">新的屬性，以控制顯示，並支援列舉</span><span class="sxs-lookup"><span data-stu-id="04082-918">New Attributes to Control Display and Support Enumerations</span></span>

<span data-ttu-id="04082-919">*DisplayAttribute*已新增，讓您進一步控制欄位的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="04082-919">The *DisplayAttribute* has been added to give you additional control over how fields are displayed.</span></span> <span data-ttu-id="04082-920">*DisplayName*屬性在舊版的動態資料允許您變更用來當做標題欄位的名稱。</span><span class="sxs-lookup"><span data-stu-id="04082-920">The *DisplayName* attribute in earlier versions of Dynamic Data allowed you to change the name that is used as a caption for a field.</span></span> <span data-ttu-id="04082-921">新*DisplayAttribute*類別可讓您指定多個選項來顯示一個欄位，例如欄位會顯示以及是否將使用欄位作為篩選條件的順序。</span><span class="sxs-lookup"><span data-stu-id="04082-921">The new *DisplayAttribute* class lets you specify more options for displaying a field, such as the order in which a field is displayed and whether a field will be used as a filter.</span></span> <span data-ttu-id="04082-922">屬性也會提供獨立的使用中的標籤名稱的控制項*GridView*控制項，用於的名稱*DetailsView*控制項，[] 欄位中的說明文字和浮水印用於欄位 （如果欄位可接受文字輸入）。</span><span class="sxs-lookup"><span data-stu-id="04082-922">The attribute also provides independent control of the name used for the labels in a *GridView* control, the name used in a *DetailsView* control, the help text for the field, and the watermark used for the field (if the field accepts text input).</span></span>

<span data-ttu-id="04082-923">*EnumDataTypeAttribute*類別已新增可讓您將欄位對應至列舉型別。</span><span class="sxs-lookup"><span data-stu-id="04082-923">The *EnumDataTypeAttribute* class has been added to let you map fields to enumerations.</span></span> <span data-ttu-id="04082-924">當您將這個屬性套用至欄位時，您會指定列舉型別。</span><span class="sxs-lookup"><span data-stu-id="04082-924">When you apply this attribute to a field, you specify an enumeration type.</span></span> <span data-ttu-id="04082-925">使用新的動態資料`Enumeration.ascx`建立 UI，來顯示和編輯列舉值的欄位樣板。</span><span class="sxs-lookup"><span data-stu-id="04082-925">Dynamic Data uses the new `Enumeration.ascx` field template to create UI for displaying and editing enumeration values.</span></span> <span data-ttu-id="04082-926">範本會對應資料庫中的值給列舉型別中的名稱。</span><span class="sxs-lookup"><span data-stu-id="04082-926">The template maps the values from the database to the names in the enumeration.</span></span>

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a><span data-ttu-id="04082-927">篩選器的增強的支援</span><span class="sxs-lookup"><span data-stu-id="04082-927">Enhanced Support for Filters</span></span>

<span data-ttu-id="04082-928">動態資料 1.0 隨附內建布林資料行和外部索引鍵資料行的篩選器。</span><span class="sxs-lookup"><span data-stu-id="04082-928">Dynamic Data 1.0 shipped with built-in filters for Boolean columns and foreign-key columns.</span></span> <span data-ttu-id="04082-929">篩選不允許您指定是否已顯示，請將它們，或它們所顯示的順序。</span><span class="sxs-lookup"><span data-stu-id="04082-929">The filters did not allow you to specify whether they were displayed or in what order they were displayed.</span></span> <span data-ttu-id="04082-930">新*DisplayAttribute*屬性的地址這兩個這些問題，讓您控制是否顯示資料行做為篩選條件和順序將會顯示。</span><span class="sxs-lookup"><span data-stu-id="04082-930">The new *DisplayAttribute* attribute addresses both of these issues by giving you control over whether a column is displayed as a filter and in what order it will be displayed.</span></span>

<span data-ttu-id="04082-931">其他的增強功能是篩選支援已經過重新[編寫成使用新](#0.2__QueryExtender "_QueryExtender") Web Form 的功能。</span><span class="sxs-lookup"><span data-stu-id="04082-931">An additional enhancement is that filtering support has been re[written to use the new](#0.2__QueryExtender "_QueryExtender") feature of Web Forms.</span></span> <span data-ttu-id="04082-932">這可讓您建立篩選器，而不需要篩選器可搭配資料來源控制項的知識。</span><span class="sxs-lookup"><span data-stu-id="04082-932">This lets you create filters without requiring knowledge of the data source control that the filters will be used with.</span></span> <span data-ttu-id="04082-933">這些擴充功能，以及篩選有也已轉換成範本的控制項，可讓您新增新的。</span><span class="sxs-lookup"><span data-stu-id="04082-933">Along with these extensions, filters have also been turned into template controls, which allows you to add new ones.</span></span> <span data-ttu-id="04082-934">最後， *DisplayAttribute*先前所述的類別可讓預設篩選，以覆寫，在同一個採用的方式*UIHint*允許覆寫的資料行的預設欄位樣板。</span><span class="sxs-lookup"><span data-stu-id="04082-934">Finally, the *DisplayAttribute* class mentioned earlier allows the default filter to be overridden, in the same way that *UIHint* allows the default field template for a column to be overridden.</span></span>

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a><span data-ttu-id="04082-935">Visual Studio 2010 Web 開發方面的改進</span><span class="sxs-lookup"><span data-stu-id="04082-935">Visual Studio 2010 Web Development Improvements</span></span>

<span data-ttu-id="04082-936">已增強 Visual Studio 2010 中的 web 開發更 CSS 相容性，透過 HTML 和 ASP.NET 的標記程式碼片段和新動態的 IntelliSense JavaScript 的提升產能。</span><span class="sxs-lookup"><span data-stu-id="04082-936">Web development in Visual Studio 2010 has been enhanced for greater CSS compatibility, increased productivity through HTML and ASP.NET markup snippets and new dynamic IntelliSense JavaScript.</span></span>

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a><span data-ttu-id="04082-937">改良的 CSS 相容性</span><span class="sxs-lookup"><span data-stu-id="04082-937">Improved CSS Compatibility</span></span>

<span data-ttu-id="04082-938">Visual Web Developer 設計工具，Visual Studio 2010 中的有已更新並改良 CSS 2.1 標準相容性。</span><span class="sxs-lookup"><span data-stu-id="04082-938">The Visual Web Developer designer in Visual Studio 2010 has been updated to improve CSS 2.1 standards compliance.</span></span> <span data-ttu-id="04082-939">設計工具更好會保留的 HTML 原始檔的完整性，並且會比在舊版的 Visual Studio 更健全。</span><span class="sxs-lookup"><span data-stu-id="04082-939">The designer better preserves integrity of the HTML source and is more robust than in previous versions of Visual Studio.</span></span> <span data-ttu-id="04082-940">在幕後，架構也進行了改進的就是好讓未來的增強功能，在轉譯、 版面配置，與服務性。</span><span class="sxs-lookup"><span data-stu-id="04082-940">Under the hood, architectural improvements have also been made that will better enable future enhancements in rendering, layout, and serviceability.</span></span>

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a><span data-ttu-id="04082-941">HTML 和 JavaScript 程式碼片段</span><span class="sxs-lookup"><span data-stu-id="04082-941">HTML and JavaScript Snippets</span></span>

<span data-ttu-id="04082-942">在 HTML 編輯器中，IntelliSense 自動完成標記名稱。</span><span class="sxs-lookup"><span data-stu-id="04082-942">In the HTML editor, IntelliSense auto-completes tag names.</span></span> <span data-ttu-id="04082-943">IntelliSense 程式碼片段功能會自動完成整個標籤和更多功能。</span><span class="sxs-lookup"><span data-stu-id="04082-943">The IntelliSense Snippets feature auto-completes entire tags and more.</span></span> <span data-ttu-id="04082-944">在 Visual Studio 2010 中，JavaScript 與 C# 和 Visual Basic 中，不支援在舊版的 Visual Studio 支援 IntelliSense 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="04082-944">In Visual Studio 2010, IntelliSense snippets are supported for JavaScript, alongside C# and Visual Basic, which were supported in earlier versions of Visual Studio.</span></span>

<span data-ttu-id="04082-945">Visual Studio 2010 包含 200 多個程式碼片段，協助您自動完成一般 ASP.NET 和 HTML 標記，包括必要的屬性 (例如 runat ="server") 和標記特有的通用屬性 (例如*識別碼*， *DataSourceID*， *ControlToValidate*，以及*文字*)。</span><span class="sxs-lookup"><span data-stu-id="04082-945">Visual Studio 2010 includes over 200 snippets that help you auto-complete common ASP.NET and HTML tags, including required attributes (such as runat="server") and common attributes specific to a tag (such as *ID*, *DataSourceID*, *ControlToValidate*, and *Text*).</span></span>

<span data-ttu-id="04082-946">您可以下載其他程式碼片段，或您可以撰寫您自己片段，可封裝您或您的小組使用的常見工作的標記區塊。</span><span class="sxs-lookup"><span data-stu-id="04082-946">You can download additional snippets, or you can write your own snippets that encapsulate the blocks of markup that you or your team use for common tasks.</span></span>

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a><span data-ttu-id="04082-947">JavaScript IntelliSense 的增強功能</span><span class="sxs-lookup"><span data-stu-id="04082-947">JavaScript IntelliSense Enhancements</span></span>

<span data-ttu-id="04082-948">在 Visual 2010 中，JavaScript IntelliSense 已重新設計，提供更豐富的編輯體驗。</span><span class="sxs-lookup"><span data-stu-id="04082-948">In Visual 2010, JavaScript IntelliSense has been redesigned to provide an even richer editing experience.</span></span> <span data-ttu-id="04082-949">IntelliSense 現在會表彰動態產生的方法這類的物件*registerNamespace*和其他 JavaScript 架構所使用的類似技術。</span><span class="sxs-lookup"><span data-stu-id="04082-949">IntelliSense now recognizes objects that have been dynamically generated by methods such as *registerNamespace* and by similar techniques used by other JavaScript frameworks.</span></span> <span data-ttu-id="04082-950">已改善效能，分析大型的程式庫的指令碼，並顯示幾乎沒有任何處理延遲的 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="04082-950">Performance has been improved to analyze large libraries of script and to display IntelliSense with little or no processing delay.</span></span> <span data-ttu-id="04082-951">為支援幾乎所有的協力廠商程式庫，並支援各種不同的編碼樣式已大幅增加相容性。</span><span class="sxs-lookup"><span data-stu-id="04082-951">Compatibility has been dramatically increased to support nearly all third-party libraries and to support diverse coding styles.</span></span> <span data-ttu-id="04082-952">當您輸入，並立即利用 IntelliSense，現在會剖析文件註解。</span><span class="sxs-lookup"><span data-stu-id="04082-952">Documentation comments are now parsed as you type and are immediately leveraged by IntelliSense.</span></span>

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a><span data-ttu-id="04082-953">使用 Visual Studio 2010 的 web 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="04082-953">Web Application Deployment with Visual Studio 2010</span></span>

<span data-ttu-id="04082-954">當 ASP.NET 開發人員部署 Web 應用程式時，他們經常遇到問題，如下所示：</span><span class="sxs-lookup"><span data-stu-id="04082-954">When ASP.NET developers deploy a Web application, they often find that they encounter issues such as the following:</span></span>

- <span data-ttu-id="04082-955">部署到共用裝載站台需要技術，例如 FTP，可能會很慢。</span><span class="sxs-lookup"><span data-stu-id="04082-955">Deploying to a shared hosting site requires technologies such as FTP, which can be slow.</span></span> <span data-ttu-id="04082-956">此外，您必須手動執行工作，例如執行 SQL 指令碼，以設定資料庫，而且您必須變更 IIS 設定，例如做為應用程式中設定虛擬目錄資料夾。</span><span class="sxs-lookup"><span data-stu-id="04082-956">In addition, you must manually perform tasks such as running SQL scripts to configure a database and you must change IIS settings, such as configuring a virtual directory folder as an application.</span></span>
- <span data-ttu-id="04082-957">在企業環境中，除了部署 Web 應用程式檔案中，系統管理員通常必須修改 ASP.NET 組態檔和 IIS 設定。</span><span class="sxs-lookup"><span data-stu-id="04082-957">In an enterprise environment, in addition to deploying the Web application files, administrators frequently must modify ASP.NET configuration files and IIS settings.</span></span> <span data-ttu-id="04082-958">資料庫管理員必須執行一系列的 SQL 指令碼，以取得執行的應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="04082-958">Database administrators must run a series of SQL scripts to get the application database running.</span></span> <span data-ttu-id="04082-959">這類安裝大量的人力，通常需要較小時才能完成，且必須仔細記錄。</span><span class="sxs-lookup"><span data-stu-id="04082-959">Such installations are labor intensive, often take hours to complete, and must be carefully documented.</span></span>

<span data-ttu-id="04082-960">Visual Studio 2010 包含的技術，以解決這些問題，並讓您順暢地部署 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-960">Visual Studio 2010 includes technologies that address these issues and that let you seamlessly deploy Web applications.</span></span> <span data-ttu-id="04082-961">這些技術的其中之一是 IIS Web Deployment Tool (MsDeploy.exe)。</span><span class="sxs-lookup"><span data-stu-id="04082-961">One of these technologies is the IIS Web Deployment Tool (MsDeploy.exe).</span></span>

<span data-ttu-id="04082-962">Visual Studio 2010 中的 web 部署功能包括下列重點：</span><span class="sxs-lookup"><span data-stu-id="04082-962">Web deployment features in Visual Studio 2010 include the following major areas:</span></span>

- <span data-ttu-id="04082-963">Web 封裝</span><span class="sxs-lookup"><span data-stu-id="04082-963">Web packaging</span></span>
- <span data-ttu-id="04082-964">Web.config 轉換</span><span class="sxs-lookup"><span data-stu-id="04082-964">Web.config transformation</span></span>
- <span data-ttu-id="04082-965">資料庫部署</span><span class="sxs-lookup"><span data-stu-id="04082-965">Database deployment</span></span>
- <span data-ttu-id="04082-966">單鍵發行為 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="04082-966">One-click publish for Web applications</span></span>

<span data-ttu-id="04082-967">下列各節提供有關這些功能的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="04082-967">The following sections provide details about these features.</span></span>

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a><span data-ttu-id="04082-968">Web 封裝</span><span class="sxs-lookup"><span data-stu-id="04082-968">Web Packaging</span></span>

<span data-ttu-id="04082-969">Visual Studio 2010 使用 MSDeploy 工具，來建立您的應用程式，這指的壓縮 (.zip) 檔案*網頁套件*。</span><span class="sxs-lookup"><span data-stu-id="04082-969">Visual Studio 2010 uses the MSDeploy tool to create a compressed (.zip) file for your application, which is referred to as a *Web package*.</span></span> <span data-ttu-id="04082-970">封裝檔案會包含您的應用程式加上下列內容的相關中繼資料：</span><span class="sxs-lookup"><span data-stu-id="04082-970">The package file contains metadata about your application plus the following content:</span></span>

- <span data-ttu-id="04082-971">IIS 設定，其中包括應用程式集區設定、 錯誤頁面設定，等等。</span><span class="sxs-lookup"><span data-stu-id="04082-971">IIS settings, which includes application pool settings, error page settings, and so on.</span></span>
- <span data-ttu-id="04082-972">實際的 Web 內容，其中包含 Web 網頁、 使用者控制項、 靜態內容 （映像和 HTML 檔案），等等。</span><span class="sxs-lookup"><span data-stu-id="04082-972">The actual Web content, which includes Web pages, user controls, static content (images and HTML files), and so on.</span></span>
- <span data-ttu-id="04082-973">SQL Server 資料庫結構描述和資料。</span><span class="sxs-lookup"><span data-stu-id="04082-973">SQL Server database schemas and data.</span></span>
- <span data-ttu-id="04082-974">安全性憑證，將安裝在 GAC、 登錄設定等等的元件。</span><span class="sxs-lookup"><span data-stu-id="04082-974">Security certificates, components to install in the GAC, registry settings, and so on.</span></span>

<span data-ttu-id="04082-975">網頁套件可以複製到任何伺服器，並再使用 IIS 管理員以手動方式安裝。</span><span class="sxs-lookup"><span data-stu-id="04082-975">A Web package can be copied to any server and then installed manually by using IIS Manager.</span></span> <span data-ttu-id="04082-976">或者，用於自動化部署，封裝可以安裝使用命令列命令，或使用部署 Api。</span><span class="sxs-lookup"><span data-stu-id="04082-976">Alternatively, for automated deployment, the package can be installed by using command-line commands or by using deployment APIs.</span></span>

<span data-ttu-id="04082-977">Visual Studio 2010 提供內建的 MSBuild 工作與建立 Web 封裝的目標。</span><span class="sxs-lookup"><span data-stu-id="04082-977">Visual Studio 2010 provides built in MSBuild tasks and targets to create Web packages.</span></span> <span data-ttu-id="04082-978">如需詳細資訊，請參閱 < [ASP.NET Web 應用程式專案部署概觀](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx)MSDN 網站上並[10 + 20 的原因，為什麼您應該建立網頁套件](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html)Vishal Joshi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="04082-978">For more information, see [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) on the MSDN Web site and [10 + 20 reasons why you should create a Web Package](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a><span data-ttu-id="04082-979">Web.config 轉換</span><span class="sxs-lookup"><span data-stu-id="04082-979">Web.config Transformation</span></span>

<span data-ttu-id="04082-980">部署 Web 應用程式，Visual Studio 2010 推出[XML 文件轉換 (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)，這是一個功能，可讓您轉換`Web.config`開發設定生產環境設定的檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-980">For Web application deployment, Visual Studio 2010 introduces [XML Document Transform (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), which is a feature that lets you transform a `Web.config` file from development settings to production settings.</span></span> <span data-ttu-id="04082-981">轉換設定會指定於名為轉換檔案`web.debug.config`， `web.release.config`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="04082-981">Transformation settings are specified in transform files named `web.debug.config`, `web.release.config`, and so on.</span></span> <span data-ttu-id="04082-982">（這些檔案的名稱符合 MSBuild 組態）。轉換檔包含只變更，您必須先部署`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="04082-982">(The names of these files match MSBuild configurations.) A transform file includes just the changes that you need to make to a deployed `Web.config` file.</span></span> <span data-ttu-id="04082-983">您可以使用簡單的語法，以指定所做的變更。</span><span class="sxs-lookup"><span data-stu-id="04082-983">You specify the changes by using simple syntax.</span></span>

<span data-ttu-id="04082-984">下列範例會顯示部分`web.release.config`部署您的發行設定檔可能會產生。</span><span class="sxs-lookup"><span data-stu-id="04082-984">The following example shows a portion of a `web.release.config` file that might be produced for deployment of your release configuration.</span></span> <span data-ttu-id="04082-985">取代關鍵字，在範例中所指定的是，在部署期間*connectionString*中的節點`Web.config`檔案將會取代此範例中所列的值。</span><span class="sxs-lookup"><span data-stu-id="04082-985">The Replace keyword in the example specifies that during deployment the *connectionString* node in the `Web.config` file will be replaced with the values that are listed in the example.</span></span>

[!code-xml[Main](overview/samples/sample102.xml)]

<span data-ttu-id="04082-986">如需詳細資訊，請參閱 < [Web 應用程式專案部署的 Web.config 轉換語法](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx)msdn <a id="0.2_a"> </a>的網站和[Web 部署： Web.Config 轉換](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="04082-986">For more information, see [Web.config Transformation Syntax for Web Application Project Deployment](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) on the MSDN <a id="0.2_a"></a> Web site and[Web Deployment: Web.Config Transformation](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a><span data-ttu-id="04082-987">資料庫部署</span><span class="sxs-lookup"><span data-stu-id="04082-987">Database Deployment</span></span>

<span data-ttu-id="04082-988">Visual Studio 2010 的部署套件可以包含在 SQL Server 資料庫上的相依性。</span><span class="sxs-lookup"><span data-stu-id="04082-988">A Visual Studio 2010 deployment package can include dependencies on SQL Server databases.</span></span> <span data-ttu-id="04082-989">封裝定義的一部分中,，您可以提供連接字串的來源資料庫。</span><span class="sxs-lookup"><span data-stu-id="04082-989">As part of the package definition, you provide the connection string for your source database.</span></span> <span data-ttu-id="04082-990">當您建立的 Web 套件時，Visual Studio 2010 建立 SQL 指令碼的資料庫結構描述以及選擇性的資料，並再將這些封裝。</span><span class="sxs-lookup"><span data-stu-id="04082-990">When you create the Web package, Visual Studio 2010 creates SQL scripts for the database schema and optionally for the data, and then adds these to the package.</span></span> <span data-ttu-id="04082-991">您也可以提供自訂的 SQL 指令碼，並指定在伺服器應該執行的順序。</span><span class="sxs-lookup"><span data-stu-id="04082-991">You can also provide custom SQL scripts and specify the sequence in which they should run on the server.</span></span> <span data-ttu-id="04082-992">您可以在部署期間提供適用於目標伺服器的連接字串部署程序接著會使用此連接字串執行的指令碼所建立的資料庫結構描述並加入資料。</span><span class="sxs-lookup"><span data-stu-id="04082-992">At deployment time, you provide a connection string that is appropriate for the target server; the deployment process then uses this connection string to run the scripts that create the database schema and add the data.</span></span>

<span data-ttu-id="04082-993">此外，使用單鍵發行，您可以設定部署至應用程式發佈到遠端共用裝載站台時，直接發佈您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="04082-993">In addition, by using one-click publish, you can configure deployment to publish your database directly when the application is published to a remote shared hosting site.</span></span> <span data-ttu-id="04082-994">如需詳細資訊，請參閱[如何： 部署資料庫與 Web 應用程式專案](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx)MSDN 網站上並[VS 2010 中的資料庫部署](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="04082-994">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) on the MSDN Web site and [Database Deployment with VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a><span data-ttu-id="04082-995">單鍵發行為 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="04082-995">One-Click Publish for Web Applications</span></span>

<span data-ttu-id="04082-996">Visual Studio 2010 也可讓您使用 IIS 遠端管理服務發佈至遠端伺服器的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04082-996">Visual Studio 2010 also lets you use the IIS remote management service to publish a Web application to a remote server.</span></span> <span data-ttu-id="04082-997">針對您的裝載帳戶或測試伺服器或臨時伺服器，您可以建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="04082-997">You can create a publish profile for your hosting account or for testing servers or staging servers.</span></span> <span data-ttu-id="04082-998">每個設定檔可以安全地儲存適當的認證。</span><span class="sxs-lookup"><span data-stu-id="04082-998">Each profile can save appropriate credentials securely.</span></span> <span data-ttu-id="04082-999">然後您可以部署到任何目標伺服器，只要按一下，使用 Web 單鍵發行工具列。</span><span class="sxs-lookup"><span data-stu-id="04082-999">You can then deploy to any of the target servers with one click by using the Web one-click publish toolbar.</span></span> <span data-ttu-id="04082-1000">Visual Studio 2010 中，您也可以使用 MSBuild 命令列發佈。</span><span class="sxs-lookup"><span data-stu-id="04082-1000">With Visual Studio 2010, you can also publish by using the MSBuild command line.</span></span> <span data-ttu-id="04082-1001">這可讓您設定持續整合模型中包含發行您小組的建置環境。</span><span class="sxs-lookup"><span data-stu-id="04082-1001">This lets you configure your team build environment to include publishing in a continuous-integration model.</span></span>

<span data-ttu-id="04082-1002">如需詳細資訊，請參閱 <<c0> [ 如何： 部署 Web 應用程式專案使用單鍵發行和 Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) MSDN 網站上並[Web 1-按一下發佈與 VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="04082-1002">For more information, see [How to: Deploy a Web Application Project Using One-Click Publish and Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) on the MSDN Web site and [Web 1-Click Publish with VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) on Vishal Joshi's blog.</span></span> <span data-ttu-id="04082-1003">若要檢視 Visual Studio 2010 中的 Web 應用程式部署相關的視訊簡報，請參閱[VS 2010 進行 Web 開發人員預覽](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html)Vishal Joshi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="04082-1003">To view video presentations about Web application deployment in Visual Studio 2010, see [VS 2010 for Web Developer Previews](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) on Vishal Joshi's blog.</span></span>

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a><span data-ttu-id="04082-1004">資源</span><span class="sxs-lookup"><span data-stu-id="04082-1004">Resources</span></span>

<span data-ttu-id="04082-1005">欞眭寍鯚提供 ASP.NET 4 和 Visual Studio 2010 的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="04082-1005">The following Web sites provide additional information about ASP.NET 4 and Visual Studio 2010.</span></span>

- <span data-ttu-id="04082-1006">[ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — MSDN 網站上的 ASP.NET 4 的官方文件。</span><span class="sxs-lookup"><span data-stu-id="04082-1006">[ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — The official documentation for ASP.NET 4 on the MSDN Web site.</span></span>
- <span data-ttu-id="04082-1007">[https://www.asp.net/](https://www.asp.net/) -ASP.NET 小組自己的網站。</span><span class="sxs-lookup"><span data-stu-id="04082-1007">[https://www.asp.net/](https://www.asp.net/) — The ASP.NET team's own Web site.</span></span>
- <span data-ttu-id="04082-1008">[https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) 並[ASP.NET 動態資料內容對應](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx)— ASP.NET 小組網站和 ASP.NET Dynamic Data 的官方文件中的線上資源。</span><span class="sxs-lookup"><span data-stu-id="04082-1008">[https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) and [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — Online resources on the ASP.NET team site and in the official documentation for ASP.NET Dynamic Data.</span></span>
- <span data-ttu-id="04082-1009">[https://www.asp.net/ajax/](../../ajax/index.md) — ASP.NET Ajax 開發主要 Web 資源。</span><span class="sxs-lookup"><span data-stu-id="04082-1009">[https://www.asp.net/ajax/](../../ajax/index.md) — The main Web resource for ASP.NET Ajax development.</span></span>
- <span data-ttu-id="04082-1010">[https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) -Visual Web Developer 小組部落格，其中包含 Visual Studio 2010 中的功能的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="04082-1010">[https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — The Visual Web Developer Team blog, which includes information about features in Visual Studio 2010.</span></span>
- <span data-ttu-id="04082-1011">[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — ASP.NET 的 preview 版本的主要 Web 資源。</span><span class="sxs-lookup"><span data-stu-id="04082-1011">[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — The main Web resource for preview releases of ASP.NET.</span></span>

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a><span data-ttu-id="04082-1012">免責聲明</span><span class="sxs-lookup"><span data-stu-id="04082-1012">Disclaimer</span></span>

<span data-ttu-id="04082-1013">這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。</span><span class="sxs-lookup"><span data-stu-id="04082-1013">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="04082-1014">本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。</span><span class="sxs-lookup"><span data-stu-id="04082-1014">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="04082-1015">Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。</span><span class="sxs-lookup"><span data-stu-id="04082-1015">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="04082-1016">本技術白皮書僅供參考。</span><span class="sxs-lookup"><span data-stu-id="04082-1016">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="04082-1017">MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。</span><span class="sxs-lookup"><span data-stu-id="04082-1017">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="04082-1018">承諾遵守所有適用的著作權法是使用者的責任。</span><span class="sxs-lookup"><span data-stu-id="04082-1018">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="04082-1019">著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。</span><span class="sxs-lookup"><span data-stu-id="04082-1019">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="04082-1020">本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。</span><span class="sxs-lookup"><span data-stu-id="04082-1020">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="04082-1021">除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。</span><span class="sxs-lookup"><span data-stu-id="04082-1021">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="04082-1022">除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人物、 地點及事件本文所述之屬虛構，以及與任何真實公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件是純屬巧合。</span><span class="sxs-lookup"><span data-stu-id="04082-1022">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="04082-1023">© 2009 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="04082-1023">© 2009 Microsoft Corporation.</span></span> <span data-ttu-id="04082-1024">著作權所有，並保留一切權利。</span><span class="sxs-lookup"><span data-stu-id="04082-1024">All rights reserved.</span></span>

<span data-ttu-id="04082-1025">Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。</span><span class="sxs-lookup"><span data-stu-id="04082-1025">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="04082-1026">本文件中所提實際公司和產品，可能為各所有人所有之商標。</span><span class="sxs-lookup"><span data-stu-id="04082-1026">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
