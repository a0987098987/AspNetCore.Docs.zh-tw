---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API 中的路由 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a>ASP.NET Web API 中的路由
====================
由[Mike Wasson](https://github.com/MikeWasson)

本文說明 ASP.NET Web API 將 HTTP 要求路由到控制器的方式。

> [!NOTE]
> 如果您已熟悉 ASP.NET MVC、 Web API 路由是非常類似於 MVC 路由。 主要差異在於，Web API 所使用的 HTTP 方法，而不是 URI 路徑，來選取的動作。 您也可以使用 Web 應用程式開發介面中的路由，MVC 樣式。 這篇文章不會採用了解 ASP.NET MVC。


## <a name="routing-tables"></a>路由表

在 ASP.NET Web API 中，*控制器*是用來處理 HTTP 要求的類別。 公用方法的控制器會呼叫*動作方法*或只是*動作*。 Web API framework 收到要求時，它會要求路由至的動作。

若要判斷何種動作要叫用，架構會使用*路由表*。 Web 應用程式開發介面的 Visual Studio 專案範本會建立預設路由：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

此路由在 WebApiConfig.cs 檔案中，放在應用程式定義\_開始目錄：

![](routing-in-aspnet-web-api/_static/image1.png)

如需有關**到 WebApiConfig**類別，請參閱[設定 ASP.NET Web API](../advanced/configuring-aspnet-web-api.md)。

如果您自行裝載 Web 應用程式開發介面，您必須直接在設定路由表**HttpSelfHostConfiguration**物件。 如需詳細資訊，請參閱[自我裝載的 Web API](../older-versions/self-host-a-web-api.md)。

路由表中的每個項目包含*路由範本*。 Web 應用程式開發介面的預設路由範本是&quot;api / {controller} / {id}&quot;。 此範本中&quot;api&quot;是常值路徑區段和 {控制器} 和 {id} 是預留位置變數。

Web API framework 收到 HTTP 要求時，它會嘗試比對針對其中一個路由表中的路由範本的 URI。 如果沒有路由相符，用戶端收到 404 錯誤。 例如，下列 Uri 會符合預設路由：

- / api/連絡人
- /api/contacts/1
- /api/products/gizmo1

不過，下列 URI 不符合，因為其欠缺&quot;api&quot;區段：

- / 連絡人/1

> [!NOTE]
> 在路由中使用 「 應用程式開發介面 」 的原因是避免與 ASP.NET MVC routing 發生衝突。 這樣一來，您可以讓&quot;/連絡人&quot;移至 MVC 控制站和&quot;/api/連絡人&quot;移至 Web API 控制器。 當然，如果您不喜歡此慣例，您可以變更預設路由表。

一旦找到相符的路由，Web API 會選取控制器和動作：

- 若要尋找控制器，將 Web API&quot;控制器&quot;值 *{控制器}* 變數。
- 若要尋找此動作，Web API HTTP 方法，並接著會尋找名稱開頭之 HTTP 方法名稱的動作。 例如，透過 GET 要求，Web API 會尋找開頭的動作&quot;取得...&quot;，例如&quot;GetContact&quot;或&quot;GetAllContacts&quot;。 這個慣例僅適用於 GET、 POST、 PUT 和 DELETE 方法。 您可以在控制器上使用屬性來啟用其他 HTTP 方法。 我們稍後就會看到的範例。
- 其他的預留位置變數在路由範本中，例如 *{id}，* 對應至動作參數。

讓我們來看一個範例。 假設您定義下列控制站：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

以下是一些可能的 HTTP 要求，以及每個叫用的動作：

| HTTP 方法 | URI 的路徑 | 動作 | 參數 |
| --- | --- | --- | --- |
| GET | 應用程式開發介面/產品 | GetAllProducts | *（無）* |
| GET | 應用程式開發介面/產品/4 | GetProductById | 4 |
| DELETE | 應用程式開發介面/產品/4 | DeleteProduct | 4 |
| POST | 應用程式開發介面/產品 | *（沒有相符項目）* |  |

請注意， *{id}* 區段的 URI，如果存在，會對應到*識別碼*動作參數。 在此範例中，控制器會定義兩個 GET 方法，其中一個有*識別碼*參數和一個不含任何參數。

另外，請注意，POST 要求將會失敗，因為未定義控制器&quot;Post...&quot;方法。

## <a name="routing-variations"></a>路由的變化

上一節所述 ASP.NET Web API 的基本路由的機制。 本章節描述一些變化。

### <a name="http-methods"></a>HTTP 方法

而不是使用 HTTP 方法的命名慣例，您可以明確指定動作的 HTTP 方法而將動作方法，以**HttpGet**， **HttpPut**， **HttpPost**，或**HttpDelete**屬性。

在下列範例中，FindProduct 方法會對應至 GET 要求：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

若要允許多個 HTTP 方法的動作，或允許 GET、 PUT、 POST 和 DELETE 以外的 HTTP 方法，使用**AcceptVerbs**屬性，根據 HTTP 方法的清單。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>路由動作名稱

使用預設路由範本中，Web API 所使用的 HTTP 方法來選取的動作。 不過，您也可以建立的路由，其中動作名稱會包含在 URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

此路由範本中 *{action}* 參數名稱控制站上的動作方法。 這種樣式的路由，以使用屬性來指定允許的 HTTP 方法。 例如，假設您的控制器有下列方法：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

在此情況下，「 應用程式開發介面/產品/詳細資料/1"的 GET 要求會將對應至詳細資料的方法。 這種樣式的路由類似於 ASP.NET MVC 中，而且可能適用於 RPC 樣式應用程式開發介面。

您可以使用覆寫動作名稱**ActionName**屬性。 在下列範例中，有兩個動作對應至&quot;縮圖 api/產品/*識別碼*。其中一個支援 GET，但其他支援 POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>非動作

若要防止方法取得叫用的動作，請使用**NonAction**屬性。 這會通知架構方法不是動作，即使否則找到相符的路由規則。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>進一步閱讀

本主題提供路由的高層級檢視。 如需詳細資訊，請參閱[路由和動作選取](routing-and-action-selection.md)，用來描述完全如何架構比對路由的 URI，選取控制器，，然後選取要叫用動作。
