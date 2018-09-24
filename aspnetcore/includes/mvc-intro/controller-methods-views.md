
接下來的教學課程會涵蓋 [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)。 [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 屬性指定要顯示的欄位名稱 (在本例中為 "Release Date"，而不是 "ReleaseDate")。 [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 屬性指定資料的類型 (Date)，因此不會顯示儲存在欄位中的時間資訊。

`[Column(TypeName = "decimal(18, 2)")]` 資料註解為必要項，因此 Entity Framework Core 可將 `Price` 正確對應到資料庫中的貨幣。 如需詳細資訊，請參閱[資料類型](/ef/core/modeling/relational/data-types)。

瀏覽至 `Movies` 控制器，並將滑鼠指標停留在 **Edit** 連結，以查看目標 URL。

![滑鼠停留在 Edit 連結並顯示 http://localhost:1234/Movies/Edit/5 之 URL 的瀏覽器視窗](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

在 *Views/Movies/Index.cshtml* 檔案中，**Edit**、**Details**  和 **Delete** 連結是由 Core MVC 錨點標記協助程式所產生。

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。 在上述程式碼中，`AnchorTagHelper` 會從控制器動作方法和路由識別碼動態產生 HTML `href` 屬性值。從您最喜愛的瀏覽器中使用 [檢視原始檔] 或使用開發工具來檢查產生的標記。 產生的 HTML 部分如下所示：

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

回想在 *Startup.cs* 檔案中設定的[路由](xref:mvc/controllers/routing)格式：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core 會將 `http://localhost:1234/Movies/Edit/4` 轉譯成對 `Movies` 控制器的 `Edit` 動作方法的要求，其參數 `Id` 為 4 (控制器方法也稱為動作方法)。

[標記協助程式](xref:mvc/views/tag-helpers/intro)是 ASP.NET Core 的其中一種最受歡迎的新功能。 如需詳細資訊，請參閱[其他資源](#additional-resources)。

開啟 `Movies` 控制器，並檢查兩個 `Edit` 動作方法。 下列程式碼示範 `HTTP GET Edit` 方法，這個方法會擷取電影，並填入 *Edit.cshtml* Razor 檔案所產生的編輯表單。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

下列程式碼示範 `HTTP POST Edit` 方法，這個方法會處理已發佈的電影值：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

下列程式碼示範 `HTTP POST Edit` 方法，這個方法會處理已發佈的電影值：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[Bind]` 屬性是一種防止[過度發佈](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost)的方式。 您應該只在想要變更的 `[Bind]` 屬性 (attribute) 中包含屬性 ( property)。 如需詳細資訊，請參閱[保護控制器避免過度發佈](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)。 [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) 提供防止過度發佈的替代方法。

請注意，第二個 `Edit` 動作方法的前面是 `[HttpPost]` 屬性。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

`HttpPost` 屬性指定「只」能為 `POST` 要求叫用這個 `Edit` 方法。 您可以將 `[HttpGet]` 屬性套用至第一個編輯方法，但不需要執行此動作，因為 `[HttpGet]` 是預設值。

`ValidateAntiForgeryToken` 屬性是用來[防範要求偽造](xref:security/anti-request-forgery)，並與編輯檢視檔案 (*Views/Movies/Edit.cshtml*) 所產生的防偽語彙基元成對。 編輯檢視檔案使用[表單標記協助程式](xref:mvc/views/working-with-forms)產生防偽語彙基元。

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[表單標記協助程式](xref:mvc/views/working-with-forms)會產生隱藏的防偽語彙基元，其必須符合電影控制器的 `Edit` 方法中 `[ValidateAntiForgeryToken]` 產生的防偽語彙基元。 如需詳細資訊，請參閱[反要求偽造](xref:security/anti-request-forgery)。

`HttpGet Edit` 方法會採用電影 `ID` 參數，使用 Entity Framework `SingleOrDefaultAsync` 方法查詢電影，並將選取的電影傳回 Edit 檢視。 如果找不到電影，會傳回 `NotFound` (HTTP 404)。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

::: moniker-end

當 Scaffolding 系統建立 Edit 檢視時，它會檢查 `Movie` 類別，並建立程式碼為類別的每個屬性轉譯 `<label>` 和 `<input>` 元素。 下列範例會顯示 Visual Studio Scaffolding 系統所產生的 Edit 檢視：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

請注意檢視範本在檔案最上方指定 `@model MvcMovie.Models.Movie` 陳述式的方式。 `@model MvcMovie.Models.Movie` 指定檢視預期檢視範本的模型必須是 `Movie` 型別。

包含 Scaffold 的程式碼會使用數個標記協助程式方法來簡化 HTML 標記。 [標籤標記協助程式](xref:mvc/views/working-with-forms)顯示欄位的名稱 ("Title"、"ReleaseDate"、"Genre" 或 "Price")。 [輸入標記協助程式](xref:mvc/views/working-with-forms)轉譯 HTML `<input>` 元素。 [驗證標記協助程式](xref:mvc/views/working-with-forms)則顯示與該屬性相關聯的任何驗證訊息。

執行應用程式，並巡覽至 `/Movies` URL。 按一下 **Edit** 連結。 在瀏覽器中，檢視頁面的原始檔。 `<form>` 元素產生的 HTML 如下所示。

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` 元素位於 `HTML <form>` 元素中，而後者的 `action` 屬性設定為發佈到 `/Movies/Edit/id` URL。 按一下 `Save` 按鈕時，表單資料將發佈至伺服器。 在結尾 `</form>` 元素之前的最後一行會顯示[表單標記協助程式](xref:mvc/views/working-with-forms)所產生的隱藏 [XSRF](xref:security/anti-request-forgery) 語彙基元。

## <a name="processing-the-post-request"></a>處理 POST 要求

下列清單顯示 `[HttpPost]` 版本的 `Edit` 動作方法。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[ValidateAntiForgeryToken]` 屬性會驗證[表單標記協助程式](xref:mvc/views/working-with-forms) 中的防偽語彙基元產生器所產生的隱藏 [XSRF](xref:security/anti-request-forgery) 語彙基元

[模型繫結](xref:mvc/models/model-binding)系統採用已發佈的表單值，並建立以 `movie` 參數傳遞的 `Movie` 物件。 `ModelState.IsValid` 方法會驗證表單中提交的資料可用於修改 (編輯或更新) `Movie` 物件。 如果資料有效，則會進行儲存。 藉由呼叫資料庫內容的 `SaveChangesAsync` 方法，更新 (編輯) 的電影資料會儲存到資料庫。 儲存資料之後，程式碼將使用者重新導向至 `MoviesController` 類別的 `Index` 動作方法，此方法會顯示電影集合，包括剛剛所進行的變更。

在表單發佈至伺服器之前，用戶端驗證會對欄位檢查任何驗證規則。 如果出現任何驗證錯誤，即會顯示錯誤訊息，且不會發佈該表單。 如果已停用 JavaScript，就不會進行用戶端驗證，但伺服器偵測到無效的發佈值，因此會重新顯示表單值並顯示錯誤訊息。 稍後在本教學課程中，我們會更詳細檢查[模型驗證](xref:mvc/models/validation)。 *Views/Movies/Edit.cshtml* 檢視範本中的[驗證標記協助程式](xref:mvc/views/working-with-forms)負責顯示適當的錯誤訊息。

![Edit 檢視：Price 值 abc 不正確的例外狀況指出 Price 欄位必須是數字。 Release Date 值 xyz 不正確的例外狀況指出請輸入有效的日期。](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

電影控制器中的所有 `HttpGet` 方法都遵循類似的模式。 他們會取得電影物件 (如果是 `Index`則為物件清單)，並將此物件 (模型) 傳遞至檢視。 `Create` 方法會將空白電影物件傳遞至 `Create` 檢視。 建立、編輯、刪除或以其他方式修改資料的所有方法都會在方法的 `[HttpPost]` 多載中執行這個動作。 修改 `HTTP GET` 方法中的資料會造成安全性風險。 修改 `HTTP GET` 方法中的資料，也違反 HTTP 最佳做法及架構式 [REST](http://rest.elkstein.org/) 模式，此模式指定 GET 要求不應該變更應用程式的狀態。 也就是說，執行 GET 作業應該是安全的作業，沒有任何副作用，而且不會修改您的保存資料。

## <a name="additional-resources"></a>其他資源

* [全球化和當地語系化](xref:fundamentals/localization)
* [標記協助程式簡介](xref:mvc/views/tag-helpers/intro)
* [撰寫標記協助程式](xref:mvc/views/tag-helpers/authoring)
* [防偽要求](xref:security/anti-request-forgery)
* 保護控制器避免[過度發佈](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [表單標記協助程式](xref:mvc/views/working-with-forms)
* [輸入標記協助程式](xref:mvc/views/working-with-forms)
* [標籤標記協助程式](xref:mvc/views/working-with-forms)
* [選取標記協助程式](xref:mvc/views/working-with-forms)
* [驗證標記協助程式](xref:mvc/views/working-with-forms)
