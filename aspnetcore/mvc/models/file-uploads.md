---
title: 上傳ASP.NET核心中的檔案
author: rick-anderson
description: 如何使用模型繫結和資料流在 ASP.NET Core MVC 上傳檔案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2020
uid: mvc/models/file-uploads
ms.openlocfilehash: e25da0b3867181a16a4636768f36c148a152dd23
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661730"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="a28ca-103">上傳ASP.NET核心中的檔案</span><span class="sxs-lookup"><span data-stu-id="a28ca-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="a28ca-104">由[史蒂夫史密斯](https://ardalis.com/)和[魯特格風暴](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="a28ca-104">By [Steve Smith](https://ardalis.com/) and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a28ca-105">ASP.NET Core 支援使用緩衝模型綁定上傳一個或多個檔,用於較小的檔,以及針對較大檔的未緩衝流。</span><span class="sxs-lookup"><span data-stu-id="a28ca-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="a28ca-106">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a28ca-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="a28ca-107">安全性考量</span><span class="sxs-lookup"><span data-stu-id="a28ca-107">Security considerations</span></span>

<span data-ttu-id="a28ca-108">在為使用者提供將檔上傳到伺服器的能力時,請謹慎。</span><span class="sxs-lookup"><span data-stu-id="a28ca-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="a28ca-109">攻擊者可能嘗試:</span><span class="sxs-lookup"><span data-stu-id="a28ca-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="a28ca-110">執行[拒絕服務](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="a28ca-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="a28ca-111">上傳病毒或惡意軟體。</span><span class="sxs-lookup"><span data-stu-id="a28ca-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="a28ca-112">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="a28ca-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="a28ca-113">降低成功攻擊可能性的安全步驟包括:</span><span class="sxs-lookup"><span data-stu-id="a28ca-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="a28ca-114">將檔上載到專用檔上傳區域,最好將其上傳到非系統驅動器。</span><span class="sxs-lookup"><span data-stu-id="a28ca-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="a28ca-115">專用位置使對上載的文件實施安全限制變得更加容易。</span><span class="sxs-lookup"><span data-stu-id="a28ca-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="a28ca-116">禁用對檔上載位置執行許可權。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="a28ca-117">**不要**將上載的檔保留在同一目錄樹中。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="a28ca-118">使用由應用確定的安全檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="a28ca-119">不要使用使用者提供的檔案名或上傳檔的不受信任的檔名。&dagger; HTML 在顯示不受信任的檔名時對其進行編碼。</span><span class="sxs-lookup"><span data-stu-id="a28ca-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="a28ca-120">例如,在 UI 中記錄檔名或顯示(Razor 自動 HTML 編碼輸出)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="a28ca-121">僅允許應用設計規範的已批准文件副檔名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="a28ca-122">驗證在伺服器上是否執行客戶端檢查。&dagger;客戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="a28ca-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="a28ca-123">檢查上載檔的大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="a28ca-124">設置最大大小限制以防止大型上載。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="a28ca-125">當檔不應被同名上傳的文件覆蓋時,請在上載檔之前對照資料庫或物理存儲檢查檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="a28ca-126">**在存儲檔之前,對上載的內容運行病毒/惡意軟體掃描程式。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="a28ca-127">&dagger;示例應用演示了滿足條件的方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-128">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="a28ca-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="a28ca-129">完全控制系統。</span><span class="sxs-lookup"><span data-stu-id="a28ca-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="a28ca-130">使系統過載,導致系統崩潰。</span><span class="sxs-lookup"><span data-stu-id="a28ca-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="a28ca-131">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="a28ca-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="a28ca-132">將塗鴉應用於公共 UI。</span><span class="sxs-lookup"><span data-stu-id="a28ca-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="a28ca-133">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="a28ca-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="a28ca-134">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="a28ca-134">Unrestricted File Upload</span></span>](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
> * [<span data-ttu-id="a28ca-135">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="a28ca-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="a28ca-136">有關實現安全措施的詳細資訊(包括示例應用的示例),請參閱[驗證](#validation)部分。</span><span class="sxs-lookup"><span data-stu-id="a28ca-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="a28ca-137">儲存機制</span><span class="sxs-lookup"><span data-stu-id="a28ca-137">Storage scenarios</span></span>

<span data-ttu-id="a28ca-138">檔案的常見記憶體選項包括:</span><span class="sxs-lookup"><span data-stu-id="a28ca-138">Common storage options for files include:</span></span>

* <span data-ttu-id="a28ca-139">資料庫</span><span class="sxs-lookup"><span data-stu-id="a28ca-139">Database</span></span>

  * <span data-ttu-id="a28ca-140">對於小型檔上載,資料庫通常比實體存儲(檔案系統或網路共用)選項更快。</span><span class="sxs-lookup"><span data-stu-id="a28ca-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="a28ca-141">資料庫通常比物理存儲選項更方便,因為檢索用戶數據的資料庫記錄可以同時提供文件內容(例如,頭像映射)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="a28ca-142">與使用數據存儲服務相比,資料庫的成本可能更低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="a28ca-143">實體儲存(檔案系統或網路分享)</span><span class="sxs-lookup"><span data-stu-id="a28ca-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="a28ca-144">對大型檔案上載:</span><span class="sxs-lookup"><span data-stu-id="a28ca-144">For large file uploads:</span></span>
    * <span data-ttu-id="a28ca-145">資料庫限制可能會限制上載的大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="a28ca-146">物理存儲通常比資料庫中的存儲經濟程度低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="a28ca-147">物理存儲可能比使用數據存儲服務的成本更低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="a28ca-148">應用的進程必須具有存儲位置的讀取和寫入許可權。</span><span class="sxs-lookup"><span data-stu-id="a28ca-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="a28ca-149">**切勿授予執行許可權。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="a28ca-150">資料儲存服務(例如[,Azure Blob 儲存](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="a28ca-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="a28ca-151">與通常單點故障的本地解決方案,服務通常提供改進的可擴充性和彈性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="a28ca-152">在大型存儲基礎結構方案中,服務的成本可能會更低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="a28ca-153">有關詳細資訊,請參閱[快速入門:使用 .NET 在物件存儲中創建 Blob。](/azure/storage/blobs/storage-quickstart-blobs-dotnet)</span><span class="sxs-lookup"><span data-stu-id="a28ca-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="a28ca-154">檔案上傳機制</span><span class="sxs-lookup"><span data-stu-id="a28ca-154">File upload scenarios</span></span>

<span data-ttu-id="a28ca-155">上傳檔的兩種常規方法是緩衝和流式傳輸。</span><span class="sxs-lookup"><span data-stu-id="a28ca-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="a28ca-156">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="a28ca-156">**Buffering**</span></span>

<span data-ttu-id="a28ca-157">整個檔被讀入一<xref:Microsoft.AspNetCore.Http.IFormFile>個 ,它是用於處理或保存檔的檔的 C# 表示形式。</span><span class="sxs-lookup"><span data-stu-id="a28ca-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="a28ca-158">檔上載使用的資源(磁碟、記憶體)取決於併發檔上載的數量和大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="a28ca-159">如果應用嘗試緩衝過多的上載,則當網站記憶體或磁碟空間不足時,該網站將崩潰。</span><span class="sxs-lookup"><span data-stu-id="a28ca-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="a28ca-160">如果檔上載的大小或頻率耗盡了應用資源,請使用流式處理。</span><span class="sxs-lookup"><span data-stu-id="a28ca-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="a28ca-161">任何超過 64 KB 的單個緩衝檔將從記憶體移動到磁碟上的臨時檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="a28ca-162">本主題的以下部分介紹了緩衝小檔:</span><span class="sxs-lookup"><span data-stu-id="a28ca-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="a28ca-163">實體儲存</span><span class="sxs-lookup"><span data-stu-id="a28ca-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="a28ca-164">Database</span><span class="sxs-lookup"><span data-stu-id="a28ca-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="a28ca-165">**串流**</span><span class="sxs-lookup"><span data-stu-id="a28ca-165">**Streaming**</span></span>

<span data-ttu-id="a28ca-166">該檔從多部分請求接收,並由應用直接處理或保存。</span><span class="sxs-lookup"><span data-stu-id="a28ca-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="a28ca-167">流式處理不會顯著提高性能。</span><span class="sxs-lookup"><span data-stu-id="a28ca-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="a28ca-168">流式傳輸減少了上傳檔時對記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="a28ca-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="a28ca-169">流式處理大型檔在[「上傳包含流式處理的大型檔](#upload-large-files-with-streaming)」部分中介紹。</span><span class="sxs-lookup"><span data-stu-id="a28ca-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="a28ca-170">將具有緩衝模型的小型檔案載入物理儲存</span><span class="sxs-lookup"><span data-stu-id="a28ca-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="a28ca-171">要上載小檔,請使用多部分窗體或使用 JAVAScript 構造 POST 請求。</span><span class="sxs-lookup"><span data-stu-id="a28ca-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="a28ca-172">下面的範例展示使用 Razor Pages 表單上載單檔案(例如範*單中的網頁/緩衝單檔案上傳物理.cshtml):*</span><span class="sxs-lookup"><span data-stu-id="a28ca-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="a28ca-173">以下示例與以前的示例類似,只不過:</span><span class="sxs-lookup"><span data-stu-id="a28ca-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="a28ca-174">JavaScript 的 ([提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) 用於提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="a28ca-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="a28ca-175">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="a28ca-175">There's no validation.</span></span>

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

<span data-ttu-id="a28ca-176">要對[不支援 Fetch API 的](https://caniuse.com/#feat=fetch)用戶端在 JavaScript 執行表單 POST,請使用以下方法之一:</span><span class="sxs-lookup"><span data-stu-id="a28ca-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="a28ca-177">使用「擷取」多邊形填充(例如,[視窗.fetch 多邊填充(github/提取)。](https://github.com/github/fetch)</span><span class="sxs-lookup"><span data-stu-id="a28ca-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="a28ca-178">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="a28ca-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="a28ca-179">例如：</span><span class="sxs-lookup"><span data-stu-id="a28ca-179">For example:</span></span>

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

<span data-ttu-id="a28ca-180">為了支援檔案中載,HTML 窗型必須指定的編碼型`enctype`態`multipart/form-data`( )</span><span class="sxs-lookup"><span data-stu-id="a28ca-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="a28ca-181">對於支援`files`上載多個檔案的輸入元素,`multiple``<input>`該 元素在元素上提供屬性:</span><span class="sxs-lookup"><span data-stu-id="a28ca-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="a28ca-182">上傳到伺服器的各個檔案可以使用[模型連結](xref:mvc/models/model-binding)<xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="a28ca-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="a28ca-183">範例應用示範了多個用於資料庫和實體存儲方案的緩衝檔上載。</span><span class="sxs-lookup"><span data-stu-id="a28ca-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="a28ca-184">**請勿**`FileName`使用 顯示和日<xref:Microsoft.AspNetCore.Http.IFormFile>誌記錄 以外的屬性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-184">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="a28ca-185">顯示或紀錄記錄時,HTML 對檔案名進行編碼。</span><span class="sxs-lookup"><span data-stu-id="a28ca-185">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="a28ca-186">攻擊者可以提供惡意檔名,包括完整路徑或相對路徑。</span><span class="sxs-lookup"><span data-stu-id="a28ca-186">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="a28ca-187">應用程式應:</span><span class="sxs-lookup"><span data-stu-id="a28ca-187">Applications should:</span></span>
>
> * <span data-ttu-id="a28ca-188">從使用者提供的檔名中刪除路徑。</span><span class="sxs-lookup"><span data-stu-id="a28ca-188">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="a28ca-189">儲存 HTML 編碼的路徑刪除的檔名,用於 UI 或紀錄記錄。</span><span class="sxs-lookup"><span data-stu-id="a28ca-189">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="a28ca-190">生成新的隨機檔名進行存儲。</span><span class="sxs-lookup"><span data-stu-id="a28ca-190">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="a28ca-191">以下代碼從檔案名中刪除路徑:</span><span class="sxs-lookup"><span data-stu-id="a28ca-191">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="a28ca-192">到目前為止提供的示例沒有考慮到安全注意事項。</span><span class="sxs-lookup"><span data-stu-id="a28ca-192">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="a28ca-193">其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:</span><span class="sxs-lookup"><span data-stu-id="a28ca-193">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="a28ca-194">安全性考量</span><span class="sxs-lookup"><span data-stu-id="a28ca-194">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="a28ca-195">驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-195">Validation</span></span>](#validation)

<span data-ttu-id="a28ca-196">使用模型綁定<xref:Microsoft.AspNetCore.Http.IFormFile>和 上載檔時,操作方法可以接受:</span><span class="sxs-lookup"><span data-stu-id="a28ca-196">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="a28ca-197">單個<xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="a28ca-197">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="a28ca-198">表示多個檔案的以下任何集合:</span><span class="sxs-lookup"><span data-stu-id="a28ca-198">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="a28ca-199">[清單](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="a28ca-199">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="a28ca-200">綁定依名稱匹配表單檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-200">Binding matches form files by name.</span></span> <span data-ttu-id="a28ca-201">例如,中的`name``<input type="file" name="formFile">`HTML 值必須與 C# 參數`FormFile`/ 屬性繫結 ( ) 符合。</span><span class="sxs-lookup"><span data-stu-id="a28ca-201">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="a28ca-202">關於詳細資訊,請參閱 POST[方法部份的參數名稱的比名稱屬性值](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-202">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="a28ca-203">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="a28ca-203">The following example:</span></span>

* <span data-ttu-id="a28ca-204">循環訪問一個或多個上傳的檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-204">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="a28ca-205">使用[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)傳回檔案的完整路徑,包括檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-205">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="a28ca-206">使用應用生成的檔案名將檔儲存到本地檔案系統。</span><span class="sxs-lookup"><span data-stu-id="a28ca-206">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="a28ca-207">返回上載的檔的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-207">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="a28ca-208">用於`Path.GetRandomFileName`生成沒有路徑的檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-208">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="a28ca-209">在下面的範例中,路徑是從設定中取得的:</span><span class="sxs-lookup"><span data-stu-id="a28ca-209">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="a28ca-210">傳遞到<xref:System.IO.FileStream>的路徑*必須*包括檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-210">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="a28ca-211">如果未提供檔案名稱,則在執行時引發<xref:System.UnauthorizedAccessException>。</span><span class="sxs-lookup"><span data-stu-id="a28ca-211">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="a28ca-212">在處理之前,使用<xref:Microsoft.AspNetCore.Http.IFormFile>該技術上載的檔將緩衝在記憶體中或伺服器上的磁碟上。</span><span class="sxs-lookup"><span data-stu-id="a28ca-212">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="a28ca-213">在操作方法中,<xref:Microsoft.AspNetCore.Http.IFormFile>內容可作為<xref:System.IO.Stream>可訪問。</span><span class="sxs-lookup"><span data-stu-id="a28ca-213">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="a28ca-214">除了本地檔案系統之外,檔案還可以保存到網路共享或檔案存儲服務(如[Azure Blob 儲存](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs))。</span><span class="sxs-lookup"><span data-stu-id="a28ca-214">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="a28ca-215">有關迴圈存取多個檔以進行上載並使用安全檔名的範例,請參閱範例應用中*的頁面/緩衝倍數檔檔物理.cshtml.cs。*</span><span class="sxs-lookup"><span data-stu-id="a28ca-215">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-216">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)<xref:System.IO.IOException>會引發一個,如果創建了超過 65,535 個檔,而不刪除以前的臨時檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-216">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="a28ca-217">65,535 個檔的限制是每個伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="a28ca-217">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="a28ca-218">有關 Windows 作業系統上此限制的詳細資訊,請參閱以下主題中的備註:</span><span class="sxs-lookup"><span data-stu-id="a28ca-218">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="a28ca-219">取得tempFilenameA函式</span><span class="sxs-lookup"><span data-stu-id="a28ca-219">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="a28ca-220">將具有緩衝模型的小檔案載到資料庫</span><span class="sxs-lookup"><span data-stu-id="a28ca-220">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="a28ca-221">要使用[實體框架](/ef/core/index)將二進位檔案資料儲存在資料庫中,<xref:System.Byte>請在實體上定義數位屬性:</span><span class="sxs-lookup"><span data-stu-id="a28ca-221">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="a28ca-222">指定的類別指定頁面模型屬性<xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="a28ca-222">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="a28ca-223"><xref:Microsoft.AspNetCore.Http.IFormFile>可以直接用作操作方法參數或綁定模型屬性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-223"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="a28ca-224">上一個示例使用綁定模型屬性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-224">The prior example uses a bound model property.</span></span>

<span data-ttu-id="a28ca-225">在`FileUpload`剃刀頁窗體中使用:</span><span class="sxs-lookup"><span data-stu-id="a28ca-225">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="a28ca-226">將表單 POSTed 複製到伺服器時<xref:Microsoft.AspNetCore.Http.IFormFile>,將複製到流並將其另存為資料庫中的位元組。</span><span class="sxs-lookup"><span data-stu-id="a28ca-226">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="a28ca-227">在以下範例中,`_dbContext`儲存應用的資料庫內容:</span><span class="sxs-lookup"><span data-stu-id="a28ca-227">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="a28ca-228">前面的範例類似於範例應用中展示的方案:</span><span class="sxs-lookup"><span data-stu-id="a28ca-228">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="a28ca-229">*頁面/緩衝單檔案上傳Db.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a28ca-229">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="a28ca-230">*頁面/緩衝單檔案上傳Db.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a28ca-230">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-231">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="a28ca-231">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="a28ca-232">未經驗證,不要依賴或信任`FileName`的屬性。 <xref:Microsoft.AspNetCore.Http.IFormFile></span><span class="sxs-lookup"><span data-stu-id="a28ca-232">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="a28ca-233">該`FileName`屬性應僅用於顯示目的,並且僅在 HTML 編碼後使用。</span><span class="sxs-lookup"><span data-stu-id="a28ca-233">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="a28ca-234">提供的示例不考慮安全注意事項。</span><span class="sxs-lookup"><span data-stu-id="a28ca-234">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="a28ca-235">其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:</span><span class="sxs-lookup"><span data-stu-id="a28ca-235">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="a28ca-236">安全性考量</span><span class="sxs-lookup"><span data-stu-id="a28ca-236">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="a28ca-237">驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-237">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="a28ca-238">使用流式上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="a28ca-238">Upload large files with streaming</span></span>

<span data-ttu-id="a28ca-239">下面的範例展示如何使用 JavaScript 將檔案流式傳輸到控制器操作。</span><span class="sxs-lookup"><span data-stu-id="a28ca-239">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="a28ca-240">該檔的反偽造權杖使用自定義篩選器屬性生成,並傳遞到客戶端 HTTP 標頭而不是請求正文中。</span><span class="sxs-lookup"><span data-stu-id="a28ca-240">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="a28ca-241">由於操作方法直接處理上載的數據,因此表單模型綁定被另一個自定義篩選器禁用。</span><span class="sxs-lookup"><span data-stu-id="a28ca-241">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="a28ca-242">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-242">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="a28ca-243">讀取多部分部分后,操作將執行其自己的模型綁定。</span><span class="sxs-lookup"><span data-stu-id="a28ca-243">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="a28ca-244">初始頁面回應載入表單,並將反偽造權杖保存在 Cookie 中`GenerateAntiforgeryTokenCookieAttribute`(通過屬性 )。</span><span class="sxs-lookup"><span data-stu-id="a28ca-244">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="a28ca-245">該屬性使用 ASP.NET Core 的內建[反偽造支援](xref:security/anti-request-forgery)來設定具有請求權杖的 Cookie:</span><span class="sxs-lookup"><span data-stu-id="a28ca-245">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="a28ca-246">`DisableFormValueModelBindingAttribute`關閉模型的系統:</span><span class="sxs-lookup"><span data-stu-id="a28ca-246">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="a28ca-247">在`GenerateAntiforgeryTokenCookieAttribute`範例應用中,並`DisableFormValueModelBindingAttribute`作為篩選器應用`/StreamedSingleFileUploadDb``/StreamedSingleFileUploadPhysical``Startup.ConfigureServices`於使用 Razor Pages 約定的 頁面應用程式模型和 中的[頁面](xref:razor-pages/razor-pages-conventions)應用程式模型:</span><span class="sxs-lookup"><span data-stu-id="a28ca-247">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="a28ca-248">由於模型綁定不讀取窗體,因此從窗體綁定的參數不會綁定(查詢、路由和標頭繼續工作)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-248">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="a28ca-249">操作方法直接與 屬性`Request`一起工作。</span><span class="sxs-lookup"><span data-stu-id="a28ca-249">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="a28ca-250">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="a28ca-250">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="a28ca-251">鍵/數值資料儲存在`KeyValueAccumulator`中 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-251">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="a28ca-252">讀取多部分部分後,使用的內容`KeyValueAccumulator`將窗體數據綁定到模型類型。</span><span class="sxs-lookup"><span data-stu-id="a28ca-252">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="a28ca-253">使用`StreamingController.UploadDatabase`EF Core 串流式傳輸到資料庫的完整方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-253">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="a28ca-254">`MultipartRequestHelper`(*公用程式/ 多部份要求説明者.cs*):</span><span class="sxs-lookup"><span data-stu-id="a28ca-254">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="a28ca-255">串流式傳輸`StreamingController.UploadPhysical`到 物理位置的完整方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-255">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="a28ca-256">在範例應用中,驗證檢查由`FileHelpers.ProcessStreamedFile`處理 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-256">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="a28ca-257">驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-257">Validation</span></span>

<span data-ttu-id="a28ca-258">示例應用的`FileHelpers`類演示了對緩<xref:Microsoft.AspNetCore.Http.IFormFile>衝 和流式檔上載的多次檢查。</span><span class="sxs-lookup"><span data-stu-id="a28ca-258">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="a28ca-259">有關在<xref:Microsoft.AspNetCore.Http.IFormFile>示例應用中處理緩衝檔上傳,請參閱`ProcessFormFile`*實用程式/FileHelpers.cs*檔中的方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-259">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="a28ca-260">有關處理流式處理檔,`ProcessStreamedFile`請參閱同一檔中的方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-260">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-261">範例應用中演示的驗證處理方法不會掃描上載文件的內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-261">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="a28ca-262">在大多數生產方案中,在將檔可供使用者或其他系統使用之前,在檔上使用病毒/惡意軟體掃描器 API。</span><span class="sxs-lookup"><span data-stu-id="a28ca-262">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="a28ca-263">儘管主題示例提供了驗證技術的工作示例,但除非在生產應用中實現`FileHelpers`類,否則不要實現該類:</span><span class="sxs-lookup"><span data-stu-id="a28ca-263">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="a28ca-264">完全理解實施情況。</span><span class="sxs-lookup"><span data-stu-id="a28ca-264">Fully understand the implementation.</span></span>
> * <span data-ttu-id="a28ca-265">根據需要修改應用的環境和規範的實現。</span><span class="sxs-lookup"><span data-stu-id="a28ca-265">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="a28ca-266">**如果不滿足這些要求,切勿在應用中不加區別地實施安全代碼。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-266">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="a28ca-267">內容驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-267">Content validation</span></span>

<span data-ttu-id="a28ca-268">**對上傳的內容使用第三方病毒/惡意軟體掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-268">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="a28ca-269">在高容量方案中,掃描檔對伺服器資源的要求很高。</span><span class="sxs-lookup"><span data-stu-id="a28ca-269">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="a28ca-270">如果由於文件掃描而使請求處理性能降低,請考慮將掃描工作卸載到[後台服務](xref:fundamentals/host/hosted-services),可能是在伺服器上運行的服務不同於應用的伺服器。</span><span class="sxs-lookup"><span data-stu-id="a28ca-270">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="a28ca-271">通常,上傳的檔在隔離區域中,直到背景病毒掃描程式檢查它們。</span><span class="sxs-lookup"><span data-stu-id="a28ca-271">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="a28ca-272">當檔通過時,檔將移動到正常檔案存儲位置。</span><span class="sxs-lookup"><span data-stu-id="a28ca-272">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="a28ca-273">這些步驟通常與指示檔的掃描狀態的資料庫記錄一起執行。</span><span class="sxs-lookup"><span data-stu-id="a28ca-273">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="a28ca-274">通過使用這種方法,應用和應用程式伺服器仍然專注於回應請求。</span><span class="sxs-lookup"><span data-stu-id="a28ca-274">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="a28ca-275">檔案副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-275">File extension validation</span></span>

<span data-ttu-id="a28ca-276">應根據允許的擴展清單檢查上載的檔案副檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-276">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="a28ca-277">例如：</span><span class="sxs-lookup"><span data-stu-id="a28ca-277">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="a28ca-278">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-278">File signature validation</span></span>

<span data-ttu-id="a28ca-279">檔的簽名由檔開頭的前幾個位元組決定。</span><span class="sxs-lookup"><span data-stu-id="a28ca-279">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="a28ca-280">這些位元組可用於指示擴展名是否與文件的內容匹配。</span><span class="sxs-lookup"><span data-stu-id="a28ca-280">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="a28ca-281">範例應用檢查檔簽名中有幾個常見的文件類型。</span><span class="sxs-lookup"><span data-stu-id="a28ca-281">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="a28ca-282">在下面的範例中,針對該檔案檢查 JPEG 影像的檔案簽名:</span><span class="sxs-lookup"><span data-stu-id="a28ca-282">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="a28ca-283">要獲取其他文件簽名,請參閱[文件簽名資料庫](https://www.filesignatures.net/)和官方文件規範。</span><span class="sxs-lookup"><span data-stu-id="a28ca-283">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="a28ca-284">檔案名稱安全性</span><span class="sxs-lookup"><span data-stu-id="a28ca-284">File name security</span></span>

<span data-ttu-id="a28ca-285">切勿使用用戶端提供的檔案名將檔儲存到物理存儲。</span><span class="sxs-lookup"><span data-stu-id="a28ca-285">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="a28ca-286">使用[Path.GetRandomFilename](xref:System.IO.Path.GetRandomFileName*)或[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)為檔案建立安全性的檔名,以創建用於暫時儲存的完整路徑(包括檔名)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-286">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="a28ca-287">Razor 會自動對顯示的屬性值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="a28ca-287">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="a28ca-288">以下代碼可安全使用:</span><span class="sxs-lookup"><span data-stu-id="a28ca-288">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="a28ca-289">在 Razor<xref:System.Net.WebUtility.HtmlEncode*>之外 ,始終從使用者請求中提交姓名內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-289">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="a28ca-290">許多實現必須包括檢查該檔是否存在;否則,該檔將被同名文件覆蓋。</span><span class="sxs-lookup"><span data-stu-id="a28ca-290">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="a28ca-291">提供其他邏輯以滿足應用的規範。</span><span class="sxs-lookup"><span data-stu-id="a28ca-291">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="a28ca-292">大小驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-292">Size validation</span></span>

<span data-ttu-id="a28ca-293">限制上載檔的大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-293">Limit the size of uploaded files.</span></span>

<span data-ttu-id="a28ca-294">在範例應用中,檔的大小限制為2 MB(以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-294">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="a28ca-295">限制透過來自*應用程式設定.json*檔的[設定](xref:fundamentals/configuration/index)提供:</span><span class="sxs-lookup"><span data-stu-id="a28ca-295">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="a28ca-296">註`FileSizeLimit`入 :`PageModel`</span><span class="sxs-lookup"><span data-stu-id="a28ca-296">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="a28ca-297">當檔案大小超過限制時,檔案將被拒絕:</span><span class="sxs-lookup"><span data-stu-id="a28ca-297">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="a28ca-298">將名稱屬性值與 POST 方法的參數名稱符合</span><span class="sxs-lookup"><span data-stu-id="a28ca-298">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="a28ca-299">在 POST 形成資料或使用 JavaScript`FormData`的非 Razor 窗體中,在窗體`FormData`元素中指定的名稱或 必須與控制器操作中參數的名稱匹配。</span><span class="sxs-lookup"><span data-stu-id="a28ca-299">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="a28ca-300">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="a28ca-300">In the following example:</span></span>

* <span data-ttu-id="a28ca-301">使用`<input>`元素時,屬性`name`設定為值`battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="a28ca-301">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="a28ca-302">`FormData`在 JavaScript 中使用時,名稱設定`battlePlans`為 :</span><span class="sxs-lookup"><span data-stu-id="a28ca-302">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="a28ca-303">描述 C# 方法的參數使用符合`battlePlans`名稱 ( :</span><span class="sxs-lookup"><span data-stu-id="a28ca-303">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="a28ca-304">對於名為的`Upload`Razor 頁面頁面處理程式方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-304">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="a28ca-305">對於 MVC POST 控制器操作方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-305">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="a28ca-306">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="a28ca-306">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="a28ca-307">多部份體長度限制</span><span class="sxs-lookup"><span data-stu-id="a28ca-307">Multipart body length limit</span></span>

<span data-ttu-id="a28ca-308"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>設置每個多部分實體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="a28ca-308"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="a28ca-309">超過此限制的表單的元件在<xref:System.IO.InvalidDataException>分析 時會引發 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-309">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="a28ca-310">默認值為 134,217,728 (128 MB)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-310">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="a28ca-311">使用的<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>`Startup.ConfigureServices`設定自訂限制:</span><span class="sxs-lookup"><span data-stu-id="a28ca-311">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="a28ca-312"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>為單一頁面或<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>操作設定 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-312"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="a28ca-313">在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:</span><span class="sxs-lookup"><span data-stu-id="a28ca-313">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="a28ca-314">在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面模型或操作方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-314">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="a28ca-315">凱斯特雷爾最大請求正文大小</span><span class="sxs-lookup"><span data-stu-id="a28ca-315">Kestrel maximum request body size</span></span>

<span data-ttu-id="a28ca-316">對於 Kestrel 託管的應用,預設的最大請求正文大小為 30,000,000 位元組,約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="a28ca-316">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="a28ca-317">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制:</span><span class="sxs-lookup"><span data-stu-id="a28ca-317">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="a28ca-318"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>用於為單個頁面或操作設置[最大請求博量。](xref:fundamentals/servers/kestrel#maximum-request-body-size)</span><span class="sxs-lookup"><span data-stu-id="a28ca-318"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="a28ca-319">在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:</span><span class="sxs-lookup"><span data-stu-id="a28ca-319">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="a28ca-320">在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面處理程式類或操作方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-320">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="a28ca-321">`RequestSizeLimitAttribute`也可以使用[`@attribute`](xref:mvc/views/razor#attribute)Razor 指令應用:</span><span class="sxs-lookup"><span data-stu-id="a28ca-321">The `RequestSizeLimitAttribute` can also be applied using the [`@attribute`](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="a28ca-322">其他凱斯特雷爾限制</span><span class="sxs-lookup"><span data-stu-id="a28ca-322">Other Kestrel limits</span></span>

<span data-ttu-id="a28ca-323">其他凱斯特雷爾限制可能適用於由凱斯特雷爾託管的應用程式:</span><span class="sxs-lookup"><span data-stu-id="a28ca-323">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="a28ca-324">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="a28ca-324">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="a28ca-325">要求與回應資料速率</span><span class="sxs-lookup"><span data-stu-id="a28ca-325">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="a28ca-326">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="a28ca-326">IIS content length limit</span></span>

<span data-ttu-id="a28ca-327">默認請求限制`maxAllowedContentLength`( ) 是 30,000,000 位元組,約為 28.6MB。</span><span class="sxs-lookup"><span data-stu-id="a28ca-327">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="a28ca-328">自訂*Web.config*檔案中的限制:</span><span class="sxs-lookup"><span data-stu-id="a28ca-328">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="a28ca-329">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="a28ca-329">This setting only applies to IIS.</span></span> <span data-ttu-id="a28ca-330">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="a28ca-330">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="a28ca-331">有關詳細資訊,請參閱[要求\<限制 請求限制>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-331">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="a28ca-332">ASP.NET核心模組或IIS請求篩選模組的存在的限制可能會將上載限制為2或4 GB。</span><span class="sxs-lookup"><span data-stu-id="a28ca-332">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="a28ca-333">有關詳細資訊,請參閱[無法上載大小大於 2GB 的檔(dotnet/AspNetCore #2711)。](https://github.com/dotnet/AspNetCore/issues/2711)</span><span class="sxs-lookup"><span data-stu-id="a28ca-333">For more information, see [Unable to upload file greater than 2GB in size (dotnet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="a28ca-334">疑難排解</span><span class="sxs-lookup"><span data-stu-id="a28ca-334">Troubleshoot</span></span>

<span data-ttu-id="a28ca-335">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="a28ca-335">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="a28ca-336">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="a28ca-336">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="a28ca-337">以下錯誤指示上載的檔案超過伺服器設定的內容長度:</span><span class="sxs-lookup"><span data-stu-id="a28ca-337">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="a28ca-338">有關增加限制的詳細資訊,請參閱[IIS內容長度限制](#iis-content-length-limit)部分。</span><span class="sxs-lookup"><span data-stu-id="a28ca-338">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="a28ca-339">連線失敗</span><span class="sxs-lookup"><span data-stu-id="a28ca-339">Connection failure</span></span>

<span data-ttu-id="a28ca-340">連接錯誤和重置伺服器連接可能表示上載的文件超過 Kestrel 的最大請求正文大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-340">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="a28ca-341">有關詳細資訊,請參閱[Kestrel 最大請求正文大小](#kestrel-maximum-request-body-size)部分。</span><span class="sxs-lookup"><span data-stu-id="a28ca-341">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="a28ca-342">Kestrel 用戶端連接限制可能還需要調整。</span><span class="sxs-lookup"><span data-stu-id="a28ca-342">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="a28ca-343">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="a28ca-343">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="a28ca-344">如果控制器<xref:Microsoft.AspNetCore.Http.IFormFile>正在使用 接受上載的檔案,但`null`值為 ,請確認`enctype`HTML`multipart/form-data`窗體指定的值 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-344">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="a28ca-345">如果未在`<form>`元素上設定此屬性,則不會發生檔案上載,並且任何繫<xref:Microsoft.AspNetCore.Http.IFormFile>結參數`null`都是 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-345">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="a28ca-346">您可以確認[表單資料中的上傳命名與套用的命名符合](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-346">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="a28ca-347">流太長</span><span class="sxs-lookup"><span data-stu-id="a28ca-347">Stream was too long</span></span>

<span data-ttu-id="a28ca-348">本主題中的範例依賴於<xref:System.IO.MemoryStream>保存上載的文件的內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-348">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="a28ca-349">的大小限制`MemoryStream`為`int.MaxValue`。</span><span class="sxs-lookup"><span data-stu-id="a28ca-349">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="a28ca-350">如果應用的檔上載方案需要保存大於 50 MB 的檔內容,請使用不`MemoryStream`依賴單個 檔案來保存上載檔內容的替代方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-350">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a28ca-351">ASP.NET Core 支援使用緩衝模型綁定上傳一個或多個檔,用於較小的檔,以及針對較大檔的未緩衝流。</span><span class="sxs-lookup"><span data-stu-id="a28ca-351">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="a28ca-352">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a28ca-352">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="a28ca-353">安全性考量</span><span class="sxs-lookup"><span data-stu-id="a28ca-353">Security considerations</span></span>

<span data-ttu-id="a28ca-354">在為使用者提供將檔上傳到伺服器的能力時,請謹慎。</span><span class="sxs-lookup"><span data-stu-id="a28ca-354">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="a28ca-355">攻擊者可能嘗試:</span><span class="sxs-lookup"><span data-stu-id="a28ca-355">Attackers may attempt to:</span></span>

* <span data-ttu-id="a28ca-356">執行[拒絕服務](/windows-hardware/drivers/ifs/denial-of-service)攻擊。</span><span class="sxs-lookup"><span data-stu-id="a28ca-356">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="a28ca-357">上傳病毒或惡意軟體。</span><span class="sxs-lookup"><span data-stu-id="a28ca-357">Upload viruses or malware.</span></span>
* <span data-ttu-id="a28ca-358">以其他方式危害網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="a28ca-358">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="a28ca-359">降低成功攻擊可能性的安全步驟包括:</span><span class="sxs-lookup"><span data-stu-id="a28ca-359">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="a28ca-360">將檔上載到專用檔上傳區域,最好將其上傳到非系統驅動器。</span><span class="sxs-lookup"><span data-stu-id="a28ca-360">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="a28ca-361">專用位置使對上載的文件實施安全限制變得更加容易。</span><span class="sxs-lookup"><span data-stu-id="a28ca-361">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="a28ca-362">禁用對檔上載位置執行許可權。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-362">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="a28ca-363">**不要**將上載的檔保留在同一目錄樹中。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-363">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="a28ca-364">使用由應用確定的安全檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-364">Use a safe file name determined by the app.</span></span> <span data-ttu-id="a28ca-365">不要使用使用者提供的檔案名或上傳檔的不受信任的檔名。&dagger; HTML 在顯示不受信任的檔名時對其進行編碼。</span><span class="sxs-lookup"><span data-stu-id="a28ca-365">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="a28ca-366">例如,在 UI 中記錄檔名或顯示(Razor 自動 HTML 編碼輸出)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-366">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="a28ca-367">僅允許應用設計規範的已批准文件副檔名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-367">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="a28ca-368">驗證在伺服器上是否執行客戶端檢查。&dagger;客戶端檢查很容易規避。</span><span class="sxs-lookup"><span data-stu-id="a28ca-368">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="a28ca-369">檢查上載檔的大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-369">Check the size of an uploaded file.</span></span> <span data-ttu-id="a28ca-370">設置最大大小限制以防止大型上載。&dagger;</span><span class="sxs-lookup"><span data-stu-id="a28ca-370">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="a28ca-371">當檔不應被同名上傳的文件覆蓋時,請在上載檔之前對照資料庫或物理存儲檢查檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-371">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="a28ca-372">**在存儲檔之前,對上載的內容運行病毒/惡意軟體掃描程式。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-372">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="a28ca-373">&dagger;示例應用演示了滿足條件的方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-373">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-374">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="a28ca-374">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="a28ca-375">完全控制系統。</span><span class="sxs-lookup"><span data-stu-id="a28ca-375">Completely gain control of a system.</span></span>
> * <span data-ttu-id="a28ca-376">使系統過載,導致系統崩潰。</span><span class="sxs-lookup"><span data-stu-id="a28ca-376">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="a28ca-377">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="a28ca-377">Compromise user or system data.</span></span>
> * <span data-ttu-id="a28ca-378">將塗鴉應用於公共 UI。</span><span class="sxs-lookup"><span data-stu-id="a28ca-378">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="a28ca-379">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="a28ca-379">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="a28ca-380">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="a28ca-380">Unrestricted File Upload</span></span>](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
> * [<span data-ttu-id="a28ca-381">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="a28ca-381">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="a28ca-382">有關實現安全措施的詳細資訊(包括示例應用的示例),請參閱[驗證](#validation)部分。</span><span class="sxs-lookup"><span data-stu-id="a28ca-382">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="a28ca-383">儲存機制</span><span class="sxs-lookup"><span data-stu-id="a28ca-383">Storage scenarios</span></span>

<span data-ttu-id="a28ca-384">檔案的常見記憶體選項包括:</span><span class="sxs-lookup"><span data-stu-id="a28ca-384">Common storage options for files include:</span></span>

* <span data-ttu-id="a28ca-385">資料庫</span><span class="sxs-lookup"><span data-stu-id="a28ca-385">Database</span></span>

  * <span data-ttu-id="a28ca-386">對於小型檔上載,資料庫通常比實體存儲(檔案系統或網路共用)選項更快。</span><span class="sxs-lookup"><span data-stu-id="a28ca-386">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="a28ca-387">資料庫通常比物理存儲選項更方便,因為檢索用戶數據的資料庫記錄可以同時提供文件內容(例如,頭像映射)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-387">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="a28ca-388">與使用數據存儲服務相比,資料庫的成本可能更低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-388">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="a28ca-389">實體儲存(檔案系統或網路分享)</span><span class="sxs-lookup"><span data-stu-id="a28ca-389">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="a28ca-390">對大型檔案上載:</span><span class="sxs-lookup"><span data-stu-id="a28ca-390">For large file uploads:</span></span>
    * <span data-ttu-id="a28ca-391">資料庫限制可能會限制上載的大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-391">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="a28ca-392">物理存儲通常比資料庫中的存儲經濟程度低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-392">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="a28ca-393">物理存儲可能比使用數據存儲服務的成本更低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-393">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="a28ca-394">應用的進程必須具有存儲位置的讀取和寫入許可權。</span><span class="sxs-lookup"><span data-stu-id="a28ca-394">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="a28ca-395">**切勿授予執行許可權。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-395">**Never grant execute permission.**</span></span>

* <span data-ttu-id="a28ca-396">資料儲存服務(例如[,Azure Blob 儲存](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="a28ca-396">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="a28ca-397">與通常單點故障的本地解決方案,服務通常提供改進的可擴充性和彈性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-397">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="a28ca-398">在大型存儲基礎結構方案中,服務的成本可能會更低。</span><span class="sxs-lookup"><span data-stu-id="a28ca-398">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="a28ca-399">有關詳細資訊,請參閱[快速入門:使用 .NET 在物件存儲中創建 Blob。](/azure/storage/blobs/storage-quickstart-blobs-dotnet)</span><span class="sxs-lookup"><span data-stu-id="a28ca-399">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="a28ca-400">本主題示範<xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>,<xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*>但可用於在使用<xref:System.IO.FileStream><xref:System.IO.Stream>時儲存到 blob 儲存。</span><span class="sxs-lookup"><span data-stu-id="a28ca-400">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="a28ca-401">檔案上傳機制</span><span class="sxs-lookup"><span data-stu-id="a28ca-401">File upload scenarios</span></span>

<span data-ttu-id="a28ca-402">上傳檔的兩種常規方法是緩衝和流式傳輸。</span><span class="sxs-lookup"><span data-stu-id="a28ca-402">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="a28ca-403">**緩衝**</span><span class="sxs-lookup"><span data-stu-id="a28ca-403">**Buffering**</span></span>

<span data-ttu-id="a28ca-404">整個檔被讀入一<xref:Microsoft.AspNetCore.Http.IFormFile>個 ,它是用於處理或保存檔的檔的 C# 表示形式。</span><span class="sxs-lookup"><span data-stu-id="a28ca-404">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="a28ca-405">檔上載使用的資源(磁碟、記憶體)取決於併發檔上載的數量和大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-405">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="a28ca-406">如果應用嘗試緩衝過多的上載,則當網站記憶體或磁碟空間不足時,該網站將崩潰。</span><span class="sxs-lookup"><span data-stu-id="a28ca-406">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="a28ca-407">如果檔上載的大小或頻率耗盡了應用資源,請使用流式處理。</span><span class="sxs-lookup"><span data-stu-id="a28ca-407">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="a28ca-408">任何超過 64 KB 的單個緩衝檔將從記憶體移動到磁碟上的臨時檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-408">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="a28ca-409">本主題的以下部分介紹了緩衝小檔:</span><span class="sxs-lookup"><span data-stu-id="a28ca-409">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="a28ca-410">實體儲存</span><span class="sxs-lookup"><span data-stu-id="a28ca-410">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="a28ca-411">Database</span><span class="sxs-lookup"><span data-stu-id="a28ca-411">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="a28ca-412">**串流**</span><span class="sxs-lookup"><span data-stu-id="a28ca-412">**Streaming**</span></span>

<span data-ttu-id="a28ca-413">該檔從多部分請求接收,並由應用直接處理或保存。</span><span class="sxs-lookup"><span data-stu-id="a28ca-413">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="a28ca-414">流式處理不會顯著提高性能。</span><span class="sxs-lookup"><span data-stu-id="a28ca-414">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="a28ca-415">流式傳輸減少了上傳檔時對記憶體或磁碟空間的需求。</span><span class="sxs-lookup"><span data-stu-id="a28ca-415">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="a28ca-416">流式處理大型檔在[「上傳包含流式處理的大型檔](#upload-large-files-with-streaming)」部分中介紹。</span><span class="sxs-lookup"><span data-stu-id="a28ca-416">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="a28ca-417">將具有緩衝模型的小型檔案載入物理儲存</span><span class="sxs-lookup"><span data-stu-id="a28ca-417">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="a28ca-418">要上載小檔,請使用多部分窗體或使用 JAVAScript 構造 POST 請求。</span><span class="sxs-lookup"><span data-stu-id="a28ca-418">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="a28ca-419">下面的範例展示使用 Razor Pages 表單上載單檔案(例如範*單中的網頁/緩衝單檔案上傳物理.cshtml):*</span><span class="sxs-lookup"><span data-stu-id="a28ca-419">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="a28ca-420">以下示例與以前的示例類似,只不過:</span><span class="sxs-lookup"><span data-stu-id="a28ca-420">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="a28ca-421">JavaScript 的 ([提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) 用於提交表單的資料。</span><span class="sxs-lookup"><span data-stu-id="a28ca-421">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="a28ca-422">沒有驗證。</span><span class="sxs-lookup"><span data-stu-id="a28ca-422">There's no validation.</span></span>

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

<span data-ttu-id="a28ca-423">要對[不支援 Fetch API 的](https://caniuse.com/#feat=fetch)用戶端在 JavaScript 執行表單 POST,請使用以下方法之一:</span><span class="sxs-lookup"><span data-stu-id="a28ca-423">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="a28ca-424">使用「擷取」多邊形填充(例如,[視窗.fetch 多邊填充(github/提取)。](https://github.com/github/fetch)</span><span class="sxs-lookup"><span data-stu-id="a28ca-424">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="a28ca-425">使用 `XMLHttpRequest`。</span><span class="sxs-lookup"><span data-stu-id="a28ca-425">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="a28ca-426">例如：</span><span class="sxs-lookup"><span data-stu-id="a28ca-426">For example:</span></span>

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

<span data-ttu-id="a28ca-427">為了支援檔案中載,HTML 窗型必須指定的編碼型`enctype`態`multipart/form-data`( )</span><span class="sxs-lookup"><span data-stu-id="a28ca-427">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="a28ca-428">對於支援`files`上載多個檔案的輸入元素,`multiple``<input>`該 元素在元素上提供屬性:</span><span class="sxs-lookup"><span data-stu-id="a28ca-428">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="a28ca-429">上傳到伺服器的各個檔案可以使用[模型連結](xref:mvc/models/model-binding)<xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="a28ca-429">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="a28ca-430">範例應用示範了多個用於資料庫和實體存儲方案的緩衝檔上載。</span><span class="sxs-lookup"><span data-stu-id="a28ca-430">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="a28ca-431">**請勿**`FileName`使用 顯示和日<xref:Microsoft.AspNetCore.Http.IFormFile>誌記錄 以外的屬性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-431">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="a28ca-432">顯示或紀錄記錄時,HTML 對檔案名進行編碼。</span><span class="sxs-lookup"><span data-stu-id="a28ca-432">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="a28ca-433">攻擊者可以提供惡意檔名,包括完整路徑或相對路徑。</span><span class="sxs-lookup"><span data-stu-id="a28ca-433">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="a28ca-434">應用程式應:</span><span class="sxs-lookup"><span data-stu-id="a28ca-434">Applications should:</span></span>
>
> * <span data-ttu-id="a28ca-435">從使用者提供的檔名中刪除路徑。</span><span class="sxs-lookup"><span data-stu-id="a28ca-435">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="a28ca-436">儲存 HTML 編碼的路徑刪除的檔名,用於 UI 或紀錄記錄。</span><span class="sxs-lookup"><span data-stu-id="a28ca-436">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="a28ca-437">生成新的隨機檔名進行存儲。</span><span class="sxs-lookup"><span data-stu-id="a28ca-437">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="a28ca-438">以下代碼從檔案名中刪除路徑:</span><span class="sxs-lookup"><span data-stu-id="a28ca-438">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="a28ca-439">到目前為止提供的示例沒有考慮到安全注意事項。</span><span class="sxs-lookup"><span data-stu-id="a28ca-439">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="a28ca-440">其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:</span><span class="sxs-lookup"><span data-stu-id="a28ca-440">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="a28ca-441">安全性考量</span><span class="sxs-lookup"><span data-stu-id="a28ca-441">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="a28ca-442">驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-442">Validation</span></span>](#validation)

<span data-ttu-id="a28ca-443">使用模型綁定<xref:Microsoft.AspNetCore.Http.IFormFile>和 上載檔時,操作方法可以接受:</span><span class="sxs-lookup"><span data-stu-id="a28ca-443">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="a28ca-444">單個<xref:Microsoft.AspNetCore.Http.IFormFile>。</span><span class="sxs-lookup"><span data-stu-id="a28ca-444">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="a28ca-445">表示多個檔案的以下任何集合:</span><span class="sxs-lookup"><span data-stu-id="a28ca-445">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="a28ca-446">[清單](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="a28ca-446">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="a28ca-447">綁定依名稱匹配表單檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-447">Binding matches form files by name.</span></span> <span data-ttu-id="a28ca-448">例如,中的`name``<input type="file" name="formFile">`HTML 值必須與 C# 參數`FormFile`/ 屬性繫結 ( ) 符合。</span><span class="sxs-lookup"><span data-stu-id="a28ca-448">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="a28ca-449">關於詳細資訊,請參閱 POST[方法部份的參數名稱的比名稱屬性值](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-449">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="a28ca-450">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="a28ca-450">The following example:</span></span>

* <span data-ttu-id="a28ca-451">循環訪問一個或多個上傳的檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-451">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="a28ca-452">使用[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)傳回檔案的完整路徑,包括檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-452">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="a28ca-453">使用應用生成的檔案名將檔儲存到本地檔案系統。</span><span class="sxs-lookup"><span data-stu-id="a28ca-453">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="a28ca-454">返回上載的檔的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-454">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="a28ca-455">用於`Path.GetRandomFileName`生成沒有路徑的檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-455">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="a28ca-456">在下面的範例中,路徑是從設定中取得的:</span><span class="sxs-lookup"><span data-stu-id="a28ca-456">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="a28ca-457">傳遞到<xref:System.IO.FileStream>的路徑*必須*包括檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-457">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="a28ca-458">如果未提供檔案名稱,則在執行時引發<xref:System.UnauthorizedAccessException>。</span><span class="sxs-lookup"><span data-stu-id="a28ca-458">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="a28ca-459">在處理之前,使用<xref:Microsoft.AspNetCore.Http.IFormFile>該技術上載的檔將緩衝在記憶體中或伺服器上的磁碟上。</span><span class="sxs-lookup"><span data-stu-id="a28ca-459">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="a28ca-460">在操作方法中,<xref:Microsoft.AspNetCore.Http.IFormFile>內容可作為<xref:System.IO.Stream>可訪問。</span><span class="sxs-lookup"><span data-stu-id="a28ca-460">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="a28ca-461">除了本地檔案系統之外,檔案還可以保存到網路共享或檔案存儲服務(如[Azure Blob 儲存](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs))。</span><span class="sxs-lookup"><span data-stu-id="a28ca-461">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="a28ca-462">有關迴圈存取多個檔以進行上載並使用安全檔名的範例,請參閱範例應用中*的頁面/緩衝倍數檔檔物理.cshtml.cs。*</span><span class="sxs-lookup"><span data-stu-id="a28ca-462">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-463">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)<xref:System.IO.IOException>會引發一個,如果創建了超過 65,535 個檔,而不刪除以前的臨時檔。</span><span class="sxs-lookup"><span data-stu-id="a28ca-463">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="a28ca-464">65,535 個檔的限制是每個伺服器的限制。</span><span class="sxs-lookup"><span data-stu-id="a28ca-464">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="a28ca-465">有關 Windows 作業系統上此限制的詳細資訊,請參閱以下主題中的備註:</span><span class="sxs-lookup"><span data-stu-id="a28ca-465">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="a28ca-466">取得tempFilenameA函式</span><span class="sxs-lookup"><span data-stu-id="a28ca-466">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="a28ca-467">將具有緩衝模型的小檔案載到資料庫</span><span class="sxs-lookup"><span data-stu-id="a28ca-467">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="a28ca-468">要使用[實體框架](/ef/core/index)將二進位檔案資料儲存在資料庫中,<xref:System.Byte>請在實體上定義數位屬性:</span><span class="sxs-lookup"><span data-stu-id="a28ca-468">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="a28ca-469">指定的類別指定頁面模型屬性<xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="a28ca-469">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="a28ca-470"><xref:Microsoft.AspNetCore.Http.IFormFile>可以直接用作操作方法參數或綁定模型屬性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-470"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="a28ca-471">上一個示例使用綁定模型屬性。</span><span class="sxs-lookup"><span data-stu-id="a28ca-471">The prior example uses a bound model property.</span></span>

<span data-ttu-id="a28ca-472">在`FileUpload`剃刀頁窗體中使用:</span><span class="sxs-lookup"><span data-stu-id="a28ca-472">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="a28ca-473">將表單 POSTed 複製到伺服器時<xref:Microsoft.AspNetCore.Http.IFormFile>,將複製到流並將其另存為資料庫中的位元組。</span><span class="sxs-lookup"><span data-stu-id="a28ca-473">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="a28ca-474">在以下範例中,`_dbContext`儲存應用的資料庫內容:</span><span class="sxs-lookup"><span data-stu-id="a28ca-474">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="a28ca-475">前面的範例類似於範例應用中展示的方案:</span><span class="sxs-lookup"><span data-stu-id="a28ca-475">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="a28ca-476">*頁面/緩衝單檔案上傳Db.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a28ca-476">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="a28ca-477">*頁面/緩衝單檔案上傳Db.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a28ca-477">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-478">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="a28ca-478">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="a28ca-479">未經驗證,不要依賴或信任`FileName`的屬性。 <xref:Microsoft.AspNetCore.Http.IFormFile></span><span class="sxs-lookup"><span data-stu-id="a28ca-479">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="a28ca-480">該`FileName`屬性應僅用於顯示目的,並且僅在 HTML 編碼後使用。</span><span class="sxs-lookup"><span data-stu-id="a28ca-480">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="a28ca-481">提供的示例不考慮安全注意事項。</span><span class="sxs-lookup"><span data-stu-id="a28ca-481">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="a28ca-482">其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:</span><span class="sxs-lookup"><span data-stu-id="a28ca-482">Additional information is provided by the following sections and the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="a28ca-483">安全性考量</span><span class="sxs-lookup"><span data-stu-id="a28ca-483">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="a28ca-484">驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-484">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="a28ca-485">使用流式上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="a28ca-485">Upload large files with streaming</span></span>

<span data-ttu-id="a28ca-486">下面的範例展示如何使用 JavaScript 將檔案流式傳輸到控制器操作。</span><span class="sxs-lookup"><span data-stu-id="a28ca-486">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="a28ca-487">該檔的反偽造權杖使用自定義篩選器屬性生成,並傳遞到客戶端 HTTP 標頭而不是請求正文中。</span><span class="sxs-lookup"><span data-stu-id="a28ca-487">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="a28ca-488">由於操作方法直接處理上載的數據,因此表單模型綁定被另一個自定義篩選器禁用。</span><span class="sxs-lookup"><span data-stu-id="a28ca-488">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="a28ca-489">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-489">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="a28ca-490">讀取多部分部分后,操作將執行其自己的模型綁定。</span><span class="sxs-lookup"><span data-stu-id="a28ca-490">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="a28ca-491">初始頁面回應載入表單,並將反偽造權杖保存在 Cookie 中`GenerateAntiforgeryTokenCookieAttribute`(通過屬性 )。</span><span class="sxs-lookup"><span data-stu-id="a28ca-491">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="a28ca-492">該屬性使用 ASP.NET Core 的內建[反偽造支援](xref:security/anti-request-forgery)來設定具有請求權杖的 Cookie:</span><span class="sxs-lookup"><span data-stu-id="a28ca-492">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="a28ca-493">`DisableFormValueModelBindingAttribute`關閉模型的系統:</span><span class="sxs-lookup"><span data-stu-id="a28ca-493">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="a28ca-494">在`GenerateAntiforgeryTokenCookieAttribute`範例應用中,並`DisableFormValueModelBindingAttribute`作為篩選器應用`/StreamedSingleFileUploadDb``/StreamedSingleFileUploadPhysical``Startup.ConfigureServices`於使用 Razor Pages 約定的 頁面應用程式模型和 中的[頁面](xref:razor-pages/razor-pages-conventions)應用程式模型:</span><span class="sxs-lookup"><span data-stu-id="a28ca-494">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="a28ca-495">由於模型綁定不讀取窗體,因此從窗體綁定的參數不會綁定(查詢、路由和標頭繼續工作)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-495">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="a28ca-496">操作方法直接與 屬性`Request`一起工作。</span><span class="sxs-lookup"><span data-stu-id="a28ca-496">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="a28ca-497">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="a28ca-497">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="a28ca-498">鍵/數值資料儲存在`KeyValueAccumulator`中 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-498">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="a28ca-499">讀取多部分部分後,使用的內容`KeyValueAccumulator`將窗體數據綁定到模型類型。</span><span class="sxs-lookup"><span data-stu-id="a28ca-499">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="a28ca-500">使用`StreamingController.UploadDatabase`EF Core 串流式傳輸到資料庫的完整方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-500">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="a28ca-501">`MultipartRequestHelper`(*公用程式/ 多部份要求説明者.cs*):</span><span class="sxs-lookup"><span data-stu-id="a28ca-501">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="a28ca-502">串流式傳輸`StreamingController.UploadPhysical`到 物理位置的完整方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-502">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="a28ca-503">在範例應用中,驗證檢查由`FileHelpers.ProcessStreamedFile`處理 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-503">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="a28ca-504">驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-504">Validation</span></span>

<span data-ttu-id="a28ca-505">示例應用的`FileHelpers`類演示了對緩<xref:Microsoft.AspNetCore.Http.IFormFile>衝 和流式檔上載的多次檢查。</span><span class="sxs-lookup"><span data-stu-id="a28ca-505">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="a28ca-506">有關在<xref:Microsoft.AspNetCore.Http.IFormFile>示例應用中處理緩衝檔上傳,請參閱`ProcessFormFile`*實用程式/FileHelpers.cs*檔中的方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-506">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="a28ca-507">有關處理流式處理檔,`ProcessStreamedFile`請參閱同一檔中的方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-507">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="a28ca-508">範例應用中演示的驗證處理方法不會掃描上載文件的內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-508">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="a28ca-509">在大多數生產方案中,在將檔可供使用者或其他系統使用之前,在檔上使用病毒/惡意軟體掃描器 API。</span><span class="sxs-lookup"><span data-stu-id="a28ca-509">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="a28ca-510">儘管主題示例提供了驗證技術的工作示例,但除非在生產應用中實現`FileHelpers`類,否則不要實現該類:</span><span class="sxs-lookup"><span data-stu-id="a28ca-510">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="a28ca-511">完全理解實施情況。</span><span class="sxs-lookup"><span data-stu-id="a28ca-511">Fully understand the implementation.</span></span>
> * <span data-ttu-id="a28ca-512">根據需要修改應用的環境和規範的實現。</span><span class="sxs-lookup"><span data-stu-id="a28ca-512">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="a28ca-513">**如果不滿足這些要求,切勿在應用中不加區別地實施安全代碼。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-513">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="a28ca-514">內容驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-514">Content validation</span></span>

<span data-ttu-id="a28ca-515">**對上傳的內容使用第三方病毒/惡意軟體掃描 API。**</span><span class="sxs-lookup"><span data-stu-id="a28ca-515">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="a28ca-516">在高容量方案中,掃描檔對伺服器資源的要求很高。</span><span class="sxs-lookup"><span data-stu-id="a28ca-516">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="a28ca-517">如果由於文件掃描而使請求處理性能降低,請考慮將掃描工作卸載到[後台服務](xref:fundamentals/host/hosted-services),可能是在伺服器上運行的服務不同於應用的伺服器。</span><span class="sxs-lookup"><span data-stu-id="a28ca-517">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="a28ca-518">通常,上傳的檔在隔離區域中,直到背景病毒掃描程式檢查它們。</span><span class="sxs-lookup"><span data-stu-id="a28ca-518">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="a28ca-519">當檔通過時,檔將移動到正常檔案存儲位置。</span><span class="sxs-lookup"><span data-stu-id="a28ca-519">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="a28ca-520">這些步驟通常與指示檔的掃描狀態的資料庫記錄一起執行。</span><span class="sxs-lookup"><span data-stu-id="a28ca-520">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="a28ca-521">通過使用這種方法,應用和應用程式伺服器仍然專注於回應請求。</span><span class="sxs-lookup"><span data-stu-id="a28ca-521">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="a28ca-522">檔案副檔名驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-522">File extension validation</span></span>

<span data-ttu-id="a28ca-523">應根據允許的擴展清單檢查上載的檔案副檔名。</span><span class="sxs-lookup"><span data-stu-id="a28ca-523">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="a28ca-524">例如：</span><span class="sxs-lookup"><span data-stu-id="a28ca-524">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="a28ca-525">檔案簽章驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-525">File signature validation</span></span>

<span data-ttu-id="a28ca-526">檔的簽名由檔開頭的前幾個位元組決定。</span><span class="sxs-lookup"><span data-stu-id="a28ca-526">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="a28ca-527">這些位元組可用於指示擴展名是否與文件的內容匹配。</span><span class="sxs-lookup"><span data-stu-id="a28ca-527">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="a28ca-528">範例應用檢查檔簽名中有幾個常見的文件類型。</span><span class="sxs-lookup"><span data-stu-id="a28ca-528">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="a28ca-529">在下面的範例中,針對該檔案檢查 JPEG 影像的檔案簽名:</span><span class="sxs-lookup"><span data-stu-id="a28ca-529">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="a28ca-530">要獲取其他文件簽名,請參閱[文件簽名資料庫](https://www.filesignatures.net/)和官方文件規範。</span><span class="sxs-lookup"><span data-stu-id="a28ca-530">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="a28ca-531">檔案名稱安全性</span><span class="sxs-lookup"><span data-stu-id="a28ca-531">File name security</span></span>

<span data-ttu-id="a28ca-532">切勿使用用戶端提供的檔案名將檔儲存到物理存儲。</span><span class="sxs-lookup"><span data-stu-id="a28ca-532">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="a28ca-533">使用[Path.GetRandomFilename](xref:System.IO.Path.GetRandomFileName*)或[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)為檔案建立安全性的檔名,以創建用於暫時儲存的完整路徑(包括檔名)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-533">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="a28ca-534">Razor 會自動對顯示的屬性值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="a28ca-534">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="a28ca-535">以下代碼可安全使用:</span><span class="sxs-lookup"><span data-stu-id="a28ca-535">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="a28ca-536">在 Razor<xref:System.Net.WebUtility.HtmlEncode*>之外 ,始終從使用者請求中提交姓名內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-536">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="a28ca-537">許多實現必須包括檢查該檔是否存在;否則,該檔將被同名文件覆蓋。</span><span class="sxs-lookup"><span data-stu-id="a28ca-537">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="a28ca-538">提供其他邏輯以滿足應用的規範。</span><span class="sxs-lookup"><span data-stu-id="a28ca-538">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="a28ca-539">大小驗證</span><span class="sxs-lookup"><span data-stu-id="a28ca-539">Size validation</span></span>

<span data-ttu-id="a28ca-540">限制上載檔的大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-540">Limit the size of uploaded files.</span></span>

<span data-ttu-id="a28ca-541">在範例應用中,檔的大小限制為2 MB(以位元組為單位)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-541">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="a28ca-542">限制透過來自*應用程式設定.json*檔的[設定](xref:fundamentals/configuration/index)提供:</span><span class="sxs-lookup"><span data-stu-id="a28ca-542">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="a28ca-543">註`FileSizeLimit`入 :`PageModel`</span><span class="sxs-lookup"><span data-stu-id="a28ca-543">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="a28ca-544">當檔案大小超過限制時,檔案將被拒絕:</span><span class="sxs-lookup"><span data-stu-id="a28ca-544">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="a28ca-545">將名稱屬性值與 POST 方法的參數名稱符合</span><span class="sxs-lookup"><span data-stu-id="a28ca-545">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="a28ca-546">在 POST 形成資料或使用 JavaScript`FormData`的非 Razor 窗體中,在窗體`FormData`元素中指定的名稱或 必須與控制器操作中參數的名稱匹配。</span><span class="sxs-lookup"><span data-stu-id="a28ca-546">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="a28ca-547">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="a28ca-547">In the following example:</span></span>

* <span data-ttu-id="a28ca-548">使用`<input>`元素時,屬性`name`設定為值`battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="a28ca-548">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="a28ca-549">`FormData`在 JavaScript 中使用時,名稱設定`battlePlans`為 :</span><span class="sxs-lookup"><span data-stu-id="a28ca-549">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="a28ca-550">描述 C# 方法的參數使用符合`battlePlans`名稱 ( :</span><span class="sxs-lookup"><span data-stu-id="a28ca-550">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="a28ca-551">對於名為的`Upload`Razor 頁面頁面處理程式方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-551">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="a28ca-552">對於 MVC POST 控制器操作方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-552">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="a28ca-553">伺服器和應用程式設定</span><span class="sxs-lookup"><span data-stu-id="a28ca-553">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="a28ca-554">多部份體長度限制</span><span class="sxs-lookup"><span data-stu-id="a28ca-554">Multipart body length limit</span></span>

<span data-ttu-id="a28ca-555"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>設置每個多部分實體的長度限制。</span><span class="sxs-lookup"><span data-stu-id="a28ca-555"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="a28ca-556">超過此限制的表單的元件在<xref:System.IO.InvalidDataException>分析 時會引發 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-556">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="a28ca-557">默認值為 134,217,728 (128 MB)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-557">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="a28ca-558">使用的<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>`Startup.ConfigureServices`設定自訂限制:</span><span class="sxs-lookup"><span data-stu-id="a28ca-558">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="a28ca-559"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>為單一頁面或<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>操作設定 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-559"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="a28ca-560">在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:</span><span class="sxs-lookup"><span data-stu-id="a28ca-560">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="a28ca-561">在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面模型或操作方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-561">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="a28ca-562">凱斯特雷爾最大請求正文大小</span><span class="sxs-lookup"><span data-stu-id="a28ca-562">Kestrel maximum request body size</span></span>

<span data-ttu-id="a28ca-563">對於 Kestrel 託管的應用,預設的最大請求正文大小為 30,000,000 位元組,約為 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="a28ca-563">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="a28ca-564">使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制:</span><span class="sxs-lookup"><span data-stu-id="a28ca-564">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="a28ca-565"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>用於為單個頁面或操作設置[最大請求博量。](xref:fundamentals/servers/kestrel#maximum-request-body-size)</span><span class="sxs-lookup"><span data-stu-id="a28ca-565"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="a28ca-566">在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:</span><span class="sxs-lookup"><span data-stu-id="a28ca-566">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="a28ca-567">在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面處理程式類或操作方法:</span><span class="sxs-lookup"><span data-stu-id="a28ca-567">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="a28ca-568">其他凱斯特雷爾限制</span><span class="sxs-lookup"><span data-stu-id="a28ca-568">Other Kestrel limits</span></span>

<span data-ttu-id="a28ca-569">其他凱斯特雷爾限制可能適用於由凱斯特雷爾託管的應用程式:</span><span class="sxs-lookup"><span data-stu-id="a28ca-569">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="a28ca-570">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="a28ca-570">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="a28ca-571">要求與回應資料速率</span><span class="sxs-lookup"><span data-stu-id="a28ca-571">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="a28ca-572">IIS 內容長度限制</span><span class="sxs-lookup"><span data-stu-id="a28ca-572">IIS content length limit</span></span>

<span data-ttu-id="a28ca-573">默認請求限制`maxAllowedContentLength`( ) 是 30,000,000 位元組,約為 28.6MB。</span><span class="sxs-lookup"><span data-stu-id="a28ca-573">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="a28ca-574">自訂*Web.config*檔案中的限制:</span><span class="sxs-lookup"><span data-stu-id="a28ca-574">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="a28ca-575">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="a28ca-575">This setting only applies to IIS.</span></span> <span data-ttu-id="a28ca-576">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="a28ca-576">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="a28ca-577">有關詳細資訊,請參閱[要求\<限制 請求限制>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-577">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="a28ca-578">ASP.NET核心模組或IIS請求篩選模組的存在的限制可能會將上載限制為2或4 GB。</span><span class="sxs-lookup"><span data-stu-id="a28ca-578">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="a28ca-579">有關詳細資訊,請參閱[無法上載大小大於 2GB 的檔(dotnet/AspNetCore #2711)。](https://github.com/dotnet/AspNetCore/issues/2711)</span><span class="sxs-lookup"><span data-stu-id="a28ca-579">For more information, see [Unable to upload file greater than 2GB in size (dotnet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="a28ca-580">疑難排解</span><span class="sxs-lookup"><span data-stu-id="a28ca-580">Troubleshoot</span></span>

<span data-ttu-id="a28ca-581">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="a28ca-581">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="a28ca-582">部署到 IIS 伺服器時找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="a28ca-582">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="a28ca-583">以下錯誤指示上載的檔案超過伺服器設定的內容長度:</span><span class="sxs-lookup"><span data-stu-id="a28ca-583">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="a28ca-584">有關增加限制的詳細資訊,請參閱[IIS內容長度限制](#iis-content-length-limit)部分。</span><span class="sxs-lookup"><span data-stu-id="a28ca-584">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="a28ca-585">連線失敗</span><span class="sxs-lookup"><span data-stu-id="a28ca-585">Connection failure</span></span>

<span data-ttu-id="a28ca-586">連接錯誤和重置伺服器連接可能表示上載的文件超過 Kestrel 的最大請求正文大小。</span><span class="sxs-lookup"><span data-stu-id="a28ca-586">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="a28ca-587">有關詳細資訊,請參閱[Kestrel 最大請求正文大小](#kestrel-maximum-request-body-size)部分。</span><span class="sxs-lookup"><span data-stu-id="a28ca-587">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="a28ca-588">Kestrel 用戶端連接限制可能還需要調整。</span><span class="sxs-lookup"><span data-stu-id="a28ca-588">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="a28ca-589">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="a28ca-589">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="a28ca-590">如果控制器<xref:Microsoft.AspNetCore.Http.IFormFile>正在使用 接受上載的檔案,但`null`值為 ,請確認`enctype`HTML`multipart/form-data`窗體指定的值 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-590">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="a28ca-591">如果未在`<form>`元素上設定此屬性,則不會發生檔案上載,並且任何繫<xref:Microsoft.AspNetCore.Http.IFormFile>結參數`null`都是 。</span><span class="sxs-lookup"><span data-stu-id="a28ca-591">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="a28ca-592">您可以確認[表單資料中的上傳命名與套用的命名符合](#match-name-attribute-value-to-parameter-name-of-post-method)。</span><span class="sxs-lookup"><span data-stu-id="a28ca-592">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="a28ca-593">流太長</span><span class="sxs-lookup"><span data-stu-id="a28ca-593">Stream was too long</span></span>

<span data-ttu-id="a28ca-594">本主題中的範例依賴於<xref:System.IO.MemoryStream>保存上載的文件的內容。</span><span class="sxs-lookup"><span data-stu-id="a28ca-594">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="a28ca-595">的大小限制`MemoryStream`為`int.MaxValue`。</span><span class="sxs-lookup"><span data-stu-id="a28ca-595">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="a28ca-596">如果應用的檔上載方案需要保存大於 50 MB 的檔內容,請使用不`MemoryStream`依賴單個 檔案來保存上載檔內容的替代方法。</span><span class="sxs-lookup"><span data-stu-id="a28ca-596">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="a28ca-597">其他資源</span><span class="sxs-lookup"><span data-stu-id="a28ca-597">Additional resources</span></span>

* [<span data-ttu-id="a28ca-598">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="a28ca-598">Unrestricted File Upload</span></span>](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
* [<span data-ttu-id="a28ca-599">Azure 安全性:安全框架:輸入驗證 |緩解措施</span><span class="sxs-lookup"><span data-stu-id="a28ca-599">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="a28ca-600">Azure 雲設計模式:代客金鑰模式</span><span class="sxs-lookup"><span data-stu-id="a28ca-600">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
