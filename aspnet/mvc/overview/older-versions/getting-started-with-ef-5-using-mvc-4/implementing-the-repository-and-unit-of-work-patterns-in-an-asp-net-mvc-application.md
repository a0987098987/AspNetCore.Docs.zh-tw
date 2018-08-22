---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: ASP.NET MVC 應用程式 (10 個 9) 中實作存放庫和工作單元模式 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 20f82744582faddaffdc5b4785bf208ecf0955a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830187"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>ASP.NET MVC 應用程式 (10 個 9) 中實作存放庫和工作單元模式
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


您可以先前的教學課程中使用繼承來減少多餘的程式碼中`Student`和`Instructor`實體類別。 在本教學課程中，您會看到一些方法，若要使用的儲存機制和工作單元模式的 CRUD 作業。 如同先前的教學課程中，在這一個您要變更您的程式碼的運作的方式與頁面您已經建立，而非建立新的頁面。

## <a name="the-repository-and-unit-of-work-patterns"></a>存放庫和工作單元模式

存放庫和工作單元模式被要建立資料存取層與應用程式的商務邏輯層之間的抽象層。 實作這些模式可協助隔離您的應用程式與資料存放區中的變更，並可促進自動化單元測試或測試驅動開發 (TDD)。

在本教學課程中，您將實作每個實體類型的儲存機制類別。 針對`Student`實體類型，您將建立的儲存機制介面和儲存機制類別。 當您具現化您的控制器中的存放庫會使用介面，讓控制器將會接受實作存放庫介面的任何物件的參考。 當控制器會在 web 伺服器下執行時，它就會收到與 Entity Framework 搭配運作的存放庫。 當控制器來執行單元測試類別時，它就會收到適用於您可以輕易地操作進行測試，例如記憶體中集合的方式儲存資料的儲存機制。

稍後在本教學課程中，您將使用多個存放庫和工作類別的單位`Course`並`Department`中的實體型別`Course`控制站。 工作類別的單位協調多個儲存機制的工作建立的所有人都共用的單一資料庫內容類別。 如果您想要能夠執行自動化的單元測試，您會建立並使用這些類別的介面，在相同的方式，您對`Student`存放庫。 不過，為了簡化本教學課程，您會建立並使用這些類別，而不需要的介面。

下圖顯示一個概念化方式，控制站和相較於不使用的存放庫或工作單位模式完全的內容類別之間的關聯性。

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

在本教學課程系列中，您將不會建立單元測試。 使用儲存機制模式的 MVC 應用程式使用 tdd 的簡介，請參閱[逐步解說： 使用 ASP.NET MVC 中 TDD](https://msdn.microsoft.com/library/ff847525.aspx)。 如需儲存機制模式的詳細資訊，請參閱下列資源：

- [儲存機制模式](https://msdn.microsoft.com/library/ff649690.aspx)MSDN 上。
- [使用 Entity Framework 4.0 中的儲存機制和工作單位模式](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)Entity Framework 小組部落格上。
- [敏捷式軟體開發 Entity Framework 4 存放庫](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)的一系列 Julie Lerman 的部落格上的文章。
- [建置快速 HTML5/jQuery 應用程式的帳戶](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin 部落格上。

> [!NOTE]
> 有許多方式可實作的存放庫和工作單元模式。 您可以使用儲存機制類別，無論工作類別的單位。 您可以實作所有實體類型，或另一個用於每種類型的單一儲存機制。 如果您實作每種類型一個，您可以使用個別的類別、 泛型基底類別衍生的類別，或抽象的基底類別和衍生的類別。 您可以納入您的存放庫中的商務邏輯，或將其限制資料存取邏輯。 您也可以建置到您的資料庫內容類別的抽象層，使用[IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx)那里介面而非[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)類型的實體集。 若要實作的抽象層，本教學課程中，方法是您應考慮，不建議所有的案例和環境的其中一個選項。


## <a name="creating-the-student-repository-class"></a>建立學生儲存機制類別

在  *DAL*資料夾中，建立名為的類別檔案*IStudentRepository.cs*並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

此程式碼會宣告一組典型 CRUD 方法，包括兩個讀取的方法，會傳回所有`Student`實體，以及尋找單一`Student`實體的識別碼。

在  *DAL*資料夾中，建立名為的類別檔案*StudentRepository.cs*檔案。 現有的程式碼取代為下列程式碼，它會實作`IStudentRepository`介面：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

資料庫內容定義在類別變數中，並建構函式預期呼叫的物件，用於傳遞內容的執行個體中：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

您可以具現化新的內容，在儲存機制，但然後如果您使用一個控制器中的多個存放庫時，每個會得到不同的內容。 稍後您將使用中的多個存放庫`Course`控制站，而且您會看到如何工作類別的單位可以確保所有的存放庫會使用相同的內容。

存放庫會實作[IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)和處置的資料庫內容，因為您將看到稍早在控制器中，而其 CRUD 方法呼叫的資料庫內容中稍早所見的相同方式。

## <a name="change-the-student-controller-to-use-the-repository"></a>變更學生控制器使用的儲存機制

在  *StudentController.cs*，目前在類別中的程式碼取代為下列程式碼。 所做的變更已醒目提示。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

控制器現在會宣告一個類別變數的物件，實作`IStudentRepository`介面，而不是內容類別：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

預設 （無參數） 建構函式會建立新的內容執行個體，並選擇性的建構函式可讓呼叫者傳入的內容執行個體。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(如果您使用*相依性插入*，或 DI，您不需要預設建構函式，因為 DI 軟體可確保一律會提供正確的存放庫物件。)

在 CRUD 方法中，而非內容現在稱為 「 存放庫：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

和`Dispose`方法現在會處置而非內容存放庫：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

執行站台，然後按一下**學生** 索引標籤。

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

頁面的外觀與之前變更程式碼以使用儲存機制，以及的其他學生頁面也使用相同的運作方式相同。 不過，還有一項重要差異方式`Index`控制器方法未篩選和排序。 這個方法的原始版本包含下列程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

已更新`Index`方法包含下列程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

僅反白顯示的程式碼變更。

在原始版本的程式碼中，`students`型別為`IQueryable`物件。 查詢不傳送至資料庫，直到它會轉換成集合，例如使用方法`ToList`，這並不會發生，直到 [索引] 檢視存取學生模型。 `Where`上面的原始程式碼的方法會變成`WHERE`傳送至資料庫的 SQL 查詢中的子句。 這又表示選取的實體，會傳回資料庫。 不過，因為變更而`context.Students`要`studentRepository.GetStudents()`，則`students`此陳述式之後的變數`IEnumerable`包含資料庫中的所有學生的集合。 套用的最終結果`Where`方法相同，但是現在在 web 伺服器上，而不是由資料庫的記憶體中完成工作。 傳回大量資料的查詢，這可能會沒有效率。

> [!TIP]
> 
> **IQueryable vs。IEnumerable**
> 
> 實作存放庫如此處所示，即使您已輸入中的項目後**搜尋**方塊查詢傳送至 SQL Server 會傳回所有的學生資料列，因為它不包含您的搜尋準則：
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> 因為存放庫執行查詢，而不需要知道有關搜尋條件，此查詢會傳回所有的學生資料。 排序、 套用的搜尋條件，和選取的資料子集 （在此情況下，顯示只有 3 個資料列） 的分頁在記憶體中的程序時，更新`ToPagedList`上呼叫方法`IEnumerable`集合。
> 
> 在程式碼 （在您實作儲存機制） 之前的先前版本中，查詢不會傳送給資料庫之前套用的搜尋條件之後，當`ToPagedList`上呼叫`IQueryable`物件。
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> 當上呼叫 ToPagedList`IQueryable`物件傳送至 SQL Server 查詢會指定搜尋字串，如此一來，系統會傳回符合搜尋準則的資料列，並沒有篩選必須在記憶體中完成。
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> （下列教學課程說明如何檢查傳送至 SQL Server 的查詢）。


下一節示範如何實作存放庫方法，讓您指定資料庫應該完成此工作。

您現在已建立控制器與 Entity Framework 資料庫內容之間的抽象層。 如果您要執行自動化的單元測試與此應用程式，您可以建立替代的儲存機制類別中實作的單元測試專案`IStudentRepository` *。* 而不是呼叫要讀取和寫入資料的內容，此模擬儲存機制類別無法管理記憶體中集合，才能測試控制器的函式。

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>實作泛型儲存機制和單位的工作類別

建立每個實體類型的儲存機制類別可能會導致大量多餘的程式碼，並可能會造成部分更新。 例如，假設您有更新的相同交易一部分的兩個不同的實體類型。 如果每個使用不同的資料庫內容執行個體，其中可能會成功，而且其他可能會失敗。 減少多餘的程式碼的方法之一是使用一般的存放庫和一個可確實的所有存放庫使用相同的資料庫內容 （並因此協調所有更新） 是使用工作類別的單位。

在本節的教學課程中，您將建立`GenericRepository`類別和`UnitOfWork`類別，並使用它們`Course`控制站來存取`Department`和`Course`實體集。 如先前所述，為了簡化本教學課程的這個部分，您沒有建立這些類別的介面。 如果您要使用它們來促進 TDD，您會通常會實作這些介面使用相同的方式，但`Student`存放庫。

### <a name="create-a-generic-repository"></a>建立一般的存放庫

在  *DAL*資料夾中，建立*GenericRepository.cs*並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

資料庫內容和存放庫針對具現化的實體集，會宣告類別變數：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

建構函式會接受資料庫內容執行個體，並初始化實體設定變數：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get`方法使用 lambda 運算式，以允許呼叫的程式碼，以指定的篩選條件和資料行排序結果，以及字串參數可讓呼叫端提供的積極式載入導覽屬性以逗號分隔清單：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

程式碼`Expression<Func<TEntity, bool>> filter`表示呼叫端會提供 lambda 運算式，根據`TEntity`類型，以及此運算式會傳回布林值。 例如，如果存放庫針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`student => student.LastName == "Smith`&quot;如`filter`參數。

程式碼`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`也表示呼叫者會提供 lambda 運算式。 但在此情況下，運算式的輸入`IQueryable`物件`TEntity`型別。 運算式將傳回的已排序的版本`IQueryable`物件。 例如，如果存放庫針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`q => q.OrderBy(s => s.LastName)`如`orderBy`參數。

中的程式碼`Get`方法會建立`IQueryable`物件，然後再套用篩選條件運算式，如果有一個：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

接下來它積極式載入會將運算式套用之後剖析逗號分隔的清單：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

最後，它會套用`orderBy`運算式，如果有的話，並傳回結果，否則會傳回結果從排序的查詢：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

當您呼叫`Get`方法中，您可以執行篩選和排序`IEnumerable`方法而非提供參數，這些函式所傳回的集合。 但是，排序和篩選工作就會對執行 web 伺服器上的記憶體。 藉由使用這些參數，您可以確保工作由資料庫，而不是 web 伺服器。 替代方法是建立特定實體型別的衍生的類別並新增特製化`Get`方法，例如`GetStudentsInNameOrder`或`GetStudentsByName`。 不過，在複雜的應用程式，這可能會造成大量的這種特殊的方法，可能是更多工作來維護及衍生的類別中。

中的程式碼`GetByID`， `Insert`，和`Update`方法是類似於您所見的內容中的非泛型儲存機制。 (您未提供中的參數，積極式載入`GetByID`簽章，因為您無法使用積極式載入`Find`方法。)

兩個多載可供`Delete`方法：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

其中一個這些可讓您傳入只是實體的識別碼來刪除，而且另一個則採用實體執行個體。 如您所見中[處理並行](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程中，並行處理，您需要`Delete`方法會採用實體執行個體，包括追蹤屬性的原始值。

這個一般的存放庫會處理一般的 CRUD 需求。 當特定的實體類型有特殊需求，例如更複雜的篩選或排序，您可以建立有額外的方法，該類型的衍生的類別。

## <a name="creating-the-unit-of-work-class"></a>建立工作類別的單位

工作類別的單位有一個用途： 若要確定，當您使用多個存放庫時，它們會共用單一資料庫的內容。 這樣一來，完成的工作單位時，您可以呼叫`SaveChanges`內容的執行個體上的方法，及保證所有相關的變更將會協調。 所有這些類別的需求是`Save`方法和屬性，以每個存放庫。 每個存放庫屬性會傳回已具現化與其他存放庫執行個體使用相同的資料庫內容執行個體的存放庫執行個體。

在  *DAL*資料夾中，建立名為的類別檔案*UnitOfWork.cs*並以下列程式碼取代範本程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

程式碼會建立資料庫內容和每個存放庫的類別變數。 針對`context`變數時，新的內容會具現化：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

每個存放庫的屬性會檢查是否已存在於存放庫。 如果沒有，它會具現化的存放庫中，傳入內容執行個體。 如此一來，所有存放庫都會共用相同的內容執行個體。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save`方法呼叫`SaveChanges`上的資料庫內容。

和在類別變數中，資料庫內容會具現化任何類別一樣`UnitOfWork`類別會實作`IDisposable`和處置內容。

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>變更使用 UnitOfWork 類別和儲存機制的課程控制器

將程式碼中目前沒有*CourseController.cs*為下列程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

這個程式碼加入的類別變數`UnitOfWork`類別。 (如果您在這裡使用的介面，您不會初始化的變數; 相反地，您會實作兩個建構函式的模式就像您對`Student`存放庫。)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

在類別的其餘部分，所有參考的資料庫內容會以都取代參考適當的存放庫中，使用`UnitOfWork`屬性來存取儲存機制。 `Dispose`方法將處置`UnitOfWork`執行個體。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

執行站台，然後按一下**課程** 索引標籤。

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

頁面的外觀與之前變更，以及其他課程頁面也使用相同的運作方式相同。

## <a name="summary"></a>總結

您現在已實作存放庫和工作單元模式。 您已使用 lambda 運算式做為一般的存放庫中的方法參數。 如需有關如何使用這些運算式搭配`IQueryable`物件，請參閱 < [IQueryable(T) 介面 (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN Library 中。 在下一個教學課程，您將了解如何處理一些進階的案例。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
