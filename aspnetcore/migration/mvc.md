---
title: 瞭解如何從 ASP.NET MVC 遷移至 ASP.NET Core MVC
author: wadepickett
description: 瞭解如何開始將 ASP.NET MVC 專案遷移至 ASP.NET Core MVC。
ms.author: wpickett
ms.date: 06/18/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/mvc
ms.openlocfilehash: 6a645d0e5959b4301ee7d2bcfc692f7499574dc4
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407319"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="d1ad4-103">從 ASP.NET MVC 移轉至 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="d1ad4-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d1ad4-104">本文說明如何開始將 ASP.NET MVC 專案遷移至[ASP.NET CORE mvc](xref:mvc/overview)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-104">This article shows how to start migrating an ASP.NET MVC project to [ASP.NET Core MVC](xref:mvc/overview).</span></span> <span data-ttu-id="d1ad4-105">在此過程中，它會反白顯示 ASP.NET MVC 的相關變更。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-105">In the process, it highlights related changes from ASP.NET MVC.</span></span>

<span data-ttu-id="d1ad4-106">從 ASP.NET MVC 進行遷移是一個多步驟的程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-106">Migrating from ASP.NET MVC is a multi-step process.</span></span> <span data-ttu-id="d1ad4-107">本文將說明：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-107">This article covers:</span></span>

* <span data-ttu-id="d1ad4-108">初始設定。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-108">Initial setup.</span></span>
* <span data-ttu-id="d1ad4-109">基本控制器和 views。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-109">Basic controllers and views.</span></span>
* <span data-ttu-id="d1ad4-110">靜態內容。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-110">Static content.</span></span>
* <span data-ttu-id="d1ad4-111">用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-111">Client-side dependencies.</span></span>

<span data-ttu-id="d1ad4-112">如需遷移設定和程式 Identity 代碼，請參閱[將設定遷移至 ASP.NET Core](xref:migration/configuration)並[遷移驗證和 Identity ASP.NET Core](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-112">For migrating configuration and Identity code, see [Migrate configuration to ASP.NET Core](xref:migration/configuration) and [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1ad4-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="d1ad4-113">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs-3.1.md)]

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="d1ad4-114">建立 starter ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-114">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="d1ad4-115">在 Visual Studio 中建立要遷移的範例 ASP.NET MVC 專案：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-115">Create an example ASP.NET MVC project in Visual Studio to migrate:</span></span>

1. <span data-ttu-id="d1ad4-116">從 [檔案]\*\*\*\* 功能表選取 [新增]**[專案]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-116">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="d1ad4-117">選取 [ **ASP.NET Web 應用程式（.NET Framework）** ]，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-117">Select **ASP.NET Web Application (.NET Framework)** and then select **Next**.</span></span>
1. <span data-ttu-id="d1ad4-118">將專案命名為*WebApp1* ，使命名空間符合下一個步驟中建立的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-118">Name the project *WebApp1* so the namespace matches the ASP.NET Core project created in the next step.</span></span> <span data-ttu-id="d1ad4-119">選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-119">Select **Create**.</span></span>
1. <span data-ttu-id="d1ad4-120">選取 [ **MVC**]，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-120">Select **MVC**, and then select **Create**.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="d1ad4-121">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-121">Create the ASP.NET Core project</span></span>

<span data-ttu-id="d1ad4-122">使用要遷移至的新 ASP.NET Core 專案來建立新的方案：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-122">Create a new solution with a new ASP.NET Core project to migrate to:</span></span>

1. <span data-ttu-id="d1ad4-123">啟動 Visual Studio 的第二個實例。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-123">Launch a second instance of Visual Studio.</span></span>
1. <span data-ttu-id="d1ad4-124">從 [檔案]\*\*\*\* 功能表選取 [新增]**[專案]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-124">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="d1ad4-125">選取 [ **ASP.NET Web Core Web 應用程式**]，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-125">Select **ASP.NET Web Core Web Application** and then select **Next**.</span></span>
1. <span data-ttu-id="d1ad4-126">在 [**設定您的新專案**] 對話方塊中，將專案命名為*WebApp1*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-126">In the **Configure your new project** dialog, Name the project *WebApp1*.</span></span>
1. <span data-ttu-id="d1ad4-127">將 [位置] 設定為與前一個專案不同的目錄，以使用相同的專案名稱。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-127">Set the location to a different directory than the previous project to use the same project name.</span></span> <span data-ttu-id="d1ad4-128">使用相同的命名空間，可讓您更輕鬆地在這兩個專案之間複製程式碼。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-128">Using the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="d1ad4-129">選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-129">Select **Create**.</span></span>
1. <span data-ttu-id="d1ad4-130">在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中，確認已選取 [ **.net Core** ] 和 [ **ASP.NET Core 3.1** ]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-130">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="d1ad4-131">選取 [ **Web 應用程式（模型-視圖控制器）** ] 專案範本，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-131">Select the **Web Application (Model-View-Controller)** project template, and select **Create**.</span></span>

## <a name="configure-the-aspnet-core-site-to-use-mvc"></a><span data-ttu-id="d1ad4-132">將 ASP.NET Core 網站設定為使用 MVC</span><span class="sxs-lookup"><span data-stu-id="d1ad4-132">Configure the ASP.NET Core site to use MVC</span></span>

<span data-ttu-id="d1ad4-133">在 ASP.NET Core 3.0 和更新版本的專案中，.NET Framework 不再是受支援的目標 Framework。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-133">In ASP.NET Core 3.0 and later projects, .NET Framework is no longer a supported target framework.</span></span> <span data-ttu-id="d1ad4-134">您的專案必須以 .NET Core 為目標。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-134">Your project must target .NET Core.</span></span> <span data-ttu-id="d1ad4-135">包含 MVC 的 ASP.NET Core 共用架構是 .NET Core 執行時間安裝的一部分。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-135">The ASP.NET Core shared framework, which includes MVC, is part of the .NET Core runtime installation.</span></span> <span data-ttu-id="d1ad4-136">在專案檔中使用 SDK 時，會自動參考共用架構 `Microsoft.NET.Sdk.Web` ：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-136">The shared framework is automatically referenced when using the `Microsoft.NET.Sdk.Web` SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="d1ad4-137">如需詳細資訊，請參閱[架構參考](xref:migration/22-to-30#framework-reference)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-137">For more information, see [Framework reference](xref:migration/22-to-30#framework-reference).</span></span>

<span data-ttu-id="d1ad4-138">在 ASP.NET Core 中， `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-138">In ASP.NET Core, the `Startup` class:</span></span>

* <span data-ttu-id="d1ad4-139">取代*global.asax*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-139">Replaces *Global.asax*.</span></span>
* <span data-ttu-id="d1ad4-140">處理所有應用程式啟動工作。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-140">Handles all app startup tasks.</span></span>

<span data-ttu-id="d1ad4-141">如需詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-141">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="d1ad4-142">在 ASP.NET Core 專案中，開啟*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-142">In the ASP.NET Core project, open the *Startup.cs* file:</span></span>

[!code-csharp[](mvc/samples/3.x/Startup.cs?highlight=13,30,32&name=snippet)]

<span data-ttu-id="d1ad4-143">ASP.NET Core 應用程式必須使用中介軟體加入宣告架構功能。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-143">ASP.NET Core apps must opt in to framework features with middleware.</span></span> <span data-ttu-id="d1ad4-144">先前範本產生的程式碼會新增下列服務和中介軟體：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-144">The previous template-generated code adds the following services and middleware:</span></span>

* <span data-ttu-id="d1ad4-145"><xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddControllersWithViews%2A>擴充方法會針對控制器、API 相關的功能和 views 註冊 MVC 服務支援。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-145">The <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddControllersWithViews%2A> extension method registers MVC service support for controllers, API-related features, and views.</span></span> <span data-ttu-id="d1ad4-146">如需 MVC 服務註冊選項的詳細資訊，請參閱[mvc 服務註冊](xref:migration/22-to-30#mvc-service-registration)</span><span class="sxs-lookup"><span data-stu-id="d1ad4-146">For more information on MVC service registration options, see [MVC service registration](xref:migration/22-to-30#mvc-service-registration)</span></span>
* <span data-ttu-id="d1ad4-147"><xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A>擴充方法會加入靜態檔案處理常式 `Microsoft.AspNetCore.StaticFiles` 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-147">The <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A> extension method adds the static file handler `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="d1ad4-148">`UseStaticFiles`必須先呼叫擴充方法 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-148">The `UseStaticFiles` extension method must be called before `UseRouting`.</span></span> <span data-ttu-id="d1ad4-149">如需詳細資訊，請參閱 <xref:fundamentals/static-files> 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-149">For more information, see <xref:fundamentals/static-files>.</span></span>
* <span data-ttu-id="d1ad4-150"><xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting%2A>擴充方法會新增路由。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-150">The <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting%2A> extension method adds routing.</span></span> <span data-ttu-id="d1ad4-151">如需詳細資訊，請參閱 <xref:fundamentals/routing> 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-151">For more information, see <xref:fundamentals/routing>.</span></span>

<span data-ttu-id="d1ad4-152">此現有設定包含遷移範例 ASP.NET MVC 專案所需的功能。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-152">This existing configuration includes what is needed to migrate the example ASP.NET MVC project.</span></span> <span data-ttu-id="d1ad4-153">如需 ASP.NET Core 中介軟體選項的詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-153">For more information on ASP.NET Core middleware options, see <xref:fundamentals/startup>.</span></span>

## <a name="migrate-controllers-and-views"></a><span data-ttu-id="d1ad4-154">遷移控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="d1ad4-154">Migrate controllers and views</span></span>

<span data-ttu-id="d1ad4-155">在 ASP.NET Core 專案中，會加入新的空白控制器類別和 view 類別，做為預留位置，其使用與任何 ASP.NET MVC 專案中的控制器和 view 類別相同的名稱來進行遷移。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-155">In the ASP.NET Core project, a new empty controller class and view class would be added to serve as placeholders using the same names as the controller and view classes in any ASP.NET MVC project to migrate from.</span></span>

<span data-ttu-id="d1ad4-156">ASP.NET Core *WebApp1*專案已包含與 ASP.NET MVC 專案相同名稱的最低範例控制器和視圖。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-156">The ASP.NET Core *WebApp1* project already includes a minimal example controller and view by the same name as the ASP.NET MVC project.</span></span> <span data-ttu-id="d1ad4-157">因此，這些會作為 ASP.NET MVC 控制器的預留位置，以及要從 ASP.NET MVC *WebApp1*專案中遷移的 views。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-157">So those will serve as placeholders for the ASP.NET MVC controller and views to be migrated from the ASP.NET MVC *WebApp1* project.</span></span>

1. <span data-ttu-id="d1ad4-158">複製 ASP.NET MVC 中的方法 `HomeController` ，以取代新的 ASP.NET Core `HomeController` 方法。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-158">Copy the methods from the ASP.NET MVC `HomeController` to replace the new ASP.NET Core `HomeController` methods.</span></span> <span data-ttu-id="d1ad4-159">不需要變更動作方法的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-159">There's no need to change the return type of the action methods.</span></span> <span data-ttu-id="d1ad4-160">ASP.NET MVC 內建範本的控制器動作方法傳回類型為[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx);在 ASP.NET Core MVC 中，動作方法會 `IActionResult` 改為傳回。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-160">The ASP.NET MVC built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="d1ad4-161">`ActionResult` 會實作 `IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-161">`ActionResult` implements `IActionResult`.</span></span>
1. <span data-ttu-id="d1ad4-162">在 ASP.NET Core 專案中，以滑鼠右鍵按一下 [ *Views]/[Home* ] 目錄，然後選取 [**加入** > **現有專案**]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-162">In the ASP.NET Core project, right-click the *Views/Home* directory, select **Add** > **Existing Item**.</span></span>
1. <span data-ttu-id="d1ad4-163">在 [**加入現有專案**] 對話方塊中，流覽至 ASP.NET MVC *WebApp1*專案的*Views/Home*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-163">In the **Add Existing Item** dialog, navigate to the ASP.NET MVC *WebApp1* project's *Views/Home* directory.</span></span>
1. <span data-ttu-id="d1ad4-164">選取 [ *About*]、[ *Contact*] 和 [ *Index.* cshtml] 檔案 Razor ，然後選取 [**新增**]，取代現有的檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-164">Select the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files, then select **Add**, replacing the existing files.</span></span>

<span data-ttu-id="d1ad4-165">如需詳細資訊，請參閱 <xref:mvc/controllers/actions> 和 <xref:mvc/views/overview>。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-165">For more information, see <xref:mvc/controllers/actions> and <xref:mvc/views/overview>.</span></span>

## <a name="test-each-method"></a><span data-ttu-id="d1ad4-166">測試每個方法</span><span class="sxs-lookup"><span data-stu-id="d1ad4-166">Test each method</span></span>

<span data-ttu-id="d1ad4-167">您可以測試每個控制器端點，不過，檔稍後會涵蓋版面配置和樣式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-167">Each controller endpoint can be tested, however, layout and styles are covered later in the document.</span></span>

1. <span data-ttu-id="d1ad4-168">執行 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-168">Run the ASP.NET Core app.</span></span>
1. <span data-ttu-id="d1ad4-169">藉由以 ASP.NET Core 專案中使用的埠號碼取代目前的埠號碼，在執行中的 ASP.NET Core 應用程式上從瀏覽器叫用呈現的視圖。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-169">Invoke the rendered views from the browser on the running ASP.NET Core app by replacing the current port number with the port number used in the ASP.NET Core project.</span></span> <span data-ttu-id="d1ad4-170">例如： `https://localhost:44375/home/about` 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-170">For example, `https://localhost:44375/home/about`.</span></span>

## <a name="migrate-static-content"></a><span data-ttu-id="d1ad4-171">遷移靜態內容</span><span class="sxs-lookup"><span data-stu-id="d1ad4-171">Migrate static content</span></span>

<span data-ttu-id="d1ad4-172">在 ASP.NET MVC 5 和更早版本中，靜態內容是由 Web 專案的根目錄所主控，並與伺服器端檔案混合。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-172">In ASP.NET MVC 5 and earlier, static content was hosted from the web project's root directory and was intermixed with server-side files.</span></span> <span data-ttu-id="d1ad4-173">在 ASP.NET Core 中，靜態檔案會儲存在專案的[web 根目錄](xref:fundamentals/index#web-root)中。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-173">In ASP.NET Core, static files are stored within the project's [web root](xref:fundamentals/index#web-root) directory.</span></span> <span data-ttu-id="d1ad4-174">預設目錄是 *{content root}/wwwroot*，但可以變更。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-174">The default directory is *{content root}/wwwroot*, but it can be changed.</span></span> <span data-ttu-id="d1ad4-175">如需詳細資訊，請參閱 [ASP.NET Core 中的靜態檔案](xref:fundamentals/static-files#serve-static-files)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-175">For more information, see [Static files in ASP.NET Core](xref:fundamentals/static-files#serve-static-files).</span></span>

<span data-ttu-id="d1ad4-176">將 ASP.NET MVC *WebApp1*專案中的靜態內容複寫到 ASP.NET Core *WebApp1*專案中的*wwwroot*目錄：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-176">Copy the static content from the ASP.NET MVC *WebApp1* project to the *wwwroot* directory in the ASP.NET Core *WebApp1* project:</span></span>

1. <span data-ttu-id="d1ad4-177">在 ASP.NET Core 專案中，以滑鼠右鍵按一下 [ *wwwroot* ] 目錄，然後選取 [**加入** > **現有專案**]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-177">In the ASP.NET Core project, right-click the *wwwroot* directory, select **Add** > **Existing Item**.</span></span>
1. <span data-ttu-id="d1ad4-178">在 [**加入現有專案**] 對話方塊中，流覽至 ASP.NET MVC *WebApp1*專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-178">In the **Add Existing Item** dialog, navigate to the ASP.NET MVC *WebApp1* project.</span></span>
1. <span data-ttu-id="d1ad4-179">選取 [ *favicon* ] 檔案，然後選取 [**新增**]，取代現有的檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-179">Select the *favicon.ico* file, then select **Add**, replacing the existing file.</span></span>

## <a name="migrate-the-layout-files"></a><span data-ttu-id="d1ad4-180">遷移版面配置檔案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-180">Migrate the layout files</span></span>

<span data-ttu-id="d1ad4-181">將 ASP.NET MVC 專案版面配置檔案複製到 ASP.NET Core 專案：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-181">Copy the ASP.NET MVC project layout files to the ASP.NET Core project:</span></span>

1. <span data-ttu-id="d1ad4-182">在 ASP.NET Core 專案中，以滑鼠右鍵按一下*Views*目錄，然後選取 [**加入** > **現有專案**]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-182">In the ASP.NET Core project, right-click the *Views* directory, select **Add** > **Existing Item**.</span></span>
1. <span data-ttu-id="d1ad4-183">在 [**加入現有專案**] 對話方塊中，流覽至 ASP.NET MVC *WebApp1*專案的*Views*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-183">In the **Add Existing Item** dialog, navigate to the ASP.NET MVC *WebApp1* project's *Views* directory.</span></span>
1. <span data-ttu-id="d1ad4-184">選取 [ *_ViewStart* ] 檔案，然後選取 [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-184">Select the *_ViewStart.cshtml* file then select **Add**.</span></span>

<span data-ttu-id="d1ad4-185">將 ASP.NET MVC 專案共用版面配置檔案複製到 ASP.NET Core 專案：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-185">Copy the ASP.NET MVC project shared layout files to the ASP.NET Core project:</span></span>

1. <span data-ttu-id="d1ad4-186">在 ASP.NET Core 專案中，以滑鼠右鍵按一下*Views/Shared*目錄，然後選取 [**新增**] [ > **現有專案**]。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-186">In the ASP.NET Core project, right-click the *Views/Shared* directory, select **Add** > **Existing Item**.</span></span>
1. <span data-ttu-id="d1ad4-187">在 [**加入現有專案**] 對話方塊中，流覽至 ASP.NET MVC *WebApp1*專案的*Views/Shared*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-187">In the **Add Existing Item** dialog, navigate to the ASP.NET MVC *WebApp1* project's *Views/Shared* directory.</span></span>
1. <span data-ttu-id="d1ad4-188">選取 [ *_Layout* ] 檔案，然後選取 [**新增**]，取代現有的檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-188">Select the *_Layout.cshtml* file, then select **Add**, replacing the existing file.</span></span>

<span data-ttu-id="d1ad4-189">在 ASP.NET Core 專案中，開啟 *_Layout. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-189">In the ASP.NET Core project, open the *_Layout.cshtml* file.</span></span> <span data-ttu-id="d1ad4-190">進行下列變更，以符合如下所示的已完成程式碼：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-190">Make the following changes to match the completed code shown below:</span></span>

<span data-ttu-id="d1ad4-191">更新啟動載入 CSS 包含，以符合下列已完成的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-191">Update the Bootstrap CSS inclusion to match the completed code below:</span></span>

1. <span data-ttu-id="d1ad4-192">取代為 `@Styles.Render("~/Content/css")` `<link>` 要載入*啟動*程式的元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-192">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>
1. <span data-ttu-id="d1ad4-193">移除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-193">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

<span data-ttu-id="d1ad4-194">啟動載入 CSS 的已完成取代標記：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-194">The completed replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="d1ad4-195">更新 jQuery 和啟動程式 JavaScript 包含，以符合下列已完成的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-195">Update the jQuery and Bootstrap JavaScript inclusion to match the completed code below:</span></span>

1. <span data-ttu-id="d1ad4-196">取代為 `@Scripts.Render("~/bundles/jquery")` `<script>` 元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-196">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>
1. <span data-ttu-id="d1ad4-197">取代為 `@Scripts.Render("~/bundles/bootstrap")` `<script>` 元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-197">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="d1ad4-198">JQuery 和啟動程式 JavaScript 包含的已完成取代標記：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-198">The completed replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="d1ad4-199">更新後的 *_Layout. cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-199">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/samples/3.x/Views/Shared/_Layout.cshtml?highlight=7-10,40-42)]

<span data-ttu-id="d1ad4-200">在瀏覽器中觀看網站。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-200">View the site in the browser.</span></span> <span data-ttu-id="d1ad4-201">它應該會以預期的樣式進行轉譯。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-201">It should render with the expected styles in place.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="d1ad4-202">設定捆綁和縮制</span><span class="sxs-lookup"><span data-stu-id="d1ad4-202">Configure bundling and minification</span></span>

<span data-ttu-id="d1ad4-203">ASP.NET Core 與數個開放原始碼的包裝和縮制解決方案（例如[WebOptimizer](https://github.com/ligershark/WebOptimizer)和其他類似的程式庫）相容。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-203">ASP.NET Core is compatible with several open-source bundling and minification solutions such as [WebOptimizer](https://github.com/ligershark/WebOptimizer) and other similar libraries.</span></span> <span data-ttu-id="d1ad4-204">ASP.NET Core 不提供原生的包裝和縮制解決方案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-204">ASP.NET Core doesn't provide a native bundling and minification solution.</span></span> <span data-ttu-id="d1ad4-205">如需設定捆綁和縮制的詳細資訊，請參閱組合[和縮制](xref:client-side/bundling-and-minification)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-205">For information on configuring bundling and minification, see [Bundling and Minification](xref:client-side/bundling-and-minification).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="d1ad4-206">解決 HTTP 500 錯誤</span><span class="sxs-lookup"><span data-stu-id="d1ad4-206">Solve HTTP 500 errors</span></span>

<span data-ttu-id="d1ad4-207">有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題來源的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-207">There are many problems that can cause an HTTP 500 error message that contains no information on the source of the problem.</span></span> <span data-ttu-id="d1ad4-208">例如，如果*Views/_ViewImports. cshtml*檔案包含不存在於專案中的命名空間，則會產生 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-208">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in the project, an HTTP 500 error is generated.</span></span> <span data-ttu-id="d1ad4-209">根據預設，在 ASP.NET Core 應用程式中， `UseDeveloperExceptionPage` 延伸模組會新增至， `IApplicationBuilder` 並在*開發*環境時執行。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-209">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the environment is *Development*.</span></span> <span data-ttu-id="d1ad4-210">下列程式碼將詳細說明這一點：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-210">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/samples/3.x/Startup.cs?highlight=17-21&name=snippet)]

<span data-ttu-id="d1ad4-211">ASP.NET Core 會將未處理的例外狀況轉換成 HTTP 500 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-211">ASP.NET Core converts unhandled exceptions into HTTP 500 error responses.</span></span> <span data-ttu-id="d1ad4-212">一般來說，錯誤詳細資料不會包含在這些回應中，以防止洩漏伺服器的潛在敏感性資訊。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-212">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="d1ad4-213">如需詳細資訊，請參閱[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-213">For more information, see [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1ad4-214">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d1ad4-214">Next steps</span></span>

* <xref:migration/identity>

## <a name="additional-resources"></a><span data-ttu-id="d1ad4-215">其他資源</span><span class="sxs-lookup"><span data-stu-id="d1ad4-215">Additional resources</span></span>

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d1ad4-216">本文說明如何開始將 ASP.NET MVC 專案遷移至[ASP.NET CORE mvc](xref:mvc/overview) 2.2。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-216">This article shows how to start migrating an ASP.NET MVC project to [ASP.NET Core MVC](xref:mvc/overview) 2.2.</span></span> <span data-ttu-id="d1ad4-217">在此過程中，它強調了許多已從 ASP.NET MVC 變更的專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-217">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="d1ad4-218">從 ASP.NET MVC 進行遷移是一個多步驟的程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-218">Migrating from ASP.NET MVC is a multi-step process.</span></span> <span data-ttu-id="d1ad4-219">本文將說明：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-219">This article covers:</span></span>

* <span data-ttu-id="d1ad4-220">初始設定</span><span class="sxs-lookup"><span data-stu-id="d1ad4-220">Initial setup</span></span>
* <span data-ttu-id="d1ad4-221">基本控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="d1ad4-221">Basic controllers and views</span></span>
* <span data-ttu-id="d1ad4-222">靜態內容</span><span class="sxs-lookup"><span data-stu-id="d1ad4-222">Static content</span></span>
* <span data-ttu-id="d1ad4-223">用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-223">Client-side dependencies.</span></span>

<span data-ttu-id="d1ad4-224">如需遷移設定和程式 Identity 代碼，請參閱 <xref:migration/configuration> 和 <xref:migration/identity> 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-224">For migrating configuration and Identity code, see <xref:migration/configuration> and <xref:migration/identity>.</span></span>

> [!NOTE]
> <span data-ttu-id="d1ad4-225">範例中的版本號碼可能不是最新的，請據以更新專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-225">The version numbers in the samples might not be current, update the projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="d1ad4-226">建立 starter ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-226">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="d1ad4-227">為了示範升級，我們將從建立 ASP.NET MVC 應用程式開始。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-227">To demonstrate the upgrade, we'll start by creating an ASP.NET MVC app.</span></span> <span data-ttu-id="d1ad4-228">使用名稱*WebApp1*建立它，讓命名空間符合下一個步驟中建立的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-228">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project created in the next step.</span></span>

![Visual Studio 的 [新增專案] 對話方塊](mvc/_static/new-project.png)

![[新增 Web 應用程式] 對話方塊：已在 [ASP.NET 範本] 面板中選取 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="d1ad4-231">*選擇性：* 將解決方案的名稱從*WebApp1*變更為*Mvc5*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-231">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="d1ad4-232">Visual Studio 會顯示新的方案名稱（*Mvc5*），讓您更輕鬆地從下一個專案告訴此專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-232">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="d1ad4-233">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-233">Create the ASP.NET Core project</span></span>

<span data-ttu-id="d1ad4-234">使用與前一個專案相同的名稱建立新的*空白*ASP.NET Core web 應用程式（*WebApp1*），讓這兩個專案中的命名空間相符。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-234">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="d1ad4-235">擁有相同的命名空間可讓您更輕鬆地在這兩個專案之間複製程式碼。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-235">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="d1ad4-236">在與前一個專案不同的目錄中建立此專案，以使用相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-236">Create this project in a different directory than the previous project to use the same name.</span></span>

![[新增專案] 對話方塊](mvc/_static/new_core.png)

![[新增 ASP.NET Web 應用程式] 對話方塊：在 ASP.NET Core 範本] 面板中選取了空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="d1ad4-239">*選擇性：* 使用*Web 應用程式*專案範本建立新的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-239">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="d1ad4-240">將專案命名為*WebApp1*，然後選取**個別使用者帳戶**的驗證選項。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-240">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="d1ad4-241">將此應用程式重新命名為*FullAspNetCore*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-241">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="d1ad4-242">建立此專案可節省轉換時間。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-242">Creating this project saves time in the conversion.</span></span> <span data-ttu-id="d1ad4-243">您可以在範本產生的程式碼中查看最終結果、將程式碼複製到轉換專案，或與範本產生的專案相比較。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-243">The end result can be viewed in the template-generated code, code can be copied to the conversion project, or compared with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="d1ad4-244">將網站設定為使用 MVC</span><span class="sxs-lookup"><span data-stu-id="d1ad4-244">Configure the site to use MVC</span></span>

* <span data-ttu-id="d1ad4-245">以 .NET Core 為目標時，預設會參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-245">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="d1ad4-246">此套件包含 MVC 應用程式經常使用的套件。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-246">This package contains packages commonly used by MVC apps.</span></span> <span data-ttu-id="d1ad4-247">如果目標 .NET Framework，則套件參考必須個別列在專案檔中。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-247">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

<span data-ttu-id="d1ad4-248">`Microsoft.AspNetCore.Mvc`是 ASP.NET Core MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-248">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="d1ad4-249">`Microsoft.AspNetCore.StaticFiles`這是靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-249">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="d1ad4-250">ASP.NET Core 的應用程式會明確加入宣告中介軟體，例如用於提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-250">ASP.NET Core apps explicitly opt in for middleware, such as for serving static files.</span></span> <span data-ttu-id="d1ad4-251">如需詳細資訊，請參閱[靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-251">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

* <span data-ttu-id="d1ad4-252">開啟*Startup.cs*檔案，並變更程式碼以符合下列內容：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-252">Open the *Startup.cs* file and change the code to match the following:</span></span>

[!code-csharp[](mvc/samples/2.x/Startup.cs?highlight=7,20-25&name=snippet)]

<span data-ttu-id="d1ad4-253"><xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>擴充方法會加入靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-253">The <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> extension method adds the static file handler.</span></span> <span data-ttu-id="d1ad4-254">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)和[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-254">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="d1ad4-255">新增控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="d1ad4-255">Add a controller and view</span></span>

<span data-ttu-id="d1ad4-256">在本節中，已新增最基本的控制器和視圖，做為 ASP.NET MVC 控制器的預留位置，以及下一節中所遷移的視圖。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-256">In this section, a minimal controller and view are added to serve as placeholders for the ASP.NET MVC controller and views migrated in the next section.</span></span>

* <span data-ttu-id="d1ad4-257">新增*控制器*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-257">Add a *Controllers* directory.</span></span>

* <span data-ttu-id="d1ad4-258">將名為*HomeController.cs*的**控制器類別**新增至*控制器*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-258">Add a **Controller Class** named *HomeController.cs* to the *Controllers* directory.</span></span>

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="d1ad4-260">加入*Views*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-260">Add a *Views* directory.</span></span>

* <span data-ttu-id="d1ad4-261">新增*Views/Home*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-261">Add a *Views/Home* directory.</span></span>

* <span data-ttu-id="d1ad4-262">將名為*Index. cshtml*的\*\* Razor 視圖\**加入至*Views/Home\*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-262">Add a **Razor View** named *Index.cshtml* to the *Views/Home* directory.</span></span>

![[新增項目] 對話方塊](mvc/_static/view.png)

<span data-ttu-id="d1ad4-264">專案結構如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-264">The project structure is shown below:</span></span>

![方案總管顯示 WebApp1 的檔案和目錄](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="d1ad4-266">將 *Views/Home/Index.cshtml* 檔案的內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-266">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="d1ad4-267">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-267">Run the app.</span></span>

![在 Microsoft Edge 中開啟 Web 應用程式](mvc/_static/hello-world.png)

<span data-ttu-id="d1ad4-269">如需詳細資訊，請參閱[控制器](xref:mvc/controllers/actions)和[Views](xref:mvc/views/overview)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-269">For more information, see [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview).</span></span>

<span data-ttu-id="d1ad4-270">下列功能需要從範例 ASP.NET MVC 專案遷移至 ASP.NET Core 專案：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-270">The following functionality requires migration from the example ASP.NET MVC project to the ASP.NET Core project:</span></span>

* <span data-ttu-id="d1ad4-271">用戶端內容（CSS、字型和腳本）</span><span class="sxs-lookup"><span data-stu-id="d1ad4-271">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="d1ad4-272">controllers</span><span class="sxs-lookup"><span data-stu-id="d1ad4-272">controllers</span></span>

* <span data-ttu-id="d1ad4-273">檢視</span><span class="sxs-lookup"><span data-stu-id="d1ad4-273">views</span></span>

* <span data-ttu-id="d1ad4-274">模型</span><span class="sxs-lookup"><span data-stu-id="d1ad4-274">models</span></span>

* <span data-ttu-id="d1ad4-275">統合</span><span class="sxs-lookup"><span data-stu-id="d1ad4-275">bundling</span></span>

* <span data-ttu-id="d1ad4-276">filters</span><span class="sxs-lookup"><span data-stu-id="d1ad4-276">filters</span></span>

* <span data-ttu-id="d1ad4-277">登入/登出 Identity （這會在下一個教學課程中完成）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-277">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="d1ad4-278">控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="d1ad4-278">Controllers and views</span></span>

* <span data-ttu-id="d1ad4-279">將 ASP.NET MVC 中的每個方法複製 `HomeController` 到新的 `HomeController` 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-279">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="d1ad4-280">在 ASP.NET MVC 中，內建範本的控制器動作方法傳回類型為[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx);在 ASP.NET Core MVC 中，動作方法會 `IActionResult` 改為傳回。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-280">In ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="d1ad4-281">`ActionResult`會執行 `IActionResult` ，因此不需要變更動作方法的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-281">`ActionResult` implements `IActionResult`, so there's no need to change the return type of the action methods.</span></span>

* <span data-ttu-id="d1ad4-282">將 [*關於*ASP.NET MVC] 專案中的 [About]、[ *Contact*] 和 [ *Index. cshtml* ] 檔案複製 Razor 到 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-282">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

## <a name="test-each-method"></a><span data-ttu-id="d1ad4-283">測試每個方法</span><span class="sxs-lookup"><span data-stu-id="d1ad4-283">Test each method</span></span>

<span data-ttu-id="d1ad4-284">尚未遷移版面配置檔案和樣式，因此轉譯的視圖只會包含視圖檔案中的內容。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-284">The layout file and styles have not been migrated yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="d1ad4-285">和 views 的設定檔案產生的連結 `About` `Contact` 尚無法使用。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-285">The layout file generated links for the `About` and `Contact` views will not be available yet.</span></span>

<span data-ttu-id="d1ad4-286">藉由以 ASP.NET core 專案中使用的埠號碼取代目前的埠號碼，從執行中 ASP.NET 核心應用程式上的瀏覽器叫用呈現的視圖。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-286">Invoke the rendered views from the browser on the running ASP.NET core app by replacing the current port number with the port number used in the ASP.NET core project.</span></span> <span data-ttu-id="d1ad4-287">例如： `https://localhost:44375/home/about` 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-287">For example: `https://localhost:44375/home/about`.</span></span>

![連絡人頁面](mvc/_static/contact-page.png)

<span data-ttu-id="d1ad4-289">請注意，缺少樣式和功能表項目。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-289">Note the lack of styling and menu items.</span></span> <span data-ttu-id="d1ad4-290">下一節將會修正樣式設定。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-290">The styling will be fixed in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="d1ad4-291">靜態內容</span><span class="sxs-lookup"><span data-stu-id="d1ad4-291">Static content</span></span>

<span data-ttu-id="d1ad4-292">在 ASP.NET MVC 5 和更早版本中，靜態內容是由 Web 專案的根目錄所裝載，並且與伺服器端檔案混合使用。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-292">In ASP.NET MVC 5 and earlier, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="d1ad4-293">在 ASP.NET Core 中，靜態內容會裝載在*wwwroot*目錄中。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-293">In ASP.NET Core, static content is hosted in the *wwwroot* directory.</span></span> <span data-ttu-id="d1ad4-294">將 ASP.NET MVC 應用程式中的靜態內容複寫到 ASP.NET Core 專案中的*wwwroot*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-294">Copy the static content from the ASP.NET MVC app to the *wwwroot* directory in the ASP.NET Core project.</span></span> <span data-ttu-id="d1ad4-295">在此範例轉換中：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-295">In this sample conversion:</span></span>

* <span data-ttu-id="d1ad4-296">將 ASP.NET MVC 專案中的*favicon*複製到 ASP.NET Core 專案中的*wwwroot*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-296">Copy the *favicon.ico* file from the ASP.NET MVC project to the *wwwroot* directory in the ASP.NET Core project.</span></span>

<span data-ttu-id="d1ad4-297">ASP.NET MVC 專案會使用[啟動](https://getbootstrap.com/)程式來進行其樣式設定，並將啟動載入器檔案儲存在*Content*和*Scripts*目錄中。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-297">The ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* directories.</span></span> <span data-ttu-id="d1ad4-298">產生 ASP.NET MVC 專案的範本會參考版面配置檔案中的啟動程式（*Views/Shared/_Layout. cshtml*）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-298">The template, which generated the ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="d1ad4-299">*bootstrap.js*和*啟動程式 .css*檔案可以從 ASP.NET MVC 專案複製到新專案中的*wwwroot*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-299">The *bootstrap.js* and *bootstrap.css* files could be copied from the ASP.NET MVC project to the *wwwroot* directory in the new project.</span></span> <span data-ttu-id="d1ad4-300">相反地，本檔會在下一節中，使用 Cdn 新增對啟動程式（和其他用戶端程式庫）的支援。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-300">Instead, this document adds support for Bootstrap (and other client-side libraries) using CDNs, in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="d1ad4-301">遷移版面配置檔案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-301">Migrate the layout file</span></span>

* <span data-ttu-id="d1ad4-302">將 ASP.NET MVC 專案*views*目錄中的 *_ViewStart. cshtml*檔案複製到 ASP.NET Core 專案的*views*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-302">Copy the *_ViewStart.cshtml* file from the ASP.NET MVC project's *Views* directory into the ASP.NET Core project's *Views* directory.</span></span> <span data-ttu-id="d1ad4-303">ASP.NET Core MVC 中的 *_ViewStart. cshtml*檔案尚未變更。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-303">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="d1ad4-304">建立*Views/Shared*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-304">Create a *Views/Shared* directory.</span></span>

* <span data-ttu-id="d1ad4-305">*選擇性：* 將*FullAspNetCore* MVC 專案*views*目錄中的 *_ViewImports. cshtml*複製到 ASP.NET Core 專案的*views*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-305">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* directory into the ASP.NET Core project's *Views* directory.</span></span> <span data-ttu-id="d1ad4-306">移除 *_ViewImports. cshtml*檔案中的任何命名空間宣告。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-306">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="d1ad4-307">*_ViewImports 的 cshtml*檔案提供所有視圖檔案的命名空間，並帶入標籤協助[程式。](xref:mvc/views/tag-helpers/intro)</span><span class="sxs-lookup"><span data-stu-id="d1ad4-307">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="d1ad4-308">標籤協助程式會用於新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-308">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="d1ad4-309">*_ViewImports 的 cshtml*檔案是 ASP.NET Core 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-309">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="d1ad4-310">將 ASP.NET MVC 專案的*views/shared*目錄中的 *_Layout. cshtml*檔案複製到 ASP.NET Core 專案的*views/shared*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-310">Copy the *_Layout.cshtml* file from the ASP.NET MVC project's *Views/Shared* directory into the ASP.NET Core project's *Views/Shared* directory.</span></span>

<span data-ttu-id="d1ad4-311">開啟 *_Layout 的 cshtml*檔案，並進行下列變更（完成的程式碼如下所示）：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-311">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="d1ad4-312">取代為 `@Styles.Render("~/Content/css")` `<link>` 要載入*啟動*程式的元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-312">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="d1ad4-313">移除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-313">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="d1ad4-314">將行標記為批註 `@Html.Partial("_LoginPartial")` （以括住行 `@*...*@` ）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-314">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="d1ad4-315">如需詳細資訊，請參閱[遷移驗證和 Identity ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="d1ad4-315">For more information, see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="d1ad4-316">取代為 `@Scripts.Render("~/bundles/jquery")` `<script>` 元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-316">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="d1ad4-317">取代為 `@Scripts.Render("~/bundles/bootstrap")` `<script>` 元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-317">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="d1ad4-318">啟動載入 CSS 的取代標記包含：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-318">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="d1ad4-319">JQuery 和啟動程式 JavaScript 包含的取代標記：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-319">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="d1ad4-320">更新後的 *_Layout. cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-320">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/samples/2.x/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="d1ad4-321">在瀏覽器中觀看網站。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-321">View the site in the browser.</span></span> <span data-ttu-id="d1ad4-322">現在應該會正確地載入，且應具備預期的樣式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-322">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="d1ad4-323">*選擇性：* 嘗試使用新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-323">*Optional:* Try using the new layout file.</span></span> <span data-ttu-id="d1ad4-324">從*FullAspNetCore*專案複製版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-324">Copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="d1ad4-325">新的設定檔案[會使用卷](xref:mvc/views/tag-helpers/intro)標協助程式，並具有其他改良功能。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-325">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="d1ad4-326">設定捆綁和縮制</span><span class="sxs-lookup"><span data-stu-id="d1ad4-326">Configure bundling and minification</span></span>

<span data-ttu-id="d1ad4-327">如需有關如何設定配套和縮制的詳細資訊，請參閱[捆綁和縮制](xref:client-side/bundling-and-minification)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-327">For information about how to configure bundling and minification, see [Bundling and Minification](xref:client-side/bundling-and-minification).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="d1ad4-328">解決 HTTP 500 錯誤</span><span class="sxs-lookup"><span data-stu-id="d1ad4-328">Solve HTTP 500 errors</span></span>

<span data-ttu-id="d1ad4-329">有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題來源的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-329">There are many problems that can cause an HTTP 500 error messages that contain no information on the source of the problem.</span></span> <span data-ttu-id="d1ad4-330">例如，如果*Views/_ViewImports. cshtml*檔案包含不存在於專案中的命名空間，則會產生 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-330">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in the project, a HTTP 500 error is generated.</span></span> <span data-ttu-id="d1ad4-331">根據預設，在 ASP.NET Core 應用程式中， `UseDeveloperExceptionPage` 延伸模組會新增至， `IApplicationBuilder` 並在設定為*開發*時執行。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-331">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="d1ad4-332">請參閱下列程式碼中的範例：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-332">See an example in the following code:</span></span>

[!code-csharp[](mvc/samples/2.x/Startup.cs?highlight=11-15&name=snippet)]

<span data-ttu-id="d1ad4-333">ASP.NET Core 會將未處理的例外狀況轉換成 HTTP 500 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-333">ASP.NET Core converts unhandled exceptions into HTTP 500 error responses.</span></span> <span data-ttu-id="d1ad4-334">一般來說，錯誤詳細資料不會包含在這些回應中，以防止洩漏伺服器的潛在敏感性資訊。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-334">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="d1ad4-335">如需詳細資訊，請參閱[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-335">For more information, see [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1ad4-336">其他資源</span><span class="sxs-lookup"><span data-stu-id="d1ad4-336">Additional resources</span></span>

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="d1ad4-337">本文說明如何開始將 ASP.NET MVC 專案遷移至[ASP.NET CORE mvc](xref:mvc/overview) 2.1。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-337">This article shows how to start migrating an ASP.NET MVC project to [ASP.NET Core MVC](xref:mvc/overview) 2.1.</span></span> <span data-ttu-id="d1ad4-338">在此過程中，它強調了許多已從 ASP.NET MVC 變更的專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-338">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="d1ad4-339">從 ASP.NET MVC 進行遷移是一個多步驟的程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-339">Migrating from ASP.NET MVC is a multi-step process.</span></span> <span data-ttu-id="d1ad4-340">本文將說明：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-340">This article covers:</span></span>

* <span data-ttu-id="d1ad4-341">初始設定</span><span class="sxs-lookup"><span data-stu-id="d1ad4-341">Initial setup</span></span>
* <span data-ttu-id="d1ad4-342">基本控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="d1ad4-342">Basic controllers and views</span></span>
* <span data-ttu-id="d1ad4-343">靜態內容</span><span class="sxs-lookup"><span data-stu-id="d1ad4-343">Static content</span></span>
* <span data-ttu-id="d1ad4-344">用戶端相依性。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-344">Client-side dependencies.</span></span>

<span data-ttu-id="d1ad4-345">如需遷移設定和程式 Identity 代碼，請參閱[將設定遷移至 ASP.NET Core](xref:migration/configuration)並[遷移驗證和 Identity ASP.NET Core](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-345">For migrating configuration and Identity code, see [Migrate configuration to ASP.NET Core](xref:migration/configuration) and [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity).</span></span>

> [!NOTE]
> <span data-ttu-id="d1ad4-346">範例中的版本號碼可能不是最新的，請據以更新專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-346">The version numbers in the samples might not be current, update the projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="d1ad4-347">建立 starter ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-347">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="d1ad4-348">為了示範升級，我們將從建立 ASP.NET MVC 應用程式開始。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-348">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="d1ad4-349">使用名稱*WebApp1*建立它，讓命名空間符合下一個步驟中建立的 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-349">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project created in the next step.</span></span>

![Visual Studio 的 [新增專案] 對話方塊](mvc/_static/new-project.png)

![[新增 Web 應用程式] 對話方塊：已在 [ASP.NET 範本] 面板中選取 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="d1ad4-352">*選擇性：* 將解決方案的名稱從*WebApp1*變更為*Mvc5*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-352">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="d1ad4-353">Visual Studio 會顯示新的方案名稱（*Mvc5*），讓您更輕鬆地從下一個專案告訴此專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-353">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="d1ad4-354">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-354">Create the ASP.NET Core project</span></span>

<span data-ttu-id="d1ad4-355">使用與前一個專案相同的名稱建立新的*空白*ASP.NET Core web 應用程式（*WebApp1*），讓這兩個專案中的命名空間相符。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-355">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="d1ad4-356">擁有相同的命名空間可讓您更輕鬆地在這兩個專案之間複製程式碼。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-356">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="d1ad4-357">在與前一個專案不同的目錄中建立此專案，以使用相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-357">Create this project in a different directory than the previous project to use the same name.</span></span>

![[新增專案] 對話方塊](mvc/_static/new_core.png)

![[新增 ASP.NET Web 應用程式] 對話方塊：在 ASP.NET Core 範本] 面板中選取了空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="d1ad4-360">*選擇性：* 使用*Web 應用程式*專案範本建立新的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-360">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="d1ad4-361">將專案命名為*WebApp1*，然後選取**個別使用者帳戶**的驗證選項。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-361">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="d1ad4-362">將此應用程式重新命名為*FullAspNetCore*。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-362">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="d1ad4-363">建立此專案可節省轉換時間。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-363">Creating this project saves time in the conversion.</span></span> <span data-ttu-id="d1ad4-364">您可以在範本產生的程式碼中查看最終結果、將程式碼複製到轉換專案，或與範本產生的專案相比較。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-364">The end result can be viewed in the template-generated code, code can be copied to the conversion project, or compared with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="d1ad4-365">將網站設定為使用 MVC</span><span class="sxs-lookup"><span data-stu-id="d1ad4-365">Configure the site to use MVC</span></span>

* <span data-ttu-id="d1ad4-366">以 .NET Core 為目標時，預設會參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-366">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="d1ad4-367">此套件包含 MVC 應用程式經常使用的套件。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-367">This package contains packages commonly used by MVC apps.</span></span> <span data-ttu-id="d1ad4-368">如果目標 .NET Framework，則套件參考必須個別列在專案檔中。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-368">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

<span data-ttu-id="d1ad4-369">`Microsoft.AspNetCore.Mvc`是 ASP.NET Core MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-369">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="d1ad4-370">`Microsoft.AspNetCore.StaticFiles`這是靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-370">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="d1ad4-371">ASP.NET Core 的應用程式會明確加入宣告中介軟體，例如用於提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-371">ASP.NET Core apps explicitly opt in for middleware, such as for serving static files.</span></span> <span data-ttu-id="d1ad4-372">如需詳細資訊，請參閱[靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-372">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

* <span data-ttu-id="d1ad4-373">開啟*Startup.cs*檔案，並變更程式碼以符合下列內容：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-373">Open the *Startup.cs* file and change the code to match the following:</span></span>

[!code-csharp[](mvc/samples/2.x/Startup.cs?highlight=7,20-25&name=snippet)]

<span data-ttu-id="d1ad4-374"><xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>擴充方法會加入靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-374">The <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> extension method adds the static file handler.</span></span> <span data-ttu-id="d1ad4-375">`UseMvc`擴充方法會新增路由。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-375">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="d1ad4-376">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)和[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-376">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="d1ad4-377">新增控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="d1ad4-377">Add a controller and view</span></span>

<span data-ttu-id="d1ad4-378">在本節中，已新增最基本的控制器和視圖，做為 ASP.NET MVC 控制器的預留位置，以及下一節中所遷移的視圖。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-378">In this section, a minimal controller and view are added to serve as placeholders for the ASP.NET MVC controller and views migrated in the next section.</span></span>

* <span data-ttu-id="d1ad4-379">新增*控制器*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-379">Add a *Controllers* directory.</span></span>

* <span data-ttu-id="d1ad4-380">將名為*HomeController.cs*的**控制器類別**新增至*控制器*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-380">Add a **Controller Class** named *HomeController.cs* to the *Controllers* directory.</span></span>

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="d1ad4-382">加入*Views*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-382">Add a *Views* directory.</span></span>

* <span data-ttu-id="d1ad4-383">新增*Views/Home*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-383">Add a *Views/Home* directory.</span></span>

* <span data-ttu-id="d1ad4-384">將名為*Index. cshtml*的\*\* Razor 視圖\**加入至*Views/Home\*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-384">Add a **Razor View** named *Index.cshtml* to the *Views/Home* directory.</span></span>

![[新增項目] 對話方塊](mvc/_static/view.png)

<span data-ttu-id="d1ad4-386">專案結構如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-386">The project structure is shown below:</span></span>

![方案總管顯示 WebApp1 的檔案和目錄](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="d1ad4-388">將 *Views/Home/Index.cshtml* 檔案的內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-388">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="d1ad4-389">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-389">Run the app.</span></span>

![在 Microsoft Edge 中開啟 Web 應用程式](mvc/_static/hello-world.png)

<span data-ttu-id="d1ad4-391">如需詳細資訊，請參閱[控制器](xref:mvc/controllers/actions)和[Views](xref:mvc/views/overview)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-391">For more information, see [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview).</span></span>

<span data-ttu-id="d1ad4-392">下列功能需要從範例 ASP.NET MVC 專案遷移至 ASP.NET Core 專案：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-392">The following functionality requires migration from the example ASP.NET MVC project to the ASP.NET Core project:</span></span>

* <span data-ttu-id="d1ad4-393">用戶端內容（CSS、字型和腳本）</span><span class="sxs-lookup"><span data-stu-id="d1ad4-393">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="d1ad4-394">controllers</span><span class="sxs-lookup"><span data-stu-id="d1ad4-394">controllers</span></span>

* <span data-ttu-id="d1ad4-395">檢視</span><span class="sxs-lookup"><span data-stu-id="d1ad4-395">views</span></span>

* <span data-ttu-id="d1ad4-396">模型</span><span class="sxs-lookup"><span data-stu-id="d1ad4-396">models</span></span>

* <span data-ttu-id="d1ad4-397">統合</span><span class="sxs-lookup"><span data-stu-id="d1ad4-397">bundling</span></span>

* <span data-ttu-id="d1ad4-398">filters</span><span class="sxs-lookup"><span data-stu-id="d1ad4-398">filters</span></span>

* <span data-ttu-id="d1ad4-399">登入/登出 Identity （這會在下一個教學課程中完成）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-399">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="d1ad4-400">控制器和視圖</span><span class="sxs-lookup"><span data-stu-id="d1ad4-400">Controllers and views</span></span>

* <span data-ttu-id="d1ad4-401">將 ASP.NET MVC 中的每個方法複製 `HomeController` 到新的 `HomeController` 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-401">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="d1ad4-402">在 ASP.NET MVC 中，內建範本的控制器動作方法傳回類型為[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx);在 ASP.NET Core MVC 中，動作方法會 `IActionResult` 改為傳回。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-402">In ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="d1ad4-403">`ActionResult`會執行 `IActionResult` ，因此不需要變更動作方法的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-403">`ActionResult` implements `IActionResult`, so there's no need to change the return type of the action methods.</span></span>

* <span data-ttu-id="d1ad4-404">將 [*關於*ASP.NET MVC] 專案中的 [About]、[ *Contact*] 和 [ *Index. cshtml* ] 檔案複製 Razor 到 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-404">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

## <a name="test-each-method"></a><span data-ttu-id="d1ad4-405">測試每個方法</span><span class="sxs-lookup"><span data-stu-id="d1ad4-405">Test each method</span></span>

<span data-ttu-id="d1ad4-406">尚未遷移版面配置檔案和樣式，因此轉譯的視圖只會包含視圖檔案中的內容。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-406">The layout file and styles have not been migrated yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="d1ad4-407">和 views 的設定檔案產生的連結 `About` `Contact` 尚無法使用。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-407">The layout file generated links for the `About` and `Contact` views will not be available yet.</span></span>

* <span data-ttu-id="d1ad4-408">藉由以 ASP.NET core 專案中使用的埠號碼取代目前的埠號碼，從執行中 ASP.NET 核心應用程式上的瀏覽器叫用呈現的視圖。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-408">Invoke the rendered views from the browser on the running ASP.NET core app by replacing the current port number with the port number used in the ASP.NET core project.</span></span> <span data-ttu-id="d1ad4-409">例如： `https://localhost:44375/home/about` 。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-409">For example: `https://localhost:44375/home/about`.</span></span>

![連絡人頁面](mvc/_static/contact-page.png)

<span data-ttu-id="d1ad4-411">請注意，缺少樣式和功能表項目。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-411">Note the lack of styling and menu items.</span></span> <span data-ttu-id="d1ad4-412">下一節將會修正樣式設定。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-412">The styling will be fixed in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="d1ad4-413">靜態內容</span><span class="sxs-lookup"><span data-stu-id="d1ad4-413">Static content</span></span>

<span data-ttu-id="d1ad4-414">在 ASP.NET MVC 5 和更早版本中，靜態內容是由 Web 專案的根目錄所裝載，並且與伺服器端檔案混合使用。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-414">In ASP.NET MVC 5 and earlier, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="d1ad4-415">在 ASP.NET Core 中，靜態內容會裝載在*wwwroot*目錄中。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-415">In ASP.NET Core, static content is hosted in the *wwwroot* directory.</span></span> <span data-ttu-id="d1ad4-416">將 ASP.NET MVC 應用程式中的靜態內容複寫到 ASP.NET Core 專案中的*wwwroot*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-416">Copy the static content from the ASP.NET MVC app to the *wwwroot* directory in the ASP.NET Core project.</span></span> <span data-ttu-id="d1ad4-417">在此範例轉換中：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-417">In this sample conversion:</span></span>

* <span data-ttu-id="d1ad4-418">將 ASP.NET MVC 專案中的*favicon*複製到 ASP.NET Core 專案中的*wwwroot*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-418">Copy the *favicon.ico* file from the ASP.NET MVC project to the *wwwroot* directory in the ASP.NET Core project.</span></span>

<span data-ttu-id="d1ad4-419">ASP.NET MVC 專案會使用[啟動](https://getbootstrap.com/)程式來進行其樣式設定，並將啟動載入器檔案儲存在*Content*和*Scripts*目錄中。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-419">The ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* directories.</span></span> <span data-ttu-id="d1ad4-420">產生 ASP.NET MVC 專案的範本會參考版面配置檔案中的啟動程式（*Views/Shared/_Layout. cshtml*）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-420">The template, which generated the ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="d1ad4-421">*bootstrap.js*和*啟動程式 .css*檔案可以從 ASP.NET MVC 專案複製到新專案中的*wwwroot*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-421">The *bootstrap.js* and *bootstrap.css* files could be copied from the ASP.NET MVC project to the *wwwroot* directory in the new project.</span></span> <span data-ttu-id="d1ad4-422">相反地，本檔會在下一節中，使用 Cdn 新增對啟動程式（和其他用戶端程式庫）的支援。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-422">Instead, this document adds support for Bootstrap (and other client-side libraries) using CDNs, in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="d1ad4-423">遷移版面配置檔案</span><span class="sxs-lookup"><span data-stu-id="d1ad4-423">Migrate the layout file</span></span>

* <span data-ttu-id="d1ad4-424">將 ASP.NET MVC 專案*views*目錄中的 *_ViewStart. cshtml*檔案複製到 ASP.NET Core 專案的*views*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-424">Copy the *_ViewStart.cshtml* file from the ASP.NET MVC project's *Views* directory into the ASP.NET Core project's *Views* directory.</span></span> <span data-ttu-id="d1ad4-425">ASP.NET Core MVC 中的 *_ViewStart. cshtml*檔案尚未變更。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-425">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="d1ad4-426">建立*Views/Shared*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-426">Create a *Views/Shared* directory.</span></span>

* <span data-ttu-id="d1ad4-427">*選擇性：* 將*FullAspNetCore* MVC 專案*views*目錄中的 *_ViewImports. cshtml*複製到 ASP.NET Core 專案的*views*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-427">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* directory into the ASP.NET Core project's *Views* directory.</span></span> <span data-ttu-id="d1ad4-428">移除 *_ViewImports. cshtml*檔案中的任何命名空間宣告。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-428">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="d1ad4-429">*_ViewImports 的 cshtml*檔案提供所有視圖檔案的命名空間，並帶入標籤協助[程式。](xref:mvc/views/tag-helpers/intro)</span><span class="sxs-lookup"><span data-stu-id="d1ad4-429">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="d1ad4-430">標籤協助程式會用於新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-430">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="d1ad4-431">*_ViewImports 的 cshtml*檔案是 ASP.NET Core 的新檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-431">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="d1ad4-432">將 ASP.NET MVC 專案的*views/shared*目錄中的 *_Layout. cshtml*檔案複製到 ASP.NET Core 專案的*views/shared*目錄。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-432">Copy the *_Layout.cshtml* file from the ASP.NET MVC project's *Views/Shared* directory into the ASP.NET Core project's *Views/Shared* directory.</span></span>

<span data-ttu-id="d1ad4-433">開啟 *_Layout 的 cshtml*檔案，並進行下列變更（完成的程式碼如下所示）：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-433">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="d1ad4-434">取代為 `@Styles.Render("~/Content/css")` `<link>` 要載入*啟動*程式的元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-434">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="d1ad4-435">移除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-435">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="d1ad4-436">將行標記為批註 `@Html.Partial("_LoginPartial")` （以括住行 `@*...*@` ）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-436">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="d1ad4-437">如需詳細資訊，請參閱[遷移驗證和 Identity ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="d1ad4-437">For more information, see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="d1ad4-438">取代為 `@Scripts.Render("~/bundles/jquery")` `<script>` 元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-438">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="d1ad4-439">取代為 `@Scripts.Render("~/bundles/bootstrap")` `<script>` 元素（請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-439">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="d1ad4-440">啟動載入 CSS 的取代標記包含：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-440">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="d1ad4-441">JQuery 和啟動程式 JavaScript 包含的取代標記：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-441">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="d1ad4-442">更新後的 *_Layout. cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-442">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/samples/2.x/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="d1ad4-443">在瀏覽器中觀看網站。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-443">View the site in the browser.</span></span> <span data-ttu-id="d1ad4-444">現在應該會正確地載入，且應具備預期的樣式。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-444">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="d1ad4-445">*選擇性：* 嘗試使用新的版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-445">*Optional:* Try using the new layout file.</span></span> <span data-ttu-id="d1ad4-446">從*FullAspNetCore*專案複製版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-446">Copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="d1ad4-447">新的設定檔案[會使用卷](xref:mvc/views/tag-helpers/intro)標協助程式，並具有其他改良功能。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-447">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="d1ad4-448">設定捆綁和縮制</span><span class="sxs-lookup"><span data-stu-id="d1ad4-448">Configure bundling and minification</span></span>

<span data-ttu-id="d1ad4-449">如需有關如何設定配套和縮制的詳細資訊，請參閱[捆綁和縮制](xref:client-side/bundling-and-minification)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-449">For information about how to configure bundling and minification, see [Bundling and Minification](xref:client-side/bundling-and-minification).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="d1ad4-450">解決 HTTP 500 錯誤</span><span class="sxs-lookup"><span data-stu-id="d1ad4-450">Solve HTTP 500 errors</span></span>

<span data-ttu-id="d1ad4-451">有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題來源的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-451">There are many problems that can cause an HTTP 500 error messages that contain no information on the source of the problem.</span></span> <span data-ttu-id="d1ad4-452">例如，如果*Views/_ViewImports. cshtml*檔案包含不存在於專案中的命名空間，則會產生 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-452">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in the project, a HTTP 500 error is generated.</span></span> <span data-ttu-id="d1ad4-453">根據預設，在 ASP.NET Core 應用程式中， `UseDeveloperExceptionPage` 延伸模組會新增至， `IApplicationBuilder` 並在設定為*開發*時執行。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-453">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="d1ad4-454">請參閱下列程式碼中的範例：</span><span class="sxs-lookup"><span data-stu-id="d1ad4-454">See an example in the following code:</span></span>

[!code-csharp[](mvc/samples/2.x/Startup.cs?highlight=11-15&name=snippet)]

<span data-ttu-id="d1ad4-455">ASP.NET Core 會將未處理的例外狀況轉換成 HTTP 500 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-455">ASP.NET Core converts unhandled exceptions into HTTP 500 error responses.</span></span> <span data-ttu-id="d1ad4-456">一般來說，錯誤詳細資料不會包含在這些回應中，以防止洩漏伺服器的潛在敏感性資訊。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-456">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="d1ad4-457">如需詳細資訊，請參閱[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。</span><span class="sxs-lookup"><span data-stu-id="d1ad4-457">For more information, see [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1ad4-458">其他資源</span><span class="sxs-lookup"><span data-stu-id="d1ad4-458">Additional resources</span></span>

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>

::: moniker-end
