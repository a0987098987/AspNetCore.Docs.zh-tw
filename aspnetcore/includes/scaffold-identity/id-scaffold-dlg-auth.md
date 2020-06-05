::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bc15d-101">執行身分識別 scaffolder：</span><span class="sxs-lookup"><span data-stu-id="bc15d-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bc15d-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc15d-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bc15d-103">在**方案總管**中，以滑鼠右鍵按一下專案 > [**加入** > **新的 scaffold 專案**]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="bc15d-104">從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [身分**識別**] [ > **新增**]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="bc15d-105">在 [**新增識別**] 對話方塊中，選取您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="bc15d-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="bc15d-106">選取現有的版面配置頁，讓您的版面配置檔案不會以不正確的標記覆寫。</span><span class="sxs-lookup"><span data-stu-id="bc15d-106">Select your existing layout page so your layout file isn't overwritten with incorrect markup.</span></span> <span data-ttu-id="bc15d-107">選取現有的\* \_ 版面配置. cshtml\*檔案時，**不**會覆寫該檔案。</span><span class="sxs-lookup"><span data-stu-id="bc15d-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span> <span data-ttu-id="bc15d-108">例如：</span><span class="sxs-lookup"><span data-stu-id="bc15d-108">For example:</span></span>
    * <span data-ttu-id="bc15d-109">`~/Pages/Shared/_Layout.cshtml`針對具有現有 Razor Pages 基礎結構的 Razor Pages 或 Blazor 伺服器專案</span><span class="sxs-lookup"><span data-stu-id="bc15d-109">`~/Pages/Shared/_Layout.cshtml` for Razor Pages or Blazor Server projects with existing Razor Pages infrastructure</span></span>
    * <span data-ttu-id="bc15d-110">`~/Views/Shared/_Layout.cshtml`適用于具有現有 MVC 基礎結構的 MVC 專案或 Blazor 伺服器專案</span><span class="sxs-lookup"><span data-stu-id="bc15d-110">`~/Views/Shared/_Layout.cshtml` for MVC projects or Blazor Server projects with existing MVC infrastructure</span></span>
* <span data-ttu-id="bc15d-111">若要使用現有的資料內容，請至少選取一個要覆寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="bc15d-111">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="bc15d-112">您必須至少選取一個檔案來新增資料內容。</span><span class="sxs-lookup"><span data-stu-id="bc15d-112">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="bc15d-113">選取您的資料內容類別。</span><span class="sxs-lookup"><span data-stu-id="bc15d-113">Select your data context class.</span></span>
  * <span data-ttu-id="bc15d-114">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-114">Select **Add**.</span></span>
* <span data-ttu-id="bc15d-115">若要建立新的使用者內容，並可能建立身分識別的自訂使用者類別：</span><span class="sxs-lookup"><span data-stu-id="bc15d-115">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="bc15d-116">選取 [] **+** 按鈕以建立新的**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="bc15d-116">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="bc15d-117">接受預設值或指定類別（例如， `MyApplication.Data.ApplicationDbContext` ）。</span><span class="sxs-lookup"><span data-stu-id="bc15d-117">Accept the default value or specify a class (for example, `MyApplication.Data.ApplicationDbContext`).</span></span>
  * <span data-ttu-id="bc15d-118">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-118">Select **Add**.</span></span>

<span data-ttu-id="bc15d-119">注意：如果您要建立新的使用者內容，就不需要選取要覆寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="bc15d-119">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="bc15d-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bc15d-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bc15d-121">如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="bc15d-121">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="bc15d-122">將必要的 NuGet 套件參考新增至專案檔（*.csproj*）。</span><span class="sxs-lookup"><span data-stu-id="bc15d-122">Add required NuGet package references to the project file (*.csproj*).</span></span> <span data-ttu-id="bc15d-123">在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bc15d-123">Run the following commands in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="bc15d-124">執行下列命令以列出身分識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="bc15d-124">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="bc15d-125">在專案資料夾中，以您想要的選項執行 [身分識別 scaffolder]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-125">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="bc15d-126">例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="bc15d-126">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="bc15d-127">針對您的資料庫內容，請使用正確的完整名稱：</span><span class="sxs-lookup"><span data-stu-id="bc15d-127">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyApplication.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="bc15d-128">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="bc15d-128">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="bc15d-129">使用 PowerShell 時，請將檔案清單中的分號 escape，或以雙引號括住檔案清單。</span><span class="sxs-lookup"><span data-stu-id="bc15d-129">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="bc15d-130">例如：</span><span class="sxs-lookup"><span data-stu-id="bc15d-130">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyApplication.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="bc15d-131">如果您在未指定旗標或旗標的情況下執行身分識別 scaffolder `--files` `--useDefaultUI` ，則會在專案中建立所有可用的身分識別 UI 頁面。</span><span class="sxs-lookup"><span data-stu-id="bc15d-131">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bc15d-132">執行身分識別 scaffolder：</span><span class="sxs-lookup"><span data-stu-id="bc15d-132">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bc15d-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc15d-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bc15d-134">在**方案總管**中，以滑鼠右鍵按一下專案 > [**加入** > **新的 scaffold 專案**]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-134">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="bc15d-135">從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [身分**識別**] [ > **新增**]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-135">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="bc15d-136">在 [**新增識別**] 對話方塊中，選取您想要的選項。</span><span class="sxs-lookup"><span data-stu-id="bc15d-136">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="bc15d-137">選取您現有的版面配置頁，否則會以不正確的標記覆寫您的配置檔案。</span><span class="sxs-lookup"><span data-stu-id="bc15d-137">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="bc15d-138">選取現有的\* \_ 版面配置. cshtml\*檔案時，**不**會覆寫該檔案。</span><span class="sxs-lookup"><span data-stu-id="bc15d-138">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span> <span data-ttu-id="bc15d-139">例如：</span><span class="sxs-lookup"><span data-stu-id="bc15d-139">For example:</span></span>
    * <span data-ttu-id="bc15d-140">`~/Pages/Shared/_Layout.cshtml`針對 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bc15d-140">`~/Pages/Shared/_Layout.cshtml` for Razor Pages</span></span>
    * <span data-ttu-id="bc15d-141">`~/Views/Shared/_Layout.cshtml`針對 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="bc15d-141">`~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="bc15d-142">若要使用現有的資料內容，請至少選取一個要覆寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="bc15d-142">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="bc15d-143">您必須至少選取一個檔案來新增資料內容。</span><span class="sxs-lookup"><span data-stu-id="bc15d-143">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="bc15d-144">選取您的資料內容類別。</span><span class="sxs-lookup"><span data-stu-id="bc15d-144">Select your data context class.</span></span>
  * <span data-ttu-id="bc15d-145">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-145">Select **Add**.</span></span>
* <span data-ttu-id="bc15d-146">若要建立新的使用者內容，並可能建立身分識別的自訂使用者類別：</span><span class="sxs-lookup"><span data-stu-id="bc15d-146">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="bc15d-147">選取 [] **+** 按鈕以建立新的**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="bc15d-147">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="bc15d-148">接受預設值或指定類別（例如， `MyApplication.Data.ApplicationDbContext` ）。</span><span class="sxs-lookup"><span data-stu-id="bc15d-148">Accept the default value or specify a class (for example, `MyApplication.Data.ApplicationDbContext`).</span></span>
  * <span data-ttu-id="bc15d-149">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-149">Select **Add**.</span></span>

<span data-ttu-id="bc15d-150">注意：如果您要建立新的使用者內容，就不需要選取要覆寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="bc15d-150">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="bc15d-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bc15d-151">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bc15d-152">如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：</span><span class="sxs-lookup"><span data-stu-id="bc15d-152">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="bc15d-153">將[nswag.codegeneration.csharp](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)的套件參考新增至專案檔（*.csproj*）中。</span><span class="sxs-lookup"><span data-stu-id="bc15d-153">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project file (*.csproj*).</span></span> <span data-ttu-id="bc15d-154">在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bc15d-154">Run the following commands in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="bc15d-155">執行下列命令以列出身分識別 scaffolder 選項：</span><span class="sxs-lookup"><span data-stu-id="bc15d-155">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="bc15d-156">在專案資料夾中，以您想要的選項執行 [身分識別 scaffolder]。</span><span class="sxs-lookup"><span data-stu-id="bc15d-156">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="bc15d-157">例如，若要使用預設 UI 和最小檔案數目來設定身分識別，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="bc15d-157">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="bc15d-158">針對您的資料庫內容，請使用正確的完整名稱：</span><span class="sxs-lookup"><span data-stu-id="bc15d-158">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyApplication.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="bc15d-159">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="bc15d-159">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="bc15d-160">使用 PowerShell 時，請將檔案清單中的分號 escape，或以雙引號括住檔案清單。</span><span class="sxs-lookup"><span data-stu-id="bc15d-160">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="bc15d-161">例如：</span><span class="sxs-lookup"><span data-stu-id="bc15d-161">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyApplication.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="bc15d-162">如果您在未指定旗標或旗標的情況下執行身分識別 scaffolder `--files` `--useDefaultUI` ，則會在專案中建立所有可用的身分識別 UI 頁面。</span><span class="sxs-lookup"><span data-stu-id="bc15d-162">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end
