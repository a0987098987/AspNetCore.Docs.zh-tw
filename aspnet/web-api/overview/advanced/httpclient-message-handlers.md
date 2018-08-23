---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API 中的 HttpClient 訊息處理常式 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 764244d1299d8cfcb59c3f15d63b42ebff4f6ac0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831841"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API 中的 HttpClient 訊息處理常式
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

A*訊息處理常式*是接收 HTTP 要求，並傳回 HTTP 回應的類別。

一般而言，一系列的訊息處理常式鏈結在一起。 第一個處理常式會接收 HTTP 要求、 執行一些處理、 和讓下一個處理常式的要求。 在某些時候，回應會建立，並會回到鏈結。 這個模式稱為*委派*處理常式。

![](httpclient-message-handlers/_static/image1.png)

在用戶端**HttpClient**類別使用的訊息處理常式來處理要求。 預設處理常式**HttpClientHandler**，這會透過網路傳送要求，並從伺服器取得回應。 您可以將自訂訊息處理常式插入用戶端管線：

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API 也會使用伺服器端的訊息處理常式。 如需詳細資訊，請參閱 < [HTTP 訊息處理常式](http-message-handlers.md)。


## <a name="custom-message-handlers"></a>自訂訊息處理常式

若要撰寫的自訂訊息處理常式，衍生自**System.Net.Http.DelegatingHandler** ，並覆寫**SendAsync**方法。 以下是方法簽章：

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

此方法會採用**HttpRequestMessage**做為輸入，並以非同步方式傳回**HttpResponseMessage**。 一般實作會執行下列作業：

1. 處理要求訊息。
2. 呼叫`base.SendAsync`將要求傳送至內部處理常式。
3. 內部處理常式會傳回回應訊息。 （此步驟是非同步的）。
4. 處理回應，並將它傳回給呼叫者。

下列範例示範將自訂標頭新增至傳出要求的訊息處理常式：

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

若要呼叫`base.SendAsync`是非同步的。 如果處理常式會執行任何工作，此呼叫之後，使用**await**關鍵字，在方法完成之後繼續執行。 下列範例示範會記錄錯誤代碼的處理常式。 記錄本身不太妙，但此範例示範如何取得在處理常式內回應。

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>將訊息處理常式新增至用戶端管線

若要加入自訂的處理常式來**HttpClient**，使用**HttpClientFactory.Create**方法：

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

您傳遞它們的順序呼叫訊息處理常式**建立**方法。 因為處理常式為巢狀，回應訊息會傳送另一個方向。 也就是最後一個處理常式會是第一個取得回應訊息。
