---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 在 ASP.NET MVC 應用程式 (10 個 2) 中實作 Entity framework 的基本 CRUD 功能 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f580ba6d0db07430e991fba2d061bb2632c95353
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817079"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>在 ASP.NET MVC 應用程式 (10 個 2) 中實作 Entity framework 的基本 CRUD 功能
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中，您建立 MVC 應用程式儲存及顯示資料使用 Entity Framework 和 SQL Server LocalDB。 在本教學課程中，您將檢閱並自訂 CRUD （建立、 讀取、 更新、 刪除） 的 MVC scaffolding 自動為您建立控制器和檢視表中的程式碼。

> [!NOTE]
> 實作儲存機制模式，以在您的控制器及資料存取層之間建立抽象層是一種非常常見的做法。 為了簡化這些教學課程中，您將不會實作儲存機制之前在本系列稍後的教學課程。


在本教學課程中，您將建立下列網頁：

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>建立詳細資料頁面

Scaffold 的程式碼，讓學生`Index`向左翻頁`Enrollments`屬性，因為該屬性的值集合。 在 `Details`頁面，您將會以 HTML 表格顯示集合的內容。

 在  *Controllers\StudentController.cs*，動作方法，如`Details`檢視使用`Find`方法來擷取單一`Student`實體。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 索引鍵的值會傳遞給方法的`id`參數和路由資料是來自**詳細資料**索引 頁面上的超連結。 

1. 開啟*Views\Student\Details.cshtml*。 每個欄位會顯示使用`DisplayFor`協助程式，如下列範例所示： 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. 在後`EnrollmentDate`欄位，然後立即在關閉前`fieldset`標記中加入程式碼，顯示一份註冊，如下列範例所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    此程式碼會以迴圈逐一巡覽 `Enrollments` 導覽屬性中的實體。 每個`Enrollment`實體在屬性中，它會顯示課程標題及成績。 課程標題會從`Course`中所儲存的實體`Course`導覽屬性`Enrollments`實體。 所有這些資料是從資料庫擷取自動在需要時。 （換句話說，您使用消極式載入這裡。 您未指定*積極式載入*如`Courses`導覽屬性，使您嘗試存取該屬性的第一次，查詢會傳送至資料庫擷取資料。 您可以深入了解消極式載入和中的積極式載入[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列教學課程。)
3. 選取執行網頁**學生** 索引標籤，然後按一下**詳細資料**Alexander Carson 的連結。 您會看到選取學生的課程及成績清單：

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>更新 [建立] 頁面

1. 在  *Controllers\StudentController.cs*，取代`HttpPost``Create`動作方法，以下列的程式碼新增`try-catch`區塊和[繫結屬性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)scaffold 方法： 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    這個程式碼加入`Student`建立 ASP.NET MVC 模型繫結至實體`Students`實體設定，然後將變更儲存到資料庫。 (*模型繫結器*指的 ASP.NET MVC 功能可讓您更輕鬆地由表單送出的資料搭配使用它，模型繫結至 CLR 型別值轉換張貼表單，並將它們傳遞至動作方法參數中。 In this case，模型繫結具現化`Student`實體，讓您使用屬性值從`Form`集合。)

    `ValidateAntiForgeryToken`屬性可協助防止[跨網站偽造要求](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻擊。

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. 選取執行網頁**學生**索引標籤，然後按一下**建立新**。

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    某些資料驗證運作方式是預設值。 輸入名稱和無效的日期，然後按一下**建立**以查看錯誤訊息。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    下列醒目提示的程式碼顯示的模型驗證檢查。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    變更為有效的值，例如 2005 年 9 月 1 日的日期，然後按一下**建立**若要查看出現在新的學生**Index**頁面。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>更新 [編輯] 文章頁面

在*Controllers\StudentController.cs*，則`HttpGet``Edit`方法 (沒有`HttpPost`屬性) 會使用`Find`方法來擷取所選`Student`為您所見的實體，在 `Details`方法。 您不需要變更這個方法。

不過，取代`HttpPost``Edit`動作方法，以下列程式碼加入`try-catch`區塊並[繫結屬性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

這段程式碼如下所示`HttpPost``Create`方法。 不過，而不是新增模型繫結至實體集建立的實體，此程式碼上設定旗標表示它已變更的實體。 當[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)呼叫方法時， [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx)旗標會使 Entity Framework 建立 SQL 陳述式來更新資料庫資料列。 資料庫的資料列的所有資料行會更新，包括使用者未變更，而且會忽略並行衝突。 （您將了解如何處理在稍後的教學課程中，在這一系列的並行存取。）

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>實體狀態的附加和 SaveChanges 方法

資料庫內容會追蹤實體在記憶體中是否與其在資料庫中相對應的資料列保持同步，並且此項資訊會決定當您呼叫 `SaveChanges` 方法時會發生什麼事情。 例如，當您傳遞至新的實體[新增](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx)方法時，該實體的狀態會設為`Added`。 當您呼叫[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)方法，將資料庫內容會發出 SQL`INSERT`命令。

實體可能是下列其中一種[遵循狀態](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. 在資料庫中尚未存在的實體。 `SaveChanges`方法必須發出`INSERT`陳述式。
- `Unchanged`. `SaveChanges` 方法針對這個實體不需要進行任何動作。 當您從資料庫讀取一個實體時，實體便會以此狀態開始。
- `Modified`. 實體中一部分或全部的屬性值已經過修改。 `SaveChanges`方法必須發出`UPDATE`陳述式。
- `Deleted`. 實體已遭標示刪除。 `SaveChanges`方法必須發出`DELETE`陳述式。
- `Detached`. 實體未獲得資料庫內容追蹤。

在桌面應用程式中，狀態變更通常會自動進行設定。 中的桌面應用程式類型，您會讀取一個實體，並變更其部分屬性值。 這會使得其實體狀態自動變更為 `Modified`。 當您呼叫`SaveChanges`，Entity Framework 會產生 SQL`UPDATE`陳述式可更新只有您所變更的實際的屬性。

中斷連接的 web 應用程式的本質不允許此連續順序。 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)讀取實體頁面轉譯之後處置。 當`HttpPost``Edit`呼叫動作方法提出新要求時，您有的新執行個體[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)，因此您必須手動將實體狀態設定為`Modified.`然後當您呼叫`SaveChanges`，Entity Framework 會更新資料庫資料列的所有資料行，因為內容無從得知您變更的屬性。

如果您想要將 SQL`Update`陳述式來更新使用者實際變更的欄位，使其可用時，您可以在某些方面 （例如隱藏的欄位） 中儲存原始的值`HttpPost``Edit`呼叫方法。 然後您可以建立`Student`實體使用原始值，也就是呼叫`Attach`方法使用原始版本的實體，實體的值更新為新的值，然後再呼叫`SaveChanges.`如需詳細資訊，請參閱[實體狀態和 SaveChanges](https://msdn.microsoft.com/data/jj592676)並[本機資料](https://msdn.microsoft.com/data/jj592872)MSDN 資料開發人員中心。

中的程式碼*Views\Student\Edit.cshtml*類似於您所見的內容中*Create.cshtml*，並不不需要任何變更。

選取執行網頁**學生**索引標籤，然後按一下**編輯**超連結。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

變更一部分的資料，然後按一下 [儲存]。 您會看到 [索引] 頁面中變更的資料。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>更新 [刪除] 頁面

在  *Controllers\StudentController.cs*，範本程式碼`HttpGet``Delete`方法會使用`Find`方法來擷取所選`Student`實體，當您在中所見`Details`和`Edit`方法。 然而，若要在呼叫 `SaveChanges` 失敗時實作自訂錯誤訊息，您需要將一些功能新增至此方法及其對應的檢視。

如同您在更新及建立作業中所看到的，刪除作業需要兩個動作方法。 呼叫以回應 GET 要求的方法會顯示檢視，給予使用者機會核准或取消刪除作業。 若使用者核准，則便會建立 POST 要求。 當發生這種情況`HttpPost``Delete`呼叫方法，然後該方法會實際執行刪除作業。

您會新增`try-catch`封鎖`HttpPost``Delete`方法來處理資料庫更新時可能會發生任何錯誤。 發生錯誤時， `HttpPost` `Delete`方法呼叫`HttpGet``Delete`方法，將表示發生錯誤的參數傳遞給它。 `HttpGet Delete`方法接著會重新顯示確認頁面及錯誤訊息，讓使用者有機會取消或再試一次。

1. 取代`HttpGet``Delete`動作方法，以下列程式碼，用來管理錯誤報告： 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    此程式碼會接受[選擇性](https://msdn.microsoft.com/library/dd264739.aspx)布林值參數，指出是否已呼叫無法儲存變更之後。 這個參數是`false`時`HttpGet``Delete`而上一次的失敗不會呼叫方法。 當它由呼叫`HttpPost``Delete`方法以回應資料庫更新錯誤，參數是`true`和錯誤訊息傳遞至檢視。
2. 取代`HttpPost``Delete`動作方法 (名為`DeleteConfirmed`) 為下列程式碼，這會執行實際的刪除作業和攔截的任何資料庫更新錯誤。

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     此程式碼會擷取選取的實體，然後呼叫[移除](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx)方法，以將實體的狀態設定為`Deleted`。 當`SaveChanges`呼叫時，SQL`DELETE`命令會產生。 您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。 名為的 scaffold 程式碼`HttpPost``Delete`方法`DeleteConfirmed`給`HttpPost`方法唯一的簽章。 （CLR 要求多載的方法，以不同的方法參數）。既然簽章是唯一的您可以繼續使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`delete 方法。

     改善效能的大量應用程式中做為優先考量，如果您無法避免不必要的 SQL 查詢來取代呼叫的程式碼行中擷取資料列`Find`和`Remove`為下列程式碼所示以黃色標示的方法反白顯示：

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     此程式碼會具現化`Student`使用只有主索引鍵值的實體，然後將實體狀態設定為`Deleted`。 這便是 Entity Framework 要刪除實體所需要的一切資訊。

     如所述， `HttpGet` `Delete`方法不會刪除資料。 執行刪除作業，以回應 GET 要求 （或就此而言，執行任何的編輯作業，建立作業或變更資料的任何其他作業） 會產生安全性風險。 如需詳細資訊，請參閱 < [ASP.NET MVC 秘訣 #46 — 不用刪除連結，因為它們會造成安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen walther 所撰寫的部落格上。
3. 在  *Views\Student\Delete.cshtml*，新增一則錯誤訊息之間`h2`標題和`h3`標題之下，如下列範例所示：

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     選取執行網頁**學生**索引標籤，然後按一下**刪除**超連結：

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. 按一下 [刪除]。 顯示的 [索引] 頁面將不會包含遭刪除的學生。 (您會看到的錯誤處理中的作用中的程式碼範例[處理並行](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列教學課程。)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>確保資料庫連接不會保持開啟狀態

若要確定妥善關閉資料庫連接，而且它們會保存向上釋放資源，您應該會看到它已處置的內容執行個體。 也就是為什麼包含 scaffold 的程式碼提供[處置](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx)方法的結尾`StudentController`類別*StudentController.cs*，如下列範例所示：

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

基底`Controller`已經類別會實作`IDisposable`介面，這個程式碼直接加入覆寫，以便`Dispose(bool)`明確處置內容執行個體的方法。

## <a name="summary"></a>總結

您現在有一組完整的頁面，執行簡單的 CRUD 作業，如`Student`實體。 您可以用於 MVC 協助程式產生的資料欄位 UI 元素。 如需有關 MVC 協助程式的詳細資訊，請參閱[轉譯表單使用 HTML 協助程式](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx)（頁面是為 MVC 3 但仍與 MVC 4）。

在下一個教學課程中，您將會展開的 [索引] 頁面的功能加上排序和分頁。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [下一頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
