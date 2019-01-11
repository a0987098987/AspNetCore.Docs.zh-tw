---
title: NSwag 與 ASP.NET Core 使用者入門
author: zuckerthoben
description: 了解如何使用 NSwag 來產生 ASP.NET Core Web API 的文件和說明頁面。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/18/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 8af5bed1e042c4f6d83043b05084c51b3064a548
ms.sourcegitcommit: ea215df889e89db44037a6ac2f01baede0450da9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53595356"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="f48e3-103">NSwag 與 ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="f48e3-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="f48e3-104">作者：[Christoph Nienaber](https://twitter.com/zuckerthoben) 和 [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="f48e3-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f48e3-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f48e3-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f48e3-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f48e3-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="f48e3-107">將 NSwag 中介軟體註冊於：</span><span class="sxs-lookup"><span data-stu-id="f48e3-107">Register the NSwag middlewares to:</span></span>

* <span data-ttu-id="f48e3-108">為實作的 Web API 產生 Swagger 規格。</span><span class="sxs-lookup"><span data-stu-id="f48e3-108">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="f48e3-109">提供 Swagger UI 以瀏覽並測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="f48e3-109">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="f48e3-110">若要使用 [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core 中介軟體，請安裝 [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f48e3-110">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="f48e3-111">此套件包含中介軟體，以產生並提供 Swagger 規格、Swagger UI (v2 和 v3) 和 [ReDoc UI](https://github.com/Rebilly/ReDoc)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-111">This package contains the middlewares to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="f48e3-112">此外，強烈建議您利用 NSwag 的程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="f48e3-112">Additionally, it's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="f48e3-113">選擇下列其中一個選項來使用程式碼產生功能：</span><span class="sxs-lookup"><span data-stu-id="f48e3-113">Choose one of the following options to use the code generation capabilities:</span></span>

* <span data-ttu-id="f48e3-114">使用 [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) 這個 Windows 桌面應用程式，以利用 C# 和 TypeScript 為您的 API 產生用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="f48e3-114">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="f48e3-115">使用 [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) 或 [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet 套件，以在您的專案內執行程式碼產生作業。</span><span class="sxs-lookup"><span data-stu-id="f48e3-115">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="f48e3-116">從[命令列](https://github.com/NSwag/NSwag/wiki/CommandLine)使用 NSwag。</span><span class="sxs-lookup"><span data-stu-id="f48e3-116">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="f48e3-117">使用 [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f48e3-117">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="f48e3-118">功能</span><span class="sxs-lookup"><span data-stu-id="f48e3-118">Features</span></span>

<span data-ttu-id="f48e3-119">使用 NSwag 的主要原因是不僅能夠引進 Swagger UI 和 Swagger 產生器，還能夠利用彈性的程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="f48e3-119">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to also make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="f48e3-120">您不需要現有的 API&mdash;您可以使用包含 Swagger 的協力廠商 API ，並讓 NSwag 產生用戶端實作。</span><span class="sxs-lookup"><span data-stu-id="f48e3-120">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="f48e3-121">無論哪一種方式都會加速開發週期，因此您可以更輕鬆地隨著 API 變更進行調整。</span><span class="sxs-lookup"><span data-stu-id="f48e3-121">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="f48e3-122">套件安裝</span><span class="sxs-lookup"><span data-stu-id="f48e3-122">Package installation</span></span>

<span data-ttu-id="f48e3-123">可使用下列方法新增 NSwag NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="f48e3-123">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f48e3-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f48e3-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f48e3-125">從 [套件管理員主控台] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="f48e3-125">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="f48e3-126">移至 [檢視] > [其他視窗] > [套件管理員主控台]</span><span class="sxs-lookup"><span data-stu-id="f48e3-126">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="f48e3-127">巡覽至 *TodoApi.csproj* 檔案所在目錄</span><span class="sxs-lookup"><span data-stu-id="f48e3-127">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="f48e3-128">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f48e3-128">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="f48e3-129">從 [管理 NuGet 套件] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f48e3-129">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="f48e3-130">在 [方案總管] > [管理 NuGet 套件] 中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="f48e3-130">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="f48e3-131">將 [套件來源] 設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="f48e3-131">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="f48e3-132">在搜尋方塊中輸入 "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="f48e3-132">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="f48e3-133">從 [瀏覽] 索引標籤中選取 "NSwag.AspNetCore" 套件，並按一下 [安裝]</span><span class="sxs-lookup"><span data-stu-id="f48e3-133">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f48e3-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f48e3-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f48e3-135">在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="f48e3-135">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="f48e3-136">將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="f48e3-136">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="f48e3-137">在搜尋方塊中輸入 "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="f48e3-137">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="f48e3-138">從結果窗格中選取 "NSwag.AspNetCore" 套件，並按一下 [新增套件]</span><span class="sxs-lookup"><span data-stu-id="f48e3-138">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f48e3-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f48e3-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f48e3-140">從 [整合式終端機] 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f48e3-140">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f48e3-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f48e3-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f48e3-142">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f48e3-142">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="f48e3-143">新增和設定 Swagger 中介軟體</span><span class="sxs-lookup"><span data-stu-id="f48e3-143">Add and configure Swagger middleware</span></span>

<span data-ttu-id="f48e3-144">在 `Startup` 類別中匯入下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="f48e3-144">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="f48e3-145">在 `Startup.ConfigureServices` 方法中，註冊所需的 Swagger 服務：</span><span class="sxs-lookup"><span data-stu-id="f48e3-145">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span> 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="f48e3-146">在 `Startup.Configure` 方法中，啟用中介軟體來為產生的 Swagger 規格和 SwaggerUI v3 提供服務：</span><span class="sxs-lookup"><span data-stu-id="f48e3-146">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI v3:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="f48e3-147">啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f48e3-147">Launch the app.</span></span> <span data-ttu-id="f48e3-148">巡覽至 `http://localhost:<port>/swagger` 以檢視 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="f48e3-148">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="f48e3-149">巡覽至 `http://localhost:<port>/swagger/v1/swagger.json` 以檢視 Swagger 規格。</span><span class="sxs-lookup"><span data-stu-id="f48e3-149">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="f48e3-150">程式碼產生</span><span class="sxs-lookup"><span data-stu-id="f48e3-150">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="f48e3-151">透過 NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="f48e3-151">Via NSwagStudio</span></span>

* <span data-ttu-id="f48e3-152">從正式 [GitHub 存放庫](https://github.com/RSuter/NSwag/wiki/NSwagStudio)安裝 NSwagStudio。</span><span class="sxs-lookup"><span data-stu-id="f48e3-152">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="f48e3-153">啟動 NSwagStudio。</span><span class="sxs-lookup"><span data-stu-id="f48e3-153">Launch NSwagStudio.</span></span> <span data-ttu-id="f48e3-154">在 [Swagger 規格 URL] 文字方塊中輸入 檔案 URL，並按一下 [建立本機複本] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f48e3-154">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="f48e3-155">選取 [CSharp 用戶端] 用戶端輸出類型。</span><span class="sxs-lookup"><span data-stu-id="f48e3-155">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="f48e3-156">選項包括 [TypeScript 用戶端] 和 [CSharp Web API 控制器]。</span><span class="sxs-lookup"><span data-stu-id="f48e3-156">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="f48e3-157">使用 Web API 控制器基本上是反向產生作業。</span><span class="sxs-lookup"><span data-stu-id="f48e3-157">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="f48e3-158">它會使用服務的規格來重建服務。</span><span class="sxs-lookup"><span data-stu-id="f48e3-158">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="f48e3-159">按一下 [產生輸出] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f48e3-159">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="f48e3-160">隨即會產生 *TodoApi.NSwag* 專案的 C# 用戶端實作。</span><span class="sxs-lookup"><span data-stu-id="f48e3-160">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="f48e3-161">按一下 [CSharp 用戶端] 索引標籤的 [輸出] 區段，就可查看產生的用戶端程式碼：</span><span class="sxs-lookup"><span data-stu-id="f48e3-161">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
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
> <span data-ttu-id="f48e3-162">C# 用戶端程式碼會根據 [CSharp 用戶端] 索引標籤之 [設定] 索引標籤中定義的設定來產生。修改設定以執行工作，例如重新命名預設的命名空間和產生同步方法。</span><span class="sxs-lookup"><span data-stu-id="f48e3-162">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="f48e3-163">將產生的 C# 程式碼複製到用戶端專案中的檔案 (例如，[Xamarin.Forms](/xamarin/xamarin-forms/) 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-163">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="f48e3-164">開始取用 Web API：</span><span class="sxs-lookup"><span data-stu-id="f48e3-164">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="f48e3-165">您可以將基礎 URL 和/或 HTTP 用戶端插入至 API 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f48e3-165">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="f48e3-166">最佳做法是一律[重複使用 HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-166">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="f48e3-167">產生用戶端程式碼的其他方式</span><span class="sxs-lookup"><span data-stu-id="f48e3-167">Other ways to generate client code</span></span>

<span data-ttu-id="f48e3-168">您可以使用更適合工作流程的其他方式來產生用戶端程式碼：</span><span class="sxs-lookup"><span data-stu-id="f48e3-168">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="f48e3-169">MSBuild</span><span class="sxs-lookup"><span data-stu-id="f48e3-169">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="f48e3-170">在程式碼中</span><span class="sxs-lookup"><span data-stu-id="f48e3-170">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="f48e3-171">T4 範本</span><span class="sxs-lookup"><span data-stu-id="f48e3-171">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="f48e3-172">自訂</span><span class="sxs-lookup"><span data-stu-id="f48e3-172">Customize</span></span>

<span data-ttu-id="f48e3-173">Swagger 提供選項來記錄物件模型，以簡化 Web API 的取用作業。</span><span class="sxs-lookup"><span data-stu-id="f48e3-173">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="f48e3-174">API 資訊與描述</span><span class="sxs-lookup"><span data-stu-id="f48e3-174">API info and description</span></span>

<span data-ttu-id="f48e3-175">在 `Startup.Configure` 方法中，傳遞至 `UseSwagger` 方法的組態動作會新增作者、授權和描述等資訊：</span><span class="sxs-lookup"><span data-stu-id="f48e3-175">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="f48e3-176">Swagger UI 會顯示版本資訊：</span><span class="sxs-lookup"><span data-stu-id="f48e3-176">The Swagger UI displays the version's information:</span></span>

![含有版本資訊的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="f48e3-178">XML 註解</span><span class="sxs-lookup"><span data-stu-id="f48e3-178">XML comments</span></span>

<span data-ttu-id="f48e3-179">XML 註解可使用下列方式啟用：</span><span class="sxs-lookup"><span data-stu-id="f48e3-179">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="f48e3-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f48e3-180">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="f48e3-181">以滑鼠右鍵按一下 [方案總管] 中的專案，然後選取 [編輯 <專案名稱>.csproj]。</span><span class="sxs-lookup"><span data-stu-id="f48e3-181">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="f48e3-182">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f48e3-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="f48e3-183">以滑鼠右鍵按一下方案總管中的專案，然後選取 [屬性]</span><span class="sxs-lookup"><span data-stu-id="f48e3-183">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="f48e3-184">核取 [組建] 索引標籤的 [輸出] 區段下方的 [XML 文件檔] 方塊</span><span class="sxs-lookup"><span data-stu-id="f48e3-184">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="f48e3-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f48e3-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="f48e3-186">從 [Solution Pad] 中，按下 [控制項]，然後按一下專案名稱。</span><span class="sxs-lookup"><span data-stu-id="f48e3-186">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="f48e3-187">巡覽至 [工具] > [編輯檔案]。</span><span class="sxs-lookup"><span data-stu-id="f48e3-187">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="f48e3-188">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f48e3-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="f48e3-189">開啟 [專案選項] 對話方塊 > [組建] >[編譯器]</span><span class="sxs-lookup"><span data-stu-id="f48e3-189">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="f48e3-190">核取 [一般選項] 區段下方的 [產生 XML 文件] 方塊</span><span class="sxs-lookup"><span data-stu-id="f48e3-190">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="f48e3-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f48e3-191">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="f48e3-192">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f48e3-192">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="f48e3-193">資料註解</span><span class="sxs-lookup"><span data-stu-id="f48e3-193">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f48e3-194">NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而 Web API 動作的建議傳回型別是 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-194">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="f48e3-195">因此，NSwag 無法推斷您的動作所執行的作業，以及它所傳回的內容。</span><span class="sxs-lookup"><span data-stu-id="f48e3-195">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="f48e3-196">參考下列範例：</span><span class="sxs-lookup"><span data-stu-id="f48e3-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="f48e3-197">上述動作會傳回 `IActionResult`，但在動作內部則會傳回 [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) 或 [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-197">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="f48e3-198">資料註解是用來通知用戶端已知此動作要傳回的 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f48e3-198">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="f48e3-199">使用下列屬性裝飾動作：</span><span class="sxs-lookup"><span data-stu-id="f48e3-199">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f48e3-200">NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而 Web API 動作的建議傳回型別是 [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-200">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="f48e3-201">目前，NSwag 只能推斷 `T` 所定義的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="f48e3-201">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="f48e3-202">無法推斷動作中其他可能的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="f48e3-202">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="f48e3-203">參考下列範例：</span><span class="sxs-lookup"><span data-stu-id="f48e3-203">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="f48e3-204">上述動作會傳回 `ActionResult<T>`。</span><span class="sxs-lookup"><span data-stu-id="f48e3-204">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="f48e3-205">在動作內部則會傳回 [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-205">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="f48e3-206">由於控制器是以 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性裝飾，因此也可能傳回 [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) 回應。</span><span class="sxs-lookup"><span data-stu-id="f48e3-206">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="f48e3-207">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-207">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="f48e3-208">資料註解是用來通知用戶端已知此動作要傳回的 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f48e3-208">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="f48e3-209">使用下列屬性裝飾動作：</span><span class="sxs-lookup"><span data-stu-id="f48e3-209">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="f48e3-210">在 ASP.NET Core 2.2 或更新版本中，慣例可作為使用 `[ProducesResponseType]` 明確裝飾個別動作的替代方案。</span><span class="sxs-lookup"><span data-stu-id="f48e3-210">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="f48e3-211">如需詳細資訊，請參閱<xref:web-api/advanced/conventions>。</span><span class="sxs-lookup"><span data-stu-id="f48e3-211">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="f48e3-212">Swagger 產生器現在可以正確描述此動作，而產生的用戶端會知道呼叫端點時它們所接收的內容。</span><span class="sxs-lookup"><span data-stu-id="f48e3-212">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="f48e3-213">強烈建議使用這些屬性裝飾所有動作。</span><span class="sxs-lookup"><span data-stu-id="f48e3-213">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="f48e3-214">如需 API 動作應該傳回哪些 HTTP 回應的指導方針，請參閱 [RFC 7231 規格](https://tools.ietf.org/html/rfc7231#section-4.3)。</span><span class="sxs-lookup"><span data-stu-id="f48e3-214">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
