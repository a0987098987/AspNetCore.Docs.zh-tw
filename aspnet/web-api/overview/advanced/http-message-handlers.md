---
uid: web-api/overview/advanced/http-message-handlers
title: ASP.NET Web API 中的 HTTP 訊息處理常式 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 0b0d7b4c543dc4e597c6c472083898f3a8095a83
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834290"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API 中的 HTTP 訊息處理常式
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

A*訊息處理常式*是接收 HTTP 要求，並傳回 HTTP 回應的類別。 訊息處理常式衍生自抽象**HttpMessageHandler**類別。

一般而言，一系列的訊息處理常式鏈結在一起。 第一個處理常式會接收 HTTP 要求、 執行一些處理、 和讓下一個處理常式的要求。 在某些時候，回應會建立，並會回到鏈結。 這個模式稱為*委派*處理常式。

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>伺服器端訊息處理常式

伺服器端上的 Web API 管線會使用一些內建的訊息處理常式：

- **HttpServer**從主機取得要求。
- **HttpRoutingDispatcher**分派根據路由的要求。
- **HttpControllerDispatcher**傳送要求到 Web API 控制器。

您可以將自訂處理常式新增至管線。 訊息處理常式，適合用於在 HTTP 訊息 （而非控制器動作） 的層級運作的跨領域關注。 例如，可能會將訊息處理常式：

- 讀取或修改要求標頭。
- 回應標頭加入回應。
- 到達控制器之前，請驗證要求。

下圖顯示兩個自訂的處理常式插入管線：

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> 在用戶端，HttpClient 也會使用訊息處理常式。 如需詳細資訊，請參閱 < [HttpClient 訊息處理常式](httpclient-message-handlers.md)。


## <a name="custom-message-handlers"></a>自訂訊息處理常式

若要撰寫的自訂訊息處理常式，衍生自**System.Net.Http.DelegatingHandler** ，並覆寫**SendAsync**方法。 此方法具有下列簽章：

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

此方法會採用**HttpRequestMessage**做為輸入，並以非同步方式傳回**HttpResponseMessage**。 一般實作會執行下列作業：

1. 處理要求訊息。
2. 呼叫`base.SendAsync`將要求傳送至內部處理常式。
3. 內部處理常式會傳回回應訊息。 （此步驟是非同步的）。
4. 處理回應，並將它傳回給呼叫者。

以下是簡單的範例：

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> 若要呼叫`base.SendAsync`是非同步的。 如果處理常式會執行任何工作，此呼叫之後，使用**await**關鍵字，如所示。


委派的處理常式可以也略過內部處理常式，並直接建立回應：

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

如果委派處理常式會建立的回應，而不需呼叫`base.SendAsync`，要求會略過其餘的管線。 這可用於驗證要求 （會產生錯誤回應） 的處理常式。

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>將處理常式新增至管線

若要新增伺服器端的訊息處理常式，加入處理常式**HttpConfiguration.MessageHandlers**集合。 如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 範本建立專案時，您可以執行這個內部**WebApiConfig**類別：

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

它們會出現在同一個順序呼叫訊息處理常式**MessageHandlers**集合。 是巢狀，回應訊息會傳送另一個方向。 也就是最後一個處理常式會是第一個取得回應訊息。

請注意，您不需要設定內部的處理常式，Web API 架構會自動連線的訊息處理常式。

您若是[自我裝載](../older-versions/self-host-a-web-api.md)，建立的執行個體**HttpSelfHostConfiguration**類別，並加入處理常式來**MessageHandlers**集合。

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

現在讓我們看看一些自訂的訊息處理常式的範例。

## <a name="example-x-http-method-override"></a>範例： X-HTTP-方法-覆寫

X-HTTP-方法-覆寫為非標準的 HTTP 標頭。 它是無法傳送特定 HTTP 要求類型，例如 PUT 或 DELETE 的用戶端。 相反地，用戶端傳送 POST 要求，並設定 X-HTTP-方法-覆寫標頭的所需的方法。 例如: 

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

以下是新增 X-HTTP-方法-覆寫支援的訊息處理常式：

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

在  **SendAsync**方法，處理常式會檢查要求訊息是否為 POST 要求，以及是否包含 X-HTTP-方法-覆寫標頭。 如果是的話，它會驗證標頭的值，並接著修改的要求方法。 最後，處理常式會呼叫`base.SendAsync`來將訊息傳遞至下一個處理常式。

當要求到達**HttpControllerDispatcher**類別**HttpControllerDispatcher**根據更新的要求方法要求會路由傳送。

## <a name="example-adding-a-custom-response-header"></a>範例： 新增自訂的回應標頭

以下是將自訂標頭加入至每個回應訊息的訊息處理常式：

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

首先，處理常式會呼叫`base.SendAsync`將要求傳遞至內部訊息處理常式。 內部處理常式會傳回回應訊息，但它會以非同步的方式使用**任務&lt;T&gt;** 物件。 回應訊息之前不會提供`base.SendAsync`以非同步方式完成。

這個範例會使用**await**關鍵字來執行工作以非同步方式之後`SendAsync`完成。 如果您的目標.NET Framework 4.0，使用**任務**&lt;T&gt;**。ContinueWith**方法：

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>範例： 檢查 API 金鑰

有些 web 服務要求用戶端在要求中包含 API 金鑰。 下列範例顯示的訊息處理常式如何檢查有效的 API 金鑰的要求：

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

這個處理常式會尋找在 URI 查詢字串中的 API 金鑰。 （此範例中，我們假設的索引鍵是靜態字串。 實際的實作可能會使用更複雜的驗證。）如果查詢字串包含索引鍵，這個處理常式會要求傳遞至內部處理常式。

如果要求沒有有效的金鑰，這個處理常式建立回應訊息的狀態為 403 禁止。 不在此情況下，不會呼叫處理常式`base.SendAsync`，因此內部處理常式永遠不會接收要求時，也不會控制站。 因此，控制器可以假設所有的連入要求都有有效的 API 金鑰。

> [!NOTE]
> 如果 API 金鑰只套用至特定控制器的動作，請考慮使用動作篩選條件，而不訊息處理常式。 URI 的路由會執行之後，就會執行動作篩選條件。


## <a name="per-route-message-handlers"></a>每個路由的訊息處理常式

中的處理常式**HttpConfiguration.MessageHandlers**集合全域套用。

或者，您可以加入訊息處理常式至特定的路由，當您定義的路由：

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

在此範例中，如果在要求 URI 比對"Route2 」，該要求分派至`MessageHandler2`。 下圖顯示這兩個路由的管線：

![](http-message-handlers/_static/image4.png)

請注意，`MessageHandler2`來取代預設值**HttpControllerDispatcher**。 在此範例中，`MessageHandler2`建立回應，並要求的符合"Route2 」 永遠不會移到控制站。 這可讓您的整個 Web API 控制器機制取代您自己的自訂端點。

或者，每個路由的訊息處理常式可以委派給**HttpControllerDispatcher**，後者接著會分派至控制器。

![](http-message-handlers/_static/image5.png)

下列程式碼示範如何設定此路由：

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
