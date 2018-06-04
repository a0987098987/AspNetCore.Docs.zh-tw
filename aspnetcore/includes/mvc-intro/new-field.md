<!-- This include not used by windows version -->
# <a name="adding-a-new-field"></a>新增欄位

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會將新欄位新增至 `Movies` 資料表。 我們會在變更結構描述 (新增欄位) 時，卸除資料庫並建立一個新的資料庫。 在開發早期我們不需要保存任何生產環境資料時，這個工作流程可正常運作。

一旦部署了應用程式，而且有需要保存的資料後，在需要變更結構描述時就無法卸除資料庫。 Entity Framework [Code First 移轉](/ef/core/get-started/aspnetcore/new-db)可讓您更新結構描述並移轉資料庫，而不會遺失資料。 移轉是使用 SQL Server 時常用的功能，但 SQLlite 不支援許多移轉結構描述作業，因此只能執行移轉。 如需詳細資訊，請參閱 [SQLite 限制](/ef/core/providers/sqlite/limitations)。

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

因為您已將新欄位新增至 `Movie` 類別，所以也需要更新繫結允許清單，以便包含這個新屬性。 在 *MoviesController.cs* 中，更新 `Create` 和 `Edit` 這兩個動作方法的 `[Bind]` 屬性 (attribute)，以包括 `Rating` 屬性 (property)：

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

您也需要更新檢視範本，以便在瀏覽器檢視中顯示、建立和編輯新的 `Rating` 屬性。

編輯 */Views/Movies/Index.cshtml* 檔案，然後新增 `Rating` 欄位：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

使用 `Rating` 欄位更新 */Views/Movies/Create.cshtml*。

在我們更新資料庫以包含新欄位之前，應用程式無法運作。 如果您立即執行應用程式，則會收到下列 `SqliteException`：

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

您看到此錯誤是因為更新的電影模型類別，不同於現有資料庫之電影資料表的結構描述。 (資料庫資料表中沒有任何 `Rating` 資料行)。

有幾個方法可以解決這個錯誤：

1. 卸除資料庫，並且讓 Entity Framework 自動重新建立以新的模型類別結構描述為基礎的資料庫。 使用此方法時，您會遺失資料庫中的現有資料，因此不能對生產環境資料庫執行此操作！ 使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。

2. 以手動方式修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。

3. 使用 Code First 移轉來更新資料庫結構描述。

在此教學課程中，當結構描述變更時，我們將卸除並重新建立資料庫。 從終端機中執行下列命令，以卸除資料庫：

`dotnet ef database drop`

更新 `SeedData` 類別，使其提供新資料行的值。 範例變更如下所示，但您會想要為每個 `new Movie` 進行這項變更。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

將 `Rating` 欄位新增至 `Edit`、`Details` 和 `Delete` 檢視。

執行應用程式，並確認您可以使用 `Rating` 欄位建立/編輯/顯示電影。 範本。
