---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "在 ASP.NET MVC 應用程式 (10-8) 中實作繼承與 Entity Framework |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 54e46c6f996b6fe86a227c851562e61678b02780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>實作 ASP.NET MVC 應用程式 (10-8) 中的 Entity Framework 的繼承
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。 您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中，您會處理並行存取例外狀況。 本教學課程會示範如何實作資料模型中的繼承。

在物件導向程式設計中，您可以使用繼承，以消除重複的程式碼。 在此教學課程中，您要變更`Instructor`和`Student`類別，讓它們衍生自`Person`基底類別，其中包含屬性，例如`LastName`通用講師和學生。 您將不會加入或變更任何網頁，但是您要變更的一些程式碼，而且這些變更將會自動反映在資料庫中。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>資料表每個階層與資料表每個類型的繼承

在物件導向程式設計中，您可以使用繼承，讓您更輕鬆地使用相關的類別。 例如，`Instructor`和`Student`中的類別`School`資料模型共用數個屬性，這會導致多餘的程式碼：

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

假設您想要消除多餘的程式碼所共用的屬性`Instructor`和`Student`實體。 您可以建立`Person`基底類別，其中包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

有幾種的方式可能會表示此繼承結構，在資料庫中。 您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。 某些資料行可以只對講師套用 (`HireDate`)，有些則只能學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。 一般而言，您就必須*鑑別子*指出哪種類型的每個資料列的資料行代表。 例如，鑑別子資料行可能會有 「 講師 」 講師和 「 學生 」 學生版。

![資料表每個 hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

這種模式的單一資料庫資料表產生實體繼承結構會呼叫*資料表每個階層*(TPH) 繼承。

替代方法是讓資料庫起來更繼承結構。 例如，您無法在只有名稱欄位`Person`資料表，並有個別`Instructor`和`Student`日期欄位的資料表。

![資料表每個 type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

此模式的資料庫資料表的每個實體類別就稱為*資料表每個型別*(TPT) 繼承。

因為 TPT 模式可能會導致複雜的聯結查詢 TPH 繼承模式通常 TPT 繼承的模式，比在 Entity framework 提供更佳的效能。 本教學課程會示範如何實作 TPH 繼承。 您將會執行，藉由執行下列步驟：

- 建立`Person`類別，並變更`Instructor`和`Student`類別衍生自`Person`。
- 資料庫內容類別加入至資料庫模型對應程式碼。
- 變更`InstructorID`和`StudentID`整個專案的參考`PersonID`。

## <a name="creating-the-person-class"></a>建立個人類別

 注意： 您將無法建立下列類別，除非您更新使用這些類別的控制站之後，編譯專案。 

在*模型*資料夾中，建立*Person.cs*和範本程式碼取代為下列程式碼：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

在*Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。 程式碼看起來像下列的範例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

類似變更*Student.cs*。 `Student`類別看起來像下列的範例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>加入至模型的人員實體類型

在*SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

這是所有 Entity Framework 必須以設定資料表每個階層繼承。 您會發現，當資料庫是重新建立，它會有`Person`資料表取代`Student`和`Instructor`資料表。

## <a name="changing-instructorid-and-studentid-to-personid"></a>變更 PersonID InstructorID 和 StudentID

在*SchoolContext.cs*，講師課程對應陳述式中變更`MapRightKey("InstructorID")`至`MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

這項變更不是必要的。只會變更 InstructorID 中的資料行的多對多聯結資料表的名稱。 如果您保留 InstructorID 相同的名稱時，應用程式就仍然正常運作。 以下是 已完成*SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

接下來您必須變更`InstructorID`至`PersonID`和`StudentID`至`PersonID`整個專案***除了***的時間戳記移轉檔案中*移轉*資料夾。 若要這樣做，您要尋找並開啟予以變更，需要的檔案，然後在開啟的檔案上執行全域的變更。 中唯一的檔案*移轉*應該變更的資料夾是*Migrations\Configuration.cs。*

1. > [!IMPORTANT]
 > 關閉所有開啟的檔案，Visual Studio 中開始。
2. 按一下**尋找和取代-尋找所有檔案**中**編輯**功能表上，並在專案中包含的所有檔案然後搜尋`InstructorID`。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 開啟的每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_.cs*移轉檔案中*移轉*資料夾中，按兩下每個檔案的一條線。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. 開啟**檔案中取代**對話方塊，並變更**查看**至**所有開啟的文件**。
5. 使用**檔案中取代**對話方塊，即可變更所有`InstructorID`至`PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. 尋找專案中所包含的所有檔案`StudentID`。
7. 開啟的每個檔案**尋找結果**視窗***除了***&lt;時間戳記&gt;*\_\*.cs*移轉檔案在*移轉*資料夾中，按兩下每個檔案的一條線。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. 開啟**檔案中取代**對話方塊，並變更**查看**至**所有開啟的文件**。
9. 使用**檔案中取代**對話方塊，即可變更所有`StudentID`至`PersonID`。   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. 建置專案。

(請注意，這將示範*缺點*的`classnameID`模式來命名主索引鍵。 如果您有不含類別名稱，前面加上命名主索引鍵識別碼*沒有*重新命名為必要操作現在。)

## <a name="create-and-update-a-migrations-file"></a>建立和更新的移轉檔案

在 封裝管理員主控台 (PMC)，輸入下列命令：

`Add-Migration Inheritance`

執行`Update-Database`PMC 命令。 命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。 您會收到下列錯誤：

*與外部索引鍵條件約束的 ALTER TABLE 陳述式衝突 」 FK\_dbo。部門\_dbo。人員\_PersonID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。Person"，'PersonID' 資料行。*

開啟*移轉\&lt; 時間戳記&gt;\_Inheritance.cs*和取代`Up`方法取代下列程式碼：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

執行`update-database`命令一次。

> [!NOTE]
> 很可能在移轉資料和進行結構描述變更時，取得其他錯誤。 如果您移轉時發生錯誤無法解決，您可以繼續這個教學課程中的連接字串，進而*Web.config*檔案或刪除資料庫。 最簡單的方法是在資料庫重新命名*Web.config*檔案。 例如，將資料庫名稱變更為 CU\_測試下列範例所示：
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> 使用新資料庫時，沒有資料移轉，而`update-database`命令是更有可能能順利完成。 如需有關如何刪除資料庫的指示，請參閱[如何卸除資料庫，從 Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。 如果您採用這個方法，才能繼續進行本教學課程，略過部署步驟在本教學課程結尾處之後自動執行移轉時，已部署的站台會得到相同的錯誤。 如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 之一。


## <a name="testing"></a>測試

執行網站，然後再次嘗試各種不同的頁面。 運作一切正常相同與以前一樣。

在**伺服器總管 中，**展開**SchoolContext**然後**資料表**，而且您會看到的**學生**和**講師**資料表已取代**人員**資料表。 展開**人員**資料表，而且您看到它有使用中的資料行的所有**學生**和**講師**資料表。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

以滑鼠右鍵按一下 [人員] 資料表，然後按一下**顯示資料表資料**看見鑑別子資料行。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

下圖說明新的 School 資料庫的結構：

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>總結

現在的實作每個階層的資料表繼承`Person`， `Student`，和`Instructor`類別。 如需有關這個主題以及其他的繼承結構的詳細資訊，請參閱[繼承對應策略](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi 部落格上。 在下一個教學課程中，您會看到一些方式來實作的儲存機制和工作模式的單位。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
