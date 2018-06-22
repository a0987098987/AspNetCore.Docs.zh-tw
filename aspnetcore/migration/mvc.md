---
title: ASP.NET MVC 從移轉至 ASP.NET Core MVC
author: ardalis
description: 了解如何開始將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC。
ms.author: riande
ms.date: 03/07/2017
uid: migration/mvc
ms.openlocfilehash: 6fecc820177ca5033cfd00d632904a950c1b29bb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274771"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="c096c-103">ASP.NET MVC 從移轉至 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c096c-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="c096c-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)， [Steve Smith](https://ardalis.com/)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c096c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c096c-105">本文將說明如何開始移轉至 ASP.NET MVC 專案[ASP.NET Core MVC](../mvc/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="c096c-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="c096c-106">在過程中，它會反白顯示已從 ASP.NET MVC 的事項。</span><span class="sxs-lookup"><span data-stu-id="c096c-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="c096c-107">ASP.NET MVC 從移轉是多重步驟的程序，本文章涵蓋初始設定，基本的控制器和檢視、 靜態內容，與用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="c096c-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="c096c-108">其他文章涵蓋移轉設定和許多的 ASP.NET MVC 專案中的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="c096c-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="c096c-109">樣本中的版本號碼可能不是最新資訊。</span><span class="sxs-lookup"><span data-stu-id="c096c-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="c096c-110">您可能要據以更新您的專案。</span><span class="sxs-lookup"><span data-stu-id="c096c-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="c096c-111">建立入門 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="c096c-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="c096c-112">若要示範升級，我們會先建立 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c096c-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="c096c-113">建立具有名稱*WebApp1*使命名空間符合我們在下一個步驟中建立的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="c096c-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio 新專案 對話方塊](mvc/_static/new-project.png)

![新的 Web 應用程式 對話方塊： ASP.NET 範本 面板中選取的 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="c096c-116">*選擇性：* 變更解決方案的名稱*WebApp1*至*Mvc5*。</span><span class="sxs-lookup"><span data-stu-id="c096c-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="c096c-117">Visual Studio 會顯示新的方案名稱 (*Mvc5*)，讓您更方便區分此專案，從下一個專案。</span><span class="sxs-lookup"><span data-stu-id="c096c-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="c096c-118">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="c096c-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="c096c-119">建立新*空*ASP.NET Core web 應用程式與先前的專案名稱相同 (*WebApp1*) 使兩個專案中的命名空間相符。</span><span class="sxs-lookup"><span data-stu-id="c096c-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="c096c-120">具有相同的命名空間，可以更輕鬆地將程式碼的兩個專案之間複製。</span><span class="sxs-lookup"><span data-stu-id="c096c-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="c096c-121">您必須在比先前的專案以使用相同的名稱不同的目錄中建立此專案。</span><span class="sxs-lookup"><span data-stu-id="c096c-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![[新增專案] 對話](mvc/_static/new_core.png)

![新的 ASP.NET Web 應用程式 對話方塊： 選取 ASP.NET Core 範本面板中的空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="c096c-124">*選擇性：* 建立新的 ASP.NET Core 應用程式使用*Web 應用程式*專案範本。</span><span class="sxs-lookup"><span data-stu-id="c096c-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="c096c-125">將專案命名*WebApp1*，以及選取的驗證選項**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="c096c-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="c096c-126">重新命名此應用程式， *FullAspNetCore*。</span><span class="sxs-lookup"><span data-stu-id="c096c-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="c096c-127">轉換中建立此專案節省您的時間。</span><span class="sxs-lookup"><span data-stu-id="c096c-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="c096c-128">您可以查看範本產生的程式碼以查看結果，或將程式碼複製到轉換專案。</span><span class="sxs-lookup"><span data-stu-id="c096c-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="c096c-129">它也是很有幫助時堵塞在要與範本產生的專案比較轉換步驟。</span><span class="sxs-lookup"><span data-stu-id="c096c-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="c096c-130">將網站設定為使用 MVC</span><span class="sxs-lookup"><span data-stu-id="c096c-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="c096c-131">當目標為.NET Core，ASP.NET Core metapackage 會加入至專案，稱為`Microsoft.AspNetCore.All`預設。</span><span class="sxs-lookup"><span data-stu-id="c096c-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="c096c-132">此套件包含像是`Microsoft.AspNetCore.Mvc`和`Microsoft.AspNetCore.StaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="c096c-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="c096c-133">如果目標.NET Framework，封裝會參照需要個別列出 \*.csproj 檔案中。</span><span class="sxs-lookup"><span data-stu-id="c096c-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="c096c-134">`Microsoft.AspNetCore.Mvc` 是 ASP.NET Core MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="c096c-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="c096c-135">`Microsoft.AspNetCore.StaticFiles` 是靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="c096c-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="c096c-136">ASP.NET Core 執行階段是一個模組，以及您必須明確地選擇提供靜態檔案 (請參閱[靜態檔案](xref:fundamentals/static-files))。</span><span class="sxs-lookup"><span data-stu-id="c096c-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="c096c-137">開啟*Startup.cs*檔案，並且變更以符合下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="c096c-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="c096c-138">`UseStaticFiles`擴充方法新增靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="c096c-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="c096c-139">如先前所述，ASP.NET 執行階段模組化，而且您必須明確地選擇提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="c096c-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="c096c-140">`UseMvc`擴充方法會將路由。</span><span class="sxs-lookup"><span data-stu-id="c096c-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="c096c-141">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)和[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="c096c-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="c096c-142">加入控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="c096c-142">Add a controller and view</span></span>

<span data-ttu-id="c096c-143">在本節中，您會加入最少的控制器和檢視，以做為預留位置的 ASP.NET MVC 控制器和檢視表會在下一節中移轉。</span><span class="sxs-lookup"><span data-stu-id="c096c-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="c096c-144">新增*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="c096c-145">新增**控制器類別**名為*HomeController.cs*至*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="c096c-147">新增*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="c096c-148">新增*Views/Home*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="c096c-149">新增**Razor 檢視**名為*Index.cshtml*至*Views/Home*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/view.png)

<span data-ttu-id="c096c-151">專案結構如下所示：</span><span class="sxs-lookup"><span data-stu-id="c096c-151">The project structure is shown below:</span></span>

![方案總管 中顯示檔案和資料夾的 WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="c096c-153">取代內容*Views/Home/Index.cshtml*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="c096c-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="c096c-154">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c096c-154">Run the app.</span></span>

![在 Microsoft Edge 中開啟的 web 應用程式](mvc/_static/hello-world.png)

<span data-ttu-id="c096c-156">請參閱[控制器](xref:mvc/controllers/actions)和[檢視](xref:mvc/views/overview)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="c096c-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="c096c-157">現在我們有最小的工作 ASP.NET Core 專案時，我們可以開始從 ASP.NET MVC 專案移轉功能。</span><span class="sxs-lookup"><span data-stu-id="c096c-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="c096c-158">我們需要移動下列：</span><span class="sxs-lookup"><span data-stu-id="c096c-158">We need to move the following:</span></span>

* <span data-ttu-id="c096c-159">用戶端內容 （CSS、 字型和指令碼）</span><span class="sxs-lookup"><span data-stu-id="c096c-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="c096c-160">控制器</span><span class="sxs-lookup"><span data-stu-id="c096c-160">controllers</span></span>

* <span data-ttu-id="c096c-161">檢視</span><span class="sxs-lookup"><span data-stu-id="c096c-161">views</span></span>

* <span data-ttu-id="c096c-162">模型</span><span class="sxs-lookup"><span data-stu-id="c096c-162">models</span></span>

* <span data-ttu-id="c096c-163">結合在一起</span><span class="sxs-lookup"><span data-stu-id="c096c-163">bundling</span></span>

* <span data-ttu-id="c096c-164">篩選條件</span><span class="sxs-lookup"><span data-stu-id="c096c-164">filters</span></span>

* <span data-ttu-id="c096c-165">記錄檔 in/out、 身分識別 （這在下一個教學課程中完成）。</span><span class="sxs-lookup"><span data-stu-id="c096c-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="c096c-166">控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="c096c-166">Controllers and views</span></span>

* <span data-ttu-id="c096c-167">每個方法複製 ASP.NET MVC`HomeController`新`HomeController`。</span><span class="sxs-lookup"><span data-stu-id="c096c-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="c096c-168">請注意，ASP.NET MVC 中內建的範本控制器動作方法傳回型別是[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET Core MVC 動作方法傳回`IActionResult`改為。</span><span class="sxs-lookup"><span data-stu-id="c096c-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="c096c-169">`ActionResult` 實作`IActionResult`，因此不需要變更您的動作方法的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="c096c-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="c096c-170">複製*About.cshtml*， *Contact.cshtml*，和*Index.cshtml* Razor 檢視檔案，從 ASP.NET MVC 專案加入 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="c096c-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="c096c-171">執行 ASP.NET Core 應用程式和測試每一種方法。</span><span class="sxs-lookup"><span data-stu-id="c096c-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="c096c-172">我們尚未樣式的配置檔案尚未移轉，因此只能包含呈現的檢視的檢視檔案中的內容。</span><span class="sxs-lookup"><span data-stu-id="c096c-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="c096c-173">您不需要的配置檔案產生連結`About`和`Contact`檢視時，所以您必須先從瀏覽器叫用 (取代**4492**專案中使用的連接埠號碼)。</span><span class="sxs-lookup"><span data-stu-id="c096c-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

<span data-ttu-id="c096c-175">請注意缺乏樣式和功能表項目。</span><span class="sxs-lookup"><span data-stu-id="c096c-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="c096c-176">我們將在下節修正該問題。</span><span class="sxs-lookup"><span data-stu-id="c096c-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="c096c-177">靜態內容</span><span class="sxs-lookup"><span data-stu-id="c096c-177">Static content</span></span>

<span data-ttu-id="c096c-178">在舊版 ASP.NET MVC 中，靜態內容裝載 web 專案的根目錄和伺服器端檔案已混合。</span><span class="sxs-lookup"><span data-stu-id="c096c-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="c096c-179">在 ASP.NET Core 靜態內容裝載於*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="c096c-180">您要複製舊 ASP.NET MVC 應用程式中的靜態內容*wwwroot* ASP.NET Core 專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="c096c-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="c096c-181">在這個範例轉換：</span><span class="sxs-lookup"><span data-stu-id="c096c-181">In this sample conversion:</span></span>

* <span data-ttu-id="c096c-182">複製*favicon.ico*檔案從舊的 MVC 專案至*wwwroot* ASP.NET Core 專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="c096c-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="c096c-183">舊的 ASP.NET MVC 專案會使用[Bootstrap](https://getbootstrap.com/)啟動安裝程式中的檔案其設定樣式和存放區的*內容*和*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="c096c-184">產生舊的 ASP.NET MVC 專案範本會參考配置檔案中的啟動程序 (*Views/Shared/_Layout.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="c096c-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="c096c-185">您無法複製*bootstrap.js*和*bootstrap.css*檔案從 ASP.NET MVC 專案加入*wwwroot*新專案中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="c096c-186">相反地，我們會將新增的啟動程序支援 （和其他用戶端程式庫） 使用 Cdn 下一節。</span><span class="sxs-lookup"><span data-stu-id="c096c-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="c096c-187">移轉配置檔案</span><span class="sxs-lookup"><span data-stu-id="c096c-187">Migrate the layout file</span></span>

* <span data-ttu-id="c096c-188">複製 *_viewstart.vbhtml*檔案從舊的 ASP.NET MVC 專案*檢視*ASP.NET Core 專案的資料夾*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c096c-189">*_Viewstart.vbhtml* ASP.NET Core MVC 中未變更檔案。</span><span class="sxs-lookup"><span data-stu-id="c096c-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="c096c-190">建立*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="c096c-191">*選擇性：* 複製 *_ViewImports.cshtml*從*FullAspNetCore* MVC 專案*檢視*ASP.NET Core 專案的資料夾*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c096c-192">中的任何命名空間宣告中移除 *_ViewImports.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="c096c-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c096c-193">*_ViewImports.cshtml*檔案命名空間提供檢視的所有檔案，並使[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="c096c-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c096c-194">新的版面配置檔中使用標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="c096c-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="c096c-195">*_ViewImports.cshtml*檔案是新的 ASP.NET core。</span><span class="sxs-lookup"><span data-stu-id="c096c-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="c096c-196">複製 *_Layout.cshtml*檔案從舊的 ASP.NET MVC 專案*Views/Shared* ASP.NET Core 專案的資料夾*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c096c-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="c096c-197">開啟 *_Layout.cshtml*檔案，然後進行下列變更 （已完成的程式碼如下所示）：</span><span class="sxs-lookup"><span data-stu-id="c096c-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="c096c-198">取代`@Styles.Render("~/Content/css")`與`<link>`載入的項目*bootstrap.css* （請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="c096c-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="c096c-199">移除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="c096c-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="c096c-200">標記為註解`@Html.Partial("_LoginPartial")`列 (周圍的線條`@*...*@`)。</span><span class="sxs-lookup"><span data-stu-id="c096c-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="c096c-201">我們將在未來的教學課程中傳回它。</span><span class="sxs-lookup"><span data-stu-id="c096c-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="c096c-202">取代`@Scripts.Render("~/bundles/jquery")`與`<script>`項目 （請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="c096c-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="c096c-203">取代`@Scripts.Render("~/bundles/bootstrap")`與`<script>`項目 （請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="c096c-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="c096c-204">包含啟動載入器的 CSS 取代標記：</span><span class="sxs-lookup"><span data-stu-id="c096c-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="c096c-205">JQuery 和啟動程序 JavaScript 內含的取代標記：</span><span class="sxs-lookup"><span data-stu-id="c096c-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="c096c-206">已更新 *_Layout.cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="c096c-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="c096c-207">在瀏覽器中檢視站台。</span><span class="sxs-lookup"><span data-stu-id="c096c-207">View the site in the browser.</span></span> <span data-ttu-id="c096c-208">它應該現在正確載入，以就地預期的樣式。</span><span class="sxs-lookup"><span data-stu-id="c096c-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="c096c-209">*選擇性：* 要再試一次使用新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="c096c-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="c096c-210">這個專案的版面配置檔案可以複製*FullAspNetCore*專案。</span><span class="sxs-lookup"><span data-stu-id="c096c-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="c096c-211">新的版面配置檔案使用[標記協助程式](xref:mvc/views/tag-helpers/intro)且具有其他改進功能。</span><span class="sxs-lookup"><span data-stu-id="c096c-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="c096c-212">統合及縮製的設定</span><span class="sxs-lookup"><span data-stu-id="c096c-212">Configure bundling and minification</span></span>

<span data-ttu-id="c096c-213">如需如何設定統合及縮製的資訊，請參閱[組合和縮製](../client-side/bundling-and-minification.md)。</span><span class="sxs-lookup"><span data-stu-id="c096c-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="c096c-214">解決 HTTP 500 錯誤</span><span class="sxs-lookup"><span data-stu-id="c096c-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="c096c-215">有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題的來源上的任何資訊。</span><span class="sxs-lookup"><span data-stu-id="c096c-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="c096c-216">例如，如果*Views/_ViewImports.cshtml*檔案包含不存在於您的專案命名空間，您會收到 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="c096c-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="c096c-217">根據預設，ASP.NET Core 應用程式，在`UseDeveloperExceptionPage`延伸會加入至`IApplicationBuilder`而設定時，執行*開發*。</span><span class="sxs-lookup"><span data-stu-id="c096c-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="c096c-218">下列程式碼中有詳細說明：</span><span class="sxs-lookup"><span data-stu-id="c096c-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="c096c-219">ASP.NET Core 會將 web 應用程式中的未處理例外狀況轉換成 HTTP 500 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="c096c-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="c096c-220">一般來說，若要防止潛在的敏感資訊伺服器公開這些回應不包含錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c096c-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="c096c-221">請參閱**使用開發人員例外狀況頁面**中[處理錯誤](../fundamentals/error-handling.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="c096c-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c096c-222">其他資源</span><span class="sxs-lookup"><span data-stu-id="c096c-222">Additional resources</span></span>

* [<span data-ttu-id="c096c-223">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="c096c-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="c096c-224">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="c096c-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
