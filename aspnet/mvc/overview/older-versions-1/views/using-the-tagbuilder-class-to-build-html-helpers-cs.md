---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: "使用 TagBuilder 類別來建立 HTML Helper (C#) |Microsoft 文件"
author: StephenWalther
description: "作者： Stephen Walther 為您介紹 TagBuilder 類別命名為 ASP.NET MVC 架構中非常有用的公用程式類別。 您可以輕鬆地使用 TagBuilder 類別..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc4e91bb14082c7c5e889d064d29d2bf91f7329
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>使用 TagBuilder 類別來建立 HTML Helper (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 作者： Stephen Walther 為您介紹 TagBuilder 類別命名為 ASP.NET MVC 架構中非常有用的公用程式類別。 您可以使用 TagBuilder 類別來輕鬆地建立 HTML 標記。


ASP.NET MVC 架構包括名為 TagBuilder 類別可讓您建置 HTML helper 時很有用的公用程式類別。 TagBuilder 類別，如下所示的類別名稱，可讓您輕鬆地建立 HTML 標記。 在這個簡短的教學課程中，為您提供 TagBuilder 類別的概觀和您了解如何使用這個類別，當建置簡單的 HTML helper 呈現 HTML &lt;img&gt;標記。

## <a name="overview-of-the-tagbuilder-class"></a>TagBuilder 類別的概觀

TagBuilder 類別被包含在 system.web.mvc 的參考命名空間。 它有五種方法：

- AddCssClass()-可讓您新增新*類別 =""*屬性至標記。
- GenerateId()-可讓您將識別碼屬性新增至標記。 這個方法會自動取代識別碼中的句點 （根據預設，句號會以底線取代）
- MergeAttribute()-可讓您將屬性加入至標記。 有多個多載，這個方法。
- SetInnerText()-可讓您設定標記的內部文字。 內部文字是 HTML 編碼自動。
- Tostring （）-可讓您呈現標記。 您可以指定是否要建立的標準標記、 開始標記、 結束標記或自行關閉標記。
  

TagBuilder 類別有四個重要的屬性：

- 屬性-代表所有標記的屬性。
- IdAttributeDotReplacement-表示 GenerateId() 方法用來取代的週期 （預設值為底線） 的字元。
- InnerHTML-表示標記的內部內容。 將字串指派給這個屬性*不*HTML 編碼字串。
- TagName-表示標記的名稱。

這些方法和屬性可讓您的所有基本方法和您要建置之 HTML 標記的屬性。 實際上，您不需要使用 TagBuilder 類別。 您可以改為使用 StringBuilder 類別。 不過，此 TagBuilder 類別可讓您的生活更為容易。

## <a name="creating-an-image-html-helper"></a>建立映像的 HTML Helper

當您建立 TagBuilder 類別的執行個體時，您會傳遞您要建置 TagBuilder 建構函式的標記名稱。 接下來，您可以呼叫等 AddCssClass 和 MergeAttribute() 方法修改的屬性標記的方法。 最後，您呼叫 tostring （） 方法來呈現標記。

例如，列出 1 包含影像的 HTML helper。 影像協助程式在內部實作與代表 HTML TagBuilder &lt;img&gt;標記。

**列出 1-Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

程式碼範例 1 中的類別包含兩個靜態的多載的方法，名為映像。 當您呼叫 Image() 方法時，您可以傳遞或不代表一組 HTML 屬性的物件。

請注意 TagBuilder.MergeAttribute() 方法用來將個別的屬性，例如 src 屬性加入至 TagBuilder 的方式。 請注意，此外，如何使用 TagBuilder.MergeAttributes() 方法來加入 TagBuilder 屬性的集合。 MergeAttributes() 方法接受字典&lt;字串、 物件&gt;參數。 RouteValueDictionary 類別用來轉換為字典集合的屬性物件&lt;字串、 物件&gt;。

建立影像協助程式之後，您可以使用協助專家在 ASP.NET MVC 檢視就像任何其他標準 HTML helper。 列表 2 中的檢視使用映像 helper 顯示相同的映像的 Xbox 兩次 （請參閱圖 1）。 Image() helper 稱為有和沒有 HTML 屬性集合。

**列出 2-Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![新增專案 對話方塊](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**圖 01**： 使用映像 helper ([按一下以檢視完整大小的影像](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


請注意，您必須匯入 Index.aspx 檢視頂端的影像協助程式相關聯的命名空間。 協助專家匯入下列指示詞：

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

>[!div class="step-by-step"]
[上一頁](creating-custom-html-helpers-cs.md)
[下一頁](creating-page-layouts-with-view-master-pages-cs.md)
