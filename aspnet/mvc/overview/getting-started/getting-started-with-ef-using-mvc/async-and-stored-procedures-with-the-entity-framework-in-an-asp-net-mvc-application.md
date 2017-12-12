---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Async 和 Entity framework，ASP.NET MVC 應用程式中的預存程序 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5b4904037838441942ea266ce71d735642d0a717
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async 和 Entity framework，ASP.NET MVC 應用程式中的預存程序
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在較早的教學課程中，您學會如何讀取和更新使用同步程式撰寫模型的資料。 在本教學課程，您會了解如何實作非同步程式設計模型。 非同步程式碼可協助執行更好，因為它可更妥善運用伺服器資源的應用程式。

在本教學課程也會看到如何使用預存程序插入、 更新和刪除作業對實體。

最後，您將會重新部署至 Azure，以及所有您已實作自第一次您部署的資料庫變更的應用程式。

下圖顯示一些您將使用的頁面。

![部門頁面](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![建立部門](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>以非同步的程式碼費

Web 伺服器的有限的數目的執行緒可用，而且在高負載情況下的所有可用的執行緒可能正在使用中。 當發生這種情況時，伺服器無法處理新的要求，直到執行緒釋放。 使用同步程式碼，許多執行緒可能會將繫結起來雖然它們實際上並不執行任何工作，因為他們正在等候 I/O 完成。 使用非同步程式碼，當處理程序在等候 I/O 完成，它的執行緒釋放針對伺服器將用於處理其他要求。 如此一來，非同步程式碼可讓伺服器資源使用更有效率，而且伺服器已啟用以處理更多流量不會造成延遲。

在舊版的.NET 中，撰寫和測試非同步程式碼很複雜，容易出錯，而且容易進行偵錯。 .NET 4.5 中撰寫、 測試和偵錯非同步程式碼會因此比較容易，您應該通常撰寫非同步程式碼除非您有理由不。 非同步程式碼會導致少量的額外負荷，但效能的衝擊並不顯著，同時針對高流量的情況低流量的情況下，可能的效能提升的很大。

如需有關非同步程式設計的詳細資訊，請參閱[使用.NET 4.5 的非同步支援，以避免封鎖呼叫](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)。

## <a name="create-the-department-controller"></a>建立部門控制站

建立部門控制站相同的方式就像您先前的控制站，但這次選取**使用非同步控制器**動作核取方塊。

![部門控制站 scaffold](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

下列反白顯示為同步的程式碼加入的項目`Index`進行非同步的方法：

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

若要啟用 Entity Framework 資料庫查詢，以非同步方式執行套用四個變更：

- 方法會標示`async`關鍵字會指示編譯器以產生組件的方法主體的回呼，並自動建立`Task<ActionResult>`傳回物件。
- 傳回型別已經從`ActionResult`至`Task<ActionResult>`。 `Task<T>`類型代表進行中的工作，且結果型別`T`。
- `await`關鍵字已套用至 web 服務呼叫。 當編譯器查看此關鍵字時，在幕後它分割方法兩個部分。 以非同步方式啟動的作業結束的第一個部分。 第二個部分會放入作業完成時呼叫的回呼方法。
- 非同步版本`ToList`呼叫擴充方法。

為什麼是`departments.ToList`陳述式但不是修改`departments = db.Departments`陳述式？ 原因是只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。 `departments = db.Departments`陳述式會設定查詢，不過不會執行查詢，直到`ToList`方法呼叫。 因此，只有`ToList`以非同步方式執行方法。

在`Details`方法和`HttpGet``Edit`和`Delete`方法`Find`方法是，會使查詢傳送至資料庫，因此是以非同步方式執行的方法：

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

在`Create`， `HttpPost Edit`，和`DeleteConfirmed`方法，它是`SaveChanges`會導致要執行命令的方法呼叫、 不等的陳述式`db.Departments.Add(department)`而只在記憶體中，修改導致實體。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

開啟*Views\Department\Index.cshtml*，並將範本程式碼取代下列程式碼：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

此程式碼從索引標題變更為 「 部門，往右移動，系統管理員名稱，並提供系統管理員的完整名稱。

在 [建立] 刪除，詳細資料，並編輯檢視、 變更標題`InstructorID`欄位設為 「 系統管理員 」 中的課程檢視變更 「 部門 」 部門名稱欄位的相同方式。

在建立與編輯檢視，請使用下列程式碼：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

在 刪除 和 詳細資料檢視中使用下列程式碼：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

執行應用程式，然後按一下**部門** 索引標籤。

![部門頁面](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

一切運作中的相同其他控制站，但此控制器中所有的 SQL 查詢會以非同步方式執行。

要注意當您使用 Entity Framework 使用非同步程式設計的一些事項：

- 非同步程式碼不是安全執行緒。 換句話說，也就是說，不要嘗試執行多個作業以平行方式使用相同的內容執行個體。
- 如果您想要充分利用非同步程式碼的效能優點，請確定任何程式庫封裝，您只使用 （例如分頁），也使用非同步呼叫會造成查詢傳送至資料庫的任何 Entity Framework 方法。

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>用於插入、 更新和刪除的預存程序

某些開發人員和 Dba 偏好使用預存程序來存取資料庫。 在舊版的 Entity Framework 中，您可以擷取資料使用的預存程序[執行原始的 SQL 查詢](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)，但您無法指示 EF 来用於更新作業的預存程序。 在 EF 6 很容易設定 Code First 使用預存程序。

1. 在*DAL\SchoolContext.cs*，將反白顯示的程式碼加入`OnModelCreating`方法。

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    此程式碼會指示 Entity Framework 使用預存程序插入、 更新和刪除操作`Department`實體。
2. 在封裝管理主控台中，輸入下列命令：

    `add-migration DepartmentSP`

    開啟*移轉\&lt; 時間戳記&gt;\_DepartmentSP.cs*查看程式碼中的`Up`方法建立 Insert、 Update 和 Delete 預存程序：

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- 在封裝管理主控台中，輸入下列命令：

    `update-database`
- 在偵錯模式執行應用程式，請按一下**部門**索引標籤，然後再按一下**新建**。
- 新的部門，輸入資料，然後按一下**建立**。

    ![建立部門](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- 在 Visual Studio 中，檢視記錄檔中**輸出**視窗，請參閱 < 預存程序用來插入新的部門資料列。

    ![部門 Insert 預存程序](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

程式碼會先建立預設預存程序名稱。 如果您使用現有的資料庫，您可能需要自訂預存程序名稱，才能使用資料庫中已定義的預存程序。 如需如何進行這項資訊，請參閱[Entity Framework 程式碼第一個 Insert/Update/Delete 預存程序](https://msdn.microsoft.com/en-us/data/dn468673)。

如果您想要自訂所產生的預存程序，您可以編輯 scaffold 的程式碼移轉`Up`建立預存程序的方法。 如此一來您的變更會反映每當執行移轉，並會在實際執行部署後自動執行移轉時套用到您的生產資料庫。

如果您想要變更現有的預存程序中的先前移轉所建立，您可以使用 Add-migration 命令來產生空白的移轉，並手動撰寫程式碼呼叫[AlterStoredProcedure](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)方法.

## <a name="deploy-to-azure"></a>部署至 Azure

本節會要求您已經完成選擇性**將應用程式部署至 Azure**一節中[移轉和部署](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)此數列的教學課程。 如果您在您解決方法是刪除您的本機專案中的資料庫的移轉錯誤，請略過本節。

1. 在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**從內容功能表。
2. 按一下 [發行] 。

    Visual Studio 應用程式部署至 Azure，並在應用程式中開啟預設的瀏覽器，在 Azure 中執行。
3. 測試應用程式，以確認它是否運作。

    第一次您執行頁面，來存取資料庫時，Entity Framework 會執行所有移轉`Up`使資料庫的最新狀態的目前資料模型所需的方法。 您現在可以使用所有的部署，包括您在本教學課程中加入的部門頁面最後一次您新增的網頁。

## <a name="summary"></a>總結

在本教學課程中您已看到如何改善伺服器效能，撰寫程式碼以非同步方式執行，以及如何使用預存程序插入、 更新和刪除作業。 在下一個教學課程中，您會看到如何避免資料遺失，當多位使用者嘗試同時編輯同一筆記錄。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
