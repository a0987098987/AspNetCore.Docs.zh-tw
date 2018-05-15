---
title: ASP.NET Core 類別庫中的可重複使用 Razor UI
author: Rick-Anderson
description: 說明如何在類別庫中建立可重複使用的 Razor UI。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="fd6bd-103">在 ASP.NET Core 中使用 Razor 類別庫專案建立可重複使用的 UI。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="fd6bd-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fd6bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fd6bd-105">Razor 檢視、頁面、控制器、頁面模型和資料模型可以建置到 Razor 類別庫 (RCL) 中。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library(RCL).</span></span> <span data-ttu-id="fd6bd-106">RCL 可以封裝和重複使用。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-106">The RCL can be and packaged and reused.</span></span> <span data-ttu-id="fd6bd-107">應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="fd6bd-108">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="fd6bd-109">這個功能需要 [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="fd6bd-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="fd6bd-110">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd6bd-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="fd6bd-111">建立包含 Razor UI 的類別庫</span><span class="sxs-lookup"><span data-stu-id="fd6bd-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd6bd-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd6bd-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd6bd-113">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fd6bd-114">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="fd6bd-115">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="fd6bd-116">選取 [Razor 類別庫] > [確定]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fd6bd-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fd6bd-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fd6bd-118">從命令列執行 `dotnet new razorclasslib`。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="fd6bd-119">例如: </span><span class="sxs-lookup"><span data-stu-id="fd6bd-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="fd6bd-120">如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="fd6bd-121">新增 Razor 檔案至 RCL。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="fd6bd-122">我們建議 RCL 內容放到 *Areas* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="fd6bd-123">參考 Razor 類別庫內容</span><span class="sxs-lookup"><span data-stu-id="fd6bd-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="fd6bd-124">RCL 可以由下列各項參考：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="fd6bd-125">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-125">NuGet package.</span></span> <span data-ttu-id="fd6bd-126">請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="fd6bd-127">*{ProjectName}.csproj*。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="fd6bd-128">請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="fd6bd-129">RCL 中的部分檔案存取</span><span class="sxs-lookup"><span data-stu-id="fd6bd-129">Partial files access in the RCL</span></span>

<span data-ttu-id="fd6bd-130">對於 RCL 以外的內容，ASP.NET Core 執行階段不會搜尋 RCL 中的部分檔案。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="fd6bd-131">例如，在下載範例中，*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視**不可以**在 *WebApp1\Pages\About.cshtml* 中參考。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="fd6bd-132">不過，RCL 中的頁面 (*RazorUIClassLib /* **可以**存取 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="fd6bd-133">逐步解說：建立 Razor 類別庫專案，並從 Razor 頁面專案使用</span><span class="sxs-lookup"><span data-stu-id="fd6bd-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="fd6bd-134">您可以下載[完整專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples)並測試它，而不是建立它。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="fd6bd-135">下載範例包含額外的程式碼和連結，讓您輕鬆地測試專案。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="fd6bd-136">您可以在[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6098)留下意見反應以及您對下載範例與逐步指示的評論。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="fd6bd-137">測試下載應用程式</span><span class="sxs-lookup"><span data-stu-id="fd6bd-137">Test the download app</span></span>

<span data-ttu-id="fd6bd-138">如果您尚未下載已完成的應用程式，而是要建立逐步解說專案，請跳至[下一節](#create-a-razor-class-library)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd6bd-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd6bd-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fd6bd-140">在 Visual Studio 中開啟 *.sln* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="fd6bd-141">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fd6bd-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fd6bd-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fd6bd-143">從命令提示字元中在 *cli* 目錄中，建置 RCL 和 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="fd6bd-144">移至 *WebApp1* 目錄並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="fd6bd-145">遵循[測試 WebApp1](#test) 中的指示執行。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="fd6bd-146">建立 Razor 類別庫</span><span class="sxs-lookup"><span data-stu-id="fd6bd-146">Create a Razor Class Library</span></span>

<span data-ttu-id="fd6bd-147">在本節中，會建立 Razor 類別庫 (RCL)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="fd6bd-148">會有 Razor 檔案新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd6bd-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd6bd-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fd6bd-150">建立 RCL 專案：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-150">Create the RCL project:</span></span>

* <span data-ttu-id="fd6bd-151">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fd6bd-152">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="fd6bd-153">將應用程式命名為 **RazorUIClassLib**。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="fd6bd-154">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="fd6bd-155">選取 [Razor 類別庫] > [確定]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="fd6bd-156">建立 Razor 頁面 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="fd6bd-157">在 [方案總管] 中，以滑鼠右鍵按一下解決方案 > [新增] >  [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="fd6bd-158">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="fd6bd-159">將應用程式命名為 **WebApp1**。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="fd6bd-160">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="fd6bd-161">選取 [Web 應用程式] > [確定]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="fd6bd-162">將 Razor 檔案和資料夾新增至專案。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="fd6bd-163">新增名為 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 的 Razor 部分檢視檔。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="fd6bd-164">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="fd6bd-165">將 *_ViewStart.cshtml* 檔案從 WebApp1 專案複製到 *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="fd6bd-166">需要 [viewstart](xref:mvc/views/layout#running-code-before-each-view) 檔案，才能使用 Razor 頁面專案的版面配置。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fd6bd-167">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fd6bd-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fd6bd-168">從命令列執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="fd6bd-169">上述命令：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-169">The preceding commands:</span></span>

* <span data-ttu-id="fd6bd-170">建立 `RazorUIClassLib` Razor 類別庫 (RCL)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="fd6bd-171">建立 Razor _Message 頁面，並將它新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="fd6bd-172">`-np` 參數會建立頁面而不使用 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="fd6bd-173">建立 [viewstart](xref:mvc/views/layout#running-code-before-each-view) 檔案，並將它新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="fd6bd-174">需要 viewstart 檔案，才能使用 Razor 頁面專案的版面配置 (在下一節新增)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="fd6bd-175">更新 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="fd6bd-176">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="fd6bd-177">將 *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="fd6bd-178">需要 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 才能使用部分檢視 (`<partial name="_Message" />`)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="fd6bd-179">您可以新增 *_ViewImports.cshtml* 檔案，而不要包含 `@addTagHelper` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="fd6bd-180">例如: </span><span class="sxs-lookup"><span data-stu-id="fd6bd-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="fd6bd-181">如需 viewimports 的詳細資訊，請參閱[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="fd6bd-182">建置類別庫，以確認沒有任何編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="fd6bd-183">建置輸出包含 *RazorUIClassLib.dll* 和 *RazorUIClassLib.Views.dll*。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="fd6bd-184">*RazorUIClassLib.Views.dll* 包含已編譯的 Razor 內容。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="fd6bd-185">從 Razor 頁面專案使用 Razor UI 程式庫</span><span class="sxs-lookup"><span data-stu-id="fd6bd-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd6bd-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd6bd-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd6bd-187">從 [方案總管]，以滑鼠右鍵按一下 **WebApp1**，然後選取 [設定為啟始專案]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="fd6bd-188">從方案總管，以滑鼠右鍵按一下 **WebApp1**，然後選取 [建置相依性] > [專案相依性]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="fd6bd-189">核取 **RazorUIClassLib** 作為 **WebApp1** 的相依性。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="fd6bd-190">從 [方案總管]，以滑鼠右鍵按一下 **WebApp1**，然後選取 [新增] > [參考]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="fd6bd-191">在 [參考管理員] 對話方塊中，核取 **RazorUIClassLib** > [確定]。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="fd6bd-192">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fd6bd-193">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fd6bd-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fd6bd-194">建立 Razor 頁面的 Web 應用程式和方案檔，其中包含 Razor 頁面應用程式和 Razor 類別庫：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="fd6bd-195">建置和執行 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="fd6bd-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="fd6bd-196">測試 WebApp1</span><span class="sxs-lookup"><span data-stu-id="fd6bd-196">Test WebApp1</span></span>

<span data-ttu-id="fd6bd-197">確認正在使用 Razor UI 類別庫。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="fd6bd-198">瀏覽至 `/MyFeature/Page1`。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="fd6bd-199">覆寫檢視、部分檢視和頁面</span><span class="sxs-lookup"><span data-stu-id="fd6bd-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="fd6bd-200">在 Web 應用程式和 Razor 類別庫中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="fd6bd-201">例如，將 *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* 新增至 WebApp1，WebApp1 中的 Page1 將會優先於 Razor 類別庫中的 Page1。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="fd6bd-202">在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="fd6bd-203">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="fd6bd-204">更新標記來指示新的位置。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="fd6bd-205">建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="fd6bd-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
