* Startup.cs:[啟動類別](../fundamentals/startup.md)-類別設定要求管線處理對該應用程式的所有要求。
* Program.cs: [Program 類別](../fundamentals/index.md)，其中包含應用程式的主要進入點。
* firstapp.csproj:[專案檔](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj)ASP.NET Core 應用程式的 MSBuild 專案檔案格式。 包含專案對專案參考 NuGet 參考和其他專案相關的項目。
* appsettings.json / appsettings。Development.json： 環境基底應用程式設定設定檔。 [請參閱組態](xref:fundamentals/configuration)。
* bower.json: Bower 封裝專案的相依性。
* .bowerrc: bower 的組態檔定義 where Bower 下載必要資產時，安裝元件。
* bundleconfig.json： 統合及縮小前端 JavaScript 和 CSS 的資產的組態檔。
* 檢視： 包含 Razor 檢視。 檢視會顯示應用程式的使用者介面 (UI) 的元件。 一般而言，此 UI 會顯示模型資料。
* 控制站： 一開始包含 MVC 控制器*HomeController.cs*。 處理瀏覽器要求類別的控制站。
* wwwroot: Web 應用程式根資料夾。

如需詳細資訊，請參閱[MVC 模式](xref:mvc/overview)。
