執行身分識別框架：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。
* 從左窗格**新增 Scaffold**對話方塊中，選取**識別** > **新增**。
* 在 [ **ADD 身分識別**] 對話方塊中，選取您想要的選項。
  * 選取現有的版面配置頁面，或您的版面配置檔將會覆寫以不正確的標記。 當現有 *\_Layout.cshtml*已選取檔案，就**不**覆寫。

 比方說`~/Pages/Shared/_Layout.cshtml`Razor 頁面`~/Views/Shared/_Layout.cshtml`MVC 專案
* 若要使用您現有的資料內容，請選取至少一個檔案，以覆寫。 您必須選取至少一個檔案，以加入您的資料內容。
  * 選取您的資料內容類別。
  * 選取 **新增**。
* 若要建立新的使用者內容，並可能是建立身分識別的自訂使用者類別：
  * 選取  **+** 來建立新的按鈕**資料內容類別**。
  * 選取 **新增**。

注意： 如果您要建立新的使用者內容，您不需要選取要覆寫的檔案。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

新增的套件參考[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)至專案 (\*.csproj) 檔案。 在專案目錄中，執行下列命令：

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令來列出識別 scaffolder 選項：

```cli
dotnet aspnet-codegenerator identity -h
```

在 [專案] 資料夾中，執行身分識別 scaffolder 使用您想要的選項。 例如，若要設定的預設 UI 身分識別和檔案的最小數目，請執行下列命令。 使用正確的完整的名稱，為您的資料庫內容：

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

Powershell 會使用分號做為命令分隔符號。 使用 powershell 時，逸出分號區隔，在檔案清單，或置於雙引號括住的檔案清單。 例如: 

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

如果您未指定執行身分識別 scaffolder`--files`旗標或`--useDefaultUI`旗標，所有可用的身分識別 UI 頁面將會建立在您的專案。

-------------
