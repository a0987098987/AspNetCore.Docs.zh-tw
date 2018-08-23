---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 2 部分 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a0b4acca93deee4fb514fa1bc3e2bd13490cf10e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833875"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>開始使用 Entity Framework 4.0 Database 中第一次和 ASP.NET 4 Web Form-第 2 部分
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource 控制項

在上一個教學課程中，您建立網站、 資料庫和資料模型。 在本教學課程中您使用`EntityDataSource`ASP.NET 提供為了讓您輕鬆地使用 Entity Framework 資料模型的控制項。 您會建立`GridView`控制項的顯示和編輯學生資料`DetailsView`控制項加入新的學生，和`DropDownList`選取部門 （這稍後會用來顯示相關聯的課程） 控制項。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

請注意，此應用程式中您不是輸入將驗證新增至頁面，更新資料庫，一些錯誤處理不會如需要在實際執行的應用程式的強固。 會保留本教學課程著重於 Entity Framework 會防止其得太長。 如需如何將這些功能新增至您的應用程式的詳細資訊，請參閱[驗證使用者輸入在 ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx)並[處理 ASP.NET 網頁和應用程式中的錯誤](https://msdn.microsoft.com/library/w16865z6.aspx)。

## <a name="adding-and-configuring-the-entitydatasource-control"></a>加入和設定 EntityDataSource 控制項

一開始要先設定`EntityDataSource`讀取控制項`Person`實體`People`實體集。

請確定您已開啟的 Visual Studio，以及您建立第 1 部分中，您正在使用的專案。 如果您建立資料模型，或自上次變更您對它之後，尚未建置專案，現在就建置專案。 資料模型變更都不會提供給設計工具之前建置專案。

建立新的網頁上使用**使用主版頁面的 Web Form**範本，並將它命名*Students.aspx*。

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

指定*Site.Master*作為主版頁面。 所有您建立這些教學課程中的頁面將會使用此主版頁面。

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

在 **來源**檢視、 新增`h2`標題來`Content`控制項，名為`Content2`，如下列範例所示：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

從**資料**索引標籤**工具箱**，拖曳`EntityDataSource`控制項加入頁面，將其放在標題下方，並變更至識別碼`StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

若要切換**設計**檢視，按一下 資料來源控制項的智慧標籤，然後按一下 **設定資料來源**來啟動**設定資料來源**精靈。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

在 **設定 ObjectContext**精靈步驟中，選取**SchoolEntities**做為值**具名的連接**，然後選取**SchoolEntities**作為**DefaultContainerName**值。 然後按 [下一步] 。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

注意： 如果您在此時收到下列對話方塊中，您必須建置專案後再繼續。

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

在 **設定資料選取範圍**步驟中，選取**人**做為值**EntitySetName**。 底下**選取**，請確定**選取 A** ll 核取方塊已選取。 然後選取 啟用更新及刪除選項。 當您完成時，按一下**完成**。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>設定以允許刪除的資料庫規則

您將會建立新的頁面，可讓使用者刪除的學生`Person`具有與其他資料表的三個關聯性的資料表 (`Course`， `StudentGrade`，和`OfficeAssignment`)。 根據預設，資料庫會阻止您刪除資料列中的`Person`如果有其中一種其他資料表的相關資料列。 首先，您可以手動刪除相關的資料列，或者您可以設定要自動刪除它們，當您刪除的資料庫`Person`資料列。 在本教學課程的學生記錄，您將設定要自動刪除相關的資料的資料庫。 因為學生可以有相關的資料列只能在`StudentGrade`資料表中，您必須設定三個關聯性的其中之一。

如果您使用*School.mdf*檔案下載會在本教學課程專案中，因為這些組態變更已完成，則可以略過本節。 如果您執行指令碼建立資料庫，請執行下列程序來設定資料庫。

在 **伺服器總管**，開啟您在第 1 部分中建立的資料庫圖表。 以滑鼠右鍵按一下之間的關聯性`Person`並`StudentGrade`（資料表之間的行），然後選取**屬性**。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

在**屬性** 視窗中，展開**INSERT 及 UPDATE 規格**並設定**DeleteRule**屬性設**Cascade**。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

儲存並關閉圖表。 如果系統詢問您是否想要更新資料庫，按一下**是**。

若要確定此模型會保留在與資料庫正在進行同步的記憶體中的實體，您必須在資料模型中設定對應規則。 開啟*SchoolModel.edmx*，以滑鼠右鍵按一下之間的關聯線`Person`並`StudentGrade`，然後選取**屬性**。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

在 **屬性**視窗中，將**End1 OnDelete**來**Cascade**。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

儲存並關閉*SchoolModel.edmx*檔案，然後再重新建置專案。

一般情況下，當資料庫變更時，您會有如何同步處理模型的幾個選擇：

- 針對特定種類的變更 （例如新增或重新整理資料表、 檢視或預存程序），以滑鼠右鍵按一下設計工具，然後選取**從資料庫更新模型**才能自動讓設計工具進行變更。
- 重新產生的資料模型。
- 進行手動更新，與下列類似。

在此情況下，您可能已重新產生模型，或重新整理資料表受到變更的關聯性，但是，您必須再次進行欄位名稱變更 (從`FirstName`至`FirstMidName`)。

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>使用 GridView 控制項來讀取和更新實體

在本節中，您將使用`GridView`控制項來顯示、 更新或刪除的學生。

開啟或切換至*Students.aspx* ，並切換至**設計**檢視。 從**資料**索引標籤**工具箱**，拖曳`GridView`右邊的控制項`EntityDataSource`控制，將其命名`StudentsGridView`，按一下 智慧標籤、，然後選取  **StudentsEntityDataSource**做為資料來源。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

按一下 **重新整理結構描述**(按一下**Yes**如果系統提示您確認)，然後按一下**啟用分頁**，**啟用排序**， **啟用編輯**，並**啟用刪除**。

按一下 **編輯資料行**。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

在 **選取的欄位**方塊中，刪除**PersonID**， **LastName**，以及**HireDate**。 您通常不記錄金鑰向使用者顯示、 雇用日期不是相關的學生，而且您會將這兩個部分的名稱放在一個欄位，因此您只需要一個 [名稱] 欄位。）

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

選取  **FirstMidName**欄位，然後按一下**將此欄位轉換為 TemplateField**。

執行相同的動作**EnrollmentDate**。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

按一下  **確定** ，然後切換至**來源**檢視。 剩餘的變更會直接在標記中執行的工作變得更容易。 `GridView`控制標記現在看起來如下列範例所示。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

第一個資料行之後 [命令] 欄位目前是範本欄位, 會顯示第一個名稱。 變更此範本欄位，看起來像下列範例的標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

在顯示模式中，兩個`Label`控制項顯示的第一個和最後一個名稱。 處於編輯模式，因此您可以變更的第一個和最後一個名稱，會提供兩個文字方塊。 如同`Label`控制項中的顯示模式，則使用`Bind`和`Eval`完全依照您的運算式會使用直接連接到資料庫的 ASP.NET 資料來源控制項。 唯一的差別是您在指定的實體屬性，而不是資料庫資料行。

最後一個資料行是範本欄位顯示 註冊日期。 變更此欄位，看起來像下列範例的標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

在顯示和編輯模式中，格式字串"{0，d}"會導致顯示 「 簡短日期 」 格式的日期。 （您的電腦可能設定為顯示此格式，不同於本教學課程中所示的畫面影像）。

請注意，在每個範本欄位，設計工具使用`Bind`運算式，預設值，但您已變更，以`Eval`中的運算式`ItemTemplate`項目。 `Bind`運算式可讓資料在`GridView`控制屬性，如果您需要存取程式碼中的資料。 在此頁面中，您不需要存取程式碼中，此資料，因此您可以使用`Eval`，這是更有效率。 如需詳細資訊，請參閱 <<c0> [ 將資料從資料控制項](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx)。

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>修改 EntityDataSource 控制項的標記，以改善效能

中的標記`EntityDataSource`控制項，移除`ConnectionString`並`DefaultContainerName`屬性，並將其取代為`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`屬性。 這是每次建立，您應該進行的變更`EntityDataSource`控制項，除非您需要使用不同的是硬式編碼在物件內容類別中的連接。 使用`ContextTypeName`屬性會提供下列優點：

- 效能較佳。 當`EntityDataSource`控制初始化的資料模型使用`ConnectionString`和`DefaultContainerName`屬性，它會執行其他工作，以載入每個要求的中繼資料。 這並非必要，如果您指定`ContextTypeName`屬性。
- 消極式載入會開啟產生的物件內容類別中的預設值 (例如`SchoolEntities`在本教學課程) 中 Entity Framework 4.0。 這表示，導覽屬性載入與相關資料會自動在需要時，權限。 消極式載入會更詳細說明稍後在本教學課程。
- 您已套用至物件內容類別的任何自訂項目 (在此情況下，`SchoolEntities`類別) 可供使用的控制項`EntityDataSource`控制項。 自訂物件內容類別是未涵蓋在本教學課程系列中的進階的主題。 如需詳細資訊，請參閱 <<c0> [ 擴充 Entity Framework 產生的型別](https://msdn.microsoft.com/library/dd456844.aspx)。

標記現在會類似下列範例 （的屬性順序可能會不同）：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening`屬性會參考到因為外部索引鍵資料行並不會公開為實體的屬性，在舊版的 Entity Framework 所需的功能。 目前的版本可讓您使用*外部索引鍵關聯*，這表示外部索引鍵屬性以外的所有多對多關聯的公開。 如果您的實體有外部索引鍵屬性，但沒有[複雜型別](https://msdn.microsoft.com/library/bb738472.aspx)，您可以將此屬性設為`False`。 未移除的屬性標記，因為預設值為`True`。 如需詳細資訊，請參閱 <<c0> [ 壓平合併的物件 (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx)。

執行網頁，您會看到一份學生和員工 （您將篩選只要的學生在下一個教學課程中）。 名字和姓氏會一起顯示。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

若要排序顯示，請按一下資料行名稱。

按一下 **編輯**中任何資料列。 文字方塊會顯示您可以在此變更的第一個和最後一個名稱。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**刪除**按鈕也可以運作。 針對具有註冊日期的資料列和資料列就會消失，請按一下 [刪除]。 （而不需要註冊日期的資料列代表講師，而且您可能會收到參考完整性錯誤。 在下一個教學課程中您會篩選此清單包含只要的學生。）

## <a name="displaying-data-from-a-navigation-property"></a>顯示從導覽屬性的資料

現在假設您想要知道多少課程每位學生在中註冊。 Entity Framework 會提供該資訊`StudentGrades`導覽屬性`Person`實體。 因為資料庫設計不允許註冊課程中，而不需要指派一個等級的學生，本教學課程中您可以假設在中，有一個資料列`StudentGrade`課程相關聯的資料表資料列等同於註冊過程中。 (`Courses`導覽屬性只能為講師。)

當您使用`ContextTypeName`屬性的`EntityDataSource`控制項，Entity Framework 會自動擷取資訊的導覽屬性存取該屬性時。 這就叫做*消極式載入*。 不過，這可能是效率不佳，因為它會導致個別呼叫需要每個階段的其他資訊的資料庫。 如果您需要從導覽屬性的資料所傳回的每個實體之`EntityDataSource`控制項，它會擷取相關的資料，以及實體本身的資料庫的單一呼叫中更有效率。 這就叫做*積極式載入*，而且您藉由設定指定導覽屬性的積極式載入`Include`屬性`EntityDataSource`控制項。

在  *Students.aspx*、 您想要顯示的每個學生的課程的數量，因此積極式載入是最佳選擇。 如果您已顯示所有的學生，但顯示數量的課程只會針對幾個 （這需要撰寫一些程式碼，除了標記以外） 上的消極式載入可能是較好的選擇。

開啟或切換至*Students.aspx*，切換至**設計**檢視中，選取`StudentsEntityDataSource`，然後在**屬性** 視窗中設定**Include**屬性，以**StudentGrades**。 (如果您想要取得多個導覽屬性，您可以指定其名稱以逗號分隔，例如**StudentGrades、 課程**。)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

若要切換**來源**檢視。 在 `StudentsGridView`控制項，最後一個之後`asp:TemplateField`項目，新增下列新的 範本 欄位：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

在 `Eval`運算式中，您可以參考導覽屬性`StudentGrades`。 此屬性包含集合，因為它有`Count`可用來顯示課程的學生是否已註冊的屬性。 在稍後的教學課程中，您會看到如何顯示從包含單一的實體，而不是集合的導覽屬性的資料。 (請注意，您無法使用`BoundField`來顯示資料的導覽屬性的項目。)

執行網頁，您現在會看到幾個課程中，已註冊學生。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>若要插入實體使用 DetailsView 控制項

下一個步驟是建立頁面，其中包含`DetailsView`控制項可讓您將新增新的學生。 關閉瀏覽器，然後再建立 新的網頁上使用*Site.Master*主版頁面。 將頁面命名*StudentsAdd.aspx*，然後切換到**來源**檢視。

加入下列標記取代的現有標記`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

此標記會建立`EntityDataSource`類似於您在所建立的控制項*Students.aspx*，但它可讓插入。 如同`GridView`控制項，繫結的欄位`DetailsView`完全依照其方式是根據資料控制項直接連接到資料庫，不同之處在於它們會參考實體屬性，會自動程式化控制項。 在此情況下，`DetailsView`控制項只適用於插入資料列，所以已設定的預設模式為`Insert`。

執行網頁，並加入新的學生。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

插入新的學生之後，但現在執行會發生任何事*Students.aspx*，您會看到新的學生資訊。

## <a name="displaying-data-in-a-drop-down-list"></a>在下拉式清單中顯示資料

在下列步驟中，您將資料繫結`DropDownList`控制項的實體集使用`EntityDataSource`控制項。 在本教學課程的這個部分，您不會執行許多與這份清單。 在後續的部分，不過，您將使用清單，讓使用者選取部門，以顯示相關聯的部門的課程。

建立名為的新網頁*Courses.aspx*。 在 **來源**檢視中，新增標題，以`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

在 **設計**檢視、 新增`EntityDataSource`控制項加入網頁一樣，但這次它命名為`DepartmentsEntityDataSource`。 選取 **部門**作為**EntitySetName**值，然後選取 僅**DepartmentID**並**名稱**屬性。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

從**標準**索引標籤**工具箱**，拖曳`DropDownList`控制項加入頁面，將其命名`DepartmentsDropDownList`，按一下智慧標籤，然後選取**選擇資料來源**至開始**資料來源組態精靈**。

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

在 **選擇資料來源**步驟中，選取**DepartmentsEntityDataSource**做為資料來源中，按一下 **重新整理結構描述**，然後選取 **名稱**為要顯示的資料欄位並**DepartmentID**做為值的資料欄位。 按一下 [確定 **Deploying Office Solutions**]。

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

與其他 ASP.NET 資料來源控制項，除非您正在指定實體和實體屬性，您可以使用資料繫結控制項使用 Entity Framework 的方法都是一樣的。

切換至**來源**檢視和加入 「 選取部門: 」 之前，立即`DropDownList`控制項。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

提醒您，變更的標記`EntityDataSource`控制項在此時取代`ConnectionString`並`DefaultContainerName`屬性與`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`屬性。 通常最好是等到您已建立連結到資料來源控制項變更之前，先將資料繫結控制項`EntityDataSource`控制標記，因為您進行變更之後，設計工具不會提供您與**重新整理結構描述**資料繫結控制項中的選項。

執行網頁，您可以從下拉式清單中選取部門。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

如此即完成簡介使用`EntityDataSource`控制項。 與這個控制項的工作與使用其他 ASP.NET 資料來源控制項，通常不不同，不同之處在於您實體和屬性，而不是資料表和資料行的參考。 唯一的例外狀況時，您想要存取導覽屬性。 在下一個教學課程中您會看到，語法搭配`EntityDataSource`篩選、 分組和排序資料時控制項也可能會與其他資料來源控制項不同。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-3.md)
