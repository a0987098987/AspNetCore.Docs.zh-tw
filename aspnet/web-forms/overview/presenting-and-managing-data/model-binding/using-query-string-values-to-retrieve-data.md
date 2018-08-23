---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: 使用查詢字串值來使用模型繫結篩選資料，以及 web form |Microsoft Docs
author: tfitzmac
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: fc4ec64cf583f25eca6795f7ff9215f025170054
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831219"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>使用模型繫結和 web form 中的查詢字串值來篩選資料
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。
> 
> 本教學課程會示範如何在查詢字串中傳遞的值，並使用該值來擷取資料，透過模型繫結。
> 
> 本教學課程中建立的專案是根據[早](retrieving-data.md)系列的一部分。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。


## <a name="what-youll-build"></a>您將建置

在本教學課程中，您將會：

1. 加入新的頁面，以便顯示已註冊的課程的學生身分
2. 擷取查詢字串中的值為基礎的所選學生的已註冊的課程
3. 從格線檢視中加入新的頁面的超連結，使用查詢字串值

在本教學課程的步驟都相當類似您在先前[教學課程](sorting-paging-and-filtering-data.md)來篩選顯示的學生，取決於使用者選擇的下拉式清單中。 在該教學課程中，您已使用**控制**中選取的方法，以指定的參數值來自控制項的屬性。 在本教學課程中，您將使用**QueryString**在選取的方法，以指定的參數值來自查詢字串中的屬性。

## <a name="add-new-page-for-displaying-a-students-courses"></a>加入新的頁面，來顯示某學生的課程

加入新的 web form 使用 Site.master 主版頁面，再將頁面命名**課程**。

在  **Courses.aspx**檔案中，新增 方格檢視來顯示所選取學生的課程。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>定義 select 方法

在  **Courses.aspx.cs**，將您在格線檢視中指定的名稱與 select 方法**SelectMethod**屬性。 在該方法中，您將定義查詢來擷取某學生的課程，並指定參數來自具有相同名稱做為參數的查詢字串值。

首先，您必須新增下列**使用**陳述式。

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

然後，將下列程式碼新增至 Courses.aspx.cs 中：

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

查詢字串屬性表示，為 StudentID 的查詢字串值會自動指派給這個方法中的參數。

## <a name="add-hyperlink-with-query-string-value"></a>加入超連結，以查詢字串值

在上 Students.aspx 方格檢視中，您將加入超連結欄位是連結到您的新課程頁面。 超連結會包含查詢字串值的學生識別碼。

在 Students.aspx，加入下列欄位之欄位的正下方的方格檢視資料行的信用總額。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

執行應用程式，並請注意，[格線] 檢視現在會包含 [課程] 連結。

![加入超連結](using-query-string-values-to-retrieve-data/_static/image1.png)

當您按一下其中一個連結時，您會看到該學生的已註冊的課程。

![顯示課程](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>結論

在本教學課程中，您可以加入連結，以使用查詢字串值。 您可以使用該查詢字串值選取方法的參數值。

在接下來[教學課程](adding-business-logic-layer.md)，您也會將程式碼從程式碼後置檔案中，移到商業邏輯層和資料存取層。

> [!div class="step-by-step"]
> [上一頁](integrating-jquery-ui.md)
> [下一頁](adding-business-logic-layer.md)
