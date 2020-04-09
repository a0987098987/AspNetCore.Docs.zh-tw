---
title: 在視覺工作室中將 LibMan 與ASP.NET核心一起使用
author: scottaddie
description: 瞭解如何在視覺工作室的ASP.NET核心專案中使用 LibMan。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: e92e6bc28ec58b26785dd6c79e71512368202a26
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658309"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="3052e-103">在視覺工作室中將 LibMan 與ASP.NET核心一起使用</span><span class="sxs-lookup"><span data-stu-id="3052e-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="3052e-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3052e-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3052e-105">Visual Studio 在 ASP.NET 核心專案中為[LibMan](xref:client-side/libman/index)提供了內建支援,包括:</span><span class="sxs-lookup"><span data-stu-id="3052e-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="3052e-106">支援在生成時配置和運行 LibMan 還原操作。</span><span class="sxs-lookup"><span data-stu-id="3052e-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="3052e-107">用於觸發 LibMan 還原和清潔操作的功能表項。</span><span class="sxs-lookup"><span data-stu-id="3052e-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="3052e-108">搜尋對話框以查找庫並將檔添加到專案中。</span><span class="sxs-lookup"><span data-stu-id="3052e-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="3052e-109">編輯對*Libman.json*&mdash;的Libman清單檔的支援。</span><span class="sxs-lookup"><span data-stu-id="3052e-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="3052e-110">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/)[(如何下載)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3052e-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3052e-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="3052e-111">Prerequisites</span></span>

* <span data-ttu-id="3052e-112">[視覺工作室 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)與**ASP.NET和網路開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="3052e-112">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="3052e-113">新增函式庫檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-113">Add library files</span></span>

<span data-ttu-id="3052e-114">函式庫檔案可以透過兩種不同的方式加入 ASP.NET 核心專案中:</span><span class="sxs-lookup"><span data-stu-id="3052e-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="3052e-115">使用「新增用戶端-側庫」對話框</span><span class="sxs-lookup"><span data-stu-id="3052e-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="3052e-116">手動設定 LibMan 清單檔項目</span><span class="sxs-lookup"><span data-stu-id="3052e-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="3052e-117">使用「新增用戶端-側庫」對話框</span><span class="sxs-lookup"><span data-stu-id="3052e-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="3052e-118">依以下步驟安裝用戶端庫:</span><span class="sxs-lookup"><span data-stu-id="3052e-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="3052e-119">在**解決方案資源管理器**中,右鍵單擊應在其中添加檔的項目資料夾。</span><span class="sxs-lookup"><span data-stu-id="3052e-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="3052e-120">選擇 **「添加** > **用戶端庫**」。</span><span class="sxs-lookup"><span data-stu-id="3052e-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="3052e-121">將顯示 **'新增用戶端-端庫**'對話框:</span><span class="sxs-lookup"><span data-stu-id="3052e-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![新增客戶端函式庫對話框](_static/add-library-dialog.png)

* <span data-ttu-id="3052e-123">從 **「提供程式**」下拉清單中選擇庫提供程式。</span><span class="sxs-lookup"><span data-stu-id="3052e-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="3052e-124">CDNJS 是預設提供程式。</span><span class="sxs-lookup"><span data-stu-id="3052e-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="3052e-125">在 **「庫**」 文字框中鍵入要提取的庫名稱。</span><span class="sxs-lookup"><span data-stu-id="3052e-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="3052e-126">IntelliSense 提供以提供的文本開頭的庫清單。</span><span class="sxs-lookup"><span data-stu-id="3052e-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="3052e-127">從"感知"列表中選擇庫。</span><span class="sxs-lookup"><span data-stu-id="3052e-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="3052e-128">請注意,庫名稱後綴在`@`符號和所選提供者已知的最新穩定版本中。</span><span class="sxs-lookup"><span data-stu-id="3052e-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="3052e-129">決定要包括哪些檔案:</span><span class="sxs-lookup"><span data-stu-id="3052e-129">Decide which files to include:</span></span>
  * <span data-ttu-id="3052e-130">選擇「**包括所有庫檔案**單選按鈕」 以包括庫的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="3052e-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="3052e-131">選擇 **「選擇特定檔案**單選按鈕」 以包括庫檔案的子集。</span><span class="sxs-lookup"><span data-stu-id="3052e-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="3052e-132">選擇單選按鈕後,將啟用檔案選擇器樹。</span><span class="sxs-lookup"><span data-stu-id="3052e-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="3052e-133">選中要下載的檔名左側的複選框。</span><span class="sxs-lookup"><span data-stu-id="3052e-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="3052e-134">指定用於在 **「目標位置」** 文字框中儲存檔案的項目資料夾。</span><span class="sxs-lookup"><span data-stu-id="3052e-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="3052e-135">作為建議,將每個庫存儲在單獨的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3052e-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="3052e-136">建議**的目標位置**資料夾基於對話框啟動的位置:</span><span class="sxs-lookup"><span data-stu-id="3052e-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="3052e-137">如果從專案根部啟動:</span><span class="sxs-lookup"><span data-stu-id="3052e-137">If launched from the project root:</span></span>
    * <span data-ttu-id="3052e-138">如果*存在 wwwroot,* 則使用*wwwroot/lib。*</span><span class="sxs-lookup"><span data-stu-id="3052e-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="3052e-139">如果*wwwroot*不存在,則使用*lib。*</span><span class="sxs-lookup"><span data-stu-id="3052e-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="3052e-140">如果從項目資料夾啟動,則使用相應的資料夾名稱。</span><span class="sxs-lookup"><span data-stu-id="3052e-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="3052e-141">資料夾建議後綴在庫名稱中。</span><span class="sxs-lookup"><span data-stu-id="3052e-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="3052e-142">下表說明了在 Razor Pages 專案中安裝 jQuery 時的資料夾建議。</span><span class="sxs-lookup"><span data-stu-id="3052e-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="3052e-143">啟動位置</span><span class="sxs-lookup"><span data-stu-id="3052e-143">Launch location</span></span>                           |<span data-ttu-id="3052e-144">建議資料夾</span><span class="sxs-lookup"><span data-stu-id="3052e-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="3052e-145">專案根(如果存在*wwwroot)*</span><span class="sxs-lookup"><span data-stu-id="3052e-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="3052e-146">*wwwroot/lib/jquery/*</span><span class="sxs-lookup"><span data-stu-id="3052e-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="3052e-147">專案根(如果*wwwroot*不存在)</span><span class="sxs-lookup"><span data-stu-id="3052e-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="3052e-148">*lib/jquery/*</span><span class="sxs-lookup"><span data-stu-id="3052e-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="3052e-149">專案中*的頁面*資料夾</span><span class="sxs-lookup"><span data-stu-id="3052e-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="3052e-150">*頁面/jquery/*</span><span class="sxs-lookup"><span data-stu-id="3052e-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="3052e-151">按下 **「安裝**」按鈕下載檔案,根據*libman.json*中的配置。</span><span class="sxs-lookup"><span data-stu-id="3052e-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="3052e-152">檢視 **「輸出**」視窗的**庫管理員**,瞭解安裝詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="3052e-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="3052e-153">例如：</span><span class="sxs-lookup"><span data-stu-id="3052e-153">For example:</span></span>

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

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="3052e-154">手動設定 LibMan 清單檔項目</span><span class="sxs-lookup"><span data-stu-id="3052e-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="3052e-155">Visual Studio 中的所有 LibMan 操作都基於專案根的 LibMan 清單 *(libman.json)* 的內容。</span><span class="sxs-lookup"><span data-stu-id="3052e-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="3052e-156">您可以手動編輯*libman.json*來設定專案的庫檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="3052e-157">可視化工作室在*保存 libman.json*後恢復所有庫檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="3052e-158">要開啟*libman.json*進行編輯,存在以下選項:</span><span class="sxs-lookup"><span data-stu-id="3052e-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="3052e-159">雙擊**解決方案資源管理員**中的*libman.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="3052e-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="3052e-160">右鍵單擊**解決方案資源管理員**中的專案,然後選擇 **「管理用戶端庫**」。</span><span class="sxs-lookup"><span data-stu-id="3052e-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="3052e-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="3052e-161">**&#8224;**</span></span>
* <span data-ttu-id="3052e-162">從可視化工作室**項目**功能表中選擇 **「管理用戶端端庫**」。</span><span class="sxs-lookup"><span data-stu-id="3052e-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="3052e-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="3052e-163">**&#8224;**</span></span>

<span data-ttu-id="3052e-164">**&#8224;** 如果*libman.json*檔在專案根中不存在,它將使用預設項範本內容創建。</span><span class="sxs-lookup"><span data-stu-id="3052e-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="3052e-165">Visual Studio 提供豐富的 JSON 編輯支援,如著色、格式設置、IntelliSense 和架構驗證。</span><span class="sxs-lookup"><span data-stu-id="3052e-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="3052e-166">LibMan 清單的 JSON[https://json.schemastore.org/libman](https://json.schemastore.org/libman)架構位於 。</span><span class="sxs-lookup"><span data-stu-id="3052e-166">The LibMan manifest's JSON schema is found at [https://json.schemastore.org/libman](https://json.schemastore.org/libman).</span></span>

<span data-ttu-id="3052e-167">使用以下清單檔,LibMan`libraries`根據 屬性中定義的設定檢索檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="3052e-168">在以下範圍內`libraries`定義的物件文本的說明如下:</span><span class="sxs-lookup"><span data-stu-id="3052e-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="3052e-169">從 CDNJS 提供程式檢索[jQuery](https://jquery.com/)版本 3.3.1 的子集。</span><span class="sxs-lookup"><span data-stu-id="3052e-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="3052e-170">`files`子集在屬性&mdash;*jquery.min.js、jquery.js*和*jquery.min.map*中定義。 *jquery.js*</span><span class="sxs-lookup"><span data-stu-id="3052e-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="3052e-171">這些檔被放置在專案的*wwwroot/lib/jquery*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3052e-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="3052e-172">將檢索整個[Bootstrap](https://getbootstrap.com/)版本 4.1.3 並將其放置在*wwwroot/lib/引導資料夾中*。</span><span class="sxs-lookup"><span data-stu-id="3052e-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="3052e-173">物件文字的屬性`provider``defaultProvider`覆蓋 屬性值。</span><span class="sxs-lookup"><span data-stu-id="3052e-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="3052e-174">LibMan 從 unpkg 提供程式檢索引導檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="3052e-175">[Lodash](https://lodash.com/)的子集得到了組織內的一個理事機構的批准。</span><span class="sxs-lookup"><span data-stu-id="3052e-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="3052e-176">*lodash.js*和*lodash.min.js*檔從*C:\\臨時\\lodash\\*的本地文件系統中檢索。</span><span class="sxs-lookup"><span data-stu-id="3052e-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="3052e-177">這些檔案將複製到專案的*wwwroot/lib/lodash*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3052e-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="3052e-178">LibMan 僅支援每個提供程式中每個庫的一個版本。</span><span class="sxs-lookup"><span data-stu-id="3052e-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="3052e-179">如果*libman.json*檔包含兩個庫具有相同的給定提供程式的庫名,則它無法進行架構驗證。</span><span class="sxs-lookup"><span data-stu-id="3052e-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="3052e-180">復原庫檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-180">Restore library files</span></span>

<span data-ttu-id="3052e-181">要從 Visual Studio 中還原庫檔,專案根中必須有一個有效的*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="3052e-182">還原的檔案放置在為每個庫指定的位置的專案中。</span><span class="sxs-lookup"><span data-stu-id="3052e-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="3052e-183">函式庫檔案可以通過兩種方式在ASP.NET核心專案中還原:</span><span class="sxs-lookup"><span data-stu-id="3052e-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="3052e-184">產生期間還原檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="3052e-185">手動回復檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="3052e-186">產生期間還原檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-186">Restore files during build</span></span>

<span data-ttu-id="3052e-187">LibMan 可以還原定義的庫文件作為生成過程的一部分。</span><span class="sxs-lookup"><span data-stu-id="3052e-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="3052e-188">默認情況下,將禁用*生成時還原*行為。</span><span class="sxs-lookup"><span data-stu-id="3052e-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="3052e-189">要啟用和測試產生時還原行為,請:</span><span class="sxs-lookup"><span data-stu-id="3052e-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="3052e-190">右鍵按一下**解決方案資源管理程式**中的*libman.json,* 然後從上下文選單中選擇 **「在生成上啟用還原用戶端端庫**」。</span><span class="sxs-lookup"><span data-stu-id="3052e-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="3052e-191">當提示安裝 NuGet 包時,按下「**是**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="3052e-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="3052e-192">[Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet 包被添加到專案中:</span><span class="sxs-lookup"><span data-stu-id="3052e-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="3052e-193">生成專案以確認 LibMan 檔還原發生。</span><span class="sxs-lookup"><span data-stu-id="3052e-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="3052e-194">該`Microsoft.Web.LibraryManager.Build`包注入一個 MSBuild 目標,該目標在專案的生成操作期間運行 LibMan。</span><span class="sxs-lookup"><span data-stu-id="3052e-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="3052e-195">檢視 LibMan 活動紀錄的**輸出「視窗**的**產生**來源:</span><span class="sxs-lookup"><span data-stu-id="3052e-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

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

<span data-ttu-id="3052e-196">啟用生成還原行為後 *,libman.json*上下文菜單**會在生成上顯示"禁用還原用戶端端庫"** 選項。</span><span class="sxs-lookup"><span data-stu-id="3052e-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="3052e-197">選擇此選項會`Microsoft.Web.LibraryManager.Build`從專案檔中刪除包引用。</span><span class="sxs-lookup"><span data-stu-id="3052e-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="3052e-198">因此,用戶端庫不再在每個生成上還原。</span><span class="sxs-lookup"><span data-stu-id="3052e-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="3052e-199">無論生成時還原設置如何,您都可以隨時從*libman.json*上下文菜單手動還原。</span><span class="sxs-lookup"><span data-stu-id="3052e-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="3052e-200">有關詳細資訊,請參閱[手動回復檔](#restore-files-manually)。</span><span class="sxs-lookup"><span data-stu-id="3052e-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="3052e-201">手動回復檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-201">Restore files manually</span></span>

<span data-ttu-id="3052e-202">手動還原函式庫檔:</span><span class="sxs-lookup"><span data-stu-id="3052e-202">To manually restore library files:</span></span>

* <span data-ttu-id="3052e-203">對解決方案中的所有專案:</span><span class="sxs-lookup"><span data-stu-id="3052e-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="3052e-204">右鍵單擊**解決方案資源管理器**中的解決方案名稱。</span><span class="sxs-lookup"><span data-stu-id="3052e-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="3052e-205">選擇 **「還原用戶端庫」** 選項。</span><span class="sxs-lookup"><span data-stu-id="3052e-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="3052e-206">對特定項目:</span><span class="sxs-lookup"><span data-stu-id="3052e-206">For a specific project:</span></span>
  * <span data-ttu-id="3052e-207">右鍵按一下**解決方案資源管理程式**中的*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="3052e-208">選擇 **「還原用戶端庫」** 選項。</span><span class="sxs-lookup"><span data-stu-id="3052e-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="3052e-209">回復執行時:</span><span class="sxs-lookup"><span data-stu-id="3052e-209">While the restore operation is running:</span></span>

* <span data-ttu-id="3052e-210">"視覺工作室"狀態列上的任務狀態中心 (TSC) 圖示將設置動畫,並將讀取 *"恢復操作已啟動*"。</span><span class="sxs-lookup"><span data-stu-id="3052e-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="3052e-211">按一下該圖示將打開一個工具提示,其中列出了已知的後台任務。</span><span class="sxs-lookup"><span data-stu-id="3052e-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="3052e-212">訊息將發送到**輸出視窗的狀態**列和**庫管理員**來源。</span><span class="sxs-lookup"><span data-stu-id="3052e-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="3052e-213">例如：</span><span class="sxs-lookup"><span data-stu-id="3052e-213">For example:</span></span>

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

## <a name="delete-library-files"></a><span data-ttu-id="3052e-214">刪除庫檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-214">Delete library files</span></span>

<span data-ttu-id="3052e-215">要執行*乾淨*操作,請刪除以前在 Visual Studio 中還原的庫檔,</span><span class="sxs-lookup"><span data-stu-id="3052e-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="3052e-216">右鍵按一下**解決方案資源管理程式**中的*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="3052e-217">選擇 **「清理用戶端-側庫」** 選項。</span><span class="sxs-lookup"><span data-stu-id="3052e-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="3052e-218">為了防止意外刪除非庫檔,清理操作不會刪除整個目錄。</span><span class="sxs-lookup"><span data-stu-id="3052e-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="3052e-219">它僅刪除上一個還原中包含的檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="3052e-220">執行清潔操作時:</span><span class="sxs-lookup"><span data-stu-id="3052e-220">While the clean operation is running:</span></span>

* <span data-ttu-id="3052e-221">Visual Studio 狀態列上的 TSC 圖示會設定動畫,並將讀取*客戶端庫操作啟動*。</span><span class="sxs-lookup"><span data-stu-id="3052e-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="3052e-222">按一下該圖示將打開一個工具提示,其中列出了已知的後台任務。</span><span class="sxs-lookup"><span data-stu-id="3052e-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="3052e-223">訊息將發送到**輸出視窗的狀態**列和**庫管理員**來源。</span><span class="sxs-lookup"><span data-stu-id="3052e-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="3052e-224">例如：</span><span class="sxs-lookup"><span data-stu-id="3052e-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="3052e-225">清除操作僅從項目中刪除檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="3052e-226">庫檔保留在緩存中,以便在未來還原操作中更快地檢索。</span><span class="sxs-lookup"><span data-stu-id="3052e-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="3052e-227">要管理儲存在本地電腦快取中的函式庫檔案,請使用[LibMan CLI](xref:client-side/libman/libman-cli)。</span><span class="sxs-lookup"><span data-stu-id="3052e-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="3052e-228">卸載庫檔案</span><span class="sxs-lookup"><span data-stu-id="3052e-228">Uninstall library files</span></span>

<span data-ttu-id="3052e-229">要卸載庫檔:</span><span class="sxs-lookup"><span data-stu-id="3052e-229">To uninstall library files:</span></span>

* <span data-ttu-id="3052e-230">開啟*利伯曼.json*。</span><span class="sxs-lookup"><span data-stu-id="3052e-230">Open *libman.json*.</span></span>
* <span data-ttu-id="3052e-231">將 caret 放置`libraries`在相應的 物件文本中。</span><span class="sxs-lookup"><span data-stu-id="3052e-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="3052e-232">按下左邊邊界中顯示的燈泡圖示,然後選擇 **「卸\<載\<library_name>* library_version>:*\*</span><span class="sxs-lookup"><span data-stu-id="3052e-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![卸載庫上下文選單選項](_static/uninstall-menu-option.png)

<span data-ttu-id="3052e-234">或者,您可以手動編輯和保存 LibMan 清單 *(libman.json*)。</span><span class="sxs-lookup"><span data-stu-id="3052e-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="3052e-235">儲存檔案時,[還原操作](#restore-library-files)將執行。</span><span class="sxs-lookup"><span data-stu-id="3052e-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="3052e-236">不再在*libman.json*中定義的庫檔將從專案中刪除。</span><span class="sxs-lookup"><span data-stu-id="3052e-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="3052e-237">更新函式庫版本</span><span class="sxs-lookup"><span data-stu-id="3052e-237">Update library version</span></span>

<span data-ttu-id="3052e-238">要檢查更新的庫版本,</span><span class="sxs-lookup"><span data-stu-id="3052e-238">To check for an updated library version:</span></span>

* <span data-ttu-id="3052e-239">開啟*利伯曼.json*。</span><span class="sxs-lookup"><span data-stu-id="3052e-239">Open *libman.json*.</span></span>
* <span data-ttu-id="3052e-240">將 caret 放置`libraries`在相應的 物件文本中。</span><span class="sxs-lookup"><span data-stu-id="3052e-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="3052e-241">按一下左側邊距中顯示的燈泡圖示。</span><span class="sxs-lookup"><span data-stu-id="3052e-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="3052e-242">將滑鼠懸停在 **「檢查更新**」。</span><span class="sxs-lookup"><span data-stu-id="3052e-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="3052e-243">LibMan 檢查庫版本比安裝的版本更新。</span><span class="sxs-lookup"><span data-stu-id="3052e-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="3052e-244">可能會出現以下結果:</span><span class="sxs-lookup"><span data-stu-id="3052e-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="3052e-245">如果已安裝最新版本,則顯示 **「未找到的更新**」消息。</span><span class="sxs-lookup"><span data-stu-id="3052e-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="3052e-246">如果未安裝,將顯示最新的穩定版本。</span><span class="sxs-lookup"><span data-stu-id="3052e-246">The latest stable version is displayed if not already installed.</span></span>

  ![檢查更新內容選單選項](_static/update-menu-option.png)

* <span data-ttu-id="3052e-248">如果具有比已安裝版本更新的預先發佈可用,則顯示預先發佈。</span><span class="sxs-lookup"><span data-stu-id="3052e-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="3052e-249">要降級為較舊的庫版本,請手動編輯*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="3052e-250">儲存檔案後, LibMan[回復操作](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="3052e-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="3052e-251">從以前的版本中刪除冗餘檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="3052e-252">從新版本添加新和更新的檔。</span><span class="sxs-lookup"><span data-stu-id="3052e-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3052e-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="3052e-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="3052e-254">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="3052e-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
