---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: "ASP.NET MVC 應用程式 (9 / 10) 中實作的儲存機制和單位的工作模式 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c920dc8defe18b6f27d122c2cd1a6c6ffdaad608
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>ASP.NET MVC 應用程式 (9 / 10) 中實作的儲存機制和單位的工作模式
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。 您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中您可以使用繼承減少多餘的程式碼中`Student`和`Instructor`實體類別。 在本教學課程中，您會看到一些 CRUD 作業使用的儲存機制和工作模式的單位。 如同上一個教學課程中，在這一個您要變更您的程式碼的運作的方式與網頁您已經建立而非建立新的頁面。

## <a name="the-repository-and-unit-of-work-patterns"></a>儲存機制和單位的工作模式

儲存機制和工作模式的單位被要建立資料存取層和應用程式的商務邏輯層之間的抽象層。 實作這些模式可以協助您隔離您的應用程式資料存放區中的變更，以及有助於自動化的單元測試為導向的開發 (TDD)。

在本教學課程中，您將實作每個實體類型的儲存機制類別。 如`Student`實體類型，您將建立的儲存機制介面及儲存機制類別。 時，在控制器初始化儲存機制，您將使用的介面，以便控制器將會接受實作儲存機制介面的任何物件的參考。 當控制器會執行 web 伺服器 下時，它就會收到適用於 Entity Framework 的儲存機制。 當控制器會執行單元測試類別下時，它就會收到適用於您就可以輕鬆地操作進行測試，例如記憶體中集合的方式儲存資料的儲存機制。

您稍後在本教學課程中使用多個儲存機制和工作類別的一個單位`Course`和`Department`中的實體類型`Course`控制站。 工作類別的單元協調多個儲存機制的工作，藉由建立它們的所有共用的單一資料庫內容類別。 如果您想要能夠執行自動化的單元測試，您會建立並使用這些類別的介面，如同您對`Student`儲存機制。 不過，為了簡化教學課程，您將建立和使用這些類別沒有介面。

下圖顯示手法內容類別，相較於未使用的儲存機制或單位的工作模式完全控制站之間的關聯性的其中一種方式。

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

您將不會在此教學課程中建立單元測試。 如簡介 TDD 的 MVC 應用程式會使用儲存機制模式，請參閱[逐步解說： 使用 ASP.NET MVC 中 TDD](https://msdn.microsoft.com/en-us/library/ff847525.aspx)。 如需儲存機制模式的詳細資訊，請參閱下列資源：

- [儲存機制模式](https://msdn.microsoft.com/en-us/library/ff649690.aspx)MSDN 上。
- [使用儲存機制和工作單位的模式與 Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) Entity Framework 小組部落格上。
- [敏捷式軟體開發 Entity Framework 4 儲存機制](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)Julie Lerman 部落格文章系列。
- [建置瀏覽 HTML5/jQuery 應用程式的帳戶](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin 部落格上。

> [!NOTE]
> 有許多方式來實作的儲存機制和工作模式的單位。 不論工作類別的一個單位，您可以使用儲存機制類別。 您可以實作所有的實體類型，或另一個用於每種類型的單一儲存機制。 如果您實作每個類型的其中一個，您可以使用個別的類別、 泛型基底類別和衍生的類別，或抽象的基底類別和衍生的類別。 您可以將商務邏輯併入您的儲存機制，或將其限制資料存取邏輯。 您也可以建置成您的資料庫內容類別的抽象層，使用[IDbSet](https://msdn.microsoft.com/en-us/library/gg679233(v=vs.103).aspx)那里介面，而不是[DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=vs.103).aspx)實體集的類型。 實作抽象層，在此教學課程中所示的方法是其中一個選項，您應考慮所有的案例和環境的建議。


## <a name="creating-the-student-repository-class"></a>建立學生儲存機制類別

在*DAL*資料夾中，建立名為的類別檔案*IStudentRepository.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

此程式碼會宣告一組典型 CRUD 方法，包含兩個讀取的方法，會傳回所有`Student`實體，以及尋找單一`Student`實體的識別碼。

在*DAL*資料夾中，建立名為的類別檔案*StudentRepository.cs*檔案。 將現有的程式碼取代下列程式碼，它會實作`IStudentRepository`介面：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

資料庫內容中定義類別變數，而建構函式必須要有傳遞內容的執行個體中呼叫的物件：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

您可以具現化新的內容在儲存機制，但然後如果您使用一個控制器中的多個儲存機制時，每個最後會出現在個別的內容。 稍後您將使用中的多個儲存機制`Course`控制站，而且您會看到一個單位的工作類別如何確保所有儲存機制使用相同的內容。

儲存機制實作[IDisposable](https://msdn.microsoft.com/en-us/library/system.idisposable.aspx)並處置的資料庫內容，因為您將看到稍控制器，而其 CRUD 方法進行呼叫的資料庫內容中稍早所見的相同方式。

## <a name="change-the-student-controller-to-use-the-repository"></a>變更為使用儲存機制的學生控制器

在*StudentController.cs*，目前在類別中的程式碼取代下列程式碼。 所做的變更會反白顯示。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

控制器現在會宣告實作的物件的類別變數`IStudentRepository`介面，而不是內容類別：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

預設 （無參數） 建構函式會建立新的內容執行個體，並可選擇性的建構函式可讓呼叫端將內容執行個體中。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(如果您使用*相依性插入*，或 DI，您不需要預設建構函式，因為 DI 軟體會確保一律會提供正確的儲存機制物件。)

在 CRUD 方法中，而非內容現在稱為儲存機制：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

和`Dispose`方法現在會處置而非內容儲存機制：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

執行站台，然後按一下**學生** 索引標籤。

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

網頁看起來並之前變更為使用儲存機制中，程式碼和其他學生頁面也使用相同的運作方式相同。 不過，沒有一項重要差異方式`Index`控制器方法進行篩選和排序。 這個方法的原始版本包含下列程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

已更新`Index`方法包含下列程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

僅反白顯示的程式碼已變更。

程式碼，原始的版本中`students`型別為`IQueryable`物件。 查詢不傳送至資料庫，直到它會轉換成集合，例如使用方法`ToList`，這並不會發生之前的索引檢視存取學生模型。 `Where`上述原始的程式碼中的方法會變成`WHERE`傳送至資料庫的 SQL 查詢中的子句。 而表示選取的實體會傳回資料庫中。 不過，因為變更而`context.Students`至`studentRepository.GetStudents()`、`students`這個陳述式之後的變數`IEnumerable`包括在所有學生的資料庫中的集合。 套用的最終結果`Where`方法相同，但是工作現在都是在 web 伺服器上，而不是由資料庫的記憶體中。 傳回的大量資料的查詢，這可能會沒有效率。

> [!TIP] 
> 
> **IQueryable vs。IEnumerable**
> 
> 實作儲存機制所示，即使您已輸入中的項目之後**搜尋**方塊查詢傳送至 SQL Server 會傳回所有學生資料列，因為它不會包含您的搜尋準則：
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> 因為儲存機制執行查詢，而不需要知道關於搜尋準則，此查詢會傳回所有學生資料。 排序、 套用的搜尋條件，以及選取的資料子集的分頁 （在此情況下，顯示只有 3 個資料列） 會在記憶體中完成的程序時，更新`ToPagedList`上呼叫方法`IEnumerable`集合。
> 
> 在程式碼 （在您實作儲存機制） 的先前版本中，查詢不會傳送至資料庫之前套用的搜尋條件之後，當`ToPagedList`上呼叫`IQueryable`物件。
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> 當上呼叫 ToPagedList`IQueryable`物件傳送至 SQL Server 查詢會指定搜尋字串，因此會傳回符合搜尋準則的資料列，並沒有篩選必須在記憶體中完成。
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> （下列教學課程說明如何檢查查詢傳送至 SQL Server）。


下列章節示範如何實作儲存機制的方法可讓您指定資料庫應該這項工作。

現在您已建立控制器與 Entity Framework 資料庫內容之間的抽象層。 如果您要執行自動化的單元測試與此應用程式，您可以建立替代儲存機制類別中實作的單元測試專案`IStudentRepository` *。* 而不是呼叫要讀取和寫入資料的內容，此模擬儲存機制類別無法管理記憶體中集合，若要測試控制器函式。

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>實作泛型的儲存機制和單位的工作類別

建立每個實體類型的儲存機制類別可能會導致大量多餘的程式碼，它可能會導致部分更新。 例如，假設您有要更新兩個不同的實體類型做為在相同交易的一部分。 如果每個使用不同的資料庫內容執行個體，其中可能會成功，且其他可能會失敗。 一種方法，以減少多餘的程式碼是使用泛型的儲存機制和確認的方法之一，所有儲存機制使用相同的資料庫內容 （因此協調所有更新），請使用工作類別的單位。

在本章節的教學課程中，您將建立`GenericRepository`類別和`UnitOfWork`類別，並使用它們`Course`控制站來存取`Department`和`Course`實體集。 如前所述，為了簡化這個部分的教學課程，您不建立這些類別的介面。 如果您要使用它們來促進 TDD，您會通常會實作這些介面具有相同的方式，但`Student`儲存機制。

### <a name="create-a-generic-repository"></a>建立一般的儲存機制

在*DAL*資料夾中，建立*GenericRepository.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

資料庫內容和儲存機制針對具現化的實體集，會宣告類別變數：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

建構函式會接受資料庫的內容執行個體，並初始化實體集合變數：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get`方法可讓呼叫的程式碼指定篩選條件和資料行排序結果，請使用 lambda 運算式和字串參數可讓呼叫端提供以逗號分隔的清單導覽屬性的積極式載入：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

程式碼`Expression<Func<TEntity, bool>> filter`表示呼叫端會提供 lambda 運算式，根據`TEntity`類型，以及此運算式會傳回布林值。 例如，如果儲存機制針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`student => student.LastName == "Smith`&quot;如`filter`參數。

程式碼`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`也表示呼叫端將會提供 lambda 運算式。 但在此情況下，運算式的輸入是`IQueryable`物件`TEntity`型別。 運算式將傳回的已排序的版本`IQueryable`物件。 例如，如果儲存機制針對具現化`Student`中呼叫方法的程式碼可能會指定實體類型`q => q.OrderBy(s => s.LastName)`如`orderBy`參數。

中的程式碼`Get`方法會建立`IQueryable`物件，然後再套用篩選條件運算式，如果有一個：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

接下來它之後，套用 eager 載入運算式剖析的逗號分隔的清單：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

最後，它會套用`orderBy`如果有一個運算式，並傳回結果; 否則它會傳回結果為未排序的查詢：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

當您呼叫`Get`方法，您可以執行篩選及排序`IEnumerable`而不是這些函式提供參數之方法所傳回的集合。 但是，排序和篩選工作會再完成 web 伺服器上的記憶體中。 使用這些參數，您可以確保工作都是資料庫，而不是 web 伺服器。 替代方式是建立特定的實體類型的衍生的類別並新增特殊`Get`方法，例如`GetStudentsInNameOrder`或`GetStudentsByName`。 不過，在複雜的應用程式，這可能會造成大量資料這類衍生的類別和方法，這可能是更多工作來維護。

中的程式碼`GetByID`， `Insert`，和`Update`方法很類似您看到非泛型儲存機制中。 (您未提供中的積極式載入參數`GetByID`簽章，因為您無法使用積極式載入`Find`方法。)

兩個多載可供`Delete`方法：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

其中一個這些可讓您傳入剛才識別碼的實體被刪除，一個接受實體執行個體。 如同[處理並行](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程，並行處理，您必須`Delete`採用實體執行個體方法包含追蹤屬性的原始值。

這個泛型的儲存機制會處理一般 CRUD 需求。 當特定實體類型有更複雜的篩選或排序等的特殊需求，您可以建立具有該類型的其他方法的衍生的類別。

## <a name="creating-the-unit-of-work-class"></a>建立工作類別的單位

工作類別的單元有一個用途： 若要確定當您使用多個儲存機制時，它們共用單一資料庫內容。 這樣一來，完成的工作單位時，您可以呼叫`SaveChanges`內容的執行個體上的方法，及保證所有相關變更都將經過協調。 所有的類別需要都會`Save`方法與每個儲存機制的屬性。 每個儲存機制屬性會傳回已經與其他儲存機制執行個體使用相同的資料庫內容執行個體產生的儲存機制執行個體。

在*DAL*資料夾中，建立名為的類別檔案*UnitOfWork.cs*和範本程式碼取代為下列程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

程式碼建立的資料庫內容和每個儲存機制的類別變數。 如`context`變數時，新的內容會具現化：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

儲存機制中的每個屬性會檢查是否已存在於儲存機制。 如果沒有，它會具現化的儲存機制，在內容執行個體中傳遞。 如此一來，所有儲存機制會共用相同的內容執行個體。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save`方法呼叫`SaveChanges`上的資料庫內容。

具現化資料庫的內容，在類別變數中，任何類別一樣`UnitOfWork`類別會實作`IDisposable`並處置內容。

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>變更使用 UnitOfWork 類別和儲存機制的課程控制器

您目前已在中的程式碼取代*CourseController.cs*為下列程式碼：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

這個程式碼加入的類別變數`UnitOfWork`類別。 (如果在這裡使用的介面，則不會初始化的變數; 相反地，您也會實作兩個建構函式模式，就如同您對`Student`儲存機制。)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

在類別的其餘部分，將資料庫內容的所有參考都被參考適當的儲存機制，使用`UnitOfWork`屬性來存取儲存機制。 `Dispose`方法將處置`UnitOfWork`執行個體。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

執行站台，然後按一下**課程** 索引標籤。

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

網頁看起來，並為您的變更以前, 一樣，和其他課程頁面也使用相同的運作方式相同。

## <a name="summary"></a>總結

您現在已實作儲存機制和工作模式的單位。 您使用 lambda 運算式做為泛型之儲存機制中的方法參數。 如需有關如何使用這些運算式與`IQueryable`物件，請參閱[IQueryable(T) 介面 (System.Linq)](https://msdn.microsoft.com/en-us/library/bb351562.aspx) MSDN Library 中。 在下一個教學課程，您將了解如何處理某些進階案例。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
