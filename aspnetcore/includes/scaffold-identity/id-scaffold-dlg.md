執行身分識別 scaffolder:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。
* 從左窗格**新增 Scaffold**對話方塊中，選取**識別** > **新增**。
* 在 [**新增識別**] 對話方塊中，選取您想要的選項。
  * 選取您現有的版面配置頁, 否則會以不正確的標記覆寫您的配置檔案。 例如，MVC 專案的 Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`
  * 選取  **+** 來建立新的按鈕**資料內容類別**。
* 選取 **新增**。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

將[nswag.codegeneration.csharp](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)的套件參考新增至專案 (.csproj) 檔案。 (\*VisualStudio)。 在專案目錄中，執行下列命令：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令來列出識別 scaffolder 選項：

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

在專案資料夾中, 以您想要的選項執行 [身分識別 scaffolder]。 例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令：

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
