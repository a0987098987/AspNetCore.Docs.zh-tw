---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: 建立 ASP.NET MVC 應用程式 (VB) 的單元測試 |Microsoft Docs
author: StephenWalther
description: 了解如何建立適用於控制器動作的單元測試。 在本教學課程中，Stephen Walther 會示範如何測試控制器動作傳回 parti...
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 3a8f142063a7addc8acca2b1a515295e36b26e29
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837178"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>建立單元測試的 ASP.NET MVC 應用程式 (VB)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

[下載 PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> 了解如何建立適用於控制器動作的單元測試。 在本教學課程中，Stephen Walther 會示範如何將測試控制器動作傳回的特定檢視，傳回一組特定的資料，或傳回不同類型的動作結果。


本教學課程的目的在於示範如何撰寫單元測試的控制站在您的 ASP.NET MVC 應用程式。 我們會討論如何建置三種不同的單元測試。 您將了解如何在測試控制器動作所傳回之檢視、 如何在測試控制器動作，所傳回的檢視資料以及如何測試或有一個控制器動作會將您導向第二個控制器動作。

## <a name="creating-the-controller-under-test"></a>建立測試控制器

現在就開始建立我們想要測試的控制器。 控制器中，名為`ProductController`，包含在程式碼範例 1。

**列表 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

`ProductController`包含兩個動作方法，名為`Index()`和`Details()`。 這兩個動作方法傳回的檢視。 請注意，`Details()`動作接受參數，名為識別碼。

## <a name="testing-the-view-returned-by-a-controller"></a>測試檢視傳回的控制站

假設我們想要測試是否`ProductController`傳回正確的檢視。 我們想要確定，當`ProductController.Details()`叫用動作時，會傳回詳細資料檢視。 在 列表 2 中的測試類別包含測試所傳回之檢視的單元測試`ProductController.Details()`動作。

**列表 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

列表 2 中的類別包含名為測試方法`TestDetailsView()`。 這個方法包含三行程式碼。 第一行程式碼建立的新執行個體`ProductController`類別。 第二行程式碼會叫用控制器的`Details()`動作方法。 最後，程式碼檢查檢視是否傳回的最後一行`Details()`動作是 詳細資料檢視。

`ViewResult.ViewName`屬性代表傳回的控制站之檢視的名稱。 測試此屬性相關的一個重要警告。 有兩種方式，控制站可以傳回的檢視。 控制器可以明確地傳回的檢視，像這樣：

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

或者，從控制器動作，就像這樣的名稱，可以推斷檢視的名稱：

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

此控制器動作也會傳回名為檢視`Details`。 不過，檢視的名稱是從動作名稱來推斷。 如果您想要測試的檢視名稱，然後您必須明確地傳回檢視表名稱從控制器動作。

您也可以輸入鍵盤組合在列表 2 中執行單元測試**CTRL-R、 A**或按一下**執行方案中的所有測試**按鈕 （請參閱 圖 1）。 如果測試成功，您會看到 [圖 2] 中的 [測試結果] 視窗。


[![在方案中執行所有測試](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**圖 01**： 在方案中執行所有測試 ([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![成功 ！](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**圖 02**： 成功 ！ ([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>測試檢視資料傳回的控制站

此 MVC 控制器會將資料傳遞至檢視使用所謂*`View Data`*。 例如，假設您想要顯示特定產品的詳細資料，當您叫用`ProductController Details()`動作。 在此情況下，您可以在其中建立的執行個體`Product`（在模型中定義） 的類別，並傳遞要的執行個體`Details`利用檢視`View Data`。

已修改`ProductController`包含 列表 3 中的 更新`Details()`傳回產品的動作。

**列表 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

首先，`Details()`動作會建立的新執行個體`Product`類別，表示在膝上型電腦。 接下來，執行個體`Product`做為第二個參數來傳遞類別`View()`方法。

您可以撰寫單元測試來測試是否為預期的資料包含在檢視中的資料。 單元測試列表 4 測試中，當您呼叫時，會傳回代表膝上型電腦的產品是否`ProductController Details()`動作方法。

**列表 4 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

在 [列表 4]`TestDetailsView()`方法會測試所叫用傳回的檢視資料`Details()`方法。 `ViewData`上公開為屬性`ViewResult`藉由叫用傳回`Details()`方法。 `ViewData.Model`屬性包含傳遞至檢視的產品。 測試只會確認產品檢視資料中包含的膝上型電腦的名稱。

## <a name="testing-the-action-result-returned-by-a-controller"></a>測試動作結果傳回的控制站

更複雜的控制器動作可能會傳回不同類型的動作取決於值中的結果傳遞至控制器動作的參數。 控制器動作可以傳回各種類型的動作結果，包括`ViewResult`， `RedirectToRouteResult`，或`JsonResult`。

例如，已修改`Details()`表 5 中的動作傳回`Details`檢視當您將有效的產品識別碼傳遞至動作。 如果您傳遞無效的產品識別碼，識別碼的值小於 1，則您會重新導向至`Index()`動作。

**列表 5 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

您可以測試的行為`Details()`與單元測試列表 6 中的動作。 在 列表 6 中的單元測試驗證，將您重新導向`Index`檢視識別碼為-1 的值傳遞至`Details()`方法。

**列表 6 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

當您呼叫`RedirectToAction()`方法的控制器動作中，控制器動作傳回`RedirectToRouteResult`。 測試檢查是否`RedirectToRouteResult`至名為控制器動作會將使用者重新導向`Index`。

## <a name="summary"></a>總結

在本教學課程中，您已了解如何建置適用於 MVC 控制器動作的單元測試。 首先，您已了解如何確認正確的檢視由控制器動作。 您已了解如何使用`ViewResult.ViewName`屬性，確認檢視的名稱。

接下來，我們檢查如何測試的內容`View Data`。 您已了解如何檢查是否已傳回正確的產品`View Data`之後呼叫控制器動作。

最後，我們會討論如何測試是否從控制器動作傳回不同類型的動作結果。 您已了解如何測試是否會傳回一個控制站`ViewResult`或`RedirectToRouteResult`。

> [!div class="step-by-step"]
> [上一步](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
