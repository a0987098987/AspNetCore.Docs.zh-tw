---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: 使用 TagBuilder 類別可建置 HTML 協助程式 (C#) |Microsoft Docs
author: StephenWalther
description: Stephen Walther 會向您介紹在 ASP.NET MVC framework 將 TagBuilder 類別命名為實用的公用程式類別。 您可以輕鬆地使用 TagBuilder 類別可...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ec50fca1d65d95aaf2baf00c84c080ba98bf333
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383864"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>使用 TagBuilder 類別可建置 HTML 協助程式 (C#)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther 會向您介紹在 ASP.NET MVC framework 將 TagBuilder 類別命名為實用的公用程式類別。 您可以使用 TagBuilder 類別，輕鬆地建置 HTML 標記。


ASP.NET MVC 架構包括名為 TagBuilder 類別可建置 HTML 協助程式時，您可以使用的實用的公用程式類別。 TagBuilder 類別名所示的類別，可讓您輕鬆地建置 HTML 標記。 在這個簡短的教學課程中，為您提供 TagBuilder 類別的概觀和您將了解如何使用這個類別，當建置簡單的 HTML helper 來呈現 HTML &lt;img&gt;標記。

## <a name="overview-of-the-tagbuilder-class"></a>TagBuilder 類別的概觀

TagBuilder 類別包含在 System.Web.Mvc 命名空間中。 它有五個方法：

- AddCssClass()-可讓您新增的新*類別 =""* 屬性標記。
- GenerateId()-可讓您將識別碼屬性新增至標記。 這個方法會自動取代識別碼中的句點 （根據預設，週期會以底線取代）
- MergeAttribute()-可讓您將屬性加入至標記。 有多個多載，這個方法。
- SetInnerText()-可讓您設定標記的內部文字。 內部文字會自動對 HTML 編碼。
- Tostring （）-可讓您呈現標記。 您可以指定是否要建立的標準標記、 開始標記、 結束標記或自我結尾標記。
  

TagBuilder 類別有四個重要的屬性：

- 屬性-代表所有標記的屬性。
- IdAttributeDotReplacement-表示 GenerateId() 方法用來取代的期間 （預設值為底線） 的字元。
- InnerHTML-表示標記的內部的內容。 將字串指派給這個屬性*則否*HTML 編碼字串。
- TagName-表示標記的名稱。

這些方法和屬性提供給您所有的基本的方法和您要建置的 HTML 標記的屬性。 您真的不需要使用 TagBuilder 類別。 您可以改為使用 StringBuilder 類別。 不過，TagBuilder 類別可讓您的生活更輕鬆。

## <a name="creating-an-image-html-helper"></a>建立映像 HTML 協助程式

當您建立 TagBuilder 類別的執行個體時，您會傳遞您想要建置 TagBuilder 建構函式的標記名稱。 接下來，您可以呼叫方法，例如 AddCssClass 和 MergeAttribute() 方法修改標記的屬性。 最後，您會呼叫 tostring （） 方法，來呈現標記。

例如，列表 1 包含的映像 HTML 協助程式。 影像協助程式在內部實作代表 HTML TagBuilder &lt;img&gt;標記。

**列表 1-Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

列表 1 中的類別包含兩個靜態的多載的方法，名為映像。 當您呼叫 Image() 方法時，您可以傳遞或不代表一組 HTML 屬性的物件。

請注意 TagBuilder.MergeAttribute() 方法用來將個別的屬性，例如 src 屬性新增至 TagBuilder 的方式。 請注意，此外，將屬性的集合新增至 TagBuilder 用法 TagBuilder.MergeAttributes() 方法。 MergeAttributes() 方法接受字典&lt;string，object&gt;參數。 RouteValueDictionary 類別用來轉換物件，表示為字典的屬性集合&lt;string，object&gt;。

建立影像協助程式之後，您可以使用協助程式，在您的 ASP.NET MVC 檢視就像任何其他標準的 HTML 協助程式。 列表 2 中的檢視使用影像協助程式兩次顯示 Xbox 的相同映像 （請參閱 圖 1）。 Image() 協助程式會呼叫具有和沒有 HTML 屬性集合。

**列表 2-Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![[新增專案] 對話方塊](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**圖 01**： 使用影像協助程式 ([按一下以檢視完整大小的影像](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


請注意，您必須匯入影像協助程式頂端的 Index.aspx 檢視相關聯的命名空間。 使用下列指示詞，匯入協助程式：

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [上一頁](creating-custom-html-helpers-cs.md)
> [下一頁](creating-page-layouts-with-view-master-pages-cs.md)
