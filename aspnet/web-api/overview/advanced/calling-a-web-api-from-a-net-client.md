---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: 呼叫 Web API 的.NET 用戶端 (C#) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: be237bee43bc5e32939cb0b3e0948fd8b35bd1eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831767"
---
<a name="call-a-web-api-from-a-net-client-c"></a>呼叫 Web API 的.NET 用戶端 (C#)
====================
藉由[Mike Wasson](https://github.com/MikeWasson)和[Rick Anderson](https://twitter.com/RickAndMSFT)

[下載已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)。 [下載指示](/aspnet/core/tutorials/#how-to-download-a-sample)。 

本教學課程示範如何從.NET 應用程式呼叫 web API 使用[System.Net.Http.HttpClient。](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

在本教學課程中，用戶端應用程式會寫入，使用下列 web API:

| 動作 | HTTP 方法 | 相對 URI |
| --- | --- | --- |
| 取得產品識別碼 | GET | /api/產品/*識別碼* |
| 建立新的產品 | POST | / api/產品 |
| 將產品更新 | PUT | /api/產品/*識別碼* |
| 刪除產品 | DELETE | /api/產品/*識別碼* |

若要了解如何實作這個使用 ASP.NET Web API 的 API，請參閱[建立 Web API 的支援 CRUD 作業](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
)。

為了簡單起見，本教學課程中的用戶端應用程式是一個 Windows 主控台應用程式。 **HttpClient**也支援 Windows Phone 和 Windows 市集應用程式。 如需詳細資訊，請參閱[撰寫 Web API 用戶端程式碼的多個平台使用可攜式程式庫](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>建立主控台應用程式

在 Visual Studio 中建立名為的新 Windows 主控台應用程式**HttpClientSample**並貼上下列程式碼：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

上述程式碼是完整的用戶端應用程式。

`RunAsync` 執行並封鎖，直到完成為止。 大部分**HttpClient**方法都是非同步，因為它們執行網路 I/O。 內的所有非同步工作完成`RunAsync`。 通常應用程式不會封鎖主執行緒，但此應用程式不允許任何互動。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>安裝 Web API 用戶端程式庫

使用 NuGet 套件管理員安裝的 Web API 用戶端程式庫封裝。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。 在 套件管理員主控台 (PMC)，輸入下列命令：

`Install-Package Microsoft.AspNet.WebApi.Client`

上述命令會將下列 NuGet 封裝加入專案：

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET 是受歡迎的高效能 JSON 架構適用於.NET。

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>新增模型類別

檢查`Product`類別：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

此類別會比對 web API 所使用的資料模型。 應用程式可以使用**HttpClient**讀取`Product`之 HTTP 回應中的執行個體。 應用程式不需要撰寫任何的還原序列化程式碼。

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>建立並初始化 HttpClient

檢查靜態**HttpClient**屬性：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient**是用來具現化一次，並且重複使用的應用程式生命週期。 在下列情況可能會導致**SocketException**錯誤：

* 建立新**HttpClient**每個要求的執行個體。
* 伺服器負載過重。

建立新**HttpClient**每個要求的執行個體可能會耗盡可用的通訊端。

下列程式碼會初始化**HttpClient**執行個體：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

上述程式碼：

* 設定 HTTP 要求的基底 URI。 將伺服器應用程式中使用的連接埠的連接埠號碼。 應用程式將無法運作，除非會使用伺服器應用程式的連接埠。
* 設定為"application/json"的 Accept 標頭。 設定此標頭會要求伺服器傳送資料以 JSON 格式。

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>傳送 GET 要求擷取資源

下列程式碼會傳送 GET 要求產品：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync**方法會傳送 HTTP GET 要求。 方法完成時，它會傳回**HttpResponseMessage** ，其中包含 HTTP 回應。 如果在回應中的狀態碼是成功的程式碼，回應主體會包含產品的 JSON 表示法。 呼叫**ReadAsAsync**還原序列化 JSON 承載，以`Product`執行個體。 **ReadAsAsync**方法是非同步的因為回應主體可以是任意大。

**HttpClient** HTTP 回應會包含錯誤程式碼時，不會擲回例外狀況。 相反地， **IsSuccessStatusCode**屬性是**false**如果狀態為錯誤碼。 如果您想要視為例外狀況的 HTTP 錯誤代碼，呼叫[HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx)回應物件上。 `EnsureSuccessStatusCode` 如果狀態碼超出範圍 200，則擲回例外狀況&ndash;299。 請注意， **HttpClient**可以擲回例外狀況，因為其他原因而&mdash;比方說，如果要求逾時。

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>若要還原序列化的媒體類型格式器

當**ReadAsAsync**呼叫使用任何參數，它會使用一組預設*媒體格式器*讀取回應主體。 預設格式器支援 JSON、 XML 和表單 url 編碼資料。

而不是使用預設格式器，您可以提供的格式器清單**ReadAsAsync**方法。  使用格式器清單很有用，如果您有自訂的媒體類型格式器：

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

如需詳細資訊，請參閱[ASP.NET Web API 2 中的媒體格式器](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>傳送 POST 要求來建立資源

下列程式碼會傳送 POST 要求，其中包含`Product`以 JSON 格式的執行個體：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync**方法：

* 將物件序列化為 JSON。
* POST 要求中傳送的 JSON 承載。

如果要求成功：

* 它應該傳回 201 （已建立） 回應。
* 回應應包含建立的資源 URL 中的 Location 標頭。

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>傳送 PUT 的要求來更新資源

下列程式碼會傳送 PUT 要求，將產品更新：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync**方法的運作方式類似**PostAsJsonAsync**，只不過它會傳送 PUT 要求，而不是 POST。

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>傳送 DELETE 要求來刪除資源

下列程式碼會傳送 DELETE 要求來刪除產品：

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

例如 GET、 DELETE 要求沒有要求主體。 您不需要與 DELETE 一起指定 JSON 或 XML 格式。

## <a name="test-the-sample"></a>測試範例

若要測試用戶端應用程式：

1. [下載](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server)並執行伺服器應用程式。 [下載指示](/aspnet/core/tutorials/#how-to-download-a-sample)。 確認伺服器應用程式正常運作。 針對 exaxmple，`http://localhost:64195/api/products`應該會傳回一份產品。
2. 設定 HTTP 要求的基底 URI。 將伺服器應用程式中使用的連接埠的連接埠號碼。
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. 執行用戶端應用程式。 會產生下列輸出：

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
