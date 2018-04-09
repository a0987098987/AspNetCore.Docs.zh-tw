---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 2 部分： 加入商務邏輯層和單元測試 |Microsoft 文件
author: tdykstra
description: 此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: ecdfb2bdc546f55778ec4cc1f61aa66e129134ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 2 部分： 加入商務邏輯層和單元測試
====================
由[Tom Dykstra](https://github.com/tdykstra)

> 此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程中，您建立使用 Entity Framework 的多層式架構 web 應用程式和`ObjectDataSource`控制項。 本教學課程示範如何將商務邏輯加入同時維持商務邏輯層 (BLL) 資料存取層 (DAL) 不同，並顯示如何建立 BLL 自動化的單元測試。

在本教學課程中，您將完成下列工作：

- 建立儲存機制介面宣告您需要的資料存取方法。
- 儲存機制類別中實作儲存機制的介面。
- 建立呼叫儲存機制類別，以執行資料存取功能的商務邏輯類別。
- 連接`ObjectDataSource`控制項至儲存機制類別而不是商務邏輯類別。
- 建立單元測試專案和其資料存放區的使用記憶體中集合的儲存機制類別。
- 建立您想要加入至商務邏輯類別，則執行測試，並查看它失敗的商務邏輯的單元測試。
- 商務邏輯在類別中實作商務邏輯，然後重新執行單元測試，並查看其通過。

您將使用*Departments.aspx*和*DepartmentsAdd.aspx*您在上一個教學課程中建立的網頁。

## <a name="creating-a-repository-interface"></a>建立儲存機制介面

您一開始所建立的儲存機制介面。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

在*DAL*資料夾中，建立新的類別檔案，其命名*ISchoolRepository.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

介面會定義一種方法的每個 CRUD （建立、 讀取、 更新、 刪除） 您建立儲存機制類別中的方法。

在`SchoolRepository`類別*SchoolRepository.cs*，指出這個類別會實作`ISchoolRepository`介面：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>建立商務邏輯類別

接下來，您將建立商務邏輯類別。 您執行這項操作，讓您可以新增將由執行的商務邏輯`ObjectDataSource`控制，您將不會執行，尚未雖然。 現在，新的商務邏輯類別只會執行相同的 CRUD 作業的儲存機制。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

建立新的資料夾並將其命名*BLL*。 (在真實世界應用程式中，商務邏輯層會通常會實作為類別程式庫 — 個別的專案，但為了簡化這個教學課程，BLL 類別將會保留在專案資料夾。)

在*BLL*資料夾中，建立新的類別檔案，其命名*SchoolBL.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

此程式碼會建立您所見的相同 CRUD 方法稍早在儲存機制類別中，但而不是直接存取 Entity Framework 方法，它會呼叫在儲存機制類別方法。

保存至儲存機制類別的參考類別變數定義為介面類型，並儲存機制類別具現化的程式碼包含兩個建構函式中。 無參數建構函式將會使用`ObjectDataSource`控制項。 它會建立的執行個體`SchoolRepository`您稍早建立的類別。 其他建構函式可讓任何商務邏輯類別，以在任何實作儲存機制介面的物件具現化的程式碼。

儲存機制類別和兩個建構函式呼叫的 CRUD 方法可讓您選擇任何後端資料存放區中使用商務邏輯類別。 商務邏輯類別不需要知道它正在呼叫的類別將資料的保存。 (這通常稱為*永續性無知之*。)這有助於單元測試，因為您可以連接使用的項目為簡單的儲存機制實作商務邏輯類別做為記憶體中`List`來儲存資料的集合。

> [!NOTE]
> 實體物件技術上來說，是仍不持續性-不知道，因為它們正在執行個體化的類別繼承自 Entity Framework`EntityObject`類別。 您可以使用完整的持續性忽略*純舊 CLR 物件*，或*POCOs*，來取代繼承的物件`EntityObject`類別。 使用 POCOs 超出本教學課程的範圍。 如需詳細資訊，請參閱[可測試性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) MSDN 網站上。)


現在您可以連接`ObjectDataSource`控制項加入至商務邏輯類別而不是至儲存機制，並確認一切運作與以前一樣。

在*Departments.aspx*和*DepartmentsAdd.aspx*，變更出現的每個`TypeName="ContosoUniversity.DAL.SchoolRepository"`至`TypeName="ContosoUniversity.BLL.SchoolBL`"。 （有四個執行個體中所有。）

執行*Departments.aspx*和*DepartmentsAdd.aspx*頁面，以確認它們仍然運作之前一樣。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>建立單元測試專案和儲存機制實作

將新的專案加入方案中使用**測試專案**範本，並將其命名`ContosoUniversity.Tests`。

在測試專案中，加入參考`System.Data.Entity`並加入專案參考`ContosoUniversity`專案。

您現在可以建立包含單元測試，您將使用的儲存機制類別。 這個儲存機制的資料存放區會在類別中。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

在測試專案中，建立新的類別檔案，其命名*MockSchoolRepository.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

這個儲存機制類別具有相同的 CRUD 方法，直接存取 Entity Framework，但可使用`List`與資料庫而不是記憶體中的集合。 這項功能可更容易設定和驗證商務邏輯類別的單元測試的測試類別。

## <a name="creating-unit-tests"></a>建立單元測試

**測試**專案範本建立虛設常式單元測試類別，和您的下一個工作是由您想要加入至商務邏輯類別的商務邏輯加入單元測試方法，以修改此類別。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

在 Contoso 大學任何個別講師只能是單一部門的系統管理員，而且您需要加入商務邏輯來強制執行此規則。 一開始會加入測試及執行測試以查看失敗。 然後將加入程式碼，並重新執行測試，預期看到測試成功。

開啟*UnitTest1.cs*檔案，然後加入`using`您 ContosoUniversity 專案中建立商務邏輯和資料存取層級的陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

取代`TestMethod1`方法使用下列方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL`方法會建立儲存機制建立的類別，您的單元測試專案，然後將傳遞至商務邏輯類別的新執行個體的執行個體。 然後，此方法會插入測試方法中，您可以使用三個部門使用商務邏輯類別。

測試方法確認有人嘗試插入新的部門由相同的系統管理員為現有的部門，或當有人嘗試更新部門的系統管理員設定的個人識別碼為商務邏輯類別擲回例外狀況使用者已經是另一個部門的系統管理員。

例外狀況類別尚未建立，因此將不會編譯此程式碼。 若要取得其進行編譯，以滑鼠右鍵按一下`DuplicateAdministratorException`選取**產生**，然後**類別**。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

這會在您可以刪除的測試專案建立類別主要專案中建立例外狀況類別之後。 實作商務邏輯。

執行測試專案。 如預期般，測試就會失敗。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>加入商務邏輯來進行測試

接下來，您將實作商務邏輯，可讓您更難以將設定為部門的系統管理員已經是另一個部門的系統管理員的人。 您將會擲回例外狀況於商務邏輯層，並再加以攔截，展示層如果使用者編輯部門，並按一下**更新**之後選取已經是系統管理員的人。 （您也無法從已系統管理員，才能轉譯頁面上，下拉式清單中移除講師但這裡的用途是要使用商務邏輯層）。

開始建立當使用者嘗試進行講師多個部門的系統管理員時，會擲回的例外狀況類別。 在主專案中，建立新的類別檔案中*BLL*資料夾中，其命名*DuplicateAdministratorException.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

現在刪除暫存*DuplicateAdministratorException.cs*您稍早建立的測試專案中才能編譯的檔案。

在主專案中，開啟*SchoolBL.cs*檔案，然後加入下列方法，其中包含驗證邏輯。 （程式碼參照的方法，您將在稍後建立）。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

當您要插入或更新時，會呼叫這個方法`Department`實體，以檢查另一個部門是否已經有相同的系統管理員。

程式碼會呼叫方法來搜尋資料庫`Department`具有相同的實體`Administrator`插入或更新與實體的屬性值。 如果找到，則程式碼擲回例外狀況。 如果正在插入或更新的實體沒有任何驗證檢查是必要`Administrator`如果在更新期間呼叫的方法擲回的值和任何例外狀況和`Department`找到相符項目實體`Department`正在更新實體。

呼叫的新方法，從`Insert`和`Update`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

在*ISchoolRepository.cs*，加入新的資料存取方法的下列宣告：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

在*SchoolRepository.cs*，加入下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

在*SchoolRepository.cs*，新增下列新的資料存取方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

此程式碼會擷取`Department`具有指定的系統管理員的實體。 （如果有的話），則應該找到只能有一個部門。 不過，因為沒有條件約束內建資料庫，傳回型別是集合萬一發現多個部門。

根據預設，物件內容從資料庫擷取實體時，它會追蹤的它們在其物件狀態管理員。 `MergeOption.NoTracking`參數會指定此追蹤不會完成此查詢。 這是必要的因為查詢可能會傳回您想要更新的確切實體，然後您就無法將該實體。 例如，如果您編輯中的歷程記錄部門*Departments.aspx*頁面並保留不變的系統管理員，此查詢會傳回記錄部門。 如果`NoTracking`未設定，內容物件會在其物件狀態管理員中已經有記錄 department 實體。 然後物件內容附加從檢視狀態時，都會重新建立歷程記錄 department 實體時，會擲回例外狀況指出`"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`。

(若要指定替代`MergeOption.NoTracking`，您可以建立新的物件內容，只針對此查詢。 因為新的物件內容會有它自己的物件狀態管理員，會有任何衝突時您呼叫`Attach`方法。 新的物件內容會與原始的物件內容中，共用中繼資料和資料庫連接，因此這個替代方法的效能負面影響很小。 方法在這裡顯示，不過，導入了`NoTracking`選項，您會發現在其他內容中很有用的。 `NoTracking`選項會討論在這一系列之後的教學課程中進一步。)

在測試專案中，加入新的資料存取方法*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

此程式碼使用 LINQ 來執行相同的資料選取範圍，`ContosoUniversity`專案儲存機制使用 LINQ to Entities 的。

再次執行測試專案。 測試這一次會成功。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>ObjectDataSource 例外狀況處理

在`ContosoUniversity`專案中，執行*Departments.aspx*頁面上，然後再次嘗試變更部門的系統管理員已經是另一個部門的系統管理員的人。 （請記住，您可以只進行編輯期間本教學課程中，新增的部門因為資料庫會恢復預先載入具有無效的資料）。您會得到下列伺服器錯誤頁面：

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

您不希望使用者看到這種錯誤 頁面上，因此您需要加入錯誤處理程式碼。 開啟*Departments.aspx*和指定的處理常式`OnUpdated`事件`DepartmentsObjectDataSource`。 `ObjectDataSource`現在開頭標記類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

在*Departments.aspx.cs*，加入下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

加入下列處理常式`Updated`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

如果`ObjectDataSource`控制項攔截例外狀況，當它嘗試執行更新時，事件引數中傳遞例外狀況 (`e`) 至這個處理常式。 此處理常式中的程式碼會檢查以查看例外狀況是否重複的系統管理員例外狀況。 如果是，程式碼會建立包含錯誤訊息的驗證程式控制項`ValidationSummary`控制項來顯示。

執行網頁，並嘗試再次某人的兩個部門系統管理員。 這次`ValidationSummary`控制項顯示的錯誤訊息。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

類似變更*DepartmentsAdd.aspx*頁面。 在*DepartmentsAdd.aspx*，指定的處理常式`OnInserted`事件`DepartmentsObjectDataSource`。 產生的標記將類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

在*DepartmentsAdd.aspx.cs*，加入相同`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

加入下列事件處理常式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

您現在可以測試*DepartmentsAdd.aspx.cs*頁面，確認它也會正確處理嘗試讓一人以上部門的系統管理員。

如此即完成實作使用的儲存機制模式的介紹`ObjectDataSource`與 Entity Framework 的控制項。 如需儲存機制模式和可測試性的詳細資訊，請參閱 MSDN 技術白皮書[可測試性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx)。

下列教學課程中，您會看到如何加入排序和篩選應用程式的功能。

> [!div class="step-by-step"]
> [上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [下一頁](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
