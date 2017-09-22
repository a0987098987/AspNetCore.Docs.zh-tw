---
title: "ASP.NET MVC 從移轉至 ASP.NET Core MVC"
author: ardalis
description: 
keywords: "ASP.NET Core, MVC, 移轉"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 385ab7dfea5b92687a427bdfe9558462227113b1
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="5db29-103">ASP.NET MVC 從移轉至 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5db29-103">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="5db29-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)， [Steve Smith](https://ardalis.com/)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="5db29-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="5db29-105">本文將說明如何開始移轉至 ASP.NET MVC 專案[ASP.NET Core MVC](../mvc/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="5db29-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="5db29-106">在過程中，它會反白顯示已從 ASP.NET MVC 的事項。</span><span class="sxs-lookup"><span data-stu-id="5db29-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="5db29-107">ASP.NET MVC 從移轉是多重步驟的程序，本文章涵蓋初始設定，基本的控制器和檢視、 靜態內容，與用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="5db29-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="5db29-108">其他文章涵蓋移轉設定和許多的 ASP.NET MVC 專案中的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="5db29-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="5db29-109">樣本中的版本號碼可能不是最新資訊。</span><span class="sxs-lookup"><span data-stu-id="5db29-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="5db29-110">您可能要據以更新您的專案。</span><span class="sxs-lookup"><span data-stu-id="5db29-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="5db29-111">建立入門 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="5db29-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="5db29-112">若要示範升級，我們會先建立 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5db29-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="5db29-113">建立具有名稱*WebApp1*讓命名空間會比對我們在下一個步驟中建立的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="5db29-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio 新專案 對話方塊](mvc/_static/new-project.png)

![新的 Web 應用程式 對話方塊： ASP.NET 範本 面板中選取的 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="5db29-116">*選擇性：*變更解決方案的名稱*WebApp1*至*Mvc5*。</span><span class="sxs-lookup"><span data-stu-id="5db29-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="5db29-117">Visual Studio 會顯示新的方案名稱 (*Mvc5*)，都將您更方便區分此專案，從下一個專案。</span><span class="sxs-lookup"><span data-stu-id="5db29-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="5db29-118">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="5db29-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="5db29-119">建立新*空*ASP.NET Core web 應用程式與先前的專案名稱相同 (*WebApp1*) 使兩個專案中的命名空間相符。</span><span class="sxs-lookup"><span data-stu-id="5db29-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="5db29-120">具有相同的命名空間，可以更輕鬆地將程式碼的兩個專案之間複製。</span><span class="sxs-lookup"><span data-stu-id="5db29-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="5db29-121">您必須在比先前的專案以使用相同的名稱不同的目錄中建立此專案。</span><span class="sxs-lookup"><span data-stu-id="5db29-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![[新增專案] 對話](mvc/_static/new_core.png)

![新的 ASP.NET Web 應用程式 對話方塊： 選取 ASP.NET Core 範本面板中的空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="5db29-124">*選擇性：*建立新的 ASP.NET Core 應用程式使用*Web 應用程式*專案範本。</span><span class="sxs-lookup"><span data-stu-id="5db29-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="5db29-125">將專案命名*WebApp1*，以及選取的驗證選項**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="5db29-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="5db29-126">重新命名此應用程式， *FullAspNetCore*。</span><span class="sxs-lookup"><span data-stu-id="5db29-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="5db29-127">建立您的時間儲存此專案將會在轉換中。</span><span class="sxs-lookup"><span data-stu-id="5db29-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="5db29-128">您可以查看範本產生的程式碼以查看結果，或將程式碼複製到轉換專案。</span><span class="sxs-lookup"><span data-stu-id="5db29-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="5db29-129">它也是很有幫助時堵塞在要與範本產生的專案比較轉換步驟。</span><span class="sxs-lookup"><span data-stu-id="5db29-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="5db29-130">將網站設定為使用 MVC</span><span class="sxs-lookup"><span data-stu-id="5db29-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="5db29-131">安裝`Microsoft.AspNetCore.Mvc`和`Microsoft.AspNetCore.StaticFiles`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="5db29-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="5db29-132">`Microsoft.AspNetCore.Mvc`是 ASP.NET Core MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="5db29-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="5db29-133">`Microsoft.AspNetCore.StaticFiles`是靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="5db29-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="5db29-134">ASP.NET 執行階段是一個模組，以及您必須明確地選擇提供靜態檔案 (請參閱[靜態檔案處理](../fundamentals/static-files.md))。</span><span class="sxs-lookup"><span data-stu-id="5db29-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="5db29-135">開啟*.csproj*檔案 (在專案上按一下滑鼠右鍵**方案總管 中**選取**編輯 WebApp1.csproj**) 並加入`PrepareForPublish`目標：</span><span class="sxs-lookup"><span data-stu-id="5db29-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="5db29-136">`PrepareForPublish`目標所需的取得透過 Bower 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="5db29-136">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="5db29-137">我們將討論的更新版本。</span><span class="sxs-lookup"><span data-stu-id="5db29-137">We'll talk about that later.</span></span>

* <span data-ttu-id="5db29-138">開啟*Startup.cs*檔案，並且變更以符合下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5db29-138">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="5db29-139">`UseStaticFiles`擴充方法新增靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="5db29-139">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="5db29-140">如先前所述，ASP.NET 執行階段模組化，而且您必須明確地選擇提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="5db29-140">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="5db29-141">`UseMvc`擴充方法會將路由。</span><span class="sxs-lookup"><span data-stu-id="5db29-141">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="5db29-142">如需詳細資訊，請參閱[應用程式啟動](../fundamentals/startup.md)和[路由](../fundamentals/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="5db29-142">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="5db29-143">加入控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="5db29-143">Add a controller and view</span></span>

<span data-ttu-id="5db29-144">在本節中，您會加入最少的控制器和檢視，以做為預留位置的 ASP.NET MVC 控制器和檢視表會在下一節中移轉。</span><span class="sxs-lookup"><span data-stu-id="5db29-144">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="5db29-145">新增*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-145">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="5db29-146">新增**MVC 控制器類別**名稱*HomeController.cs*至*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-146">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="5db29-148">新增*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-148">Add a *Views* folder.</span></span>

* <span data-ttu-id="5db29-149">新增*Views/Home*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-149">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="5db29-150">新增*Index.cshtml* MVC 檢視頁面，即可*Views/Home*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-150">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/view.png)

<span data-ttu-id="5db29-152">專案結構如下所示：</span><span class="sxs-lookup"><span data-stu-id="5db29-152">The project structure is shown below:</span></span>

![方案總管 中顯示檔案和資料夾的 WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="5db29-154">取代內容*Views/Home/Index.cshtml*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="5db29-154">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="5db29-155">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5db29-155">Run the app.</span></span>

![在 Microsoft Edge 中開啟的 web 應用程式](mvc/_static/hello-world.png)

<span data-ttu-id="5db29-157">請參閱[控制器](../mvc/controllers/index.md)和[檢視](../mvc/views/index.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="5db29-157">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="5db29-158">現在我們有最小的工作 ASP.NET Core 專案時，我們可以開始從 ASP.NET MVC 專案移轉功能。</span><span class="sxs-lookup"><span data-stu-id="5db29-158">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="5db29-159">我們需要將下列：</span><span class="sxs-lookup"><span data-stu-id="5db29-159">We will need to move the following:</span></span>

* <span data-ttu-id="5db29-160">用戶端內容 （CSS、 字型和指令碼）</span><span class="sxs-lookup"><span data-stu-id="5db29-160">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="5db29-161">控制器</span><span class="sxs-lookup"><span data-stu-id="5db29-161">controllers</span></span>

* <span data-ttu-id="5db29-162">檢視</span><span class="sxs-lookup"><span data-stu-id="5db29-162">views</span></span>

* <span data-ttu-id="5db29-163">模型</span><span class="sxs-lookup"><span data-stu-id="5db29-163">models</span></span>

* <span data-ttu-id="5db29-164">結合在一起</span><span class="sxs-lookup"><span data-stu-id="5db29-164">bundling</span></span>

* <span data-ttu-id="5db29-165">篩選條件</span><span class="sxs-lookup"><span data-stu-id="5db29-165">filters</span></span>

* <span data-ttu-id="5db29-166">記錄檔 in/out、 身分識別 （這會執行下一個教學課程中。）</span><span class="sxs-lookup"><span data-stu-id="5db29-166">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="5db29-167">控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="5db29-167">Controllers and views</span></span>

* <span data-ttu-id="5db29-168">每個方法複製 ASP.NET MVC`HomeController`新`HomeController`。</span><span class="sxs-lookup"><span data-stu-id="5db29-168">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="5db29-169">請注意，ASP.NET MVC 中內建的範本控制器動作方法傳回型別是[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET Core MVC 動作方法傳回`IActionResult`改為。</span><span class="sxs-lookup"><span data-stu-id="5db29-169">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="5db29-170">`ActionResult`實作`IActionResult`，因此不需要變更您的動作方法的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="5db29-170">`ActionResult` implements `IActionResult`, so there is no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="5db29-171">複製*About.cshtml*， *Contact.cshtml*，和*Index.cshtml* Razor 檢視檔案，從 ASP.NET MVC 專案加入 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="5db29-171">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="5db29-172">執行 ASP.NET Core 應用程式和測試每一種方法。</span><span class="sxs-lookup"><span data-stu-id="5db29-172">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="5db29-173">我們尚未樣式的配置檔案尚未移轉，讓呈現的檢視表只會包含在檢視檔案內容。</span><span class="sxs-lookup"><span data-stu-id="5db29-173">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="5db29-174">您不需要的配置檔案產生連結`About`和`Contact`檢視時，所以您必須先從瀏覽器叫用 (取代**4492**專案中使用的連接埠號碼)。</span><span class="sxs-lookup"><span data-stu-id="5db29-174">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

<span data-ttu-id="5db29-176">請注意缺乏樣式和功能表項目。</span><span class="sxs-lookup"><span data-stu-id="5db29-176">Note the lack of styling and menu items.</span></span> <span data-ttu-id="5db29-177">我們將修正的下一節。</span><span class="sxs-lookup"><span data-stu-id="5db29-177">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="5db29-178">靜態內容</span><span class="sxs-lookup"><span data-stu-id="5db29-178">Static content</span></span>

<span data-ttu-id="5db29-179">在舊版 ASP.NET MVC 中，靜態內容裝載 web 專案的根目錄和伺服器端檔案已混合。</span><span class="sxs-lookup"><span data-stu-id="5db29-179">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="5db29-180">在 ASP.NET Core 靜態內容裝載於*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-180">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="5db29-181">您要複製舊 ASP.NET MVC 應用程式中的靜態內容*wwwroot* ASP.NET Core 專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="5db29-181">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="5db29-182">在這個範例轉換：</span><span class="sxs-lookup"><span data-stu-id="5db29-182">In this sample conversion:</span></span>

* <span data-ttu-id="5db29-183">複製*favicon.ico*檔案從舊的 MVC 專案至*wwwroot* ASP.NET Core 專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="5db29-183">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="5db29-184">舊的 ASP.NET MVC 專案會使用[Bootstrap](http://getbootstrap.com/)啟動安裝程式中的檔案其設定樣式和存放區的*內容*和*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-184">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="5db29-185">產生舊的 ASP.NET MVC 專案範本會參考配置檔案中的啟動程序 (*Views/Shared/_Layout.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="5db29-185">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="5db29-186">您無法複製*bootstrap.js*和*bootstrap.css*檔案從 ASP.NET MVC 專案加入*wwwroot*不會使用新的專案，而該方法中的資料夾改良的機制讓您管理 ASP.NET Core 中的用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="5db29-186">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="5db29-187">在新的專案中，我們會將啟動程序 （及其他用戶端程式庫） 使用支援新增[Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="5db29-187">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="5db29-188">新增[Bower](https://bower.io/)名為組態檔*bower.json*專案根目錄 (以滑鼠右鍵按一下專案，然後**新增 > 新的項目 > Bower 的組態檔**)。</span><span class="sxs-lookup"><span data-stu-id="5db29-188">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="5db29-189">新增[Bootstrap](http://getbootstrap.com/)和[jQuery](https://jquery.com/)檔案 （請參閱下列反白顯示的程式行）。</span><span class="sxs-lookup"><span data-stu-id="5db29-189">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="5db29-190">在儲存檔案，Bower 將會自動下載相依性， *wwwroot/lib*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-190">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="5db29-191">您可以使用**搜尋方案總管**方塊，即可尋找資產的路徑：</span><span class="sxs-lookup"><span data-stu-id="5db29-191">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![方案總管 中搜尋結果中顯示的 jquery 資產](mvc/_static/search.png)

<span data-ttu-id="5db29-193">請參閱[Bower 與管理用戶端封裝](../client-side/bower.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="5db29-193">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name=migrate-layout-file></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="5db29-194">移轉配置檔案</span><span class="sxs-lookup"><span data-stu-id="5db29-194">Migrate the layout file</span></span>

* <span data-ttu-id="5db29-195">複製*_viewstart.vbhtml*檔案從舊的 ASP.NET MVC 專案*檢視*ASP.NET Core 專案的資料夾*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-195">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="5db29-196">*_Viewstart.vbhtml* ASP.NET Core MVC 中未變更檔案。</span><span class="sxs-lookup"><span data-stu-id="5db29-196">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="5db29-197">建立*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-197">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="5db29-198">*選擇性：*複製*_ViewImports.cshtml*從*FullAspNetCore* MVC 專案*檢視*資料夾到 ASP.NET Core 專案*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-198">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="5db29-199">中的任何命名空間宣告中移除*_ViewImports.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="5db29-199">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="5db29-200">*_ViewImports.cshtml*檔案命名空間提供檢視的所有檔案，並使[標記協助程式](../mvc/views/tag-helpers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="5db29-200">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="5db29-201">新的版面配置檔中使用標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="5db29-201">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="5db29-202">*_ViewImports.cshtml*檔案是新的 ASP.NET core。</span><span class="sxs-lookup"><span data-stu-id="5db29-202">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="5db29-203">複製*_Layout.cshtml*檔案從舊的 ASP.NET MVC 專案*Views/Shared* ASP.NET Core 專案的資料夾*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5db29-203">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="5db29-204">開啟*_Layout.cshtml*檔案，然後進行下列變更 （已完成的程式碼如下所示）：</span><span class="sxs-lookup"><span data-stu-id="5db29-204">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="5db29-205">取代`@Styles.Render("~/Content/css")`與`<link>`載入的項目*bootstrap.css* （請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="5db29-205">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="5db29-206">移除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="5db29-206">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="5db29-207">標記為註解`@Html.Partial("_LoginPartial")`列 (周圍的線條`@*...*@`)。</span><span class="sxs-lookup"><span data-stu-id="5db29-207">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="5db29-208">我們將在未來的教學課程中傳回它。</span><span class="sxs-lookup"><span data-stu-id="5db29-208">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="5db29-209">取代`@Scripts.Render("~/bundles/jquery")`與`<script>`項目 （請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="5db29-209">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="5db29-210">取代`@Scripts.Render("~/bundles/bootstrap")`與`<script>`項目 （請參閱下文）...</span><span class="sxs-lookup"><span data-stu-id="5db29-210">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="5db29-211">取代 CSS 連結：</span><span class="sxs-lookup"><span data-stu-id="5db29-211">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="5db29-212">取代指令碼標記中：</span><span class="sxs-lookup"><span data-stu-id="5db29-212">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="5db29-213">已更新*_Layout.cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="5db29-213">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="5db29-214">在瀏覽器中檢視站台。</span><span class="sxs-lookup"><span data-stu-id="5db29-214">View the site in the browser.</span></span> <span data-ttu-id="5db29-215">它應該現在正確載入，以就地預期的樣式。</span><span class="sxs-lookup"><span data-stu-id="5db29-215">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="5db29-216">*選擇性：*要再試一次使用新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="5db29-216">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="5db29-217">這個專案的版面配置檔案可以複製*FullAspNetCore*專案。</span><span class="sxs-lookup"><span data-stu-id="5db29-217">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="5db29-218">新的版面配置檔案使用[標記協助程式](../mvc/views/tag-helpers/index.md)且具有其他改進功能。</span><span class="sxs-lookup"><span data-stu-id="5db29-218">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="5db29-219">設定結合在一起 （& s) 縮製</span><span class="sxs-lookup"><span data-stu-id="5db29-219">Configure Bundling & Minification</span></span>

<span data-ttu-id="5db29-220">如需如何設定統合及縮製的資訊，請參閱[組合和縮製](../client-side/bundling-and-minification.md)。</span><span class="sxs-lookup"><span data-stu-id="5db29-220">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="5db29-221">解決 HTTP 500 錯誤</span><span class="sxs-lookup"><span data-stu-id="5db29-221">Solving HTTP 500 errors</span></span>

<span data-ttu-id="5db29-222">有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題的來源上的任何資訊。</span><span class="sxs-lookup"><span data-stu-id="5db29-222">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="5db29-223">例如，如果*Views/_ViewImports.cshtml*檔案包含不存在於您的專案命名空間，您會收到 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="5db29-223">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="5db29-224">若要取得詳細的錯誤訊息，請加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5db29-224">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="5db29-225">請參閱**使用開發人員例外狀況頁面**中[錯誤處理](../fundamentals/error-handling.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="5db29-225">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5db29-226">其他資源</span><span class="sxs-lookup"><span data-stu-id="5db29-226">Additional Resources</span></span>

* [<span data-ttu-id="5db29-227">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="5db29-227">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="5db29-228">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="5db29-228">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
