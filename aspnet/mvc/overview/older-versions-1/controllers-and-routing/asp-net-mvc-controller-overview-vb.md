---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: ASP.NET MVC 控制器概觀 (VB) |Microsoft Docs
author: StephenWalther
description: 在本教學課程中，Stephen walther 將向您介紹 ASP.NET MVC 控制站。 您了解如何建立新的控制站，並傳回不同類型的動作資源...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ac5e9242f494b8472e582bc76a6f4805db2f770f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809219"
---
<a name="aspnet-mvc-controller-overview-vb"></a>ASP.NET MVC 控制器概觀 (VB)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> 在本教學課程中，Stephen walther 將向您介紹 ASP.NET MVC 控制站。 您了解如何建立新的控制站，並傳回不同類型的動作結果。


本教學課程將探討 ASP.NET MVC 控制器、 控制器動作和動作結果的主題。 完成本教學課程之後，您將了解如何使用控制器來控制與 ASP.NET MVC 網站的訪客互動的方式。

## <a name="understanding-controllers"></a>了解控制器

MVC 控制器會負責回應對 ASP.NET MVC 網站提出的要求。 每個瀏覽器要求對應至特定的控制器。 例如，假設您的瀏覽器的網址列輸入下列 URL:

`http://localhost/Product/Index/3`

在此情況下，會叫用的控制器命名為 ProductController。 ProductController 負責產生瀏覽器要求的回應。 例如，控制器可能會傳回至瀏覽器傳回特定的檢視或控制器可能會將使用者重新導向至另一個控制器。

列表 1 包含名為 ProductController 的簡單控制器。

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

您可以看到從列表 1 中，控制器是只類別 （Visual Basic.NET 或 C# 類別）。 控制器是衍生自基底 system.web.mvc.controller 衍生類別的類別。 因為控制器會繼承自此基底類別，控制站可免費繼承幾個有用的方法 （我們這些方法於稍後討論）。

## <a name="understanding-controller-actions"></a>了解控制器動作

控制器會顯示控制器的動作。 動作是當您在瀏覽器位址列中輸入特定的 URL 時所呼叫的控制站上的方法。 例如，假設您提出要求，下列 url:

`http://localhost/Product/Index/3`

在此情況下，index （） 方法被名 ProductController 類別上。 Index （） 方法是控制器動作的範例。

控制器動作必須是控制器類別的公用方法。 Visual Basic.NET 方法，根據預設，會是公用的方法。 了解您將新增至控制器類別的任何公用方法會自動公開為控制器動作 （您必須非常小心一點因為控制器動作可以叫用 universe 中的任何人只要瀏覽器網址列中輸入正確的 URL）。

有一些額外的需求必須滿足的控制器動作。 無法多載方法，做為控制器動作。 此外，控制器動作不能是靜態的方法。 除此之外，您可以使用幾乎任何方法，做為控制器動作。

## <a name="understanding-action-results"></a>了解動作結果

控制器動作傳回所謂*動作結果*。 動作結果是在瀏覽器要求的回應中傳回的控制器動作。

ASP.NET MVC 架構支援數種類型的動作結果，包括：

1. ViewResult-代表 HTML 和標記。
2. EmptyResult-表示沒有結果。
3. RedirectResult-代表重新導向至新的 URL。
4. JsonResult-表示可用在 AJAX 應用程式的 JavaScript 物件標記法結果。
5. JavaScriptResult-表示 JavaScript 指令碼。
6. ContentResult-表示文字結果。
7. FileContentResult-表示的可下載檔案 （二進位內容）。
8. FilePathResult-代表可下載檔案 （含有路徑）。
9. FileStreamResult-代表可下載檔案 （使用檔案資料流）。

所有這些動作結果會繼承基底的 ActionResult 類別。

在大部分情況下，控制器動作傳回 ViewResult。 比方說，在 列表 2 中的索引控制器動作傳回 ViewResult。

**列表 2-Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

當動作傳回 ViewResult 時，HTML 會傳回至瀏覽器中。 列表 2 中的 index （） 方法會傳回名為 Index 至瀏覽器的檢視。

請注意，在 列表 2 中的 index （） 動作不會傳回 ViewResult()。 相反地，View() 控制器的基底類別會呼叫方法。 一般來說，您不會傳回的動作結果直接。 相反地，您呼叫其中一個控制器的基底類別的下列方法：

1. 檢視-傳回 ViewResult 動作結果。
2. 重新導向-傳回 RedirectResult 動作結果。
3. RedirectToAction-傳回 RedirectToRouteResult 動作結果。
4. RedirectToRoute-傳回 RedirectToRouteResult 動作結果。
5. Json-傳回 JsonResult 動作結果。
6. JavaScriptResult-傳回 JavaScriptResult。
7. 內容-傳回 ContentResult 動作結果。
8. 檔案-FileContentResult、 FilePathResult 或根據參數 FileStreamResult 傳遞給方法的傳回。

因此，如果您想要傳回至瀏覽器的檢視，您會呼叫 View() 方法。 如果您想要重新導向到另一個控制器動作的使用者，您會呼叫 RedirectToAction() 方法。 比方說，Details() 動作列表 3 中的顯示的檢視，或是將使用者重新導向至 index （） 動作，取決於是否 Id 參數的值。

**列表 3-CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

ContentResult 動作結果是特殊的。 您可以使用 ContentResult 動作結果傳回為純文字的動作結果。 比方說，在 列表 4 中的 index （） 方法會傳回訊息以純文字，而不是 HTML。

**列表 4-Controllers\StatusController.vb**

> StatusController
> 
> 
> System.Web.Mvc.Controller


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

叫用 StatusController.Index() 動作時，不會傳回檢視。 相反地，未經處理的文字"Hello World ！" 會傳回至瀏覽器。

如果控制器動作傳回的結果是沒有的動作結果-例如，日期或整數-然後結果會包裝在 ContentResult 自動。 例如，叫用 WorkController 表 5 中的 index （） 動作時，日期會傳回 ContentResult 為自動。

**列表 5-WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

表 5 中的 index （） 動作會傳回 DateTime 物件。 ASP.NET MVC 架構會將 DateTime 物件轉換為字串和日期時間值中 ContentResult 會自動換行。 瀏覽器接收的日期和時間，以純文字。

## <a name="summary"></a>總結

本教學課程的目的是要為您介紹 ASP.NET MVC 控制器、 控制器動作和控制器動作結果的概念。 在第一個區段中，您已了解如何將新的控制站新增至 ASP.NET MVC 專案。 接下來，您已了解如何公用控制器方法會公開至 universe 當做控制器動作。 最後，我們會討論不同類型的可從控制器動作傳回的動作結果。 特別是，我們會討論如何從控制器動作傳回 ViewResult、 RedirectToActionResult 和 ContentResult。

> [!div class="step-by-step"]
> [上一頁](creating-a-custom-route-constraint-cs.md)
> [下一頁](creating-custom-routes-vb.md)
