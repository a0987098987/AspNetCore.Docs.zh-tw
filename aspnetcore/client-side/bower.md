---
title: "使用 ASP.NET Core 中的 Bower"
author: rick-anderson
description: "Bower 管理用戶端封裝。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 67695843846cfaf1619db11a7bffcc65802e0f69
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="018ec-103">管理用戶端封裝，以在 ASP.NET Core Bower</span><span class="sxs-lookup"><span data-stu-id="018ec-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="018ec-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="018ec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="018ec-105">雖然仍會保留 Bower，其維護程式會建議使用不同的解決方案。</span><span class="sxs-lookup"><span data-stu-id="018ec-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="018ec-106">與 Webpack yarn 是一種常用的替代方法，[移轉指示](https://bower.io/blog/2017/how-to-migrate-away-from-bower/)可用。</span><span class="sxs-lookup"><span data-stu-id="018ec-106">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="018ec-107">[Bower](https://bower.io/)呼叫其本身"web 版封裝管理員 」。</span><span class="sxs-lookup"><span data-stu-id="018ec-107">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="018ec-108">在.NET 生態系統中，填滿由 NuGet 的傳遞靜態內容的檔案無法驗證為 void。</span><span class="sxs-lookup"><span data-stu-id="018ec-108">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="018ec-109">針對 ASP.NET Core 專案，這些靜態檔案會於用戶端程式庫，例如固有[jQuery](http://jquery.com/)和[Bootstrap](http://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="018ec-109">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="018ec-110">.NET 程式庫，您仍然使用[NuGet](https://www.nuget.org/)封裝管理員。</span><span class="sxs-lookup"><span data-stu-id="018ec-110">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="018ec-111">設定用戶端之 ASP.NET Core 專案範本建立的新專案建置程序。</span><span class="sxs-lookup"><span data-stu-id="018ec-111">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="018ec-112">[jQuery](http://jquery.com/)和[啟動程序](http://getbootstrap.com/)安裝，而且受到 Bower。</span><span class="sxs-lookup"><span data-stu-id="018ec-112">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="018ec-113">用戶端封裝中所列*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="018ec-113">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="018ec-114">ASP.NET Core 專案範本會設定*bower.json* jQuery、 jQuery 驗證與啟動程序。</span><span class="sxs-lookup"><span data-stu-id="018ec-114">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="018ec-115">在本教學課程中，我們會將新增的支援[字型臻](http://fontawesome.io)。</span><span class="sxs-lookup"><span data-stu-id="018ec-115">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="018ec-116">可以使用安裝 bower 封裝**管理 Bower 封裝**UI 或以手動方式在*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="018ec-116">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="018ec-117">透過管理 UI 的 Bower 封裝安裝</span><span class="sxs-lookup"><span data-stu-id="018ec-117">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="018ec-118">建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)**範本。</span><span class="sxs-lookup"><span data-stu-id="018ec-118">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="018ec-119">選取**Web 應用程式**和**不驗證**。</span><span class="sxs-lookup"><span data-stu-id="018ec-119">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="018ec-120">以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理 Bower 封裝**(或者從主功能表**專案** > **管理 Bower 封裝**).</span><span class="sxs-lookup"><span data-stu-id="018ec-120">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="018ec-121">在**Bower:\<專案名稱\>**視窗中，按一下 [瀏覽] 索引標籤中，並輸入，以篩選封裝清單`font-awesome`[搜尋] 方塊中：</span><span class="sxs-lookup"><span data-stu-id="018ec-121">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![管理 bower 封裝](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="018ec-123">確認 」 將變更儲存*bower.json*」 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="018ec-123">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="018ec-124">從下拉式清單中選取版本，然後按一下**安裝** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="018ec-124">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="018ec-125">**輸出** 視窗會顯示安裝詳細資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-125">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="018ec-126">手動安裝在 bower.json</span><span class="sxs-lookup"><span data-stu-id="018ec-126">Manual installation in bower.json</span></span>

<span data-ttu-id="018ec-127">開啟*bower.json*檔案，然後加入 「 字型-實用 」 的相依性。</span><span class="sxs-lookup"><span data-stu-id="018ec-127">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="018ec-128">IntelliSense 會顯示可用的封裝。</span><span class="sxs-lookup"><span data-stu-id="018ec-128">IntelliSense shows the available packages.</span></span> <span data-ttu-id="018ec-129">選取封裝時，會顯示可用的版本。</span><span class="sxs-lookup"><span data-stu-id="018ec-129">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="018ec-130">下列映像和都不會符合您所看到的內容。</span><span class="sxs-lookup"><span data-stu-id="018ec-130">The images below are older and won't match what you see.</span></span>

![Bower 封裝總管 的 IntelliSense](bower/_static/add-package.png)

![bower 版本 IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="018ec-133">Bower 使用[語意版本設定](http://semver.org/)將組織的相依性。</span><span class="sxs-lookup"><span data-stu-id="018ec-133">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="018ec-134">語意版本設定，也稱為 SemVer 編號方式識別封裝\<主要 >。\<次要 >。\<修補程式 >。</span><span class="sxs-lookup"><span data-stu-id="018ec-134">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="018ec-135">IntelliSense 會顯示只有幾個常見的選項，以簡化語意版本設定。</span><span class="sxs-lookup"><span data-stu-id="018ec-135">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="018ec-136">IntelliSense 清單 (在上述範例 4.6.3) 中的最上層項目會被視為封裝的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="018ec-136">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="018ec-137">插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 符合最新的次要版本。</span><span class="sxs-lookup"><span data-stu-id="018ec-137">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="018ec-138">儲存*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="018ec-138">Save the *bower.json* file.</span></span> <span data-ttu-id="018ec-139">Visual Studio 會監看*bower.json*檔是否有變更。</span><span class="sxs-lookup"><span data-stu-id="018ec-139">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="018ec-140">在儲存時*bower 安裝*執行命令。</span><span class="sxs-lookup"><span data-stu-id="018ec-140">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="018ec-141">請參閱 [輸出] 視窗**Bower/npm**檢視執行的確切命令。</span><span class="sxs-lookup"><span data-stu-id="018ec-141">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="018ec-142">開啟*.bowerrc*底下*bower.json*。</span><span class="sxs-lookup"><span data-stu-id="018ec-142">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="018ec-143">`directory`屬性設定為*wwwroot/lib*表示 Bower 的位置將會安裝封裝資產。</span><span class="sxs-lookup"><span data-stu-id="018ec-143">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="018ec-144">您可以使用 [方案總管] 中的 [搜尋] 方塊尋找並顯示字型實用的封裝。</span><span class="sxs-lookup"><span data-stu-id="018ec-144">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="018ec-145">開啟*_layout.cshtml\_Layout.cshtml*檔案，然後加入至環境字型實用的 CSS 檔案[標記協助程式](xref:mvc/views/tag-helpers/intro)如`Development`。</span><span class="sxs-lookup"><span data-stu-id="018ec-145">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="018ec-146">從 [方案總管] 中，拖曳和卸除*字型 awesome.css*內`<environment names="Development">`項目。</span><span class="sxs-lookup"><span data-stu-id="018ec-146">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="018ec-147">您可以在生產環境應用程式中加入*字型 awesome.min.css*環境標記協助程式，如`Staging,Production`。</span><span class="sxs-lookup"><span data-stu-id="018ec-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="018ec-148">取代內容*Views\Home\About.cshtml* Razor 檔案，以下列標記：</span><span class="sxs-lookup"><span data-stu-id="018ec-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="018ec-149">執行應用程式，並瀏覽至 [關於] 檢視，確認字型實用的封裝可運作。</span><span class="sxs-lookup"><span data-stu-id="018ec-149">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="018ec-150">探索用戶端建置程序</span><span class="sxs-lookup"><span data-stu-id="018ec-150">Exploring the client-side build process</span></span>

<span data-ttu-id="018ec-151">大部分的 ASP.NET Core 專案範本已設定為使用 Bower。</span><span class="sxs-lookup"><span data-stu-id="018ec-151">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="018ec-152">這個下一個逐步解說開頭空白的 ASP.NET Core 專案，並將手動加入每一項，讓您可以感受 Bower 專案中的使用方式。</span><span class="sxs-lookup"><span data-stu-id="018ec-152">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="018ec-153">您可以看到所發生的專案結構和執行階段輸出每個組態變更的狀況。</span><span class="sxs-lookup"><span data-stu-id="018ec-153">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="018ec-154">使用 Bower 用戶端建置程序的一般步驟如下：</span><span class="sxs-lookup"><span data-stu-id="018ec-154">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="018ec-155">定義您的專案中使用的封裝。</span><span class="sxs-lookup"><span data-stu-id="018ec-155">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="018ec-156">從您網頁參考封裝。</span><span class="sxs-lookup"><span data-stu-id="018ec-156">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="018ec-157">定義封裝</span><span class="sxs-lookup"><span data-stu-id="018ec-157">Define packages</span></span>

<span data-ttu-id="018ec-158">一旦您在清單中的封裝*bower.json*檔案，Visual Studio 會下載它們。</span><span class="sxs-lookup"><span data-stu-id="018ec-158">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="018ec-159">下列範例會使用 Bower jQuery 和啟動程序來載入*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="018ec-159">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="018ec-160">建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)**範本。</span><span class="sxs-lookup"><span data-stu-id="018ec-160">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="018ec-161">選取**空**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="018ec-161">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="018ec-162">在 [方案總管] 中，以滑鼠右鍵按一下專案 >**加入新項目**選取**Bower 的組態檔**。</span><span class="sxs-lookup"><span data-stu-id="018ec-162">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="018ec-163">注意： A *.bowerrc*也會加入檔案。</span><span class="sxs-lookup"><span data-stu-id="018ec-163">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="018ec-164">開啟*bower.json*，加入 jquery 和要啟動`dependencies`> 一節。</span><span class="sxs-lookup"><span data-stu-id="018ec-164">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="018ec-165">產生*bower.json*檔案看起來像下列的範例。</span><span class="sxs-lookup"><span data-stu-id="018ec-165">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="018ec-166">版本會隨時間而改變，而不符合下方的影像。</span><span class="sxs-lookup"><span data-stu-id="018ec-166">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="018ec-167">儲存*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="018ec-167">Save the *bower.json* file.</span></span>

 <span data-ttu-id="018ec-168">請確認專案包含*啟動程序*和*jQuery*中的目錄*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="018ec-168">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="018ec-169">Bower 使用*.bowerrc*檔案，以安裝中的資產*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="018ec-169">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="018ec-170">注意: < 管理 Bower 封裝 > UI 提供替代檔案手動編輯。</span><span class="sxs-lookup"><span data-stu-id="018ec-170">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="018ec-171">啟用靜態檔案</span><span class="sxs-lookup"><span data-stu-id="018ec-171">Enable static files</span></span>

* <span data-ttu-id="018ec-172">新增`Microsoft.AspNetCore.StaticFiles`NuGet 封裝加入專案。</span><span class="sxs-lookup"><span data-stu-id="018ec-172">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="018ec-173">啟用靜態檔案來提供[靜態檔案中介軟體](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)。</span><span class="sxs-lookup"><span data-stu-id="018ec-173">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="018ec-174">將呼叫加入[UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)至`Configure`方法`Startup`。</span><span class="sxs-lookup"><span data-stu-id="018ec-174">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="018ec-175">參考封裝</span><span class="sxs-lookup"><span data-stu-id="018ec-175">Reference packages</span></span>

<span data-ttu-id="018ec-176">在本節中，您將建立 HTML 網頁，確認它可以存取部署的封裝。</span><span class="sxs-lookup"><span data-stu-id="018ec-176">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="018ec-177">加入名為新的 HTML 頁面*Index.html*至*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="018ec-177">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="018ec-178">注意： 您必須加入至 HTML 檔*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="018ec-178">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="018ec-179">根據預設，無法提供靜態內容之外*wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="018ec-179">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="018ec-180">請參閱[靜態檔案處理](xref:fundamentals/static-files)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="018ec-180">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="018ec-181">取代內容*Index.html*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="018ec-181">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="018ec-182">執行應用程式，並瀏覽至`http://localhost:<port>/Index.html`。</span><span class="sxs-lookup"><span data-stu-id="018ec-182">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="018ec-183">或者，使用*Index.html*開啟，請按`Ctrl+Shift+W`。</span><span class="sxs-lookup"><span data-stu-id="018ec-183">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="018ec-184">請確認 jumbotron 樣式會套用、 jQuery 程式碼可以回應在按一下按鈕時，以及啟動程序的按鈕狀態變更。</span><span class="sxs-lookup"><span data-stu-id="018ec-184">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![套用 jumbotron 樣式](bower/_static/jumbotron.png)
