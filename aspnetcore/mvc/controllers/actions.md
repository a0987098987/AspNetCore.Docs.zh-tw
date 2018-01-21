---
title: "處理要求中 ASP.NET Core MVC 控制站"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: cef493fc2010d1c82e5c1dfec85864539252b817
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a>處理要求中 ASP.NET Core MVC 控制站

由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://github.com/scottaddie)

控制器、 動作和動作結果是開發人員如何建置使用 ASP.NET Core MVC 應用程式的基礎部分。

## <a name="what-is-a-controller"></a>什麼是控制站？

在控制站用來定義和群組的一組動作。 動作 (或*動作方法*) 是一種方法來處理要求的控制器。 控制站以邏輯方式分組類似動作。 此彙總的動作可讓一般組規則，例如路由、 快取，以及授權，統一套用。 要求會對應至動作透過[路由](xref:mvc/controllers/routing)。

依照慣例，控制器類別：
* 位於專案的根層級*控制器*資料夾
* 繼承自`Microsoft.AspNetCore.Mvc.Controller`

在控制站是可具現化類別至少一個下列條件為 true:
* 類別名稱後置字元"Controller"
* 類別繼承自類別，其名稱的後置字元"Controller"
* 類別以裝飾`[Controller]`屬性

控制器類別不具有相關聯`[NonController]`屬性。

控制站應該遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。 有幾種方法可以實作此原則。 如果多個控制器的動作需要在相同的服務，請考慮使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)要求的相依性。 如果只有單一動作方法所需的服務，請考慮使用[動作資料隱碼](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)要求的相依性。

內**M**模型-**V**檢視-**C**ontroller 模式控制站會負責初始要求的處理和具現化的模型。 一般而言，模型內，應執行的商業決策。

控制器使用模型的處理 （如果有的話） 的結果，並傳回適當的檢視和其相關聯的檢視資料或應用程式開發介面呼叫的結果。 進一步了解在[ASP.NET Core MVC 概觀](xref:mvc/overview)和[開始使用 ASP.NET Core MVC 和 Visual Studio](xref:tutorials/first-mvc-app/start-mvc)。

控制器是*UI 層級*抽象概念。 其負責的部分是以確保要求的資料無效，並選擇應傳回哪些檢視 （或應用程式開發介面的結果）。 在構造良好的應用程式，它不會直接包含資料存取或商務邏輯。 相反地，服務處理這些責任委派給控制器。

## <a name="defining-actions"></a>定義動作

控制站上的公用方法，但不包括使用裝飾`[NonAction]`屬性，是動作。 在 [動作] 參數會要求資料繫結，並使用驗證的[模型繫結](xref:mvc/models/model-binding)。 模型驗證就會發生的模型繫結的所有項目。 `ModelState.IsValid`屬性值表示模型繫結和驗證是否成功。

動作方法應該包含邏輯將要求對應至商務問題。 透過存取控制站的服務應該通常表示業務疑慮[相依性插入](xref:mvc/controllers/dependency-injection)。 然後，動作會將商務動作的結果對應至應用程式狀態。

動作可以傳回任何項目，但經常傳回的執行個體`IActionResult`(或`Task<IActionResult>`非同步方法) 產生回應。 動作方法會負責選擇*何種回應*。 在動作結果*未回應*。

### <a name="controller-helper-methods"></a>控制器的 Helper 方法

控制站通常是繼承自[控制器](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)，不過這並非必要。 衍生自`Controller`提供三種 helper 方法的存取：

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1.產生空的回應主體中的方法

否`Content-Type`HTTP 回應標頭都會包括在內，因為回應主體缺少來描述的內容。

此類別內的兩種結果類型： 重新導向與 HTTP 狀態碼。

* **HTTP 狀態碼**

    這種類型會傳回 HTTP 狀態碼。 此類型的一些協助程式方法都是`BadRequest`， `NotFound`，和`Ok`。 例如，`return BadRequest();`產生 400 狀態碼執行時。 當這類方法`BadRequest`， `NotFound`，和`Ok`會多載，它們不會再限定為 HTTP 狀態碼回應，因為執行內容交涉。

* **Redirect**

    這種類型會傳回重新導向到某個動作或目的地 (使用`Redirect`， `LocalRedirect`， `RedirectToAction`，或`RedirectToRoute`)。 例如，`return RedirectToAction("Complete", new {id = 123});`重新導向至`Complete`，傳遞匿名物件。

    重新導向結果型別不同於 HTTP 狀態碼類型中加入主要`Location`HTTP 回應標頭。

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2.導致非空白的回應主體的預先定義的內容類型的方法

此類別中的大多數協助程式方法包括`ContentType`屬性，可讓您設定`Content-Type`來描述回應主體的回應標頭。

此類別內的兩種結果類型：[檢視](xref:mvc/views/overview)和[格式化回應](xref:mvc/models/formatting)。

* **檢視**

    這種類型傳回使用模型來呈現 HTML 的檢視表。 例如，`return View(customer);`將模型傳遞至資料繫結的檢視。

* **格式化的回應**

    這種類型會傳回以特定方式代表物件的 JSON 或類似的資料交換格式。 例如，`return Json(customer);`所提供的物件序列化為 JSON 格式。
    
    此類型的其他常用的方法包括`File`， `PhysicalFile`，和`VirtualFile`。 例如，`return PhysicalFile(customerFilePath, "text/xml");`傳回所描述的 XML 檔案`Content-Type`"text/xml"的回應標頭值。

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3.方法所產生的非空白的回應主體中格式化與用戶端交涉的內容類型

這個類別是更好稱為**內容交涉**。 [內容交涉](xref:mvc/models/formatting#content-negotiation)套用時的動作傳回[ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult)類型或內容以外[IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult)實作。 傳回非動作`IActionResult`實作 (例如， `object`) 也會傳回格式化的回應。

此類型的一些協助程式方法包括`BadRequest`， `CreatedAtRoute`，和`Ok`。 這些方法的範例包括`return BadRequest(modelState);`， `return CreatedAtRoute("routename", values, newobject);`，和`return Ok(value);`分別。 請注意，`BadRequest`和`Ok`執行內容交涉的值傳遞時，才; 而不傳遞一個值，它們而是做為 HTTP 狀態碼的結果型別。 `CreatedAtRoute`方法時，相反地，一律會執行內容交涉，因為所有需要的值將會收到其多載。

### <a name="cross-cutting-concerns"></a>跨領域考量

應用程式通常會共用工作流程的組件。 範例包括應用程式需要驗證存取購物車，或是某些頁面上的資料會快取的應用程式。 若要執行的邏輯，在動作方法的前後，使用*篩選*。 使用[篩選](xref:mvc/controllers/filters)交互碼橫切入顧慮上可以減少重複，讓他們遵循[不重複自行 （乾） 原則](http://deviq.com/don-t-repeat-yourself/)。

最篩選屬性，例如`[Authorize]`，可以套用在控制器或動作層級所需的資料粒度層級而定。

錯誤處理和回應的快取通常會跨領域考量：
   * [錯誤處理](xref:mvc/controllers/filters#exception-filters)
   * [回應快取](xref:performance/caching/response)

可以使用篩選器或自訂處理許多跨領域考量[中介軟體](xref:fundamentals/middleware)。
