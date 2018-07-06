---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: 了解 ASP.NET MVC 執行程序 |Microsoft Docs
author: microsoft
description: 了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 34699c314eb862e91ecba8831c5092af9d47dc70
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823731"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>了解 ASP.NET MVC 執行程序
====================
by [Microsoft](https://github.com/microsoft)

> 了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。


ASP.NET MVC 為基礎的 Web 應用程式的要求先通過**UrlRoutingModule**物件，也就是 HTTP 模組。 此模組會剖析該要求，並執行路由選取。 **UrlRoutingModule**物件會選取符合目前要求的第一個路由物件。 (路由物件是類別可實作**RouteBase**，且通常是的執行個體**路由**類別。)如果沒有路由相符， **UrlRoutingModule**物件不執行任何動作，並讓要求回到標準 ASP.NET 或 IIS 要求處理。

從所選**路由**物件， **UrlRoutingModule**物件取得**IRouteHandler**相關聯的物件**路由**物件。 一般而言，在 MVC 應用程式，這會是的執行個體**MvcRouteHandler**。 **IRouteHandler**執行個體會建立**IHttpHandler**物件，並將它傳遞**IHttpContext**物件。 根據預設， **IHttpHandler**執行個體為 MVC **MvcHandler**物件。 **MvcHandler**物件接著會選取最後會處理要求的控制器。

> [!NOTE]
> 在 IIS 7.0 中，執行的 ASP.NET MVC Web 應用程式，無副檔名時需要的 MVC 專案。 不過，在 IIS 6.0 中，處理常式需要您將.mvc 副檔名對應至 ASP.NET ISAPI DLL。


模組與處理常式是 ASP.NET MVC 架構的進入點。 執行下列動作：

- MVC Web 應用程式中選取適當的控制器。
- 取得特定控制器執行個體。
- 呼叫控制器的**Execute**方法。

以下列出 MVC Web 專案執行階段：

- 接收應用程式的第一個要求 

    - 在 Global.asax 檔案中，**路由**物件加入至**RouteTable**物件。
- 執行路由 

    - **UrlRoutingModule**模組使用的第一個符合**路由**物件**RouteTable**來建立集合**RouteData**物件，然後使用建立該**RequestContext** (**IHttpContext**) 物件。
- 建立 MVC 要求處理常式 

    - **MvcRouteHandler**物件建立的執行個體**MvcHandler**類別，並將它傳遞**RequestContext**執行個體。
- 建立控制器 

    - **MvcHandler**物件會使用**RequestContext**執行個體，以識別**IControllerFactory**物件 (通常是的執行個體**DefaultControllerFactory**類別) 來建立控制器執行個體。
- 執行控制器- **MvcHandler**執行個體會呼叫控制器 s **Execute**方法。 |
- 叫用動作 

    - 大部分的控制站會繼承**控制器**基底類別。 控制站，這樣做，請**ControllerActionInvoker**與控制器相關聯的物件決定的動作方法的呼叫，控制器類別，然後呼叫該方法。
- 執行結果 

    - 典型的動作方法可能會接收使用者輸入、 準備適當的回應資料，並藉由傳回的結果型別執行結果。 可以執行的內建結果型別包括下列： **ViewResult** （其中將檢視呈現並是最常用的結果型別）， **RedirectToRouteResult**， **RedirectResult**， **ContentResult**， **JsonResult**，和**EmptyResult**。
