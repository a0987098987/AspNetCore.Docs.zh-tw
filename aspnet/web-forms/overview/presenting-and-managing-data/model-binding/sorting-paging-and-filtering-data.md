---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: 排序、 分頁和篩選資料與模型繫結和 web form |Microsoft Docs
author: Rick-Anderson
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 624f98cea6030e0b7b022f86c4c1aa37f1db9726
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020946"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>排序、 分頁和篩選資料與模型繫結和 web form
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。
> 
> 本教學課程會示範如何新增排序、 分頁和篩選資料的模型繫結。
> 
> 本教學課程是根據在第一個建立的專案[一部分](retrieving-data.md)的數列。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。


## <a name="what-youll-build"></a>您將建置

在本教學課程中，您將會：

1. 啟用排序和分頁資料
2. 啟用篩選，根據使用者選取的資料

## <a name="add-sorting"></a>加入排序

啟用排序 GridView 裡的方法很簡單。 在 Student.aspx 檔案中，只要設定**AllowSorting**要 **，則為 true** GridView 裡。 您不需要設定**SortExpression** DataField 會自動使用每個資料行值。 GridView 修改查詢包含的資料排序所選取的值。 下列反白顯示的程式碼顯示加入，您必須先啟用排序。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

執行 web 應用程式，並測試排序的學生記錄不同的資料行中的值。

![排序學生](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>加入分頁

啟用分頁方法也很簡單。 在 gridview 裡，設定**AllowPaging**屬性設 **，則為 true**並設定**PageSize**屬性，以您想要在每個頁面上顯示的記錄數目。 在本教學課程中，您可以將它設定為 4。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

執行 web 應用程式，並請注意，現在記錄時，會分割透過多個頁面上，不能超過 4 顯示在單一頁面上的記錄。

![加入分頁](sorting-paging-and-filtering-data/_static/image4.png)

延後的查詢執行會改善應用程式的效率。 而不會擷取整個資料集，GridView 會修改查詢以擷取目前頁面的記錄。

## <a name="filter-records-by-user-selection"></a>依使用者選取項目來篩選記錄

模型繫結會新增數個屬性可讓您指定如何在模型繫結方法設定參數的值。 這些屬性位於**System.Web.ModelBinding**命名空間。 包括：

- 控制項
- Cookie
- 表單
- 設定檔
- QueryString
- RouteData
- 工作階段
- 使用者設定檔
- ViewState

在本教學課程中，您將使用控制項的值來篩選在 GridView 中顯示的記錄。 您將會加入**控制**屬性加入您先前建立的查詢方法。 在 [稍後](using-query-string-values-to-retrieve-data.md)教學課程中，您將會套用**QueryString**參數來指定參數值來自查詢字串值，此屬性。

首先，ValidationSummary，上述新增的下拉式清單來篩選顯示的學生。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

在程式碼後置檔案中，修改選取的方法，以接收來自控制項的值，並提供值之控制項的名稱設定參數的名稱。

您必須新增**使用**陳述式**System.Web.ModelBinding**解決控制項屬性的命名空間。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

下列程式碼會顯示重新用來篩選傳回的資料值的下拉式清單選取方法。 參數可讓您指定這個參數的值是來自具有相同名稱的控制項之前，請加入控制項屬性。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

執行 web 應用程式，並從下拉式清單以篩選學員清單中選取不同的值。

![篩選學生](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>結論

在本教學課程中，您已啟用排序和分頁的資料。 您也已啟用控制項的值所篩選的資料。

在接下來[教學課程](integrating-jquery-ui.md)JQuery UI 小工具整合的動態資料範本，您將提升 UI。

> [!div class="step-by-step"]
> [上一頁](updating-deleting-and-creating-data.md)
> [下一頁](integrating-jquery-ui.md)
