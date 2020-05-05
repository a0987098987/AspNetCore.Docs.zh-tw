---
title: 從 ASP.NET MVC 移轉至 ASP.NET Core MVC
author: ardalis
description: 瞭解如何開始將 ASP.NET MVC 專案遷移至 ASP.NET Core MVC。
ms.author: riande
ms.date: 04/06/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/mvc
ms.openlocfilehash: 59a10c002958e5f719dbd59686f21df69da5f43e
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777042"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="a3cb8-103">從 ASP.NET MVC 移轉至 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a3cb8-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="a3cb8-104">作者： [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="a3cb8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="a3cb8-105">本文說明如何開始將 ASP.NET MVC 專案遷移至[ASP.NET CORE mvc](../mvc/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="a3cb8-106">在此過程中，它強調了許多已從 ASP.NET MVC 變更的專案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="a3cb8-107">從 ASP.NET MVC 遷移是一個多步驟程式，本文涵蓋初始設定、基本控制器和視圖、靜態內容和用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="a3cb8-108">其他文章涵蓋了在許多 ASP.NET MVC 專案中找到的遷移設定和識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="a3cb8-109">範例中的版本號碼可能不是最新的。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="a3cb8-110">您可能需要據以更新您的專案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="a3cb8-111">建立 starter ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="a3cb8-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="a3cb8-112">為了示範升級，我們將從建立 ASP.NET MVC 應用程式開始。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="a3cb8-113">使用名稱*WebApp1*建立它，讓命名空間符合我們在下一個步驟中建立的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio 的 [新增專案] 對話方塊](mvc/_static/new-project.png)

![[新增 Web 應用程式] 對話方塊：已在 [ASP.NET 範本] 面板中選取 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="a3cb8-116">*選擇性：* 將解決方案的名稱從*WebApp1*變更為*Mvc5*。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="a3cb8-117">Visual Studio 會顯示新的方案名稱（*Mvc5*），讓您更輕鬆地從下一個專案告訴此專案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="a3cb8-118">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="a3cb8-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="a3cb8-119">使用與前一個專案相同的名稱建立新的*空白*ASP.NET Core web 應用程式（*WebApp1*），讓這兩個專案中的命名空間相符。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="a3cb8-120">擁有相同的命名空間可讓您更輕鬆地在這兩個專案之間複製程式碼。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="a3cb8-121">您必須在與前一個專案不同的目錄中建立此專案，以使用相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![[新增專案] 對話方塊](mvc/_static/new_core.png)

![[新增 ASP.NET Web 應用程式] 對話方塊：在 ASP.NET Core 範本] 面板中選取了空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="a3cb8-124">*選擇性：* 使用*Web 應用程式*專案範本建立新的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="a3cb8-125">將專案命名為*WebApp1*，然後選取**個別使用者帳戶**的驗證選項。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="a3cb8-126">將此應用程式重新命名為*FullAspNetCore*。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="a3cb8-127">建立這個專案可讓您省下轉換的時間。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="a3cb8-128">您可以查看範本產生的程式碼，以查看最終結果，或將程式碼複製到轉換專案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="a3cb8-129">當您停滯在轉換步驟，與範本產生的專案進行比較時，這也很有説明。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="a3cb8-130">將網站設定為使用 MVC</span><span class="sxs-lookup"><span data-stu-id="a3cb8-130">Configure the site to use MVC</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="a3cb8-131">以 .NET Core 為目標時，預設會參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-131">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="a3cb8-132">此套件包含 MVC 應用程式經常使用的套件。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-132">This package contains packages commonly used by MVC apps.</span></span> <span data-ttu-id="a3cb8-133">如果目標 .NET Framework，則套件參考必須個別列在專案檔中。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-133">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="a3cb8-134">以 .NET Core 為目標時，預設會參考[AspNetCore。](xref:fundamentals/metapackage)</span><span class="sxs-lookup"><span data-stu-id="a3cb8-134">When targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) is referenced by default.</span></span> <span data-ttu-id="a3cb8-135">此套件包含 MVC 應用程式常用的套件套件。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-135">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="a3cb8-136">如果目標 .NET Framework，則套件參考必須個別列在專案檔中。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-136">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="a3cb8-137">以 .NET Core 或 .NET Framework 為目標時，MVC 應用程式的套件常用封裝會分別列在專案檔中。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-137">When targeting .NET Core or .NET Framework, packages commonly used packages by MVC apps are listed individually in the project file.</span></span>

::: moniker-end

<span data-ttu-id="a3cb8-138">`Microsoft.AspNetCore.Mvc`是 ASP.NET Core MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-138">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="a3cb8-139">`Microsoft.AspNetCore.StaticFiles`這是靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-139">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="a3cb8-140">ASP.NET Core 執行時間是模組化的，而且您必須明確加入宣告以提供靜態檔案（請參閱[靜態](xref:fundamentals/static-files)檔案）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-140">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="a3cb8-141">開啟*Startup.cs*檔案，並變更程式碼以符合下列內容：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-141">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="a3cb8-142">`UseStaticFiles`擴充方法會加入靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-142">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="a3cb8-143">如先前所述，ASP.NET 執行時間是模組化的，您必須明確加入宣告靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-143">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="a3cb8-144">`UseMvc`擴充方法會新增路由。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-144">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="a3cb8-145">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)和[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-145">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="a3cb8-146">新增控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="a3cb8-146">Add a controller and view</span></span>

<span data-ttu-id="a3cb8-147">在本節中，您將新增最少的控制器和視圖，做為 ASP.NET MVC 控制器的預留位置，以及您將在下一節中遷移的視圖。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-147">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="a3cb8-148">新增 [*控制器*] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-148">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="a3cb8-149">將名為*HomeController.cs*的**控制器類別**新增至 [*控制器*] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-149">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="a3cb8-151">加入*Views*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-151">Add a *Views* folder.</span></span>

* <span data-ttu-id="a3cb8-152">新增*Views/Home*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-152">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="a3cb8-153">將名為*Index. cshtml*的\*\* Razor視圖\**新增至*Views/Home\*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-153">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![[新增項目] 對話方塊](mvc/_static/view.png)

<span data-ttu-id="a3cb8-155">專案結構如下所示：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-155">The project structure is shown below:</span></span>

![方案總管顯示 WebApp1 的檔案和資料夾](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="a3cb8-157">以下列內容取代*Views/Home/Index. cshtml*檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-157">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="a3cb8-158">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-158">Run the app.</span></span>

![在 Microsoft Edge 中開啟 Web 應用程式](mvc/_static/hello-world.png)

<span data-ttu-id="a3cb8-160">如需詳細資訊，請參閱[控制器](xref:mvc/controllers/actions)和[Views](xref:mvc/views/overview) 。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-160">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="a3cb8-161">既然我們已有最小的工作 ASP.NET Core 專案，我們就可以開始從 ASP.NET MVC 專案遷移功能。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-161">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="a3cb8-162">我們需要移動下列各項：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-162">We need to move the following:</span></span>

* <span data-ttu-id="a3cb8-163">用戶端內容（CSS、字型和腳本）</span><span class="sxs-lookup"><span data-stu-id="a3cb8-163">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="a3cb8-164">controllers</span><span class="sxs-lookup"><span data-stu-id="a3cb8-164">controllers</span></span>

* <span data-ttu-id="a3cb8-165">檢視</span><span class="sxs-lookup"><span data-stu-id="a3cb8-165">views</span></span>

* <span data-ttu-id="a3cb8-166">模型</span><span class="sxs-lookup"><span data-stu-id="a3cb8-166">models</span></span>

* <span data-ttu-id="a3cb8-167">統合</span><span class="sxs-lookup"><span data-stu-id="a3cb8-167">bundling</span></span>

* <span data-ttu-id="a3cb8-168">filters</span><span class="sxs-lookup"><span data-stu-id="a3cb8-168">filters</span></span>

* <span data-ttu-id="a3cb8-169">登入/登出Identity （這會在下一個教學課程中完成）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-169">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="a3cb8-170">控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="a3cb8-170">Controllers and views</span></span>

* <span data-ttu-id="a3cb8-171">將 ASP.NET MVC `HomeController`中的每個方法複製到新`HomeController`的。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-171">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="a3cb8-172">請注意，在 ASP.NET MVC 中，內建範本的控制器動作方法傳回類型為[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx);在 ASP.NET Core MVC 中，動作方法會`IActionResult`改為傳回。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-172">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="a3cb8-173">`ActionResult`會`IActionResult`執行，因此不需要變更動作方法的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-173">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="a3cb8-174">將 [*關於*ASP.NET MVC] 專案中的 [About]、[ *Contact*] 和 [ *Index. cshtml* Razor ] 檔案複製到 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-174">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="a3cb8-175">執行 ASP.NET Core 應用程式，並測試每個方法。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-175">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="a3cb8-176">我們尚未遷移版面配置檔案或樣式，因此轉譯的視圖只會包含視圖檔案中的內容。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-176">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="a3cb8-177">您不會有`About`和`Contact` views 的配置檔案產生連結，因此您必須從瀏覽器叫用（以專案中使用的埠號碼取代**4492** ）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-177">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

<span data-ttu-id="a3cb8-179">請注意，缺少樣式和功能表項目。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-179">Note the lack of styling and menu items.</span></span> <span data-ttu-id="a3cb8-180">我們將在下節修正該問題。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-180">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="a3cb8-181">靜態內容</span><span class="sxs-lookup"><span data-stu-id="a3cb8-181">Static content</span></span>

<span data-ttu-id="a3cb8-182">在舊版的 ASP.NET MVC 中，靜態內容是由 Web 專案的根目錄所裝載，並且與伺服器端檔案混合使用。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-182">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="a3cb8-183">在 ASP.NET Core 中，靜態內容會裝載在*wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-183">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="a3cb8-184">您會想要將靜態內容從舊的 ASP.NET MVC 應用程式複製到 ASP.NET Core 專案中的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-184">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="a3cb8-185">在此範例轉換中：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-185">In this sample conversion:</span></span>

* <span data-ttu-id="a3cb8-186">將*favicon*檔案從舊的 MVC 專案複製到 ASP.NET Core 專案中的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-186">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="a3cb8-187">舊的 ASP.NET MVC 專案會使用[啟動](https://getbootstrap.com/)程式來進行其樣式設定，並將啟動載入器檔案儲存在*內容*和*腳本*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-187">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="a3cb8-188">產生舊 ASP.NET MVC 專案的範本會參考配置檔案中的啟動程式（*Views/Shared/_Layout. cshtml*）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-188">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="a3cb8-189">您可以將 ASP.NET MVC 專案中的*啟動*載入器和*啟動程式 .css*檔案複製到新專案中的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-189">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="a3cb8-190">相反地，我們會在下一節中使用 Cdn 來新增對啟動程式（和其他用戶端程式庫）的支援。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-190">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="a3cb8-191">遷移版面配置檔案</span><span class="sxs-lookup"><span data-stu-id="a3cb8-191">Migrate the layout file</span></span>

* <span data-ttu-id="a3cb8-192">從舊的 ASP.NET MVC 專案的*views*資料夾，將 *_ViewStart. cshtml*檔案複製到 ASP.NET Core 專案的 [ *views* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-192">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="a3cb8-193">ASP.NET Core MVC 中的 *_ViewStart. cshtml*檔案尚未變更。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-193">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="a3cb8-194">建立*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-194">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="a3cb8-195">*選擇性：* 將*FullAspNetCore* MVC 專案的*views*資料夾中的 *_ViewImports. cshtml*複製到 ASP.NET Core 專案的*views*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-195">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="a3cb8-196">移除 *_ViewImports. cshtml*檔案中的任何命名空間宣告。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-196">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a3cb8-197">*_ViewImports 的 cshtml*檔案提供所有視圖檔案的命名空間，並帶入標籤協助[程式。](xref:mvc/views/tag-helpers/intro)</span><span class="sxs-lookup"><span data-stu-id="a3cb8-197">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="a3cb8-198">標籤協助程式會用於新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-198">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="a3cb8-199">*_ViewImports 的 cshtml*檔案是 ASP.NET Core 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-199">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="a3cb8-200">從舊的 ASP.NET MVC 專案的*views/shared*資料夾，將 *_Layout. cshtml*檔案複製到 ASP.NET Core 專案的*views/shared*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-200">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="a3cb8-201">開啟 *_Layout 的 cshtml*檔案，並進行下列變更（完成的程式碼如下所示）：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-201">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="a3cb8-202">取代`@Styles.Render("~/Content/css")`為要`<link>`載入*啟動*程式的元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-202">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="a3cb8-203">移除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-203">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="a3cb8-204">將`@Html.Partial("_LoginPartial")`行標記為批註（以`@*...*@`括住行）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-204">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="a3cb8-205">如需詳細資訊，請參閱[遷移驗證和Identity ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="a3cb8-205">For more information see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="a3cb8-206">取代`@Scripts.Render("~/bundles/jquery")`為`<script>`元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-206">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="a3cb8-207">取代`@Scripts.Render("~/bundles/bootstrap")`為`<script>`元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-207">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="a3cb8-208">啟動載入 CSS 的取代標記包含：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-208">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="a3cb8-209">JQuery 和啟動程式 JavaScript 包含的取代標記：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-209">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="a3cb8-210">更新後的 *_Layout. cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-210">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="a3cb8-211">在瀏覽器中觀看網站。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-211">View the site in the browser.</span></span> <span data-ttu-id="a3cb8-212">現在應該會正確地載入，且應具備預期的樣式。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-212">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="a3cb8-213">*選擇性：* 您可能想要嘗試使用新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-213">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="a3cb8-214">針對此專案，您可以從*FullAspNetCore*專案複製版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-214">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="a3cb8-215">新的設定檔案[會使用卷](xref:mvc/views/tag-helpers/intro)標協助程式，並具有其他改良功能。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-215">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="a3cb8-216">設定捆綁和縮制</span><span class="sxs-lookup"><span data-stu-id="a3cb8-216">Configure bundling and minification</span></span>

<span data-ttu-id="a3cb8-217">如需有關如何設定配套和縮制的詳細資訊，請參閱[捆綁和縮制](../client-side/bundling-and-minification.md)。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-217">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="a3cb8-218">解決 HTTP 500 錯誤</span><span class="sxs-lookup"><span data-stu-id="a3cb8-218">Solve HTTP 500 errors</span></span>

<span data-ttu-id="a3cb8-219">有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題來源的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-219">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="a3cb8-220">例如，如果*Views/_ViewImports. cshtml*檔案包含的命名空間不存在於您的專案中，您將會收到 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-220">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="a3cb8-221">根據預設，在 ASP.NET Core 應用程式`UseDeveloperExceptionPage`中，延伸模組會`IApplicationBuilder`新增至，並在設定為*開發*時執行。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-221">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="a3cb8-222">下列程式碼將詳細說明這一點：</span><span class="sxs-lookup"><span data-stu-id="a3cb8-222">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="a3cb8-223">ASP.NET Core 會將 web 應用程式中未處理的例外狀況轉換成 HTTP 500 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-223">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="a3cb8-224">一般來說，錯誤詳細資料不會包含在這些回應中，以防止洩漏伺服器的潛在敏感性資訊。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-224">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="a3cb8-225">如需詳細資訊，請參閱在[處理錯誤](../fundamentals/error-handling.md)中**使用開發人員例外狀況頁面**。</span><span class="sxs-lookup"><span data-stu-id="a3cb8-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3cb8-226">其他資源</span><span class="sxs-lookup"><span data-stu-id="a3cb8-226">Additional resources</span></span>

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
