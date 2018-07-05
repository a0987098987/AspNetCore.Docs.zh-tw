---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: 建立自訂路由 (C#) |Microsoft Docs
author: microsoft
description: 了解如何將自訂的路由新增至 ASP.NET MVC 應用程式。 在本教學課程中，您將了解如何修改 Global.asax 檔案中的預設路由表。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bb694b08d595b2ce75123c3da0e9b8e8d60a652
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399949"
---
<a name="creating-custom-routes-c"></a>建立自訂路由 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何將自訂的路由新增至 ASP.NET MVC 應用程式。 在本教學課程中，您將了解如何修改 Global.asax 檔案中的預設路由表。


在本教學課程中，您將了解如何將自訂的路由新增至 ASP.NET MVC 應用程式。 您了解如何修改預設的路由表，在 Global.asax 檔案中，以自訂路由。

對於許多簡單的 ASP.NET MVC 應用程式，預設路由表將正常運作。 不過，您可能會發現您專用的路由需求。 在此情況下，您可以建立自訂路由。

例如，假設您要建置的部落格應用程式。 您可能想要處理傳入的要求看起來像這樣：

/ 封存/12-25-2009

當使用者輸入此要求時，您想要的日期傳回與相對應的部落格項目 2009 年 12 月 25 日。 為了處理這種類型的要求，您需要建立自訂路由。

列表 1 中的 Global.asax 檔包含新的自訂路由，名為部落格，哪一個處理要求，看起來像 /Archive/*項目日期*。

**列表 1-Global.asax （具有自訂路由）**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

您將新增至路由表路由的順序很重要。 現有的預設路由前面會加上我們新的自訂部落格路由。 如果您反轉順序，則預設路由一律會取得呼叫而不是自訂的路由。

自訂的部落格路由比對開頭為/封存/任何要求。 因此，它會符合所有下列 Url:

- / 封存/12-25-2009

- 日封存/10-6-2004 年

- / 封存/apple

自訂的路由將內送的要求對應至名為 Archive 的控制站，並叫用 Entry() 動作。 Entry() 方法呼叫時，就會將項目日期傳遞做為參數命名為 entryDate 中。

您可以使用列表 2 中的控制站的部落格的自訂路由。

**列表 2-ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

請注意，在 列表 2 Entry() 方法接受 DateTime 類型的參數。 MVC 架構是夠聰明，無法從 URL 中將項目日期轉換成 DateTime 值時，自動的。 如果 URL 中的項目日期參數無法轉換成日期時間，就會引發錯誤 （請參閱 圖 1）。

**圖 1-轉換參數時發生錯誤**


[![[新增專案] 對話方塊](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**圖 01**： 轉換參數時發生錯誤 ([按一下以檢視完整大小的影像](creating-custom-routes-cs/_static/image2.png))


## <a name="summary"></a>總結

本教學課程的目標是要示範如何建立自訂路由。 您已了解如何將自訂路由加入至 Global.asax 檔案表示部落格文章中的路由表。 我們會討論如何將要求的部落格項目對應至名為 ArchiveController 控制器和名為 Entry() 控制器動作。

> [!div class="step-by-step"]
> [上一頁](aspnet-mvc-controllers-overview-cs.md)
> [下一頁](creating-a-route-constraint-cs.md)
