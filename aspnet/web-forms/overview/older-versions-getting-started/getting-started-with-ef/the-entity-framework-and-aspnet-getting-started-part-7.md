---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: 開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-部分 7 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: cb84f4f3e130fedb3e2f1a17d630767ff65bfa05
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 一部分 7
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>使用預存程序

在上一個教學課程中，您會實作資料表每個階層繼承模式。 本教學課程將說明如何使用預存程序更充分掌控資料庫存取權。

Entity Framework 可讓您指定它應該使用預存程序進行資料庫存取。 任何實體型別，您可以指定要用於建立、 更新或刪除實體，該類型的預存程序。 然後資料模型中，您可以加入預存程序可讓您執行工作，例如擷取的實體集的參考。

使用預存程序是常見的需求進行資料庫存取。 在某些情況下的資料庫系統管理員可能需要的所有資料庫存取都經過基於安全性考量的預存程序。 在其他情況下，您可能要部分更新資料庫時，會使用 Entity Framework 處理序中建立商務邏輯。 例如，每當刪除實體時您可以將它複製到封存資料庫。 或者，每次更新一個資料列時您可能想要寫入記錄到記錄資料表何人進行變更所記錄的資料列。 您可以在每當 Entity Framework 會刪除實體，或更新實體，就會呼叫預存程序中執行這些種類的工作。

如同上一個教學課程中，您將不建立任何新頁面。 相反地，您要變更 Entity Framework 部分已建立的頁面存取資料庫的方式。

在本教學課程中，您將建立插入資料庫中的預存程序`Student`和`Instructor`實體。 您會將它們加入至資料模型，並指定，Entity Framework 應該加入使用`Student`和`Instructor`實體資料庫。 您也會建立預存程序可讓您擷取`Course`實體。

## <a name="creating-stored-procedures-in-the-database"></a>在資料庫中建立預存程序

(如果您使用*School.mdf*檔案從下載與本教學課程專案中，您可以略過此區段因為已經存在的預存程序。)

在**伺服器總管**，依序展開*School.mdf*，以滑鼠右鍵按一下**預存程序**，然後選取**加入新的預存程序**。

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

複製下列 SQL 陳述式，並將它們貼至 [預存程序] 視窗中，取代基本架構預存程序。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` 實體有四個屬性： `PersonID`， `LastName`， `FirstName`，和`EnrollmentDate`。 此資料庫會自動產生的識別碼值和預存程序會接受其他三個參數。 預存程序會傳回新的資料列記錄索引鍵的值，讓 Entity Framework 可以追蹤的它會保存在記憶體中的實體版本。

儲存並關閉 [預存程序] 視窗。

建立`InsertInstructor`預存程序相同的方式，使用下列 SQL 陳述式：

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

建立`Update`預存程序`Student`和`Instructor`實體也。 (資料庫中已經有`DeletePerson`預存程序同時運作`Instructor`和`Student`實體。)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

在本教學課程中，您將對應所有三個函式-insert、 update 和 delete-每個實體類型。 Entity Framework 第 4 版可讓您對應其中一個或兩個函式預存程序不含對應其他人，但有一個例外： 如果您將對應更新函式，但不是刪除函式，Entity Framework 將會擲回例外狀況時您嘗試刪除實體。 在 Entity Framework 3.5 版中，您不需要這麼多的彈性對應預存程序： 如果您對應一個函式您都需要對應這三個。

若要建立的預存程序讀取，而不是更新資料，建立一個會選取所有`Course`實體，使用下列 SQL 陳述式：

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>加入資料模型中的預存程序

預存程序現在已定義在資料庫中，但是必須將它們加入至資料模型，以供使用，Entity Framework。 開啟*SchoolModel.edmx*，以滑鼠右鍵按一下設計介面，並選取**從資料庫更新模型**。 在**新增** 索引標籤**選擇您的資料庫物件**對話方塊方塊中，展開 **預存程序**，選取新建立的預存程序和`DeletePerson`預存程序，，然後按一下 **完成**。

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>對應的預存程序

在資料模型設計師中，以滑鼠右鍵按一下`Student`實體，然後選取**預存程序對應**。

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**對應詳細資料**視窗隨即出現，您可以在其中指定 Entity Framework 應該用於插入、 更新及刪除實體，此類型的預存程序。

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

設定**插入**函式以**InsertStudent**。 視窗會顯示預存程序參數，其中每一個都必須對應至實體屬性的清單。 因為名稱相同，其中兩個都會自動對應。 沒有名為的實體屬性`FirstName`，因此您必須手動選取`FirstMidName`從下拉式清單會顯示可用的實體屬性。 (這是因為變更名稱`FirstName`屬性`FirstMidName`第一個教學課程中。)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

在同一個**對應詳細資料**視窗中，對應`Update`函式以`UpdateStudent`預存程序 (請確定您指定`FirstMidName`做為參數值`FirstName`、 與您`Insert`預存程序） 和`Delete`函式以`DeletePerson`預存程序。

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

遵循相同的程序對應 insert、 update 和 delete 預存程序對講師至`Instructor`實體。

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

讀取而不是更新資料的預存程序，您使用**模型瀏覽器**視窗，將預存程序對應至實體類型就會傳回。 在資料模型設計師中，以滑鼠右鍵按一下設計介面，並選取**模型瀏覽器**。 開啟**SchoolModel.Store**節點然後再開啟**預存程序**節點。 以滑鼠右鍵按一下`GetCourses`預存程序，然後選取**新增函式匯入**。

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

在**新增函式匯入**對話方塊的 **傳回集合的**選取**實體**，然後選取`Course`作為實體類型傳回。 當您完成時，按一下**確定**。 儲存並關閉*.edmx*檔案。

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>使用 Insert、 Update 和 Delete 預存程序

預存程序插入、 更新和刪除的資料由 Entity Framework 自動加入資料模型及它們對應到適當的實體之後。 您現在可以執行*StudentsAdd.aspx*  頁面上，且每次您建立新的學生，將會使用 Entity Framework`InsertStudent`預存程序加入新的資料列`Student`資料表。

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

執行*Students.aspx*頁面和新的學生會出現在清單中。

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

變更名稱以確認更新函式可運作，然後再刪除 學生確認 delete 函式可正常運作。

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>使用選取的預存程序

Entity Framework 不會自動執行預存程序這類`GetCourses`，而且您無法使用它們與`EntityDataSource`控制項。 若要使用它們，您呼叫它們從程式碼。

開啟*InstructorsCourses.aspx.cs*檔案。 `PopulateDropDownLists`方法使用 LINQ-實體查詢以擷取所有課程實體，讓它可以循環清單，並判斷的哪些講師指派給，哪些是未指定：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

下列程式碼取代此項：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

頁面現在會使用`GetCourses`預存程序擷取所有課程的清單。 執行頁面，以驗證它的運作與以前一樣。

(預存程序所擷取的實體的導覽屬性可能不會自動填入這些實體，取決於與相關的資料`ObjectContext`預設設定。 如需詳細資訊，請參閱[載入相關物件](https://msdn.microsoft.com/library/bb896272.aspx)MSDN Library 中。)

在下一個教學課程中，您將學習如何使用動態資料功能，更輕鬆地程式和測試資料格式和驗證規則。 而不是指定每個網頁規則，例如資料格式字串，以及為必要欄位，您可以在資料模型中繼資料中指定這類規則，它們會自動套用每一頁上。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-8.md)
