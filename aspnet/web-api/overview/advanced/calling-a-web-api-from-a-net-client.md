---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: 從.NET 用戶端 (C#) 呼叫 Web API |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: fdb74b0eb74ce7f387f49a0b25ceebd3fc389da9
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "32006153"
---
<a name="call-a-web-api-from-a-net-client-c"></a>呼叫 Web API，從.NET 用戶端 (C#)
====================
由[Mike Wasson](https://github.com/MikeWasson)和[Rick Anderson](https://twitter.com/RickAndMSFT)

[下載完成的專案](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)。 [下載指示](/aspnet/core/tutorials/#how-to-download-a-sample)。 

本教學課程示範如何從.NET 應用程式呼叫 web API 使用[System.Net.Http.HttpClient。](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

在本教學課程中，用戶端應用程式會撰寫可使用下列 web 應用程式開發介面：

| 動作 | HTTP 方法 | 相對 URI |
| --- | --- | --- |
| 取得產品識別碼 | GET | /api/產品/*識別碼* |
| 建立新的產品 | POST | / api/產品 |
| 產品更新 | PUT | /api/產品/*識別碼* |
| 刪除產品 | DELETE | /api/產品/*識別碼* |

若要深入了解如何實作此 API 與 ASP.NET Web API，請參閱[建立 Web API 的支援 CRUD 操作](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
)。

為了簡單起見，本教學課程中的用戶端應用程式是 Windows 主控台應用程式。 **HttpClient**也支援 Windows Phone 和 Windows 市集應用程式。 如需詳細資訊，請參閱[撰寫 Web API 用戶端程式碼的多個平台使用可攜式程式庫](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>建立主控台應用程式

在 Visual Studio 中，建立名為的新 Windows 主控台應用程式**HttpClientSample**並貼上下列程式碼：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

上述程式碼是完整的用戶端應用程式。

`RunAsync` 執行，並封鎖，直到它完成。 大部分**HttpClient**方法都是非同步，因為它們執行網路 I/O。 所有的非同步工作完成內`RunAsync`。 一般應用程式不會封鎖主執行緒，但此應用程式不允許任何互動。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>安裝 Web API 用戶端程式庫

使用 NuGet 套件管理員來安裝 Web API 的用戶端程式庫套件。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。 在 封裝管理員主控台 (PMC)，輸入下列命令：

`Install-Package Microsoft.AspNet.WebApi.Client`

上述命令會將下列 NuGet 封裝加入專案：

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET 是常用的高效能 JSON framework for.NET。

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>將模型類別

檢查`Product`類別：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

這個類別會比對 web API 所使用的資料模型。 應用程式可以使用**HttpClient**讀取`Product`之 HTTP 回應中的執行個體。 應用程式不需要撰寫任何的還原序列化程式碼。

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>建立和初始化 HttpClient

檢查靜態**HttpClient**屬性：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient**是用來具現化一次，並且在應用程式的生命週期中重複使用。 以下條件可能會導致**SocketException**錯誤：

* 建立新**HttpClient**每個要求的執行個體。
* 伺服器負載過重。

建立新**HttpClient**每個要求的執行個體可以耗盡可用的通訊端。

下列程式碼會初始化**HttpClient**執行個體：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

上述程式碼：

* 設定 HTTP 要求的基底 URI。 將連接埠號碼變更為伺服器應用程式中使用的通訊埠。 應用程式將無法運作，除非連接埠為伺服器應用程式。
* 設定為"application/json"Accept 標頭。 設定此標頭會要求伺服器傳送 JSON 格式的資料。

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>傳送 GET 要求來擷取資源

下列程式碼會傳送 GET 要求的產品：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync**方法會傳送 HTTP GET 要求。 此方法完成時，它會傳回**HttpResponseMessage** ，其中包含 HTTP 回應。 在回應中的狀態碼是否成功的程式碼，回應主體會包含產品的 JSON 表示法。 呼叫**ReadAsAsync**還原序列化 JSON 裝載`Product`執行個體。 **ReadAsAsync**方法是非同步的因為回應主體可以是任意大。

**HttpClient** HTTP 回應包含錯誤程式碼時，不會擲回例外狀況。 相反地， **IsSuccessStatusCode**屬性是**false**如果狀態為錯誤碼。 如果您想要將 HTTP 錯誤碼都視為例外狀況，呼叫[HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx)回應物件上。 `EnsureSuccessStatusCode` 擲回例外狀況，如果狀態碼超出範圍 200&ndash;299。 請注意， **HttpClient**可以擲回例外狀況，因為其他原因而&mdash;比方說，如果要求逾時。

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>若要還原序列化的媒體類型格式器

當**ReadAsAsync**呼叫不含任何參數，它會使用一組預設的*媒體格式器*讀取回應主體。 預設格式器支援 JSON、 XML 和表單 url 編碼資料。

不使用預設格式器，您可以提供的格式器清單**ReadAsAsync**方法。  使用格式器清單很有用，如果您有自訂的媒體類型格式器：

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

如需詳細資訊，請參閱[ASP.NET Web API 2 中的媒體格式器](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>傳送 POST 要求建立資源

下列程式碼傳送 POST 要求，其中包含`Product`JSON 格式的執行個體：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync**方法：

* 將物件序列化為 JSON。
* 將 JSON 承載傳送 POST 要求中。

如果要求成功：

* 它應該會傳回 201 （已建立） 回應。
* 回應應包含建立資源的 URL 位置標頭。

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>傳送 PUT 的要求來更新資源

下列程式碼會傳送 PUT 要求，將產品更新：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync**方法的運作方式類似**PostAsJsonAsync**，只不過它會傳送 PUT 要求，而不是 POST。

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>刪除要求傳送到刪除資源

下列程式碼會傳送 DELETE 要求來刪除產品：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

例如 GET、 DELETE 要求並沒有要求主體。 您不需要與 DELETE 一起指定 JSON 或 XML 格式。

## <a name="test-the-sample"></a>測試範例

若要測試用戶端應用程式：

1. [下載](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server)並執行伺服器應用程式。 [下載指示](/aspnet/core/tutorials/#how-to-download-a-sample)。 請確認伺服器應用程式運作。 Exaxmple，如`http://localhost:64195/api/products`應該會傳回一份產品清單。
2. 設定 HTTP 要求的基底 URI。 將連接埠號碼變更為伺服器應用程式中使用的通訊埠。
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. 執行用戶端應用程式。 會產生下列輸出：

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
