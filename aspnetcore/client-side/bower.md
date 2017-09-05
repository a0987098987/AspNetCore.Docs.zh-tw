---
title: "使用 ASP.NET Core 中的 Bower"
author: rick-anderson
description: "Bower 管理用戶端封裝。"
keywords: "ASP.NET Core、 bower"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: df7c43da-280e-4df6-86cb-eecec8f12bfc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 409f7afba8dd7d03b7b9aa27d93ec9167252b965
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="eedc9-104">管理用戶端封裝，以在 ASP.NET Core Bower</span><span class="sxs-lookup"><span data-stu-id="eedc9-104">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="eedc9-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Noel Rice](http://blog.falafel.com/author/noel-rice/)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="eedc9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](http://blog.falafel.com/author/noel-rice/), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="eedc9-106">[Bower](https://bower.io/)呼叫其本身"web 封裝管理員。 」</span><span class="sxs-lookup"><span data-stu-id="eedc9-106">[Bower](https://bower.io/) calls itself "A package manager for the web."</span></span> <span data-ttu-id="eedc9-107">在.NET 生態系統中，填滿由 NuGet 的傳遞靜態內容的檔案無法驗證為 void。</span><span class="sxs-lookup"><span data-stu-id="eedc9-107">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="eedc9-108">針對 ASP.NET Core 專案，這些靜態檔案會於用戶端程式庫，例如固有[jQuery](http://jquery.com/)和[Bootstrap](http://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="eedc9-108">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="eedc9-109">.NET 程式庫，您仍然使用[NuGet](https://nuget.org/)封裝管理員。</span><span class="sxs-lookup"><span data-stu-id="eedc9-109">For .NET libraries, you still use [NuGet](https://nuget.org/) package manager.</span></span>

<span data-ttu-id="eedc9-110">設定用戶端之 ASP.NET Core 專案範本建立的新專案建置程序。</span><span class="sxs-lookup"><span data-stu-id="eedc9-110">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="eedc9-111">[jQuery](http://jquery.com/)和[啟動程序](http://getbootstrap.com/)安裝，而且受到 Bower。</span><span class="sxs-lookup"><span data-stu-id="eedc9-111">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="eedc9-112">用戶端封裝中所列*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="eedc9-112">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="eedc9-113">ASP.NET Core 專案範本會設定*bower.json* jQuery、 jQuery 驗證與啟動程序。</span><span class="sxs-lookup"><span data-stu-id="eedc9-113">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="eedc9-114">在本教學課程中，我們會將新增的支援[字型臻](http://fontawesome.io)。</span><span class="sxs-lookup"><span data-stu-id="eedc9-114">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="eedc9-115">可以使用安裝 bower 封裝**管理 Bower 封裝**UI 或以手動方式在*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="eedc9-115">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="eedc9-116">透過管理 UI 的 Bower 封裝安裝</span><span class="sxs-lookup"><span data-stu-id="eedc9-116">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="eedc9-117">建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)**範本。</span><span class="sxs-lookup"><span data-stu-id="eedc9-117">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="eedc9-118">選取**Web 應用程式**和**不驗證**。</span><span class="sxs-lookup"><span data-stu-id="eedc9-118">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="eedc9-119">以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理 Bower 封裝**(或者從主功能表**專案** > **管理 Bower 封裝**).</span><span class="sxs-lookup"><span data-stu-id="eedc9-119">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="eedc9-120">在**Bower:\<專案名稱\>**視窗中，按一下 [瀏覽] 索引標籤中，並輸入，以篩選封裝清單`font-awesome`[搜尋] 方塊中：</span><span class="sxs-lookup"><span data-stu-id="eedc9-120">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![管理 bower 封裝](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="eedc9-122">確認 」 將變更儲存*bower.json*」 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eedc9-122">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="eedc9-123">從下拉式清單中選取版本，然後按一下**安裝** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="eedc9-123">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="eedc9-124">**輸出** 視窗會顯示安裝詳細資料。</span><span class="sxs-lookup"><span data-stu-id="eedc9-124">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="eedc9-125">手動安裝在 bower.json</span><span class="sxs-lookup"><span data-stu-id="eedc9-125">Manual installation in bower.json</span></span>

<span data-ttu-id="eedc9-126">開啟*bower.json*檔案，然後加入 「 字型-實用 」 的相依性。</span><span class="sxs-lookup"><span data-stu-id="eedc9-126">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="eedc9-127">IntelliSense 會顯示可用的封裝。</span><span class="sxs-lookup"><span data-stu-id="eedc9-127">IntelliSense shows the available packages.</span></span> <span data-ttu-id="eedc9-128">選取封裝時，會顯示可用的版本。</span><span class="sxs-lookup"><span data-stu-id="eedc9-128">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="eedc9-129">下列影像還舊，並不會符合您所看到的內容。</span><span class="sxs-lookup"><span data-stu-id="eedc9-129">The images below are older and will not match what you see.</span></span>

![Bower 封裝總管 的 IntelliSense](bower/_static/add-package.png)

![bower 版本 IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="eedc9-132">Bower 使用[語意版本設定](http://semver.org/)將組織的相依性。</span><span class="sxs-lookup"><span data-stu-id="eedc9-132">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="eedc9-133">語意版本設定，也稱為 SemVer 編號方式識別封裝\<主要 >。\<次要 >。\<修補程式 >。</span><span class="sxs-lookup"><span data-stu-id="eedc9-133">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="eedc9-134">IntelliSense 會顯示只有幾個常見的選項，以簡化語意版本設定。</span><span class="sxs-lookup"><span data-stu-id="eedc9-134">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="eedc9-135">IntelliSense 清單 (在上述範例 4.6.3) 中的最上層項目會被視為封裝的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="eedc9-135">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="eedc9-136">插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 符合最新的次要版本。</span><span class="sxs-lookup"><span data-stu-id="eedc9-136">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="eedc9-137">儲存*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="eedc9-137">Save the *bower.json* file.</span></span> <span data-ttu-id="eedc9-138">Visual Studio 會監看*bower.json*檔是否有變更。</span><span class="sxs-lookup"><span data-stu-id="eedc9-138">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="eedc9-139">在儲存時*bower 安裝*執行命令。</span><span class="sxs-lookup"><span data-stu-id="eedc9-139">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="eedc9-140">請參閱 [輸出] 視窗**Bower/npm**檢視執行的確切命令。</span><span class="sxs-lookup"><span data-stu-id="eedc9-140">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="eedc9-141">開啟*.bowerrc*底下*bower.json*。</span><span class="sxs-lookup"><span data-stu-id="eedc9-141">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="eedc9-142">`directory`屬性設定為*wwwroot/lib*表示 Bower 的位置將會安裝封裝資產。</span><span class="sxs-lookup"><span data-stu-id="eedc9-142">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="eedc9-143">您可以使用 [方案總管] 中的 [搜尋] 方塊尋找並顯示字型實用的封裝。</span><span class="sxs-lookup"><span data-stu-id="eedc9-143">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="eedc9-144">開啟*_layout.cshtml\_Layout.cshtml*檔案，然後加入至環境字型實用的 CSS 檔案[標記協助程式](xref:mvc/views/tag-helpers/intro)如`Development`。</span><span class="sxs-lookup"><span data-stu-id="eedc9-144">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="eedc9-145">從 [方案總管] 中，拖曳和卸除*字型 awesome.css*內`<environment names="Development">`項目。</span><span class="sxs-lookup"><span data-stu-id="eedc9-145">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

<span data-ttu-id="eedc9-146">[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]</span><span class="sxs-lookup"><span data-stu-id="eedc9-146">[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]</span></span>

<span data-ttu-id="eedc9-147">您可以在生產環境應用程式中加入*字型 awesome.min.css*環境標記協助程式，如`Staging,Production`。</span><span class="sxs-lookup"><span data-stu-id="eedc9-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="eedc9-148">取代內容*Views\Home\About.cshtml* Razor 檔案，以下列標記：</span><span class="sxs-lookup"><span data-stu-id="eedc9-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

<span data-ttu-id="eedc9-149">[!code-html[Main](bower/sample/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="eedc9-149">[!code-html[Main](bower/sample/About.cshtml)]</span></span>

<span data-ttu-id="eedc9-150">執行應用程式，並瀏覽至 [關於] 檢視，確認字型實用的封裝可運作。</span><span class="sxs-lookup"><span data-stu-id="eedc9-150">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="eedc9-151">探索用戶端建置程序</span><span class="sxs-lookup"><span data-stu-id="eedc9-151">Exploring the client-side build process</span></span>

<span data-ttu-id="eedc9-152">大部分的 ASP.NET Core 專案範本已設定為使用 Bower。</span><span class="sxs-lookup"><span data-stu-id="eedc9-152">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="eedc9-153">這個下一個逐步解說開頭空白的 ASP.NET Core 專案，並將手動加入每一項，讓您可以感受 Bower 專案中的使用方式。</span><span class="sxs-lookup"><span data-stu-id="eedc9-153">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="eedc9-154">您會看到可以專案結構會發生什麼事，且由輸出每個組態變更為執行階段。</span><span class="sxs-lookup"><span data-stu-id="eedc9-154">You see can what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="eedc9-155">使用 Bower 用戶端建置程序的一般步驟如下：</span><span class="sxs-lookup"><span data-stu-id="eedc9-155">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="eedc9-156">定義您的專案中使用的封裝。</span><span class="sxs-lookup"><span data-stu-id="eedc9-156">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="eedc9-157">從您網頁參考封裝。</span><span class="sxs-lookup"><span data-stu-id="eedc9-157">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="eedc9-158">定義封裝</span><span class="sxs-lookup"><span data-stu-id="eedc9-158">Define packages</span></span>

<span data-ttu-id="eedc9-159">一旦您在清單中的封裝*bower.json*檔案，Visual Studio 會下載它們。</span><span class="sxs-lookup"><span data-stu-id="eedc9-159">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="eedc9-160">下列範例會使用 Bower jQuery 和啟動程序來載入*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="eedc9-160">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="eedc9-161">建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)**範本。</span><span class="sxs-lookup"><span data-stu-id="eedc9-161">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="eedc9-162">選取**空**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="eedc9-162">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="eedc9-163">在 [方案總管] 中，以滑鼠右鍵按一下專案 >**加入新項目**選取**Bower 的組態檔**。</span><span class="sxs-lookup"><span data-stu-id="eedc9-163">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="eedc9-164">注意： A *.bowerrc*也會加入檔案。</span><span class="sxs-lookup"><span data-stu-id="eedc9-164">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="eedc9-165">開啟*bower.json*，加入 jquery 和要啟動`dependencies`> 一節。</span><span class="sxs-lookup"><span data-stu-id="eedc9-165">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="eedc9-166">產生*bower.json*檔案看起來像下列的範例。</span><span class="sxs-lookup"><span data-stu-id="eedc9-166">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="eedc9-167">版本會隨時間而改變，而不符合下方的影像。</span><span class="sxs-lookup"><span data-stu-id="eedc9-167">The versions will change over time and may not match the image below.</span></span>

<span data-ttu-id="eedc9-168">[!code-json[Main](bower/sample/bower.json?highlight=5,6)]</span><span class="sxs-lookup"><span data-stu-id="eedc9-168">[!code-json[Main](bower/sample/bower.json?highlight=5,6)]</span></span>

* <span data-ttu-id="eedc9-169">儲存*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="eedc9-169">Save the *bower.json* file.</span></span>

 <span data-ttu-id="eedc9-170">請確認專案包含*啟動程序*和*jQuery*中的目錄*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="eedc9-170">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="eedc9-171">Bower 使用*.bowerrc*檔案，以安裝中的資產*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="eedc9-171">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="eedc9-172">注意: < 管理 Bower 封裝 > UI 提供替代檔案手動編輯。</span><span class="sxs-lookup"><span data-stu-id="eedc9-172">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="eedc9-173">啟用靜態檔案</span><span class="sxs-lookup"><span data-stu-id="eedc9-173">Enable static files</span></span>

* <span data-ttu-id="eedc9-174">新增`Microsoft.AspNetCore.StaticFiles`NuGet 封裝加入專案。</span><span class="sxs-lookup"><span data-stu-id="eedc9-174">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="eedc9-175">啟用靜態檔案來提供[靜態檔案中介軟體](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)。</span><span class="sxs-lookup"><span data-stu-id="eedc9-175">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="eedc9-176">將呼叫加入[UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)至`Configure`方法`Startup`。</span><span class="sxs-lookup"><span data-stu-id="eedc9-176">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

<span data-ttu-id="eedc9-177">[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="eedc9-177">[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]</span></span>

### <a name="reference-packages"></a><span data-ttu-id="eedc9-178">參考封裝</span><span class="sxs-lookup"><span data-stu-id="eedc9-178">Reference packages</span></span>

<span data-ttu-id="eedc9-179">在本節中，您將建立 HTML 網頁，確認它可以存取部署的封裝。</span><span class="sxs-lookup"><span data-stu-id="eedc9-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="eedc9-180">加入名為新的 HTML 頁面*Index.html*至*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="eedc9-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="eedc9-181">注意： 您必須加入至 HTML 檔*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="eedc9-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="eedc9-182">根據預設，無法提供靜態內容之外*wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="eedc9-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="eedc9-183">請參閱[靜態檔案處理](xref:fundamentals/static-files)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="eedc9-183">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="eedc9-184">取代內容*Index.html*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="eedc9-184">Replace the contents of *Index.html* with the following markup:</span></span>

<span data-ttu-id="eedc9-185">[!code-html[Main](bower/sample/Index.html)]</span><span class="sxs-lookup"><span data-stu-id="eedc9-185">[!code-html[Main](bower/sample/Index.html)]</span></span>

* <span data-ttu-id="eedc9-186">執行應用程式，並瀏覽至`http://localhost:<port>/Index.html`。</span><span class="sxs-lookup"><span data-stu-id="eedc9-186">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="eedc9-187">或者，使用*Index.html*開啟，請按`Ctrl+Shift+W`。</span><span class="sxs-lookup"><span data-stu-id="eedc9-187">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="eedc9-188">請確認 jumbotron 樣式會套用、 jQuery 程式碼可以回應在按一下按鈕時，以及啟動程序的按鈕狀態變更。</span><span class="sxs-lookup"><span data-stu-id="eedc9-188">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![套用 jumbotron 樣式](bower/_static/jumbotron.png)
