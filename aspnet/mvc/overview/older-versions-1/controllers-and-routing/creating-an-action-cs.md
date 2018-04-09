---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: 建立動作 (C#) |Microsoft 文件
author: microsoft
description: 了解如何將新的動作加入至 ASP.NET MVC 控制器。 深入了解將動作方法的需求。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-c"></a>建立動作 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何將新的動作加入至 ASP.NET MVC 控制器。 深入了解將動作方法的需求。


本教學課程的目標是要說明如何建立新的控制器動作。 您了解需求的動作方法。 您也了解如何防止公開為動作方法。

## <a name="adding-an-action-to-a-controller"></a>新增動作至控制器

您加入新的動作，您可以控制站將新方法加入至控制器。 例如，列表 1 中的控制站包含名為 index （） 的動作，以及名為 SayHello() 的動作。 這兩種方法會公開為動作。

**列出 1-Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

若要為動作 universe 來公開，方法必須符合特定需求：

- 這個方法必須是公用的。
- 此方法不能是靜態方法。
- 方法不可以擴充方法。
- 方法不能是建構函式、 getter 或 setter。
- 此方法不能有開放式的泛型型別。
- 方法不是方法的控制器的基底類別。
- 方法不能包含**ref**或**出**參數。

請注意，控制器動作的傳回型別沒有限制。 控制器動作，可以傳回字串，是日期時間，或 void Random 類別的執行個體。 ASP.NET MVC 架構會轉換成字串的動作結果不是任何傳回型別，並轉譯至瀏覽器的字串。

當您新增任何符合這些需求的控制站的方法時，做為控制器動作公開的方法。 這裡格外小心。 連線到網際網路的任何人都可以叫用控制器動作。 請勿，例如，建立 DeleteMyWebsite() 控制器動作。

## <a name="preventing-a-public-method-from-being-invoked"></a>防止叫用的公用方法

如果您要在控制器類別中建立的公用方法，而且您不想要公開 （expose） 做為控制器動作方法然後您就可以防止方法叫用使用 [NonAction] 屬性。 例如，列表 2 中的控制站會包含名為 CompanySecrets() [NonAction] 屬性裝飾的公用方法。

**Listing 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

如果您嘗試叫用 CompanySecrets() 控制器動作，您的瀏覽器的網址列中輸入 /Work/CompanySecrets 您會在圖 1 中取得的錯誤訊息。


[![叫用 NonAction 方法](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**圖 01**： 叫用 NonAction 方法 ([按一下以檢視完整大小的影像](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [上一頁](creating-a-controller-cs.md)
> [下一頁](asp-net-mvc-routing-overview-vb.md)
