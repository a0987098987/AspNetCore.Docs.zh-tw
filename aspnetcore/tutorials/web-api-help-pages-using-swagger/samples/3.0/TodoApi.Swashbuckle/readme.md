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
ms.openlocfilehash: d48288de90626ada83f5da1759f0057f0be46f19
ms.sourcegitcommit: f91d322f790123d41ec3271fa084ae20ed9f89a6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2019
ms.locfileid: "74155141"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a>Swashbuckle 與 ASP.NET Core 使用者入門

使用 Web API 時，了解其各種不同的方法對開對發人員來說可能是一項挑戰。 [Swagger](https://swagger.io/) (也稱為 [Open API](https://www.openapis.org/)) 解決為 Web API 產生有用文件與說明頁面的問題。 它提供如互動式文件、用戶端 SDK 產生作業和 API 發現性等優點。

在此範例中，會顯示 .NET 實作 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)。

## <a name="add-and-configure-swagger-middleware"></a>新增和設定 Swagger 中介軟體

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

在 `Startup.Configure` 方法中，啟用中介軟體為產生的 JSON 文件和 Swagger UI 提供服務：

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

上述 `UseSwaggerUI` 方法呼叫會啟用[靜態檔案中介軟體](https://docs.microsoft.com/aspnet/core/fundamentals/static-files)。 如果以 .NET Framework 或 .NET Core 1.x 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet 套件新增至專案。

啟動應用程式，並巡覽至 `http://localhost:<port>/swagger/v1/swagger.json`。 描述端點的已產生文件隨即出現，如 [Swagger 規格 (swagger.json)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson) 中所示。

您可以在 `http://localhost:<port>/swagger` 找到 Swagger UI。 透過 Swagger UI 探索 API，並將其併入其他程式。

> [!TIP]
> 若要在應用程式的根目錄 (`http://localhost:<port>/`) 上提供 Swagger UI，請將 `RoutePrefix` 屬性設為空字串：
>
> ```csharp
>app.UseSwaggerUI(c =>
>{
>    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
>    c.RoutePrefix = string.Empty;
>});
>```

若使用目錄搭配 IIS 或 反向 Proxy，請將 Swagger 端點設定為使用 `./` 前置詞的相對路徑。 例如，`./swagger/v1/swagger.json`。 使用 `/swagger/v1/swagger.json` 指示應用程式在 URL 的真實根目錄 (若已使用，請加上路由前置詞) 尋找 JSON 檔案。 例如，使用 `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` 而不是 `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`。

## <a name="customize-and-extend"></a>自訂與擴充

Swagger 提供用來記載物件模型和自訂 UI 以符合佈景主題的選項。

在 `Startup` 類別中，加入下列命名空間：
```csharp
using System;
using System.Reflection;
using System.IO;
```

### <a name="api-info-and-description"></a>API 資訊與描述

傳遞至 `AddSwaggerGen` 方法的組態動作會新增作者、授權和描述等資訊：

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

Swagger UI 會顯示版本資訊：

![含有版本資訊的 Swagger UI：描述、作者，以及查看更多連結](sample_images/custom-info.png)

### <a name="xml-comments"></a>XML 註解

XML 註解可以使用下列方式啟用：

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 以滑鼠右鍵按一下 [方案總管] 中的專案，然後選取 [編輯 <專案名稱>.csproj]。
* 將醒目提示的程式碼行手動新增至 *.csproj* 檔案：

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 從 [Solution Pad] 中，按下 [控制項]，然後按一下專案名稱。 巡覽至 [工具] > [編輯檔案]。
* 將醒目提示的程式碼行手動新增至 *.csproj* 檔案：

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

將醒目提示的程式碼行手動新增至 *.csproj* 檔案：

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

---

啟用 XML 註解，可提供未記載之公用類型與成員的偵錯資訊。 未記載的類型和成員會以警告訊息表示。 例如，下列訊息指出警告碼 1591 的違規：

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

在專案檔中定義要忽略的警告碼清單 (以分號分隔)，即可隱藏警告。 將警告碼附加至 `$(NoWarn);` 也會套用 [C# 預設值](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16)。

```xml
<NoWarn>$(NoWarn);1591</NoWarn>
```

若只要針對特定成員隱藏，請將程式碼放在 [#pragma 警告](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning)前置處理器指示詞中。 這個方法適用于不應透過 API 檔公開的程式碼。在下列範例中，會忽略整個 `Program` 類別的警告程式碼 CS1591。 會在類別定義結尾處還原強制執行的警告碼。 以逗號分隔清單指定多個警告碼。

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

設定 Swagger 以使用先前指示所產生的 XML 檔案。 對於 Linux 或非 Windows 作業系統，檔案名稱和路徑可以區分大小寫。 例如，*TodoApi.XML* 檔案在 Windows 上有效，但在 CentOS 上則無效。

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

在上述程式碼中，[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) 用來建置與 Web API 專案名稱相符的 XML 檔案名稱。 [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory) 屬性用來建構 XML 檔案的路徑。 某些 Swagger 功能 (例如輸入參數的結構描述，或來自相對應屬性的 HTTP 方法和回應碼) 能在不使用 XML 文件檔案的情況下運作。 對於大部分的功能 (也就是方法摘要，以及參數和回應碼的描述) 而言，皆必須使用 XML 檔案。

將三斜線註解新增至動作，即可透過將描述新增至區段標頭來增強 Swagger UI。 在 `Delete` 動作上方新增 [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) 項目：

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
Swagger UI 會顯示上述程式碼的 `<summary>` 項目的內部文字：

![顯示 XML 註解 'Deletes a specific TodoItem.' 的 Swagger UI 適用於 Delete 方法](sample_images/triple-slash-comments.png)

UI 是由產生的 JSON 結構描述所驅動：

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
請將 [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) 項目新增至 `Create` 動作方法文件。 它會補充 `<summary>` 項目中指定的資訊，並提供更強固的 Swagger UI。 `<remarks>` 項目內容可以包含文字、JSON 或 XML。

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
請注意使用這些額外註解的 UI 增強功能：

![顯示額外註解的 Swagger UI](sample_images/xml-comments-extended.png)

### <a name="data-annotations"></a>資料註解

使用 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 命名空間中找到的屬性來裝飾模型，可協助驅動 Swagger UI 元件。

將 `[Required]` 屬性 (attribute) 新增至 `TodoItem` 類別的 `Name` 屬性 (property)：

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

這個屬性的存在會變更 UI 行為，並改變基礎的 JSON 結構描述：

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

將 `[Produces("application/json")]` 屬性新增至 API 控制器。 其目的是要宣告控制器的動作支援回應內容類型為 *application/json*：

```csharp
[Produces("application/json")]
[Route("api/[controller]")]
[ApiController]
public class TodoController : ControllerBase
{
    private readonly TodoContext _context;
```
[回應內容類型] 下拉式清單會選取此內容類型，作為控制器 GET 動作的預設值：

![含有預設回應內容類型的 Swagger UI](sample_images/json-response-content-type.png)

當 Web API 中的資料註釋使用量增加時，UI 和 API 說明頁面會變得更清楚且更有用。

### <a name="describe-response-types"></a>描述回應類型

取用 Web API 的開發人員最關心的是傳回的內容 &mdash; 特別是回應類型和錯誤碼 (如果不是標準的話)。 回應類型和錯誤碼會在 XML 註解及資料註解中表示。

`Create` 動作在成功時會傳回 HTTP 201 狀態碼。 張貼的要求主體為 Null 時，會傳回 HTTP 400 狀態碼。 如果 Swagger UI 中沒有適當的文件，取用者便會缺乏對這些預期結果的了解。 在下列範例中新增強調顯示的那幾行來修正該問題：

```csharp
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
```

現在，Swagger UI 清楚記載了預期的 HTTP 回應碼：

![顯示 POST 回應類別描述 'Returns the newly created Todo item'，並針對回應訊息下方的狀態碼和原因顯示 '400 - If the item is null' 的 Swagger UI](sample_images/data-annotations-response-types.png)

在 ASP.NET Core 2.2 或更新版本中，慣例可作為使用 `[ProducesResponseType]` 明確裝飾個別動作的替代方案。 如需詳細資訊，請參閱[使用 Web API 慣例](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions)。

如需自訂 UI 的詳細資訊，請參閱[自訂 ui](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend) 。
