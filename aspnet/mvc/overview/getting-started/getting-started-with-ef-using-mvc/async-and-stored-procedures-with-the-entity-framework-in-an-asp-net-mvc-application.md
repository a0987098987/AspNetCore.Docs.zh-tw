---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async 和 Entity framework 的 ASP.NET MVC 應用程式中的預存程序 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4c2deb53856d79a52415db48e04ea96111833b02
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381925"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async 和 Entity framework 的 ASP.NET MVC 應用程式中的預存程序
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在先前的教學課程中，您學會如何讀取和更新使用同步程式設計模型的資料。 在本教學課程中，您會看到如何實作非同步程式設計模型。 非同步程式碼有助於比較好，因為它能夠更妥善運用伺服器資源的應用程式。

在本教學課程也會看到如何使用預存程序插入、 更新和刪除作業的實體。

最後，您將會重新部署至 Azure，以及您部署的第一次之後，您已實作的資料庫變更的所有應用程式。

下列圖例顯示了您將操作的一些頁面。

![部門頁面](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![建立部門](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>為何要麻煩使用非同步程式碼

網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。 發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。 使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。 使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。 如此一來，非同步程式碼可讓伺服器資源會更有效率地使用，而且伺服器已啟用以處理更多的流量，不會造成延遲。

在舊版的.NET 中，撰寫和測試非同步程式碼很複雜，容易出錯且難以偵錯。 在.NET 4.5 中，撰寫、 測試和偵錯非同步程式碼會更加容易，您通常應該寫入非同步程式碼除非您有理由不要。 非同步程式碼會導致少量的額外負荷，但低流量情況下效能的衝擊非常微小; 在高流量情況下，潛在的效能改善則相當大。

如需有關非同步程式設計的詳細資訊，請參閱 <<c0> [ 使用.NET 4.5 的非同步支援，以避免封鎖呼叫](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)。

## <a name="create-the-department-controller"></a>建立 Department 控制器

建立使用相同方式較早的控制站，但這次選取 Department 控制器**使用非同步控制器**動作 核取方塊。

![部門控制站 scaffold](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

下列反白顯示的同步程式碼新增了哪些功能`Index`進行非同步的方法：

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

若要啟用 Entity Framework 資料庫查詢，以非同步方式執行，已套用四個變更：

- 方法會標示`async`關鍵字，它會告訴編譯器方法主體的一部分產生回呼，並自動建立`Task<ActionResult>`傳回物件。
- 傳回的型別已經從`ActionResult`至`Task<ActionResult>`。 `Task<T>`型別代表進行中的工作，其結果型別的`T`。
- `await`關鍵字已套用至 web 服務呼叫。 當編譯器看到這個關鍵字時，在幕後它會將方法分成兩個部分。 第一個部分的結尾會以非同步方式啟動的作業。 第二個部分會放入作業完成時呼叫的回呼方法。
- 非同步版本`ToList`呼叫擴充方法。

為什麼會`departments.ToList`陳述式但不是修改`departments = db.Departments`陳述式？ 原因是只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。 `departments = db.Departments`陳述式會設定查詢，但查詢不會執行直到`ToList`呼叫方法。 因此，只有`ToList`方法以非同步方式執行。

在`Details`方法和`HttpGet``Edit`並`Delete`方法，`Find`方法就是會使查詢傳送至資料庫，因此會以非同步方式執行的方法：

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

在  `Create`， `HttpPost Edit`，和`DeleteConfirmed`方法，它是`SaveChanges`會導致要執行命令的方法呼叫，不等的陳述式`db.Departments.Add(department)`這只會導致實體記憶體中要修改。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

開啟*Views\Department\Index.cshtml*，並以下列程式碼取代範本程式碼：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

此程式碼索引從標題變更為 「 部門，往右移動，系統管理員名稱，並提供系統管理員的完整名稱。

在 Create、 Delete、 Details、 和編輯檢視、 變更標題`InstructorID`欄位設為 「 系統管理員 」 課程檢視中變更 「 部門 」 部門的 [名稱] 欄位的方式相同。

在 建立 和 編輯檢視，請使用下列程式碼：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

在 刪除 和 詳細資料檢視中使用下列程式碼：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

執行應用程式，然後按一下**部門** 索引標籤。

![部門頁面](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

一切都運作中的相同其他控制器，但此控制器中所有的 SQL 查詢會以非同步方式執行。

要注意當您使用 Entity Framework 使用非同步程式設計的一些事項：

- 非同步程式碼不是安全執行緒。 換句話說，也就是說，不要嘗試執行多個作業以平行方式使用相同的內容執行個體。
- 若您想要充分利用非同步程式碼帶來的效能優點，請確保任何您正在使用的程式庫 (例如分頁) 也使用了非同步 (若它們有呼叫任何可能會傳送查詢到資料庫的 Entity Framework 方法的話)。

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>用於插入、 更新和刪除的預存程序

有些開發人員和 Dba 想要使用預存程序來存取資料庫。 在舊版的 Entity Framework 中，您可以擷取使用預存程序的資料[執行原始的 SQL 查詢](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)，但您無法指示 EF 來使用預存程序進行更新作業。 在 EF 6 很容易設定 Code First 至使用預存程序。

1. 在  *DAL\SchoolContext.cs*，將反白顯示的程式碼加入`OnModelCreating`方法。

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    此程式碼會指示 Entity Framework 使用預存程序插入、 更新和刪除作業上`Department`實體。
2. 在封裝管理主控台中，輸入下列命令：

    `add-migration DepartmentSP`

    開啟*移轉\&l t; 時間戳記&gt;\_DepartmentSP.cs*若要查看中的程式碼`Up`方法，以建立 Insert、 Update 和 Delete 預存程序：

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. 在封裝管理主控台中，輸入下列命令：

     `update-database`
4. 在 偵錯模式中執行應用程式中，按一下**部門**索引標籤，然後再按一下**建立新**。
5. 新的部門中，輸入資料，然後按一下**建立**。

     ![建立部門](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. 在 Visual Studio 中檢視記錄檔，在**輸出**視窗來查看預存程序用來插入新的 Department 資料列。

     ![部門 Insert 預存程序](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

程式碼首先會建立預設預存程序名稱。 如果您使用現有的資料庫，您可能需要自訂預存程序名稱，若要使用已經在資料庫中定義的預存程序。 如需如何執行該動作的資訊，請參閱[Entity Framework 程式碼第一個 Insert/Update/Delete 預存程序](https://msdn.microsoft.com/data/dn468673)。

如果您想要自訂項目產生預存程序執行，您可以編輯移轉的 scaffold 程式碼`Up`方法，以建立預存程序。 如此一來您的變更會反映每當移轉執行時，並會在部署後的生產環境中自動執行移轉時套用到您的生產資料庫。

如果您想要變更現有的預存程序中先前移轉所建立，您可以使用新增移轉命令來產生空白的移轉，並以手動方式撰寫程式碼呼叫[AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)方法.

## <a name="deploy-to-azure"></a>部署到 Azure

本章節會要求您已經完成選擇性**將應用程式部署至 Azure**一節[移轉和部署](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)本系列教學課程。 如果您刪除本機專案中的資料庫來判斷已解決的移轉錯誤，請略過本節。

1. 在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。
2. 按一下 [發行] 。

    Visual Studio 會部署至 Azure，應用程式和應用程式會開啟預設瀏覽器，在 Azure 中執行。
3. 測試應用程式，以確認它是否運作。

    第一次您執行頁面，來存取資料庫，Entity Framework 便會執行所有移轉`Up`才能讓資料庫維持在最新狀態與目前的資料模型的方法。 您現在可以使用所有的部署，包括您在本教學課程新增部門頁面最後一次您新增的網頁。

## <a name="summary"></a>總結

在本教學課程中您會看到如何增進伺服器的效能，撰寫程式碼以非同步方式執行，以及如何使用預存程序插入、 更新和刪除作業。 在下一個教學課程中，您會看到如何防止資料遺失，當多位使用者嘗試同時編輯同一筆記錄。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
