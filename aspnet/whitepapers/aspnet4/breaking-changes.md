---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 的重大變更 |Microsoft 文件
author: rick-anderson
description: 本文件說明已為.NET Framework 版本 4 可能會影響使用所建立的應用程式的發行版本的變更...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 7eea51add6b05684357314e3d6aa5087383c6408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899112"
---
<a name="aspnet-4-breaking-changes"></a><span data-ttu-id="34c5c-103">ASP.NET 4 的重大變更</span><span class="sxs-lookup"><span data-stu-id="34c5c-103">ASP.NET 4 Breaking Changes</span></span>
====================
> <span data-ttu-id="34c5c-104">本文件描述所做之.NET Framework 版本 4 可能會影響使用較早的版本，包括 ASP.NET 4 版 Beta 1 和 Beta 2 版本所建立的應用程式的發行的變更。</span><span class="sxs-lookup"><span data-stu-id="34c5c-104">This document describes changes that have been made for the .NET Framework version 4 release that can potentially affect applications that were created using earlier releases, including the ASP.NET 4 Beta 1 and Beta 2 releases.</span></span>
> 
> [<span data-ttu-id="34c5c-105">下載此技術白皮書</span><span class="sxs-lookup"><span data-stu-id="34c5c-105">Download This Whitepaper</span></span>](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a><span data-ttu-id="34c5c-106">內容</span><span class="sxs-lookup"><span data-stu-id="34c5c-106">Contents</span></span>

[<span data-ttu-id="34c5c-107">在 Web.config 檔案中的 ControlRenderingCompatibilityVersion 設定</span><span class="sxs-lookup"><span data-stu-id="34c5c-107">ControlRenderingCompatibilityVersion Setting in the Web.config File</span></span>](#0.1__Toc256770141 "_Toc256770141")  
[<span data-ttu-id="34c5c-108">ClientIDMode 變更</span><span class="sxs-lookup"><span data-stu-id="34c5c-108">ClientIDMode Changes</span></span>](#0.1__Toc256770142 "_Toc256770142")  
[<span data-ttu-id="34c5c-109">HtmlEncode 和進行 urlencode 處理現在編碼單引號</span><span class="sxs-lookup"><span data-stu-id="34c5c-109">HtmlEncode and UrlEncode Now Encode Single Quotation Marks</span></span>](#0.1__Toc256770143 "_Toc256770143")  
[<span data-ttu-id="34c5c-110">ASP.NET 網頁 (.aspx) 剖析器為 Stricter</span><span class="sxs-lookup"><span data-stu-id="34c5c-110">ASP.NET Page (.aspx) Parser is Stricter</span></span>](#0.1__Toc256770144 "_Toc256770144")  
[<span data-ttu-id="34c5c-111">瀏覽器定義檔案更新</span><span class="sxs-lookup"><span data-stu-id="34c5c-111">Browser Definition Files Updated</span></span>](#0.1__Toc256770145 "_Toc256770145")  
[<span data-ttu-id="34c5c-112">從根 Web 組態檔中移除 System.Web.Mobile.dll</span><span class="sxs-lookup"><span data-stu-id="34c5c-112">System.Web.Mobile.dll Removed from Root Web Configuration File</span></span>](#0.1__Toc256770146 "_Toc256770146")  
[<span data-ttu-id="34c5c-113">ASP.NET 要求驗證</span><span class="sxs-lookup"><span data-stu-id="34c5c-113">ASP.NET Request Validation</span></span>](#0.1__Toc256770147 "_Toc256770147")  
[<span data-ttu-id="34c5c-114">雜湊演算法的預設值為 Now HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="34c5c-114">Default Hashing Algorithm Is Now HMACSHA256</span></span>](#0.1__Toc256770148 "_Toc256770148")  
[<span data-ttu-id="34c5c-115">新的 ASP.NET 4 根組態相關組態錯誤</span><span class="sxs-lookup"><span data-stu-id="34c5c-115">Configuration Errors Related to New ASP.NET 4 Root Configuration</span></span>](#0.1__Toc256770149 "_Toc256770149")  
[<span data-ttu-id="34c5c-116">ASP.NET 4 個子應用程式無法啟動時在 ASP.NET 2.0 或 ASP.NET 3.5 應用程式</span><span class="sxs-lookup"><span data-stu-id="34c5c-116">ASP.NET 4 Child Applications Fail to Start When Under ASP.NET 2.0 or ASP.NET 3.5 Applications</span></span>](#0.1__Toc256770150 "_Toc256770150")  
[<span data-ttu-id="34c5c-117">ASP.NET 4 Web Sites 無法安裝 SharePoint 的電腦上啟動</span><span class="sxs-lookup"><span data-stu-id="34c5c-117">ASP.NET 4 Web Sites Fail to Start on Computers Where SharePoint Is Installed</span></span>](#0.1__Toc256770151 "_Toc256770151")  
[<span data-ttu-id="34c5c-118">HttpRequest.FilePath 屬性不再包含 PathInfo 值</span><span class="sxs-lookup"><span data-stu-id="34c5c-118">The HttpRequest.FilePath Property No Longer Includes PathInfo Values</span></span>](#0.1__Toc256770152 "_Toc256770152")  
[<span data-ttu-id="34c5c-119">ASP.NET 2.0 應用程式可能會產生參考 eurl.axd HttpException 錯誤</span><span class="sxs-lookup"><span data-stu-id="34c5c-119">ASP.NET 2.0 Applications Might Generate HttpException Errors that Reference eurl.axd</span></span>](#0.1__Toc256770153 "_Toc256770153")  
[<span data-ttu-id="34c5c-120">事件處理常式可能不會不引發 IIS 7 或 IIS 7.5 中的預設文件在整合模式</span><span class="sxs-lookup"><span data-stu-id="34c5c-120">Event Handlers Might Not Be Not Raised in a Default Document in IIS 7 or IIS 7.5 Integrated Mode</span></span>](#0.1__Toc256770154 "_Toc256770154")  
[<span data-ttu-id="34c5c-121">ASP.NET 程式碼存取安全性 (CAS) 實作變更</span><span class="sxs-lookup"><span data-stu-id="34c5c-121">Changes to the ASP.NET Code Access Security (CAS) Implementation</span></span>](#0.1__Toc256770155 "_Toc256770155")  
[<span data-ttu-id="34c5c-122">移動 MembershipUser 和 System.Web.Security 命名空間中的其他類型</span><span class="sxs-lookup"><span data-stu-id="34c5c-122">MembershipUser and Other Types in the System.Web.Security Namespace Have Been Moved</span></span>](#0.1__Toc256770156 "_Toc256770156")  
[<span data-ttu-id="34c5c-123">輸出快取的變更來改變\*HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="34c5c-123">Output Caching Changes to Vary \* HTTP Header</span></span>](#0.1__Toc256770157 "_Toc256770157")  
[<span data-ttu-id="34c5c-124">Passport System.Web.Security 類型為過時</span><span class="sxs-lookup"><span data-stu-id="34c5c-124">System.Web.Security Types for Passport are Obsolete</span></span>](#0.1__Toc256770158 "_Toc256770158")  
[<span data-ttu-id="34c5c-125">無法轉譯 ASP.NET 4 中的映像 MenuItem.PopOutImageUrl 屬性</span><span class="sxs-lookup"><span data-stu-id="34c5c-125">The MenuItem.PopOutImageUrl Property Fails to Render an Image in ASP.NET 4</span></span>](#0.1__Toc256770159 "_Toc256770159")  
[<span data-ttu-id="34c5c-126">Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 容錯移轉至呈現影像時路徑包含反斜線</span><span class="sxs-lookup"><span data-stu-id="34c5c-126">Menu.StaticPopOutImageUrl and Menu.DynamicPopOutImageUrl Fail to Render Images When Paths Contain Backslashes</span></span>](#0.1__Toc256770160 "_Toc256770160")  
[<span data-ttu-id="34c5c-127">Disclaimer</span><span class="sxs-lookup"><span data-stu-id="34c5c-127">Disclaimer</span></span>](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a><span data-ttu-id="34c5c-128">ControlRenderingCompatibilityVersion 設定 Web.config 檔案中</span><span class="sxs-lookup"><span data-stu-id="34c5c-128">ControlRenderingCompatibilityVersion Setting in the Web.config File</span></span>

<span data-ttu-id="34c5c-129">ASP.NET 控制項，讓您指定更精確地說如何轉譯標記已修改.NET Framework 第 4 版中。</span><span class="sxs-lookup"><span data-stu-id="34c5c-129">ASP.NET controls have been modified in the .NET Framework version 4 in order to let you specify more precisely how they render markup.</span></span> <span data-ttu-id="34c5c-130">在舊版的.NET framework 中，某些控制項來發出您並沒有方式可以停用的標記。</span><span class="sxs-lookup"><span data-stu-id="34c5c-130">In previous versions of the .NET Framework, some controls emitted markup that you had no way to disable.</span></span> <span data-ttu-id="34c5c-131">根據預設，不會再產生 ASP.NET 4 這種類型的標記。</span><span class="sxs-lookup"><span data-stu-id="34c5c-131">By default, ASP.NET 4 this type of markup is no longer generated.</span></span>

<span data-ttu-id="34c5c-132">如果您使用 Visual Studio 2010 來升級您的應用程式從 ASP.NET 2.0 或 ASP.NET 3.5 為目標時，此工具會自動加入的設定`Web.config`保留舊版的轉譯檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-132">If you use Visual Studio 2010 to upgrade your application from ASP.NET 2.0 or ASP.NET 3.5, the tool automatically adds a setting to the `Web.config` file that preserves legacy rendering.</span></span> <span data-ttu-id="34c5c-133">不過，如果您將 IIS 中的應用程式集區變更成以.NET Framework 4 為目標來升級應用程式，則 ASP.NET 預設會使用新轉譯模式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-133">However, if you upgrade an application by changing the application pool in IIS to target the .NET Framework 4, ASP.NET uses the new rendering mode by default.</span></span> <span data-ttu-id="34c5c-134">若要停用新的呈現模式，新增下列設定中的`Web.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="34c5c-134">To disable the new rendering mode, add the following setting in the `Web.config` file:</span></span>

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

<span data-ttu-id="34c5c-135">新的行為會將主要轉譯變更如下：</span><span class="sxs-lookup"><span data-stu-id="34c5c-135">The major rendering changes that the new behavior brings are as follows:</span></span>

- <span data-ttu-id="34c5c-136">**映像**和**ImageButton**控制項不再呈現`border="0"`屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-136">The **Image** and **ImageButton** controls no longer render a `border="0"` attribute.</span></span>
- <span data-ttu-id="34c5c-137">**BaseValidator**從中衍生的類別和驗證控制項不再預設呈現紅色文字。</span><span class="sxs-lookup"><span data-stu-id="34c5c-137">The **BaseValidator** class and validation controls that derive from it no longer render red text by default.</span></span>
- <span data-ttu-id="34c5c-138">**HtmlForm**控制項不會呈現**名稱**屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-138">The **HtmlForm** control does not render a **name** attribute.</span></span>
- <span data-ttu-id="34c5c-139">**資料表**控制不再呈現`border="0"`屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-139">The **Table** control no longer renders a `border="0"` attribute.</span></span>
- <span data-ttu-id="34c5c-140">未專為使用者輸入的控制項 (例如，**標籤**控制項) 不再呈現`disabled="disabled"`屬性如果其**啟用**屬性設定為**false**（或如果它們是從容器控制項繼承此設定）。</span><span class="sxs-lookup"><span data-stu-id="34c5c-140">Controls that are not designed for user input (for example, the **Label** control) no longer render the `disabled="disabled"` attribute if their **Enabled** property is set to **false** (or if they inherit this setting from a container control).</span></span>

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a><span data-ttu-id="34c5c-141">ClientIDMode 變更</span><span class="sxs-lookup"><span data-stu-id="34c5c-141">ClientIDMode Changes</span></span>

<span data-ttu-id="34c5c-142">**ClientIDMode** ASP.NET 4 中的設定可讓您指定如何產生 ASP.NET**識別碼**HTML 元素的屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-142">The **ClientIDMode** setting in ASP.NET 4 lets you specify how ASP.NET generates the **id** attribute for HTML elements.</span></span> <span data-ttu-id="34c5c-143">在舊版 ASP.NET 中，預設行為是相當於**AutoID**設定**ClientIDMode**。</span><span class="sxs-lookup"><span data-stu-id="34c5c-143">In previous versions of ASP.NET, the default behavior was equivalent to the **AutoID** setting of **ClientIDMode**.</span></span> <span data-ttu-id="34c5c-144">不過，預設值是現在**可預測**。</span><span class="sxs-lookup"><span data-stu-id="34c5c-144">However, the default setting is now **Predictable**.</span></span>

<span data-ttu-id="34c5c-145">如果您使用 Visual Studio 2010 來升級您的應用程式從 ASP.NET 2.0 或 ASP.NET 3.5 為目標時，此工具會自動加入的設定`Web.config`檔案會保留舊版的.NET Framework 的行為。</span><span class="sxs-lookup"><span data-stu-id="34c5c-145">If you use Visual Studio 2010 to upgrade your application from ASP.NET 2.0 or ASP.NET 3.5, the tool automatically adds a setting to the `Web.config` file that preserves the behavior of earlier versions of the .NET Framework.</span></span> <span data-ttu-id="34c5c-146">不過，如果您將 IIS 中的應用程式集區變更成以.NET Framework 4 為目標來升級應用程式，則 ASP.NET 預設會使用新模式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-146">However, if you upgrade an application by changing the application pool in IIS to target the .NET Framework 4, ASP.NET uses the new mode by default.</span></span> <span data-ttu-id="34c5c-147">若要停用新的用戶端識別碼模式，新增下列設定中的`Web.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="34c5c-147">To disable the new client ID mode, add the following setting in the `Web.config` file:</span></span>

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a><span data-ttu-id="34c5c-148">HtmlEncode 和進行 urlencode 處理現在編碼單引號</span><span class="sxs-lookup"><span data-stu-id="34c5c-148">HtmlEncode and UrlEncode Now Encode Single Quotation Marks</span></span>

<span data-ttu-id="34c5c-149">在 ASP.NET 4 **HtmlEncode**和**進行 urlencode 處理**方法**HttpUtility**和**HttpServerUtility**類別已被更新為編碼的單引號字元 （'），如下所示：</span><span class="sxs-lookup"><span data-stu-id="34c5c-149">In ASP.NET 4, the **HtmlEncode** and **UrlEncode** methods of the **HttpUtility** and **HttpServerUtility** classes have been updated to encode the single quotation mark character (') as follows:</span></span>

- <span data-ttu-id="34c5c-150">**HtmlEncode**方法編碼單引號的執行個體 」。</span><span class="sxs-lookup"><span data-stu-id="34c5c-150">The **HtmlEncode** method encodes instances of the single quotation mark as ' .</span></span>
- <span data-ttu-id="34c5c-151">**進行 urlencode 處理**方法編碼為 %27 單引號的執行個體。</span><span class="sxs-lookup"><span data-stu-id="34c5c-151">The **UrlEncode** method encodes instances of the single quotation mark as %27.</span></span>

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a><span data-ttu-id="34c5c-152">ASP.NET 網頁 (.aspx) 剖析器為 Stricter</span><span class="sxs-lookup"><span data-stu-id="34c5c-152">ASP.NET Page (.aspx) Parser is Stricter</span></span>

<span data-ttu-id="34c5c-153">ASP.NET 網頁的網頁剖析器 (`.aspx`檔案) 和使用者控制項 (`.ascx`檔案) 更為嚴格 ASP.NET 4 中，將會拒絕多個執行個體無效的標記。</span><span class="sxs-lookup"><span data-stu-id="34c5c-153">The page parser for ASP.NET pages (`.aspx` files) and user controls (`.ascx` files) is stricter in ASP.NET 4 and will reject more instances of invalid markup.</span></span> <span data-ttu-id="34c5c-154">例如，下列兩個程式碼片段會成功剖析在舊版 ASP.NET 中，但現在將會引發在 ASP.NET 4 中的剖析器錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-154">For example, the following two snippets would successfully parse in earlier releases of ASP.NET, but will now raise parser errors in ASP.NET 4.</span></span>

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

<span data-ttu-id="34c5c-155">請注意結尾的分號無效**HiddenField**標記。</span><span class="sxs-lookup"><span data-stu-id="34c5c-155">Notice the invalid semicolon at the end of the **HiddenField** tag.</span></span>

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

<span data-ttu-id="34c5c-156">請注意封閉**樣式**會碰到的屬性**CssClass**屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-156">Notice the unclosed **style** attribute that runs into the **CssClass** attribute.</span></span>

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a><span data-ttu-id="34c5c-157">更新的瀏覽器定義檔案</span><span class="sxs-lookup"><span data-stu-id="34c5c-157">Browser Definition Files Updated</span></span>

<span data-ttu-id="34c5c-158">瀏覽器定義檔已更新成包含新增和已更新瀏覽器及裝置的資訊。</span><span class="sxs-lookup"><span data-stu-id="34c5c-158">The browser definition files have been updated to include information about new and updated browsers and devices.</span></span> <span data-ttu-id="34c5c-159">已移除 Netscape Navigator 這類較舊的瀏覽器和裝置，並已新增 Google Chrome 和 Apple iPhone 這類較新的瀏覽器和裝置。</span><span class="sxs-lookup"><span data-stu-id="34c5c-159">Older browsers and devices such as Netscape Navigator have been removed, and newer browsers and devices such as Google Chrome and Apple iPhone have been added.</span></span>

<span data-ttu-id="34c5c-160">如果您的應用程式包含繼承自其中一個已移除瀏覽器定義的自訂瀏覽器定義，則您會看到錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-160">If your application contains custom browser definitions that inherit from one of the browser definitions that have been removed, you will see an error.</span></span> <span data-ttu-id="34c5c-161">例如，如果`App_Browsers`資料夾包含瀏覽器定義繼承自 IE2 瀏覽器的定義，您會收到下列的組態錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="34c5c-161">For example, if the `App_Browsers` folder contains a browser definition that inherits from the IE2 browser definition, you will receive the following configuration error message:</span></span>

- <span data-ttu-id="34c5c-162">找不到識別碼為 'IE2' 的瀏覽器或閘道項目。</span><span class="sxs-lookup"><span data-stu-id="34c5c-162">The browser or gateway element with ID 'IE2' cannot be found.</span></span>

> [!NOTE]
> <span data-ttu-id="34c5c-163">**HttpBrowserCapabilities**物件 (它由頁面**Request.Browser**屬性) 由瀏覽器定義檔案所驅動。</span><span class="sxs-lookup"><span data-stu-id="34c5c-163">The **HttpBrowserCapabilities** object (which is exposed by the page's **Request.Browser** property) is driven by the browser definitions files.</span></span> <span data-ttu-id="34c5c-164">因此，存取這個物件在 ASP.NET 4 中的屬性所傳回的資訊可能不同於在舊版 ASP.NET 中傳回的資訊。</span><span class="sxs-lookup"><span data-stu-id="34c5c-164">Therefore, the information returned by accessing a property of this object in ASP.NET 4 might be different than the information returned in an earlier version of ASP.NET.</span></span>


<span data-ttu-id="34c5c-165">您可以從下列資料夾複製瀏覽器定義檔案還原到舊的瀏覽器定義檔案：</span><span class="sxs-lookup"><span data-stu-id="34c5c-165">You can revert to the old browser definition files by copying the browser definition files from the following folder:</span></span>

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

<span data-ttu-id="34c5c-166">將檔案複製到對應`\CONFIG\Browsers`針對 ASP.NET 4 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="34c5c-166">Copy the files into the corresponding `\CONFIG\Browsers` folder for ASP.NET 4.</span></span> <span data-ttu-id="34c5c-167">複製檔案之後，請執行 Aspnet\_regbrowsers.exe 命令列工具。</span><span class="sxs-lookup"><span data-stu-id="34c5c-167">After you copy the files, run the Aspnet\_regbrowsers.exe command-line tool.</span></span>

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a><span data-ttu-id="34c5c-168">System.Web.Mobile.dll 從根 Web 組態檔中移除</span><span class="sxs-lookup"><span data-stu-id="34c5c-168">System.Web.Mobile.dll Removed from Root Web Configuration File</span></span>

<span data-ttu-id="34c5c-169">在舊版的 ASP.NET，System.Web.Mobile.dll 組件的參考已包含在根`Web.config`檔案**組件**下一節。</span><span class="sxs-lookup"><span data-stu-id="34c5c-169">In previous versions of ASP.NET, a reference to the System.Web.Mobile.dll assembly was included in the root `Web.config` file in the **assemblies** section under.</span></span> <span data-ttu-id="34c5c-170">為了改善效能，已移除此組件的參考。</span><span class="sxs-lookup"><span data-stu-id="34c5c-170">In order to improve performance, the reference to this assembly was removed.</span></span>

<span data-ttu-id="34c5c-171">System.Web.Mobile.dll 組件會包含在 ASP.NET 4，但是它已被取代。</span><span class="sxs-lookup"><span data-stu-id="34c5c-171">The System.Web.Mobile.dll assembly is included in ASP.NET 4, but it is deprecated.</span></span> <span data-ttu-id="34c5c-172">如果您想要使用來自 System.Web.Mobile.dll 組件的類型，將這個組件的參考加入任一個根`Web.config`檔案或應用程式`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-172">If you want to use types from the System.Web.Mobile.dll assembly, add a reference to this assembly to either the root `Web.config` file or to an application `Web.config` file.</span></span> <span data-ttu-id="34c5c-173">例如，如果您想要使用任何 （已過時） 的 ASP.NET 行動控制項，您必須加入 System.Web.Mobile.dll 組件，以參考`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-173">For example, if you want to use any of the (deprecated) ASP.NET mobile controls, you must add a reference to the System.Web.Mobile.dll assembly to the `Web.config` file.</span></span>

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a><span data-ttu-id="34c5c-174">ASP.NET 要求驗證</span><span class="sxs-lookup"><span data-stu-id="34c5c-174">ASP.NET Request Validation</span></span>

<span data-ttu-id="34c5c-175">在 ASP.NET 要求驗證的功能提供特定層級的預設保護，防止跨網站指令碼 (XSS) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="34c5c-175">The request validation feature in ASP.NET provides a certain level of default protection against cross-site scripting (XSS) attacks.</span></span> <span data-ttu-id="34c5c-176">在舊版 ASP.NET 中，依預設會啟用要求驗證。</span><span class="sxs-lookup"><span data-stu-id="34c5c-176">In previous versions of ASP.NET, request validation was enabled by default.</span></span> <span data-ttu-id="34c5c-177">不過，它只套用到 ASP.NET 網頁 (`.aspx`檔和其類別檔案) 以及只執行那些頁面所。</span><span class="sxs-lookup"><span data-stu-id="34c5c-177">However, it applied only to ASP.NET pages (`.aspx` files and their class files) and only when those pages were executing.</span></span>

<span data-ttu-id="34c5c-178">在 ASP.NET 4 中，依預設，已啟用要求驗證的所有要求，因為它已啟用之前**BeginRequest** HTTP 要求的階段。</span><span class="sxs-lookup"><span data-stu-id="34c5c-178">In ASP.NET 4, by default, request validation is enabled for all requests, because it is enabled before the **BeginRequest** phase of an HTTP request.</span></span> <span data-ttu-id="34c5c-179">如此一來，要求驗證適用於所有 ASP.NET 資源，不只是.aspx 網頁要求的要求。</span><span class="sxs-lookup"><span data-stu-id="34c5c-179">As a result, request validation applies to requests for all ASP.NET resources, not just .aspx page requests.</span></span> <span data-ttu-id="34c5c-180">這包括例如 Web 服務呼叫和自訂 HTTP 處理常式的要求。</span><span class="sxs-lookup"><span data-stu-id="34c5c-180">This includes requests such as Web service calls and custom HTTP handlers.</span></span> <span data-ttu-id="34c5c-181">自訂 HTTP 模組會讀取 HTTP 要求的內容時，也是使用中要求驗證。</span><span class="sxs-lookup"><span data-stu-id="34c5c-181">Request validation is also active when custom HTTP modules are reading the contents of an HTTP request.</span></span>

<span data-ttu-id="34c5c-182">如此一來，先前未不會觸發錯誤的要求現在可能會要求驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-182">As a result, request validation errors might now occur for requests that previously did not trigger errors.</span></span> <span data-ttu-id="34c5c-183">若要還原的 ASP.NET 2.0 要求驗證功能的行為，新增下列設定中的`Web.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="34c5c-183">To revert to the behavior of the ASP.NET 2.0 request validation feature, add the following setting in the `Web.config` file:</span></span>

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

<span data-ttu-id="34c5c-184">不過，我們建議您分析以判斷是否有現有的處理常式、 模組或其他自訂程式碼可能會存取不安全的 HTTP 輸入可能 XSS 攻擊向量的任何要求驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-184">However, we recommend that you analyze any request validation errors to determine whether existing handlers, modules, or other custom code accesses potentially unsafe HTTP inputs that could be XSS attack vectors.</span></span>

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a><span data-ttu-id="34c5c-185">雜湊演算法的預設值為 Now HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="34c5c-185">Default Hashing Algorithm Is Now HMACSHA256</span></span>

<span data-ttu-id="34c5c-186">ASP.NET 使用加密和雜湊演算法來協助保護資料，例如表單驗證 Cookie 和檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="34c5c-186">ASP.NET uses both encryption and hashing algorithms to help secure data such as forms authentication cookies and view state.</span></span> <span data-ttu-id="34c5c-187">根據預設，ASP.NET 4 現在使用 HMACSHA256 演算法 cookie 和檢視狀態上的雜湊作業。</span><span class="sxs-lookup"><span data-stu-id="34c5c-187">By default, ASP.NET 4 now uses the HMACSHA256 algorithm for hash operations on cookies and view state.</span></span> <span data-ttu-id="34c5c-188">舊版 ASP.NET 會使用較舊的 HMACSHA1 演算法。</span><span class="sxs-lookup"><span data-stu-id="34c5c-188">Earlier versions of ASP.NET used the older HMACSHA1 algorithm.</span></span>

<span data-ttu-id="34c5c-189">如果您執行混合的 ASP.NET 2.0/ASP.NET 4，可能會影響您的應用程式資料，例如表單驗證 cookie 必須工作 across.NET Framework 版本的所在的環境。</span><span class="sxs-lookup"><span data-stu-id="34c5c-189">Your applications might be affected if you run mixed ASP.NET 2.0/ASP.NET 4 environments where data such as forms authentication cookies must work across.NET Framework versions.</span></span> <span data-ttu-id="34c5c-190">若要設定 ASP.NET 4 Web 應用程式使用較舊的 HMACSHA1 演算法，加入下列設定中的`Web.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="34c5c-190">To configure an ASP.NET 4 Web application to use the older HMACSHA1 algorithm, add the following setting in the `Web.config` file:</span></span>

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a><span data-ttu-id="34c5c-191">新的 ASP.NET 4 根組態相關的組態錯誤</span><span class="sxs-lookup"><span data-stu-id="34c5c-191">Configuration Errors Related to New ASP.NET 4 Root Configuration</span></span>

<span data-ttu-id="34c5c-192">根組態檔 (`machine.config`檔案及根`Web.config`檔案) 的.NET Framework 4 和 （因此 ASP.NET 4） 更新包含大部分的重複使用組態資訊，可在 ASP.NET 3.5 中找不到應用程式`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-192">The root configuration files (the `machine.config` file and the root `Web.config` file) for the .NET Framework 4 (and therefore ASP.NET 4) have been updated to include most of the boilerplate configuration information that in ASP.NET 3.5 was found in the application `Web.config` files.</span></span> <span data-ttu-id="34c5c-193">由於受管理的 IIS 7 和 IIS 7.5 組態系統的複雜度，執行 ASP.NET 4 下方和 IIS 7 和 IIS 7.5 的 ASP.NET 3.5 應用程式可能會導致 ASP.NET 或 IIS 組態錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-193">Because of the complexity of the managed IIS 7 and IIS 7.5 configuration systems, running ASP.NET 3.5 applications under ASP.NET 4 and under IIS 7 and IIS 7.5 can result in either ASP.NET or IIS configuration errors.</span></span>

<span data-ttu-id="34c5c-194">我們建議您在 Visual Studio 2010 中，使用專案升級工具來升級 ASP.NET 3.5 應用程式以 ASP.NET 4，如果可行的話。</span><span class="sxs-lookup"><span data-stu-id="34c5c-194">We recommend that you upgrade ASP.NET 3.5 applications to ASP.NET 4 by using the project upgrade tools in Visual Studio 2010, if practical.</span></span> <span data-ttu-id="34c5c-195">Visual Studio 2010 會自動修改 ASP.NET 3.5 應用程式的`Web.config`檔案以包含適當的設定，針對 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="34c5c-195">Visual Studio 2010 automatically modifies the ASP.NET 3.5 application's `Web.config` file to contain the appropriate settings for ASP.NET 4.</span></span>

<span data-ttu-id="34c5c-196">不過，它是支援的案例來執行 ASP.NET 3.5 應用程式使用.NET Framework 4，而不必重新編譯。</span><span class="sxs-lookup"><span data-stu-id="34c5c-196">However, it is a supported scenario to run ASP.NET 3.5 applications using the .NET Framework 4 without recompilation.</span></span> <span data-ttu-id="34c5c-197">在此情況下，您可能必須手動修改應用程式的`Web.config`檔案然後再執行 .NET Framework 4 下方和 IIS 7 或 IIS 7.5 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-197">In that case, you might have to manually modify the application's `Web.config` file before you run the application under the .NET Framework 4 and under IIS 7 or IIS 7.5.</span></span>

<span data-ttu-id="34c5c-198">在下兩節描述您可能需要不同組合的軟體進行的變更。</span><span class="sxs-lookup"><span data-stu-id="34c5c-198">The next two sections describe changes that you might need to make for different combinations of software.</span></span>

<span data-ttu-id="34c5c-199">**安裝既不 hotfix KB958854 或 SP2 的 Windows Vista SP1 或 Windows Server 2008 SP1。**</span><span class="sxs-lookup"><span data-stu-id="34c5c-199">**Windows Vista SP1 or Windows Server 2008 SP1, where neither hotfix KB958854 nor SP2 are installed.**</span></span> <span data-ttu-id="34c5c-200">在此組態中，IIS 7 組態系統未正確應用程式的受管理的設定的合併比較應用程式層級`Web.config`檔案，以 ASP.NET 2.0`machine.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-200">In this configuration, the IIS 7 configuration system incorrectly merges an application's managed configuration by comparing the application-level `Web.config` file to the ASP.NET 2.0 `machine.config` files.</span></span> <span data-ttu-id="34c5c-201">因為如此，應用程式層級`Web.config`來自.NET Framework 3.5 或更新版本的檔案必須有**system.web.extensions**以避免導致 IIS 7 驗證失敗的組態區段定義 （項目）。</span><span class="sxs-lookup"><span data-stu-id="34c5c-201">Because of this, application-level `Web.config` files from the .NET Framework 3.5 or later must have a **system.web.extensions** configuration section definition (the element) in order not to cause an IIS 7 validation failure.</span></span>

<span data-ttu-id="34c5c-202">不過，以手動方式修改應用程式層級`Web.config`不精確地符合 Visual Studio 2008 所導入的原始未定案組態 > 一節定義的檔案項目會導致 ASP.NET 組態錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-202">However, manually modified application-level `Web.config` file entries that do not precisely match the original boilerplate configuration section definitions that were introduced with Visual Studio 2008 will cause ASP.NET configuration errors.</span></span> <span data-ttu-id="34c5c-203">（預設組態項目所產生的 Visual Studio 2008 正常運作。）常見的問題是，以手動方式修改`Web.config`檔案遺漏**allowDefinition**和**requirePermission**各種設定區段找到的組態屬性定義。</span><span class="sxs-lookup"><span data-stu-id="34c5c-203">(The default configuration entries that are generated by Visual Studio 2008 work correctly.) A common problem is that manually modified `Web.config` files leave out the **allowDefinition** and **requirePermission** configuration attributes that are found on various configuration section definitions.</span></span> <span data-ttu-id="34c5c-204">這會導致應用程式層級中的縮寫的組態區段不符`Web.config`檔案和 ASP.NET 4 中的完整定義`machine.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-204">This causes a mismatch between the abbreviated configuration section in application-level `Web.config` files and the complete definition in the ASP.NET 4 `machine.config` file.</span></span> <span data-ttu-id="34c5c-205">如此一來，在執行階段，ASP.NET 4 組態系統，會擲回組態錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-205">As a result, at run time, the ASP.NET 4 configuration system throws a configuration error.</span></span>

<span data-ttu-id="34c5c-206">**Windows Vista SP2、 Windows Server 2008 SP2、 Windows 7、 Windows Server 2008 R2，並也 Windows Vista SP1 和 Windows Server 2008 SP1 已安裝 hotfix KB958854。**</span><span class="sxs-lookup"><span data-stu-id="34c5c-206">**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2, and also Windows Vista SP1 and Windows Server 2008 SP1 where hotfix KB958854 is installed.**</span></span>

<span data-ttu-id="34c5c-207">在此案例中，IIS 7 和 IIS 7.5 的原生的組態系統傳回組態錯誤，因為它對文字比較**類型**定義為受管理的組態區段處理常式的屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-207">In this scenario, the IIS 7 and IIS 7.5 native configuration system returns a configuration error because it performs a text comparison on the **type** attribute that is defined for a managed configuration section handler.</span></span> <span data-ttu-id="34c5c-208">因為所有`Web.config`檔案所產生的 Visual Studio 2008 與 Visual Studio 2008 SP1 已 「 3.5"中的型別字串**system.web.extensions** （及相關） 的組態區段處理常式，而且因為 ASP.NET4`machine.config`檔案中有"4.0"**類型**相同組態區段的處理常式，一律會產生 Visual Studio 2008 或 Visual Studio 2008 SP1 中的應用程式無法在 IIS 7 中的設定驗證屬性和IIS 7.5。</span><span class="sxs-lookup"><span data-stu-id="34c5c-208">Because all `Web.config` files that are generated by Visual Studio 2008 and Visual Studio 2008 SP1 have "3.5" in the type string for the **system.web.extensions** (and related) configuration section handlers, and because the ASP.NET 4 `machine.config` file has "4.0" in the **type** attribute for the same configuration section handlers, applications that are generated in Visual Studio 2008 or Visual Studio 2008 SP1 always fail configuration validation in IIS 7 and IIS 7.5.</span></span>

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a><span data-ttu-id="34c5c-209">解決這些問題</span><span class="sxs-lookup"><span data-stu-id="34c5c-209">Resolving These Issues</span></span>

<span data-ttu-id="34c5c-210">第一個案例的因應措施是，更新應用程式層級`Web.config`檔案中的未定案設定文字，藉以`Web.config`由 Visual Studio 2008 會自動產生的檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-210">The workaround for the first scenario is to update the application-level `Web.config` file by including the boilerplate configuration text from a `Web.config` file that was generated automatically by Visual Studio 2008.</span></span>

<span data-ttu-id="34c5c-211">替代的解決方法的第一個案例是在電腦上安裝 Service Pack 2 for Vista 或 Windows Server 2008 或 hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) 若要修正的不正確的組態合併行為IIS 組態系統。</span><span class="sxs-lookup"><span data-stu-id="34c5c-211">An alternative workaround for the first scenario is to install Service Pack 2 for Vista or Windows Server 2008 on your computer or to install hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) to fix the incorrect configuration-merge behavior of the IIS configuration system.</span></span> <span data-ttu-id="34c5c-212">不過，您執行這些動作之後，您的應用程式可能會遇到的組態錯誤，因為第二個案例所述的問題。</span><span class="sxs-lookup"><span data-stu-id="34c5c-212">However, after you perform either of these actions, your application will likely encounter a configuration error due to the issue described for the second scenario.</span></span>

<span data-ttu-id="34c5c-213">第二個案例的因應措施是將其刪除或標記為註解所有**system.web.extensions**組態區段定義和組態區段群組中應用程式層級定義`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-213">The workaround for the second scenario is to delete or comment out all the **system.web.extensions** configuration section definitions and configuration section group definitions from the application-level `Web.config` file.</span></span> <span data-ttu-id="34c5c-214">這些定義通常是在應用程式層級頂端`Web.config`檔案，並可以藉由識別**c**元素和其子系。</span><span class="sxs-lookup"><span data-stu-id="34c5c-214">These definitions are usually at the top of the application-level `Web.config` file and can be identified by the **configSections** element and its children.</span></span>

<span data-ttu-id="34c5c-215">如需這兩種案例中，建議您也可以手動刪除**system.codedom**區段中，雖然這並非必要。</span><span class="sxs-lookup"><span data-stu-id="34c5c-215">For both scenarios, it is recommended that you also manually delete the **system.codedom** section, although this is not required.</span></span>

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a><span data-ttu-id="34c5c-216">ASP.NET 4 個子應用程式無法啟動時在 ASP.NET 2.0 或 ASP.NET 3.5 應用程式</span><span class="sxs-lookup"><span data-stu-id="34c5c-216">ASP.NET 4 Child Applications Fail to Start When Under ASP.NET 2.0 or ASP.NET 3.5 Applications</span></span>

<span data-ttu-id="34c5c-217">因為發生組態或編譯錯誤，所以可能無法啟動設定為執行舊版 ASP.NET 之應用程式子系的 ASP.NET 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-217">ASP.NET 4 applications that are configured as children of applications that run earlier versions of ASP.NET might fail to start because of configuration or compilation errors.</span></span> <span data-ttu-id="34c5c-218">下列範例會顯示受影響的應用程式的目錄結構。</span><span class="sxs-lookup"><span data-stu-id="34c5c-218">The following example shows a directory structure for an affected application.</span></span>

<span data-ttu-id="34c5c-219">`/parentwebapp` （設定為使用 ASP.NET 2.0 或 ASP.NET 3.5）</span><span class="sxs-lookup"><span data-stu-id="34c5c-219">`/parentwebapp` (configured to use ASP.NET 2.0 or ASP.NET 3.5)</span></span>  
<span data-ttu-id="34c5c-220">`/childwebapp` （設定為使用 ASP.NET 4）</span><span class="sxs-lookup"><span data-stu-id="34c5c-220">`/childwebapp` (configured to use ASP.NET 4)</span></span>

<span data-ttu-id="34c5c-221">中的應用程式`childwebapp`資料夾將無法啟動 IIS 7 或 IIS 7.5 上，並會回報組態錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-221">The application in the `childwebapp` folder will fail to start on IIS 7 or IIS 7.5 and will report a configuration error.</span></span> <span data-ttu-id="34c5c-222">錯誤文字將包含類似下列訊息：</span><span class="sxs-lookup"><span data-stu-id="34c5c-222">The error text will include a message similar to the following:</span></span>

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

<span data-ttu-id="34c5c-223">在 IIS 6 中的應用程式上`childwebapp`資料夾也將無法啟動，但是它會報告不同的錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-223">On IIS 6, the application in the `childwebapp` folder will also fail to start, but it will report a different error.</span></span> <span data-ttu-id="34c5c-224">例如，錯誤文字可能會說明下列：</span><span class="sxs-lookup"><span data-stu-id="34c5c-224">For example, the error text might state the following:</span></span>

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

<span data-ttu-id="34c5c-225">因為發生這些狀況中的父應用程式的組態資訊`parentwebapp`資料夾是決定所使用的子網路的最後一個合併的組態設定的組態資訊的階層架構的一部分應用程式中`childwebapp`資料夾。</span><span class="sxs-lookup"><span data-stu-id="34c5c-225">These scenarios occur because the configuration information from the parent application in the `parentwebapp` folder is part of the hierarchy of configuration information that determines the final merged configuration settings that are used by the child web application in the `childwebapp` folder.</span></span> <span data-ttu-id="34c5c-226">根據 ASP.NET 4 Web 應用程式是否執行 IIS 6 或 IIS 7 （或 IIS 7.5） 上，IIS 組態系統或 ASP.NET 4 編譯系統會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-226">Depending on whether the ASP.NET 4 Web application is running on IIS 7 (or IIS 7.5) or on IIS 6, either the IIS configuration system or the ASP.NET 4 compilation system will return an error.</span></span>

<span data-ttu-id="34c5c-227">若要解決此問題，並取得子 ASP.NET 4 應用程式以使用，您必須遵循的步驟取決於 ASP.NET 4 應用程式是否執行 IIS 6 或 IIS 7 （或 IIS 7.5）。</span><span class="sxs-lookup"><span data-stu-id="34c5c-227">The steps that you must follow to resolve this issue and to get the child ASP.NET 4 application to work depend on whether the ASP.NET 4 application runs on IIS 6 or on IIS 7 (or IIS 7.5).</span></span>

### <a name="step-1-iis-7-or-iis-75-only"></a><span data-ttu-id="34c5c-228">步驟 1 （IIS 7 或 IIS 7.5 只）</span><span class="sxs-lookup"><span data-stu-id="34c5c-228">Step 1 (IIS 7 or IIS 7.5 only)</span></span>

<span data-ttu-id="34c5c-229">這個步驟是在執行 IIS 7 的作業系統或 IIS 7.5，包括 Windows Vista、 Windows Server 2008、 Windows 7 和 Windows Server 2008 R2 上必要的。</span><span class="sxs-lookup"><span data-stu-id="34c5c-229">This step is necessary only on operating systems that run IIS 7 or IIS 7.5, which includes Windows Vista, Windows Server 2008, Windows 7, and Windows Server 2008 R2.</span></span>

<span data-ttu-id="34c5c-230">移動**c**中的定義`Web.config`上層應用程式 （執行 ASP.NET 2.0 或 ASP.NET 3.5 為目標的應用程式） 的檔案到根`Web.config`.net Framework 2.0 的檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-230">Move the **configSections** definition in the `Web.config` file of the parent application (the application that runs ASP.NET 2.0 or ASP.NET 3.5) into the root `Web.config` file for the.NET Framework 2.0.</span></span> <span data-ttu-id="34c5c-231">IIS 7 和 IIS 7.5 的原生組態系統會掃描**c**時它會合併組態檔的階層項目。</span><span class="sxs-lookup"><span data-stu-id="34c5c-231">The IIS 7 and IIS 7.5 native configuration system scans the **configSections** element when it merges the hierarchy of configuration files.</span></span> <span data-ttu-id="34c5c-232">移動**c**從上層 Web 應用程式定義`Web.config`根檔案`Web.config`檔案可以有效地隱藏的組態合併處理程序之子系 ASP.NET 4 中的項目應用程式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-232">Moving the **configSections** definition from the parent Web application's `Web.config` file to the root `Web.config` file effectively hides the element from the configuration merge process that occurs for the child ASP.NET 4 application.</span></span>

<span data-ttu-id="34c5c-233">在 32 位元作業系統上或 32 位元應用程式集區，根`Web.config`ASP.NET 2.0 與 ASP.NET 3.5 檔通常位於下列資料夾中：</span><span class="sxs-lookup"><span data-stu-id="34c5c-233">On a 32-bit operating system or for 32-bit application pools, the root `Web.config` file for ASP.NET 2.0 and ASP.NET 3.5 is normally located in the following folder:</span></span>

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

<span data-ttu-id="34c5c-234">在 64 位元作業系統上或 64 位元應用程式集區，根`Web.config`ASP.NET 2.0 與 ASP.NET 3.5 檔通常位於下列資料夾中：</span><span class="sxs-lookup"><span data-stu-id="34c5c-234">On a 64-bit operating system or for 64-bit application pools, the root `Web.config` file for ASP.NET 2.0 and ASP.NET 3.5 is normally located in the following folder:</span></span>

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

<span data-ttu-id="34c5c-235">如果您在 64 位元電腦上執行 32 位元和 64 位元的 Web 應用程式，您必須移動**c**項目上移至根`Web.config`32 位元和 64 位元系統檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-235">If you run both 32-bit and 64-bit Web applications on a 64-bit computer, you must move the **configSections** element up into root `Web.config` files for both the 32-bit and the 64-bit systems.</span></span>

<span data-ttu-id="34c5c-236">當您將**c**根目錄中的項目`Web.config`檔案中，貼上一節之後立即**組態**項目。</span><span class="sxs-lookup"><span data-stu-id="34c5c-236">When you put the **configSections** element in the root `Web.config` file, paste the section immediately after the **configuration** element.</span></span> <span data-ttu-id="34c5c-237">下列範例顯示哪些上半部根`Web.config`檔案應該看起來像當您完成移動項目。</span><span class="sxs-lookup"><span data-stu-id="34c5c-237">The following example shows what the top portion of the root `Web.config` file should look like when you have finished moving the elements.</span></span>

> [!NOTE]
> <span data-ttu-id="34c5c-238">在下列範例中，已針對可讀性包裝行。</span><span class="sxs-lookup"><span data-stu-id="34c5c-238">In the following example, lines have been wrapped for readability.</span></span>


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a><span data-ttu-id="34c5c-239">步驟 2 （所有版本的 IIS）</span><span class="sxs-lookup"><span data-stu-id="34c5c-239">Step 2 (all versions of IIS)</span></span>

<span data-ttu-id="34c5c-240">這個步驟是必要的 ASP.NET 4 子系 Web 應用程式是否正在執行 IIS 6 或 IIS 7 （或 IIS 7.5）。</span><span class="sxs-lookup"><span data-stu-id="34c5c-240">This step is required whether the ASP.NET 4 child Web application is running on IIS 6 or on IIS 7 (or IIS 7.5).</span></span>

<span data-ttu-id="34c5c-241">在`Web.config`父 Web 應用程式正在使用 ASP.NET 2 或 ASP.NET 3.5 為目標的檔案加入**位置**（適用於 IIS 及 ASP.NET 組態系統） 會明確指定的標記，只能的組態項目適用於父 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-241">In the `Web.config` file of the parent Web application that is running ASP.NET 2 or ASP.NET 3.5, add a **location** tag that explicitly specifies (for both the IIS and ASP.NET configuration systems) that the configuration entries only apply to the parent Web application.</span></span> <span data-ttu-id="34c5c-242">下列範例示範的語法**位置**来加入項目：</span><span class="sxs-lookup"><span data-stu-id="34c5c-242">The following example shows the syntax of the **location** element to add:</span></span>

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

<span data-ttu-id="34c5c-243">下列範例會示範如何**位置**標記用來包裝開頭的所有設定區段**appSettings**區段，以結束**system.webServer**> 一節。</span><span class="sxs-lookup"><span data-stu-id="34c5c-243">The following example shows how the **location** tag is used to wrap all configuration sections starting with the **appSettings** section and ending with **system.webServer** section.</span></span>

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

<span data-ttu-id="34c5c-244">當您完成步驟 1 和 2 時，子 ASP.NET 4 Web 應用程式會啟動，沒有錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-244">When you have completed steps 1 and 2, child ASP.NET 4 Web applications will start without errors.</span></span>

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a><span data-ttu-id="34c5c-245">ASP.NET 4 Web Sites 無法安裝 SharePoint 的電腦上啟動</span><span class="sxs-lookup"><span data-stu-id="34c5c-245">ASP.NET 4 Web Sites Fail to Start on Computers Where SharePoint Is Installed</span></span>

<span data-ttu-id="34c5c-246">執行 SharePoint 的網頁伺服器則有`Web.config`部署在 SharePoint 網站的根目錄的檔案 (例如，`c:\inetpub\wwwroot\web.config`預設的網站)。</span><span class="sxs-lookup"><span data-stu-id="34c5c-246">Web servers that run SharePoint have a `Web.config` file that is deployed at the root of a SharePoint Web site (for example, `c:\inetpub\wwwroot\web.config` for Default Web Site).</span></span> <span data-ttu-id="34c5c-247">在這個`Web.config`檔案中，SharePoint 設定自訂的部分信任層級名為 WSS\_最少。</span><span class="sxs-lookup"><span data-stu-id="34c5c-247">In this `Web.config` file, SharePoint sets a custom partial-trust level named WSS\_Minimal.</span></span>

<span data-ttu-id="34c5c-248">如果您嘗試以這種類型的 SharePoint 網站的子系執行部署 ASP.NET 4 Web 站台，您會看到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="34c5c-248">If you try to run an ASP.NET 4 Web site that is deployed as a child of this type of SharePoint Web site, you will see the following error:</span></span>

`Could not find permission set named 'ASP.NET'.`

<span data-ttu-id="34c5c-249">因為 ASP.NET 4 程式碼存取安全性 (CAS) 基礎結構會尋找名為 ASP.NET 的權限集合，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-249">This error occurs because the ASP.NET 4 code access security (CAS) infrastructure looks for a permission set named ASP.NET.</span></span> <span data-ttu-id="34c5c-250">不過，部分信任 WSS 所參考的組態檔\_最少不包含具有該名稱的任何權限集。</span><span class="sxs-lookup"><span data-stu-id="34c5c-250">However, the partial trust configuration file that is referenced by WSS\_Minimal does not contain any permission sets with that name.</span></span>

<span data-ttu-id="34c5c-251">目前沒有任何可用的 SharePoint 與 ASP.NET 相容的版本。</span><span class="sxs-lookup"><span data-stu-id="34c5c-251">Currently there is not a version of SharePoint available that is compatible with ASP.NET.</span></span> <span data-ttu-id="34c5c-252">如此一來，您應該嘗試執行 ASP.NET 4 Web 站台作為子站台之下 SharePoint Web 站台。</span><span class="sxs-lookup"><span data-stu-id="34c5c-252">As a result, you should not attempt to run an ASP.NET 4 Web site as a child site underneath SharePoint Web sites.</span></span>

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a><span data-ttu-id="34c5c-253">HttpRequest.FilePath 屬性不再包含 PathInfo 值</span><span class="sxs-lookup"><span data-stu-id="34c5c-253">The HttpRequest.FilePath Property No Longer Includes PathInfo Values</span></span>

<span data-ttu-id="34c5c-254">舊版 ASP.NET 包含**PathInfo**從各種不同的檔案路徑相關的屬性，包括傳回值中的值**HttpRequest.FilePath**， **HttpRequest.AppRelativeCurrentExecutionFilePath**，和**HttpRequest.CurrentExecutionFilePath**。</span><span class="sxs-lookup"><span data-stu-id="34c5c-254">Previous versions of ASP.NET included a **PathInfo** value in the value returned from various file path-related properties, including **HttpRequest.FilePath**, **HttpRequest.AppRelativeCurrentExecutionFilePath**, and **HttpRequest.CurrentExecutionFilePath**.</span></span> <span data-ttu-id="34c5c-255">ASP.NET 4 已不再包含**PathInfo**這些屬性的傳回值中的值。</span><span class="sxs-lookup"><span data-stu-id="34c5c-255">ASP.NET 4 no longer includes the **PathInfo** value in the return values from these properties.</span></span> <span data-ttu-id="34c5c-256">相反地， **PathInfo**資訊可用於**HttpRequest.PathInfo**。</span><span class="sxs-lookup"><span data-stu-id="34c5c-256">Instead, the **PathInfo** information is available in **HttpRequest.PathInfo**.</span></span> <span data-ttu-id="34c5c-257">例如，假設有下列 URL 片段：</span><span class="sxs-lookup"><span data-stu-id="34c5c-257">For example, imagine the following URL fragment:</span></span>

`/testapp/Action.mvc/SomeAction`

<span data-ttu-id="34c5c-258">在舊版 ASP.NET， **HttpRequest**屬性有下列值：</span><span class="sxs-lookup"><span data-stu-id="34c5c-258">In earlier versions of ASP.NET, **HttpRequest** properties have the following values:</span></span>

<span data-ttu-id="34c5c-259">**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`</span><span class="sxs-lookup"><span data-stu-id="34c5c-259">**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`</span></span>

<span data-ttu-id="34c5c-260">**HttpRequest.PathInfo**: （空白）</span><span class="sxs-lookup"><span data-stu-id="34c5c-260">**HttpRequest.PathInfo**: (empty)</span></span>

<span data-ttu-id="34c5c-261">在 ASP.NET 4 **HttpRequest**屬性改為包含下列值：</span><span class="sxs-lookup"><span data-stu-id="34c5c-261">In ASP.NET 4, **HttpRequest** properties instead have the following values:</span></span>

<span data-ttu-id="34c5c-262">**HttpRequest.FilePath**: `/testapp/Action.mvc`</span><span class="sxs-lookup"><span data-stu-id="34c5c-262">**HttpRequest.FilePath**: `/testapp/Action.mvc`</span></span>

<span data-ttu-id="34c5c-263">**HttpRequest.PathInfo**: `SomeAction`</span><span class="sxs-lookup"><span data-stu-id="34c5c-263">**HttpRequest.PathInfo**: `SomeAction`</span></span>

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a><span data-ttu-id="34c5c-264">ASP.NET 2.0 應用程式可能會產生參考 eurl.axd HttpException 錯誤</span><span class="sxs-lookup"><span data-stu-id="34c5c-264">ASP.NET 2.0 Applications Might Generate HttpException Errors that Reference eurl.axd</span></span>

<span data-ttu-id="34c5c-265">ASP.NET 4 啟用 IIS 6 上之後，在 IIS 6 （在 Windows Server 2003 或 Windows Server 2003 R2） 執行的 ASP.NET 2.0 應用程式可能會產生錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="34c5c-265">After ASP.NET 4 has been enabled on IIS 6, ASP.NET 2.0 applications that run on IIS 6 (in either Windows Server 2003 or Windows Server 2003 R2) might generate errors such as the following:</span></span>

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

<span data-ttu-id="34c5c-266">當 ASP.NET 偵測到網站已設定為使用 ASP.NET 4，原生元件的 ASP.NET 4 將無副檔名 URL ASP.NET 的 managed 部分進行進一步的處理，就會發生這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-266">This error occurs because when ASP.NET detects that a Web site is configured to use ASP.NET 4, a native component of ASP.NET 4 passes an extensionless URL to the managed portion of ASP.NET for further processing.</span></span> <span data-ttu-id="34c5c-267">不過，如果 ASP.NET 4 Web 站台之下的虛擬目錄設定為使用 ASP.NET 2.0，正在處理中修改過的 URL，其中包含字串"eurl.axd"此方法會產生無副檔名 URL。</span><span class="sxs-lookup"><span data-stu-id="34c5c-267">However, if virtual directories that are below an ASP.NET 4 Web site are configured to use ASP.NET 2.0, processing the extensionless URL in this way results in a modified URL that contains the string "eurl.axd".</span></span> <span data-ttu-id="34c5c-268">這個修改過的 URL，然後會傳送至 ASP.NET 2.0 應用程式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-268">This modified URL is then sent to the ASP.NET 2.0 application.</span></span> <span data-ttu-id="34c5c-269">ASP.NET 2.0 無法辨識"eurl.axd 」 格式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-269">ASP.NET 2.0 cannot recognize the "eurl.axd" format.</span></span> <span data-ttu-id="34c5c-270">因此，ASP.NET 2.0 會尋找名為`eurl.axd`並加以執行。</span><span class="sxs-lookup"><span data-stu-id="34c5c-270">Therefore, ASP.NET 2.0 tries to find a file named `eurl.axd` and execute it.</span></span> <span data-ttu-id="34c5c-271">沒有這類檔案存在，因此要求失敗與**HttpException**例外狀況。</span><span class="sxs-lookup"><span data-stu-id="34c5c-271">Because no such file exists, the request fails with an **HttpException** exception.</span></span>

<span data-ttu-id="34c5c-272">您可以暫時解決此問題，使用下列選項之一。</span><span class="sxs-lookup"><span data-stu-id="34c5c-272">You can work around this issue using one of the following options.</span></span>

### <a name="option-1"></a><span data-ttu-id="34c5c-273">選項 1</span><span class="sxs-lookup"><span data-stu-id="34c5c-273">Option 1</span></span>

<span data-ttu-id="34c5c-274">如果 ASP.NET 4 不需要執行的網站，請重新對應站台改為使用 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="34c5c-274">If ASP.NET 4 is not required in order to run the Web site, remap the site to use ASP.NET 2.0 instead.</span></span>

### <a name="option-2"></a><span data-ttu-id="34c5c-275">選項 2</span><span class="sxs-lookup"><span data-stu-id="34c5c-275">Option 2</span></span>

<span data-ttu-id="34c5c-276">如果 ASP.NET 4，才能執行網站，請將任何子 ASP.NET 2.0 的虛擬目錄移至不同的網站，則對應到 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="34c5c-276">If ASP.NET 4 is required in order to run the Web site, move any child ASP.NET 2.0 virtual directories to a different Web site that is mapped to ASP.NET 2.0.</span></span>

### <a name="option-3"></a><span data-ttu-id="34c5c-277">選項 3</span><span class="sxs-lookup"><span data-stu-id="34c5c-277">Option 3</span></span>

<span data-ttu-id="34c5c-278">如果您不太實用，若要重新對應至 ASP.NET 2.0 網站，或變更虛擬目錄的位置，明確停用 ASP.NET 4 中的處理無副檔名 URL。</span><span class="sxs-lookup"><span data-stu-id="34c5c-278">If it is not practical to remap the Web site to ASP.NET 2.0 or to change the location of a virtual directory, explicitly disable extensionless URL processing in ASP.NET 4.</span></span> <span data-ttu-id="34c5c-279">使用下列程序：</span><span class="sxs-lookup"><span data-stu-id="34c5c-279">Use the following procedure:</span></span>

1. <span data-ttu-id="34c5c-280">在 Windows 登錄中，開啟下列節點：</span><span class="sxs-lookup"><span data-stu-id="34c5c-280">In the Windows registry, open the following node:</span></span>

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. <span data-ttu-id="34c5c-281">建立新**DWORD**名為值**EnableExtensionlessUrls**。</span><span class="sxs-lookup"><span data-stu-id="34c5c-281">Create a new **DWORD** value named **EnableExtensionlessUrls**.</span></span>
2. <span data-ttu-id="34c5c-282">設定**EnableExtensionlessUrls**設為 0。</span><span class="sxs-lookup"><span data-stu-id="34c5c-282">Set **EnableExtensionlessUrls** to 0.</span></span> <span data-ttu-id="34c5c-283">這會停用無副檔名 URL 行為。</span><span class="sxs-lookup"><span data-stu-id="34c5c-283">This disables extensionless URL behavior.</span></span>
3. <span data-ttu-id="34c5c-284">儲存登錄值並關閉登錄編輯程式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-284">Save the registry value and close the registry editor.</span></span>
4. <span data-ttu-id="34c5c-285">執行**iisreset**命令列工具，這會導致 IIS 以讀取新的登錄值。</span><span class="sxs-lookup"><span data-stu-id="34c5c-285">Run the **iisreset** command-line tool, which causes IIS to read the new registry value.</span></span>

> [!NOTE]
> <span data-ttu-id="34c5c-286">設定**EnableExtensionlessUrls** 1 會讓無副檔名 URL 行為。</span><span class="sxs-lookup"><span data-stu-id="34c5c-286">Setting **EnableExtensionlessUrls** to 1 enables extensionless URL behavior.</span></span> <span data-ttu-id="34c5c-287">這是未不指定任何值時的預設值。</span><span class="sxs-lookup"><span data-stu-id="34c5c-287">This is the default setting if no value is specified.</span></span>


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a><span data-ttu-id="34c5c-288">事件處理常式可能不會不引發 IIS 7 或 IIS 7.5 中的預設文件在整合模式</span><span class="sxs-lookup"><span data-stu-id="34c5c-288">Event Handlers Might Not Be Not Raised in a Default Document in IIS 7 or IIS 7.5 Integrated Mode</span></span>

<span data-ttu-id="34c5c-289">ASP.NET 4 包含變更的修改如何**動作**屬性的 html**表單**項目會呈現時的無副檔名 URL 解析為預設文件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-289">ASP.NET 4 includes modifications that change how the **action** attribute of the HTML **form** element is rendered when an extensionless URL resolves to a default document.</span></span> <span data-ttu-id="34c5c-290">無副檔名 URL 解析為預設文件的範例是[ http://contoso.com/ ](http://contoso.com/)，產生的要求中[ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx)。</span><span class="sxs-lookup"><span data-stu-id="34c5c-290">An example of an extensionless URL resolving to a default document would be [http://contoso.com/](http://contoso.com/), resulting in a request to [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).</span></span>

<span data-ttu-id="34c5c-291">ASP.NET 4 現在會呈現 HTML**表單**項目的**動作**至已對應到它的預設文件的無副檔名 URL 提出要求時，屬性值為空字串。</span><span class="sxs-lookup"><span data-stu-id="34c5c-291">ASP.NET 4 now renders the HTML **form** element's **action** attribute value as an empty string when a request is made to an extensionless URL that has a default document mapped to it.</span></span> <span data-ttu-id="34c5c-292">例如，在較舊版本的 ASP.NET，要求[ http://contoso.com ](http://contoso.com)的要求會導致`Default.aspx`。</span><span class="sxs-lookup"><span data-stu-id="34c5c-292">For example, in earlier releases of ASP.NET, a request to [http://contoso.com](http://contoso.com) would result in a request to `Default.aspx`.</span></span> <span data-ttu-id="34c5c-293">在文件中開啟**表單**會呈現標記，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="34c5c-293">In that document, the opening **form** tag would be rendered as in the following example:</span></span>

`<form action="Default.aspx" />`

<span data-ttu-id="34c5c-294">在 ASP.NET 4 中，要求[ http://contoso.com ](http://contoso.com)也會導致的要求`Default.aspx`。</span><span class="sxs-lookup"><span data-stu-id="34c5c-294">In ASP.NET 4, a request to [http://contoso.com](http://contoso.com) also results in a request to `Default.aspx`.</span></span> <span data-ttu-id="34c5c-295">不過，ASP.NET 現在會呈現 HTML 開啟**表單**標記，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="34c5c-295">However, ASP.NET now renders the HTML opening **form** tag as in the following example:</span></span>

`<form action="" />`

<span data-ttu-id="34c5c-296">如何在這項差異**動作**轉譯屬性會導致表單張貼的 IIS 和 ASP.NET 的處理方式稍有變更。</span><span class="sxs-lookup"><span data-stu-id="34c5c-296">This difference in how the **action** attribute is rendered can cause subtle changes in how a form post is processed by IIS and ASP.NET.</span></span> <span data-ttu-id="34c5c-297">當**動作**屬性是空字串，IIS **DefaultDocumentModule**物件將會建立的子要求`Default.aspx`。</span><span class="sxs-lookup"><span data-stu-id="34c5c-297">When the **action** attribute is an empty string, the IIS **DefaultDocumentModule** object will create a child request to `Default.aspx`.</span></span> <span data-ttu-id="34c5c-298">在大部分情況中，這個子要求是對應用程式的程式碼，而`Default.aspx`頁面將會正常執行。</span><span class="sxs-lookup"><span data-stu-id="34c5c-298">Under most conditions, this child request is transparent to application code, and the `Default.aspx` page runs normally.</span></span>

<span data-ttu-id="34c5c-299">不過，Managed 程式碼與 IIS 7 或 IIS 7.5 整合模式之間的可能互動可能會在子要求期間讓受管理 .aspx 頁面適當地停止運作。</span><span class="sxs-lookup"><span data-stu-id="34c5c-299">However, a potential interaction between managed code and IIS 7 or IIS 7.5 Integrated mode can cause managed .aspx pages to stop working properly during the child request.</span></span> <span data-ttu-id="34c5c-300">如果發生下列狀況，子要求`Default.aspx`文件將會導致錯誤或非預期的行為：</span><span class="sxs-lookup"><span data-stu-id="34c5c-300">If the following conditions occur, the child request to a `Default.aspx` document will result in an error or in unexpected behavior:</span></span>

1. <span data-ttu-id="34c5c-301">.aspx 網頁傳送到瀏覽器中使用**表單**項目的**動作**屬性設為""。</span><span class="sxs-lookup"><span data-stu-id="34c5c-301">An .aspx page is sent to the browser with the **form** element's **action** attribute set to "".</span></span>
2. <span data-ttu-id="34c5c-302">表單回傳至 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="34c5c-302">The form is posted back to ASP.NET.</span></span>
3. <span data-ttu-id="34c5c-303">受管理的 HTTP 模組會讀取實體內容的某些部分。</span><span class="sxs-lookup"><span data-stu-id="34c5c-303">A managed HTTP module reads some part of the entity body.</span></span> <span data-ttu-id="34c5c-304">例如，模組會讀取**Request.Form**或**Request.Params**。</span><span class="sxs-lookup"><span data-stu-id="34c5c-304">For example, a module reads **Request.Form** or **Request.Params**.</span></span> <span data-ttu-id="34c5c-305">這會將 POST 要求的實體主體讀入受管理記憶體中。</span><span class="sxs-lookup"><span data-stu-id="34c5c-305">This causes the entity body of the POST request to be read into managed memory.</span></span> <span data-ttu-id="34c5c-306">因此，任何以 IIS 7 或 IIS 7.5 整合模式執行的機器碼模組都無法再使用實體主體。</span><span class="sxs-lookup"><span data-stu-id="34c5c-306">As a result, the entity body is no longer available to any native code modules that are running in IIS 7 or IIS 7.5 Integrated mode.</span></span>
4. <span data-ttu-id="34c5c-307">IIS **DefaultDocumentModule**物件最後執行並在建立的子要求`Default.aspx`文件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-307">The IIS **DefaultDocumentModule** object eventually runs and creates a child request to the `Default.aspx` document.</span></span> <span data-ttu-id="34c5c-308">不過，因為 Managed 程式碼的某個部分已經讀取實體主體，所以沒有實體主體可用來傳送至子要求。</span><span class="sxs-lookup"><span data-stu-id="34c5c-308">However, because the entity body has already been read by a piece of managed code, there is no entity body available to send to the child request.</span></span>
5. <span data-ttu-id="34c5c-309">針對子要求的處理常式的 HTTP 管線執行時`.aspx`handler-execute 階段期間執行的檔案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-309">When the HTTP pipeline runs for the child request, the handler for `.aspx` files runs during the handler-execute phase.</span></span>
6. <span data-ttu-id="34c5c-310">沒有實體主體，因為有任何形式的變數和檢視狀態，，因此沒有資訊可用來判斷哪些事件 （如果有的話） 應該要引發之.aspx 網頁處理常式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-310">Because there is no entity body, there are no form variables and no view state, and therefore no information is available for the .aspx page handler to determine what event (if any) is supposed to be raised.</span></span> <span data-ttu-id="34c5c-311">因此，未執行受影響 .aspx 頁面的回傳事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="34c5c-311">As a result, none of the postback event handlers for the affected .aspx page run.</span></span>

<span data-ttu-id="34c5c-312">您可以下列方式來解決這個問題：</span><span class="sxs-lookup"><span data-stu-id="34c5c-312">You can work around this behavior in the following ways:</span></span>

- <span data-ttu-id="34c5c-313">識別在預設文件要求期間存取該要求實體主體的 HTTP 模組，並判斷是否可以設定要執行只會針對受管理的要求。</span><span class="sxs-lookup"><span data-stu-id="34c5c-313">Identify the HTTP module that is accessing the request's entity body during default document requests and determine whether it can be configured to run only for managed requests.</span></span> <span data-ttu-id="34c5c-314">在 IIS 7 和 IIS 7.5 的整合式模式，可以標示 HTTP 模組執行只會針對受管理的要求，將下列屬性加入至模組的**system.webserver/modules**項目：</span><span class="sxs-lookup"><span data-stu-id="34c5c-314">In Integrated mode for both IIS 7 and IIS 7.5, HTTP modules can be marked to run only for managed requests by adding the following attribute to the module's **system.webServer/modules** entry:</span></span>

- `precondition="managedHandler"`

- <span data-ttu-id="34c5c-315">針對此模組要求 IIS 7 和 IIS 7.5 判斷為不符合這個設定會停用受管理的要求。</span><span class="sxs-lookup"><span data-stu-id="34c5c-315">This setting disables the module for requests that IIS 7 and IIS 7.5 determine as not being managed requests.</span></span> <span data-ttu-id="34c5c-316">預設文件的要求，第一個要求是無副檔名的 URL。</span><span class="sxs-lookup"><span data-stu-id="34c5c-316">For default document requests, the first request is to an extensionless URL.</span></span> <span data-ttu-id="34c5c-317">因此，IIS 不會標示任何受管理的模組會以執行 managed 處理常式的前置條件初始要求處理期間。</span><span class="sxs-lookup"><span data-stu-id="34c5c-317">Therefore, IIS does not run any managed modules that are marked with a precondition of managed Handler during initial request processing.</span></span> <span data-ttu-id="34c5c-318">如此一來，受管理的模組將不會意外讀取實體內容，因此實體仍然可以使用，並傳遞沿著子要求，並預設文件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-318">As a result, managed modules will not accidentally read the entity body and thus the entity body is still available and is passed along to the child request and to the default document.</span></span>

- <span data-ttu-id="34c5c-319">如果有問題的 HTTP 模組都必須執行的所有要求 (用於靜態檔案，解析成無副檔名 url **DefaultDocumentModule**物件，針對受管理的要求等)，明確修改受影響的.aspx 頁面設定**動作**屬性時，頁面**System.Web.UI.HtmlControls.HtmlForm**控制項為非空白字串。</span><span class="sxs-lookup"><span data-stu-id="34c5c-319">If the problematic HTTP modules have to run for all requests (for static files, for extensionless URLs that resolve to the **DefaultDocumentModule** object, for managed requests, etc.), modify the affected .aspx pages by explicitly setting the **Action** property of the page's **System.Web.UI.HtmlControls.HtmlForm** control to a non-empty string.</span></span> <span data-ttu-id="34c5c-320">例如，如果預設文件是`Default.aspx`，修改頁面的程式碼，以明確地設定**HtmlForm**控制項的**動作**"Default.aspx"的屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-320">For example, if the default document is `Default.aspx`, modify the page's code to explicitly set the **HtmlForm** control's **Action** property to "Default.aspx".</span></span>

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a><span data-ttu-id="34c5c-321">ASP.NET 程式碼存取安全性 (CAS) 實作的變更</span><span class="sxs-lookup"><span data-stu-id="34c5c-321">Changes to the ASP.NET Code Access Security (CAS) Implementation</span></span>

<span data-ttu-id="34c5c-322">ASP.NET 2.0 中，並由延伸模組 3.5 中所加入的 ASP.NET 功能會使用.NET Framework 1.1 和 2.0 程式碼存取安全性 (CAS) 模型。</span><span class="sxs-lookup"><span data-stu-id="34c5c-322">ASP.NET 2.0, and by extension the ASP.NET features that were added in 3.5, use the .NET Framework 1.1 and 2.0 code access security (CAS) model.</span></span> <span data-ttu-id="34c5c-323">不過，已大幅全面檢查 ASP.NET 4 中 CAS 的實作。</span><span class="sxs-lookup"><span data-stu-id="34c5c-323">However, the implementation of CAS in ASP.NET 4 has been substantially overhauled.</span></span> <span data-ttu-id="34c5c-324">如此一來，在全域組件快取 (GAC) 中執行的受信任程式碼所依賴的部分信任 ASP.NET 應用程式可能會因各種不同的安全性例外狀況。</span><span class="sxs-lookup"><span data-stu-id="34c5c-324">As a result, partial-trust ASP.NET applications that rely on trusted code running in the global assembly cache (GAC) might fail with various security exceptions.</span></span> <span data-ttu-id="34c5c-325">部分信任的應用程式廣泛修改機器 CAS 原則，也可能會因安全性例外狀況。</span><span class="sxs-lookup"><span data-stu-id="34c5c-325">Partial-trust applications that rely on extensive modifications to machine CAS policy might also fail with security exceptions.</span></span>

<span data-ttu-id="34c5c-326">您可以還原部分信任 ASP.NET 4 應用程式行為的 ASP.NET 1.1 和 2.0 版使用新**legacyCasModel**屬性**信任**組態項目，如下列範例所示:</span><span class="sxs-lookup"><span data-stu-id="34c5c-326">You can revert partial-trust ASP.NET 4 applications to the behavior of ASP.NET 1.1 and 2.0 using the new **legacyCasModel** attribute in the **trust** configuration element, as shown in the following example:</span></span>

`<trust level= "Medium" legacyCasModel="true" />`

<span data-ttu-id="34c5c-327">當您還原舊版的 CAS 模型時，會啟用下列的舊 CA 的行為：</span><span class="sxs-lookup"><span data-stu-id="34c5c-327">When you revert to the legacy CAS model, the following old CAS behaviors are enabled:</span></span>

- <span data-ttu-id="34c5c-328">機器 CAS 原則已生效。</span><span class="sxs-lookup"><span data-stu-id="34c5c-328">Machine CAS policy is honored.</span></span>
- <span data-ttu-id="34c5c-329">允許在單一應用程式定義域中的多個不同的權限集。</span><span class="sxs-lookup"><span data-stu-id="34c5c-329">Multiple different permission sets in a single application domain are allowed.</span></span>
- <span data-ttu-id="34c5c-330">不需要的組件在 GAC 中，會在堆疊上只有 ASP.NET 或其他.NET Framework 程式碼時叫用明確的權限的判斷提示。</span><span class="sxs-lookup"><span data-stu-id="34c5c-330">Explicit permission assertions are not required for assemblies in the GAC that are invoked when only ASP.NET or other .NET Framework code is on the stack.</span></span>

<span data-ttu-id="34c5c-331">無法還原情形的情境之一，在.NET Framework 4： 非 Web 部分信任應用程式不再可以呼叫 System.Web.dll 與 System.Web.Extensions.dll 中的某些 Api。</span><span class="sxs-lookup"><span data-stu-id="34c5c-331">One scenario cannot be reverted in the .NET Framework 4: non-Web partial-trust applications can no longer call certain APIs in System.Web.dll and System.Web.Extensions.dll.</span></span> <span data-ttu-id="34c5c-332">在舊版的.NET framework 中，是讓非 Web 部分信任應用程式，可以明確授與<strong>AspNetHostingPermission</strong>權限。</span><span class="sxs-lookup"><span data-stu-id="34c5c-332">In previous versions of the .NET Framework, it was possible for non-Web partial-trust applications to be explicitly granted <strong>AspNetHostingPermission</strong> permissions.</span></span> <span data-ttu-id="34c5c-333">這些應用程式無法再使用<strong>System.Web.HttpUtility</strong>中的型別<strong>System.Web.ClientServices。\<n g > \* 命名空間和類型與成員資格、 角色和設定檔。</span><span class="sxs-lookup"><span data-stu-id="34c5c-333">These applications could then use <strong>System.Web.HttpUtility</strong>, types in the <strong>System.Web.ClientServices.\</strong>\* namespaces, and types related to membership, roles, and profiles.</span></span> <span data-ttu-id="34c5c-334">不再支援.NET Framework 4 中從非 Web 部分信任應用程式呼叫這些類型。</span><span class="sxs-lookup"><span data-stu-id="34c5c-334">Calling these types from non-Web partial trust applications is no longer supported in the .NET Framework 4.</span></span>

> [!NOTE]
> <span data-ttu-id="34c5c-335">**HtmlEncode**和**HtmlDecode**功能**System.Web.HttpUtility**類別已移至新的.NET Framework 4 **System.Net.WebUtility**類別。</span><span class="sxs-lookup"><span data-stu-id="34c5c-335">The **HtmlEncode** and **HtmlDecode** functionality of the **System.Web.HttpUtility** class was moved to the new .NET Framework 4 **System.Net.WebUtility** class.</span></span> <span data-ttu-id="34c5c-336">如果這是所使用的唯一 ASP.NET 功能，修改為使用新的應用程式的程式碼**WebUtility**類別。</span><span class="sxs-lookup"><span data-stu-id="34c5c-336">If that was the only ASP.NET functionality that was being used, modify the application's code to use the new **WebUtility** class instead.</span></span>


<span data-ttu-id="34c5c-337">高層級的預設 CA 實作 ASP.NET 4 中所做的變更摘要如下：</span><span class="sxs-lookup"><span data-stu-id="34c5c-337">The following is a high-level summary of the changes to the default CAS implementation in ASP.NET 4:</span></span>

- <span data-ttu-id="34c5c-338">ASP.NET 應用程式定義域現在是同質性的應用程式定義域。</span><span class="sxs-lookup"><span data-stu-id="34c5c-338">ASP.NET application domains are now homogeneous application domains.</span></span> <span data-ttu-id="34c5c-339">只有部分信任和完全信任的授權集可用應用程式定義域中。</span><span class="sxs-lookup"><span data-stu-id="34c5c-339">Only partial-trust and full-trust grant sets are available in an application domain.</span></span>
- <span data-ttu-id="34c5c-340">ASP.NET 部分信任的授權集為獨立於任何企業層級、 電腦層級或使用者層級的 CAS 原則。</span><span class="sxs-lookup"><span data-stu-id="34c5c-340">ASP.NET partial-trust grant sets are independent from any enterprise-level, machine-level, or user-level CAS policy.</span></span>
- <span data-ttu-id="34c5c-341">ASP.NET 3.5 和 3.5 SP1 隨附的組件已轉換成使用.NET Framework 4 的透明度模型。</span><span class="sxs-lookup"><span data-stu-id="34c5c-341">ASP.NET assemblies that shipped in 3.5 and 3.5 SP1 have been converted to use the .NET Framework 4 transparency model.</span></span>
- <span data-ttu-id="34c5c-342">使用 ASP.NET **AspNetHostingPermission**屬性已大幅降低。</span><span class="sxs-lookup"><span data-stu-id="34c5c-342">Use of the ASP.NET **AspNetHostingPermission** attribute has been substantially reduced.</span></span> <span data-ttu-id="34c5c-343">已從公用的 ASP.NET 應用程式開發介面中移除這個屬性的大多數執行個體。</span><span class="sxs-lookup"><span data-stu-id="34c5c-343">Most instances of this attribute have been removed from the public ASP.NET APIs.</span></span>
- <span data-ttu-id="34c5c-344">動態編譯的組件所建立的 ASP.NET 組建提供者已明確地將標示為透明的組件更新。</span><span class="sxs-lookup"><span data-stu-id="34c5c-344">Dynamically compiled assemblies that are created by ASP.NET build providers have been updated to explicitly mark assemblies as transparent.</span></span>
- <span data-ttu-id="34c5c-345">現在標記的方式接受 APTCA 屬性時，則只有在 Web 裝載環境中的所有 ASP.NET 組件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-345">All ASP.NET assemblies are now marked in such a way that the APTCA attribute is honored only in Web hosting environments.</span></span> <span data-ttu-id="34c5c-346">部分信任的非 Web 裝載環境類似 ClickOnce 不能呼叫 ASP.NET 組件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-346">Partially trusted non-Web hosting environments like ClickOnce will not be able to call into ASP.NET assemblies.</span></span>

<span data-ttu-id="34c5c-347">如需有關新的 ASP.NET 4 程式碼存取安全性模型的詳細資訊，請參閱[ASP.NET 應用程式中使用程式碼存取安全性](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="34c5c-347">For more information about the new ASP.NET 4 code access security model, see [Using Code Access Security in ASP.NET Applications](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) on the MSDN Web site.</span></span>

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a><span data-ttu-id="34c5c-348">移動 MembershipUser 和 System.Web.Security 命名空間中的其他類型</span><span class="sxs-lookup"><span data-stu-id="34c5c-348">MembershipUser and Other Types in the System.Web.Security Namespace Have Been Moved</span></span>

<span data-ttu-id="34c5c-349">ASP.NET 成員資格中使用某些類型已從移動`System.Web.dll`新 System.Web.ApplicationServices.dll 組件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-349">Some types that are used in ASP.NET membership have been moved from `System.Web.dll` to the new System.Web.ApplicationServices.dll assembly.</span></span> <span data-ttu-id="34c5c-350">已移動類型，以解析用戶端和擴充 .NET Framework SKU 中類型之間的架構層相依性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-350">The types were moved in order to resolve architectural layering dependencies between types in the client and in extended .NET Framework SKUs.</span></span>

<span data-ttu-id="34c5c-351">網站專案沒有問題，因為將這些類型，因為 System.Web.ApplicationServices.dll ASP.NET 編譯系統，已加入至預設會使用參考組件的清單。</span><span class="sxs-lookup"><span data-stu-id="34c5c-351">Web site projects do not have problems as a result of moving these types, because System.Web.ApplicationServices.dll was added to the list of referenced assemblies that is used by default by the ASP.NET compilation system.</span></span> <span data-ttu-id="34c5c-352">如果您升級使用較早版本的 ASP.NET 至 ASP.NET 4 開啟 Visual Studio 2010 中建立的網站專案，專案會編譯無誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-352">If you upgrade a Web site project that was created using an earlier version of ASP.NET to ASP.NET 4 by opening it in Visual Studio 2010, the project will compile without errors.</span></span>

<span data-ttu-id="34c5c-353">同樣地，如果您升級舊版 ASP.NET 至 ASP.NET 4 中開啟 Visual Studio 2010 中建立的 Web 應用程式專案時，升級程序會將 System.Web.ApplicationServices.dll 的參考加入至專案。</span><span class="sxs-lookup"><span data-stu-id="34c5c-353">Similarly, if you upgrade a Web application project that was created in an earlier version of ASP.NET to ASP.NET 4 by opening it in Visual Studio 2010, the upgrade process adds a reference to System.Web.ApplicationServices.dll to the project.</span></span> <span data-ttu-id="34c5c-354">因此，升級的 Web 應用程式專案也會編譯無誤。</span><span class="sxs-lookup"><span data-stu-id="34c5c-354">Therefore, upgraded Web application projects will also compile without errors.</span></span>

<span data-ttu-id="34c5c-355">無誤編譯使用較早版本的 ASP.NET 所建立的 （二進位） 檔案也在 ASP.NET 4 執行，即使成員資格類型移到不同的組件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-355">Compiled (binary) files that were created using earlier versions of ASP.NET will also run without errors on ASP.NET 4, even though the membership types were moved to a different assembly.</span></span> <span data-ttu-id="34c5c-356">類型轉送資訊已加入至 ASP.NET 4 新版`System.Web.dll`，會自動傳送這些類型的執行階段參考類型的新位置。</span><span class="sxs-lookup"><span data-stu-id="34c5c-356">Type forwarding information has been added to the ASP.NET 4 version of `System.Web.dll` that automatically routes run-time references for these types to the new location for the types.</span></span>

<span data-ttu-id="34c5c-357">不過，使用特定的成員資格類型，並從舊版的 ASP.NET 已經升級的類別庫將無法編譯 ASP.NET 4 專案中使用時。</span><span class="sxs-lookup"><span data-stu-id="34c5c-357">However, class libraries that use specific membership types and that have been upgraded from earlier versions of ASP.NET will fail to compile when used in an ASP.NET 4 project.</span></span> <span data-ttu-id="34c5c-358">例如，類別庫專案可能無法編譯，並回報錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="34c5c-358">For example, a class library project might fail to compile and report an error such as the following:</span></span>

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

<span data-ttu-id="34c5c-359">您可以將您的類別庫專案中的參考，加入 System.Web.ApplicationServices.dll 來解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="34c5c-359">You can work around this problem by adding a reference in your class library project to System.Web.ApplicationServices.dll.</span></span>

<span data-ttu-id="34c5c-360">下列清單顯示*System.Web.Security*從其中移動的型別`System.Web.dll`System.Web.ApplicationServices.dll 至：</span><span class="sxs-lookup"><span data-stu-id="34c5c-360">The following list shows the *System.Web.Security* types that were moved from `System.Web.dll` to System.Web.ApplicationServices.dll:</span></span>

- <span data-ttu-id="34c5c-361">*System.Web.Security.MembershipCreateStatus*</span><span class="sxs-lookup"><span data-stu-id="34c5c-361">*System.Web.Security.MembershipCreateStatus*</span></span>
- <span data-ttu-id="34c5c-362">*System.Web.Security.Membership.CreateUserException*</span><span class="sxs-lookup"><span data-stu-id="34c5c-362">*System.Web.Security.Membership.CreateUserException*</span></span>
- <span data-ttu-id="34c5c-363">*System.Web.Security.MembershipPasswordException*</span><span class="sxs-lookup"><span data-stu-id="34c5c-363">*System.Web.Security.MembershipPasswordException*</span></span>
- <span data-ttu-id="34c5c-364">*System.Web.Security.MembershipPasswordFormat*</span><span class="sxs-lookup"><span data-stu-id="34c5c-364">*System.Web.Security.MembershipPasswordFormat*</span></span>
- <span data-ttu-id="34c5c-365">*System.Web.Security.MembershipProvider*</span><span class="sxs-lookup"><span data-stu-id="34c5c-365">*System.Web.Security.MembershipProvider*</span></span>
- <span data-ttu-id="34c5c-366">*System.Web.Security.MembershipProviderCollection*</span><span class="sxs-lookup"><span data-stu-id="34c5c-366">*System.Web.Security.MembershipProviderCollection*</span></span>
- <span data-ttu-id="34c5c-367">*System.Web.Security.MembershipUser*</span><span class="sxs-lookup"><span data-stu-id="34c5c-367">*System.Web.Security.MembershipUser*</span></span>
- <span data-ttu-id="34c5c-368">*System.Web.Security.MembershipUserCollection*</span><span class="sxs-lookup"><span data-stu-id="34c5c-368">*System.Web.Security.MembershipUserCollection*</span></span>
- <span data-ttu-id="34c5c-369">*System.Web.Security.MembershipValidatePasswordEventHandler*</span><span class="sxs-lookup"><span data-stu-id="34c5c-369">*System.Web.Security.MembershipValidatePasswordEventHandler*</span></span>
- <span data-ttu-id="34c5c-370">*System.Web.Security.ValidatePasswordEventArgs*</span><span class="sxs-lookup"><span data-stu-id="34c5c-370">*System.Web.Security.ValidatePasswordEventArgs*</span></span>
- <span data-ttu-id="34c5c-371">*System.Web.Security.RoleProvider*</span><span class="sxs-lookup"><span data-stu-id="34c5c-371">*System.Web.Security.RoleProvider*</span></span>
- <a id="0.1_a"></a><span data-ttu-id="34c5c-372">*System.Web.Configuration.MembershipPasswordCompatibilityMode*</span><span class="sxs-lookup"><span data-stu-id="34c5c-372">*System.Web.Configuration.MembershipPasswordCompatibilityMode*</span></span>

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a><span data-ttu-id="34c5c-373">輸出快取的變更來改變\*HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="34c5c-373">Output Caching Changes to Vary \* HTTP Header</span></span>

<span data-ttu-id="34c5c-374">在 ASP.NET 1.0 中造成的錯誤快取指定的頁面`Location="ServerAndClient"`做為輸出 – 快取設定，來發出`Vary:*`回應的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="34c5c-374">In ASP.NET 1.0, a bug caused cached pages that specified `Location="ServerAndClient"` as an output–cache setting to emit a `Vary:*` HTTP header in the response.</span></span> <span data-ttu-id="34c5c-375">這會影響如何告知用戶端瀏覽器永遠不要在本機快取頁面。</span><span class="sxs-lookup"><span data-stu-id="34c5c-375">This had the effect of telling client browsers to never cache the page locally.</span></span>

<span data-ttu-id="34c5c-376">在 ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar**加入方法，您可以放心地呼叫`Vary:*`標頭。</span><span class="sxs-lookup"><span data-stu-id="34c5c-376">In ASP.NET 1.1, the **System.Web.HttpCachePolicy.SetOmitVaryStar** method was added, which you could call to suppress the `Vary:*` header.</span></span> <span data-ttu-id="34c5c-377">因為可能最新變更的發出的 HTTP 標頭視為選擇此方法時的變更。</span><span class="sxs-lookup"><span data-stu-id="34c5c-377">This method was chosen because changing the emitted HTTP header was considered a potentially breaking change at the time.</span></span> <span data-ttu-id="34c5c-378">不過，開發人員有困惑在 ASP.NET 中，行為和問題報告建議開發人員不知道現有**SetOmitVaryStar**行為。</span><span class="sxs-lookup"><span data-stu-id="34c5c-378">However, developers have been confused by the behavior in ASP.NET, and bug reports suggest that developers are unaware of the existing **SetOmitVaryStar** behavior.</span></span>

<span data-ttu-id="34c5c-379">在 ASP.NET 4 中，已修正根本問題所決定。</span><span class="sxs-lookup"><span data-stu-id="34c5c-379">In ASP.NET 4, the decision was made to fix the root problem.</span></span> <span data-ttu-id="34c5c-380">`Vary:*` HTTP 標頭由指定下列指示詞的回應所不再發出：</span><span class="sxs-lookup"><span data-stu-id="34c5c-380">The `Vary:*` HTTP header is no longer emitted from responses that specify the following directive:</span></span>

`<%@OutputCache Location="ServerAndClient" %>`

<span data-ttu-id="34c5c-381">如此一來， **SetOmitVaryStar**不再需要為了隱藏`Vary:*`標頭。</span><span class="sxs-lookup"><span data-stu-id="34c5c-381">As a result, **SetOmitVaryStar** is no longer needed in order to suppress the `Vary:*` header.</span></span>

<span data-ttu-id="34c5c-382">在指定的應用程式中`Location="ServerAndClient"`中 **@ OutputCache**指示詞在頁面上，您現在可以看到的名稱所隱含的行為**位置**屬性的值-也就是說，將頁面在您呼叫而不需要瀏覽器快取**SetOmitVaryStar**方法。</span><span class="sxs-lookup"><span data-stu-id="34c5c-382">In applications that specify `Location="ServerAndClient"` in the **@ OutputCache** directive on a page, you will now see the behavior implied by the name of the **Location** attribute's value – that is, pages will be cacheable in the browser without requiring that you call the **SetOmitVaryStar** method.</span></span>

<span data-ttu-id="34c5c-383">如果您的應用程式中的頁面必須發出`Vary:*`，呼叫**AppendHeader**方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="34c5c-383">If pages in your application must emit `Vary:*`, call the **AppendHeader** method, as in the following example:</span></span>

`HttpResponse.AppendHeader("Vary","*");`

<span data-ttu-id="34c5c-384">或者，您可以變更輸出快取的值**位置**屬性設定為 「 伺服器 」。</span><span class="sxs-lookup"><span data-stu-id="34c5c-384">Alternatively, you can change the value of the output caching **Location** attribute to "Server".</span></span>

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a><span data-ttu-id="34c5c-385">Passport System.Web.Security 類型為過時</span><span class="sxs-lookup"><span data-stu-id="34c5c-385">System.Web.Security Types for Passport are Obsolete</span></span>

<span data-ttu-id="34c5c-386">內建於 ASP.NET 2.0 的 Passport 支援已經過時，不支援幾年因為 Passport (現在 LiveID) 已變更。</span><span class="sxs-lookup"><span data-stu-id="34c5c-386">The Passport support built into ASP.NET 2.0 has been obsolete and unsupported for a few years due to changes in Passport (now LiveID).</span></span> <span data-ttu-id="34c5c-387">如此一來，五種類型與 Passport 中**System.Web.Security**此時會標示為**ObsoleteAttribute**屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-387">As a result, the five types related to Passport in **System.Web.Security** are now marked with the **ObsoleteAttribute** attribute.</span></span>

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a><span data-ttu-id="34c5c-388">MenuItem.PopOutImageUrl 屬性無法轉譯 ASP.NET 4 中的映像</span><span class="sxs-lookup"><span data-stu-id="34c5c-388">The MenuItem.PopOutImageUrl Property Fails to Render an Image in ASP.NET 4</span></span>

<span data-ttu-id="34c5c-389">在 ASP.NET 3.5 *MenuItem.PopOutImageUrl*屬性可讓您指定功能表項目來表示功能表項目具有動態子功能表中顯示影像的 URL。</span><span class="sxs-lookup"><span data-stu-id="34c5c-389">In ASP.NET 3.5, the *MenuItem.PopOutImageUrl* property lets you specify the URL for an image that is displayed in a menu item to indicate that the menu item has a dynamic submenu.</span></span> <span data-ttu-id="34c5c-390">下列範例會示範如何在 ASP.NET 3.5 中的標記中指定這個屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-390">The following example shows how to specify this property in markup in ASP.NET 3.5.</span></span>

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

<span data-ttu-id="34c5c-391">因為在 ASP.NET 4 設計變更，呈現沒有輸出*PopOutImageUrl*如果設定的屬性*MenuItem*類別。</span><span class="sxs-lookup"><span data-stu-id="34c5c-391">As a result of a design change in ASP.NET 4, no output is rendered for the *PopOutImageUrl* if the property is set for the *MenuItem* class.</span></span> <span data-ttu-id="34c5c-392">相反地，您必須指定影像 URL 直接在*功能表*控制使用*StaticPopOutImageUrl*屬性或*DynamicPopOutImageUrl*屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-392">Instead, you must specify an image URL directly in the *Menu* control using either the *StaticPopOutImageUrl* property or the *DynamicPopOutImageUrl* property.</span></span> <span data-ttu-id="34c5c-393">當您使用靜態功能表中， *Menu.StaticPopOutImageUrl*屬性指定的映像，會顯示以表示靜態功能表項目具有子功能表，如下列範例所示的 URL:</span><span class="sxs-lookup"><span data-stu-id="34c5c-393">When you work with a static menu, the *Menu.StaticPopOutImageUrl* property specifies the URL for an image that is displayed in order to indicate that the static menu item has a submenu, as shown in the following example:</span></span>

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

<span data-ttu-id="34c5c-394">如果您正在使用動態功能表中，您使用*Menu.DynamicPopOutImageUrl*屬性來指定表示動態功能表項目具有子功能表的影像 URL。</span><span class="sxs-lookup"><span data-stu-id="34c5c-394">If you are working with a dynamic menu, you use the *Menu.DynamicPopOutImageUrl* property to specify the URL for an image that indicates that a dynamic menu item has a submenu.</span></span> <span data-ttu-id="34c5c-395">下列範例類似，但示範如何設定*DynamicPopOutImageUrl*針對動態功能表的屬性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-395">The following example is similar to the previous one, but shows how to set the *DynamicPopOutImageUrl* property for a dynamic menu.</span></span>

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

<span data-ttu-id="34c5c-396">如果*Menu.DynamicPopOutImageUrl*屬性未設定而*Menu.DynamicEnableDefaultPopOutImage*屬性設定為*false*，不會顯示影像。</span><span class="sxs-lookup"><span data-stu-id="34c5c-396">If the *Menu.DynamicPopOutImageUrl* property is not set and the *Menu.DynamicEnableDefaultPopOutImage* property is set to *false*, no image is displayed.</span></span> <span data-ttu-id="34c5c-397">同樣地，如果*StaticPopOutImageUrl*屬性未設定而*StaticEnableDefaultPopOutImage*屬性設定為*false*，不會顯示影像。</span><span class="sxs-lookup"><span data-stu-id="34c5c-397">Similarly, if the *StaticPopOutImageUrl* property is not set and the *StaticEnableDefaultPopOutImage* property is set to *false*, no image is displayed.</span></span>

<span data-ttu-id="34c5c-398">當您設定這些屬性的路徑時，使用正斜線 （/），而不是反斜線 (\)。</span><span class="sxs-lookup"><span data-stu-id="34c5c-398">When you set the paths for these properties, use a forward slash (/) instead of a backslash (\).</span></span> <span data-ttu-id="34c5c-399">如需詳細資訊，請參閱[Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 容錯移轉至呈現影像時的路徑包含反斜線](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu。")</span><span class="sxs-lookup"><span data-stu-id="34c5c-399">For more information, see [Menu.StaticPopOutImageUrl and Menu.DynamicPopOutImageUrl Fail to Render Images When Paths Contain Backslashes](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.")</span></span> <span data-ttu-id="34c5c-400">其他位置中這份文件。</span><span class="sxs-lookup"><span data-stu-id="34c5c-400">elsewhere in this document.</span></span>

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a><span data-ttu-id="34c5c-401">Menu.StaticPopOutImageUrl Menu.DynamicPopOutImageUrl 無法呈現影像時路徑包含反斜線</span><span class="sxs-lookup"><span data-stu-id="34c5c-401">Menu.StaticPopOutImageUrl and Menu.DynamicPopOutImageUrl Fail to Render Images When Paths Contain Backslashes</span></span>

<span data-ttu-id="34c5c-402">在 ASP.NET 4 中，使用指定的映像*Menu.StaticPopOutImageUrl*和*Menu.DynamicPopOutImageUrl*屬性將不會呈現如果路徑包含 backlashes (\)。</span><span class="sxs-lookup"><span data-stu-id="34c5c-402">In ASP.NET 4, the images that you specify using the *Menu.StaticPopOutImageUrl* and *Menu.DynamicPopOutImageUrl* properties will not render if the path contains backlashes (\).</span></span> <span data-ttu-id="34c5c-403">這是從舊版的 ASP.NET 的變更。</span><span class="sxs-lookup"><span data-stu-id="34c5c-403">This is a change from earlier versions of ASP.NET.</span></span>

<span data-ttu-id="34c5c-404">下列範例會*功能表*控制標記顯示*StaticPopOutImageUrl*屬性設定使用的路徑包含反斜線。</span><span class="sxs-lookup"><span data-stu-id="34c5c-404">The following example of *Menu* control markup shows the *StaticPopOutImageUrl* property set using a path that contains a backslash.</span></span> <span data-ttu-id="34c5c-405">在 ASP.NET 4 中，將不會呈現在屬性中指定的映像。</span><span class="sxs-lookup"><span data-stu-id="34c5c-405">In ASP.NET 4, the image specified in the property will not render.</span></span>

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

<span data-ttu-id="34c5c-406">若要更正此問題，變更在所指定的路徑值*StaticPopOutImageUrl*和*DynamicPopOutImageUrl*內容，以使用正斜線 （/）。</span><span class="sxs-lookup"><span data-stu-id="34c5c-406">To correct this issue, change path values that are specified in the *StaticPopOutImageUrl* and *DynamicPopOutImageUrl* properties to use forward slashes (/).</span></span> <span data-ttu-id="34c5c-407">下列範例會示範這項變更：</span><span class="sxs-lookup"><span data-stu-id="34c5c-407">The following example shows this change:</span></span>

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

<span data-ttu-id="34c5c-408">請注意，較早版本的 ASP.NET 至 ASP.NET 4 已移轉的應用程式可能也會受到影響，因為*MenuItem.PopOutImageUrl*屬性已變更。</span><span class="sxs-lookup"><span data-stu-id="34c5c-408">Note that applications that were migrated from earlier versions of ASP.NET to ASP.NET 4 might also be affected, because the *MenuItem.PopOutImageUrl* property has been changed.</span></span> <span data-ttu-id="34c5c-409">如需詳細資訊，請參閱[MenuItem.PopOutImageUrl 屬性無法轉譯 ASP.NET 4 中的映像](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert")這份文件中的其他位置。</span><span class="sxs-lookup"><span data-stu-id="34c5c-409">For more information, see [The MenuItem.PopOutImageUrl Property Fails to Render an Image in ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") elsewhere in this document.</span></span>

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a><span data-ttu-id="34c5c-410">免責聲明</span><span class="sxs-lookup"><span data-stu-id="34c5c-410">Disclaimer</span></span>

<span data-ttu-id="34c5c-411">這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。</span><span class="sxs-lookup"><span data-stu-id="34c5c-411">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="34c5c-412">本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。</span><span class="sxs-lookup"><span data-stu-id="34c5c-412">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="34c5c-413">Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。</span><span class="sxs-lookup"><span data-stu-id="34c5c-413">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="34c5c-414">本技術白皮書僅供參考。</span><span class="sxs-lookup"><span data-stu-id="34c5c-414">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="34c5c-415">MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。</span><span class="sxs-lookup"><span data-stu-id="34c5c-415">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="34c5c-416">承諾遵守所有適用的著作權法是使用者的責任。</span><span class="sxs-lookup"><span data-stu-id="34c5c-416">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="34c5c-417">著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。</span><span class="sxs-lookup"><span data-stu-id="34c5c-417">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="34c5c-418">本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。</span><span class="sxs-lookup"><span data-stu-id="34c5c-418">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="34c5c-419">除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。</span><span class="sxs-lookup"><span data-stu-id="34c5c-419">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="34c5c-420">除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人員、 地點及事件屬虛構，以及與任何真實的公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件純屬巧合。</span><span class="sxs-lookup"><span data-stu-id="34c5c-420">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="34c5c-421">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="34c5c-421">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="34c5c-422">著作權所有，並保留一切權利。</span><span class="sxs-lookup"><span data-stu-id="34c5c-422">All rights reserved.</span></span>

<span data-ttu-id="34c5c-423">Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。</span><span class="sxs-lookup"><span data-stu-id="34c5c-423">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="34c5c-424">本文件中所提實際公司和產品，可能為各所有人所有之商標。</span><span class="sxs-lookup"><span data-stu-id="34c5c-424">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
