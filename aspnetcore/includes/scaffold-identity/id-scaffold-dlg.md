執行身分識別 scaffolder：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在**方案總管**中，以滑鼠右鍵按一下專案 > [**加入**  >  **新的 scaffold 專案**]。
* 從 [**加入新的 scaffold 專案**] 對話方塊的左窗格中，選取 [身分**識別**] [  >  **新增**]。
* 在 [**新增識別**] 對話方塊中，選取您想要的選項。
  * 選取您現有的版面配置頁，否則會以不正確的標記覆寫您的配置檔案：
    * `~/Pages/Shared/_Layout.cshtml`針對 Razor Pages
    * `~/Views/Shared/_Layout.cshtml`針對 MVC 專案
    * 根據 `blazorserver` 預設，不會為 Razor Pages 或 MVC 設定從 Blazor 伺服器範本（）建立的 Blazor 伺服器應用程式。 將 [版面配置頁] 專案保留空白。
  * 選取 [] **+** 按鈕以建立新的**資料內容類別**。 接受預設值或指定類別（例如， `MyApplication.Data.ApplicationDbContext` ）。
* 選取 [新增]。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

將必要的 NuGet 套件參考新增至專案檔（*.csproj*）。 在專案目錄中執行下列命令：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

執行下列命令以列出身分識別 scaffolder 選項：

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

在專案資料夾中，以您想要的選項執行 [身分識別 scaffolder]。 例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令：

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
