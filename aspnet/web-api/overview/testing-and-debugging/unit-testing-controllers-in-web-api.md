---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "單元測試中 ASP.NET Web API 2 控制器 |Microsoft 文件"
author: MikeWasson
description: "本主題說明一些特定的技術，單元測試中 Web API 2 控制器。 閱讀本主題之前，可能會想要閱讀本教學課程單位..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>單元測試中 ASP.NET Web API 2 控制器
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 本主題說明一些特定的技術，單元測試中 Web API 2 控制器。 閱讀本主題之前，您可能想要閱讀本教學課程[單元測試 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，其中示範如何將單元測試專案加入至您的方案。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> 我使用 Moq，但相同的概念適用於任何模擬的架構。 Moq 4.5.30 （和更新版本） 支援 Visual Studio 2017、 Roslyn 和.NET 4.5 及更新版本。

在單元測試中常見的模式是&quot;排列-act assert&quot;:

- 排列： 設定要執行測試的任何必要條件。
- Act： 執行測試。
- 判斷提示： 確認測試成功。

在排列步驟中，您通常會使用模擬或虛設常式物件。 讓測試著重於測試一件事，可減少相依性的數目。

以下是您在 Web API 控制器應該單元測試的一些事項：

- 此動作會傳回正確的回應類型。
- 無效的參數傳回正確的錯誤回應。
- 動作的儲存機制或服務的圖層上呼叫正確的方法。
- 如果回應包含網域模型，請確認模型類型。

以下是一些一般項目，若要測試，但是細節取決於您控制站的實作。 特別是，它會很大的差異是否控制器動作傳回**HttpResponseMessage**或**IHttpActionResult**。 如需這些結果類型的詳細資訊，請參閱[Web Api 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。

## <a name="testing-actions-that-return-httpresponsemessage"></a>傳回 HttpResponseMessage 的測試動作

以下是其動作傳回的控制站範例**HttpResponseMessage**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

請注意，將插入的控制器會使用的相依性插入`IProductRepository`。 這使控制站更可測試，因為您可以將插入模擬儲存機制。 以下的單元測試會驗證`Get`方法寫入`Product`回應主體。 假設`repository`是模擬`IProductRepository`。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

請務必設定**要求**和**組態**控制站上。 否則，測試將會失敗，並**ArgumentNullException**或**InvalidOperationException**。

## <a name="testing-link-generation"></a>測試連結產生

`Post`方法呼叫**UrlHelper.Link**回應中建立連結。 您需要在單元測試中一些更多的設定：

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper**類別需要要求 URL 和路由資料，因此測試這些設定值。 另一個選項是模擬或虛設常式**UrlHelper**。 您可以使用這個方法，取代預設值[ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx)模擬或虛設常式版本會傳回固定的值。

讓我們重新撰寫使用測試[Moq](https://github.com/Moq)架構。 安裝`Moq`測試專案中的 NuGet 封裝。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

在此版本中，您不需要設定任何路由的資料，因為模擬**UrlHelper**傳回常數字串。


## <a name="testing-actions-that-return-ihttpactionresult"></a>傳回 IHttpActionResult 的測試動作

在 Web API 2 控制器動作，可以傳回**IHttpActionResult**，相當於**ActionResult** ASP.NET MVC 中。 **IHttpActionResult**介面會定義建立的 HTTP 回應的命令模式。 控制站會傳回而不是直接建立回應， **IHttpActionResult**。 更新版本中，管線會叫用**IHttpActionResult**建立回應。 這種方法可讓您更輕鬆地撰寫單元測試，因為您可以略過所需的安裝程式的許多**HttpResponseMessage**。

以下是此範例控制器會執行其動作傳回**IHttpActionResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

這個範例會示範一些常見的模式使用**IHttpActionResult**。 我們來看看如何單位測試它們。

### <a name="action-returns-200-ok-with-a-response-body"></a>動作將會傳回 200 （確定） 回應內文

`Get`方法呼叫`Ok(product)`如果找到的產品。 在單元測試時，請確定傳回的型別是**OkNegotiatedContentResult**且傳回的產品具有權限的識別碼。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

請注意，單元測試不會執行動作結果。 您可以假設在動作結果會建立正確的 HTTP 回應。 (這就是為什麼 Web API 架構有它自己的單元測試 ！)

### <a name="action-returns-404-not-found"></a>動作將會傳回 404 （找不到）

`Get`方法呼叫`NotFound()`如果找不到產品。 此案例中，單元測試只檢查傳回的型別是否**NotFoundResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>動作會傳回 「 200 （確定） 沒有回應主體

`Delete`方法呼叫`Ok()`傳回空的 HTTP 200 回應。 就像先前範例中，單元測試會檢查傳回的型別，在此情況下**OkResult**。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>動作將會傳回 201 （已建立） 的位置標頭

`Post`方法呼叫`CreatedAtRoute`以位置標頭中傳回 HTTP 201 回應的 uri。 在單元測試，確認動作設定正確的路由值。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>動作傳回的回應主體的另一個 2xx

`Put`方法呼叫`Content`傳回回應主體與 HTTP 202 （已接受） 回應。 此情況下傳回 200 （確定），類似，但在單元測試也應該檢查的狀態碼。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>其他資源

- [模擬 Entity Framework 時單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [撰寫測試的 ASP.NET Web API 服務](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)（Youssef Moussaoui 部落格文章）。
- [偵錯 ASP.NET Web API 路由偵錯工具](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
