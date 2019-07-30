---
title: Swashbuckle 與 ASP.NET Core 使用者入門
author: zuckerthoben
description: 了解如何將 Swashbuckle 新增至 ASP.NET Core Web API 專案，以整合 Swagger UI。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 0ffd437bbb48ef1c7a9159fbf3ac41441613f434
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372056"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="889e0-103">Swashbuckle 與 ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="889e0-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="889e0-104">由 [Shayne Boyer](https://twitter.com/spboyer) 和 [Scott Addie](https://twitter.com/Scott_Addie) 提供</span><span class="sxs-lookup"><span data-stu-id="889e0-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="889e0-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="889e0-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="889e0-106">Swashbuckle 有三個主要元件：</span><span class="sxs-lookup"><span data-stu-id="889e0-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="889e0-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/)：Swagger 物件模型和中介軟體，用來公開 `SwaggerDocument` 物件作為 JSON 端點。</span><span class="sxs-lookup"><span data-stu-id="889e0-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="889e0-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/)：Swagger 產生器，可直接從您的路由、控制器和模型建置 `SwaggerDocument` 物件。</span><span class="sxs-lookup"><span data-stu-id="889e0-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="889e0-109">它通常會結合 Swagger 端點中介軟體，以便自動公開 Swagger JSON。</span><span class="sxs-lookup"><span data-stu-id="889e0-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="889e0-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/)：Swagger UI 工具的內嵌版本。</span><span class="sxs-lookup"><span data-stu-id="889e0-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="889e0-111">它可解譯 Swagger JSON 來建置描述 Web API 功能的可自訂豐富體驗。</span><span class="sxs-lookup"><span data-stu-id="889e0-111">It interprets Swagger JSON to build a rich, customizable experience for describing the web API functionality.</span></span> <span data-ttu-id="889e0-112">它包含公用方法的內建測試載入器。</span><span class="sxs-lookup"><span data-stu-id="889e0-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="889e0-113">套件安裝</span><span class="sxs-lookup"><span data-stu-id="889e0-113">Package installation</span></span>

<span data-ttu-id="889e0-114">可使用下列方法新增 Swashbuckle：</span><span class="sxs-lookup"><span data-stu-id="889e0-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="889e0-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="889e0-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="889e0-116">從 [套件管理員主控台]  視窗中：</span><span class="sxs-lookup"><span data-stu-id="889e0-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="889e0-117">移至 [檢視]   > [其他視窗]   > [套件管理員主控台] </span><span class="sxs-lookup"><span data-stu-id="889e0-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="889e0-118">巡覽至 *TodoApi.csproj* 檔案所在目錄</span><span class="sxs-lookup"><span data-stu-id="889e0-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="889e0-119">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="889e0-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore -Version 5.0.0-rc2
    ```

* <span data-ttu-id="889e0-120">從 [管理 NuGet 套件]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="889e0-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="889e0-121">在 [方案總管]   > [管理 NuGet 套件]  中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="889e0-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="889e0-122">將 [套件來源]  設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="889e0-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="889e0-123">請確定已啟用 [包含發行前版本] 選項</span><span class="sxs-lookup"><span data-stu-id="889e0-123">Ensure the "Include prerelease" option is enabled</span></span>
  * <span data-ttu-id="889e0-124">在搜尋方塊中輸入 "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="889e0-124">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="889e0-125">從 [瀏覽]  索引標籤中選取最新的 "Swashbuckle.AspNetCore" 套件，並按一下 [安裝] </span><span class="sxs-lookup"><span data-stu-id="889e0-125">Select the latest "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="889e0-126">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="889e0-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="889e0-127">在 [Solution Pad]   > [新增套件]  中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="889e0-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="889e0-128">將 [新增套件]  視窗的 [來源]  下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="889e0-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="889e0-129">請確定已啟用 [選定發行前版本的套件] 選項</span><span class="sxs-lookup"><span data-stu-id="889e0-129">Ensure the "Show pre-release packages" option is enabled</span></span>
* <span data-ttu-id="889e0-130">在搜尋方塊中輸入 "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="889e0-130">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="889e0-131">從結果窗格中選取最新的 "Swashbuckle.AspNetCore" 套件，並按一下 [新增套件] </span><span class="sxs-lookup"><span data-stu-id="889e0-131">Select the latest "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="889e0-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="889e0-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="889e0-133">從 [整合式終端機]  執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="889e0-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.0.0-rc2
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="889e0-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="889e0-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="889e0-135">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="889e0-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.0.0-rc2
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="889e0-136">新增和設定 Swagger 中介軟體</span><span class="sxs-lookup"><span data-stu-id="889e0-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="889e0-137">在 `Startup` 類別中，匯入下列命名空間以使用 `OpenApiInfo` 類別：</span><span class="sxs-lookup"><span data-stu-id="889e0-137">In the `Startup` class, import the following namespace to use the `OpenApiInfo` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="889e0-138">將 Swagger 產生器新增至 `Startup.ConfigureServices` 方法中的服務集合：</span><span class="sxs-lookup"><span data-stu-id="889e0-138">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="889e0-139">在 `Startup.Configure` 方法中，啟用中介軟體為產生的 JSON 文件和 Swagger UI 提供服務：</span><span class="sxs-lookup"><span data-stu-id="889e0-139">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="889e0-140">上述 `UseSwaggerUI` 方法呼叫會啟用[靜態檔案中介軟體](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="889e0-140">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="889e0-141">如果以 .NET Framework 或 .NET Core 1.x 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="889e0-141">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="889e0-142">啟動應用程式，並巡覽至 `http://localhost:<port>/swagger/v1/swagger.json`。</span><span class="sxs-lookup"><span data-stu-id="889e0-142">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="889e0-143">描述端點的已產生文件隨即出現，如 [Swagger 規格 (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson) 中所示。</span><span class="sxs-lookup"><span data-stu-id="889e0-143">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="889e0-144">您可以在 `http://localhost:<port>/swagger` 找到 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="889e0-144">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="889e0-145">透過 Swagger UI 探索 API，並將其併入其他程式。</span><span class="sxs-lookup"><span data-stu-id="889e0-145">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="889e0-146">若要在應用程式的根目錄 (`http://localhost:<port>/`) 上提供 Swagger UI，請將 `RoutePrefix` 屬性設為空字串：</span><span class="sxs-lookup"><span data-stu-id="889e0-146">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="889e0-147">若使用目錄搭配 IIS 或 反向 Proxy，請將 Swagger 端點設定為使用 `./` 前置詞的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="889e0-147">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="889e0-148">例如，`./swagger/v1/swagger.json`。</span><span class="sxs-lookup"><span data-stu-id="889e0-148">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="889e0-149">使用 `/swagger/v1/swagger.json` 指示應用程式在 URL 的真實根目錄 (若已使用，請加上路由前置詞) 尋找 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="889e0-149">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="889e0-150">例如，使用 `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` 而不是 `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`。</span><span class="sxs-lookup"><span data-stu-id="889e0-150">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="889e0-151">自訂與擴充</span><span class="sxs-lookup"><span data-stu-id="889e0-151">Customize and extend</span></span>

<span data-ttu-id="889e0-152">Swagger 提供用來記載物件模型和自訂 UI 以符合佈景主題的選項。</span><span class="sxs-lookup"><span data-stu-id="889e0-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="889e0-153">API 資訊與描述</span><span class="sxs-lookup"><span data-stu-id="889e0-153">API info and description</span></span>

<span data-ttu-id="889e0-154">傳遞至 `AddSwaggerGen` 方法的組態動作會新增作者、授權和描述等資訊：</span><span class="sxs-lookup"><span data-stu-id="889e0-154">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="889e0-155">Swagger UI 會顯示版本資訊：</span><span class="sxs-lookup"><span data-stu-id="889e0-155">The Swagger UI displays the version's information:</span></span>

![含有版本資訊的 Swagger UI：描述、作者，以及查看更多連結](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="889e0-157">XML 註解</span><span class="sxs-lookup"><span data-stu-id="889e0-157">XML comments</span></span>

<span data-ttu-id="889e0-158">XML 註解可以使用下列方式啟用：</span><span class="sxs-lookup"><span data-stu-id="889e0-158">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="889e0-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="889e0-159">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="889e0-160">以滑鼠右鍵按一下 [方案總管]  中的專案，然後選取 [編輯 <專案名稱>.csproj]  。</span><span class="sxs-lookup"><span data-stu-id="889e0-160">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="889e0-161">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="889e0-161">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="889e0-162">以滑鼠右鍵按一下 [方案總管]  中的專案，然後選取 [屬性]  。</span><span class="sxs-lookup"><span data-stu-id="889e0-162">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="889e0-163">核取 [組建]  索引標籤之 [輸出]  區段下方的 [XML 文件檔]  方塊。</span><span class="sxs-lookup"><span data-stu-id="889e0-163">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="889e0-164">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="889e0-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="889e0-165">從 [Solution Pad]  中，按下 [控制項]  ，然後按一下專案名稱。</span><span class="sxs-lookup"><span data-stu-id="889e0-165">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="889e0-166">巡覽至 [工具]   > [編輯檔案]  。</span><span class="sxs-lookup"><span data-stu-id="889e0-166">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="889e0-167">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="889e0-167">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="889e0-168">開啟 [專案選項]  對話方塊 > [組建]  >[編譯器] </span><span class="sxs-lookup"><span data-stu-id="889e0-168">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="889e0-169">核取 [一般選項]  區段下方的 [產生 XML 文件]  方塊</span><span class="sxs-lookup"><span data-stu-id="889e0-169">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="889e0-170">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="889e0-170">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="889e0-171">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="889e0-171">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="889e0-172">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="889e0-172">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="889e0-173">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="889e0-173">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="889e0-174">啟用 XML 註解，可提供未記載之公用類型與成員的偵錯資訊。</span><span class="sxs-lookup"><span data-stu-id="889e0-174">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="889e0-175">未記載的類型和成員會以警告訊息表示。</span><span class="sxs-lookup"><span data-stu-id="889e0-175">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="889e0-176">例如，下列訊息指出警告碼 1591 的違規：</span><span class="sxs-lookup"><span data-stu-id="889e0-176">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="889e0-177">在專案檔中定義要忽略的警告碼清單 (以分號分隔)，即可隱藏警告。</span><span class="sxs-lookup"><span data-stu-id="889e0-177">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="889e0-178">將警告碼附加至 `$(NoWarn);` 也會套用 [C# 預設值](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16)。</span><span class="sxs-lookup"><span data-stu-id="889e0-178">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="889e0-179">若只要針對特定成員隱藏，請將程式碼放在 [#pragma 警告](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning)前置處理器指示詞中。</span><span class="sxs-lookup"><span data-stu-id="889e0-179">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="889e0-180">針對不應該透過 API 文件公開的程式碼，此方法非常有用。在下列範例中，會針對整個 `Program` 類別忽略警告碼 CS1591。</span><span class="sxs-lookup"><span data-stu-id="889e0-180">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="889e0-181">會在類別定義結尾處還原強制執行的警告碼。</span><span class="sxs-lookup"><span data-stu-id="889e0-181">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="889e0-182">以逗號分隔清單指定多個警告碼。</span><span class="sxs-lookup"><span data-stu-id="889e0-182">Specify multiple warning codes with a comma-delimited list.</span></span>

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

<span data-ttu-id="889e0-183">設定 Swagger 以使用先前指示所產生的 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="889e0-183">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="889e0-184">對於 Linux 或非 Windows 作業系統，檔案名稱和路徑可以區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="889e0-184">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="889e0-185">例如，*TodoApi.XML* 檔案在 Windows 上有效，但在 CentOS 上則無效。</span><span class="sxs-lookup"><span data-stu-id="889e0-185">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="889e0-186">在上述程式碼中，[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) 用來建置與 Web API 專案名稱相符的 XML 檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="889e0-186">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="889e0-187">[AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) 屬性用來建構 XML 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="889e0-187">The [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="889e0-188">某些 Swagger 功能 (例如輸入參數的結構描述，或來自相對應屬性的 HTTP 方法和回應碼) 能在不使用 XML 文件檔案的情況下運作。</span><span class="sxs-lookup"><span data-stu-id="889e0-188">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="889e0-189">對於大部分的功能 (也就是方法摘要，以及參數和回應碼的描述) 而言，皆必須使用 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="889e0-189">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="889e0-190">將三斜線註解新增至動作，即可透過將描述新增至區段標頭來增強 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="889e0-190">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="889e0-191">在 `Delete` 動作上方新增 [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) 項目：</span><span class="sxs-lookup"><span data-stu-id="889e0-191">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="889e0-192">Swagger UI 會顯示上述程式碼的 `<summary>` 項目的內部文字：</span><span class="sxs-lookup"><span data-stu-id="889e0-192">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![顯示 XML 註解 'Deletes a specific TodoItem.' 的 Swagger UI](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="889e0-195">UI 是由產生的 JSON 結構描述所驅動：</span><span class="sxs-lookup"><span data-stu-id="889e0-195">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="889e0-196">請將 [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) 項目新增至 `Create` 動作方法文件。</span><span class="sxs-lookup"><span data-stu-id="889e0-196">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="889e0-197">它會補充 `<summary>` 項目中指定的資訊，並提供更強固的 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="889e0-197">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="889e0-198">`<remarks>` 項目內容可以包含文字、JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="889e0-198">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="889e0-199">請注意使用這些額外註解的 UI 增強功能：</span><span class="sxs-lookup"><span data-stu-id="889e0-199">Notice the UI enhancements with these additional comments:</span></span>

![顯示額外註解的 Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="889e0-201">資料註解</span><span class="sxs-lookup"><span data-stu-id="889e0-201">Data annotations</span></span>

<span data-ttu-id="889e0-202">使用 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 命名空間中找到的屬性來裝飾模型，可協助驅動 Swagger UI 元件。</span><span class="sxs-lookup"><span data-stu-id="889e0-202">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="889e0-203">將 `[Required]` 屬性 (attribute) 新增至 `TodoItem` 類別的 `Name` 屬性 (property)：</span><span class="sxs-lookup"><span data-stu-id="889e0-203">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="889e0-204">這個屬性的存在會變更 UI 行為，並改變基礎的 JSON 結構描述：</span><span class="sxs-lookup"><span data-stu-id="889e0-204">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="889e0-205">將 `[Produces("application/json")]` 屬性新增至 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="889e0-205">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="889e0-206">其目的是要宣告控制器的動作支援回應內容類型為 *application/json*：</span><span class="sxs-lookup"><span data-stu-id="889e0-206">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="889e0-207">[回應內容類型]  下拉式清單會選取此內容類型，作為控制器 GET 動作的預設值：</span><span class="sxs-lookup"><span data-stu-id="889e0-207">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![含有預設回應內容類型的 Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="889e0-209">當 Web API 中的資料註釋使用量增加時，UI 和 API 說明頁面會變得更清楚且更有用。</span><span class="sxs-lookup"><span data-stu-id="889e0-209">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="889e0-210">描述回應類型</span><span class="sxs-lookup"><span data-stu-id="889e0-210">Describe response types</span></span>

<span data-ttu-id="889e0-211">取用 Web API 的開發人員最關心的是傳回的內容 &mdash; 特別是回應類型和錯誤碼 (如果不是標準的話)。</span><span class="sxs-lookup"><span data-stu-id="889e0-211">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="889e0-212">回應類型和錯誤碼會在 XML 註解及資料註解中表示。</span><span class="sxs-lookup"><span data-stu-id="889e0-212">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="889e0-213">`Create` 動作在成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="889e0-213">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="889e0-214">張貼的要求主體為 Null 時，會傳回 HTTP 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="889e0-214">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="889e0-215">如果 Swagger UI 中沒有適當的文件，取用者便會缺乏對這些預期結果的了解。</span><span class="sxs-lookup"><span data-stu-id="889e0-215">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="889e0-216">在下列範例中新增強調顯示的那幾行來修正該問題：</span><span class="sxs-lookup"><span data-stu-id="889e0-216">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="889e0-217">現在，Swagger UI 清楚記載了預期的 HTTP 回應碼：</span><span class="sxs-lookup"><span data-stu-id="889e0-217">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![顯示 POST 回應類別描述 'Returns the newly created Todo item'，並針對回應訊息下方的狀態碼和原因顯示 '400 - If the item is null' 的 Swagger UI](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="889e0-219">在 ASP.NET Core 2.2 或更新版本中，慣例可作為使用 `[ProducesResponseType]` 明確裝飾個別動作的替代方案。</span><span class="sxs-lookup"><span data-stu-id="889e0-219">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="889e0-220">如需詳細資訊，請參閱 <xref:web-api/advanced/conventions>。</span><span class="sxs-lookup"><span data-stu-id="889e0-220">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

### <a name="customize-the-ui"></a><span data-ttu-id="889e0-221">自訂 UI</span><span class="sxs-lookup"><span data-stu-id="889e0-221">Customize the UI</span></span>

<span data-ttu-id="889e0-222">股票 UI 既可運作又能呈現。</span><span class="sxs-lookup"><span data-stu-id="889e0-222">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="889e0-223">不過，API 的文件頁面應該表示您的品牌或佈景主題。</span><span class="sxs-lookup"><span data-stu-id="889e0-223">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="889e0-224">設定 Swashbuckle 元件商標時，需要新增資源來處理靜態檔案，然後建置裝載這些檔案的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="889e0-224">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="889e0-225">如果以 .NET Framework 或 .NET Core 1.x 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet 套件新增至您的專案：</span><span class="sxs-lookup"><span data-stu-id="889e0-225">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="889e0-226">如果以 .NET Core 2.x 為目標並使用[中繼套件](xref:fundamentals/metapackage)，則已安裝上述 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="889e0-226">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="889e0-227">啟用靜態檔案中介軟體：</span><span class="sxs-lookup"><span data-stu-id="889e0-227">Enable Static File Middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="889e0-228">從 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui/tree/master/dist)中取得 *dist* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="889e0-228">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="889e0-229">此資料夾包含 Swagger UI 頁面所需的資產。</span><span class="sxs-lookup"><span data-stu-id="889e0-229">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="889e0-230">建立 *wwwroot/swagger/ui* 資料夾，並將 *dist* 資料夾的內容複製到其中。</span><span class="sxs-lookup"><span data-stu-id="889e0-230">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="889e0-231">使用下列 CSS 在 *wwwroot/swagger/ui* 中建立 *custom.css* 檔案，以自訂頁面標頭：</span><span class="sxs-lookup"><span data-stu-id="889e0-231">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="889e0-232">在 *index.html* 檔案中的其他任何 CSS 檔案之後，參考 *custom.css*：</span><span class="sxs-lookup"><span data-stu-id="889e0-232">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="889e0-233">瀏覽至 `http://localhost:<port>/swagger/ui/index.html` 上的 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="889e0-233">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="889e0-234">在標頭的文字方塊中輸入 `http://localhost:<port>/swagger/v1/swagger.json`，然後按一下 [瀏覽]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="889e0-234">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="889e0-235">結果頁面如下所示：</span><span class="sxs-lookup"><span data-stu-id="889e0-235">The resulting page looks as follows:</span></span>

![含有自訂標頭標題的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="889e0-237">您還可以透過此頁面進行更多功能。</span><span class="sxs-lookup"><span data-stu-id="889e0-237">There's much more you can do with the page.</span></span> <span data-ttu-id="889e0-238">請在 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui)中查看 UI 資源的完整功能。</span><span class="sxs-lookup"><span data-stu-id="889e0-238">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
