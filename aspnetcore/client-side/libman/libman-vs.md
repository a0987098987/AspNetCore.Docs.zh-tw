---
title: 使用 Visual Studio 中的 ASP.NET Core 使用 LibMan
author: scottaddie
description: 了解如何使用 Visual Studio 的 ASP.NET Core 專案中使用 LibMan。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: ebfb405516d968bf5d5b8cff956a9892457027f2
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813465"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="42222-103">使用 Visual Studio 中的 ASP.NET Core 使用 LibMan</span><span class="sxs-lookup"><span data-stu-id="42222-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="42222-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="42222-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="42222-105">Visual Studio 的內建支援[LibMan](xref:client-side/libman/index)在 ASP.NET Core 專案中，包括：</span><span class="sxs-lookup"><span data-stu-id="42222-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="42222-106">設定和執行組建 LibMan 還原作業的支援。</span><span class="sxs-lookup"><span data-stu-id="42222-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="42222-107">觸發 LibMan 還原和清除作業的功能表項目。</span><span class="sxs-lookup"><span data-stu-id="42222-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="42222-108">尋找程式庫及將檔案新增至專案的 [搜尋] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="42222-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="42222-109">編輯支援*libman.json*&mdash;LibMan 資訊清單檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="42222-110">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="42222-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42222-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="42222-111">Prerequisites</span></span>

* <span data-ttu-id="42222-112">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 和 **ASP.NET 與 Web 開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="42222-112">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="42222-113">新增程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="42222-113">Add library files</span></span>

<span data-ttu-id="42222-114">程式庫檔案可以新增至 ASP.NET Core 專案中，兩個不同的方式：</span><span class="sxs-lookup"><span data-stu-id="42222-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. <span data-ttu-id="42222-115">[使用 [新增用戶端程式庫] 對話方塊](#use-the-add-client-side-library-dialog)</span><span class="sxs-lookup"><span data-stu-id="42222-115">[Use the Add Client-Side Library dialog](#use-the-add-client-side-library-dialog)</span></span>
1. [<span data-ttu-id="42222-116">手動設定 LibMan 資訊清單檔案項目</span><span class="sxs-lookup"><span data-stu-id="42222-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="42222-117">使用 [新增用戶端程式庫] 對話方塊</span><span class="sxs-lookup"><span data-stu-id="42222-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="42222-118">請遵循下列步驟來安裝用戶端程式庫：</span><span class="sxs-lookup"><span data-stu-id="42222-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="42222-119">在 [**方案總管] 中**，以滑鼠右鍵按一下專案資料夾中的檔案應該新增。</span><span class="sxs-lookup"><span data-stu-id="42222-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="42222-120">選擇**新增** > **用戶端程式庫**。</span><span class="sxs-lookup"><span data-stu-id="42222-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="42222-121">**新增用戶端程式庫**對話方塊隨即出現：</span><span class="sxs-lookup"><span data-stu-id="42222-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![新增用戶端程式庫 對話方塊](_static/add-library-dialog.png)

* <span data-ttu-id="42222-123">選取 從程式庫提供者**提供者**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="42222-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="42222-124">CDNJS 是預設提供者。</span><span class="sxs-lookup"><span data-stu-id="42222-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="42222-125">類型程式庫名稱，在擷取**程式庫**文字方塊。</span><span class="sxs-lookup"><span data-stu-id="42222-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="42222-126">IntelliSense 提供一份文件庫開始提供的文字。</span><span class="sxs-lookup"><span data-stu-id="42222-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="42222-127">從 [IntelliSense] 清單中選取程式庫。</span><span class="sxs-lookup"><span data-stu-id="42222-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="42222-128">請注意，程式庫名稱會加上`@`符號和已知的所選的提供者的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="42222-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="42222-129">決定要包含哪些檔案：</span><span class="sxs-lookup"><span data-stu-id="42222-129">Decide which files to include:</span></span>
  * <span data-ttu-id="42222-130">選取 **包括所有的程式庫檔**選項按鈕，以包含所有的程式庫的檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="42222-131">選取 **選擇特定的檔案**選項按鈕，以包含程式庫檔案的子集。</span><span class="sxs-lookup"><span data-stu-id="42222-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="42222-132">選取選項按鈕時，會啟用檔案選取器樹狀目錄。</span><span class="sxs-lookup"><span data-stu-id="42222-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="42222-133">請檢查要下載的檔案名稱的左邊的方塊。</span><span class="sxs-lookup"><span data-stu-id="42222-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="42222-134">指定儲存在檔案的專案資料夾**目標位置**文字方塊。</span><span class="sxs-lookup"><span data-stu-id="42222-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="42222-135">建議，另一個資料夾中儲存每個程式庫。</span><span class="sxs-lookup"><span data-stu-id="42222-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="42222-136">建議**目標位置**資料夾以啟動 [] 對話方塊的位置為基礎：</span><span class="sxs-lookup"><span data-stu-id="42222-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="42222-137">如果從專案根目錄中啟動：</span><span class="sxs-lookup"><span data-stu-id="42222-137">If launched from the project root:</span></span>
    * <span data-ttu-id="42222-138">*wwwroot/lib*如果使用*wwwroot*存在。</span><span class="sxs-lookup"><span data-stu-id="42222-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="42222-139">*lib*如果使用*wwwroot*不存在。</span><span class="sxs-lookup"><span data-stu-id="42222-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="42222-140">如果啟動專案資料夾中，會使用對應的資料夾名稱。</span><span class="sxs-lookup"><span data-stu-id="42222-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="42222-141">程式庫名稱會加上資料夾的建議。</span><span class="sxs-lookup"><span data-stu-id="42222-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="42222-142">下表說明資料夾建議，Razor 頁面專案中安裝 jQuery 時。</span><span class="sxs-lookup"><span data-stu-id="42222-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="42222-143">啟動位置</span><span class="sxs-lookup"><span data-stu-id="42222-143">Launch location</span></span>                           |<span data-ttu-id="42222-144">建議的資料夾</span><span class="sxs-lookup"><span data-stu-id="42222-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="42222-145">專案根目錄 (如果*wwwroot*存在)</span><span class="sxs-lookup"><span data-stu-id="42222-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="42222-146">*wwwroot/lib/jquery/*</span><span class="sxs-lookup"><span data-stu-id="42222-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="42222-147">專案根目錄 (如果*wwwroot*不存在)</span><span class="sxs-lookup"><span data-stu-id="42222-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="42222-148">*lib/jquery/*</span><span class="sxs-lookup"><span data-stu-id="42222-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="42222-149">*頁面*專案中的資料夾</span><span class="sxs-lookup"><span data-stu-id="42222-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="42222-150">*Pages/jquery/*</span><span class="sxs-lookup"><span data-stu-id="42222-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="42222-151">按一下 [**安裝**] 按鈕，下載的檔案，每個在組態*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="42222-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="42222-152">檢閱**程式庫管理員**摘要**輸出**安裝詳細資料 視窗。</span><span class="sxs-lookup"><span data-stu-id="42222-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="42222-153">例如：</span><span class="sxs-lookup"><span data-stu-id="42222-153">For example:</span></span>

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

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="42222-154">手動設定 LibMan 資訊清單檔案項目</span><span class="sxs-lookup"><span data-stu-id="42222-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="42222-155">在 Visual Studio 中的所有 LibMan 作業為都基礎的專案根目錄 LibMan 資訊清單的內容 (*libman.json*)。</span><span class="sxs-lookup"><span data-stu-id="42222-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="42222-156">您可以手動編輯*libman.json*設定專案的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="42222-157">Visual Studio 中還原所有的程式庫檔案一次*libman.json*儲存。</span><span class="sxs-lookup"><span data-stu-id="42222-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="42222-158">若要開啟  *libman.json*進行編輯，有下列選項：</span><span class="sxs-lookup"><span data-stu-id="42222-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="42222-159">按兩下*libman.json*中的檔案**方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="42222-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="42222-160">中的專案上按一下滑鼠右鍵**方案總管**，然後選取**管理用戶端程式庫**。</span><span class="sxs-lookup"><span data-stu-id="42222-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="42222-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="42222-161">**&#8224;**</span></span>
* <span data-ttu-id="42222-162">選取 **管理的用戶端程式庫**從 Visual Studio**專案**功能表。</span><span class="sxs-lookup"><span data-stu-id="42222-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="42222-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="42222-163">**&#8224;**</span></span>

<span data-ttu-id="42222-164">**&#8224;** 如果*libman.json*檔案不存在的專案根目錄中，將會使用預設項目範本內容建立。</span><span class="sxs-lookup"><span data-stu-id="42222-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="42222-165">Visual Studio 提供豐富編輯支援，像是顏色標示、 格式、 IntelliSense 和結構描述驗證的 JSON。</span><span class="sxs-lookup"><span data-stu-id="42222-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="42222-166">LibMan 資訊清單的 JSON 結構描述位於[ https://json.schemastore.org/libman ](https://json.schemastore.org/libman)。</span><span class="sxs-lookup"><span data-stu-id="42222-166">The LibMan manifest's JSON schema is found at [https://json.schemastore.org/libman](https://json.schemastore.org/libman).</span></span>

<span data-ttu-id="42222-167">下列資訊清單檔案時，LibMan 擷取檔案中定義的組態每`libraries`屬性。</span><span class="sxs-lookup"><span data-stu-id="42222-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="42222-168">物件常值中定義的說明`libraries`遵循：</span><span class="sxs-lookup"><span data-stu-id="42222-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="42222-169">子集[jQuery](https://jquery.com/) 3.3.1 版會從 CDNJS 提供者。</span><span class="sxs-lookup"><span data-stu-id="42222-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="42222-170">中所定義的子集`files`屬性&mdash;*jquery.min.js*， *jquery.js*，以及*jquery.min.map*。</span><span class="sxs-lookup"><span data-stu-id="42222-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="42222-171">檔案會放在專案的*wwwroot/lib/jquery*資料夾。</span><span class="sxs-lookup"><span data-stu-id="42222-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="42222-172">將整個[Bootstrap](https://getbootstrap.com/) 4.1.3 版會擷取並放置於*wwwroot/lib/啟動程序*資料夾。</span><span class="sxs-lookup"><span data-stu-id="42222-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="42222-173">物件常值`provider`屬性會覆寫`defaultProvider`屬性值。</span><span class="sxs-lookup"><span data-stu-id="42222-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="42222-174">LibMan 擷取 unpkg 提供者的啟動程序的檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="42222-175">子集[Lodash](https://lodash.com/)已核准的組織內的控管主體。</span><span class="sxs-lookup"><span data-stu-id="42222-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="42222-176">*Lodash.js*並*lodash.min.js*檔案會從本機檔案系統中擷取*c:\\temp\\lodash\\* 。</span><span class="sxs-lookup"><span data-stu-id="42222-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="42222-177">檔案會複製到專案的*wwwroot/lib/lodash*資料夾。</span><span class="sxs-lookup"><span data-stu-id="42222-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="42222-178">LibMan 僅支援一個版本從每個提供者的每個程式庫。</span><span class="sxs-lookup"><span data-stu-id="42222-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="42222-179">*Libman.json*檔案無法通過結構描述驗證，如果它包含具有相同的程式庫名稱，指定提供者的兩個程式庫。</span><span class="sxs-lookup"><span data-stu-id="42222-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="42222-180">將程式庫檔案還原</span><span class="sxs-lookup"><span data-stu-id="42222-180">Restore library files</span></span>

<span data-ttu-id="42222-181">若要還原從 Visual Studio 內的程式庫檔案，必須有有效*libman.json*專案根目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="42222-182">已還原的檔案會放置在專案中指定的每個程式庫的位置。</span><span class="sxs-lookup"><span data-stu-id="42222-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="42222-183">您可以在 ASP.NET Core 專案中有兩種還原程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="42222-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="42222-184">在建置時還原檔案</span><span class="sxs-lookup"><span data-stu-id="42222-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="42222-185">以手動方式還原檔案</span><span class="sxs-lookup"><span data-stu-id="42222-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="42222-186">在建置時還原檔案</span><span class="sxs-lookup"><span data-stu-id="42222-186">Restore files during build</span></span>

<span data-ttu-id="42222-187">LibMan 可以還原的已定義的程式庫檔案做為建置程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="42222-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="42222-188">根據預設，*還原上建置*停用行為。</span><span class="sxs-lookup"><span data-stu-id="42222-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="42222-189">若要啟用並測試還原上建置行為：</span><span class="sxs-lookup"><span data-stu-id="42222-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="42222-190">以滑鼠右鍵按一下*libman.json*中**方案總管**，然後選取**啟用還原用戶端程式庫建置**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="42222-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="42222-191">按一下 **是**按鈕時提示您安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="42222-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="42222-192">[Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet 封裝加入至專案：</span><span class="sxs-lookup"><span data-stu-id="42222-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="42222-193">建置專案，以確認 LibMan 檔案還原，就會發生。</span><span class="sxs-lookup"><span data-stu-id="42222-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="42222-194">`Microsoft.Web.LibraryManager.Build`套件插入專案的建立作業期間執行 LibMan MSBuild 目標。</span><span class="sxs-lookup"><span data-stu-id="42222-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="42222-195">檢閱**建置**摘要**輸出**LibMan 活動記錄 視窗：</span><span class="sxs-lookup"><span data-stu-id="42222-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

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

<span data-ttu-id="42222-196">啟用還原上建置行為時， *libman.json*操作功能表會顯示**停用還原用戶端程式庫建置**選項。</span><span class="sxs-lookup"><span data-stu-id="42222-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="42222-197">選取此選項會移除`Microsoft.Web.LibraryManager.Build`套件從專案檔的參考。</span><span class="sxs-lookup"><span data-stu-id="42222-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="42222-198">因此，用戶端程式庫不再會在每個組建上還原。</span><span class="sxs-lookup"><span data-stu-id="42222-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="42222-199">不論還原上組建設定中，您可以手動還原的任何時候*libman.json*操作功能表。</span><span class="sxs-lookup"><span data-stu-id="42222-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="42222-200">如需詳細資訊，請參閱 <<c0> [ 以手動方式還原檔案](#restore-files-manually)。</span><span class="sxs-lookup"><span data-stu-id="42222-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="42222-201">以手動方式還原檔案</span><span class="sxs-lookup"><span data-stu-id="42222-201">Restore files manually</span></span>

<span data-ttu-id="42222-202">若要以手動方式還原程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="42222-202">To manually restore library files:</span></span>

* <span data-ttu-id="42222-203">在方案中的所有專案：</span><span class="sxs-lookup"><span data-stu-id="42222-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="42222-204">中的方案名稱上按一下滑鼠右鍵**方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="42222-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="42222-205">選取 **還原用戶端程式庫**選項。</span><span class="sxs-lookup"><span data-stu-id="42222-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="42222-206">針對特定專案：</span><span class="sxs-lookup"><span data-stu-id="42222-206">For a specific project:</span></span>
  * <span data-ttu-id="42222-207">以滑鼠右鍵按一下*libman.json*中的檔案**方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="42222-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="42222-208">選取 **還原用戶端程式庫**選項。</span><span class="sxs-lookup"><span data-stu-id="42222-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="42222-209">雖然還原作業正在執行：</span><span class="sxs-lookup"><span data-stu-id="42222-209">While the restore operation is running:</span></span>

* <span data-ttu-id="42222-210">在 Visual Studio 的 [狀態] 列上的工作狀態中心 (TSC) 圖示會變成動畫，以及將讀取*還原作業已開始*。</span><span class="sxs-lookup"><span data-stu-id="42222-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="42222-211">按一下圖示即可開啟列出的已知的背景工作的工具提示。</span><span class="sxs-lookup"><span data-stu-id="42222-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="42222-212">會將訊息傳送至 [狀態] 列與**程式庫管理員**摘要**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="42222-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="42222-213">例如：</span><span class="sxs-lookup"><span data-stu-id="42222-213">For example:</span></span>

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

## <a name="delete-library-files"></a><span data-ttu-id="42222-214">刪除程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="42222-214">Delete library files</span></span>

<span data-ttu-id="42222-215">若要執行*全新*作業，這會刪除先前還原 Visual Studio 中的程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="42222-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="42222-216">以滑鼠右鍵按一下*libman.json*中的檔案**方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="42222-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="42222-217">選取 **全新的用戶端程式庫**選項。</span><span class="sxs-lookup"><span data-stu-id="42222-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="42222-218">若要防止意外移除程式庫檔案，清除作業並不會刪除整個目錄。</span><span class="sxs-lookup"><span data-stu-id="42222-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="42222-219">它只會移除已包含在前一個還原的檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="42222-220">當正在執行清除的作業：</span><span class="sxs-lookup"><span data-stu-id="42222-220">While the clean operation is running:</span></span>

* <span data-ttu-id="42222-221">Visual Studio 的 [狀態] 列 TSC 圖示項目建立動畫，並將讀取*用戶端程式庫作業啟動*。</span><span class="sxs-lookup"><span data-stu-id="42222-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="42222-222">按一下圖示即可開啟列出的已知的背景工作的工具提示。</span><span class="sxs-lookup"><span data-stu-id="42222-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="42222-223">訊息會傳送至 [狀態] 列與**程式庫管理員**摘要**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="42222-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="42222-224">例如：</span><span class="sxs-lookup"><span data-stu-id="42222-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="42222-225">清除作業只會刪除從專案的檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="42222-226">程式庫檔案保持在未來的還原作業上更快速地擷取的快取中。</span><span class="sxs-lookup"><span data-stu-id="42222-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="42222-227">若要管理儲存在本機電腦的快取中的程式庫檔案，請使用[LibMan CLI](xref:client-side/libman/libman-cli)。</span><span class="sxs-lookup"><span data-stu-id="42222-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="42222-228">解除安裝程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="42222-228">Uninstall library files</span></span>

<span data-ttu-id="42222-229">若要解除安裝程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="42222-229">To uninstall library files:</span></span>

* <span data-ttu-id="42222-230">開啟*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="42222-230">Open *libman.json*.</span></span>
* <span data-ttu-id="42222-231">放置在對應的插入號`libraries`物件常值。</span><span class="sxs-lookup"><span data-stu-id="42222-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="42222-232">按一下左邊界中，會出現燈泡圖示，然後選取**解除安裝\<library_name > @\<library_version >** :</span><span class="sxs-lookup"><span data-stu-id="42222-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![解除安裝程式庫操作功能表選項](_static/uninstall-menu-option.png)

<span data-ttu-id="42222-234">或者，您可以手動編輯並儲存 LibMan 資訊清單 (*libman.json*)。</span><span class="sxs-lookup"><span data-stu-id="42222-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="42222-235">[還原作業](#restore-library-files)時儲存檔案時執行。</span><span class="sxs-lookup"><span data-stu-id="42222-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="42222-236">不會再定義中的程式庫檔案*libman.json*從專案中移除。</span><span class="sxs-lookup"><span data-stu-id="42222-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="42222-237">更新程式庫版本</span><span class="sxs-lookup"><span data-stu-id="42222-237">Update library version</span></span>

<span data-ttu-id="42222-238">若要檢查有更新的程式庫版本：</span><span class="sxs-lookup"><span data-stu-id="42222-238">To check for an updated library version:</span></span>

* <span data-ttu-id="42222-239">開啟*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="42222-239">Open *libman.json*.</span></span>
* <span data-ttu-id="42222-240">放置在對應的插入號`libraries`物件常值。</span><span class="sxs-lookup"><span data-stu-id="42222-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="42222-241">按一下燈泡圖示出現在左邊界中。</span><span class="sxs-lookup"><span data-stu-id="42222-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="42222-242">將滑鼠停留**檢查是否有更新**。</span><span class="sxs-lookup"><span data-stu-id="42222-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="42222-243">LibMan 會檢查程式庫版本比安裝的版本還新。</span><span class="sxs-lookup"><span data-stu-id="42222-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="42222-244">可能會發生下列結果：</span><span class="sxs-lookup"><span data-stu-id="42222-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="42222-245">A**找不到更新**如果已安裝最新版本，就會顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="42222-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="42222-246">最新穩定版本會顯示如果沒有已安裝。</span><span class="sxs-lookup"><span data-stu-id="42222-246">The latest stable version is displayed if not already installed.</span></span>

  ![檢查有更新快顯功能表選項](_static/update-menu-option.png)

* <span data-ttu-id="42222-248">如果可用的發行前版本比已安裝的版本還新，則會顯示發行前版本。</span><span class="sxs-lookup"><span data-stu-id="42222-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="42222-249">若要降級至較舊的程式庫版本，請以手動方式編輯*libman.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="42222-250">當儲存檔案時，LibMan[還原作業](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="42222-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="42222-251">從先前版本中移除多餘的檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="42222-252">從新的版本中加入全新和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="42222-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42222-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="42222-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="42222-254">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="42222-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
