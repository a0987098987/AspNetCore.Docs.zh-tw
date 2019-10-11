---
title: ASP.NET Core 類別庫中的可重複使用 Razor UI
author: Rick-Anderson
description: 說明如何建立可重複使用 Razor UI，使用 ASP.NET Core 中的類別庫中的部分檢視。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: dcd24f7dafd198f88cdf84d1ab67c84f45428a95
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179327"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="f8fc0-103">在 ASP.NET Core 中使用 Razor 類別庫專案建立可重複使用的 UI</span><span class="sxs-lookup"><span data-stu-id="f8fc0-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="f8fc0-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8fc0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f8fc0-105">Razor 視圖、頁面、控制器、頁面模型、 [razor 元件](xref:blazor/class-libraries)、[視圖元件](xref:mvc/views/view-components)和資料模型可以內建于 RAZOR 類別庫（RCL）中。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="f8fc0-106">RCL 可以封裝和重複使用。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="f8fc0-107">應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="f8fc0-108">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="f8fc0-109">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f8fc0-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="f8fc0-110">建立包含 Razor UI 的類別庫</span><span class="sxs-lookup"><span data-stu-id="f8fc0-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8fc0-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8fc0-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8fc0-112">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f8fc0-113">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f8fc0-114">命名程式庫 (例如，"RazorClassLib") > [確定]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="f8fc0-115">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="f8fc0-116">確認已選取**ASP.NET Core 3.0**或更新版本。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="f8fc0-117">選取 [ **Razor 類別庫**] > **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="f8fc0-118">根據預設，Razor 類別庫（RCL）範本預設為 Razor 元件開發。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="f8fc0-119">Visual Studio 中的範本選項會提供頁面和視圖的範本支援。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f8fc0-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f8fc0-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f8fc0-121">從命令列執行 `dotnet new razorclasslib`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="f8fc0-122">例如:</span><span class="sxs-lookup"><span data-stu-id="f8fc0-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="f8fc0-123">根據預設，Razor 類別庫（RCL）範本預設為 Razor 元件開發。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="f8fc0-124">傳遞 `--support-pages-and-views` 選項（`dotnet new razorclasslib --support-pages-and-views`）以提供頁面和視圖的支援。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-124">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="f8fc0-125">如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="f8fc0-126">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="f8fc0-127">新增 Razor 檔案至 RCL。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="f8fc0-128">ASP.NET Core 範本假設 RCL 內容位於*領域*資料夾。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="f8fc0-129">請參閱[RCL 頁面配置](#rcl-pages-layout)，以建立會在 `~/Pages` 中公開內容的 RCL，而不是 `~/Areas/Pages`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="f8fc0-130">參考 RCL 內容</span><span class="sxs-lookup"><span data-stu-id="f8fc0-130">Reference RCL content</span></span>

<span data-ttu-id="f8fc0-131">RCL 可以由下列各項參考：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="f8fc0-132">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-132">NuGet package.</span></span> <span data-ttu-id="f8fc0-133">請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="f8fc0-134">*{ProjectName}.csproj*。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="f8fc0-135">請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="f8fc0-136">覆寫檢視、部分檢視和頁面</span><span class="sxs-lookup"><span data-stu-id="f8fc0-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="f8fc0-137">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="f8fc0-138">例如，將*WebApp1/Areas/MyFeature/Pages/Page1. cshtml*新增到 WebApp1，WebApp1 中的 page1 會優先于 RCL 中的 page1。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="f8fc0-139">在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="f8fc0-140">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="f8fc0-141">更新標記來指示新的位置。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="f8fc0-142">建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="f8fc0-143">RCL 網頁版面配置</span><span class="sxs-lookup"><span data-stu-id="f8fc0-143">RCL Pages layout</span></span>

<span data-ttu-id="f8fc0-144">參考 RCL 內容，就好像它是 web 應用程式的一部分*頁*資料夾中，建立 RCL 專案檔案結構如下：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="f8fc0-145">*RazorUIClassLib/頁面*</span><span class="sxs-lookup"><span data-stu-id="f8fc0-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="f8fc0-146">*RazorUIClassLib/網頁/共用*</span><span class="sxs-lookup"><span data-stu-id="f8fc0-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="f8fc0-147">假設*RazorUIClassLib/網頁/Shared*包含兩個部分的檔案： *_Header.cshtml*並 *_Footer.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="f8fc0-148">`<partial>`標記新增到 *_Layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="f8fc0-149">建立具有靜態資產的 RCL</span><span class="sxs-lookup"><span data-stu-id="f8fc0-149">Create an RCL with static assets</span></span>

<span data-ttu-id="f8fc0-150">RCL 可能需要可供 RCL 取用應用程式參考的隨附靜態資產。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="f8fc0-151">ASP.NET Core 可讓您建立包含可供取用應用程式使用之靜態資產的 RCLs。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="f8fc0-152">若要將隨附的資產納入為 RCL 的一部分，請在類別庫中建立*wwwroot*資料夾，並在該資料夾中包含任何必要的檔案。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="f8fc0-153">封裝 RCL 時， *wwwroot*資料夾中的所有附屬資產都會自動包含在套件中。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="f8fc0-154">排除靜態資產</span><span class="sxs-lookup"><span data-stu-id="f8fc0-154">Exclude static assets</span></span>

<span data-ttu-id="f8fc0-155">若要排除靜態資產，請將所需的排除路徑新增至專案檔中的 `$(DefaultItemExcludes)` 屬性群組。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="f8fc0-156">以分號分隔專案（`;`）。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="f8fc0-157">在下列範例中， *wwwroot*資料夾中的*lib .css*樣式不會被視為靜態資產，而且不會包含在已發行的 RCL 中：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="f8fc0-158">Typescript 整合</span><span class="sxs-lookup"><span data-stu-id="f8fc0-158">Typescript integration</span></span>

<span data-ttu-id="f8fc0-159">若要在 RCL 中包含 TypeScript 檔案：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="f8fc0-160">將 TypeScript 檔案（ *. ts*）放在*wwwroot*資料夾外部。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="f8fc0-161">例如，將檔案放在*用戶端*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="f8fc0-162">設定 [ *wwwroot* ] 資料夾的 TypeScript 組建輸出。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="f8fc0-163">在專案檔的 `PropertyGroup` 內設定 `TypescriptOutDir` 屬性：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="f8fc0-164">將 TypeScript 目標納入為 `ResolveCurrentProjectStaticWebAssets` 目標的相依性，方法是在專案檔中的 `PropertyGroup` 內新增下列目標：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
  <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
    CompileTypeScript;
    $(ResolveCurrentProjectStaticWebAssetsInputs)
  </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="f8fc0-165">從參考的 RCL 取用內容</span><span class="sxs-lookup"><span data-stu-id="f8fc0-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="f8fc0-166">RCL 的*wwwroot*資料夾中所包含的檔案，會向取用應用程式公開至前置詞 `_content/{LIBRARY NAME}/`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="f8fc0-167">例如，名為 Razor. *.lib*的程式庫會產生 `_content/Razor.Class.Lib/` 的靜態內容路徑。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="f8fc0-168">取用應用程式會參考程式庫所提供的靜態資產，`<script>`、`<style>`、@no__t 2 和其他 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="f8fc0-169">取用應用程式必須在中`Startup.Configure`啟用[靜態檔案支援](xref:fundamentals/static-files)：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="f8fc0-170">從組建輸出（`dotnet run`）執行取用應用程式時，預設會在開發環境中啟用靜態 web 資產。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="f8fc0-171">若要在從組建輸出執行時支援其他環境中的資產，請在*Program.cs*的主機產生器上呼叫 `UseStaticWebAssets`：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="f8fc0-172">從已發佈的輸出（`dotnet publish`）執行應用程式時，不需要呼叫 `UseStaticWebAssets`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="f8fc0-173">多專案開發流程</span><span class="sxs-lookup"><span data-stu-id="f8fc0-173">Multi-project development flow</span></span>

<span data-ttu-id="f8fc0-174">當使用中的應用程式執行時：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-174">When the consuming app runs:</span></span>

* <span data-ttu-id="f8fc0-175">RCL 中的資產會保留在其原始檔案夾中。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="f8fc0-176">資產不會移至取用應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="f8fc0-177">重建 RCL 之後，RCL *wwwroot*資料夾內的任何變更都會反映在取用應用程式中，而不需要重建取用應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="f8fc0-178">建立 RCL 時，會產生描述靜態 web 資產位置的資訊清單。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="f8fc0-179">取用應用程式會在執行時間讀取資訊清單，以從參考的專案和套件取用資產。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="f8fc0-180">將新資產新增至 RCL 時，必須重建 RCL 以更新其資訊清單，取用應用程式才能存取新資產。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="f8fc0-181">發行</span><span class="sxs-lookup"><span data-stu-id="f8fc0-181">Publish</span></span>

<span data-ttu-id="f8fc0-182">當應用程式發佈時，所有參考專案和套件中的附屬資產都會複製到 `_content/{LIBRARY NAME}/` 下已發佈應用程式的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f8fc0-183">Razor 視圖、頁面、控制器、頁面模型、 [razor 元件](xref:blazor/class-libraries)、[視圖元件](xref:mvc/views/view-components)和資料模型可以內建于 RAZOR 類別庫（RCL）中。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="f8fc0-184">RCL 可以封裝和重複使用。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="f8fc0-185">應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="f8fc0-186">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="f8fc0-187">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f8fc0-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="f8fc0-188">建立包含 Razor UI 的類別庫</span><span class="sxs-lookup"><span data-stu-id="f8fc0-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8fc0-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8fc0-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8fc0-190">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f8fc0-191">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f8fc0-192">命名程式庫 (例如，"RazorClassLib") > [確定]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="f8fc0-193">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="f8fc0-194">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f8fc0-195">選取 [ **Razor 類別庫**] > **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="f8fc0-196">RCL 具有下列專案檔：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f8fc0-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f8fc0-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f8fc0-198">從命令列執行 `dotnet new razorclasslib`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="f8fc0-199">例如:</span><span class="sxs-lookup"><span data-stu-id="f8fc0-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="f8fc0-200">如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="f8fc0-201">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="f8fc0-202">新增 Razor 檔案至 RCL。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="f8fc0-203">ASP.NET Core 範本假設 RCL 內容位於*領域*資料夾。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="f8fc0-204">請參閱[RCL 頁面配置](#rcl-pages-layout)，以建立會在 `~/Pages` 中公開內容的 RCL，而不是 `~/Areas/Pages`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="f8fc0-205">參考 RCL 內容</span><span class="sxs-lookup"><span data-stu-id="f8fc0-205">Reference RCL content</span></span>

<span data-ttu-id="f8fc0-206">RCL 可以由下列各項參考：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="f8fc0-207">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-207">NuGet package.</span></span> <span data-ttu-id="f8fc0-208">請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="f8fc0-209">*{ProjectName}.csproj*。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="f8fc0-210">請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="f8fc0-211">逐步解說：建立 RCL 專案，並從 Razor Pages 專案使用</span><span class="sxs-lookup"><span data-stu-id="f8fc0-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="f8fc0-212">您可以下載[完整專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)並測試它，而不是建立它。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="f8fc0-213">下載範例包含額外的程式碼和連結，讓您輕鬆地測試專案。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="f8fc0-214">您可以在[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/6098)留下意見反應以及您對下載範例與逐步指示的評論。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="f8fc0-215">測試下載應用程式</span><span class="sxs-lookup"><span data-stu-id="f8fc0-215">Test the download app</span></span>

<span data-ttu-id="f8fc0-216">如果您尚未下載已完成的應用程式，而是要建立逐步解說專案，請跳至[下一節](#create-an-rcl)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8fc0-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8fc0-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8fc0-218">在 Visual Studio 中開啟 *.sln* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="f8fc0-219">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f8fc0-220">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f8fc0-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f8fc0-221">從命令提示字元中在 *cli* 目錄中，建置 RCL 和 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="f8fc0-222">移至 *WebApp1* 目錄並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="f8fc0-223">遵循[測試 WebApp1](#test-webapp1) 中的指示執行。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="f8fc0-224">建立 RCL</span><span class="sxs-lookup"><span data-stu-id="f8fc0-224">Create an RCL</span></span>

<span data-ttu-id="f8fc0-225">在本節中，會建立 RCL。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-225">In this section, an RCL is created.</span></span> <span data-ttu-id="f8fc0-226">會有 Razor 檔案新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8fc0-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8fc0-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8fc0-228">建立 RCL 專案：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-228">Create the RCL project:</span></span>

* <span data-ttu-id="f8fc0-229">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f8fc0-230">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f8fc0-231">將應用程式命名為**RazorUIClassLib** > **OK**。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="f8fc0-232">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f8fc0-233">選取 [ **Razor 類別庫**] > **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="f8fc0-234">新增名為 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 的 Razor 部分檢視檔。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f8fc0-235">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f8fc0-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f8fc0-236">從命令列執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="f8fc0-237">上述命令：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-237">The preceding commands:</span></span>

* <span data-ttu-id="f8fc0-238">建立 `RazorUIClassLib` RCL。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="f8fc0-239">建立 Razor _Message 頁面，並將它新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="f8fc0-240">`-np` 參數會建立頁面而不使用 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="f8fc0-241">會建立[_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view)檔案，並將它新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="f8fc0-242">*_ViewStart.cshtml*檔案，才能使用 Razor 頁面專案 （這會新增下一節） 的配置。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="f8fc0-243">加入專案中的 Razor 檔案和資料夾</span><span class="sxs-lookup"><span data-stu-id="f8fc0-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="f8fc0-244">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="f8fc0-245">將 *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="f8fc0-246">需要 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 才能使用部分檢視 (`<partial name="_Message" />`)。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="f8fc0-247">您可以新增 *_ViewImports.cshtml* 檔案，而不要包含 `@addTagHelper` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f8fc0-248">例如:</span><span class="sxs-lookup"><span data-stu-id="f8fc0-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="f8fc0-249">如需詳細資訊 *_ViewImports.cshtml*，請參閱[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="f8fc0-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="f8fc0-250">建置類別庫，以確認沒有任何編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="f8fc0-251">建置輸出包含 *RazorUIClassLib.dll* 和 *RazorUIClassLib.Views.dll*。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="f8fc0-252">*RazorUIClassLib.Views.dll* 包含已編譯的 Razor 內容。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="f8fc0-253">從 Razor 頁面專案使用 Razor UI 程式庫</span><span class="sxs-lookup"><span data-stu-id="f8fc0-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8fc0-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8fc0-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8fc0-255">建立 Razor 頁面 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="f8fc0-256">在**方案總管**中，以滑鼠右鍵按一下方案 **> 新增**[> 個**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="f8fc0-257">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f8fc0-258">將應用程式命名為 **WebApp1**。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="f8fc0-259">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f8fc0-260">選取 [ **Web 應用程式**] > **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="f8fc0-261">從 [方案總管]，以滑鼠右鍵按一下 **WebApp1**，然後選取 [設定為啟始專案]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="f8fc0-262">在**方案總管**中，以滑鼠右鍵按一下 WebApp1 ，然後選取 組建相依性 >**專案**相依性。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="f8fc0-263">核取 **RazorUIClassLib** 作為 **WebApp1** 的相依性。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="f8fc0-264">在**方案總管**中，以滑鼠右鍵按一下 [ **WebApp1** ]，然後選取 [**新增**>**參考**]。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="f8fc0-265">在 **參考管理員** 對話方塊中，檢查**RazorUIClassLib** >**確定**。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="f8fc0-266">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f8fc0-267">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f8fc0-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f8fc0-268">建立 Razor Pages web 應用程式，以及包含 Razor Pages 應用程式和 RCL 的方案檔：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="f8fc0-269">建置和執行 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="f8fc0-270">測試 WebApp1</span><span class="sxs-lookup"><span data-stu-id="f8fc0-270">Test WebApp1</span></span>

<span data-ttu-id="f8fc0-271">流覽至 `/MyFeature/Page1`，確認 Razor UI 類別庫正在使用中。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="f8fc0-272">覆寫檢視、部分檢視和頁面</span><span class="sxs-lookup"><span data-stu-id="f8fc0-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="f8fc0-273">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="f8fc0-274">例如，將*WebApp1/Areas/MyFeature/Pages/Page1. cshtml*新增到 WebApp1，WebApp1 中的 page1 會優先于 RCL 中的 page1。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="f8fc0-275">在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="f8fc0-276">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="f8fc0-277">更新標記來指示新的位置。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="f8fc0-278">建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="f8fc0-279">RCL 網頁版面配置</span><span class="sxs-lookup"><span data-stu-id="f8fc0-279">RCL Pages layout</span></span>

<span data-ttu-id="f8fc0-280">參考 RCL 內容，就好像它是 web 應用程式的一部分*頁*資料夾中，建立 RCL 專案檔案結構如下：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="f8fc0-281">*RazorUIClassLib/頁面*</span><span class="sxs-lookup"><span data-stu-id="f8fc0-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="f8fc0-282">*RazorUIClassLib/網頁/共用*</span><span class="sxs-lookup"><span data-stu-id="f8fc0-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="f8fc0-283">假設*RazorUIClassLib/網頁/Shared*包含兩個部分的檔案： *_Header.cshtml*並 *_Footer.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f8fc0-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="f8fc0-284">`<partial>`標記新增到 *_Layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="f8fc0-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
