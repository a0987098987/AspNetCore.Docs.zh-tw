---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: 屬性 ASP.NET Web API 2 中的路由 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 22eb2fd748d52ec95e813ada8b1bf3b4826ad573
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348477"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的屬性路由
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

*路由*是 Web API 如何比對動作的 URI。 Web API 2 支援新類型的路由，稱為*屬性路由*。 如同名稱所暗示，屬性路由會使用屬性來定義的路由。 屬性路由可讓您更充分掌控 Uri 在 web API 中。 例如，您可以輕鬆地建立描述階層的資源的 Uri。

呼叫以慣例為基礎的舊版樣式的路由，路由仍然完整支援。 事實上，您可以結合這兩種技巧，在相同的專案。

本主題說明如何啟用屬性路由，並且描述屬性路由的各種選項。 使用屬性路由端對端教學課程中，請參閱[使用 Web API 2 中的屬性路由建立 REST API](create-a-rest-api-with-attribute-routing.md)。

## <a name="prerequisites"></a>必要條件

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、 Professional 或 Enterprise edition

或者，使用 NuGet 套件管理員來安裝必要的套件。 從**工具**功能表，在 Visual Studio 中，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中輸入下列命令：

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>為什麼屬性路由？

使用 Web API 的第一版*以慣例為基礎*路由。 在這類的路由中，您定義一個或多個路由範本，基本上便是參數化字串。 當架構收到要求時，它會比對路由範本的 URI。 (如需以慣例為基礎的路由的詳細資訊，請參閱[ASP.NET Web API 中的路由](routing-in-aspnet-web-api.md)。

以慣例為基礎的路由的優點之一是，範本會定義在單一位置，和路由規則會一致地套用到所有控制站。 不幸的是，將慣例為基礎的路由，讓更難以支援特定常見於 RESTful Api 的 URI 模式。 例如，資源通常會包含子資源： 客戶具有訂單，電影的動作項目，而活頁簿有作者，等等。 很自然地建立反映這些關聯性的 Uri:

`/customers/1/orders`

此類型的 URI 很難建立使用以慣例為基礎的路由。 雖然也可以完成，但如果您有許多的控制站或資源類型，就不會調整結果。

使用屬性路由中，就一般定義的路由，這個 uri。 您只會將屬性新增至控制器動作：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

以下是一些其他屬性路由可讓您輕鬆的模式。

**API 版本設定**

在此範例中，"/api/v1/products"路由會傳送到不同的控制器"/api/v2/products"。

`/api/v1/products`
`/api/v2/products`

**多載的 URI 區段**

在此範例中，"1"是順序的數字，但是"pending"對應至集合。

`/orders/1`
`/orders/pending`

**多個參數類型**

在此範例中，"1"是順序的數字，但是"2013/06/16"指定的日期。

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>啟用屬性路由

若要啟用屬性路由，請呼叫**MapHttpAttributeRoutes**設定期間。 這個擴充方法中定義**System.Web.Http.HttpConfigurationExtensions**類別。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

屬性路由可以結合[以慣例為基礎](routing-in-aspnet-web-api.md)路由。 若要定義以慣例為基礎的路由，請呼叫**MapHttpRoute**方法。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

如需有關如何設定 Web API 的詳細資訊，請參閱 <c0> [ 設定 ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md)。

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>注意： 從 Web API 1 進行移轉

Web API 2 之前, 的 Web API 專案範本會產生如下的程式碼：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

如果已啟用屬性路由，此程式碼會擲回例外狀況。 如果您升級現有的 Web API 專案以使用屬性路由，請務必更新這個組態程式碼所示：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> 如需詳細資訊，請參閱 <<c0> [ 設定與 ASP.NET 裝載的 Web API](../advanced/configuring-aspnet-web-api.md#webhost)。


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>新增路由屬性

使用屬性來定義路由的範例如下：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

字串&quot;customers/{customerId}/orders&quot;是路由的 URI 範本。 Web API 會嘗試比對要求的 URI 範本。 在此範例中，"customers"和"orders"是常值的區段， 下列 Uri 會符合此範本：

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

您可以藉由比對來限制[條件約束](#constraints)，本主題稍後所述。

請注意， &quot;{customerId}&quot;路由範本中的參數名稱相符*customerId*方法中的參數。 當 Web API 會叫用控制器動作時，它會嘗試將路由參數繫結。 比方說，如果 URI 為`http://example.com/customers/1/orders`，Web API 會嘗試將值"1"的繫結*customerId*動作中的參數。

URI 樣板可以有數個參數：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

路由屬性沒有任何控制器方法會使用以慣例為基礎的路由。 如此一來，您可以結合這兩種類型的同一個專案中的路由。

## <a name="http-methods"></a>HTTP 方法

Web API，也會根據要求的 HTTP 方法 （GET、 POST 等） 的動作。 根據預設，Web API 會尋找不區分大小寫的比對控制器方法名稱的開頭。 例如，名為控制器方法`PutCustomers`符合 HTTP PUT 要求。

您可以覆寫此慣例裝飾的方法與任何下列屬性：

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

下列範例會將 CreateBook 方法對應至 HTTP POST 要求。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

對於所有其他 HTTP 方法，包括非標準的方法，請使用**AcceptVerbs**屬性，它會使用 HTTP 方法清單。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>路由前置詞

通常中的路由所有以相同的前置詞開頭的控制站。 例如: 

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

您也可以使用整個控制器設定一般的前置詞 **[RoutePrefix]** 屬性：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

方法的屬性上使用波狀符號 （~），來覆寫的路由前置詞：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

路由前置詞可以包含參數：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>路由條件約束

路由條件約束可讓您限制如何比對路由範本中的參數。 一般語法&quot;{參數： 條件約束}&quot;。 例如: 

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

在這裡，第一個路由將只會選取如果&quot;識別碼&quot;URI 區段是一個整數。 否則，將會選擇第二個路由。

下表列出支援的條件約束。

| 條件約束 | 描述 | 範例 |
| --- | --- | --- |
| alpha | 比對大寫或小寫英文字母字元 (a-z、 A-Z) | {x: alpha} |
| bool | 布林值，會比對。 | {x: bool} |
| datetime | 相符項目**DateTime**值。 | {x: datetime} |
| decimal | 符合十進位值。 | {x： 十進位} |
| double | 比對 64 位元浮點值。 | {x： 雙} |
| float | 比對 32 位元浮點值。 | {x: float} |
| guid | 比對的 GUID 值。 | {x: guid} |
| int | 比對 32 位元整數值。 | {x: int} |
| 長度 | 符合使用指定的長度，或在指定的長度範圍內的字串。 | {x: length(6)}{x: length(1,20)} |
| long | 比對 64 位元整數值。 | {x： 長時間} |
| max | 比對的最大值的整數。 | {x:max(10)} |
| maxlength | 符合最大長度的字串。 | {x: maxlength(10)} |
| min | 符合最小值的整數。 | {x: min(10)} |
| minlength | 符合最小長度的字串。 | {x: minlength(10)} |
| range | 比對的值範圍內的整數。 | {x: range(10,50)} |
| regex | 符合規則運算式。 | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

請注意一些條件約束，例如&quot;min&quot;，接受括號括住的引數。 您可以將多個條件約束套用至參數，並以冒號分隔。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>自訂路由條件約束

您可以建立自訂路由條件約束，藉由實作**IHttpRouteConstraint**介面。 例如，下列條件約束限制參數為非零的整數值。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

下列程式碼示範如何註冊條件約束：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

現在您可以套用條件約束，在您的路由：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

您也可以取代整個**DefaultInlineConstraintResolver**藉由實作類別**IInlineConstraintResolver**介面。 這樣將會取代所有的內建的條件約束，除非您實作**IInlineConstraintResolver**特別將其加入。

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>選擇性 URI 參數和預設值

您可以讓 URI 參數選擇性加入路由參數的問號。 如果路由參數為選擇性的您必須定義方法參數的預設值。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

在此範例中，`/api/books/locale/1033`和`/api/books/locale`傳回相同的資源。

或者，您可以依下列方式指定在路由範本中，預設值：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

這是上述範例中，幾乎相同，但有所些微差異的行為時所套用的預設值。

- 在第一個範例 ("{lcid?}") 中，1033年的預設值是直接指派給方法參數，讓參數會將此的確切值。
- 在第二個範例中 ("{lcid = 1033年}")，"1033"的預設值會透過模型繫結程序。 預設模型繫結器將轉換成數值 1033年"1033"。 不過，您可以插入可能會執行一些不同的自訂模型繫結器。

（在大部分情況下，除非您有自訂模型繫結器在管線中，兩種形式會是對等）。

<a id="route-names"></a>
## <a name="route-names"></a>路由名稱

在 Web API 中，每個路由會有一個名稱。 路由名稱適合用於產生連結，以便您可以在 HTTP 回應中包含的連結。

指定的路由名稱，請設定**名稱**屬性上的屬性。 下列範例會示範如何設定路由名稱，以及如何產生連結時所使用的路由名稱。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>路由順序

當此架構會嘗試比對路由的 URI 時，它會評估以特定順序的路由。 若要指定的順序，將**順序**屬性路由。 會先評估較低的值。 預設順序值為零。

以下是如何決定總排序：

1. 比較**順序**路由屬性的屬性。
2. 查看路由範本中的每個 URI 區段。 對於每個區段中，排序，如下所示：

    1. 常值的區段。
    2. 路由條件約束使用的參數。
    3. 路由參數，而不需要條件約束。
    4. 使用條件約束的萬用字元參數區段。
    5. 萬用字元參數區段，而不需要條件約束。
3. 在繫結的情況下，路由會依不區分大小寫的序數字串比較 ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) 的路由範本。

以下是一個範例。 假設您會定義下列控制器：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

這些路由會排序，如下所示。

1. orders/details
2. orders/{id}
3. orders/{customerName}
4. orders/{\*date}
5. orders/pending

請注意，"details"是常值的區段以及"{id}"之前出現，但"pending"會顯示上次因為**順序**屬性為 1。 (這個範例假設沒有客戶名叫做"details"或"pending"。 一般情況下，請盡量避免模稜兩可的路由。 在此範例中，較佳的路由範本，如`GetByCustomer`是'customers/{customerName}")
