---
title: 在 ASP.NET Core MVC 中處理控制器要求
author: ardalis
description: ''
ms.author: riande
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/controllers/actions
ms.openlocfilehash: b7c4d61c4a71939e84bdea180a2f77b6438b15d5
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774193"
---
# <a name="handle-requests-with-controllers-in-aspnet-core-mvc"></a>在 ASP.NET Core MVC 中處理控制器要求

作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://github.com/scottaddie)

控制器、動作和動作結果是開發人員使用 ASP.NET Core MVC 建置應用程式的基礎部分。

## <a name="what-is-a-controller"></a>什麼是控制器？

控制器是用來定義和分組一組動作。 動作 (或「動作方法」**) 是控制器上處理要求的方法。 控制器會以邏輯方式將類似動作群組在一起。 這項動作彙總允許統一套用一組通用規則 (例如路由、快取和授權)。 要求會透過[路由](xref:mvc/controllers/routing)對應至動作。

依照慣例，控制器類別：

* 位於專案的根層級*控制器*資料夾中。
* 繼承自 `Microsoft.AspNetCore.Mvc.Controller`。

控制器是至少下列其中一個條件為 true 的可具現化類別：

* 類別名稱尾碼為`Controller`。
* 類別繼承自名稱尾碼為的類別`Controller`。
* `[Controller]`屬性會套用至類別。

控制器類別不得具有相關聯的 `[NonController]` 屬性。

控制器應該遵循[明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。 有幾種方法可以實作此準則。 如果多個控制器動作需要相同的服務，請考慮使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)來要求這些相依性。 如果只有單一動作方法需要服務，請考慮使用[動作插入](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)來要求相依性。

在**模型檢視控制器********** 模式內，控制器負責初始處理要求和模型具現化。 一般而言，應該在模型內執行商業決策。

控制器會使用模型處理結果 (如果有的話)，並傳回適當檢視和其相關聯的檢視資料或 API 呼叫結果。 深入了解 [ASP.NET Core MVC 概觀](xref:mvc/overview)和 [ASP.NET Core MVC 與 Visual Studio 使用者入門](xref:tutorials/first-mvc-app/start-mvc)。

控制器是「UI 層級」** 抽象概念。 其負責確保要求資料有效，並選擇應該傳回的檢視 (或 API 的結果)。 在構造良好的應用程式中，不會直接包括資料存取或商務邏輯。 相反地，控制器會委派給處理這些責任的服務。

## <a name="defining-actions"></a>定義動作

控制器上的`[NonAction]`公用方法（屬性除外）是動作。 動作上的參數會繫結至要求資料，並使用[模型繫結](xref:mvc/models/model-binding)進行驗證。 繫結模型的所有項目都會進行模型驗證。 `ModelState.IsValid` 屬性值指出模型繫結和驗證是否成功。

動作方法應該包含將要求對應至商務關注的邏輯。 商務關注應該一般呈現為控制器透過[相依性插入](xref:mvc/controllers/dependency-injection)存取的服務。 動作接著會將商務動作的結果對應至應用程式狀態。

動作可以傳回任何項目，但經常會傳回可產生回應的 `IActionResult` 執行個體 (或適用於非同步方法的 `Task<IActionResult>`)。 動作方法負責選擇「回應類型」**。 動作結果「會執行對應項目」**。

### <a name="controller-helper-methods"></a>控制器協助程式方法

控制器通常繼承自[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller)，但這不是必要的。 衍生自 `Controller` 提供對三種類別的協助程式方法的存取：

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. 產生空白回應主體的方法

因為回應本文缺少要描述的內容，所以未包括 `Content-Type` HTTP 回應標頭。

此類別內有兩種結果類型：「重新導向」和「HTTP 狀態碼」。

* **HTTP 狀態碼**

    此類型會傳回 HTTP 狀態碼。 此類型的一些協助程式方法是 `BadRequest`、`NotFound` 和 `Ok`。 例如，`return BadRequest();` 在執行時會產生 400 狀態碼。 `BadRequest`、`NotFound` 和 `Ok` 這類方法進行多載時，就不再適合作為 HTTP 狀態碼回應程式，因為會執行內容交涉。

* **導向**

    此類型會傳回到某個動作或目的地的重新導向 (使用 `Redirect`、`LocalRedirect`、`RedirectToAction` 或 `RedirectToRoute`)。 例如，`return RedirectToAction("Complete", new {id = 123});` 會重新導向至 `Complete`，並傳遞匿名物件。

    「重新導向」結果類型不同於主要用來新增 `Location` HTTP 回應標頭的「HTTP 狀態碼」類型。

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. 以預先定義的內容類型產生非空白回應主體的方法

此類別中的大多數協助程式方法包括 `ContentType` 屬性，可讓您設定 `Content-Type` 回應標頭來描述回應本文。

此類別內有兩種結果類型：[檢視](xref:mvc/views/overview)和[格式化回應](xref:web-api/advanced/formatting)。

* **檢視**

    此類型會傳回使用模型來轉譯 HTML 的檢視。 例如，`return View(customer);` 會將模型傳遞至資料繫結的檢視。

* **格式化回應**

    此類型會傳回 JSON 或類似的資料交換格式，以特定方式來代表物件。 例如，`return Json(customer);` 會將所提供的物件序列化為 JSON 格式。
    
    此類型的其他通用方法包括 `File` 與 `PhysicalFile`。 例如，`return PhysicalFile(customerFilePath, "text/xml");` 會傳回 [PhysicalFileResult](/dotnet/api/microsoft.aspnetcore.mvc.physicalfileresult)。

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. 在與用戶端協商的內容類型中格式化為非空白回應主體的方法

此類別普遍稱為**內容交涉**。 只要動作傳回 [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) 類型或 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) 實作以外的某個項目，就會套用[內容交涉](xref:web-api/advanced/formatting#content-negotiation)。 傳回非 `IActionResult` 實作的動作 (例如，`object`) 也會傳回「格式化回應」。

此類型的一些協助程式方法包括 `BadRequest`、`CreatedAtRoute` 和 `Ok`。 這些方法的範例分別包括 `return BadRequest(modelState);`、`return CreatedAtRoute("routename", values, newobject);` 和 `return Ok(value);`。 請注意，只有在傳遞值時，`BadRequest` 和 `Ok` 才會執行內容交涉；如果未傳遞值，則會改成作為「HTTP 狀態碼」結果類型。 相反地，`CreatedAtRoute` 方法一律會執行內容交涉，因為其多載全部都需要傳遞值。

### <a name="cross-cutting-concerns"></a>跨領域關注

應用程式通常會共用其工作流程的各部分。 範例包括需要驗證購物車存取權的應用程式，或快取某些頁面上資料的應用程式。 若要在動作方法之前或之後執行邏輯，請使用 *filter*。 在交叉關注上使用[篩選](xref:mvc/controllers/filters)可減少重複。

大部分的篩選屬性 (例如 `[Authorize]`) 可以套用至控制器或動作層級 (視所需的細微性層級而定)。

錯誤處理和回應快取通常是跨領域關注：
* [處理錯誤](xref:mvc/controllers/filters#exception-filters)
* [回應快取](xref:performance/caching/response)

許多跨領域關注都可以使用篩選或自訂[中介軟體](xref:fundamentals/middleware/index)進行處理。
