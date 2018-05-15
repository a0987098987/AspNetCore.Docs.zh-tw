---
title: NSwag 與 ASP.NET Core 使用者入門
author: zuckerthoben
description: 了解如何使用 NSwag 來產生 ASP.NET Core Web API 應用程式的文件和說明頁面。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="ce149-103">NSwag 與 ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="ce149-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="ce149-104">作者：[Christoph Nienaber](https://twitter.com/zuckerthoben) 和 [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="ce149-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="ce149-105">搭配使用 [NSwag](https://github.com/RSuter/NSwag) 與 ASP.NET Core 中介軟體需要有 [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="ce149-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="ce149-106">該套件包含 Swagger 產生器、Swagger UI (第 2 版和第 3 版)，以及 [ReDoc UI](https://github.com/Rebilly/ReDoc)。</span><span class="sxs-lookup"><span data-stu-id="ce149-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="ce149-107">強烈建議您利用 NSwag 的程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="ce149-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="ce149-108">選擇下列其中一個選項來產生程式碼：</span><span class="sxs-lookup"><span data-stu-id="ce149-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="ce149-109">使用 [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) 這個 Windows 桌面應用程式，以利用 C# 和 TypeScript 為您的 API 產生用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="ce149-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="ce149-110">使用 [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) 或 [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet 套件，以在您的專案內執行程式碼產生作業</span><span class="sxs-lookup"><span data-stu-id="ce149-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="ce149-111">從[命令列](https://github.com/NSwag/NSwag/wiki/CommandLine)使用 NSwag</span><span class="sxs-lookup"><span data-stu-id="ce149-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="ce149-112">使用 [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="ce149-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="ce149-113">功能</span><span class="sxs-lookup"><span data-stu-id="ce149-113">Features</span></span>

<span data-ttu-id="ce149-114">要使用 NSwag 的主要原因是不僅能夠引進 Swagger UI 和 Swagger 產生器，還能夠利用彈性的程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="ce149-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="ce149-115">您不需要現有的 API&mdash;您可以使用包含 Swagger 的協力廠商 API ，並讓 NSwag 產生用戶端實作。</span><span class="sxs-lookup"><span data-stu-id="ce149-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="ce149-116">無論哪一種方式都會加速開發週期，因此您可以更輕鬆地隨著 API 變更進行調整。</span><span class="sxs-lookup"><span data-stu-id="ce149-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="ce149-117">套件安裝</span><span class="sxs-lookup"><span data-stu-id="ce149-117">Package installation</span></span>

<span data-ttu-id="ce149-118">可使用下列方法新增 NSwag NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="ce149-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ce149-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce149-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ce149-120">從 [套件管理員主控台] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="ce149-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="ce149-121">從 [管理 NuGet 套件] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="ce149-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="ce149-122">在方案總管 > [管理 NuGet 套件] 中，以滑鼠右鍵按一下您的專案</span><span class="sxs-lookup"><span data-stu-id="ce149-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="ce149-123">將 [套件來源] 設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="ce149-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="ce149-124">在搜尋方塊中輸入 "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="ce149-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="ce149-125">從 [瀏覽] 索引標籤中選取 "NSwag.AspNetCore" 套件，並按一下 [安裝]</span><span class="sxs-lookup"><span data-stu-id="ce149-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ce149-126">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ce149-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ce149-127">在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="ce149-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="ce149-128">將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="ce149-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="ce149-129">在搜尋方塊中輸入 NSwag.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="ce149-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="ce149-130">從結果窗格中選取 NSwag.AspNetCore 套件，並按一下 [新增套件]</span><span class="sxs-lookup"><span data-stu-id="ce149-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ce149-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce149-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ce149-132">從 [整合式終端機] 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ce149-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ce149-133">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ce149-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ce149-134">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ce149-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="ce149-135">新增和設定 Swagger 中介軟體</span><span class="sxs-lookup"><span data-stu-id="ce149-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="ce149-136">在 `Info` 類別中匯入下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="ce149-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="ce149-137">在 `Startup.Configure` 方法中，啟用中介軟體為產生的 Swagger 規格和 SwaggerUI 提供服務：</span><span class="sxs-lookup"><span data-stu-id="ce149-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="ce149-138">啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="ce149-138">Launch the app.</span></span> <span data-ttu-id="ce149-139">巡覽至 `/swagger` 以檢視 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="ce149-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="ce149-140">巡覽至 `/swagger/v1/swagger.json` 以檢視 Swagger 規格。</span><span class="sxs-lookup"><span data-stu-id="ce149-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="ce149-141">程式碼產生</span><span class="sxs-lookup"><span data-stu-id="ce149-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="ce149-142">透過 NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="ce149-142">Via NSwagStudio</span></span>

* <span data-ttu-id="ce149-143">從正式 [GitHub 存放庫](https://github.com/RSuter/NSwag/wiki/NSwagStudio)安裝 `NSwagStudio`。</span><span class="sxs-lookup"><span data-stu-id="ce149-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="ce149-144">啟動 NSwagStudio。</span><span class="sxs-lookup"><span data-stu-id="ce149-144">Launch NSwagStudio.</span></span> <span data-ttu-id="ce149-145">輸入 *swagger.json* 的位置或直接複製它：</span><span class="sxs-lookup"><span data-stu-id="ce149-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="ce149-147">指出所需的用戶端輸出類型。</span><span class="sxs-lookup"><span data-stu-id="ce149-147">Indicate the desired client output type.</span></span> <span data-ttu-id="ce149-148">選項包括 [TypeScript 用戶端]、[CSharp 用戶端] 或 [CSharp Web API 控制器]。</span><span class="sxs-lookup"><span data-stu-id="ce149-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="ce149-149">使用 Web API 控制器基本上是反向產生作業。</span><span class="sxs-lookup"><span data-stu-id="ce149-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="ce149-150">它會使用服務的規格來重建服務。</span><span class="sxs-lookup"><span data-stu-id="ce149-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="ce149-151">按一下 [產生輸出]。</span><span class="sxs-lookup"><span data-stu-id="ce149-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="ce149-152">您可以在這裡看到使用 C# 中 *TodoApi.NSwag* 範例的完整用戶端實作：</span><span class="sxs-lookup"><span data-stu-id="ce149-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="ce149-154">將檔案放入用戶端專案 (例如，[Xamarin.Forms](/xamarin/xamarin-forms/) 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="ce149-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="ce149-155">開始取用 API：</span><span class="sxs-lookup"><span data-stu-id="ce149-155">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="ce149-156">您可以將基礎 URL 和/或 HTTP 用戶端插入至 API 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ce149-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="ce149-157">最佳做法是一律[重複使用 HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)。</span><span class="sxs-lookup"><span data-stu-id="ce149-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="ce149-158">您現在可以開始輕鬆地將 API 實作到用戶端專案中。</span><span class="sxs-lookup"><span data-stu-id="ce149-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="ce149-159">產生用戶端程式碼的其他方式</span><span class="sxs-lookup"><span data-stu-id="ce149-159">Other ways to generate client code</span></span>

<span data-ttu-id="ce149-160">您可以使用更適合工作流程的其他方式來產生程式碼：</span><span class="sxs-lookup"><span data-stu-id="ce149-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="ce149-161"> MSBuild</span><span class="sxs-lookup"><span data-stu-id="ce149-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="ce149-162">在程式碼中</span><span class="sxs-lookup"><span data-stu-id="ce149-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="ce149-163">T4 範本</span><span class="sxs-lookup"><span data-stu-id="ce149-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="ce149-164">自訂</span><span class="sxs-lookup"><span data-stu-id="ce149-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="ce149-165">XML 註解</span><span class="sxs-lookup"><span data-stu-id="ce149-165">XML comments</span></span>

<span data-ttu-id="ce149-166">XML 註解可使用下列方式啟用：</span><span class="sxs-lookup"><span data-stu-id="ce149-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="ce149-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce149-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="ce149-168">以滑鼠右鍵按一下方案總管中的專案，然後選取 [屬性]</span><span class="sxs-lookup"><span data-stu-id="ce149-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="ce149-169">檢查 [組建] 索引標籤的 [輸出] 區段下方的 [XML 文件檔]：</span><span class="sxs-lookup"><span data-stu-id="ce149-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![專案屬性的 [組建] 索引標籤](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="ce149-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ce149-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="ce149-172">開啟 [專案選項] 對話方塊 > [組建] > [編譯器]</span><span class="sxs-lookup"><span data-stu-id="ce149-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="ce149-173">檢查 [一般選項] 區段下方的 [產生 XML 文件] 方塊：</span><span class="sxs-lookup"><span data-stu-id="ce149-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![專案選項的 [一般選項] 區段](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="ce149-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce149-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="ce149-176">將下列程式碼片段手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="ce149-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="ce149-177">資料註解</span><span class="sxs-lookup"><span data-stu-id="ce149-177">Data annotations</span></span>

<span data-ttu-id="ce149-178">NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而 Web API 動作的最佳做法是傳回 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult)。</span><span class="sxs-lookup"><span data-stu-id="ce149-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="ce149-179">因此，NSwag 無法推斷您的動作所執行的作業，以及它所傳回的內容。</span><span class="sxs-lookup"><span data-stu-id="ce149-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="ce149-180">參考下列範例：</span><span class="sxs-lookup"><span data-stu-id="ce149-180">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="ce149-181">上述動作會傳回 `IActionResult`，但在動作內部則會傳回 [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) 或 [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest)。</span><span class="sxs-lookup"><span data-stu-id="ce149-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="ce149-182">資料註解是用來告知用戶端此動作所傳回的 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="ce149-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="ce149-183">使用下列屬性裝飾動作：</span><span class="sxs-lookup"><span data-stu-id="ce149-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="ce149-184">Swagger 產生器現在可以正確描述此動作，而產生的用戶端會知道呼叫端點時它們所接收的內容。</span><span class="sxs-lookup"><span data-stu-id="ce149-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="ce149-185">強烈建議使用這些屬性裝飾所有動作。</span><span class="sxs-lookup"><span data-stu-id="ce149-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="ce149-186">如需 API 動作應該傳回哪些 HTTP 回應的指導方針，請參閱 [RFC 7231 規格](https://tools.ietf.org/html/rfc7231#section-4.3)。</span><span class="sxs-lookup"><span data-stu-id="ce149-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
