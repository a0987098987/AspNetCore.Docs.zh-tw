---
title: ASP.NET Core 類別庫中的可重複使用 Razor UI
author: Rick-Anderson
description: 說明如何在ASP.NET Core 中的類庫中使用部分檢視創建可重用的 Razor UI。
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: f24dc62eba345a8a3d35143805b4966cb51832fa
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667563"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="20681-103">使用 ASP.NET Core 中的 Razor 函式庫專案建立可重用的 UI</span><span class="sxs-lookup"><span data-stu-id="20681-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="20681-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="20681-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="20681-105">Razor 檢視、頁面、控制器、頁面模型[、Razor 元件](xref:blazor/class-libraries)、[檢視元件](xref:mvc/views/view-components)和資料模型可以內置到 Razor 類庫 (RCL) 中。</span><span class="sxs-lookup"><span data-stu-id="20681-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="20681-106">RCL 可以封裝和重複使用。</span><span class="sxs-lookup"><span data-stu-id="20681-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="20681-107">應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。</span><span class="sxs-lookup"><span data-stu-id="20681-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="20681-108">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="20681-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="20681-109">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="20681-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="20681-110">建立包含 Razor UI 的類別庫</span><span class="sxs-lookup"><span data-stu-id="20681-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="20681-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20681-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="20681-112">從視覺化工作室選擇**建立新專案**。</span><span class="sxs-lookup"><span data-stu-id="20681-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="20681-113">選擇**剃刀類別庫**>**下一個**。</span><span class="sxs-lookup"><span data-stu-id="20681-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="20681-114">命名庫(例如,"RazorClassLib"),>**建立**。</span><span class="sxs-lookup"><span data-stu-id="20681-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="20681-115">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="20681-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="20681-116">如果需要支援檢視,請選擇 **「支援」 頁面與檢視**。</span><span class="sxs-lookup"><span data-stu-id="20681-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="20681-117">默認情況下,僅支援剃刀頁。</span><span class="sxs-lookup"><span data-stu-id="20681-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="20681-118">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="20681-118">Select **Create**.</span></span>

<span data-ttu-id="20681-119">默認情況下,Razor 類庫 (RCL) 範本預設為 Razor 元件開發。</span><span class="sxs-lookup"><span data-stu-id="20681-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="20681-120">支援**頁面和檢視**「選項支援頁面和檢視。</span><span class="sxs-lookup"><span data-stu-id="20681-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="20681-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20681-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20681-122">從命令列執行 `dotnet new razorclasslib`。</span><span class="sxs-lookup"><span data-stu-id="20681-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="20681-123">例如：</span><span class="sxs-lookup"><span data-stu-id="20681-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="20681-124">默認情況下,Razor 類庫 (RCL) 範本預設為 Razor 元件開發。</span><span class="sxs-lookup"><span data-stu-id="20681-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="20681-125">通過`--support-pages-and-views`選項`dotnet new razorclasslib --support-pages-and-views`() 以提供對頁面和檢視的支援。</span><span class="sxs-lookup"><span data-stu-id="20681-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="20681-126">如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="20681-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="20681-127">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="20681-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="20681-128">新增 Razor 檔案至 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="20681-129">ASP.NET核心範本假定 RCL 內容位於 *「區域」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="20681-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="20681-130">請參閱[RCL 頁面佈局](#rcl-pages-layout)以`~/Pages``~/Areas/Pages`創建在 而不是公開 內容的 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="20681-131">參考 RCL 內容</span><span class="sxs-lookup"><span data-stu-id="20681-131">Reference RCL content</span></span>

<span data-ttu-id="20681-132">RCL 可以由下列各項參考：</span><span class="sxs-lookup"><span data-stu-id="20681-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="20681-133">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="20681-133">NuGet package.</span></span> <span data-ttu-id="20681-134">請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="20681-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="20681-135">*{ProjectName}.csproj*。</span><span class="sxs-lookup"><span data-stu-id="20681-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="20681-136">請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。</span><span class="sxs-lookup"><span data-stu-id="20681-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="20681-137">覆寫檢視、部分檢視和頁面</span><span class="sxs-lookup"><span data-stu-id="20681-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="20681-138">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="20681-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="20681-139">例如,將*WebApp1/區域/我的功能/頁面/Page1.cshtml*添加到 WebApp1,WebApp1 中的第 1 頁將優先於 RCL 中的第 1 頁。</span><span class="sxs-lookup"><span data-stu-id="20681-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="20681-140">在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。</span><span class="sxs-lookup"><span data-stu-id="20681-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="20681-141">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="20681-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="20681-142">更新標記來指示新的位置。</span><span class="sxs-lookup"><span data-stu-id="20681-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="20681-143">建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="20681-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="20681-144">RCL 頁面配置</span><span class="sxs-lookup"><span data-stu-id="20681-144">RCL Pages layout</span></span>

<span data-ttu-id="20681-145">要參考 RCL 內容,就好像它是 Web 應用的*Pages*資料夾一樣,請建立具有以下檔案結構的 RCL 專案:</span><span class="sxs-lookup"><span data-stu-id="20681-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="20681-146">*RazorUIClassLib/頁面*</span><span class="sxs-lookup"><span data-stu-id="20681-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="20681-147">*RazorUIClassLib/頁面/共用*</span><span class="sxs-lookup"><span data-stu-id="20681-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="20681-148">假設*RazorUIClassLib/Pages/Shared*包含兩個部分檔 *:_Header.cshtml*和 *_Footer.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="20681-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="20681-149">標記`<partial>`可以加入 *_Layout.cshtml*檔案中:</span><span class="sxs-lookup"><span data-stu-id="20681-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="20681-150">使用靜態資產建立 RCL</span><span class="sxs-lookup"><span data-stu-id="20681-150">Create an RCL with static assets</span></span>

<span data-ttu-id="20681-151">RCL 可能需要可供 RCL 或 RCL 使用應用引用的配套靜態資產。</span><span class="sxs-lookup"><span data-stu-id="20681-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="20681-152">ASP.NET核心允許創建包含可供使用應用的靜態資產的 LPL。</span><span class="sxs-lookup"><span data-stu-id="20681-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="20681-153">要將配套資產包含在 RCL 中,請在類庫中創建一個*wwwroot*資料夾,並在該資料夾中包含任何必需的檔。</span><span class="sxs-lookup"><span data-stu-id="20681-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="20681-154">打包 RCL 時 *,wwwroot*資料夾中的所有配套資產將自動包含在包中。</span><span class="sxs-lookup"><span data-stu-id="20681-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="20681-155">排除靜態資產</span><span class="sxs-lookup"><span data-stu-id="20681-155">Exclude static assets</span></span>

<span data-ttu-id="20681-156">要排除靜態資產,將所需的排除路徑添加到專案檔中`$(DefaultItemExcludes)`的屬性組。</span><span class="sxs-lookup"><span data-stu-id="20681-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="20681-157">分號 (`;`的單獨條目)。</span><span class="sxs-lookup"><span data-stu-id="20681-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="20681-158">在下面的範例中 *,wwwroot*資料夾中的*lib.css*樣式表不被視爲靜態資產,並且不包括在已發佈的 RCL 中:</span><span class="sxs-lookup"><span data-stu-id="20681-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="20681-159">打字文本整合</span><span class="sxs-lookup"><span data-stu-id="20681-159">Typescript integration</span></span>

<span data-ttu-id="20681-160">在 RCL 中包含 TypeScript 檔,請進行以下操作:</span><span class="sxs-lookup"><span data-stu-id="20681-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="20681-161">將 TypeScript 檔 *(.ts*) 放在*wwwroot*資料夾之外。</span><span class="sxs-lookup"><span data-stu-id="20681-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="20681-162">例如,將檔案放在*客戶端*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="20681-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="20681-163">為*wwwroot*資料夾配置 TypeScript 生成輸出。</span><span class="sxs-lookup"><span data-stu-id="20681-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="20681-164">在`TypescriptOutDir`專案檔中的`PropertyGroup`中 設定 屬性:</span><span class="sxs-lookup"><span data-stu-id="20681-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="20681-165">以專案檔案中 新增`ResolveCurrentProjectStaticWebAssets``PropertyGroup`以下目標,將 TypeScript 目標作為目標的相依項:</span><span class="sxs-lookup"><span data-stu-id="20681-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="20681-166">使用參考的 RCL 中的內容</span><span class="sxs-lookup"><span data-stu-id="20681-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="20681-167">包含在 RCL 的*wwwroot*資料夾中的檔案會`_content/{LIBRARY NAME}/`公開到前綴 下的 RCL 或使用的應用程式。</span><span class="sxs-lookup"><span data-stu-id="20681-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="20681-168">例如,名為*Razor.Class.Lib*的庫會導致`_content/Razor.Class.Lib/`在處的靜態內容路徑。</span><span class="sxs-lookup"><span data-stu-id="20681-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="20681-169">生成 NuGet 套件且程式集名稱與包 ID 不同時`{LIBRARY NAME}`,請使用的包 ID。</span><span class="sxs-lookup"><span data-stu-id="20681-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="20681-170">使用的應用程式引用函式庫提供的靜態`<script>``<style>`資源,`<img>`以及、和其他 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="20681-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="20681-171">使用要使用靜`Startup.Configure`[態檔案支援](xref:fundamentals/static-files):</span><span class="sxs-lookup"><span data-stu-id="20681-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="20681-172">從生成輸出 ()`dotnet run`運行使用應用時,靜態 Web 資產預設在開發環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="20681-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="20681-173">要支援其他環境中的資產,請調用`UseStaticWebAssets` *Program.cs*中的主機生成器:</span><span class="sxs-lookup"><span data-stu-id="20681-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="20681-174">從`UseStaticWebAssets`已發佈輸出 () 運行應用`dotnet publish`時,不需要調用。</span><span class="sxs-lookup"><span data-stu-id="20681-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="20681-175">多項目開發流程</span><span class="sxs-lookup"><span data-stu-id="20681-175">Multi-project development flow</span></span>

<span data-ttu-id="20681-176">使用應用程式執行時:</span><span class="sxs-lookup"><span data-stu-id="20681-176">When the consuming app runs:</span></span>

* <span data-ttu-id="20681-177">RCL 中的資產保留在其原始資料夾中。</span><span class="sxs-lookup"><span data-stu-id="20681-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="20681-178">資產不會移動到使用的應用。</span><span class="sxs-lookup"><span data-stu-id="20681-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="20681-179">重建 RCL 後,RCL 的*wwwroot*資料夾中的任何更改都將反映在使用應用中,而不重新生成使用的應用程式。</span><span class="sxs-lookup"><span data-stu-id="20681-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="20681-180">生成 RCL 時,將生成描述靜態 Web 資產位置的清單。</span><span class="sxs-lookup"><span data-stu-id="20681-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="20681-181">使用的應用程式在運行時讀取清單以使用引用的專案和包中的資產。</span><span class="sxs-lookup"><span data-stu-id="20681-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="20681-182">將新資產添加到 RCL 時,必須重新生成 RCL 以更新其清單,然後才能使用的應用訪問新資產。</span><span class="sxs-lookup"><span data-stu-id="20681-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="20681-183">發佈</span><span class="sxs-lookup"><span data-stu-id="20681-183">Publish</span></span>

<span data-ttu-id="20681-184">發佈應用時,所有引用的專案和包的配套資源將複製到`_content/{LIBRARY NAME}/`下 已發佈應用的*wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="20681-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="20681-185">Razor 檢視、頁面、控制器、頁面模型[、Razor 元件](xref:blazor/class-libraries)、[檢視元件](xref:mvc/views/view-components)和資料模型可以內置到 Razor 類庫 (RCL) 中。</span><span class="sxs-lookup"><span data-stu-id="20681-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="20681-186">RCL 可以封裝和重複使用。</span><span class="sxs-lookup"><span data-stu-id="20681-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="20681-187">應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。</span><span class="sxs-lookup"><span data-stu-id="20681-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="20681-188">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="20681-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="20681-189">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="20681-189">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="20681-190">建立包含 Razor UI 的類別庫</span><span class="sxs-lookup"><span data-stu-id="20681-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="20681-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20681-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="20681-192">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="20681-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="20681-193">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="20681-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="20681-194">命名程式庫 (例如，"RazorClassLib") > [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="20681-195">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="20681-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="20681-196">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="20681-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="20681-197">選取 [Razor 類別庫]**[確定]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="20681-198">RCL 具有以下項目檔:</span><span class="sxs-lookup"><span data-stu-id="20681-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-cli"></a>[<span data-ttu-id="20681-199">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20681-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20681-200">從命令列執行 `dotnet new razorclasslib`。</span><span class="sxs-lookup"><span data-stu-id="20681-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="20681-201">例如：</span><span class="sxs-lookup"><span data-stu-id="20681-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="20681-202">如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="20681-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="20681-203">若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。</span><span class="sxs-lookup"><span data-stu-id="20681-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="20681-204">新增 Razor 檔案至 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="20681-205">ASP.NET核心範本假定 RCL 內容位於 *「區域」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="20681-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="20681-206">請參閱[RCL 頁面佈局](#rcl-pages-layout)以`~/Pages``~/Areas/Pages`創建在 而不是公開 內容的 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="20681-207">參考 RCL 內容</span><span class="sxs-lookup"><span data-stu-id="20681-207">Reference RCL content</span></span>

<span data-ttu-id="20681-208">RCL 可以由下列各項參考：</span><span class="sxs-lookup"><span data-stu-id="20681-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="20681-209">NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="20681-209">NuGet package.</span></span> <span data-ttu-id="20681-210">請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="20681-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="20681-211">*{ProjectName}.csproj*。</span><span class="sxs-lookup"><span data-stu-id="20681-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="20681-212">請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。</span><span class="sxs-lookup"><span data-stu-id="20681-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="20681-213">演練:建立 RCL 專案並從 Razor 頁面專案使用</span><span class="sxs-lookup"><span data-stu-id="20681-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="20681-214">您可以下載[完整專案](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)並測試它，而不是建立它。</span><span class="sxs-lookup"><span data-stu-id="20681-214">You can download the [complete project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="20681-215">下載範例包含額外的程式碼和連結，讓您輕鬆地測試專案。</span><span class="sxs-lookup"><span data-stu-id="20681-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="20681-216">您可以在[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/6098)留下意見反應以及您對下載範例與逐步指示的評論。</span><span class="sxs-lookup"><span data-stu-id="20681-216">You can leave feedback in [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="20681-217">測試下載應用程式</span><span class="sxs-lookup"><span data-stu-id="20681-217">Test the download app</span></span>

<span data-ttu-id="20681-218">如果您尚未下載已完成的應用程式，而是要建立逐步解說專案，請跳至[下一節](#create-an-rcl)。</span><span class="sxs-lookup"><span data-stu-id="20681-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="20681-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20681-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20681-220">在 Visual Studio 中開啟 *.sln* 檔案。</span><span class="sxs-lookup"><span data-stu-id="20681-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="20681-221">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="20681-221">Run the app.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="20681-222">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20681-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20681-223">從命令提示字元中在 *cli* 目錄中，建置 RCL 和 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="20681-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="20681-224">移至 *WebApp1* 目錄並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="20681-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="20681-225">遵循[測試 WebApp1](#test-webapp1) 中的指示執行。</span><span class="sxs-lookup"><span data-stu-id="20681-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="20681-226">建立 RCL</span><span class="sxs-lookup"><span data-stu-id="20681-226">Create an RCL</span></span>

<span data-ttu-id="20681-227">在本節中,將創建一個 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-227">In this section, an RCL is created.</span></span> <span data-ttu-id="20681-228">會有 Razor 檔案新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="20681-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20681-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20681-230">建立 RCL 專案：</span><span class="sxs-lookup"><span data-stu-id="20681-230">Create the RCL project:</span></span>

* <span data-ttu-id="20681-231">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="20681-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="20681-232">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="20681-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="20681-233">命名應用程式**RazorUClassLib** > **OK**。</span><span class="sxs-lookup"><span data-stu-id="20681-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="20681-234">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="20681-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="20681-235">選取 [Razor 類別庫]**[確定]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="20681-236">新增名為 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 的 Razor 部分檢視檔。</span><span class="sxs-lookup"><span data-stu-id="20681-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="20681-237">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20681-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20681-238">從命令列執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="20681-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="20681-239">上述命令：</span><span class="sxs-lookup"><span data-stu-id="20681-239">The preceding commands:</span></span>

* <span data-ttu-id="20681-240">創建`RazorUIClassLib`RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="20681-241">建立 Razor _Message 頁面，並將它新增至 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="20681-242">`-np` 參數會建立頁面而不使用 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="20681-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="20681-243">建立[_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view)檔並將其添加到 RCL。</span><span class="sxs-lookup"><span data-stu-id="20681-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="20681-244">*_ViewStart.cshtml*檔需要使用 Razor Pages 專案的佈局(下一節將添加)。</span><span class="sxs-lookup"><span data-stu-id="20681-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="20681-245">將 Razor 檔案與資料夾加入項目中</span><span class="sxs-lookup"><span data-stu-id="20681-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="20681-246">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="20681-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="20681-247">將 *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* 中的標記取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="20681-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="20681-248">需要 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 才能使用部分檢視 (`<partial name="_Message" />`)。</span><span class="sxs-lookup"><span data-stu-id="20681-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="20681-249">您可以新增 *_ViewImports.cshtml* 檔案，而不要包含 `@addTagHelper` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="20681-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="20681-250">例如：</span><span class="sxs-lookup"><span data-stu-id="20681-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="20681-251">有關 *_ViewImports.cshtml*的詳細資訊,請參閱[匯入分享指令](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="20681-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="20681-252">建置類別庫，以確認沒有任何編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="20681-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="20681-253">建置輸出包含 *RazorUIClassLib.dll* 和 *RazorUIClassLib.Views.dll*。</span><span class="sxs-lookup"><span data-stu-id="20681-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="20681-254">*RazorUIClassLib.Views.dll* 包含已編譯的 Razor 內容。</span><span class="sxs-lookup"><span data-stu-id="20681-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="20681-255">從 Razor 頁面專案使用 Razor UI 程式庫</span><span class="sxs-lookup"><span data-stu-id="20681-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="20681-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20681-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20681-257">建立 Razor 頁面 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="20681-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="20681-258">在 [方案總管]\*\*\*\* 中，以滑鼠右鍵按一下解決方案 > [新增]**[新增專案]** >  \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="20681-259">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="20681-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="20681-260">將應用程式命名為 **WebApp1**。</span><span class="sxs-lookup"><span data-stu-id="20681-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="20681-261">確認已選取 **ASP.NET Core 2.1** 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="20681-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="20681-262">選取 [Web 應用程式]**[確定]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="20681-263">從 [方案總管]\*\*\*\*，以滑鼠右鍵按一下 **WebApp1**，然後選取 [設定為啟始專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="20681-264">從方案總管\*\*\*\*，以滑鼠右鍵按一下 **WebApp1**，然後選取 [建置相依性]**[專案相依性]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="20681-265">核取 **RazorUIClassLib** 作為 **WebApp1** 的相依性。</span><span class="sxs-lookup"><span data-stu-id="20681-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="20681-266">從 [方案總管]\*\*\*\*，以滑鼠右鍵按一下 **WebApp1**，然後選取 [新增]**[參考]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="20681-267">在 [參考管理員]\*\*\*\* 對話方塊中，核取 **RazorUIClassLib[確定]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="20681-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="20681-268">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="20681-268">Run the app.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="20681-269">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20681-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20681-270">建立 Razor 網頁網頁 應用程式與包含 Razor 網頁應用和 RCL 的解決方案檔:</span><span class="sxs-lookup"><span data-stu-id="20681-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="20681-271">建置和執行 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="20681-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="20681-272">測試 WebApp1</span><span class="sxs-lookup"><span data-stu-id="20681-272">Test WebApp1</span></span>

<span data-ttu-id="20681-273">瀏覽`/MyFeature/Page1`以 驗證 Razor UI 類庫是否正在使用中。</span><span class="sxs-lookup"><span data-stu-id="20681-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="20681-274">覆寫檢視、部分檢視和頁面</span><span class="sxs-lookup"><span data-stu-id="20681-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="20681-275">在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。</span><span class="sxs-lookup"><span data-stu-id="20681-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="20681-276">例如,將*WebApp1/區域/我的功能/頁面/Page1.cshtml*添加到 WebApp1,WebApp1 中的第 1 頁將優先於 RCL 中的第 1 頁。</span><span class="sxs-lookup"><span data-stu-id="20681-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="20681-277">在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。</span><span class="sxs-lookup"><span data-stu-id="20681-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="20681-278">將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="20681-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="20681-279">更新標記來指示新的位置。</span><span class="sxs-lookup"><span data-stu-id="20681-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="20681-280">建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="20681-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="20681-281">RCL 頁面配置</span><span class="sxs-lookup"><span data-stu-id="20681-281">RCL Pages layout</span></span>

<span data-ttu-id="20681-282">要參考 RCL 內容,就好像它是 Web 應用的*Pages*資料夾一樣,請建立具有以下檔案結構的 RCL 專案:</span><span class="sxs-lookup"><span data-stu-id="20681-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="20681-283">*RazorUIClassLib/頁面*</span><span class="sxs-lookup"><span data-stu-id="20681-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="20681-284">*RazorUIClassLib/頁面/共用*</span><span class="sxs-lookup"><span data-stu-id="20681-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="20681-285">假設*RazorUIClassLib/Pages/Shared*包含兩個部分檔 *:_Header.cshtml*和 *_Footer.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="20681-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="20681-286">標記`<partial>`可以加入 *_Layout.cshtml*檔案中:</span><span class="sxs-lookup"><span data-stu-id="20681-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="20681-287">其他資源</span><span class="sxs-lookup"><span data-stu-id="20681-287">Additional resources</span></span>

* <xref:blazor/class-libraries>
