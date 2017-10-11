<span data-ttu-id="2d320-101">將下列屬性新增至 `Movie` 類別：</span><span class="sxs-lookup"><span data-stu-id="2d320-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="2d320-102">`ID` 欄位是資料庫對於主索引鍵的必要欄位。</span><span class="sxs-lookup"><span data-stu-id="2d320-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="2d320-103">新增資料庫內容類別</span><span class="sxs-lookup"><span data-stu-id="2d320-103">Add a database context class</span></span>

<span data-ttu-id="2d320-104">將名為 *MovieContext.cs* 的 `DbContext` 衍生類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2d320-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

<span data-ttu-id="2d320-105">上述程式碼會建立實體集的 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d320-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="2d320-106">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="2d320-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="2d320-107">`DbSet` 屬性名稱為 `Movies`。</span><span class="sxs-lookup"><span data-stu-id="2d320-107">The `DbSet` property name is `Movies`.</span></span> <span data-ttu-id="2d320-108">由於資料庫是使用單數名稱，因此範例會覆寫 `OnModelCreating` 以使用單數形式 (`Movie`) 的資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="2d320-108">Since the database uses singular names, the sample overrides `OnModelCreating` to use the singular form (`Movie`) for the table name.</span></span>
