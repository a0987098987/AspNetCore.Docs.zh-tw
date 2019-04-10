---
title: 從 ASP.NET MVC 移轉至 ASP.NET Core MVC
author: ardalis
description: 了解如何將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC 開始使用。
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a85b9f15be8ad9ca66b20ef1f4422fe67806a797
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468536"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="3af1c-103">從 ASP.NET MVC 移轉至 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="3af1c-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="3af1c-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)， [Steve Smith](https://ardalis.com/)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="3af1c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="3af1c-105">本文說明如何開始移轉至 ASP.NET MVC 專案[ASP.NET Core MVC](../mvc/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="3af1c-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="3af1c-106">在過程中，它會反白顯示已從 ASP.NET MVC 的事項。</span><span class="sxs-lookup"><span data-stu-id="3af1c-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="3af1c-107">從 ASP.NET MVC 移轉為多個步驟的程序，這篇文章涵蓋初始設定，基本的控制器和檢視、 靜態內容，與用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="3af1c-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="3af1c-108">其他文章涵蓋移轉設定和許多的 ASP.NET MVC 專案中找到的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="3af1c-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="3af1c-109">在範例中的版本號碼可能不是最新資訊。</span><span class="sxs-lookup"><span data-stu-id="3af1c-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="3af1c-110">您可能需要據此更新您的專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="3af1c-111">建立 ASP.NET MVC 專案的入門</span><span class="sxs-lookup"><span data-stu-id="3af1c-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="3af1c-112">若要示範升級，我們先建立 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3af1c-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="3af1c-113">建立具有名稱*WebApp1*使命名空間符合我們在下一個步驟中建立 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio 新增專案對話方塊](mvc/_static/new-project.png)

![新的 Web 應用程式 對話方塊中：ASP.NET 範本 面板中選取的 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="3af1c-116">*選擇性：* 從方案的名稱變更*WebApp1*要*Mvc5*。</span><span class="sxs-lookup"><span data-stu-id="3af1c-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="3af1c-117">Visual Studio 會顯示新的方案名稱 (*Mvc5*)，讓您更方便區分此專案從下一步 的專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="3af1c-118">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="3af1c-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="3af1c-119">建立新*空*與前一個專案同名的 ASP.NET Core web 應用程式 (*WebApp1*) 使兩個專案中的命名空間相符。</span><span class="sxs-lookup"><span data-stu-id="3af1c-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="3af1c-120">具有相同的命名空間可讓您更輕鬆地複製兩個專案之間的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3af1c-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="3af1c-121">您必須在不同於使用相同名稱的前一個專案目錄中建立此專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![[新增專案] 對話](mvc/_static/new_core.png)

![新的 ASP.NET Web 應用程式 對話方塊中：ASP.NET Core 範本 面板中選取的空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="3af1c-124">*選擇性：* 建立新的 ASP.NET Core 應用程式使用*Web 應用程式*專案範本。</span><span class="sxs-lookup"><span data-stu-id="3af1c-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="3af1c-125">將專案命名為*WebApp1*，然後選取驗證選項**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="3af1c-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="3af1c-126">重新命名此應用程式*FullAspNetCore*。</span><span class="sxs-lookup"><span data-stu-id="3af1c-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="3af1c-127">轉換中建立此專案可節省時間。</span><span class="sxs-lookup"><span data-stu-id="3af1c-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="3af1c-128">您可以查看範本產生的程式碼以查看最後的結果，或將程式碼複製到轉換專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="3af1c-129">它也很有用時停滯的轉換步驟，以比較與範本產生專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="3af1c-130">設定站台使用 MVC</span><span class="sxs-lookup"><span data-stu-id="3af1c-130">Configure the site to use MVC</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="3af1c-131">當以.NET Core 為目標[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)預設參考。</span><span class="sxs-lookup"><span data-stu-id="3af1c-131">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="3af1c-132">此套件會包含封裝常用套件的 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3af1c-132">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="3af1c-133">如果目標.NET Framework，套件參考都必須列出個別專案檔中。</span><span class="sxs-lookup"><span data-stu-id="3af1c-133">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="3af1c-134">當以.NET Core 為目標[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)預設參考。</span><span class="sxs-lookup"><span data-stu-id="3af1c-134">When targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) is referenced by default.</span></span> <span data-ttu-id="3af1c-135">此套件會包含封裝常用套件的 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3af1c-135">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="3af1c-136">如果目標.NET Framework，套件參考都必須列出個別專案檔中。</span><span class="sxs-lookup"><span data-stu-id="3af1c-136">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="3af1c-137">當目標為.NET Core 或.NET Framework，常用套件的 MVC 應用程式列出封裝個別專案檔中。</span><span class="sxs-lookup"><span data-stu-id="3af1c-137">When targeting .NET Core or .NET Framework, packages commonly used packages by MVC apps are listed individually in the project file.</span></span>

::: moniker-end

`Microsoft.AspNetCore.Mvc` <span data-ttu-id="3af1c-138">是 ASP.NET Core MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="3af1c-138">is the ASP.NET Core MVC framework.</span></span> `Microsoft.AspNetCore.StaticFiles` <span data-ttu-id="3af1c-139">是靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="3af1c-139">is the static file handler.</span></span> <span data-ttu-id="3af1c-140">ASP.NET Core 執行階段已模組化的您必須明確選擇中提供靜態檔案 (請參閱[靜態檔案](xref:fundamentals/static-files))。</span><span class="sxs-lookup"><span data-stu-id="3af1c-140">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="3af1c-141">開啟*Startup.cs*檔案，並變更程式碼以符合下列各項：</span><span class="sxs-lookup"><span data-stu-id="3af1c-141">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="3af1c-142">`UseStaticFiles`延伸模組方法會將靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="3af1c-142">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="3af1c-143">如先前所述，ASP.NET 執行階段模組化，而且您必須明確地選擇要提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-143">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="3af1c-144">`UseMvc`擴充方法新增路由。</span><span class="sxs-lookup"><span data-stu-id="3af1c-144">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="3af1c-145">如需詳細資訊，請參閱 <<c0> [ 應用程式啟動](xref:fundamentals/startup)並[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="3af1c-145">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="3af1c-146">新增控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="3af1c-146">Add a controller and view</span></span>

<span data-ttu-id="3af1c-147">在本節中，您將新增的最小的控制器和檢視，以做為預留位置的 ASP.NET MVC 控制器和檢視下一節中，您會移轉。</span><span class="sxs-lookup"><span data-stu-id="3af1c-147">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="3af1c-148">新增*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-148">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="3af1c-149">新增**控制器類別**名為*HomeController.cs*來*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-149">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="3af1c-151">新增*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-151">Add a *Views* folder.</span></span>

* <span data-ttu-id="3af1c-152">新增*Views/Home*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-152">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="3af1c-153">新增**Razor 檢視**名為*Index.cshtml*來*Views/Home*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-153">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/view.png)

<span data-ttu-id="3af1c-155">專案結構如下所示：</span><span class="sxs-lookup"><span data-stu-id="3af1c-155">The project structure is shown below:</span></span>

![顯示檔案和資料夾的 WebApp1 的方案總管](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="3af1c-157">內容取代*Views/Home/Index.cshtml*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="3af1c-157">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="3af1c-158">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3af1c-158">Run the app.</span></span>

![在 Microsoft Edge 中開啟的 web 應用程式](mvc/_static/hello-world.png)

<span data-ttu-id="3af1c-160">請參閱[控制器](xref:mvc/controllers/actions)並[檢視](xref:mvc/views/overview)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="3af1c-160">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="3af1c-161">既然我們已最小的使用 ASP.NET Core 專案，我們可以開始移轉功能從 ASP.NET MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-161">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="3af1c-162">我們需要將下列：</span><span class="sxs-lookup"><span data-stu-id="3af1c-162">We need to move the following:</span></span>

* <span data-ttu-id="3af1c-163">用戶端內容 （CSS、 字型和指令碼）</span><span class="sxs-lookup"><span data-stu-id="3af1c-163">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="3af1c-164">控制器</span><span class="sxs-lookup"><span data-stu-id="3af1c-164">controllers</span></span>

* <span data-ttu-id="3af1c-165">檢視</span><span class="sxs-lookup"><span data-stu-id="3af1c-165">views</span></span>

* <span data-ttu-id="3af1c-166">模型</span><span class="sxs-lookup"><span data-stu-id="3af1c-166">models</span></span>

* <span data-ttu-id="3af1c-167">統合</span><span class="sxs-lookup"><span data-stu-id="3af1c-167">bundling</span></span>

* <span data-ttu-id="3af1c-168">篩選條件</span><span class="sxs-lookup"><span data-stu-id="3af1c-168">filters</span></span>

* <span data-ttu-id="3af1c-169">輸入/輸出的記錄檔、 身分識別 （這會在下一個教學課程中完成）。</span><span class="sxs-lookup"><span data-stu-id="3af1c-169">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="3af1c-170">控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="3af1c-170">Controllers and views</span></span>

* <span data-ttu-id="3af1c-171">複製每一個方法從 ASP.NET MVC`HomeController`新`HomeController`。</span><span class="sxs-lookup"><span data-stu-id="3af1c-171">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="3af1c-172">請注意，在 ASP.NET MVC 中內建範本的控制器動作方法傳回型別[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET Core MVC 動作方法會傳回`IActionResult`改。</span><span class="sxs-lookup"><span data-stu-id="3af1c-172">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> `ActionResult` <span data-ttu-id="3af1c-173">實作`IActionResult`，這樣就不需要變更您的動作方法的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="3af1c-173">implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="3af1c-174">複製*About.cshtml*， *Contact.cshtml*，並*Index.cshtml*從 ASP.NET MVC 專案 」 至 ASP.NET Core 專案中的 Razor 檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-174">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="3af1c-175">執行 ASP.NET Core 應用程式並測試每個方法。</span><span class="sxs-lookup"><span data-stu-id="3af1c-175">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="3af1c-176">我們還沒有樣式的配置檔案或尚未移轉，讓呈現的檢視只會包含在檢視檔案內容。</span><span class="sxs-lookup"><span data-stu-id="3af1c-176">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="3af1c-177">您不需要的版面配置檔案產生連結`About`並`Contact`檢視，因此您必須從瀏覽器叫用它們 (取代**4492**專案中使用的連接埠號碼)。</span><span class="sxs-lookup"><span data-stu-id="3af1c-177">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

<span data-ttu-id="3af1c-179">請注意缺乏樣式和功能表項目。</span><span class="sxs-lookup"><span data-stu-id="3af1c-179">Note the lack of styling and menu items.</span></span> <span data-ttu-id="3af1c-180">我們將在下節修正該問題。</span><span class="sxs-lookup"><span data-stu-id="3af1c-180">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="3af1c-181">靜態內容</span><span class="sxs-lookup"><span data-stu-id="3af1c-181">Static content</span></span>

<span data-ttu-id="3af1c-182">在舊版的 ASP.NET MVC 中，靜態內容裝載的 web 專案的根目錄，並已混合使用伺服器端檔案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-182">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="3af1c-183">ASP.NET Core 中的靜態內容裝載於*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-183">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="3af1c-184">您會想要複製您舊的 ASP.NET MVC 應用程式，以從靜態內容*wwwroot* ASP.NET Core 專案中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-184">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="3af1c-185">在此範例轉換：</span><span class="sxs-lookup"><span data-stu-id="3af1c-185">In this sample conversion:</span></span>

* <span data-ttu-id="3af1c-186">複製 */favicon.ico*至舊的 MVC 專案中的檔案*wwwroot* ASP.NET Core 專案中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-186">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="3af1c-187">舊的 ASP.NET MVC 專案會使用[Bootstrap](https://getbootstrap.com/)啟動安裝程式中的檔案及其樣式和存放區*內容*並*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-187">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="3af1c-188">範本，用來產生舊的 ASP.NET MVC 專案，參考在配置檔案中的啟動程序 (*Views/Shared/_Layout.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="3af1c-188">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="3af1c-189">您可以複製*bootstrap.js*並*bootstrap.css*檔案從 ASP.NET MVC 專案加入*wwwroot*在新的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-189">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="3af1c-190">相反地，我們將新增的啟動程序支援 （和其他用戶端程式庫） 使用 Cdn 下, 一節。</span><span class="sxs-lookup"><span data-stu-id="3af1c-190">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="3af1c-191">移轉的配置檔案</span><span class="sxs-lookup"><span data-stu-id="3af1c-191">Migrate the layout file</span></span>

* <span data-ttu-id="3af1c-192">複製 *_ViewStart.cshtml*舊的 ASP.NET MVC 專案中的檔案*檢視*到 ASP.NET Core 專案的資料夾*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-192">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="3af1c-193">*_ViewStart.cshtml*尚未變更的檔案，這是 ASP.NET Core MVC 中。</span><span class="sxs-lookup"><span data-stu-id="3af1c-193">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="3af1c-194">建立*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-194">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="3af1c-195">*選擇性：* 複製 *_ViewImports.cshtml*從*FullAspNetCore* MVC 專案*檢視*到 ASP.NET Core 專案的資料夾*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-195">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="3af1c-196">中的任何命名空間宣告中移除 *_ViewImports.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-196">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="3af1c-197">*_ViewImports.cshtml*檔案提供的所有檢視檔案的命名空間和帶入[標籤協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="3af1c-197">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="3af1c-198">標籤協助程式會在新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-198">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="3af1c-199">*_ViewImports.cshtml*檔案是 ASP.NET Core 的新功能。</span><span class="sxs-lookup"><span data-stu-id="3af1c-199">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="3af1c-200">複製 *_Layout.cshtml*舊的 ASP.NET MVC 專案中的檔案*Views/Shared*到 ASP.NET Core 專案的資料夾*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3af1c-200">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="3af1c-201">開啟 *_Layout.cshtml*檔案，並進行下列變更 （已完成的程式碼如下所示）：</span><span class="sxs-lookup"><span data-stu-id="3af1c-201">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="3af1c-202">取代`@Styles.Render("~/Content/css")`具有`<link>`載入的項目*bootstrap.css* （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="3af1c-202">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="3af1c-203">移除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="3af1c-203">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="3af1c-204">標記為註解`@Html.Partial("_LoginPartial")`列 (括住的那行`@*...*@`)。</span><span class="sxs-lookup"><span data-stu-id="3af1c-204">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="3af1c-205">如需詳細資訊，請參閱[移轉驗證和身分識別至 ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="3af1c-205">For more information see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="3af1c-206">取代`@Scripts.Render("~/bundles/jquery")`與`<script>`項目 （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="3af1c-206">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="3af1c-207">取代`@Scripts.Render("~/bundles/bootstrap")`與`<script>`項目 （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="3af1c-207">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="3af1c-208">Bootstrap CSS 包含取代標記：</span><span class="sxs-lookup"><span data-stu-id="3af1c-208">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="3af1c-209">JQuery 和 Bootstrap JavaScript 包含的取代標記：</span><span class="sxs-lookup"><span data-stu-id="3af1c-209">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="3af1c-210">已更新 *_Layout.cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="3af1c-210">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="3af1c-211">在瀏覽器中檢視站台。</span><span class="sxs-lookup"><span data-stu-id="3af1c-211">View the site in the browser.</span></span> <span data-ttu-id="3af1c-212">它應現在可正確載入，以就地的預期樣式。</span><span class="sxs-lookup"><span data-stu-id="3af1c-212">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="3af1c-213">*選擇性：* 您可能會想嘗試使用新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-213">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="3af1c-214">此專案的版面配置檔案可以複製*FullAspNetCore*專案。</span><span class="sxs-lookup"><span data-stu-id="3af1c-214">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="3af1c-215">新的版面配置檔會使用[標籤協助程式](xref:mvc/views/tag-helpers/intro)還有其他改進功能。</span><span class="sxs-lookup"><span data-stu-id="3af1c-215">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="3af1c-216">設定統合和縮製</span><span class="sxs-lookup"><span data-stu-id="3af1c-216">Configure bundling and minification</span></span>

<span data-ttu-id="3af1c-217">如需如何設定統合和縮製的詳細資訊，請參閱[統合和縮製](../client-side/bundling-and-minification.md)。</span><span class="sxs-lookup"><span data-stu-id="3af1c-217">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="3af1c-218">解決 HTTP 500 錯誤</span><span class="sxs-lookup"><span data-stu-id="3af1c-218">Solve HTTP 500 errors</span></span>

<span data-ttu-id="3af1c-219">有許多問題，可能會造成 HTTP 500 錯誤訊息包含問題的來源上的任何資訊。</span><span class="sxs-lookup"><span data-stu-id="3af1c-219">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="3af1c-220">例如，如果*views/_viewimports.cshtml*檔案包含在您的專案不存在的命名空間，您會收到 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="3af1c-220">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="3af1c-221">根據預設，ASP.NET Core 應用程式，在`UseDeveloperExceptionPage`延伸模組新增至`IApplicationBuilder`並執行設定時*開發*。</span><span class="sxs-lookup"><span data-stu-id="3af1c-221">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="3af1c-222">下列程式碼中有詳細說明：</span><span class="sxs-lookup"><span data-stu-id="3af1c-222">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="3af1c-223">ASP.NET Core 會將 web 應用程式中的未處理例外狀況轉換成 HTTP 500 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="3af1c-223">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="3af1c-224">一般來說，這些回應以避免洩露可能含有機密伺服器的相關資訊不包含錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="3af1c-224">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="3af1c-225">請參閱**使用開發人員例外狀況頁面**中[處理錯誤](../fundamentals/error-handling.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="3af1c-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3af1c-226">其他資源</span><span class="sxs-lookup"><span data-stu-id="3af1c-226">Additional resources</span></span>

* <xref:razor-components/index>
* <xref:mvc/views/tag-helpers/intro>
