預設範本會建立 **RazorPagesMovie**、**Home**、**About** 和 **Contact** 連結和頁面。 根據瀏覽器視窗的大小，您可能需要按一下瀏覽圖示來顯示連結。

![Home 或 Index 頁面](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

測試連結。 **RazorPagesMovie** 和 **Home** 連結會移至 Index 頁面。 **About** 和 **Contact** 連結則分別移至 `About` 和 `Contact` 頁面。

## <a name="project-files-and-folders"></a>專案檔和資料夾

下表列出專案中的檔案和資料夾。 在本教學課程中，*Startup.cs* 檔案是需要了解的最重要項目。 您不需要檢閱以下提供的每個連結。 當您需要專案中的檔案或資料夾的詳細資訊時，便會提供連結作為參考。

| 檔案或資料夾              | 用途 |
| ----------------- | ------------ | 
| wwwroot | 包含靜態檔案。 請參閱[靜態檔案](xref:fundamentals/static-files)。 |
| 頁面 | [Razor 頁面](xref:mvc/razor-pages/index)的資料夾。 | 
| *appsettings.json* | [組態](xref:fundamentals/configuration/index) |
| *Program.cs* | [裝載](xref:fundamentals/host/index) ASP.NET Core 應用程式。|
| *Startup.cs* | 設定服務和要求管線。 請參閱 [Startup](xref:fundamentals/startup)。|

### <a name="the-pages-folder"></a>Pages 資料夾

*_Layout.cshtml* 檔案包含通用的 HTML 元素 (指令碼和樣式表)，並設定應用程式的配置。 例如，當您按一下 **RazorPagesMovie**、**Home**、**About** 或 **Contact** 時，會看到相同的元素。 通用元素包含頂端的導覽功能表和視窗底部的標頭。 如需詳細資訊，請參閱 [Layout](xref:mvc/views/layout)。

*_ViewStart.cshtml* 會設定 Razor 頁面 `Layout` 屬性，以使用 *_Layout.cshtml* 檔案。 如需詳細資訊，請參閱 [Layout](xref:mvc/views/layout)。

*_ViewImports.cshtml* 檔案包含匯入至每個 Razor 頁面的 Razor 指示詞。 如需詳細資訊，請參閱[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)。

*_ValidationScriptsPartial.cshtml* 檔案提供 [jQuery](https://jquery.com/) 驗證指令碼的參考。 當我們稍後在教學課程中新增至 `Create` 和 `Edit` 頁面時，將會使用 *_ValidationScriptsPartial.cshtml* 檔案。

`About`、`Contact` 和 `Index` 頁面是您可以用來啟動應用程式的基本頁面。 `Error` 頁面則用來顯示錯誤資訊。