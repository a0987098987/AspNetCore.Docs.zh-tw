---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: 建立的 ASP.NET Web API 說明頁面 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: c081064a32151a71fc4f3ea407e0c48a1539432a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913121"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="98dab-102">建立的 ASP.NET Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="98dab-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="98dab-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="98dab-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="98dab-104">當您建立的 web API 時，它通常很有用來建立說明網頁，以便在其他開發人員知道如何呼叫您的 API。</span><span class="sxs-lookup"><span data-stu-id="98dab-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="98dab-105">您可以手動建立所有文件，但最好是盡量的自動產生。</span><span class="sxs-lookup"><span data-stu-id="98dab-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="98dab-106">若要簡化這項工作中，ASP.NET Web API 會在執行階段，程式庫提供自動產生說明頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="98dab-107">建立 API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="98dab-107">Creating API Help Pages</span></span>

<span data-ttu-id="98dab-108">安裝[ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。</span><span class="sxs-lookup"><span data-stu-id="98dab-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="98dab-109">此更新會整合到 Web API 專案範本的 [說明] 頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="98dab-110">接下來，建立新的 ASP.NET MVC 4 專案，然後選取 Web API 專案範本。</span><span class="sxs-lookup"><span data-stu-id="98dab-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="98dab-111">專案範本會建立名為範例 API 控制器`ValuesController`。</span><span class="sxs-lookup"><span data-stu-id="98dab-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="98dab-112">此範本也會建立 API 說明頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-112">The template also creates the API help pages.</span></span> <span data-ttu-id="98dab-113">所有 [說明] 頁面的程式碼檔案位於專案的 [區域] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="98dab-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="98dab-114">當您執行應用程式時，[首頁] 頁面會包含 API 說明頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="98dab-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="98dab-115">從 [首頁] 頁面中，相對路徑會是 /Help。</span><span class="sxs-lookup"><span data-stu-id="98dab-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="98dab-116">此連結將帶您前往 API 摘要頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="98dab-117">此頁面的 [MVC] 檢視會定義在 Areas/HelpPage/Views/Help/Index.cshtml。</span><span class="sxs-lookup"><span data-stu-id="98dab-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="98dab-118">您可以編輯此頁面修改版面配置、 簡介、 標題、 樣式和其他等等。</span><span class="sxs-lookup"><span data-stu-id="98dab-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="98dab-119">頁面的主要部分是依控制站的 api 的資料表。</span><span class="sxs-lookup"><span data-stu-id="98dab-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="98dab-120">資料表項目使用，以動態方式產生**IApiExplorer**介面。</span><span class="sxs-lookup"><span data-stu-id="98dab-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="98dab-121">（我會詳細討論有關此介面更新版本。）如果您新增新的 API 控制器，請在執行階段時，會自動更新的資料表。</span><span class="sxs-lookup"><span data-stu-id="98dab-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="98dab-122">「 API 」 資料行列出的 HTTP 方法和相對 URI。</span><span class="sxs-lookup"><span data-stu-id="98dab-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="98dab-123">「 說明 」 資料行包含每個 API 的文件。</span><span class="sxs-lookup"><span data-stu-id="98dab-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="98dab-124">一開始，文件就是只是預留位置文字。</span><span class="sxs-lookup"><span data-stu-id="98dab-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="98dab-125">下一節，我將示範如何從 XML 註解加入文件。</span><span class="sxs-lookup"><span data-stu-id="98dab-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="98dab-126">每個 API 有更詳細的資訊，包括範例要求和回應內文的頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="98dab-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="98dab-127">加入現有的專案中的 [說明] 頁面</span><span class="sxs-lookup"><span data-stu-id="98dab-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="98dab-128">您可以透過 NuGet 套件管理員，將 [說明] 頁面新增至現有的 Web API 專案中。</span><span class="sxs-lookup"><span data-stu-id="98dab-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="98dab-129">這個選項適合您開始從不同的專案範本，於 [Web API] 範本。</span><span class="sxs-lookup"><span data-stu-id="98dab-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="98dab-130">從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="98dab-130">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="98dab-131">在  [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)視窗中，輸入下列命令之一：</span><span class="sxs-lookup"><span data-stu-id="98dab-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="98dab-132">針對**C#** 應用程式： `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="98dab-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="98dab-133">針對**Visual Basic**應用程式： `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="98dab-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="98dab-134">有兩個套件，一個適用於 C#，一個適用於 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="98dab-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="98dab-135">請務必使用符合您的專案。</span><span class="sxs-lookup"><span data-stu-id="98dab-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="98dab-136">此命令會安裝必要的組件，並新增 （位於 [區域/HelpPage] 資料夾） 的 [說明] 頁的 MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="98dab-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="98dab-137">您必須手動將連結新增至 [說明] 頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="98dab-138">URI 是 /Help。</span><span class="sxs-lookup"><span data-stu-id="98dab-138">The URI is /Help.</span></span> <span data-ttu-id="98dab-139">若要建立連結，在 razor 檢視中，新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="98dab-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="98dab-140">此外，請確定註冊區域。</span><span class="sxs-lookup"><span data-stu-id="98dab-140">Also, make sure to register areas.</span></span> <span data-ttu-id="98dab-141">在 Global.asax 檔案中，新增下列程式碼**應用程式\_啟動**方法，如果找不到已：</span><span class="sxs-lookup"><span data-stu-id="98dab-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="98dab-142">新增 API 文件</span><span class="sxs-lookup"><span data-stu-id="98dab-142">Adding API Documentation</span></span>

<span data-ttu-id="98dab-143">根據預設，說明 頁面都有文件的預留位置字串。</span><span class="sxs-lookup"><span data-stu-id="98dab-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="98dab-144">您可以使用[XML 文件註解](https://msdn.microsoft.com/library/b2s063f7.aspx)建立文件。</span><span class="sxs-lookup"><span data-stu-id="98dab-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="98dab-145">若要啟用此功能，開啟 區域/HelpPage/應用程式的檔案\_Start/HelpPageConfig.cs 並取消註解下面這一行：</span><span class="sxs-lookup"><span data-stu-id="98dab-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="98dab-146">現在可讓 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="98dab-146">Now enable XML documentation.</span></span> <span data-ttu-id="98dab-147">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="98dab-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="98dab-148">選取 **建置**頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="98dab-149">底下**輸出**，檢查**XML 文件檔案**。</span><span class="sxs-lookup"><span data-stu-id="98dab-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="98dab-150">在 [編輯] 方塊中，輸入 「 應用程式\_Data/XmlDocument.xml"。</span><span class="sxs-lookup"><span data-stu-id="98dab-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="98dab-151">接下來，開啟的程式碼`ValuesController`API 控制器，其定義於 /Controllers/ValuesControler.cs。</span><span class="sxs-lookup"><span data-stu-id="98dab-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="98dab-152">控制器方法中加入一些文件註解。</span><span class="sxs-lookup"><span data-stu-id="98dab-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="98dab-153">例如: </span><span class="sxs-lookup"><span data-stu-id="98dab-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="98dab-154">提示： 如果您的方法上方列上放置插入號，並輸入三個正斜線，Visual Studio 會自動插入的 XML 項目。</span><span class="sxs-lookup"><span data-stu-id="98dab-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="98dab-155">然後您可以填上空白。</span><span class="sxs-lookup"><span data-stu-id="98dab-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="98dab-156">現在建置並再次執行應用程式，並瀏覽至 [說明] 頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="98dab-157">文件字串，應該會出現在 API 資料表。</span><span class="sxs-lookup"><span data-stu-id="98dab-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="98dab-158">[說明] 頁面會在執行階段，從 XML 檔案讀取的字串。</span><span class="sxs-lookup"><span data-stu-id="98dab-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="98dab-159">（當您部署應用程式時，請確定部署的 XML 檔案。）</span><span class="sxs-lookup"><span data-stu-id="98dab-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="98dab-160">背後原理</span><span class="sxs-lookup"><span data-stu-id="98dab-160">Under the Hood</span></span>

<span data-ttu-id="98dab-161">說明網頁為基礎建置的**ApiExplorer**類別，這是 Web API 架構的一部分。</span><span class="sxs-lookup"><span data-stu-id="98dab-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="98dab-162">**ApiExplorer**類別提供原始資料來建立說明頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="98dab-163">每個 API，如**ApiExplorer**包含**ApiDescription**描述 API。</span><span class="sxs-lookup"><span data-stu-id="98dab-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="98dab-164">基於此目的，「 API 」 定義為 HTTP 方法和相對 URI 的組合。</span><span class="sxs-lookup"><span data-stu-id="98dab-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="98dab-165">例如，以下是一些不同的 Api:</span><span class="sxs-lookup"><span data-stu-id="98dab-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="98dab-166">取得 /api/Products</span><span class="sxs-lookup"><span data-stu-id="98dab-166">GET /api/Products</span></span>
- <span data-ttu-id="98dab-167">取得 /api/產品 / {id}</span><span class="sxs-lookup"><span data-stu-id="98dab-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="98dab-168">張貼/api/產品</span><span class="sxs-lookup"><span data-stu-id="98dab-168">POST /api/Products</span></span>

<span data-ttu-id="98dab-169">如果控制器動作支援多個 HTTP 方法， **ApiExplorer**視為不同的 API 中的每個方法。</span><span class="sxs-lookup"><span data-stu-id="98dab-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="98dab-170">若要隱藏的 API **ApiExplorer**，新增**ApiExplorerSettings**屬性設為動作和 set *IgnoreApi*設為 true。</span><span class="sxs-lookup"><span data-stu-id="98dab-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="98dab-171">您也可以將這個屬性加入到控制站，以排除整個控制器中。</span><span class="sxs-lookup"><span data-stu-id="98dab-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="98dab-172">ApiExplorer 類別取得文件字串，從**IDocumentationProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="98dab-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="98dab-173">如您稍早所見，說明頁面的程式庫會提供**IDocumentationProvider** ，取得文件從 XML 文件字串。</span><span class="sxs-lookup"><span data-stu-id="98dab-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="98dab-174">程式碼位於 /Areas/HelpPage/XmlDocumentationProvider.cs。</span><span class="sxs-lookup"><span data-stu-id="98dab-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="98dab-175">您可以取得文件從其他來源藉由撰寫您自己**IDocumentationProvider**。</span><span class="sxs-lookup"><span data-stu-id="98dab-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="98dab-176">若要將其註冊，呼叫**SetDocumentationProvider**擴充方法，定義於**HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="98dab-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="98dab-177">**ApiExplorer**會自動呼叫**IDocumentationProvider**介面，以取得每個 API 的文件字串。</span><span class="sxs-lookup"><span data-stu-id="98dab-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="98dab-178">它會儲存在**文件**屬性**ApiDescription**並**ApiParameterDescription**物件。</span><span class="sxs-lookup"><span data-stu-id="98dab-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98dab-179">後續步驟</span><span class="sxs-lookup"><span data-stu-id="98dab-179">Next Steps</span></span>

<span data-ttu-id="98dab-180">您不限於如下所示的 [說明] 頁。</span><span class="sxs-lookup"><span data-stu-id="98dab-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="98dab-181">事實上， **ApiExplorer**不限於建立 [說明] 頁面。</span><span class="sxs-lookup"><span data-stu-id="98dab-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="98dab-182">Yao Huang 連結已寫入一些很棒的部落格文章以取得您想要立即可用的：</span><span class="sxs-lookup"><span data-stu-id="98dab-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="98dab-183">將簡單的測試用戶端新增至 ASP.NET Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="98dab-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="98dab-184">進行 ASP.NET Web API 說明頁面在自我裝載的服務上運作</span><span class="sxs-lookup"><span data-stu-id="98dab-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="98dab-185">設計階段產生說明頁面 （或用戶端） 的 ASP.NET Web api</span><span class="sxs-lookup"><span data-stu-id="98dab-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="98dab-186">進階的說明頁面自訂</span><span class="sxs-lookup"><span data-stu-id="98dab-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
