---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 0bfe9cdc215226457ccfafff2b85ace87325b91b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-3"></a><span data-ttu-id="05d71-102">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="05d71-102">ASP.NET MVC 3</span></span>
====================
- [<span data-ttu-id="05d71-103">概觀</span><span class="sxs-lookup"><span data-stu-id="05d71-103">Overview</span></span>](#overview)
- [<span data-ttu-id="05d71-104">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="05d71-104">Installation Notes</span></span>](#installation-notes)
- [<span data-ttu-id="05d71-105">軟體需求</span><span class="sxs-lookup"><span data-stu-id="05d71-105">Software Requirements</span></span>](#software-requirements)
- [<span data-ttu-id="05d71-106">文件 (英文)</span><span class="sxs-lookup"><span data-stu-id="05d71-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="05d71-107">支援</span><span class="sxs-lookup"><span data-stu-id="05d71-107">Support</span></span>](#support)
- [<span data-ttu-id="05d71-108">ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="05d71-108">Upgrading an ASP.NET MVC 2 Project to ASP.NET MVC 3 Tools Update</span></span>](#upgrading)
- [<span data-ttu-id="05d71-109">ASP.NET MVC 3 Tools Update (2011 年 4 月 12 日)</span><span class="sxs-lookup"><span data-stu-id="05d71-109">ASP.NET MVC 3 Tools Update (April 12, 2011)</span></span>](#tu-changes)

    - <span data-ttu-id="05d71-110">[[加入控制器] 對話方塊現在可以 scaffold 控制器與檢視和資料存取程式碼](#tu-AddControllerDialog)</span><span class="sxs-lookup"><span data-stu-id="05d71-110">["Add Controller" dialog box can now scaffold controllers with views and data access code](#tu-AddControllerDialog)</span></span>
    - [<span data-ttu-id="05d71-111">改善 「 ASP.NET MVC 3 新專案 」 對話方塊</span><span class="sxs-lookup"><span data-stu-id="05d71-111">Improvements to the "ASP.NET MVC 3 New Project" Dialog Box</span></span>](#tu-ImprovementsNewDialogBox)
    - [<span data-ttu-id="05d71-112">專案範本現在包含 Modernizr 1.7</span><span class="sxs-lookup"><span data-stu-id="05d71-112">Project templates now include Modernizr 1.7</span></span>](#tu-Modernizr)
    - [<span data-ttu-id="05d71-113">專案範本包含 jQuery、 jQuery UI 和 jQuery 的更新的版本驗證</span><span class="sxs-lookup"><span data-stu-id="05d71-113">Project templates include updated versions of jQuery, jQuery UI, and jQuery Validation</span></span>](#tu-UpdatedJQuery)
    - [<span data-ttu-id="05d71-114">專案範本現在包含 ADO.NET Entity Framework 4.1 以預先安裝的 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="05d71-114">Project templates now include ADO.NET Entity Framework 4.1 as a pre-installed NuGet package</span></span>](#tu-EF)
    - [<span data-ttu-id="05d71-115">專案範本會納入預先安裝的 NuGet 套件中的 JavaScript 程式庫</span><span class="sxs-lookup"><span data-stu-id="05d71-115">Project templates include JavaScript libraries as pre-installed NuGet packages</span></span>](#tu-JavaScriptLibsNuget)
    - [<span data-ttu-id="05d71-116">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-116">Known Issues</span></span>](#tu-KI)
- [<span data-ttu-id="05d71-117">ASP.NET MVC 3 RTM (2011 年 1 月 13 日)</span><span class="sxs-lookup"><span data-stu-id="05d71-117">ASP.NET MVC 3 RTM (January 13, 2011)</span></span>](#MVC3RTM)

    - [<span data-ttu-id="05d71-118">變更： 更新至 1.8.7 jQuery UI 的版本</span><span class="sxs-lookup"><span data-stu-id="05d71-118">Change: Updated the version of jQuery UI to 1.8.7</span></span>](#RTM-1)
    - [<span data-ttu-id="05d71-119">變更： 變更的預設 ModelMetadataProvider 回 DataAnnotationsModelMetadataProvider</span><span class="sxs-lookup"><span data-stu-id="05d71-119">Change: Changed the default ModelMetadataProvider back to DataAnnotationsModelMetadataProvider</span></span>](#RTM-2)
    - [<span data-ttu-id="05d71-120">已修正︰ 貼上含有空白結果中要反轉的 Razor 運算式的一部分</span><span class="sxs-lookup"><span data-stu-id="05d71-120">Fixed: Pasting part of a Razor expression that contains whitespace results in it being reversed</span></span>](#RTM-3)
    - [<span data-ttu-id="05d71-121">已修正︰ 重新命名 Razor 檔案在編輯器中開啟時，會停用語法顏色標示和 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="05d71-121">Fixed: Renaming a Razor file that is opened in the editor disables syntax colorization and IntelliSense</span></span>](#RTM-4)
    - [<span data-ttu-id="05d71-122">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-122">Known Issues</span></span>](#RTM-KI)
    - [<span data-ttu-id="05d71-123">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-123">Breaking Changes</span></span>](#RTM-BC)
- [<span data-ttu-id="05d71-124">ASP.NET MVC 3 候選版 2 (2010 年 12 月 10 日)</span><span class="sxs-lookup"><span data-stu-id="05d71-124">ASP.NET MVC 3 Release Candidate 2 (December 10, 2010)</span></span>](#_Toc2)

    - [<span data-ttu-id="05d71-125">專案範本變更為包含 jQuery 1.4.4、 jQuery 驗證 1.7 和 jQuery UI 1.8.6y UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="05d71-125">Project Templates Changed to Include jQuery 1.4.4, jQuery Validation 1.7, and jQuery UI 1.8.6y UI 1.8.6</span></span>](#_Toc2_1)
    - [<span data-ttu-id="05d71-126">已新增 「 AdditionalMetadataAttribute 」 類別</span><span class="sxs-lookup"><span data-stu-id="05d71-126">Added "AdditionalMetadataAttribute" Class</span></span>](#_Toc2_2)
    - [<span data-ttu-id="05d71-127">改善的檢視 Scaffolding</span><span class="sxs-lookup"><span data-stu-id="05d71-127">Improved View Scaffolding</span></span>](#_Toc2_3)
    - [<span data-ttu-id="05d71-128">加入的 Html.Raw 方法</span><span class="sxs-lookup"><span data-stu-id="05d71-128">Added Html.Raw Method</span></span>](#_Toc2_3)
    - [<span data-ttu-id="05d71-129">重新命名 「 Controller.ViewModel"屬性，「 檢視 」 屬性設定為"ViewBag"</span><span class="sxs-lookup"><span data-stu-id="05d71-129">Renamed "Controller.ViewModel" Property and the "View" Property To "ViewBag"</span></span>](#_Toc2_4)
    - [<span data-ttu-id="05d71-130">重新命名 「 ControllerSessionStateAttribute 「 類別 」 SessionStateAttribute"</span><span class="sxs-lookup"><span data-stu-id="05d71-130">Renamed "ControllerSessionStateAttribute" Class to "SessionStateAttribute"</span></span>](#_Toc2_5)
    - [<span data-ttu-id="05d71-131">重新命名的 RemoteAttribute 「 欄位 」 屬性 「 AdditionalFields"</span><span class="sxs-lookup"><span data-stu-id="05d71-131">Renamed RemoteAttribute "Fields" Property to "AdditionalFields"</span></span>](#_Toc2_6)
    - [<span data-ttu-id="05d71-132">重新命名 「 SkipRequestValidationAttribute"至"AllowHtmlAttribute"</span><span class="sxs-lookup"><span data-stu-id="05d71-132">Renamed "SkipRequestValidationAttribute" to "AllowHtmlAttribute"</span></span>](#_Toc2_7)
    - [<span data-ttu-id="05d71-133">已變更 」 Html.ValidationMessage"方法，以顯示第一個有用的錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="05d71-133">Changed "Html.ValidationMessage" Method to Display the First Useful Error Message</span></span>](#_Toc2_8)
    - [<span data-ttu-id="05d71-134">固定@model不要將空白字元加入至文件的宣告</span><span class="sxs-lookup"><span data-stu-id="05d71-134">Fixed @model Declaration to not Add Whitespace to the Document</span></span>](#_Toc2_9)
    - [<span data-ttu-id="05d71-135">已新增 「 FileExtensions"屬性，以檢視引擎，以支援引擎特定檔案名稱</span><span class="sxs-lookup"><span data-stu-id="05d71-135">Added "FileExtensions" Property to View Engines to Support Engine-Specific File Names</span></span>](#_Toc2_10)
    - [<span data-ttu-id="05d71-136">發出正確的值為"For"屬性的固定"LabelFor 「 協助程式</span><span class="sxs-lookup"><span data-stu-id="05d71-136">Fixed "LabelFor" Helper to Emit the Correct Value for the "For" Attribute</span></span>](#_Toc2_11)
    - [<span data-ttu-id="05d71-137">固定"RenderAction"方法，來提供模型繫結期間明確的值優先順序</span><span class="sxs-lookup"><span data-stu-id="05d71-137">Fixed "RenderAction" Method to Give Explicit Values Precedence During Model Binding</span></span>](#_Toc2_12)
    - [<span data-ttu-id="05d71-138">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-138">Breaking Changes</span></span>](#_Toc2_BC)
    - [<span data-ttu-id="05d71-139">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-139">Known Issues</span></span>](#_Toc2_KI)
- [<span data-ttu-id="05d71-140">ASP.NET MVC 3 候選版 (2010 年 11 月 9 日)</span><span class="sxs-lookup"><span data-stu-id="05d71-140">ASP.NET MVC 3 Release Candidate (Nov 9, 2010)</span></span>](#TOC_ASP_NET_3_RC)

    - [<span data-ttu-id="05d71-141">ASP.NET MVC 3 RC 的新功能</span><span class="sxs-lookup"><span data-stu-id="05d71-141">New Features in ASP.NET MVC 3 RC</span></span>](#_Toc276711785)
    - [<span data-ttu-id="05d71-142">NuGet 封裝管理員</span><span class="sxs-lookup"><span data-stu-id="05d71-142">NuGet Package Manager</span></span>](#_Toc276711786)
    - <span data-ttu-id="05d71-143">[改善 [新增專案] 對話方塊](#_Toc276711787)</span><span class="sxs-lookup"><span data-stu-id="05d71-143">[Improved "New Project" Dialog Box](#_Toc276711787)</span></span>
    - [<span data-ttu-id="05d71-144">無工作階段的控制站</span><span class="sxs-lookup"><span data-stu-id="05d71-144">Sessionless Controllers</span></span>](#_Toc276711788)
    - [<span data-ttu-id="05d71-145">新的驗證屬性</span><span class="sxs-lookup"><span data-stu-id="05d71-145">New Validation Attributes</span></span>](#_Toc276711789)
    - [<span data-ttu-id="05d71-146">新的 「 LabelFor"和"LabelForModel"方法的多載</span><span class="sxs-lookup"><span data-stu-id="05d71-146">New Overloads for "LabelFor" and "LabelForModel" Methods</span></span>](#_Toc276711790)
    - [<span data-ttu-id="05d71-147">子系動作輸出快取</span><span class="sxs-lookup"><span data-stu-id="05d71-147">Child Action Output Caching</span></span>](#_Toc276711791)
    - <span data-ttu-id="05d71-148">[[新增檢視] 對話方塊改進](#_Toc276711792)</span><span class="sxs-lookup"><span data-stu-id="05d71-148">["Add View" Dialog Box Improvements](#_Toc276711792)</span></span>
    - [<span data-ttu-id="05d71-149">詳細的要求驗證</span><span class="sxs-lookup"><span data-stu-id="05d71-149">Granular Request Validation</span></span>](#_Toc276711793)
    - [<span data-ttu-id="05d71-150">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-150">Breaking Changes</span></span>](#_Toc276711794)
    - [<span data-ttu-id="05d71-151">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-151">Known Issues</span></span>](#_Toc276711795)
- [<span data-ttu-id="05d71-152">ASP。MVC 3 Beta 資訊 (2010 年 10 月 6 日)</span><span class="sxs-lookup"><span data-stu-id="05d71-152">ASP.MVC 3 Beta Notes (Oct 6, 2010)</span></span>](#TOC_ASP_NET_3_Beta)

    - [<span data-ttu-id="05d71-153">ASP.NET MVC 3 Beta 的新功能</span><span class="sxs-lookup"><span data-stu-id="05d71-153">New Features in ASP.NET MVC 3 Beta</span></span>](#0.1__Toc274034215)
    - [<span data-ttu-id="05d71-154">NuPack 封裝管理員</span><span class="sxs-lookup"><span data-stu-id="05d71-154">NuPack Package Manager</span></span>](#0.1__Toc274034216)
    - [<span data-ttu-id="05d71-155">改善 新增專案對話方塊</span><span class="sxs-lookup"><span data-stu-id="05d71-155">Improved New Project Dialog Box</span></span>](#0.1__Toc274034217)
    - [<span data-ttu-id="05d71-156">簡化的方式指定強類型模型在 Razor 檢視</span><span class="sxs-lookup"><span data-stu-id="05d71-156">Simplified Way to Specify Strongly Typed Models in Razor Views</span></span>](#0.1__Toc274034218)
    - [<span data-ttu-id="05d71-157">Helper 方法支援新的 ASP.NET Web 網頁</span><span class="sxs-lookup"><span data-stu-id="05d71-157">Support for New ASP.NET Web Pages Helper Methods</span></span>](#0.1__Toc274034219)
    - [<span data-ttu-id="05d71-158">其他相依性插入的支援</span><span class="sxs-lookup"><span data-stu-id="05d71-158">Additional Dependency Injection Support</span></span>](#0.1__Toc274034220)
    - [<span data-ttu-id="05d71-159">新的不顯眼的 jQuery 為基礎的 Ajax 支援</span><span class="sxs-lookup"><span data-stu-id="05d71-159">New Support for Unobtrusive jQuery-Based Ajax</span></span>](#0.1__Toc274034221)
    - [<span data-ttu-id="05d71-160">不顯眼的 jQuery 驗證的新支援</span><span class="sxs-lookup"><span data-stu-id="05d71-160">New Support for Unobtrusive jQuery Validation</span></span>](#0.1__Toc274034222)
    - [<span data-ttu-id="05d71-161">新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript</span><span class="sxs-lookup"><span data-stu-id="05d71-161">New Application-Wide Flags for Client Validation and Unobtrusive JavaScript</span></span>](#0.1__Toc274034223)
    - [<span data-ttu-id="05d71-162">檢視執行之前，所執行的程式碼的新支援</span><span class="sxs-lookup"><span data-stu-id="05d71-162">New Support for Code that Runs Before Views Run</span></span>](#0.1__Toc274034224)
    - [<span data-ttu-id="05d71-163">新 VBHTML Razor 語法的支援</span><span class="sxs-lookup"><span data-stu-id="05d71-163">New Support for the VBHTML Razor Syntax</span></span>](#0.1__Toc274034225)
    - [<span data-ttu-id="05d71-164">更精確地控制從而 validateinputattribute 套用</span><span class="sxs-lookup"><span data-stu-id="05d71-164">More Granular Control over ValidateInputAttribute</span></span>](#0.1__Toc274034226)
    - [<span data-ttu-id="05d71-165">協助程式轉換成底線連字號為使用匿名物件指定的 HTML 屬性名稱</span><span class="sxs-lookup"><span data-stu-id="05d71-165">Helpers Convert Underscores to Hyphens for HTML Attribute Names Specified Using Anonymous Objects</span></span>](#0.1__Toc274034227)
    - [<span data-ttu-id="05d71-166">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="05d71-166">Bug Fixes</span></span>](#0.1__Toc274034228)
    - [<span data-ttu-id="05d71-167">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-167">Breaking Changes</span></span>](#0.1__Toc274034229)
    - [<span data-ttu-id="05d71-168">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-168">Known Issues</span></span>](#0.1__Toc274034230)
- [<span data-ttu-id="05d71-169">Disclaimer</span><span class="sxs-lookup"><span data-stu-id="05d71-169">Disclaimer</span></span>](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a><span data-ttu-id="05d71-170">總覽</span><span class="sxs-lookup"><span data-stu-id="05d71-170">Overview</span></span>

<span data-ttu-id="05d71-171">本文件描述 for Visual Studio 2010 版本的 ASP.NET MVC 3 RTM。</span><span class="sxs-lookup"><span data-stu-id="05d71-171">This document describes the release of ASP.NET MVC 3 RTM for Visual Studio 2010.</span></span> <span data-ttu-id="05d71-172">ASP.NET MVC 是使用 「 模型檢視控制器 (MVC) 模式開發 Web 應用程式架構。</span><span class="sxs-lookup"><span data-stu-id="05d71-172">ASP.NET MVC is a framework for developing Web applications that uses the Model-View-Controller (MVC) pattern.</span></span> <span data-ttu-id="05d71-173">ASP.NET MVC 3 安裝程式包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="05d71-173">The ASP.NET MVC 3 installer includes the following components:</span></span>

- <span data-ttu-id="05d71-174">ASP.NET MVC 3 執行階段元件</span><span class="sxs-lookup"><span data-stu-id="05d71-174">ASP.NET MVC 3 runtime components</span></span>
- <span data-ttu-id="05d71-175">ASP.NET MVC 3 Visual Studio 2010 工具</span><span class="sxs-lookup"><span data-stu-id="05d71-175">ASP.NET MVC 3 Visual Studio 2010 tools</span></span>
- <span data-ttu-id="05d71-176">ASP.NET Web 網頁的執行階段元件</span><span class="sxs-lookup"><span data-stu-id="05d71-176">ASP.NET Web Pages run-time components</span></span>
- <span data-ttu-id="05d71-177">ASP.NET Web Pages Visual Studio 2010 工具</span><span class="sxs-lookup"><span data-stu-id="05d71-177">ASP.NET Web Pages Visual Studio 2010 tools</span></span>
- <span data-ttu-id="05d71-178">Microsoft Package Manager for.NET (NuGet)</span><span class="sxs-lookup"><span data-stu-id="05d71-178">Microsoft Package Manager for .NET (NuGet)</span></span>
- <span data-ttu-id="05d71-179">啟用 Razor 語法支援的 Visual Studio 2010 的更新。</span><span class="sxs-lookup"><span data-stu-id="05d71-179">An update for Visual Studio 2010 that enables support for Razor syntax.</span></span> <span data-ttu-id="05d71-180">（如需詳細資訊，請參閱知識庫文件 2483190）。</span><span class="sxs-lookup"><span data-stu-id="05d71-180">(For details, see KnowledgeBase article 2483190.)</span></span>

<span data-ttu-id="05d71-181">下列 url 的 ASP.NET 網站上可以找到一組完整的每個發行前版本的 ASP.NET MVC 3 版本資訊：</span><span class="sxs-lookup"><span data-stu-id="05d71-181">The full set of release notes for each pre-release version of ASP.NET MVC 3 can be found on the ASP.NET website at the following URL:</span></span>

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a><span data-ttu-id="05d71-182">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="05d71-182">Installation Notes</span></span>

<span data-ttu-id="05d71-183">若要安裝 ASP.NET MVC 3 RTM 使用 Web Platform Installer (Web PI)，請造訪下列網頁：</span><span class="sxs-lookup"><span data-stu-id="05d71-183">To install ASP.NET MVC 3 RTM using the Web Platform Installer (Web PI), visit the following page:</span></span>

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

<span data-ttu-id="05d71-184">或者，您可以下載適用於 Visual Studio 2010 的 ASP.NET MVC 3 RTM 的安裝程式，從下列頁面：</span><span class="sxs-lookup"><span data-stu-id="05d71-184">Alternatively, you can download the installer for ASP.NET MVC 3 RTM for Visual Studio 2010 from the following page:</span></span>

https://go.microsoft.com/fwlink/?LinkID=208140

<span data-ttu-id="05d71-185">ASP.NET MVC 3 可以安裝並可執行的並行與 ASP.NET MVC 2。</span><span class="sxs-lookup"><span data-stu-id="05d71-185">ASP.NET MVC 3 can be installed and can run side-by-side with ASP.NET MVC 2.</span></span>

<a id="software-requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="05d71-186">軟體需求</span><span class="sxs-lookup"><span data-stu-id="05d71-186">Software Requirements</span></span>

<span data-ttu-id="05d71-187">ASP.NET MVC 3 執行階段元件需要下列軟體：</span><span class="sxs-lookup"><span data-stu-id="05d71-187">The ASP.NET MVC 3 run-time components require the following software:</span></span>

- <span data-ttu-id="05d71-188">.NET framework 第 4 版。</span><span class="sxs-lookup"><span data-stu-id="05d71-188">.NET Framework version 4.</span></span> 

    <span data-ttu-id="05d71-189">ASP.NET MVC 3 Visual Studio 2010 工具需要下列軟體：</span><span class="sxs-lookup"><span data-stu-id="05d71-189">ASP.NET MVC 3 Visual Studio 2010 tools require the following software:</span></span>
- <span data-ttu-id="05d71-190">Visual Studio 2010 或 Visual Web Developer 2010 Express。</span><span class="sxs-lookup"><span data-stu-id="05d71-190">Visual Studio 2010 or Visual Web Developer 2010 Express.</span></span>

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="05d71-191">文件</span><span class="sxs-lookup"><span data-stu-id="05d71-191">Documentation</span></span>

<span data-ttu-id="05d71-192">ASP.NET MVC 文件會提供在 MSDN 網站上下列 url:</span><span class="sxs-lookup"><span data-stu-id="05d71-192">Documentation for ASP.NET MVC is available on the MSDN Web site at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

<span data-ttu-id="05d71-193">教學課程和 ASP.NET MVC 的其他資訊可在 ASP.NET 網站的下列 URL [MVC] 頁面上：</span><span class="sxs-lookup"><span data-stu-id="05d71-193">Tutorials and other information about ASP.NET MVC are available on the MVC page of the ASP.NET Web site at the following URL:</span></span>

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a><span data-ttu-id="05d71-194">支援</span><span class="sxs-lookup"><span data-stu-id="05d71-194">Support</span></span>

<span data-ttu-id="05d71-195">這是完整支援的版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-195">This is a fully supported release.</span></span> <span data-ttu-id="05d71-196">取得技術支援人員的相關資訊，請參閱[Microsoft 技術支援網站](https://support.microsoft.com/)。</span><span class="sxs-lookup"><span data-stu-id="05d71-196">Information about getting technical support can be found at the [Microsoft Support website](https://support.microsoft.com/).</span></span>

<span data-ttu-id="05d71-197">也請放心將這一版的相關的問題張貼至 ASP.NET MVC 論壇，其中的 ASP.NET 社群成員可經常提供非正式的支援：</span><span class="sxs-lookup"><span data-stu-id="05d71-197">Also feel free to post questions about this release to the ASP.NET MVC forum, where members of the ASP.NET community are frequently able to provide informal support:</span></span>

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a><span data-ttu-id="05d71-198">ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="05d71-198">Upgrading an ASP.NET MVC 2 Project to ASP.NET MVC 3 Tools Update</span></span>

<span data-ttu-id="05d71-199">ASP.NET MVC 3 可以在同一部電腦，讓您彈性選擇何時要升級至 ASP.NET MVC 3 的 ASP.NET MVC 2 應用程式的 ASP.NET MVC 2 並存安裝。</span><span class="sxs-lookup"><span data-stu-id="05d71-199">ASP.NET MVC 3 can be installed side by side with ASP.NET MVC 2 on the same computer, which gives you flexibility in choosing when to upgrade an ASP.NET MVC 2 application to ASP.NET MVC 3.</span></span>

<span data-ttu-id="05d71-200">若要手動升級現有的 ASP.NET MVC 2 應用程式版本 3，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="05d71-200">To manually upgrade an existing ASP.NET MVC 2 application to version 3, do the following:</span></span>

1. <span data-ttu-id="05d71-201">在電腦上建立新的空白 ASP.NET MVC 3 專案。</span><span class="sxs-lookup"><span data-stu-id="05d71-201">Create a new empty ASP.NET MVC 3 project on your computer.</span></span> <span data-ttu-id="05d71-202">這個專案會包含所需升級的某些檔案。</span><span class="sxs-lookup"><span data-stu-id="05d71-202">This project will contain some files that are required for the upgrade.</span></span>
2. <span data-ttu-id="05d71-203">將下列檔案從 ASP.NET MVC 3 專案複製到您的 ASP.NET MVC 2 專案的對應位置。</span><span class="sxs-lookup"><span data-stu-id="05d71-203">Copy the following files from the ASP.NET MVC 3 project into the corresponding location of your ASP.NET MVC 2 project.</span></span> <span data-ttu-id="05d71-204">您將需要更新任何參考 jQuery 程式庫來說明新的檔案名稱 (jquery-1.5.1.js):</span><span class="sxs-lookup"><span data-stu-id="05d71-204">You'll need to update any references to the jQuery library to account for the new filename ( jQuery-1.5.1.js):</span></span> 

    - <span data-ttu-id="05d71-205">/Views/Web.config</span><span class="sxs-lookup"><span data-stu-id="05d71-205">/Views/Web.config</span></span>
    - <span data-ttu-id="05d71-206">/packages.config</span><span class="sxs-lookup"><span data-stu-id="05d71-206">/packages.config</span></span>
    - <span data-ttu-id="05d71-207">/scripts/\*.js</span><span class="sxs-lookup"><span data-stu-id="05d71-207">/scripts/\*.js</span></span>
    - <span data-ttu-id="05d71-208">/Content/themes/\*.\*</span><span class="sxs-lookup"><span data-stu-id="05d71-208">/Content/themes/\*.\*</span></span>
3. <span data-ttu-id="05d71-209">複製*封裝*根目錄中的空白 ASP.NET MVC 3 專案方案根目錄中的方案時，這是方案的.sln 檔案所在的目錄中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="05d71-209">Copy the *packages* folder in the root of the empty ASP.NET MVC 3 project solution into the root of your solution, which is in the directory where the solution's .sln file is located.</span></span>
4. <span data-ttu-id="05d71-210">如果您的 ASP.NET MVC 2 專案會包含任何區域，將 /Views/Web.config 檔案複製*檢視*之每個區域的資料夾。</span><span class="sxs-lookup"><span data-stu-id="05d71-210">If your ASP.NET MVC 2 project contains any areas, copy the /Views/Web.config file to the *Views* folder of each area.</span></span>
5. <span data-ttu-id="05d71-211">在這兩個 Web.config 檔案在 ASP.NET MVC 2 專案中，全域搜尋並取代 ASP.NET MVC 版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-211">In both Web.config files in the ASP.NET MVC 2 project, globally search and replace the ASP.NET MVC version.</span></span> <span data-ttu-id="05d71-212">搜尋下列文字：</span><span class="sxs-lookup"><span data-stu-id="05d71-212">Find the following:</span></span> 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    <span data-ttu-id="05d71-213">使用下列加以取代：</span><span class="sxs-lookup"><span data-stu-id="05d71-213">Replace it with the following:</span></span>

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. <span data-ttu-id="05d71-214">在 [方案總管] 中，刪除其參考*System.Web.Mvc* （指向 DLL 從第 2 版），然後將參考加入*System.Web.Mvc* (v3.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="05d71-214">In Solution Explorer, delete the reference to *System.Web.Mvc* (which points to the DLL from version 2), then add a reference to *System.Web.Mvc* (v3.0.0.0).</span></span>
7. <span data-ttu-id="05d71-215">將參考加入至 System.Web.WebPages.dll 和 system.web.helpers.dll 的參考。</span><span class="sxs-lookup"><span data-stu-id="05d71-215">Add a reference to System.Web.WebPages.dll and System.Web.Helpers.dll.</span></span> <span data-ttu-id="05d71-216">這些組件位於下列資料夾：</span><span class="sxs-lookup"><span data-stu-id="05d71-216">These assemblies are located in the following folders:</span></span> 

    - <span data-ttu-id="05d71-217">%ProgramFiles%\Microsoft ASP.NET\ASP.NET MVC 3\Assemblies</span><span class="sxs-lookup"><span data-stu-id="05d71-217">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies</span></span>
    - <span data-ttu-id="05d71-218">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies</span><span class="sxs-lookup"><span data-stu-id="05d71-218">%ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies</span></span>
8. <span data-ttu-id="05d71-219">在 方案總管 中，以滑鼠右鍵按一下專案名稱，並選取 卸載專案。</span><span class="sxs-lookup"><span data-stu-id="05d71-219">In Solution Explorer, right-click the project name and select Unload Project.</span></span> <span data-ttu-id="05d71-220">專案名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。</span><span class="sxs-lookup"><span data-stu-id="05d71-220">Then right-click the project name again and select Edit *ProjectName*.csproj.</span></span>
9. <span data-ttu-id="05d71-221">找出*ProjectTypeGuids*項目和取代 {F85E285D-A4E0-4152-9332-AB1D724D3325} 為 {E53F8FEA-EAE0-44A6-8774-FFD645390401}。</span><span class="sxs-lookup"><span data-stu-id="05d71-221">Locate the *ProjectTypeGuids* element and replace {F85E285D-A4E0-4152-9332-AB1D724D3325} with {E53F8FEA-EAE0-44A6-8774-FFD645390401}.</span></span>
10. <span data-ttu-id="05d71-222">儲存變更，以滑鼠右鍵按一下專案，然後選取重新載入專案。</span><span class="sxs-lookup"><span data-stu-id="05d71-222">Save the changes, right-click the project, and then select Reload Project.</span></span>
11. <span data-ttu-id="05d71-223">在應用程式的根 Web.config 檔案中，新增下列設定，才能*組件*> 一節。</span><span class="sxs-lookup"><span data-stu-id="05d71-223">In the application's root Web.config file, add the following settings to the *assemblies* section.</span></span> 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. <span data-ttu-id="05d71-224">如果專案參考使用 ASP.NET MVC 2 編譯任何協力廠商程式庫，加入下列反白顯示*bindingRedirect*  下的應用程式根目錄中的 Web.config 檔案的項目*組態*> 一節：</span><span class="sxs-lookup"><span data-stu-id="05d71-224">If the project references any third-party libraries that are compiled using ASP.NET MVC 2, add the following highlighted *bindingRedirect* element to the Web.config file in the application root under the *configuration* section:</span></span> 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a><span data-ttu-id="05d71-225">變更在 ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="05d71-225">Changes in ASP.NET MVC 3 Tools Update</span></span>

<span data-ttu-id="05d71-226">本節說明 ASP.NET MVC 3 RTM 發行版本以來，在 ASP.NET MVC 3 Tools Update 版本中所做的變更。</span><span class="sxs-lookup"><span data-stu-id="05d71-226">This section describes changes made in the ASP.NET MVC 3 Tools Update release since the ASP.NET MVC 3 RTM release.</span></span>

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a><span data-ttu-id="05d71-227">[加入控制器] 對話方塊現在可以 scaffold 控制器與檢視和資料存取程式碼</span><span class="sxs-lookup"><span data-stu-id="05d71-227">"Add Controller" dialog box can now scaffold controllers with views and data access code</span></span>

<span data-ttu-id="05d71-228">Scaffolding 是一種快速產生的控制器和檢視您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="05d71-228">Scaffolding is a way of quickly generating a controller and views for your application.</span></span> <span data-ttu-id="05d71-229">在產生的程式碼之後，您可以編輯它以符合您的專案需求。</span><span class="sxs-lookup"><span data-stu-id="05d71-229">After the code has been generated, you can edit it to meet your project's requirements.</span></span>

<span data-ttu-id="05d71-230">若要啟動*加入控制器*對話方塊中，在 ASP.NET MVC 3，以滑鼠右鍵按一下*控制器*資料夾中的*方案總管] 中*，按一下*新增*，然後按一下 [*控制器*。</span><span class="sxs-lookup"><span data-stu-id="05d71-230">To launch the *Add Controller* dialog box in ASP.NET MVC 3, right-click the *Controllers* folder in *Solution Explorer*, click *Add*, and then click *Controller*.</span></span> <span data-ttu-id="05d71-231">對話方塊中已經增強，可提供額外的 scaffolding 選項。</span><span class="sxs-lookup"><span data-stu-id="05d71-231">The dialog box has been enhanced to offer additional scaffolding options.</span></span>

![](mvc3-release-notes/_static/image1.png)

<span data-ttu-id="05d71-232">預設情況下有三個 scaffolding 範本可用。</span><span class="sxs-lookup"><span data-stu-id="05d71-232">There are three scaffolding templates available by default.</span></span>

#### <a name="empty-controller"></a><span data-ttu-id="05d71-233">空白控制器</span><span class="sxs-lookup"><span data-stu-id="05d71-233">Empty Controller</span></span>

<span data-ttu-id="05d71-234">此範本會產生空白的控制器檔案。</span><span class="sxs-lookup"><span data-stu-id="05d71-234">This template generates an empty controller file.</span></span> <span data-ttu-id="05d71-235">此範本就相當於不檢查*新增動作的建立、 編輯、 詳細資料、 刪除案例*在舊版 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="05d71-235">This template is equivalent to not checking *Add actions for create, edit, details, delete scenarios* in previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="05d71-236">如果您選擇此選項，沒有進一步的選項使用。</span><span class="sxs-lookup"><span data-stu-id="05d71-236">If you choose this, no further options are available.</span></span>

#### <a name="controller-with-empty-readwrite-actions"></a><span data-ttu-id="05d71-237">具有空白讀取/寫入動作的控制器</span><span class="sxs-lookup"><span data-stu-id="05d71-237">Controller with empty read/write actions</span></span>

<span data-ttu-id="05d71-238">此範本會產生在方法中有所有必要的動作方法，但沒有實作程式碼的控制器檔案。</span><span class="sxs-lookup"><span data-stu-id="05d71-238">This template generates a controller file that has all the required action methods but no implementation code in the methods.</span></span> <span data-ttu-id="05d71-239">此範本就相當於檢查*新增動作的建立、 編輯、 詳細資料、 刪除案例*在舊版 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="05d71-239">This template is equivalent to checking *Add actions for create, edit, details, delete scenarios* in previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="05d71-240">如果您選擇此選項，沒有進一步的選項使用。</span><span class="sxs-lookup"><span data-stu-id="05d71-240">If you choose this, no further options are available.</span></span>

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a><span data-ttu-id="05d71-241">控制器會讀取/寫入動作和檢視表、 使用 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="05d71-241">Controller with read/write actions and views, using Entity Framework</span></span>

<span data-ttu-id="05d71-242">此範本可讓您快速地建立工作資料輸入的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="05d71-242">This template enables you to quickly create a working data-entry user interface.</span></span> <span data-ttu-id="05d71-243">它會產生處理一組通用需求和案例，如下所示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="05d71-243">It generates code that handles a range of common requirements and scenarios, such as the following:</span></span>

- <span data-ttu-id="05d71-244">*資料存取*。</span><span class="sxs-lookup"><span data-stu-id="05d71-244">*Data access*.</span></span> <span data-ttu-id="05d71-245">產生的程式碼會讀取並寫入資料庫中的實體。</span><span class="sxs-lookup"><span data-stu-id="05d71-245">The generated code reads and writes entities in a database.</span></span> <span data-ttu-id="05d71-246">如果您選擇現有的資料內容類別，或是讓範本產生新的 Entity Framework Code First 方法運作*DbContext*類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-246">It works with the Entity Framework Code First approach if you choose an existing data context class or if you let the template generate a new *DbContext* class.</span></span> <span data-ttu-id="05d71-247">它也可以使用的 Entity Framework 資料庫優先 」 或 「 模型優先 」 方法如果您選擇現有*ObjectContext*類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-247">It also works with the Entity Framework Database First or Model First approach if you choose an existing *ObjectContext* class.</span></span>
- <span data-ttu-id="05d71-248">*驗證*。</span><span class="sxs-lookup"><span data-stu-id="05d71-248">*Validation*.</span></span> <span data-ttu-id="05d71-249">產生的程式碼會使用 ASP.NET MVC 模型繫結和中繼資料功能，以便提交表單會根據模型類別上宣告的規則進行驗證。</span><span class="sxs-lookup"><span data-stu-id="05d71-249">The generated code uses ASP.NET MVC model binding and metadata features so that form submissions are validated according to rules declared on your model class.</span></span> <span data-ttu-id="05d71-250">這包含內建驗證規則，例如*需要*和*StringLength*屬性和自訂驗證規則。</span><span class="sxs-lookup"><span data-stu-id="05d71-250">This includes built-in validation rules, such as the *Required* and *StringLength* attributes, and custom validation rules.</span></span>
- <span data-ttu-id="05d71-251">*一對多關聯性*。</span><span class="sxs-lookup"><span data-stu-id="05d71-251">*One-to-many relationships*.</span></span> <span data-ttu-id="05d71-252">如果模型類別之間定義一對多外部索引鍵關聯性，則產生的程式碼會產生下拉式清單中的選取相關的實體。</span><span class="sxs-lookup"><span data-stu-id="05d71-252">If you define one-to-many foreign-key relationships between your model classes, the generated code will produce drop-down lists for selecting related entities.</span></span> <span data-ttu-id="05d71-253">例如，您可能會定義下列模型類別遵循 Entity Framework Code First 慣例：</span><span class="sxs-lookup"><span data-stu-id="05d71-253">For example, you might define the following model classes following Entity Framework Code First conventions:</span></span> 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    <span data-ttu-id="05d71-254">當您接著 scaffold 控制器*產品*類別，它的檢視可讓使用者選擇*類別*物件給每個*產品*執行個體。</span><span class="sxs-lookup"><span data-stu-id="05d71-254">When you then scaffold a controller for the *Product* class, its views will allow users to choose a *Category* object for each *Product* instance.</span></span>

    <span data-ttu-id="05d71-255">此範本可讓中的其他選項*加入控制器* 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="05d71-255">This template enables additional options in the *Add Controller* dialog box.</span></span> <span data-ttu-id="05d71-256">如*模型類別*，您可以選擇任何模型類別，在您方案中，用來決定的使用者將能夠建立或編輯的資料類型：</span><span class="sxs-lookup"><span data-stu-id="05d71-256">For *Model class*, you can choose any model class in your solution, which determines the type of data that users will be able to create or edit:</span></span>
- <span data-ttu-id="05d71-257">如果您想要使用 Entity Framework Code First 時，您可以選擇任何模型類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-257">If you want to use Entity Framework Code First, you can choose any model class.</span></span>
- <span data-ttu-id="05d71-258">如果您使用 Entity Framework 資料庫優先 」 或 「 Entity Framework 模型優先，請務必選擇您概念模型中定義的實體類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-258">If you are using Entity Framework Database First or Entity Framework Model First, be sure to choose an entity class defined in your conceptual model.</span></span>

<span data-ttu-id="05d71-259">如*資料內容類別*，您可以進行下列選擇：</span><span class="sxs-lookup"><span data-stu-id="05d71-259">For *Data Context class*, you can make these choices:</span></span>

- <span data-ttu-id="05d71-260">如果您想要使用程式碼優先 」 而沒有現有的資料內容類別中，選擇 * * 新的資料內容 * *。</span><span class="sxs-lookup"><span data-stu-id="05d71-260">If you want to use Code First and have no existing data context class, choose **New data context **.</span></span> <span data-ttu-id="05d71-261">然後會為您產生資料內容類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-261">A data context class will then be generated for you.</span></span>
- <span data-ttu-id="05d71-262">如果您想要使用 Code First 和現有的資料內容類別，請在此選擇。</span><span class="sxs-lookup"><span data-stu-id="05d71-262">If you want to use Code First and have an existing data context class, choose it here.</span></span> <span data-ttu-id="05d71-263">它會進行更新以保存您選取的模型類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-263">It will be updated to persist the model class you have selected.</span></span>
- <span data-ttu-id="05d71-264">如果您使用 Database First 或 Model First，選擇您的物件內容類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-264">If you are using Database First or Model First, choose your object context class here.</span></span>

<span data-ttu-id="05d71-265">對於檢視，請選擇您要使用的檢視引擎，或選擇 [無] 如果您不要 scaffold 任何檢視。</span><span class="sxs-lookup"><span data-stu-id="05d71-265">For Views, choose the view engine you want to use, or choose None if you don't want to scaffold any views.</span></span>

<span data-ttu-id="05d71-266">您可以選取進階 Optionsto 指定其他選項來產生的檢視表。</span><span class="sxs-lookup"><span data-stu-id="05d71-266">You can select Advanced Optionsto specify further options for the generated views.</span></span> <span data-ttu-id="05d71-267">例如，您可以選擇版面配置頁或主版頁面，即可使用。</span><span class="sxs-lookup"><span data-stu-id="05d71-267">For example, you can choose the layout or master page to use.</span></span>

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a><span data-ttu-id="05d71-268">改善 「 ASP.NET MVC 3 新專案 」 對話方塊</span><span class="sxs-lookup"><span data-stu-id="05d71-268">Improvements to the "ASP.NET MVC 3 New Project" Dialog Box</span></span>

<span data-ttu-id="05d71-269">您用來建立新的 ASP.NET MVC 3 專案的對話方塊包含多項改良，如下所示。</span><span class="sxs-lookup"><span data-stu-id="05d71-269">The dialog box you use to create new ASP.NET MVC 3 projects includes multiple improvements, as listed below.</span></span>

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a><span data-ttu-id="05d71-270">新的 「 內部網路專案 」 範本</span><span class="sxs-lookup"><span data-stu-id="05d71-270">New "Intranet Project" Template</span></span>

<span data-ttu-id="05d71-271">專案範本清單中包含新的內部網路應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="05d71-271">The Project Template list includes a new Intranet Application template.</span></span> <span data-ttu-id="05d71-272">此範本包含設定來建置 web 應用程式使用 Windows 驗證，而不表單驗證。</span><span class="sxs-lookup"><span data-stu-id="05d71-272">This template contains settings for building a web application using Windows authentication instead of forms authentication.</span></span> <span data-ttu-id="05d71-273">因為內部網路應用程式需要部分 IIS 設定無法封裝專案範本中，範本包含讀我檔案，其中包含如何在 IIS 中運作，專案範本的指示。</span><span class="sxs-lookup"><span data-stu-id="05d71-273">Because an intranet application requires some IIS settings that can't be encapsulated in a project template, the template includes a readme file with instructions for how to make the project template work in IIS.</span></span> <span data-ttu-id="05d71-274">文件新的內部網路應用程式範本位於 MSDN 網站，位於下列 URL:</span><span class="sxs-lookup"><span data-stu-id="05d71-274">Documentation for the a new Intranet Application template is available on the MSDN website at the following URL:</span></span>

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a><span data-ttu-id="05d71-275">專案範本現在都已啟用 HTML5</span><span class="sxs-lookup"><span data-stu-id="05d71-275">Project templates are now HTML5 enabled</span></span>

<span data-ttu-id="05d71-276">[新增專案] 對話方塊現在包含一個選項以將 HTML5 專屬功能加入至專案範本。</span><span class="sxs-lookup"><span data-stu-id="05d71-276">The new-project dialog box now contains an option to add HTML5-specific features to the project templates.</span></span> <span data-ttu-id="05d71-277">選取此選項可讓要產生的檢視包含新的 HTML5 `<header>`， `<footer>`，和`<navigation>`項目。</span><span class="sxs-lookup"><span data-stu-id="05d71-277">Selecting the option causes views to be generated that contain the new HTML5 `<header>`, `<footer>`, and `<navigation>` elements.</span></span>

<span data-ttu-id="05d71-278">請注意，舊版的瀏覽器不支援 HTML5 專屬標記。</span><span class="sxs-lookup"><span data-stu-id="05d71-278">Note that earlier versions of browsers do not support HTML5-specific tags.</span></span> <span data-ttu-id="05d71-279">若要解決這項限制，HTML5 專案範本包含 Modernizr 程式庫的參考。</span><span class="sxs-lookup"><span data-stu-id="05d71-279">To address this limitation, the HTML5 project templates include a reference to the Modernizr library.</span></span> <span data-ttu-id="05d71-280">（請參閱下一節）。</span><span class="sxs-lookup"><span data-stu-id="05d71-280">(See the next section.)</span></span>

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a><span data-ttu-id="05d71-281">專案範本現在包含 Modernizr 1.7</span><span class="sxs-lookup"><span data-stu-id="05d71-281">Project templates now include Modernizr 1.7</span></span>

<span data-ttu-id="05d71-282">Modernizr 是 JavaScript 程式庫可支援 CSS 3 和 HTML5 的瀏覽器目前還不支援這些功能。</span><span class="sxs-lookup"><span data-stu-id="05d71-282">Modernizr is a JavaScript library that enables support for CSS 3 and HTML5 in browsers that do not yet support these features.</span></span> <span data-ttu-id="05d71-283">此程式庫是在 ASP.NET MVC 3 專案範本中預先安裝的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="05d71-283">This library is included as a pre-installed NuGet package in templates for ASP.NET MVC 3 projects.</span></span> <span data-ttu-id="05d71-284">如需 Modernizr 的詳細資訊，請參閱[ http://www.modernizr.com/ ](http://www.modernizr.com/)。</span><span class="sxs-lookup"><span data-stu-id="05d71-284">For more information about Modernizr, see [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a><span data-ttu-id="05d71-285">專案範本包含 jQuery、 jQuery UI 和 jQuery 的更新的版本驗證</span><span class="sxs-lookup"><span data-stu-id="05d71-285">Project templates include updated versions of jQuery, jQuery UI, and jQuery Validation</span></span>

<span data-ttu-id="05d71-286">專案範本現在包含下列 jQuery 指令碼版本：</span><span class="sxs-lookup"><span data-stu-id="05d71-286">The project templates now include the following versions of the jQuery scripts:</span></span>

- <span data-ttu-id="05d71-287">1.5.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="05d71-287">jQuery 1.5.1</span></span>
- <span data-ttu-id="05d71-288">jQuery 驗證 1.8</span><span class="sxs-lookup"><span data-stu-id="05d71-288">jQuery Validation 1.8</span></span>
- <span data-ttu-id="05d71-289">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="05d71-289">jQuery UI 1.8.11</span></span>

<span data-ttu-id="05d71-290">這些程式庫會包含為預先安裝的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="05d71-290">These libraries are included as pre-installed NuGet packages.</span></span>

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a><span data-ttu-id="05d71-291">專案範本現在包含 ADO.NET Entity Framework 4.1 以預先安裝的 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="05d71-291">Project templates now include ADO.NET Entity Framework 4.1 as a pre-installed NuGet package</span></span>

<span data-ttu-id="05d71-292">ADO.NET Entity Framework 4.1 包含第一個程式碼的功能。</span><span class="sxs-lookup"><span data-stu-id="05d71-292">The ADO.NET Entity Framework 4.1 includes the Code First feature.</span></span> <span data-ttu-id="05d71-293">程式碼第一次是為現有的第一個資料庫和 Model First 模式提供替代的 ADO.NET Entity Framework 的新開發模式。</span><span class="sxs-lookup"><span data-stu-id="05d71-293">Code First is a new development pattern for the ADO.NET Entity Framework that provides an alternative to the existing Database First and Model First patterns.</span></span>

<span data-ttu-id="05d71-294">程式碼第一次 」 的重點定義模型使用在 Visual Basic 或 C# 撰寫的 POCO 類別 （「 純舊 CLR 物件 」）。</span><span class="sxs-lookup"><span data-stu-id="05d71-294">Code First is focused around defining your model using POCO classes ("plain old CLR objects") written in Visual Basic or C#.</span></span> <span data-ttu-id="05d71-295">這些類別接著可以對應到現有的資料庫，或用來產生資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="05d71-295">These classes can then be mapped to an existing database or be used to generate a database schema.</span></span> <span data-ttu-id="05d71-296">可以提供額外的設定，使用*DataAnnotations*屬性，或使用 fluent 應用程式開發的應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="05d71-296">Additional configuration can be supplied using *DataAnnotations* attributes or using fluent APIs.</span></span>

<span data-ttu-id="05d71-297">使用程式碼 Firstwith ASP.NET MVC 的文件位於 ASP.NET 網站，在下列 Url:</span><span class="sxs-lookup"><span data-stu-id="05d71-297">Documentation for using Code Firstwith ASP.NET MVC is available on the ASP.NET website at the following URLs:</span></span>

<span data-ttu-id="05d71-298">[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="05d71-298">[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)</span></span>

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a><span data-ttu-id="05d71-299">專案範本會納入預先安裝的 NuGet 套件中的 JavaScript 程式庫</span><span class="sxs-lookup"><span data-stu-id="05d71-299">Project templates include JavaScript libraries as pre-installed NuGet packages</span></span>

<span data-ttu-id="05d71-300">當您建立新的 ASP.NET MVC 3 專案時，專案會先安裝包含之前提到的 JavaScript 檔案 （例如 Modernizr 程式庫） 使用 NuGet 而非直接在專案範本中的指令碼 資料夾中新增指令碼內容。</span><span class="sxs-lookup"><span data-stu-id="05d71-300">When you create a new ASP.NET MVC 3 project, the project includes the JavaScript files mentioned previously (for example, the Modernizr library) by installing them using NuGet instead of directly adding the scripts to the Scripts folder in the project template contents.</span></span> <span data-ttu-id="05d71-301">這可讓您使用 NuGet 將指令碼更新為最新版本發行新版本的指令碼時。</span><span class="sxs-lookup"><span data-stu-id="05d71-301">This enables you to use NuGet to update the scripts to the latest version when new versions of the scripts are released.</span></span>

<span data-ttu-id="05d71-302">例如，提供新的 jQuery 版本的頻率，專案範本中所包含的 jQuery 版本在某個時間點會過期。</span><span class="sxs-lookup"><span data-stu-id="05d71-302">For example, given the frequency of new jQuery releases, the version of jQuery included in the project template will at some point be out of date.</span></span> <span data-ttu-id="05d71-303">不過，因為 jQuery 是隨附安裝的 NuGet 套件，系統會通知您 NuGet 對話方塊中可用的 jQuery 新版時。</span><span class="sxs-lookup"><span data-stu-id="05d71-303">However, because jQuery is included as an installed NuGet package, you will be notified in the NuGet dialog box when newer versions of jQuery are available.</span></span>

<span data-ttu-id="05d71-304">因為 jQuery 在檔案名稱中包含的版本號碼，jQuery 更新為最新版本也需要更新`<script>`參考 jQuery 檔案，才能使用新的檔案名稱的標記。</span><span class="sxs-lookup"><span data-stu-id="05d71-304">Because jQuery includes the version number in the file name, updating jQuery to the latest version also requires updating the `<script>` tag that references the jQuery file to use the new file name.</span></span> <span data-ttu-id="05d71-305">其他內含的指令碼程式庫不包含版本號碼中的指令碼名稱，因此可以更輕鬆地其最新版本更新。</span><span class="sxs-lookup"><span data-stu-id="05d71-305">Other included script libraries do not include the version number in the script name, so they can be more easily updated to their latest versions.</span></span>

<a id="tu-KI"></a>
## <a name="known-issues"></a><span data-ttu-id="05d71-306">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-306">Known Issues</span></span>

- <span data-ttu-id="05d71-307">在某些情況下，安裝可能會因錯誤訊息 「 安裝失敗，錯誤碼為 (0x80070643) 」。</span><span class="sxs-lookup"><span data-stu-id="05d71-307">In some cases, installation may fail with the error message "Installation failed with error code (0x80070643)".</span></span> <span data-ttu-id="05d71-308">如需如何解決此問題的資訊，請參閱[知識庫文件 2531566](https://support.microsoft.com/kb/2531566)。</span><span class="sxs-lookup"><span data-stu-id="05d71-308">For information about how to work around this issue, see [KnowledgeBase article 2531566](https://support.microsoft.com/kb/2531566).</span></span>
- <span data-ttu-id="05d71-309">用於加入控制器的 scaffolding 不會 scaffold 利用 Entity Framework 實體繼承支援的實體。</span><span class="sxs-lookup"><span data-stu-id="05d71-309">The scaffolding for adding a controller does not scaffold entities that take advantage of entity inheritance support within Entity Framework.</span></span> <span data-ttu-id="05d71-310">例如，假設基底*人員*類別所繼承*學生*類別，scaffolding*學生*類別將導致產生不會編譯的程式碼。</span><span class="sxs-lookup"><span data-stu-id="05d71-310">For example, given a base *Person* class that's inherited by a *Student* class, scaffolding the *Student* class will result in generated code that does not compile.</span></span>
- <span data-ttu-id="05d71-311">建立新的 ASP.NET MVC 3 專案的方案資料夾內原因*NullReferenceException*錯誤。</span><span class="sxs-lookup"><span data-stu-id="05d71-311">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="05d71-312">因應措施是在方案根目錄中建立 ASP.NET MVC 3 專案，然後將它移至方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="05d71-312">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="05d71-313">時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。</span><span class="sxs-lookup"><span data-stu-id="05d71-313">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="05d71-314">如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) （英文） 部落格上的討論如何一起搭配使用今天。</span><span class="sxs-lookup"><span data-stu-id="05d71-314">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="05d71-315">在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。</span><span class="sxs-lookup"><span data-stu-id="05d71-315">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="05d71-316">當您編輯 Razor 檢視 (.cshtml 或。*vbhtml*檔案)，檢視。</span><span class="sxs-lookup"><span data-stu-id="05d71-316">When you are editing a Razor view (.cshtml or .*vbhtml* file), views.</span></span> <span data-ttu-id="05d71-317">ASP.NET MVC 3 不包含 Razor 檢視的任何程式碼片段...aspxselecting ASP.NET MVC 的程式碼片段會顯示程式碼的片段</span><span class="sxs-lookup"><span data-stu-id="05d71-317">ASP.NET MVC 3 does not include any snippets for Razor views..aspxselecting a code snippet for ASP.NET MVC will show snippets for</span></span>
- <span data-ttu-id="05d71-318">如果在安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="05d71-318">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="05d71-319">Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。</span><span class="sxs-lookup"><span data-stu-id="05d71-319">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="05d71-320">如果您沒有 Visual Web Developer Express 以及然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，就會發生相同問題。</span><span class="sxs-lookup"><span data-stu-id="05d71-320">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a><span data-ttu-id="05d71-321">在 ASP.NET MVC 3 rtm 版本中的變更</span><span class="sxs-lookup"><span data-stu-id="05d71-321">Changes in ASP.NET MVC 3 RTM</span></span>

<span data-ttu-id="05d71-322">本章節描述變更及 RC2 版本之後進行 ASP.NET MVC 3 RTM 版本中的 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="05d71-322">This section describes changes and bug fixes made in the ASP.NET MVC 3 RTM release since the RC2 release.</span></span>

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a><span data-ttu-id="05d71-323">變更： 更新至 1.8.7 jQuery UI 的版本</span><span class="sxs-lookup"><span data-stu-id="05d71-323">Change: Updated the version of jQuery UI to 1.8.7</span></span>

<span data-ttu-id="05d71-324">Visual Studio 的 ASP.NET MVC 專案範本已更新以包含 jQuery UI 程式庫的最新版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-324">The ASP.NET MVC project templates for Visual Studio were updated to include the latest version of the jQuery UI library.</span></span> <span data-ttu-id="05d71-325">範本也包含最少的 jQuery UI，例如相關聯的 CSS 和影像檔所需的資源檔案集。</span><span class="sxs-lookup"><span data-stu-id="05d71-325">The templates also include the minimal set of resource files required by jQuery UI, such as the associated CSS and image files.</span></span>

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a><span data-ttu-id="05d71-326">變更： 變更的預設 ModelMetadataProvider 回 DataAnnotationsModelMetadataProvider</span><span class="sxs-lookup"><span data-stu-id="05d71-326">Change: Changed the default ModelMetadataProvider back to DataAnnotationsModelMetadataProvider</span></span>

<span data-ttu-id="05d71-327">RC2 版本的 ASP.NET MVC 3 引進*CachedDataAnnotationsMetadataProvider*類別提供的快取之上現有*DataAnnotationsModelMetadataProvider*類別做為效能改善。</span><span class="sxs-lookup"><span data-stu-id="05d71-327">The RC2 release of ASP.NET MVC 3 introduced a *CachedDataAnnotationsMetadataProvider* class that provided caching on top of the existing *DataAnnotationsModelMetadataProvider* class as a performance improvement.</span></span> <span data-ttu-id="05d71-328">不過，某些錯誤報告與這個實作中，讓變更已還原，且已移至 MVC 未來專案中，將會位於[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。</span><span class="sxs-lookup"><span data-stu-id="05d71-328">However, some bugs were reported with this implementation, so the change has been reverted and moved into the MVC Futures project, which is available at [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).</span></span>

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a><span data-ttu-id="05d71-329">已修正︰ 貼上含有空白結果中要反轉的 Razor 運算式的一部分</span><span class="sxs-lookup"><span data-stu-id="05d71-329">Fixed: Pasting part of a Razor expression that contains whitespace results in it being reversed</span></span>

<span data-ttu-id="05d71-330">在 ASP.NET MVC 3 的發行前版本中，當您將貼到 Razor 檔案，包含空格的 Razor 運算式的一部分會反轉所產生的運算式。</span><span class="sxs-lookup"><span data-stu-id="05d71-330">In pre-release versions of ASP.NET MVC 3, when you paste a part of a Razor expression that contains whitespace into a Razor file, the resulting expression is reversed.</span></span> <span data-ttu-id="05d71-331">例如，請考慮下列的 Razor 程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="05d71-331">For example, consider the following Razor code block:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

<span data-ttu-id="05d71-332">如果您選取文字 「 第一個參數 「 第一個方法，並將它做為引數貼到第二種方法，結果如下所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-332">If you select the text "first param" in the first method and paste it as an argument into the second method, the result is as follows:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

<span data-ttu-id="05d71-333">正確的行為是，貼上作業應該會導致下列：</span><span class="sxs-lookup"><span data-stu-id="05d71-333">The correct behavior is that the paste operation should result in the following:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

<span data-ttu-id="05d71-334">以便正確貼上作業期間保留運算式已經在 RTM 版本中修正此問題。</span><span class="sxs-lookup"><span data-stu-id="05d71-334">This issue has been fixed in the RTM release so that the expression is correctly preserved during the paste operation.</span></span>

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a><span data-ttu-id="05d71-335">已修正︰ 重新命名 Razor 檔案在編輯器中開啟時，會停用語法顏色標示和 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="05d71-335">Fixed: Renaming a Razor file that is opened in the editor disables syntax colorization and IntelliSense</span></span>

<span data-ttu-id="05d71-336">重新命名 Razor 檔案在編輯器視窗中開啟的檔案時，使用方案總管 會反白顯示語法和 IntelliSense 無法運作，該檔案。</span><span class="sxs-lookup"><span data-stu-id="05d71-336">Renaming a Razor file using Solution Explorer while the file is opened in the editor window causes syntax highlighting and IntelliSense to stop working for that file.</span></span> <span data-ttu-id="05d71-337">此問題已修正，以便反白顯示，IntelliSense 會維護，重新命名後。</span><span class="sxs-lookup"><span data-stu-id="05d71-337">This has been fixed so that highlighting and IntelliSense are maintained after a rename.</span></span>

<a id="RTM-KI"></a>
## <a name="known-issues"></a><span data-ttu-id="05d71-338">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-338">Known Issues</span></span>

- <span data-ttu-id="05d71-339">如果您關閉 Visual Studio 2010 SP1 Beta，NuGet 封裝管理員主控台開啟時，Visual Studio 會損毀，並嘗試重新啟動。</span><span class="sxs-lookup"><span data-stu-id="05d71-339">If you close Visual Studio 2010 SP1 Beta while the NuGet Package Manager Console is open, Visual Studio crashes and attempts to restart.</span></span> <span data-ttu-id="05d71-340">將會修正此問題在 RTM 版本的 Visual Studio 2010 SP1。</span><span class="sxs-lookup"><span data-stu-id="05d71-340">This will be fixed in the RTM release of Visual Studio 2010 SP1.</span></span>
- <span data-ttu-id="05d71-341">ASP.NET MVC 3 安裝程式，才能夠安裝的 NuGet 套件管理員的初始版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-341">The ASP.NET MVC 3 installer is only able to install an initial version of the NuGet package manager.</span></span> <span data-ttu-id="05d71-342">您已安裝的初始版本之後，可以安裝 NuGet，並使用 Visual Studio 擴充功能管理員更新。</span><span class="sxs-lookup"><span data-stu-id="05d71-342">After you have installed the initial version, NuGet can be installed and updated using Visual Studio Extension Manager.</span></span> <span data-ttu-id="05d71-343">如果您已經安裝 NuGet，請移至更新至最新版的 NuGet Visual Studio 擴充程式庫。</span><span class="sxs-lookup"><span data-stu-id="05d71-343">If you already have NuGet installed, go to the Visual Studio Extension Gallery to update to the latest version of NuGet.</span></span>
- <span data-ttu-id="05d71-344">建立新的 ASP.NET MVC 3 專案的方案資料夾內原因*NullReferenceException*錯誤。</span><span class="sxs-lookup"><span data-stu-id="05d71-344">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="05d71-345">因應措施是在方案根目錄中建立 ASP.NET MVC 3 專案，然後將它移至方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="05d71-345">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="05d71-346">安裝程式可能需要更長的時間比舊版 ASP.NET MVC，才能完成。</span><span class="sxs-lookup"><span data-stu-id="05d71-346">The installer might take much longer than previous versions of ASP.NET MVC to complete.</span></span> <span data-ttu-id="05d71-347">這是因為它會更新 Visual Studio 2010 的元件。</span><span class="sxs-lookup"><span data-stu-id="05d71-347">This is because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="05d71-348">時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。</span><span class="sxs-lookup"><span data-stu-id="05d71-348">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="05d71-349">如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) （英文） 部落格上的討論如何一起搭配使用今天。</span><span class="sxs-lookup"><span data-stu-id="05d71-349">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="05d71-350">建立 ASP.NET MVC 3 的 Beta 版本的 CCSHTML 和 VBHTML 檢視並沒有正確地設定其建置動作，與這些檢視的結果類型會省略發行專案時。</span><span class="sxs-lookup"><span data-stu-id="05d71-350">CCSHTML and VBHTML views created with the Beta version of ASP.NET MVC 3 do not have their build action set correctly, with the result that these view types are omitted when the project is published.</span></span> <span data-ttu-id="05d71-351">這些檔案的建置動作值應該設定為 「 內容 」 中。</span><span class="sxs-lookup"><span data-stu-id="05d71-351">The Build Action value for these files should be set to "Content".</span></span> <span data-ttu-id="05d71-352">ASP.NET MVC 3 RTM 修正此問題，對於新的檔案，但不會更正專案發行前版本建立的現有檔案的設定。</span><span class="sxs-lookup"><span data-stu-id="05d71-352">ASP.NET MVC 3 RTM fixes this issue for new files, but doesn't correct the setting for existing files for a project created with prerelease versions.</span></span>
- ![](mvc3-release-notes/_static/image3.png)
- <span data-ttu-id="05d71-353">在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。</span><span class="sxs-lookup"><span data-stu-id="05d71-353">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="05d71-354">當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器功能表項目，在 Visual Studio 中的將無法使用，而且沒有任何程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="05d71-354">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>
- <span data-ttu-id="05d71-355">如果在安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="05d71-355">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="05d71-356">Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。</span><span class="sxs-lookup"><span data-stu-id="05d71-356">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="05d71-357">如果您沒有 Visual Web Developer Express 以及然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，就會發生相同問題。</span><span class="sxs-lookup"><span data-stu-id="05d71-357">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a><span data-ttu-id="05d71-358">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-358">Breaking Changes</span></span>

- <span data-ttu-id="05d71-359">在舊版 ASP.NET MVC 中，每個要求除了在少數情況下建立的篩選條件的動作。</span><span class="sxs-lookup"><span data-stu-id="05d71-359">In previous versions of ASP.NET MVC, action filters are create per request except in a few cases.</span></span> <span data-ttu-id="05d71-360">這種行為是不保證的行為，但只是實作詳細資料，就是把它視為無狀態篩選器的合約。</span><span class="sxs-lookup"><span data-stu-id="05d71-360">This behavior was never a guaranteed behavior but merely an implementation detail and the contract for filters was to consider them stateless.</span></span> <span data-ttu-id="05d71-361">在 ASP.NET MVC 3 中，篩選是更積極地快取。</span><span class="sxs-lookup"><span data-stu-id="05d71-361">In ASP.NET MVC 3, filters are cached more aggressively.</span></span> <span data-ttu-id="05d71-362">因此，任何自訂動作篩選條件不正確儲存執行個體的狀態可能已損壞。</span><span class="sxs-lookup"><span data-stu-id="05d71-362">Therefore, any custom action filters which improperly store instance state might be broken.</span></span>
- <span data-ttu-id="05d71-363">例外狀況篩選條件的執行順序已變更為具有相同的例外狀況篩選條件*順序*值。</span><span class="sxs-lookup"><span data-stu-id="05d71-363">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="05d71-364">在 ASP.NET MVC 2 和舊版中，例外狀況篩選條件具有相同的控制站上*順序*值上執行的動作方法執行時，可以在動作方法的例外狀況篩選條件之前。</span><span class="sxs-lookup"><span data-stu-id="05d71-364">In ASP.NET MVC 2 and earlier, exception filters on the controller that have the same *Order* value as those on an action method are executed before the exception filters on the action method.</span></span> <span data-ttu-id="05d71-365">這通常會發生此情況時例外狀況篩選條件會套用但未指定*順序*值。</span><span class="sxs-lookup"><span data-stu-id="05d71-365">This would typically be the case when exception filters are applied without a specified *Order* value.</span></span> <span data-ttu-id="05d71-366">在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。</span><span class="sxs-lookup"><span data-stu-id="05d71-366">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="05d71-367">如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。</span><span class="sxs-lookup"><span data-stu-id="05d71-367">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="05d71-368">新的屬性，名為*FileExtensions*已新增至*VirtualPathProviderViewEngine*基底類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-368">A new property named *FileExtensions* was added to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="05d71-369">當 ASP.NET 尋找檢視的路徑 （不是依名稱） 時，則會被視為只有檢視與此新屬性所指定的清單中所包含的副檔名。</span><span class="sxs-lookup"><span data-stu-id="05d71-369">When ASP.NET looks up a view by path (not by name), only views with a file extension contained in the list specified by this new property are considered.</span></span> <span data-ttu-id="05d71-370">這是應用程式中的重大變更，自訂組建提供者註冊以啟用 Web 表單檢視自訂檔案的副檔名，而且提供者所使用的完整路徑，而不是名稱參考這些檢視表。</span><span class="sxs-lookup"><span data-stu-id="05d71-370">This is a breaking change in applications where a custom build provider is registered in order to enable a custom file extension for Web Form views and where the provider references those views by using a full path rather than a name.</span></span> <span data-ttu-id="05d71-371">因應措施是要修改的值*FileExtensions*屬性設定為包含自訂檔案的副檔名。</span><span class="sxs-lookup"><span data-stu-id="05d71-371">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>
- <span data-ttu-id="05d71-372">直接實作的自訂控制器 factory 實作*IControllerFactory*介面必須提供新的實作*GetControllerSessionBehavior*方法加入至這一版中的介面。</span><span class="sxs-lookup"><span data-stu-id="05d71-372">Custom controller factory implementations that directly implement the *IControllerFactory* interface must provide an implementation of the new *GetControllerSessionBehavior* method that was added to the interface in this release.</span></span> <span data-ttu-id="05d71-373">一般情況下，建議您不要不直接實作這個介面，並且改為衍生您的類別，從*DefaultControllerFactory*。</span><span class="sxs-lookup"><span data-stu-id="05d71-373">In general, it is recommended that you do not implement this interface directly and instead derive your class from *DefaultControllerFactory*.</span></span>

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a><span data-ttu-id="05d71-374">ASP.NET MVC 3 RC2 中的變更</span><span class="sxs-lookup"><span data-stu-id="05d71-374">Changes in ASP.NET MVC 3 RC2</span></span>

<span data-ttu-id="05d71-375">本章節描述 （新功能及 bug 修正） 所做的變更在 ASP.NET MVC 3 RC2 版本的 RC 版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-375">This section describes changes (new features and bug fixes) made in the ASP.NET MVC 3 RC2 release since the RC release.</span></span>

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a><span data-ttu-id="05d71-376">專案範本變更為包含 jQuery 1.4.4、 jQuery 驗證 1.7 和 jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="05d71-376">Project Templates Changed to Include jQuery 1.4.4, jQuery Validation 1.7, and jQuery UI 1.8.6</span></span>

<span data-ttu-id="05d71-377">ASP.NET MVC 3 的專案範本現在包含最新版的 jQuery、 jQuery、 驗證和 jQuery UI。</span><span class="sxs-lookup"><span data-stu-id="05d71-377">The project templates for ASP.NET MVC 3 now include the latest versions of jQuery, jQuery Validation, and jQuery UI.</span></span> <span data-ttu-id="05d71-378">jQuery UI 是新增至專案範本，並提供實用的使用者介面的 widget。</span><span class="sxs-lookup"><span data-stu-id="05d71-378">jQuery UI is a new addition to the project templates and provides useful user interface widgets.</span></span> <span data-ttu-id="05d71-379">如需 jQuery UI 的詳細資訊，請瀏覽其/首頁： [ http://jqueryui.com/ ](http://jqueryui.com/)。</span><span class="sxs-lookup"><span data-stu-id="05d71-379">For more information about jQuery UI, visit their homepage: [http://jqueryui.com/](http://jqueryui.com/).</span></span>

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a><span data-ttu-id="05d71-380">已新增 「 AdditionalMetadataAttribute 」 類別</span><span class="sxs-lookup"><span data-stu-id="05d71-380">Added "AdditionalMetadataAttribute" Class</span></span>

<span data-ttu-id="05d71-381">您可以使用*AdditionalMetadataAttribute*類別，以填入*ModelMetadata.AdditionalValues*模型屬性的字典。</span><span class="sxs-lookup"><span data-stu-id="05d71-381">You can use the *AdditionalMetadataAttribute* class to populate the *ModelMetadata.AdditionalValues* dictionary for a model property.</span></span>

<span data-ttu-id="05d71-382">例如，假設 「 檢視模型 」 具有應該只會顯示系統管理員的屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-382">For example, suppose a view model has properties that should be displayed only to an administrator.</span></span> <span data-ttu-id="05d71-383">該模型可以標註具有新屬性作為索引鍵和 true 使用 AdminOnly 當做值，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-383">That model can be annotated with the new attribute using AdminOnly as the key and true as the value, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

<span data-ttu-id="05d71-384">產品檢視模型呈現時，才可以提供給任何顯示或編輯器範本此中繼資料。</span><span class="sxs-lookup"><span data-stu-id="05d71-384">This metadata is made available to any display or editor template when a product view model is rendered.</span></span> <span data-ttu-id="05d71-385">它是由您決定將解譯的中繼資料資訊的應用程式開發人員。</span><span class="sxs-lookup"><span data-stu-id="05d71-385">It is up to you as application developer to interpret the metadata information.</span></span>

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a><span data-ttu-id="05d71-386">改善的檢視 Scaffolding</span><span class="sxs-lookup"><span data-stu-id="05d71-386">Improved View Scaffolding</span></span>

<span data-ttu-id="05d71-387">現在使用 scaffolding 檢視 T4 範本產生範本協助程式方法的呼叫例如*EditorFor*而不是協助程式，例如*TextBoxFor*。</span><span class="sxs-lookup"><span data-stu-id="05d71-387">The T4 templates used for scaffolding views now generate calls to template helper methods such as *EditorFor* instead of helpers such as *TextBoxFor*.</span></span> <span data-ttu-id="05d71-388">這項變更可改善資料附註屬性的形式在模型上的中繼資料的支援時加入檢視 對話方塊產生的檢視。</span><span class="sxs-lookup"><span data-stu-id="05d71-388">This change improves support for metadata on the model in the form of data annotation attributes when the Add View dialog box generates a view.</span></span>

<span data-ttu-id="05d71-389">加入檢視 scaffolding 也會包含改良的偵測和慣例為基礎的模型上的主索引鍵資訊的使用方式。</span><span class="sxs-lookup"><span data-stu-id="05d71-389">The Add View scaffolding also includes improved detection and usage of primary key information on the model, based on convention.</span></span> <span data-ttu-id="05d71-390">例如，加入檢視 對話方塊會使用此資訊以確保主要金鑰的值不會建立結構做為可編輯的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="05d71-390">For example, the Add View dialog box uses this information to ensure that the primary key value is not scaffolded as an editable form field.</span></span>

<span data-ttu-id="05d71-391">預設編輯和建立範本包含 jQuery 指令碼所需的用戶端驗證的參考。</span><span class="sxs-lookup"><span data-stu-id="05d71-391">The default Edit and Create templates include references to the jQuery scripts needed for client validation.</span></span>

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a><span data-ttu-id="05d71-392">加入的 Html.Raw 方法</span><span class="sxs-lookup"><span data-stu-id="05d71-392">Added Html.Raw Method</span></span>

<span data-ttu-id="05d71-393">根據預設，Razor 檢視引擎將 HTML 編碼所有的值。</span><span class="sxs-lookup"><span data-stu-id="05d71-393">By default, the Razor view engine HTML-encodes all values.</span></span> <span data-ttu-id="05d71-394">例如，下列程式碼片段將編碼的問候語變數內的 HTML，使它顯示在網頁中做`<strong>Hello World!</strong>`。</span><span class="sxs-lookup"><span data-stu-id="05d71-394">For example, the following code snippet encodes the HTML inside the greeting variable so that it is displayed in the page as `<strong>Hello World!</strong>`.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

<span data-ttu-id="05d71-395">新*Html.Raw*方法提供簡單的方法的內容是否已知為安全時，顯示未編碼的 HTML。</span><span class="sxs-lookup"><span data-stu-id="05d71-395">The new *Html.Raw* method provides a simple way of displaying unencoded HTML when the content is known to be safe.</span></span> <span data-ttu-id="05d71-396">下列範例顯示相同的字串，但是字串會轉譯為標記：</span><span class="sxs-lookup"><span data-stu-id="05d71-396">The following example displays the same string, but the string is rendered as markup:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a><span data-ttu-id="05d71-397">重新命名 「 Controller.ViewModel"屬性，「 檢視 」 屬性設定為"ViewBag"</span><span class="sxs-lookup"><span data-stu-id="05d71-397">Renamed "Controller.ViewModel" Property and the "View" Property To "ViewBag"</span></span>

<span data-ttu-id="05d71-398">先前， *ViewModel*屬性*控制器*採納*檢視*檢視的屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-398">Previously, the *ViewModel* property of *Controller* corresponded to the *View* property of a view.</span></span> <span data-ttu-id="05d71-399">這兩個屬性提供一個方式來存取值*ViewDataDictionary*物件使用動態屬性存取子語法。</span><span class="sxs-lookup"><span data-stu-id="05d71-399">Both of these properties provide a way to access values of the *ViewDataDictionary* object using dynamic property-accessor syntax.</span></span> <span data-ttu-id="05d71-400">這兩個屬性已重新命名，以避免混淆，並會更一致必須相同。</span><span class="sxs-lookup"><span data-stu-id="05d71-400">Both properties have been renamed to be the same in order to avoid confusion and to be more consistent.</span></span>

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a><span data-ttu-id="05d71-401">重新命名 「 ControllerSessionStateAttribute 「 類別 」 SessionStateAttribute"</span><span class="sxs-lookup"><span data-stu-id="05d71-401">Renamed "ControllerSessionStateAttribute" Class to "SessionStateAttribute"</span></span>

<span data-ttu-id="05d71-402">*ControllerSessionStateAttribute*類別在 ASP.NET MVC 3 RC 版本中引進。</span><span class="sxs-lookup"><span data-stu-id="05d71-402">The *ControllerSessionStateAttribute* class was introduced in the RC release of ASP.NET MVC 3.</span></span> <span data-ttu-id="05d71-403">屬性已重新命名為更簡潔。</span><span class="sxs-lookup"><span data-stu-id="05d71-403">The property was renamed to be more succinct.</span></span>

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a><span data-ttu-id="05d71-404">重新命名的 RemoteAttribute 「 欄位 」 屬性 「 AdditionalFields"</span><span class="sxs-lookup"><span data-stu-id="05d71-404">Renamed RemoteAttribute "Fields" Property to "AdditionalFields"</span></span>

<span data-ttu-id="05d71-405">*RemoteAttribute*類別的*欄位*屬性造成使用者混淆。</span><span class="sxs-lookup"><span data-stu-id="05d71-405">The *RemoteAttribute* class's *Fields* property caused some confusion among users.</span></span> <span data-ttu-id="05d71-406">重新命名此屬性為*AdditionalFields*將釐清其意圖。</span><span class="sxs-lookup"><span data-stu-id="05d71-406">Renaming this property to *AdditionalFields* clarifies its intent.</span></span>

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a><span data-ttu-id="05d71-407">重新命名 「 SkipRequestValidationAttribute"至"AllowHtmlAttribute"</span><span class="sxs-lookup"><span data-stu-id="05d71-407">Renamed "SkipRequestValidationAttribute" to "AllowHtmlAttribute"</span></span>

<span data-ttu-id="05d71-408">*SkipRequestValidationAttribute*屬性已重新命名為*AllowHtmlAttribute*到更能代表其預定使用方式。</span><span class="sxs-lookup"><span data-stu-id="05d71-408">The *SkipRequestValidationAttribute* attribute was renamed to *AllowHtmlAttribute* to better represent its intended usage.</span></span>

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a><span data-ttu-id="05d71-409">已變更 」 Html.ValidationMessage"方法，以顯示第一個有用的錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="05d71-409">Changed "Html.ValidationMessage" Method to Display the First Useful Error Message</span></span>

<span data-ttu-id="05d71-410">*Html.ValidationMessage*方法已修正顯示第一個有用的錯誤訊息，而不是只顯示第一個錯誤。</span><span class="sxs-lookup"><span data-stu-id="05d71-410">The *Html.ValidationMessage* method was fixed to show the first useful error message instead of simply displaying the first error.</span></span>

<span data-ttu-id="05d71-411">模型繫結，期間*ModelState*字典可以擴展多個來源的屬性，包括從模型本身相關的錯誤訊息 (如果它實作*IValidatableObject*)，從套用至屬性的驗證屬性和屬性在存取時擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="05d71-411">During model binding, the *ModelState* dictionary can be populated from multiple sources with error messages about the property, including from the model itself (if it implements *IValidatableObject*), from validation attributes applied to the property, and from exceptions thrown while the property is being accessed.</span></span>

<span data-ttu-id="05d71-412">當*Html.ValidationMessage*方法會顯示驗證訊息，它會略過包含例外狀況的模型狀態項目，因為這些通常不適合終端使用者。</span><span class="sxs-lookup"><span data-stu-id="05d71-412">When the *Html.ValidationMessage* method displays a validation message, it skips model-state entries that include an exception, because these are generally not intended for the end user.</span></span> <span data-ttu-id="05d71-413">相反地，方法會尋找相關聯的例外狀況並不會顯示該訊息的第一個驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="05d71-413">Instead, the method looks for the first validation message that is not associated with an exception and displays that message.</span></span> <span data-ttu-id="05d71-414">如果不找到任何這類訊息，則會預設為第一個例外狀況相關聯的一般錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="05d71-414">If no such message is found, it defaults to a generic error message that is associated with the first exception.</span></span>

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a><span data-ttu-id="05d71-415">固定@model不要將空白字元加入至文件的宣告</span><span class="sxs-lookup"><span data-stu-id="05d71-415">Fixed @model Declaration to not Add Whitespace to the Document</span></span>

<span data-ttu-id="05d71-416">在舊版中， <em>@model</em>檢視頂端的宣告新增至呈現的 HTML 輸出的空白行。</span><span class="sxs-lookup"><span data-stu-id="05d71-416">In earlier releases, the <em>@model</em> declaration at the top of a view added a blank line to the rendered HTML output.</span></span> <span data-ttu-id="05d71-417">此問題已修正，讓宣告不會產生空白字元。</span><span class="sxs-lookup"><span data-stu-id="05d71-417">This has been fixed so that the declaration does not introduce whitespace.</span></span>

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a><span data-ttu-id="05d71-418">已新增 「 FileExtensions"屬性，以檢視引擎，以支援引擎特定檔案名稱</span><span class="sxs-lookup"><span data-stu-id="05d71-418">Added "FileExtensions" Property to View Engines to Support Engine-Specific File Names</span></span>

<span data-ttu-id="05d71-419">檢視引擎可以傳回的檢視使用明確的檢視路徑，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-419">A view engine can return a view using an explicit view path as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

<span data-ttu-id="05d71-420">第一個檢視引擎一律會嘗試將檢視呈現。</span><span class="sxs-lookup"><span data-stu-id="05d71-420">The first view engine always attempts to render the view.</span></span> <span data-ttu-id="05d71-421">根據預設，Web Form 檢視引擎是第一個檢視引擎。Web Form 引擎無法轉譯 Razor 檢視，因為發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="05d71-421">By default, the Web Forms view engine is the first view engine; because the Web Forms engine cannot render a Razor view, an error occurs.</span></span> <span data-ttu-id="05d71-422">檢視引擎現在有*FileExtensions*支援用來指定哪些檔案延伸模組的屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-422">View engines now have a *FileExtensions* property that is used to specify which file extensions they support.</span></span> <span data-ttu-id="05d71-423">ASP.NET 會判斷是否要檢視引擎可以轉譯檔案時，會檢查這個屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-423">This property is checked when ASP.NET determines whether a view engine can render a file.</span></span> <span data-ttu-id="05d71-424">這是一項重大變更和更多詳細資料中包含[的重大變更](#_Toc2_BC)本文件一節。</span><span class="sxs-lookup"><span data-stu-id="05d71-424">This is a breaking change and more details are included in the [Breaking Changes](#_Toc2_BC) section of this document.</span></span>

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a><span data-ttu-id="05d71-425">發出正確的值為"For"屬性的固定"LabelFor 「 協助程式</span><span class="sxs-lookup"><span data-stu-id="05d71-425">Fixed "LabelFor" Helper to Emit the Correct Value for the "For" Attribute</span></span>

<span data-ttu-id="05d71-426">Bug 的修正 where *LabelFor*方法呈現*如*屬性符合*輸入*項目的*名稱*屬性，屬性識別碼</span><span class="sxs-lookup"><span data-stu-id="05d71-426">A bug was fixed where the *LabelFor* method rendered a *for* attribute that matches the *input* element's *name* attribute instead of its ID.</span></span> <span data-ttu-id="05d71-427">根據 W3C*如*屬性應該符合*輸入*項目的識別碼。</span><span class="sxs-lookup"><span data-stu-id="05d71-427">According to the W3C, the *for* attribute should match the *input* element's ID.</span></span>

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a><span data-ttu-id="05d71-428">固定"RenderAction"方法，來提供模型繫結期間明確的值優先順序</span><span class="sxs-lookup"><span data-stu-id="05d71-428">Fixed "RenderAction" Method to Give Explicit Values Precedence During Model Binding</span></span>

<span data-ttu-id="05d71-429">在舊版中，明確的值傳遞至*RenderAction*方法會改用目前的表單值在內部子系動作的模型繫結期間忽略。</span><span class="sxs-lookup"><span data-stu-id="05d71-429">In earlier versions, explicit values that were passed to the *RenderAction* method were being ignored in favor of the current form values during model binding inside a child action.</span></span> <span data-ttu-id="05d71-430">修正可確保明確的值優先模型繫結期間。</span><span class="sxs-lookup"><span data-stu-id="05d71-430">The fix ensures that explicit values take precedence during model binding.</span></span>

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a><span data-ttu-id="05d71-431">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-431">Breaking Changes</span></span>

- <span data-ttu-id="05d71-432">在舊版 ASP.NET MVC 中，每個要求除了在少數情況下建立動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="05d71-432">In previous versions of ASP.NET MVC, action filters were created per request except in a few cases.</span></span> <span data-ttu-id="05d71-433">這種行為是不保證的行為，但只是實作詳細資料，就是把它視為無狀態篩選器的合約。</span><span class="sxs-lookup"><span data-stu-id="05d71-433">This behavior was never a guaranteed behavior but merely an implementation detail and the contract for filters was to consider them stateless.</span></span> <span data-ttu-id="05d71-434">在 ASP.NET MVC 3 中，篩選是更積極地快取。</span><span class="sxs-lookup"><span data-stu-id="05d71-434">In ASP.NET MVC 3, filters are cached more aggressively.</span></span> <span data-ttu-id="05d71-435">因此，任何自訂動作篩選條件不正確儲存執行個體的狀態可能已損壞。</span><span class="sxs-lookup"><span data-stu-id="05d71-435">Therefore, any custom action filters which improperly store instance state might be broken.</span></span>
- <span data-ttu-id="05d71-436">例外狀況篩選條件的執行順序已變更為具有相同的例外狀況篩選條件*順序*值。</span><span class="sxs-lookup"><span data-stu-id="05d71-436">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="05d71-437">在 ASP.NET MVC 2 和舊版中，例外狀況篩選條件具有相同的控制站上*順序*動作方法上所執行的動作方法上的例外狀況篩選條件之前的值。</span><span class="sxs-lookup"><span data-stu-id="05d71-437">In ASP.NET MVC 2 and earlier, exception filters on the controller that had the same *Order* value as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="05d71-438">這通常會發生此情況時套用例外狀況篩選條件但未指定*順序*值。</span><span class="sxs-lookup"><span data-stu-id="05d71-438">This would typically be the case when exception filters were applied without a specified *Order* value.</span></span> <span data-ttu-id="05d71-439">在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。</span><span class="sxs-lookup"><span data-stu-id="05d71-439">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="05d71-440">如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。</span><span class="sxs-lookup"><span data-stu-id="05d71-440">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="05d71-441">新的屬性，名為*FileExtensions*已新增至*VirtualPathProviderViewEngine*基底類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-441">A new property named *FileExtensions* was added to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="05d71-442">當 ASP.NET 尋找檢視的路徑 （不是依名稱） 時，則會被視為只有檢視與此新屬性所指定的清單中所包含的副檔名。</span><span class="sxs-lookup"><span data-stu-id="05d71-442">When ASP.NET looks up a view by path (not by name), only views with a file extension contained in the list specified by this new property are considered.</span></span> <span data-ttu-id="05d71-443">這是應用程式中的重大變更，自訂組建提供者註冊以啟用 Web 表單檢視自訂檔案的副檔名，而且提供者所使用的完整路徑，而不是名稱參考這些檢視表。</span><span class="sxs-lookup"><span data-stu-id="05d71-443">This is a breaking change in applications where a custom build provider is registered in order to enable a custom file extension for Web Form views and where the provider references those views by using a full path rather than a name.</span></span> <span data-ttu-id="05d71-444">因應措施是要修改的值*FileExtensions*屬性設定為包含自訂檔案的副檔名。</span><span class="sxs-lookup"><span data-stu-id="05d71-444">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>
- <span data-ttu-id="05d71-445">直接實作的自訂控制器 factory 實作<em>IControllerFactory</em>介面必須提供新的實作<em>GetControllerSessionBehavior</em> <em>已新增至這一版中的介面方法</em>。</span><span class="sxs-lookup"><span data-stu-id="05d71-445">Custom controller factory implementations that directly implement the <em>IControllerFactory</em> interface must provide an implementation of the new <em>GetControllerSessionBehavior</em><em>method that was added to the interface in this release</em>.</span></span> <span data-ttu-id="05d71-446">一般情況下，建議您不要不直接實作這個介面，並且改為衍生您的類別，從<em>DefaultControllerFactory</em>。</span><span class="sxs-lookup"><span data-stu-id="05d71-446">In general, it is recommended that you do not implement this interface directly and instead derive your class from <em>DefaultControllerFactory</em>.</span></span>

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a><span data-ttu-id="05d71-447">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-447">Known Issues</span></span>

- <span data-ttu-id="05d71-448">ASP.NET MVC 3 安裝程式，才能夠安裝的 NuGet 套件管理員的初始版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-448">The ASP.NET MVC 3 installer is only able to install an initial version of the NuGet package manager.</span></span> <span data-ttu-id="05d71-449">您已安裝的初始版本之後，可以安裝 NuGet，並使用 Visual Studio 擴充功能管理員更新。</span><span class="sxs-lookup"><span data-stu-id="05d71-449">After you have installed the initial version, NuGet can be installed and updated using Visual Studio Extension Manager.</span></span> <span data-ttu-id="05d71-450">如果您已經安裝 NuGet，請移至更新至最新版的 NuGet Visual Studio 擴充程式庫。</span><span class="sxs-lookup"><span data-stu-id="05d71-450">If you already have NuGet installed, go to the Visual Studio Extension Gallery to update to the latest version of NuGet.</span></span>
- <span data-ttu-id="05d71-451">建立新的 ASP.NET MVC 3 專案的方案資料夾內原因*NullReferenceException*錯誤。</span><span class="sxs-lookup"><span data-stu-id="05d71-451">Creating a new ASP.NET MVC 3 project within a solution folder causes a *NullReferenceException* error.</span></span> <span data-ttu-id="05d71-452">因應措施是在方案根目錄中建立 ASP.NET MVC 3 專案，然後將它移至方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="05d71-452">The workaround is to create the ASP.NET MVC 3 project in the root of the solution and then move it into the solution folder.</span></span>
- <span data-ttu-id="05d71-453">安裝程式可能需要更長的時間比舊版 ASP.NET MVC，才能完成。</span><span class="sxs-lookup"><span data-stu-id="05d71-453">The installer might take much longer than previous versions of ASP.NET MVC to complete.</span></span> <span data-ttu-id="05d71-454">這是因為它會更新 Visual Studio 2010 的元件。</span><span class="sxs-lookup"><span data-stu-id="05d71-454">This is because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="05d71-455">時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。</span><span class="sxs-lookup"><span data-stu-id="05d71-455">IntelliSense for Razor syntax does not work when ReSharper is installed.</span></span> <span data-ttu-id="05d71-456">如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 rc2 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) （英文） 部落格上的討論如何一起搭配使用今天。</span><span class="sxs-lookup"><span data-stu-id="05d71-456">If you have ReSharper installed and want to take advantage of the Razor IntelliSense support in ASP.NET MVC 3 RC2, see the entry [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) on Hadi Hariri's blog, which discusses ways to use them together today.</span></span>
- <span data-ttu-id="05d71-457">建立 ASP.NET MVC 3 的 Beta 版本的 CSHTML 和 VBHTML 檢視並沒有正確地設定其建置動作，與這些檢視的結果類型會省略發行專案時。</span><span class="sxs-lookup"><span data-stu-id="05d71-457">CSHTML and VBHTML views created with the Beta version of ASP.NET MVC 3 do not have their build action set correctly, with the result that these view types are omitted when the project is published.</span></span> <span data-ttu-id="05d71-458">*建置動作*這些檔案應該設定為內容的值 」。</span><span class="sxs-lookup"><span data-stu-id="05d71-458">The *Build Action* value for these files should be set to Content".</span></span> <span data-ttu-id="05d71-459">ASP.NET MVC 3 RC2 修正此問題，對於新的檔案，但不正確的 Beta 版本以建立專案的現有檔案設定。![](mvc3-release-notes/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="05d71-459">ASP.NET MVC 3 RC2 fixes this issue for new files, but doesn't correct the setting for existing files for a project created with the Beta version.![](mvc3-release-notes/_static/image4.png)</span></span>
- <span data-ttu-id="05d71-460">在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。</span><span class="sxs-lookup"><span data-stu-id="05d71-460">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="05d71-461">當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器功能表項目，在 Visual Studio 中的將無法使用，而且沒有任何程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="05d71-461">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>
- <span data-ttu-id="05d71-462">如果在安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="05d71-462">If you install ASP.NET MVC 3 for Visual Web Developer Express on a computer where Visual Studio is not installed, and then later install Visual Studio, you must reinstall ASP.NET MVC 3.</span></span> <span data-ttu-id="05d71-463">Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。</span><span class="sxs-lookup"><span data-stu-id="05d71-463">Visual Studio and Visual Web Developer Express share components that are upgraded by the ASP.NET MVC 3 installer.</span></span> <span data-ttu-id="05d71-464">如果您沒有 Visual Web Developer Express 以及然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，就會發生相同問題。</span><span class="sxs-lookup"><span data-stu-id="05d71-464">The same issue applies if you install ASP.NET MVC 3 for Visual Studio on a computer that does not have Visual Web Developer Express and then later install Visual Web Developer Express.</span></span>
- <span data-ttu-id="05d71-465">如果您已經安裝，安裝 ASP.NET MVC 3 RC 2 並不會更新 NuGet。</span><span class="sxs-lookup"><span data-stu-id="05d71-465">Installing ASP.NET MVC 3 RC 2 does not update NuGet if you already have it installed.</span></span> <span data-ttu-id="05d71-466">若要升級 NuGet，請移至 Visual Studio 擴充功能管理員和它應該顯示為可用的更新。</span><span class="sxs-lookup"><span data-stu-id="05d71-466">To upgrade NuGet, go to the Visual Studio Extension manager and it should show up as an available update.</span></span> <span data-ttu-id="05d71-467">您可以將 NuGet 升級為最新版本，從該處。</span><span class="sxs-lookup"><span data-stu-id="05d71-467">You can upgrade NuGet to the latest release from there.</span></span>

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a><span data-ttu-id="05d71-468">ASP.NET MVC 3 發行候選版本</span><span class="sxs-lookup"><span data-stu-id="05d71-468">ASP.NET MVC 3 Release Candidate</span></span>

<span data-ttu-id="05d71-469">ASP.NET MVC 發行候選版本已於 2010 年 11 月 9 日發行。</span><span class="sxs-lookup"><span data-stu-id="05d71-469">ASP.NET MVC Release Candidate was released on November 9, 2010.</span></span>

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a><span data-ttu-id="05d71-470">ASP.NET MVC 3 RC 的新功能</span><span class="sxs-lookup"><span data-stu-id="05d71-470">New Features in ASP.NET MVC 3 RC</span></span>

<span data-ttu-id="05d71-471">本節說明已引進的功能測試版自 ASP.NET MVC 3 RC 版本中。</span><span class="sxs-lookup"><span data-stu-id="05d71-471">This section describes features that have been introduced in the ASP.NET MVC 3 RC release since the Beta release.</span></span>

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a><span data-ttu-id="05d71-472">NuGet 封裝管理員</span><span class="sxs-lookup"><span data-stu-id="05d71-472">NuGet Package Manager</span></span>

<span data-ttu-id="05d71-473">ASP.NET MVC 3 包含 NuGet 封裝管理員 （之前稱為 NuPack），也就是將程式庫和工具加入至 Visual Studio 專案的整合式的套件管理工具。</span><span class="sxs-lookup"><span data-stu-id="05d71-473">ASP.NET MVC 3 includes the NuGet Package Manager (formerly known as NuPack), which is an integrated package management tool for adding libraries and tools to Visual Studio projects.</span></span> <span data-ttu-id="05d71-474">此工具會自動取得其來源樹狀結構的程式庫現在開發人員，採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="05d71-474">This tool automates the steps that developers take today to get a library into their source tree.</span></span>

<span data-ttu-id="05d71-475">命令列工具、 Visual Studio 2010、 內部整合的主控台視窗，從 Visual Studio 內容功能表中，和一組 PowerShell 指令程式，您可以使用 NuGet。</span><span class="sxs-lookup"><span data-stu-id="05d71-475">You can work with NuGet as a command-line tool, as an integrated console window inside Visual Studio 2010, from the Visual Studio context menu, and as a set of PowerShell cmdlets.</span></span>

<span data-ttu-id="05d71-476">如需 NuGet 的詳細資訊，請參閱[Nuget 文件](https://docs.microsoft.com/nuget/)。</span><span class="sxs-lookup"><span data-stu-id="05d71-476">For more information about NuGet, read the [Nuget Documentation](https://docs.microsoft.com/nuget/).</span></span>

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a><span data-ttu-id="05d71-477">改善 [新增專案] 對話方塊</span><span class="sxs-lookup"><span data-stu-id="05d71-477">Improved "New Project" Dialog Box</span></span>

<span data-ttu-id="05d71-478">當您建立新的專案時，[新增專案] 對話方塊現在可讓您指定的檢視引擎，以及 ASP.NET MVC 專案類型。</span><span class="sxs-lookup"><span data-stu-id="05d71-478">When you create a new project, the New Project dialog box now lets you specify the view engine as well as an ASP.NET MVC project type.</span></span>

![](mvc3-release-notes/_static/image5.png)

<span data-ttu-id="05d71-479">支援修改的範本及引擎列在對話方塊中的檢視清單包含在此版本中。</span><span class="sxs-lookup"><span data-stu-id="05d71-479">Support for modifying the list of templates and view engines listed in the dialog box is included in this release.</span></span>

<span data-ttu-id="05d71-480">預設範本，如下所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-480">The default templates are the following:</span></span>

<span data-ttu-id="05d71-481">空的。</span><span class="sxs-lookup"><span data-stu-id="05d71-481">Empty.</span></span> <span data-ttu-id="05d71-482">包含最基本的 ASP.NET MVC 專案，包括預設的目錄結構 ASP.NET MVC 專案中，包含 ASP.NET MVC 樣式時，預設值，而且包含預設 JavaScript 檔案的指令碼目錄 Site.css 檔案的檔案。</span><span class="sxs-lookup"><span data-stu-id="05d71-482">Contains a minimal set of files for an ASP.NET MVC project, including the default directory structure for ASP.NET MVC projects, a Site.css file that contains the default ASP.NET MVC styles, and a Scripts directory that contains the default JavaScript files.</span></span>

<span data-ttu-id="05d71-483">網際網路應用程式。</span><span class="sxs-lookup"><span data-stu-id="05d71-483">Internet Application.</span></span> <span data-ttu-id="05d71-484">包含範例示範如何使用 ASP.NET MVC 中的成員資格提供者的功能。</span><span class="sxs-lookup"><span data-stu-id="05d71-484">Contains sample functionality that demonstrates how to use the membership provider with ASP.NET MVC.</span></span>

<span data-ttu-id="05d71-485">在 Windows 登錄中指定會顯示在對話方塊中的專案範本清單。</span><span class="sxs-lookup"><span data-stu-id="05d71-485">The list of project templates that is displayed in the dialog box is specified in the Windows registry.</span></span>

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a><span data-ttu-id="05d71-486">無工作階段的控制站</span><span class="sxs-lookup"><span data-stu-id="05d71-486">Sessionless Controllers</span></span>

<span data-ttu-id="05d71-487">新*ControllerSessionStateAttribute*可讓您更充分掌控工作階段狀態行為的控制站藉由指定[System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx)列舉值。</span><span class="sxs-lookup"><span data-stu-id="05d71-487">The new *ControllerSessionStateAttribute* gives you more control over session-state behavior for controllers by specifying a [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) enumeration value.</span></span>

<span data-ttu-id="05d71-488">下列範例會示範如何關閉控制站的所有要求的工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="05d71-488">The following example shows how to turn off session state for all requests to a controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

<span data-ttu-id="05d71-489">下列範例會示範如何設定唯讀的工作階段狀態的所有要求的控制站。</span><span class="sxs-lookup"><span data-stu-id="05d71-489">The following example shows how to set read-only session state for all requests to a controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a><span data-ttu-id="05d71-490">新的驗證屬性</span><span class="sxs-lookup"><span data-stu-id="05d71-490">New Validation Attributes</span></span>

#### <a name="compareattribute"></a><span data-ttu-id="05d71-491">CompareAttribute</span><span class="sxs-lookup"><span data-stu-id="05d71-491">CompareAttribute</span></span>

<span data-ttu-id="05d71-492">新*CompareAttribute*驗證屬性可讓您比較兩個不同模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="05d71-492">The new *CompareAttribute* validation attribute lets you compare the values of two different properties of a model.</span></span> <span data-ttu-id="05d71-493">在下列範例中， *ComparePassword*屬性必須符合*密碼*欄位才會生效。</span><span class="sxs-lookup"><span data-stu-id="05d71-493">In the following example, the *ComparePassword* property must match the *Password* field in order to be valid.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a><span data-ttu-id="05d71-494">RemoteAttribute</span><span class="sxs-lookup"><span data-stu-id="05d71-494">RemoteAttribute</span></span>

<span data-ttu-id="05d71-495">新*RemoteAttribute*驗證屬性會利用 jQuery 驗證外掛程式中的遠端驗證程式，可讓用戶端驗證，以呼叫執行實際驗證邏輯的伺服器上的方法。</span><span class="sxs-lookup"><span data-stu-id="05d71-495">The new *RemoteAttribute* validation attribute takes advantage of the jQuery Validation plug-in's remote validator, which enables client-side validation to call a method on the server that performs the actual validation logic.</span></span>

<span data-ttu-id="05d71-496">在下列範例中， *UserName*屬性具有*RemoteAttribute*套用。</span><span class="sxs-lookup"><span data-stu-id="05d71-496">In the following example, the *UserName* property has the *RemoteAttribute* applied.</span></span> <span data-ttu-id="05d71-497">編輯時編輯的檢視，這個屬性，用戶端驗證將會呼叫名為動作*UserNameAvailable*上*UsersController*類別以驗證這個欄位。</span><span class="sxs-lookup"><span data-stu-id="05d71-497">When editing this property in an Edit view, client validation will call an action named *UserNameAvailable* on the *UsersController* class in order to validate this field.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

<span data-ttu-id="05d71-498">下列範例顯示相對應的控制項。</span><span class="sxs-lookup"><span data-stu-id="05d71-498">The following example shows the corresponding controller.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

<span data-ttu-id="05d71-499">根據預設，屬性會套用至屬性名稱會做為查詢字串參數傳送至動作方法。</span><span class="sxs-lookup"><span data-stu-id="05d71-499">By default, the property name that the attribute is applied to is sent to the action method as a query-string parameter.</span></span>

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a><span data-ttu-id="05d71-500">新的 「 LabelFor"和"LabelForModel"方法的多載</span><span class="sxs-lookup"><span data-stu-id="05d71-500">New Overloads for "LabelFor" and "LabelForModel" Methods</span></span>

<span data-ttu-id="05d71-501">已新增新的多載*LabelFor*和*LabelForModel*方法可讓您指定的標籤文字。</span><span class="sxs-lookup"><span data-stu-id="05d71-501">New overloads have been added for the *LabelFor* and *LabelForModel* methods that let you specify the label text.</span></span> <span data-ttu-id="05d71-502">下列範例會示範如何使用這些多載。</span><span class="sxs-lookup"><span data-stu-id="05d71-502">The following example shows how to use these overloads.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a><span data-ttu-id="05d71-503">子系動作輸出快取</span><span class="sxs-lookup"><span data-stu-id="05d71-503">Child Action Output Caching</span></span>

<span data-ttu-id="05d71-504">*OutputCacheAttribute*支援輸出快取子系動作，透過呼叫*Html.RenderAction*或*Html.Action* helper 方法。</span><span class="sxs-lookup"><span data-stu-id="05d71-504">The *OutputCacheAttribute* supports output caching of child actions that are called by using the *Html.RenderAction* or *Html.Action* helper methods.</span></span> <span data-ttu-id="05d71-505">下列範例會顯示呼叫另一個動作的檢視。</span><span class="sxs-lookup"><span data-stu-id="05d71-505">The following example shows a view that calls another action.</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

<span data-ttu-id="05d71-506">*GetDate*動作以註解*OutputCacheAttribute*:</span><span class="sxs-lookup"><span data-stu-id="05d71-506">The *GetDate* action is annotated with the *OutputCacheAttribute*:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

<span data-ttu-id="05d71-507">執行此程式碼，Html.Action("GetDate") 要呼叫的結果是針對快取 100 秒。</span><span class="sxs-lookup"><span data-stu-id="05d71-507">When this code runs, the result of the call to Html.Action("GetDate") is cached for 100 seconds.</span></span>

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a><span data-ttu-id="05d71-508">[新增檢視] 對話方塊改進</span><span class="sxs-lookup"><span data-stu-id="05d71-508">"Add View" Dialog Box Improvements</span></span>

<span data-ttu-id="05d71-509">當您將強型別的檢視時，[新增檢視] 對話方塊現在會篩選出更多非適用類型比在舊版中，例如許多核心.NET Framework 型別。</span><span class="sxs-lookup"><span data-stu-id="05d71-509">When you add a strongly typed view, the Add View dialog box now filters out more non-applicable types than in previous releases, such as many core .NET Framework types.</span></span> <span data-ttu-id="05d71-510">此外，現在排序清單的類別名稱，而不是完整限定的類型名稱，使其更容易尋找到型別。</span><span class="sxs-lookup"><span data-stu-id="05d71-510">Also, the list is now sorted by the class name and not by the fully qualified type name, which makes it easier to find types.</span></span> <span data-ttu-id="05d71-511">例如，型別名稱現在會顯示如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-511">For example, the type name is now displayed as in the following example:</span></span>

<span data-ttu-id="05d71-512">類別名稱 （命名空間）</span><span class="sxs-lookup"><span data-stu-id="05d71-512">ClassName (namespace)</span></span>

<span data-ttu-id="05d71-513">在舊版中，這會顯示如下所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-513">In earlier releases, this would have been displayed as the following:</span></span>

<span data-ttu-id="05d71-514">Namespace.ClassName</span><span class="sxs-lookup"><span data-stu-id="05d71-514">Namespace.ClassName</span></span>

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a><span data-ttu-id="05d71-515">詳細的要求驗證</span><span class="sxs-lookup"><span data-stu-id="05d71-515">Granular Request Validation</span></span>

<span data-ttu-id="05d71-516">*排除*屬性*validateinputattribute 套用*不再存在。</span><span class="sxs-lookup"><span data-stu-id="05d71-516">The *Exclude* property of *ValidateInputAttribute* no longer exists.</span></span> <span data-ttu-id="05d71-517">相反地，若要在模型繫結期間略過之模型的特定屬性的要求驗證，請使用 新*SkipRequestValidationAttribute*。</span><span class="sxs-lookup"><span data-stu-id="05d71-517">Instead, to have request validation skipped for specific properties of a model during model binding, use the new *SkipRequestValidationAttribute*.</span></span>

<span data-ttu-id="05d71-518">例如，假設動作方法用來編輯部落格文章：</span><span class="sxs-lookup"><span data-stu-id="05d71-518">For example, suppose an action method is used to edit a blog post:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

<span data-ttu-id="05d71-519">下列範例顯示的檢視模型的部落格文章。</span><span class="sxs-lookup"><span data-stu-id="05d71-519">The following example shows the view model for a blog post.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

<span data-ttu-id="05d71-520">使用者送出某些描述屬性的標記，將會失敗模型繫結，因為要求驗證。</span><span class="sxs-lookup"><span data-stu-id="05d71-520">When a user submits some markup for the Description property, model binding will fail due to request validation.</span></span> <span data-ttu-id="05d71-521">若要停用部落格文章描述的模型繫結期間要求驗證，請套用*SkipRequpestValidationAttribute*屬性，如本範例所示:。</span><span class="sxs-lookup"><span data-stu-id="05d71-521">To disable request validation during model binding for the blog post Description, apply the *SkipRequpestValidationAttribute* to the property, as shown in this example:.</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

<span data-ttu-id="05d71-522">或者，若要關閉此模型的每個屬性的要求驗證，套用*validateinputattribute 套用*值是*false*至動作方法：</span><span class="sxs-lookup"><span data-stu-id="05d71-522">Alternatively, to turn off request validation for every property of the model, apply *ValidateInputAttribute* with a value of *false* to the action method:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a><span data-ttu-id="05d71-523">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-523">Breaking Changes</span></span>

- <span data-ttu-id="05d71-524">例外狀況篩選條件的執行順序已變更為具有相同的例外狀況篩選條件*順序*值。</span><span class="sxs-lookup"><span data-stu-id="05d71-524">The order of execution for exception filters has changed for exception filters that have the same *Order* value.</span></span> <span data-ttu-id="05d71-525">在 ASP.NET MVC 2 和舊版中，例外狀況篩選條件具有相同的控制站上*順序*動作方法上所執行的動作方法上的例外狀況篩選條件之前。</span><span class="sxs-lookup"><span data-stu-id="05d71-525">In ASP.NET MVC 2 and earlier, exception filters on the controller that had the same *Order* as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="05d71-526">這通常會發生此情況時套用例外狀況篩選條件但未指定*順序*值。</span><span class="sxs-lookup"><span data-stu-id="05d71-526">This would typically be the case when exception filters were applied without a specified *Order* value.</span></span> <span data-ttu-id="05d71-527">在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。</span><span class="sxs-lookup"><span data-stu-id="05d71-527">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="05d71-528">如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。</span><span class="sxs-lookup"><span data-stu-id="05d71-528">As in earlier versions, if the *Order* property is explicitly specified, the filters are run in the specified order.</span></span>
- <span data-ttu-id="05d71-529">加入新的屬性，名為*FileExtensions*至*VirtualPathProviderViewEngine*基底類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-529">Added a new property named *FileExtensions* to the *VirtualPathProviderViewEngine* base class.</span></span> <span data-ttu-id="05d71-530">查閱時檢視路徑 （而不是名稱），只檢視中所包含的檔案副檔名為會被視為新的屬性所指定的清單。</span><span class="sxs-lookup"><span data-stu-id="05d71-530">When looking up a view by path (and not by name), only views with a file extension contained in the list specified by this new property is considered.</span></span> <span data-ttu-id="05d71-531">這是一項重大變更的人員註冊自訂的組建提供者啟用自訂檔案延伸模組的 web 表單檢視和使用完整路徑，而不是名稱參考這些檢視表。</span><span class="sxs-lookup"><span data-stu-id="05d71-531">This is a breaking change for those who register a custom build provider to enable a custom file extension for web form views and are referencing those views by using a full path rather than a name.</span></span> <span data-ttu-id="05d71-532">因應措施是要修改的值*FileExtensions*屬性設定為包含自訂檔案的副檔名。</span><span class="sxs-lookup"><span data-stu-id="05d71-532">The workaround is to modify the value of the *FileExtensions* property to include the custom file extension.</span></span>

<a id="_Toc276711795"></a>
## <a name="known-issues"></a><span data-ttu-id="05d71-533">已知問題</span><span class="sxs-lookup"><span data-stu-id="05d71-533">Known Issues</span></span>

- <span data-ttu-id="05d71-534">安裝程式可能需要更長的時間比舊版 ASP.NET MVC 完成，因為它會更新 Visual Studio 2010 的元件。</span><span class="sxs-lookup"><span data-stu-id="05d71-534">The installer may take much longer than previous versions of ASP.NET MVC to complete because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="05d71-535">當您選取 astrongly 時加入檢視 scaffolding 型別的檢視 scaffold 唯寫屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-535">The Add View scaffolding when selecting astrongly typed view scaffolds write-only properties.</span></span> <span data-ttu-id="05d71-536">這些應該一律忽略 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="05d71-536">These should always be ignored by scaffolding.</span></span> <span data-ttu-id="05d71-537">[新增檢視] 對話方塊也 scaffold 唯讀屬性時產生的 「 編輯 」 或 「 建立 」 檢視。</span><span class="sxs-lookup"><span data-stu-id="05d71-537">The Add View dialog also scaffolds read-only properties when generating an "Edit" or "Create" view.</span></span> <span data-ttu-id="05d71-538">唯讀屬性只應針對顯示和清單檢視建立結構。</span><span class="sxs-lookup"><span data-stu-id="05d71-538">Read-only properties should only be scaffolded for the Display and List views.</span></span>
- <span data-ttu-id="05d71-539">偵錯不適用於 ASP.NET MVC 3 與非同步 CTP 一起安裝。</span><span class="sxs-lookup"><span data-stu-id="05d71-539">Debugging doesn't work when ASP.NET MVC 3 is installed alongside the Async CTP.</span></span> <span data-ttu-id="05d71-540">ASP.NET MVC 3 不能安裝-並存與非同步 CTP。</span><span class="sxs-lookup"><span data-stu-id="05d71-540">ASP.NET MVC 3 cannot be installed side-by-side with the Async CTP.</span></span> <span data-ttu-id="05d71-541">解除安裝非同步 CTP 修復偵錯。</span><span class="sxs-lookup"><span data-stu-id="05d71-541">Uninstall the Async CTP to repair debugging.</span></span> <span data-ttu-id="05d71-542">如需詳細資訊，請參閱[此部落格文章](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)需解除安裝 ASP.NET MVC 3 RC 的所有項目。</span><span class="sxs-lookup"><span data-stu-id="05d71-542">For more details, read [this blog post](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) about uninstalling all the pieces of ASP.NET MVC 3 RC.</span></span>
- <span data-ttu-id="05d71-543">安裝 Resharper，razor Intellisense 不會無法運作。</span><span class="sxs-lookup"><span data-stu-id="05d71-543">Razor Intellisense does not work when Resharper is installed.</span></span> <span data-ttu-id="05d71-544">如果您已安裝 ReSharper 且想要利用 Razor intellisense 支援在 ASP.NET MVC 3 RC，請閱讀[此部落格文章](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)從 JetBrains 其中討論如何一起搭配使用今天。</span><span class="sxs-lookup"><span data-stu-id="05d71-544">If you have ReSharper installed and want to take advantage of the Razor intellisense support in ASP.NET MVC 3 RC, please read [this blog post](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) from JetBrains which discusses ways to use them together today.</span></span>
- <span data-ttu-id="05d71-545">建立 ASP.NET MVC 3 Beta 的 CSHTML 和 VBHTML 檢視並沒有其建置動作正確，從發行會省略它們。</span><span class="sxs-lookup"><span data-stu-id="05d71-545">CSHTML and VBHTML views created with Beta of ASP.NET MVC 3 do not have their build action correctly which omits them from publishing.</span></span> <span data-ttu-id="05d71-546">*建置動作*這些檔案應該設定為 「 內容 」。</span><span class="sxs-lookup"><span data-stu-id="05d71-546">The *Build Action* for these files should be set to "Content".</span></span> <span data-ttu-id="05d71-547">ASP.NET MVC 3 RC 修正此問題，對於新的檔案，但不會正確設定現有的檔案，如使用 beta 版建立的專案。</span><span class="sxs-lookup"><span data-stu-id="05d71-547">ASP.NET MVC 3 RC fixes this issue for new files, but doesn't correct the setting for existing files for a project created with the Beta.</span></span>
- <span data-ttu-id="05d71-548">安裝程式可能需要更長的時間比舊版 ASP.NET MVC 完成，因為它會更新 Visual Studio 2010 的元件。</span><span class="sxs-lookup"><span data-stu-id="05d71-548">The installer may take much longer than previous versions of ASP.NET MVC to complete because it updates components of Visual Studio 2010.</span></span>
- <span data-ttu-id="05d71-549">當選取"Edit"的強型別檢視 scaffold 加入檢視 scaffolding 唯讀屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-549">The Add View scaffolding when selecting an "Edit" strongly typed view scaffolds read only properties.</span></span> <span data-ttu-id="05d71-550">同樣地，唯寫屬性會建立 「 顯示 」 檢視的結構。</span><span class="sxs-lookup"><span data-stu-id="05d71-550">Likewise, write-only properties are scaffolded for "Display" views.</span></span>
- <span data-ttu-id="05d71-551">在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。</span><span class="sxs-lookup"><span data-stu-id="05d71-551">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>
- <span data-ttu-id="05d71-552">安裝 Visual Studio 非同步 CTP 中包含的工具安裝 ASP.NET MVC 3 Razor 版本導致衝突。</span><span class="sxs-lookup"><span data-stu-id="05d71-552">Installing the Visual Studio Async CTP causes a conflict with the Razor release that is included as part of the ASP.NET MVC 3 tooling installation.</span></span> <span data-ttu-id="05d71-553">請確定您未嘗試在同一部電腦上安裝 Visual Studio 非同步 CTP 和 Razor 版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-553">Make sure that you do not try to install both the Visual Studio Async CTP and the Razor release on the same machine.</span></span>
- <span data-ttu-id="05d71-554">當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器功能表項目，在 Visual Studio 中的將無法使用，而且沒有任何程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="05d71-554">When you are editing a Razor view (.cshtml file), the Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a><span data-ttu-id="05d71-555">ASP.NET MVC 3 Beta</span><span class="sxs-lookup"><span data-stu-id="05d71-555">ASP.NET MVC 3 Beta</span></span>

<span data-ttu-id="05d71-556">ASP.NET MVC 3 Beta 已於 2010 年 10 月 6 日發行。</span><span class="sxs-lookup"><span data-stu-id="05d71-556">ASP.NET MVC 3 Beta was released on October 6, 2010.</span></span> <span data-ttu-id="05d71-557">下列附註僅供測試版，，而且受限於的任何更新或變更上述 ASP.NET MVC 3 發行候選版本 > 一節中所參考。</span><span class="sxs-lookup"><span data-stu-id="05d71-557">The following notes are specific to the Beta release, and are subject to any updates or changes referenced in the ASP.NET MVC 3 Release Candidate section above.</span></span>

## <a id="0.1__Toc274034215"></a>  <span data-ttu-id="05d71-558">新的 Featuresin ASP.NET MVC 3 Beta</span><span class="sxs-lookup"><span data-stu-id="05d71-558">New Featuresin ASP.NET MVC 3 Beta</span></span>

<a id="0.1__Default_validation_system"></a><span data-ttu-id="05d71-559">本節說明已引進的功能在 ASP.NET MVC 3 Beta 版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-559">This section describes features that have been introduced in the ASP.NET MVC 3 Beta release.</span></span>

### <a id="0.1__Toc274034216"></a>  <span data-ttu-id="05d71-560">NuGet 封裝管理員</span><span class="sxs-lookup"><span data-stu-id="05d71-560">NuGet Package Manager</span></span>

<span data-ttu-id="05d71-561">ASP.NET MVC 3 包含 NuGet 封裝管理員，也就是新增程式庫的整合式的封裝管理工具和 Visual Studio 專案的工具。</span><span class="sxs-lookup"><span data-stu-id="05d71-561">ASP.NET MVC 3 includes NuGet Package Manager, which is an integrated package management tool for adding libraries and tools to Visual Studio projects.</span></span> <span data-ttu-id="05d71-562">大部分的情況下，它會自動取得其來源樹狀結構的程式庫現在開發人員，採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="05d71-562">For the most part, it automates the steps that developers take today to get a library into their source tree.</span></span>

<span data-ttu-id="05d71-563">命令列工具、 Visual Studio 2010 內從 Visual Studio 內容功能表中，整合的主控台 視窗和組 PowerShell 指令程式，您可以使用 NuGet。</span><span class="sxs-lookup"><span data-stu-id="05d71-563">You can work with NuGet as a command line tool, as an integrated console window inside Visual Studio 2010, from the Visual Studio context menu, and as set of PowerShell cmdlets.</span></span>

<span data-ttu-id="05d71-564">如需 NuGet 的詳細資訊，請參閱[NuGet 文件](https://docs.microsoft.com/nuget/)。</span><span class="sxs-lookup"><span data-stu-id="05d71-564">For more information about NuGet, read the [NuGet Documentation](https://docs.microsoft.com/nuget/).</span></span>

### <a id="0.1__Toc274034217"></a>  <span data-ttu-id="05d71-565">改善 新增專案對話方塊</span><span class="sxs-lookup"><span data-stu-id="05d71-565">Improved New Project Dialog Box</span></span>

<span data-ttu-id="05d71-566">當您建立新的專案時，[新增專案] 對話方塊現在可讓您指定的檢視引擎，以及 ASP.NET MVC 專案類型。</span><span class="sxs-lookup"><span data-stu-id="05d71-566">When you create a new project, the New Project dialog box now lets you specify the view engine as well as an ASP.NET MVC project type.</span></span>

![](mvc3-release-notes/_static/image6.png)

<span data-ttu-id="05d71-567">在此版本中不支援修改的範本及引擎列在對話方塊中的檢視清單。</span><span class="sxs-lookup"><span data-stu-id="05d71-567">Support for modifying the list of templates and view engines listed in the dialog box is not included in this release.</span></span>

<span data-ttu-id="05d71-568">預設範本，如下所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-568">The default templates are the following:</span></span>

<span data-ttu-id="05d71-569">空的。</span><span class="sxs-lookup"><span data-stu-id="05d71-569">Empty.</span></span> <span data-ttu-id="05d71-570">包含 ASP.NET MVC 專案，包括預設的目錄結構 ASP.NET MVC 專案小型 Site.css 檔案，其中包含 ASP.NET MVC 樣式時，預設值，而且包含預設 JavaScript 檔案的指令碼目錄檔案的最少。</span><span class="sxs-lookup"><span data-stu-id="05d71-570">Contains a minimal set of files for an ASP.NET MVC project, including the default directory structure for ASP.NET MVC projects, a small Site.css file that contains the default ASP.NET MVC styles, and a Scripts directory that contains the default JavaScript files.</span></span>

<span data-ttu-id="05d71-571">網際網路應用程式。</span><span class="sxs-lookup"><span data-stu-id="05d71-571">Internet Application.</span></span> <span data-ttu-id="05d71-572">包含範例示範如何使用 ASP.NET MVC 中的成員資格提供者的功能。</span><span class="sxs-lookup"><span data-stu-id="05d71-572">Contains sample functionality that demonstrates how to use the membership provider within ASP.NET MVC.</span></span>

### <a id="0.1__Toc274034218"></a>  <span data-ttu-id="05d71-573">簡化的方式指定強類型模型在 Razor 檢視</span><span class="sxs-lookup"><span data-stu-id="05d71-573">Simplified Way to Specify Strongly Typed Models in Razor Views</span></span>

<span data-ttu-id="05d71-574">指定強型別 Razor 檢視的模型類型的方法已經過簡化，使用新@modelCSHTML 檢視的指示詞和@ModelTypeVBHTML 檢視的指示詞。</span><span class="sxs-lookup"><span data-stu-id="05d71-574">The way to specify the model type for strongly typed Razor views has been simplified by using the new @model directive for CSHTML views and @ModelType directive for VBHTML views.</span></span> <span data-ttu-id="05d71-575">在舊版 ASP.NET MVC 中，您會指定 Razor 的強型別的模型檢視這種方式：</span><span class="sxs-lookup"><span data-stu-id="05d71-575">In earlier versions of ASP.NET MVC, you would specify a strongly typed model for Razor views this way:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

<span data-ttu-id="05d71-576">在此版本中，您可以使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="05d71-576">In this release, you can use the following syntax:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  <span data-ttu-id="05d71-577">Helper 方法支援新的 ASP.NET Web 網頁</span><span class="sxs-lookup"><span data-stu-id="05d71-577">Support for New ASP.NET Web Pages Helper Methods</span></span>

<span data-ttu-id="05d71-578">新的 ASP.NET Web Pages 技術包含一組可用於將常用的功能加入至檢視和控制器的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="05d71-578">The new ASP.NET Web Pages technology includes a set of helper methods that are useful for adding commonly used functionality to views and controllers.</span></span> <span data-ttu-id="05d71-579">ASP.NET MVC 3 支援使用中控制器和檢視這些 helper 方法 （如適用）。</span><span class="sxs-lookup"><span data-stu-id="05d71-579">ASP.NET MVC 3 supports using these helper methods within controllers and views (where appropriate).</span></span> <span data-ttu-id="05d71-580">這些方法都包含在 System.Web.Helpers 組件。</span><span class="sxs-lookup"><span data-stu-id="05d71-580">These methods are contained in the System.Web.Helpers assembly.</span></span> <span data-ttu-id="05d71-581">下表列出幾個 ASP.NET Web Pages helper 方法。</span><span class="sxs-lookup"><span data-stu-id="05d71-581">The following table lists a few of the ASP.NET Web Pages helper methods.</span></span>

| <span data-ttu-id="05d71-582">**Helper**</span><span class="sxs-lookup"><span data-stu-id="05d71-582">**Helper**</span></span> | <span data-ttu-id="05d71-583">**描述**</span><span class="sxs-lookup"><span data-stu-id="05d71-583">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="05d71-584">圖表</span><span class="sxs-lookup"><span data-stu-id="05d71-584">Chart</span></span> | <span data-ttu-id="05d71-585">呈現於檢視內的圖表。</span><span class="sxs-lookup"><span data-stu-id="05d71-585">Renders a chart within a view.</span></span> <span data-ttu-id="05d71-586">包含例如 Chart.ToWebImage、 Chart.Save 和 Chart.Write 方法。</span><span class="sxs-lookup"><span data-stu-id="05d71-586">Contains methods such as Chart.ToWebImage, Chart.Save, and Chart.Write.</span></span> |
| <span data-ttu-id="05d71-587">密碼編譯</span><span class="sxs-lookup"><span data-stu-id="05d71-587">Crypto</span></span> | <span data-ttu-id="05d71-588">雜湊演算法來建立正確的使用 salt 和密碼雜湊處理。</span><span class="sxs-lookup"><span data-stu-id="05d71-588">Uses hashing algorithms to create properly salted and hashed passwords.</span></span> |
| <span data-ttu-id="05d71-589">WebGrid</span><span class="sxs-lookup"><span data-stu-id="05d71-589">WebGrid</span></span> | <span data-ttu-id="05d71-590">以方格呈現物件 （通常，從資料庫的資料） 的集合。</span><span class="sxs-lookup"><span data-stu-id="05d71-590">Renders a collection of objects (typically, data from a database) as a grid.</span></span> <span data-ttu-id="05d71-591">支援分頁和排序。</span><span class="sxs-lookup"><span data-stu-id="05d71-591">Supports paging and sorting.</span></span> |
| <span data-ttu-id="05d71-592">WebImage</span><span class="sxs-lookup"><span data-stu-id="05d71-592">WebImage</span></span> | <span data-ttu-id="05d71-593">呈現影像。</span><span class="sxs-lookup"><span data-stu-id="05d71-593">Renders an image.</span></span> |
| <span data-ttu-id="05d71-594">WebMail</span><span class="sxs-lookup"><span data-stu-id="05d71-594">WebMail</span></span> | <span data-ttu-id="05d71-595">傳送電子郵件訊息。</span><span class="sxs-lookup"><span data-stu-id="05d71-595">Sends an email message.</span></span> |

<span data-ttu-id="05d71-596">快速參考主題，列出的協助程式及基本語法是使用 ASP.NET Razor 語法文件集，下列 url 的一部分：</span><span class="sxs-lookup"><span data-stu-id="05d71-596">A quick reference topic that lists the helpers and basic syntax is available as part of the ASP.NET Razor syntax documentation at the following URL:</span></span>

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  <span data-ttu-id="05d71-597">其他相依性插入的支援</span><span class="sxs-lookup"><span data-stu-id="05d71-597">Additional Dependency Injection Support</span></span>

<span data-ttu-id="05d71-598">建置在 ASP.NET MVC 3 Preview 1 版本上，目前的版本會包含已加入的支援的兩個新的服務和四個現有的服務和改良的相依性解析和通用服務定位程式支援。</span><span class="sxs-lookup"><span data-stu-id="05d71-598">Building on the ASP.NET MVC 3 Preview 1 release, the current release includes added support for two new services and four existing services, and improved support for dependency resolution and the Common Service Locator.</span></span>

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a><span data-ttu-id="05d71-599">新的 IControllerActivator 介面的更細緻的控制站執行個體化</span><span class="sxs-lookup"><span data-stu-id="05d71-599">New IControllerActivator Interface for Fine-Grained Controller Instantiation</span></span>

<span data-ttu-id="05d71-600">新的 IControllerActivator 介面還提供如何控制站會具現化透過相依性插入更多更細微的控制。</span><span class="sxs-lookup"><span data-stu-id="05d71-600">The new IControllerActivator interface provides more fine-grained control over how controllers are instantiated via dependency injection.</span></span> <span data-ttu-id="05d71-601">下列範例示範介面：</span><span class="sxs-lookup"><span data-stu-id="05d71-601">The following example shows the interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

<span data-ttu-id="05d71-602">以此對照控制器 factory 的角色。</span><span class="sxs-lookup"><span data-stu-id="05d71-602">Contrast this to the role of the controller factory.</span></span> <span data-ttu-id="05d71-603">控制器 factory 會負責尋找控制器型別以及具現化該控制器型別的執行個體的 IControllerFactory 介面的實作。</span><span class="sxs-lookup"><span data-stu-id="05d71-603">A controller factory is an implementation of the IControllerFactory interface, which is responsible both for locating a controller type and for instantiating an instance of that controller type.</span></span>

<span data-ttu-id="05d71-604">控制器啟動項只負責具現化控制器型別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="05d71-604">Controller activators are responsible only for instantiating an instance of a controller type.</span></span> <span data-ttu-id="05d71-605">它們不執行控制器的型別查閱。</span><span class="sxs-lookup"><span data-stu-id="05d71-605">They do not perform the controller type lookup.</span></span> <span data-ttu-id="05d71-606">找到適當的控制器類型之後, 控制器 factory 應該委派到 IControllerActivator 的執行個體，以處理控制站的實際的具現化。</span><span class="sxs-lookup"><span data-stu-id="05d71-606">After locating a proper controller type, controller factories should delegate to an instance of IControllerActivator to handle the actual instantiation of the controller.</span></span>

<span data-ttu-id="05d71-607">DefaultControllerFactory 類別具有新的建構函式可接受 IControllerFactory 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05d71-607">The DefaultControllerFactory class has a new constructor that accepts an IControllerFactory instance.</span></span> <span data-ttu-id="05d71-608">這可讓您將管理此部分的控制站建立不必覆寫預設的控制器類型查閱行為的相依性插入套用。</span><span class="sxs-lookup"><span data-stu-id="05d71-608">This lets you apply Dependency Injection to manage this aspect of controller creation without having to override the default controller-type lookup behavior.</span></span>

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a><span data-ttu-id="05d71-609">IServiceLocator 介面取代 IDependencyResolver</span><span class="sxs-lookup"><span data-stu-id="05d71-609">IServiceLocator Interface Replaced with IDependencyResolver</span></span>

<span data-ttu-id="05d71-610">根據社群意見反應，ASP.NET MVC 3 版已取代 IServiceLocator 介面的使用電腦類 IDependencyResolver 介面適用於 ASP.NET MVC 的需求。</span><span class="sxs-lookup"><span data-stu-id="05d71-610">Based on community feedback, the ASP.NET MVC 3 Beta release has replaced the use of the IServiceLocator interface with a slimmed-down IDependencyResolver interface specific to the needs of ASP.NET MVC.</span></span> <span data-ttu-id="05d71-611">下列範例示範新的介面：</span><span class="sxs-lookup"><span data-stu-id="05d71-611">The following example shows the new interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

<span data-ttu-id="05d71-612">這項變更的一部分，ServiceLocator 類別也已經取代 dependencyresolver 來註冊類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-612">As part of this change, the ServiceLocator class was also replaced with the DependencyResolver class.</span></span> <span data-ttu-id="05d71-613">相依性解析程式註冊會類似於舊版 ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="05d71-613">Registration of a dependency resolver is similar to earlier versions of ASP.NET MVC:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

<span data-ttu-id="05d71-614">提供已註冊的服務要求類型，此介面的實作只應該委派到基礎的相依性插入容器。</span><span class="sxs-lookup"><span data-stu-id="05d71-614">Implementations of this interface should simply delegate to the underlying dependency injection container to provide the registered service for the requested type.</span></span>

<span data-ttu-id="05d71-615">要求類型的任何註冊的服務時，ASP.NET MVC 預期會實作這個介面，可從 GetService 傳回 null，而且從 GetServices 傳回空集合。</span><span class="sxs-lookup"><span data-stu-id="05d71-615">When there are no registered services of the requested type, ASP.NET MVC expects implementations of this interface to return null from GetService and to return an empty collection from GetServices.</span></span>

<span data-ttu-id="05d71-616">新的 dependencyresolver 來註冊類別可讓您註冊新的 IDependencyResolver 介面或通用服務定位程式介面 (IServiceLocator) 實作的類別。</span><span class="sxs-lookup"><span data-stu-id="05d71-616">The new DependencyResolver class lets you register classes that implement either the new IDependencyResolver interface or the Common Service Locator interface (IServiceLocator).</span></span> <span data-ttu-id="05d71-617">如需通用服務定位程式的詳細資訊，請參閱[GitHub 上的 CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)。</span><span class="sxs-lookup"><span data-stu-id="05d71-617">For more information about Common Service Locator, see [CommonServiceLocator on GitHub](https://github.com/unitycontainer/commonservicelocator).</span></span>

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a><span data-ttu-id="05d71-618">新的 IViewActivator 介面的更細緻的檢視頁面具現化</span><span class="sxs-lookup"><span data-stu-id="05d71-618">New IViewActivator Interface for Fine-Grained View Page Instantiation</span></span>

<span data-ttu-id="05d71-619">新的 IViewPageActivator 介面還提供更細微的控制，透過檢視頁面如何透過相依性插入具現。</span><span class="sxs-lookup"><span data-stu-id="05d71-619">The new IViewPageActivator interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="05d71-620">這適用於 WebFormView 執行個體和 RazorView 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05d71-620">This applies to both WebFormView instances and RazorView instances.</span></span> <span data-ttu-id="05d71-621">下列範例示範新的介面：</span><span class="sxs-lookup"><span data-stu-id="05d71-621">The following example shows the new interface:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

<span data-ttu-id="05d71-622">這些類別現在接受 IViewPageActivator 建構函式引數，可讓您使用相依性插入來控制如何 ViewPage、 ViewUserControl 和 WebViewPage 型別會具現化。</span><span class="sxs-lookup"><span data-stu-id="05d71-622">These classes now accept an IViewPageActivator constructor argument, which lets you use dependency injection to control how the ViewPage, ViewUserControl, and WebViewPage types are instantiated.</span></span>

#### <a name="new-dependency-resolver-support-for-existing-services"></a><span data-ttu-id="05d71-623">新的相依性解析程式支援現有的服務</span><span class="sxs-lookup"><span data-stu-id="05d71-623">New Dependency Resolver Support for Existing Services</span></span>

<span data-ttu-id="05d71-624">新的版本會包含相依性解析支援下列服務：</span><span class="sxs-lookup"><span data-stu-id="05d71-624">The new release includes dependency resolution support for the following services:</span></span>

- <span data-ttu-id="05d71-625">模型驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="05d71-625">Model validation providers.</span></span> <span data-ttu-id="05d71-626">類別可實作 ModelValidatorProvider 可以註冊相依性解析程式中，系統會使用它們以支援用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="05d71-626">Classes that implement ModelValidatorProvider can be registered in the dependency resolver, and the system will use them to support client- and server-side validation.</span></span>
- <span data-ttu-id="05d71-627">模型中繼資料提供者。</span><span class="sxs-lookup"><span data-stu-id="05d71-627">Model metadata provider.</span></span> <span data-ttu-id="05d71-628">單一類別可實作 ModelMetadataProvider 可以註冊相依性解析程式中，系統會使用樣板化及驗證系統提供的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="05d71-628">A single class that implements ModelMetadataProvider can be registered in the dependency resolver, and the system will use it to provide metadata for the templating and validation systems.</span></span>
- <span data-ttu-id="05d71-629">值提供者。</span><span class="sxs-lookup"><span data-stu-id="05d71-629">Value providers.</span></span> <span data-ttu-id="05d71-630">類別可實作 ValueProviderFactory 可以註冊相依性解析程式中，系統會使用它們來建立控制器和模型繫結期間所使用的值提供者。</span><span class="sxs-lookup"><span data-stu-id="05d71-630">Classes that implement ValueProviderFactory can be registered in the dependency resolver, and the system will use them to create value providers that are consumed by the controller and during model binding.</span></span>
- <span data-ttu-id="05d71-631">模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="05d71-631">Model binders.</span></span> <span data-ttu-id="05d71-632">類別可實作 IModelBinderProvider 可以註冊相依性解析程式中，系統會使用它們來建立模型繫結器所使用的模型繫結系統。</span><span class="sxs-lookup"><span data-stu-id="05d71-632">Classes that implement IModelBinderProvider can be registered in the dependency resolver, and the system will use them to create model binders that are consumed by the model binding system.</span></span>

### <a id="0.1__Toc274034221"></a>  <span data-ttu-id="05d71-633">新的不顯眼的 jQuery 為基礎的 Ajax 支援</span><span class="sxs-lookup"><span data-stu-id="05d71-633">New Support for Unobtrusive jQuery-Based Ajax</span></span>

<span data-ttu-id="05d71-634">ASP.NET MVC 包括 Ajax helper 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-634">ASP.NET MVC includes Ajax helper methods such as the following:</span></span>

- <span data-ttu-id="05d71-635">Ajax.ActionLink</span><span class="sxs-lookup"><span data-stu-id="05d71-635">Ajax.ActionLink</span></span>
- <span data-ttu-id="05d71-636">Ajax.RouteLink</span><span class="sxs-lookup"><span data-stu-id="05d71-636">Ajax.RouteLink</span></span>
- <span data-ttu-id="05d71-637">Ajax.BeginForm</span><span class="sxs-lookup"><span data-stu-id="05d71-637">Ajax.BeginForm</span></span>
- <span data-ttu-id="05d71-638">Ajax.BeginRouteForm</span><span class="sxs-lookup"><span data-stu-id="05d71-638">Ajax.BeginRouteForm</span></span>

<span data-ttu-id="05d71-639">這些方法會使用 JavaScript，叫用動作方法上的伺服器，而不是使用完整的回傳。</span><span class="sxs-lookup"><span data-stu-id="05d71-639">These methods use JavaScript to invoke an action method on the server rather than using a full postback.</span></span> <span data-ttu-id="05d71-640">這項功能已更新為利用 jQuery 不顯眼的方式。</span><span class="sxs-lookup"><span data-stu-id="05d71-640">This functionality has been updated to take advantage of jQuery in an unobtrusive manner.</span></span> <span data-ttu-id="05d71-641">而不是 intrusively 發出內嵌用戶端指令碼，這些 helper 方法分隔行為從標記發出使用 HTML5 屬性*資料 ajax*前置詞。</span><span class="sxs-lookup"><span data-stu-id="05d71-641">Rather than intrusively emitting inline client scripts, these helper methods separate the behavior from the markup by emitting HTML5 attributes using the *data-ajax* prefix.</span></span> <span data-ttu-id="05d71-642">然後行為會套用至標記以參考適當的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="05d71-642">Behavior is then applied to the markup by referencing the appropriate JavaScript files.</span></span> <span data-ttu-id="05d71-643">請確定所參考 下列 JavaScript 檔案：</span><span class="sxs-lookup"><span data-stu-id="05d71-643">Make sure that the following JavaScript files are referenced:</span></span>

- <span data-ttu-id="05d71-644">jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="05d71-644">jquery-1.4.1.js</span></span>
- <span data-ttu-id="05d71-645">jquery.unobtrusive.ajax.js</span><span class="sxs-lookup"><span data-stu-id="05d71-645">jquery.unobtrusive.ajax.js</span></span>

<span data-ttu-id="05d71-646">在 ASP.NET MVC 3 新專案範本中，在 Web.config 檔案中預設會啟用此功能，但現有的專案預設會停用。</span><span class="sxs-lookup"><span data-stu-id="05d71-646">This feature is enabled by default in the Web.config file in the ASP.NET MVC 3 new project templates, but is disabled by default for existing projects.</span></span> <span data-ttu-id="05d71-647">如需詳細資訊，請參閱[加入應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1_AddedApplicationWideFlagsForClientValida)本文件後面。</span><span class="sxs-lookup"><span data-stu-id="05d71-647">For more information, see [Added application-wide flags for client validation and unobtrusive JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) later in this document.</span></span>

### <a id="0.1__Toc274034222"></a>  <span data-ttu-id="05d71-648">不顯眼的 jQuery 驗證的新支援</span><span class="sxs-lookup"><span data-stu-id="05d71-648">New Support for Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="05d71-649">根據預設，ASP.NET MVC 3 Beta 會使用 jQuery 驗證以執行用戶端驗證不顯眼的方式。</span><span class="sxs-lookup"><span data-stu-id="05d71-649">By default, ASP.NET MVC 3 Beta uses jQuery validation in an unobtrusive manner in order to perform client-side validation.</span></span> <span data-ttu-id="05d71-650">若要啟用不顯眼的用戶端驗證，請如下所示從檢視表中的呼叫：</span><span class="sxs-lookup"><span data-stu-id="05d71-650">To enable unobtrusive client validation, make a call like the following from within a view:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

<span data-ttu-id="05d71-651">這需要 ViewContext.UnobtrusiveJavaScriptEnabled 屬性設定為 true，您可以藉由下列呼叫：</span><span class="sxs-lookup"><span data-stu-id="05d71-651">This requires that ViewContext.UnobtrusiveJavaScriptEnabled property is set to true, which you can do by making the following call:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

<span data-ttu-id="05d71-652">也請確定下列 JavaScript 檔案參考。</span><span class="sxs-lookup"><span data-stu-id="05d71-652">Also make sure the following JavaScript files are referenced.</span></span>

- <span data-ttu-id="05d71-653">jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="05d71-653">jquery-1.4.1.js</span></span>
- <span data-ttu-id="05d71-654">jquery.validate.js</span><span class="sxs-lookup"><span data-stu-id="05d71-654">jquery.validate.js</span></span>
- <span data-ttu-id="05d71-655">jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="05d71-655">jquery.validate.unobtrusive.js</span></span>

<span data-ttu-id="05d71-656">這項功能預設會在 Web.config 檔案在 ASP.NET MVC 3 新專案範本中，已啟用，但現有的專案預設會停用。</span><span class="sxs-lookup"><span data-stu-id="05d71-656">This feature is enabled on by default in the Web.config file in the ASP.NET MVC 3 new project templates, but is disabled by default for existing projects.</span></span> <span data-ttu-id="05d71-657">如需詳細資訊，請參閱[新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1_AddedApplicationWideFlagsForClientValida)本文件後面。</span><span class="sxs-lookup"><span data-stu-id="05d71-657">For more information, see [New application-wide flags for client validation and unobtrusive JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) later in this document.</span></span>

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  <span data-ttu-id="05d71-658">新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript</span><span class="sxs-lookup"><span data-stu-id="05d71-658">New Application-Wide Flags for Client Validation and Unobtrusive JavaScript</span></span>

<span data-ttu-id="05d71-659">您可以啟用或停用用戶端驗證和不顯眼的 JavaScript 全域使用靜態成員的 HtmlHelper 類別，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-659">You can enable or disable client validation and unobtrusive JavaScript globally using static members of the HtmlHelper class, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

<span data-ttu-id="05d71-660">預設的專案範本預設會啟用不顯眼的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="05d71-660">The default project templates enable unobtrusive JavaScript by default.</span></span> <span data-ttu-id="05d71-661">您也可以啟用或停用這些功能使用下列設定的應用程式的根 Web.config 檔案中：</span><span class="sxs-lookup"><span data-stu-id="05d71-661">You can also enable or disable these features in the root Web.config file of your application using the following settings:</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

<span data-ttu-id="05d71-662">根據預設，您可以啟用這些功能，因為新的多載引進 HtmlHelper 類別，可讓您覆寫預設設定，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-662">Because you can enable these features by default, new overloads were introduced to the HtmlHelper class that let you override the default settings, as shown in the following examples:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

<span data-ttu-id="05d71-663">回溯相容性，這兩種功能預設為停用。</span><span class="sxs-lookup"><span data-stu-id="05d71-663">For backward compatibility, both of these features are disabled by default.</span></span>

### <a id="0.1__Toc274034224"></a>  <span data-ttu-id="05d71-664">檢視執行之前，所執行的程式碼的新支援</span><span class="sxs-lookup"><span data-stu-id="05d71-664">New Support for Code that Runs Before Views Run</span></span>

<span data-ttu-id="05d71-665">您現在可以將名為\_viewstart.cshtml (或\_viewstart.vbhtml) 檢視目錄中，請加入程式碼，將該目錄及其子目錄中的 在多個檢視之間共用。</span><span class="sxs-lookup"><span data-stu-id="05d71-665">You can now put a file named \_viewstart.cshtml (or \_viewstart.vbhtml) in the Views directory and add code to it that will be shared among multiple views in that directory and its subdirectories.</span></span> <span data-ttu-id="05d71-666">例如，下列程式碼可能會放入\_viewstart.cshtml 頁面 ~/Views 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="05d71-666">For example, you might put the following code into the \_viewstart.cshtml page in the ~/Views folder:</span></span>

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

<span data-ttu-id="05d71-667">這會設定在 [檢視] 資料夾內的每個檢視和其所有子資料夾遞迴的版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="05d71-667">This sets the layout page for every view within the Views folder and all its subfolders recursively.</span></span> <span data-ttu-id="05d71-668">當檢視所呈現中的程式碼\_viewstart.cshtml 檔案執行檢視程式碼執行之前。</span><span class="sxs-lookup"><span data-stu-id="05d71-668">When a view is being rendered, the code in the \_viewstart.cshtml file runs before the view code runs.</span></span> <span data-ttu-id="05d71-669">\_Viewstart.cshtml 程式碼適用於該資料夾中的每個檢視。</span><span class="sxs-lookup"><span data-stu-id="05d71-669">The \_viewstart.cshtml code applies to every view in that folder.</span></span>

<span data-ttu-id="05d71-670">根據預設中的程式碼\_viewstart.cshtml 檔案也會套用至任何子資料夾中的檢視。</span><span class="sxs-lookup"><span data-stu-id="05d71-670">By default, the code in the \_viewstart.cshtml file also applies to views in any subfolder.</span></span> <span data-ttu-id="05d71-671">不過，個別的子資料夾可以有自己版本的\_viewstart.cshtml 檔案; 該中的情況下，會優先使用本機版本。</span><span class="sxs-lookup"><span data-stu-id="05d71-671">However, individual subfolders can have their own version of the \_viewstart.cshtml file; in that case, the local version takes precedence.</span></span> <span data-ttu-id="05d71-672">例如，若要執行 HomeController 所有檢視都通用的程式碼，放入\_viewstart.cshtml ~/Views/Home 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="05d71-672">For example, to run code that is common to all views for the HomeController, put a \_viewstart.cshtml file in the ~/Views/Home folder.</span></span>

### <a id="0.1__Toc274034225"></a>  <span data-ttu-id="05d71-673">新 VBHTML Razor 語法的支援</span><span class="sxs-lookup"><span data-stu-id="05d71-673">New Support for the VBHTML Razor Syntax</span></span>

<span data-ttu-id="05d71-674">先前的 ASP.NET MVC preview 包含檢視使用根據 C# 的 Razor 語法的支援。</span><span class="sxs-lookup"><span data-stu-id="05d71-674">The previous ASP.NET MVC preview included support for views using Razor syntax based on C#.</span></span> <span data-ttu-id="05d71-675">這些檢視會使用.cshtml 檔案的副檔名。</span><span class="sxs-lookup"><span data-stu-id="05d71-675">These views use the .cshtml file extension.</span></span> <span data-ttu-id="05d71-676">支援 Razor 的進行中工作的一部分，ASP.NET MVC 3 beta 版導入了在 Visual Basic，會使用.vbhtml 副檔名 Razor 語法的支援。</span><span class="sxs-lookup"><span data-stu-id="05d71-676">As part of ongoing work to support Razor, the ASP.NET MVC 3 Beta introduces support for the Razor syntax in Visual Basic, which uses the .vbhtml file extension.</span></span>

<span data-ttu-id="05d71-677">如需使用 Visual Basic 語法 VBHTML 頁面中的簡介，請參閱教學課程中的，下列 url:</span><span class="sxs-lookup"><span data-stu-id="05d71-677">For an introduction to using Visual Basic syntax in VBHTML pages, see the tutorial at the following URL:</span></span>

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  <span data-ttu-id="05d71-678">更精確地控制從而 validateinputattribute 套用</span><span class="sxs-lookup"><span data-stu-id="05d71-678">More Granular Control over ValidateInputAttribute</span></span>

<span data-ttu-id="05d71-679">ASP.NET MVC 一律具有包含 validateinputattribute 套用類別，可叫用核心 ASP.NET 要求驗證基礎結構，並確定內送要求不包含可能是惡意的輸入。</span><span class="sxs-lookup"><span data-stu-id="05d71-679">ASP.NET MVC has always included the ValidateInputAttribute class, which invokes the core ASP.NET request validation infrastructure to make sure that the incoming request does not contain potentially malicious input.</span></span> <span data-ttu-id="05d71-680">根據預設，會啟用輸入的驗證。</span><span class="sxs-lookup"><span data-stu-id="05d71-680">By default, input validation is enabled.</span></span> <span data-ttu-id="05d71-681">您可使用 validateinputattribute 套用屬性，如下列範例所示，停用要求驗證：</span><span class="sxs-lookup"><span data-stu-id="05d71-681">It is possible to disable request validation by using the ValidateInputAttribute attribute, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

<span data-ttu-id="05d71-682">不過，許多 web 應用程式有需要時剩下的欄位不應該允許 HTML，個別的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="05d71-682">However, many web applications have individual form fields that need to allow HTML, while the remaining fields should not.</span></span> <span data-ttu-id="05d71-683">Validateinputattribute 套用類別現在可讓您指定不應該包含在要求驗證的欄位清單。</span><span class="sxs-lookup"><span data-stu-id="05d71-683">The ValidateInputAttribute class now lets you specify a list of fields that should not be included in request validation.</span></span>

<span data-ttu-id="05d71-684">比方說，如果您正在開發部落格引擎，您可能想要允許主體和摘要欄位中的標記。</span><span class="sxs-lookup"><span data-stu-id="05d71-684">For example, if you are developing a blog engine, you might want to allow markup in the Body and Summary fields.</span></span> <span data-ttu-id="05d71-685">這些欄位可能會表示兩個輸入項目，每個都有對應的屬性名稱 （「 主體 」 和 「 摘要 」） 的名稱屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-685">These fields might be represented by two input element, each with a name attribute corresponding to the property name ("Body" and "Summary").</span></span> <span data-ttu-id="05d71-686">若要停用要求驗證這些欄位，指定名稱 （以逗號分隔） 中排除 ValidateInput 類別屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-686">To disable request validation for these fields only, specify the names (comma-separated) in the Exclude property of the ValidateInput class, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  <span data-ttu-id="05d71-687">協助程式轉換成底線連字號為使用匿名物件指定的 HTML 屬性名稱</span><span class="sxs-lookup"><span data-stu-id="05d71-687">Helpers Convert Underscores to Hyphens for HTML Attribute Names Specified Using Anonymous Objects</span></span>

<span data-ttu-id="05d71-688">Helper 方法可讓您指定屬性名稱/值組，使用匿名物件，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="05d71-688">Helper methods let you specify attribute name/value pairs using an anonymous object, as in the following example:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

<span data-ttu-id="05d71-689">這個方法不會讓您使用屬性名稱中的連字號因為連字號不能用在 ASP.NET 中的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="05d71-689">This approach doesn't let you use hyphens in the attribute name, because a hyphen cannot be used for a property name in ASP.NET.</span></span> <span data-ttu-id="05d71-690">不過，連字號是很重要的自訂 HTML5 屬性;比方說，HTML5 會使用 「 資料-」 前置詞。</span><span class="sxs-lookup"><span data-stu-id="05d71-690">However, hyphens are important for custom HTML5 attributes; for example, HTML5 uses the "data-" prefix.</span></span>

<span data-ttu-id="05d71-691">同時，底線不能用於在 HTML 中，屬性名稱，但會在屬性名稱中有效。</span><span class="sxs-lookup"><span data-stu-id="05d71-691">At the same time, underscores cannot be used for attribute names in HTML, but are valid within property names.</span></span> <span data-ttu-id="05d71-692">因此，如果您指定使用匿名的物件、 屬性和屬性名稱包含底線、 helper 方法會將轉換底線連字號。</span><span class="sxs-lookup"><span data-stu-id="05d71-692">Therefore, if you specify attributes using an anonymous object, and if the attribute names include an underscore,, helper methods will convert the underscores to hyphens.</span></span> <span data-ttu-id="05d71-693">例如，下列 helper 語法會使用底線：</span><span class="sxs-lookup"><span data-stu-id="05d71-693">For example, the following helper syntax uses an underscore:</span></span>

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

<span data-ttu-id="05d71-694">協助程式執行時前, 一個範例就會呈現下列標記：</span><span class="sxs-lookup"><span data-stu-id="05d71-694">The previous example renders the following markup when the helper runs:</span></span>

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  <span data-ttu-id="05d71-695">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="05d71-695">Bug Fixes</span></span>

<span data-ttu-id="05d71-696">EditorFor 和 DisplayFor 範本協助程式的預設物件範本現在支援 DisplayAttribute.Order 屬性中指定的順序。</span><span class="sxs-lookup"><span data-stu-id="05d71-696">The default object template for the EditorFor and DisplayFor template helpers now supports the ordering specified in the DisplayAttribute.Order property.</span></span> <span data-ttu-id="05d71-697">（在舊版中，順序設定已無法使用。）</span><span class="sxs-lookup"><span data-stu-id="05d71-697">(In previous versions, the Order setting was not used.)</span></span>

<span data-ttu-id="05d71-698">現在，用戶端驗證支援驗證的覆寫已套用的驗證屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="05d71-698">Client validation now supports validation of overridden properties that have validation attributes applied.</span></span>

<span data-ttu-id="05d71-699">預設現在會註冊的 JsonValueProviderFactory。</span><span class="sxs-lookup"><span data-stu-id="05d71-699">JsonValueProviderFactory is now registered by default.</span></span>

## <a id="0.1__Toc274034229"></a>  <span data-ttu-id="05d71-700">重大變更</span><span class="sxs-lookup"><span data-stu-id="05d71-700">Breaking Changes</span></span>

<span data-ttu-id="05d71-701">具有相同的順序值的例外狀況篩選條件已經變更的例外狀況篩選條件執行順序。</span><span class="sxs-lookup"><span data-stu-id="05d71-701">The order of execution for exception filters has changed for exception filters that have the same Order value.</span></span> <span data-ttu-id="05d71-702">在 ASP.NET MVC 2 和舊版中，例外狀況的篩選具有相同的順序控制器動作方法上所執行的動作方法上的例外狀況篩選條件之前。</span><span class="sxs-lookup"><span data-stu-id="05d71-702">In ASP.NET MVC 2 and earlier, exception filters on the controller with the same Order as those on an action method were executed before the exception filters on the action method.</span></span> <span data-ttu-id="05d71-703">例外狀況篩選條件已套用沒有指定的順序值，這通常會是大小寫。</span><span class="sxs-lookup"><span data-stu-id="05d71-703">This would typically be the case when exception filters were applied without a specified Order value.</span></span> <span data-ttu-id="05d71-704">在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。</span><span class="sxs-lookup"><span data-stu-id="05d71-704">In ASP.NET MVC 3, this order has been reversed so that the most specific exception handler executes first.</span></span> <span data-ttu-id="05d71-705">如同舊版本中，如果明確指定 Order 屬性，篩選條件會執行指定的順序。</span><span class="sxs-lookup"><span data-stu-id="05d71-705">As in earlier versions, if the Order property is explicitly specified, the filters are run in the specified order.</span></span>

## <a id="0.1__Toc274034230"></a>  <span data-ttu-id="05d71-706">已知的問題</span><span class="sxs-lookup"><span data-stu-id="05d71-706">Known Issues</span></span>

<span data-ttu-id="05d71-707">在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。</span><span class="sxs-lookup"><span data-stu-id="05d71-707">During installation, the EULA acceptance dialog box displays the license terms in a window that is smaller than intended.</span></span>

<span data-ttu-id="05d71-708">Razor 檢視沒有 IntelliSense 支援，也不語法反白顯示。</span><span class="sxs-lookup"><span data-stu-id="05d71-708">Razor views do not have IntelliSense support nor syntax highlighting.</span></span> <span data-ttu-id="05d71-709">它預期會在 Visual Studio 中的 Razor 語法支援的是較新版本的一部分。</span><span class="sxs-lookup"><span data-stu-id="05d71-709">It is anticipated that support for Razor syntax in Visual Studio will be included as part of a later release.</span></span>

<span data-ttu-id="05d71-710">當您在編輯 Razor 檢視 （CSHTML 檔案）、 <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a>移至控制器在 Visual Studio 中的功能表項目將無法使用，而且沒有程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="05d71-710">When you are editing a Razor view (CSHTML file), the <a id="0.1__Toc224729061"></a><a id="0.1__Toc238051347"></a> Go To Controller menu item in Visual Studio will not be available, and there are no code snippets.</span></span>

<span data-ttu-id="05d71-711">當使用@model語法來指定強型別的 CSHTML 檢視中，類型的語言特定快速鍵無法辨識。</span><span class="sxs-lookup"><span data-stu-id="05d71-711">When using the @model syntax to specify a strongly typed CSHTML view, language-specific shortcuts for types are not recognized.</span></span> <span data-ttu-id="05d71-712">例如， @model int 將無法運作，但@modelInt32 運作。</span><span class="sxs-lookup"><span data-stu-id="05d71-712">For example, @model int will not work, but @model Int32 will work.</span></span> <span data-ttu-id="05d71-713">此錯誤的因應措施是當您指定的模型型別時使用的實際型別名稱。</span><span class="sxs-lookup"><span data-stu-id="05d71-713">The workaround for this bug is to use the actual type name when you specify the model type.</span></span>

<span data-ttu-id="05d71-714">當使用@model語法來指定強型別的 CSHTML 檢視 (或@ModelType指定強型別的 VBHTML 檢視)，不支援可為 null 的類型和陣列宣告。</span><span class="sxs-lookup"><span data-stu-id="05d71-714">When using the @model syntax to specify a strongly typed CSHTML view (or @ModelType to specify a strongly typed VBHTML view), nullable types and array declarations are not supported.</span></span> <span data-ttu-id="05d71-715">例如， @model int？ 不支援。</span><span class="sxs-lookup"><span data-stu-id="05d71-715">For example, @model int? is not supported.</span></span> <span data-ttu-id="05d71-716">請改用`@model Nullable<Int32>`。</span><span class="sxs-lookup"><span data-stu-id="05d71-716">Instead, use `@model Nullable<Int32>`.</span></span> <span data-ttu-id="05d71-717">語法@modelstring [] 也不支援; 請改用`@model IList<string>`。</span><span class="sxs-lookup"><span data-stu-id="05d71-717">The syntax @model string[] is also not supported; instead, use `@model IList<string>`.</span></span>

<span data-ttu-id="05d71-718">當您將 ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 時，請確定 Web.config 檔的 appSettings 區段中加入下列：</span><span class="sxs-lookup"><span data-stu-id="05d71-718">When you upgrade an ASP.NET MVC 2 project to ASP.NET MVC 3, make sure to add the following to the appSettings section of the Web.config file:</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

<span data-ttu-id="05d71-719">沒有會造成永遠將未經驗證的使用者重新導向至 ~/Account/登入，略過在 Web.config 中使用表單驗證設定表單驗證的已知的問題。因應措施是將下列應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="05d71-719">There's a known issue that causes Forms Authentication to always redirect unauthenticated users to ~/Account/Login, ignoring the forms authentication setting used in Web.config. The workaround is to add the following app setting.</span></span>

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  <span data-ttu-id="05d71-720">Disclaimer</span><span class="sxs-lookup"><span data-stu-id="05d71-720">Disclaimer</span></span>

<span data-ttu-id="05d71-721">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="05d71-721">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="05d71-722">著作權所有，並保留一切權利。</span><span class="sxs-lookup"><span data-stu-id="05d71-722">All rights reserved.</span></span> <span data-ttu-id="05d71-723">本文件係"做為-是。 」</span><span class="sxs-lookup"><span data-stu-id="05d71-723">This document is provided "as-is."</span></span> <span data-ttu-id="05d71-724">資訊及檢視在此文件，包括 URL 及其他網際網路網站參考資料，可能會變更恕不另行通知。</span><span class="sxs-lookup"><span data-stu-id="05d71-724">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span> <span data-ttu-id="05d71-725">貴用戶須自行承擔使用風險。</span><span class="sxs-lookup"><span data-stu-id="05d71-725">You bear the risk of using it.</span></span>

<span data-ttu-id="05d71-726">本文件不提供　貴用戶任何 Microsoft 產品之智慧財產權的法定權利。</span><span class="sxs-lookup"><span data-stu-id="05d71-726">This document does not provide you with any legal rights to any intellectual property in any Microsoft product.</span></span> <span data-ttu-id="05d71-727">貴用戶可以複製本文件供內部參考之用。</span><span class="sxs-lookup"><span data-stu-id="05d71-727">You may copy and use this document for your internal, reference purposes.</span></span>
