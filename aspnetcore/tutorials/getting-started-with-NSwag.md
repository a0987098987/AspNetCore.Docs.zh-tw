---
title: NSwag 與 ASP.NET Core 使用者入門
author: zuckerthoben
description: 了解如何使用 NSwag 來產生 ASP.NET Core Web API 的文件和說明頁面。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 5e62a8cc50947969d42981350b65a24781929d62
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71691188"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="daac1-103">NSwag 與 ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="daac1-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="daac1-104">作者：[Christoph Nienaber](https://twitter.com/zuckerthoben)、[Rico Suter](https://rsuter.com) 及 [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="daac1-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="daac1-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="daac1-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="daac1-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="daac1-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="daac1-107">NSwag 提供下列功能：</span><span class="sxs-lookup"><span data-stu-id="daac1-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="daac1-108">能夠運用 Swagger UI 和 Swagger 產生器。</span><span class="sxs-lookup"><span data-stu-id="daac1-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="daac1-109">彈性的程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="daac1-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="daac1-110">使用 NSwag 時，您不需要有現有的 API &mdash; 您可以使用包含 Swagger 的協力廠商 API，然後產生用戶端實作。</span><span class="sxs-lookup"><span data-stu-id="daac1-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="daac1-111">NSwag 可讓您加速開發週期，並輕鬆地因應 API 變更進行調整。</span><span class="sxs-lookup"><span data-stu-id="daac1-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="daac1-112">註冊 NSwag 中介軟體</span><span class="sxs-lookup"><span data-stu-id="daac1-112">Register the NSwag middleware</span></span>

<span data-ttu-id="daac1-113">註冊 NSwag 中介軟體來：</span><span class="sxs-lookup"><span data-stu-id="daac1-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="daac1-114">為實作的 Web API 產生 Swagger 規格。</span><span class="sxs-lookup"><span data-stu-id="daac1-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="daac1-115">提供 Swagger UI 以瀏覽並測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="daac1-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="daac1-116">若要使用 [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core 中介軟體，請安裝 [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="daac1-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="daac1-117">此套件包含用以產生並提供 Swagger 規格、Swagger UI (v2 和 v3) 及 [ReDoc UI](https://github.com/Rebilly/ReDoc) 的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="daac1-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="daac1-118">請使用下列其中一種方法來安裝 NSwag NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="daac1-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="daac1-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="daac1-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="daac1-120">從 [套件管理員主控台] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="daac1-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="daac1-121">移至 [檢視] > [其他視窗] > [套件管理員主控台]</span><span class="sxs-lookup"><span data-stu-id="daac1-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="daac1-122">巡覽至 *TodoApi.csproj* 檔案所在目錄</span><span class="sxs-lookup"><span data-stu-id="daac1-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="daac1-123">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="daac1-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="daac1-124">從 [管理 NuGet 套件] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="daac1-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="daac1-125">在 [方案總管] > [管理 NuGet 套件] 中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="daac1-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="daac1-126">將 [套件來源] 設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="daac1-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="daac1-127">在搜尋方塊中輸入 "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="daac1-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="daac1-128">從 [瀏覽] 索引標籤中選取 "NSwag.AspNetCore" 套件，並按一下 [安裝]</span><span class="sxs-lookup"><span data-stu-id="daac1-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="daac1-129">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="daac1-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="daac1-130">在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="daac1-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="daac1-131">將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="daac1-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="daac1-132">在搜尋方塊中輸入 "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="daac1-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="daac1-133">從結果窗格中選取 "NSwag.AspNetCore" 套件，並按一下 [新增套件]</span><span class="sxs-lookup"><span data-stu-id="daac1-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="daac1-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="daac1-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="daac1-135">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="daac1-135">Run the following command:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="daac1-136">新增和設定 Swagger 中介軟體</span><span class="sxs-lookup"><span data-stu-id="daac1-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="daac1-137">請執行下列步驟，以在您的 ASP.NET Core 應用程式中新增及設定 Swagger：</span><span class="sxs-lookup"><span data-stu-id="daac1-137">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="daac1-138">在 `Startup.ConfigureServices` 方法中，註冊所需的 Swagger 服務：</span><span class="sxs-lookup"><span data-stu-id="daac1-138">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="daac1-139">在 `Startup.Configure` 方法中，啟用中介軟體為產生的 Swagger 規格和 SwaggerUI 提供服務：</span><span class="sxs-lookup"><span data-stu-id="daac1-139">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="daac1-140">啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="daac1-140">Launch the app.</span></span> <span data-ttu-id="daac1-141">瀏覽至：</span><span class="sxs-lookup"><span data-stu-id="daac1-141">Navigate to:</span></span>
  * <span data-ttu-id="daac1-142">`http://localhost:<port>/swagger` 以檢視 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="daac1-142">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="daac1-143">`http://localhost:<port>/swagger/v1/swagger.json` 以檢視 Swagger 規格。</span><span class="sxs-lookup"><span data-stu-id="daac1-143">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="daac1-144">程式碼產生</span><span class="sxs-lookup"><span data-stu-id="daac1-144">Code generation</span></span>

<span data-ttu-id="daac1-145">您可以選擇下列其中一個選項來利用 NSwag 的程式碼產生功能：</span><span class="sxs-lookup"><span data-stu-id="daac1-145">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="daac1-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; 一個能夠以 C# 或 TypeScript 產生 API 用戶端程式碼的 Windows 傳統型應用程式。</span><span class="sxs-lookup"><span data-stu-id="daac1-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="daac1-147">可在您專案內產生程式碼的 [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) 或 [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="daac1-147">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="daac1-148">從[命令列](https://github.com/RicoSuter/NSwag/wiki/CommandLine)使用 NSwag。</span><span class="sxs-lookup"><span data-stu-id="daac1-148">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="daac1-149">[NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="daac1-149">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) NuGet package.</span></span>
* <span data-ttu-id="daac1-150">[Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; 以 C# 或 TypeScript 產生 API 用戶端程式碼的 Visual Studio 已連線服務。</span><span class="sxs-lookup"><span data-stu-id="daac1-150">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="daac1-151">也會使用 NSwag 產生用於 OpenAPI 服務的 C# 控制器。</span><span class="sxs-lookup"><span data-stu-id="daac1-151">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="daac1-152">使用 NSwagStudio 來產生程式碼</span><span class="sxs-lookup"><span data-stu-id="daac1-152">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="daac1-153">依照 [NSwagStudio GitHub 存放庫](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) \(英文\) 的指示來安裝 NSwagStudio。</span><span class="sxs-lookup"><span data-stu-id="daac1-153">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="daac1-154">啟動 NSwagStudio，然後在 [Swagger Specification URL] \(Swagger 規格 URL\) 文字方塊中輸入 *swagger.json* 檔案 URL。</span><span class="sxs-lookup"><span data-stu-id="daac1-154">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="daac1-155">例如， *http://localhost:44354/swagger/v1/swagger.json* 。</span><span class="sxs-lookup"><span data-stu-id="daac1-155">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="daac1-156">按一下 [Create local Copy] \(建立本機複本\) 按鈕，以產生 Swagger 規格的 JSON 表示法。</span><span class="sxs-lookup"><span data-stu-id="daac1-156">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![建立 Swagger 規格的本機複本](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="daac1-158">在 [Outputs] \(輸出\) 區域中，按一下 [CSharp Client] \(CSharp 用戶端\) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="daac1-158">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="daac1-159">視您的專案而定，您也可以選擇 [TypeScript Client] \(TypeScript 用戶端\)或 [CSharp Web API Controller] \(CSharp Web API 控制器\)。</span><span class="sxs-lookup"><span data-stu-id="daac1-159">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="daac1-160">如果您選取 [CSharp Web API Controller] \(CSharp Web API 控制器\)，服務規格會重建服務，作為反向產生。</span><span class="sxs-lookup"><span data-stu-id="daac1-160">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="daac1-161">按一下 [Generate Outputs] \(產生輸出\)，以產生 *TodoApi.NSwag* 專案 的完整 C# 用戶端實作。</span><span class="sxs-lookup"><span data-stu-id="daac1-161">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="daac1-162">若要查看所產生的用戶端程式碼，請按一下 [CSharp Client] \(CSharp 用戶端\) 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="daac1-162">To see the generated client code, click the **CSharp Client** tab:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient;
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="daac1-163">C# 用戶端程式碼會根據 [Settings] \(設定\) 索引標籤中的選取項目來產生。修改設定以執行工作，例如重新命名預設的命名空間和產生同步方法。</span><span class="sxs-lookup"><span data-stu-id="daac1-163">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="daac1-164">將產生的 C# 程式碼複製到將取用 API 的用戶端專案中檔案。</span><span class="sxs-lookup"><span data-stu-id="daac1-164">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="daac1-165">開始取用 Web API：</span><span class="sxs-lookup"><span data-stu-id="daac1-165">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="daac1-166">自訂 API 文件</span><span class="sxs-lookup"><span data-stu-id="daac1-166">Customize API documentation</span></span>

<span data-ttu-id="daac1-167">Swagger 提供選項來記錄物件模型，以簡化 Web API 的取用作業。</span><span class="sxs-lookup"><span data-stu-id="daac1-167">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="daac1-168">API 資訊與描述</span><span class="sxs-lookup"><span data-stu-id="daac1-168">API info and description</span></span>

<span data-ttu-id="daac1-169">在 `Startup.ConfigureServices` 方法中，傳遞至 `AddSwaggerDocument` 方法的組態動作會新增作者、授權和描述等資訊：</span><span class="sxs-lookup"><span data-stu-id="daac1-169">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="daac1-170">Swagger UI 會顯示版本資訊：</span><span class="sxs-lookup"><span data-stu-id="daac1-170">The Swagger UI displays the version's information:</span></span>

![含有版本資訊的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="daac1-172">XML 註解</span><span class="sxs-lookup"><span data-stu-id="daac1-172">XML comments</span></span>

<span data-ttu-id="daac1-173">若要啟用 XML 註解，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="daac1-173">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="daac1-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="daac1-174">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="daac1-175">以滑鼠右鍵按一下 [方案總管] 中的專案，然後選取 [編輯 <專案名稱>.csproj]。</span><span class="sxs-lookup"><span data-stu-id="daac1-175">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="daac1-176">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="daac1-176">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="daac1-177">以滑鼠右鍵按一下方案總管中的專案，然後選取 [屬性]</span><span class="sxs-lookup"><span data-stu-id="daac1-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="daac1-178">核取 [組建] 索引標籤的 [輸出] 區段下方的 [XML 文件檔] 方塊</span><span class="sxs-lookup"><span data-stu-id="daac1-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="daac1-179">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="daac1-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="daac1-180">從 [Solution Pad] 中，按下 [控制項]，然後按一下專案名稱。</span><span class="sxs-lookup"><span data-stu-id="daac1-180">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="daac1-181">巡覽至 [工具] > [編輯檔案]。</span><span class="sxs-lookup"><span data-stu-id="daac1-181">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="daac1-182">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="daac1-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="daac1-183">開啟 [專案選項] 對話方塊 > [組建] >[編譯器]</span><span class="sxs-lookup"><span data-stu-id="daac1-183">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="daac1-184">核取 [一般選項] 區段下方的 [產生 XML 文件] 方塊</span><span class="sxs-lookup"><span data-stu-id="daac1-184">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="daac1-185">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="daac1-185">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="daac1-186">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="daac1-186">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="daac1-187">資料註解</span><span class="sxs-lookup"><span data-stu-id="daac1-187">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="daac1-188">由於 NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而針對 Web API 動作建議使用的傳回型別是 [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult)，因此它無法推斷您動作的執行內容和傳回內容。</span><span class="sxs-lookup"><span data-stu-id="daac1-188">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="daac1-189">參考下列範例：</span><span class="sxs-lookup"><span data-stu-id="daac1-189">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="daac1-190">上述動作會傳回 `IActionResult`，但在動作內部則會傳回 [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) 或 [BadRequest](xref:System.Web.Http.ApiController.BadRequest*)。</span><span class="sxs-lookup"><span data-stu-id="daac1-190">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="daac1-191">請使用資料註解來告知用戶端已知此動作要傳回哪些 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="daac1-191">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="daac1-192">使用下列屬性裝飾動作：</span><span class="sxs-lookup"><span data-stu-id="daac1-192">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="daac1-193">由於 NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而針對 Web API 動作建議使用的傳回型別是 [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601)，因此它只能推斷 `T` 所定義的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="daac1-193">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="daac1-194">您無法自動推斷其他可能的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="daac1-194">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="daac1-195">參考下列範例：</span><span class="sxs-lookup"><span data-stu-id="daac1-195">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="daac1-196">上述動作會傳回 `ActionResult<T>`。</span><span class="sxs-lookup"><span data-stu-id="daac1-196">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="daac1-197">在動作內部則會傳回 [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*)。</span><span class="sxs-lookup"><span data-stu-id="daac1-197">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="daac1-198">由於控制器是以 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性裝飾，因此也可能傳回 [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) 回應。</span><span class="sxs-lookup"><span data-stu-id="daac1-198">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="daac1-199">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="daac1-199">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="daac1-200">請使用資料註解來告知用戶端已知此動作要傳回哪些 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="daac1-200">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="daac1-201">使用下列屬性裝飾動作：</span><span class="sxs-lookup"><span data-stu-id="daac1-201">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="daac1-202">在 ASP.NET Core 2.2 或更新版本中，您可以使用慣例，而不使用 `[ProducesResponseType]` 來明確地裝飾個別動作。</span><span class="sxs-lookup"><span data-stu-id="daac1-202">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="daac1-203">如需詳細資訊，請參閱<xref:web-api/advanced/conventions>。</span><span class="sxs-lookup"><span data-stu-id="daac1-203">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="daac1-204">Swagger 產生器現在可以正確描述此動作，而產生的用戶端會知道呼叫端點時它們所接收的內容。</span><span class="sxs-lookup"><span data-stu-id="daac1-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="daac1-205">建議做法是，使用這些屬性來裝飾所有動作。</span><span class="sxs-lookup"><span data-stu-id="daac1-205">As a recommendation, decorate all actions with these attributes.</span></span>

<span data-ttu-id="daac1-206">如需 API 動作應該傳回哪些 HTTP 回應的指導方針，請參閱 [RFC 7231 規格](https://tools.ietf.org/html/rfc7231#section-4.3)。</span><span class="sxs-lookup"><span data-stu-id="daac1-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
