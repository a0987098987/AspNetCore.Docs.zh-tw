## <a name="add-initial-migration-and-update-the-database"></a>加入初始的移轉，並更新資料庫

* 開啟命令提示字元並巡覽至專案目錄。 (目錄包含*Startup.cs*檔案)。

* 在命令提示字元中執行下列命令：

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](https://docs.microsoft.com/dotnet/core/tools/index)是.NET 的跨平台實作。 以下是這些命令所執行的動作：

  * `dotnet restore`： 會下載 NuGet 封裝中指定*.csproj*檔案。
  * `dotnet ef migrations add Initial`執行實體架構.NET Core CLI 移轉命令，並建立初始的移轉。 「 加入 」 之後的參數是您指派給移轉的名稱。 這裡您正在命名移轉 「 初始 」 因為它是初始資料庫移轉。 此操作會建立*資料/移轉/\<日期時間 > _Initial.cs*包含移轉命令，來新增檔案*影片*到資料庫資料表。
  * `dotnet ef database update`更新資料庫剛剛建立的移轉。

在下一個教學課程中，您將了解資料庫和連接字串。 您將了解中的資料模型變更[將欄位加入](xref:tutorials/first-mvc-app/new-field)教學課程。
