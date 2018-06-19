---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: 了解 ASP.NET MVC 執行程序 |Microsoft 文件
author: microsoft
description: 了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500727"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>了解 ASP.NET MVC 執行程序
====================
由[Microsoft](https://github.com/microsoft)

> 了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。


對 ASP.NET MVC 為基礎的 Web 應用程式要求先通過**UrlRoutingModule**物件，它是 HTTP 模組。 此模組會剖析該要求，並執行路由選取。 **UrlRoutingModule**物件會選取符合目前要求的第一個路由物件。 (路由物件是實作的類別**RouteBase**，且通常是的執行個體**路由**類別。)如果沒有路由相符， **UrlRoutingModule**物件不做任何動作，並讓要求回到標準 ASP.NET 或 IIS 要求處理。

從所選取**路由**物件**UrlRoutingModule**物件取得**IRouteHandler**與其相關聯物件**路由**物件。 一般來說，在 MVC 應用程式，這會是的執行個體**MvcRouteHandler**。 **IRouteHandler**執行個體建立**IHttpHandler**物件，並將它傳遞至**IHttpContext**物件。 根據預設， **IHttpHandler** MVC 是執行個體**MvcHandler**物件。 **MvcHandler**物件接著會選取最後會處理要求的控制站。

> [!NOTE]
> ASP.NET MVC Web 應用程式執行時在 IIS 7.0 中，沒有名稱的副檔名是必要的 MVC 專案。 不過，在 IIS 6.0 中，處理常式需要您將.mvc 副檔名對應至 ASP.NET ISAPI DLL。


模組與處理常式是 ASP.NET MVC 架構的進入點。 使用者執行下列動作：

- 在 MVC Web 應用程式中選取適當的控制器。
- 取得特定控制器執行個體。
- 呼叫控制器的**Execute**方法。

以下列出 MVC Web 專案的執行階段：

- 接收應用程式的第一個要求 

    - 在 Global.asax 檔案中，**路由**物件加入至**RouteTable**物件。
- 執行路由 

    - **UrlRoutingModule**模組會使用第一個符合**路由**物件存放至**RouteTable**集合，以建立**RouteData**物件，然後用來建立**RequestContext** (**IHttpContext**) 物件。
- 建立 MVC 要求處理常式 

    - **MvcRouteHandler**物件建立的執行個體**MvcHandler**類別，並將它傳遞至**RequestContext**執行個體。
- 建立控制站 

    - **MvcHandler**物件會使用**RequestContext**執行個體來識別**IControllerFactory**物件 (通常的執行個體**DefaultControllerFactory**類別) 來建立控制器執行個體。
- 執行控制器- **MvcHandler**執行個體會呼叫控制器 s **Execute**方法。 |
- 叫用動作 

    - 大部分的控制站繼承自**控制器**基底類別。 這樣做，請的控制站的**ControllerActionInvoker**與控制器相關聯的物件決定的動作方法的呼叫，控制器類別，然後呼叫該方法。
- 執行結果 

    - 典型的動作方法可能會收到使用者輸入、 準備適當的回應資料，並藉由傳回的結果型別執行結果。 可以執行內建的結果類型包括下列： **ViewResult** （其中呈現檢視，是最常用的結果型別）， **RedirectToRouteResult**， **RedirectResult**， **ContentResult**， **JsonResult**，和**EmptyResult**。
