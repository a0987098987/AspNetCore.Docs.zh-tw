<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="2aa86-101">執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="2aa86-101">Perform initial migration</span></span>

<span data-ttu-id="2aa86-102">從命令列中，執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="2aa86-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="2aa86-103">`dotnet ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="2aa86-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="2aa86-104">結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="2aa86-104">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="2aa86-105">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="2aa86-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="2aa86-106">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="2aa86-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="2aa86-107">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="2aa86-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="2aa86-108">`dotnet ef database update` 命令會執行 *Migrations/*\<時間戳記>_InitialCreate.cs 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="2aa86-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>