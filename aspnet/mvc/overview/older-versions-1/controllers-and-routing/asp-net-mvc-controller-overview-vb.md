---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: ASP.NET MVC 控制器概觀 (VB) |Microsoft 文件
author: StephenWalther
description: 在本教學課程中，作者： Stephen Walther 向您介紹 ASP.NET MVC 控制器。 您了解如何建立新的控制站，並傳回不同類型的動作 res...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a45630e8f2d5ae0548bb6b8496df08ca5877a40
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868980"
---
<a name="aspnet-mvc-controller-overview-vb"></a>ASP.NET MVC 控制器概觀 (VB)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 在本教學課程中，作者： Stephen Walther 向您介紹 ASP.NET MVC 控制器。 您了解如何建立新的控制站，並傳回不同類型的動作結果。


本教學課程中，瀏覽 ASP.NET MVC 控制器、 控制器動作和動作結果的主題。 完成本教學課程之後，您將了解如何控制站用來控制與 ASP.NET MVC 網站的訪客互動的方式。

## <a name="understanding-controllers"></a>了解控制器

MVC 控制器負責回應對 ASP.NET MVC 網站提出的要求。 每個瀏覽器要求對應到特定的控制站。 例如，假設您的瀏覽器的網址列輸入下列 URL:

`http://localhost/Product/Index/3`

在此情況下，會叫用名為 ProductController 的控制站。 ProductController 是負責產生瀏覽器要求的回應。 例如，控制器可能會傳回至瀏覽器的特定檢視或控制站可能會將使用者重新導向至另一個控制器。

清單 1 包含名為 ProductController 簡單控制器。

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

您可以看到列出 1，控制站是只類別 （Visual Basic.NET 或 C# 類別）。 控制站是衍生自基底 System.Web.Mvc.Controller 類別的類別。 由於控制器都繼承自這個基底類別，控制站可免費繼承數種有用方法 （我們討論這些方法在不久後）。

## <a name="understanding-controller-actions"></a>了解控制器動作

控制器會公開控制器的動作。 動作是當您在瀏覽器位址列中輸入特定的 URL 時所呼叫的控制站上的方法。 例如，假設您提出要求，下列 url:

`http://localhost/Product/Index/3`

在此情況下，index （） 方法 ProductController 類別上呼叫。 Index （） 方法是控制器動作的範例。

控制器動作，必須是公用方法的控制器類別。 根據預設，visual basic.net 方法都是公用的方法。 了解的任何公用方法，您將加入至控制器類別會自動公開為控制器動作 （您必須小心這因為控制器動作，可以叫用宇宙中的任何人只要在瀏覽器網址列中輸入正確的 URL）。

有一些必須滿足的控制器動作的其他需求。 做為控制器動作方法無法多載。 此外，控制器動作不能是靜態方法。 除此之外，您可以使用幾乎任何方法做為控制器動作。

## <a name="understanding-action-results"></a>了解動作結果

控制器動作傳回所謂*動作結果*。 動作結果會是在瀏覽器要求的回應中傳回的控制器動作。

ASP.NET MVC 架構支援數種類型的動作結果，包括：

1. ViewResult-代表 HTML 和標記。
2. EmptyResult-表示沒有結果。
3. RedirectResult-代表重新導向至新的 URL。
4. JsonResult-表示可用於 AJAX 應用程式的 JavaScript 物件標記法結果。
5. JavaScriptResult-代表 JavaScript 指令碼。
6. ContentResult-表示文字結果。
7. FileContentResult-代表可下載檔案 （具有二進位內容）。
8. FilePathResult-表示可下載檔案 （路徑）。
9. FileStreamResult-代表可下載檔案 （使用檔案資料流）。

所有這些動作會繼承自基底 ActionResult 類別。

在大部分情況下，控制器動作傳回 ViewResult。 例如，列出 2 中的 Index 控制器動作傳回 ViewResult。

**Listing 2 - Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

當動作傳回 ViewResult 時，HTML 會傳回至瀏覽器。 列出 2 中的 index （） 方法會傳回名為 Index 至瀏覽器的檢視。

請注意，index （） 中的動作清單 2 不會傳回 ViewResult()。 相反地，會呼叫控制器的基底類別 View() 方法。 一般來說，您不會傳回的動作結果直接。 相反地，您呼叫其中一個控制器的基底類別的下列方法：

1. 檢視-傳回 ViewResult 動作結果。
2. 重新導向-傳回 RedirectResult 動作結果。
3. RedirectToAction-傳回 RedirectToRouteResult 動作結果。
4. RedirectToRoute-傳回 RedirectToRouteResult 動作結果。
5. Json-傳回 JsonResult 動作結果。
6. JavaScriptResult-傳回 JavaScriptResult。
7. 內容-傳回 ContentResult 動作結果。
8. 檔案-FileContentResult、 FilePathResult 或 FileStreamResult 根據的參數傳遞給方法的傳回。

因此，如果您想要傳回至瀏覽器的檢視，您可以呼叫 View() 方法。 如果您想要重新導向至另一個控制器動作的使用者，您會呼叫 RedirectToAction() 方法。 例如，Details() 動作中列出的 3 顯示的檢視，或是將使用者導向至 index （） 動作，根據是否 Id 參數的值。

**列出 3-CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

ContentResult 動作結果是特殊的。 您可以使用 ContentResult 動作結果傳回成純文字的動作結果。 比方說，在列出的 4 index （） 方法會傳回訊息以純文字，而不是 HTML。

**列出 4-Controllers\StatusController.vb**

> StatusController
> 
> 
> System.Web.Mvc.Controller


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

叫用 StatusController.Index() 動作時，不會傳回檢視表。 相反地，未經處理的文字"Hello World ！" 會傳回給瀏覽器。

如果控制器動作傳回的結果是不是動作結果-例如，日期或整數-然後結果會包裝在 ContentResult 自動。 例如，叫用的列表 5 中 WorkController index 動作時，日期會當成 ContentResult 自動。

**列出 5-WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

Index （） 中的動作清單 5 會傳回 DateTime 物件。 ASP.NET MVC 架構將 DateTime 物件轉換為字串和日期時間值中 ContentResult 會自動換行。 瀏覽器接收的日期和時間以純文字。

## <a name="summary"></a>總結

本教學課程的目的是為您介紹的概念 ASP.NET MVC 控制器、 控制器動作，以及控制器動作結果。 在第一個區段中，您將學會如何將新的控制站新增至 ASP.NET MVC 專案。 接下來，您學到如何公用方法，控制站都會公開至 universe 當做控制器的動作。 最後，我們將討論不同類型的可從控制器動作傳回的動作結果。 特別是，我們將討論如何從控制器動作傳回 ViewResult、 RedirectToActionResult 和 ContentResult。

> [!div class="step-by-step"]
> [上一頁](creating-a-custom-route-constraint-cs.md)
> [下一頁](creating-custom-routes-vb.md)
