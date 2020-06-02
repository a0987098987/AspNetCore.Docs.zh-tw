---
title: 在 Visual Studio 中搭配 ASP.NET Core 使用 LibMan
author: scottaddie
description: 瞭解如何在具有 Visual Studio 的 ASP.NET Core 專案中使用 LibMan。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: client-side/libman/libman-vs
ms.openlocfilehash: 45f81cbc713e7e7c1f335aef49360992d2297a81
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82770089"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="7d898-103">在 Visual Studio 中搭配 ASP.NET Core 使用 LibMan</span><span class="sxs-lookup"><span data-stu-id="7d898-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="7d898-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="7d898-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="7d898-105">Visual Studio 具有 ASP.NET Core 專案中[LibMan](xref:client-side/libman/index)的內建支援，包括：</span><span class="sxs-lookup"><span data-stu-id="7d898-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="7d898-106">支援在組建上設定和執行 LibMan 還原作業。</span><span class="sxs-lookup"><span data-stu-id="7d898-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="7d898-107">用於觸發 LibMan 還原和清除作業的功能表項目。</span><span class="sxs-lookup"><span data-stu-id="7d898-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="7d898-108">搜尋對話方塊，用來尋找程式庫並將檔案新增至專案。</span><span class="sxs-lookup"><span data-stu-id="7d898-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="7d898-109">編輯*libman* &mdash; 的支援 libman 資訊清單檔。</span><span class="sxs-lookup"><span data-stu-id="7d898-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="7d898-110">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="7d898-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d898-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="7d898-111">Prerequisites</span></span>

* <span data-ttu-id="7d898-112">**ASP.NET 和 網頁程式開發**工作負載的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="7d898-112">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="7d898-113">新增程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="7d898-113">Add library files</span></span>

<span data-ttu-id="7d898-114">程式庫檔案可以透過兩種不同的方式新增至 ASP.NET Core 專案：</span><span class="sxs-lookup"><span data-stu-id="7d898-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. <span data-ttu-id="7d898-115">[使用 [新增用戶端程式庫] 對話方塊](#use-the-add-client-side-library-dialog)</span><span class="sxs-lookup"><span data-stu-id="7d898-115">[Use the Add Client-Side Library dialog](#use-the-add-client-side-library-dialog)</span></span>
1. [<span data-ttu-id="7d898-116">手動設定 LibMan 資訊清單檔案專案</span><span class="sxs-lookup"><span data-stu-id="7d898-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="7d898-117">使用 [新增用戶端程式庫] 對話方塊</span><span class="sxs-lookup"><span data-stu-id="7d898-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="7d898-118">請遵循下列步驟來安裝用戶端程式庫：</span><span class="sxs-lookup"><span data-stu-id="7d898-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="7d898-119">在**方案總管**中，以滑鼠右鍵按一下要在其中新增檔案的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="7d898-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="7d898-120">選擇 [**新增**  >  **客戶**端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="7d898-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="7d898-121">[**新增客戶**端程式庫] 對話方塊隨即出現：</span><span class="sxs-lookup"><span data-stu-id="7d898-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![[新增用戶端程式庫] 對話方塊](_static/add-library-dialog.png)

* <span data-ttu-id="7d898-123">從 [**提供者**] 下拉式選選取程式庫提供者。</span><span class="sxs-lookup"><span data-stu-id="7d898-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="7d898-124">CDNJS 是預設的提供者。</span><span class="sxs-lookup"><span data-stu-id="7d898-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="7d898-125">在 [連結**庫**] 文字方塊中，輸入要提取的程式庫名稱。</span><span class="sxs-lookup"><span data-stu-id="7d898-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="7d898-126">IntelliSense 會提供以提供的文字開頭的程式庫清單。</span><span class="sxs-lookup"><span data-stu-id="7d898-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="7d898-127">從 [IntelliSense] 清單中選取 [程式庫]。</span><span class="sxs-lookup"><span data-stu-id="7d898-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="7d898-128">請注意，程式庫名稱的後面會加上 `@` 符號和所選提供者已知的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="7d898-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="7d898-129">決定要包含哪些檔案：</span><span class="sxs-lookup"><span data-stu-id="7d898-129">Decide which files to include:</span></span>
  * <span data-ttu-id="7d898-130">選取 [**包含所有文件庫**檔案] 選項按鈕，以包含程式庫的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="7d898-131">選取 [**選擇特定**檔案] 選項按鈕，以包含文件庫檔案的子集。</span><span class="sxs-lookup"><span data-stu-id="7d898-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="7d898-132">選取選項按鈕時，就會啟用檔案選取器樹狀目錄。</span><span class="sxs-lookup"><span data-stu-id="7d898-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="7d898-133">選取要下載的檔案名左邊的方塊。</span><span class="sxs-lookup"><span data-stu-id="7d898-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="7d898-134">指定專案資料夾，將檔案儲存在 [**目標位置**] 文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="7d898-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="7d898-135">建議將每個程式庫儲存在不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7d898-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="7d898-136">建議的 [**目標位置**] 資料夾是以啟動對話的位置為基礎：</span><span class="sxs-lookup"><span data-stu-id="7d898-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="7d898-137">如果是從專案根目錄啟動：</span><span class="sxs-lookup"><span data-stu-id="7d898-137">If launched from the project root:</span></span>
    * <span data-ttu-id="7d898-138">如果*wwwroot*存在，則會使用*wwwroot/lib* 。</span><span class="sxs-lookup"><span data-stu-id="7d898-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="7d898-139">如果*wwwroot*不存在，則會使用*lib* 。</span><span class="sxs-lookup"><span data-stu-id="7d898-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="7d898-140">如果從專案資料夾啟動，則會使用對應的資料夾名稱。</span><span class="sxs-lookup"><span data-stu-id="7d898-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="7d898-141">資料夾建議會加上程式庫名稱的尾碼。</span><span class="sxs-lookup"><span data-stu-id="7d898-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="7d898-142">下表說明在 Razor Pages 專案中安裝 jQuery 時的資料夾建議。</span><span class="sxs-lookup"><span data-stu-id="7d898-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="7d898-143">啟動位置</span><span class="sxs-lookup"><span data-stu-id="7d898-143">Launch location</span></span>                           |<span data-ttu-id="7d898-144">建議的資料夾</span><span class="sxs-lookup"><span data-stu-id="7d898-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="7d898-145">專案根目錄（如果*wwwroot*存在）</span><span class="sxs-lookup"><span data-stu-id="7d898-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="7d898-146">*wwwroot/lib/jquery/*</span><span class="sxs-lookup"><span data-stu-id="7d898-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="7d898-147">專案根目錄（如果*wwwroot*不存在）</span><span class="sxs-lookup"><span data-stu-id="7d898-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="7d898-148">*lib/jquery/*</span><span class="sxs-lookup"><span data-stu-id="7d898-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="7d898-149">Project 中的*Pages*資料夾</span><span class="sxs-lookup"><span data-stu-id="7d898-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="7d898-150">*Pages/jquery/*</span><span class="sxs-lookup"><span data-stu-id="7d898-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="7d898-151">按一下 [**安裝**] 按鈕，依據*libman*中的設定下載檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="7d898-152">查看 [**輸出**] 視窗的 [連結**庫管理員**] 摘要，以取得安裝詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7d898-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="7d898-153">例如：</span><span class="sxs-lookup"><span data-stu-id="7d898-153">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="7d898-154">手動設定 LibMan 資訊清單檔案專案</span><span class="sxs-lookup"><span data-stu-id="7d898-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="7d898-155">Visual Studio 中的所有 LibMan 作業都是以專案根目錄的 LibMan 資訊清單（*LibMan*）的內容為基礎。</span><span class="sxs-lookup"><span data-stu-id="7d898-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="7d898-156">您可以手動編輯*libman* ，以設定專案的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="7d898-157">Visual Studio 在儲存*libman*之後，就會還原所有的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="7d898-158">若要開啟*libman*以進行編輯，有下列選項：</span><span class="sxs-lookup"><span data-stu-id="7d898-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="7d898-159">按兩下**方案總管**中的*libman json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="7d898-160">以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [**管理客戶**端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="7d898-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="7d898-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="7d898-161">**&#8224;**</span></span>
* <span data-ttu-id="7d898-162">從 [Visual Studio**專案**] 功能表中，選取 [**管理客戶**端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="7d898-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="7d898-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="7d898-163">**&#8224;**</span></span>

<span data-ttu-id="7d898-164">**& #8224;** 如果專案根目錄中還沒有*libman* ，則會使用預設的專案範本內容來建立該檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="7d898-165">Visual Studio 提供豐富的 JSON 編輯支援，例如顏色標示、格式設定、IntelliSense 和架構驗證。</span><span class="sxs-lookup"><span data-stu-id="7d898-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="7d898-166">您可在找到 LibMan 資訊清單的 JSON 架構 [https://json.schemastore.org/libman](https://json.schemastore.org/libman) 。</span><span class="sxs-lookup"><span data-stu-id="7d898-166">The LibMan manifest's JSON schema is found at [https://json.schemastore.org/libman](https://json.schemastore.org/libman).</span></span>

<span data-ttu-id="7d898-167">使用下列資訊清單檔案時，LibMan 會根據屬性中所定義的設定來抓取檔案 `libraries` 。</span><span class="sxs-lookup"><span data-stu-id="7d898-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="7d898-168">在中定義的物件常值的說明 `libraries` 如下：</span><span class="sxs-lookup"><span data-stu-id="7d898-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="7d898-169">[JQuery](https://jquery.com/)版本3.3.1 的子集會從 CDNJS 提供者抓取。</span><span class="sxs-lookup"><span data-stu-id="7d898-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="7d898-170">子集是在 `files` 屬性 &mdash; *jquery. .js*、 *jquery*和*jquery. min. map*中定義。</span><span class="sxs-lookup"><span data-stu-id="7d898-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="7d898-171">這些檔案會放在專案的*wwwroot/lib/jquery*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7d898-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="7d898-172">完整的[啟動](https://getbootstrap.com/)程式版本4.1.3 會被取出並放在*wwwroot/lib/啟動*程式資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7d898-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="7d898-173">物件常值的 `provider` 屬性會覆寫 `defaultProvider` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="7d898-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="7d898-174">LibMan 會從 unpkg 提供者抓取啟動程式檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="7d898-175">組織內的管理主體已核准[lodash 所](https://lodash.com/)的子集。</span><span class="sxs-lookup"><span data-stu-id="7d898-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="7d898-176">從本機檔案系統 *（位於 C： \\ temp \\ lodash 所 \\ *）取出*lodash 所*和*lodash 所*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="7d898-177">這些檔案會複製到專案的*wwwroot/lib/lodash 所*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7d898-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="7d898-178">LibMan 只支援每個提供者的一個版本的程式庫。</span><span class="sxs-lookup"><span data-stu-id="7d898-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="7d898-179">如果*libman json*檔案包含兩個具有相同提供者之程式庫名稱的程式庫，則架構驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="7d898-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="7d898-180">還原程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="7d898-180">Restore library files</span></span>

<span data-ttu-id="7d898-181">若要從 Visual Studio 內還原程式庫檔案，專案根目錄中必須有有效的*libman json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="7d898-182">還原的檔案會放在專案中的每個程式庫所指定的位置。</span><span class="sxs-lookup"><span data-stu-id="7d898-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="7d898-183">程式庫檔案可以透過兩種方式在 ASP.NET Core 專案中還原：</span><span class="sxs-lookup"><span data-stu-id="7d898-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="7d898-184">在組建期間還原檔案</span><span class="sxs-lookup"><span data-stu-id="7d898-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="7d898-185">手動還原檔</span><span class="sxs-lookup"><span data-stu-id="7d898-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="7d898-186">在組建期間還原檔案</span><span class="sxs-lookup"><span data-stu-id="7d898-186">Restore files during build</span></span>

<span data-ttu-id="7d898-187">LibMan 可以還原已定義的程式庫檔案，做為建立程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="7d898-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="7d898-188">根據預設，會停用「*還原組建*」行為。</span><span class="sxs-lookup"><span data-stu-id="7d898-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="7d898-189">若要啟用和測試還原後的行為：</span><span class="sxs-lookup"><span data-stu-id="7d898-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="7d898-190">以滑鼠右鍵按一下**方案總管**中的 [ *libman* ]，然後從內容功能表選取 [**啟用從組建還原客戶**端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="7d898-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="7d898-191">當系統提示您安裝 NuGet 套件時，請按一下 [**是]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7d898-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="7d898-192">隨即會將[LibraryManager](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet 套件新增至專案：</span><span class="sxs-lookup"><span data-stu-id="7d898-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="7d898-193">建立專案，以確認發生 LibMan 檔案還原。</span><span class="sxs-lookup"><span data-stu-id="7d898-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="7d898-194">`Microsoft.Web.LibraryManager.Build`封裝會插入在專案的組建作業期間執行 LibMan 的 MSBuild 目標。</span><span class="sxs-lookup"><span data-stu-id="7d898-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="7d898-195">檢查 [**輸出**] 視窗的 [**組建**摘要]，以取得 LibMan 活動記錄：</span><span class="sxs-lookup"><span data-stu-id="7d898-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

<span data-ttu-id="7d898-196">啟用「還原組建」行為時，[ *libman* ] 內容功能表會在 [組建] 選項**上顯示 [停用還原客戶**端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="7d898-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="7d898-197">選取此選項會 `Microsoft.Web.LibraryManager.Build` 從專案檔中移除封裝參考。</span><span class="sxs-lookup"><span data-stu-id="7d898-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="7d898-198">因此，用戶端程式庫不會再在每個組建上還原。</span><span class="sxs-lookup"><span data-stu-id="7d898-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="7d898-199">不論「還原時的組建」設定為何，您都可以隨時從 [ *libman* ] 內容功能表手動還原。</span><span class="sxs-lookup"><span data-stu-id="7d898-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="7d898-200">如需詳細資訊，請參閱[手動還原](#restore-files-manually)檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="7d898-201">手動還原檔</span><span class="sxs-lookup"><span data-stu-id="7d898-201">Restore files manually</span></span>

<span data-ttu-id="7d898-202">若要手動還原程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="7d898-202">To manually restore library files:</span></span>

* <span data-ttu-id="7d898-203">針對方案中的所有專案：</span><span class="sxs-lookup"><span data-stu-id="7d898-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="7d898-204">以滑鼠右鍵按一下**方案總管**中的方案名稱。</span><span class="sxs-lookup"><span data-stu-id="7d898-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="7d898-205">選取 [**還原用戶端程式庫**] 選項。</span><span class="sxs-lookup"><span data-stu-id="7d898-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="7d898-206">針對特定專案：</span><span class="sxs-lookup"><span data-stu-id="7d898-206">For a specific project:</span></span>
  * <span data-ttu-id="7d898-207">以滑鼠右鍵按一下**方案總管**中的*libman json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="7d898-208">選取 [**還原用戶端程式庫**] 選項。</span><span class="sxs-lookup"><span data-stu-id="7d898-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="7d898-209">當還原作業正在執行時：</span><span class="sxs-lookup"><span data-stu-id="7d898-209">While the restore operation is running:</span></span>

* <span data-ttu-id="7d898-210">Visual Studio 狀態列上的 [工作狀態中心（TSC）] 圖示會顯示為動畫，且會*開始讀取還原*作業。</span><span class="sxs-lookup"><span data-stu-id="7d898-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="7d898-211">按一下圖示即可開啟工具提示，其中列出已知的背景工作。</span><span class="sxs-lookup"><span data-stu-id="7d898-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="7d898-212">訊息將會傳送至 [**輸出**] 視窗的狀態列和 [連結**庫管理員**] 摘要。</span><span class="sxs-lookup"><span data-stu-id="7d898-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="7d898-213">例如：</span><span class="sxs-lookup"><span data-stu-id="7d898-213">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a><span data-ttu-id="7d898-214">刪除程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="7d898-214">Delete library files</span></span>

<span data-ttu-id="7d898-215">執行*清除*作業，這會刪除先前在 Visual Studio 中還原的程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="7d898-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="7d898-216">以滑鼠右鍵按一下**方案總管**中的*libman json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="7d898-217">選取 [**清除客戶**端程式庫] 選項。</span><span class="sxs-lookup"><span data-stu-id="7d898-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="7d898-218">為了防止意外移除非程式庫檔案，清除作業不會刪除整個目錄。</span><span class="sxs-lookup"><span data-stu-id="7d898-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="7d898-219">它只會移除先前還原中所包含的檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="7d898-220">當清除作業正在執行時：</span><span class="sxs-lookup"><span data-stu-id="7d898-220">While the clean operation is running:</span></span>

* <span data-ttu-id="7d898-221">[Visual Studio] 狀態列上的 [TSC] 圖示會以動畫顯示，而且會*開始讀取用戶端程式庫*作業。</span><span class="sxs-lookup"><span data-stu-id="7d898-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="7d898-222">按一下圖示即可開啟工具提示，其中列出已知的背景工作。</span><span class="sxs-lookup"><span data-stu-id="7d898-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="7d898-223">訊息會傳送至 [**輸出**] 視窗的狀態列和 [連結**庫管理員**] 摘要。</span><span class="sxs-lookup"><span data-stu-id="7d898-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="7d898-224">例如：</span><span class="sxs-lookup"><span data-stu-id="7d898-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="7d898-225">「清除」作業只會刪除專案中的檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="7d898-226">程式庫檔案會保留在快取中，以便在未來的還原作業中更快速地取得。</span><span class="sxs-lookup"><span data-stu-id="7d898-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="7d898-227">若要管理儲存在本機電腦快取中的程式庫檔案，請使用[LIBMAN CLI](xref:client-side/libman/libman-cli)。</span><span class="sxs-lookup"><span data-stu-id="7d898-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="7d898-228">卸載程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="7d898-228">Uninstall library files</span></span>

<span data-ttu-id="7d898-229">若要卸載程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="7d898-229">To uninstall library files:</span></span>

* <span data-ttu-id="7d898-230">開啟*libman*。</span><span class="sxs-lookup"><span data-stu-id="7d898-230">Open *libman.json*.</span></span>
* <span data-ttu-id="7d898-231">將插入號放在對應的 `libraries` 物件常值內。</span><span class="sxs-lookup"><span data-stu-id="7d898-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="7d898-232">按一下左邊界中顯示的燈泡圖示，然後選取 [卸載] \*\* \< library_name> @ \< library_version>\*\*：</span><span class="sxs-lookup"><span data-stu-id="7d898-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![卸載程式庫內容功能表選項](_static/uninstall-menu-option.png)

<span data-ttu-id="7d898-234">或者，您可以手動編輯並儲存 LibMan 資訊清單（*LibMan*）。</span><span class="sxs-lookup"><span data-stu-id="7d898-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="7d898-235">[還原](#restore-library-files)作業會在儲存檔案時執行。</span><span class="sxs-lookup"><span data-stu-id="7d898-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="7d898-236">不再定義于*libman*中的程式庫檔案會從專案中移除。</span><span class="sxs-lookup"><span data-stu-id="7d898-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="7d898-237">更新程式庫版本</span><span class="sxs-lookup"><span data-stu-id="7d898-237">Update library version</span></span>

<span data-ttu-id="7d898-238">若要檢查更新的程式庫版本：</span><span class="sxs-lookup"><span data-stu-id="7d898-238">To check for an updated library version:</span></span>

* <span data-ttu-id="7d898-239">開啟*libman*。</span><span class="sxs-lookup"><span data-stu-id="7d898-239">Open *libman.json*.</span></span>
* <span data-ttu-id="7d898-240">將插入號放在對應的 `libraries` 物件常值內。</span><span class="sxs-lookup"><span data-stu-id="7d898-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="7d898-241">按一下左邊界中顯示的燈泡圖示。</span><span class="sxs-lookup"><span data-stu-id="7d898-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="7d898-242">將滑鼠停留在 [**檢查更新**] 上方。</span><span class="sxs-lookup"><span data-stu-id="7d898-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="7d898-243">LibMan 會檢查程式庫版本是否比安裝的版本還要新。</span><span class="sxs-lookup"><span data-stu-id="7d898-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="7d898-244">可能會發生下列結果：</span><span class="sxs-lookup"><span data-stu-id="7d898-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="7d898-245">如果已安裝最新版本，則會顯示 [**找不到更新**] 訊息。</span><span class="sxs-lookup"><span data-stu-id="7d898-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="7d898-246">如果尚未安裝，則會顯示最新的穩定版本。</span><span class="sxs-lookup"><span data-stu-id="7d898-246">The latest stable version is displayed if not already installed.</span></span>

  ![檢查更新內容功能表選項](_static/update-menu-option.png)

* <span data-ttu-id="7d898-248">如果預先發行版本比安裝的版本可用，則會顯示發行前版本。</span><span class="sxs-lookup"><span data-stu-id="7d898-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="7d898-249">若要降級為舊版的程式庫版本，請手動編輯*libman* 。</span><span class="sxs-lookup"><span data-stu-id="7d898-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="7d898-250">儲存檔案時，LibMan[還原](#restore-library-files)作業：</span><span class="sxs-lookup"><span data-stu-id="7d898-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="7d898-251">從舊版本移除多餘的檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="7d898-252">從新版本加入新的和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="7d898-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d898-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="7d898-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="7d898-254">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="7d898-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
