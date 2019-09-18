<span data-ttu-id="3542c-101">執行身分識別 scaffolder:</span><span class="sxs-lookup"><span data-stu-id="3542c-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3542c-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3542c-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3542c-103">從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="3542c-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="3542c-104">從左窗格**新增 Scaffold**對話方塊中，選取**識別** > **新增**。</span><span class="sxs-lookup"><span data-stu-id="3542c-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="3542c-105">在 [**新增識別**] 對話方塊中，選取您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="3542c-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="3542c-106">選取您現有的版面配置頁, 否則會以不正確的標記覆寫您的配置檔案。</span><span class="sxs-lookup"><span data-stu-id="3542c-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="3542c-107">例如，MVC 專案的 Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="3542c-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="3542c-108">選取  **+** 來建立新的按鈕**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="3542c-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="3542c-109">選取 **新增**。</span><span class="sxs-lookup"><span data-stu-id="3542c-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3542c-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3542c-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3542c-111">如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="3542c-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="3542c-112">將[nswag.codegeneration.csharp](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)的套件參考新增至專案 (.csproj) 檔案。 (\*VisualStudio)。</span><span class="sxs-lookup"><span data-stu-id="3542c-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="3542c-113">在專案目錄中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3542c-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="3542c-114">執行下列命令來列出識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="3542c-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="3542c-115">在專案資料夾中, 以您想要的選項執行 [身分識別 scaffolder]。</span><span class="sxs-lookup"><span data-stu-id="3542c-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="3542c-116">例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3542c-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
