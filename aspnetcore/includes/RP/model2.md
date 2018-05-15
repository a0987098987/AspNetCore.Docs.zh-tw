<span data-ttu-id="be95c-101">將下列屬性新增至 `Movie` 類別：</span><span class="sxs-lookup"><span data-stu-id="be95c-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="be95c-102">`ID` 欄位是資料庫對於主索引鍵的必要欄位。</span><span class="sxs-lookup"><span data-stu-id="be95c-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="be95c-103">新增資料庫內容類別</span><span class="sxs-lookup"><span data-stu-id="be95c-103">Add a database context class</span></span>

<span data-ttu-id="be95c-104">將下列 *MovieContext.cs* 類別新增至 *Models* 資料夾：[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="be95c-104">Add the following  *MovieContext.cs* class to the *Models* folder: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="be95c-105">上述程式碼會建立實體集的 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="be95c-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="be95c-106">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="be95c-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
