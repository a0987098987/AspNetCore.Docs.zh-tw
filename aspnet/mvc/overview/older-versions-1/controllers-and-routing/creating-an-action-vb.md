---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: 建立動作 (VB) |Microsoft Docs
author: microsoft
description: 了解如何將新的動作新增至 ASP.NET MVC 控制器。 深入了解將動作方法的需求。
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 6ec69f5aa9f7a789cb533a8dc04dca952adf35d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824042"
---
<a name="creating-an-action-vb"></a>建立動作 (VB)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何將新的動作新增至 ASP.NET MVC 控制器。 深入了解將動作方法的需求。


本教學課程的目標是要說明如何建立新的控制器動作。 您了解動作方法的需求。 您也了解如何防止公開做為動作方法。

## <a name="adding-an-action-to-a-controller"></a>將動作新增至控制器

您新增新的動作，您可以控制站的新方法新增至控制器。 例如，列表 1 中的控制器包含名為 index （） 的動作，以及名為 SayHello() 的動作。 這兩種方法都會公開為動作。

**列表 1-Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

若要公開給 universe 做為動作，方法必須符合特定需求：

- 這個方法必須是公用的。
- 此方法不能是靜態方法。
- 方法無法擴充方法。
- 方法不可為建構函式、 getter 或 setter。
- 此方法不能有開放式的泛型型別。
- 此方法不是控制器的基底類別的方法。
- 此方法不能包含**ref**或是**出**參數。

請注意，控制器動作的傳回型別不受任何限制。 控制器動作可以傳回的字串、 日期時間，或 void Random 類別的執行個體。 ASP.NET MVC 架構會轉換成字串的動作結果不是任何傳回型別，並呈現至瀏覽器的字串。

當您新增不違反這些需求，來控制站的任何方法時，則會將方法公開為控制器動作。 只能小心為妙。 可以叫用控制器動作，任何人連線到網際網路。 ，例如，建立 DeleteMyWebsite() 控制器動作。

## <a name="preventing-a-public-method-from-being-invoked"></a>阻止叫用的公用方法

如果您要在控制器類別中建立的公用方法，而且您不想要公開為控制器動作方法則可以防止方法藉由叫用&lt;NonAction&gt;屬性。 例如，列表 2 中的控制器包含公用方法，名為裝飾的 CompanySecrets() &lt;NonAction&gt;屬性。

**列表 2-Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

如果您嘗試叫用 CompanySecrets() 控制器動作，在您的瀏覽器的網址列中輸入 /Work/CompanySecrets 您會在 圖 1 中收到錯誤訊息。


[![叫用 NonAction 方法](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**圖 01**： 叫用 NonAction 方法 ([按一下以檢視完整大小的影像](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [上一頁](creating-a-controller-vb.md)
> [下一頁](aspnet-mvc-controllers-overview-cs.md)
