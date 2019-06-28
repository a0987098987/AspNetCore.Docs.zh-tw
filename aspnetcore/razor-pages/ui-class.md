---
title: ASP.NET Core 類別庫中的可重複使用 Razor UI
author: Rick-Anderson
description: 說明如何建立可重複使用 Razor UI，使用 ASP.NET Core 中的類別庫中的部分檢視。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/28/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: d59f643a23b48ccbddf498ef534ee8432b010f40
ms.sourcegitcommit: 6d9cf728465cdb0de1037633a8b7df9a8989cccb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2019
ms.locfileid: "67463261"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="9f529-103">建立可重複使用 UI 在 ASP.NET Core 中使用 Razor 類別庫專案</span><span class="sxs-lookup"><span data-stu-id="9f529-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="9f529-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9f529-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9f529-105">Razor 檢視、 頁面、 控制器、 頁面模型[Razor 元件](xref:blazor/class-libraries)，[檢視元件](xref:mvc/views/view-components)，和資料模型可以建立 Razor 類別庫 (RCL)。</span><span class="sxs-lookup"><span data-stu-id="9f529-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="9f529-106">RCL 可以封裝和重複使用。</span><span class="sxs-lookup"><span data-stu-id="9f529-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="9f529-107">應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。</span><span class="sxs-lookup"><span data-stu-id="9f529-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="9f529-108">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="9f529-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="9f529-109">這項功能需要 [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="9f529-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="9f529-110">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f529-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="9f529-111">建立包含 Razor UI 的類別庫</span><span class="sxs-lookup"><span data-stu-id="9f529-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f529-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f529-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f529-113">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9f529-114">選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9f529-115">命名程式庫 (例如，"RazorClassLib") > [確定]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="9f529-116">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="9f529-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="9f529-117">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f529-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="9f529-118">選取 [Razor 類別庫]   > [確定]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="9f529-119">RCL 具有下列專案檔：</span><span class="sxs-lookup"><span data-stu-id="9f529-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9f529-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9f529-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9f529-121">從命令列執行 `dotnet new razorclasslib`。</span><span class="sxs-lookup"><span data-stu-id="9f529-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="9f529-122">例如:</span><span class="sxs-lookup"><span data-stu-id="9f529-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="9f529-123">如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="9f529-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="9f529-124">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="9f529-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="9f529-125">新增 Razor 檔案至 RCL。</span><span class="sxs-lookup"><span data-stu-id="9f529-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="9f529-126">ASP.NET Core 範本假設 RCL 內容位於*領域*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f529-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="9f529-127">請參閱[RCL 網頁版面配置](#afs)建立中的內容會公開 RCL`~/Pages`而非`~/Areas/Pages`。</span><span class="sxs-lookup"><span data-stu-id="9f529-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="9f529-128">參考 RCL 內容</span><span class="sxs-lookup"><span data-stu-id="9f529-128">Referencing RCL content</span></span>

<span data-ttu-id="9f529-129">RCL 可以由下列各項參考：</span><span class="sxs-lookup"><span data-stu-id="9f529-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="9f529-130">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="9f529-130">NuGet package.</span></span> <span data-ttu-id="9f529-131">請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="9f529-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="9f529-132">*{ProjectName}.csproj*。</span><span class="sxs-lookup"><span data-stu-id="9f529-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="9f529-133">請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。</span><span class="sxs-lookup"><span data-stu-id="9f529-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="9f529-134">逐步解說：建立 RCL 專案，並使用從 Razor Pages 專案</span><span class="sxs-lookup"><span data-stu-id="9f529-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="9f529-135">您可以下載[完整專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)並測試它，而不是建立它。</span><span class="sxs-lookup"><span data-stu-id="9f529-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="9f529-136">下載範例包含額外的程式碼和連結，讓您輕鬆地測試專案。</span><span class="sxs-lookup"><span data-stu-id="9f529-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="9f529-137">您可以在[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/6098)留下意見反應以及您對下載範例與逐步指示的評論。</span><span class="sxs-lookup"><span data-stu-id="9f529-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="9f529-138">測試下載應用程式</span><span class="sxs-lookup"><span data-stu-id="9f529-138">Test the download app</span></span>

<span data-ttu-id="9f529-139">如果您尚未下載已完成的應用程式，而是要建立逐步解說專案，請跳至[下一節](#create-an-rcl)。</span><span class="sxs-lookup"><span data-stu-id="9f529-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f529-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f529-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9f529-141">在 Visual Studio 中開啟 *.sln* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9f529-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="9f529-142">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f529-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9f529-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9f529-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9f529-144">從命令提示字元中在 *cli* 目錄中，建置 RCL 和 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f529-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="9f529-145">移至 *WebApp1* 目錄並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="9f529-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="9f529-146">遵循[測試 WebApp1](#test) 中的指示執行。</span><span class="sxs-lookup"><span data-stu-id="9f529-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="9f529-147">建立 RCL</span><span class="sxs-lookup"><span data-stu-id="9f529-147">Create an RCL</span></span>

<span data-ttu-id="9f529-148">在本節中，會建立 RCL。</span><span class="sxs-lookup"><span data-stu-id="9f529-148">In this section, an RCL is created.</span></span> <span data-ttu-id="9f529-149">會有 Razor 檔案新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="9f529-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f529-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f529-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9f529-151">建立 RCL 專案：</span><span class="sxs-lookup"><span data-stu-id="9f529-151">Create the RCL project:</span></span>

* <span data-ttu-id="9f529-152">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9f529-153">選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9f529-154">將應用程式命名**RazorUIClassLib** > **確定**。</span><span class="sxs-lookup"><span data-stu-id="9f529-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="9f529-155">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f529-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="9f529-156">選取 [Razor 類別庫]   > [確定]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="9f529-157">新增名為 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 的 Razor 部分檢視檔。</span><span class="sxs-lookup"><span data-stu-id="9f529-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9f529-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9f529-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9f529-159">從命令列執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9f529-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="9f529-160">上述命令：</span><span class="sxs-lookup"><span data-stu-id="9f529-160">The preceding commands:</span></span>

* <span data-ttu-id="9f529-161">建立`RazorUIClassLib`RCL。</span><span class="sxs-lookup"><span data-stu-id="9f529-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="9f529-162">建立 Razor _Message 頁面，並將它新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="9f529-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="9f529-163">`-np` 參數會建立頁面而不使用 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="9f529-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="9f529-164">會建立[_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view)檔案，並將它新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="9f529-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="9f529-165">*_ViewStart.cshtml*檔案，才能使用 Razor 頁面專案 （這會新增下一節） 的配置。</span><span class="sxs-lookup"><span data-stu-id="9f529-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="9f529-166">加入專案中的 Razor 檔案和資料夾</span><span class="sxs-lookup"><span data-stu-id="9f529-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="9f529-167">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f529-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="9f529-168">將 *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f529-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="9f529-169">需要 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 才能使用部分檢視 (`<partial name="_Message" />`)。</span><span class="sxs-lookup"><span data-stu-id="9f529-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="9f529-170">您可以新增 *_ViewImports.cshtml* 檔案，而不要包含 `@addTagHelper` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="9f529-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="9f529-171">例如:</span><span class="sxs-lookup"><span data-stu-id="9f529-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="9f529-172">如需詳細資訊 *_ViewImports.cshtml*，請參閱[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="9f529-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="9f529-173">建置類別庫，以確認沒有任何編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="9f529-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="9f529-174">建置輸出包含 *RazorUIClassLib.dll* 和 *RazorUIClassLib.Views.dll*。</span><span class="sxs-lookup"><span data-stu-id="9f529-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="9f529-175">*RazorUIClassLib.Views.dll* 包含已編譯的 Razor 內容。</span><span class="sxs-lookup"><span data-stu-id="9f529-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="9f529-176">從 Razor 頁面專案使用 Razor UI 程式庫</span><span class="sxs-lookup"><span data-stu-id="9f529-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f529-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f529-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9f529-178">建立 Razor 頁面 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="9f529-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="9f529-179">在 [方案總管]  中，以滑鼠右鍵按一下解決方案 > [新增]   >  [新增專案]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="9f529-180">選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9f529-181">將應用程式命名為 **WebApp1**。</span><span class="sxs-lookup"><span data-stu-id="9f529-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="9f529-182">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f529-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="9f529-183">選取 [Web 應用程式]   > [確定]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="9f529-184">從 [方案總管]  ，以滑鼠右鍵按一下 **WebApp1**，然後選取 [設定為啟始專案]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="9f529-185">從方案總管  ，以滑鼠右鍵按一下 **WebApp1**，然後選取 [建置相依性]   > [專案相依性]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="9f529-186">核取 **RazorUIClassLib** 作為 **WebApp1** 的相依性。</span><span class="sxs-lookup"><span data-stu-id="9f529-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="9f529-187">從 [方案總管]  ，以滑鼠右鍵按一下 **WebApp1**，然後選取 [新增]   > [參考]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="9f529-188">在 [參考管理員]  對話方塊中，核取 **RazorUIClassLib** > [確定]  。</span><span class="sxs-lookup"><span data-stu-id="9f529-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="9f529-189">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f529-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9f529-190">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9f529-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9f529-191">建立 Razor 頁面 web 應用程式和方案檔包含 Razor 頁面應用程式和 RCL:</span><span class="sxs-lookup"><span data-stu-id="9f529-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="9f529-192">建置和執行 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="9f529-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="9f529-193">測試 WebApp1</span><span class="sxs-lookup"><span data-stu-id="9f529-193">Test WebApp1</span></span>

<span data-ttu-id="9f529-194">確認 Razor UI 類別庫是使用中：</span><span class="sxs-lookup"><span data-stu-id="9f529-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="9f529-195">瀏覽至 `/MyFeature/Page1`。</span><span class="sxs-lookup"><span data-stu-id="9f529-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="9f529-196">覆寫檢視、部分檢視和頁面</span><span class="sxs-lookup"><span data-stu-id="9f529-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="9f529-197">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="9f529-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="9f529-198">例如，新增*WebApp1/Areas/MyFeature/Pages/Page1.cshtml* WebApp1，以和 Page1 WebApp1 中的將會優先於 RCL 中的 Page1。</span><span class="sxs-lookup"><span data-stu-id="9f529-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="9f529-199">在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。</span><span class="sxs-lookup"><span data-stu-id="9f529-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="9f529-200">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9f529-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="9f529-201">更新標記來指示新的位置。</span><span class="sxs-lookup"><span data-stu-id="9f529-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="9f529-202">建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="9f529-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="9f529-203">RCL 網頁版面配置</span><span class="sxs-lookup"><span data-stu-id="9f529-203">RCL Pages layout</span></span>

<span data-ttu-id="9f529-204">參考 RCL 內容，就好像它是 web 應用程式的一部分*頁*資料夾中，建立 RCL 專案檔案結構如下：</span><span class="sxs-lookup"><span data-stu-id="9f529-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="9f529-205">*RazorUIClassLib/頁面*</span><span class="sxs-lookup"><span data-stu-id="9f529-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="9f529-206">*RazorUIClassLib/網頁/共用*</span><span class="sxs-lookup"><span data-stu-id="9f529-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="9f529-207">假設*RazorUIClassLib/網頁/Shared*包含兩個部分的檔案： *_Header.cshtml*並 *_Footer.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9f529-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="9f529-208">`<partial>`標記新增到 *_Layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="9f529-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="9f529-209">使用靜態資產建立 RCL</span><span class="sxs-lookup"><span data-stu-id="9f529-209">Create an RCL with static assets</span></span>

<span data-ttu-id="9f529-210">RCL 可能需要的 RCL 使用的應用程式可以參考的附屬靜態資產。</span><span class="sxs-lookup"><span data-stu-id="9f529-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="9f529-211">ASP.NET Core 可讓您建立包含可供使用的應用程式靜態資產的 RCLs。</span><span class="sxs-lookup"><span data-stu-id="9f529-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="9f529-212">若要納入隨附資產 RCL，建立*wwwroot*類別庫中的資料夾，並在該資料夾中包含任何必要的檔案。</span><span class="sxs-lookup"><span data-stu-id="9f529-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="9f529-213">當封裝 RCL，所有附屬中的資產*wwwroot*資料夾會自動包含在封裝中，並且可供參考套件的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f529-213">When packing an RCL, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="9f529-214">使用來自參考 RCL 內容</span><span class="sxs-lookup"><span data-stu-id="9f529-214">Consume content from a referenced RCL</span></span>

<span data-ttu-id="9f529-215">中包含的檔案*wwwroot* RCL 資料夾會公開使用的應用程式，在前置詞至`_content/{LIBRARY NAME}/`。</span><span class="sxs-lookup"><span data-stu-id="9f529-215">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="9f529-216">`{LIBRARY NAME}` 程式庫專案名稱轉換成小寫與期間 (`.`) 移除。</span><span class="sxs-lookup"><span data-stu-id="9f529-216">`{LIBRARY NAME}` is the library project name converted to lowercase with periods (`.`) removed.</span></span> <span data-ttu-id="9f529-217">例如，名為媒體櫃*Razor.Class.Lib*靜態內容，在產生的路徑`_content/razorclasslib/`。</span><span class="sxs-lookup"><span data-stu-id="9f529-217">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/razorclasslib/`.</span></span>

<span data-ttu-id="9f529-218">使用的應用程式參考與程式庫所提供的靜態資產`<script>`， `<style>`， `<img>`，以及其他的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="9f529-218">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="9f529-219">使用的應用程式必須擁有[靜態檔案支援](xref:fundamentals/static-files)啟用。</span><span class="sxs-lookup"><span data-stu-id="9f529-219">The consuming app must have [static file support](xref:fundamentals/static-files) enabled.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="9f529-220">多專案的開發流程</span><span class="sxs-lookup"><span data-stu-id="9f529-220">Multi-project development flow</span></span>

<span data-ttu-id="9f529-221">使用的應用程式執行時：</span><span class="sxs-lookup"><span data-stu-id="9f529-221">When the consuming app runs:</span></span>

* <span data-ttu-id="9f529-222">RCL 留在原始資料夾中的資產。</span><span class="sxs-lookup"><span data-stu-id="9f529-222">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="9f529-223">資產不會移至使用的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f529-223">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="9f529-224">RCL 中的任何變更*wwwroot*資料夾會反映在 使用的應用程式之後重建 RCL，而不需重建使用的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f529-224">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="9f529-225">建置 RCL 時，會產生資訊清單描述靜態 web 資產位置。</span><span class="sxs-lookup"><span data-stu-id="9f529-225">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="9f529-226">使用的應用程式會讀取在執行階段使用 從參考的專案和封裝資產的資訊清單。</span><span class="sxs-lookup"><span data-stu-id="9f529-226">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="9f529-227">當新的資產新增至 RCL 時，您就必須重建 RCL 之前使用的應用程式可以存取新的資產更新其資訊清單。</span><span class="sxs-lookup"><span data-stu-id="9f529-227">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="9f529-228">發行</span><span class="sxs-lookup"><span data-stu-id="9f529-228">Publish</span></span>

<span data-ttu-id="9f529-229">發行應用程式時，所有參考的專案和套件隨附資產會複製到*wwwroot*資料夾的已發佈的應用程式下`_content/{LIBRARY NAME}/`。</span><span class="sxs-lookup"><span data-stu-id="9f529-229">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
