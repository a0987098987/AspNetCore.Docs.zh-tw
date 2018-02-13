---
title: "將檔案上傳至 ASP.NET Core 的 Razor 頁面"
author: guardrex
description: "了解如何將檔案上傳至 Razor 頁面。"
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 4a2c6da6ed698d1a65ee51bd00a557e607f012da
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="02eaf-103">將檔案上傳至 ASP.NET Core 的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="02eaf-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="02eaf-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="02eaf-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="02eaf-105">本節會示範如何使用 Razor 頁面上傳檔案。</span><span class="sxs-lookup"><span data-stu-id="02eaf-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="02eaf-106">在本教學課程中，[Razor 頁面電影範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)會使用簡單的模型繫結上傳檔案；這種方法很適合用來上傳小型檔案。</span><span class="sxs-lookup"><span data-stu-id="02eaf-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="02eaf-107">如需串流大型檔案的資訊，請參閱[使用串流上傳大型檔案](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)。</span><span class="sxs-lookup"><span data-stu-id="02eaf-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="02eaf-108">在下列步驟中，電影排程檔案上傳功能會新增至範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="02eaf-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="02eaf-109">電影排程是由 `Schedule` 類別表示。</span><span class="sxs-lookup"><span data-stu-id="02eaf-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="02eaf-110">此類別包含兩個版本的排程。</span><span class="sxs-lookup"><span data-stu-id="02eaf-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="02eaf-111">`PublicSchedule` 為提供給客戶的版本，</span><span class="sxs-lookup"><span data-stu-id="02eaf-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="02eaf-112">而另一個 `PrivateSchedule` 版本則用於公司員工。</span><span class="sxs-lookup"><span data-stu-id="02eaf-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="02eaf-113">每個版本都會以個別的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="02eaf-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="02eaf-114">本教學課程示範如何使用單一 POST 將兩個檔案從頁面上傳到伺服器。</span><span class="sxs-lookup"><span data-stu-id="02eaf-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="02eaf-115">安全性考量</span><span class="sxs-lookup"><span data-stu-id="02eaf-115">Security considerations</span></span>

<span data-ttu-id="02eaf-116">為使用者提供將檔案上傳到伺服器的能力時，請特別注意。</span><span class="sxs-lookup"><span data-stu-id="02eaf-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="02eaf-117">攻擊者可能會對系統發動[拒絕服務](/windows-hardware/drivers/ifs/denial-of-service)等攻擊。</span><span class="sxs-lookup"><span data-stu-id="02eaf-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="02eaf-118">以下安全性步驟可減少攻擊成功的可能性：</span><span class="sxs-lookup"><span data-stu-id="02eaf-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="02eaf-119">將檔案上傳至系統上的專用檔案上傳區域，可讓您更輕鬆地對上傳內容實施安全性措施。</span><span class="sxs-lookup"><span data-stu-id="02eaf-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="02eaf-120">允許檔案上傳時，請確保上傳位置的執行權限已停用。</span><span class="sxs-lookup"><span data-stu-id="02eaf-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="02eaf-121">使用應用程式認為安全的檔案名稱，而不使用使用者輸入或上傳檔的檔名。</span><span class="sxs-lookup"><span data-stu-id="02eaf-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="02eaf-122">僅允許特定一組核准的副檔名。</span><span class="sxs-lookup"><span data-stu-id="02eaf-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="02eaf-123">確認用戶端檢查在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="02eaf-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="02eaf-124">用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="02eaf-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="02eaf-125">檢查上傳項目的大小，並防止上傳超過預期的大小。</span><span class="sxs-lookup"><span data-stu-id="02eaf-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="02eaf-126">對上傳內容執行病毒/惡意程式碼掃描器。</span><span class="sxs-lookup"><span data-stu-id="02eaf-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="02eaf-127">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="02eaf-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="02eaf-128">完全侵占系統。</span><span class="sxs-lookup"><span data-stu-id="02eaf-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="02eaf-129">使系統超載，導致系統全面失效。</span><span class="sxs-lookup"><span data-stu-id="02eaf-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="02eaf-130">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="02eaf-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="02eaf-131">在公用介面上塗鴉。</span><span class="sxs-lookup"><span data-stu-id="02eaf-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="02eaf-132">新增 FileUpload 類別</span><span class="sxs-lookup"><span data-stu-id="02eaf-132">Add a FileUpload class</span></span>

<span data-ttu-id="02eaf-133">您可以建立 Razor 頁面來處理一對檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="02eaf-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="02eaf-134">新增 `FileUpload` 類別，以繫結至頁面並取得排程資料。</span><span class="sxs-lookup"><span data-stu-id="02eaf-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="02eaf-135">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="02eaf-135">Right click the *Models* folder.</span></span> <span data-ttu-id="02eaf-136">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="02eaf-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="02eaf-137">將類別命名為 **FileUpload** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="02eaf-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="02eaf-138">針對這兩個版本的排程，此類別具有每種版本的排程標題和屬性。</span><span class="sxs-lookup"><span data-stu-id="02eaf-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="02eaf-139">所有三個屬性都是必要項目，且標題長度必須為 3-60 個字元。</span><span class="sxs-lookup"><span data-stu-id="02eaf-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="02eaf-140">新增可上傳檔案的 Helper 方法</span><span class="sxs-lookup"><span data-stu-id="02eaf-140">Add a helper method to upload files</span></span>

<span data-ttu-id="02eaf-141">若要避免處理已上傳之排程檔案的程式碼有所重複，您可以新增靜態 Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="02eaf-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="02eaf-142">在應用程式中，建立 *Utilities* 資料夾，並使用下列內容新增 *FileHelpers.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="02eaf-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="02eaf-143">`ProcessFormFile` Helper 方法會採用 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary)，並傳回包含檔案大小及內容的字串。</span><span class="sxs-lookup"><span data-stu-id="02eaf-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="02eaf-144">系統會檢查內容類型和長度。</span><span class="sxs-lookup"><span data-stu-id="02eaf-144">The content type and length are checked.</span></span> <span data-ttu-id="02eaf-145">如果檔案未通過驗證檢查，`ModelState` 就會新增一項錯誤。</span><span class="sxs-lookup"><span data-stu-id="02eaf-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="02eaf-146">將檔案儲存至磁碟</span><span class="sxs-lookup"><span data-stu-id="02eaf-146">Save the file to disk</span></span>

<span data-ttu-id="02eaf-147">範例應用程式會將檔案的內容儲存至資料庫欄位。</span><span class="sxs-lookup"><span data-stu-id="02eaf-147">The sample app saves the file's content into a database field.</span></span> <span data-ttu-id="02eaf-148">若要將檔案的內容儲存至磁碟，請使用 [FileStream](/dotnet/api/system.io.filestream)：</span><span class="sxs-lookup"><span data-stu-id="02eaf-148">To save the file's content to disk, use a [FileStream](/dotnet/api/system.io.filestream):</span></span>

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

<span data-ttu-id="02eaf-149">背景工作處理序必須具備 `filePath` 指定之位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="02eaf-149">The worker process must have write permissions to the location specified by `filePath`.</span></span>

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="02eaf-150">將檔案儲存至 Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="02eaf-150">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="02eaf-151">若要將檔案內容上傳至 Azure Blob 儲存體，請參閱[以 .NET 開始使用 Azure Blob 儲存體](/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。</span><span class="sxs-lookup"><span data-stu-id="02eaf-151">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="02eaf-152">本主題會示範如何使用 [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) 將 [FileStream](/dotnet/api/system.io.filestream) 儲存至 Blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="02eaf-152">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="02eaf-153">新增 Schedule 類別</span><span class="sxs-lookup"><span data-stu-id="02eaf-153">Add the Schedule class</span></span>

<span data-ttu-id="02eaf-154">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="02eaf-154">Right click the *Models* folder.</span></span> <span data-ttu-id="02eaf-155">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="02eaf-155">Select **Add** > **Class**.</span></span> <span data-ttu-id="02eaf-156">將類別命名為 **Schedule** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="02eaf-156">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="02eaf-157">此類別會使用 `Display` 和 `DisplayFormat` 屬性，以在呈現排程資料時產生易記的標題和格式設定。</span><span class="sxs-lookup"><span data-stu-id="02eaf-157">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="02eaf-158">更新 MovieContext</span><span class="sxs-lookup"><span data-stu-id="02eaf-158">Update the MovieContext</span></span>

<span data-ttu-id="02eaf-159">在 `MovieContext` 中指定 `DbSet` (*Models/MovieContext.cs*) 以進行排程：</span><span class="sxs-lookup"><span data-stu-id="02eaf-159">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="02eaf-160">將 Schedule 資料表新增至資料庫</span><span class="sxs-lookup"><span data-stu-id="02eaf-160">Add the Schedule table to the database</span></span>

<span data-ttu-id="02eaf-161">開啟 [套件管理員主控台] (PMC)：[工具] > [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="02eaf-161">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="02eaf-163">在 PMC 中，執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="02eaf-163">In the PMC, execute the following commands.</span></span> <span data-ttu-id="02eaf-164">這些命令可將 `Schedule` 資料表新增至資料庫：</span><span class="sxs-lookup"><span data-stu-id="02eaf-164">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="02eaf-165">新增檔案上傳 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="02eaf-165">Add a file upload Razor Page</span></span>

<span data-ttu-id="02eaf-166">在 *Pages* 資料夾中，建立 *Schedules* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="02eaf-166">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="02eaf-167">在 *Schedules* 資料夾中，建立名為 *Index.cshtml* 的頁面，以上傳排程與下列內容：</span><span class="sxs-lookup"><span data-stu-id="02eaf-167">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="02eaf-168">每個表單群組都包含 **\<標籤 >**，其會顯示每個類別屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="02eaf-168">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="02eaf-169">`FileUpload` 模型中的 `Display` 屬性提供標籤的顯示值。</span><span class="sxs-lookup"><span data-stu-id="02eaf-169">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="02eaf-170">例如，系統會將 `UploadPublicSchedule` 屬性的顯示名稱設為 `[Display(Name="Public Schedule")]`，因此當表單呈現時會顯示 "Public Schedule"。</span><span class="sxs-lookup"><span data-stu-id="02eaf-170">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="02eaf-171">每個表單群組包含驗證 **\<範圍>**。</span><span class="sxs-lookup"><span data-stu-id="02eaf-171">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="02eaf-172">如果使用者的輸入不符合 `FileUpload` 類別中所設的內容屬性，或若有任何 `ProcessFormFile` 方法的檔案驗證檢查失敗，則模型驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="02eaf-172">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="02eaf-173">模型驗證失敗時，系統會顯示對使用者很有幫助的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="02eaf-173">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="02eaf-174">例如，系統會將 `Title` 屬性附註 `[Required]` 和 `[StringLength(60, MinimumLength = 3)]`。</span><span class="sxs-lookup"><span data-stu-id="02eaf-174">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="02eaf-175">如果使用者未提供標題，則會收到訊息，指出該值為必要項目。</span><span class="sxs-lookup"><span data-stu-id="02eaf-175">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="02eaf-176">如果使用者輸入的值少於 3 個字元或超過 60 個字元，則會收到訊息，指出該值的長度有誤。</span><span class="sxs-lookup"><span data-stu-id="02eaf-176">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="02eaf-177">如果提供的檔案沒有任何內容，則會顯示訊息，指出檔案為空白。</span><span class="sxs-lookup"><span data-stu-id="02eaf-177">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="02eaf-178">新增頁面模型</span><span class="sxs-lookup"><span data-stu-id="02eaf-178">Add the page model</span></span>

<span data-ttu-id="02eaf-179">將頁面模型 (*Index.cshtml.cs*) 新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="02eaf-179">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="02eaf-180">頁面模型 (*Index.cshtml.cs* 中的 `IndexModel`) 會繫結 `FileUpload` 類別：</span><span class="sxs-lookup"><span data-stu-id="02eaf-180">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="02eaf-181">模型也會使用排程的清單 (`IList<Schedule>`) 來顯示儲存在頁面資料庫中的排程：</span><span class="sxs-lookup"><span data-stu-id="02eaf-181">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="02eaf-182">當頁面載入 `OnGetAsync` 時，會從資料庫填入 `Schedules`，並用其來產生已載入之排程的 HTML 資料表：</span><span class="sxs-lookup"><span data-stu-id="02eaf-182">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="02eaf-183">當表單張貼至伺服器時，系統即會檢查 `ModelState`。</span><span class="sxs-lookup"><span data-stu-id="02eaf-183">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="02eaf-184">如果無效，就會重建 `Schedule`，且頁面會顯示一或多則驗證訊息，指出頁面驗證失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="02eaf-184">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="02eaf-185">如果有效，即會在 *OnPostAsync* 中使用 `FileUpload` 屬性，以完成上傳這兩個排程版本的檔案，並建立新的 `Schedule` 物件來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="02eaf-185">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="02eaf-186">接著，系統就會將排程儲存到資料庫中：</span><span class="sxs-lookup"><span data-stu-id="02eaf-186">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="02eaf-187">連結檔案上傳 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="02eaf-187">Link the file upload Razor Page</span></span>

<span data-ttu-id="02eaf-188">開啟 *_Layout.cshtml*，然後將連結新增至導覽列以存取檔案上傳頁面：</span><span class="sxs-lookup"><span data-stu-id="02eaf-188">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="02eaf-189">新增用來確認刪除排程的頁面</span><span class="sxs-lookup"><span data-stu-id="02eaf-189">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="02eaf-190">當使用者按一下 [刪除排程] 時，系統會提供取消作業的機會。</span><span class="sxs-lookup"><span data-stu-id="02eaf-190">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="02eaf-191">將刪除確認頁面 (*Delete.cshtml*) 新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="02eaf-191">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="02eaf-192">頁面模型 (*Delete.cshtml.cs*) 會將 `id` 所識別的單一排程載入要求的路由資料中。</span><span class="sxs-lookup"><span data-stu-id="02eaf-192">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="02eaf-193">將 *Delete.cshtml.cs* 檔案新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="02eaf-193">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="02eaf-194">`OnPostAsync` 方法會依據排程的 `id` 來處理其刪除作業：</span><span class="sxs-lookup"><span data-stu-id="02eaf-194">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="02eaf-195">成功刪除排程之後，`RedirectToPage` 會將使用者送回排程的 *Index.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="02eaf-195">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="02eaf-196">運作中的排程 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="02eaf-196">The working Schedules Razor Page</span></span>

<span data-ttu-id="02eaf-197">載入頁面時，排程標題的標籤和輸入、公用排程和私用排程都會顯示提交按鈕：</span><span class="sxs-lookup"><span data-stu-id="02eaf-197">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![初始載入的排程 Razor 頁面，其中不含任何驗證錯誤和空白欄位](uploading-files/_static/browser1.png)

<span data-ttu-id="02eaf-199">如果任何欄位未填妥就選取 [上傳] 按鈕，即會違反模型上的 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="02eaf-199">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="02eaf-200">`ModelState` 無效。</span><span class="sxs-lookup"><span data-stu-id="02eaf-200">The `ModelState` is invalid.</span></span> <span data-ttu-id="02eaf-201">使用者會看到驗證錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="02eaf-201">The validation error messages are displayed to the user:</span></span>

![每個輸入控制項旁會出現驗證錯誤訊息](uploading-files/_static/browser2.png)

<span data-ttu-id="02eaf-203">在 [標題] 欄位中鍵入兩個字母。</span><span class="sxs-lookup"><span data-stu-id="02eaf-203">Type two letters into the **Title** field.</span></span> <span data-ttu-id="02eaf-204">驗證訊息會變更為指出標題必須介於 3-60 個字元：</span><span class="sxs-lookup"><span data-stu-id="02eaf-204">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![標題驗證訊息已變更](uploading-files/_static/browser3.png)

<span data-ttu-id="02eaf-206">當上傳一或多個排程時，[Loaded Schedules] (載入排程) 區段會顯示載入的排程：</span><span class="sxs-lookup"><span data-stu-id="02eaf-206">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![載入的排程表，其中顯示每個排程的標題、上傳日期 (UTC)、公用版本的檔案大小和私用版本的檔案大小](uploading-files/_static/browser4.png)

<span data-ttu-id="02eaf-208">當使用者按一下這裡的 [刪除] 連結時，可前往刪除確認檢視，以選擇確認或取消刪除作業。</span><span class="sxs-lookup"><span data-stu-id="02eaf-208">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="02eaf-209">疑難排解</span><span class="sxs-lookup"><span data-stu-id="02eaf-209">Troubleshooting</span></span>

<span data-ttu-id="02eaf-210">如需針對 `IFormFile` 上傳進行疑難排解的資訊，請參閱 [ASP.NET Core 的檔案上傳：疑難排解](xref:mvc/models/file-uploads#troubleshooting)。</span><span class="sxs-lookup"><span data-stu-id="02eaf-210">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="02eaf-211">感謝您看完這份 Razor 頁面簡介。</span><span class="sxs-lookup"><span data-stu-id="02eaf-211">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="02eaf-212">感謝您提供意見反應。</span><span class="sxs-lookup"><span data-stu-id="02eaf-212">We appreciate feedback.</span></span> <span data-ttu-id="02eaf-213">完成本教學課程之後，非常建議您繼續參閱 [MVC 和 EF Core 使用者入門](xref:data/ef-mvc/intro)。</span><span class="sxs-lookup"><span data-stu-id="02eaf-213">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02eaf-214">其他資源</span><span class="sxs-lookup"><span data-stu-id="02eaf-214">Additional resources</span></span>

* [<span data-ttu-id="02eaf-215">ASP.NET Core 的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="02eaf-215">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="02eaf-216">IFormFile</span><span class="sxs-lookup"><span data-stu-id="02eaf-216">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="02eaf-217">上一步：驗證</span><span class="sxs-lookup"><span data-stu-id="02eaf-217">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
