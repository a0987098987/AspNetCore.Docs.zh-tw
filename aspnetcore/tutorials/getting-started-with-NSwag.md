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
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag 與 ASP.NET Core 使用者入門

作者：[Christoph Nienaber](https://twitter.com/zuckerthoben) 和 [Rico Suter](https://rsuter.com)

搭配使用 [NSwag](https://github.com/RSuter/NSwag) 與 ASP.NET Core 中介軟體需要有 [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet 套件。 該套件包含 Swagger 產生器、Swagger UI (第 2 版和第 3 版)，以及 [ReDoc UI](https://github.com/Rebilly/ReDoc)。

強烈建議您利用 NSwag 的程式碼產生功能。 選擇下列其中一個選項來產生程式碼：

* 使用 [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) 這個 Windows 桌面應用程式，以利用 C# 和 TypeScript 為您的 API 產生用戶端程式碼
* 使用 [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) 或 [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet 套件，以在您的專案內執行程式碼產生作業
* 從[命令列](https://github.com/NSwag/NSwag/wiki/CommandLine)使用 NSwag
* 使用 [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet 套件

## <a name="features"></a>功能

要使用 NSwag 的主要原因是不僅能夠引進 Swagger UI 和 Swagger 產生器，還能夠利用彈性的程式碼產生功能。 您不需要現有的 API&mdash;您可以使用包含 Swagger 的協力廠商 API ，並讓 NSwag 產生用戶端實作。 無論哪一種方式都會加速開發週期，因此您可以更輕鬆地隨著 API 變更進行調整。

## <a name="package-installation"></a>套件安裝

可使用下列方法新增 NSwag NuGet 套件：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 [套件管理員主控台] 視窗中：

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* 從 [管理 NuGet 套件] 對話方塊中：
  * 在方案總管 > [管理 NuGet 套件] 中，以滑鼠右鍵按一下您的專案
  * 將 [套件來源] 設定為 "nuget.org"
  * 在搜尋方塊中輸入 "NSwag.AspNetCore"
  * 從 [瀏覽] 索引標籤中選取 "NSwag.AspNetCore" 套件，並按一下 [安裝]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾
* 將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"
* 在搜尋方塊中輸入 NSwag.AspNetCore
* 從結果窗格中選取 NSwag.AspNetCore 套件，並按一下 [新增套件]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

從 [整合式終端機] 執行下列命令：

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

執行下列命令：

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>新增和設定 Swagger 中介軟體

在 `Info` 類別中匯入下列命名空間：

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

在 `Startup.Configure` 方法中，啟用中介軟體為產生的 Swagger 規格和 SwaggerUI 提供服務：

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

啟動應用程式。 巡覽至 `/swagger` 以檢視 Swagger UI。 巡覽至 `/swagger/v1/swagger.json` 以檢視 Swagger 規格。

## <a name="code-generation"></a>程式碼產生

### <a name="via-nswagstudio"></a>透過 NSwagStudio

* 從正式 [GitHub 存放庫](https://github.com/RSuter/NSwag/wiki/NSwagStudio)安裝 `NSwagStudio`。

* 啟動 NSwagStudio。 輸入 *swagger.json* 的位置或直接複製它：

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* 指出所需的用戶端輸出類型。 選項包括 [TypeScript 用戶端]、[CSharp 用戶端] 或 [CSharp Web API 控制器]。 使用 Web API 控制器基本上是反向產生作業。 它會使用服務的規格來重建服務。

* 按一下 [產生輸出]。

* 您可以在這裡看到使用 C# 中 *TodoApi.NSwag* 範例的完整用戶端實作：

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* 將檔案放入用戶端專案 (例如，[Xamarin.Forms](/xamarin/xamarin-forms/) 應用程式)。 開始取用 API：

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
> 您可以將基礎 URL 和/或 HTTP 用戶端插入至 API 用戶端。 最佳做法是一律[重複使用 HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)。

您現在可以開始輕鬆地將 API 實作到用戶端專案中。

### <a name="other-ways-to-generate-client-code"></a>產生用戶端程式碼的其他方式

您可以使用更適合工作流程的其他方式來產生程式碼：

* [ MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [在程式碼中](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [T4 範本](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>自訂

### <a name="xml-comments"></a>XML 註解

XML 註解可使用下列方式啟用：

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* 以滑鼠右鍵按一下方案總管中的專案，然後選取 [屬性]
* 檢查 [組建] 索引標籤的 [輸出] 區段下方的 [XML 文件檔]：

![專案屬性的 [組建] 索引標籤](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)
* 開啟 [專案選項] 對話方塊 > [組建] > [編譯器]
* 檢查 [一般選項] 區段下方的 [產生 XML 文件] 方塊：

![專案選項的 [一般選項] 區段](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
將下列程式碼片段手動新增至 *.csproj* 檔案：

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>資料註解

NSwag 會使用[反映](/dotnet/csharp/programming-guide/concepts/reflection)，而 Web API 動作的最佳做法是傳回 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult)。 因此，NSwag 無法推斷您的動作所執行的作業，以及它所傳回的內容。 參考下列範例：

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

上述動作會傳回 `IActionResult`，但在動作內部則會傳回 [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) 或 [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest)。 資料註解是用來告知用戶端此動作所傳回的 HTTP 回應。 使用下列屬性裝飾動作：

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Swagger 產生器現在可以正確描述此動作，而產生的用戶端會知道呼叫端點時它們所接收的內容。 強烈建議使用這些屬性裝飾所有動作。 如需 API 動作應該傳回哪些 HTTP 回應的指導方針，請參閱 [RFC 7231 規格](https://tools.ietf.org/html/rfc7231#section-4.3)。
