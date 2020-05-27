---
<span data-ttu-id="de32c-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="de32c-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="de32c-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="de32c-102">'Blazor'</span></span>
- <span data-ttu-id="de32c-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="de32c-103">'Identity'</span></span>
- <span data-ttu-id="de32c-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="de32c-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="de32c-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="de32c-105">'Razor'</span></span>
- <span data-ttu-id="de32c-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="de32c-106">'SignalR' uid:</span></span> 

---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="de32c-107">上傳 ASP.NET Core 中的檔案</span><span class="sxs-lookup"><span data-stu-id="de32c-107">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="de32c-108">作者： [Steve Smith](https://ardalis.com/)和[Rutger 風暴](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="de32c-108">By [Steve Smith](https://ardalis.com/) and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="de32c-109">ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。</span><span class="sxs-lookup"><span data-stu-id="de32c-109">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="de32c-110">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="de32c-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="de32c-111">安全性考量</span><span class="sxs-lookup"><span data-stu-id="de32c-111">Security considerations</span></span>

<span data-ttu-id="de32c-112">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="de32c-112">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="de32c-113">攻擊者可能會嘗試：</span><span class="sxs-lookup"><span data-stu-id="de32c-113">Attackers may attempt to:</span></span>

* <span data-ttu-id="de32c-114">執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="de32c-114">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="de32c-115">上傳病毒或惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-115">Upload viruses or malware.</span></span>
* <span data-ttu-id="de32c-116">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="de32c-116">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="de32c-117">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="de32c-117">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="de32c-118">將檔案上傳到專用的檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="de32c-118">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="de32c-119">專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。</span><span class="sxs-lookup"><span data-stu-id="de32c-119">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="de32c-120">停用檔案上傳位置的 execute 許可權。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-120">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="de32c-121">請勿將上傳的**檔案保存在**與應用程式相同的目錄樹狀結構中。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-121">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="de32c-122">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-122">Use a safe file name determined by the app.</span></span> <span data-ttu-id="de32c-123">請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。 &dagger;HTML 會在顯示不受信任的檔案名時進行編碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-123">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="de32c-124">例如，記錄檔案名或在 UI 中顯示（自動以 Razor HTML 編碼輸出）。</span><span class="sxs-lookup"><span data-stu-id="de32c-124">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="de32c-125">僅允許應用程式設計規格的已核准副檔名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-125">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="de32c-126">確認用戶端檢查是在伺服器上執行。 &dagger;用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="de32c-126">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="de32c-127">檢查上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-127">Check the size of an uploaded file.</span></span> <span data-ttu-id="de32c-128">設定最大大小限制以防止大量上傳。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-128">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="de32c-129">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-129">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="de32c-130">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="de32c-130">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="de32c-131">&dagger;範例應用程式會示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="de32c-131">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-132">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="de32c-132">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="de32c-133">完全取得系統的控制權。</span><span class="sxs-lookup"><span data-stu-id="de32c-133">Completely gain control of a system.</span></span>
> * <span data-ttu-id="de32c-134">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="de32c-134">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="de32c-135">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="de32c-135">Compromise user or system data.</span></span>
> * <span data-ttu-id="de32c-136">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="de32c-136">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="de32c-137">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="de32c-137">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="de32c-138">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="de32c-138">Unrestricted File Upload</span></span>](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
> * [<span data-ttu-id="de32c-139">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="de32c-139">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="de32c-140">如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-140">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="de32c-141">儲存案例</span><span class="sxs-lookup"><span data-stu-id="de32c-141">Storage scenarios</span></span>

<span data-ttu-id="de32c-142">檔案的一般儲存選項包括：</span><span class="sxs-lookup"><span data-stu-id="de32c-142">Common storage options for files include:</span></span>

* <span data-ttu-id="de32c-143">資料庫</span><span class="sxs-lookup"><span data-stu-id="de32c-143">Database</span></span>

  * <span data-ttu-id="de32c-144">針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。</span><span class="sxs-lookup"><span data-stu-id="de32c-144">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="de32c-145">資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。</span><span class="sxs-lookup"><span data-stu-id="de32c-145">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="de32c-146">資料庫的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="de32c-146">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="de32c-147">實體存放裝置（檔案系統或網路共用）</span><span class="sxs-lookup"><span data-stu-id="de32c-147">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="de32c-148">針對大型檔案上傳：</span><span class="sxs-lookup"><span data-stu-id="de32c-148">For large file uploads:</span></span>
    * <span data-ttu-id="de32c-149">資料庫限制可能會限制上傳的大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-149">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="de32c-150">實體儲存體通常比在資料庫中儲存的成本低。</span><span class="sxs-lookup"><span data-stu-id="de32c-150">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="de32c-151">實體儲存體的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="de32c-151">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="de32c-152">應用程式的進程必須具有儲存位置的讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="de32c-152">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="de32c-153">**絕對不要授與 execute 許可權。**</span><span class="sxs-lookup"><span data-stu-id="de32c-153">**Never grant execute permission.**</span></span>

* <span data-ttu-id="de32c-154">資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）</span><span class="sxs-lookup"><span data-stu-id="de32c-154">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="de32c-155">服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。</span><span class="sxs-lookup"><span data-stu-id="de32c-155">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="de32c-156">在大型儲存體基礎結構案例中，服務可能會降低成本。</span><span class="sxs-lookup"><span data-stu-id="de32c-156">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="de32c-157">如需詳細資訊，請參閱[快速入門：使用 .net 在物件儲存體中建立 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="de32c-157">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="de32c-158">檔案上傳案例</span><span class="sxs-lookup"><span data-stu-id="de32c-158">File upload scenarios</span></span>

<span data-ttu-id="de32c-159">上傳檔案的兩個一般方法是緩衝和串流處理。</span><span class="sxs-lookup"><span data-stu-id="de32c-159">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="de32c-160">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="de32c-160">**Buffering**</span></span>

<span data-ttu-id="de32c-161">整個檔案會讀入 <xref:Microsoft.AspNetCore.Http.IFormFile> ，這是用來處理或儲存檔案之檔案的 c # 標記法。</span><span class="sxs-lookup"><span data-stu-id="de32c-161">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="de32c-162">檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-162">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="de32c-163">如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。</span><span class="sxs-lookup"><span data-stu-id="de32c-163">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="de32c-164">如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。</span><span class="sxs-lookup"><span data-stu-id="de32c-164">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="de32c-165">任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-165">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="de32c-166">此主題的下列各節會涵蓋緩衝處理小型檔案：</span><span class="sxs-lookup"><span data-stu-id="de32c-166">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="de32c-167">實體儲存體</span><span class="sxs-lookup"><span data-stu-id="de32c-167">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="de32c-168">Database</span><span class="sxs-lookup"><span data-stu-id="de32c-168">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="de32c-169">**串流**</span><span class="sxs-lookup"><span data-stu-id="de32c-169">**Streaming**</span></span>

<span data-ttu-id="de32c-170">此檔案會從多部分要求接收，並由應用程式直接處理或儲存。</span><span class="sxs-lookup"><span data-stu-id="de32c-170">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="de32c-171">串流不會大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="de32c-171">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="de32c-172">串流處理可減少上傳檔案時記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="de32c-172">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="de32c-173">[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-173">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="de32c-174">將具有緩衝模型系結的小型檔案上傳至實體儲存體</span><span class="sxs-lookup"><span data-stu-id="de32c-174">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="de32c-175">若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="de32c-175">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="de32c-176">下列範例示範 Razor 如何使用頁面表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：</span><span class="sxs-lookup"><span data-stu-id="de32c-176">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="de32c-177">下列範例類似于先前的範例，不同之處在于：</span><span class="sxs-lookup"><span data-stu-id="de32c-177">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="de32c-178">JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="de32c-178">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="de32c-179">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="de32c-179">There's no validation.</span></span>

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

<span data-ttu-id="de32c-180">若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-180">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="de32c-181">使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。</span><span class="sxs-lookup"><span data-stu-id="de32c-181">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="de32c-182">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="de32c-182">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="de32c-183">例如：</span><span class="sxs-lookup"><span data-stu-id="de32c-183">For example:</span></span>

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

<span data-ttu-id="de32c-184">為了支援檔案上傳，HTML 表單必須指定的編碼類型（ `enctype` ） `multipart/form-data` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-184">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="de32c-185">`files`若要讓輸入元素支援上傳多個檔案，請在 `multiple` 元素上提供屬性 `<input>` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-185">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="de32c-186">您可以使用，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-186">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="de32c-187">範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="de32c-187">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="de32c-188">**not** `FileName` 除了顯示和記錄之外，請勿使用以外的屬性 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-188">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="de32c-189">當顯示或記錄時，HTML 會將檔案名編碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-189">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="de32c-190">攻擊者可以提供惡意的檔案名，包括完整路徑或相對路徑。</span><span class="sxs-lookup"><span data-stu-id="de32c-190">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="de32c-191">應用程式應該：</span><span class="sxs-lookup"><span data-stu-id="de32c-191">Applications should:</span></span>
>
> * <span data-ttu-id="de32c-192">從使用者提供的檔案名中移除路徑。</span><span class="sxs-lookup"><span data-stu-id="de32c-192">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="de32c-193">針對 UI 或記錄儲存以 HTML 編碼、路徑移除的檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-193">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="de32c-194">為儲存區產生新的隨機檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-194">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="de32c-195">下列程式碼會從檔案名中移除路徑：</span><span class="sxs-lookup"><span data-stu-id="de32c-195">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="de32c-196">到目前為止所提供的範例並不考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="de32c-196">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="de32c-197">下列各節和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="de32c-197">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de32c-198">安全性考量</span><span class="sxs-lookup"><span data-stu-id="de32c-198">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de32c-199">驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-199">Validation</span></span>](#validation)

<span data-ttu-id="de32c-200">使用模型系結和來上傳檔案時 <xref:Microsoft.AspNetCore.Http.IFormFile> ，動作方法可以接受：</span><span class="sxs-lookup"><span data-stu-id="de32c-200">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="de32c-201">單一 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-201">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="de32c-202">下列任何代表數個檔案的集合：</span><span class="sxs-lookup"><span data-stu-id="de32c-202">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="de32c-203">[名單](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="de32c-203">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="de32c-204">系結符合依名稱的表單檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-204">Binding matches form files by name.</span></span> <span data-ttu-id="de32c-205">例如，中的 HTML `name` 值 `<input type="file" name="formFile">` 必須符合 c # 參數/屬性系結（ `FormFile` ）。</span><span class="sxs-lookup"><span data-stu-id="de32c-205">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="de32c-206">如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-206">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="de32c-207">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="de32c-207">The following example:</span></span>

* <span data-ttu-id="de32c-208">迴圈一或多個已上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-208">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="de32c-209">會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-209">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="de32c-210">使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。</span><span class="sxs-lookup"><span data-stu-id="de32c-210">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="de32c-211">傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-211">Returns the total number and size of files uploaded.</span></span>

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

    return Ok(new { count = files.Count, size });
}
```

<span data-ttu-id="de32c-212">使用 `Path.GetRandomFileName` 來產生不含路徑的檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-212">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="de32c-213">在下列範例中，路徑是從設定取得：</span><span class="sxs-lookup"><span data-stu-id="de32c-213">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="de32c-214">傳遞至的路徑 <xref:System.IO.FileStream> *必須*包含檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-214">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="de32c-215">如果未提供檔案名， <xref:System.UnauthorizedAccessException> 則會在執行時間擲回。</span><span class="sxs-lookup"><span data-stu-id="de32c-215">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="de32c-216">使用此技術所上傳的檔案 <xref:Microsoft.AspNetCore.Http.IFormFile> ，會在處理之前，先緩衝到伺服器上的記憶體或磁片上。</span><span class="sxs-lookup"><span data-stu-id="de32c-216">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="de32c-217">在動作方法內， <xref:Microsoft.AspNetCore.Http.IFormFile> 內容可當做來存取 <xref:System.IO.Stream> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-217">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="de32c-218">除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。</span><span class="sxs-lookup"><span data-stu-id="de32c-218">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="de32c-219">如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。</span><span class="sxs-lookup"><span data-stu-id="de32c-219">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-220">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) <xref:System.IO.IOException> 如果建立超過65535個檔案，而不刪除先前的暫存檔案，則 GetTempFileName 會擲回。</span><span class="sxs-lookup"><span data-stu-id="de32c-220">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="de32c-221">65535檔案的限制是每一伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="de32c-221">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="de32c-222">如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：</span><span class="sxs-lookup"><span data-stu-id="de32c-222">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="de32c-223">GetTempFileNameA 函式</span><span class="sxs-lookup"><span data-stu-id="de32c-223">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="de32c-224">將具有緩衝模型系結的小型檔案上傳至資料庫</span><span class="sxs-lookup"><span data-stu-id="de32c-224">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="de32c-225">若要使用[Entity Framework](/ef/core/index)將二進位檔案資料儲存在資料庫中，請 <xref:System.Byte> 在實體上定義陣列屬性：</span><span class="sxs-lookup"><span data-stu-id="de32c-225">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="de32c-226">為包含下列內容的類別指定頁面模型屬性 <xref:Microsoft.AspNetCore.Http.IFormFile> ：</span><span class="sxs-lookup"><span data-stu-id="de32c-226">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="de32c-227"><xref:Microsoft.AspNetCore.Http.IFormFile>可以直接當做動作方法參數或系結模型屬性來使用。</span><span class="sxs-lookup"><span data-stu-id="de32c-227"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="de32c-228">先前的範例會使用系結模型屬性。</span><span class="sxs-lookup"><span data-stu-id="de32c-228">The prior example uses a bound model property.</span></span>

<span data-ttu-id="de32c-229">在 `FileUpload` [ Razor 頁面] 表單中使用：</span><span class="sxs-lookup"><span data-stu-id="de32c-229">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="de32c-230">當表單張貼至伺服器時，請將複製 <xref:Microsoft.AspNetCore.Http.IFormFile> 到資料流程，並將它儲存為資料庫中的位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="de32c-230">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="de32c-231">在下列範例中，會 `_dbContext` 儲存應用程式的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="de32c-231">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="de32c-232">前述範例類似于範例應用程式中所示範的案例：</span><span class="sxs-lookup"><span data-stu-id="de32c-232">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="de32c-233">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="de32c-233">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="de32c-234">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="de32c-234">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-235">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="de32c-235">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="de32c-236">請勿依賴或信任的 `FileName` 屬性， <xref:Microsoft.AspNetCore.Http.IFormFile> 而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="de32c-236">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="de32c-237">`FileName`屬性只應用於顯示用途，而且只能用於 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-237">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="de32c-238">提供的範例不會考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="de32c-238">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="de32c-239">下列各節和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="de32c-239">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de32c-240">安全性考量</span><span class="sxs-lookup"><span data-stu-id="de32c-240">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de32c-241">驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-241">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="de32c-242">使用串流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="de32c-242">Upload large files with streaming</span></span>

<span data-ttu-id="de32c-243">下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="de32c-243">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="de32c-244">檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="de32c-244">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="de32c-245">由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。</span><span class="sxs-lookup"><span data-stu-id="de32c-245">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="de32c-246">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-246">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="de32c-247">讀取多部分區段之後，動作會執行自己的模型系結。</span><span class="sxs-lookup"><span data-stu-id="de32c-247">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="de32c-248">初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="de32c-248">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="de32c-249">屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：</span><span class="sxs-lookup"><span data-stu-id="de32c-249">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="de32c-250">`DisableFormValueModelBindingAttribute`是用來停用模型系結：</span><span class="sxs-lookup"><span data-stu-id="de32c-250">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="de32c-251">在範例應用程式中， `GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會 `/StreamedSingleFileUploadDb` `/StreamedSingleFileUploadPhysical` `Startup.ConfigureServices` 使用[ Razor 頁面慣例](xref:razor-pages/razor-pages-conventions)，套用為和頁面應用程式模型的篩選：</span><span class="sxs-lookup"><span data-stu-id="de32c-251">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="de32c-252">由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。</span><span class="sxs-lookup"><span data-stu-id="de32c-252">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="de32c-253">動作方法會直接與屬性搭配運作 `Request` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-253">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="de32c-254">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="de32c-254">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="de32c-255">索引鍵/值資料會儲存在中 `KeyValueAccumulator` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-255">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="de32c-256">讀取多部分區段之後，會使用的內容將 `KeyValueAccumulator` 表單資料系結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="de32c-256">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="de32c-257">`StreamingController.UploadDatabase`使用 EF Core 串流至資料庫的完整方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-257">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="de32c-258">`MultipartRequestHelper`（*公用程式/MultipartRequestHelper .cs*）：</span><span class="sxs-lookup"><span data-stu-id="de32c-258">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="de32c-259">`StreamingController.UploadPhysical`串流至實體位置的完整方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-259">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="de32c-260">在範例應用程式中，驗證檢查是由所處理 `FileHelpers.ProcessStreamedFile` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-260">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="de32c-261">驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-261">Validation</span></span>

<span data-ttu-id="de32c-262">範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理和串流處理檔案上傳的檢查 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-262">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="de32c-263">如需 <xref:Microsoft.AspNetCore.Http.IFormFile> 在範例應用程式中處理緩衝的檔案上傳，請參閱 `ProcessFormFile` *公用程式/FileHelpers .cs*檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="de32c-263">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="de32c-264">如需處理資料流程檔案，請參閱 `ProcessStreamedFile` 相同檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="de32c-264">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-265">範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-265">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="de32c-266">在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-266">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="de32c-267">雖然主題範例提供驗證技術的實用範例，但請勿 `FileHelpers` 在生產應用程式中執行類別，除非您：</span><span class="sxs-lookup"><span data-stu-id="de32c-267">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="de32c-268">完全瞭解此實作為。</span><span class="sxs-lookup"><span data-stu-id="de32c-268">Fully understand the implementation.</span></span>
> * <span data-ttu-id="de32c-269">針對應用程式的環境和規格修改適當的執行。</span><span class="sxs-lookup"><span data-stu-id="de32c-269">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="de32c-270">**絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**</span><span class="sxs-lookup"><span data-stu-id="de32c-270">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="de32c-271">內容驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-271">Content validation</span></span>

<span data-ttu-id="de32c-272">**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="de32c-272">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="de32c-273">在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。</span><span class="sxs-lookup"><span data-stu-id="de32c-273">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="de32c-274">如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。</span><span class="sxs-lookup"><span data-stu-id="de32c-274">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="de32c-275">一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。</span><span class="sxs-lookup"><span data-stu-id="de32c-275">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="de32c-276">當檔案通過時，檔案就會移至一般檔案儲存位置。</span><span class="sxs-lookup"><span data-stu-id="de32c-276">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="de32c-277">這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。</span><span class="sxs-lookup"><span data-stu-id="de32c-277">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="de32c-278">藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。</span><span class="sxs-lookup"><span data-stu-id="de32c-278">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="de32c-279">副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-279">File extension validation</span></span>

<span data-ttu-id="de32c-280">已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。</span><span class="sxs-lookup"><span data-stu-id="de32c-280">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="de32c-281">例如：</span><span class="sxs-lookup"><span data-stu-id="de32c-281">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="de32c-282">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-282">File signature validation</span></span>

<span data-ttu-id="de32c-283">檔案的簽章取決於檔案開頭的前幾個位元組。</span><span class="sxs-lookup"><span data-stu-id="de32c-283">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="de32c-284">這些位元組可用來指出延伸模組是否符合檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-284">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="de32c-285">範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="de32c-285">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="de32c-286">在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：</span><span class="sxs-lookup"><span data-stu-id="de32c-286">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="de32c-287">若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。</span><span class="sxs-lookup"><span data-stu-id="de32c-287">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="de32c-288">檔案名安全性</span><span class="sxs-lookup"><span data-stu-id="de32c-288">File name security</span></span>

<span data-ttu-id="de32c-289">絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。</span><span class="sxs-lookup"><span data-stu-id="de32c-289">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="de32c-290">使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。</span><span class="sxs-lookup"><span data-stu-id="de32c-290">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

Razor<span data-ttu-id="de32c-291">自動對屬性值進行 HTML 編碼以供顯示。</span><span class="sxs-lookup"><span data-stu-id="de32c-291"> automatically HTML encodes property values for display.</span></span> <span data-ttu-id="de32c-292">以下是可安全使用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="de32c-292">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="de32c-293">在之外 Razor ，一律 <xref:System.Net.WebUtility.HtmlEncode*> 是來自使用者要求的檔案名內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-293">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="de32c-294">許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。</span><span class="sxs-lookup"><span data-stu-id="de32c-294">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="de32c-295">提供其他邏輯以符合您應用程式的規格。</span><span class="sxs-lookup"><span data-stu-id="de32c-295">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="de32c-296">大小驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-296">Size validation</span></span>

<span data-ttu-id="de32c-297">限制上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-297">Limit the size of uploaded files.</span></span>

<span data-ttu-id="de32c-298">在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。</span><span class="sxs-lookup"><span data-stu-id="de32c-298">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="de32c-299">此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的*appsettings.json*設定來提供：</span><span class="sxs-lookup"><span data-stu-id="de32c-299">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="de32c-300">`FileSizeLimit`會插入類別中 `PageModel` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-300">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="de32c-301">當檔案大小超過限制時，就會拒絕此檔案：</span><span class="sxs-lookup"><span data-stu-id="de32c-301">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="de32c-302">Match name 屬性值與 POST 方法的參數名稱</span><span class="sxs-lookup"><span data-stu-id="de32c-302">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="de32c-303">在 Razor 張貼表單資料或直接使用 JavaScript 的非表單中 `FormData` ，在表單的元素中指定的名稱或 `FormData` 必須符合控制器動作中參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="de32c-303">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="de32c-304">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="de32c-304">In the following example:</span></span>

* <span data-ttu-id="de32c-305">使用元素時 `<input>` ， `name` 屬性會設定為值 `battlePlans` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-305">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="de32c-306">`FormData`在 JavaScript 中使用時，名稱會設定為值 `battlePlans` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-306">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="de32c-307">針對 c # 方法的參數使用相符的名稱（ `battlePlans` ）：</span><span class="sxs-lookup"><span data-stu-id="de32c-307">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="de32c-308">針對名為的 Razor 頁面頁面處理常式方法 `Upload` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-308">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="de32c-309">針對 MVC POST 控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-309">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="de32c-310">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="de32c-310">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="de32c-311">多部分主體長度限制</span><span class="sxs-lookup"><span data-stu-id="de32c-311">Multipart body length limit</span></span>

<span data-ttu-id="de32c-312"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>設定每個多部分主體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="de32c-312"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="de32c-313">超過此限制的表單區段會 <xref:System.IO.InvalidDataException> 在剖析時擲回。</span><span class="sxs-lookup"><span data-stu-id="de32c-313">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="de32c-314">預設值為134217728（128 MB）。</span><span class="sxs-lookup"><span data-stu-id="de32c-314">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="de32c-315">使用中的設定來自訂限制 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-315">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de32c-316"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>是用來設定 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 單一頁面或動作的。</span><span class="sxs-lookup"><span data-stu-id="de32c-316"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="de32c-317">在 Razor 頁面應用程式中，將篩選套用至中的[慣例](xref:razor-pages/razor-pages-conventions) `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-317">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de32c-318">在 Razor 頁面應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-318">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="de32c-319">Kestrel 要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="de32c-319">Kestrel maximum request body size</span></span>

<span data-ttu-id="de32c-320">針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="de32c-320">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="de32c-321">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：</span><span class="sxs-lookup"><span data-stu-id="de32c-321">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel((context, options) =>
            {
                // Handle requests up to 50 MB
                options.Limits.MaxRequestBodySize = 52428800;
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="de32c-322"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>是用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。</span><span class="sxs-lookup"><span data-stu-id="de32c-322"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="de32c-323">在 Razor 頁面應用程式中，將篩選套用至中的[慣例](xref:razor-pages/razor-pages-conventions) `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-323">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de32c-324">在 Razor 頁面應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-324">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="de32c-325">`RequestSizeLimitAttribute`也可以使用指示詞來套用 [`@attribute`](xref:mvc/views/razor#attribute) Razor ：</span><span class="sxs-lookup"><span data-stu-id="de32c-325">The `RequestSizeLimitAttribute` can also be applied using the [`@attribute`](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="de32c-326">其他 Kestrel 限制</span><span class="sxs-lookup"><span data-stu-id="de32c-326">Other Kestrel limits</span></span>

<span data-ttu-id="de32c-327">其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="de32c-327">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="de32c-328">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="de32c-328">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="de32c-329">要求和回應資料速率</span><span class="sxs-lookup"><span data-stu-id="de32c-329">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="de32c-330">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="de32c-330">IIS content length limit</span></span>

<span data-ttu-id="de32c-331">預設要求限制（ `maxAllowedContentLength` ）是30000000個位元組，大約是 28.6 mb。</span><span class="sxs-lookup"><span data-stu-id="de32c-331">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="de32c-332">自訂*web.config*檔案中的限制：</span><span class="sxs-lookup"><span data-stu-id="de32c-332">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="de32c-333">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="de32c-333">This setting only applies to IIS.</span></span> <span data-ttu-id="de32c-334">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="de32c-334">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="de32c-335">如需詳細資訊，請參閱[要求限制 \< requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="de32c-335">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="de32c-336">ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。</span><span class="sxs-lookup"><span data-stu-id="de32c-336">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="de32c-337">如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（dotnet/AspNetCore #2711）](https://github.com/dotnet/AspNetCore/issues/2711)。</span><span class="sxs-lookup"><span data-stu-id="de32c-337">For more information, see [Unable to upload file greater than 2GB in size (dotnet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="de32c-338">疑難排解</span><span class="sxs-lookup"><span data-stu-id="de32c-338">Troubleshoot</span></span>

<span data-ttu-id="de32c-339">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="de32c-339">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="de32c-340">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="de32c-340">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="de32c-341">下列錯誤表示上傳的檔案超過伺服器設定的內容長度：</span><span class="sxs-lookup"><span data-stu-id="de32c-341">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="de32c-342">如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-342">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="de32c-343">連線失敗</span><span class="sxs-lookup"><span data-stu-id="de32c-343">Connection failure</span></span>

<span data-ttu-id="de32c-344">連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。</span><span class="sxs-lookup"><span data-stu-id="de32c-344">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="de32c-345">如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-345">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="de32c-346">Kestrel 用戶端連接限制也可能需要調整。</span><span class="sxs-lookup"><span data-stu-id="de32c-346">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="de32c-347">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="de32c-347">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="de32c-348">如果控制器接受使用上傳的檔案 <xref:Microsoft.AspNetCore.Http.IFormFile> ，但值為 `null` ，請確認 HTML 表單指定了 `enctype` 的值 `multipart/form-data` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-348">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="de32c-349">如果未在專案上設定這個屬性 `<form>` ，就不會進行檔案上傳，而且任何系結 <xref:Microsoft.AspNetCore.Http.IFormFile> 引數都是 `null` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-349">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="de32c-350">此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="de32c-350">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="de32c-351">資料流程太長</span><span class="sxs-lookup"><span data-stu-id="de32c-351">Stream was too long</span></span>

<span data-ttu-id="de32c-352">本主題中的範例依賴 <xref:System.IO.MemoryStream> 于保存上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-352">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="de32c-353">的大小限制 `MemoryStream` 為 `int.MaxValue` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-353">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="de32c-354">如果應用程式的檔案上傳案例需要保留大於 50 MB 的檔案內容，請使用不依賴單一的替代方法 `MemoryStream` 來保存上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-354">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="de32c-355">ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。</span><span class="sxs-lookup"><span data-stu-id="de32c-355">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="de32c-356">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="de32c-356">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="de32c-357">安全性考量</span><span class="sxs-lookup"><span data-stu-id="de32c-357">Security considerations</span></span>

<span data-ttu-id="de32c-358">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="de32c-358">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="de32c-359">攻擊者可能會嘗試：</span><span class="sxs-lookup"><span data-stu-id="de32c-359">Attackers may attempt to:</span></span>

* <span data-ttu-id="de32c-360">執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="de32c-360">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="de32c-361">上傳病毒或惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-361">Upload viruses or malware.</span></span>
* <span data-ttu-id="de32c-362">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="de32c-362">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="de32c-363">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="de32c-363">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="de32c-364">將檔案上傳到專用的檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="de32c-364">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="de32c-365">專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。</span><span class="sxs-lookup"><span data-stu-id="de32c-365">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="de32c-366">停用檔案上傳位置的 execute 許可權。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-366">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="de32c-367">請勿將上傳的**檔案保存在**與應用程式相同的目錄樹狀結構中。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-367">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="de32c-368">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-368">Use a safe file name determined by the app.</span></span> <span data-ttu-id="de32c-369">請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。 &dagger;HTML 會在顯示不受信任的檔案名時進行編碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-369">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="de32c-370">例如，記錄檔案名或在 UI 中顯示（自動以 Razor HTML 編碼輸出）。</span><span class="sxs-lookup"><span data-stu-id="de32c-370">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="de32c-371">僅允許應用程式設計規格的已核准副檔名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-371">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="de32c-372">確認用戶端檢查是在伺服器上執行。 &dagger;用戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="de32c-372">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="de32c-373">檢查上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-373">Check the size of an uploaded file.</span></span> <span data-ttu-id="de32c-374">設定最大大小限制以防止大量上傳。&dagger;</span><span class="sxs-lookup"><span data-stu-id="de32c-374">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="de32c-375">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-375">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="de32c-376">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="de32c-376">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="de32c-377">&dagger;範例應用程式會示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="de32c-377">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-378">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="de32c-378">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="de32c-379">完全取得系統的控制權。</span><span class="sxs-lookup"><span data-stu-id="de32c-379">Completely gain control of a system.</span></span>
> * <span data-ttu-id="de32c-380">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="de32c-380">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="de32c-381">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="de32c-381">Compromise user or system data.</span></span>
> * <span data-ttu-id="de32c-382">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="de32c-382">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="de32c-383">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="de32c-383">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="de32c-384">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="de32c-384">Unrestricted File Upload</span></span>](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
> * [<span data-ttu-id="de32c-385">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="de32c-385">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="de32c-386">如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-386">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="de32c-387">儲存案例</span><span class="sxs-lookup"><span data-stu-id="de32c-387">Storage scenarios</span></span>

<span data-ttu-id="de32c-388">檔案的一般儲存選項包括：</span><span class="sxs-lookup"><span data-stu-id="de32c-388">Common storage options for files include:</span></span>

* <span data-ttu-id="de32c-389">資料庫</span><span class="sxs-lookup"><span data-stu-id="de32c-389">Database</span></span>

  * <span data-ttu-id="de32c-390">針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。</span><span class="sxs-lookup"><span data-stu-id="de32c-390">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="de32c-391">資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。</span><span class="sxs-lookup"><span data-stu-id="de32c-391">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="de32c-392">資料庫的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="de32c-392">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="de32c-393">實體存放裝置（檔案系統或網路共用）</span><span class="sxs-lookup"><span data-stu-id="de32c-393">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="de32c-394">針對大型檔案上傳：</span><span class="sxs-lookup"><span data-stu-id="de32c-394">For large file uploads:</span></span>
    * <span data-ttu-id="de32c-395">資料庫限制可能會限制上傳的大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-395">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="de32c-396">實體儲存體通常比在資料庫中儲存的成本低。</span><span class="sxs-lookup"><span data-stu-id="de32c-396">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="de32c-397">實體儲存體的成本可能比使用資料儲存體服務來得低。</span><span class="sxs-lookup"><span data-stu-id="de32c-397">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="de32c-398">應用程式的進程必須具有儲存位置的讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="de32c-398">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="de32c-399">**絕對不要授與 execute 許可權。**</span><span class="sxs-lookup"><span data-stu-id="de32c-399">**Never grant execute permission.**</span></span>

* <span data-ttu-id="de32c-400">資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）</span><span class="sxs-lookup"><span data-stu-id="de32c-400">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="de32c-401">服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。</span><span class="sxs-lookup"><span data-stu-id="de32c-401">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="de32c-402">在大型儲存體基礎結構案例中，服務可能會降低成本。</span><span class="sxs-lookup"><span data-stu-id="de32c-402">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="de32c-403">如需詳細資訊，請參閱[快速入門：使用 .net 在物件儲存體中建立 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="de32c-403">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="de32c-404">本主題 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*> 會示範，但 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> 可在使用時，用來將儲存 <xref:System.IO.FileStream> 至 blob 儲存體 <xref:System.IO.Stream> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-404">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="de32c-405">檔案上傳案例</span><span class="sxs-lookup"><span data-stu-id="de32c-405">File upload scenarios</span></span>

<span data-ttu-id="de32c-406">上傳檔案的兩個一般方法是緩衝和串流處理。</span><span class="sxs-lookup"><span data-stu-id="de32c-406">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="de32c-407">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="de32c-407">**Buffering**</span></span>

<span data-ttu-id="de32c-408">整個檔案會讀入 <xref:Microsoft.AspNetCore.Http.IFormFile> ，這是用來處理或儲存檔案之檔案的 c # 標記法。</span><span class="sxs-lookup"><span data-stu-id="de32c-408">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="de32c-409">檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-409">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="de32c-410">如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。</span><span class="sxs-lookup"><span data-stu-id="de32c-410">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="de32c-411">如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。</span><span class="sxs-lookup"><span data-stu-id="de32c-411">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="de32c-412">任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-412">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="de32c-413">此主題的下列各節會涵蓋緩衝處理小型檔案：</span><span class="sxs-lookup"><span data-stu-id="de32c-413">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="de32c-414">實體儲存體</span><span class="sxs-lookup"><span data-stu-id="de32c-414">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="de32c-415">Database</span><span class="sxs-lookup"><span data-stu-id="de32c-415">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="de32c-416">**串流**</span><span class="sxs-lookup"><span data-stu-id="de32c-416">**Streaming**</span></span>

<span data-ttu-id="de32c-417">此檔案會從多部分要求接收，並由應用程式直接處理或儲存。</span><span class="sxs-lookup"><span data-stu-id="de32c-417">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="de32c-418">串流不會大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="de32c-418">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="de32c-419">串流處理可減少上傳檔案時記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="de32c-419">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="de32c-420">[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-420">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="de32c-421">將具有緩衝模型系結的小型檔案上傳至實體儲存體</span><span class="sxs-lookup"><span data-stu-id="de32c-421">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="de32c-422">若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="de32c-422">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="de32c-423">下列範例示範 Razor 如何使用頁面表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：</span><span class="sxs-lookup"><span data-stu-id="de32c-423">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="de32c-424">下列範例類似于先前的範例，不同之處在于：</span><span class="sxs-lookup"><span data-stu-id="de32c-424">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="de32c-425">JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="de32c-425">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="de32c-426">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="de32c-426">There's no validation.</span></span>

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

<span data-ttu-id="de32c-427">若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-427">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="de32c-428">使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。</span><span class="sxs-lookup"><span data-stu-id="de32c-428">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="de32c-429">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="de32c-429">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="de32c-430">例如：</span><span class="sxs-lookup"><span data-stu-id="de32c-430">For example:</span></span>

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

<span data-ttu-id="de32c-431">為了支援檔案上傳，HTML 表單必須指定的編碼類型（ `enctype` ） `multipart/form-data` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-431">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="de32c-432">`files`若要讓輸入元素支援上傳多個檔案，請在 `multiple` 元素上提供屬性 `<input>` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-432">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="de32c-433">您可以使用，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-433">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="de32c-434">範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="de32c-434">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="de32c-435">**not** `FileName` 除了顯示和記錄之外，請勿使用以外的屬性 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-435">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="de32c-436">當顯示或記錄時，HTML 會將檔案名編碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-436">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="de32c-437">攻擊者可以提供惡意的檔案名，包括完整路徑或相對路徑。</span><span class="sxs-lookup"><span data-stu-id="de32c-437">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="de32c-438">應用程式應該：</span><span class="sxs-lookup"><span data-stu-id="de32c-438">Applications should:</span></span>
>
> * <span data-ttu-id="de32c-439">從使用者提供的檔案名中移除路徑。</span><span class="sxs-lookup"><span data-stu-id="de32c-439">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="de32c-440">針對 UI 或記錄儲存以 HTML 編碼、路徑移除的檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-440">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="de32c-441">為儲存區產生新的隨機檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-441">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="de32c-442">下列程式碼會從檔案名中移除路徑：</span><span class="sxs-lookup"><span data-stu-id="de32c-442">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="de32c-443">到目前為止所提供的範例並不考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="de32c-443">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="de32c-444">下列各節和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="de32c-444">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de32c-445">安全性考量</span><span class="sxs-lookup"><span data-stu-id="de32c-445">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de32c-446">驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-446">Validation</span></span>](#validation)

<span data-ttu-id="de32c-447">使用模型系結和來上傳檔案時 <xref:Microsoft.AspNetCore.Http.IFormFile> ，動作方法可以接受：</span><span class="sxs-lookup"><span data-stu-id="de32c-447">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="de32c-448">單一 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-448">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="de32c-449">下列任何代表數個檔案的集合：</span><span class="sxs-lookup"><span data-stu-id="de32c-449">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="de32c-450">[名單](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="de32c-450">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="de32c-451">系結符合依名稱的表單檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-451">Binding matches form files by name.</span></span> <span data-ttu-id="de32c-452">例如，中的 HTML `name` 值 `<input type="file" name="formFile">` 必須符合 c # 參數/屬性系結（ `FormFile` ）。</span><span class="sxs-lookup"><span data-stu-id="de32c-452">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="de32c-453">如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-453">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="de32c-454">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="de32c-454">The following example:</span></span>

* <span data-ttu-id="de32c-455">迴圈一或多個已上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-455">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="de32c-456">會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-456">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="de32c-457">使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。</span><span class="sxs-lookup"><span data-stu-id="de32c-457">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="de32c-458">傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-458">Returns the total number and size of files uploaded.</span></span>

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

    return Ok(new { count = files.Count, size });
}
```

<span data-ttu-id="de32c-459">使用 `Path.GetRandomFileName` 來產生不含路徑的檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-459">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="de32c-460">在下列範例中，路徑是從設定取得：</span><span class="sxs-lookup"><span data-stu-id="de32c-460">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="de32c-461">傳遞至的路徑 <xref:System.IO.FileStream> *必須*包含檔案名。</span><span class="sxs-lookup"><span data-stu-id="de32c-461">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="de32c-462">如果未提供檔案名， <xref:System.UnauthorizedAccessException> 則會在執行時間擲回。</span><span class="sxs-lookup"><span data-stu-id="de32c-462">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="de32c-463">使用此技術所上傳的檔案 <xref:Microsoft.AspNetCore.Http.IFormFile> ，會在處理之前，先緩衝到伺服器上的記憶體或磁片上。</span><span class="sxs-lookup"><span data-stu-id="de32c-463">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="de32c-464">在動作方法內， <xref:Microsoft.AspNetCore.Http.IFormFile> 內容可當做來存取 <xref:System.IO.Stream> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-464">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="de32c-465">除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。</span><span class="sxs-lookup"><span data-stu-id="de32c-465">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="de32c-466">如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。</span><span class="sxs-lookup"><span data-stu-id="de32c-466">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-467">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) <xref:System.IO.IOException> 如果建立超過65535個檔案，而不刪除先前的暫存檔案，則 GetTempFileName 會擲回。</span><span class="sxs-lookup"><span data-stu-id="de32c-467">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="de32c-468">65535檔案的限制是每一伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="de32c-468">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="de32c-469">如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：</span><span class="sxs-lookup"><span data-stu-id="de32c-469">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="de32c-470">GetTempFileNameA 函式</span><span class="sxs-lookup"><span data-stu-id="de32c-470">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="de32c-471">將具有緩衝模型系結的小型檔案上傳至資料庫</span><span class="sxs-lookup"><span data-stu-id="de32c-471">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="de32c-472">若要使用[Entity Framework](/ef/core/index)將二進位檔案資料儲存在資料庫中，請 <xref:System.Byte> 在實體上定義陣列屬性：</span><span class="sxs-lookup"><span data-stu-id="de32c-472">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="de32c-473">為包含下列內容的類別指定頁面模型屬性 <xref:Microsoft.AspNetCore.Http.IFormFile> ：</span><span class="sxs-lookup"><span data-stu-id="de32c-473">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="de32c-474"><xref:Microsoft.AspNetCore.Http.IFormFile>可以直接當做動作方法參數或系結模型屬性來使用。</span><span class="sxs-lookup"><span data-stu-id="de32c-474"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="de32c-475">先前的範例會使用系結模型屬性。</span><span class="sxs-lookup"><span data-stu-id="de32c-475">The prior example uses a bound model property.</span></span>

<span data-ttu-id="de32c-476">在 `FileUpload` [ Razor 頁面] 表單中使用：</span><span class="sxs-lookup"><span data-stu-id="de32c-476">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="de32c-477">當表單張貼至伺服器時，請將複製 <xref:Microsoft.AspNetCore.Http.IFormFile> 到資料流程，並將它儲存為資料庫中的位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="de32c-477">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="de32c-478">在下列範例中，會 `_dbContext` 儲存應用程式的資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="de32c-478">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="de32c-479">前述範例類似于範例應用程式中所示範的案例：</span><span class="sxs-lookup"><span data-stu-id="de32c-479">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="de32c-480">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="de32c-480">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="de32c-481">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="de32c-481">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-482">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="de32c-482">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="de32c-483">請勿依賴或信任的 `FileName` 屬性， <xref:Microsoft.AspNetCore.Http.IFormFile> 而不進行驗證。</span><span class="sxs-lookup"><span data-stu-id="de32c-483">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="de32c-484">`FileName`屬性只應用於顯示用途，而且只能用於 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="de32c-484">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="de32c-485">提供的範例不會考慮安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="de32c-485">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="de32c-486">下列各節和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：</span><span class="sxs-lookup"><span data-stu-id="de32c-486">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de32c-487">安全性考量</span><span class="sxs-lookup"><span data-stu-id="de32c-487">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de32c-488">驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-488">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="de32c-489">使用串流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="de32c-489">Upload large files with streaming</span></span>

<span data-ttu-id="de32c-490">下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="de32c-490">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="de32c-491">檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。</span><span class="sxs-lookup"><span data-stu-id="de32c-491">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="de32c-492">由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。</span><span class="sxs-lookup"><span data-stu-id="de32c-492">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="de32c-493">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-493">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="de32c-494">讀取多部分區段之後，動作會執行自己的模型系結。</span><span class="sxs-lookup"><span data-stu-id="de32c-494">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="de32c-495">初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="de32c-495">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="de32c-496">屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：</span><span class="sxs-lookup"><span data-stu-id="de32c-496">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="de32c-497">`DisableFormValueModelBindingAttribute`是用來停用模型系結：</span><span class="sxs-lookup"><span data-stu-id="de32c-497">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="de32c-498">在範例應用程式中， `GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會 `/StreamedSingleFileUploadDb` `/StreamedSingleFileUploadPhysical` `Startup.ConfigureServices` 使用[ Razor 頁面慣例](xref:razor-pages/razor-pages-conventions)，套用為和頁面應用程式模型的篩選：</span><span class="sxs-lookup"><span data-stu-id="de32c-498">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="de32c-499">由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。</span><span class="sxs-lookup"><span data-stu-id="de32c-499">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="de32c-500">動作方法會直接與屬性搭配運作 `Request` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-500">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="de32c-501">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="de32c-501">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="de32c-502">索引鍵/值資料會儲存在中 `KeyValueAccumulator` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-502">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="de32c-503">讀取多部分區段之後，會使用的內容將 `KeyValueAccumulator` 表單資料系結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="de32c-503">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="de32c-504">`StreamingController.UploadDatabase`使用 EF Core 串流至資料庫的完整方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-504">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="de32c-505">`MultipartRequestHelper`（*公用程式/MultipartRequestHelper .cs*）：</span><span class="sxs-lookup"><span data-stu-id="de32c-505">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="de32c-506">`StreamingController.UploadPhysical`串流至實體位置的完整方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-506">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="de32c-507">在範例應用程式中，驗證檢查是由所處理 `FileHelpers.ProcessStreamedFile` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-507">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="de32c-508">驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-508">Validation</span></span>

<span data-ttu-id="de32c-509">範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理和串流處理檔案上傳的檢查 <xref:Microsoft.AspNetCore.Http.IFormFile> 。</span><span class="sxs-lookup"><span data-stu-id="de32c-509">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="de32c-510">如需 <xref:Microsoft.AspNetCore.Http.IFormFile> 在範例應用程式中處理緩衝的檔案上傳，請參閱 `ProcessFormFile` *公用程式/FileHelpers .cs*檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="de32c-510">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="de32c-511">如需處理資料流程檔案，請參閱 `ProcessStreamedFile` 相同檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="de32c-511">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="de32c-512">範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-512">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="de32c-513">在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。</span><span class="sxs-lookup"><span data-stu-id="de32c-513">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="de32c-514">雖然主題範例提供驗證技術的實用範例，但請勿 `FileHelpers` 在生產應用程式中執行類別，除非您：</span><span class="sxs-lookup"><span data-stu-id="de32c-514">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="de32c-515">完全瞭解此實作為。</span><span class="sxs-lookup"><span data-stu-id="de32c-515">Fully understand the implementation.</span></span>
> * <span data-ttu-id="de32c-516">針對應用程式的環境和規格修改適當的執行。</span><span class="sxs-lookup"><span data-stu-id="de32c-516">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="de32c-517">**絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**</span><span class="sxs-lookup"><span data-stu-id="de32c-517">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="de32c-518">內容驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-518">Content validation</span></span>

<span data-ttu-id="de32c-519">**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="de32c-519">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="de32c-520">在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。</span><span class="sxs-lookup"><span data-stu-id="de32c-520">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="de32c-521">如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。</span><span class="sxs-lookup"><span data-stu-id="de32c-521">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="de32c-522">一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。</span><span class="sxs-lookup"><span data-stu-id="de32c-522">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="de32c-523">當檔案通過時，檔案就會移至一般檔案儲存位置。</span><span class="sxs-lookup"><span data-stu-id="de32c-523">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="de32c-524">這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。</span><span class="sxs-lookup"><span data-stu-id="de32c-524">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="de32c-525">藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。</span><span class="sxs-lookup"><span data-stu-id="de32c-525">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="de32c-526">副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-526">File extension validation</span></span>

<span data-ttu-id="de32c-527">已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。</span><span class="sxs-lookup"><span data-stu-id="de32c-527">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="de32c-528">例如：</span><span class="sxs-lookup"><span data-stu-id="de32c-528">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="de32c-529">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-529">File signature validation</span></span>

<span data-ttu-id="de32c-530">檔案的簽章取決於檔案開頭的前幾個位元組。</span><span class="sxs-lookup"><span data-stu-id="de32c-530">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="de32c-531">這些位元組可用來指出延伸模組是否符合檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-531">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="de32c-532">範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="de32c-532">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="de32c-533">在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：</span><span class="sxs-lookup"><span data-stu-id="de32c-533">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="de32c-534">若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。</span><span class="sxs-lookup"><span data-stu-id="de32c-534">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="de32c-535">檔案名安全性</span><span class="sxs-lookup"><span data-stu-id="de32c-535">File name security</span></span>

<span data-ttu-id="de32c-536">絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。</span><span class="sxs-lookup"><span data-stu-id="de32c-536">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="de32c-537">使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。</span><span class="sxs-lookup"><span data-stu-id="de32c-537">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

Razor<span data-ttu-id="de32c-538">自動對屬性值進行 HTML 編碼以供顯示。</span><span class="sxs-lookup"><span data-stu-id="de32c-538"> automatically HTML encodes property values for display.</span></span> <span data-ttu-id="de32c-539">以下是可安全使用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="de32c-539">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="de32c-540">在之外 Razor ，一律 <xref:System.Net.WebUtility.HtmlEncode*> 是來自使用者要求的檔案名內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-540">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="de32c-541">許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。</span><span class="sxs-lookup"><span data-stu-id="de32c-541">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="de32c-542">提供其他邏輯以符合您應用程式的規格。</span><span class="sxs-lookup"><span data-stu-id="de32c-542">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="de32c-543">大小驗證</span><span class="sxs-lookup"><span data-stu-id="de32c-543">Size validation</span></span>

<span data-ttu-id="de32c-544">限制上傳檔案的大小。</span><span class="sxs-lookup"><span data-stu-id="de32c-544">Limit the size of uploaded files.</span></span>

<span data-ttu-id="de32c-545">在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。</span><span class="sxs-lookup"><span data-stu-id="de32c-545">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="de32c-546">此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的*appsettings.json*設定來提供：</span><span class="sxs-lookup"><span data-stu-id="de32c-546">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="de32c-547">`FileSizeLimit`會插入類別中 `PageModel` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-547">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="de32c-548">當檔案大小超過限制時，就會拒絕此檔案：</span><span class="sxs-lookup"><span data-stu-id="de32c-548">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="de32c-549">Match name 屬性值與 POST 方法的參數名稱</span><span class="sxs-lookup"><span data-stu-id="de32c-549">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="de32c-550">在 Razor 張貼表單資料或直接使用 JavaScript 的非表單中 `FormData` ，在表單的元素中指定的名稱或 `FormData` 必須符合控制器動作中參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="de32c-550">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="de32c-551">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="de32c-551">In the following example:</span></span>

* <span data-ttu-id="de32c-552">使用元素時 `<input>` ， `name` 屬性會設定為值 `battlePlans` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-552">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="de32c-553">`FormData`在 JavaScript 中使用時，名稱會設定為值 `battlePlans` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-553">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="de32c-554">針對 c # 方法的參數使用相符的名稱（ `battlePlans` ）：</span><span class="sxs-lookup"><span data-stu-id="de32c-554">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="de32c-555">針對名為的 Razor 頁面頁面處理常式方法 `Upload` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-555">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="de32c-556">針對 MVC POST 控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-556">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="de32c-557">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="de32c-557">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="de32c-558">多部分主體長度限制</span><span class="sxs-lookup"><span data-stu-id="de32c-558">Multipart body length limit</span></span>

<span data-ttu-id="de32c-559"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>設定每個多部分主體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="de32c-559"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="de32c-560">超過此限制的表單區段會 <xref:System.IO.InvalidDataException> 在剖析時擲回。</span><span class="sxs-lookup"><span data-stu-id="de32c-560">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="de32c-561">預設值為134217728（128 MB）。</span><span class="sxs-lookup"><span data-stu-id="de32c-561">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="de32c-562">使用中的設定來自訂限制 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-562">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de32c-563"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>是用來設定 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 單一頁面或動作的。</span><span class="sxs-lookup"><span data-stu-id="de32c-563"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="de32c-564">在 Razor 頁面應用程式中，將篩選套用至中的[慣例](xref:razor-pages/razor-pages-conventions) `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-564">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de32c-565">在 Razor 頁面應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-565">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="de32c-566">Kestrel 要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="de32c-566">Kestrel maximum request body size</span></span>

<span data-ttu-id="de32c-567">針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="de32c-567">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="de32c-568">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：</span><span class="sxs-lookup"><span data-stu-id="de32c-568">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="de32c-569"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>是用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。</span><span class="sxs-lookup"><span data-stu-id="de32c-569"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="de32c-570">在 Razor 頁面應用程式中，將篩選套用至中的[慣例](xref:razor-pages/razor-pages-conventions) `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="de32c-570">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de32c-571">在 Razor 頁面應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="de32c-571">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="de32c-572">其他 Kestrel 限制</span><span class="sxs-lookup"><span data-stu-id="de32c-572">Other Kestrel limits</span></span>

<span data-ttu-id="de32c-573">其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="de32c-573">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="de32c-574">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="de32c-574">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="de32c-575">要求和回應資料速率</span><span class="sxs-lookup"><span data-stu-id="de32c-575">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="de32c-576">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="de32c-576">IIS content length limit</span></span>

<span data-ttu-id="de32c-577">預設要求限制（ `maxAllowedContentLength` ）是30000000個位元組，大約是 28.6 mb。</span><span class="sxs-lookup"><span data-stu-id="de32c-577">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="de32c-578">自訂*web.config*檔案中的限制：</span><span class="sxs-lookup"><span data-stu-id="de32c-578">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="de32c-579">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="de32c-579">This setting only applies to IIS.</span></span> <span data-ttu-id="de32c-580">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="de32c-580">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="de32c-581">如需詳細資訊，請參閱[要求限制 \< requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="de32c-581">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="de32c-582">ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。</span><span class="sxs-lookup"><span data-stu-id="de32c-582">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="de32c-583">如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（dotnet/AspNetCore #2711）](https://github.com/dotnet/AspNetCore/issues/2711)。</span><span class="sxs-lookup"><span data-stu-id="de32c-583">For more information, see [Unable to upload file greater than 2GB in size (dotnet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="de32c-584">疑難排解</span><span class="sxs-lookup"><span data-stu-id="de32c-584">Troubleshoot</span></span>

<span data-ttu-id="de32c-585">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="de32c-585">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="de32c-586">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="de32c-586">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="de32c-587">下列錯誤表示上傳的檔案超過伺服器設定的內容長度：</span><span class="sxs-lookup"><span data-stu-id="de32c-587">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="de32c-588">如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-588">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="de32c-589">連線失敗</span><span class="sxs-lookup"><span data-stu-id="de32c-589">Connection failure</span></span>

<span data-ttu-id="de32c-590">連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。</span><span class="sxs-lookup"><span data-stu-id="de32c-590">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="de32c-591">如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。</span><span class="sxs-lookup"><span data-stu-id="de32c-591">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="de32c-592">Kestrel 用戶端連接限制也可能需要調整。</span><span class="sxs-lookup"><span data-stu-id="de32c-592">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="de32c-593">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="de32c-593">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="de32c-594">如果控制器接受使用上傳的檔案 <xref:Microsoft.AspNetCore.Http.IFormFile> ，但值為 `null` ，請確認 HTML 表單指定了 `enctype` 的值 `multipart/form-data` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-594">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="de32c-595">如果未在專案上設定這個屬性 `<form>` ，就不會進行檔案上傳，而且任何系結 <xref:Microsoft.AspNetCore.Http.IFormFile> 引數都是 `null` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-595">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="de32c-596">此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="de32c-596">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="de32c-597">資料流程太長</span><span class="sxs-lookup"><span data-stu-id="de32c-597">Stream was too long</span></span>

<span data-ttu-id="de32c-598">本主題中的範例依賴 <xref:System.IO.MemoryStream> 于保存上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-598">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="de32c-599">的大小限制 `MemoryStream` 為 `int.MaxValue` 。</span><span class="sxs-lookup"><span data-stu-id="de32c-599">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="de32c-600">如果應用程式的檔案上傳案例需要保留大於 50 MB 的檔案內容，請使用不依賴單一的替代方法 `MemoryStream` 來保存上傳檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="de32c-600">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="de32c-601">其他資源</span><span class="sxs-lookup"><span data-stu-id="de32c-601">Additional resources</span></span>

* [<span data-ttu-id="de32c-602">HTTP 連接要求清空</span><span class="sxs-lookup"><span data-stu-id="de32c-602">HTTP connection request draining</span></span>](xref:fundamentals/servers/kestrel#http11-request-draining)
* [<span data-ttu-id="de32c-603">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="de32c-603">Unrestricted File Upload</span></span>](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
* [<span data-ttu-id="de32c-604">Azure 安全性：安全性框架：輸入驗證 |緩和措施</span><span class="sxs-lookup"><span data-stu-id="de32c-604">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="de32c-605">Azure 雲端設計模式： Valet 金鑰模式</span><span class="sxs-lookup"><span data-stu-id="de32c-605">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
