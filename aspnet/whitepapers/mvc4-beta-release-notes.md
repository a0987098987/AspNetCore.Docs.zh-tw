---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: f1d949ec716ea8cb677c54fe5b07431161c58fbc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831847"
---
<a name="aspnet-mvc-4"></a><span data-ttu-id="12a9c-103">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="12a9c-103">ASP.NET MVC 4</span></span>
====================
> <span data-ttu-id="12a9c-104">本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。</span><span class="sxs-lookup"><span data-stu-id="12a9c-104">This document describes the release of ASP.NET MVC 4 Beta for Visual Studio 2010.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="12a9c-105">這不是最新的版本。</span><span class="sxs-lookup"><span data-stu-id="12a9c-105">This is not the most current release.</span></span> <span data-ttu-id="12a9c-106">ASP.NET MVC 4 RC 版本資訊可從[此處](mvc4-release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="12a9c-106">The ASP.NET MVC 4 RC release notes are available [here](mvc4-release-notes.md).</span></span>


- [<span data-ttu-id="12a9c-107">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="12a9c-107">Installation Notes</span></span>](#_Toc303253802)
- [<span data-ttu-id="12a9c-108">文件</span><span class="sxs-lookup"><span data-stu-id="12a9c-108">Documentation</span></span>](#_Toc303253803)
- [<span data-ttu-id="12a9c-109">支援</span><span class="sxs-lookup"><span data-stu-id="12a9c-109">Support</span></span>](#_Toc303253804)
- [<span data-ttu-id="12a9c-110">軟體需求</span><span class="sxs-lookup"><span data-stu-id="12a9c-110">Software Requirements</span></span>](#_Toc303253805)
- [<span data-ttu-id="12a9c-111">將 ASP.NET MVC 3 專案升級至 ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="12a9c-111">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>](#_Toc303253806)
- [<span data-ttu-id="12a9c-112">ASP.NET MVC 4 beta 版的新功能</span><span class="sxs-lookup"><span data-stu-id="12a9c-112">New Features in ASP.NET MVC 4 Beta</span></span>](#_Toc303253807)

    - [<span data-ttu-id="12a9c-113">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="12a9c-113">ASP.NET Web API</span></span>](#_Toc317096197)
    - [<span data-ttu-id="12a9c-114">ASP.NET 單一頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="12a9c-114">ASP.NET Single Page Application</span></span>](#_Toc317096198)
    - [<span data-ttu-id="12a9c-115">預設專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="12a9c-115">Enhancements to Default Project Templates</span></span>](#_Toc303253808)
    - [<span data-ttu-id="12a9c-116">行動裝置專案範本</span><span class="sxs-lookup"><span data-stu-id="12a9c-116">Mobile Project Template</span></span>](#_Toc303253809)
    - [<span data-ttu-id="12a9c-117">顯示模式</span><span class="sxs-lookup"><span data-stu-id="12a9c-117">Display Modes</span></span>](#_Toc303253810)
    - [<span data-ttu-id="12a9c-118">jQuery Mobile，檢視切換器，以及瀏覽器覆寫</span><span class="sxs-lookup"><span data-stu-id="12a9c-118">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>](#_Toc303253811)
    - [<span data-ttu-id="12a9c-119">Visual Studio 中的程式碼產生的訣竅</span><span class="sxs-lookup"><span data-stu-id="12a9c-119">Recipes for Code Generation in Visual Studio</span></span>](#_Toc303253812)
    - [<span data-ttu-id="12a9c-120">非同步控制器的的工作支援</span><span class="sxs-lookup"><span data-stu-id="12a9c-120">Task Support for Asynchronous Controllers</span></span>](#_Toc303253813)
    - [<span data-ttu-id="12a9c-121">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="12a9c-121">Azure SDK</span></span>](#_Toc303253814)
    - [<span data-ttu-id="12a9c-122">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="12a9c-122">Known Issues and Breaking Changes</span></span>](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a><span data-ttu-id="12a9c-123">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="12a9c-123">Installation Notes</span></span>

<span data-ttu-id="12a9c-124">您可以從安裝 Visual Studio 2010 的 ASP.NET MVC 4 Beta [ASP.NET MVC 4 首頁](../mvc/mvc4.md)使用 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="12a9c-124">ASP.NET MVC 4 Beta for Visual Studio 2010 can be installed from the [ASP.NET MVC 4 home page](../mvc/mvc4.md) using the Web Platform Installer.</span></span>

<span data-ttu-id="12a9c-125">您必須先解除安裝任何先前安裝的預覽，再安裝 ASP.NET MVC 4 beta 版的 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="12a9c-125">You must uninstall any previously installed previews of ASP.NET MVC 4 prior to installing ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="12a9c-126">此版本不相容於.NET Framework 4.5 Developer Preview。</span><span class="sxs-lookup"><span data-stu-id="12a9c-126">This release is not compatible with the .NET Framework 4.5 Developer Preview.</span></span> <span data-ttu-id="12a9c-127">您必須解除安裝.NET 4.5 Developer Preview，然後再安裝 ASP.NET MVC 4 Beta。</span><span class="sxs-lookup"><span data-stu-id="12a9c-127">You must uninstall the .NET 4.5 Developer Preview before installing the ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="12a9c-128">ASP.NET MVC 4 可以安裝，而且可以執行並排顯示 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="12a9c-128">ASP.NET MVC 4 can be installed and can run side-by-side with ASP.NET MVC 3.</span></span>

<a id="_Toc303253803"></a>
## <a name="documentation"></a><span data-ttu-id="12a9c-129">文件</span><span class="sxs-lookup"><span data-stu-id="12a9c-129">Documentation</span></span>

<span data-ttu-id="12a9c-130">ASP.NET mvc 的文件位於 MSDN 網站，下列 URL:</span><span class="sxs-lookup"><span data-stu-id="12a9c-130">Documentation for ASP.NET MVC is available on the MSDN website at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

<span data-ttu-id="12a9c-131">教學課程和 ASP.NET MVC 的其他資訊位於 ASP.NET 網站的 MVC 4 頁 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。</span><span class="sxs-lookup"><span data-stu-id="12a9c-131">Tutorials and other information about ASP.NET MVC are available on the MVC 4 page of the ASP.NET website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).</span></span>

<a id="_Toc303253804"></a>
## <a name="support"></a><span data-ttu-id="12a9c-132">支援</span><span class="sxs-lookup"><span data-stu-id="12a9c-132">Support</span></span>

<span data-ttu-id="12a9c-133">這是預覽版本，而且未正式支援。</span><span class="sxs-lookup"><span data-stu-id="12a9c-133">This is a preview release and is not officially supported.</span></span> <span data-ttu-id="12a9c-134">如果您有使用此版本的相關問題，將其張貼至 ASP.NET MVC 論壇 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中的 ASP.NET 社群成員可經常提供非正式的支援。</span><span class="sxs-lookup"><span data-stu-id="12a9c-134">If you have questions about working with this release, post them to the ASP.NET MVC forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), where members of the ASP.NET community are frequently able to provide informal support.</span></span>

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a><span data-ttu-id="12a9c-135">軟體需求</span><span class="sxs-lookup"><span data-stu-id="12a9c-135">Software Requirements</span></span>

<span data-ttu-id="12a9c-136">Visual Studio 的 ASP.NET MVC 4 元件需要 PowerShell 2.0 和為 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="12a9c-136">The ASP.NET MVC 4 components for Visual Studio require PowerShell 2.0 and either Visual Studio 2010 with Service Pack 1 or Visual Web Developer Express 2010 with Service Pack 1.</span></span>

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a><span data-ttu-id="12a9c-137">將 ASP.NET MVC 3 專案升級至 ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="12a9c-137">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>

<span data-ttu-id="12a9c-138">ASP.NET MVC 4 可以與 ASP.NET MVC 3 並存安裝在同一部電腦，讓您彈性地選擇何時要升級至 ASP.NET MVC 4 的 ASP.NET MVC 3 應用程式。</span><span class="sxs-lookup"><span data-stu-id="12a9c-138">ASP.NET MVC 4 can be installed side by side with ASP.NET MVC 3 on the same computer, which gives you flexibility in choosing when to upgrade an ASP.NET MVC 3 application to ASP.NET MVC 4.</span></span>

<span data-ttu-id="12a9c-139">若要建立新的 ASP.NET MVC 4 專案，並複製所有檢視、 控制器、 程式碼和內容都檔案從現有的 MVC 3 專案至新的專案，然後若要更新組件參考中新的專案，以符合舊的專案最簡單的方式升級。</span><span class="sxs-lookup"><span data-stu-id="12a9c-139">The simplest way to upgrade is to create a new ASP.NET MVC 4 project and copy all the views, controllers, code, and content files from the existing MVC 3 project to the new project and then to update the assembly references in the new project to match the old project.</span></span> <span data-ttu-id="12a9c-140">如果您變更至 MVC 3 專案中的 Web.config 檔案的內容，您必須也那些變更合併至 MVC 4 專案中的 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="12a9c-140">If you have made changes to the Web.config file in the MVC 3 project, you must also merge those changes into the Web.config file in the MVC 4 project.</span></span>

<span data-ttu-id="12a9c-141">若要手動升級現有的 ASP.NET MVC 3 應用程式第 4 版，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="12a9c-141">To manually upgrade an existing ASP.NET MVC 3 application to version 4, do the following:</span></span>

1. <span data-ttu-id="12a9c-142">在專案 （有一個專案，在 [Views] 資料夾中，一個在專案中的每個區域的 [Views] 資料夾的根目錄中） 中的所有 Web.config 檔案，將每個執行個體的下列文字：</span><span class="sxs-lookup"><span data-stu-id="12a9c-142">In all Web.config files in the project (there is one in the root of the project, one in the Views folder, and one in the Views folder for each area in your project), replace every instance of the following text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    <span data-ttu-id="12a9c-143">使用下列相對應的文字：</span><span class="sxs-lookup"><span data-stu-id="12a9c-143">with the following corresponding text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. <span data-ttu-id="12a9c-144">在根 Web.config 檔案中，更新*webPages:Version*為"2.0.0.0"的項目並新增一個新*PreserveLoginUrl*具有值"true"的機碼：</span><span class="sxs-lookup"><span data-stu-id="12a9c-144">In the root Web.config file, update the *webPages:Version* element to "2.0.0.0" and add a new *PreserveLoginUrl* key that has the value "true":</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. <span data-ttu-id="12a9c-145">在 [方案總管] 中，刪除其參考*System.Web.Mvc* （指向版本 3 的 DLL）。</span><span class="sxs-lookup"><span data-stu-id="12a9c-145">In Solution Explorer, delete the reference to *System.Web.Mvc* (which points to the version 3 DLL).</span></span> <span data-ttu-id="12a9c-146">然後將參考加入*System.Web.Mvc* (v4.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="12a9c-146">Then add a reference to *System.Web.Mvc* (v4.0.0.0).</span></span> <span data-ttu-id="12a9c-147">特別是，進行下列變更來更新組件參考。</span><span class="sxs-lookup"><span data-stu-id="12a9c-147">In particular, make the following changes to update the assembly references.</span></span> <span data-ttu-id="12a9c-148">詳細資料如下：</span><span class="sxs-lookup"><span data-stu-id="12a9c-148">Here are the details:</span></span>

    1. <span data-ttu-id="12a9c-149">在 [方案總管] 中，刪除下列組件參考：</span><span class="sxs-lookup"><span data-stu-id="12a9c-149">In Solution Explorer, delete the references to the following assemblies:</span></span> 

        - <span data-ttu-id="12a9c-150">*System.Web.Mvc*(v3.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-150">*System.Web.Mvc*(v3.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-151">*System.Web.WebPages*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-151">*System.Web.WebPages*(v1.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-152">*System.Web.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-152">*System.Web.Razor*(v1.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-153">*System.Web.WebPages.Deployment*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-153">*System.Web.WebPages.Deployment*(v1.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-154">*System.Web.WebPages.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-154">*System.Web.WebPages.Razor*(v1.0.0.0)</span></span>
    2. <span data-ttu-id="12a9c-155">新增下列組件的參考：</span><span class="sxs-lookup"><span data-stu-id="12a9c-155">Add a references to the following assemblies:</span></span> 

        - <span data-ttu-id="12a9c-156">*System.Web.Mvc*(v4.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-156">*System.Web.Mvc*(v4.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-157">*System.Web.WebPages*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-157">*System.Web.WebPages*(v2.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-158">*System.Web.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-158">*System.Web.Razor*(v2.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-159">*System.Web.WebPages.Deployment*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-159">*System.Web.WebPages.Deployment*(v2.0.0.0)</span></span>
        - <span data-ttu-id="12a9c-160">*System.Web.WebPages.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="12a9c-160">*System.Web.WebPages.Razor*(v2.0.0.0)</span></span>
4. <span data-ttu-id="12a9c-161">在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案</span><span class="sxs-lookup"><span data-stu-id="12a9c-161">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="12a9c-162">然後再次以滑鼠右鍵按一下名稱，然後選取 編輯*ProjectName*.csproj。</span><span class="sxs-lookup"><span data-stu-id="12a9c-162">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
5. <span data-ttu-id="12a9c-163">找出*ProjectTypeGuids*項目，並取代 {E53F8FEA-EAE0-44A6-8774-FFD645390401} 與 {E3E379DF-F4C6-4180-9B81-6769533ABE47}。</span><span class="sxs-lookup"><span data-stu-id="12a9c-163">Locate the *ProjectTypeGuids* element and replace {E53F8FEA-EAE0-44A6-8774-FFD645390401} with {E3E379DF-F4C6-4180-9B81-6769533ABE47}.</span></span>
6. <span data-ttu-id="12a9c-164">儲存變更，關閉您所編輯的專案 (.csproj) 檔案，以滑鼠右鍵按一下專案，，然後選取重新載入專案。</span><span class="sxs-lookup"><span data-stu-id="12a9c-164">Save the changes, close the project (.csproj) file you were editing, right-click the project, and then select Reload Project.</span></span>
7. <span data-ttu-id="12a9c-165">如果專案參考會編譯使用舊版 ASP.NET MVC 的任何協力廠商程式庫，請開啟根 Web.config 檔案，並新增下列三個*bindingRedirect*下方的項目*設定*區段：</span><span class="sxs-lookup"><span data-stu-id="12a9c-165">If the project references any third-party libraries that are compiled using previous versions of ASP.NET MVC, open the root Web.config file and add the following three *bindingRedirect* elements under the *configuration* section:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a><span data-ttu-id="12a9c-166">ASP.NET MVC 4 beta 版的新功能</span><span class="sxs-lookup"><span data-stu-id="12a9c-166">New Features in ASP.NET MVC 4 Beta</span></span>

<span data-ttu-id="12a9c-167">本節說明已引進的功能在 ASP.NET MVC 4 Beta 版本。</span><span class="sxs-lookup"><span data-stu-id="12a9c-167">This section describes features that have been introduced in the ASP.NET MVC 4 Beta release.</span></span>

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a><span data-ttu-id="12a9c-168">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="12a9c-168">ASP.NET Web API</span></span>

<span data-ttu-id="12a9c-169">ASP.NET MVC 4 現在包含 ASP.NET Web API，新的架構，用於建立 HTTP 服務可以連線到各種包括瀏覽器和行動裝置的用戶端。</span><span class="sxs-lookup"><span data-stu-id="12a9c-169">ASP.NET MVC 4 now includes ASP.NET Web API, a new framework for creating HTTP services that can reach a broad range of clients including browsers and mobile devices.</span></span> <span data-ttu-id="12a9c-170">ASP.NET Web API 也是建置 RESTful 服務的理想平台。</span><span class="sxs-lookup"><span data-stu-id="12a9c-170">ASP.NET Web API is also an ideal platform for building RESTful services.</span></span>

<span data-ttu-id="12a9c-171">ASP.NET Web API 包括下列功能的支援：</span><span class="sxs-lookup"><span data-stu-id="12a9c-171">ASP.NET Web API includes support for the following features:</span></span>

- <span data-ttu-id="12a9c-172">**現代的 HTTP 程式設計模型：** 直接存取和處理 HTTP 要求和回應中使用新的強型別 HTTP 物件模型對 Web Api。</span><span class="sxs-lookup"><span data-stu-id="12a9c-172">**Modern HTTP programming model:** Directly access and manipulate HTTP requests and responses in your Web APIs using a new, strongly typed HTTP object model.</span></span> <span data-ttu-id="12a9c-173">相同程式設計模型和 HTTP 管線功能數採對稱式用戶端透過新的 HttpClient 型別。</span><span class="sxs-lookup"><span data-stu-id="12a9c-173">The same programming model and HTTP pipeline is symmetrically available on the client through the new HttpClient type.</span></span>
- <span data-ttu-id="12a9c-174">**路由的完整支援**: Web Api 現在支援一組完整的路由功能，一直 Web 堆疊，包括路由參數和條件約束的一部分。</span><span class="sxs-lookup"><span data-stu-id="12a9c-174">**Full support for routes**: Web APIs now support the full set of route capabilities that have always been a part of the Web stack, including route parameters and constraints.</span></span> <span data-ttu-id="12a9c-175">此外，對應至動作必須完整支援慣例，所以您不再需要套用屬性，例如 [HttpPost] 您的類別和方法。</span><span class="sxs-lookup"><span data-stu-id="12a9c-175">Additionally, mapping to actions has full support for conventions, so you no longer need to apply attributes such as [HttpPost] to your classes and methods.</span></span>
- <span data-ttu-id="12a9c-176">**內容交涉**： 用戶端和伺服器可一起運作來判斷正確的格式，從 API 傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="12a9c-176">**Content negotiation**: The client and server can work together to determine the right format for data being returned from an API.</span></span> <span data-ttu-id="12a9c-177">我們會提供預設的支援，如 XML、 JSON 和表單 URL 編碼格式，而且您可以新增您自己的格式器，來擴充這項支援，或甚至取代預設的內容交涉策略。</span><span class="sxs-lookup"><span data-stu-id="12a9c-177">We provide default support for XML, JSON, and Form URL-encoded formats, and you can extend this support by adding your own formatters, or even replace the default content negotiation strategy.</span></span>
- <span data-ttu-id="12a9c-178">**模型繫結和驗證：** 模型繫結器提供簡單的方式擷取 HTTP 要求的各種組件中的資料，並將這些訊息部分轉換成.NET 物件可由 Web API 動作。</span><span class="sxs-lookup"><span data-stu-id="12a9c-178">**Model binding and validation:** Model binders provide an easy way to extract data from various parts of an HTTP request and convert those message parts into .NET objects which can be used by the Web API actions.</span></span>
- <span data-ttu-id="12a9c-179">**篩選器：** Web Api 現在支援篩選，包括知名的篩選條件，例如 [Authorize] 屬性。</span><span class="sxs-lookup"><span data-stu-id="12a9c-179">**Filters:** Web APIs now supports filters, including well-known filters such as the [Authorize] attribute.</span></span> <span data-ttu-id="12a9c-180">您可以撰寫，並插入您自己的篩選動作、 授權和例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="12a9c-180">You can author and plug in your own filters for actions, authorization and exception handling.</span></span>
- <span data-ttu-id="12a9c-181">**查詢組合：** 只要傳回 IQueryable&lt;T&gt;，您的 Web API 會支援查詢透過 OData URL 慣例。</span><span class="sxs-lookup"><span data-stu-id="12a9c-181">**Query composition:** By simply returning IQueryable&lt;T&gt;, your Web API will support querying via the OData URL conventions.</span></span>
- <span data-ttu-id="12a9c-182">**改善可測試性的詳細資料，HTTP:** 而不是靜態的內容物件中設定 HTTP 詳細資料之外，Web API 動作現在可以搭配 HttpRequestMessage 和 HttpResponseMessage 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="12a9c-182">**Improved testability of HTTP details:** Rather than setting HTTP details in static context objects, Web API actions can now work with instances of HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="12a9c-183">可讓您使用您的自訂類型，除了 HTTP 型別也有這些物件的泛型版本。</span><span class="sxs-lookup"><span data-stu-id="12a9c-183">Generic versions of these objects also exist to let you work with your custom types in addition to the HTTP types.</span></span>
- <span data-ttu-id="12a9c-184">**改善的控制反轉 (IoC) 透過 dependencyresolver 來註冊：** Web API 現在使用 MVC 的相依性解析程式所實作的服務定位程式模式，來取得執行個體的許多不同的功能。</span><span class="sxs-lookup"><span data-stu-id="12a9c-184">**Improved Inversion of Control (IoC) via DependencyResolver:** Web API now uses the service locator pattern implemented by MVC's dependency resolver to obtain instances for many different facilities.</span></span>
- <span data-ttu-id="12a9c-185">**程式碼為基礎的組態：** Web API 完成只會透過程式碼，讓您設定檔案清除。</span><span class="sxs-lookup"><span data-stu-id="12a9c-185">**Code-based configuration:** Web API configuration is accomplished solely through code, leaving your config files clean.</span></span>
- <span data-ttu-id="12a9c-186">**自我裝載：** Web Api 可以裝載在自己的程序，除了 IIS 之外，同時仍然使用路由的完整功能和其他功能的 Web API。</span><span class="sxs-lookup"><span data-stu-id="12a9c-186">**Self-host:** Web APIs can be hosted in your own process in addition to IIS while still using the full power of routes and other features of Web API.</span></span>

<span data-ttu-id="12a9c-187">如需 ASP.NET Web API 的詳細資訊，請造訪[ https://www.asp.net/web-api ](../web-api/index.md)。</span><span class="sxs-lookup"><span data-stu-id="12a9c-187">For more details on ASP.NET Web API please visit [https://www.asp.net/web-api](../web-api/index.md).</span></span>

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a><span data-ttu-id="12a9c-188">ASP.NET 單一頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="12a9c-188">ASP.NET Single Page Application</span></span>

<span data-ttu-id="12a9c-189">ASP.NET MVC 4 現在包含建置單一頁面應用程式的重要使用 JavaScript 和 Web Api 的用戶端互動的體驗的早期預覽。</span><span class="sxs-lookup"><span data-stu-id="12a9c-189">ASP.NET MVC 4 now includes an early preview of the experience for building single page applications with significant client-side interactions using JavaScript and Web APIs.</span></span> <span data-ttu-id="12a9c-190">這項支援包括：</span><span class="sxs-lookup"><span data-stu-id="12a9c-190">This support includes:</span></span>

- <span data-ttu-id="12a9c-191">一組更豐富互動本機快取資料的 JavaScript 程式庫</span><span class="sxs-lookup"><span data-stu-id="12a9c-191">A set of JavaScript libraries for richer local interactions with cached data</span></span>
- <span data-ttu-id="12a9c-192">額外的工作單位和 DAL 支援的 Web API 元件</span><span class="sxs-lookup"><span data-stu-id="12a9c-192">Additional Web API components for unit of work and DAL support</span></span>
- <span data-ttu-id="12a9c-193">若要快速開始使用 scaffolding MVC 專案範本</span><span class="sxs-lookup"><span data-stu-id="12a9c-193">An MVC project template with scaffolding to get started quickly</span></span>

<span data-ttu-id="12a9c-194">支援 ASP.NET MVC 4 中的單一頁面應用程式上的更多詳細資料請造訪[ https://www.asp.net/single-page-application ](../single-page-application/index.md)。</span><span class="sxs-lookup"><span data-stu-id="12a9c-194">For more details on the Single Page Application support in ASP.NET MVC 4 please visit [https://www.asp.net/single-page-application](../single-page-application/index.md).</span></span>

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a><span data-ttu-id="12a9c-195">預設專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="12a9c-195">Enhancements to Default Project Templates</span></span>

<span data-ttu-id="12a9c-196">已更新用來建立新的 ASP.NET MVC 4 專案的範本來建立更現代化外觀的網站：</span><span class="sxs-lookup"><span data-stu-id="12a9c-196">The template that is used to create new ASP.NET MVC 4 projects has been updated to create a more modern-looking website:</span></span>

![](mvc4-beta-release-notes/_static/image1.png)

<span data-ttu-id="12a9c-197">除了外觀的增強功能，已改進新的範本中的功能。</span><span class="sxs-lookup"><span data-stu-id="12a9c-197">In addition to cosmetic improvements, there's improved functionality in the new template.</span></span> <span data-ttu-id="12a9c-198">範本會針對稱為適應性呈現看來，在桌面瀏覽器和行動瀏覽器，而不需要任何自訂的技術。</span><span class="sxs-lookup"><span data-stu-id="12a9c-198">The template employs a technique called adaptive rendering to look good in both desktop browsers and mobile browsers without any customization.</span></span>

![](mvc4-beta-release-notes/_static/image2.png)

<span data-ttu-id="12a9c-199">若要查看作用中的自適性轉譯，您可以使用行動裝置的模擬器，或試試調整大小變得更小的桌面瀏覽器 視窗。</span><span class="sxs-lookup"><span data-stu-id="12a9c-199">To see adaptive rendering in action, you can use a mobile emulator or just try resizing the desktop browser window to be smaller.</span></span> <span data-ttu-id="12a9c-200">當瀏覽器視窗中取得夠小時，將變更頁面的配置。</span><span class="sxs-lookup"><span data-stu-id="12a9c-200">When the browser window gets small enough, the layout of the page will change.</span></span>

<span data-ttu-id="12a9c-201">預設專案範本的其他增強功能是使用 JavaScript 來提供更豐富的 UI。</span><span class="sxs-lookup"><span data-stu-id="12a9c-201">Another enhancement to the default project template is the use of JavaScript to provide a richer UI.</span></span> <span data-ttu-id="12a9c-202">使用範本中的登入和註冊連結是有關如何使用 jQuery UI 對話方塊呈現豐富的登入畫面的範例：</span><span class="sxs-lookup"><span data-stu-id="12a9c-202">The Login and Register links that are used in the template are examples of how to use the jQuery UI Dialog to present a rich login screen:</span></span>

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a><span data-ttu-id="12a9c-203">行動裝置專案範本</span><span class="sxs-lookup"><span data-stu-id="12a9c-203">Mobile Project Template</span></span>

<span data-ttu-id="12a9c-204">如果您正在新的專案，並想要建立站台，專為行動和平板電腦瀏覽器，您可以使用新的行動應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="12a9c-204">If you're starting a new project and want to create a site specifically for mobile and tablet browsers, you can use the new Mobile Application project template.</span></span> <span data-ttu-id="12a9c-205">這是以基礎的 jQuery Mobile，開放原始碼程式庫建置觸控最佳化的 UI:</span><span class="sxs-lookup"><span data-stu-id="12a9c-205">This is based on jQuery Mobile, an open-source library for building touch-optimized UI:</span></span>

![](mvc4-beta-release-notes/_static/image4.png)

<span data-ttu-id="12a9c-206">此範本包含與網際網路應用程式範本的應用程式結構相同 （而且控制器程式碼幾乎完全相同），但樣式呈現最佳效果，並能在觸控式行動裝置的行為使用 jQuery Mobile。</span><span class="sxs-lookup"><span data-stu-id="12a9c-206">This template contains the same application structure as the Internet Application template (and the controller code is virtually identical), but it's styled using jQuery Mobile to look good and behave well on touch-based mobile devices.</span></span> <span data-ttu-id="12a9c-207">若要深入了解如何建構和設定行動裝置的 UI 的樣式，請參閱[jQuery 行動專案網站](http://jquerymobile.com/)。</span><span class="sxs-lookup"><span data-stu-id="12a9c-207">To learn more about how to structure and style mobile UI, see the [jQuery Mobile project website](http://jquerymobile.com/).</span></span>

<span data-ttu-id="12a9c-208">如果您已經有的桌面導向的網站，您想要新增行動裝置最佳化的檢視，或如果您想要建立單一站台做桌面和行動瀏覽器的不同樣式化的檢視，您可以使用新的顯示模式功能。</span><span class="sxs-lookup"><span data-stu-id="12a9c-208">If you already have a desktop-oriented site that you want to add mobile-optimized views to, or if you want to create a single site that serves differently styled views to desktop and mobile browsers, you can use the new Display Modes feature.</span></span> <span data-ttu-id="12a9c-209">（請參閱下一節。）</span><span class="sxs-lookup"><span data-stu-id="12a9c-209">(See the next section.)</span></span>

<a id="_Toc303253810"></a>
### <a name="display-modes"></a><span data-ttu-id="12a9c-210">顯示模式</span><span class="sxs-lookup"><span data-stu-id="12a9c-210">Display Modes</span></span>

<span data-ttu-id="12a9c-211">新的顯示模式功能可讓應用程式選取根據提出要求的瀏覽器的檢視。</span><span class="sxs-lookup"><span data-stu-id="12a9c-211">The new Display Modes feature lets an application select views depending on the browser that's making the request.</span></span> <span data-ttu-id="12a9c-212">例如，如果桌面瀏覽器要求首頁上，應用程式可能使用 Views\Home\Index.cshtml 範本。</span><span class="sxs-lookup"><span data-stu-id="12a9c-212">For example, if a desktop browser requests the Home page, the application might use the Views\Home\Index.cshtml template.</span></span> <span data-ttu-id="12a9c-213">如果在行動瀏覽器要求首頁上，應用程式可能會傳回 Views\Home\Index.mobile.cshtml 範本。</span><span class="sxs-lookup"><span data-stu-id="12a9c-213">If a mobile browser requests the Home page, the application might return the Views\Home\Index.mobile.cshtml template.</span></span>

<span data-ttu-id="12a9c-214">版面配置和部分可以也覆寫特定瀏覽器類型。</span><span class="sxs-lookup"><span data-stu-id="12a9c-214">Layouts and partials can also be overridden for particular browser types.</span></span> <span data-ttu-id="12a9c-215">例如: </span><span class="sxs-lookup"><span data-stu-id="12a9c-215">For example:</span></span>

- <span data-ttu-id="12a9c-216">如果您的 Views\Shared 資料夾同時包含\_Layout.cshtml 和\_Layout.mobile.cshtml 範本，根據預設，應用程式會使用\_Layout.mobile.cshtml與行動瀏覽器要求期間\_Layout.cshtml 期間其他要求。</span><span class="sxs-lookup"><span data-stu-id="12a9c-216">If your Views\Shared folder contains both the \_Layout.cshtml and \_Layout.mobile.cshtml templates, by default the application will use \_Layout.mobile.cshtml during requests from mobile browsers and \_Layout.cshtml during other requests.</span></span>
- <span data-ttu-id="12a9c-217">如果資料夾同時包含\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指示@Html.Partial(「\_MyPartial") 會轉譯\_MyPartial.mobile.cshtml 期間從行動裝置的要求瀏覽器和\_MyPartial.cshtml 期間其他要求。</span><span class="sxs-lookup"><span data-stu-id="12a9c-217">If a folder contains both \_MyPartial.cshtml and \_MyPartial.mobile.cshtml, the instruction @Html.Partial("\_MyPartial") will render \_MyPartial.mobile.cshtml during requests from mobile browsers, and \_MyPartial.cshtml during other requests.</span></span>

<span data-ttu-id="12a9c-218">如果您想要建立更具體的檢視表、 版面配置或針對其他裝置的部分檢視，您可以註冊新*DefaultDisplayMode*指定的名稱： 要求符合特定條件時，要搜尋的執行個體。</span><span class="sxs-lookup"><span data-stu-id="12a9c-218">If you want to create more specific views, layouts, or partial views for other devices, you can register a new *DefaultDisplayMode* instance to specify which name to search for when a request satisfies particular conditions.</span></span> <span data-ttu-id="12a9c-219">例如，您可以在其中新增下列程式碼*應用程式\_啟動*註冊"iPhone"字串做為適用於 Apple iPhone 瀏覽器提出要求時的顯示模式的 Global.asax 檔案中的方法：</span><span class="sxs-lookup"><span data-stu-id="12a9c-219">For example, you could add the following code to the *Application\_Start* method in the Global.asax file to register the string "iPhone" as a display mode that applies when the Apple iPhone browser makes a request:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

<span data-ttu-id="12a9c-220">執行此程式碼，當 Apple iPhone 瀏覽器要求之後，您的應用程式將使用 Views\Shared\\_Layout.iPhone.cshtml 版面配置 （若有的話）。</span><span class="sxs-lookup"><span data-stu-id="12a9c-220">After this code runs, when an Apple iPhone browser makes a request, your application will use the Views\Shared\\_Layout.iPhone.cshtml layout (if it exists).</span></span>

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a><span data-ttu-id="12a9c-221">jQuery Mobile，檢視切換器，以及瀏覽器覆寫</span><span class="sxs-lookup"><span data-stu-id="12a9c-221">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>

<span data-ttu-id="12a9c-222">jQuery Mobile 是開放原始碼程式庫建置觸控最佳化的 web UI。</span><span class="sxs-lookup"><span data-stu-id="12a9c-222">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="12a9c-223">如果您想要與 ASP.NET MVC 4 應用程式中使用 jQuery Mobile，您可以下載並安裝 NuGet 套件，可協助您開始著手。</span><span class="sxs-lookup"><span data-stu-id="12a9c-223">If you want to use jQuery Mobile with an ASP.NET MVC 4 application, you can download and install a NuGet package that helps you get started.</span></span> <span data-ttu-id="12a9c-224">若要安裝它從 Visual Studio Package Manager 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="12a9c-224">To install it from the Visual Studio Package Manager Console, type the following command:</span></span>

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

<span data-ttu-id="12a9c-225">這會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：</span><span class="sxs-lookup"><span data-stu-id="12a9c-225">This installs jQuery Mobile and some helper files, including the following:</span></span>

- <span data-ttu-id="12a9c-226">Views/Shared/\_Layout.Mobile.cshtml，這是 jQuery mobile 的版面配置。</span><span class="sxs-lookup"><span data-stu-id="12a9c-226">Views/Shared/\_Layout.Mobile.cshtml, which is a jQuery Mobile-based layout.</span></span>
- <span data-ttu-id="12a9c-227">檢視切換器元件，其中包含Views/Shared/\_ViewSwitcher.cshtml 部分檢視和 ViewSwitcherController.cs 控制站。</span><span class="sxs-lookup"><span data-stu-id="12a9c-227">A view-switcher component, which consists of the Views/Shared/\_ViewSwitcher.cshtml partial view and the ViewSwitcherController.cs controller.</span></span>

<span data-ttu-id="12a9c-228">在安裝套件之後，執行您的應用程式使用行動瀏覽器 (或同等權限，例如 Firefox[使用者代理程式切換器](http://chrispederick.com/work/user-agent-switcher/)附加元件)。</span><span class="sxs-lookup"><span data-stu-id="12a9c-228">After you install the package, run your application using a mobile browser (or equivalent, like the Firefox [User Agent Switcher](http://chrispederick.com/work/user-agent-switcher/) add-on).</span></span> <span data-ttu-id="12a9c-229">您會看到，頁面看起來相當不同，因為 jQuery Mobile 處理配置和樣式設定。</span><span class="sxs-lookup"><span data-stu-id="12a9c-229">You'll see that your pages look quite different, because jQuery Mobile handles layout and styling.</span></span> <span data-ttu-id="12a9c-230">若要利用這一點，您可以執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="12a9c-230">To take advantage of this, you can do the following:</span></span>

- <span data-ttu-id="12a9c-231">建立行動專用檢視覆寫，如底下所述[顯示模式](#_Toc303253810)之前 （例如，建立覆寫行動瀏覽器 Views\Home\Index.cshtml Views\Home\Index.mobile.cshtml）。</span><span class="sxs-lookup"><span data-stu-id="12a9c-231">Create mobile-specific view overrides as described under [Display Modes](#_Toc303253810) earlier (for example, create Views\Home\Index.mobile.cshtml to override Views\Home\Index.cshtml for mobile browsers).</span></span>
- <span data-ttu-id="12a9c-232">讀取[jQuery Mobile 文件](http://jquerymobile.com/)若要深入了解如何將觸控最佳化的 UI 項目加入行動檢視。</span><span class="sxs-lookup"><span data-stu-id="12a9c-232">Read the [jQuery Mobile documentation](http://jquerymobile.com/) to learn more about how to add touch-optimized UI elements in mobile views.</span></span>

<span data-ttu-id="12a9c-233">最適合行動裝置的網頁通常會加入其文字是像桌面檢視] 或 [完整的網站模式可讓使用者切換到桌面版本的頁面連結。</span><span class="sxs-lookup"><span data-stu-id="12a9c-233">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="12a9c-234">JQuery.Mobile.MVC 套件包含針對此目的的範例檢視切換器元件。</span><span class="sxs-lookup"><span data-stu-id="12a9c-234">The jQuery.Mobile.MVC package includes a sample view-switcher component for this purpose.</span></span> <span data-ttu-id="12a9c-235">它會在預設 Views\Shared\\_Layout.Mobile.cshtml 檢視中，並在呈現的頁面時，其外觀如下：</span><span class="sxs-lookup"><span data-stu-id="12a9c-235">It's used in the default Views\Shared\\_Layout.Mobile.cshtml view, and it looks like this when the page is rendered:</span></span>

![](mvc4-beta-release-notes/_static/image5.png)

<span data-ttu-id="12a9c-236">如果造訪者按一下連結時，它們被切換到桌面版本的同一個頁面。</span><span class="sxs-lookup"><span data-stu-id="12a9c-236">If visitors click the link, they're switched to the desktop version of the same page.</span></span>

<span data-ttu-id="12a9c-237">因為您桌面的配置不會包含檢視切換器，根據預設，訪客不會有行動模式取得的方法。</span><span class="sxs-lookup"><span data-stu-id="12a9c-237">Because your desktop layout will not include a view switcher by default, visitors won't have a way to get to mobile mode.</span></span> <span data-ttu-id="12a9c-238">若要啟用此功能，加入下列參考 *\_ViewSwitcher*至您的桌面配置，只是內部*&lt;主體&gt;* 項目：</span><span class="sxs-lookup"><span data-stu-id="12a9c-238">To enable this, add the following reference to *\_ViewSwitcher* to your desktop layout, just inside the *&lt;body&gt;* element:</span></span>

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

<span data-ttu-id="12a9c-239">檢視切換程式會使用稱為 瀏覽器覆寫的新功能。</span><span class="sxs-lookup"><span data-stu-id="12a9c-239">The view switcher uses a new feature called Browser Overriding.</span></span> <span data-ttu-id="12a9c-240">這項功能可讓您的應用程式處理要求，如同它們即將從不同的瀏覽器 （使用者代理程式） 與它們實際從。</span><span class="sxs-lookup"><span data-stu-id="12a9c-240">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they're actually from.</span></span> <span data-ttu-id="12a9c-241">下表列出可在瀏覽器覆寫的方法。</span><span class="sxs-lookup"><span data-stu-id="12a9c-241">The following table lists the methods that Browser Overriding provides.</span></span>

| `HttpContext.SetOverriddenBrowser(userAgentString)` | <span data-ttu-id="12a9c-242">使用指定的使用者代理程式要求的實際使用者代理程式 」 值會覆寫。</span><span class="sxs-lookup"><span data-stu-id="12a9c-242">Overrides the request's actual user agent value using the specified user agent.</span></span> |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | <span data-ttu-id="12a9c-243">如果已不指定任何覆寫，會傳回要求的使用者代理程式覆寫值或實際使用者代理字串。</span><span class="sxs-lookup"><span data-stu-id="12a9c-243">Returns the request's user agent override value, or the actual user agent string if no override has been specified.</span></span> |
| `HttpContext.GetOverriddenBrowser()` | <span data-ttu-id="12a9c-244">傳回*HttpBrowserCapabilitiesBase*對應至目前設定為要求的使用者代理程式的執行個體 （實際或覆寫）。</span><span class="sxs-lookup"><span data-stu-id="12a9c-244">Returns an *HttpBrowserCapabilitiesBase* instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="12a9c-245">您可以使用此值，取得屬性，例如*IsMobileDevice*。</span><span class="sxs-lookup"><span data-stu-id="12a9c-245">You can use this value to get properties such as *IsMobileDevice*.</span></span> |
| `HttpContext.ClearOverriddenBrowser()` | <span data-ttu-id="12a9c-246">移除目前要求的任何覆寫的使用者代理程式。</span><span class="sxs-lookup"><span data-stu-id="12a9c-246">Removes any overridden user agent for the current request.</span></span> |

<span data-ttu-id="12a9c-247">瀏覽器覆寫是 ASP.NET MVC 4 的核心功能，並為可用，即使您未安裝 jQuery.Mobile.MVC 封裝。</span><span class="sxs-lookup"><span data-stu-id="12a9c-247">Browser Overriding is a core feature of ASP.NET MVC 4 and is available even if you don't install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="12a9c-248">不過，它會影響檢視、 配置和部分檢視的選取範圍，它不會影響其他任何 ASP.NET 功能，取決於*Request.Browser*物件。</span><span class="sxs-lookup"><span data-stu-id="12a9c-248">However, it affects only view, layout, and partial-view selection — it does not affect any other ASP.NET feature that depends on the *Request.Browser* object.</span></span>

<span data-ttu-id="12a9c-249">根據預設，使用者代理程式覆寫會使用 cookie 來儲存。</span><span class="sxs-lookup"><span data-stu-id="12a9c-249">By default, the user-agent override is stored using a cookie.</span></span> <span data-ttu-id="12a9c-250">如果您想要存放覆寫其他地方 （例如，在資料庫中），您可以取代預設的提供者 (*BrowserOverrideStores.Current*)。</span><span class="sxs-lookup"><span data-stu-id="12a9c-250">If you want to store the override elsewhere (for example, in a database), you can replace the default provider (*BrowserOverrideStores.Current*).</span></span> <span data-ttu-id="12a9c-251">此提供者的文件會隨附了一個版本的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="12a9c-251">Documentation for this provider will be available to accompany a later release of ASP.NET MVC.</span></span>

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a><span data-ttu-id="12a9c-252">Visual Studio 中的程式碼產生的訣竅</span><span class="sxs-lookup"><span data-stu-id="12a9c-252">Recipes for Code Generation in Visual Studio</span></span>

<span data-ttu-id="12a9c-253">新的 「 配方 」 功能可讓 Visual Studio 來產生您可以使用 NuGet 來安裝的套件為基礎的解決方案特定程式碼。</span><span class="sxs-lookup"><span data-stu-id="12a9c-253">The new Recipes feature enables Visual Studio to generate solution-specific code based on packages that you can install using NuGet.</span></span> <span data-ttu-id="12a9c-254">配方 framework 可方便開發人員撰寫程式碼產生外掛程式，您也可以使用新增區域、 新增控制器及 新增檢視取代內建的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="12a9c-254">The Recipes framework makes it easy for developers to write code-generation plugins, which you can also use to replace the built-in code generators for Add Area, Add Controller, and Add View.</span></span> <span data-ttu-id="12a9c-255">因為配方會部署為 NuGet 套件時，可以輕鬆地簽入原始檔控制並自動共用的專案中的所有開發人員。</span><span class="sxs-lookup"><span data-stu-id="12a9c-255">Because recipes are deployed as NuGet packages, they can easily be checked into source control and shared with all developers on the project automatically.</span></span> <span data-ttu-id="12a9c-256">它們也有每個方案為基礎。</span><span class="sxs-lookup"><span data-stu-id="12a9c-256">They are also available on a per-solution basis.</span></span>

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a><span data-ttu-id="12a9c-257">非同步控制器的的工作支援</span><span class="sxs-lookup"><span data-stu-id="12a9c-257">Task Support for Asynchronous Controllers</span></span>

<span data-ttu-id="12a9c-258">您現在可以撰寫非同步動作方法傳回的型別物件的單一方法*任務*或是*工作&lt;ActionResult&gt;*。</span><span class="sxs-lookup"><span data-stu-id="12a9c-258">You can now write asynchronous action methods as single methods that return an object of type *Task* or *Task&lt;ActionResult&gt;*.</span></span>

<span data-ttu-id="12a9c-259">例如，如果您使用 Visual C# 5 (或使用[Async CTP](https://msdn.microsoft.com/vstudio/async.aspx))，您可以建立非同步動作方法看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="12a9c-259">For example, if you're using Visual C# 5 (or using the [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), you can create an asynchronous action method that looks like the following:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

<span data-ttu-id="12a9c-260">在上一個動作方法中，呼叫*newsService.GetHeadlinesAsync*並*sportsService.GetScoresAsync*非同步呼叫，而不會封鎖執行緒集區的執行緒。</span><span class="sxs-lookup"><span data-stu-id="12a9c-260">In the previous action method, the calls to *newsService.GetHeadlinesAsync* and *sportsService.GetScoresAsync* are called asynchronously and do not block a thread from the thread pool.</span></span>

<span data-ttu-id="12a9c-261">非同步動作方法會傳回*任務*執行個體也可以支援逾時。</span><span class="sxs-lookup"><span data-stu-id="12a9c-261">Asynchronous action methods that return *Task* instances can also support timeouts.</span></span> <span data-ttu-id="12a9c-262">若要讓您的動作方法可取消，新增類型的參數*CancellationToken*至動作方法簽章。</span><span class="sxs-lookup"><span data-stu-id="12a9c-262">To make your action method cancellable, add a parameter of type *CancellationToken* to the action method signature.</span></span> <span data-ttu-id="12a9c-263">下列範例示範非同步動作方法具有逾時為 2500年毫秒，且會顯示*TimedOut*檢視用戶端，如果發生逾時。</span><span class="sxs-lookup"><span data-stu-id="12a9c-263">The following example shows an asynchronous action method that has a timeout of 2500 milliseconds and that displays a *TimedOut* view to the client if a timeout occurs.</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a><span data-ttu-id="12a9c-264">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="12a9c-264">Azure SDK</span></span>

<span data-ttu-id="12a9c-265">ASP.NET MVC 4 Beta 支援年 9 月 2011 1.5 版的 Windows Azure SDK。</span><span class="sxs-lookup"><span data-stu-id="12a9c-265">ASP.NET MVC 4 Beta supports the September 2011 1.5 release of the Windows Azure SDK.</span></span>

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="12a9c-266">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="12a9c-266">Known Issues and Breaking Changes</span></span>

- <span data-ttu-id="12a9c-267">**安裝 ASP.NET MVC 4 Beta 之後, CSHTML/VBHTML 編輯器，在 Visual Studio 2010 Service Pack 1 CSHTML/VBHTML 編輯器可能會暫停很長的時間之後輸入 cshtml 」 或 「 vbhtml 檔案內的程式碼片段或 JavaScript。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-267">**After installing ASP.NET MVC 4 Beta, the CSHTML/VBHTML editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML editor may pause for a long time after typing snippet or JavaScript inside cshtml or vbhtml files.**</span></span> <span data-ttu-id="12a9c-268">這只會發生在具有剛剛建立，而且尚未經過編譯的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="12a9c-268">This occurs only in ASP.NET MVC 4 applications which have just been created and have not yet been compiled.</span></span>

    <span data-ttu-id="12a9c-269">因應措施是編譯專案，以取得組件的 bin 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="12a9c-269">The workaround is to compile the project to get the assemblies in the bin folder.</span></span> <span data-ttu-id="12a9c-270">請注意，是否您清除 [bin] 資料夾中移除組件的專案時，會回復編輯器問題。</span><span class="sxs-lookup"><span data-stu-id="12a9c-270">Note, if you clean the project which removes the assemblies from the bin folder, the editor problem will come back.</span></span>

    <span data-ttu-id="12a9c-271">這將會在下一個版本中更正。</span><span class="sxs-lookup"><span data-stu-id="12a9c-271">This will be corrected in the next release.</span></span>
- <span data-ttu-id="12a9c-272">**Visual Studio 11 beta 版的 C# 專案範本包含 Global.asax.cs 中不正確的連接字串。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-272">**C# Project templates for Visual Studio 11 Beta contain an incorrect connection string in Global.asax.cs.**</span></span> <span data-ttu-id="12a9c-273">指定應用程式中的預設連接\_Start 方法，在 Visual Studio 11 Beta 中建立的專案包含 LocalDB 連接字串，其中包含未逸出反斜線 (\)字元。</span><span class="sxs-lookup"><span data-stu-id="12a9c-273">The default connection specified in the Application\_Start method for projects created in Visual Studio 11 Beta contain a LocalDB connection string which contains an unescaped backslash (\) character.</span></span> <span data-ttu-id="12a9c-274">這會導致連線錯誤時嘗試存取 Entity Framework DbContext，它會產生 SqlException。</span><span class="sxs-lookup"><span data-stu-id="12a9c-274">This results in a connection error upon attempts to access a Entity Framework DbContext, which generates a SqlException.</span></span>

    <span data-ttu-id="12a9c-275">若要修正此問題，逸出反斜線字元在應用程式\_Start Global.asax.cs 的方法，使其內容，如下所示：</span><span class="sxs-lookup"><span data-stu-id="12a9c-275">To correct this issue, escape the backslash character in the App\_Start method of Global.asax.cs so that it reads as follows:</span></span>

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- <span data-ttu-id="12a9c-276">**.NET 4.5 為目標的 ASP.NET MVC 4 應用程式將會擲回 FileLoadException 時嘗試存取 System.Net.Http.dll 組件時執行.NET 4.0。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-276">**ASP.NET MVC 4 applications which target .NET 4.5 will throw a FileLoadException upon attempt to access the System.Net.Http.dll assembly when run under .NET 4.0.**</span></span> <span data-ttu-id="12a9c-277">ASP.NET MVC 4 應用程式在.NET 4.5 之下，建立包含繫結重新導向，將會導致 FileLoadException，指出 「 無法載入檔案或組件 'System.Net.Http' 或其中一個相依性。 」</span><span class="sxs-lookup"><span data-stu-id="12a9c-277">ASP.NET MVC 4 applications created under .NET 4.5 contain a binding redirect that will result in a FileLoadException which that states "Could not load file or assembly 'System.Net.Http' or one of its dependencies."</span></span> <span data-ttu-id="12a9c-278">應用程式執行時的系統上安裝.NET 4.0。</span><span class="sxs-lookup"><span data-stu-id="12a9c-278">when the application is executed on a system with .NET 4.0 installed.</span></span> <span data-ttu-id="12a9c-279">若要修正此問題，請將下列繫結重新導向移除 web.config:</span><span class="sxs-lookup"><span data-stu-id="12a9c-279">To correct this issue, remove the following binding redirect from web.config:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    <span data-ttu-id="12a9c-280">在已修改的 web.config 中的組件繫結項目應如下所示：</span><span class="sxs-lookup"><span data-stu-id="12a9c-280">The assembly binding element in the modified web.config should appear as follows:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <span data-ttu-id="12a9c-281"><strong>Visual Basic 專案中的 「 新增控制器 」 項目範本會產生不正確的命名空間時叫用</strong><strong>從區域中。</strong></span><span class="sxs-lookup"><span data-stu-id="12a9c-281"><strong>The "Add Controller" item template in Visual Basic projects generates an incorrect namespace when invoked</strong><strong>from inside an area.</strong></span></span> <span data-ttu-id="12a9c-282">當您將控制器加入 ASP.NET MVC 專案會使用 Visual Basic 中的區域時，項目範本會插入控制器的命名空間錯誤。</span><span class="sxs-lookup"><span data-stu-id="12a9c-282">When you add a controller to an area in an ASP.NET MVC project that uses Visual Basic, the item template inserts the wrong namespace into the controller.</span></span> <span data-ttu-id="12a9c-283">當您瀏覽至控制器中的任何動作時，結果會是 「 找不到檔案 」 錯誤。</span><span class="sxs-lookup"><span data-stu-id="12a9c-283">The result is a "file not found" error when you navigate to any action in the controller.</span></span>  
  
  <span data-ttu-id="12a9c-284">產生的命名空間會省略所有項目之後的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="12a9c-284">The generated namespace omits everything after the root namespace.</span></span> <span data-ttu-id="12a9c-285">比方說，產生的命名空間是*RootNamespace*但應該*RootNamespace.Areas.AreaName.Controllers* 。</span><span class="sxs-lookup"><span data-stu-id="12a9c-285">For example, the namespace generated is *RootNamespace* but should be *RootNamespace.Areas.AreaName.Controllers* .</span></span>
- <span data-ttu-id="12a9c-286">**Razor 檢視引擎的重大變更。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-286">**Breaking changes in the Razor View Engine.**</span></span> <span data-ttu-id="12a9c-287">隨著重寫 Razor 剖析器的詳細資訊，下列類型移除了*System.Web.Mvc.Razor*:</span><span class="sxs-lookup"><span data-stu-id="12a9c-287">As part of a rewrite of the Razor parser, the following types were removed from *System.Web.Mvc.Razor*:</span></span> 

    - <span data-ttu-id="12a9c-288">*ModelSpan*</span><span class="sxs-lookup"><span data-stu-id="12a9c-288">*ModelSpan*</span></span>
    - <span data-ttu-id="12a9c-289">*MvcVBRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="12a9c-289">*MvcVBRazorCodeGenerator*</span></span>
    - <span data-ttu-id="12a9c-290">*MvcCSharpRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="12a9c-290">*MvcCSharpRazorCodeGenerator*</span></span>
    - <span data-ttu-id="12a9c-291">*MvcVBRazorCodeParser*</span><span class="sxs-lookup"><span data-stu-id="12a9c-291">*MvcVBRazorCodeParser*</span></span>

  <span data-ttu-id="12a9c-292">也已移除下列方法：</span><span class="sxs-lookup"><span data-stu-id="12a9c-292">The following methods were also removed:</span></span> 

    - <span data-ttu-id="12a9c-293">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="12a9c-293">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
    - <span data-ttu-id="12a9c-294">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span><span class="sxs-lookup"><span data-stu-id="12a9c-294">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span></span>
    - <span data-ttu-id="12a9c-295">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="12a9c-295">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
- <span data-ttu-id="12a9c-296">**當 WebMatrix.WebData.dll 包含 ASP.NET MVC 4 應用程式的 /bin 目錄中時，它會高於表單驗證的 URL。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-296">**When WebMatrix.WebData.dll is included in the /bin directory of an ASP.NET MVC 4 apps, it takes over the URL for forms authentication.**</span></span> <span data-ttu-id="12a9c-297">（例如，藉由使用 [新增可部署的相依性] 對話方塊時，請選取 「 ASP.NET 網頁使用 Razor 語法 」） 時，將 WebMatrix.WebData.dll 組件新增至您的應用程式將會覆寫/帳戶/登入來驗證登入重新導向而 /帳戶/登入如預期般預設 ASP.NET MVC 帳戶控制器。</span><span class="sxs-lookup"><span data-stu-id="12a9c-297">Adding the WebMatrix.WebData.dll assembly to your application (for example, by selecting "ASP.NET Web Pages with Razor Syntax" when using the Add Deployable Dependencies dialog) will override the authentication login redirect to /account/logon rather than /account/login as expected by the default ASP.NET MVC Account Controller.</span></span> <span data-ttu-id="12a9c-298">若要避免這種行為，並使用指定的 URL 已經在 web.config 的 [驗證] 區段中，您可以加入稱為 PreserveLoginUrl appSetting，並將它設定為 true:</span><span class="sxs-lookup"><span data-stu-id="12a9c-298">To prevent this behavior and use the URL specified already in the authentication section of web.config, you can add an appSetting called PreserveLoginUrl and set it to true:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- <span data-ttu-id="12a9c-299">**NuGet 套件管理員安裝嘗試安裝 ASP.NET MVC 4 的 Visual Studio 2010 和 Visual Web Developer 2010 的並存安裝時失敗。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-299">**The NuGet package manager fails to install when attempting to install ASP.NET MVC 4 for side by side installations of Visual Studio 2010 and Visual Web Developer 2010.**</span></span> <span data-ttu-id="12a9c-300">若要執行 Visual Studio 2010 和 Visual Web Developer 2010 與 ASP.NET MVC 4 中，您必須安裝 ASP.NET MVC 4 之後已安裝這兩個版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="12a9c-300">To run Visual Studio 2010 and Visual Web Developer 2010 side by side with ASP.NET MVC 4 you must install ASP.NET MVC 4 after both versions of Visual Studio have already been installed.</span></span>
- <span data-ttu-id="12a9c-301">**如果已解除安裝必要條件，解除安裝 ASP.NET MVC 4 就會失敗。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-301">**Uninstalling ASP.NET MVC 4 fails if prerequisites have already been uninstalled.**</span></span> <span data-ttu-id="12a9c-302">若要完全解除安裝 ASP.NET MVC 4you 必須解除安裝 ASP.NET MVC 4，然後再解除安裝 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="12a9c-302">To cleanly uninstall ASP.NET MVC 4you must uninstall ASP.NET MVC 4 prior to uninstalling Visual Studio.</span></span>
- <span data-ttu-id="12a9c-303">**執行預設的 Web API 專案會顯示不正確地將使用者導向至新增路由使用 RegisterApis 方法不存在的指示。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-303">**Running a default Web API project shows instructions that incorrectly direct the user to add routes using the RegisterApis method, which doesn't exist.**</span></span> <span data-ttu-id="12a9c-304">使用 ASP.NET 路由表 RegisterRoutes 方法中應該新增路由。</span><span class="sxs-lookup"><span data-stu-id="12a9c-304">Routes should be added in the RegisterRoutes method using the ASP.NET route table.</span></span>
- <span data-ttu-id="12a9c-305">**安裝 ASP.NET MVC 4 Beta 中斷 ASP.NET MVC 3 RTM 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="12a9c-305">**Installing ASP.NET MVC 4 Beta breaks ASP.NET MVC 3 RTM applications.**</span></span> <span data-ttu-id="12a9c-306">ASP.NET MVC 3 應用程式所建立的 rtm 版本 （不含 ASP.NET MVC 3 Tools Update 版本） 必須執行下列變更才能使用 ASP.NET MVC 4 Beta 並存。</span><span class="sxs-lookup"><span data-stu-id="12a9c-306">ASP.NET MVC 3 applications that were created with the RTM release (not with the ASP.NET MVC 3 Tools Update release) require the following changes in order to work side-by-side with ASP.NET MVC 4 Beta.</span></span> <span data-ttu-id="12a9c-307">建置專案，而不需要進行這些更新結果中的編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="12a9c-307">Building the project without making these updates results in compilation errors.</span></span> 

    <span data-ttu-id="12a9c-308">**必要的更新**</span><span class="sxs-lookup"><span data-stu-id="12a9c-308">**Required updates**</span></span>

  1. <span data-ttu-id="12a9c-309">在根 Web.config 檔案中，加入新*&lt;appSettings&gt;* 與索引鍵的項目*webPages:Version* ，而*1.0.0.0*。</span><span class="sxs-lookup"><span data-stu-id="12a9c-309">In the root Web.config file, add a new *&lt;appSettings&gt;* entry with the key *webPages:Version* and the value *1.0.0.0*.</span></span>

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. <span data-ttu-id="12a9c-310">在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案</span><span class="sxs-lookup"><span data-stu-id="12a9c-310">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="12a9c-311">然後再次以滑鼠右鍵按一下名稱，然後選取 編輯*ProjectName*.csproj。</span><span class="sxs-lookup"><span data-stu-id="12a9c-311">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
  3. <span data-ttu-id="12a9c-312">找出下列組件參考：</span><span class="sxs-lookup"><span data-stu-id="12a9c-312">Locate the following assembly references:</span></span> 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      <span data-ttu-id="12a9c-313">您可以將它們取代為下列：</span><span class="sxs-lookup"><span data-stu-id="12a9c-313">Replace them with the following:</span></span>

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. <span data-ttu-id="12a9c-314">儲存變更，請關閉您所編輯，然後以滑鼠右鍵按一下專案並選取 重新載入專案 (.csproj) 檔案。</span><span class="sxs-lookup"><span data-stu-id="12a9c-314">Save the changes, close the project (.csproj) file you were editing, and then right-click the project and select Reload.</span></span>
