---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: 若要使用模型繫結和 web form 的專案加入商務邏輯層 |Microsoft 文件
author: tfitzmac
description: 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>加入商務邏輯層級使用模型繫結和 web form 的專案
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。
> 
> 本教學課程會示範如何使用商務邏輯層包含模型繫結。 您將設定 OnCallingDataMethods 成員來指定目前的頁面以外的物件用來呼叫資料方法。
> 
> 本教學課程中建立的專案是[早](retrieving-data.md)數列的組件。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。


## <a name="what-youll-build"></a>您將建置

模型繫結可讓您放置資料互動的程式碼，可能是程式碼後置檔案中的網頁或不同的商務邏輯類別中。 先前的教學課程顯示如何針對資料互動的程式碼使用程式碼後置檔案。 這個方法適用於小型站台，但它可能會導致程式碼重複和更不容易維護的大型網站時。 它也可以很難測試位於程式碼後置檔案，因為沒有任何抽象層的程式碼，以程式設計的方式。

要集中管理資料互動的程式碼，您可以建立包含所有的邏輯與資料互動的商務邏輯層。 然後，您會呼叫商務邏輯層從您網頁。 本教學課程會示範如何將所有的商務邏輯層到先前的教學課程中您撰寫的程式碼，然後使用 從網頁程式碼。

在此教學課程中，您將會：

1. 程式碼後置檔案中的程式碼移到商務邏輯層
2. 變更您的資料繫結控制項，以呼叫商務邏輯層中的方法

## <a name="create-business-logic-layer"></a>建立商務邏輯層

現在，您將建立會從網頁呼叫的類別。 這個類別中的方法看起來類似您在先前的教學課程中使用的方法，並包含值提供者屬性。

首先，新增新的資料夾，稱為**BLL**。

![新增資料夾](adding-business-logic-layer/_static/image1.png)

在 BLL 資料夾中建立新的類別，名為**SchoolBL.cs**。 它會包含所有的程式碼後置檔案最初的資料作業。 方法會在程式碼後置檔案中，方法幾乎完全相同，但會包括一些變更。

請注意最重要的變更是不會再執行中的程式碼執行個體內**頁面**類別。 頁面類別包含**TryUpdateModel**方法和**ModelState**屬性。 此程式碼移到商務邏輯層中，當您不再需要呼叫這些成員頁面類別的執行個體。 若要解決此問題，您必須新增**ModelMethodContext**存取 TryUpdateModel 或 ModelState 任何方法的參數。 您可以使用這個 ModelMethodContext 參數來呼叫 TryUpdateModel 或擷取 ModelState。 您不需要變更任何項目在網頁上的這個新的參數。

SchoolBL.cs 中的程式碼取代為下列程式碼。

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>修改現有的頁面，來擷取商務邏輯層中的資料

最後，您會將頁面 Students.aspx、 AddStudent.aspx，以及 Courses.aspx 從在要使用商務邏輯層的程式碼後置檔案中使用查詢轉換。

在程式碼後置檔案學生版 AddStudent，和課程中，刪除或標記為註解下列的查詢方法：

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

您現在應該有任何程式碼的相關資料的作業程式碼後置檔案中。

**OnCallingDataMethods**事件處理常式可讓您指定要用於資料方法的物件。 在 Students.aspx，新增該事件處理常式的值，將資料方法的名稱變更為商務邏輯類別中方法的名稱。

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

在 Students.aspx 的程式碼後置檔案，定義 CallingDataMethods 事件的事件處理常式。 在此事件處理常式中，您可以指定資料作業的商務邏輯類別。

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

在 AddStudent.aspx，進行類似的變更。

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

在 Courses.aspx，進行類似的變更。

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

執行應用程式，請注意，所有頁面函式，它們在先前。 驗證邏輯也運作正常。

## <a name="conclusion"></a>結論

在本教學課程中，重新建構您的應用程式使用資料存取層和商務邏輯層。 您指定的資料控制項使用不是資料作業的目前頁面的物件。

> [!div class="step-by-step"]
> [上一步](using-query-string-values-to-retrieve-data.md)
