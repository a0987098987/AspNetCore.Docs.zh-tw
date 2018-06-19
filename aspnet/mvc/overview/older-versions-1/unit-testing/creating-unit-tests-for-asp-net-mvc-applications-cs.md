---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: 建立單元測試的 ASP.NET MVC 應用程式 (C#) |Microsoft 文件
author: StephenWalther
description: 了解如何建立適用於控制器動作的單元測試。 在本教學課程中，作者： Stephen Walther 會示範如何測試是否控制器動作傳回 parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: ccd9a1b3aee8379c23c01c5eb7f756a786f6359d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869708"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>建立單元測試的 ASP.NET MVC 應用程式 (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

[下載 PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> 了解如何建立適用於控制器動作的單元測試。 在本教學課程中，作者： Stephen Walther 會示範如何測試控制器動作傳回的特定檢視，傳回一組特定的資料，或傳回不同類型的動作結果。


本教學課程的目標是為了示範如何撰寫單元測試的控制站在 ASP.NET MVC 應用程式。 我們會討論如何建立單元測試的三種不同的類型。 您了解如何測試控制器動作傳回的檢視、 測試控制器動作，所傳回的檢視資料以及如何測試一個控制器動作將您重新導向至第二個控制器動作。

## <a name="creating-the-controller-under-test"></a>建立測試控制器

讓我們開始建立我們想要測試的控制器。 具名的控制站`ProductController`，包含在程式碼範例 1。

**列出 1 – `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

`ProductController`包含兩個動作方法，名為`Index()`和`Details()`。 這兩個動作方法傳回的檢視。 請注意，`Details()`動作接受參數，名為識別碼。

## <a name="testing-the-view-returned-by-a-controller"></a>測試檢視傳回的控制站

假設我們想要測試是否`ProductController`傳回右方檢視。 我們想要確定當`ProductController.Details()`叫用動作時，會傳回詳細資料檢視。 列出 2 中的測試類別包含單元測試來測試所傳回的檢視`ProductController.Details()`動作。

**列出 2 – `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

列出 2 中的類別包含名為的測試方法`TestDetailsView()`。 這個方法包含三行程式碼。 第一行程式碼建立的新執行個體`ProductController`類別。 程式碼的第二行叫用控制器的`Details()`動作方法。 最後，程式碼會檢查檢視是否傳回的最後一行`Details()`動作是詳細資料檢視。

`ViewResult.ViewName`屬性表示檢視傳回的控制站的名稱。 關於測試這個屬性的其中一個大警告。 有兩種方式，控制站可以傳回的檢視。 控制器可以明確傳回檢視就像這樣：

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

或者，可以推斷檢視的名稱，從控制器動作，就像這樣的名稱：

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

此控制器動作也會傳回名為檢視`Details`。 不過，檢視的名稱被推斷的動作名稱。 如果您想要測試的檢視名稱，然後您必須明確地傳回檢視表名稱從控制器動作。

您也可以輸入鍵盤組合清單 2 中執行單元測試**Ctrl-R、 A**或按一下**方案中執行所有測試**按鈕 （請參閱圖 1）。 如果測試成功，您會看到在圖 2 中的測試結果 視窗。


[![在方案中執行所有測試](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**圖 01**： 方案中執行所有測試 ([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**圖 02**： 成功 ！ ([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>測試檢視資料傳回的控制站

此 MVC 控制器會將資料傳遞至檢視使用所謂*`View Data`*。 例如，假設您想要顯示特定產品的詳細資料，當您叫用`ProductController Details()`動作。 在此情況下，您可以建立的執行個體`Product`類別 （定義於您的模型），並傳遞要的執行個體`Details`檢視藉由運用`View Data`。

修改`ProductController`中列出的 3 包含更新`Details()`傳回產品的動作。

**列出 3 – `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

首先，`Details()`動作建立的新執行個體`Product`表示膝上型電腦類別。 下一步，執行個體`Product`類別會當做第二個參數傳遞`View()`方法。

您可以撰寫單元測試來測試是否為預期的資料包含在檢視中的資料。 單元測試中列出的 4 測試是否會傳回代表膝上型電腦的產品，當您呼叫`ProductController Details()`動作方法。

**列出 4 – `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

列出第 4`TestDetailsView()`方法會測試所叫用傳回的檢視資料`Details()`方法。 `ViewData`上公開為屬性`ViewResult`所叫用傳回`Details()`方法。 `ViewData.Model`屬性包含傳遞至檢視的產品。 測試只會驗證檢視資料中包含產品具有膝上型電腦的名稱。

## <a name="testing-the-action-result-returned-by-a-controller"></a>測試動作結果傳回的控制站

更複雜的控制器動作可能會傳回不同類型的動作結果取決於值傳遞至控制器動作的參數。 控制器動作，可以傳回各種類型的動作結果，包括`ViewResult`， `RedirectToRouteResult`，或`JsonResult`。

例如，已修改`Details()`列出 5 中的動作傳回`Details`檢視當您將有效的產品識別碼傳遞至動作。 如果您傳遞無效的產品識別碼-識別碼的值小於 1 部--就會重新導向至`Index()`動作。

**列出 5 – `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

您可以測試的行為`Details()`與單元測試程式碼範例 6 中的動作。 列出第 6 單元測試會驗證您會重新導向至`Index`檢視時的識別碼值為-1 會傳遞至`Details()`方法。

**列出 6 – `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

當您呼叫`RedirectToAction()`方法控制器動作中的，控制器動作傳回`RedirectToRouteResult`。 測試檢查是否`RedirectToRouteResult`至名為控制器動作會將使用者重新導向`Index`。

## <a name="summary"></a>總結

在本教學課程中，您將學會如何建立單元測試的 MVC 控制器的動作。 首先，您學會了如何以確認控制器動作是否傳回右方檢視。 您已學習如何使用`ViewResult.ViewName`屬性，確認為檢視的名稱。

接下來，我們檢驗如何測試的內容`View Data`。 您已學會如何以檢查是否已傳回正確的產品`View Data`之後呼叫控制器動作。

最後，我們將討論如何測試是否從控制器動作傳回不同類型的動作結果。 您已學習如何測試是否為控制站傳回`ViewResult`或`RedirectToRouteResult`。

> [!div class="step-by-step"]
> [下一步](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
