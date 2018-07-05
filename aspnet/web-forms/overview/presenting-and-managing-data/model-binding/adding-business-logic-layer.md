---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: 新增商業邏輯層級使用模型繫結和 web forms 的專案 |Microsoft Docs
author: tfitzmac
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: be69bf56c6a1c5bf601a47e90e4e1c67c48a760f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386496"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>新增商業邏輯層級使用模型繫結和 web forms 的專案
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。
> 
> 本教學課程會示範如何使用商務邏輯層的模型繫結。 您將設定 OnCallingDataMethods 成員指定目前頁面以外的物件用來呼叫資料方法。
> 
> 本教學課程中建立的專案是根據[早](retrieving-data.md)系列的一部分。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。


## <a name="what-youll-build"></a>您將建置

模型繫結可讓您將資料互動的程式碼，可能是程式碼後置檔案中的網頁或個別的商務邏輯類別中。 先前的教學課程已示範如何使用資料互動的程式碼的程式碼後置檔案。 這種方法適用於小型站台，但它可能會導致程式碼重複和更不容易維護的大型網站時。 它也可以是很難進行程式設計的方式測試程式碼位於程式碼後置檔案，因為沒有任何抽象層。

集中管理的資料互動的程式碼，您可以建立商業邏輯層，其中包含所有的邏輯與資料互動。 然後，您會呼叫商務邏輯層從您的網頁。 本教學課程會示範如何移動所有的商務邏輯層，到先前的教學課程中，您已撰寫的程式碼，然後使用該頁面的程式碼。

在本教學課程中，您將會：

1. 從程式碼後置檔案的程式碼移到商業邏輯層
2. 變更您的資料繫結控制項，以呼叫商務邏輯層中的方法

## <a name="create-business-logic-layer"></a>建立商業邏輯層

現在，您將建立會從網頁呼叫的類別。 此類別中的方法看起來類似您在先前的教學課程中使用的方法，並包含值提供者屬性。

首先，新增 新資料夾，稱為**BLL**。

![新增資料夾](adding-business-logic-layer/_static/image1.png)

在 [BLL] 資料夾中，建立新類別，名為**SchoolBL.cs**。 它會包含所有最初在程式碼後置檔案中的資料作業。 方法的方法，在程式碼後置檔案中，幾乎相同，但會包括一些變更。

要注意的最重要變更是，您無法再執行中的程式碼執行個體內**網頁**類別。 頁面類別包含**TryUpdateModel**方法並**ModelState**屬性。 此程式碼移到商業邏輯層，當您不再需要呼叫這些成員的頁面類別的執行個體。 若要解決此問題，您必須新增**ModelMethodContext**存取 TryUpdateModel 或 ModelState 任何方法的參數。 您可以使用這個 ModelMethodContext 參數呼叫 TryUpdateModel 或擷取 ModelState。 您不需要變更網頁以說明這個新的參數中的任何項目。

SchoolBL.cs 中的程式碼取代為下列程式碼。

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>修改現有的頁面，來擷取商業邏輯層中的資料

最後，您會從使用商務邏輯層的程式碼後置檔案中使用查詢來轉換 Students.aspx、 AddStudent.aspx 和 Courses.aspx 的頁面。

在程式碼後置檔案學生、 AddStudent，及課程中，刪除或註解化下列的查詢方法：

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

您現在應該有程式碼不屬於資料作業的程式碼後置檔案中。

**OnCallingDataMethods**事件處理常式可讓您指定要用於資料方法的物件。 Students.aspx，在加入該事件處理常式的值並將變更之資料方法的名稱之商務邏輯類別中方法的名稱。

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

在 Students.aspx 的程式碼後置檔案，定義 CallingDataMethods 事件的事件處理常式。 在這個事件處理常式中，您可以指定資料作業的商務邏輯類別。

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

在 AddStudent.aspx，進行類似變更。

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

在 Courses.aspx，進行類似變更。

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

執行應用程式，並請注意，所有的頁面做為它們之前已擁有。 驗證邏輯能正確運作。

## <a name="conclusion"></a>結論

在本教學課程中，重新建構您的應用程式使用的資料存取層和商務邏輯層。 您指定資料控制項使用不是資料作業的目前頁面的物件。

> [!div class="step-by-step"]
> [上一步](using-query-string-values-to-retrieve-data.md)
