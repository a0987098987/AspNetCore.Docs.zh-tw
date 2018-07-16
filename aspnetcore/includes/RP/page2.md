`<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。 表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。

Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 ` <span asp-validation-for`) 會顯示驗證錯誤。 驗證將於本文稍後詳細討論到。

[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `Title` 屬性 (property) 的標籤標題和 `for` 屬性 (attribute)。

[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) 會使用[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。
