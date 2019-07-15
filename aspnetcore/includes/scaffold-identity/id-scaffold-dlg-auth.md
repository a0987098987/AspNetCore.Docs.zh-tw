<span data-ttu-id="dba5d-101">執行身分識別框架：</span><span class="sxs-lookup"><span data-stu-id="dba5d-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dba5d-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dba5d-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dba5d-103">從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="dba5d-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="dba5d-104">從左窗格**新增 Scaffold**對話方塊中，選取**識別** > **新增**。</span><span class="sxs-lookup"><span data-stu-id="dba5d-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="dba5d-105">在 [ **ADD 身分識別**] 對話方塊中，選取您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="dba5d-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="dba5d-106">選取現有的版面配置頁面，或您的版面配置檔將會覆寫以不正確的標記。</span><span class="sxs-lookup"><span data-stu-id="dba5d-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="dba5d-107">當現有 *\_Layout.cshtml*已選取檔案，就**不**覆寫。</span><span class="sxs-lookup"><span data-stu-id="dba5d-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="dba5d-108">比方說`~/Pages/Shared/_Layout.cshtml`Razor 頁面`~/Views/Shared/_Layout.cshtml`MVC 專案</span><span class="sxs-lookup"><span data-stu-id="dba5d-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="dba5d-109">若要使用您現有的資料內容，請選取至少一個檔案，以覆寫。</span><span class="sxs-lookup"><span data-stu-id="dba5d-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="dba5d-110">您必須選取至少一個檔案，以加入您的資料內容。</span><span class="sxs-lookup"><span data-stu-id="dba5d-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="dba5d-111">選取您的資料內容類別。</span><span class="sxs-lookup"><span data-stu-id="dba5d-111">Select your data context class.</span></span>
  * <span data-ttu-id="dba5d-112">選取 **新增**。</span><span class="sxs-lookup"><span data-stu-id="dba5d-112">Select **ADD**.</span></span>
* <span data-ttu-id="dba5d-113">若要建立新的使用者內容，並可能是建立身分識別的自訂使用者類別：</span><span class="sxs-lookup"><span data-stu-id="dba5d-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="dba5d-114">選取  **+** 來建立新的按鈕**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="dba5d-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="dba5d-115">選取 **新增**。</span><span class="sxs-lookup"><span data-stu-id="dba5d-115">Select **ADD**.</span></span>

<span data-ttu-id="dba5d-116">注意:如果您要建立新的使用者內容，您不需要選取要覆寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="dba5d-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dba5d-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="dba5d-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dba5d-118">如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="dba5d-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="dba5d-119">新增的套件參考[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)至專案 (\*.csproj) 檔案。</span><span class="sxs-lookup"><span data-stu-id="dba5d-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="dba5d-120">在專案目錄中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="dba5d-120">Run the following command in the project directory:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="dba5d-121">執行下列命令來列出識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="dba5d-121">Run the following command to list the Identity scaffolder options:</span></span>

```console
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="dba5d-122">在 [專案] 資料夾中，執行身分識別 scaffolder 使用您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="dba5d-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="dba5d-123">例如，若要設定的預設 UI 身分識別和檔案的最小數目，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="dba5d-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="dba5d-124">使用正確的完整的名稱，為您的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="dba5d-124">Use the correct fully qualified name for your DB context:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="dba5d-125">PowerShell 會使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="dba5d-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="dba5d-126">使用 PowerShell 時，逸出分號區隔，在檔案清單，或置於雙引號括住的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="dba5d-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="dba5d-127">例如：</span><span class="sxs-lookup"><span data-stu-id="dba5d-127">For example:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="dba5d-128">如果您未指定執行身分識別 scaffolder`--files`旗標或`--useDefaultUI`旗標，所有可用的身分識別 UI 頁面將會建立在您的專案。</span><span class="sxs-lookup"><span data-stu-id="dba5d-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
