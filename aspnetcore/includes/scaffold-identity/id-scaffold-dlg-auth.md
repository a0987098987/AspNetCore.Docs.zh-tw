執行身分識別 scaffolder:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。
* 從 [**新增 Scaffold** ] 對話方塊的左窗格中, 選取 [身分**識別** > ] [**新增**]。
* 在 [**新增識別**] 對話方塊中, 選取您想要的選項。
  * 選取您現有的版面配置頁, 否則會以不正確的標記覆寫您的配置檔案。 選取現有 *\_的版面配置. cshtml*檔案時,**不**會覆寫該檔案。

 例如: `~/Pages/Shared/_Layout.cshtml` MVC 專案的`~/Views/Shared/_Layout.cshtml` Razor Pages
* 若要使用現有的資料內容, 請至少選取一個要覆寫的檔案。 您必須至少選取一個檔案來新增資料內容。
  * 選取您的資料內容類別。
  * 選取 [新增]  。
* 若要建立新的使用者內容, 並可能建立身分識別的自訂使用者類別:
  * 選取  **+** 來建立新的按鈕**資料內容類別**。
  * 選取 [新增]  。

注意:如果您要建立新的使用者內容, 就不需要選取要覆寫的檔案。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

將[nswag.codegeneration.csharp](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)的套件參考新增至專案 (.csproj) 檔案。 (\*VisualStudio)。 在專案目錄中，執行下列命令：

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令來列出識別 scaffolder 選項：

```console
dotnet aspnet-codegenerator identity -h
```

在專案資料夾中, 以您想要的選項執行 [身分識別 scaffolder]。 例如, 若要使用預設 UI 和最小檔案數目來設定身分識別, 請執行下列命令。 針對您的資料庫內容, 請使用正確的完整名稱:

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

PowerShell 使用分號做為命令分隔符號。 使用 PowerShell 時, 請將檔案清單中的分號 escape, 或以雙引號括住檔案清單。 例如：

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

如果您在未指定`--files`旗標`--useDefaultUI`或旗標的情況下執行身分識別 scaffolder, 則會在專案中建立所有可用的身分識別 UI 頁面。

---
