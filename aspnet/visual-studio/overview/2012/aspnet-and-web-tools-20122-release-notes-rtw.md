---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET 和 Web 工具 2012.2 版本資訊 |Microsoft 文件
author: rick-anderson
description: 適用於 ASP.NET 和 Web 工具 2012.2 版本資訊。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: ab1642f1a3de298919aa9c6c1ddbd6bbb0cb99b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-and-web-tools-20122-release-notes"></a><span data-ttu-id="99605-103">ASP.NET 及 Web 工具 2012.2 版本資訊</span><span class="sxs-lookup"><span data-stu-id="99605-103">ASP.NET and Web Tools 2012.2 Release Notes</span></span>
====================
> <span data-ttu-id="99605-104">本文件說明 ASP.NET 和 Web 工具 2012.2 的版本。</span><span class="sxs-lookup"><span data-stu-id="99605-104">This document describes the release of ASP.NET and Web Tools 2012.2.</span></span> <span data-ttu-id="99605-105">它是 Visual Studio Web Tooling 和 ASP.NET 的更新。</span><span class="sxs-lookup"><span data-stu-id="99605-105">It is an update to Visual Studio Web Tooling and ASP.NET.</span></span>


- [<span data-ttu-id="99605-106">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="99605-106">Installation Notes</span></span>](#_Installation)
- [<span data-ttu-id="99605-107">文件 (英文)</span><span class="sxs-lookup"><span data-stu-id="99605-107">Documentation</span></span>](#_Documentation)
- [<span data-ttu-id="99605-108">支援</span><span class="sxs-lookup"><span data-stu-id="99605-108">Support</span></span>](#_Support)
- [<span data-ttu-id="99605-109">軟體需求</span><span class="sxs-lookup"><span data-stu-id="99605-109">Software Requirements</span></span>](#_Software_Requirements)
- [<span data-ttu-id="99605-110">在 ASP.NET 和 Web 工具 2012.2 中的新功能</span><span class="sxs-lookup"><span data-stu-id="99605-110">New Features in ASP.NET and Web Tools 2012.2</span></span>](#_New_Features_in)

    - [<span data-ttu-id="99605-111">工具</span><span class="sxs-lookup"><span data-stu-id="99605-111">Tooling</span></span>](#_Tooling)
    - [<span data-ttu-id="99605-112">Web 發行</span><span class="sxs-lookup"><span data-stu-id="99605-112">Web Publishing</span></span>](#_Web_Publishing)
    - [<span data-ttu-id="99605-113">ASP.NET MVC 範本</span><span class="sxs-lookup"><span data-stu-id="99605-113">ASP.NET MVC Templates</span></span>](#_Templates)
    - [<span data-ttu-id="99605-114">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="99605-114">ASP.NET Web API</span></span>](#_ASP.NET_Web_API)

    - [<span data-ttu-id="99605-115">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="99605-115">ASP.NET SignalR</span></span>](#_ASP.NET_SignalR)
    - [<span data-ttu-id="99605-116">ASP.NET 易記 Url</span><span class="sxs-lookup"><span data-stu-id="99605-116">ASP.NET Friendly URLs</span></span>](#_ASP.NET_Friendly_URLs)
- [<span data-ttu-id="99605-117">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="99605-117">Known Issues and Breaking Changes</span></span>](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a><span data-ttu-id="99605-118">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="99605-118">Installation Notes</span></span>

<span data-ttu-id="99605-119">ASP.NET 和 Web 工具 2012.2 for Visual Studio 2012 可以使用安裝[Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)。</span><span class="sxs-lookup"><span data-stu-id="99605-119">ASP.NET and Web Tools 2012.2 for Visual Studio 2012 can be installed using [Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).</span></span> <span data-ttu-id="99605-120">這是 Visual Studio 2012 或 Visual Studio Express 2012 for Web，這是必要的更新。</span><span class="sxs-lookup"><span data-stu-id="99605-120">This is an update to Visual Studio 2012 or Visual Studio Express 2012 for Web, which is required.</span></span> <span data-ttu-id="99605-121">如果您沒有安裝 Visual Studio，將會安裝 Visual Studio Express 2012 for Web。</span><span class="sxs-lookup"><span data-stu-id="99605-121">If you do not have Visual Studio installed, Visual Studio Express 2012 for Web will be installed.</span></span>

<span data-ttu-id="99605-122">您也可以安裝 ASP.NET 和 Web 工具 2012.2 手動。</span><span class="sxs-lookup"><span data-stu-id="99605-122">You can also install ASP.NET and Web Tools 2012.2 manually.</span></span> <span data-ttu-id="99605-123">您必須擁有 Visual Studio 2012 或 Visual Studio Express 2012 for Web 安裝。</span><span class="sxs-lookup"><span data-stu-id="99605-123">You must have Visual Studio 2012 or Visual Studio Express 2012 for Web installed.</span></span> <span data-ttu-id="99605-124">然後使用下列指示：</span><span class="sxs-lookup"><span data-stu-id="99605-124">Then use the following instructions:</span></span> 

1. <span data-ttu-id="99605-125">下載[ASP.NET 和 Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe)從下載中心安裝程式。</span><span class="sxs-lookup"><span data-stu-id="99605-125">Download [ASP.NET and Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) installer from Download Center.</span></span>
2. <span data-ttu-id="99605-126">當提示按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="99605-126">When prompted click Run.</span></span> <span data-ttu-id="99605-127">您也可以儲存檔案，以供日後執行。</span><span class="sxs-lookup"><span data-stu-id="99605-127">You can also save the file to run it later.</span></span>
3. <span data-ttu-id="99605-128">請確認您要更新的 Visual Studio 版本。</span><span class="sxs-lookup"><span data-stu-id="99605-128">Verify the version of Visual Studio you will update.</span></span> <span data-ttu-id="99605-129">您可以啟動您想要更新的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="99605-129">You can do this by launching the Visual Studio you wish to update.</span></span> <span data-ttu-id="99605-130">然後按一下 [說明] 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="99605-130">Then click the Help menu item.</span></span>   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. <span data-ttu-id="99605-131">如果您看到的功能表項目&quot;關於 Microsoft Visual Studio 2012 for Web&quot;然後下載[Web 開發人員工具 2012.2 Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228)。</span><span class="sxs-lookup"><span data-stu-id="99605-131">If you see the menu item &quot;About Microsoft Visual Studio 2012 for Web&quot; then download [Web Developer Tools 2012.2 - Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228).</span></span> <span data-ttu-id="99605-132">否則下載[Web 開發人員工具 2012.2 Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228)。</span><span class="sxs-lookup"><span data-stu-id="99605-132">Otherwise download [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).</span></span>
5. <span data-ttu-id="99605-133">當提示按一下 [執行]。</span><span class="sxs-lookup"><span data-stu-id="99605-133">When prompted click Run.</span></span> <span data-ttu-id="99605-134">您也可以儲存檔案，以供日後執行。</span><span class="sxs-lookup"><span data-stu-id="99605-134">You can also save the file to run it later.</span></span>

> [!NOTE]
> <span data-ttu-id="99605-135">ASP.NET 和 Web 工具 2012.2 版本不包含 SQL Server Data Tools。</span><span class="sxs-lookup"><span data-stu-id="99605-135">ASP.NET and Web Tools 2012.2 release does not include SQL Server Data Tools.</span></span> <span data-ttu-id="99605-136">SQL Server 及 Windows Azure SQL Database 提供豐富的工具包括離線專案為基礎的開發、 結構描述比較以及增強的資料庫部署功能的資料庫。</span><span class="sxs-lookup"><span data-stu-id="99605-136">SQL Server and Windows Azure SQL Databases provides a richer set of database tooling including offline project-backed development, schema comparison and enhanced database deployment capabilities.</span></span> <span data-ttu-id="99605-137">如需詳細資訊，或若要安裝 SQL Server Data Tools 造訪[ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127)。</span><span class="sxs-lookup"><span data-stu-id="99605-137">For more information or to install SQL Server Data Tools visit [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).</span></span>

<a id="_Documentation"></a>
## <a name="documentation"></a><span data-ttu-id="99605-138">文件</span><span class="sxs-lookup"><span data-stu-id="99605-138">Documentation</span></span>

<span data-ttu-id="99605-139">教學課程和其他資訊，ASP.NET 和 Web 工具 2012.2 都是從 ASP.NET 網站 ( https://www.asp.net)。</span><span class="sxs-lookup"><span data-stu-id="99605-139">Tutorials and other information about ASP.NET and Web Tools 2012.2 are available from ASP.NET web site ( https://www.asp.net).</span></span>

<a id="_Support"></a>
## <a name="support"></a><span data-ttu-id="99605-140">支援</span><span class="sxs-lookup"><span data-stu-id="99605-140">Support</span></span>

<span data-ttu-id="99605-141">ASP.NET 和 Web 工具 2012.2 是正式發行和支援。</span><span class="sxs-lookup"><span data-stu-id="99605-141">ASP.NET and Web Tools 2012.2 is officially released and supported.</span></span> <span data-ttu-id="99605-142">您可以使用一般支援通道。</span><span class="sxs-lookup"><span data-stu-id="99605-142">You can use your normal support channel.</span></span> <span data-ttu-id="99605-143">您也可以將問題張貼至 ASP.NET 論壇 ([https://forums.asp.net/](https://forums.asp.net/))，其中的 ASP.NET 社群成員可經常提供非正式的支援。</span><span class="sxs-lookup"><span data-stu-id="99605-143">You can also post questions to the ASP.NET forums ([https://forums.asp.net/](https://forums.asp.net/)), where members of the ASP.NET community are frequently able to provide informal support.</span></span>

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="99605-144">軟體需求</span><span class="sxs-lookup"><span data-stu-id="99605-144">Software Requirements</span></span>

<span data-ttu-id="99605-145">ASP.NET 和 Web 工具 2012.2 需要 Visual Studio 2012 或 Visual Studio Express 2012 for Web。</span><span class="sxs-lookup"><span data-stu-id="99605-145">The ASP.NET and Web Tools 2012.2 requires Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a><span data-ttu-id="99605-146">在 ASP.NET 和 Web 工具 2012.2 中的新功能</span><span class="sxs-lookup"><span data-stu-id="99605-146">New Features in ASP.NET and Web Tools 2012.2</span></span>

<span data-ttu-id="99605-147">本節說明已在 ASP.NET 和 Web 工具 2012.2 版本中引進的功能。</span><span class="sxs-lookup"><span data-stu-id="99605-147">This section describes features that have been introduced in the ASP.NET and Web Tools 2012.2 release.</span></span>

<a id="_Tooling"></a>
### <a name="tooling"></a><span data-ttu-id="99605-148">Tooling</span><span class="sxs-lookup"><span data-stu-id="99605-148">Tooling</span></span>

- <span data-ttu-id="99605-149">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="99605-149">Page Inspector</span></span> 

    - <span data-ttu-id="99605-150">支援 JavaScript 允許將頁面回傳至對應的 JavaScript 程式碼以動態方式加入的項目對應至 Page Inspector 的選取範圍對應。</span><span class="sxs-lookup"><span data-stu-id="99605-150">Support JavaScript selection mapping allowing Page Inspector to map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span>
    - <span data-ttu-id="99605-151">若要查看即時 CSS 更新功能。</span><span class="sxs-lookup"><span data-stu-id="99605-151">The ability to see CSS updates in real-time.</span></span>
    - <span data-ttu-id="99605-152">如需詳細資訊，請參閱[CSS 自動同步處理和 Page Inspector 中的 JavaScript 選取對應](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx)。</span><span class="sxs-lookup"><span data-stu-id="99605-152">For more information, read [CSS Auto-Sync and JavaScript Selection Mapping in Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).</span></span>
- <span data-ttu-id="99605-153">編輯器</span><span class="sxs-lookup"><span data-stu-id="99605-153">Editor</span></span> 

    - <span data-ttu-id="99605-154">支援的 CoffeeScript、 Mustache、 Handlebars 和 JsRender 反白顯示語法。</span><span class="sxs-lookup"><span data-stu-id="99605-154">Support syntax highlighting for CoffeeScript, Mustache, Handlebars, and JsRender.</span></span>
    - <span data-ttu-id="99605-155">HTML 編輯器中 Intellisense 提供 Knockout 繫結。</span><span class="sxs-lookup"><span data-stu-id="99605-155">The HTML editor provides Intellisense for Knockout bindings.</span></span>
    - <span data-ttu-id="99605-156">較少的編輯和編譯器若要啟用建置使用小於動態 CSS 支援。</span><span class="sxs-lookup"><span data-stu-id="99605-156">LESS editing and compiler support to enable building dynamic CSS using LESS.</span></span>
    - <span data-ttu-id="99605-157">貼上 JSON 做為.NET 類別。</span><span class="sxs-lookup"><span data-stu-id="99605-157">Paste JSON as a .NET class.</span></span> <span data-ttu-id="99605-158">使用這個特殊貼上命令的 C# 或 VB.NET 程式碼檔案和 Visual Studio 中貼上 JSON，會自動產生.NET 類別從 JSON 推斷。</span><span class="sxs-lookup"><span data-stu-id="99605-158">Using this Special Paste command to paste JSON into a C# or VB.NET code file, and Visual Studio will automatically generate .NET classes inferred from the JSON.</span></span>
- <span data-ttu-id="99605-159">行動裝置版模擬器支援將擴充性攔截程序，讓第三方模擬器可以安裝為 VSIX。</span><span class="sxs-lookup"><span data-stu-id="99605-159">Mobile Emulator support adds extensibility hooks so that third-party emulators can be installed as a VSIX.</span></span> <span data-ttu-id="99605-160">安裝的模擬器會顯示 [F5] 下拉式清單中，讓開發人員可以預覽他們的行動裝置上的網站。</span><span class="sxs-lookup"><span data-stu-id="99605-160">The installed emulators will show up in the F5 dropdown, so that developers can preview their websites on a variety of mobile devices.</span></span> <span data-ttu-id="99605-161">深入了解這項功能在 Scott Hanselman 上的部落格項目[與 Visual Studio 的新 BrowserStack 整合](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx)。</span><span class="sxs-lookup"><span data-stu-id="99605-161">Read more about this feature in Scott Hanselman's blog entry on [the new BrowserStack integration with Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).</span></span>

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a><span data-ttu-id="99605-162">Web 發行</span><span class="sxs-lookup"><span data-stu-id="99605-162">Web Publishing</span></span>

- <span data-ttu-id="99605-163">網站專案現在有包括發行至 Windows Azure 網站的 Web 應用程式專案與相同的發行體驗。</span><span class="sxs-lookup"><span data-stu-id="99605-163">Web site projects now have the same publishing experience as Web Application projects including publishing to Windows Azure Web Sites.</span></span>
- <span data-ttu-id="99605-164">選擇性發行&#8211;一或多個檔案可以執行下列動作 （之後發行至 Web Deploy 的端點）：</span><span class="sxs-lookup"><span data-stu-id="99605-164">Selective publish &#8211; for one or more files you can perform the following actions (after publishing to a Web Deploy endpoint):</span></span> 

    - <span data-ttu-id="99605-165">發佈選取的檔案。</span><span class="sxs-lookup"><span data-stu-id="99605-165">Publish selected files.</span></span>
    - <span data-ttu-id="99605-166">請參閱本機檔案與遠端的檔案之間的差異。</span><span class="sxs-lookup"><span data-stu-id="99605-166">See the difference between a local file and a remote file.</span></span>
    - <span data-ttu-id="99605-167">更新本機檔案與遠端的檔案或本機檔案以更新遠端檔案。</span><span class="sxs-lookup"><span data-stu-id="99605-167">Update the local file with the remote file or update the remote file with the local file.</span></span>

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a><span data-ttu-id="99605-168">ASP.NET MVC 範本</span><span class="sxs-lookup"><span data-stu-id="99605-168">ASP.NET MVC Templates</span></span>

- <span data-ttu-id="99605-169">新的 Facebook 應用程式範本可讓撰寫 Facebook Canvas 應用程式輕鬆。</span><span class="sxs-lookup"><span data-stu-id="99605-169">The new Facebook Application template makes writing Facebook Canvas applications easy.</span></span> <span data-ttu-id="99605-170">在幾個簡單步驟中，您可以建立 Facebook 應用程式，從已登入的使用者取得資料並與朋友一起整合。</span><span class="sxs-lookup"><span data-stu-id="99605-170">In a few simple steps, you can create a Facebook application that gets data from a logged in user and integrates with their friends.</span></span> <span data-ttu-id="99605-171">範本包含新的程式庫來建置 Facebook 應用程式，包括驗證、 權限、 存取 Facebook 資料等等涉及的所有配管負責。</span><span class="sxs-lookup"><span data-stu-id="99605-171">The template includes a new library to take care of all the plumbing involved in building a Facebook app, including authentication, permissions, accessing Facebook data and more.</span></span> <span data-ttu-id="99605-172">如需使用 Facebook 應用程式範本的詳細資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921)。</span><span class="sxs-lookup"><span data-stu-id="99605-172">For more information on using the Facebook Application template see [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).</span></span>
- <span data-ttu-id="99605-173">新的單一頁面應用程式 MVC 範本可讓開發人員建置使用 HTML 5、 CSS 3 和熱門的 Knockout 與 jQuery JavaScript 程式庫，在 ASP.NET Web API 的互動的用戶端 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="99605-173">A new Single Page Application MVC template allows developers to build interactive client-side web apps using HTML 5, CSS 3, and the popular Knockout and jQuery JavaScript libraries, on top of ASP.NET Web API.</span></span> <span data-ttu-id="99605-174">範本包含"todo"清單應用程式，示範如何建置使用 RESTful 伺服器 API 的 JavaScript HTML5 應用程式的常見作法。</span><span class="sxs-lookup"><span data-stu-id="99605-174">The template includes a "todo" list application that demonstrates common practices for building a JavaScript HTML5 application that uses a RESTful server API.</span></span> <span data-ttu-id="99605-175">您可以閱讀更多在[ https://www.asp.net/single-page-application ](../../../single-page-application/index.md)。</span><span class="sxs-lookup"><span data-stu-id="99605-175">You can read more at [https://www.asp.net/single-page-application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="99605-176">您現在可以建立 VSIX，將新的樣板加入至 ASP.NET MVC 新專案] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="99605-176">You can now create a VSIX that adds new templates to the ASP.NET MVC New Project dialog.</span></span> <span data-ttu-id="99605-177">這裡了解詳情： [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)</span><span class="sxs-lookup"><span data-stu-id="99605-177">Learn how here: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)</span></span>
- <span data-ttu-id="99605-178">FixedDisplayModes 封裝&#8211;MVC 專案範本已更新為包含新的 'FixedDisplayModes' NuGet 套件，其中包含因應措施的 MVC 4 中的 bug。</span><span class="sxs-lookup"><span data-stu-id="99605-178">FixedDisplayModes package &#8211; MVC project templates have been updated to include the new ‘FixedDisplayModes' NuGet package, which contains a workaround for a bug in MVC 4.</span></span> <span data-ttu-id="99605-179">如需有關封裝中包含的修正程式的詳細資訊，請參閱此部落格文章 ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) 從 MVC 小組。</span><span class="sxs-lookup"><span data-stu-id="99605-179">For more information on the fix contained in the package, refer to this blog post ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) from the MVC team.</span></span>

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a><span data-ttu-id="99605-180">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="99605-180">ASP.NET Web API</span></span>

<span data-ttu-id="99605-181">ASP.NET Web 應用程式開發介面已透過數種新功能增強：</span><span class="sxs-lookup"><span data-stu-id="99605-181">ASP.NET Web API has been enhanced with several new features:</span></span>

- <span data-ttu-id="99605-182">ASP.NET Web API OData</span><span class="sxs-lookup"><span data-stu-id="99605-182">ASP.NET Web API OData</span></span>
- <span data-ttu-id="99605-183">ASP.NET Web API 追蹤</span><span class="sxs-lookup"><span data-stu-id="99605-183">ASP.NET Web API Tracing</span></span>
- <span data-ttu-id="99605-184">ASP.NET Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="99605-184">ASP.NET Web API Help Page</span></span>

#### <a name="aspnet-web-api-odata"></a><span data-ttu-id="99605-185">ASP.NET Web API OData</span><span class="sxs-lookup"><span data-stu-id="99605-185">ASP.NET Web API OData</span></span>

<span data-ttu-id="99605-186">ASP.NET Web API OData 提供您所需要的任何資料來源上建立具有豐富的商務邏輯的 OData 端點的彈性。</span><span class="sxs-lookup"><span data-stu-id="99605-186">ASP.NET Web API OData gives you the flexibility you need to build OData endpoints with rich business logic over any data source.</span></span> <span data-ttu-id="99605-187">您可以使用 ASP.NET Web API OData 控制您想要公開的 OData 語意的數量。</span><span class="sxs-lookup"><span data-stu-id="99605-187">With ASP.NET Web API OData you control the amount of OData semantics that you want to expose.</span></span> <span data-ttu-id="99605-188">ASP.NET Web API OData 隨附的 ASP.NET MVC 4 專案範本，並且也會提供 NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata))。</span><span class="sxs-lookup"><span data-stu-id="99605-188">ASP.NET Web API OData is included with the ASP.NET MVC 4 project templates and is also available from NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).</span></span>

<span data-ttu-id="99605-189">ASP.NET Web API OData 目前支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="99605-189">ASP.NET Web API OData currently supports the following features:</span></span>

- <span data-ttu-id="99605-190">藉由套用 [Queryable] 屬性中啟用 OData 查詢語意。</span><span class="sxs-lookup"><span data-stu-id="99605-190">Enable OData query semantics by applying the [Queryable] attribute.</span></span>
- <span data-ttu-id="99605-191">輕鬆地驗證 OData 查詢，並限制一組支援的查詢選項、 運算子和函式。</span><span class="sxs-lookup"><span data-stu-id="99605-191">Easily validate OData queries and restrict the set of supported query options, operators and functions.</span></span>
- <span data-ttu-id="99605-192">參數繫結 ODataQueryOptions 直接可取得抽象語法樹狀結構表示，再進行驗證並套用到 IQueryable 或 IEnumerable 的查詢。</span><span class="sxs-lookup"><span data-stu-id="99605-192">Parameter bind to ODataQueryOptions directly to get an abstract syntax tree representation of the query that can then be validated and applied to an IQueryable or IEnumerable.</span></span>
- <span data-ttu-id="99605-193">啟用服務導向的分頁和產生下一個頁面連結由指定 [Queryable] 屬性的結果限制。</span><span class="sxs-lookup"><span data-stu-id="99605-193">Enable service-driven paging and next page link generation by specifying result limits on [Queryable] attribute.</span></span>
- <span data-ttu-id="99605-194">要求使用 $inlinecount 相符的資源總數的內嵌的計數。</span><span class="sxs-lookup"><span data-stu-id="99605-194">Request an inlined count of the total number of matching resources using $inlinecount.</span></span>
- <span data-ttu-id="99605-195">控制 null 傳播。</span><span class="sxs-lookup"><span data-stu-id="99605-195">Control null propagation.</span></span>
- <span data-ttu-id="99605-196">$Filter 中的任何/所有運算子。</span><span class="sxs-lookup"><span data-stu-id="99605-196">Any/All operators in $filter.</span></span>
- <span data-ttu-id="99605-197">依照慣例推斷實體資料模型，或明確地自訂模型以 Entity Framework 程式碼優先類似的方式。</span><span class="sxs-lookup"><span data-stu-id="99605-197">Infer an entity data model by convention or explicitly customize a model in a manner similar to Entity Framework Code-First.</span></span>
- <span data-ttu-id="99605-198">由衍生自 EntitySetController，公開實體集。</span><span class="sxs-lookup"><span data-stu-id="99605-198">Expose entity sets by deriving from EntitySetController.</span></span>
- <span data-ttu-id="99605-199">簡單、 可自訂的公開的導覽屬性、 操作連結和慣例實作 OData 動作。</span><span class="sxs-lookup"><span data-stu-id="99605-199">Simple, customizable conventions for exposing navigation properties, manipulating links and implementing OData actions.</span></span>
- <span data-ttu-id="99605-200">簡化路由 MapODataRoute 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="99605-200">Simplified routing using the MapODataRoute extension method.</span></span>
- <span data-ttu-id="99605-201">藉由公開多個的 EDM 模型的版本控制支援。</span><span class="sxs-lookup"><span data-stu-id="99605-201">Support for versioning by exposing multiple EDM models.</span></span>
- <span data-ttu-id="99605-202">公開 （expose) 服務文件和 $metadata 讓您可以產生用戶端 （.NET、 Windows Phone、 Windows 市集等） 的 Web API。</span><span class="sxs-lookup"><span data-stu-id="99605-202">Expose service document and $metadata so you can generate clients (.NET, Windows Phone, Windows Store, etc.) for your Web API.</span></span>
- <span data-ttu-id="99605-203">格式的支援 OData Atom、 JSON 和 JSON 詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="99605-203">Support for the OData Atom, JSON, and JSON verbose formats.</span></span>
- <span data-ttu-id="99605-204">建立、 更新、 部分更新 (PATCH) 和刪除實體。</span><span class="sxs-lookup"><span data-stu-id="99605-204">Create, update, partially update (PATCH) and delete entities.</span></span>
- <span data-ttu-id="99605-205">查詢和操作實體之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="99605-205">Query and manipulate relationships between entities.</span></span>
- <span data-ttu-id="99605-206">建立您的路由到網路的關聯性連結。</span><span class="sxs-lookup"><span data-stu-id="99605-206">Create relationship links that wire up to your routes.</span></span>
- <span data-ttu-id="99605-207">複雜類型。</span><span class="sxs-lookup"><span data-stu-id="99605-207">Complex types.</span></span>
- <span data-ttu-id="99605-208">實體類型的繼承。</span><span class="sxs-lookup"><span data-stu-id="99605-208">Entity Type Inheritance.</span></span>
- <span data-ttu-id="99605-209">集合屬性。</span><span class="sxs-lookup"><span data-stu-id="99605-209">Collection properties.</span></span>
- <span data-ttu-id="99605-210">列舉。</span><span class="sxs-lookup"><span data-stu-id="99605-210">Enums.</span></span>
- <span data-ttu-id="99605-211">OData 動作。</span><span class="sxs-lookup"><span data-stu-id="99605-211">OData actions.</span></span>
- <span data-ttu-id="99605-212">建置在相同的基礎為 WCF 資料服務，也就是 ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata))。</span><span class="sxs-lookup"><span data-stu-id="99605-212">Built upon the same foundation as WCF Data Services, namely ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).</span></span>

<span data-ttu-id="99605-213">如需 ASP.NET Web API OData 的詳細資訊請參閱[ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141)。</span><span class="sxs-lookup"><span data-stu-id="99605-213">For more information on ASP.NET Web API OData see [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).</span></span>

#### <a name="aspnet-web-api-tracing"></a><span data-ttu-id="99605-214">ASP.NET Web API 追蹤</span><span class="sxs-lookup"><span data-stu-id="99605-214">ASP.NET Web API Tracing</span></span>

<span data-ttu-id="99605-215">ASP.NET Web API 追蹤與.NET 追蹤整合，從您的 web 應用程式開發介面的追蹤資料。</span><span class="sxs-lookup"><span data-stu-id="99605-215">ASP.NET Web API Tracing integrates tracing data from your web APIs with .NET Tracing.</span></span> <span data-ttu-id="99605-216">它現在預設會啟用 Web API 專案範本中。</span><span class="sxs-lookup"><span data-stu-id="99605-216">It is now enabled by default in the Web API project template.</span></span> <span data-ttu-id="99605-217">追蹤資料，為您的 web 應用程式開發介面會傳送至 [輸出] 視窗，並可透過 IntelliTrace。</span><span class="sxs-lookup"><span data-stu-id="99605-217">Tracing data for your web APIs is sent to the Output window and is made available through IntelliTrace.</span></span> <span data-ttu-id="99605-218">ASP.NET Web API 追蹤功能可讓您將您的 Web API 時透過整合 Windows Azure 上裝載的追蹤資訊[Windows Azure 診斷](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx)。</span><span class="sxs-lookup"><span data-stu-id="99605-218">ASP.NET Web API Tracing enables you to trace information about your Web API when hosted on Windows Azure through integration with [Windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx).</span></span> <span data-ttu-id="99605-219">您也可以安裝並使用 ASP.NET Web API 追蹤的 NuGet 套件的任何應用程式中啟用 [ASP.NET Web API Tracing ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing))。</span><span class="sxs-lookup"><span data-stu-id="99605-219">You can also install and enable ASP.NET Web API Tracing in any application using the ASP.NET Web API Tracing NuGet package ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).</span></span>

<span data-ttu-id="99605-220">如需有關設定和使用 ASP.NET Web API Tracing [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874)。</span><span class="sxs-lookup"><span data-stu-id="99605-220">For more information on configuring and using ASP.NET Web API Tracing see [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).</span></span>

#### <a name="aspnet-web-api-help-page"></a><span data-ttu-id="99605-221">ASP.NET Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="99605-221">ASP.NET Web API Help Page</span></span>

<span data-ttu-id="99605-222">預設的 Web API 專案範本現在包含 ASP.NET Web API 說明頁面。</span><span class="sxs-lookup"><span data-stu-id="99605-222">The ASP.NET Web API Help Page is now included by default in the Web API project template.</span></span> <span data-ttu-id="99605-223">ASP.NET Web API 說明頁面會自動產生 web 應用程式開發介面，包括 HTTP 端點、 支援的 HTTP 方法、 參數和範例要求和回應訊息裝載的文的件。</span><span class="sxs-lookup"><span data-stu-id="99605-223">The ASP.NET Web API Help Page automatically generates documentation for web APIs including the HTTP endpoints, the supported HTTP methods, parameters and example request and response message payloads.</span></span> <span data-ttu-id="99605-224">文件自動取自您的程式碼中的註解。</span><span class="sxs-lookup"><span data-stu-id="99605-224">Documentation is automatically pulled from comments in your code.</span></span> <span data-ttu-id="99605-225">您也可以在任何應用程式使用 ASP.NET Web API 說明頁面 NuGet 封裝加入 ASP.NET Web API 說明頁面 ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage))。</span><span class="sxs-lookup"><span data-stu-id="99605-225">You can also add the ASP.NET Web API Help Page to any application using the ASP.NET Web API Help Page NuGet package ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).</span></span>

<span data-ttu-id="99605-226">如需有關設定和自訂 ASP.NET Web API 說明頁面，請參閱[ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140)。</span><span class="sxs-lookup"><span data-stu-id="99605-226">For more information on setting up and customizing the ASP.NET Web API Help Page see [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).</span></span>

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a><span data-ttu-id="99605-227">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="99605-227">ASP.NET SignalR</span></span>

<span data-ttu-id="99605-228">ASP.NET SignalR 」 可簡化將即時 web 功能加入至您的 ASP.NET 應用程式，使用 WebSockets，如果有的話，會自動回到其他技術時不是。</span><span class="sxs-lookup"><span data-stu-id="99605-228">ASP.NET SignalR makes it simple to add real-time web capabilities to your ASP.NET application, using WebSockets if available and automatically falling back to other techniques when it isn't.</span></span>

<span data-ttu-id="99605-229">如需使用 ASP.NET SignalR 的詳細資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271)。</span><span class="sxs-lookup"><span data-stu-id="99605-229">For more information on using ASP.NET SignalR see [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).</span></span>

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a><span data-ttu-id="99605-230">ASP.NET 易記 Url</span><span class="sxs-lookup"><span data-stu-id="99605-230">ASP.NET Friendly URLs</span></span>

<span data-ttu-id="99605-231">ASP.NET FriendlyURLs 非常容易 web form 開發人員產生清潔器尋找 Url （不含副檔名的.aspx）。</span><span class="sxs-lookup"><span data-stu-id="99605-231">ASP.NET FriendlyURLs makes it very easy for web forms developers to generate cleaner looking URLs(without the .aspx extension).</span></span> <span data-ttu-id="99605-232">它需要不到組態設定少，並且可以搭配現有的 ASP.NET v4.0 應用程式。</span><span class="sxs-lookup"><span data-stu-id="99605-232">It requires little to no configuration and can be used with existing ASP.NET v4.0 applications.</span></span> <span data-ttu-id="99605-233">FriendlyURLs 功能也可支援桌上型電腦和行動裝置版的檢視之間切換，將行動裝置的支援新增到應用程式中，開發人員更容易。</span><span class="sxs-lookup"><span data-stu-id="99605-233">The FriendlyURLs feature also makes it easier for developers to add mobile support to their applications, by supporting switching between desktop and mobile views.</span></span>

<span data-ttu-id="99605-234">如需安裝及使用 ASP.NET 易記 Url 的詳細資訊，請參閱[ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)。</span><span class="sxs-lookup"><span data-stu-id="99605-234">For more information on installing and using ASP.NET Friendly URLs see [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).</span></span>

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="99605-235">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="99605-235">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="99605-236">本章節描述的已知的問題和 ASP.NET 和 Web 工具 2012.2 版本中的重大變更。</span><span class="sxs-lookup"><span data-stu-id="99605-236">This section describes known issues and breaking changes that are in the ASP.NET and Web Tools 2012.2 release.</span></span>

### <a name="installation-issues"></a><span data-ttu-id="99605-237">安裝問題</span><span class="sxs-lookup"><span data-stu-id="99605-237">Installation Issues</span></span>

#### <a name="out-of-order-installs-of-visual-studio-2012"></a><span data-ttu-id="99605-238">順序會安裝 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="99605-238">Out of order installs of Visual Studio 2012</span></span>

<span data-ttu-id="99605-239">安裝 ASP.NET 和 Web 工具 2012.2 會需要修復作業後，請安裝其他 SKU 的 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="99605-239">Installing an additional SKU of Visual Studio 2012 after installing the ASP.NET and Web Tools 2012.2 will require a repair operation.</span></span> <span data-ttu-id="99605-240">請考慮下列順序：</span><span class="sxs-lookup"><span data-stu-id="99605-240">Consider the following sequence:</span></span>

1. <span data-ttu-id="99605-241">安裝 Visual Studio 2012 Express for Web</span><span class="sxs-lookup"><span data-stu-id="99605-241">Install Visual Studio 2012 Express for Web</span></span>
2. <span data-ttu-id="99605-242">安裝 ASP.NET 及 Web 工具 2012.2</span><span class="sxs-lookup"><span data-stu-id="99605-242">Install ASP.NET and Web Tools 2012.2</span></span>
3. <span data-ttu-id="99605-243">安裝 Visual Studio 2012 Professional、 Premium 或 Ultimate</span><span class="sxs-lookup"><span data-stu-id="99605-243">Install Visual Studio 2012 Professional, Premium or Ultimate</span></span>

<span data-ttu-id="99605-244">步驟 2 只會導致安裝更新的 Express for Web。</span><span class="sxs-lookup"><span data-stu-id="99605-244">Step 2 would only result in installing updates for Express for Web.</span></span> <span data-ttu-id="99605-245">若要確保安裝在步驟 3 的其他 SKU 包含更新，您必須修復安裝的更新安裝的最後一個 sku 的 ASP.NET 和 Web 工具 2012.2。</span><span class="sxs-lookup"><span data-stu-id="99605-245">To ensure that the additional SKU installed during step 3 contains the update you will need to repair the ASP.NET and Web Tools 2012.2 to install the updates for the last SKU installed.</span></span> <span data-ttu-id="99605-246">這也適用於在步驟 1 中的 Sku 和 3 會反轉。</span><span class="sxs-lookup"><span data-stu-id="99605-246">This also applies if the SKUs in Step 1 and 3 are reversed.</span></span>

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a><span data-ttu-id="99605-247">開啟 Visual Studio 時安裝 Microsoft ASP.NET 及 Web 工具 2012.2</span><span class="sxs-lookup"><span data-stu-id="99605-247">Installing Microsoft ASP.NET and Web Tools 2012.2 when Visual Studio is open</span></span>

<span data-ttu-id="99605-248">如果在安裝 Microsoft ASP.NET 及 Web 工具 2012.2 VS 開啟，Visual Studio 可能最後會在不正常的狀態。</span><span class="sxs-lookup"><span data-stu-id="99605-248">If VS is open during installation of Microsoft ASP.NET and Web Tools 2012.2, Visual Studio might end up in a bad state.</span></span> <span data-ttu-id="99605-249">建議使用者關閉之前繼續進行安裝的 Visual Studio 的所有執行個體。</span><span class="sxs-lookup"><span data-stu-id="99605-249">It is recommended that users close all instances of Visual Studio before proceeding with install.</span></span>

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a><span data-ttu-id="99605-250">安裝期間取消的 ASP.NET 和 Web 工具 2012.2 安裝程式</span><span class="sxs-lookup"><span data-stu-id="99605-250">Canceling ASP.NET and Web Tools 2012.2 setup in the middle of installation</span></span>

<span data-ttu-id="99605-251">ASP.NET 和 Web 工具 2012.2 取消安裝程式安裝期間會將保留在 Visual Studio 不正常的狀態。</span><span class="sxs-lookup"><span data-stu-id="99605-251">Canceling ASP.NET and Web Tools 2012.2 setup in the middle of installation will leave Visual Studio in a bad state.</span></span> <span data-ttu-id="99605-252">若要解決此問題的後續步驟執行：</span><span class="sxs-lookup"><span data-stu-id="99605-252">To address this problem follow these steps:</span></span> 

- <span data-ttu-id="99605-253">移至 [新增移除程式</span><span class="sxs-lookup"><span data-stu-id="99605-253">Go to Add Remove Programs</span></span>
- <span data-ttu-id="99605-254">解除安裝 Microsoft ASP.NET 及 Web 工具 2012.2，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="99605-254">Uninstall Microsoft ASP.NET and Web Tools 2012.2, if present.</span></span>
- <span data-ttu-id="99605-255">重新安裝 Microsoft ASP.NET 及 Web 工具 2012.2</span><span class="sxs-lookup"><span data-stu-id="99605-255">Reinstall Microsoft ASP.NET and Web Tools 2012.2</span></span>

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a><span data-ttu-id="99605-256">解除安裝 ASP.NET 和 Web 工具 2012.2 ASP.NET MVC 4 之後範本和 Razor v2 網站範本已遺失</span><span class="sxs-lookup"><span data-stu-id="99605-256">After uninstalling ASP.NET and Web Tools 2012.2 the ASP.NET MVC 4 templates and Razor v2 Web Site templates are missing</span></span>

<span data-ttu-id="99605-257">解除安裝 ASP.NET 和 Web 工具 2012.2 也會解除安裝 ASP.NET MVC 4 和 Razor v2 網站範本的所有從 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="99605-257">Uninstalling ASP.NET and Web Tools 2012.2 will also uninstall all of ASP.NET MVC 4 and Razor v2 Web Site templates from Visual Studio 2012.</span></span>

<span data-ttu-id="99605-258">因應措施是修復您的 Visual Studio 2012 安裝，以重新安裝 ASP.NET MVC 4 和 Razor v2 網站範本。</span><span class="sxs-lookup"><span data-stu-id="99605-258">The workaround is to repair your Visual Studio 2012 installation to reinstall ASP.NET MVC 4 and Razor v2 Web Site templates.</span></span>

### <a name="tooling-issues"></a><span data-ttu-id="99605-259">工具問題</span><span class="sxs-lookup"><span data-stu-id="99605-259">Tooling Issues</span></span>

#### <a name="nuget-error-reported-during-project-creation"></a><span data-ttu-id="99605-260">報告在專案建立期間的 NuGet 錯誤</span><span class="sxs-lookup"><span data-stu-id="99605-260">NuGet error reported during project creation</span></span>

<span data-ttu-id="99605-261">在安裝 ASP.NET 和 Web 工具 2012.2 後您可能會看到下列錯誤時建立的 MVC 4 專案</span><span class="sxs-lookup"><span data-stu-id="99605-261">After installing ASP.NET and Web Tools 2012.2 you may see the following error when creating an MVC 4 project</span></span>

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

<span data-ttu-id="99605-262">ASP.NET 和 Web 工具 2012.2 隨附 NuGet 2.1，將會升級 Visual Studio 2012 中的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="99605-262">The ASP.NET and Web Tools 2012.2 ships NuGet 2.1 and will upgrade the extension in Visual Studio 2012.</span></span> <span data-ttu-id="99605-263">在某些情況下，VSIX 安裝程式將無法正確更新 VSIX。</span><span class="sxs-lookup"><span data-stu-id="99605-263">In some cases, the VSIX installer will fail to correctly update the VSIX.</span></span> <span data-ttu-id="99605-264">下列步驟可讓您解決這個問題：</span><span class="sxs-lookup"><span data-stu-id="99605-264">The following steps will allow you to address this problem:</span></span>

1. <span data-ttu-id="99605-265">系統管理員身分啟動 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="99605-265">Start Visual Studio 2012 as an Administrator</span></span>
2. <span data-ttu-id="99605-266">移至 Tools-&gt;擴充功能和更新及解除安裝 NuGet。</span><span class="sxs-lookup"><span data-stu-id="99605-266">Go to Tools-&gt;Extensions and Updates and uninstall NuGet.</span></span>
3. <span data-ttu-id="99605-267">關閉 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99605-267">Close Visual Studio</span></span>
4. <span data-ttu-id="99605-268">瀏覽至 ASP.NET 和 Web 工具 2012.2 安裝資料夾：</span><span class="sxs-lookup"><span data-stu-id="99605-268">Navigate to the ASP.NET and Web Tools 2012.2 installation folder:</span></span>

    1. <span data-ttu-id="99605-269">適用於 Visual Studio 2012:**程式 Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**</span><span class="sxs-lookup"><span data-stu-id="99605-269">For Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**</span></span>
    2. <span data-ttu-id="99605-270">針對 Visual Studio 2012 Express for Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**</span><span class="sxs-lookup"><span data-stu-id="99605-270">For Visual Studio 2012 Express for Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**</span></span>
5. <span data-ttu-id="99605-271">若要重新安裝 NuGet NuGet.Tools.vsix 按兩下</span><span class="sxs-lookup"><span data-stu-id="99605-271">Double click on the NuGet.Tools.vsix to reinstall NuGet</span></span>

### <a name="web-api-issues"></a><span data-ttu-id="99605-272">Web 應用程式開發介面問題</span><span class="sxs-lookup"><span data-stu-id="99605-272">Web API Issues</span></span>

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a><span data-ttu-id="99605-273">剖析 $filter 和日期時間常值中的問題</span><span class="sxs-lookup"><span data-stu-id="99605-273">Parsing issues in $filter and DateTime literals</span></span>

<span data-ttu-id="99605-274">OData URI 剖析器無法正確地剖析部分日期時間常值。</span><span class="sxs-lookup"><span data-stu-id="99605-274">The OData URI parser fails to parse partial datetime literals properly.</span></span> <span data-ttu-id="99605-275">例如，$filter = 開始 eq datetime' 2012年-12-31T12:00' 無法正確剖析。</span><span class="sxs-lookup"><span data-stu-id="99605-275">For example, $filter=start eq datetime'2012-12-31T12:00' fails to parse properly.</span></span> <span data-ttu-id="99605-276">因應措施是使用完整常值，$filter = 開始 eq datetime' 2012年-12-31T12:00:00'。</span><span class="sxs-lookup"><span data-stu-id="99605-276">A workaround is to use the full literal, $filter=start eq datetime'2012-12-31T12:00:00'.</span></span>

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a><span data-ttu-id="99605-277">OData 不支援不區分大小寫的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="99605-277">OData doesn't support case-insensitive property names.</span></span>

<span data-ttu-id="99605-278">OData 不支援不區分大小寫的屬性名稱中的 OData 查詢和 odata 路徑。</span><span class="sxs-lookup"><span data-stu-id="99605-278">OData doesn't support case-insensitive property names in OData queries and odata path.</span></span> <span data-ttu-id="99605-279">請參閱工作項目：</span><span class="sxs-lookup"><span data-stu-id="99605-279">See workitems:</span></span>

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

<span data-ttu-id="99605-280">如果使用者有不同的大小寫，javascript 用戶端和伺服器端上，它們可能會發生這個問題。</span><span class="sxs-lookup"><span data-stu-id="99605-280">If users have different casing on javascript client side and server side, they probably will encounter this issue.</span></span> <span data-ttu-id="99605-281">此問題是依據 odata 通訊協定中的設計。</span><span class="sxs-lookup"><span data-stu-id="99605-281">This issue is by design in odata protocol.</span></span> <span data-ttu-id="99605-282">不過，許多使用者報告此問題。</span><span class="sxs-lookup"><span data-stu-id="99605-282">However, many users reports this issue.</span></span> <span data-ttu-id="99605-283">若要暫時解決它，使用者必須更正它們的大小寫，在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="99605-283">To work around it, users have to correct their cases in URL.</span></span>

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a><span data-ttu-id="99605-284">預設 OData 路由慣例不支援導覽屬性上的 POST/PUT。</span><span class="sxs-lookup"><span data-stu-id="99605-284">Default OData routing conventions doesn't support POST/PUT on navigation property.</span></span>

<span data-ttu-id="99605-285">預設 OData 路由慣例不支援導覽屬性上的 POST/PUT。</span><span class="sxs-lookup"><span data-stu-id="99605-285">Default OData routing conventions doesn't support POST/PUT on navigation property.</span></span> <span data-ttu-id="99605-286">請參閱工作項目[ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366)。</span><span class="sxs-lookup"><span data-stu-id="99605-286">See workitem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366).</span></span> <span data-ttu-id="99605-287">我們會遺漏這個常用的慣例，在預設慣例。</span><span class="sxs-lookup"><span data-stu-id="99605-287">We are missing this commonly used convention in default conventions.</span></span>

<span data-ttu-id="99605-288">若要暫時解決它，使用者需要擴充來支援它的新路由慣例。</span><span class="sxs-lookup"><span data-stu-id="99605-288">To work around it, users need to extend new routing convention to support it.</span></span>

### <a name="facebook-template-issues"></a><span data-ttu-id="99605-289">Facebook 範本問題</span><span class="sxs-lookup"><span data-stu-id="99605-289">Facebook Template Issues</span></span>

#### <a name="facebook-application-template-only-works-using-net-45"></a><span data-ttu-id="99605-290">Facebook 應用程式範本僅適用於使用.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="99605-290">Facebook Application template only works using .NET 4.5</span></span>

<span data-ttu-id="99605-291">您必須選取 [在.NET 4.5 framework 下拉式清單中看到的 Facebook 應用程式範本，在 ASP.NET MVC 4 中的 [新增專案] 對話方塊中。</span><span class="sxs-lookup"><span data-stu-id="99605-291">You must select .NET 4.5 in the framework dropdown list in the New Project dialog to see the Facebook Application template in ASP.NET MVC 4.</span></span>

#### <a name="real-time-update-controller"></a><span data-ttu-id="99605-292">即時更新控制站</span><span class="sxs-lookup"><span data-stu-id="99605-292">Real-time Update Controller</span></span>

<span data-ttu-id="99605-293">Facebook 應用程式範本可讓使用者輕鬆地建立此 Web API 控制器會處理來自 Facebook 即時更新。</span><span class="sxs-lookup"><span data-stu-id="99605-293">The Facebook Application template allows user easily create a Web API Controller to handle real-time updates from Facebook.</span></span> <span data-ttu-id="99605-294">如果您在開發電腦是位於 NAT 後方，您的控制站可能無法運作而不需要進一步的網路設定。</span><span class="sxs-lookup"><span data-stu-id="99605-294">If your development machine is behind NAT, your Controller may not work without further network configuration.</span></span> <span data-ttu-id="99605-295">如需詳細資訊，請參閱這裡： [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)</span><span class="sxs-lookup"><span data-stu-id="99605-295">See here for details: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)</span></span>

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a><span data-ttu-id="99605-296">查詢字串值與 Facebook OAuth 參數衝突</span><span class="sxs-lookup"><span data-stu-id="99605-296">Query string values conflict with Facebook OAuth parameters</span></span>

<span data-ttu-id="99605-297">下列欄位衝突 Facebook OAuth 對話方塊呼叫以傳回 URL。</span><span class="sxs-lookup"><span data-stu-id="99605-297">The following fields conflict with Facebook OAuth dialog's call back URL.</span></span> <span data-ttu-id="99605-298">無法加入您自己的查詢字串值，以下列名稱： 程式碼、 錯誤、 錯誤\_描述、 錯誤\_原因。</span><span class="sxs-lookup"><span data-stu-id="99605-298">Do not add your own query string values with the following names: code, error, error\_description, error\_reason.</span></span>

#### <a name="using-page-inspector-with-facebook-template"></a><span data-ttu-id="99605-299">Page Inspector 使用 Facebook 範本</span><span class="sxs-lookup"><span data-stu-id="99605-299">Using Page Inspector with Facebook Template</span></span>

<span data-ttu-id="99605-300">您無法在偵錯您的 Facebook 應用程式時，Visual Studio 2012 中使用 Page Inspector 功能。</span><span class="sxs-lookup"><span data-stu-id="99605-300">You can't use the Page Inspector feature in Visual Studio 2012 while debugging your Facebook Application.</span></span> <span data-ttu-id="99605-301">Page Inspector 目前不支援 iframe。</span><span class="sxs-lookup"><span data-stu-id="99605-301">The Page Inspector does not currently support iframes.</span></span>

### <a name="single-page-application-template-issues"></a><span data-ttu-id="99605-302">單一頁面應用程式範本問題</span><span class="sxs-lookup"><span data-stu-id="99605-302">Single Page Application Template Issues</span></span>

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a><span data-ttu-id="99605-303">使用 JQuery 1.9 Knockout 2.2.1 更新，執行預設 MVC SPA 專案中，新增 todo 項目編輯時輸入焦點事件不會處理正確。</span><span class="sxs-lookup"><span data-stu-id="99605-303">With JQuery 1.9/Knockout 2.2.1 update, when running default MVC SPA project, new todo item edit enter focus event is not handled properly.</span></span>

<span data-ttu-id="99605-304">使用 JQuery 1.9/Knockout 2.2.1 更新中，執行預設 MVC SPA 專案時，新增 todo 項目編輯輸入不再焦點回新的 todo 項目編輯方塊輸入新的 todo 項目至 todo 清單之後。</span><span class="sxs-lookup"><span data-stu-id="99605-304">With JQuery 1.9/Knockout 2.2.1 update, when running default MVC SPA project, new todo item edit enter no longer focus back to the new todo item edit box after entering the new todo item to the todo list.</span></span>

<span data-ttu-id="99605-305">因應措施參考[ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)，而且使得在下列範例程式碼類似的修正程式：</span><span class="sxs-lookup"><span data-stu-id="99605-305">To workaround reference [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html), and make similar fix to the following sample code:</span></span>

<span data-ttu-id="99605-306">檔案 todo.model.js</span><span class="sxs-lookup"><span data-stu-id="99605-306">File todo.model.js</span></span>  
 <span data-ttu-id="99605-307">函式 todolist(data) 中，加入下列：</span><span class="sxs-lookup"><span data-stu-id="99605-307">function todolist(data), add following:</span></span>  
 <span data-ttu-id="99605-308">**self.isSelected = ko.observable(false);**</span><span class="sxs-lookup"><span data-stu-id="99605-308">**self.isSelected = ko.observable(false);**</span></span>

<span data-ttu-id="99605-309">todoList.prototype.addTodo 函式中，加入下列 blacked 的文字：</span><span class="sxs-lookup"><span data-stu-id="99605-309">function todoList.prototype.addTodo, add the following blacked text:</span></span>  
 <span data-ttu-id="99605-310">**self.isSelected(true);**</span><span class="sxs-lookup"><span data-stu-id="99605-310">**self.isSelected(true);**</span></span>  
 <span data-ttu-id="99605-311">self.newTodoTitle(&quot;&quot;);</span><span class="sxs-lookup"><span data-stu-id="99605-311">self.newTodoTitle(&quot;&quot;);</span></span>

<span data-ttu-id="99605-312">檔案 index.cshtml 中，加入下列 blacked 的文字：</span><span class="sxs-lookup"><span data-stu-id="99605-312">File index.cshtml, add the following blacked text:</span></span>  
 <span data-ttu-id="99605-313">&lt;資料繫結會形成 =&quot;提交： addTodo&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="99605-313">&lt;form data-bind=&quot;submit: addTodo&quot;&gt;</span></span>  
 <span data-ttu-id="99605-314">&lt;輸入類別 =&quot;addTodo&quot;類型 =&quot;文字&quot;資料繫結 =&quot;值： newTodoTitle，預留位置: '這裡要加入型別'、 blurOnEnter： 為 true， **hasfocus: isSelected**，事件: {模糊： addTodo}&quot; /&gt;</span><span class="sxs-lookup"><span data-stu-id="99605-314">&lt;input class=&quot;addTodo&quot; type=&quot;text&quot; data-bind=&quot;value: newTodoTitle, placeholder: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: { blur: addTodo }&quot; /&gt;</span></span>  
 <span data-ttu-id="99605-315">&lt;/form&gt;</span><span class="sxs-lookup"><span data-stu-id="99605-315">&lt;/form&gt;</span></span>
