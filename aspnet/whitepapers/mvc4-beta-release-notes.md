---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 58ae178a0e6578d8353e1a4e9d67fc1026e99f55
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4"></a><span data-ttu-id="15bf5-103">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="15bf5-103">ASP.NET MVC 4</span></span>
====================
> <span data-ttu-id="15bf5-104">本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-104">This document describes the release of ASP.NET MVC 4 Beta for Visual Studio 2010.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="15bf5-105">這不是最新的版本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-105">This is not the most current release.</span></span> <span data-ttu-id="15bf5-106">ASP.NET MVC 4 RC 版本資訊可用[這裡](mvc4-release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="15bf5-106">The ASP.NET MVC 4 RC release notes are available [here](mvc4-release-notes.md).</span></span>


- [<span data-ttu-id="15bf5-107">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="15bf5-107">Installation Notes</span></span>](#_Toc303253802)
- [<span data-ttu-id="15bf5-108">文件 (英文)</span><span class="sxs-lookup"><span data-stu-id="15bf5-108">Documentation</span></span>](#_Toc303253803)
- [<span data-ttu-id="15bf5-109">支援</span><span class="sxs-lookup"><span data-stu-id="15bf5-109">Support</span></span>](#_Toc303253804)
- [<span data-ttu-id="15bf5-110">軟體需求</span><span class="sxs-lookup"><span data-stu-id="15bf5-110">Software Requirements</span></span>](#_Toc303253805)
- [<span data-ttu-id="15bf5-111">ASP.NET MVC 3 專案升級至 ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="15bf5-111">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>](#_Toc303253806)
- [<span data-ttu-id="15bf5-112">ASP.NET MVC 4 Beta 的新功能</span><span class="sxs-lookup"><span data-stu-id="15bf5-112">New Features in ASP.NET MVC 4 Beta</span></span>](#_Toc303253807)

    - [<span data-ttu-id="15bf5-113">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="15bf5-113">ASP.NET Web API</span></span>](#_Toc317096197)
    - [<span data-ttu-id="15bf5-114">ASP.NET 單一頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="15bf5-114">ASP.NET Single Page Application</span></span>](#_Toc317096198)
    - [<span data-ttu-id="15bf5-115">預設專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="15bf5-115">Enhancements to Default Project Templates</span></span>](#_Toc303253808)
    - [<span data-ttu-id="15bf5-116">行動裝置的專案範本</span><span class="sxs-lookup"><span data-stu-id="15bf5-116">Mobile Project Template</span></span>](#_Toc303253809)
    - [<span data-ttu-id="15bf5-117">顯示模式</span><span class="sxs-lookup"><span data-stu-id="15bf5-117">Display Modes</span></span>](#_Toc303253810)
    - [<span data-ttu-id="15bf5-118">jQuery Mobile，檢視切換程式，並覆寫瀏覽器</span><span class="sxs-lookup"><span data-stu-id="15bf5-118">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>](#_Toc303253811)
    - [<span data-ttu-id="15bf5-119">在 Visual Studio 中的程式碼產生的訣竅</span><span class="sxs-lookup"><span data-stu-id="15bf5-119">Recipes for Code Generation in Visual Studio</span></span>](#_Toc303253812)
    - [<span data-ttu-id="15bf5-120">非同步控制器的工作支援</span><span class="sxs-lookup"><span data-stu-id="15bf5-120">Task Support for Asynchronous Controllers</span></span>](#_Toc303253813)
    - [<span data-ttu-id="15bf5-121">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="15bf5-121">Azure SDK</span></span>](#_Toc303253814)
    - [<span data-ttu-id="15bf5-122">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="15bf5-122">Known Issues and Breaking Changes</span></span>](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a><span data-ttu-id="15bf5-123">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="15bf5-123">Installation Notes</span></span>

<span data-ttu-id="15bf5-124">可以從安裝 ASP.NET MVC 4 Beta for Visual Studio 2010 [ASP.NET MVC 4 首頁](../mvc/mvc4.md)使用 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="15bf5-124">ASP.NET MVC 4 Beta for Visual Studio 2010 can be installed from the [ASP.NET MVC 4 home page](../mvc/mvc4.md) using the Web Platform Installer.</span></span>

<span data-ttu-id="15bf5-125">您必須解除安裝 ASP.NET MVC 4 Beta 之前的 ASP.NET MVC 4 的任何先前安裝的預覽。</span><span class="sxs-lookup"><span data-stu-id="15bf5-125">You must uninstall any previously installed previews of ASP.NET MVC 4 prior to installing ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="15bf5-126">此版本不相容的.NET Framework 4.5 Developer Preview。</span><span class="sxs-lookup"><span data-stu-id="15bf5-126">This release is not compatible with the .NET Framework 4.5 Developer Preview.</span></span> <span data-ttu-id="15bf5-127">您必須解除安裝.NET 4.5 Developer Preview，然後再安裝 ASP.NET MVC 4 Beta。</span><span class="sxs-lookup"><span data-stu-id="15bf5-127">You must uninstall the .NET 4.5 Developer Preview before installing the ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="15bf5-128">可以安裝 ASP.NET MVC 4，並且可以執行的並行使用 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="15bf5-128">ASP.NET MVC 4 can be installed and can run side-by-side with ASP.NET MVC 3.</span></span>

<a id="_Toc303253803"></a>
## <a name="documentation"></a><span data-ttu-id="15bf5-129">文件</span><span class="sxs-lookup"><span data-stu-id="15bf5-129">Documentation</span></span>

<span data-ttu-id="15bf5-130">ASP.NET mvc 的文件位於 MSDN 網站，位於下列 URL:</span><span class="sxs-lookup"><span data-stu-id="15bf5-130">Documentation for ASP.NET MVC is available on the MSDN website at the following URL:</span></span>

[<span data-ttu-id="15bf5-131">https://go.microsoft.com/fwlink/?LinkID=243043</span><span class="sxs-lookup"><span data-stu-id="15bf5-131">https://go.microsoft.com/fwlink/?LinkID=243043</span></span>](https://go.microsoft.com/fwlink/?LinkID=243043)

<span data-ttu-id="15bf5-132">教學課程和其他關於 ASP.NET MVC 的資訊位於 ASP.NET 網站的 MVC 4 頁 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。</span><span class="sxs-lookup"><span data-stu-id="15bf5-132">Tutorials and other information about ASP.NET MVC are available on the MVC 4 page of the ASP.NET website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).</span></span>

<a id="_Toc303253804"></a>
## <a name="support"></a><span data-ttu-id="15bf5-133">支援</span><span class="sxs-lookup"><span data-stu-id="15bf5-133">Support</span></span>

<span data-ttu-id="15bf5-134">這是預覽版本，而且未正式支援。</span><span class="sxs-lookup"><span data-stu-id="15bf5-134">This is a preview release and is not officially supported.</span></span> <span data-ttu-id="15bf5-135">如果您有關於使用此版本的問題，張貼至 ASP.NET MVC 論壇 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中的 ASP.NET 社群成員可經常提供非正式的支援。</span><span class="sxs-lookup"><span data-stu-id="15bf5-135">If you have questions about working with this release, post them to the ASP.NET MVC forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), where members of the ASP.NET community are frequently able to provide informal support.</span></span>

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a><span data-ttu-id="15bf5-136">軟體需求</span><span class="sxs-lookup"><span data-stu-id="15bf5-136">Software Requirements</span></span>

<span data-ttu-id="15bf5-137">Visual Studio 的 ASP.NET MVC 4 元件需要 PowerShell 2.0 和 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="15bf5-137">The ASP.NET MVC 4 components for Visual Studio require PowerShell 2.0 and either Visual Studio 2010 with Service Pack 1 or Visual Web Developer Express 2010 with Service Pack 1.</span></span>

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a><span data-ttu-id="15bf5-138">ASP.NET MVC 3 專案升級至 ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="15bf5-138">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>

<span data-ttu-id="15bf5-139">ASP.NET MVC 4 可以在同一部電腦，讓您彈性選擇何時要升級至 ASP.NET MVC 4 的 ASP.NET MVC 3 應用程式的 ASP.NET MVC 3 並存安裝。</span><span class="sxs-lookup"><span data-stu-id="15bf5-139">ASP.NET MVC 4 can be installed side by side with ASP.NET MVC 3 on the same computer, which gives you flexibility in choosing when to upgrade an ASP.NET MVC 3 application to ASP.NET MVC 4.</span></span>

<span data-ttu-id="15bf5-140">升級的最簡單方式是建立新的 ASP.NET MVC 4 專案，並複製所有的檢視、 控制器、 程式碼和內容檔案從現有的 MVC 3 專案加入新的專案，然後更新組件來參考新的專案，以符合舊的專案中。</span><span class="sxs-lookup"><span data-stu-id="15bf5-140">The simplest way to upgrade is to create a new ASP.NET MVC 4 project and copy all the views, controllers, code, and content files from the existing MVC 3 project to the new project and then to update the assembly references in the new project to match the old project.</span></span> <span data-ttu-id="15bf5-141">如果您已經變更 MVC 3 專案中的 Web.config 檔案，您也必須合併那些變更成 MVC 4 專案中的 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="15bf5-141">If you have made changes to the Web.config file in the MVC 3 project, you must also merge those changes into the Web.config file in the MVC 4 project.</span></span>

<span data-ttu-id="15bf5-142">若要手動升級現有的 ASP.NET MVC 3 應用程式第 4 版，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="15bf5-142">To manually upgrade an existing ASP.NET MVC 3 application to version 4, do the following:</span></span>

1. <span data-ttu-id="15bf5-143">在專案 （有一個根目錄中的專案在 [檢視] 資料夾中，一個在您的專案中的每個區域的 [檢視] 資料夾） 中的所有 Web.config 檔案，將每個執行個體的下列文字：</span><span class="sxs-lookup"><span data-stu-id="15bf5-143">In all Web.config files in the project (there is one in the root of the project, one in the Views folder, and one in the Views folder for each area in your project), replace every instance of the following text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    <span data-ttu-id="15bf5-144">使用下列相對應的文字：</span><span class="sxs-lookup"><span data-stu-id="15bf5-144">with the following corresponding text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. <span data-ttu-id="15bf5-145">在根 Web.config 檔案中，更新*webPages:Version* "2.0.0.0"的項目並加入新*PreserveLoginUrl*具有值"true"的機碼：</span><span class="sxs-lookup"><span data-stu-id="15bf5-145">In the root Web.config file, update the *webPages:Version* element to "2.0.0.0" and add a new *PreserveLoginUrl* key that has the value "true":</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. <span data-ttu-id="15bf5-146">在 [方案總管] 中，刪除其參考*System.Web.Mvc* （指向版本 3 DLL）。</span><span class="sxs-lookup"><span data-stu-id="15bf5-146">In Solution Explorer, delete the reference to *System.Web.Mvc* (which points to the version 3 DLL).</span></span> <span data-ttu-id="15bf5-147">然後將參考加入*System.Web.Mvc* (v4.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="15bf5-147">Then add a reference to *System.Web.Mvc* (v4.0.0.0).</span></span> <span data-ttu-id="15bf5-148">特別是，進行下列變更來更新組件參考。</span><span class="sxs-lookup"><span data-stu-id="15bf5-148">In particular, make the following changes to update the assembly references.</span></span> <span data-ttu-id="15bf5-149">詳細資料如下：</span><span class="sxs-lookup"><span data-stu-id="15bf5-149">Here are the details:</span></span>

    1. <span data-ttu-id="15bf5-150">在方案總管 中，刪除下列組件的參考：</span><span class="sxs-lookup"><span data-stu-id="15bf5-150">In Solution Explorer, delete the references to the following assemblies:</span></span> 

        - <span data-ttu-id="15bf5-151">*System.Web.Mvc*(v3.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-151">*System.Web.Mvc*(v3.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-152">*System.Web.WebPages*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-152">*System.Web.WebPages*(v1.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-153">*System.Web.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-153">*System.Web.Razor*(v1.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-154">*System.Web.WebPages.Deployment*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-154">*System.Web.WebPages.Deployment*(v1.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-155">*System.Web.WebPages.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-155">*System.Web.WebPages.Razor*(v1.0.0.0)</span></span>
    2. <span data-ttu-id="15bf5-156">加入下列組件的參考：</span><span class="sxs-lookup"><span data-stu-id="15bf5-156">Add a references to the following assemblies:</span></span> 

        - <span data-ttu-id="15bf5-157">*System.Web.Mvc*(v4.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-157">*System.Web.Mvc*(v4.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-158">*System.Web.WebPages*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-158">*System.Web.WebPages*(v2.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-159">*System.Web.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-159">*System.Web.Razor*(v2.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-160">*System.Web.WebPages.Deployment*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-160">*System.Web.WebPages.Deployment*(v2.0.0.0)</span></span>
        - <span data-ttu-id="15bf5-161">*System.Web.WebPages.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="15bf5-161">*System.Web.WebPages.Razor*(v2.0.0.0)</span></span>
4. <span data-ttu-id="15bf5-162">在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案。</span><span class="sxs-lookup"><span data-stu-id="15bf5-162">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="15bf5-163">然後名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。</span><span class="sxs-lookup"><span data-stu-id="15bf5-163">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
5. <span data-ttu-id="15bf5-164">找出*ProjectTypeGuids*項目和取代 {E53F8FEA-EAE0-44A6-8774-FFD645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。</span><span class="sxs-lookup"><span data-stu-id="15bf5-164">Locate the *ProjectTypeGuids* element and replace {E53F8FEA-EAE0-44A6-8774-FFD645390401} with {E3E379DF-F4C6-4180-9B81-6769533ABE47}.</span></span>
6. <span data-ttu-id="15bf5-165">儲存的變更，關閉您所編輯的專案 (.csproj) 檔案、 以滑鼠右鍵按一下專案，然後選取重新載入專案。</span><span class="sxs-lookup"><span data-stu-id="15bf5-165">Save the changes, close the project (.csproj) file you were editing, right-click the project, and then select Reload Project.</span></span>
7. <span data-ttu-id="15bf5-166">如果專案參考任何協力廠商程式庫編譯使用舊版 ASP.NET MVC，請開啟根 Web.config 檔案，並加入下列三個*bindingRedirect*下的項目*組態*> 一節：</span><span class="sxs-lookup"><span data-stu-id="15bf5-166">If the project references any third-party libraries that are compiled using previous versions of ASP.NET MVC, open the root Web.config file and add the following three *bindingRedirect* elements under the *configuration* section:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a><span data-ttu-id="15bf5-167">ASP.NET MVC 4 Beta 的新功能</span><span class="sxs-lookup"><span data-stu-id="15bf5-167">New Features in ASP.NET MVC 4 Beta</span></span>

<span data-ttu-id="15bf5-168">本節說明已引進的功能在 ASP.NET MVC 4 Beta 版本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-168">This section describes features that have been introduced in the ASP.NET MVC 4 Beta release.</span></span>

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a><span data-ttu-id="15bf5-169">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="15bf5-169">ASP.NET Web API</span></span>

<span data-ttu-id="15bf5-170">ASP.NET MVC 4 現在包含 ASP.NET Web API，用來建立 HTTP 服務的新架構可達到廣泛的用戶端包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="15bf5-170">ASP.NET MVC 4 now includes ASP.NET Web API, a new framework for creating HTTP services that can reach a broad range of clients including browsers and mobile devices.</span></span> <span data-ttu-id="15bf5-171">ASP.NET Web 應用程式開發介面也是用於建立 RESTful 服務的理想平台。</span><span class="sxs-lookup"><span data-stu-id="15bf5-171">ASP.NET Web API is also an ideal platform for building RESTful services.</span></span>

<span data-ttu-id="15bf5-172">ASP.NET Web API 包含下列功能的支援：</span><span class="sxs-lookup"><span data-stu-id="15bf5-172">ASP.NET Web API includes support for the following features:</span></span>

- <span data-ttu-id="15bf5-173">**現代 HTTP 程式設計模型：**直接存取及管理 HTTP 要求和回應您 Web 應用程式開發介面使用新的強型別 HTTP 物件模型。</span><span class="sxs-lookup"><span data-stu-id="15bf5-173">**Modern HTTP programming model:** Directly access and manipulate HTTP requests and responses in your Web APIs using a new, strongly typed HTTP object model.</span></span> <span data-ttu-id="15bf5-174">相同程式設計模型和 HTTP 管線功能對稱用戶端透過新的 HttpClient 型別。</span><span class="sxs-lookup"><span data-stu-id="15bf5-174">The same programming model and HTTP pipeline is symmetrically available on the client through the new HttpClient type.</span></span>
- <span data-ttu-id="15bf5-175">**完整支援路由**: Web Api 現在支援一組完整的路由功能已經 Web 堆疊，包括路由參數和條件約束的一部分。</span><span class="sxs-lookup"><span data-stu-id="15bf5-175">**Full support for routes**: Web APIs now support the full set of route capabilities that have always been a part of the Web stack, including route parameters and constraints.</span></span> <span data-ttu-id="15bf5-176">此外，對應到動作具有完整支援慣例，因此您不再需要套用屬性，例如 [HttpPost] 到您的類別和方法。</span><span class="sxs-lookup"><span data-stu-id="15bf5-176">Additionally, mapping to actions has full support for conventions, so you no longer need to apply attributes such as [HttpPost] to your classes and methods.</span></span>
- <span data-ttu-id="15bf5-177">**內容交涉**： 用戶端和伺服器可一起運作來判斷正確的格式從 API 傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="15bf5-177">**Content negotiation**: The client and server can work together to determine the right format for data being returned from an API.</span></span> <span data-ttu-id="15bf5-178">我們提供預設的支援，如 XML、 JSON 和表單 URL 編碼格式，而且您可以擴充此一支援藉由新增您自己的格式器，或甚至取代預設內容交涉的策略。</span><span class="sxs-lookup"><span data-stu-id="15bf5-178">We provide default support for XML, JSON, and Form URL-encoded formats, and you can extend this support by adding your own formatters, or even replace the default content negotiation strategy.</span></span>
- <span data-ttu-id="15bf5-179">**模型繫結和驗證：**模型繫結器提供一個簡單的方式，從 HTTP 要求的各種組件中擷取資料，並將這些訊息部分轉換成.NET 物件可由 Web API 的動作。</span><span class="sxs-lookup"><span data-stu-id="15bf5-179">**Model binding and validation:** Model binders provide an easy way to extract data from various parts of an HTTP request and convert those message parts into .NET objects which can be used by the Web API actions.</span></span>
- <span data-ttu-id="15bf5-180">**篩選條件：** Web Api 現在支援篩選，包括知名的篩選條件，例如 [Authorize] 屬性。</span><span class="sxs-lookup"><span data-stu-id="15bf5-180">**Filters:** Web APIs now supports filters, including well-known filters such as the [Authorize] attribute.</span></span> <span data-ttu-id="15bf5-181">您可以撰寫，並插入您自己的篩選動作、 授權和例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="15bf5-181">You can author and plug in your own filters for actions, authorization and exception handling.</span></span>
- <span data-ttu-id="15bf5-182">**在查詢組合：**只要傳回 IQueryable&lt;T&gt;，Web API 將支援查詢透過 OData URL 慣例。</span><span class="sxs-lookup"><span data-stu-id="15bf5-182">**Query composition:** By simply returning IQueryable&lt;T&gt;, your Web API will support querying via the OData URL conventions.</span></span>
- <span data-ttu-id="15bf5-183">**改善可測試性的詳細資料，HTTP:**而不是靜態內容物件中設定 HTTP 詳細資料，Web API 動作現在可以搭配 HttpRequestMessage 和 HttpResponseMessage 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="15bf5-183">**Improved testability of HTTP details:** Rather than setting HTTP details in static context objects, Web API actions can now work with instances of HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="15bf5-184">這些物件的泛型版本也存在於讓您使用您的自訂類型，除了 HTTP 型別。</span><span class="sxs-lookup"><span data-stu-id="15bf5-184">Generic versions of these objects also exist to let you work with your custom types in addition to the HTTP types.</span></span>
- <span data-ttu-id="15bf5-185">**改善的逆轉控制 (IoC) 透過 dependencyresolver 來註冊：** Web API 現在使用 MVC 的相依性解析程式所實作的服務定位程式模式來取得執行個體的許多不同的功能。</span><span class="sxs-lookup"><span data-stu-id="15bf5-185">**Improved Inversion of Control (IoC) via DependencyResolver:** Web API now uses the service locator pattern implemented by MVC's dependency resolver to obtain instances for many different facilities.</span></span>
- <span data-ttu-id="15bf5-186">**程式碼為基礎的組態：**等作業只透過程式碼已完成 Web API 設定、 清除離開您的組態檔。</span><span class="sxs-lookup"><span data-stu-id="15bf5-186">**Code-based configuration:** Web API configuration is accomplished solely through code, leaving your config files clean.</span></span>
- <span data-ttu-id="15bf5-187">**自我裝載：** Web Api 可以裝載於自己的處理序，除了 IIS 時仍在使用路由的完整功能及其他功能的 Web API。</span><span class="sxs-lookup"><span data-stu-id="15bf5-187">**Self-host:** Web APIs can be hosted in your own process in addition to IIS while still using the full power of routes and other features of Web API.</span></span>

<span data-ttu-id="15bf5-188">如需 ASP.NET Web API 的詳細資訊，請造訪[https://www.asp.net/web-api](../web-api/index.md)。</span><span class="sxs-lookup"><span data-stu-id="15bf5-188">For more details on ASP.NET Web API please visit [https://www.asp.net/web-api](../web-api/index.md).</span></span>

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a><span data-ttu-id="15bf5-189">ASP.NET 單一頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="15bf5-189">ASP.NET Single Page Application</span></span>

<span data-ttu-id="15bf5-190">ASP.NET MVC 4 現在包含早期預覽中建置單一頁面應用程式的重要使用 JavaScript 和 Web Api 的用戶端互動的體驗。</span><span class="sxs-lookup"><span data-stu-id="15bf5-190">ASP.NET MVC 4 now includes an early preview of the experience for building single page applications with significant client-side interactions using JavaScript and Web APIs.</span></span> <span data-ttu-id="15bf5-191">這項支援包括：</span><span class="sxs-lookup"><span data-stu-id="15bf5-191">This support includes:</span></span>

- <span data-ttu-id="15bf5-192">一組更豐富的本機互動與快取資料的 JavaScript 程式庫</span><span class="sxs-lookup"><span data-stu-id="15bf5-192">A set of JavaScript libraries for richer local interactions with cached data</span></span>
- <span data-ttu-id="15bf5-193">為單位的工作，以及 DAL 支援的其他 Web API 元件</span><span class="sxs-lookup"><span data-stu-id="15bf5-193">Additional Web API components for unit of work and DAL support</span></span>
- <span data-ttu-id="15bf5-194">與以便迅速開始使用 scaffolding MVC 專案範本</span><span class="sxs-lookup"><span data-stu-id="15bf5-194">An MVC project template with scaffolding to get started quickly</span></span>

<span data-ttu-id="15bf5-195">針對支援 ASP.NET MVC 4 中的單一頁面應用程式上的更多詳細資料，請瀏覽[https://www.asp.net/single-page-application](../single-page-application/index.md)。</span><span class="sxs-lookup"><span data-stu-id="15bf5-195">For more details on the Single Page Application support in ASP.NET MVC 4 please visit [https://www.asp.net/single-page-application](../single-page-application/index.md).</span></span>

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a><span data-ttu-id="15bf5-196">預設專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="15bf5-196">Enhancements to Default Project Templates</span></span>

<span data-ttu-id="15bf5-197">已更新用來建立新的 ASP.NET MVC 4 專案的範本來建立更現代尋找網站：</span><span class="sxs-lookup"><span data-stu-id="15bf5-197">The template that is used to create new ASP.NET MVC 4 projects has been updated to create a more modern-looking website:</span></span>

![](mvc4-beta-release-notes/_static/image1.png)

<span data-ttu-id="15bf5-198">除了外觀的增強功能，已改善功能的新範本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-198">In addition to cosmetic improvements, there's improved functionality in the new template.</span></span> <span data-ttu-id="15bf5-199">範本會使用稱為調整看來，桌面瀏覽器和行動裝置沒有任何自訂的瀏覽器中呈現的技術。</span><span class="sxs-lookup"><span data-stu-id="15bf5-199">The template employs a technique called adaptive rendering to look good in both desktop browsers and mobile browsers without any customization.</span></span>

![](mvc4-beta-release-notes/_static/image2.png)

<span data-ttu-id="15bf5-200">若要查看作用中的適應性呈現，您可以使用行動裝置的模擬器，或只要再試一次調整大小較小的桌面瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="15bf5-200">To see adaptive rendering in action, you can use a mobile emulator or just try resizing the desktop browser window to be smaller.</span></span> <span data-ttu-id="15bf5-201">當瀏覽器視窗中取得夠小時，將變更的頁面配置。</span><span class="sxs-lookup"><span data-stu-id="15bf5-201">When the browser window gets small enough, the layout of the page will change.</span></span>

<span data-ttu-id="15bf5-202">預設專案範本的其他增強功能，就是使用 JavaScript，提供更豐富的 UI。</span><span class="sxs-lookup"><span data-stu-id="15bf5-202">Another enhancement to the default project template is the use of JavaScript to provide a richer UI.</span></span> <span data-ttu-id="15bf5-203">使用範本中的登入和註冊連結是有關如何使用 jQuery UI 對話方塊呈現的豐富的登入畫面的範例：</span><span class="sxs-lookup"><span data-stu-id="15bf5-203">The Login and Register links that are used in the template are examples of how to use the jQuery UI Dialog to present a rich login screen:</span></span>

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a><span data-ttu-id="15bf5-204">行動裝置的專案範本</span><span class="sxs-lookup"><span data-stu-id="15bf5-204">Mobile Project Template</span></span>

<span data-ttu-id="15bf5-205">如果您開始新的專案，並想要建立站台專為行動和平板電腦瀏覽器，您可以使用新的行動應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-205">If you're starting a new project and want to create a site specifically for mobile and tablet browsers, you can use the new Mobile Application project template.</span></span> <span data-ttu-id="15bf5-206">這根據 jQuery Mobile，用於建置觸控最佳化 UI 的開放原始碼程式庫：</span><span class="sxs-lookup"><span data-stu-id="15bf5-206">This is based on jQuery Mobile, an open-source library for building touch-optimized UI:</span></span>

![](mvc4-beta-release-notes/_static/image4.png)

<span data-ttu-id="15bf5-207">此範本包含網際網路應用程式範本相同的應用程式結構 （而且控制器的程式碼幾乎完全相同），但卻能呈現最佳效果，並能在觸控式行動裝置的行為使用 jQuery Mobile 的樣式。</span><span class="sxs-lookup"><span data-stu-id="15bf5-207">This template contains the same application structure as the Internet Application template (and the controller code is virtually identical), but it's styled using jQuery Mobile to look good and behave well on touch-based mobile devices.</span></span> <span data-ttu-id="15bf5-208">若要了解有關如何在結構和樣式行動 UI 的詳細資訊，請參閱[jQuery 行動專案網站](http://jquerymobile.com/)。</span><span class="sxs-lookup"><span data-stu-id="15bf5-208">To learn more about how to structure and style mobile UI, see the [jQuery Mobile project website](http://jquerymobile.com/).</span></span>

<span data-ttu-id="15bf5-209">如果您已經有您想要加入，行動裝置最佳化的檢視，或如果您想要建立單一站台提供不同樣式桌上型電腦和行動瀏覽器來檢視桌面導向網站，您可以使用新的顯示模式功能。</span><span class="sxs-lookup"><span data-stu-id="15bf5-209">If you already have a desktop-oriented site that you want to add mobile-optimized views to, or if you want to create a single site that serves differently styled views to desktop and mobile browsers, you can use the new Display Modes feature.</span></span> <span data-ttu-id="15bf5-210">（請參閱下一節）。</span><span class="sxs-lookup"><span data-stu-id="15bf5-210">(See the next section.)</span></span>

<a id="_Toc303253810"></a>
### <a name="display-modes"></a><span data-ttu-id="15bf5-211">顯示模式</span><span class="sxs-lookup"><span data-stu-id="15bf5-211">Display Modes</span></span>

<span data-ttu-id="15bf5-212">新的顯示模式功能可讓應用程式選取依提出要求的瀏覽器的檢視。</span><span class="sxs-lookup"><span data-stu-id="15bf5-212">The new Display Modes feature lets an application select views depending on the browser that's making the request.</span></span> <span data-ttu-id="15bf5-213">例如，如果桌面瀏覽器要求在首頁上，應用程式可能會使用 Views\Home\Index.cshtml 範本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-213">For example, if a desktop browser requests the Home page, the application might use the Views\Home\Index.cshtml template.</span></span> <span data-ttu-id="15bf5-214">如果行動瀏覽器要求在首頁上，應用程式可能會傳回 Views\Home\Index.mobile.cshtml 範本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-214">If a mobile browser requests the Home page, the application might return the Views\Home\Index.mobile.cshtml template.</span></span>

<span data-ttu-id="15bf5-215">版面配置和 partials 也會覆寫特定瀏覽器類型。</span><span class="sxs-lookup"><span data-stu-id="15bf5-215">Layouts and partials can also be overridden for particular browser types.</span></span> <span data-ttu-id="15bf5-216">例如: </span><span class="sxs-lookup"><span data-stu-id="15bf5-216">For example:</span></span>

- <span data-ttu-id="15bf5-217">如果同時 Views\Shared 資料夾包含\_Layout.cshtml 和\_Layout.mobile.cshtml 範本，應用程式會使用預設\_行動瀏覽器和要求期間Layout.mobile.cshtml\_Layout.cshtml 期間其他要求。</span><span class="sxs-lookup"><span data-stu-id="15bf5-217">If your Views\Shared folder contains both the \_Layout.cshtml and \_Layout.mobile.cshtml templates, by default the application will use \_Layout.mobile.cshtml during requests from mobile browsers and \_Layout.cshtml during other requests.</span></span>
- <span data-ttu-id="15bf5-218">如果資料夾包含同時\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指示@Html.Partial("\_MyPartial") 會轉譯\_MyPartial.mobile.cshtml 期間從行動裝置的要求瀏覽器和\_MyPartial.cshtml 期間其他要求。</span><span class="sxs-lookup"><span data-stu-id="15bf5-218">If a folder contains both \_MyPartial.cshtml and \_MyPartial.mobile.cshtml, the instruction @Html.Partial("\_MyPartial") will render \_MyPartial.mobile.cshtml during requests from mobile browsers, and \_MyPartial.cshtml during other requests.</span></span>

<span data-ttu-id="15bf5-219">如果您想要建立更詳細的檢視、 配置、 或其他裝置的部分檢視，您可以註冊新*DefaultDisplayMode*指定的名稱： 要求符合特定條件時，要搜尋的執行個體。</span><span class="sxs-lookup"><span data-stu-id="15bf5-219">If you want to create more specific views, layouts, or partial views for other devices, you can register a new *DefaultDisplayMode* instance to specify which name to search for when a request satisfies particular conditions.</span></span> <span data-ttu-id="15bf5-220">例如，您可以加入下列程式碼加入*應用程式\_啟動*Global.asax 檔案以登錄為適用於 Apple iPhone 瀏覽器提出要求時的顯示模式的字串"iPhone"中的方法：</span><span class="sxs-lookup"><span data-stu-id="15bf5-220">For example, you could add the following code to the *Application\_Start* method in the Global.asax file to register the string "iPhone" as a display mode that applies when the Apple iPhone browser makes a request:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

<span data-ttu-id="15bf5-221">執行此程式碼，當提出要求時 Apple iPhone 瀏覽器之後，您的應用程式都會使用 _layout.cshtml\\_Layout.iPhone.cshtml 配置 （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="15bf5-221">After this code runs, when an Apple iPhone browser makes a request, your application will use the Views\Shared\\_Layout.iPhone.cshtml layout (if it exists).</span></span>

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a><span data-ttu-id="15bf5-222">jQuery Mobile，檢視切換程式，並覆寫瀏覽器</span><span class="sxs-lookup"><span data-stu-id="15bf5-222">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>

<span data-ttu-id="15bf5-223">jQuery Mobile 是開放原始碼程式庫建置觸控最佳化 web UI。</span><span class="sxs-lookup"><span data-stu-id="15bf5-223">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="15bf5-224">如果您想要的 ASP.NET MVC 4 應用程式使用 jQuery Mobile，您可以下載並安裝的 NuGet 封裝，可協助您開始。</span><span class="sxs-lookup"><span data-stu-id="15bf5-224">If you want to use jQuery Mobile with an ASP.NET MVC 4 application, you can download and install a NuGet package that helps you get started.</span></span> <span data-ttu-id="15bf5-225">若要安裝的 Visual Studio Package Manager Console 中，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="15bf5-225">To install it from the Visual Studio Package Manager Console, type the following command:</span></span>

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

<span data-ttu-id="15bf5-226">這會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：</span><span class="sxs-lookup"><span data-stu-id="15bf5-226">This installs jQuery Mobile and some helper files, including the following:</span></span>

- <span data-ttu-id="15bf5-227">Views/Shared/\_Layout.Mobile.cshtml，為 jQuery Mobile 架構版面配置。</span><span class="sxs-lookup"><span data-stu-id="15bf5-227">Views/Shared/\_Layout.Mobile.cshtml, which is a jQuery Mobile-based layout.</span></span>
- <span data-ttu-id="15bf5-228">檢視切換程式元件，其中包含Views/Shared/\_ViewSwitcher.cshtml 部分檢視和 ViewSwitcherController.cs 控制站。</span><span class="sxs-lookup"><span data-stu-id="15bf5-228">A view-switcher component, which consists of the Views/Shared/\_ViewSwitcher.cshtml partial view and the ViewSwitcherController.cs controller.</span></span>

<span data-ttu-id="15bf5-229">安裝封裝之後，執行應用程式使用行動裝置瀏覽器 (或同等權限，例如 Firefox[使用者代理程式切換器](http://chrispederick.com/work/user-agent-switcher/)附加元件)。</span><span class="sxs-lookup"><span data-stu-id="15bf5-229">After you install the package, run your application using a mobile browser (or equivalent, like the Firefox [User Agent Switcher](http://chrispederick.com/work/user-agent-switcher/) add-on).</span></span> <span data-ttu-id="15bf5-230">您會看到，您的網頁看起來非常不同，因為 jQuery Mobile 處理配置和樣式設定。</span><span class="sxs-lookup"><span data-stu-id="15bf5-230">You'll see that your pages look quite different, because jQuery Mobile handles layout and styling.</span></span> <span data-ttu-id="15bf5-231">若要利用這一點，您可以執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="15bf5-231">To take advantage of this, you can do the following:</span></span>

- <span data-ttu-id="15bf5-232">建立行動裝置的特定檢視覆寫，如底下所述[顯示模式](#_Toc303253810)之前 （例如，建立 Views\Home\Index.mobile.cshtml 來覆寫 Views\Home\Index.cshtml 行動瀏覽器）。</span><span class="sxs-lookup"><span data-stu-id="15bf5-232">Create mobile-specific view overrides as described under [Display Modes](#_Toc303253810) earlier (for example, create Views\Home\Index.mobile.cshtml to override Views\Home\Index.cshtml for mobile browsers).</span></span>
- <span data-ttu-id="15bf5-233">讀取[jQuery Mobile 說明文件](http://jquerymobile.com/)若要深入了解如何新增行動檢視觸控最佳化 UI 項目。</span><span class="sxs-lookup"><span data-stu-id="15bf5-233">Read the [jQuery Mobile documentation](http://jquerymobile.com/) to learn more about how to add touch-optimized UI elements in mobile views.</span></span>

<span data-ttu-id="15bf5-234">行動裝置最佳化網頁的慣例是頁面的加入連結，其文字是頁面的像桌面檢視或完整的網站模式可讓使用者切換到桌面版本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-234">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="15bf5-235">JQuery.Mobile.MVC 封裝包含可用於此用途範例檢視切換程式元件。</span><span class="sxs-lookup"><span data-stu-id="15bf5-235">The jQuery.Mobile.MVC package includes a sample view-switcher component for this purpose.</span></span> <span data-ttu-id="15bf5-236">在預設 _layout.cshtml\\_Layout.Mobile.cshtml 檢視中，並在呈現的頁面時，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="15bf5-236">It's used in the default Views\Shared\\_Layout.Mobile.cshtml view, and it looks like this when the page is rendered:</span></span>

![](mvc4-beta-release-notes/_static/image5.png)

<span data-ttu-id="15bf5-237">訪客按一下連結，如果它們正在切換到桌面版本的相同的頁面。</span><span class="sxs-lookup"><span data-stu-id="15bf5-237">If visitors click the link, they're switched to the desktop version of the same page.</span></span>

<span data-ttu-id="15bf5-238">因為您桌面的配置不會包含檢視切換程式預設的訪客將不能為行動模式取得的方法。</span><span class="sxs-lookup"><span data-stu-id="15bf5-238">Because your desktop layout will not include a view switcher by default, visitors won't have a way to get to mobile mode.</span></span> <span data-ttu-id="15bf5-239">若要啟用此功能，加入下列參考 *\_ViewSwitcher*到您桌面的配置，只是內部*&lt;主體&gt;*項目：</span><span class="sxs-lookup"><span data-stu-id="15bf5-239">To enable this, add the following reference to *\_ViewSwitcher* to your desktop layout, just inside the *&lt;body&gt;* element:</span></span>

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

<span data-ttu-id="15bf5-240">檢視切換程式會使用新功能，稱為 瀏覽器覆寫。</span><span class="sxs-lookup"><span data-stu-id="15bf5-240">The view switcher uses a new feature called Browser Overriding.</span></span> <span data-ttu-id="15bf5-241">這項功能可讓您的應用程式要求視為就像是從不同的瀏覽器 （使用者代理程式） 與它們實際從。</span><span class="sxs-lookup"><span data-stu-id="15bf5-241">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they're actually from.</span></span> <span data-ttu-id="15bf5-242">下表列出可在瀏覽器覆寫的方法。</span><span class="sxs-lookup"><span data-stu-id="15bf5-242">The following table lists the methods that Browser Overriding provides.</span></span>

| `HttpContext.SetOverriddenBrowser(userAgentString)` | <span data-ttu-id="15bf5-243">使用指定的使用者代理程式要求的實際使用者代理程式值會覆寫。</span><span class="sxs-lookup"><span data-stu-id="15bf5-243">Overrides the request's actual user agent value using the specified user agent.</span></span> |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | <span data-ttu-id="15bf5-244">如果尚未指定任何覆寫會傳回要求的使用者代理程式覆寫值或實際使用者代理字串。</span><span class="sxs-lookup"><span data-stu-id="15bf5-244">Returns the request's user agent override value, or the actual user agent string if no override has been specified.</span></span> |
| `HttpContext.GetOverriddenBrowser()` | <span data-ttu-id="15bf5-245">傳回*HttpBrowserCapabilitiesBase*對應於目前針對要求設定的使用者代理程式執行個體 （實際或覆寫）。</span><span class="sxs-lookup"><span data-stu-id="15bf5-245">Returns an *HttpBrowserCapabilitiesBase* instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="15bf5-246">您可以使用此值來取得屬性，例如*IsMobileDevice*。</span><span class="sxs-lookup"><span data-stu-id="15bf5-246">You can use this value to get properties such as *IsMobileDevice*.</span></span> |
| `HttpContext.ClearOverriddenBrowser()` | <span data-ttu-id="15bf5-247">移除目前要求的任何覆寫的使用者代理程式。</span><span class="sxs-lookup"><span data-stu-id="15bf5-247">Removes any overridden user agent for the current request.</span></span> |

<span data-ttu-id="15bf5-248">瀏覽器正在覆寫 ASP.NET MVC 4 的核心功能，而且可以使用，即使您未安裝 jQuery.Mobile.MVC 封裝。</span><span class="sxs-lookup"><span data-stu-id="15bf5-248">Browser Overriding is a core feature of ASP.NET MVC 4 and is available even if you don't install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="15bf5-249">不過，它會影響檢視、 配置和部分檢視的選取範圍，不會影響其他任何 ASP.NET 功能相依於*Request.Browser*物件。</span><span class="sxs-lookup"><span data-stu-id="15bf5-249">However, it affects only view, layout, and partial-view selection — it does not affect any other ASP.NET feature that depends on the *Request.Browser* object.</span></span>

<span data-ttu-id="15bf5-250">根據預設，使用者代理程式覆寫會使用 cookie 來儲存。</span><span class="sxs-lookup"><span data-stu-id="15bf5-250">By default, the user-agent override is stored using a cookie.</span></span> <span data-ttu-id="15bf5-251">如果您想要存放覆寫其他位置 （例如，在資料庫中），您可以取代預設提供者 (*BrowserOverrideStores.Current*)。</span><span class="sxs-lookup"><span data-stu-id="15bf5-251">If you want to store the override elsewhere (for example, in a database), you can replace the default provider (*BrowserOverrideStores.Current*).</span></span> <span data-ttu-id="15bf5-252">此提供者的文件會隨附較新版的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="15bf5-252">Documentation for this provider will be available to accompany a later release of ASP.NET MVC.</span></span>

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a><span data-ttu-id="15bf5-253">在 Visual Studio 中的程式碼產生的訣竅</span><span class="sxs-lookup"><span data-stu-id="15bf5-253">Recipes for Code Generation in Visual Studio</span></span>

<span data-ttu-id="15bf5-254">新的配方功能可讓 Visual Studio 為套件產生您可以使用 NuGet 來安裝的套件為基礎的解決方案專用程式碼。</span><span class="sxs-lookup"><span data-stu-id="15bf5-254">The new Recipes feature enables Visual Studio to generate solution-specific code based on packages that you can install using NuGet.</span></span> <span data-ttu-id="15bf5-255">配方架構，簡化開發人員撰寫程式碼產生外掛程式，您也可以使用新增區域、 加入控制器，並加入檢視來取代內建的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="15bf5-255">The Recipes framework makes it easy for developers to write code-generation plugins, which you can also use to replace the built-in code generators for Add Area, Add Controller, and Add View.</span></span> <span data-ttu-id="15bf5-256">配方部署為 NuGet 封裝，因為它們可輕鬆簽入原始檔控制及自動在專案上的所有開發人員與共用。</span><span class="sxs-lookup"><span data-stu-id="15bf5-256">Because recipes are deployed as NuGet packages, they can easily be checked into source control and shared with all developers on the project automatically.</span></span> <span data-ttu-id="15bf5-257">它們也會提供每個解決方案為基礎。</span><span class="sxs-lookup"><span data-stu-id="15bf5-257">They are also available on a per-solution basis.</span></span>

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a><span data-ttu-id="15bf5-258">非同步控制器的工作支援</span><span class="sxs-lookup"><span data-stu-id="15bf5-258">Task Support for Asynchronous Controllers</span></span>

<span data-ttu-id="15bf5-259">現在您可以撰寫非同步動作方法為單一方法的傳回型別的物件*工作*或*工作&lt;ActionResult&gt;*。</span><span class="sxs-lookup"><span data-stu-id="15bf5-259">You can now write asynchronous action methods as single methods that return an object of type *Task* or *Task&lt;ActionResult&gt;*.</span></span>

<span data-ttu-id="15bf5-260">例如，如果您使用 Visual C# 5 (或使用[非同步 CTP](https://msdn.microsoft.com/vstudio/async.aspx))，您可以建立非同步動作方法看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="15bf5-260">For example, if you're using Visual C# 5 (or using the [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), you can create an asynchronous action method that looks like the following:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

<span data-ttu-id="15bf5-261">在上一個動作方法的呼叫*newsService.GetHeadlinesAsync*和*sportsService.GetScoresAsync*非同步呼叫，而不會封鎖執行緒集區的執行緒。</span><span class="sxs-lookup"><span data-stu-id="15bf5-261">In the previous action method, the calls to *newsService.GetHeadlinesAsync* and *sportsService.GetScoresAsync* are called asynchronously and do not block a thread from the thread pool.</span></span>

<span data-ttu-id="15bf5-262">非同步動作方法會傳回*工作*執行個體也可以支援逾時。</span><span class="sxs-lookup"><span data-stu-id="15bf5-262">Asynchronous action methods that return *Task* instances can also support timeouts.</span></span> <span data-ttu-id="15bf5-263">若要在動作方法可取消，請加入類型參數的*CancellationToken*至動作方法簽章。</span><span class="sxs-lookup"><span data-stu-id="15bf5-263">To make your action method cancellable, add a parameter of type *CancellationToken* to the action method signature.</span></span> <span data-ttu-id="15bf5-264">下列範例示範非同步動作方法，有 2500年毫秒的逾時，而且會顯示*TimedOut*檢視用戶端，如果發生逾時。</span><span class="sxs-lookup"><span data-stu-id="15bf5-264">The following example shows an asynchronous action method that has a timeout of 2500 milliseconds and that displays a *TimedOut* view to the client if a timeout occurs.</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a><span data-ttu-id="15bf5-265">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="15bf5-265">Azure SDK</span></span>

<span data-ttu-id="15bf5-266">ASP.NET MVC 4 Beta 支援年 9 月 2011 1.5 版的 Windows Azure sdk。</span><span class="sxs-lookup"><span data-stu-id="15bf5-266">ASP.NET MVC 4 Beta supports the September 2011 1.5 release of the Windows Azure SDK.</span></span>

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="15bf5-267">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="15bf5-267">Known Issues and Breaking Changes</span></span>

- <span data-ttu-id="15bf5-268">**安裝 ASP.NET MVC 4 Beta 之後, CSHTML/VBHTML 編輯器，在 Visual Studio 2010 Service Pack 1 CSHTML/VBHTML 編輯器中的可能會暫停輸入 cshtml 」 或 「 vbhtml 檔內的程式碼片段或 JavaScript 之後很長的時間。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-268">**After installing ASP.NET MVC 4 Beta, the CSHTML/VBHTML editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML editor may pause for a long time after typing snippet or JavaScript inside cshtml or vbhtml files.**</span></span> <span data-ttu-id="15bf5-269">這只會發生在具有剛剛建立，而且尚未編譯的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="15bf5-269">This occurs only in ASP.NET MVC 4 applications which have just been created and have not yet been compiled.</span></span>

    <span data-ttu-id="15bf5-270">因應措施是編譯的專案，以取得組件的 bin 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="15bf5-270">The workaround is to compile the project to get the assemblies in the bin folder.</span></span> <span data-ttu-id="15bf5-271">請注意，是否您清除它會從 bin 資料夾移除組件專案，將會返回編輯器問題。</span><span class="sxs-lookup"><span data-stu-id="15bf5-271">Note, if you clean the project which removes the assemblies from the bin folder, the editor problem will come back.</span></span>

    <span data-ttu-id="15bf5-272">這將會更正的下一個版本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-272">This will be corrected in the next release.</span></span>
- <span data-ttu-id="15bf5-273">**Visual Studio 11 Beta 的 C# 專案範本包含 Global.asax.cs 中不正確的連接字串。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-273">**C# Project templates for Visual Studio 11 Beta contain an incorrect connection string in Global.asax.cs.**</span></span> <span data-ttu-id="15bf5-274">指定應用程式中的預設連接\_Start 方法的 Visual Studio 11 beta 版中建立的專案包含 LocalDB 連接字串，其中包含未逸出反斜線 (\)字元。</span><span class="sxs-lookup"><span data-stu-id="15bf5-274">The default connection specified in the Application\_Start method for projects created in Visual Studio 11 Beta contain a LocalDB connection string which contains an unescaped backslash (\) character.</span></span> <span data-ttu-id="15bf5-275">這會導致連接錯誤時嘗試存取 Entity Framework DbContext，它會產生 SqlException。</span><span class="sxs-lookup"><span data-stu-id="15bf5-275">This results in a connection error upon attempts to access a Entity Framework DbContext, which generates a SqlException.</span></span>

    <span data-ttu-id="15bf5-276">若要更正此問題，逸出反斜線字元的應用程式中\_啟動 Global.asax.cs 方法，使它會讀取，如下所示：</span><span class="sxs-lookup"><span data-stu-id="15bf5-276">To correct this issue, escape the backslash character in the App\_Start method of Global.asax.cs so that it reads as follows:</span></span>

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- <span data-ttu-id="15bf5-277">**.NET 4.5 為目標的 ASP.NET MVC 4 應用程式將會擲回 FileLoadException 時嘗試存取 System.Net.Http.dll 組件時執行.NET 4.0。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-277">**ASP.NET MVC 4 applications which target .NET 4.5 will throw a FileLoadException upon attempt to access the System.Net.Http.dll assembly when run under .NET 4.0.**</span></span> <span data-ttu-id="15bf5-278">在.NET 4.5 下建立的 ASP.NET MVC 4 應用程式包含的繫結重新導向，將會導致 FileLoadException，指出 「 無法載入檔案或組件 'System.Net.Http' 或其中一個相依性。 」</span><span class="sxs-lookup"><span data-stu-id="15bf5-278">ASP.NET MVC 4 applications created under .NET 4.5 contain a binding redirect that will result in a FileLoadException which that states "Could not load file or assembly 'System.Net.Http' or one of its dependencies."</span></span> <span data-ttu-id="15bf5-279">當應用程式執行時的系統上安裝.NET 4.0。</span><span class="sxs-lookup"><span data-stu-id="15bf5-279">when the application is executed on a system with .NET 4.0 installed.</span></span> <span data-ttu-id="15bf5-280">若要更正此問題，請從 web.config 中移除下列繫結重新導向：</span><span class="sxs-lookup"><span data-stu-id="15bf5-280">To correct this issue, remove the following binding redirect from web.config:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    <span data-ttu-id="15bf5-281">已修改的 web.config 中的組件繫結項目應該會出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="15bf5-281">The assembly binding element in the modified web.config should appear as follows:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <span data-ttu-id="15bf5-282">**「 新增控制器 」 項目範本在 Visual Basic 專案會產生不正確的命名空間時叫用 \* \* \* 從區域內。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-282">**The "Add Controller" item template in Visual Basic projects generates an incorrect namespace when invoked****from inside an area.**</span></span> <span data-ttu-id="15bf5-283">當您將控制器加入 ASP.NET MVC 專案會使用 Visual Basic 中的區域時，項目範本會插入控制器錯誤的命名空間。</span><span class="sxs-lookup"><span data-stu-id="15bf5-283">When you add a controller to an area in an ASP.NET MVC project that uses Visual Basic, the item template inserts the wrong namespace into the controller.</span></span> <span data-ttu-id="15bf5-284">當您巡覽至控制器中的任何動作時，結果會是 「 找不到檔案 」 錯誤。</span><span class="sxs-lookup"><span data-stu-id="15bf5-284">The result is a "file not found" error when you navigate to any action in the controller.</span></span>  
  
 <span data-ttu-id="15bf5-285">產生的命名空間會省略所有項目之後的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="15bf5-285">The generated namespace omits everything after the root namespace.</span></span> <span data-ttu-id="15bf5-286">例如，產生的命名空間是*RootNamespace*但應該是*RootNamespace.Areas.AreaName.Controllers* 。</span><span class="sxs-lookup"><span data-stu-id="15bf5-286">For example, the namespace generated is *RootNamespace* but should be *RootNamespace.Areas.AreaName.Controllers* .</span></span>
- <span data-ttu-id="15bf5-287">**Razor 檢視引擎中的重大變更。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-287">**Breaking changes in the Razor View Engine.**</span></span> <span data-ttu-id="15bf5-288">重寫成的 Razor 剖析器的一部分，下列類型已從移除*System.Web.Mvc.Razor*:</span><span class="sxs-lookup"><span data-stu-id="15bf5-288">As part of a rewrite of the Razor parser, the following types were removed from *System.Web.Mvc.Razor*:</span></span> 

    - <span data-ttu-id="15bf5-289">*ModelSpan*</span><span class="sxs-lookup"><span data-stu-id="15bf5-289">*ModelSpan*</span></span>
    - <span data-ttu-id="15bf5-290">*MvcVBRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="15bf5-290">*MvcVBRazorCodeGenerator*</span></span>
    - <span data-ttu-id="15bf5-291">*MvcCSharpRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="15bf5-291">*MvcCSharpRazorCodeGenerator*</span></span>
    - <span data-ttu-id="15bf5-292">*MvcVBRazorCodeParser*</span><span class="sxs-lookup"><span data-stu-id="15bf5-292">*MvcVBRazorCodeParser*</span></span>

 <span data-ttu-id="15bf5-293">也已移除下列方法：</span><span class="sxs-lookup"><span data-stu-id="15bf5-293">The following methods were also removed:</span></span> 

    - <span data-ttu-id="15bf5-294">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="15bf5-294">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
    - <span data-ttu-id="15bf5-295">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span><span class="sxs-lookup"><span data-stu-id="15bf5-295">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span></span>
    - <span data-ttu-id="15bf5-296">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="15bf5-296">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
- <span data-ttu-id="15bf5-297">**當 WebMatrix.WebData.dll 隨附於 ASP.NET MVC 4 應用程式的 /bin 目錄中時，因此它會接管表單驗證的 URL。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-297">**When WebMatrix.WebData.dll is included in in the /bin directory of an ASP.NET MVC 4 apps, it takes over the URL for forms authentication.**</span></span> <span data-ttu-id="15bf5-298">WebMatrix.WebData.dll 組件加入至應用程式 （例如，藉由使用 [新增可部署的相依性] 對話方塊時，請選取 「 ASP.NET Web 網頁使用 Razor 語法 」） 將會覆寫/帳戶/登入驗證登入重新導向而 /帳戶/登入以預期的 ASP.NET MVC 帳戶控制器的預設方式。</span><span class="sxs-lookup"><span data-stu-id="15bf5-298">Adding the WebMatrix.WebData.dll assembly to your application (for example, by selecting "ASP.NET Web Pages with Razor Syntax" when using the Add Deployable Dependencies dialog) will override the authentication login redirect to /account/logon rather than /account/login as expected by the default ASP.NET MVC Account Controller.</span></span> <span data-ttu-id="15bf5-299">要避免這種行為，並使用 web.config 的 [驗證] 區段中已指定的 URL，您可以加入稱為 PreserveLoginUrl appSetting 並將它設定為 true:</span><span class="sxs-lookup"><span data-stu-id="15bf5-299">To prevent this behavior and use the URL specified already in the authentication section of web.config, you can add an appSetting called PreserveLoginUrl and set it to true:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- <span data-ttu-id="15bf5-300">**NuGet 套件管理員安裝嘗試安裝 ASP.NET MVC 4 的 Visual Studio 2010 和 Visual Web Developer 2010 的並存安裝時失敗。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-300">**The NuGet package manager fails to install when attempting to install ASP.NET MVC 4 for side by side installations of Visual Studio 2010 and Visual Web Developer 2010.**</span></span> <span data-ttu-id="15bf5-301">若要執行 Visual Studio 2010 和 Visual Web Developer 2010 並存 ASP.NET MVC 4 中，您必須安裝 ASP.NET MVC 4 之後已安裝 Visual Studio 的兩個版本。</span><span class="sxs-lookup"><span data-stu-id="15bf5-301">To run Visual Studio 2010 and Visual Web Developer 2010 side by side with ASP.NET MVC 4 you must install ASP.NET MVC 4 after both versions of Visual Studio have already been installed.</span></span>
- <span data-ttu-id="15bf5-302">**如果已經已解除安裝先決條件，解除安裝 ASP.NET MVC 4 會失敗。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-302">**Uninstalling ASP.NET MVC 4 fails if prerequisites have already been uninstalled.**</span></span> <span data-ttu-id="15bf5-303">若要完全解除安裝 ASP.NET MVC 4you 必須解除安裝 ASP.NET MVC 4，然後再解除安裝 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="15bf5-303">To cleanly uninstall ASP.NET MVC 4you must uninstall ASP.NET MVC 4 prior to uninstalling Visual Studio.</span></span>
- <span data-ttu-id="15bf5-304">**執行預設的 Web API 專案會顯示不正確地將使用者導向至加入路由使用 RegisterApis 方法不存在的指示。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-304">**Running a default Web API project shows instructions that incorrectly direct the user to add routes using the RegisterApis method, which doesn't exist.**</span></span> <span data-ttu-id="15bf5-305">使用 ASP.NET 路由表 RegisterRoutes 方法中，加入路由。</span><span class="sxs-lookup"><span data-stu-id="15bf5-305">Routes should be added in the RegisterRoutes method using the ASP.NET route table.</span></span>
- <span data-ttu-id="15bf5-306">**安裝 ASP.NET MVC 4 Beta 中斷 ASP.NET MVC 3 RTM 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="15bf5-306">**Installing ASP.NET MVC 4 Beta breaks ASP.NET MVC 3 RTM applications.**</span></span> <span data-ttu-id="15bf5-307">ASP.NET MVC 3 應用程式所建立的 rtm 發行版本 （不能搭配 ASP.NET MVC 3 Tools Update 版本） 會需要下列變更才能使用 ASP.NET MVC 4 Beta 並存。</span><span class="sxs-lookup"><span data-stu-id="15bf5-307">ASP.NET MVC 3 applications that were created with the RTM release (not with the ASP.NET MVC 3 Tools Update release) require the following changes in order to work side-by-side with ASP.NET MVC 4 Beta.</span></span> <span data-ttu-id="15bf5-308">建置專案，而不需要進行這些更新的結果中的編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="15bf5-308">Building the project without making these updates results in compilation errors.</span></span> 

    <span data-ttu-id="15bf5-309">**必要的更新**</span><span class="sxs-lookup"><span data-stu-id="15bf5-309">**Required updates**</span></span>

    1. <span data-ttu-id="15bf5-310">在根 Web.config 檔案中，加入新 *&lt;appSettings&gt;* 具有索引鍵的項目*webPages:Version*和值*1.0.0.0*。</span><span class="sxs-lookup"><span data-stu-id="15bf5-310">In the root Web.config file, add a new *&lt;appSettings&gt;* entry with the key *webPages:Version* and the value *1.0.0.0*.</span></span>

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. <span data-ttu-id="15bf5-311">在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案。</span><span class="sxs-lookup"><span data-stu-id="15bf5-311">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="15bf5-312">然後名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。</span><span class="sxs-lookup"><span data-stu-id="15bf5-312">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
    3. <span data-ttu-id="15bf5-313">找出下列組件參考：</span><span class="sxs-lookup"><span data-stu-id="15bf5-313">Locate the following assembly references:</span></span> 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        <span data-ttu-id="15bf5-314">它們取代成下列：</span><span class="sxs-lookup"><span data-stu-id="15bf5-314">Replace them with the following:</span></span>

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. <span data-ttu-id="15bf5-315">儲存變更，請關閉您先前所編輯，然後以滑鼠右鍵按一下專案並選取重新載入專案 (.csproj) 檔案。</span><span class="sxs-lookup"><span data-stu-id="15bf5-315">Save the changes, close the project (.csproj) file you were editing, and then right-click the project and select Reload.</span></span>
