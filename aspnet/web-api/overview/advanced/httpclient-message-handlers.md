---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API 的 HttpClient 訊息處理常式 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506787"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API 的 HttpClient 訊息處理常式
====================
由[Mike Wasson](https://github.com/MikeWasson)

A*訊息處理常式*是接收 HTTP 要求，並傳回 HTTP 回應的類別。

一般而言，一系列的訊息處理常式會鏈結在一起。 第一個處理常式會接收 HTTP 要求，執行一些處理，並讓下一個處理常式要求。 在某些時候，回應會建立，並會回到鏈結。 此模式呼叫*委派*處理常式。

![](httpclient-message-handlers/_static/image1.png)

在用戶端， **HttpClient**類別使用的訊息處理常式來處理要求。 預設處理常式是**HttpClientHandler**，其透過網路傳送要求，並從伺服器取得回應。 您可以插入自訂訊息處理常式到管線的用戶端：

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API 也會使用伺服器端的訊息處理常式。 如需詳細資訊，請參閱[HTTP 訊息處理常式](http-message-handlers.md)。


## <a name="custom-message-handlers"></a>自訂訊息處理常式

若要撰寫的自訂訊息處理常式，衍生自**System.Net.Http.DelegatingHandler**並覆寫**SendAsync**方法。 以下是方法簽章：

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

這個方法會接受**HttpRequestMessage**做為輸入，並以非同步方式傳回**HttpResponseMessage**。 一般實作會執行下列動作：

1. 處理要求訊息。
2. 呼叫`base.SendAsync`將要求傳送至內部處理常式。
3. 內部處理常式會傳回回應訊息。 （此步驟是非同步的）。
4. 處理回應，並將它傳回給呼叫者。

下列範例會示範將自訂標頭加入至傳出要求的訊息處理常式：

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

若要呼叫`base.SendAsync`為非同步。 如果此處理常式沒有任何工作在這個呼叫之後，使用**await**方法完成之後繼續執行至關鍵字。 下列範例會記錄錯誤代碼的處理常式。 記錄本身不是非常有趣，但此範例示範如何取得在處理常式內部的回應。

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>加入至用戶端管線的訊息處理常式

若要加入自訂的處理常式來**HttpClient**，使用**HttpClientFactory.Create**方法：

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

訊息處理常式會呼叫您傳遞到順序**建立**方法。 因為處理常式為巢狀，回應訊息會傳送另一方向。 也就是最後一個處理常式是最先收到回應訊息。
