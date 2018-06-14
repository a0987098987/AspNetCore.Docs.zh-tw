執行身分識別 scaffolder:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**方案總管 中**，以滑鼠右鍵按一下專案 >**新增** > **新的 Scaffold 項目**。
* 從左窗格的**新增 Scaffold**對話方塊中，選取**識別** > **新增**。
* 在**新增身分識別** 對話方塊中，選取您想要的選項。
  * 選取現有的版面配置頁面，或您配置的檔案就會覆寫不正確的標記。 選取現有的 _Layout.cshtml 檔案時，它是**不**覆寫。

 例如`~/Pages/Shared/_Layout.cshtml`Razor 頁面`~/Views/Shared/_Layout.cshtml`MVC 專案
* 若要使用現有的資料內容，請選取至少一個檔案，以覆寫。 您必須選取至少一個檔案，以加入您的資料內容。
  * 選取您的資料內容類別。
  * 選取**新增**。
* 若要建立新的使用者內容，並可能是建立自訂的使用者身分識別：
  * 選取**+** 按鈕以建立新**資料內容類別**。
  * 選取**新增**。

注意： 如果您要建立新的使用者內容，您沒有選擇要覆寫的檔案。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果先前未安裝 ASP.NET scaffolder，請立即安裝：

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

封裝將參考加入至[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)至專案 (\*.csproj) 檔案。 在專案目錄中，執行下列命令：

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令來列出識別 scaffolder 選項：

```cli
dotnet aspnet-codegenerator identity -h
```

在專案資料夾中，執行身分識別 scaffolder 與您想要的選項。 例如，若要設定的預設 UI 身分識別和檔案的最小數目，請執行下列命令。 使用正確的完整的名稱的 DB 內容：

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
