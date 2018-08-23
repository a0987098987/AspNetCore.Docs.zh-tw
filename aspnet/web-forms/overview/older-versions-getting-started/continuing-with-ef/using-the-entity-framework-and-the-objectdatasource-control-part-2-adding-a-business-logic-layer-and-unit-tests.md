---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 2 部分： 新增商務邏輯層和單元測試 |Microsoft Docs
author: tdykstra
description: 本教學課程系列由開始使用 Entity Framework 4.0 的教學課程系列的 Contoso 大學 web 應用程式為基礎。 我...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6517f037a03bb520ee4f3b3185a255b0eaa9de9a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834224"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 2 部分： 新增商務邏輯層和單元測試
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> 本教學課程系列是根據所建立的 Contoso 大學 web 應用程式[Getting Started with Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，為本教學課程的起始點即可[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整的教學課程系列。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程中，您建立使用 Entity Framework 的多層式架構 web 應用程式和`ObjectDataSource`控制項。 本教學課程示範如何將商務邏輯加入時將商務邏輯層 (BLL) 和資料存取層 (DAL) 分開的並示範如何建立 BLL 自動化的單元測試。

在本教學課程中，您將完成下列工作：

- 建立儲存機制介面會宣告您所需的資料存取方法。
- 儲存機制類別中實作存放庫的介面。
- 建立呼叫儲存機制類別來執行資料存取功能的商務邏輯類別。
- 連接`ObjectDataSource`控制項至儲存機制類別而不是商務邏輯類別。
- 建立單元測試專案和其資料存放區中使用記憶體中集合的儲存機制類別。
- 建立您想要新增至商務邏輯的類別，然後執行測試，並觀察它失敗的商務邏輯的單元測試。
- 商務邏輯類別中實作商務邏輯，然後重新執行單元測試，並查看它傳遞。

您將使用*Departments.aspx*並*DepartmentsAdd.aspx*您在上一個教學課程中建立的頁面。

## <a name="creating-a-repository-interface"></a>建立儲存機制介面

您一開始先建立儲存機制介面。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

在  *DAL*資料夾中，建立新的類別檔案，將其命名*ISchoolRepository.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

介面會定義一種方法，針對每個 CRUD （建立、 讀取、 更新、 刪除） 在儲存機制類別中所建立的方法。

在 `SchoolRepository`類別內*SchoolRepository.cs*，表示這個類別會實作`ISchoolRepository`介面：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>建立商業邏輯類別

接下來，您將建立商業邏輯類別。 您執行這項操作，讓您可以新增將執行的商務邏輯`ObjectDataSource`控制項，雖然您不會尚未。 現在，新的商務邏輯類別只會執行相同的 CRUD 作業的儲存機制。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

建立新的資料夾並將它命名*BLL*。 (在真實世界應用程式中，商務邏輯層會通常實作為類別程式庫 — 個別的專案，但為了簡化本教學課程中，BLL 類別將會保留在專案資料夾中。)

在  *BLL*資料夾中，建立新的類別檔案，將其命名*SchoolBL.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

此程式碼會建立您所見相同的 CRUD 方法稍早在儲存機制類別中，但而不是直接存取 Entity Framework 方法，它會呼叫儲存機制類別方法。

保存至儲存機制類別的參考類別變數定義為介面類型，而且儲存機制類別會具現化的程式碼會包含在兩個建構函式。 無參數建構函式將會使用`ObjectDataSource`控制項。 它會建立的執行個體`SchoolRepository`您稍早建立的類別。 其他建構函式允許任何商業邏輯類別，以在任何實作存放庫介面的物件具現化的程式碼。

呼叫儲存機制類別和兩個建構函式的 CRUD 方法可使您能夠使用您選擇的任何後端資料存放區中的商務邏輯類別。 商務邏輯的類別不會不需要知道如何呼叫的類別會保存資料。 (這通常稱為*永續性無知*。)這有助於進行單元測試，因為您可以連接的項目做為簡單的儲存機制實作商務邏輯的類別為記憶體中`List`來儲存資料的集合。

> [!NOTE]
> 實體物件技術上來說，是仍不非持續，因為它們從具現化類別繼承自 Entity Framework 的`EntityObject`類別。 您可以使用完整的持續性無知*純舊 CLR 物件*，或*Poco*，來取代繼承的物件`EntityObject`類別。 使用 Poco 已超出本教學課程的範圍。 如需詳細資訊，請參閱 <<c0> [ 可測試性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) MSDN 網站上。)


現在您可以連接`ObjectDataSource`商務邏輯的控制項類別，而不是存放庫，並確認一切運作與以前一樣。

在*Departments.aspx*並*DepartmentsAdd.aspx*，變更出現的每個`TypeName="ContosoUniversity.DAL.SchoolRepository"`至`TypeName="ContosoUniversity.BLL.SchoolBL`"。 （有四個執行個體中所有。）

執行*Departments.aspx*並*DepartmentsAdd.aspx*頁面，以確認它們仍可如之前一樣。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>建立單元測試專案和儲存機制實作

將新的專案加入方案使用**測試專案**範本，並將它命名`ContosoUniversity.Tests`。

在 test 專案中加入參考`System.Data.Entity`並加入專案參考`ContosoUniversity`專案。

您現在可以建立包含單元測試，您將使用此儲存機制類別。 此存放庫的資料存放區會在類別中。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

在測試專案中，建立新的類別檔案，將其命名*MockSchoolRepository.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

此儲存機制類別具有相同的 CRUD 方法的直接存取 Entity Framework，但其使用`List`而不是與資料庫的記憶體中的集合。 這可讓您更容易設定和驗證商務邏輯類別的單元測試的測試類別。

## <a name="creating-unit-tests"></a>建立單元測試

**測試**專案範本建立虛設常式單元測試類別，和您的下一個工作是針對您想要加入商務邏輯類別的商務邏輯，加入單元測試方法來修改此類別。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso 大學任何個別的 instructor 只能是單一部門的系統管理員，而且您需要新增商務邏輯，以強制執行此規則。 您會開始將測試加入並執行測試以查看它們會失敗。 您將加入程式碼，然後重新執行測試，以查看其通過預期。

開啟*UnitTest1.cs*檔案，並新增`using`您 ContosoUniversity 專案中建立商業邏輯和資料存取層級的陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

取代`TestMethod1`方法，使用下列方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL`方法會建立您所建立的單元測試專案，然後將傳遞至商業邏輯類別的新執行個體的儲存機制類別的執行個體。 然後，該方法會將測試方法中，您可以使用三個部門使用的商務邏輯的類別。

測試方法確認商務邏輯的類別將會發生例外狀況，如果有人嘗試插入具有相同的系統管理員為現有的部門，新科系，或如果有人嘗試更新部門的系統管理員將它設定為 人員識別碼誰已是另一個科系的管理員。

您未建立例外狀況類別，因此將不會編譯此程式碼。 若要取得其進行編譯，以滑鼠右鍵按一下`DuplicateAdministratorException`，然後選取**產生**，然後**類別**。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

這會建立類別在測試專案中，您可以刪除其主專案中建立例外狀況類別之後。 和實作商務邏輯。

執行測試專案。 如預期般運作，測試就會失敗。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>新增商務邏輯，以進行測試

接下來，您將實作商務邏輯，可讓您更不可能將設定為部門的系統管理員已經是另一個科系的系統管理員的人。 您將會擲回的例外狀況，從商務邏輯層，並再攔截展示層中如果使用者編輯一個部門，並按**更新**之後選取已經是系統管理員的人。 （您也可以從下拉式清單中已經具有系統管理員身分先轉譯頁面上，移除講師，但這裡的目的是要使用商務邏輯層）。

開始建立當使用者嘗試進行講師的多個部門系統管理員，將會擲回的例外狀況類別。 在主專案中，建立新的類別檔案中*BLL*資料夾，其命名*DuplicateAdministratorException.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

現在刪除暫存*DuplicateAdministratorException.cs*您稍早建立的測試專案中，才能夠編譯的檔案。

在主專案中，開啟*SchoolBL.cs*檔案，並新增下列方法，其中包含驗證邏輯。 （程式碼是指您稍後將建立的方法）。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

您會呼叫這個方法，當您插入或更新`Department`以檢查另一個部門是否已經有相同的系統管理員的實體。

程式碼會呼叫方法來搜尋的資料庫`Department`具有相同的實體`Administrator`做為實體的屬性值所插入或更新。 如果有，程式碼會擲回例外狀況。 如果正在插入或更新的實體沒有，則需要任何驗證檢查`Administrator`如果在更新期間呼叫這個方法擲回值和任何例外狀況和`Department`找到相符項目實體`Department`正在更新實體。

呼叫新方法，從`Insert`和`Update`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

在  *ISchoolRepository.cs*，加入新的資料存取方法的下列宣告：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

在  *SchoolRepository.cs*，新增下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

在  *SchoolRepository.cs*，新增下列新的資料存取方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

此程式碼會擷取`Department`有指定的系統管理員的實體。 只有一個部門應該找 （如果有的話）。 不過，因為沒有條件約束內建資料庫，傳回的型別是集合萬一發現多個部門。

根據預設，物件內容從資料庫擷取實體時，它會追蹤的它們在其物件狀態管理員中。 `MergeOption.NoTracking`參數會指定此追蹤不會完成此查詢。 這是必要的因為查詢可能會傳回您想要更新的實際實體，然後您就無法將該實體。 例如，如果您編輯中的歷程記錄部門*Departments.aspx*頁面並保持不變的系統管理員，此查詢會傳回記錄部門。 如果`NoTracking`未設定，物件內容會在它的物件狀態管理員中已經有記錄 department 實體。 然後當您附加記錄 department 實體，就會重新建立從檢視狀態時，物件內容會擲回例外狀況指出`"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`。

(指定的替代方式為`MergeOption.NoTracking`，您可以建立新的物件內容，只針對此查詢。 因為新的物件內容會有它自己的物件狀態管理員，會有任何衝突時您呼叫`Attach`方法。 新的物件內容會與原始的物件內容中，共用中繼資料和資料庫的連線，因此此替代方法對效能造成負面影響會最少。 不過，引進了這裡所顯示的方式`NoTracking`選項，您會找到其他內容中很有用。 `NoTracking`選項將會討論在本系列稍後的教學課程中進一步。)

在測試專案中加入新的資料存取方法，來*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

此程式碼使用 LINQ 執行相同的資料選取範圍，`ContosoUniversity`專案存放庫會使用 LINQ to Entities 的。

再次執行測試專案。 測試這一次會成功。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>ObjectDataSource 例外狀況處理

在 `ContosoUniversity`專案中，執行*Departments.aspx*頁面上，並嘗試變更某部門的系統管理員已為另一個部門的系統管理員的人。 （請記住因為資料庫已預先載入含無效資料，您只能編輯您在本教學課程中，新增的部門）。您會取得下列的 [伺服器] 錯誤頁面：

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

您不想要查看這種錯誤頁面，因此，您需要新增錯誤處理程式碼的使用者。 開啟*Departments.aspx*並指定的處理常式`OnUpdated`事件的`DepartmentsObjectDataSource`。 `ObjectDataSource`開頭標記，現在類似下列範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

在  *Departments.aspx.cs*，新增下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

新增下列處理常式`Updated`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

如果`ObjectDataSource`嘗試執行更新時，控制會攔截到例外狀況，它就會傳遞例外狀況的事件引數 (`e`) 給這個處理常式。 在處理常式的程式碼會檢查例外狀況是否為重複的系統管理員的例外狀況。 如果是，程式碼會建立包含錯誤訊息的驗證程式控制項`ValidationSummary`控制項來顯示。

執行此頁面，並嘗試讓某位使用者的兩個部門系統管理員一次。 這次`ValidationSummary`控制項會顯示一則錯誤訊息。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

進行類似變更*DepartmentsAdd.aspx*頁面。 在  *DepartmentsAdd.aspx*，指定的處理常式`OnInserted`事件的`DepartmentsObjectDataSource`。 產生的標記將類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

在  *DepartmentsAdd.aspx.cs*，將相同`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

新增下列事件處理常式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

您現在可以測試*DepartmentsAdd.aspx.cs*頁面，確認它也可正確地處理嘗試讓一人以上一個部門的系統管理員。

如此即完成實作使用的儲存機制模式的簡介`ObjectDataSource`與 Entity Framework 的控制項。 如需儲存機制模式和可測試性的詳細資訊，請參閱 MSDN 白皮書[可測試性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx)。

在下列教學課程中，您會看到如何將排序和篩選功能給應用程式。

> [!div class="step-by-step"]
> [上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [下一頁](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
