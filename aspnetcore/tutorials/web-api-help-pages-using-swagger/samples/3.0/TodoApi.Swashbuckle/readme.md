---
page_type: sample
description: 了解如何將 Swashbuckle 新增至 ASP.NET Core Web API 專案，以整合 Swagger UI。
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
- vs-code
- vs-mac
urlFragment: getstarted-swashbuckle-aspnetcore
ms.openlocfilehash: e02247325f430b0ce23dbb3f5bc344a60a1a164a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659933"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="1cb4b-102">Swashbuckle 與 ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="1cb4b-102">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="1cb4b-103">使用 Web API 時，了解其各種不同的方法對開對發人員來說可能是一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-103">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="1cb4b-104">[Swagger](https://swagger.io/) (也稱為 [Open API](https://www.openapis.org/)) 解決為 Web API 產生有用文件與說明頁面的問題。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-104">[Swagger](https://swagger.io/), also known as [OpenAPI](https://www.openapis.org/), solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="1cb4b-105">它提供如互動式文件、用戶端 SDK 產生作業和 API 發現性等優點。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-105">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="1cb4b-106">在此範例中，會顯示 .NET 實作 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-106">In this sample, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) the .NET implementation is shown.</span></span>

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="1cb4b-107">新增和設定 Swagger 中介軟體</span><span class="sxs-lookup"><span data-stu-id="1cb4b-107">Add and configure Swagger middleware</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });
    });
}
```

<span data-ttu-id="1cb4b-108">在 `Startup.Configure` 方法中，啟用中介軟體為產生的 JSON 文件和 Swagger UI 提供服務：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-108">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();

    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1"); 
    });

    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

<span data-ttu-id="1cb4b-109">上述 `UseSwaggerUI` 方法呼叫會啟用[靜態檔案中介軟體](https://docs.microsoft.com/aspnet/core/fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-109">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/static-files).</span></span> <span data-ttu-id="1cb4b-110">如果以 .NET Framework 或 .NET Core 1.x 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-110">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="1cb4b-111">啟動應用程式，並巡覽至 `http://localhost:<port>/swagger/v1/swagger.json`。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-111">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="1cb4b-112">描述端點的已產生文件隨即出現，如 [Swagger 規格 (swagger.json)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson) 中所示。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-112">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="1cb4b-113">您可以在 `http://localhost:<port>/swagger` 找到 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-113">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="1cb4b-114">透過 Swagger UI 探索 API，並將其併入其他程式。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-114">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="1cb4b-115">若要在應用程式的根目錄 (`http://localhost:<port>/`) 上提供 Swagger UI，請將 `RoutePrefix` 屬性設為空字串：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-115">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> ```csharp
>app.UseSwaggerUI(c =>
>{
>    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
>    c.RoutePrefix = string.Empty;
>});
>```

<span data-ttu-id="1cb4b-116">若使用目錄搭配 IIS 或 反向 Proxy，請將 Swagger 端點設定為使用 `./` 前置詞的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-116">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="1cb4b-117">例如： `./swagger/v1/swagger.json` 。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-117">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="1cb4b-118">使用 `/swagger/v1/swagger.json` 指示應用程式在 URL 的真實根目錄 (若已使用，請加上路由前置詞) 尋找 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-118">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="1cb4b-119">例如，請使用 `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json`，而不要使用 `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-119">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="1cb4b-120">自訂與擴充</span><span class="sxs-lookup"><span data-stu-id="1cb4b-120">Customize and extend</span></span>

<span data-ttu-id="1cb4b-121">Swagger 提供用來記載物件模型和自訂 UI 以符合佈景主題的選項。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-121">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

<span data-ttu-id="1cb4b-122">在 `Startup` 類別中，加入下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-122">In the `Startup` class, add the following namespaces:</span></span>
```csharp
using System;
using System.Reflection;
using System.IO;
```

### <a name="api-info-and-description"></a><span data-ttu-id="1cb4b-123">API 資訊與描述</span><span class="sxs-lookup"><span data-stu-id="1cb4b-123">API info and description</span></span>

<span data-ttu-id="1cb4b-124">傳遞至 `AddSwaggerGen` 方法的組態動作會新增作者、授權和描述等資訊：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-124">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

```csharp
// Register the Swagger generator, defining 1 or more Swagger documents
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo
    {
        Version = "v1",
        Title = "ToDo API",
        Description = "A simple example ASP.NET Core Web API",
        TermsOfService = new Uri("https://example.com/terms"),
        Contact = new OpenApiContact
        {
            Name = "Shayne Boyer",
            Email = string.Empty,
            Url = new Uri("https://twitter.com/spboyer"),
        },
        License = new OpenApiLicense
        {
            Name = "Use under LICX",
            Url = new Uri("https://example.com/license"),
        }
    });
});
```

<span data-ttu-id="1cb4b-125">Swagger UI 會顯示版本資訊：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-125">The Swagger UI displays the version's information:</span></span>

![含有版本資訊的 Swagger UI：描述、作者，以及查看更多連結](sample_images/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="1cb4b-127">XML 註解</span><span class="sxs-lookup"><span data-stu-id="1cb4b-127">XML comments</span></span>

<span data-ttu-id="1cb4b-128">XML 註解可以使用下列方式啟用：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-128">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studio"></a>[<span data-ttu-id="1cb4b-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1cb4b-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1cb4b-130">以滑鼠右鍵按一下 [方案總管] 中的專案，然後選取 [編輯 <專案名稱>.csproj]。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-130">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="1cb4b-131">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-131">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-for-mac"></a>[<span data-ttu-id="1cb4b-132">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1cb4b-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1cb4b-133">從 [Solution Pad] 中，按下 [控制項]，然後按一下專案名稱。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-133">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="1cb4b-134">巡覽至 [工具] > [編輯檔案]。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-134">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="1cb4b-135">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-135">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-code"></a>[<span data-ttu-id="1cb4b-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1cb4b-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1cb4b-137">將醒目提示的程式碼行手動新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-137">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

---

<span data-ttu-id="1cb4b-138">啟用 XML 註解，可提供未記載之公用類型與成員的偵錯資訊。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-138">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="1cb4b-139">未記載的類型和成員會以警告訊息表示。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-139">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="1cb4b-140">例如，下列訊息指出警告碼 1591 的違規：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-140">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="1cb4b-141">在專案檔中定義要忽略的警告碼清單 (以分號分隔)，即可隱藏警告。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-141">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="1cb4b-142">將警告碼附加至 `$(NoWarn);` 也會套用 [C# 預設值](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16)。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-142">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

```xml
<NoWarn>$(NoWarn);1591</NoWarn>
```

<span data-ttu-id="1cb4b-143">若只要針對特定成員隱藏，請將程式碼放在 [#pragma 警告](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning)前置處理器指示詞中。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-143">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="1cb4b-144">這個方法適用于不應透過 API 檔公開的程式碼。在下列範例中，會忽略整個 `Program` 類別的警告程式碼 CS1591。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-144">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="1cb4b-145">會在類別定義結尾處還原強制執行的警告碼。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-145">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="1cb4b-146">以逗號分隔清單指定多個警告碼。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-146">Specify multiple warning codes with a comma-delimited list.</span></span>

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

<span data-ttu-id="1cb4b-147">設定 Swagger 以使用先前指示所產生的 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-147">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="1cb4b-148">對於 Linux 或非 Windows 作業系統，檔案名稱和路徑可以區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-148">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="1cb4b-149">例如，*TodoApi.XML* 檔案在 Windows 上有效，但在 CentOS 上則無效。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-149">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

```csharp
/// NOTE LAST 3 LINES IN THIS SNIPPET
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo
        {
            Version = "v1",
            Title = "ToDo API",
            Description = "A simple example ASP.NET Core Web API",
            TermsOfService = new Uri("https://example.com/terms"),
            Contact = new OpenApiContact
            {
                Name = "Shayne Boyer",
                Email = string.Empty,
                Url = new Uri("https://twitter.com/spboyer"),
            },
            License = new OpenApiLicense
            {
                Name = "Use under LICX",
                Url = new Uri("https://example.com/license"),
            }
        });

        // Set the comments path for the Swagger JSON and UI.
        var xmlFile = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
        var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);
        c.IncludeXmlComments(xmlPath);
    });
}
```

<span data-ttu-id="1cb4b-150">在上述程式碼中，[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) 用來建置與 Web API 專案名稱相符的 XML 檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-150">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="1cb4b-151">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory) 屬性用來建構 XML 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-151">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="1cb4b-152">某些 Swagger 功能 (例如輸入參數的結構描述，或來自相對應屬性的 HTTP 方法和回應碼) 能在不使用 XML 文件檔案的情況下運作。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-152">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="1cb4b-153">對於大部分的功能 (也就是方法摘要，以及參數和回應碼的描述) 而言，皆必須使用 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-153">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="1cb4b-154">將三斜線註解新增至動作，即可透過將描述新增至區段標頭來增強 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-154">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="1cb4b-155">在 [ 動作上方新增 \<](/dotnet/csharp/programming-guide/xmldoc/summary)summary>`Delete` 項目：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-155">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

```csharp
/// <summary>
/// Deletes a specific TodoItem.
/// </summary>
/// <param name="id"></param>        
[HttpDelete("{id}")]
public IActionResult Delete(long id)
{
    var todo = _context.TodoItems.Find(id);

    if (todo == null)
    {
        return NotFound();
    }

    _context.TodoItems.Remove(todo);
    _context.SaveChanges();

    return NoContent();
}
```
<span data-ttu-id="1cb4b-156">Swagger UI 會顯示上述程式碼的 `<summary>` 項目的內部文字：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-156">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![顯示 XML 註解 'Deletes a specific TodoItem.' 的 Swagger UI](sample_images/triple-slash-comments.png)

<span data-ttu-id="1cb4b-159">UI 是由產生的 JSON 結構描述所驅動：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-159">The UI is driven by the generated JSON schema:</span></span>

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
<span data-ttu-id="1cb4b-160">請將 [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) 項目新增至 `Create` 動作方法文件。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-160">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="1cb4b-161">它會補充 `<summary>` 項目中指定的資訊，並提供更強固的 Swagger UI。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-161">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="1cb4b-162">`<remarks>` 項目內容可以包含文字、JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-162">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

```csharp
/// <summary>
/// Creates a TodoItem.
/// </summary>
/// <remarks>
/// Sample request:
///
///     POST /Todo
///     {
///        "id": 1,
///        "name": "Item1",
///        "isComplete": true
///     }
///
/// </remarks>
/// <param name="item"></param>
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
{
    _context.TodoItems.Add(item);
    _context.SaveChanges();

    return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```
<span data-ttu-id="1cb4b-163">請注意使用這些額外註解的 UI 增強功能：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-163">Notice the UI enhancements with these additional comments:</span></span>

![顯示額外註解的 Swagger UI](sample_images/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="1cb4b-165">資料註解</span><span class="sxs-lookup"><span data-stu-id="1cb4b-165">Data annotations</span></span>

<span data-ttu-id="1cb4b-166">使用在[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)命名空間中找到的屬性來標記模型，以協助驅動 Swagger UI 元件。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-166">Mark the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="1cb4b-167">將 `[Required]` 屬性 (attribute) 新增至 `Name` 類別的 `TodoItem` 屬性 (property)：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-167">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

```csharp
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;

namespace TodoApi.Models
{
    public class TodoItem
    {
        public long Id { get; set; }

        [Required]
        public string Name { get; set; }

        [DefaultValue(false)]
        public bool IsComplete { get; set; }
    }
}
```

<span data-ttu-id="1cb4b-168">這個屬性的存在會變更 UI 行為，並改變基礎的 JSON 結構描述：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-168">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="1cb4b-169">將 `[Produces("application/json")]` 屬性新增至 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-169">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="1cb4b-170">其目的是要宣告控制器的動作支援回應內容類型為 *application/json*：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-170">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

```csharp
[Produces("application/json")]
[Route("api/[controller]")]
[ApiController]
public class TodoController : ControllerBase
{
    private readonly TodoContext _context;
```
<span data-ttu-id="1cb4b-171">[回應內容類型] 下拉式清單會選取此內容類型，作為控制器 GET 動作的預設值：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-171">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![含有預設回應內容類型的 Swagger UI](sample_images/json-response-content-type.png)

<span data-ttu-id="1cb4b-173">當 Web API 中的資料註釋使用量增加時，UI 和 API 說明頁面會變得更清楚且更有用。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-173">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="1cb4b-174">描述回應類型</span><span class="sxs-lookup"><span data-stu-id="1cb4b-174">Describe response types</span></span>

<span data-ttu-id="1cb4b-175">取用 Web API 的開發人員最關心的是傳回的內容 &mdash; 特別是回應類型和錯誤碼 (如果不是標準的話)。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-175">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="1cb4b-176">回應類型和錯誤碼會在 XML 註解及資料註解中表示。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-176">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="1cb4b-177">`Create` 動作在成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-177">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="1cb4b-178">張貼的要求主體為 Null 時，會傳回 HTTP 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-178">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="1cb4b-179">如果 Swagger UI 中沒有適當的文件，取用者便會缺乏對這些預期結果的了解。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-179">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="1cb4b-180">在下列範例中新增強調顯示的那幾行來修正該問題：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-180">Fix that problem by adding the highlighted lines in the following example:</span></span>

```csharp
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
```

<span data-ttu-id="1cb4b-181">現在，Swagger UI 清楚記載了預期的 HTTP 回應碼：</span><span class="sxs-lookup"><span data-stu-id="1cb4b-181">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![顯示 POST 回應類別描述 'Returns the newly created Todo item'，並針對回應訊息下方的狀態碼和原因顯示 '400 - If the item is null' 的 Swagger UI](sample_images/data-annotations-response-types.png)

<span data-ttu-id="1cb4b-183">在 ASP.NET Core 2.2 或更新版本中，慣例可作為使用 `[ProducesResponseType]` 明確裝飾個別動作的替代方案。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-183">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="1cb4b-184">如需詳細資訊，請參閱[使用 Web API 慣例](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions)。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-184">For more information, see [Use web API conventions](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions).</span></span>

<span data-ttu-id="1cb4b-185">如需自訂 UI 的詳細資訊，請參閱[自訂 ui](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend) 。</span><span class="sxs-lookup"><span data-stu-id="1cb4b-185">For information on customizing the UI see: [Customize the UI](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend)</span></span>
