---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: "使用查詢字串值來篩選資料與模型繫結和 web form |Microsoft 文件"
author: tfitzmac
description: "此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>使用模型繫結和 web form 中的查詢字串值來篩選資料
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。
> 
> 本教學課程會示範如何查詢字串中傳遞的值，並使用該值來透過模型繫結來擷取資料。
> 
> 本教學課程中建立的專案是[早](retrieving-data.md)數列的組件。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。


## <a name="what-youll-build"></a>您將建置

在此教學課程中，您將會：

1. 加入新的頁面，以顯示 已註冊的課程學生
2. 為選取的查詢字串中的值為基礎的學生所擷取的已註冊的課程
3. 從格線檢視的查詢字串值的超連結加入新的頁面

在本教學課程的步驟會相當類似於您所執行先前[教學課程](sorting-paging-and-filtering-data.md)來篩選顯示的學生，根據使用者選項，在下拉式清單中。 在此教學課程中，在您使用**控制項**中選取的方法，以指定的參數值來自的控制項屬性。 在此教學課程中，您將使用**QueryString**選取方法，以指定的參數值來自查詢字串中的屬性。

## <a name="add-new-page-for-displaying-a-students-courses"></a>加入新的頁面，顯示一位學生課程

加入新的 web 表單使用 Site.master 主版頁面，再將頁面命名**課程**。

在**Courses.aspx**檔案中，加入要顯示所選的學生課程方格檢視。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>定義 select 方法

在**Courses.aspx.cs**，您會將選取的方法與您在格線檢視中指定的名稱加入**SelectMethod**屬性。 在該方法中，您會定義查詢來擷取一位學生課程，並指定參數來自具有相同名稱做為參數的查詢字串值。

首先，您必須新增下列**使用**陳述式。

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

然後，Courses.aspx.cs 中加入下列程式碼：

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

查詢字串屬性表示名為 StudentID 的查詢字串值會自動指派至這個方法中的參數。

## <a name="add-hyperlink-with-query-string-value"></a>加入超連結與查詢字串值

在上 Students.aspx 方格檢視中，您將加入到新的課程頁面連結的超連結欄位。 超連結會包含與學生識別碼的查詢字串值。

在 Students.aspx，加入下列欄位欄位的正下方的方格檢視資料行的總點數。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

執行應用程式，請注意，資料格檢視現在會包含課程連結。

![加入超連結](using-query-string-values-to-retrieve-data/_static/image1.png)

當您按一下其中一個連結時，您會看到該學生的已註冊的課程。

![顯示課程](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>結論

在本教學課程中，您可以加入具有查詢字串值的連結。 您可以使用該查詢字串值的 select 方法參數值。

在接下來[教學課程](adding-business-logic-layer.md)，您也會將程式碼從程式碼後置檔案中，移到商務邏輯層和資料存取層。

>[!div class="step-by-step"]
[上一頁](integrating-jquery-ui.md)
[下一頁](adding-business-logic-layer.md)
