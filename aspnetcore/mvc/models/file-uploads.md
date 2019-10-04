---
title: 上傳 ASP.NET Core 中的檔案
author: guardrex
description: 如何使用模型繫結和資料流在 ASP.NET Core MVC 上傳檔案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/30/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 46cdb517742431a7f2a93155564832d586233e68
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925109"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="516c8-103">上傳 ASP.NET Core 中的檔案</span><span class="sxs-lookup"><span data-stu-id="516c8-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="516c8-104">作者： [Luke Latham](https://github.com/guardrex)、 [Steve Smith](https://ardalis.com/)和[Rutger 風暴](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="516c8-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

<span data-ttu-id="516c8-105">ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。</span><span class="sxs-lookup"><span data-stu-id="516c8-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="516c8-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="516c8-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="516c8-107">安全性考量</span><span class="sxs-lookup"><span data-stu-id="516c8-107">Security considerations</span></span>

<span data-ttu-id="516c8-108">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="516c8-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="516c8-109">攻擊者可能會嘗試：</span><span class="sxs-lookup"><span data-stu-id="516c8-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="516c8-110">執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="516c8-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="516c8-111">上傳病毒或惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="516c8-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="516c8-112">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="516c8-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="516c8-113">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="516c8-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="516c8-114">將檔案上傳到系統上的專用檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="516c8-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="516c8-115">使用專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。</span><span class="sxs-lookup"><span data-stu-id="516c8-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="516c8-116">停用檔案上傳位置的執行許可權。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="516c8-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="516c8-117">絕對不要將上傳的檔案保存在與應用程式相同的目錄樹狀結構中。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="516c8-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="516c8-118">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="516c8-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="516c8-119">請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。 &dagger;若要在 UI 或記錄訊息中顯示不受信任的檔案名，請以 HTML 編碼值。</span><span class="sxs-lookup"><span data-stu-id="516c8-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="516c8-120">只允許一組特定的已核准副檔名。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="516c8-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="516c8-121">請檢查檔案格式簽章，以防止使用者上傳回報偽裝檔。 &dagger;例如，不允許使用者上傳副檔名為 *.txt*的 *.exe*檔案。</span><span class="sxs-lookup"><span data-stu-id="516c8-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="516c8-122">確認用戶端檢查也會在伺服器上執行。 &dagger;用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="516c8-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="516c8-123">請檢查上傳檔案的大小，並防止上傳大於預期的上傳。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="516c8-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="516c8-124">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="516c8-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="516c8-125">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="516c8-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="516c8-126">@no__t 0The 範例應用程式示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="516c8-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="516c8-127">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="516c8-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="516c8-128">完全取得系統的控制權。</span><span class="sxs-lookup"><span data-stu-id="516c8-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="516c8-129">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="516c8-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="516c8-130">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="516c8-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="516c8-131">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="516c8-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="516c8-132">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="516c8-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="516c8-133">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="516c8-133">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="516c8-134">@no__t 0Azure 安全性：在接受來自使用者 @ no__t-0 的檔案時，確保適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="516c8-134">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="516c8-135">如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。</span><span class="sxs-lookup"><span data-stu-id="516c8-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="516c8-136">儲存案例</span><span class="sxs-lookup"><span data-stu-id="516c8-136">Storage scenarios</span></span>

<span data-ttu-id="516c8-137">檔案的一般儲存選項包括：</span><span class="sxs-lookup"><span data-stu-id="516c8-137">Common storage options for files include:</span></span>

* <span data-ttu-id="516c8-138">資料庫</span><span class="sxs-lookup"><span data-stu-id="516c8-138">Database</span></span>

  * <span data-ttu-id="516c8-139">針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。</span><span class="sxs-lookup"><span data-stu-id="516c8-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="516c8-140">資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。</span><span class="sxs-lookup"><span data-stu-id="516c8-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="516c8-141">資料庫的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="516c8-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="516c8-142">實體存放裝置（檔案系統或網路共用）</span><span class="sxs-lookup"><span data-stu-id="516c8-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="516c8-143">針對大型檔案上傳：</span><span class="sxs-lookup"><span data-stu-id="516c8-143">For large file uploads:</span></span>
    * <span data-ttu-id="516c8-144">資料庫限制可能會限制上傳的大小。</span><span class="sxs-lookup"><span data-stu-id="516c8-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="516c8-145">實體儲存體通常比在資料庫中儲存的成本低。</span><span class="sxs-lookup"><span data-stu-id="516c8-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="516c8-146">實體儲存體的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="516c8-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="516c8-147">應用程式的進程必須具有儲存位置的讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="516c8-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="516c8-148">**絕對不要授與 execute 許可權。**</span><span class="sxs-lookup"><span data-stu-id="516c8-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="516c8-149">資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）</span><span class="sxs-lookup"><span data-stu-id="516c8-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="516c8-150">服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。</span><span class="sxs-lookup"><span data-stu-id="516c8-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="516c8-151">在大型儲存體基礎結構案例中，服務可能會降低成本。</span><span class="sxs-lookup"><span data-stu-id="516c8-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="516c8-152">如需詳細資訊，請參閱[快速入門：使用 .NET 在物件儲存體 @ no__t-0 中建立 blob。</span><span class="sxs-lookup"><span data-stu-id="516c8-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="516c8-153">本主題示範 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>，但 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> 可以在使用 <xref:System.IO.Stream> 時，用來將 <xref:System.IO.FileStream> 儲存至 blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="516c8-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="516c8-154">檔案上傳案例</span><span class="sxs-lookup"><span data-stu-id="516c8-154">File upload scenarios</span></span>

<span data-ttu-id="516c8-155">上傳檔案的兩個一般方法是緩衝和串流處理。</span><span class="sxs-lookup"><span data-stu-id="516c8-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="516c8-156">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="516c8-156">**Buffering**</span></span>

<span data-ttu-id="516c8-157">系統會將整個檔案讀入 <xref:Microsoft.AspNetCore.Http.IFormFile>，這是用來C#處理或儲存檔案的檔案標記法。</span><span class="sxs-lookup"><span data-stu-id="516c8-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="516c8-158">檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。</span><span class="sxs-lookup"><span data-stu-id="516c8-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="516c8-159">如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。</span><span class="sxs-lookup"><span data-stu-id="516c8-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="516c8-160">如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。</span><span class="sxs-lookup"><span data-stu-id="516c8-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="516c8-161">任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="516c8-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="516c8-162">此主題的下列各節會涵蓋緩衝處理小型檔案：</span><span class="sxs-lookup"><span data-stu-id="516c8-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="516c8-163">實體儲存體</span><span class="sxs-lookup"><span data-stu-id="516c8-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* <span data-ttu-id="516c8-164">[[資料庫備份]](#upload-small-files-with-buffered-model-binding-to-a-database)</span><span class="sxs-lookup"><span data-stu-id="516c8-164">[Database](#upload-small-files-with-buffered-model-binding-to-a-database)</span></span>

<span data-ttu-id="516c8-165">**資料流**</span><span class="sxs-lookup"><span data-stu-id="516c8-165">**Streaming**</span></span>

<span data-ttu-id="516c8-166">此檔案會從多部分要求接收，並由應用程式直接處理或儲存。</span><span class="sxs-lookup"><span data-stu-id="516c8-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="516c8-167">串流不會大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="516c8-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="516c8-168">串流處理可減少上傳檔案時記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="516c8-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="516c8-169">[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。</span><span class="sxs-lookup"><span data-stu-id="516c8-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="516c8-170">將具有緩衝模型系結的小型檔案上傳至實體儲存體</span><span class="sxs-lookup"><span data-stu-id="516c8-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="516c8-171">若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="516c8-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="516c8-172">下列範例示範如何使用 Razor Pages 表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：</span><span class="sxs-lookup"><span data-stu-id="516c8-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="516c8-173">下列範例類似于先前的範例，不同之處在于：</span><span class="sxs-lookup"><span data-stu-id="516c8-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="516c8-174">JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="516c8-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="516c8-175">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="516c8-175">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="516c8-176">若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="516c8-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="516c8-177">使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。</span><span class="sxs-lookup"><span data-stu-id="516c8-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="516c8-178">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="516c8-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="516c8-179">例如:</span><span class="sxs-lookup"><span data-stu-id="516c8-179">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="516c8-180">為了支援檔案上傳，HTML 表單必須指定 `multipart/form-data` 的編碼類型（`enctype`）。</span><span class="sxs-lookup"><span data-stu-id="516c8-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="516c8-181">若要支援上傳多個檔案的 @no__t 0 輸入元素，請在 `<input>` 元素上提供 `multiple` 屬性：</span><span class="sxs-lookup"><span data-stu-id="516c8-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="516c8-182">您可以使用 <xref:Microsoft.AspNetCore.Http.IFormFile>，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案。</span><span class="sxs-lookup"><span data-stu-id="516c8-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="516c8-183">範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="516c8-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="516c8-184">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 @no__t 0 屬性而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="516c8-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="516c8-185">@No__t-0 屬性只應用於顯示用途，而且只能用於 HTML 編碼值。</span><span class="sxs-lookup"><span data-stu-id="516c8-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="516c8-186">到目前為止所提供的範例並不考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="516c8-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="516c8-187">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="516c8-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="516c8-188">安全性考量</span><span class="sxs-lookup"><span data-stu-id="516c8-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="516c8-189">驗證</span><span class="sxs-lookup"><span data-stu-id="516c8-189">Validation</span></span>](#validation)

<span data-ttu-id="516c8-190">使用模型系結上傳檔案並 <xref:Microsoft.AspNetCore.Http.IFormFile> 時，動作方法可以接受：</span><span class="sxs-lookup"><span data-stu-id="516c8-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="516c8-191">單一 <xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="516c8-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="516c8-192">下列任何代表數個檔案的集合：</span><span class="sxs-lookup"><span data-stu-id="516c8-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="516c8-193">[清單](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="516c8-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="516c8-194">系結符合依名稱的表單檔案。</span><span class="sxs-lookup"><span data-stu-id="516c8-194">Binding matches form files by name.</span></span> <span data-ttu-id="516c8-195">例如，`<input type="file" name="formFile">` 中的 HTML `name` 值必須符合系結C#的參數/屬性（`FormFile`）。</span><span class="sxs-lookup"><span data-stu-id="516c8-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="516c8-196">如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。</span><span class="sxs-lookup"><span data-stu-id="516c8-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="516c8-197">下列範例：</span><span class="sxs-lookup"><span data-stu-id="516c8-197">The following example:</span></span>

* <span data-ttu-id="516c8-198">迴圈一或多個已上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="516c8-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="516c8-199">會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。</span><span class="sxs-lookup"><span data-stu-id="516c8-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="516c8-200">使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。</span><span class="sxs-lookup"><span data-stu-id="516c8-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="516c8-201">傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="516c8-201">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="516c8-202">使用 `Path.GetRandomFileName` 可產生不含路徑的檔案名。</span><span class="sxs-lookup"><span data-stu-id="516c8-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="516c8-203">在下列範例中，路徑是從設定取得：</span><span class="sxs-lookup"><span data-stu-id="516c8-203">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="516c8-204">傳遞給 <xref:System.IO.FileStream> 的路徑*必須*包含檔案名。</span><span class="sxs-lookup"><span data-stu-id="516c8-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="516c8-205">如果未提供檔案名，則會在執行時間擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="516c8-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="516c8-206">使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 技術所上傳的檔案會在處理之前，先緩衝到伺服器上的記憶體或磁片上。</span><span class="sxs-lookup"><span data-stu-id="516c8-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="516c8-207">在動作方法內，<xref:Microsoft.AspNetCore.Http.IFormFile> 內容可以 <xref:System.IO.Stream> 來存取。</span><span class="sxs-lookup"><span data-stu-id="516c8-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="516c8-208">除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。</span><span class="sxs-lookup"><span data-stu-id="516c8-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="516c8-209">如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。</span><span class="sxs-lookup"><span data-stu-id="516c8-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="516c8-210">如果建立超過65535個檔案，但未刪除先前的暫存檔案，則[GetTempFileName](xref:System.IO.Path.GetTempFileName*)會擲回 <xref:System.IO.IOException>。</span><span class="sxs-lookup"><span data-stu-id="516c8-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="516c8-211">65535檔案的限制是每一伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="516c8-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="516c8-212">如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：</span><span class="sxs-lookup"><span data-stu-id="516c8-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="516c8-213">GetTempFileNameA 函式</span><span class="sxs-lookup"><span data-stu-id="516c8-213">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="516c8-214">將具有緩衝模型系結的小型檔案上傳至資料庫</span><span class="sxs-lookup"><span data-stu-id="516c8-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="516c8-215">若要使用[Entity Framework](/ef/core/index)將二進位檔案資料儲存在資料庫中，請在實體上定義 @no__t 1 陣列屬性：</span><span class="sxs-lookup"><span data-stu-id="516c8-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="516c8-216">指定包含 <xref:Microsoft.AspNetCore.Http.IFormFile> 之類別的頁面模型屬性：</span><span class="sxs-lookup"><span data-stu-id="516c8-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="516c8-217"><xref:Microsoft.AspNetCore.Http.IFormFile> 可以直接當做動作方法參數或系結模型屬性使用。</span><span class="sxs-lookup"><span data-stu-id="516c8-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="516c8-218">先前的範例會使用系結模型屬性。</span><span class="sxs-lookup"><span data-stu-id="516c8-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="516c8-219">@No__t-0 用於 Razor Pages 格式：</span><span class="sxs-lookup"><span data-stu-id="516c8-219">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="516c8-220">當表單張貼至伺服器時，將 <xref:Microsoft.AspNetCore.Http.IFormFile> 複製到資料流程，並將它儲存為資料庫中的位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="516c8-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="516c8-221">在下列範例中，`_dbContext` 會儲存應用程式的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="516c8-221">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="516c8-222">前述範例類似于範例應用程式中所示範的案例：</span><span class="sxs-lookup"><span data-stu-id="516c8-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="516c8-223">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="516c8-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="516c8-224">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="516c8-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="516c8-225">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="516c8-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="516c8-226">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 @no__t 0 屬性而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="516c8-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="516c8-227">@No__t-0 屬性只能用於顯示用途，而且只能用於 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="516c8-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="516c8-228">提供的範例不會考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="516c8-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="516c8-229">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="516c8-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="516c8-230">安全性考量</span><span class="sxs-lookup"><span data-stu-id="516c8-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="516c8-231">驗證</span><span class="sxs-lookup"><span data-stu-id="516c8-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="516c8-232">使用串流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="516c8-232">Upload large files with streaming</span></span>

<span data-ttu-id="516c8-233">下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="516c8-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="516c8-234">檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="516c8-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="516c8-235">由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。</span><span class="sxs-lookup"><span data-stu-id="516c8-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="516c8-236">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="516c8-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="516c8-237">讀取多部分區段之後，動作會執行自己的模型系結。</span><span class="sxs-lookup"><span data-stu-id="516c8-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="516c8-238">初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="516c8-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="516c8-239">屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：</span><span class="sxs-lookup"><span data-stu-id="516c8-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="516c8-240">@No__t-0 是用來停用模型系結：</span><span class="sxs-lookup"><span data-stu-id="516c8-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="516c8-241">在範例應用程式中，`GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會以篩選準則套用至 `/StreamedSingleFileUploadDb` 的頁面應用程式模型，並使用[@no__t 慣例](xref:razor-pages/razor-pages-conventions)在 Razor Pages-4 中 `/StreamedSingleFileUploadPhysical`：</span><span class="sxs-lookup"><span data-stu-id="516c8-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="516c8-242">由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。</span><span class="sxs-lookup"><span data-stu-id="516c8-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="516c8-243">動作方法會直接與 `Request` 屬性搭配運作。</span><span class="sxs-lookup"><span data-stu-id="516c8-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="516c8-244">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="516c8-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="516c8-245">索引鍵/值資料會儲存在 `KeyValueAccumulator` 中。</span><span class="sxs-lookup"><span data-stu-id="516c8-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="516c8-246">讀取多部分區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料系結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="516c8-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="516c8-247">使用 EF Core 串流至資料庫的完整 `StreamingController.UploadDatabase` 方法：</span><span class="sxs-lookup"><span data-stu-id="516c8-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="516c8-248">串流至實體位置的完整 `StreamingController.UploadPhysical` 方法：</span><span class="sxs-lookup"><span data-stu-id="516c8-248">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="516c8-249">在範例應用程式中，驗證檢查是由 `FileHelpers.ProcessStreamedFile` 來處理。</span><span class="sxs-lookup"><span data-stu-id="516c8-249">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="516c8-250">驗證</span><span class="sxs-lookup"><span data-stu-id="516c8-250">Validation</span></span>

<span data-ttu-id="516c8-251">範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理的 @no__t 1 和串流檔案上傳的檢查。</span><span class="sxs-lookup"><span data-stu-id="516c8-251">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="516c8-252">若要處理範例應用程式中 @no__t 0 的緩衝檔案上傳，請參閱*公用程式/FileHelpers*檔案中的 `ProcessFormFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="516c8-252">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="516c8-253">如需處理資料流程檔案，請參閱相同檔案中的 `ProcessStreamedFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="516c8-253">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="516c8-254">範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="516c8-254">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="516c8-255">在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。</span><span class="sxs-lookup"><span data-stu-id="516c8-255">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="516c8-256">雖然主題範例提供驗證技術的實用範例，但請不要在生產應用程式中執行 @no__t 0 類別，除非您：</span><span class="sxs-lookup"><span data-stu-id="516c8-256">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="516c8-257">完全瞭解此實作為。</span><span class="sxs-lookup"><span data-stu-id="516c8-257">Fully understand the implementation.</span></span>
> * <span data-ttu-id="516c8-258">針對應用程式的環境和規格修改適當的執行。</span><span class="sxs-lookup"><span data-stu-id="516c8-258">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="516c8-259">**絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**</span><span class="sxs-lookup"><span data-stu-id="516c8-259">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="516c8-260">內容驗證</span><span class="sxs-lookup"><span data-stu-id="516c8-260">Content validation</span></span>

<span data-ttu-id="516c8-261">**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="516c8-261">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="516c8-262">在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。</span><span class="sxs-lookup"><span data-stu-id="516c8-262">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="516c8-263">如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。</span><span class="sxs-lookup"><span data-stu-id="516c8-263">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="516c8-264">一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。</span><span class="sxs-lookup"><span data-stu-id="516c8-264">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="516c8-265">當檔案通過時，檔案就會移至一般檔案儲存位置。</span><span class="sxs-lookup"><span data-stu-id="516c8-265">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="516c8-266">這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。</span><span class="sxs-lookup"><span data-stu-id="516c8-266">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="516c8-267">藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。</span><span class="sxs-lookup"><span data-stu-id="516c8-267">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="516c8-268">副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="516c8-268">File extension validation</span></span>

<span data-ttu-id="516c8-269">已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。</span><span class="sxs-lookup"><span data-stu-id="516c8-269">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="516c8-270">例如:</span><span class="sxs-lookup"><span data-stu-id="516c8-270">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="516c8-271">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="516c8-271">File signature validation</span></span>

<span data-ttu-id="516c8-272">檔案的簽章取決於檔案開頭的前幾個位元組。</span><span class="sxs-lookup"><span data-stu-id="516c8-272">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="516c8-273">這些位元組可用來指出延伸模組是否符合檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="516c8-273">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="516c8-274">範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="516c8-274">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="516c8-275">在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：</span><span class="sxs-lookup"><span data-stu-id="516c8-275">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="516c8-276">若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。</span><span class="sxs-lookup"><span data-stu-id="516c8-276">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="516c8-277">檔案名安全性</span><span class="sxs-lookup"><span data-stu-id="516c8-277">File name security</span></span>

<span data-ttu-id="516c8-278">絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。</span><span class="sxs-lookup"><span data-stu-id="516c8-278">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="516c8-279">使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。</span><span class="sxs-lookup"><span data-stu-id="516c8-279">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="516c8-280">Razor 會自動對屬性值進行 HTML 編碼以供顯示。</span><span class="sxs-lookup"><span data-stu-id="516c8-280">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="516c8-281">以下是可安全使用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="516c8-281">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="516c8-282">在 Razor 以外，一律會從使用者的要求 @no__t 0 的檔案名內容。</span><span class="sxs-lookup"><span data-stu-id="516c8-282">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="516c8-283">許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。</span><span class="sxs-lookup"><span data-stu-id="516c8-283">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="516c8-284">提供其他邏輯以符合您應用程式的規格。</span><span class="sxs-lookup"><span data-stu-id="516c8-284">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="516c8-285">大小驗證</span><span class="sxs-lookup"><span data-stu-id="516c8-285">Size validation</span></span>

<span data-ttu-id="516c8-286">限制上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="516c8-286">Limit the size of uploaded files.</span></span>

<span data-ttu-id="516c8-287">在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。</span><span class="sxs-lookup"><span data-stu-id="516c8-287">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="516c8-288">此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的設定來提供：</span><span class="sxs-lookup"><span data-stu-id="516c8-288">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="516c8-289">@No__t-0 會插入 @no__t 1 類別中：</span><span class="sxs-lookup"><span data-stu-id="516c8-289">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="516c8-290">當檔案大小超過限制時，就會拒絕此檔案：</span><span class="sxs-lookup"><span data-stu-id="516c8-290">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="516c8-291">Match name 屬性值與 POST 方法的參數名稱</span><span class="sxs-lookup"><span data-stu-id="516c8-291">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="516c8-292">在張貼表單資料或直接使用 JavaScript `FormData` 的非 Razor 表單中，在表單的元素或 `FormData` 中指定的名稱必須符合控制器動作中參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="516c8-292">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="516c8-293">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="516c8-293">In the following example:</span></span>

* <span data-ttu-id="516c8-294">使用 `<input>` 元素時，`name` 屬性會設定為值 `battlePlans`：</span><span class="sxs-lookup"><span data-stu-id="516c8-294">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="516c8-295">在 JavaScript 中使用 `FormData` 時，此名稱會設定為 `battlePlans` 的值：</span><span class="sxs-lookup"><span data-stu-id="516c8-295">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="516c8-296">針對C#方法的參數使用相符的名稱（`battlePlans`）：</span><span class="sxs-lookup"><span data-stu-id="516c8-296">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="516c8-297">針對名為 `Upload` 的 Razor Pages 頁面處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="516c8-297">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="516c8-298">針對 MVC POST 控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="516c8-298">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="516c8-299">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="516c8-299">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="516c8-300">多部分主體長度限制</span><span class="sxs-lookup"><span data-stu-id="516c8-300">Multipart body length limit</span></span>

<span data-ttu-id="516c8-301"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 會設定每個多部分主體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="516c8-301"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="516c8-302">超過此限制的表單區段會在剖析時擲回 <xref:System.IO.InvalidDataException>。</span><span class="sxs-lookup"><span data-stu-id="516c8-302">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="516c8-303">預設值為134217728（128 MB）。</span><span class="sxs-lookup"><span data-stu-id="516c8-303">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="516c8-304">使用 `Startup.ConfigureServices` 中的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定來自訂限制：</span><span class="sxs-lookup"><span data-stu-id="516c8-304">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="516c8-305"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> 用來設定單一頁面或動作的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>。</span><span class="sxs-lookup"><span data-stu-id="516c8-305"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="516c8-306">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices` 中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="516c8-306">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions
                .AddPageApplicationModelConvention("/FileUploadPage",
                    model.Filters.Add(
                        new RequestFormLimitsAttribute()
                        {
                            // Set the limit to 256 MB
                            MultipartBodyLengthLimit = 268435456
                        });
        })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="516c8-307">在 Razor Pages 應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：</span><span class="sxs-lookup"><span data-stu-id="516c8-307">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="516c8-308">Kestrel 要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="516c8-308">Kestrel maximum request body size</span></span>

<span data-ttu-id="516c8-309">針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="516c8-309">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="516c8-310">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：</span><span class="sxs-lookup"><span data-stu-id="516c8-310">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        })
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

::: moniker-end

<span data-ttu-id="516c8-311"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 是用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。</span><span class="sxs-lookup"><span data-stu-id="516c8-311"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="516c8-312">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices` 中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="516c8-312">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions
                .AddPageApplicationModelConvention("/FileUploadPage",
                    model =>
                    {
                        // Handle requests up to 50 MB
                        model.Filters.Add(
                            new RequestSizeLimitAttribute(52428800));
                    });
        })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="516c8-313">在 Razor pages 應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="516c8-313">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="516c8-314">其他 Kestrel 限制</span><span class="sxs-lookup"><span data-stu-id="516c8-314">Other Kestrel limits</span></span>

<span data-ttu-id="516c8-315">其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="516c8-315">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="516c8-316">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="516c8-316">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="516c8-317">要求和回應資料速率</span><span class="sxs-lookup"><span data-stu-id="516c8-317">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="516c8-318">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="516c8-318">IIS content length limit</span></span>

<span data-ttu-id="516c8-319">預設要求限制（`maxAllowedContentLength`）是30000000個位元組，大約是 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="516c8-319">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="516c8-320">自訂*web.config*檔案中的限制：</span><span class="sxs-lookup"><span data-stu-id="516c8-320">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="516c8-321">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="516c8-321">This setting only applies to IIS.</span></span> <span data-ttu-id="516c8-322">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="516c8-322">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="516c8-323">如需詳細資訊，請參閱[\<requestLimits > 的要求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="516c8-323">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="516c8-324">ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。</span><span class="sxs-lookup"><span data-stu-id="516c8-324">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="516c8-325">如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（aspnet/AspNetCore #2711）](https://github.com/aspnet/AspNetCore/issues/2711)。</span><span class="sxs-lookup"><span data-stu-id="516c8-325">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="516c8-326">疑難排解</span><span class="sxs-lookup"><span data-stu-id="516c8-326">Troubleshoot</span></span>

<span data-ttu-id="516c8-327">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="516c8-327">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="516c8-328">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="516c8-328">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="516c8-329">下列錯誤表示上傳的檔案超過伺服器設定的內容長度：</span><span class="sxs-lookup"><span data-stu-id="516c8-329">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="516c8-330">如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。</span><span class="sxs-lookup"><span data-stu-id="516c8-330">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="516c8-331">連接失敗</span><span class="sxs-lookup"><span data-stu-id="516c8-331">Connection failure</span></span>

<span data-ttu-id="516c8-332">連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。</span><span class="sxs-lookup"><span data-stu-id="516c8-332">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="516c8-333">如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。</span><span class="sxs-lookup"><span data-stu-id="516c8-333">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="516c8-334">Kestrel 用戶端連接限制也可能需要調整。</span><span class="sxs-lookup"><span data-stu-id="516c8-334">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="516c8-335">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="516c8-335">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="516c8-336">如果控制器接受使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 的上傳檔案，但值為 `null`，請確認 HTML 表單指定的 `enctype` 值為 `multipart/form-data`。</span><span class="sxs-lookup"><span data-stu-id="516c8-336">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="516c8-337">如果未在 `<form>` 元素上設定此屬性，則不會進行檔案上傳，而且任何系結的 @no__t 1 引數都會 `null`。</span><span class="sxs-lookup"><span data-stu-id="516c8-337">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="516c8-338">此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="516c8-338">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="516c8-339">其他資源</span><span class="sxs-lookup"><span data-stu-id="516c8-339">Additional resources</span></span>

* [<span data-ttu-id="516c8-340">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="516c8-340">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* <span data-ttu-id="516c8-341">@no__t 0Azure 安全性：安全性框架：輸入驗證 |緩和措施 @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="516c8-341">[Azure Security: Security Frame: Input Validation | Mitigations](/azure/security/azure-security-threat-modeling-tool-input-validation)</span></span>
* <span data-ttu-id="516c8-342">@no__t 0Azure 雲端設計模式：Valet 索引鍵模式 @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="516c8-342">[Azure Cloud Design Patterns: Valet Key pattern](/azure/architecture/patterns/valet-key)</span></span>
