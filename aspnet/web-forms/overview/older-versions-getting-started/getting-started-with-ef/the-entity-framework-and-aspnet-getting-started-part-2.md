---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: "開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-第 2 部分 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a549bd62bd78573c368784fd1529a830e009b0d4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 第 2 部分
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource 控制項

在上一個教學課程中您建立網站、 資料庫和資料模型。 在本教學課程使用`EntityDataSource`ASP.NET 提供以使其更容易使用的 Entity Framework 資料模型的控制項。 您將建立`GridView`控制項的顯示和編輯學生資料`DetailsView`控制項加入新的學生，和`DropDownList`控制項來選取 （這會顯示相關聯的課程稍後使用） 的部門。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

請注意，此應用程式中您將不會將加入的輸入的驗證，這些頁面會更新資料庫，某些錯誤處理不會那麼穩固，就會需要在實際執行的應用程式。 會將焦點放在 Entity Framework 的教學課程，而且可防止它取得太長。 如需有關如何將這些功能加入至您的應用程式的詳細資訊，請參閱[驗證使用者輸入中的 ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx)和[Error Handling in ASP.NET 網頁和應用程式](https://msdn.microsoft.com/library/w16865z6.aspx)。

## <a name="adding-and-configuring-the-entitydatasource-control"></a>加入和設定 EntityDataSource 控制項

藉由設定開始，您將`EntityDataSource`讀取控制項`Person`實體`People`實體集。

請確定您已開啟的 Visual Studio，而且您正在處理的專案建立在第 1 部分。 如果您沒有專案建立資料模型，或自上次變更您對它之後，現在就建置專案。 資料模型的變更不會提供至設計工具之前建置專案。

建立新的 web 網頁使用**使用主版頁面的 Web Form**範本，並將其命名*Students.aspx*。

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

指定*Site.Master*作為主版頁面。 所有您針對這些教學課程建立的頁面會使用此主版頁面。

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

在**來源**檢視、 新增`h2`標題至`Content`控制項，名為`Content2`，如下列範例所示：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

從**資料** 索引標籤**工具箱**，拖曳`EntityDataSource`控制項加入頁面、 將其放在標題下方，並變更 ID `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

切換至**設計**檢視中，按一下 資料來源控制項的智慧標籤，然後再按一下**設定資料來源**啟動**設定資料來源**精靈。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

在**設定 ObjectContext**精靈步驟中，選取**SchoolEntities**做為值**名為連線**，然後選取**SchoolEntities**為**DefaultContainerName**值。 然後按 [下一步] 。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

注意： 如果您在此時取得下列對話方塊中，您必須建置專案後再繼續。

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

在**設定資料選取範圍**步驟中，選取**人員**做為值**EntitySetName**。 在下**選取**，請確定**選取 A** ll 核取方塊已選取。 然後選取要啟用更新和刪除的選項。 當您完成時，按一下**完成**。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>設定資料庫以允許刪除的規則

您將會建立新的頁面，可讓使用者可以刪除從學生`Person`具有三個其他資料表的關聯性的資料表 (`Course`， `StudentGrade`，和`OfficeAssignment`)。 根據預設，資料庫會導致您無法刪除資料列中的`Person`是否有相關的資料列中的其他資料表。 首先，您可以手動刪除相關的資料列，或您可以將資料庫設定為自動刪除它們，當您刪除`Person`資料列。 在本教學課程的學生記錄，您會將資料庫設定為自動刪除相關的資料。 學生可以相關資料列，因為只有`StudentGrade`資料表中，您必須設定三個關聯性的其中之一。

如果您使用*School.mdf*檔案下載隨附此教學課程之專案中，因為這些組態變更已完成，則可以略過本節。 如果您執行指令碼建立資料庫，請執行下列程序來設定資料庫。

在**伺服器總管**，開啟您在第 1 部分中建立的資料庫圖表。 以滑鼠右鍵按一下之間的關聯性`Person`和`StudentGrade`（資料表之間的行），然後選取**屬性**。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

在**屬性**視窗中，展開**INSERT 及 UPDATE 規格**並設定**DeleteRule**屬性**Cascade**。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

儲存並關閉圖表。 如果系統會詢問您是否想要更新資料庫，按一下**是**。

若要確定此模型會保留在與資料庫正在進行同步處理記憶體中的實體，您必須設定資料模型中的對應規則。 開啟*SchoolModel.edmx*，以滑鼠右鍵按一下之間的關聯線`Person`和`StudentGrade`，然後選取**屬性**。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

在**屬性**視窗中，將**End1 OnDelete**至**Cascade**。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

儲存並關閉*SchoolModel.edmx*檔案，然後再重建專案。

一般情況下，當資料庫變更時，會有幾個選擇如何同步處理模型：

- 針對特定種類的變更 （例如加入或重新整理資料表、 檢視或預存程序），以滑鼠右鍵按一下設計工具，然後選取**從資料庫更新模型**自動讓設計工具進行變更。
- 重新產生資料模型。
- 請手動更新，如下所示。

在此情況下，您無法重新產生模型，或重新整理資料表受到變更的關聯性，但您必須再執行該欄位名稱變更，然後再次 (從`FirstName`至`FirstMidName`)。

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>使用讀取及更新實體的 GridView 控制項

本節中，您將使用`GridView`控制項來顯示、 更新或刪除學生。

開啟或切換至*Students.aspx*並切換至**設計**檢視。 從**資料** 索引標籤**工具箱**，拖曳`GridView`右邊的控制項`EntityDataSource`控制項，其命名`StudentsGridView`，按一下智慧標籤，然後再選取**StudentsEntityDataSource**做為資料來源。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

按一下**重新整理結構描述**(按一下**是**如果系統提示您確認)，然後按一下 **啟用分頁**，**啟用排序**， **啟用編輯**，和**啟用刪除**。

按一下**編輯資料行**。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

在**選取的欄位**方塊中，刪除**PersonID**， **LastName**，和**HireDate**。 您通常不會顯示記錄的索引鍵的使用者、 雇用日期相關的學生，並不讓您只需要一個名稱欄位，您會在一個欄位中，放置這兩組件的名稱。）

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

選取**FirstMidName**欄位，然後按一下**這個欄位轉換為 TemplateField**。

執行相同**EnrollmentDate**。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

按一下**確定**後再切換至**來源**檢視。 將會比較容易進行直接在標記中剩餘的變更。 `GridView`控制標記現在看起來類似下列的範例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

第一個資料行之後，[命令] 欄位會是目前範本欄位會顯示第一個名稱。 變更，此範本欄位標記成類似下列的範例：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

在顯示模式中，兩個`Label`控制項顯示的第一個和最後一個名稱。 在編輯模式，讓您可以變更的第一個和最後一個名稱，會提供兩個文字方塊。 如同`Label`控制項中的顯示模式，您使用`Bind`和`Eval`完全相同的運算式會使用直接連接到資料庫的 ASP.NET 資料來源控制項。 唯一的差別在於您會指定實體屬性，而不是資料庫資料行。

最後一個資料行是範本欄位會顯示註冊日期。 變更此欄位來看起來像下列的範例標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

在顯示和編輯模式中，格式字串"{0，d}"會將日期顯示 「 簡短日期 」 格式。 （您的電腦可能設定為顯示此格式與本教學課程中所顯示的螢幕影像不同）。

請注意，在每個範本欄位中，設計工具使用`Bind`運算式預設值，但是您已變更，如果要`Eval`中的運算式`ItemTemplate`項目。 `Bind`運算式可讓資料在`GridView`控制屬性，以防您需要存取程式碼中的資料。 在此頁面中，您不需要存取程式碼中，此資料，因此您可以使用`Eval`，這會更有效率。 如需詳細資訊，請參閱[取得您的資料超出資料控制項](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx)。

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>EntityDataSource 控制項的標記，以改善效能的修訂

中的標記`EntityDataSource`控制項，移除`ConnectionString`和`DefaultContainerName`屬性，並將其取代為`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`屬性。 這是每次建立，您應該進行的變更`EntityDataSource`控制，除非您需要使用不同的是硬式編碼物件內容類別中的連線。 使用`ContextTypeName`屬性會提供下列優點：

- 效能較佳。 當`EntityDataSource`控制項初始化資料模型使用`ConnectionString`和`DefaultContainerName`屬性，它會執行其他工作，以載入每個要求的中繼資料。 這並非必要，如果您指定`ContextTypeName`屬性。
- 產生的物件內容類別中的預設會開啟消極式載入 (例如`SchoolEntities`在本教學課程) 在 Entity Framework 4.0。 這表示導覽屬性會載入與相關資料會自動在需要時，權限。 消極式載入更詳細說明，稍後在本教學課程。
- 已套用到物件內容類別的任何自訂 (在此情況下，`SchoolEntities`類別) 可供使用的控制項`EntityDataSource`控制項。 自訂物件內容類別是進階的主題未涵蓋在本教學課程系列。 如需詳細資訊，請參閱[擴充實體架構產生的型別](https://msdn.microsoft.com/library/dd456844.aspx)。

標記現在會類似下列的範例 （屬性順序可能會不同）：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening`屬性是指因為外部索引鍵資料行並不會公開為實體的屬性，在舊版的 Entity Framework 所需的功能。 目前的版本會讓您能夠使用*外部索引鍵關聯*，這表示外部索引鍵屬性以外的所有多對多關聯的公開。 如果您的實體有外部索引鍵屬性，且沒有[複雜型別](https://msdn.microsoft.com/library/bb738472.aspx)，您可以將這個屬性設為`False`。 未移除的屬性標記，因為預設值為`True`。 如需詳細資訊，請參閱[簡維物件 (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx)。

執行網頁，您會看到一份學生的員工 （您將篩選只學生在下一個教學課程）。 名字和姓氏會一起顯示。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

若要排序顯示，請按一下資料行名稱。

按一下**編輯**任何資料列中。 文字方塊會顯示您可以在此變更的第一個和最後一個名稱。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**刪除**按鈕也運作。 針對具有註冊日期的資料列和資料列就會消失，請按一下 [刪除]。 （沒有註冊日期資料列代表講師和您可能會收到參考完整性錯誤。 在下一個教學課程中您會篩選這份清單包含只學生。）

## <a name="displaying-data-from-a-navigation-property"></a>顯示資料的導覽屬性

現在假設您想要知道多少課程每一位學生在中註冊。 Entity Framework 提供該資訊`StudentGrades`導覽屬性`Person`實體。 因為資料庫設計不允許一位學生課程中註冊而不需要指派的等級，本教學課程中，您可以假設該中都有一個資料列`StudentGrade`課程相關聯的資料表資料列等同於註冊過程中。 (`Courses`導覽屬性是只對講師。)

當您使用`ContextTypeName`屬性`EntityDataSource`控制項，Entity Framework 自動擷取資訊的導覽屬性存取該屬性時。 這稱為*消極式載入*。 但是，這可能會沒有效率，因為它會產生個別呼叫需要每個階段的其他資訊的資料庫。 如果您需要資料的導覽屬性所傳回的每個實體的`EntityDataSource`控制項，它會擷取相關的資料，以及實體本身的資料庫的單一呼叫中更有效率。 這稱為*積極式載入*，而且您藉由設定指定之瀏覽屬性的積極式載入`Include`屬性`EntityDataSource`控制項。

在*Students.aspx*、 您想要顯示每一位學生課程次數，因此積極式載入是最佳選擇。 如果您顯示所有學生但顯示課程數目僅供少數幾個 （即需要撰寫一些程式碼，除了標記之外），延遲載入可能比較好的選擇。

開啟或切換至*Students.aspx*，切換至**設計**檢視中，選取`StudentsEntityDataSource`，然後在**屬性**視窗中，將**Include**屬性**StudentGrades**。 (如果您想要取得多個導覽屬性，您可以指定其名稱以逗號分隔，例如**StudentGrades、 課程**。)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

切換至**來源**檢視。 在`StudentsGridView`控制項，最後一個之後`asp:TemplateField`項目，加入下列新的範本欄位：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

在`Eval`運算式中，您可以參考導覽屬性`StudentGrades`。 這個屬性包含集合，因為它有`Count`屬性可讓您顯示的課程中註冊學生數目。 在稍後的教學課程中，您會看到從包含單一實體而不是集合導覽屬性的資料顯示方式。 (請注意，您不能使用`BoundField`從導覽屬性顯示資料的項目。)

執行網頁，您會立即看到多少課程學生註冊中。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>使用 DetailsView 控制項插入實體

下一個步驟是建立頁面，具有`DetailsView`可讓您新增新的學生的控制項。 關閉瀏覽器，然後建立新的 web 網頁使用*Site.Master*主版頁面。 將頁面命名*StudentsAdd.aspx*，然後切換到**來源**檢視。

加入下列標記取代的現有標記`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

這個標記會建立`EntityDataSource`類似您所建立的控制項*Students.aspx*，不過它可讓插入。 如同`GridView`控制，繫結的欄位`DetailsView`完全相同之處在於它們參考的實體屬性，其方式是直接連接到資料庫的資料控制項，會自動程式化控制項。 在此情況下，`DetailsView`控制項只能用來插入資料列，因此您已設定的預設模式為`Insert`。

執行網頁，並加入新的學生。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

插入新的學生之後，但現在執行會發生任何事*Students.aspx*，您會看到新的學生資訊。

## <a name="displaying-data-in-a-drop-down-list"></a>在下拉式清單中顯示資料

下列步驟在您將會進行資料繫結`DropDownList`的實體集使用的控制項`EntityDataSource`控制項。 在本教學課程的這個部分，不會執行大部分的這份清單。 在後續的組件，不過，您將使用清單，讓使用者選取以顯示課程部門相關聯的部門。

建立名為的新網頁*Courses.aspx*。 在**來源**檢視、 加入標題以`Content`控制項名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

在**設計**檢視、 新增`EntityDataSource`控制項加入網頁一樣，但這次命名`DepartmentsEntityDataSource`。 選取**部門**為**EntitySetName**值，並只選取**DepartmentID**和**名稱**屬性。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

從**標準** 索引標籤**工具箱**，拖曳`DropDownList`控制項加入頁面中，它`DepartmentsDropDownList`，按一下智慧標籤，然後選取**選擇資料來源**至啟動**資料來源組態精靈**。

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

在**選擇資料來源**步驟中，選取**DepartmentsEntityDataSource**做為資料來源中，按一下 **重新整理結構描述**，然後選取**名稱**做為要顯示的資料欄位和**DepartmentID**為數值資料欄位。 按一下 [確定 **Deploying Office Solutions**]。

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

您使用資料繫結控制項使用 Entity Framework 的方法與 ASP.NET 中的其他資料來源控制項，除了您要指定實體和實體屬性都相同。

切換至**來源**檢視和加入 「 選取部門: 」 之前，立即`DropDownList`控制項。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

提醒您，變更的標記`EntityDataSource`控制項在此時取代`ConnectionString`和`DefaultContainerName`屬性與`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`屬性。 它通常最好是建立資料繫結控制項變更之前，先連結到資料來源控制項之後，等候直到`EntityDataSource`控制項的標記，因為您進行變更之後，在設計工具不會提供您與**重新整理結構描述**資料繫結控制項中的選項。

執行網頁，您可以從下拉式清單中選取部門。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

如此即完成簡介使用`EntityDataSource`控制項。 與這個控制項的工作是使用其他 ASP.NET 資料來源控制項通常沒有不同，不同之處在於您的實體和屬性，而非資料表和資料行的參考。 唯一的例外狀況時，您想要存取導覽屬性。 在下一個教學課程中您會看到語法使用`EntityDataSource`控制項也可能和其他資料來源控制項時篩選、 分組和排序資料。

>[!div class="step-by-step"]
[上一頁](the-entity-framework-and-aspnet-getting-started-part-1.md)
[下一頁](the-entity-framework-and-aspnet-getting-started-part-3.md)
