---
title: 上傳 ASP.NET Core 中的檔案
author: guardrex
description: 如何使用模型繫結和資料流在 ASP.NET Core MVC 上傳檔案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 04e7533aa190a4875d3f66e8665fec16abec48b3
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73462948"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="96a04-103">上傳 ASP.NET Core 中的檔案</span><span class="sxs-lookup"><span data-stu-id="96a04-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="96a04-104">作者： [Luke Latham](https://github.com/guardrex)、 [Steve Smith](https://ardalis.com/)和[Rutger 風暴](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="96a04-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="96a04-105">ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。</span><span class="sxs-lookup"><span data-stu-id="96a04-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="96a04-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96a04-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="96a04-107">安全性考量</span><span class="sxs-lookup"><span data-stu-id="96a04-107">Security considerations</span></span>

<span data-ttu-id="96a04-108">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="96a04-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="96a04-109">攻擊者可能會嘗試：</span><span class="sxs-lookup"><span data-stu-id="96a04-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="96a04-110">執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="96a04-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="96a04-111">上傳病毒或惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="96a04-112">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="96a04-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="96a04-113">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="96a04-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="96a04-114">將檔案上傳到專用的檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="96a04-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="96a04-115">專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。</span><span class="sxs-lookup"><span data-stu-id="96a04-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="96a04-116">停用檔案上傳位置的 execute 許可權。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="96a04-117">請勿將上傳的**檔案保存在**與應用程式相同的目錄樹狀結構中。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="96a04-118">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="96a04-119">請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。&dagger; HTML 在顯示不受信任的檔案名時進行編碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="96a04-120">例如，記錄檔案名或在 UI 中顯示（Razor 會自動以 HTML 編碼輸出）。</span><span class="sxs-lookup"><span data-stu-id="96a04-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="96a04-121">僅允許應用程式設計規格的已核准副檔名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="96a04-122">確認用戶端檢查是在伺服器上執行。&dagger; 用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="96a04-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="96a04-123">檢查上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="96a04-124">設定最大大小限制以防止大量上傳。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="96a04-125">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="96a04-126">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="96a04-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="96a04-127">&dagger;範例應用程式示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="96a04-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-128">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="96a04-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="96a04-129">完全取得系統的控制權。</span><span class="sxs-lookup"><span data-stu-id="96a04-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="96a04-130">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="96a04-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="96a04-131">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="96a04-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="96a04-132">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="96a04-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="96a04-133">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="96a04-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="96a04-134">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="96a04-134">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="96a04-135">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="96a04-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="96a04-136">如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="96a04-137">儲存案例</span><span class="sxs-lookup"><span data-stu-id="96a04-137">Storage scenarios</span></span>

<span data-ttu-id="96a04-138">檔案的一般儲存選項包括：</span><span class="sxs-lookup"><span data-stu-id="96a04-138">Common storage options for files include:</span></span>

* <span data-ttu-id="96a04-139">資料庫</span><span class="sxs-lookup"><span data-stu-id="96a04-139">Database</span></span>

  * <span data-ttu-id="96a04-140">針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。</span><span class="sxs-lookup"><span data-stu-id="96a04-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="96a04-141">資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。</span><span class="sxs-lookup"><span data-stu-id="96a04-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="96a04-142">資料庫的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="96a04-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="96a04-143">實體存放裝置（檔案系統或網路共用）</span><span class="sxs-lookup"><span data-stu-id="96a04-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="96a04-144">針對大型檔案上傳：</span><span class="sxs-lookup"><span data-stu-id="96a04-144">For large file uploads:</span></span>
    * <span data-ttu-id="96a04-145">資料庫限制可能會限制上傳的大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="96a04-146">實體儲存體通常比在資料庫中儲存的成本低。</span><span class="sxs-lookup"><span data-stu-id="96a04-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="96a04-147">實體儲存體的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="96a04-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="96a04-148">應用程式的進程必須具有儲存位置的讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="96a04-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="96a04-149">**絕對不要授與 execute 許可權。**</span><span class="sxs-lookup"><span data-stu-id="96a04-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="96a04-150">資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）</span><span class="sxs-lookup"><span data-stu-id="96a04-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="96a04-151">服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。</span><span class="sxs-lookup"><span data-stu-id="96a04-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="96a04-152">在大型儲存體基礎結構案例中，服務可能會降低成本。</span><span class="sxs-lookup"><span data-stu-id="96a04-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="96a04-153">如需詳細資訊，請參閱[快速入門：使用 .net 在物件儲存體中建立 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="96a04-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="96a04-154">本主題示範 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>，但 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> 可以在使用 <xref:System.IO.Stream>時，用來將 <xref:System.IO.FileStream> 儲存至 blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="96a04-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="96a04-155">檔案上傳案例</span><span class="sxs-lookup"><span data-stu-id="96a04-155">File upload scenarios</span></span>

<span data-ttu-id="96a04-156">上傳檔案的兩個一般方法是緩衝和串流處理。</span><span class="sxs-lookup"><span data-stu-id="96a04-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="96a04-157">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="96a04-157">**Buffering**</span></span>

<span data-ttu-id="96a04-158">系統會將整個檔案讀入 <xref:Microsoft.AspNetCore.Http.IFormFile>，這是用來C#處理或儲存檔案的檔案標記法。</span><span class="sxs-lookup"><span data-stu-id="96a04-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="96a04-159">檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="96a04-160">如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。</span><span class="sxs-lookup"><span data-stu-id="96a04-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="96a04-161">如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。</span><span class="sxs-lookup"><span data-stu-id="96a04-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="96a04-162">任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="96a04-163">此主題的下列各節會涵蓋緩衝處理小型檔案：</span><span class="sxs-lookup"><span data-stu-id="96a04-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="96a04-164">實體儲存體</span><span class="sxs-lookup"><span data-stu-id="96a04-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="96a04-165">資料庫</span><span class="sxs-lookup"><span data-stu-id="96a04-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="96a04-166">**資料流**</span><span class="sxs-lookup"><span data-stu-id="96a04-166">**Streaming**</span></span>

<span data-ttu-id="96a04-167">此檔案會從多部分要求接收，並由應用程式直接處理或儲存。</span><span class="sxs-lookup"><span data-stu-id="96a04-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="96a04-168">串流不會大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="96a04-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="96a04-169">串流處理可減少上傳檔案時記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="96a04-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="96a04-170">[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="96a04-171">將具有緩衝模型系結的小型檔案上傳至實體儲存體</span><span class="sxs-lookup"><span data-stu-id="96a04-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="96a04-172">若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="96a04-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="96a04-173">下列範例示範如何使用 Razor Pages 表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：</span><span class="sxs-lookup"><span data-stu-id="96a04-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="96a04-174">下列範例類似于先前的範例，不同之處在于：</span><span class="sxs-lookup"><span data-stu-id="96a04-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="96a04-175">JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="96a04-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="96a04-176">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="96a04-176">There's no validation.</span></span>

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

<span data-ttu-id="96a04-177">若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="96a04-178">使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。</span><span class="sxs-lookup"><span data-stu-id="96a04-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="96a04-179">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="96a04-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="96a04-180">例如:</span><span class="sxs-lookup"><span data-stu-id="96a04-180">For example:</span></span>

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

<span data-ttu-id="96a04-181">為了支援檔案上傳，HTML 表單必須指定 `multipart/form-data`的編碼類型（`enctype`）。</span><span class="sxs-lookup"><span data-stu-id="96a04-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="96a04-182">若要支援上傳多個檔案的 `files` input 元素，請在 `<input>` 元素上提供 `multiple` 屬性：</span><span class="sxs-lookup"><span data-stu-id="96a04-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="96a04-183">您可以使用 <xref:Microsoft.AspNetCore.Http.IFormFile>，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="96a04-184">範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="96a04-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="96a04-185">除了顯示和記錄之外，**請勿使用 <xref:Microsoft.AspNetCore.Http.IFormFile>** 的 `FileName` 屬性。</span><span class="sxs-lookup"><span data-stu-id="96a04-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="96a04-186">當顯示或記錄時，HTML 會將檔案名編碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="96a04-187">攻擊者可以提供惡意的檔案名，包括完整路徑或相對路徑。</span><span class="sxs-lookup"><span data-stu-id="96a04-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="96a04-188">應用程式應該：</span><span class="sxs-lookup"><span data-stu-id="96a04-188">Applications should:</span></span>
>
> * <span data-ttu-id="96a04-189">從使用者提供的檔案名中移除路徑。</span><span class="sxs-lookup"><span data-stu-id="96a04-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="96a04-190">針對 UI 或記錄儲存以 HTML 編碼、路徑移除的檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="96a04-191">為儲存區產生新的隨機檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="96a04-192">下列程式碼會從檔案名中移除路徑：</span><span class="sxs-lookup"><span data-stu-id="96a04-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="96a04-193">到目前為止所提供的範例並不考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="96a04-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="96a04-194">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="96a04-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="96a04-195">安全性考量</span><span class="sxs-lookup"><span data-stu-id="96a04-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="96a04-196">驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-196">Validation</span></span>](#validation)

<span data-ttu-id="96a04-197">使用模型系結和 <xref:Microsoft.AspNetCore.Http.IFormFile>上傳檔案時，動作方法可以接受：</span><span class="sxs-lookup"><span data-stu-id="96a04-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="96a04-198">單一 <xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="96a04-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="96a04-199">下列任何代表數個檔案的集合：</span><span class="sxs-lookup"><span data-stu-id="96a04-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="96a04-200">[列出](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="96a04-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="96a04-201">系結符合依名稱的表單檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-201">Binding matches form files by name.</span></span> <span data-ttu-id="96a04-202">例如，`<input type="file" name="formFile">` 中的 HTML `name` 值必須符合C#參數/屬性系結（`FormFile`）。</span><span class="sxs-lookup"><span data-stu-id="96a04-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="96a04-203">如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="96a04-204">下列範例：</span><span class="sxs-lookup"><span data-stu-id="96a04-204">The following example:</span></span>

* <span data-ttu-id="96a04-205">迴圈一或多個已上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="96a04-206">會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="96a04-207">使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。</span><span class="sxs-lookup"><span data-stu-id="96a04-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="96a04-208">傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-208">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="96a04-209">使用 `Path.GetRandomFileName` 產生不含路徑的檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="96a04-210">在下列範例中，路徑是從設定取得：</span><span class="sxs-lookup"><span data-stu-id="96a04-210">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="96a04-211">傳遞至 <xref:System.IO.FileStream> 的路徑*必須*包含檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="96a04-212">如果未提供檔案名，則會在執行時間擲回 <xref:System.UnauthorizedAccessException>。</span><span class="sxs-lookup"><span data-stu-id="96a04-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="96a04-213">使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 技術所上傳的檔案會在處理之前，先緩衝到伺服器上的記憶體或磁片上。</span><span class="sxs-lookup"><span data-stu-id="96a04-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="96a04-214">在動作方法內，<xref:Microsoft.AspNetCore.Http.IFormFile> 內容可以 <xref:System.IO.Stream>存取。</span><span class="sxs-lookup"><span data-stu-id="96a04-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="96a04-215">除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。</span><span class="sxs-lookup"><span data-stu-id="96a04-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="96a04-216">如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。</span><span class="sxs-lookup"><span data-stu-id="96a04-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-217">如果建立的檔案超過65535，而未刪除先前的暫存檔案，則[GetTempFileName](xref:System.IO.Path.GetTempFileName*)會擲回 <xref:System.IO.IOException>。</span><span class="sxs-lookup"><span data-stu-id="96a04-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="96a04-218">65535檔案的限制是每一伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="96a04-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="96a04-219">如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：</span><span class="sxs-lookup"><span data-stu-id="96a04-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="96a04-220">GetTempFileNameA 函式</span><span class="sxs-lookup"><span data-stu-id="96a04-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="96a04-221">將具有緩衝模型系結的小型檔案上傳至資料庫</span><span class="sxs-lookup"><span data-stu-id="96a04-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="96a04-222">若要使用[Entity Framework](/ef/core/index)在資料庫中儲存二進位檔案資料，請在實體上定義 <xref:System.Byte> 陣列屬性：</span><span class="sxs-lookup"><span data-stu-id="96a04-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="96a04-223">指定包含 <xref:Microsoft.AspNetCore.Http.IFormFile>之類別的 [頁面模型] 屬性：</span><span class="sxs-lookup"><span data-stu-id="96a04-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="96a04-224"><xref:Microsoft.AspNetCore.Http.IFormFile> 可以直接當做動作方法參數或系結模型屬性來使用。</span><span class="sxs-lookup"><span data-stu-id="96a04-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="96a04-225">先前的範例會使用系結模型屬性。</span><span class="sxs-lookup"><span data-stu-id="96a04-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="96a04-226">`FileUpload` 用於 Razor Pages 形式：</span><span class="sxs-lookup"><span data-stu-id="96a04-226">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="96a04-227">當表單張貼至伺服器時，請將 <xref:Microsoft.AspNetCore.Http.IFormFile> 複製到資料流程，並將其儲存為資料庫中的位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="96a04-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="96a04-228">在下列範例中，`_dbContext` 會儲存應用程式的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="96a04-228">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="96a04-229">前述範例類似于範例應用程式中所示範的案例：</span><span class="sxs-lookup"><span data-stu-id="96a04-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="96a04-230">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="96a04-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="96a04-231">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="96a04-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-232">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="96a04-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="96a04-233">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 `FileName` 屬性，而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="96a04-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="96a04-234">`FileName` 屬性只能用於顯示用途，而且只能用於 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="96a04-235">提供的範例不會考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="96a04-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="96a04-236">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="96a04-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="96a04-237">安全性考量</span><span class="sxs-lookup"><span data-stu-id="96a04-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="96a04-238">驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="96a04-239">使用串流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="96a04-239">Upload large files with streaming</span></span>

<span data-ttu-id="96a04-240">下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="96a04-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="96a04-241">檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="96a04-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="96a04-242">由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。</span><span class="sxs-lookup"><span data-stu-id="96a04-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="96a04-243">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="96a04-244">讀取多部分區段之後，動作會執行自己的模型系結。</span><span class="sxs-lookup"><span data-stu-id="96a04-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="96a04-245">初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="96a04-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="96a04-246">屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：</span><span class="sxs-lookup"><span data-stu-id="96a04-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="96a04-247">`DisableFormValueModelBindingAttribute` 可用來停用模型系結：</span><span class="sxs-lookup"><span data-stu-id="96a04-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="96a04-248">在範例應用程式中，`GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會當做篩選套用至 `/StreamedSingleFileUploadDb` 的頁面應用程式模型，以及 `Startup.ConfigureServices` 中使用[Razor Pages 慣例](xref:razor-pages/razor-pages-conventions)的 `/StreamedSingleFileUploadPhysical`：</span><span class="sxs-lookup"><span data-stu-id="96a04-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="96a04-249">由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。</span><span class="sxs-lookup"><span data-stu-id="96a04-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="96a04-250">動作方法會直接與 `Request` 屬性搭配運作。</span><span class="sxs-lookup"><span data-stu-id="96a04-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="96a04-251">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="96a04-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="96a04-252">索引鍵/值資料會儲存在 `KeyValueAccumulator`中。</span><span class="sxs-lookup"><span data-stu-id="96a04-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="96a04-253">讀取多部分區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料系結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="96a04-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="96a04-254">使用 EF Core 串流至資料庫的完整 `StreamingController.UploadDatabase` 方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="96a04-255">`MultipartRequestHelper` （*公用程式/MultipartRequestHelper .cs*）：</span><span class="sxs-lookup"><span data-stu-id="96a04-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="96a04-256">串流至實體位置的完整 `StreamingController.UploadPhysical` 方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="96a04-257">在範例應用程式中，驗證檢查是由 `FileHelpers.ProcessStreamedFile`處理。</span><span class="sxs-lookup"><span data-stu-id="96a04-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="96a04-258">驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-258">Validation</span></span>

<span data-ttu-id="96a04-259">範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理的 <xref:Microsoft.AspNetCore.Http.IFormFile> 和串流檔案上傳的檢查。</span><span class="sxs-lookup"><span data-stu-id="96a04-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="96a04-260">若要處理範例應用程式中 <xref:Microsoft.AspNetCore.Http.IFormFile> 緩衝的檔案上傳，請參閱*公用程式/FileHelpers*檔案中的 `ProcessFormFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="96a04-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="96a04-261">如需處理資料流程檔案，請參閱相同檔案中的 `ProcessStreamedFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="96a04-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-262">範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="96a04-263">在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="96a04-264">雖然主題範例提供驗證技術的實用範例，但請勿在生產應用程式中執行 `FileHelpers` 類別，除非您：</span><span class="sxs-lookup"><span data-stu-id="96a04-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="96a04-265">完全瞭解此實作為。</span><span class="sxs-lookup"><span data-stu-id="96a04-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="96a04-266">針對應用程式的環境和規格修改適當的執行。</span><span class="sxs-lookup"><span data-stu-id="96a04-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="96a04-267">**絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**</span><span class="sxs-lookup"><span data-stu-id="96a04-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="96a04-268">內容驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-268">Content validation</span></span>

<span data-ttu-id="96a04-269">**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="96a04-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="96a04-270">在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。</span><span class="sxs-lookup"><span data-stu-id="96a04-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="96a04-271">如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。</span><span class="sxs-lookup"><span data-stu-id="96a04-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="96a04-272">一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。</span><span class="sxs-lookup"><span data-stu-id="96a04-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="96a04-273">當檔案通過時，檔案就會移至一般檔案儲存位置。</span><span class="sxs-lookup"><span data-stu-id="96a04-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="96a04-274">這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。</span><span class="sxs-lookup"><span data-stu-id="96a04-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="96a04-275">藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。</span><span class="sxs-lookup"><span data-stu-id="96a04-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="96a04-276">副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-276">File extension validation</span></span>

<span data-ttu-id="96a04-277">已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。</span><span class="sxs-lookup"><span data-stu-id="96a04-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="96a04-278">例如:</span><span class="sxs-lookup"><span data-stu-id="96a04-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="96a04-279">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-279">File signature validation</span></span>

<span data-ttu-id="96a04-280">檔案的簽章取決於檔案開頭的前幾個位元組。</span><span class="sxs-lookup"><span data-stu-id="96a04-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="96a04-281">這些位元組可用來指出延伸模組是否符合檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="96a04-282">範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="96a04-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="96a04-283">在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：</span><span class="sxs-lookup"><span data-stu-id="96a04-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="96a04-284">若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。</span><span class="sxs-lookup"><span data-stu-id="96a04-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="96a04-285">檔案名安全性</span><span class="sxs-lookup"><span data-stu-id="96a04-285">File name security</span></span>

<span data-ttu-id="96a04-286">絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。</span><span class="sxs-lookup"><span data-stu-id="96a04-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="96a04-287">使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。</span><span class="sxs-lookup"><span data-stu-id="96a04-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="96a04-288">Razor 會自動對屬性值進行 HTML 編碼以供顯示。</span><span class="sxs-lookup"><span data-stu-id="96a04-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="96a04-289">以下是可安全使用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="96a04-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="96a04-290">在 Razor 以外，一律 <xref:System.Net.WebUtility.HtmlEncode*> 來自使用者要求的檔案名內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="96a04-291">許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。</span><span class="sxs-lookup"><span data-stu-id="96a04-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="96a04-292">提供其他邏輯以符合您應用程式的規格。</span><span class="sxs-lookup"><span data-stu-id="96a04-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="96a04-293">大小驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-293">Size validation</span></span>

<span data-ttu-id="96a04-294">限制上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="96a04-295">在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。</span><span class="sxs-lookup"><span data-stu-id="96a04-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="96a04-296">此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的設定來提供：</span><span class="sxs-lookup"><span data-stu-id="96a04-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="96a04-297">`FileSizeLimit` 會插入 `PageModel` 類別中：</span><span class="sxs-lookup"><span data-stu-id="96a04-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="96a04-298">當檔案大小超過限制時，就會拒絕此檔案：</span><span class="sxs-lookup"><span data-stu-id="96a04-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="96a04-299">Match name 屬性值與 POST 方法的參數名稱</span><span class="sxs-lookup"><span data-stu-id="96a04-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="96a04-300">在張貼表單資料或直接使用 JavaScript `FormData` 的非 Razor 表單中，在表單的元素或 `FormData` 中指定的名稱必須符合控制器動作中參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="96a04-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="96a04-301">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="96a04-301">In the following example:</span></span>

* <span data-ttu-id="96a04-302">使用 `<input>` 元素時，`name` 屬性會設定為 `battlePlans`的值：</span><span class="sxs-lookup"><span data-stu-id="96a04-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="96a04-303">在 JavaScript 中使用 `FormData` 時，此名稱會設定為 `battlePlans`的值：</span><span class="sxs-lookup"><span data-stu-id="96a04-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="96a04-304">針對C#方法的參數使用相符的名稱（`battlePlans`）：</span><span class="sxs-lookup"><span data-stu-id="96a04-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="96a04-305">針對名為 `Upload`的 Razor Pages 頁面處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="96a04-306">針對 MVC POST 控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="96a04-307">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="96a04-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="96a04-308">多部分主體長度限制</span><span class="sxs-lookup"><span data-stu-id="96a04-308">Multipart body length limit</span></span>

<span data-ttu-id="96a04-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定每個多部分主體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="96a04-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="96a04-310">超過此限制的表單區段會在剖析時擲回 <xref:System.IO.InvalidDataException>。</span><span class="sxs-lookup"><span data-stu-id="96a04-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="96a04-311">預設值為134217728（128 MB）。</span><span class="sxs-lookup"><span data-stu-id="96a04-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="96a04-312">使用 `Startup.ConfigureServices`中的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定自訂限制：</span><span class="sxs-lookup"><span data-stu-id="96a04-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="96a04-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> 可用來設定單一頁面或動作的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>。</span><span class="sxs-lookup"><span data-stu-id="96a04-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="96a04-314">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="96a04-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="96a04-315">在 Razor Pages 應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="96a04-316">Kestrel 要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="96a04-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="96a04-317">針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="96a04-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="96a04-318">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：</span><span class="sxs-lookup"><span data-stu-id="96a04-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="96a04-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 可用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。</span><span class="sxs-lookup"><span data-stu-id="96a04-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="96a04-320">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="96a04-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="96a04-321">在 Razor pages 應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="96a04-322">`RequestSizeLimitAttribute` 也可以使用[@attribute](xref:mvc/views/razor#attribute) Razor 指示詞來套用：</span><span class="sxs-lookup"><span data-stu-id="96a04-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="96a04-323">其他 Kestrel 限制</span><span class="sxs-lookup"><span data-stu-id="96a04-323">Other Kestrel limits</span></span>

<span data-ttu-id="96a04-324">其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="96a04-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="96a04-325">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="96a04-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="96a04-326">要求和回應資料速率</span><span class="sxs-lookup"><span data-stu-id="96a04-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="96a04-327">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="96a04-327">IIS content length limit</span></span>

<span data-ttu-id="96a04-328">預設要求限制（`maxAllowedContentLength`）是30000000個位元組，大約是 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="96a04-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="96a04-329">自訂*web.config*檔案中的限制：</span><span class="sxs-lookup"><span data-stu-id="96a04-329">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="96a04-330">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="96a04-330">This setting only applies to IIS.</span></span> <span data-ttu-id="96a04-331">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="96a04-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="96a04-332">如需詳細資訊，請參閱[\<requestLimits > 的要求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="96a04-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="96a04-333">ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。</span><span class="sxs-lookup"><span data-stu-id="96a04-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="96a04-334">如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（aspnet/AspNetCore #2711）](https://github.com/aspnet/AspNetCore/issues/2711)。</span><span class="sxs-lookup"><span data-stu-id="96a04-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="96a04-335">疑難排解</span><span class="sxs-lookup"><span data-stu-id="96a04-335">Troubleshoot</span></span>

<span data-ttu-id="96a04-336">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="96a04-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="96a04-337">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="96a04-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="96a04-338">下列錯誤表示上傳的檔案超過伺服器設定的內容長度：</span><span class="sxs-lookup"><span data-stu-id="96a04-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="96a04-339">如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="96a04-340">連接失敗</span><span class="sxs-lookup"><span data-stu-id="96a04-340">Connection failure</span></span>

<span data-ttu-id="96a04-341">連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。</span><span class="sxs-lookup"><span data-stu-id="96a04-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="96a04-342">如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="96a04-343">Kestrel 用戶端連接限制也可能需要調整。</span><span class="sxs-lookup"><span data-stu-id="96a04-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="96a04-344">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="96a04-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="96a04-345">如果控制器使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 來接受上傳檔案，但 `null`值，請確認 HTML 表單是否指定 `multipart/form-data`的 `enctype` 值。</span><span class="sxs-lookup"><span data-stu-id="96a04-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="96a04-346">如果未在 `<form>` 元素上設定這個屬性，就不會進行檔案上傳，而且會 `null`任何系結 <xref:Microsoft.AspNetCore.Http.IFormFile> 引數。</span><span class="sxs-lookup"><span data-stu-id="96a04-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="96a04-347">此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="96a04-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="96a04-348">ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。</span><span class="sxs-lookup"><span data-stu-id="96a04-348">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="96a04-349">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96a04-349">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="96a04-350">安全性考量</span><span class="sxs-lookup"><span data-stu-id="96a04-350">Security considerations</span></span>

<span data-ttu-id="96a04-351">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="96a04-351">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="96a04-352">攻擊者可能會嘗試：</span><span class="sxs-lookup"><span data-stu-id="96a04-352">Attackers may attempt to:</span></span>

* <span data-ttu-id="96a04-353">執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="96a04-353">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="96a04-354">上傳病毒或惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-354">Upload viruses or malware.</span></span>
* <span data-ttu-id="96a04-355">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="96a04-355">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="96a04-356">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="96a04-356">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="96a04-357">將檔案上傳到專用的檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="96a04-357">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="96a04-358">專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。</span><span class="sxs-lookup"><span data-stu-id="96a04-358">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="96a04-359">停用檔案上傳位置的 execute 許可權。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-359">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="96a04-360">請勿將上傳的**檔案保存在**與應用程式相同的目錄樹狀結構中。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-360">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="96a04-361">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-361">Use a safe file name determined by the app.</span></span> <span data-ttu-id="96a04-362">請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。&dagger; HTML 在顯示不受信任的檔案名時進行編碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-362">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="96a04-363">例如，記錄檔案名或在 UI 中顯示（Razor 會自動以 HTML 編碼輸出）。</span><span class="sxs-lookup"><span data-stu-id="96a04-363">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="96a04-364">僅允許應用程式設計規格的已核准副檔名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-364">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="96a04-365">確認用戶端檢查是在伺服器上執行。&dagger; 用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="96a04-365">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="96a04-366">檢查上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-366">Check the size of an uploaded file.</span></span> <span data-ttu-id="96a04-367">設定最大大小限制以防止大量上傳。&dagger;</span><span class="sxs-lookup"><span data-stu-id="96a04-367">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="96a04-368">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-368">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="96a04-369">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="96a04-369">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="96a04-370">&dagger;範例應用程式示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="96a04-370">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-371">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="96a04-371">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="96a04-372">完全取得系統的控制權。</span><span class="sxs-lookup"><span data-stu-id="96a04-372">Completely gain control of a system.</span></span>
> * <span data-ttu-id="96a04-373">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="96a04-373">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="96a04-374">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="96a04-374">Compromise user or system data.</span></span>
> * <span data-ttu-id="96a04-375">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="96a04-375">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="96a04-376">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="96a04-376">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="96a04-377">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="96a04-377">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="96a04-378">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="96a04-378">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="96a04-379">如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-379">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="96a04-380">儲存案例</span><span class="sxs-lookup"><span data-stu-id="96a04-380">Storage scenarios</span></span>

<span data-ttu-id="96a04-381">檔案的一般儲存選項包括：</span><span class="sxs-lookup"><span data-stu-id="96a04-381">Common storage options for files include:</span></span>

* <span data-ttu-id="96a04-382">資料庫</span><span class="sxs-lookup"><span data-stu-id="96a04-382">Database</span></span>

  * <span data-ttu-id="96a04-383">針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。</span><span class="sxs-lookup"><span data-stu-id="96a04-383">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="96a04-384">資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。</span><span class="sxs-lookup"><span data-stu-id="96a04-384">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="96a04-385">資料庫的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="96a04-385">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="96a04-386">實體存放裝置（檔案系統或網路共用）</span><span class="sxs-lookup"><span data-stu-id="96a04-386">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="96a04-387">針對大型檔案上傳：</span><span class="sxs-lookup"><span data-stu-id="96a04-387">For large file uploads:</span></span>
    * <span data-ttu-id="96a04-388">資料庫限制可能會限制上傳的大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-388">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="96a04-389">實體儲存體通常比在資料庫中儲存的成本低。</span><span class="sxs-lookup"><span data-stu-id="96a04-389">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="96a04-390">實體儲存體的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="96a04-390">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="96a04-391">應用程式的進程必須具有儲存位置的讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="96a04-391">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="96a04-392">**絕對不要授與 execute 許可權。**</span><span class="sxs-lookup"><span data-stu-id="96a04-392">**Never grant execute permission.**</span></span>

* <span data-ttu-id="96a04-393">資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）</span><span class="sxs-lookup"><span data-stu-id="96a04-393">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="96a04-394">服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。</span><span class="sxs-lookup"><span data-stu-id="96a04-394">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="96a04-395">在大型儲存體基礎結構案例中，服務可能會降低成本。</span><span class="sxs-lookup"><span data-stu-id="96a04-395">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="96a04-396">如需詳細資訊，請參閱[快速入門：使用 .net 在物件儲存體中建立 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="96a04-396">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="96a04-397">本主題示範 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>，但 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> 可以在使用 <xref:System.IO.Stream>時，用來將 <xref:System.IO.FileStream> 儲存至 blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="96a04-397">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="96a04-398">檔案上傳案例</span><span class="sxs-lookup"><span data-stu-id="96a04-398">File upload scenarios</span></span>

<span data-ttu-id="96a04-399">上傳檔案的兩個一般方法是緩衝和串流處理。</span><span class="sxs-lookup"><span data-stu-id="96a04-399">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="96a04-400">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="96a04-400">**Buffering**</span></span>

<span data-ttu-id="96a04-401">系統會將整個檔案讀入 <xref:Microsoft.AspNetCore.Http.IFormFile>，這是用來C#處理或儲存檔案的檔案標記法。</span><span class="sxs-lookup"><span data-stu-id="96a04-401">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="96a04-402">檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-402">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="96a04-403">如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。</span><span class="sxs-lookup"><span data-stu-id="96a04-403">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="96a04-404">如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。</span><span class="sxs-lookup"><span data-stu-id="96a04-404">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="96a04-405">任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-405">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="96a04-406">此主題的下列各節會涵蓋緩衝處理小型檔案：</span><span class="sxs-lookup"><span data-stu-id="96a04-406">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="96a04-407">實體儲存體</span><span class="sxs-lookup"><span data-stu-id="96a04-407">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="96a04-408">資料庫</span><span class="sxs-lookup"><span data-stu-id="96a04-408">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="96a04-409">**資料流**</span><span class="sxs-lookup"><span data-stu-id="96a04-409">**Streaming**</span></span>

<span data-ttu-id="96a04-410">此檔案會從多部分要求接收，並由應用程式直接處理或儲存。</span><span class="sxs-lookup"><span data-stu-id="96a04-410">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="96a04-411">串流不會大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="96a04-411">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="96a04-412">串流處理可減少上傳檔案時記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="96a04-412">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="96a04-413">[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-413">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="96a04-414">將具有緩衝模型系結的小型檔案上傳至實體儲存體</span><span class="sxs-lookup"><span data-stu-id="96a04-414">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="96a04-415">若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="96a04-415">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="96a04-416">下列範例示範如何使用 Razor Pages 表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：</span><span class="sxs-lookup"><span data-stu-id="96a04-416">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="96a04-417">下列範例類似于先前的範例，不同之處在于：</span><span class="sxs-lookup"><span data-stu-id="96a04-417">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="96a04-418">JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="96a04-418">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="96a04-419">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="96a04-419">There's no validation.</span></span>

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

<span data-ttu-id="96a04-420">若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-420">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="96a04-421">使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。</span><span class="sxs-lookup"><span data-stu-id="96a04-421">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="96a04-422">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="96a04-422">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="96a04-423">例如:</span><span class="sxs-lookup"><span data-stu-id="96a04-423">For example:</span></span>

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

<span data-ttu-id="96a04-424">為了支援檔案上傳，HTML 表單必須指定 `multipart/form-data`的編碼類型（`enctype`）。</span><span class="sxs-lookup"><span data-stu-id="96a04-424">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="96a04-425">若要支援上傳多個檔案的 `files` input 元素，請在 `<input>` 元素上提供 `multiple` 屬性：</span><span class="sxs-lookup"><span data-stu-id="96a04-425">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="96a04-426">您可以使用 <xref:Microsoft.AspNetCore.Http.IFormFile>，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-426">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="96a04-427">範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="96a04-427">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="96a04-428">除了顯示和記錄之外，**請勿使用 <xref:Microsoft.AspNetCore.Http.IFormFile>** 的 `FileName` 屬性。</span><span class="sxs-lookup"><span data-stu-id="96a04-428">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="96a04-429">當顯示或記錄時，HTML 會將檔案名編碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-429">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="96a04-430">攻擊者可以提供惡意的檔案名，包括完整路徑或相對路徑。</span><span class="sxs-lookup"><span data-stu-id="96a04-430">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="96a04-431">應用程式應該：</span><span class="sxs-lookup"><span data-stu-id="96a04-431">Applications should:</span></span>
>
> * <span data-ttu-id="96a04-432">從使用者提供的檔案名中移除路徑。</span><span class="sxs-lookup"><span data-stu-id="96a04-432">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="96a04-433">針對 UI 或記錄儲存以 HTML 編碼、路徑移除的檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-433">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="96a04-434">為儲存區產生新的隨機檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-434">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="96a04-435">下列程式碼會從檔案名中移除路徑：</span><span class="sxs-lookup"><span data-stu-id="96a04-435">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="96a04-436">到目前為止所提供的範例並不考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="96a04-436">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="96a04-437">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="96a04-437">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="96a04-438">安全性考量</span><span class="sxs-lookup"><span data-stu-id="96a04-438">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="96a04-439">驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-439">Validation</span></span>](#validation)

<span data-ttu-id="96a04-440">使用模型系結和 <xref:Microsoft.AspNetCore.Http.IFormFile>上傳檔案時，動作方法可以接受：</span><span class="sxs-lookup"><span data-stu-id="96a04-440">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="96a04-441">單一 <xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="96a04-441">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="96a04-442">下列任何代表數個檔案的集合：</span><span class="sxs-lookup"><span data-stu-id="96a04-442">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="96a04-443">[列出](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="96a04-443">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="96a04-444">系結符合依名稱的表單檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-444">Binding matches form files by name.</span></span> <span data-ttu-id="96a04-445">例如，`<input type="file" name="formFile">` 中的 HTML `name` 值必須符合C#參數/屬性系結（`FormFile`）。</span><span class="sxs-lookup"><span data-stu-id="96a04-445">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="96a04-446">如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-446">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="96a04-447">下列範例：</span><span class="sxs-lookup"><span data-stu-id="96a04-447">The following example:</span></span>

* <span data-ttu-id="96a04-448">迴圈一或多個已上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-448">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="96a04-449">會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-449">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="96a04-450">使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。</span><span class="sxs-lookup"><span data-stu-id="96a04-450">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="96a04-451">傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-451">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="96a04-452">使用 `Path.GetRandomFileName` 產生不含路徑的檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-452">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="96a04-453">在下列範例中，路徑是從設定取得：</span><span class="sxs-lookup"><span data-stu-id="96a04-453">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="96a04-454">傳遞至 <xref:System.IO.FileStream> 的路徑*必須*包含檔案名。</span><span class="sxs-lookup"><span data-stu-id="96a04-454">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="96a04-455">如果未提供檔案名，則會在執行時間擲回 <xref:System.UnauthorizedAccessException>。</span><span class="sxs-lookup"><span data-stu-id="96a04-455">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="96a04-456">使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 技術所上傳的檔案會在處理之前，先緩衝到伺服器上的記憶體或磁片上。</span><span class="sxs-lookup"><span data-stu-id="96a04-456">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="96a04-457">在動作方法內，<xref:Microsoft.AspNetCore.Http.IFormFile> 內容可以 <xref:System.IO.Stream>存取。</span><span class="sxs-lookup"><span data-stu-id="96a04-457">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="96a04-458">除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。</span><span class="sxs-lookup"><span data-stu-id="96a04-458">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="96a04-459">如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。</span><span class="sxs-lookup"><span data-stu-id="96a04-459">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-460">如果建立的檔案超過65535，而未刪除先前的暫存檔案，則[GetTempFileName](xref:System.IO.Path.GetTempFileName*)會擲回 <xref:System.IO.IOException>。</span><span class="sxs-lookup"><span data-stu-id="96a04-460">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="96a04-461">65535檔案的限制是每一伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="96a04-461">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="96a04-462">如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：</span><span class="sxs-lookup"><span data-stu-id="96a04-462">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="96a04-463">GetTempFileNameA 函式</span><span class="sxs-lookup"><span data-stu-id="96a04-463">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="96a04-464">將具有緩衝模型系結的小型檔案上傳至資料庫</span><span class="sxs-lookup"><span data-stu-id="96a04-464">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="96a04-465">若要使用[Entity Framework](/ef/core/index)在資料庫中儲存二進位檔案資料，請在實體上定義 <xref:System.Byte> 陣列屬性：</span><span class="sxs-lookup"><span data-stu-id="96a04-465">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="96a04-466">指定包含 <xref:Microsoft.AspNetCore.Http.IFormFile>之類別的 [頁面模型] 屬性：</span><span class="sxs-lookup"><span data-stu-id="96a04-466">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="96a04-467"><xref:Microsoft.AspNetCore.Http.IFormFile> 可以直接當做動作方法參數或系結模型屬性來使用。</span><span class="sxs-lookup"><span data-stu-id="96a04-467"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="96a04-468">先前的範例會使用系結模型屬性。</span><span class="sxs-lookup"><span data-stu-id="96a04-468">The prior example uses a bound model property.</span></span>

<span data-ttu-id="96a04-469">`FileUpload` 用於 Razor Pages 形式：</span><span class="sxs-lookup"><span data-stu-id="96a04-469">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="96a04-470">當表單張貼至伺服器時，請將 <xref:Microsoft.AspNetCore.Http.IFormFile> 複製到資料流程，並將其儲存為資料庫中的位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="96a04-470">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="96a04-471">在下列範例中，`_dbContext` 會儲存應用程式的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="96a04-471">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="96a04-472">前述範例類似于範例應用程式中所示範的案例：</span><span class="sxs-lookup"><span data-stu-id="96a04-472">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="96a04-473">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="96a04-473">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="96a04-474">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="96a04-474">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-475">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="96a04-475">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="96a04-476">請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 `FileName` 屬性，而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="96a04-476">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="96a04-477">`FileName` 屬性只能用於顯示用途，而且只能用於 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="96a04-477">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="96a04-478">提供的範例不會考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="96a04-478">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="96a04-479">下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="96a04-479">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="96a04-480">安全性考量</span><span class="sxs-lookup"><span data-stu-id="96a04-480">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="96a04-481">驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-481">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="96a04-482">使用串流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="96a04-482">Upload large files with streaming</span></span>

<span data-ttu-id="96a04-483">下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="96a04-483">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="96a04-484">檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="96a04-484">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="96a04-485">由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。</span><span class="sxs-lookup"><span data-stu-id="96a04-485">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="96a04-486">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-486">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="96a04-487">讀取多部分區段之後，動作會執行自己的模型系結。</span><span class="sxs-lookup"><span data-stu-id="96a04-487">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="96a04-488">初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="96a04-488">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="96a04-489">屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：</span><span class="sxs-lookup"><span data-stu-id="96a04-489">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="96a04-490">`DisableFormValueModelBindingAttribute` 可用來停用模型系結：</span><span class="sxs-lookup"><span data-stu-id="96a04-490">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="96a04-491">在範例應用程式中，`GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會當做篩選套用至 `/StreamedSingleFileUploadDb` 的頁面應用程式模型，以及 `Startup.ConfigureServices` 中使用[Razor Pages 慣例](xref:razor-pages/razor-pages-conventions)的 `/StreamedSingleFileUploadPhysical`：</span><span class="sxs-lookup"><span data-stu-id="96a04-491">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="96a04-492">由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。</span><span class="sxs-lookup"><span data-stu-id="96a04-492">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="96a04-493">動作方法會直接與 `Request` 屬性搭配運作。</span><span class="sxs-lookup"><span data-stu-id="96a04-493">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="96a04-494">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="96a04-494">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="96a04-495">索引鍵/值資料會儲存在 `KeyValueAccumulator`中。</span><span class="sxs-lookup"><span data-stu-id="96a04-495">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="96a04-496">讀取多部分區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料系結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="96a04-496">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="96a04-497">使用 EF Core 串流至資料庫的完整 `StreamingController.UploadDatabase` 方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-497">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="96a04-498">`MultipartRequestHelper` （*公用程式/MultipartRequestHelper .cs*）：</span><span class="sxs-lookup"><span data-stu-id="96a04-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="96a04-499">串流至實體位置的完整 `StreamingController.UploadPhysical` 方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-499">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="96a04-500">在範例應用程式中，驗證檢查是由 `FileHelpers.ProcessStreamedFile`處理。</span><span class="sxs-lookup"><span data-stu-id="96a04-500">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="96a04-501">驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-501">Validation</span></span>

<span data-ttu-id="96a04-502">範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理的 <xref:Microsoft.AspNetCore.Http.IFormFile> 和串流檔案上傳的檢查。</span><span class="sxs-lookup"><span data-stu-id="96a04-502">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="96a04-503">若要處理範例應用程式中 <xref:Microsoft.AspNetCore.Http.IFormFile> 緩衝的檔案上傳，請參閱*公用程式/FileHelpers*檔案中的 `ProcessFormFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="96a04-503">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="96a04-504">如需處理資料流程檔案，請參閱相同檔案中的 `ProcessStreamedFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="96a04-504">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="96a04-505">範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-505">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="96a04-506">在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。</span><span class="sxs-lookup"><span data-stu-id="96a04-506">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="96a04-507">雖然主題範例提供驗證技術的實用範例，但請勿在生產應用程式中執行 `FileHelpers` 類別，除非您：</span><span class="sxs-lookup"><span data-stu-id="96a04-507">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="96a04-508">完全瞭解此實作為。</span><span class="sxs-lookup"><span data-stu-id="96a04-508">Fully understand the implementation.</span></span>
> * <span data-ttu-id="96a04-509">針對應用程式的環境和規格修改適當的執行。</span><span class="sxs-lookup"><span data-stu-id="96a04-509">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="96a04-510">**絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**</span><span class="sxs-lookup"><span data-stu-id="96a04-510">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="96a04-511">內容驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-511">Content validation</span></span>

<span data-ttu-id="96a04-512">**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="96a04-512">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="96a04-513">在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。</span><span class="sxs-lookup"><span data-stu-id="96a04-513">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="96a04-514">如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。</span><span class="sxs-lookup"><span data-stu-id="96a04-514">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="96a04-515">一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。</span><span class="sxs-lookup"><span data-stu-id="96a04-515">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="96a04-516">當檔案通過時，檔案就會移至一般檔案儲存位置。</span><span class="sxs-lookup"><span data-stu-id="96a04-516">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="96a04-517">這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。</span><span class="sxs-lookup"><span data-stu-id="96a04-517">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="96a04-518">藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。</span><span class="sxs-lookup"><span data-stu-id="96a04-518">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="96a04-519">副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-519">File extension validation</span></span>

<span data-ttu-id="96a04-520">已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。</span><span class="sxs-lookup"><span data-stu-id="96a04-520">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="96a04-521">例如:</span><span class="sxs-lookup"><span data-stu-id="96a04-521">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="96a04-522">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-522">File signature validation</span></span>

<span data-ttu-id="96a04-523">檔案的簽章取決於檔案開頭的前幾個位元組。</span><span class="sxs-lookup"><span data-stu-id="96a04-523">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="96a04-524">這些位元組可用來指出延伸模組是否符合檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-524">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="96a04-525">範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="96a04-525">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="96a04-526">在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：</span><span class="sxs-lookup"><span data-stu-id="96a04-526">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="96a04-527">若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。</span><span class="sxs-lookup"><span data-stu-id="96a04-527">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="96a04-528">檔案名安全性</span><span class="sxs-lookup"><span data-stu-id="96a04-528">File name security</span></span>

<span data-ttu-id="96a04-529">絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。</span><span class="sxs-lookup"><span data-stu-id="96a04-529">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="96a04-530">使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。</span><span class="sxs-lookup"><span data-stu-id="96a04-530">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="96a04-531">Razor 會自動對屬性值進行 HTML 編碼以供顯示。</span><span class="sxs-lookup"><span data-stu-id="96a04-531">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="96a04-532">以下是可安全使用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="96a04-532">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="96a04-533">在 Razor 以外，一律 <xref:System.Net.WebUtility.HtmlEncode*> 來自使用者要求的檔案名內容。</span><span class="sxs-lookup"><span data-stu-id="96a04-533">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="96a04-534">許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。</span><span class="sxs-lookup"><span data-stu-id="96a04-534">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="96a04-535">提供其他邏輯以符合您應用程式的規格。</span><span class="sxs-lookup"><span data-stu-id="96a04-535">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="96a04-536">大小驗證</span><span class="sxs-lookup"><span data-stu-id="96a04-536">Size validation</span></span>

<span data-ttu-id="96a04-537">限制上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="96a04-537">Limit the size of uploaded files.</span></span>

<span data-ttu-id="96a04-538">在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。</span><span class="sxs-lookup"><span data-stu-id="96a04-538">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="96a04-539">此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的設定來提供：</span><span class="sxs-lookup"><span data-stu-id="96a04-539">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="96a04-540">`FileSizeLimit` 會插入 `PageModel` 類別中：</span><span class="sxs-lookup"><span data-stu-id="96a04-540">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="96a04-541">當檔案大小超過限制時，就會拒絕此檔案：</span><span class="sxs-lookup"><span data-stu-id="96a04-541">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="96a04-542">Match name 屬性值與 POST 方法的參數名稱</span><span class="sxs-lookup"><span data-stu-id="96a04-542">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="96a04-543">在張貼表單資料或直接使用 JavaScript `FormData` 的非 Razor 表單中，在表單的元素或 `FormData` 中指定的名稱必須符合控制器動作中參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="96a04-543">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="96a04-544">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="96a04-544">In the following example:</span></span>

* <span data-ttu-id="96a04-545">使用 `<input>` 元素時，`name` 屬性會設定為 `battlePlans`的值：</span><span class="sxs-lookup"><span data-stu-id="96a04-545">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="96a04-546">在 JavaScript 中使用 `FormData` 時，此名稱會設定為 `battlePlans`的值：</span><span class="sxs-lookup"><span data-stu-id="96a04-546">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="96a04-547">針對C#方法的參數使用相符的名稱（`battlePlans`）：</span><span class="sxs-lookup"><span data-stu-id="96a04-547">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="96a04-548">針對名為 `Upload`的 Razor Pages 頁面處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-548">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="96a04-549">針對 MVC POST 控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-549">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="96a04-550">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="96a04-550">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="96a04-551">多部分主體長度限制</span><span class="sxs-lookup"><span data-stu-id="96a04-551">Multipart body length limit</span></span>

<span data-ttu-id="96a04-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定每個多部分主體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="96a04-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="96a04-553">超過此限制的表單區段會在剖析時擲回 <xref:System.IO.InvalidDataException>。</span><span class="sxs-lookup"><span data-stu-id="96a04-553">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="96a04-554">預設值為134217728（128 MB）。</span><span class="sxs-lookup"><span data-stu-id="96a04-554">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="96a04-555">使用 `Startup.ConfigureServices`中的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定自訂限制：</span><span class="sxs-lookup"><span data-stu-id="96a04-555">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="96a04-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> 可用來設定單一頁面或動作的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>。</span><span class="sxs-lookup"><span data-stu-id="96a04-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="96a04-557">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="96a04-557">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="96a04-558">在 Razor Pages 應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-558">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="96a04-559">Kestrel 要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="96a04-559">Kestrel maximum request body size</span></span>

<span data-ttu-id="96a04-560">針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="96a04-560">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="96a04-561">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：</span><span class="sxs-lookup"><span data-stu-id="96a04-561">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="96a04-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 可用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。</span><span class="sxs-lookup"><span data-stu-id="96a04-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="96a04-563">在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：</span><span class="sxs-lookup"><span data-stu-id="96a04-563">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="96a04-564">在 Razor pages 應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="96a04-564">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="96a04-565">其他 Kestrel 限制</span><span class="sxs-lookup"><span data-stu-id="96a04-565">Other Kestrel limits</span></span>

<span data-ttu-id="96a04-566">其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="96a04-566">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="96a04-567">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="96a04-567">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="96a04-568">要求和回應資料速率</span><span class="sxs-lookup"><span data-stu-id="96a04-568">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="96a04-569">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="96a04-569">IIS content length limit</span></span>

<span data-ttu-id="96a04-570">預設要求限制（`maxAllowedContentLength`）是30000000個位元組，大約是 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="96a04-570">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="96a04-571">自訂*web.config*檔案中的限制：</span><span class="sxs-lookup"><span data-stu-id="96a04-571">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="96a04-572">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="96a04-572">This setting only applies to IIS.</span></span> <span data-ttu-id="96a04-573">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="96a04-573">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="96a04-574">如需詳細資訊，請參閱[\<requestLimits > 的要求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="96a04-574">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="96a04-575">ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。</span><span class="sxs-lookup"><span data-stu-id="96a04-575">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="96a04-576">如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（aspnet/AspNetCore #2711）](https://github.com/aspnet/AspNetCore/issues/2711)。</span><span class="sxs-lookup"><span data-stu-id="96a04-576">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="96a04-577">疑難排解</span><span class="sxs-lookup"><span data-stu-id="96a04-577">Troubleshoot</span></span>

<span data-ttu-id="96a04-578">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="96a04-578">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="96a04-579">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="96a04-579">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="96a04-580">下列錯誤表示上傳的檔案超過伺服器設定的內容長度：</span><span class="sxs-lookup"><span data-stu-id="96a04-580">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="96a04-581">如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-581">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="96a04-582">連接失敗</span><span class="sxs-lookup"><span data-stu-id="96a04-582">Connection failure</span></span>

<span data-ttu-id="96a04-583">連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。</span><span class="sxs-lookup"><span data-stu-id="96a04-583">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="96a04-584">如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。</span><span class="sxs-lookup"><span data-stu-id="96a04-584">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="96a04-585">Kestrel 用戶端連接限制也可能需要調整。</span><span class="sxs-lookup"><span data-stu-id="96a04-585">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="96a04-586">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="96a04-586">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="96a04-587">如果控制器使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 來接受上傳檔案，但 `null`值，請確認 HTML 表單是否指定 `multipart/form-data`的 `enctype` 值。</span><span class="sxs-lookup"><span data-stu-id="96a04-587">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="96a04-588">如果未在 `<form>` 元素上設定這個屬性，就不會進行檔案上傳，而且會 `null`任何系結 <xref:Microsoft.AspNetCore.Http.IFormFile> 引數。</span><span class="sxs-lookup"><span data-stu-id="96a04-588">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="96a04-589">此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="96a04-589">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="96a04-590">其他資源</span><span class="sxs-lookup"><span data-stu-id="96a04-590">Additional resources</span></span>

* [<span data-ttu-id="96a04-591">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="96a04-591">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="96a04-592">Azure 安全性：安全性框架：輸入驗證 |緩和措施</span><span class="sxs-lookup"><span data-stu-id="96a04-592">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="96a04-593">Azure 雲端設計模式： Valet 金鑰模式</span><span class="sxs-lookup"><span data-stu-id="96a04-593">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
