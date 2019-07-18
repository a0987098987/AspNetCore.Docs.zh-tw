<span data-ttu-id="c9199-101">執行身分識別 scaffolder:</span><span class="sxs-lookup"><span data-stu-id="c9199-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c9199-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9199-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c9199-103">從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="c9199-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c9199-104">從 [**新增 Scaffold** ] 對話方塊的左窗格中, 選取 [身分**識別** > ] [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="c9199-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="c9199-105">在 [**新增識別**] 對話方塊中, 選取您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="c9199-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="c9199-106">選取您現有的版面配置頁, 否則會以不正確的標記覆寫您的配置檔案。</span><span class="sxs-lookup"><span data-stu-id="c9199-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="c9199-107">選取現有 *\_的版面配置. cshtml*檔案時,**不**會覆寫該檔案。</span><span class="sxs-lookup"><span data-stu-id="c9199-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="c9199-108">例如: `~/Pages/Shared/_Layout.cshtml` MVC 專案的`~/Views/Shared/_Layout.cshtml` Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c9199-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="c9199-109">若要使用現有的資料內容, 請至少選取一個要覆寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="c9199-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="c9199-110">您必須至少選取一個檔案來新增資料內容。</span><span class="sxs-lookup"><span data-stu-id="c9199-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="c9199-111">選取您的資料內容類別。</span><span class="sxs-lookup"><span data-stu-id="c9199-111">Select your data context class.</span></span>
  * <span data-ttu-id="c9199-112">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="c9199-112">Select **Add**.</span></span>
* <span data-ttu-id="c9199-113">若要建立新的使用者內容, 並可能建立身分識別的自訂使用者類別:</span><span class="sxs-lookup"><span data-stu-id="c9199-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="c9199-114">選取  **+** 來建立新的按鈕**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="c9199-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="c9199-115">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="c9199-115">Select **Add**.</span></span>

<span data-ttu-id="c9199-116">注意:如果您要建立新的使用者內容, 就不需要選取要覆寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="c9199-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c9199-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c9199-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c9199-118">如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="c9199-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="c9199-119">將[nswag.codegeneration.csharp](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)的套件參考新增至專案 (.csproj) 檔案。 (\*VisualStudio)。</span><span class="sxs-lookup"><span data-stu-id="c9199-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="c9199-120">在專案目錄中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c9199-120">Run the following command in the project directory:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="c9199-121">執行下列命令來列出識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="c9199-121">Run the following command to list the Identity scaffolder options:</span></span>

```console
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="c9199-122">在專案資料夾中, 以您想要的選項執行 [身分識別 scaffolder]。</span><span class="sxs-lookup"><span data-stu-id="c9199-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="c9199-123">例如, 若要使用預設 UI 和最小檔案數目來設定身分識別, 請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="c9199-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="c9199-124">針對您的資料庫內容, 請使用正確的完整名稱:</span><span class="sxs-lookup"><span data-stu-id="c9199-124">Use the correct fully qualified name for your DB context:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="c9199-125">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="c9199-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="c9199-126">使用 PowerShell 時, 請將檔案清單中的分號 escape, 或以雙引號括住檔案清單。</span><span class="sxs-lookup"><span data-stu-id="c9199-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="c9199-127">例如：</span><span class="sxs-lookup"><span data-stu-id="c9199-127">For example:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="c9199-128">如果您在未指定`--files`旗標`--useDefaultUI`或旗標的情況下執行身分識別 scaffolder, 則會在專案中建立所有可用的身分識別 UI 頁面。</span><span class="sxs-lookup"><span data-stu-id="c9199-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
