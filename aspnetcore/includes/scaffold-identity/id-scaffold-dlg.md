<span data-ttu-id="8f627-101">執行身分識別 scaffolder：</span><span class="sxs-lookup"><span data-stu-id="8f627-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8f627-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8f627-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8f627-103">在**方案總管**中，以滑鼠右鍵按一下專案 > [**加入**  >  **新的 scaffold 專案**]。</span><span class="sxs-lookup"><span data-stu-id="8f627-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="8f627-104">從 [**加入新的 scaffold 專案**] 對話方塊的左窗格中，選取 [身分**識別**] [  >  **新增**]。</span><span class="sxs-lookup"><span data-stu-id="8f627-104">From the left pane of the **Add New Scaffolded Item** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="8f627-105">在 [**新增識別**] 對話方塊中，選取您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="8f627-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="8f627-106">選取您現有的版面配置頁，否則會以不正確的標記覆寫您的配置檔案：</span><span class="sxs-lookup"><span data-stu-id="8f627-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup:</span></span>
    * <span data-ttu-id="8f627-107">`~/Pages/Shared/_Layout.cshtml`針對 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8f627-107">`~/Pages/Shared/_Layout.cshtml` for Razor Pages</span></span>
    * <span data-ttu-id="8f627-108">`~/Views/Shared/_Layout.cshtml`針對 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="8f627-108">`~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
    * <span data-ttu-id="8f627-109">根據 `blazorserver` 預設，不會為 Razor Pages 或 MVC 設定從 Blazor 伺服器範本（）建立的 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="8f627-109">Blazor Server apps created from the Blazor Server template (`blazorserver`) aren't configured for Razor Pages or MVC by default.</span></span> <span data-ttu-id="8f627-110">將 [版面配置頁] 專案保留空白。</span><span class="sxs-lookup"><span data-stu-id="8f627-110">Leave the layout page entry blank.</span></span>
  * <span data-ttu-id="8f627-111">選取 [] **+** 按鈕以建立新的**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="8f627-111">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="8f627-112">接受預設值或指定類別（例如， `MyApplication.Data.ApplicationDbContext` ）。</span><span class="sxs-lookup"><span data-stu-id="8f627-112">Accept the default value or specify a class (for example, `MyApplication.Data.ApplicationDbContext`).</span></span>
* <span data-ttu-id="8f627-113">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="8f627-113">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="8f627-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8f627-114">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8f627-115">如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="8f627-115">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="8f627-116">將必要的 NuGet 套件參考新增至專案檔（*.csproj*）。</span><span class="sxs-lookup"><span data-stu-id="8f627-116">Add required NuGet package references to the project file (*.csproj*).</span></span> <span data-ttu-id="8f627-117">在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8f627-117">Run the following commands in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="8f627-118">執行下列命令以列出身分識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="8f627-118">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="8f627-119">在專案資料夾中，以您想要的選項執行 [身分識別 scaffolder]。</span><span class="sxs-lookup"><span data-stu-id="8f627-119">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="8f627-120">例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8f627-120">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
