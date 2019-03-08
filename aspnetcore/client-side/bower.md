---
title: 用戶端使用管理套件在 ASP.NET Core 中的 Bower
author: rick-anderson
description: 使用 Bower 管理用戶端套件。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 08e6daa537c6c6f92a1cf80d70745e8ef606f580
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665609"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="bc2a1-103">用戶端使用管理套件在 ASP.NET Core 中的 Bower</span><span class="sxs-lookup"><span data-stu-id="bc2a1-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="bc2a1-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)， [Noel Rice](https://twitter.com/noelrice1)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="bc2a1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://twitter.com/noelrice1), and [Scott Addie](https://scottaddie.com)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc2a1-105">Bower 會維護，而其維護人員會建議使用不同的解決方案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="bc2a1-106">[程式庫管理員](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/)(簡稱 LibMan) 是 Visual Studio 的新用戶端程式庫取得的工具 (Visual Studio 15.8 或更新版本）。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side library acquisition tool (Visual Studio 15.8 or later).</span></span> <span data-ttu-id="bc2a1-107">如需詳細資訊，請參閱<xref:client-side/libman/index>。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-107">For more information, see <xref:client-side/libman/index>.</span></span> <span data-ttu-id="bc2a1-108">Bower 是透過 15.5 版支援 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-108">Bower is supported in Visual Studio through version 15.5.</span></span>
>
> <span data-ttu-id="bc2a1-109">Webpack 使用 yarn 是一個常見的方法，為其[移轉指示](https://bower.io/blog/2017/how-to-migrate-away-from-bower/)可用。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-109">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="bc2a1-110">[Bower](https://bower.io/)呼叫其本身的 「 web 的套件管理員 」。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-110">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="bc2a1-111">在.NET 生態系統，它會填滿 NuGet 無法傳遞靜態內容的檔案所留下的 void。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-111">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="bc2a1-112">針對 ASP.NET Core 專案，這些靜態檔案會將用戶端程式庫，例如原本就[jQuery](http://jquery.com/)並[Bootstrap](http://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-112">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="bc2a1-113">針對.NET 程式庫，您仍然使用[NuGet](https://www.nuget.org/)封裝管理員。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-113">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="bc2a1-114">以設定用戶端的 ASP.NET Core 專案範本建立的專案建置程序。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-114">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="bc2a1-115">[jQuery](http://jquery.com/)並[Bootstrap](http://getbootstrap.com/)安裝之後，而且 Bower 支援。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-115">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="bc2a1-116">用戶端套件都會列入*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-116">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="bc2a1-117">ASP.NET Core 專案範本會設定*bower.json* jQuery、 jQuery 驗證與啟動程序。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-117">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="bc2a1-118">在本教學課程中，我們將新增的支援[Font Awesome](http://fontawesome.io)。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-118">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="bc2a1-119">Bower 套件可以使用來安裝**管理 Bower 封裝**UI 或手動*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-119">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="bc2a1-120">透過管理 Bower 封裝 UI 的安裝</span><span class="sxs-lookup"><span data-stu-id="bc2a1-120">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="bc2a1-121">建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)** 範本。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-121">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="bc2a1-122">選取  **Web 應用程式**並**無驗證**。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-122">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="bc2a1-123">以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理 Bower 封裝**(或者主功能表中，從**專案** > **管理 Bower 封裝**).</span><span class="sxs-lookup"><span data-stu-id="bc2a1-123">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="bc2a1-124">在  **Bower:\<專案名稱\>** 視窗中，按一下 瀏覽 索引標籤，並輸入，以篩選的套件清單`font-awesome`在搜尋方塊中：</span><span class="sxs-lookup"><span data-stu-id="bc2a1-124">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![管理 bower 套件](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="bc2a1-126">確認 」 將變更儲存到*bower.json*」 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-126">Confirm that the "Save changes to *bower.json*" check box is checked.</span></span> <span data-ttu-id="bc2a1-127">從下拉式清單中選取版本，然後按一下**安裝** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-127">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="bc2a1-128">**輸出**視窗會顯示安裝詳細資料。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-128">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="bc2a1-129">在 bower.json 的手動安裝</span><span class="sxs-lookup"><span data-stu-id="bc2a1-129">Manual installation in bower.json</span></span>

<span data-ttu-id="bc2a1-130">開啟*bower.json*檔案，並將 「 字型-絕佳 」 加入至相依性。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-130">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="bc2a1-131">IntelliSense 會顯示可用的套件。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-131">IntelliSense shows the available packages.</span></span> <span data-ttu-id="bc2a1-132">選取套件時，會顯示可用的版本。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-132">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="bc2a1-133">下列影像還舊，並不會符合您所看到的內容。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-133">The images below are older and won't match what you see.</span></span>

![Bower 套件總管 的 IntelliSense](bower/_static/add-package.png)

![bower 版本 IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="bc2a1-136">Bower 用法[語意版本設定](http://semver.org/)組織相依性。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-136">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="bc2a1-137">語意版本設定，也就是 SemVer 識別套件的編號配置\<主要 >。\<次要 >。\<修補程式 >。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-137">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="bc2a1-138">IntelliSense 會顯示只有幾個常見的選擇，以簡化語意版本設定。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-138">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="bc2a1-139">在 [IntelliSense] 清單 (在上述範例中的 4.6.3) 頂端的項目會被視為封裝的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-139">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="bc2a1-140">插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 比對的最新的次要版本。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-140">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="bc2a1-141">儲存*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-141">Save the *bower.json* file.</span></span> <span data-ttu-id="bc2a1-142">Visual Studio 會監看*bower.json*變更的檔案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-142">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="bc2a1-143">在儲存時， *bower 安裝*執行命令。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-143">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="bc2a1-144">請參閱 [輸出] 視窗**Bower/npm**檢視執行的確切命令。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-144">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="bc2a1-145">開啟 *.bowerrc*下方的檔案*bower.json*。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-145">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="bc2a1-146">`directory`屬性設定為*wwwroot/lib*表示位置 Bower 將安裝套件的資產。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-146">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="bc2a1-147">您可以使用 [方案總管] 中的 [搜尋] 方塊來尋找及顯示字型很棒的封裝。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-147">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="bc2a1-148">開啟*Views\Shared\_Layout.cshtml*檔案，並將字型很棒的 CSS 檔案新增至環境[標籤協助程式](xref:mvc/views/tag-helpers/intro)如`Development`。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-148">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="bc2a1-149">從 [方案總管] 拖*字型 awesome.css*內`<environment names="Development">`項目。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-149">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="bc2a1-150">您可以在生產環境應用程式中加入*字型 awesome.min.css*環境標籤協助程式，如`Staging,Production`。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-150">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="bc2a1-151">內容取代*Views\Home\About.cshtml* Razor 檔案，以下列標記：</span><span class="sxs-lookup"><span data-stu-id="bc2a1-151">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="bc2a1-152">執行應用程式並巡覽至 About 檢視來確認字型很棒的封裝可運作。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-152">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="bc2a1-153">探索用戶端建置程序</span><span class="sxs-lookup"><span data-stu-id="bc2a1-153">Exploring the client-side build process</span></span>

<span data-ttu-id="bc2a1-154">大部分的 ASP.NET Core 專案範本已設定為使用 Bower。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-154">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="bc2a1-155">這個下一個逐步解說開頭空白的 ASP.NET Core 專案，並手動新增每個片段，因此您可以取得了解如何在專案中使用 Bower。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-155">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="bc2a1-156">您可以看到會發生什麼事的專案結構和執行階段輸出，如每個組態變更。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-156">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="bc2a1-157">可搭配 Bower 的用戶端建置程序的一般步驟如下：</span><span class="sxs-lookup"><span data-stu-id="bc2a1-157">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="bc2a1-158">定義您專案中使用的封裝。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-158">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="bc2a1-159">從您的網頁參考套件。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-159">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="bc2a1-160">定義套件</span><span class="sxs-lookup"><span data-stu-id="bc2a1-160">Define packages</span></span>

<span data-ttu-id="bc2a1-161">一旦您在清單中的套件*bower.json*檔案，Visual Studio 會下載它們。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-161">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="bc2a1-162">下列範例會使用 Bower 載入 jQuery 和 Bootstrap *wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-162">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="bc2a1-163">建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)** 範本。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-163">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="bc2a1-164">選取 **空**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-164">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="bc2a1-165">在 [方案總管] 中，以滑鼠右鍵按一下專案 >**加入新項目**，然後選取**Bower 組態檔**。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-165">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="bc2a1-166">注意:A *.bowerrc*也會加入檔案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-166">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="bc2a1-167">開啟*bower.json*，並新增 jquery 來啟動`dependencies`一節。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-167">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="bc2a1-168">產生*bower.json*檔案看起來如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-168">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="bc2a1-169">版本會隨著時間改變，而不符合下列映像。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-169">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="bc2a1-170">儲存*bower.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-170">Save the *bower.json* file.</span></span>

  <span data-ttu-id="bc2a1-171">請確認專案包含*bootstrap*並*jQuery*中的目錄*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-171">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="bc2a1-172">Bower 用法 *.bowerrc*檔案，以安裝中的資產*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-172">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="bc2a1-173">注意:[管理 Bower 封裝] UI 會提供替代檔案手動編輯。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-173">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="bc2a1-174">啟用靜態檔案</span><span class="sxs-lookup"><span data-stu-id="bc2a1-174">Enable static files</span></span>

* <span data-ttu-id="bc2a1-175">新增`Microsoft.AspNetCore.StaticFiles`NuGet 封裝加入專案。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-175">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="bc2a1-176">啟用靜態檔案，以提供[靜態檔案中介軟體](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions)。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-176">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="bc2a1-177">將呼叫加入[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions)要`Configure`方法`Startup`。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-177">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="bc2a1-178">參照套件</span><span class="sxs-lookup"><span data-stu-id="bc2a1-178">Reference packages</span></span>

<span data-ttu-id="bc2a1-179">在本節中，您將建立 HTML 網頁以確認它可以存取部署的封裝。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="bc2a1-180">加入新的 HTML 網頁，名為*Index.html*要*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="bc2a1-181">注意:您必須新增至 HTML 檔案*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="bc2a1-182">根據預設，無法提供靜態內容之外*wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="bc2a1-183">請參閱[靜態檔案](xref:fundamentals/static-files)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-183">See [Static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="bc2a1-184">內容取代*Index.html*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="bc2a1-184">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="bc2a1-185">執行應用程式，並瀏覽至`http://localhost:<port>/Index.html`。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-185">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="bc2a1-186">或者，使用*Index.html*開啟，請按`Ctrl+Shift+W`。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-186">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="bc2a1-187">請確認 jumbotron 樣式會套用，jQuery 程式碼會回應按一下按鈕時，並啟動程序的按鈕狀態變更。</span><span class="sxs-lookup"><span data-stu-id="bc2a1-187">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![套用 jumbotron 樣式](bower/_static/jumbotron.png)
