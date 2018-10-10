---
title: 將檔案上傳至 ASP.NET Core 的 Razor 頁面
author: guardrex
description: 了解如何將檔案上傳至 Razor Page。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/11/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 92e72869967b6e3202c97b92e341ea22adc69651
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912497"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="f1d05-103">將檔案上傳至 ASP.NET Core 的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="f1d05-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="f1d05-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1d05-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f1d05-105">本主題為建置基礎[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample)在<xref:tutorials/razor-pages/razor-pages-start>。</span><span class="sxs-lookup"><span data-stu-id="f1d05-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="f1d05-106">本主題說明如何使用簡單的模型繫結檔案上, 傳適用於上傳小型檔案。</span><span class="sxs-lookup"><span data-stu-id="f1d05-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="f1d05-107">如需串流大型檔案的資訊，請參閱[使用串流上傳大型檔案](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)。</span><span class="sxs-lookup"><span data-stu-id="f1d05-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="f1d05-108">在下列步驟中，電影排程檔案上傳功能會新增至範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="f1d05-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="f1d05-109">電影排程是由 `Schedule` 類別表示。</span><span class="sxs-lookup"><span data-stu-id="f1d05-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="f1d05-110">此類別包含兩個版本的排程。</span><span class="sxs-lookup"><span data-stu-id="f1d05-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="f1d05-111">`PublicSchedule` 為提供給客戶的版本，</span><span class="sxs-lookup"><span data-stu-id="f1d05-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="f1d05-112">而另一個 `PrivateSchedule` 版本則用於公司員工。</span><span class="sxs-lookup"><span data-stu-id="f1d05-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="f1d05-113">每個版本都會以個別的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="f1d05-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="f1d05-114">本教學課程示範如何使用單一 POST 將兩個檔案從頁面上傳到伺服器。</span><span class="sxs-lookup"><span data-stu-id="f1d05-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="f1d05-115">安全性考量</span><span class="sxs-lookup"><span data-stu-id="f1d05-115">Security considerations</span></span>

<span data-ttu-id="f1d05-116">為使用者提供將檔案上傳到伺服器的能力時，請特別注意。</span><span class="sxs-lookup"><span data-stu-id="f1d05-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="f1d05-117">攻擊者可能會對系統發動[拒絕服務](/windows-hardware/drivers/ifs/denial-of-service)等攻擊。</span><span class="sxs-lookup"><span data-stu-id="f1d05-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="f1d05-118">以下安全性步驟可減少攻擊成功的可能性：</span><span class="sxs-lookup"><span data-stu-id="f1d05-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="f1d05-119">將檔案上傳至系統上的專用檔案上傳區域，可讓您更輕鬆地對上傳內容實施安全性措施。</span><span class="sxs-lookup"><span data-stu-id="f1d05-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="f1d05-120">允許檔案上傳時，請確保上傳位置的執行權限已停用。</span><span class="sxs-lookup"><span data-stu-id="f1d05-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="f1d05-121">使用應用程式認為安全的檔案名稱，而不使用使用者輸入或上傳檔的檔名。</span><span class="sxs-lookup"><span data-stu-id="f1d05-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="f1d05-122">僅允許特定一組核准的副檔名。</span><span class="sxs-lookup"><span data-stu-id="f1d05-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="f1d05-123">確認用戶端檢查在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="f1d05-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="f1d05-124">用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="f1d05-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="f1d05-125">檢查上傳項目的大小，並防止上傳超過預期的大小。</span><span class="sxs-lookup"><span data-stu-id="f1d05-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="f1d05-126">對上傳內容執行病毒/惡意程式碼掃描器。</span><span class="sxs-lookup"><span data-stu-id="f1d05-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="f1d05-127">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="f1d05-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="f1d05-128">完全侵占系統。</span><span class="sxs-lookup"><span data-stu-id="f1d05-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="f1d05-129">使系統超載，導致系統全面失效。</span><span class="sxs-lookup"><span data-stu-id="f1d05-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="f1d05-130">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="f1d05-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="f1d05-131">在公用介面上塗鴉。</span><span class="sxs-lookup"><span data-stu-id="f1d05-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="f1d05-132">新增 FileUpload 類別</span><span class="sxs-lookup"><span data-stu-id="f1d05-132">Add a FileUpload class</span></span>

<span data-ttu-id="f1d05-133">您可以建立 Razor Page 來處理一對檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="f1d05-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="f1d05-134">新增 `FileUpload` 類別，以繫結至頁面並取得排程資料。</span><span class="sxs-lookup"><span data-stu-id="f1d05-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="f1d05-135">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f1d05-135">Right click the *Models* folder.</span></span> <span data-ttu-id="f1d05-136">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f1d05-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="f1d05-137">將類別命名為 **FileUpload** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="f1d05-137">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="f1d05-138">針對這兩個版本的排程，此類別具有每種版本的排程標題和屬性。</span><span class="sxs-lookup"><span data-stu-id="f1d05-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="f1d05-139">所有三個屬性都是必要項目，且標題長度必須為 3-60 個字元。</span><span class="sxs-lookup"><span data-stu-id="f1d05-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="f1d05-140">新增可上傳檔案的 Helper 方法</span><span class="sxs-lookup"><span data-stu-id="f1d05-140">Add a helper method to upload files</span></span>

<span data-ttu-id="f1d05-141">若要避免處理已上傳之排程檔案的程式碼有所重複，您可以新增靜態 Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="f1d05-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="f1d05-142">在應用程式中，建立 *Utilities* 資料夾，並使用下列內容新增 *FileHelpers.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f1d05-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="f1d05-143">`ProcessFormFile` Helper 方法會採用 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary)，並傳回包含檔案大小及內容的字串。</span><span class="sxs-lookup"><span data-stu-id="f1d05-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="f1d05-144">系統會檢查內容類型和長度。</span><span class="sxs-lookup"><span data-stu-id="f1d05-144">The content type and length are checked.</span></span> <span data-ttu-id="f1d05-145">如果檔案未通過驗證檢查，`ModelState` 就會新增一項錯誤。</span><span class="sxs-lookup"><span data-stu-id="f1d05-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="f1d05-146">將檔案儲存至磁碟</span><span class="sxs-lookup"><span data-stu-id="f1d05-146">Save the file to disk</span></span>

<span data-ttu-id="f1d05-147">範例應用程式會將上傳的檔案儲存到資料庫欄位。</span><span class="sxs-lookup"><span data-stu-id="f1d05-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="f1d05-148">若要將檔案儲存到磁碟，請使用 [FileStream](/dotnet/api/system.io.filestream)。</span><span class="sxs-lookup"><span data-stu-id="f1d05-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="f1d05-149">下列範例會將 `FileUpload.UploadPublicSchedule` 所保存的檔案複製到 `OnPostAsync` 方法的 `FileStream` 中。</span><span class="sxs-lookup"><span data-stu-id="f1d05-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="f1d05-150">`FileStream` 則會將檔案寫入磁碟所提供的 `<PATH-AND-FILE-NAME>`：</span><span class="sxs-lookup"><span data-stu-id="f1d05-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="f1d05-151">背景工作處理序必須具備 `filePath` 指定之位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="f1d05-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="f1d05-152">`filePath`「必須」包含檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="f1d05-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="f1d05-153">如果未提供檔案名稱，則會在執行階段擲回 [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception)。</span><span class="sxs-lookup"><span data-stu-id="f1d05-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="f1d05-154">永遠不要將上傳的檔案保存在與應用程式相同的目錄樹狀結構中。</span><span class="sxs-lookup"><span data-stu-id="f1d05-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="f1d05-155">程式碼範例不提供防禦惡意檔案上傳的任何伺服器端保護。</span><span class="sxs-lookup"><span data-stu-id="f1d05-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="f1d05-156">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="f1d05-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="f1d05-157">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="f1d05-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="f1d05-158">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="f1d05-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="f1d05-159">將檔案儲存至 Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="f1d05-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="f1d05-160">若要將檔案內容上傳至 Azure Blob 儲存體，請參閱[以 .NET 開始使用 Azure Blob 儲存體](/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。</span><span class="sxs-lookup"><span data-stu-id="f1d05-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="f1d05-161">本主題會示範如何使用 [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) 將 [FileStream](/dotnet/api/system.io.filestream) 儲存至 Blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="f1d05-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="f1d05-162">新增 Schedule 類別</span><span class="sxs-lookup"><span data-stu-id="f1d05-162">Add the Schedule class</span></span>

<span data-ttu-id="f1d05-163">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f1d05-163">Right click the *Models* folder.</span></span> <span data-ttu-id="f1d05-164">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f1d05-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="f1d05-165">將類別命名為 **Schedule** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="f1d05-165">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="f1d05-166">此類別會使用 `Display` 和 `DisplayFormat` 屬性，以在呈現排程資料時產生易記的標題和格式設定。</span><span class="sxs-lookup"><span data-stu-id="f1d05-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="f1d05-167">更新 RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="f1d05-167">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="f1d05-168">在 `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) 中為排程指定 `DbSet`：</span><span class="sxs-lookup"><span data-stu-id="f1d05-168">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="f1d05-169">更新 MovieContext</span><span class="sxs-lookup"><span data-stu-id="f1d05-169">Update the MovieContext</span></span>

<span data-ttu-id="f1d05-170">在 `MovieContext` 中指定 `DbSet` (*Models/MovieContext.cs*) 以進行排程：</span><span class="sxs-lookup"><span data-stu-id="f1d05-170">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="f1d05-171">將 Schedule 資料表新增至資料庫</span><span class="sxs-lookup"><span data-stu-id="f1d05-171">Add the Schedule table to the database</span></span>

<span data-ttu-id="f1d05-172">開啟 [套件管理員主控台] (PMC)：[工具] > [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="f1d05-172">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC 功能表](upload-files/_static/pmc.png)

<span data-ttu-id="f1d05-174">在 PMC 中，執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="f1d05-174">In the PMC, execute the following commands.</span></span> <span data-ttu-id="f1d05-175">這些命令可將 `Schedule` 資料表新增至資料庫：</span><span class="sxs-lookup"><span data-stu-id="f1d05-175">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="f1d05-176">新增檔案上傳 Razor Page</span><span class="sxs-lookup"><span data-stu-id="f1d05-176">Add a file upload Razor Page</span></span>

<span data-ttu-id="f1d05-177">在 *Pages* 資料夾中，建立 *Schedules* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f1d05-177">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="f1d05-178">在 *Schedules* 資料夾中，建立名為 *Index.cshtml* 的頁面，以上傳排程與下列內容：</span><span class="sxs-lookup"><span data-stu-id="f1d05-178">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="f1d05-179">每個表單群組都包含 **\<標籤 >**，其會顯示每個類別屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="f1d05-179">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="f1d05-180">`FileUpload` 模型中的 `Display` 屬性提供標籤的顯示值。</span><span class="sxs-lookup"><span data-stu-id="f1d05-180">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="f1d05-181">例如，系統會將 `UploadPublicSchedule` 屬性的顯示名稱設為 `[Display(Name="Public Schedule")]`，因此當表單呈現時會顯示 "Public Schedule"。</span><span class="sxs-lookup"><span data-stu-id="f1d05-181">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="f1d05-182">每個表單群組包含驗證 **\<範圍>**。</span><span class="sxs-lookup"><span data-stu-id="f1d05-182">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="f1d05-183">如果使用者的輸入不符合 `FileUpload` 類別中所設的內容屬性，或若有任何 `ProcessFormFile` 方法的檔案驗證檢查失敗，則模型驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="f1d05-183">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="f1d05-184">模型驗證失敗時，系統會顯示對使用者很有幫助的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="f1d05-184">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="f1d05-185">例如，系統會將 `Title` 屬性附註 `[Required]` 和 `[StringLength(60, MinimumLength = 3)]`。</span><span class="sxs-lookup"><span data-stu-id="f1d05-185">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="f1d05-186">如果使用者未提供標題，則會收到訊息，指出該值為必要項目。</span><span class="sxs-lookup"><span data-stu-id="f1d05-186">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="f1d05-187">如果使用者輸入的值少於 3 個字元或超過 60 個字元，則會收到訊息，指出該值的長度有誤。</span><span class="sxs-lookup"><span data-stu-id="f1d05-187">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="f1d05-188">如果提供的檔案沒有任何內容，則會顯示訊息，指出檔案為空白。</span><span class="sxs-lookup"><span data-stu-id="f1d05-188">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="f1d05-189">新增頁面模型</span><span class="sxs-lookup"><span data-stu-id="f1d05-189">Add the page model</span></span>

<span data-ttu-id="f1d05-190">將頁面模型 (*Index.cshtml.cs*) 新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="f1d05-190">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="f1d05-191">頁面模型 (*Index.cshtml.cs* 中的 `IndexModel`) 會繫結 `FileUpload` 類別：</span><span class="sxs-lookup"><span data-stu-id="f1d05-191">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1d05-192">模型也會使用排程的清單 (`IList<Schedule>`) 來顯示儲存在頁面資料庫中的排程：</span><span class="sxs-lookup"><span data-stu-id="f1d05-192">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="f1d05-193">當頁面載入 `OnGetAsync` 時，會從資料庫填入 `Schedules`，並用其來產生已載入之排程的 HTML 資料表：</span><span class="sxs-lookup"><span data-stu-id="f1d05-193">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="f1d05-194">當表單張貼至伺服器時，系統即會檢查 `ModelState`。</span><span class="sxs-lookup"><span data-stu-id="f1d05-194">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="f1d05-195">如果無效，就會重建 `Schedule`，且頁面會顯示一或多則驗證訊息，指出頁面驗證失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="f1d05-195">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="f1d05-196">如果有效，即會在 *OnPostAsync* 中使用 `FileUpload` 屬性，以完成上傳這兩個排程版本的檔案，並建立新的 `Schedule` 物件來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="f1d05-196">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="f1d05-197">接著，系統就會將排程儲存到資料庫中：</span><span class="sxs-lookup"><span data-stu-id="f1d05-197">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="f1d05-198">連結檔案上傳 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="f1d05-198">Link the file upload Razor Page</span></span>

<span data-ttu-id="f1d05-199">開啟 *Pages/Shared/_Layout.cshtml*，然後將連結新增至導覽列以連至排程頁面：</span><span class="sxs-lookup"><span data-stu-id="f1d05-199">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="f1d05-200">新增用來確認刪除排程的頁面</span><span class="sxs-lookup"><span data-stu-id="f1d05-200">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="f1d05-201">當使用者按一下 [刪除排程] 時，系統會提供取消作業的機會。</span><span class="sxs-lookup"><span data-stu-id="f1d05-201">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="f1d05-202">將刪除確認頁面 (*Delete.cshtml*) 新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="f1d05-202">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="f1d05-203">頁面模型 (*Delete.cshtml.cs*) 會將 `id` 所識別的單一排程載入要求的路由資料中。</span><span class="sxs-lookup"><span data-stu-id="f1d05-203">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="f1d05-204">將 *Delete.cshtml.cs* 檔案新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="f1d05-204">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="f1d05-205">`OnPostAsync` 方法會依據排程的 `id` 來處理其刪除作業：</span><span class="sxs-lookup"><span data-stu-id="f1d05-205">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="f1d05-206">成功刪除排程之後，`RedirectToPage` 會將使用者送回排程的 *Index.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="f1d05-206">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="f1d05-207">運作中的排程 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="f1d05-207">The working Schedules Razor Page</span></span>

<span data-ttu-id="f1d05-208">載入頁面時，排程標題的標籤和輸入、公用排程和私用排程都會顯示提交按鈕：</span><span class="sxs-lookup"><span data-stu-id="f1d05-208">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![初始載入的排程 Razor 頁面，其中不含任何驗證錯誤和空白欄位](upload-files/_static/browser1.png)

<span data-ttu-id="f1d05-210">如果任何欄位未填妥就選取 [上傳] 按鈕，即會違反模型上的 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="f1d05-210">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="f1d05-211">`ModelState` 無效。</span><span class="sxs-lookup"><span data-stu-id="f1d05-211">The `ModelState` is invalid.</span></span> <span data-ttu-id="f1d05-212">使用者會看到驗證錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f1d05-212">The validation error messages are displayed to the user:</span></span>

![每個輸入控制項旁會出現驗證錯誤訊息](upload-files/_static/browser2.png)

<span data-ttu-id="f1d05-214">在 [標題] 欄位中鍵入兩個字母。</span><span class="sxs-lookup"><span data-stu-id="f1d05-214">Type two letters into the **Title** field.</span></span> <span data-ttu-id="f1d05-215">驗證訊息會變更為指出標題必須介於 3-60 個字元：</span><span class="sxs-lookup"><span data-stu-id="f1d05-215">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![標題驗證訊息已變更](upload-files/_static/browser3.png)

<span data-ttu-id="f1d05-217">當上傳一或多個排程時，[Loaded Schedules] (載入排程) 區段會顯示載入的排程：</span><span class="sxs-lookup"><span data-stu-id="f1d05-217">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![載入的排程表，其中顯示每個排程的標題、上傳日期 (UTC)、公用版本的檔案大小和私用版本的檔案大小](upload-files/_static/browser4.png)

<span data-ttu-id="f1d05-219">當使用者按一下這裡的 [刪除] 連結時，可前往刪除確認檢視，以選擇確認或取消刪除作業。</span><span class="sxs-lookup"><span data-stu-id="f1d05-219">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f1d05-220">疑難排解</span><span class="sxs-lookup"><span data-stu-id="f1d05-220">Troubleshooting</span></span>

<span data-ttu-id="f1d05-221">如需疑難排解資訊`IFormFile`上傳，請參閱[ASP.NET Core 的檔案上傳： 疑難排解](xref:mvc/models/file-uploads#troubleshooting)。</span><span class="sxs-lookup"><span data-stu-id="f1d05-221">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
