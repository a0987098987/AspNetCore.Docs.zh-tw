---
title: 上傳 ASP.NET Core 中的檔案
author: guardrex
description: 如何使用模型繫結和資料流在 ASP.NET Core MVC 上傳檔案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: de8bfee22e39dfc5a6ed254cf0555887891d4590
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179311"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="5870d-103">上傳 ASP.NET Core 中的檔案</span><span class="sxs-lookup"><span data-stu-id="5870d-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="5870d-104">作者： [Luke Latham](https://github.com/guardrex)、 [Steve Smith](https://ardalis.com/)和[Rutger 風暴](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="5870d-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5870d-105">ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。</span><span class="sxs-lookup"><span data-stu-id="5870d-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="5870d-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5870d-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="5870d-107">安全性考量</span><span class="sxs-lookup"><span data-stu-id="5870d-107">Security considerations</span></span>

<span data-ttu-id="5870d-108">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="5870d-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="5870d-109">攻擊者可能會嘗試：</span><span class="sxs-lookup"><span data-stu-id="5870d-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="5870d-110">執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="5870d-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="5870d-111">上傳病毒或惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="5870d-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="5870d-112">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="5870d-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="5870d-113">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="5870d-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="5870d-114">將檔案上傳到系統上的專用檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="5870d-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="5870d-115">使用專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。</span><span class="sxs-lookup"><span data-stu-id="5870d-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="5870d-116">停用檔案上傳位置的執行許可權。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="5870d-117">絕對不要將上傳的檔案保存在與應用程式相同的目錄樹狀結構中。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="5870d-118">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="5870d-119">請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。 &dagger;若要在 UI 或記錄訊息中顯示不受信任的檔案名，請以 HTML 編碼值。</span><span class="sxs-lookup"><span data-stu-id="5870d-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="5870d-120">只允許一組特定的已核准副檔名。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="5870d-121">請檢查檔案格式簽章，以防止使用者上傳回報偽裝檔。 &dagger;例如，不允許使用者上傳副檔名為 *.txt*的 *.exe*檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="5870d-122">確認用戶端檢查也會在伺服器上執行。 &dagger;用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="5870d-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="5870d-123">請檢查上傳檔案的大小，並防止上傳大於預期的上傳。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="5870d-124">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="5870d-125">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="5870d-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="5870d-126">@no__t 0The 範例應用程式示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="5870d-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-127">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="5870d-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="5870d-128">完全取得系統的控制權。</span><span class="sxs-lookup"><span data-stu-id="5870d-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="5870d-129">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="5870d-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="5870d-130">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="5870d-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="5870d-131">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="5870d-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="5870d-132">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="5870d-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="5870d-133">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="5870d-133">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="5870d-134">@no__t 0Azure 安全性：在接受來自使用者 @ no__t-0 的檔案時，確保適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="5870d-134">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="5870d-135">如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="5870d-136">儲存案例</span><span class="sxs-lookup"><span data-stu-id="5870d-136">Storage scenarios</span></span>

<span data-ttu-id="5870d-137">檔案的一般儲存選項包括：</span><span class="sxs-lookup"><span data-stu-id="5870d-137">Common storage options for files include:</span></span>

* <span data-ttu-id="5870d-138">資料庫</span><span class="sxs-lookup"><span data-stu-id="5870d-138">Database</span></span>

  * <span data-ttu-id="5870d-139">針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。</span><span class="sxs-lookup"><span data-stu-id="5870d-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="5870d-140">資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。</span><span class="sxs-lookup"><span data-stu-id="5870d-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="5870d-141">資料庫的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="5870d-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="5870d-142">實體存放裝置（檔案系統或網路共用）</span><span class="sxs-lookup"><span data-stu-id="5870d-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="5870d-143">針對大型檔案上傳：</span><span class="sxs-lookup"><span data-stu-id="5870d-143">For large file uploads:</span></span>
    * <span data-ttu-id="5870d-144">資料庫限制可能會限制上傳的大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="5870d-145">實體儲存體通常比在資料庫中儲存的成本低。</span><span class="sxs-lookup"><span data-stu-id="5870d-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="5870d-146">實體儲存體的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="5870d-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="5870d-147">應用程式的進程必須具有儲存位置的讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="5870d-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="5870d-148">**絕對不要授與 execute 許可權。**</span><span class="sxs-lookup"><span data-stu-id="5870d-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="5870d-149">資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）</span><span class="sxs-lookup"><span data-stu-id="5870d-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="5870d-150">服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。</span><span class="sxs-lookup"><span data-stu-id="5870d-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="5870d-151">在大型儲存體基礎結構案例中，服務可能會降低成本。</span><span class="sxs-lookup"><span data-stu-id="5870d-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="5870d-152">如需詳細資訊，請參閱[快速入門：使用 .NET 在物件儲存體 @ no__t-0 中建立 blob。</span><span class="sxs-lookup"><span data-stu-id="5870d-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="5870d-153">本主題示範 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>，但 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> 可以在使用 <xref:System.IO.Stream> 時，用來將 <xref:System.IO.FileStream> 儲存至 blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="5870d-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="5870d-154">檔案上傳案例</span><span class="sxs-lookup"><span data-stu-id="5870d-154">File upload scenarios</span></span>

<span data-ttu-id="5870d-155">上傳檔案的兩個一般方法是緩衝和串流處理。</span><span class="sxs-lookup"><span data-stu-id="5870d-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="5870d-156">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="5870d-156">**Buffering**</span></span>

<span data-ttu-id="5870d-157">系統會將整個檔案讀入 <xref:Microsoft.AspNetCore.Http.IFormFile>，這是用來C#處理或儲存檔案的檔案標記法。</span><span class="sxs-lookup"><span data-stu-id="5870d-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="5870d-158">檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="5870d-159">如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。</span><span class="sxs-lookup"><span data-stu-id="5870d-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="5870d-160">如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。</span><span class="sxs-lookup"><span data-stu-id="5870d-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="5870d-161">任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="5870d-162">此主題的下列各節會涵蓋緩衝處理小型檔案：</span><span class="sxs-lookup"><span data-stu-id="5870d-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="5870d-163">實體儲存體</span><span class="sxs-lookup"><span data-stu-id="5870d-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* <span data-ttu-id="5870d-164">[[資料庫備份]](#upload-small-files-with-buffered-model-binding-to-a-database)</span><span class="sxs-lookup"><span data-stu-id="5870d-164">[Database](#upload-small-files-with-buffered-model-binding-to-a-database)</span></span>

<span data-ttu-id="5870d-165">**資料流**</span><span class="sxs-lookup"><span data-stu-id="5870d-165">**Streaming**</span></span>

<span data-ttu-id="5870d-166">此檔案會從多部分要求接收，並由應用程式直接處理或儲存。</span><span class="sxs-lookup"><span data-stu-id="5870d-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="5870d-167">串流不會大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="5870d-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="5870d-168">串流處理可減少上傳檔案時記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="5870d-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="5870d-169">[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="5870d-170">將具有緩衝模型系結的小型檔案上傳至實體儲存體</span><span class="sxs-lookup"><span data-stu-id="5870d-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="5870d-171">若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="5870d-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="5870d-172">下列範例示範如何使用 Razor Pages 表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：</span><span class="sxs-lookup"><span data-stu-id="5870d-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="5870d-173">下列範例類似于先前的範例，不同之處在于：</span><span class="sxs-lookup"><span data-stu-id="5870d-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="5870d-174">JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="5870d-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="5870d-175">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="5870d-175">There's no validation.</span></span>

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

<span data-ttu-id="5870d-176">若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="5870d-177">使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。</span><span class="sxs-lookup"><span data-stu-id="5870d-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="5870d-178">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="5870d-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="5870d-179">例如:</span><span class="sxs-lookup"><span data-stu-id="5870d-179">For example:</span></span>

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

<span data-ttu-id="5870d-180">為了支援檔案上傳，HTML 表單必須指定 `multipart/form-data` 的編碼類型（`enctype`）。</span><span class="sxs-lookup"><span data-stu-id="5870d-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="5870d-181">若要支援上傳多個檔案的 @no__t 0 輸入元素，請在 `<input>` 元素上提供 `multiple` 屬性：</span><span class="sxs-lookup"><span data-stu-id="5870d-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="5870d-182">您可以使用 <xref:Microsoft.AspNetCore.Http.IFormFile>，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="5870d-183">範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="5870d-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-184">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 @no__t 0 屬性而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="5870d-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="5870d-185">@No__t-0 屬性只應用於顯示用途，而且只能用於 HTML 編碼值。</span><span class="sxs-lookup"><span data-stu-id="5870d-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="5870d-186">到目前為止所提供的範例並不考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="5870d-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="5870d-187">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="5870d-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="5870d-188">安全性考量</span><span class="sxs-lookup"><span data-stu-id="5870d-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="5870d-189">驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-189">Validation</span></span>](#validation)

<span data-ttu-id="5870d-190">使用模型系結上傳檔案並 <xref:Microsoft.AspNetCore.Http.IFormFile> 時，動作方法可以接受：</span><span class="sxs-lookup"><span data-stu-id="5870d-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="5870d-191">單一 <xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="5870d-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="5870d-192">下列任何代表數個檔案的集合：</span><span class="sxs-lookup"><span data-stu-id="5870d-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="5870d-193">[清單](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="5870d-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="5870d-194">系結符合依名稱的表單檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-194">Binding matches form files by name.</span></span> <span data-ttu-id="5870d-195">例如，`<input type="file" name="formFile">` 中的 HTML `name` 值必須符合系結C#的參數/屬性（`FormFile`）。</span><span class="sxs-lookup"><span data-stu-id="5870d-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="5870d-196">如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="5870d-197">下列範例：</span><span class="sxs-lookup"><span data-stu-id="5870d-197">The following example:</span></span>

* <span data-ttu-id="5870d-198">迴圈一或多個已上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="5870d-199">會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="5870d-200">使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。</span><span class="sxs-lookup"><span data-stu-id="5870d-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="5870d-201">傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-201">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="5870d-202">使用 `Path.GetRandomFileName` 可產生不含路徑的檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="5870d-203">在下列範例中，路徑是從設定取得：</span><span class="sxs-lookup"><span data-stu-id="5870d-203">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="5870d-204">傳遞給 <xref:System.IO.FileStream> 的路徑*必須*包含檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="5870d-205">如果未提供檔案名，則會在執行時間擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="5870d-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="5870d-206">使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 技術所上傳的檔案會在處理之前，先緩衝到伺服器上的記憶體或磁片上。</span><span class="sxs-lookup"><span data-stu-id="5870d-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="5870d-207">在動作方法內，<xref:Microsoft.AspNetCore.Http.IFormFile> 內容可以 <xref:System.IO.Stream> 來存取。</span><span class="sxs-lookup"><span data-stu-id="5870d-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="5870d-208">除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。</span><span class="sxs-lookup"><span data-stu-id="5870d-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="5870d-209">如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。</span><span class="sxs-lookup"><span data-stu-id="5870d-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-210">如果建立超過65535個檔案，但未刪除先前的暫存檔案，則[GetTempFileName](xref:System.IO.Path.GetTempFileName*)會擲回 <xref:System.IO.IOException>。</span><span class="sxs-lookup"><span data-stu-id="5870d-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="5870d-211">65535檔案的限制是每一伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="5870d-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="5870d-212">如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：</span><span class="sxs-lookup"><span data-stu-id="5870d-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="5870d-213">GetTempFileNameA 函式</span><span class="sxs-lookup"><span data-stu-id="5870d-213">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="5870d-214">將具有緩衝模型系結的小型檔案上傳至資料庫</span><span class="sxs-lookup"><span data-stu-id="5870d-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="5870d-215">若要使用[Entity Framework](/ef/core/index)將二進位檔案資料儲存在資料庫中，請在實體上定義 @no__t 1 陣列屬性：</span><span class="sxs-lookup"><span data-stu-id="5870d-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="5870d-216">指定包含 <xref:Microsoft.AspNetCore.Http.IFormFile> 之類別的頁面模型屬性：</span><span class="sxs-lookup"><span data-stu-id="5870d-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="5870d-217"><xref:Microsoft.AspNetCore.Http.IFormFile> 可以直接當做動作方法參數或系結模型屬性使用。</span><span class="sxs-lookup"><span data-stu-id="5870d-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="5870d-218">先前的範例會使用系結模型屬性。</span><span class="sxs-lookup"><span data-stu-id="5870d-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="5870d-219">@No__t-0 用於 Razor Pages 格式：</span><span class="sxs-lookup"><span data-stu-id="5870d-219">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="5870d-220">當表單張貼至伺服器時，將 <xref:Microsoft.AspNetCore.Http.IFormFile> 複製到資料流程，並將它儲存為資料庫中的位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="5870d-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="5870d-221">在下列範例中，`_dbContext` 會儲存應用程式的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="5870d-221">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="5870d-222">前述範例類似于範例應用程式中所示範的案例：</span><span class="sxs-lookup"><span data-stu-id="5870d-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="5870d-223">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5870d-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="5870d-224">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5870d-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-225">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="5870d-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="5870d-226">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 @no__t 0 屬性而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="5870d-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="5870d-227">@No__t-0 屬性只能用於顯示用途，而且只能用於 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="5870d-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="5870d-228">提供的範例不會考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="5870d-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="5870d-229">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="5870d-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="5870d-230">安全性考量</span><span class="sxs-lookup"><span data-stu-id="5870d-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="5870d-231">驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="5870d-232">使用串流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="5870d-232">Upload large files with streaming</span></span>

<span data-ttu-id="5870d-233">下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="5870d-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="5870d-234">檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="5870d-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="5870d-235">由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。</span><span class="sxs-lookup"><span data-stu-id="5870d-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="5870d-236">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="5870d-237">讀取多部分區段之後，動作會執行自己的模型系結。</span><span class="sxs-lookup"><span data-stu-id="5870d-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="5870d-238">初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="5870d-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="5870d-239">屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：</span><span class="sxs-lookup"><span data-stu-id="5870d-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="5870d-240">@No__t-0 是用來停用模型系結：</span><span class="sxs-lookup"><span data-stu-id="5870d-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="5870d-241">在範例應用程式中，`GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會以篩選準則套用至 `/StreamedSingleFileUploadDb` 的頁面應用程式模型，並使用[@no__t 慣例](xref:razor-pages/razor-pages-conventions)在 Razor Pages-4 中 `/StreamedSingleFileUploadPhysical`：</span><span class="sxs-lookup"><span data-stu-id="5870d-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="5870d-242">由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。</span><span class="sxs-lookup"><span data-stu-id="5870d-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="5870d-243">動作方法會直接與 `Request` 屬性搭配運作。</span><span class="sxs-lookup"><span data-stu-id="5870d-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="5870d-244">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="5870d-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="5870d-245">索引鍵/值資料會儲存在 `KeyValueAccumulator` 中。</span><span class="sxs-lookup"><span data-stu-id="5870d-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="5870d-246">讀取多部分區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料系結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="5870d-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="5870d-247">使用 EF Core 串流至資料庫的完整 `StreamingController.UploadDatabase` 方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="5870d-248">`MultipartRequestHelper` （*公用程式/MultipartRequestHelper .cs*）：</span><span class="sxs-lookup"><span data-stu-id="5870d-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="5870d-249">串流至實體位置的完整 `StreamingController.UploadPhysical` 方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-249">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="5870d-250">在範例應用程式中，驗證檢查是由 `FileHelpers.ProcessStreamedFile` 來處理。</span><span class="sxs-lookup"><span data-stu-id="5870d-250">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="5870d-251">驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-251">Validation</span></span>

<span data-ttu-id="5870d-252">範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理的 @no__t 1 和串流檔案上傳的檢查。</span><span class="sxs-lookup"><span data-stu-id="5870d-252">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="5870d-253">若要處理範例應用程式中 @no__t 0 的緩衝檔案上傳，請參閱*公用程式/FileHelpers*檔案中的 `ProcessFormFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="5870d-253">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="5870d-254">如需處理資料流程檔案，請參閱相同檔案中的 `ProcessStreamedFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="5870d-254">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-255">範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-255">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="5870d-256">在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-256">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="5870d-257">雖然主題範例提供驗證技術的實用範例，但請不要在生產應用程式中執行 @no__t 0 類別，除非您：</span><span class="sxs-lookup"><span data-stu-id="5870d-257">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="5870d-258">完全瞭解此實作為。</span><span class="sxs-lookup"><span data-stu-id="5870d-258">Fully understand the implementation.</span></span>
> * <span data-ttu-id="5870d-259">針對應用程式的環境和規格修改適當的執行。</span><span class="sxs-lookup"><span data-stu-id="5870d-259">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="5870d-260">**絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**</span><span class="sxs-lookup"><span data-stu-id="5870d-260">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="5870d-261">內容驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-261">Content validation</span></span>

<span data-ttu-id="5870d-262">**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="5870d-262">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="5870d-263">在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。</span><span class="sxs-lookup"><span data-stu-id="5870d-263">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="5870d-264">如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。</span><span class="sxs-lookup"><span data-stu-id="5870d-264">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="5870d-265">一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。</span><span class="sxs-lookup"><span data-stu-id="5870d-265">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="5870d-266">當檔案通過時，檔案就會移至一般檔案儲存位置。</span><span class="sxs-lookup"><span data-stu-id="5870d-266">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="5870d-267">這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。</span><span class="sxs-lookup"><span data-stu-id="5870d-267">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="5870d-268">藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。</span><span class="sxs-lookup"><span data-stu-id="5870d-268">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="5870d-269">副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-269">File extension validation</span></span>

<span data-ttu-id="5870d-270">已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。</span><span class="sxs-lookup"><span data-stu-id="5870d-270">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="5870d-271">例如:</span><span class="sxs-lookup"><span data-stu-id="5870d-271">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="5870d-272">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-272">File signature validation</span></span>

<span data-ttu-id="5870d-273">檔案的簽章取決於檔案開頭的前幾個位元組。</span><span class="sxs-lookup"><span data-stu-id="5870d-273">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="5870d-274">這些位元組可用來指出延伸模組是否符合檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-274">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="5870d-275">範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="5870d-275">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="5870d-276">在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：</span><span class="sxs-lookup"><span data-stu-id="5870d-276">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="5870d-277">若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。</span><span class="sxs-lookup"><span data-stu-id="5870d-277">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="5870d-278">檔案名安全性</span><span class="sxs-lookup"><span data-stu-id="5870d-278">File name security</span></span>

<span data-ttu-id="5870d-279">絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。</span><span class="sxs-lookup"><span data-stu-id="5870d-279">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="5870d-280">使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。</span><span class="sxs-lookup"><span data-stu-id="5870d-280">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="5870d-281">Razor 會自動對屬性值進行 HTML 編碼以供顯示。</span><span class="sxs-lookup"><span data-stu-id="5870d-281">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="5870d-282">以下是可安全使用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="5870d-282">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="5870d-283">在 Razor 以外，一律會從使用者的要求 @no__t 0 的檔案名內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-283">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="5870d-284">許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。</span><span class="sxs-lookup"><span data-stu-id="5870d-284">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="5870d-285">提供其他邏輯以符合您應用程式的規格。</span><span class="sxs-lookup"><span data-stu-id="5870d-285">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="5870d-286">大小驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-286">Size validation</span></span>

<span data-ttu-id="5870d-287">限制上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-287">Limit the size of uploaded files.</span></span>

<span data-ttu-id="5870d-288">在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。</span><span class="sxs-lookup"><span data-stu-id="5870d-288">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="5870d-289">此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的設定來提供：</span><span class="sxs-lookup"><span data-stu-id="5870d-289">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="5870d-290">@No__t-0 會插入 @no__t 1 類別中：</span><span class="sxs-lookup"><span data-stu-id="5870d-290">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="5870d-291">當檔案大小超過限制時，就會拒絕此檔案：</span><span class="sxs-lookup"><span data-stu-id="5870d-291">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="5870d-292">Match name 屬性值與 POST 方法的參數名稱</span><span class="sxs-lookup"><span data-stu-id="5870d-292">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="5870d-293">在張貼表單資料或直接使用 JavaScript `FormData` 的非 Razor 表單中，在表單的元素或 `FormData` 中指定的名稱必須符合控制器動作中參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="5870d-293">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="5870d-294">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="5870d-294">In the following example:</span></span>

* <span data-ttu-id="5870d-295">使用 `<input>` 元素時，`name` 屬性會設定為值 `battlePlans`：</span><span class="sxs-lookup"><span data-stu-id="5870d-295">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="5870d-296">在 JavaScript 中使用 `FormData` 時，此名稱會設定為 `battlePlans` 的值：</span><span class="sxs-lookup"><span data-stu-id="5870d-296">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="5870d-297">針對C#方法的參數使用相符的名稱（`battlePlans`）：</span><span class="sxs-lookup"><span data-stu-id="5870d-297">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="5870d-298">針對名為 `Upload` 的 Razor Pages 頁面處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-298">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="5870d-299">針對 MVC POST 控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-299">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="5870d-300">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="5870d-300">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="5870d-301">多部分主體長度限制</span><span class="sxs-lookup"><span data-stu-id="5870d-301">Multipart body length limit</span></span>

<span data-ttu-id="5870d-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 會設定每個多部分主體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="5870d-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="5870d-303">超過此限制的表單區段會在剖析時擲回 <xref:System.IO.InvalidDataException>。</span><span class="sxs-lookup"><span data-stu-id="5870d-303">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="5870d-304">預設值為134217728（128 MB）。</span><span class="sxs-lookup"><span data-stu-id="5870d-304">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="5870d-305">使用 `Startup.ConfigureServices` 中的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定來自訂限制：</span><span class="sxs-lookup"><span data-stu-id="5870d-305">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="5870d-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> 用來設定單一頁面或動作的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>。</span><span class="sxs-lookup"><span data-stu-id="5870d-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="5870d-307">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices` 中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="5870d-307">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
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
    });
```

<span data-ttu-id="5870d-308">在 Razor Pages 應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-308">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="5870d-309">Kestrel 要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="5870d-309">Kestrel maximum request body size</span></span>

<span data-ttu-id="5870d-310">針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="5870d-310">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="5870d-311">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：</span><span class="sxs-lookup"><span data-stu-id="5870d-311">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="5870d-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 是用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。</span><span class="sxs-lookup"><span data-stu-id="5870d-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="5870d-313">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices` 中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="5870d-313">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
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
    });
```

<span data-ttu-id="5870d-314">在 Razor pages 應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-314">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="5870d-315">您也可以使用[@attribute](xref:mvc/views/razor#attribute) Razor 指示詞來套用 `RequestSizeLimitAttribute`：</span><span class="sxs-lookup"><span data-stu-id="5870d-315">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="5870d-316">其他 Kestrel 限制</span><span class="sxs-lookup"><span data-stu-id="5870d-316">Other Kestrel limits</span></span>

<span data-ttu-id="5870d-317">其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="5870d-317">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="5870d-318">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="5870d-318">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="5870d-319">要求和回應資料速率</span><span class="sxs-lookup"><span data-stu-id="5870d-319">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="5870d-320">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="5870d-320">IIS content length limit</span></span>

<span data-ttu-id="5870d-321">預設要求限制（`maxAllowedContentLength`）是30000000個位元組，大約是 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="5870d-321">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="5870d-322">自訂*web.config*檔案中的限制：</span><span class="sxs-lookup"><span data-stu-id="5870d-322">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="5870d-323">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="5870d-323">This setting only applies to IIS.</span></span> <span data-ttu-id="5870d-324">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="5870d-324">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="5870d-325">如需詳細資訊，請參閱[\<requestLimits > 的要求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="5870d-325">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="5870d-326">ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。</span><span class="sxs-lookup"><span data-stu-id="5870d-326">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="5870d-327">如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（aspnet/AspNetCore #2711）](https://github.com/aspnet/AspNetCore/issues/2711)。</span><span class="sxs-lookup"><span data-stu-id="5870d-327">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="5870d-328">疑難排解</span><span class="sxs-lookup"><span data-stu-id="5870d-328">Troubleshoot</span></span>

<span data-ttu-id="5870d-329">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="5870d-329">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="5870d-330">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="5870d-330">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="5870d-331">下列錯誤表示上傳的檔案超過伺服器設定的內容長度：</span><span class="sxs-lookup"><span data-stu-id="5870d-331">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="5870d-332">如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-332">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="5870d-333">連接失敗</span><span class="sxs-lookup"><span data-stu-id="5870d-333">Connection failure</span></span>

<span data-ttu-id="5870d-334">連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。</span><span class="sxs-lookup"><span data-stu-id="5870d-334">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="5870d-335">如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-335">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="5870d-336">Kestrel 用戶端連接限制也可能需要調整。</span><span class="sxs-lookup"><span data-stu-id="5870d-336">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="5870d-337">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="5870d-337">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="5870d-338">如果控制器接受使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 的上傳檔案，但值為 `null`，請確認 HTML 表單指定的 `enctype` 值為 `multipart/form-data`。</span><span class="sxs-lookup"><span data-stu-id="5870d-338">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="5870d-339">如果未在 `<form>` 元素上設定此屬性，則不會進行檔案上傳，而且任何系結的 @no__t 1 引數都會 `null`。</span><span class="sxs-lookup"><span data-stu-id="5870d-339">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="5870d-340">此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="5870d-340">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5870d-341">ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。</span><span class="sxs-lookup"><span data-stu-id="5870d-341">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="5870d-342">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5870d-342">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="5870d-343">安全性考量</span><span class="sxs-lookup"><span data-stu-id="5870d-343">Security considerations</span></span>

<span data-ttu-id="5870d-344">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="5870d-344">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="5870d-345">攻擊者可能會嘗試：</span><span class="sxs-lookup"><span data-stu-id="5870d-345">Attackers may attempt to:</span></span>

* <span data-ttu-id="5870d-346">執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="5870d-346">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="5870d-347">上傳病毒或惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="5870d-347">Upload viruses or malware.</span></span>
* <span data-ttu-id="5870d-348">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="5870d-348">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="5870d-349">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="5870d-349">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="5870d-350">將檔案上傳到系統上的專用檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="5870d-350">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="5870d-351">使用專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。</span><span class="sxs-lookup"><span data-stu-id="5870d-351">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="5870d-352">停用檔案上傳位置的執行許可權。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-352">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="5870d-353">絕對不要將上傳的檔案保存在與應用程式相同的目錄樹狀結構中。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-353">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="5870d-354">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-354">Use a safe file name determined by the app.</span></span> <span data-ttu-id="5870d-355">請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。 &dagger;若要在 UI 或記錄訊息中顯示不受信任的檔案名，請以 HTML 編碼值。</span><span class="sxs-lookup"><span data-stu-id="5870d-355">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="5870d-356">只允許一組特定的已核准副檔名。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-356">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="5870d-357">請檢查檔案格式簽章，以防止使用者上傳回報偽裝檔。 &dagger;例如，不允許使用者上傳副檔名為 *.txt*的 *.exe*檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-357">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="5870d-358">確認用戶端檢查也會在伺服器上執行。 &dagger;用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="5870d-358">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="5870d-359">請檢查上傳檔案的大小，並防止上傳大於預期的上傳。 &dagger;</span><span class="sxs-lookup"><span data-stu-id="5870d-359">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="5870d-360">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-360">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="5870d-361">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="5870d-361">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="5870d-362">@no__t 0The 範例應用程式示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="5870d-362">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-363">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="5870d-363">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="5870d-364">完全取得系統的控制權。</span><span class="sxs-lookup"><span data-stu-id="5870d-364">Completely gain control of a system.</span></span>
> * <span data-ttu-id="5870d-365">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="5870d-365">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="5870d-366">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="5870d-366">Compromise user or system data.</span></span>
> * <span data-ttu-id="5870d-367">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="5870d-367">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="5870d-368">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="5870d-368">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="5870d-369">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="5870d-369">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="5870d-370">@no__t 0Azure 安全性：在接受來自使用者 @ no__t-0 的檔案時，確保適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="5870d-370">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="5870d-371">如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-371">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="5870d-372">儲存案例</span><span class="sxs-lookup"><span data-stu-id="5870d-372">Storage scenarios</span></span>

<span data-ttu-id="5870d-373">檔案的一般儲存選項包括：</span><span class="sxs-lookup"><span data-stu-id="5870d-373">Common storage options for files include:</span></span>

* <span data-ttu-id="5870d-374">資料庫</span><span class="sxs-lookup"><span data-stu-id="5870d-374">Database</span></span>

  * <span data-ttu-id="5870d-375">針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。</span><span class="sxs-lookup"><span data-stu-id="5870d-375">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="5870d-376">資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。</span><span class="sxs-lookup"><span data-stu-id="5870d-376">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="5870d-377">資料庫的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="5870d-377">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="5870d-378">實體存放裝置（檔案系統或網路共用）</span><span class="sxs-lookup"><span data-stu-id="5870d-378">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="5870d-379">針對大型檔案上傳：</span><span class="sxs-lookup"><span data-stu-id="5870d-379">For large file uploads:</span></span>
    * <span data-ttu-id="5870d-380">資料庫限制可能會限制上傳的大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-380">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="5870d-381">實體儲存體通常比在資料庫中儲存的成本低。</span><span class="sxs-lookup"><span data-stu-id="5870d-381">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="5870d-382">實體儲存體的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="5870d-382">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="5870d-383">應用程式的進程必須具有儲存位置的讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="5870d-383">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="5870d-384">**絕對不要授與 execute 許可權。**</span><span class="sxs-lookup"><span data-stu-id="5870d-384">**Never grant execute permission.**</span></span>

* <span data-ttu-id="5870d-385">資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）</span><span class="sxs-lookup"><span data-stu-id="5870d-385">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="5870d-386">服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。</span><span class="sxs-lookup"><span data-stu-id="5870d-386">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="5870d-387">在大型儲存體基礎結構案例中，服務可能會降低成本。</span><span class="sxs-lookup"><span data-stu-id="5870d-387">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="5870d-388">如需詳細資訊，請參閱[快速入門：使用 .NET 在物件儲存體 @ no__t-0 中建立 blob。</span><span class="sxs-lookup"><span data-stu-id="5870d-388">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="5870d-389">本主題示範 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>，但 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> 可以在使用 <xref:System.IO.Stream> 時，用來將 <xref:System.IO.FileStream> 儲存至 blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="5870d-389">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="5870d-390">檔案上傳案例</span><span class="sxs-lookup"><span data-stu-id="5870d-390">File upload scenarios</span></span>

<span data-ttu-id="5870d-391">上傳檔案的兩個一般方法是緩衝和串流處理。</span><span class="sxs-lookup"><span data-stu-id="5870d-391">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="5870d-392">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="5870d-392">**Buffering**</span></span>

<span data-ttu-id="5870d-393">系統會將整個檔案讀入 <xref:Microsoft.AspNetCore.Http.IFormFile>，這是用來C#處理或儲存檔案的檔案標記法。</span><span class="sxs-lookup"><span data-stu-id="5870d-393">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="5870d-394">檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-394">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="5870d-395">如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。</span><span class="sxs-lookup"><span data-stu-id="5870d-395">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="5870d-396">如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。</span><span class="sxs-lookup"><span data-stu-id="5870d-396">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="5870d-397">任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-397">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="5870d-398">此主題的下列各節會涵蓋緩衝處理小型檔案：</span><span class="sxs-lookup"><span data-stu-id="5870d-398">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="5870d-399">實體儲存體</span><span class="sxs-lookup"><span data-stu-id="5870d-399">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* <span data-ttu-id="5870d-400">[[資料庫備份]](#upload-small-files-with-buffered-model-binding-to-a-database)</span><span class="sxs-lookup"><span data-stu-id="5870d-400">[Database](#upload-small-files-with-buffered-model-binding-to-a-database)</span></span>

<span data-ttu-id="5870d-401">**資料流**</span><span class="sxs-lookup"><span data-stu-id="5870d-401">**Streaming**</span></span>

<span data-ttu-id="5870d-402">此檔案會從多部分要求接收，並由應用程式直接處理或儲存。</span><span class="sxs-lookup"><span data-stu-id="5870d-402">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="5870d-403">串流不會大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="5870d-403">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="5870d-404">串流處理可減少上傳檔案時記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="5870d-404">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="5870d-405">[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-405">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="5870d-406">將具有緩衝模型系結的小型檔案上傳至實體儲存體</span><span class="sxs-lookup"><span data-stu-id="5870d-406">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="5870d-407">若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="5870d-407">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="5870d-408">下列範例示範如何使用 Razor Pages 表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：</span><span class="sxs-lookup"><span data-stu-id="5870d-408">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="5870d-409">下列範例類似于先前的範例，不同之處在于：</span><span class="sxs-lookup"><span data-stu-id="5870d-409">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="5870d-410">JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="5870d-410">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="5870d-411">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="5870d-411">There's no validation.</span></span>

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

<span data-ttu-id="5870d-412">若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-412">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="5870d-413">使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。</span><span class="sxs-lookup"><span data-stu-id="5870d-413">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="5870d-414">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="5870d-414">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="5870d-415">例如:</span><span class="sxs-lookup"><span data-stu-id="5870d-415">For example:</span></span>

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

<span data-ttu-id="5870d-416">為了支援檔案上傳，HTML 表單必須指定 `multipart/form-data` 的編碼類型（`enctype`）。</span><span class="sxs-lookup"><span data-stu-id="5870d-416">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="5870d-417">若要支援上傳多個檔案的 @no__t 0 輸入元素，請在 `<input>` 元素上提供 `multiple` 屬性：</span><span class="sxs-lookup"><span data-stu-id="5870d-417">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="5870d-418">您可以使用 <xref:Microsoft.AspNetCore.Http.IFormFile>，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-418">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="5870d-419">範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="5870d-419">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-420">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 @no__t 0 屬性而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="5870d-420">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="5870d-421">@No__t-0 屬性只應用於顯示用途，而且只能用於 HTML 編碼值。</span><span class="sxs-lookup"><span data-stu-id="5870d-421">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="5870d-422">到目前為止所提供的範例並不考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="5870d-422">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="5870d-423">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="5870d-423">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="5870d-424">安全性考量</span><span class="sxs-lookup"><span data-stu-id="5870d-424">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="5870d-425">驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-425">Validation</span></span>](#validation)

<span data-ttu-id="5870d-426">使用模型系結上傳檔案並 <xref:Microsoft.AspNetCore.Http.IFormFile> 時，動作方法可以接受：</span><span class="sxs-lookup"><span data-stu-id="5870d-426">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="5870d-427">單一 <xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="5870d-427">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="5870d-428">下列任何代表數個檔案的集合：</span><span class="sxs-lookup"><span data-stu-id="5870d-428">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="5870d-429">[清單](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="5870d-429">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="5870d-430">系結符合依名稱的表單檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-430">Binding matches form files by name.</span></span> <span data-ttu-id="5870d-431">例如，`<input type="file" name="formFile">` 中的 HTML `name` 值必須符合系結C#的參數/屬性（`FormFile`）。</span><span class="sxs-lookup"><span data-stu-id="5870d-431">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="5870d-432">如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-432">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="5870d-433">下列範例：</span><span class="sxs-lookup"><span data-stu-id="5870d-433">The following example:</span></span>

* <span data-ttu-id="5870d-434">迴圈一或多個已上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-434">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="5870d-435">會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-435">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="5870d-436">使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。</span><span class="sxs-lookup"><span data-stu-id="5870d-436">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="5870d-437">傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-437">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="5870d-438">使用 `Path.GetRandomFileName` 可產生不含路徑的檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-438">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="5870d-439">在下列範例中，路徑是從設定取得：</span><span class="sxs-lookup"><span data-stu-id="5870d-439">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="5870d-440">傳遞給 <xref:System.IO.FileStream> 的路徑*必須*包含檔案名。</span><span class="sxs-lookup"><span data-stu-id="5870d-440">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="5870d-441">如果未提供檔案名，則會在執行時間擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="5870d-441">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="5870d-442">使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 技術所上傳的檔案會在處理之前，先緩衝到伺服器上的記憶體或磁片上。</span><span class="sxs-lookup"><span data-stu-id="5870d-442">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="5870d-443">在動作方法內，<xref:Microsoft.AspNetCore.Http.IFormFile> 內容可以 <xref:System.IO.Stream> 來存取。</span><span class="sxs-lookup"><span data-stu-id="5870d-443">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="5870d-444">除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。</span><span class="sxs-lookup"><span data-stu-id="5870d-444">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="5870d-445">如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。</span><span class="sxs-lookup"><span data-stu-id="5870d-445">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-446">如果建立超過65535個檔案，但未刪除先前的暫存檔案，則[GetTempFileName](xref:System.IO.Path.GetTempFileName*)會擲回 <xref:System.IO.IOException>。</span><span class="sxs-lookup"><span data-stu-id="5870d-446">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="5870d-447">65535檔案的限制是每一伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="5870d-447">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="5870d-448">如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：</span><span class="sxs-lookup"><span data-stu-id="5870d-448">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="5870d-449">GetTempFileNameA 函式</span><span class="sxs-lookup"><span data-stu-id="5870d-449">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="5870d-450">將具有緩衝模型系結的小型檔案上傳至資料庫</span><span class="sxs-lookup"><span data-stu-id="5870d-450">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="5870d-451">若要使用[Entity Framework](/ef/core/index)將二進位檔案資料儲存在資料庫中，請在實體上定義 @no__t 1 陣列屬性：</span><span class="sxs-lookup"><span data-stu-id="5870d-451">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="5870d-452">指定包含 <xref:Microsoft.AspNetCore.Http.IFormFile> 之類別的頁面模型屬性：</span><span class="sxs-lookup"><span data-stu-id="5870d-452">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="5870d-453"><xref:Microsoft.AspNetCore.Http.IFormFile> 可以直接當做動作方法參數或系結模型屬性使用。</span><span class="sxs-lookup"><span data-stu-id="5870d-453"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="5870d-454">先前的範例會使用系結模型屬性。</span><span class="sxs-lookup"><span data-stu-id="5870d-454">The prior example uses a bound model property.</span></span>

<span data-ttu-id="5870d-455">@No__t-0 用於 Razor Pages 格式：</span><span class="sxs-lookup"><span data-stu-id="5870d-455">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="5870d-456">當表單張貼至伺服器時，將 <xref:Microsoft.AspNetCore.Http.IFormFile> 複製到資料流程，並將它儲存為資料庫中的位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="5870d-456">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="5870d-457">在下列範例中，`_dbContext` 會儲存應用程式的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="5870d-457">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="5870d-458">前述範例類似于範例應用程式中所示範的案例：</span><span class="sxs-lookup"><span data-stu-id="5870d-458">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="5870d-459">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5870d-459">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="5870d-460">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5870d-460">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-461">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="5870d-461">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="5870d-462">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 @no__t 0 屬性而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="5870d-462">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="5870d-463">@No__t-0 屬性只能用於顯示用途，而且只能用於 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="5870d-463">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="5870d-464">提供的範例不會考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="5870d-464">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="5870d-465">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="5870d-465">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="5870d-466">安全性考量</span><span class="sxs-lookup"><span data-stu-id="5870d-466">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="5870d-467">驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-467">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="5870d-468">使用串流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="5870d-468">Upload large files with streaming</span></span>

<span data-ttu-id="5870d-469">下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="5870d-469">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="5870d-470">檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="5870d-470">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="5870d-471">由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。</span><span class="sxs-lookup"><span data-stu-id="5870d-471">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="5870d-472">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-472">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="5870d-473">讀取多部分區段之後，動作會執行自己的模型系結。</span><span class="sxs-lookup"><span data-stu-id="5870d-473">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="5870d-474">初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="5870d-474">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="5870d-475">屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：</span><span class="sxs-lookup"><span data-stu-id="5870d-475">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="5870d-476">@No__t-0 是用來停用模型系結：</span><span class="sxs-lookup"><span data-stu-id="5870d-476">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="5870d-477">在範例應用程式中，`GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會以篩選準則套用至 `/StreamedSingleFileUploadDb` 的頁面應用程式模型，並使用[@no__t 慣例](xref:razor-pages/razor-pages-conventions)在 Razor Pages-4 中 `/StreamedSingleFileUploadPhysical`：</span><span class="sxs-lookup"><span data-stu-id="5870d-477">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="5870d-478">由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。</span><span class="sxs-lookup"><span data-stu-id="5870d-478">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="5870d-479">動作方法會直接與 `Request` 屬性搭配運作。</span><span class="sxs-lookup"><span data-stu-id="5870d-479">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="5870d-480">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="5870d-480">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="5870d-481">索引鍵/值資料會儲存在 `KeyValueAccumulator` 中。</span><span class="sxs-lookup"><span data-stu-id="5870d-481">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="5870d-482">讀取多部分區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料系結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="5870d-482">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="5870d-483">使用 EF Core 串流至資料庫的完整 `StreamingController.UploadDatabase` 方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-483">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="5870d-484">`MultipartRequestHelper` （*公用程式/MultipartRequestHelper .cs*）：</span><span class="sxs-lookup"><span data-stu-id="5870d-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="5870d-485">串流至實體位置的完整 `StreamingController.UploadPhysical` 方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-485">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="5870d-486">在範例應用程式中，驗證檢查是由 `FileHelpers.ProcessStreamedFile` 來處理。</span><span class="sxs-lookup"><span data-stu-id="5870d-486">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="5870d-487">驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-487">Validation</span></span>

<span data-ttu-id="5870d-488">範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理的 @no__t 1 和串流檔案上傳的檢查。</span><span class="sxs-lookup"><span data-stu-id="5870d-488">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="5870d-489">若要處理範例應用程式中 @no__t 0 的緩衝檔案上傳，請參閱*公用程式/FileHelpers*檔案中的 `ProcessFormFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="5870d-489">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="5870d-490">如需處理資料流程檔案，請參閱相同檔案中的 `ProcessStreamedFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="5870d-490">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="5870d-491">範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-491">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="5870d-492">在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。</span><span class="sxs-lookup"><span data-stu-id="5870d-492">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="5870d-493">雖然主題範例提供驗證技術的實用範例，但請不要在生產應用程式中執行 @no__t 0 類別，除非您：</span><span class="sxs-lookup"><span data-stu-id="5870d-493">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="5870d-494">完全瞭解此實作為。</span><span class="sxs-lookup"><span data-stu-id="5870d-494">Fully understand the implementation.</span></span>
> * <span data-ttu-id="5870d-495">針對應用程式的環境和規格修改適當的執行。</span><span class="sxs-lookup"><span data-stu-id="5870d-495">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="5870d-496">**絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**</span><span class="sxs-lookup"><span data-stu-id="5870d-496">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="5870d-497">內容驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-497">Content validation</span></span>

<span data-ttu-id="5870d-498">**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="5870d-498">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="5870d-499">在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。</span><span class="sxs-lookup"><span data-stu-id="5870d-499">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="5870d-500">如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。</span><span class="sxs-lookup"><span data-stu-id="5870d-500">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="5870d-501">一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。</span><span class="sxs-lookup"><span data-stu-id="5870d-501">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="5870d-502">當檔案通過時，檔案就會移至一般檔案儲存位置。</span><span class="sxs-lookup"><span data-stu-id="5870d-502">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="5870d-503">這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。</span><span class="sxs-lookup"><span data-stu-id="5870d-503">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="5870d-504">藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。</span><span class="sxs-lookup"><span data-stu-id="5870d-504">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="5870d-505">副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-505">File extension validation</span></span>

<span data-ttu-id="5870d-506">已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。</span><span class="sxs-lookup"><span data-stu-id="5870d-506">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="5870d-507">例如:</span><span class="sxs-lookup"><span data-stu-id="5870d-507">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="5870d-508">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-508">File signature validation</span></span>

<span data-ttu-id="5870d-509">檔案的簽章取決於檔案開頭的前幾個位元組。</span><span class="sxs-lookup"><span data-stu-id="5870d-509">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="5870d-510">這些位元組可用來指出延伸模組是否符合檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-510">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="5870d-511">範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="5870d-511">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="5870d-512">在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：</span><span class="sxs-lookup"><span data-stu-id="5870d-512">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="5870d-513">若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。</span><span class="sxs-lookup"><span data-stu-id="5870d-513">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="5870d-514">檔案名安全性</span><span class="sxs-lookup"><span data-stu-id="5870d-514">File name security</span></span>

<span data-ttu-id="5870d-515">絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。</span><span class="sxs-lookup"><span data-stu-id="5870d-515">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="5870d-516">使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。</span><span class="sxs-lookup"><span data-stu-id="5870d-516">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="5870d-517">Razor 會自動對屬性值進行 HTML 編碼以供顯示。</span><span class="sxs-lookup"><span data-stu-id="5870d-517">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="5870d-518">以下是可安全使用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="5870d-518">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="5870d-519">在 Razor 以外，一律會從使用者的要求 @no__t 0 的檔案名內容。</span><span class="sxs-lookup"><span data-stu-id="5870d-519">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="5870d-520">許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。</span><span class="sxs-lookup"><span data-stu-id="5870d-520">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="5870d-521">提供其他邏輯以符合您應用程式的規格。</span><span class="sxs-lookup"><span data-stu-id="5870d-521">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="5870d-522">大小驗證</span><span class="sxs-lookup"><span data-stu-id="5870d-522">Size validation</span></span>

<span data-ttu-id="5870d-523">限制上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="5870d-523">Limit the size of uploaded files.</span></span>

<span data-ttu-id="5870d-524">在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。</span><span class="sxs-lookup"><span data-stu-id="5870d-524">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="5870d-525">此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的設定來提供：</span><span class="sxs-lookup"><span data-stu-id="5870d-525">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="5870d-526">@No__t-0 會插入 @no__t 1 類別中：</span><span class="sxs-lookup"><span data-stu-id="5870d-526">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="5870d-527">當檔案大小超過限制時，就會拒絕此檔案：</span><span class="sxs-lookup"><span data-stu-id="5870d-527">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="5870d-528">Match name 屬性值與 POST 方法的參數名稱</span><span class="sxs-lookup"><span data-stu-id="5870d-528">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="5870d-529">在張貼表單資料或直接使用 JavaScript `FormData` 的非 Razor 表單中，在表單的元素或 `FormData` 中指定的名稱必須符合控制器動作中參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="5870d-529">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="5870d-530">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="5870d-530">In the following example:</span></span>

* <span data-ttu-id="5870d-531">使用 `<input>` 元素時，`name` 屬性會設定為值 `battlePlans`：</span><span class="sxs-lookup"><span data-stu-id="5870d-531">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="5870d-532">在 JavaScript 中使用 `FormData` 時，此名稱會設定為 `battlePlans` 的值：</span><span class="sxs-lookup"><span data-stu-id="5870d-532">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="5870d-533">針對C#方法的參數使用相符的名稱（`battlePlans`）：</span><span class="sxs-lookup"><span data-stu-id="5870d-533">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="5870d-534">針對名為 `Upload` 的 Razor Pages 頁面處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-534">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="5870d-535">針對 MVC POST 控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-535">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="5870d-536">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="5870d-536">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="5870d-537">多部分主體長度限制</span><span class="sxs-lookup"><span data-stu-id="5870d-537">Multipart body length limit</span></span>

<span data-ttu-id="5870d-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 會設定每個多部分主體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="5870d-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="5870d-539">超過此限制的表單區段會在剖析時擲回 <xref:System.IO.InvalidDataException>。</span><span class="sxs-lookup"><span data-stu-id="5870d-539">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="5870d-540">預設值為134217728（128 MB）。</span><span class="sxs-lookup"><span data-stu-id="5870d-540">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="5870d-541">使用 `Startup.ConfigureServices` 中的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定來自訂限制：</span><span class="sxs-lookup"><span data-stu-id="5870d-541">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="5870d-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> 用來設定單一頁面或動作的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>。</span><span class="sxs-lookup"><span data-stu-id="5870d-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="5870d-543">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices` 中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="5870d-543">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="5870d-544">在 Razor Pages 應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-544">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="5870d-545">Kestrel 要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="5870d-545">Kestrel maximum request body size</span></span>

<span data-ttu-id="5870d-546">針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="5870d-546">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="5870d-547">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：</span><span class="sxs-lookup"><span data-stu-id="5870d-547">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="5870d-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 是用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。</span><span class="sxs-lookup"><span data-stu-id="5870d-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="5870d-549">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices` 中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="5870d-549">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="5870d-550">在 Razor pages 應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="5870d-550">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="5870d-551">其他 Kestrel 限制</span><span class="sxs-lookup"><span data-stu-id="5870d-551">Other Kestrel limits</span></span>

<span data-ttu-id="5870d-552">其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="5870d-552">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="5870d-553">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="5870d-553">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="5870d-554">要求和回應資料速率</span><span class="sxs-lookup"><span data-stu-id="5870d-554">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="5870d-555">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="5870d-555">IIS content length limit</span></span>

<span data-ttu-id="5870d-556">預設要求限制（`maxAllowedContentLength`）是30000000個位元組，大約是 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="5870d-556">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="5870d-557">自訂*web.config*檔案中的限制：</span><span class="sxs-lookup"><span data-stu-id="5870d-557">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="5870d-558">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="5870d-558">This setting only applies to IIS.</span></span> <span data-ttu-id="5870d-559">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="5870d-559">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="5870d-560">如需詳細資訊，請參閱[\<requestLimits > 的要求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="5870d-560">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="5870d-561">ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。</span><span class="sxs-lookup"><span data-stu-id="5870d-561">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="5870d-562">如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（aspnet/AspNetCore #2711）](https://github.com/aspnet/AspNetCore/issues/2711)。</span><span class="sxs-lookup"><span data-stu-id="5870d-562">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="5870d-563">疑難排解</span><span class="sxs-lookup"><span data-stu-id="5870d-563">Troubleshoot</span></span>

<span data-ttu-id="5870d-564">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="5870d-564">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="5870d-565">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="5870d-565">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="5870d-566">下列錯誤表示上傳的檔案超過伺服器設定的內容長度：</span><span class="sxs-lookup"><span data-stu-id="5870d-566">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="5870d-567">如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-567">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="5870d-568">連接失敗</span><span class="sxs-lookup"><span data-stu-id="5870d-568">Connection failure</span></span>

<span data-ttu-id="5870d-569">連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。</span><span class="sxs-lookup"><span data-stu-id="5870d-569">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="5870d-570">如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。</span><span class="sxs-lookup"><span data-stu-id="5870d-570">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="5870d-571">Kestrel 用戶端連接限制也可能需要調整。</span><span class="sxs-lookup"><span data-stu-id="5870d-571">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="5870d-572">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="5870d-572">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="5870d-573">如果控制器接受使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 的上傳檔案，但值為 `null`，請確認 HTML 表單指定的 `enctype` 值為 `multipart/form-data`。</span><span class="sxs-lookup"><span data-stu-id="5870d-573">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="5870d-574">如果未在 `<form>` 元素上設定此屬性，則不會進行檔案上傳，而且任何系結的 @no__t 1 引數都會 `null`。</span><span class="sxs-lookup"><span data-stu-id="5870d-574">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="5870d-575">此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="5870d-575">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="5870d-576">其他資源</span><span class="sxs-lookup"><span data-stu-id="5870d-576">Additional resources</span></span>

* [<span data-ttu-id="5870d-577">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="5870d-577">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* <span data-ttu-id="5870d-578">@no__t 0Azure 安全性：安全性框架：輸入驗證 |緩和措施 @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="5870d-578">[Azure Security: Security Frame: Input Validation | Mitigations](/azure/security/azure-security-threat-modeling-tool-input-validation)</span></span>
* <span data-ttu-id="5870d-579">@no__t 0Azure 雲端設計模式：Valet 索引鍵模式 @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="5870d-579">[Azure Cloud Design Patterns: Valet Key pattern](/azure/architecture/patterns/valet-key)</span></span>
