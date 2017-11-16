將下列屬性新增至 `Movie` 類別：

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` 欄位是資料庫對於主索引鍵的必要欄位。

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>新增資料庫內容類別

將名為 *MovieContext.cs* 的 `DbContext` 衍生類別新增至 *Models* 資料夾。
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

上述程式碼會建立實體集的 `DbSet` 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。
