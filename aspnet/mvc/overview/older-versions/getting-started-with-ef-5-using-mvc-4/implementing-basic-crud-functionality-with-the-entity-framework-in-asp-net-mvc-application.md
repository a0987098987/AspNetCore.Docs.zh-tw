---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "在 ASP.NET MVC 應用程式 (2 / 10) 中實作與 Entity Framework 的基本 CRUD 功能 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 425335d72643da39faee6e457d552c9faa6a1f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>在 ASP.NET MVC 應用程式 (2 / 10) 中實作與 Entity Framework 的基本 CRUD 功能
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。 您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中，您建立 MVC 應用程式儲存和使用 Entity Framework 和 SQL Server LocalDB 顯示資料。 在本教學課程，您將檢閱並自訂 CRUD （建立、 讀取、 更新、 刪除） MVC scaffolding 自動為您建立控制器和檢視中的程式碼。

> [!NOTE]
> 它是常見的作法，若要建立您的控制器和資料存取層之間的抽象層實作儲存機制模式。 為了簡化這些教學課程，您將不會實作儲存機制之前在這一系列之後的教學課程。


在本教學課程中，您將建立下列網頁：

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>建立詳細資料頁面

學生的 scaffold 的碼`Index`頁面中省略了`Enrollments`屬性，因為該屬性會保存集合。 在`Details`您要顯示集合的內容以 HTML 表格的網頁。

 在*Controllers\StudentController.cs*，動作方法，如`Details`檢視使用`Find`方法來擷取單一`Student`實體。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 索引鍵的值會傳遞給方法的`id`參數和來自路由資料中**詳細資料**索引頁面上的超連結。 

1. 開啟*Views\Student\Details.cshtml*。 每個欄位會顯示使用`DisplayFor`helper，如下列範例所示： 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. 之後`EnrollmentDate`欄位，並立即結束之前`fieldset`標記中加入程式碼以顯示一份註冊項目，如下列範例所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    此程式碼執行迴圈中的實體`Enrollments`導覽屬性。 每個`Enrollment`實體中的屬性，它會顯示課程標題以及等級。 正在從擷取課程標題`Course`實體中所儲存`Course`導覽屬性`Enrollments`實體。 這項資料是從擷取的所有資料庫會自動在需要時。 （換句話說，您正在使用延遲載入此處。 您未指定*積極式載入*如`Courses`導覽屬性，使您嘗試存取該屬性的第一次，查詢會傳送到資料庫擷取資料。 閱讀更多有關消極式載入和中的積極式載入[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程稍後在本系列。)
3. 執行選取的頁面**學生** 索引標籤，然後按一下**詳細資料**Alexander Carson 的連結。 您的選取學生看到 courses 以及等級清單：

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>更新 [建立] 頁面

1. 在*Controllers\StudentController.cs*，取代`HttpPost``Create`動作方法，以下列程式碼以加入`try-catch`區塊和[繫結屬性](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx)scaffold 方法： 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    這個程式碼加入`Student`實體建立 ASP.NET MVC 模型繫結器`Students`實體設定，然後將變更儲存到資料庫。 (*模型繫結器*指的 ASP.NET MVC 功能可讓您更輕鬆地使用 送出的表單資料它; 模型繫結表單張貼轉換至 CLR 型別值，並將其傳遞至動作方法參數中。 In this case，模型繫結具現化`Student`實體，讓您使用屬性值從`Form`集合。)

    `ValidateAntiForgeryToken`屬性有助於防止[跨網站偽造要求](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻擊。

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. 執行選取的頁面**學生** 索引標籤，然後按一下**新建**。

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    某些資料驗證的運作方式是預設值。 輸入名稱和無效的日期並按一下**建立**以查看錯誤訊息。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    下列反白顯示的程式碼會顯示模型的驗證檢查。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    變更為有效的值，例如，2005 年 9 月 1 日的日期，並按一下**建立**以查看出現在新的學生**索引**頁面。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>更新編輯 POST 頁面

在*Controllers\StudentController.cs*、 `HttpGet` `Edit`方法 (其中一個沒有`HttpPost`屬性) 會使用`Find`方法來擷取所選`Student`做為您所看到的實體，在`Details`方法。 您不需要變更這個方法。

不過，取代`HttpPost``Edit`動作方法，以下列程式碼以加入`try-catch`區塊和[繫結屬性](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

此程式碼是類似於您在中看到`HttpPost``Create`方法。 不過，而非新增實體至實體集的模型繫結器所建立，這段程式碼上設定旗標表示已變更的實體。 當[SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)呼叫方法時， [Modified](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx)旗標會造成 Entity Framework 來建立 SQL 陳述式來更新資料庫的資料列。 所有資料行的資料庫資料列將會更新，包括使用者沒有變更，並會忽略並行衝突。 （您將學習如何處理在稍後的教學課程中，在這一系列的並行存取）。

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>實體狀態的附加和 SaveChanges 方法

是否在記憶體中的實體會在資料庫中，其對應的資料列和同步，這項資訊會決定當您呼叫時，會發生什麼情況，會持續追蹤的資料庫內容`SaveChanges`方法。 例如，當您傳遞至新的實體[新增](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.add(v=vs.103).aspx)方法中，實體的狀態設為`Added`。 然後當您呼叫[SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)方法，將資料庫內容發出 SQL`INSERT`命令。

實體可能是其中一種[遵循狀態](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx):

- `Added`. 在資料庫中尚未存在的實體。 `SaveChanges`方法必須發出`INSERT`陳述式。
- `Unchanged`. 與這個實體所完成，不需要`SaveChanges`方法。 當您從資料庫讀取實體時，實體開始於此狀態。
- `Modified`. 修改部分或所有實體的屬性值。 `SaveChanges`方法必須發出`UPDATE`陳述式。
- `Deleted`. 實體已被標示為刪除。 `SaveChanges`方法必須發出`DELETE`陳述式。
- `Detached`. 不追蹤實體的資料庫內容。

在桌面應用程式，會通常是自動設定的狀態變更。 在桌面應用程式的類型中，您可以閱讀實體，並變更它的某些屬性值。 這會導致其實體狀態，以自動變更為`Modified`。 然後當您呼叫`SaveChanges`，Entity Framework 會產生 SQL`UPDATE`陳述式可更新只針對您所變更的實際屬性。

中斷連接的 web 應用程式的本質，不允許此連續順序。 [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx)讀取呈現頁面之後，會處置實體。 當`HttpPost``Edit`呼叫動作方法，建立新的要求，而且您必須的新執行個體[DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx)，因此您必須手動將實體狀態設定為`Modified.`然後當您呼叫`SaveChanges`，Entity Framework 會更新資料庫資料列的所有資料行，因為內容中有無從得知您已變更的屬性。

如果您想要 SQL`Update`陳述式來更新欄位才實際上已變更的使用者，您可以將原始值儲存在某些方面 （例如隱藏的欄位），以便可供使用時`HttpPost``Edit`方法呼叫。 然後您可以建立`Student`實體使用的原始值，呼叫`Attach`方法的原始版本，在實體的實體的值更新為新的值，然後呼叫`SaveChanges.`如需詳細資訊，請參閱[實體狀態和 SaveChanges](https://msdn.microsoft.com/en-us/data/jj592676)和[本機資料](https://msdn.microsoft.com/en-us/data/jj592872)MSDN 資料開發人員中心。

中的程式碼*Views\Student\Edit.cshtml*類似於您在中看到*Create.cshtml*，並不不需要任何變更。

執行選取的頁面**學生**] 索引標籤，然後按一下 [**編輯**超連結。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

變更部分的資料和按一下**儲存**。 您會看到 [索引] 頁面中變更的資料。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>更新刪除頁面

在*Controllers\StudentController.cs*，範本程式碼`HttpGet``Delete`方法會使用`Find`方法來擷取所選`Student`實體，當您在中所見`Details`和`Edit`方法。 不過，若要實作自訂的錯誤訊息時呼叫`SaveChanges`失敗，您會加入一些功能至這個方法，其對應的檢視。

當您看到更新，並建立作業，刪除作業都需要兩個動作方法。 呼叫以回應 GET 要求的方法會顯示可讓使用者有機會核准或取消刪除作業的檢視。 如果使用者核准時，會建立 POST 要求。 當發生這種情況時， `HttpPost` `Delete`方法呼叫，然後該方法會實際執行刪除作業。

您要加入`try-catch`封鎖`HttpPost``Delete`方法來處理資料庫的更新時可能會發生任何錯誤。 如果發生錯誤， `HttpPost` `Delete`方法呼叫`HttpGet``Delete`方法，將其傳遞的參數，表示已發生錯誤。 `HttpGet Delete`方法然後重新顯示 [確認] 頁面，以及錯誤訊息，讓使用者有機會取消，或再試一次。

1. 取代`HttpGet``Delete`動作方法，以下列程式碼，管理錯誤報告： 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    此程式碼接受[選擇性](https://msdn.microsoft.com/en-us/library/dd264739.aspx)布林值參數，指出是否已呼叫失敗，以儲存變更之後。 這個參數是`false`時`HttpGet``Delete`呼叫方法時沒有先前的失敗。 呼叫此方法時`HttpPost``Delete`資料庫更新錯誤的回應中的方法參數是`true`和錯誤訊息會傳遞至檢視。
- 取代`HttpPost``Delete`動作方法 (名為`DeleteConfirmed`) 取代下列程式碼，這會執行實際的刪除作業和攔截的任何資料庫更新錯誤。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

    此程式碼會擷取選取的實體，然後呼叫[移除](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.remove(v=vs.103).aspx)實體的狀態設為方法`Deleted`。 當`SaveChanges`呼叫時，SQL`DELETE`命令產生。 您也已經變更的動作方法名稱`DeleteConfirmed`至`Delete`。 名為 scaffold 的程式碼`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法唯一的簽章。 （在 CLR 需要有不同的方法參數的多載的方法）。現在，簽章是唯一的您可以盡可能使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`刪除方法。

    如果改善大量的應用程式中的效能是優先考量，就可以避免不必要的 SQL 查詢來擷取資料列取代呼叫的程式碼字行`Find`和`Remove`黃色中所示為下列程式碼的方法反白顯示：

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

    此程式碼會具現化`Student`使用只主索引鍵值的實體，然後將實體狀態設定為`Deleted`。 這是所有 Entity Framework 必須以刪除實體。

    如前所述， `HttpGet` `Delete`方法不會刪除資料。 執行 delete 作業，以回應 GET 要求 （或就此而言，在執行任何的編輯作業，建立作業或變更資料的其他任何作業） 會產生安全性風險。 如需詳細資訊，請參閱[ASP.NET MVC 秘訣 #46-請勿使用刪除連結，因為它們會產生安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen Walther 部落格上。
- 在*Views\Student\Delete.cshtml*，新增一則錯誤訊息之間`h2`標題和`h3`標題之下，如下列範例所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

    執行選取的頁面**學生** 索引標籤，然後按一下**刪除**超連結：

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
- 按一下**刪除**。 索引頁會顯示不含已刪除的學生。 (您會看到錯誤處理程式碼中的動作中的範例[處理並行](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程稍後在本系列。)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>確保資料庫連接不會保持開啟狀態

若要確定適當地關閉資料庫連接，而且它們保留向上釋放資源，您應該會看到它的內容執行個體已處置。 也就是為什麼 scaffold 的程式碼提供[處置](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx)方法結尾處`StudentController`類別*StudentController.cs*，如下列範例所示：

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

基底`Controller`類別已實作`IDisposable`介面，這個程式碼只會覆寫，以便`Dispose(bool)`方法來明確處置內容執行個體。

## <a name="summary"></a>總結

現在您有一組完整的頁面，執行簡單的 CRUD 作業，如`Student`實體。 您可以用於 MVC 協助程式產生的資料欄位 UI 元素。 如需 MVC 協助程式的詳細資訊，請參閱[呈現表單使用 HTML Helper](https://msdn.microsoft.com/en-us/library/dd410596(v=VS.98).aspx) （頁面是為 MVC 3 但仍與 MVC 4）。

在下一個教學課程中您會加入排序和分頁展開的索引頁的功能。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[下一頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
