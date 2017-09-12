## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="e0fec-101">加入初始的移轉，並更新資料庫</span><span class="sxs-lookup"><span data-stu-id="e0fec-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="e0fec-102">開啟命令提示字元並巡覽至專案目錄。</span><span class="sxs-lookup"><span data-stu-id="e0fec-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="e0fec-103">(目錄包含*Startup.cs*檔案)。</span><span class="sxs-lookup"><span data-stu-id="e0fec-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="e0fec-104">在命令提示字元中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e0fec-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="e0fec-105">[.NET core](https://docs.microsoft.com/dotnet/core/tools/index)是.NET 的跨平台實作。</span><span class="sxs-lookup"><span data-stu-id="e0fec-105">[.NET Core](https://docs.microsoft.com/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="e0fec-106">以下是這些命令所執行的動作：</span><span class="sxs-lookup"><span data-stu-id="e0fec-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="e0fec-107">`dotnet restore`： 會下載 NuGet 封裝中指定*.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="e0fec-107">`dotnet restore`: Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="e0fec-108">`dotnet ef migrations add Initial`執行實體架構.NET Core CLI 移轉命令，並建立初始的移轉。</span><span class="sxs-lookup"><span data-stu-id="e0fec-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="e0fec-109">「 加入 」 之後的參數是您指派給移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="e0fec-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="e0fec-110">這裡您正在命名移轉 「 初始 」 因為它是初始資料庫移轉。</span><span class="sxs-lookup"><span data-stu-id="e0fec-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="e0fec-111">此操作會建立*資料/移轉/\<日期時間 > _Initial.cs*包含移轉命令，來新增檔案*影片*到資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="e0fec-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="e0fec-112">`dotnet ef database update`更新資料庫剛剛建立的移轉。</span><span class="sxs-lookup"><span data-stu-id="e0fec-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="e0fec-113">在下一個教學課程中，您將了解資料庫和連接字串。</span><span class="sxs-lookup"><span data-stu-id="e0fec-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="e0fec-114">您將了解中的資料模型變更[將欄位加入](xref:tutorials/first-mvc-app/new-field)教學課程。</span><span class="sxs-lookup"><span data-stu-id="e0fec-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>
