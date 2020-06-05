---
title: NSwag 與 ASP.NET Core 使用者入門
author: zuckerthoben
description: 了解如何使用 NSwag 來產生 ASP.NET Core Web API 的文件和說明頁面。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: cd4cc6778de7d2156243dc91fba64b2bdb79cf08
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84452118"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="0ab89-103">NSwag 與 ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="0ab89-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="0ab89-104">作者：[Christoph Nienaber](https://twitter.com/zuckerthoben)、[Rico Suter](https://rsuter.com) 及 [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="0ab89-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0ab89-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="0ab89-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="0ab89-106">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="0ab89-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="0ab89-107">NSwag 提供下列功能：</span><span class="sxs-lookup"><span data-stu-id="0ab89-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="0ab89-108">能夠運用 Swagger UI 和 Swagger 產生器。</span><span class="sxs-lookup"><span data-stu-id="0ab89-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="0ab89-109">彈性的程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="0ab89-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="0ab89-110">使用 NSwag 時，您不需要有現有的 API &mdash; 您可以使用包含 Swagger 的協力廠商 API，然後產生用戶端實作。</span><span class="sxs-lookup"><span data-stu-id="0ab89-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="0ab89-111">NSwag 可讓您加速開發週期，並輕鬆地因應 API 變更進行調整。</span><span class="sxs-lookup"><span data-stu-id="0ab89-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="0ab89-112">註冊 NSwag 中介軟體</span><span class="sxs-lookup"><span data-stu-id="0ab89-112">Register the NSwag middleware</span></span>

<span data-ttu-id="0ab89-113">註冊 NSwag 中介軟體來：</span><span class="sxs-lookup"><span data-stu-id="0ab89-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="0ab89-114">為實作的 Web API 產生 Swagger 規格。</span><span class="sxs-lookup"><span data-stu-id="0ab89-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="0ab89-115">提供 Swagger UI 以瀏覽並測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="0ab89-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="0ab89-116">若要使用 [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core 中介軟體，請安裝 [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0ab89-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="0ab89-117">此套件包含用以產生並提供 Swagger 規格、Swagger UI (v2 和 v3) 及 [ReDoc UI](https://github.com/Rebilly/ReDoc) 的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0ab89-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="0ab89-118">請使用下列其中一種方法來安裝 NSwag NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="0ab89-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ab89-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ab89-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ab89-120">從 [套件管理員主控台]\*\*\*\* 視窗中：</span><span class="sxs-lookup"><span data-stu-id="0ab89-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="0ab89-121">移至 [**查看**  >  **其他 Windows**  >  **套件管理員主控台**]</span><span class="sxs-lookup"><span data-stu-id="0ab89-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="0ab89-122">巡覽至 *TodoApi.csproj* 檔案所在目錄</span><span class="sxs-lookup"><span data-stu-id="0ab89-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="0ab89-123">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0ab89-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="0ab89-124">從 [管理 NuGet 套件]\*\*\*\* 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="0ab89-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="0ab89-125">以滑鼠右鍵按一下**方案總管**  >  **管理 NuGet 套件**] 中的專案</span><span class="sxs-lookup"><span data-stu-id="0ab89-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="0ab89-126">將 [套件來源]\*\*\*\* 設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="0ab89-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="0ab89-127">在搜尋方塊中輸入 "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="0ab89-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="0ab89-128">從 [瀏覽]\*\*\*\* 索引標籤中選取 "NSwag.AspNetCore" 套件，並按一下 [安裝]\*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="0ab89-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ab89-129">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ab89-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0ab89-130">在 [Solution Pad]\*\*\*\* > [新增套件...]\*\*\*\* 中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="0ab89-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="0ab89-131">將 [新增套件]\*\*\*\* 視窗的 [來源]\*\*\*\* 下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="0ab89-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="0ab89-132">在搜尋方塊中輸入 "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="0ab89-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="0ab89-133">從結果窗格中選取 "NSwag. AspNetCore" 套件，然後按一下 [**新增套件**]</span><span class="sxs-lookup"><span data-stu-id="0ab89-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="0ab89-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0ab89-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0ab89-135">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="0ab89-135">Run the following command:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="0ab89-136">新增和設定 Swagger 中介軟體</span><span class="sxs-lookup"><span data-stu-id="0ab89-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="0ab89-137">請執行下列步驟，以在您的 ASP.NET Core 應用程式中新增及設定 Swagger：</span><span class="sxs-lookup"><span data-stu-id="0ab89-137">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="0ab89-138">在 `Startup.ConfigureServices` 方法中，註冊所需的 Swagger 服務：</span><span class="sxs-lookup"><span data-stu-id="0ab89-138">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="0ab89-139">在 `Startup.Configure` 方法中，啟用中介軟體為產生的 Swagger 規格和 SwaggerUI 提供服務：</span><span class="sxs-lookup"><span data-stu-id="0ab89-139">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="0ab89-140">啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ab89-140">Launch the app.</span></span> <span data-ttu-id="0ab89-141">瀏覽至：</span><span class="sxs-lookup"><span data-stu-id="0ab89-141">Navigate to:</span></span>
  * <span data-ttu-id="0ab89-142">`http://localhost:<port>/swagger` 以檢視 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="0ab89-142">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="0ab89-143">`http://localhost:<port>/swagger/v1/swagger.json` 以檢視 Swagger 規格。</span><span class="sxs-lookup"><span data-stu-id="0ab89-143">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="0ab89-144">程式碼產生</span><span class="sxs-lookup"><span data-stu-id="0ab89-144">Code generation</span></span>

<span data-ttu-id="0ab89-145">您可以選擇下列其中一個選項來利用 NSwag 的程式碼產生功能：</span><span class="sxs-lookup"><span data-stu-id="0ab89-145">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="0ab89-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio)：用來以 c # 或 TYPESCRIPT 產生 API 用戶端程式代碼的 Windows 桌面應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ab89-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio): A Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="0ab89-147">可在您專案內產生程式碼的 [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) 或 [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0ab89-147">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="0ab89-148">從[命令列](https://github.com/RicoSuter/NSwag/wiki/CommandLine)使用 NSwag。</span><span class="sxs-lookup"><span data-stu-id="0ab89-148">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="0ab89-149">[NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0ab89-149">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) NuGet package.</span></span>
* <span data-ttu-id="0ab89-150">[Unchase OpenAPI （Swagger）聯機服務](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice)： Visual Studio 聯機服務，以 c # 或 TYPESCRIPT 產生 API 用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="0ab89-150">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice): A Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="0ab89-151">也會使用 NSwag 產生用於 OpenAPI 服務的 C# 控制器。</span><span class="sxs-lookup"><span data-stu-id="0ab89-151">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="0ab89-152">使用 NSwagStudio 來產生程式碼</span><span class="sxs-lookup"><span data-stu-id="0ab89-152">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="0ab89-153">依照 [NSwagStudio GitHub 存放庫](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) \(英文\) 的指示來安裝 NSwagStudio。</span><span class="sxs-lookup"><span data-stu-id="0ab89-153">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span> <span data-ttu-id="0ab89-154">在 [NSwag 版本] 頁面上，您可以下載 xcopy 版本，而不需安裝和系統管理員許可權即可啟動。</span><span class="sxs-lookup"><span data-stu-id="0ab89-154">On the NSwag release page you can download an xcopy version which can be started without installation and admin privileges.</span></span>
* <span data-ttu-id="0ab89-155">啟動 NSwagStudio，然後在 [Swagger Specification URL] \(Swagger 規格 URL\)\*\*\*\* 文字方塊中輸入 *swagger.json* 檔案 URL。</span><span class="sxs-lookup"><span data-stu-id="0ab89-155">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="0ab89-156">例如： *http://localhost:44354/swagger/v1/swagger.json* 。</span><span class="sxs-lookup"><span data-stu-id="0ab89-156">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="0ab89-157">按一下 [Create local Copy] \(建立本機複本\)\*\*\*\* 按鈕，以產生 Swagger 規格的 JSON 表示法。</span><span class="sxs-lookup"><span data-stu-id="0ab89-157">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![建立 Swagger 規格的本機複本](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="0ab89-159">在 [Outputs] \(輸出\)\*\*\*\* 區域中，按一下 [CSharp Client] \(CSharp 用戶端\)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0ab89-159">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="0ab89-160">視您的專案而定，您也可以選擇 [TypeScript Client] \(TypeScript 用戶端\)\*\*\*\* 或 [CSharp Web API Controller] \(CSharp Web API 控制器\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ab89-160">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="0ab89-161">如果您選取 [CSharp Web API Controller] \(CSharp Web API 控制器\)\*\*\*\*，服務規格會重建服務，作為反向產生。</span><span class="sxs-lookup"><span data-stu-id="0ab89-161">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="0ab89-162">按一下 [Generate Outputs] \(產生輸出\)\*\*\*\*，以產生 *TodoApi.NSwag* 專案 的完整 C# 用戶端實作。</span><span class="sxs-lookup"><span data-stu-id="0ab89-162">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="0ab89-163">若要查看所產生的用戶端程式碼，請按一下 [CSharp Client] \(CSharp 用戶端\)\*\*\*\* 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="0ab89-163">To see the generated client code, click the **CSharp Client** tab:</span></span>

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
> <span data-ttu-id="0ab89-164">C # 用戶端程式代碼會根據 [**設定**] 索引標籤中的選取專案來產生。請修改設定以執行工作，例如預設命名空間重新命名和同步方法產生。</span><span class="sxs-lookup"><span data-stu-id="0ab89-164">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="0ab89-165">將產生的 C# 程式碼複製到將取用 API 的用戶端專案中檔案。</span><span class="sxs-lookup"><span data-stu-id="0ab89-165">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="0ab89-166">開始取用 Web API：</span><span class="sxs-lookup"><span data-stu-id="0ab89-166">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="0ab89-167">自訂 API 文件</span><span class="sxs-lookup"><span data-stu-id="0ab89-167">Customize API documentation</span></span>

<span data-ttu-id="0ab89-168">Swagger 提供選項來記錄物件模型，以簡化 Web API 的取用作業。</span><span class="sxs-lookup"><span data-stu-id="0ab89-168">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="0ab89-169">API 資訊與描述</span><span class="sxs-lookup"><span data-stu-id="0ab89-169">API info and description</span></span>

<span data-ttu-id="0ab89-170">在 `Startup.ConfigureServices` 方法中，傳遞至 `AddSwaggerDocument` 方法的組態動作會新增作者、授權和描述等資訊：</span><span class="sxs-lookup"><span data-stu-id="0ab89-170">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="0ab89-171">Swagger UI 會顯示版本資訊：</span><span class="sxs-lookup"><span data-stu-id="0ab89-171">The Swagger UI displays the version's information:</span></span>

![含有版本資訊的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="0ab89-173">XML 註解</span><span class="sxs-lookup"><span data-stu-id="0ab89-173">XML comments</span></span>

<span data-ttu-id="0ab89-174">若要啟用 XML 註解，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="0ab89-174">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ab89-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ab89-175">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="0ab89-176">以滑鼠右鍵按一下 [方案總管]\*\*\*\* 中的專案，然後選取 [編輯 <專案名稱>.csproj]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ab89-176">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="0ab89-177">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0ab89-177">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="0ab89-178">以滑鼠右鍵按一下方案總管\*\*\*\* 中的專案，然後選取 [屬性]\*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="0ab89-178">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="0ab89-179">在 [**組建**] 索引標籤的 [**輸出**] 區段底下，選取 [ **XML**檔檔案] 方塊</span><span class="sxs-lookup"><span data-stu-id="0ab89-179">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ab89-180">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ab89-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="0ab89-181">從 [Solution Pad]\*\* 中，按下 [控制項]\*\*\*\*，然後按一下專案名稱。</span><span class="sxs-lookup"><span data-stu-id="0ab89-181">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="0ab89-182">流覽至 [**工具**] [  >  **編輯**檔案]。</span><span class="sxs-lookup"><span data-stu-id="0ab89-182">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="0ab89-183">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0ab89-183">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="0ab89-184">開啟 [專案選項]\*\*\*\* 對話方塊 > [組建]**[編譯器]** > \*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="0ab89-184">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="0ab89-185">勾選 [**一般選項**] 區段底下的 [**產生 xml 檔**] 方塊</span><span class="sxs-lookup"><span data-stu-id="0ab89-185">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="net-core-cli"></a>[<span data-ttu-id="0ab89-186">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0ab89-186">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0ab89-187">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0ab89-187">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="0ab89-188">資料註解</span><span class="sxs-lookup"><span data-stu-id="0ab89-188">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="0ab89-189">由於 NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而針對 Web API 動作建議使用的傳回型別是 [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult)，因此它無法推斷您動作的執行內容和傳回內容。</span><span class="sxs-lookup"><span data-stu-id="0ab89-189">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="0ab89-190">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="0ab89-190">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="0ab89-191">上述動作會傳回 `IActionResult`，但在動作內部則會傳回 [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) 或 [BadRequest](xref:System.Web.Http.ApiController.BadRequest*)。</span><span class="sxs-lookup"><span data-stu-id="0ab89-191">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="0ab89-192">請使用資料註解來告知用戶端已知此動作要傳回哪些 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="0ab89-192">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="0ab89-193">使用下列屬性來標記動作：</span><span class="sxs-lookup"><span data-stu-id="0ab89-193">Mark the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0ab89-194">因為 NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而且 Web API 動作的建議傳回型別[是 \<T> ActionResult](xref:Microsoft.AspNetCore.Mvc.ActionResult%601)，所以它只能推斷所定義的傳回型別 `T` 。</span><span class="sxs-lookup"><span data-stu-id="0ab89-194">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="0ab89-195">您無法自動推斷其他可能的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="0ab89-195">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="0ab89-196">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="0ab89-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="0ab89-197">上述動作會傳回 `ActionResult<T>`。</span><span class="sxs-lookup"><span data-stu-id="0ab89-197">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="0ab89-198">在動作內部則會傳回 [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*)。</span><span class="sxs-lookup"><span data-stu-id="0ab89-198">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="0ab89-199">由於控制器具有 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性，因此也可能會有[BadRequest](xref:System.Web.Http.ApiController.BadRequest*)回應。</span><span class="sxs-lookup"><span data-stu-id="0ab89-199">Since the controller has the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="0ab89-200">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="0ab89-200">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="0ab89-201">請使用資料註解來告知用戶端已知此動作要傳回哪些 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="0ab89-201">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="0ab89-202">使用下列屬性來標記動作：</span><span class="sxs-lookup"><span data-stu-id="0ab89-202">Mark the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="0ab89-203">在 ASP.NET Core 2.2 或更新版本中，您可以使用慣例，而不使用 `[ProducesResponseType]` 來明確地裝飾個別動作。</span><span class="sxs-lookup"><span data-stu-id="0ab89-203">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="0ab89-204">如需詳細資訊，請參閱 <xref:web-api/advanced/conventions> 。</span><span class="sxs-lookup"><span data-stu-id="0ab89-204">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="0ab89-205">Swagger 產生器現在可以正確描述此動作，而產生的用戶端會知道呼叫端點時它們所接收的內容。</span><span class="sxs-lookup"><span data-stu-id="0ab89-205">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="0ab89-206">建議使用這些屬性來標記所有動作。</span><span class="sxs-lookup"><span data-stu-id="0ab89-206">As a recommendation, mark all actions with these attributes.</span></span>

<span data-ttu-id="0ab89-207">如需 API 動作應該傳回哪些 HTTP 回應的指導方針，請參閱 [RFC 7231 規格](https://tools.ietf.org/html/rfc7231#section-4.3)。</span><span class="sxs-lookup"><span data-stu-id="0ab89-207">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
