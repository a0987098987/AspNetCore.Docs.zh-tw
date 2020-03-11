::: moniker range=">= aspnetcore-3.0"

執行身分識別 scaffolder：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在**方案總管**中，以滑鼠右鍵按一下專案 >**加入**>**新的 scaffold 專案**。
* 從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [身分**識別**>**新增**]。
* 在 [**新增識別**] 對話方塊中，選取您想要的選項。
  * 選取您現有的版面配置頁，否則會以不正確的標記覆寫您的配置檔案。 選取現有的 *\_配置 cshtml*檔案時，**不**會覆寫該檔案。

 例如： MVC 專案的 Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`
* 若要使用現有的資料內容，請至少選取一個要覆寫的檔案。 您必須至少選取一個檔案來新增資料內容。
  * 選取您的資料內容類別。
  * 選取 [新增]。
* 若要建立新的使用者內容，並可能建立身分識別的自訂使用者類別：
  * 選取 [ **+** ] 按鈕，以建立新的**資料內容類別**。
  * 選取 [新增]。

注意：如果您要建立新的使用者內容，就不需要選取要覆寫的檔案。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

將必要的 NuGet 套件參考新增至專案（\*.csproj）檔案。 在專案目錄中執行下列命令：

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

在專案資料夾中，以您想要的選項執行 [身分識別 scaffolder]。 例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令。 針對您的資料庫內容，請使用正確的完整名稱：

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

PowerShell 使用分號做為命令分隔符號。 使用 PowerShell 時，請將檔案清單中的分號 escape，或以雙引號括住檔案清單。 例如：

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

如果您在未指定 `--files` 旗標或 `--useDefaultUI` 旗標的情況下執行身分識別 scaffolder，則會在專案中建立所有可用的身分識別 UI 頁面。

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

執行身分識別 scaffolder：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在**方案總管**中，以滑鼠右鍵按一下專案 >**加入**>**新的 scaffold 專案**。
* 從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [身分**識別**>**新增**]。
* 在 [**新增識別**] 對話方塊中，選取您想要的選項。
  * 選取您現有的版面配置頁，否則會以不正確的標記覆寫您的配置檔案。 選取現有的 *\_配置 cshtml*檔案時，**不**會覆寫該檔案。

 例如： MVC 專案的 Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`
* 若要使用現有的資料內容，請至少選取一個要覆寫的檔案。 您必須至少選取一個檔案來新增資料內容。
  * 選取您的資料內容類別。
  * 選取 [新增]。
* 若要建立新的使用者內容，並可能建立身分識別的自訂使用者類別：
  * 選取 [ **+** ] 按鈕，以建立新的**資料內容類別**。
  * 選取 [新增]。

注意：如果您要建立新的使用者內容，就不需要選取要覆寫的檔案。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

將[VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)的套件參考新增至專案（\*.csproj）檔案中。 在專案目錄中執行下列命令：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令以列出身分識別 scaffolder 選項：

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

在專案資料夾中，以您想要的選項執行 [身分識別 scaffolder]。 例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令。 針對您的資料庫內容，請使用正確的完整名稱：

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

PowerShell 使用分號做為命令分隔符號。 使用 PowerShell 時，請將檔案清單中的分號 escape，或以雙引號括住檔案清單。 例如：

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

如果您在未指定 `--files` 旗標或 `--useDefaultUI` 旗標的情況下執行身分識別 scaffolder，則會在專案中建立所有可用的身分識別 UI 頁面。

---

::: moniker-end