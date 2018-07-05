---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 7 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 14ec7a22e9e00ac793fa5a278243e6406a212903
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401801"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>開始使用 Entity Framework 4.0 Database 中第一次和 ASP.NET 4 Web Form 第 7 節
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>使用預存程序

在上一個教學課程中，您會實作每個階層的資料表繼承模式。 本教學課程會示範如何使用預存程序來進一步控制資料庫存取權。

Entity Framework 可讓您指定它應該使用預存程序來存取資料庫。 任何實體型別中，您可以指定要用於建立、 更新或刪除的實體，該類型的預存程序。 然後資料模型中，您可以將可用來執行工作，例如擷取的實體集的預存程序的參考。

使用預存程序是常見的需求，來存取資料庫。 在某些情況下的資料庫系統管理員可能需要的所有資料庫存取都經過基於安全性考量的預存程序。 在其他情況下，您可能要更新資料庫時，會使用 Entity Framework 的處理程序的一些建置商務邏輯。 比方說，只要刪除實體時您可能想要將它複製到封存資料庫。 或者，每次更新一個資料列時您可能想要提出變更所記錄的記錄資料表寫入的資料列。 您可以在每當 Entity Framework 會刪除實體，或更新實體，就會呼叫預存程序中執行這些類型的工作。

如同先前的教學課程中，您將不建立任何新的頁面。 相反地，您要變更 Entity Framework 存取資料庫的已建立的頁面部分的方式。

在本教學課程中，您將建立插入資料庫中的預存程序`Student`和`Instructor`實體。 您會將它們加入至資料模型，並指定 Entity Framework 應該使用它們來新增`Student`和`Instructor`到資料庫的實體。 您也會建立可用來擷取預存程序`Course`實體。

## <a name="creating-stored-procedures-in-the-database"></a>在資料庫中建立預存程序

(如果您使用*School.mdf*檔案從下載本教學課程專案中，您可以略過本節因為預存程序已經存在。)

在**伺服器總管**，展開*School.mdf*，以滑鼠右鍵按一下**預存程序**，然後選取**加入新的預存程序**。

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

複製下列 SQL 陳述式，並貼到 [預存程序] 視窗中，取代基本架構的預存程序。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` 實體有四個屬性： `PersonID`， `LastName`， `FirstName`，和`EnrollmentDate`。 資料庫自動產生的識別碼值和預存程序會接受其他三個參數。 使 Entity Framework 可以追蹤的它會保留在記憶體中的實體版本中，預存程序會傳回新的資料列的資料錄索引鍵的值。

儲存並關閉 [預存程序] 視窗。

建立`InsertInstructor`預存程序，以相同的方式，使用下列 SQL 陳述式：

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

建立`Update`預存程序`Student`和`Instructor`實體也。 (資料庫中已經有`DeletePerson`預存程序適用於兩者`Instructor`和`Student`實體。)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

在本教學課程中，您要將對應所有三個函式-insert、 update 和 delete-每個實體類型。 Entity Framework 第 4 版可讓您對應其中一個或兩個預存程序函式而不需要對應其他人，但有一個例外： 如果您對應更新函式，但不是刪除函式時，Entity Framework 將會擲回例外狀況時您嘗試刪除實體。 在 Entity Framework 3.5 版中，您不需要這麼多的彈性對應預存程序： 如果您對應一個函式您都必須對應所有三個。

若要建立的讀取，而不是更新資料的預存程序，建立一個會選取所有`Course`實體，使用下列 SQL 陳述式：

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>加入資料模型中的預存程序

在資料庫中，現在已定義的預存程序，但必須加入可讓它們使用 Entity Framework 資料模型。 開啟*SchoolModel.edmx*，以滑鼠右鍵按一下設計介面，然後選取**從資料庫更新模型**。 在**新增**索引標籤**選擇您的資料庫物件**對話方塊方塊中，展開**預存程序**，選取新建立的預存程序和`DeletePerson`預存程序，然後按一下**完成**。

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>對應的預存程序

在 資料模型設計工具中，以滑鼠右鍵按一下`Student`實體，然後選取**預存程序對應**。

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**對應詳細資料**視窗隨即出現，您可以在其中指定 Entity Framework 應該用於插入、 更新和刪除實體，此類型的預存程序。

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

設定**插入**函式**InsertStudent**。 視窗會顯示預存程序參數，其中每一個都必須對應至實體屬性的清單。 因為名稱相同，其中兩個會自動對應。 沒有名為實體屬性`FirstName`，因此您必須手動選取`FirstMidName`從下拉式清單會顯示可用的實體屬性。 (這是因為您已變更的名稱`FirstName`屬性設`FirstMidName`中第一個教學課程。)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

在同一個**對應詳細資料**視窗中，地圖`Update`函式`UpdateStudent`預存程序 (請確定您指定`FirstMidName`做為參數值`FirstName`，如您對`Insert`預存程序） 和`Delete`函式以`DeletePerson`預存程序。

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

遵循相同的程序，將對應的 insert、 update 和 delete 預存程序針對至講師`Instructor`實體。

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

讀取而不是更新資料的預存程序，您使用**模型瀏覽器**視窗，將預存程序對應至實體類型就會傳回。 在 資料模型設計工具中，以滑鼠右鍵按一下設計介面，然後選取**模型瀏覽器**。 開啟**SchoolModel.Store**節點，然後開啟**預存程序**節點。 然後以滑鼠右鍵按一下`GetCourses`預存程序，然後選取**加入函式匯入**。

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

在**加入函式匯入**對話方塊的 **會傳回集合的**選取**實體**，然後選取 `Course`為實體型別傳回。 當您完成時，按一下**確定**。 儲存並關閉 *.edmx*檔案。

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>使用插入、 更新和刪除預存程序

預存程序插入、 更新和刪除的資料由 Entity Framework 會自動加入資料模型及它們對應到適當的實體之後。 您現在可以執行*StudentsAdd.aspx*頁面上，且每次您建立新的學生，將會使用 Entity Framework`InsertStudent`預存程序，以新增新的資料列`Student`資料表。

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

執行*Students.aspx*頁面和新的學生會出現在清單中。

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

變更名稱以確認更新函式運作正常，然後再刪除 以確認刪除函式，適用於學生。

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>使用選取的預存程序

Entity Framework 不會自動執行預存程序這類`GetCourses`，而且您無法使用它們搭配`EntityDataSource`控制項。 若要使用它們，您呼叫它們從程式碼。

開啟*InstructorsCourses.aspx.cs*檔案。 `PopulateDropDownLists`方法使用 LINQ 到實體查詢以擷取所有的 course 實體，讓它可以循環清單，並判斷的哪些講師指派給哪些主機且未指派：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

下列程式碼取代此項：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

此頁面現在使用`GetCourses`預存程序擷取所有課程的清單。 執行頁面，確認其運作方式跟之前一樣。

(預存程序所擷取的實體導覽屬性可能不會自動填入這些實體，根據相關的資料`ObjectContext`預設設定。 如需詳細資訊，請參閱 <<c0> [ 載入相關物件](https://msdn.microsoft.com/library/bb896272.aspx)MSDN Library 中。)

在下一個教學課程中，您將了解如何使用動態資料功能，讓您更輕鬆地計劃和測試資料格式化與驗證規則。 而不是指定每個網頁 」 規則，例如資料格式字串和是必要欄位，您可以在資料模型中繼資料中指定這類規則，以及每個頁面上會自動套用。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-8.md)
