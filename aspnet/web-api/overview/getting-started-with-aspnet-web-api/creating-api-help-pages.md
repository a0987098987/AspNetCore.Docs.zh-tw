---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "建立 ASP.NET Web API 說明頁面 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="df11c-102">建立 ASP.NET Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="df11c-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="df11c-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="df11c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="df11c-104">當您建立 web 應用程式開發介面時，它通常是用於建立一份說明網頁，，以便在其他開發人員知道如何呼叫您的 API。</span><span class="sxs-lookup"><span data-stu-id="df11c-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="df11c-105">您可以手動建立所有的文件，但最好是盡可能自動產生。</span><span class="sxs-lookup"><span data-stu-id="df11c-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="df11c-106">若要簡化這項工作中，ASP.NET Web API 會提供文件庫在執行階段自動產生說明頁面。</span><span class="sxs-lookup"><span data-stu-id="df11c-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="df11c-107">建立 API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="df11c-107">Creating API Help Pages</span></span>

<span data-ttu-id="df11c-108">安裝[ASP.NET 及 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。</span><span class="sxs-lookup"><span data-stu-id="df11c-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="df11c-109">此更新會整合至 Web API 專案範本的 [說明] 頁。</span><span class="sxs-lookup"><span data-stu-id="df11c-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="df11c-110">接下來，建立新的 ASP.NET MVC 4 專案，然後選取 Web API 專案範本。</span><span class="sxs-lookup"><span data-stu-id="df11c-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="df11c-111">專案範本會建立名為範例應用程式開發介面控制站`ValuesController`。</span><span class="sxs-lookup"><span data-stu-id="df11c-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="df11c-112">範本也會建立應用程式開發介面的 [說明] 頁。</span><span class="sxs-lookup"><span data-stu-id="df11c-112">The template also creates the API help pages.</span></span> <span data-ttu-id="df11c-113">所有 [說明] 頁面的程式碼檔案會放置在專案的 [區域] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="df11c-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="df11c-114">當您執行應用程式時，[首頁] 頁面包含 API 說明頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="df11c-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="df11c-115">在首頁上，從相對路徑會是 /Help。</span><span class="sxs-lookup"><span data-stu-id="df11c-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="df11c-116">此連結會帶您到 API 摘要頁面。</span><span class="sxs-lookup"><span data-stu-id="df11c-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="df11c-117">此頁面的 MVC 檢視 Areas/HelpPage/Views/Help/Index.cshtml 中定義。</span><span class="sxs-lookup"><span data-stu-id="df11c-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="df11c-118">您可以編輯這個頁面，即可修改版面配置、 簡介、 標題、 樣式和其他等等。</span><span class="sxs-lookup"><span data-stu-id="df11c-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="df11c-119">[] 頁面上的主要部分是應用程式開發介面，控制站所分組的資料表。</span><span class="sxs-lookup"><span data-stu-id="df11c-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="df11c-120">資料表項目使用，以動態方式產生**IApiExplorer**介面。</span><span class="sxs-lookup"><span data-stu-id="df11c-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="df11c-121">（我會詳細討論此介面更新版本。）如果您加入新的 API 控制器時，在執行階段時，會自動更新資料表。</span><span class="sxs-lookup"><span data-stu-id="df11c-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="df11c-122">「 應用程式開發介面 」 資料行列出的 HTTP 方法和相對 URI。</span><span class="sxs-lookup"><span data-stu-id="df11c-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="df11c-123">「 描述 」 資料行包含每個應用程式開發介面文件。</span><span class="sxs-lookup"><span data-stu-id="df11c-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="df11c-124">一開始，文件是只預留位置文字。</span><span class="sxs-lookup"><span data-stu-id="df11c-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="df11c-125">在下一步 區段中，我將為您示範如何從 XML 註解加入文件。</span><span class="sxs-lookup"><span data-stu-id="df11c-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="df11c-126">每個應用程式開發介面有更詳細的資訊，包括範例要求和回應主體與頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="df11c-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="df11c-127">將 [說明] 頁新增至現有的專案</span><span class="sxs-lookup"><span data-stu-id="df11c-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="df11c-128">您可以加入至現有的 Web API 專案的 [說明] 頁使用 NuGet 封裝管理員。</span><span class="sxs-lookup"><span data-stu-id="df11c-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="df11c-129">這個選項適合您從比 「 Web API 」 範本不同的專案範本開始。</span><span class="sxs-lookup"><span data-stu-id="df11c-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="df11c-130">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="df11c-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="df11c-131">在[Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)視窗中，輸入下列命令的其中一個：</span><span class="sxs-lookup"><span data-stu-id="df11c-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="df11c-132">如**C#**應用程式：`Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="df11c-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="df11c-133">如**Visual Basic**應用程式：`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="df11c-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="df11c-134">有兩個封裝，另一個適用於 C# 和 Visual basic 中的一個。</span><span class="sxs-lookup"><span data-stu-id="df11c-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="df11c-135">請務必使用符合您的專案。</span><span class="sxs-lookup"><span data-stu-id="df11c-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="df11c-136">此命令會安裝必要的組件，並新增 （位於 區域/HelpPage 資料夾） 的 說明 頁的 MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="df11c-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="df11c-137">您必須手動將連結加入至說明頁面。</span><span class="sxs-lookup"><span data-stu-id="df11c-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="df11c-138">URI 是 /Help。</span><span class="sxs-lookup"><span data-stu-id="df11c-138">The URI is /Help.</span></span> <span data-ttu-id="df11c-139">若要建立 razor 檢視的連結，加入下列內容：</span><span class="sxs-lookup"><span data-stu-id="df11c-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="df11c-140">此外，請確定註冊區域。</span><span class="sxs-lookup"><span data-stu-id="df11c-140">Also, make sure to register areas.</span></span> <span data-ttu-id="df11c-141">在 Global.asax 檔案中，加入下列程式碼**應用程式\_啟動**方法，如果它尚未有：</span><span class="sxs-lookup"><span data-stu-id="df11c-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="df11c-142">加入應用程式開發介面文件</span><span class="sxs-lookup"><span data-stu-id="df11c-142">Adding API Documentation</span></span>

<span data-ttu-id="df11c-143">根據預設，說明 頁面會提供文件集的預留位置字串。</span><span class="sxs-lookup"><span data-stu-id="df11c-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="df11c-144">您可以使用[XML 文件註解](https://msdn.microsoft.com/library/b2s063f7.aspx)建立文件。</span><span class="sxs-lookup"><span data-stu-id="df11c-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="df11c-145">若要啟用此功能，開啟檔案的 應用程式的區域/HelpPage\_Start/HelpPageConfig.cs 並取消註解下列一行：</span><span class="sxs-lookup"><span data-stu-id="df11c-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="df11c-146">現在可讓 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="df11c-146">Now enable XML documentation.</span></span> <span data-ttu-id="df11c-147">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="df11c-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="df11c-148">選取**建置**頁面。</span><span class="sxs-lookup"><span data-stu-id="df11c-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="df11c-149">在下**輸出**，檢查**XML 文件檔**。</span><span class="sxs-lookup"><span data-stu-id="df11c-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="df11c-150">在編輯方塊中，輸入 「 應用程式\_Data/XmlDocument.xml"。</span><span class="sxs-lookup"><span data-stu-id="df11c-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="df11c-151">接下來，開啟的程式碼`ValuesController`/Controllers/ValuesControler.cs 中定義的 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="df11c-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="df11c-152">將某些文件註解加入至控制器方法。</span><span class="sxs-lookup"><span data-stu-id="df11c-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="df11c-153">例如: </span><span class="sxs-lookup"><span data-stu-id="df11c-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="df11c-154">秘訣： 如果您插入號位置的方法上方列上，並輸入三個正斜線，Visual Studio 會自動插入 XML 項目。</span><span class="sxs-lookup"><span data-stu-id="df11c-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="df11c-155">然後您可以填入之空白個數。</span><span class="sxs-lookup"><span data-stu-id="df11c-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="df11c-156">現在建置並再次執行應用程式，以及瀏覽至 [說明] 頁。</span><span class="sxs-lookup"><span data-stu-id="df11c-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="df11c-157">文件字串應該會出現在應用程式開發介面資料表。</span><span class="sxs-lookup"><span data-stu-id="df11c-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="df11c-158">說明頁面會在執行階段，從 XML 檔案讀取字串。</span><span class="sxs-lookup"><span data-stu-id="df11c-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="df11c-159">（當您部署應用程式時，請確定部署 XML 檔案。）</span><span class="sxs-lookup"><span data-stu-id="df11c-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="df11c-160">背後原理</span><span class="sxs-lookup"><span data-stu-id="df11c-160">Under the Hood</span></span>

<span data-ttu-id="df11c-161">說明頁面會建立最上層的**ApiExplorer**類別，這是 Web API framework 的一部分。</span><span class="sxs-lookup"><span data-stu-id="df11c-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="df11c-162">**ApiExplorer**類別提供原料建立一份說明網頁。</span><span class="sxs-lookup"><span data-stu-id="df11c-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="df11c-163">每個 api， **ApiExplorer**包含**ApiDescription**描述應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="df11c-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="df11c-164">基於此目的，「 API 」 定義為 HTTP 方法和相對 URI 的組合。</span><span class="sxs-lookup"><span data-stu-id="df11c-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="df11c-165">例如，以下是一些不同的應用程式開發介面：</span><span class="sxs-lookup"><span data-stu-id="df11c-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="df11c-166">取得 /api/Products</span><span class="sxs-lookup"><span data-stu-id="df11c-166">GET /api/Products</span></span>
- <span data-ttu-id="df11c-167">取得 /api/產品 / {id}</span><span class="sxs-lookup"><span data-stu-id="df11c-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="df11c-168">張貼/api/產品</span><span class="sxs-lookup"><span data-stu-id="df11c-168">POST /api/Products</span></span>

<span data-ttu-id="df11c-169">如果控制器動作，支援多個 HTTP 方法， **ApiExplorer**視為不同的應用程式開發介面中的每個方法。</span><span class="sxs-lookup"><span data-stu-id="df11c-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="df11c-170">若要隱藏的 API **ApiExplorer**，新增**ApiExplorerSettings**屬性至動作，並將*IgnoreApi*為 true。</span><span class="sxs-lookup"><span data-stu-id="df11c-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="df11c-171">您也可以將此屬性加入控制器，以排除整個控制器中。</span><span class="sxs-lookup"><span data-stu-id="df11c-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="df11c-172">ApiExplorer 類別取得的文件字串從**IDocumentationProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="df11c-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="df11c-173">如稍早所見，說明 頁面的程式庫提供**IDocumentationProvider**來取得文件從 XML 文件的字串。</span><span class="sxs-lookup"><span data-stu-id="df11c-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="df11c-174">程式碼位於 /Areas/HelpPage/XmlDocumentationProvider.cs。</span><span class="sxs-lookup"><span data-stu-id="df11c-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="df11c-175">您可以取得文件從其他來源藉由撰寫您自己**IDocumentationProvider**。</span><span class="sxs-lookup"><span data-stu-id="df11c-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="df11c-176">若要連接它，呼叫**SetDocumentationProvider**中定義的擴充方法**HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="df11c-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="df11c-177">**ApiExplorer**自動呼叫**IDocumentationProvider**介面來取得每個 API 文件的字串。</span><span class="sxs-lookup"><span data-stu-id="df11c-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="df11c-178">它將其儲存在**文件**屬性**ApiDescription**和**ApiParameterDescription**物件。</span><span class="sxs-lookup"><span data-stu-id="df11c-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df11c-179">後續步驟</span><span class="sxs-lookup"><span data-stu-id="df11c-179">Next Steps</span></span>

<span data-ttu-id="df11c-180">您不限於如下所示的 [說明] 頁。</span><span class="sxs-lookup"><span data-stu-id="df11c-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="df11c-181">事實上， **ApiExplorer**不限於建立說明頁面。</span><span class="sxs-lookup"><span data-stu-id="df11c-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="df11c-182">一些很棒的部落格文章，讓您以為現成寫入 Yao Huang 連結：</span><span class="sxs-lookup"><span data-stu-id="df11c-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="df11c-183">將簡單的測試用戶端加入至 ASP.NET Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="df11c-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="df11c-184">進行 ASP.NET Web API 說明頁面在自我裝載服務上運作</span><span class="sxs-lookup"><span data-stu-id="df11c-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="df11c-185">設計階段產生的說明頁面 （或用戶端） 的 ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="df11c-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="df11c-186">說明頁面的進階自訂內容</span><span class="sxs-lookup"><span data-stu-id="df11c-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
