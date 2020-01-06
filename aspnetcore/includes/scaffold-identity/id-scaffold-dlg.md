<span data-ttu-id="fb9c5-101">執行身分識別 scaffolder：</span><span class="sxs-lookup"><span data-stu-id="fb9c5-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb9c5-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb9c5-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fb9c5-103">在**方案總管**中，以滑鼠右鍵按一下專案 >**加入** > **新的 scaffold 專案**。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="fb9c5-104">從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [身分**識別** > **新增**]。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="fb9c5-105">在 [**新增識別**] 對話方塊中，選取您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="fb9c5-106">選取您現有的版面配置頁，否則會以不正確的標記覆寫您的配置檔案。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="fb9c5-107">例如，MVC 專案的 Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="fb9c5-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="fb9c5-108">選取 [ **+** ] 按鈕，以建立新的**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="fb9c5-109">選取 [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fb9c5-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fb9c5-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fb9c5-111">如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="fb9c5-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="fb9c5-112">將必要的 NuGet 套件參考新增至專案（\*.csproj）檔案。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-112">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="fb9c5-113">在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fb9c5-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="fb9c5-114">執行下列命令以列出身分識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="fb9c5-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="fb9c5-115">在專案資料夾中，以您想要的選項執行 [身分識別 scaffolder]。</span><span class="sxs-lookup"><span data-stu-id="fb9c5-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="fb9c5-116">例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fb9c5-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
