---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "在 ASP.NET MVC 5 應用程式 (11 12 個) 中實作繼承與 Entity Framework 6 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e6ee3f9c055a15b13c27f94675006b9a7e804f1b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>實作 ASP.NET MVC 5 應用程式 (11 12 個) 的 Entity Framework 6 的繼承
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在上一個教學課程中，您會處理並行存取例外狀況。 本教學課程會示範如何實作資料模型中的繼承。

在物件導向程式設計中，您可以使用[繼承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))以便[程式碼重複使用](http://en.wikipedia.org/wiki/Code_reuse)。 在此教學課程中，您要變更`Instructor`和`Student`類別，讓它們衍生自`Person`基底類別，其中包含屬性，例如`LastName`通用講師和學生。 您將不會加入或變更任何網頁，但是您要變更的一些程式碼，而且這些變更將會自動反映在資料庫中。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>將繼承對應至資料庫資料表的選項

`Instructor`和`Student`中的類別`School`資料模型有相同的幾個屬性：

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

假設您想要消除多餘的程式碼所共用的屬性`Instructor`和`Student`實體。 或者您想要撰寫可以不含關懷名稱是否會來自講師或學生格式名稱的服務。 您可以建立`Person`基底類別，其中包含這些共用屬性，則請`Instructor`和`Student`實體繼承自該基底類別，如下圖所示：

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

有幾種的方式可能會表示此繼承結構，在資料庫中。 您可能會有`Person`包含學生和講師單一資料表中的相關資訊的資料表。 某些資料行可以只對講師套用 (`HireDate`)，有些則只能學生 (`EnrollmentDate`)，有一些兩個 (`LastName`， `FirstName`)。 一般而言，您就必須*鑑別子*指出哪種類型的每個資料列的資料行代表。 例如，鑑別子資料行可能會有 「 講師 」 講師和 「 學生 」 學生版。

![資料表每個 hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

這種模式的單一資料庫資料表產生實體繼承結構會呼叫*資料表每個階層*(TPH) 繼承。

替代方法是讓資料庫起來更繼承結構。 例如，您無法在只有名稱欄位`Person`資料表，並有個別`Instructor`和`Student`日期欄位的資料表。

![資料表每個 type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

此模式的資料庫資料表的每個實體類別就稱為*資料表每個型別*(TPT) 繼承。

還有另一個選項是將所有的非抽象類型對應至個別資料表。 所有類別的屬性，包括繼承的屬性，對應至相對應資料表的資料行。 這個模式稱為資料表每個具象類別 (TPC) 繼承。 如果您實作 TPC 繼承`Person`， `Student`，和`Instructor`類別稍早所示`Student`和`Instructor`資料表起來比之前未實作繼承之後沒有不同。

TPC 和 TPH 繼承模式通常傳遞更佳的效能比 TPT 繼承的模式，Entity Framework 中因為 TPT 模式可能會導致複雜聯結的查詢。

本教學課程會示範如何實作 TPH 繼承。 TPH 已在 Entity Framework 中，預設繼承的模式，所以您只需要建立`Person`類別中，變更`Instructor`和`Student`類別衍生自`Person`，將新的類別加入`DbContext`，並建立移轉。 (如需如何實作其他的繼承模式的詳細資訊，請參閱[對應每個類型的資料表 (TPT) 繼承](https://msdn.microsoft.com/en-us/data/jj591617#2.5)和[對應資料表每個具象類別 (TPC) 繼承](https://msdn.microsoft.com/en-us/data/jj591617#2.6)MSDN 中Entity Framework 文件。）

## <a name="create-the-person-class"></a>建立個人類別

在*模型*資料夾中，建立*Person.cs*和範本程式碼取代為下列程式碼：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>將 Student 與講師類別繼承自人員

在*Instructor.cs*，衍生`Instructor`類別從`Person`類別，並移除索引鍵和名稱的欄位。 程式碼看起來像下列的範例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

類似變更*Student.cs*。 `Student`類別看起來像下列的範例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>將人員實體類型加入模型

在*SchoolContext.cs*，新增`DbSet`屬性`Person`實體類型：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

這是所有 Entity Framework 必須以設定資料表每個階層繼承。 您會發現，更新資料庫時，會有`Person`資料表取代`Student`和`Instructor`資料表。

## <a name="create-and-update-a-migrations-file"></a>建立和更新的移轉檔案

在 封裝管理員主控台 (PMC)，輸入下列命令：

`Add-Migration Inheritance`

執行`Update-Database`PMC 命令。 命令會在此時失敗，因為我們有現有的資料移轉並不知道如何處理。 您收到錯誤訊息如下：

> *無法卸除物件 ' dbo。講師 ' 因為它正由 FOREIGN KEY 條件約束。*


開啟*移轉\&lt; 時間戳記&gt;\_Inheritance.cs*和取代`Up`方法取代下列程式碼：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

此程式碼會負責下列資料庫更新工作：

- 移除外部索引鍵條件約束和學生資料表所指向的索引。
- 重新命名為 Person 講師資料表，然後儲存學生資料所需的變更：

    - 加入可為 null 的 EnrollmentDate 學生版。
    - 加入以指出資料列是一位學生或講師鑑別子資料行。
    - 由於學生資料列將不會有雇用日期，使 HireDate 可為 null。
    - 將暫存的欄位會用來更新指向學生的外部索引鍵。 當您複製到 [人員] 資料表的學生它們會取得新的主索引鍵值。
- 從 Person 資料表學生資料表複製資料。 這會導致學生在指派新的主索引鍵值。
- 修正指向學生的外部索引鍵值。
- 重新建立 foreign key 條件約束和索引，立即將它們指向 Person 資料表。

（如果您使用 GUID 而不是整數做為主要索引鍵類型，學生的主索引鍵值不會有變更，並有幾個步驟可能已省略）。

執行`update-database`命令一次。

（在生產系統中您會變更對應的向下方法萬一您曾經使用該項資訊來返回先前的資料庫版本。 此教學課程中您將不會使用向下方法。）

> [!NOTE]
> 很可能在移轉資料和進行結構描述變更時，取得其他錯誤。 如果您移轉時發生錯誤無法解決，您可以繼續這個教學課程中的連接字串，進而*Web.config*檔案，或藉由刪除資料庫。 最簡單的方法是在資料庫重新命名*Web.config*檔案。 例如，將資料庫名稱改 ContosoUniversity2 如下列範例所示：
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> 使用新資料庫時，沒有資料移轉，而`update-database`命令是更有可能能順利完成。 如需有關如何刪除資料庫的指示，請參閱[如何卸除資料庫，從 Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。 如果您採用這個方法，才能繼續進行本教學課程，在本教學課程結尾處略過部署步驟，或部署為新的站台資料庫。 如果相同的站台，您已部署至已部署的更新，EF 會那里相同的錯誤時自動執行移轉。 如果您想要對移轉錯誤進行疑難排解時，最佳的資源是 Entity Framework 論壇或 StackOverflow.com 之一。


## <a name="testing"></a>測試

執行網站，然後再次嘗試各種不同的頁面。 運作一切正常相同與以前一樣。

在**伺服器總管 中，**展開**資料 Connections\SchoolContext**然後**資料表**，而且您會看到的**學生**和**講師**資料表已取代**人員**資料表。 展開**人員**資料表，而且您看到它有使用中的資料行的所有**學生**和**講師**資料表。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

以滑鼠右鍵按一下 [人員] 資料表，然後按一下**顯示資料表資料**看見鑑別子資料行。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

下圖說明新的 School 資料庫的結構：

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>部署至 Azure

本節會要求您已經完成選擇性**將應用程式部署至 Azure**一節中[第 3 部分排序、 篩選和分頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)此教學課程系列。 如果您在您解決方法是刪除您的本機專案中的資料庫的移轉錯誤，請略過此步驟中;建立新的站台和資料庫，或部署到新環境。

1. 在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**從內容功能表。  
  
    ![在專案內容功能表中發行](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. 按一下 [發行] 。  
  
    ![發行](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
 Web 應用程式會在預設瀏覽器中開啟的。
3. 測試應用程式，以確認它是否運作。

    第一次您執行頁面，來存取資料庫時，Entity Framework 會執行所有移轉`Up`使資料庫的最新狀態的目前資料模型所需的方法。

## <a name="summary"></a>總結

您已實作的每個階層的資料表繼承`Person`， `Student`，和`Instructor`類別。 如需有關這個主題以及其他的繼承結構的詳細資訊，請參閱[TPT 繼承模式](https://msdn.microsoft.com/en-us/data/jj618293)和[TPH 繼承模式](https://msdn.microsoft.com/en-us/data/jj618292)MSDN 上。 在下一個教學課程中，您會看到如何處理各種不同的相對較進階的 Entity Framework 案例。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
