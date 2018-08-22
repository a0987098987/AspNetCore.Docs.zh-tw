---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC 應用程式 (10 的 8) 中實作 Entity Framework 的繼承 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830747"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>ASP.NET MVC 應用程式 (10 的 8) 中實作 Entity Framework 的繼承
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


先前的教學課程中，您會處理並行例外狀況。 本教學課程將示範如何在資料模型中實作繼承。

在物件導向程式設計中，您可以使用繼承，以消除冗餘的程式碼。 在本教學課程中，您將變更 `Instructor` 和 `Student` 類別，讓它們衍生自 `Person` 基底類別，而此基底類別包含講師和學生通用的屬性，例如 `LastName`。 您不會新增或變更任何網頁，但是您將變更一些程式碼，這些變更將會自動反映在資料庫中。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>每個階層的資料表與表 (Table-per-type) 繼承

在物件導向程式設計中，您可以使用繼承，讓您輕鬆地使用相關的類別。 例如，`Instructor`並`Student`中的類別`School`資料模型會共用多個屬性，這會導致多餘的程式碼：

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

假設您想要針對 `Instructor` 和 `Student` 實體所共用的屬性消除多餘的程式碼。 您可以建立`Person`基底類別，其中只包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

有幾種方式可以在資料庫中表示此繼承結構。 您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。 某些資料行僅適用於講師 (`HireDate`)，有些只適用於學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。 一般而言，您必須*鑑別子*指出哪種類型的每個資料列的資料行代表。 例如，鑑別子資料行的 "Instructor" 代表講師，而 "Student" 代表學生。

![資料表每個 hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

從單一資料庫資料表產生實體繼承結構的這個模式稱為*每個階層的資料表*(TPH) 繼承。

替代方法是讓資料庫看起來更像繼承結構。 例如，您可以在只有 [名稱] 欄位`Person`資料表，並有不同`Instructor`和`Student`包含日期欄位的資料表。

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

這種模式的建立資料庫資料表，每個實體類別就稱為*每個類型的資料表*(TPT) 繼承。

TPH 繼承模式通常會提供更佳的效能比起 TPT 繼承模式，Entity Framework 中因為 TPT 模式可能會導致複雜的聯結查詢。 本教學課程將示範如何實作 TPH 繼承。 您將會先執行下列步驟：

- 建立`Person`類別，並變更`Instructor`並`Student`類別來衍生自`Person`。
- 加入資料庫內容類別中的模型與資料庫對應程式碼。
- 變更`InstructorID`並`StudentID`整個專案，以參考`PersonID`。

## <a name="creating-the-person-class"></a>建立 Person 類別

 注意： 您無法再建立以下的類別，除非您更新使用這些類別的控制站之後，編譯專案。 

在 *模型*資料夾中，建立*Person.cs*並以下列程式碼取代範本程式碼：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

在  *Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。 程式碼看起來應該如下列範例所示：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

進行類似變更*Student.cs*。 `Student`類別看起來如下列範例所示：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>將 Person 實體類型加入模型

在  *SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

這就是 Entity Framework 為了設定單表繼承而必須執行的所有工作。 如您所見，當資料庫是重新建立，它會有`Person`資料表的位置`Student`和`Instructor`資料表。

## <a name="changing-instructorid-and-studentid-to-personid"></a>變更 PersonID InstructorID 和 StudentID

在  *SchoolContext.cs*，在講師-課程對應陳述式中，變更`MapRightKey("InstructorID")`到`MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

這項變更不是必要的;此外，它只會變更 InstructorID 中的資料行的多對多聯結資料表的名稱。 如果您保留 InstructorID 同名時，應用程式就仍然會正確運作。 以下是 已完成*SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

接下來您需要變更`InstructorID`來`PersonID`並`StudentID`來`PersonID`整個專案***除外***中的時間戳記的移轉檔案*移轉*資料夾。 若要這樣做，您要尋找並開啟要變更的檔案，然後在開啟的檔案上執行全域變更。 中唯一的檔案*移轉*資料夾，您應該變更是*Migrations\Configuration.cs。*

1. > [!IMPORTANT]
   > 關閉所有開啟的檔案，在 Visual Studio 中開始。
2. 按一下 **尋找和取代-尋找所有的檔案**中**編輯**功能表，然後在專案中包含的所有檔案然後搜尋`InstructorID`。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 開啟每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_.cs*移轉檔案中的*移轉*資料夾中，按兩下每個檔案的一條線。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. 開啟**檔案中取代**對話方塊，並變更**查看**來**所有開啟的文件**。
5. 使用**檔案中取代**對話方塊，即可變更所有`InstructorID`至 `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. 尋找專案中所包含的所有檔案`StudentID`。
7. 開啟每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_\*.cs*移轉檔案在 *移轉*資料夾中，按兩下每個檔案的一條線。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. 開啟**檔案中取代**對話方塊，並變更**查看**來**所有開啟的文件**。
9. 使用**檔案中取代**對話方塊，即可變更所有`StudentID`至`PersonID`。   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. 建置專案。

(請注意，此範例示範*缺點*的`classnameID`模式來命名主索引鍵。 如果您有名稱主索引鍵識別碼不含類別名稱，前面加上*沒有*重新命名可能需要立即。)

## <a name="create-and-update-a-migrations-file"></a>建立和更新移轉檔案

在 套件管理員主控台 (PMC)，請輸入下列命令：

`Add-Migration Inheritance`

執行`Update-Database`PMC 命令。 命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。 您會收到下列錯誤：

*ALTER TABLE 陳述式與 FOREIGN KEY 條件約束 」 FK\_dbo。部門\_dbo。人員\_PersonID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。Person"資料行 'PersonID'。*

開啟*移轉\&l t; 時間戳記&gt;\_Inheritance.cs* ，並取代`Up`為下列程式碼的方法：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

執行`update-database`命令一次。

> [!NOTE]
> 可以移轉資料和制定的結構描述變更時，取得其他錯誤。 如果您收到的移轉錯誤無法解決，您可以藉由變更連接字串中的繼續進行本教學課程*Web.config*檔案，或刪除資料庫。 簡單的方法是在資料庫重新命名*Web.config*檔案。 例如，將資料庫名稱變更為 CU\_測試下列範例所示：
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> 使用新資料庫時，沒有資料移轉，而`update-database`命令是很有可能能順利完成。 如需有關如何刪除資料庫的指示，請參閱 <<c0> [ 如何從 Visual Studio 2012 中卸除資料庫](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。 如果您要繼續進行本教學課程採用這個方法，略過部署步驟結尾的這個教學課程中，由於已部署的站台自動執行移轉時，會取得相同的錯誤。 如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 其中之一。


## <a name="testing"></a>測試

執行網站，然後嘗試各種頁面。 一切項目的運作與之前一樣。

在 [**伺服器總管] 中，** 展開**SchoolContext** ，然後**資料表**，，您會看到**學生**和**講師**已被取代的資料表**人員**資料表。 依序展開**Person**資料表，而且您看到它有使用中的資料行的所有**學生**並**講師**資料表。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

以滑鼠右鍵按一下 Person 資料表，然後按一下 [顯示資料表資料] 以查看鑑別子資料行。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

下圖說明新的 School 資料庫的結構：

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>總結

現在針對實作每個階層的資料表繼承`Person`， `Student`，和`Instructor`類別。 如需有關這個主題以及其他繼承結構的詳細資訊，請參閱[繼承對應策略](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi 部落格上。 在下一個教學課程中，您會看到一些方式來實作儲存機制和工作單元模式。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
