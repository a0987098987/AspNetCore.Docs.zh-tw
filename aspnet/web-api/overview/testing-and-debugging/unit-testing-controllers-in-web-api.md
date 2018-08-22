---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: 單元測試 ASP.NET Web API 2 中的控制站 |Microsoft Docs
author: MikeWasson
description: 本主題描述一些單元測試控制器，Web API 2 中的特定技術。 閱讀本主題之前，您可能想要閱讀本教學課程單位...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7b0d5266757219a05b25fc3d1d4cba8514a4dff7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827011"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>單元測試 ASP.NET Web API 2 中的控制站
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 本主題描述一些單元測試控制器，Web API 2 中的特定技術。 閱讀本主題之前，您可能想要閱讀本教學課程[單元測試 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，其中顯示如何將單元測試專案新增至您的解決方案。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> 我使用 Moq，但相同的概念適用於任何模擬的架構。 Moq 4.5.30 （和更新版本） 支援 Visual Studio 2017，Roslyn 及.NET 4.5 和更新版本。

在單元測試中的常見模式是&quot;排列-act assert&quot;:

- 排列： 設定要執行測試的任何必要條件。
- Act： 執行測試。
- 判斷提示： 確認測試成功。

在排列步驟中，您會經常使用模擬或虛設常式物件。 可以降低相依性，讓測試著重於測試一件事。

以下是一些您應該在 Web API 控制器中使用單元測試的事項：

- 此動作會傳回正確的回應類型。
- 無效的參數傳回正確的錯誤回應。
- 動作會呼叫正確的方法，在存放庫或服務層級。
- 如果回應包含的領域模型，請確認模型類型。

以下是一些一般項目，若要測試，但細節取決於您的控制器實作。 特別是，它沒有太大差別控制器動作傳回是否**HttpResponseMessage**或是**IHttpActionResult**。 如需這些結果類型的詳細資訊，請參閱[Web Api 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。

## <a name="testing-actions-that-return-httpresponsemessage"></a>傳回 HttpResponseMessage 的測試動作

以下是其動作傳回的控制站的範例**HttpResponseMessage**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

請注意，控制器會使用相依性插入來插入`IProductRepository`。 這可讓控制器進行測試，因為您可以插入模擬儲存機制。 下列的單元測試會驗證`Get`方法寫入`Product`與回應本文。 假設`repository`是 mock `IProductRepository`。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

請務必設定**要求**並**組態**控制站上。 否則，測試將會失敗，並**ArgumentNullException**或是**InvalidOperationException**。

## <a name="testing-link-generation"></a>測試連結產生

`Post`方法呼叫**UrlHelper.Link**建立在回應中的連結。 這需要一些更多的設定，在單元測試：

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper**類別需要要求 URL 和路由資料，因此測試這些設定值。 另一個選項是模擬或虛設常式**UrlHelper**。 您可以使用此方法時，取代預設值[ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx)模擬或虛設常式版本會傳回固定的值。

讓我們重新撰寫測試方法[Moq](https://github.com/Moq) framework。 安裝`Moq`在測試專案的 NuGet 套件。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

在此版本中，您不需要設定任何路由資料中，因為 mock **UrlHelper**傳回常數字串。


## <a name="testing-actions-that-return-ihttpactionresult"></a>傳回 IHttpActionResult 的測試動作

在 Web API 2 中，控制器動作可以傳回**IHttpActionResult**，這相當於**ActionResult** ASP.NET MVC 中。 **IHttpActionResult**介面會定義建立 HTTP 回應的命令模式。 而不是直接建立回應，控制器會傳回**IHttpActionResult**。 更新版本中，管線會叫用**IHttpActionResult**建立回應。 這種方法可讓您更輕鬆地撰寫單元測試，因為您可以略過安裝程式所需的大量**HttpResponseMessage**。

以下是其動作傳回的範例控制器**IHttpActionResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

此範例示範一些常見的模式，使用**IHttpActionResult**。 我們來看看如何進行單元測試它們。

### <a name="action-returns-200-ok-with-a-response-body"></a>動作會傳回 200 （確定） 回應內文

`Get`方法呼叫`Ok(product)`如果找到的產品。 在單元測試時，請確定傳回的型別是**OkNegotiatedContentResult**和傳回的產品都有正確的識別碼。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

請注意，單元測試不會執行動作結果。 您可以假設動作結果正確建立 HTTP 回應。 (這就是為什麼 Web API 架構有其自己的單元測試 ！)

### <a name="action-returns-404-not-found"></a>動作會傳回 404 （找不到）

`Get`方法呼叫`NotFound()`如果找不到產品。 在此情況下，單元測試只檢查傳回的型別是否**NotFoundResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>動作會傳回 200 （確定） 沒有回應本文

`Delete`方法呼叫`Ok()`傳回空的 HTTP 200 回應。 如上述範例中，單元測試會檢查傳回的型別，在此情況下**OkResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>動作會傳回 201 （已建立） 的位置標頭

`Post`方法呼叫`CreatedAtRoute`Location 標頭中傳回 HTTP 201 的回應的 uri。 在單元測試時，確認此動作會設定正確的路由值。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>動作會傳回另一個 2xx 回應內文

`Put`方法呼叫`Content`傳回回應主體與 HTTP 202 （已接受） 回應。 此案例類似於傳回 200 （確定），但單元測試也應該檢查的狀態碼。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>其他資源

- [模擬 Entity Framework 時的單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [撰寫測試的 ASP.NET Web API 服務](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)（Youssef Moussaoui 部落格文章）。
- [偵錯 ASP.NET Web API 路由偵錯工具](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
