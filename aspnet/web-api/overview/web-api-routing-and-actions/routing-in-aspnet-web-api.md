---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API 中的路由 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207299"
---
<a name="routing-in-aspnet-web-api"></a>ASP.NET Web API 中的路由
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

這篇文章說明 ASP.NET Web API 將 HTTP 要求路由至控制器的方式。

> [!NOTE]
> 如果您熟悉 ASP.NET MVC、 Web API 路由是非常類似於 MVC 路由。 主要差異在於，Web API 會使用 HTTP 指令動詞，而不是 URI 路徑，，在選取的動作。 您也可以使用 Web API 中的 MVC 樣式路由。 這篇文章不採用任何 ASP.NET MVC 的知識。

## <a name="routing-tables"></a>路由表

在 ASP.NET Web API 中，*控制器*是處理 HTTP 要求的類別。 控制站的公用方法會呼叫*動作方法*簡稱*動作*。 當 Web API 架構會收到要求時，它會將要求路由至動作。

若要判斷要叫用動作，架構會使用*路由表*。 Web API 的 Visual Studio 專案範本會建立預設路由：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

此路由定義於*WebApiConfig.cs*檔案，其會置於*應用程式\_啟動*目錄：

![](routing-in-aspnet-web-api/_static/image1.png)

如需詳細資訊`WebApiConfig`類別，請參閱[設定 ASP.NET Web API](../advanced/configuring-aspnet-web-api.md)。

如果您自我裝載 Web API，您必須直接上設定的路由表`HttpSelfHostConfiguration`物件。 如需詳細資訊，請參閱 <<c0> [ 自我裝載 Web API](../older-versions/self-host-a-web-api.md)。

路由表中的每個項目包含*路由範本*。 Web API 的預設路由範本&quot;api / {controller} / {id}&quot;。 此範本中， &quot;api&quot;是常值路徑區段和 {controller} 和 {id} 是預留位置變數。

當 Web API 架構會收到 HTTP 要求時，它會嘗試比對針對其中一個路由表中的路由範本的 URI。 如果沒有路由相符，用戶端會收到 404 錯誤。 例如，下列 Uri 會符合預設路由：

- / api/連絡人
- /api/contacts/1
- /api/products/gizmo1

不過，下列 URI 不符合，因為它缺少&quot;api&quot;區段：

- 連絡人/1

> [!NOTE]
> 使用路由中的 「 api 」 的原因是避免與 ASP.NET MVC routing 發生衝突。 如此一來，您可以有&quot;/連絡&quot;移至 MVC 控制器，並&quot;/api/連絡人&quot;移至 Web API 控制器。 當然，如果您不喜歡這個慣例，您可以變更預設路由表。

一旦找到相符的路由，Web API 會選取控制器和動作：

- 若要尋找控制器，將 Web API&quot;控制器&quot;的值 *{controller}* 變數。
- 若要尋找此動作，Web API 會查看 HTTP 動詞命令，並接著會尋找名稱開頭為該 HTTP 指令動詞名稱的動作。 例如，透過 GET 要求，Web API 會尋找動作前面加上&quot;取得&quot;，例如&quot;GetContact&quot;或是&quot;GetAllContacts&quot;。 這個慣例僅適用於 GET、 POST、 PUT、 DELETE、 HEAD、 選項和 PATCH 動詞命令。 您可以使用屬性控制站上，以啟用其他 HTTP 動詞命令。 我們稍後所見的範例。
- 其他的預留位置變數在路由範本中，這類 *{id}，* 會對應至動作參數。

讓我們看看一個範例。 假設您定義下列控制器：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

以下是一些可能的 HTTP 要求，以及每個叫用的動作：

| HTTP 指令動詞 | URI 的路徑 | 動作 | 參數 |
| --- | --- | --- | --- |
| GET | api/產品 | GetAllProducts | *（無）* |
| GET | api/產品/4 | GetProductById | 4 |
| DELETE | api/產品/4 | DeleteProduct | 4 |
| POST | api/產品 | *（沒有相符項目）* |  |

請注意， *{id}* 區段的 URI，如果有的話，會對應至*識別碼*動作參數。 在此範例中，控制器會定義兩個 GET 方法，一個具有*識別碼*參數和一個不含任何參數。

另外，請注意，POST 要求將會失敗，因為控制器不會定義&quot;文章...&quot;方法。

## <a name="routing-variations"></a>路由的變化

上一節所述的基本 ASP.NET Web API 路由機制。 本章節描述一些變化。

### <a name="http-verbs"></a>HTTP 指令動詞

而不是使用 HTTP 動詞命令的命名慣例，您可以裝飾具有下列屬性的其中一個動作方法，明確指定動作的 HTTP 動詞命令：

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

在下列範例中，`FindProduct`方法會對應至 GET 要求：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

若要允許多個 HTTP 動詞命令動作，或允許 GET、 PUT、 POST、 DELETE、 HEAD、 選項和修補程式以外的 HTTP 指令動詞，請使用`[AcceptVerbs]`屬性，根據 HTTP 指令動詞的清單。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>路由動作名稱

這個預設路由範本中，Web API 會使用 HTTP 動詞命令，來選取的動作。 不過，您也可以建立的路由，其中包含 URI 中的動作名稱：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

此路由範本中， *{action}* 參數名稱控制站上的動作方法。 使用這種路由方法，使用屬性來指定允許的 HTTP 動詞命令。 例如，假設您的控制器具有下列方法：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

在此情況下，"api/產品/詳細資料/1"的 GET 要求會對應至`Details`方法。 這種路由樣式類似於 ASP.NET MVC，並可能是適當的 RPC 樣式 API。

您可以使用覆寫動作名稱`[ActionName]`屬性。 在下列範例中，有兩個動作對應至&quot;縮圖 api/產品 / /*識別碼*。其中一個支援 GET，另支援文章：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>非動作

若要防止方法叫用動作，請使用`[NonAction]`屬性。 這表示 framework 方法不是動作，即使否則找到相符的路由規則。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>進一步閱讀

本主題提供路由的高層級檢視。 如需詳細資訊，請參閱 <<c0> [ 路由和動作選取](routing-and-action-selection.md)，其中描述完全如何架構符合路由的 URI、 選取控制器，然後選取要叫用動作。
