---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "屬性 ASP.NET Web API 2 中的路由 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: ad44ee525601f308498967159e964aa41a2ce00c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的路由屬性
====================
由[Mike Wasson](https://github.com/MikeWasson)

*路由*是 Web 應用程式開發介面如何比對的動作 URI。 Web API 2 支援新類型的路由，稱為*屬性路由*。 正如其名，路由屬性使用屬性來定義路由。 路由屬性讓您更充分掌控 Uri 在您的 web API。 例如，您可以輕鬆地建立描述階層架構的資源的 Uri。

呼叫慣例為基礎的舊樣式的路由，路由，仍完整支援。 事實上，您可以結合相同專案中的這兩種技術。

本主題顯示如何啟用路由的屬性，並描述屬性路由的各種選項。 端對端教學課程會使用屬性的路由，請參閱[屬由路由的 Web API 2 中建立 REST API](create-a-rest-api-with-attribute-routing.md)。


## <a name="prerequisites"></a>必要條件

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise Edition

或者，使用 NuGet 套件管理員來安裝必要的封裝。 從**工具**在 Visual Studio 中，選取功能表**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中輸入下列命令：

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>為什麼屬性路由？

Web API 所使用的第一版*慣例為基礎*路由。 在該類型的路由中，定義一個或多個路由範本，這基本上是參數化字串。 架構收到要求時，它會比對針對路由範本的 URI。 (如需慣例為基礎的路由的詳細資訊，請參閱[中 ASP.NET Web API 的路由](routing-in-aspnet-web-api.md)。

慣例為基礎的路由的其中一個優點是，樣板會定義在單一位置，和路由規則會一致地套用所有控制站。 不幸的是，慣例為基礎的路由讓人難以支援 rest 式應用程式開發介面中常見特定 URI 模式。 例如，資源通常會包含子資源： 客戶具有訂單，電影演員，書籍的作者可能，等等。 很自然建立 Uri，以反映這些關聯性：

`/customers/1/orders`

這種類型的 URI 很難建立使用以慣例為基礎的路由。 雖然可以完成，結果不縮放，如果您有多個控制站或資源類型。

路由屬性，它是 trivial 這個 uri 定義路由。 您只會將屬性加入控制器動作：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

以下是一些其他屬性路由可讓您輕鬆的模式。

**API 版本設定**

在此範例中，"/ 產品/api/v1"會路由至不同的控制站，比"/ api/v2/產品"。

`/api/v1/products`  
`/api/v2/products`

**多載的 URI 區段**

在此範例中，"1"是訂單號碼，但是 「 擱置 」 對應至集合。

`/orders/1`  
`/orders/pending`

**多個參數類型**

在此範例中，"1"是訂單號碼，但是"2013年/06/16"指定的日期。

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>啟用路由的屬性

若要啟用路由的屬性，請呼叫**MapHttpAttributeRoutes**期間設定。 這個擴充方法定義在**System.Web.Http.HttpConfigurationExtensions**類別。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

路由屬性可以結合[慣例為基礎](routing-in-aspnet-web-api.md)路由。 若要定義以慣例為基礎的路由，請呼叫**MapHttpRoute**方法。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

如需有關設定 Web API 的詳細資訊，請參閱[設定 ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md)。

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>從 Web API 1 移轉附註：

Web API 2 之前, 的 Web API 專案範本會產生如下的程式碼：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

如果屬性已啟用路由，這段程式碼將會擲回例外狀況。 如果您升級現有的 Web API 專案以使用屬性路由，請務必更新這個組態程式碼所示：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> 如需詳細資訊，請參閱[設定與 ASP.NET 裝載的 Web API](../advanced/configuring-aspnet-web-api.md#webhost)。


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>新增路由屬性

路由屬性來定義的範例如下：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

字串&quot;客戶 / {customerId} / 排序&quot;是路由的 URI 範本。 Web API 會嘗試比對要求 URI 範本。 在此範例中，「 客戶 」 和 「 訂單 」 是常值的區段，而且 {"customerId}"是變數參數。 下列 Uri 會符合此範本：

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

您可以限制透過比對[條件約束](#constraints)，在本主題稍後所述。

請注意， &quot;{customerId}&quot;路由範本中的參數符合名稱的*customerId*方法中的參數。 當 Web 應用程式開發介面會叫用控制器動作時，它會嘗試將路由參數繫結。 例如，如果 URI 是`http://example.com/customers/1/orders`，Web 應用程式開發介面來繫結值"1"嘗試將*customerId*動作中的參數。

URI 樣板可以有數個參數：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

路由屬性沒有任何控制器方法使用慣例為基礎的路由。 這樣一來，您可以結合兩種類型的相同專案中的路由。

## <a name="http-methods"></a>HTTP 方法

Web 應用程式開發介面也會選取動作的要求 （GET、 POST 等） 的 HTTP 方法為基礎。 根據預設，Web API 會尋找不區分大小寫的比對控制器方法名稱的開頭。 例如，名為控制器方法`PutCustomers`符合 HTTP PUT 要求。

您可以覆寫這個慣例而將與任何 mathod 下列屬性：

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

下列範例會將 CreateBook 方法對應至 HTTP POST 要求。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

對於所有其他 HTTP 方法，包括非標準的方法，請使用**AcceptVerbs**屬性，根據 HTTP 方法的清單。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>路由前置詞

通常中的路由所有開頭為相同的前置詞的控制站。 例如: 

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

您也可以使用整個控制器設定通用的前置詞**[RoutePrefix]**屬性：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

路由前置詞會覆寫方法的屬性上使用波狀符號 （~）：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

路由前置詞可以包含參數：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>路由條件約束

路由條件約束，可讓您限制如何比對路由範本中的參數。 一般語法&quot;{參數： 條件約束}&quot;。 例如: 

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

在這裡，第一個路由將只會選取如果&quot;識別碼&quot;區段的 URI 是整數。 否則，會選擇第二個路由。

下表列出支援的條件約束。

| 條件約束 | 說明 | 範例 |
| --- | --- | --- |
| Alpha | 比對大寫或小寫英文字母字元 (a-z、 A 到 Z) | {x: alpha} |
| bool | 比對的布林值。 | {x: bool} |
| datetime | 相符項目**DateTime**值。 | {x: datetime} |
| decimal | 符合十進位值。 | {x： 小} |
| double | 比對 64 位元浮點值。 | {x： 雙} |
| 浮動 | 比對 32 位元浮點值。 | {x: f} |
| Guid | 比對的 GUID 值。 | {x: guid} |
| int | 比對 32 位元整數值。 | {x: int} |
| 長度 | 比對字串指定長度或長度的指定範圍內。 | {x: length(6)}{x: length(1,20)} |
| long | 比對 64 位元整數值。 | {x： 長時間} |
| max | 符合最大值的整數。 | {x: max(10)} |
| maxlength | 符合最大長度的字串。 | {x: maxlength(10)} |
| min | 符合最小值的整數。 | {x: min(10)} |
| minlength | 符合最小長度的字串。 | {x: minlength(10)} |
| range | 比對的值範圍內的整數。 | {x: range(10,50)} |
| regex | 符合規則運算式。 | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

請注意一些條件約束，例如&quot;min&quot;，接受引數括號括住。 您可以將多個條件約束套用至參數，以冒號分隔。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>自訂路由條件約束

您可以建立自訂的路由條件約束，藉由實作**IHttpRouteConstraint**介面。 例如，下列條件約束限制參數為非零整數值。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

下列程式碼顯示如何註冊條件約束：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

現在您可以在您的路由中套用條件約束：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

您也可以取代整個**DefaultInlineConstraintResolver**藉由實作類別**IInlineConstraintResolver**介面。 這樣將會取代所有的內建的條件約束，除非您實作**IInlineConstraintResolver**特別將其加入。

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>選擇性 URI 參數和預設值

您可以讓 URI 參數選擇性加入路由參數的問號。 路由參數為選擇性，如果您必須定義的方法參數的預設值。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

在此範例中，`/api/books/locale/1033`和`/api/books/locale`傳回相同的資源。

或者，您可以指定在路由範本中，預設值如下：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

這是與上述範例中，幾乎相同，但是時，會有些微的差異的行為會套用預設值。

- 在第一個範例 ("{lcid？}") 中，1033年的預設值是直接指派給方法參數，讓參數會有這個精確的值。
- 在第二個範例中 ("{lcid = 1033年}")，"1033"的預設值會進行模型繫結程序。 預設模型繫結器將會轉換成數值 1033年"1033"。 不過，您可以插入可能會執行一些不同的自訂模型繫結器。

（在大部分情況下，除非您有自訂的模型繫結器在管線中，兩種形式會是對等）。

<a id="route-names"></a>
## <a name="route-names"></a>路由名稱

在 Web API 中，每個路由會有一個名稱。 路由名稱可用來產生連結，以便您可以在 HTTP 回應中包含的連結。

指定的路由名稱，請設定**名稱**屬性上的屬性。 下列範例會示範如何設定路由名稱，以及如何產生連結時所使用的路由名稱。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>路由順序

架構會嘗試比對具有路由的 URI，它會評估以特定順序中的路由。 若要指定的順序，將**RouteOrder**路由的屬性上的屬性。 會先評估較低的值。 預設順序值是零。

以下是如何決定總排序：

1. 比較**RouteOrder**路由屬性的屬性。
2. 查看路由範本中每個 URI 區段。 對於每個區段，排列資料行，如下所示： 

    1. 常值的區段。
    2. 路由參數的條件約束。
    3. 沒有限制路由參數。
    4. 條件約束與萬用字元參數區段。
    5. 萬用字元參數區段沒有限制。
3. 在繫結的情況下，路由會依不區分大小寫的序數字串比較 ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) 的路由範本。

以下是一個範例。 假設您定義下列控制站：

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

這些路由會排序，如下所示。

1. 訂單/詳細資料
2. 訂單 / {id}
3. 訂單 / {customerName}
4. 訂單 / {\*日期}
5. 訂單 / 擱置中

請注意，[詳細資料] 是常值的區段，"{id}"之前出現，但是 「 擱置 」 會顯示上次因為**RouteOrder**屬性為 1。 (這個範例那里假設沒有客戶名為 「 詳細資料 」 或 「 暫止 」。 一般情況下，嘗試避免模稜兩可的路由。 在此範例中，較佳路由範本`GetByCustomer`是 「 客戶 / {customerName}")
