---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: 排序、 分頁和篩選資料，包含模型繫結和 web form |Microsoft 文件
author: tfitzmac
description: 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>排序、 分頁和篩選資料，包含模型繫結和 web form
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。
> 
> 本教學課程會示範如何加入排序、 分頁和篩選資料的模型繫結。
> 
> 本教學課程是在第一個建立的專案[一部分](retrieving-data.md)的數列。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。


## <a name="what-youll-build"></a>您將建置

在此教學課程中，您將會：

1. 啟用資料的排序和分頁
2. 啟用選取為依據的使用者資料的篩選

## <a name="add-sorting"></a>加入排序

在 GridView 中排序，是非常簡單。 在 Student.aspx 檔案中，只要設定**AllowSorting**至**true** GridView 中。 您不需要設定**SortExpression** DataField 會自動使用為每個資料行值。 在 GridView 修改查詢來包含所選取的值排序資料。 反白顯示的下列程式碼顯示新增，您必須先啟用排序。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

執行 web 應用程式和測試排序學生記錄不同的資料行中的值。

![排序學生版](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>加入分頁

啟用分頁功能也非常容易。 在 GridView 中設定**AllowPaging**屬性**true**並設定**PageSize**屬性，以您想要在每個頁面上顯示的記錄數目。 在本教學課程中，您可以將它設定為 4。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

執行 web 應用程式，並注意現在記錄會分割多個頁面上，不能超過 4 顯示在單一頁面上的記錄。

![加入分頁](sorting-paging-and-filtering-data/_static/image4.png)

延後的查詢執行會改善應用程式效率。 而不會擷取整個資料集，GridView 修改查詢以擷取目前頁面的記錄。

## <a name="filter-records-by-user-selection"></a>依使用者選取項目來篩選記錄

模型繫結會新增數個屬性可讓您指定如何在模型繫結方法中設定參數的值。 這些屬性都是**System.Web.ModelBinding**命名空間。 包括：

- 控制項
- Cookie
- 表單
- 設定檔
- QueryString
- RouteData
- 工作階段
- UserProfile
- ViewState

在本教學課程中，您將使用控制項的值來篩選在 GridView 中顯示的記錄。 您將加入**控制項**屬性設定為您稍早建立的查詢方法。 在[稍後](using-query-string-values-to-retrieve-data.md)教學課程中，您將會套用**QueryString**屬性設定為指定的參數值來自查詢字串值的參數。

首先，ValidationSummary 上述新增的下拉式清單，篩選會顯示哪些學生。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

在程式碼後置檔案中，修改選取的方法，以接收的值從控制項，而設定的參數名稱為控制項提供值的名稱。

您必須新增**使用**陳述式**System.Web.ModelBinding**解決控制項屬性的命名空間。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

下列程式碼顯示重新能夠用來篩選傳回的資料，根據值的下拉式清單選取方法。 參數會指定此參數的值來自具有相同名稱的控制項之前，請加入控制項屬性。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

執行 web 應用程式，並從下拉式清單來篩選學員清單中選取不同的值。

![篩選學生版](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>結論

在本教學課程中，您可以啟用資料的排序和分頁。 您也可以啟用控制項的值來篩選資料。

在接下來[教學課程](integrating-jquery-ui.md)JQuery UI 小工具整合到動態資料範本，您將提升 UI。

> [!div class="step-by-step"]
> [上一頁](updating-deleting-and-creating-data.md)
> [下一頁](integrating-jquery-ui.md)
