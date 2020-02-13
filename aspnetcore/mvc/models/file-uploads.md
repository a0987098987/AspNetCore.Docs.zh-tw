---
title: 上傳 ASP.NET Core 中的檔案
author: guardrex
description: 如何使用模型繫結和資料流在 ASP.NET Core MVC 上傳檔案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2020
uid: mvc/models/file-uploads
ms.openlocfilehash: 56fd26c1864089558f5cd89f693dc86ea30c3331
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172464"
---
# <a name="upload-files-in-aspnet-core"></a>上傳 ASP.NET Core 中的檔案

作者： [Luke Latham](https://github.com/guardrex)、 [Steve Smith](https://ardalis.com/)和[Rutger 風暴](https://github.com/rutix)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>安全性考量

提供使用者將檔案上傳到伺服器的能力時，請務必小心。 攻擊者可能會嘗試：

* 執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。
* 上傳病毒或惡意程式碼。
* 以其他方式危害網路和伺服器。

降低成功攻擊的可能性的安全性步驟如下：

* 將檔案上傳到專用的檔案上傳區域，最好是在非系統磁片磁碟機上。 專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。 停用檔案上傳位置的 execute 許可權。&dagger;
* 請勿將上傳的**檔案保存在**與應用程式相同的目錄樹狀結構中。&dagger;
* 使用應用程式所決定的安全檔案名。 請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。&dagger; HTML 在顯示不受信任的檔案名時進行編碼。 例如，記錄檔案名或在 UI 中顯示（Razor 會自動以 HTML 編碼輸出）。
* 僅允許應用程式設計規格的已核准副檔名。&dagger; <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* 確認用戶端檢查是在伺服器上執行。&dagger; 用戶端檢查很容易規避。
* 檢查上傳檔案的大小。 設定最大大小限制以防止大量上傳。&dagger;
* 當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。
* **在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**

&dagger;範例應用程式示範符合準則的方法。

> [!WARNING]
> 將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：
>
> * 完全取得系統的控制權。
> * 使用系統損毀的結果來多載系統。
> * 洩漏使用者或系統資料。
> * 將刻套用至公用 UI。
>
> 如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：
>
> * [不受限制的檔案上傳](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。

## <a name="storage-scenarios"></a>儲存案例

檔案的一般儲存選項包括：

* 資料庫

  * 針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。
  * 資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。
  * 資料庫的成本可能比使用資料儲存體服務來得低。

* 實體存放裝置（檔案系統或網路共用）

  * 針對大型檔案上傳：
    * 資料庫限制可能會限制上傳的大小。
    * 實體儲存體通常比在資料庫中儲存的成本低。
  * 實體儲存體的成本可能比使用資料儲存體服務來得低。
  * 應用程式的進程必須具有儲存位置的讀取和寫入權限。 **絕對不要授與 execute 許可權。**

* 資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）

  * 服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。
  * 在大型儲存體基礎結構案例中，服務可能會降低成本。

  如需詳細資訊，請參閱[快速入門：使用 .net 在物件儲存體中建立 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。

## <a name="file-upload-scenarios"></a>檔案上傳案例

上傳檔案的兩個一般方法是緩衝和串流處理。

**緩衝**

系統會將整個檔案讀入 <xref:Microsoft.AspNetCore.Http.IFormFile>，這是用來C#處理或儲存檔案的檔案標記法。

檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。 如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。 如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。

> [!NOTE]
> 任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。

此主題的下列各節會涵蓋緩衝處理小型檔案：

* [實體儲存體](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [Database](#upload-small-files-with-buffered-model-binding-to-a-database)

**串流**

此檔案會從多部分要求接收，並由應用程式直接處理或儲存。 串流不會大幅改善效能。 串流處理可減少上傳檔案時記憶體或磁碟空間的需求。

[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a>將具有緩衝模型系結的小型檔案上傳至實體儲存體

若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。

下列範例示範如何使用 Razor Pages 表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：

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

下列範例類似于先前的範例，不同之處在于：

* JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。
* 沒有驗證。

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

若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：

* 使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。
* 使用 `XMLHttpRequest`。 例如，

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

為了支援檔案上傳，HTML 表單必須指定 `multipart/form-data`的編碼類型（`enctype`）。

若要支援上傳多個檔案的 `files` input 元素，請在 `<input>` 元素上提供 `multiple` 屬性：

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

您可以使用 <xref:Microsoft.AspNetCore.Http.IFormFile>，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案。 範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。

<a name="filename"></a>

> [!WARNING]
> 除了顯示和記錄之外，**請勿使用 <xref:Microsoft.AspNetCore.Http.IFormFile>** 的 `FileName` 屬性。 當顯示或記錄時，HTML 會將檔案名編碼。 攻擊者可以提供惡意的檔案名，包括完整路徑或相對路徑。 應用程式應該：
>
> * 從使用者提供的檔案名中移除路徑。
> * 針對 UI 或記錄儲存以 HTML 編碼、路徑移除的檔案名。
> * 為儲存區產生新的隨機檔案名。
>
> 下列程式碼會從檔案名中移除路徑：
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> 到目前為止所提供的範例並不考慮安全性考慮。 下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

使用模型系結和 <xref:Microsoft.AspNetCore.Http.IFormFile>上傳檔案時，動作方法可以接受：

* 單一 <xref:Microsoft.AspNetCore.Http.IFormFile>。
* 下列任何代表數個檔案的集合：
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * [列出](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>

> [!NOTE]
> 系結符合依名稱的表單檔案。 例如，`<input type="file" name="formFile">` 中的 HTML `name` 值必須符合C#參數/屬性系結（`FormFile`）。 如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。

下列範例︰

* 迴圈一或多個已上傳的檔案。
* 會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。 
* 使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。
* 傳回已上傳檔案的總數和大小。

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

使用 `Path.GetRandomFileName` 產生不含路徑的檔案名。 在下列範例中，路徑是從設定取得：

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

傳遞至 <xref:System.IO.FileStream> 的路徑*必須*包含檔案名。 如果未提供檔案名，則會在執行時間擲回 <xref:System.UnauthorizedAccessException>。

使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 技術所上傳的檔案會在處理之前，先緩衝到伺服器上的記憶體或磁片上。 在動作方法內，<xref:Microsoft.AspNetCore.Http.IFormFile> 內容可以 <xref:System.IO.Stream>存取。 除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。

如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。

> [!WARNING]
> 如果建立的檔案超過65535，而未刪除先前的暫存檔案，則[GetTempFileName](xref:System.IO.Path.GetTempFileName*)會擲回 <xref:System.IO.IOException>。 65535檔案的限制是每一伺服器的限制。 如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：
>
> * [GetTempFileNameA 函式](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a>將具有緩衝模型系結的小型檔案上傳至資料庫

若要使用[Entity Framework](/ef/core/index)在資料庫中儲存二進位檔案資料，請在實體上定義 <xref:System.Byte> 陣列屬性：

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

指定包含 <xref:Microsoft.AspNetCore.Http.IFormFile>之類別的 [頁面模型] 屬性：

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
> <xref:Microsoft.AspNetCore.Http.IFormFile> 可以直接當做動作方法參數或系結模型屬性來使用。 先前的範例會使用系結模型屬性。

`FileUpload` 用於 Razor Pages 形式：

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

當表單張貼至伺服器時，請將 <xref:Microsoft.AspNetCore.Http.IFormFile> 複製到資料流程，並將其儲存為資料庫中的位元組陣列。 在下列範例中，`_dbContext` 會儲存應用程式的資料庫內容：

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

前述範例類似于範例應用程式中所示範的案例：

* *Pages/BufferedSingleFileUploadDb. cshtml*
* *Pages/BufferedSingleFileUploadDb. cshtml*

> [!WARNING]
> 將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。
>
> 請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 `FileName` 屬性，而不進行驗證。 `FileName` 屬性只能用於顯示用途，而且只能用於 HTML 編碼。
>
> 提供的範例不會考慮安全性考慮。 下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

### <a name="upload-large-files-with-streaming"></a>使用串流上傳大型檔案

下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。 檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。 由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。 在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。 讀取多部分區段之後，動作會執行自己的模型系結。

初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。 屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

`DisableFormValueModelBindingAttribute` 可用來停用模型系結：

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

在範例應用程式中，`GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會當做篩選套用至 `/StreamedSingleFileUploadDb` 的頁面應用程式模型，以及 `Startup.ConfigureServices` 中使用[Razor Pages 慣例](xref:razor-pages/razor-pages-conventions)的 `/StreamedSingleFileUploadPhysical`：

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。 動作方法會直接與 `Request` 屬性搭配運作。 `MultipartReader` 是用來讀取每個區段。 索引鍵/值資料會儲存在 `KeyValueAccumulator`中。 讀取多部分區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料系結至模型類型。

使用 EF Core 串流至資料庫的完整 `StreamingController.UploadDatabase` 方法：

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

`MultipartRequestHelper` （*公用程式/MultipartRequestHelper .cs*）：

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

串流至實體位置的完整 `StreamingController.UploadPhysical` 方法：

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

在範例應用程式中，驗證檢查是由 `FileHelpers.ProcessStreamedFile`處理。

## <a name="validation"></a>驗證

範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理的 <xref:Microsoft.AspNetCore.Http.IFormFile> 和串流檔案上傳的檢查。 若要處理範例應用程式中 <xref:Microsoft.AspNetCore.Http.IFormFile> 緩衝的檔案上傳，請參閱*公用程式/FileHelpers*檔案中的 `ProcessFormFile` 方法。 如需處理資料流程檔案，請參閱相同檔案中的 `ProcessStreamedFile` 方法。

> [!WARNING]
> 範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。 在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。
>
> 雖然主題範例提供驗證技術的實用範例，但請勿在生產應用程式中執行 `FileHelpers` 類別，除非您：
>
> * 完全瞭解此實作為。
> * 針對應用程式的環境和規格修改適當的執行。
>
> **絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**

### <a name="content-validation"></a>內容驗證

**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**

在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。 如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。 一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。 當檔案通過時，檔案就會移至一般檔案儲存位置。 這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。 藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。

### <a name="file-extension-validation"></a>副檔名驗證

已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。 例如，

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a>檔案簽章驗證

檔案的簽章取決於檔案開頭的前幾個位元組。 這些位元組可用來指出延伸模組是否符合檔案的內容。 範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。 在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：

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

若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。

### <a name="file-name-security"></a>檔案名安全性

絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。 使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。

Razor 會自動對屬性值進行 HTML 編碼以供顯示。 以下是可安全使用的程式碼：

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

在 Razor 以外，一律 <xref:System.Net.WebUtility.HtmlEncode*> 來自使用者要求的檔案名內容。

許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。 提供其他邏輯以符合您應用程式的規格。

### <a name="size-validation"></a>大小驗證

限制上傳檔案的大小。

在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。 此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的設定來提供：

```json
{
  "FileSizeLimit": 2097152
}
```

`FileSizeLimit` 會插入 `PageModel` 類別中：

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

當檔案大小超過限制時，就會拒絕此檔案：

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a>Match name 屬性值與 POST 方法的參數名稱

在張貼表單資料或直接使用 JavaScript `FormData` 的非 Razor 表單中，在表單的元素或 `FormData` 中指定的名稱必須符合控制器動作中參數的名稱。

在下列範例中：

* 使用 `<input>` 元素時，`name` 屬性會設定為 `battlePlans`的值：

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* 在 JavaScript 中使用 `FormData` 時，此名稱會設定為 `battlePlans`的值：

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

針對C#方法的參數使用相符的名稱（`battlePlans`）：

* 針對名為 `Upload`的 Razor Pages 頁面處理常式方法：

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* 針對 MVC POST 控制器動作方法：

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a>伺服器和應用程式設定

### <a name="multipart-body-length-limit"></a>多部分主體長度限制

<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定每個多部分主體的長度限制。 超過此限制的表單區段會在剖析時擲回 <xref:System.IO.InvalidDataException>。 預設值為134217728（128 MB）。 使用 `Startup.ConfigureServices`中的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定自訂限制：

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

<xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> 可用來設定單一頁面或動作的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>。

在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：

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

在 Razor Pages 應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a>Kestrel 要求主體大小上限

針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。 使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：

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

<xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 可用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。

在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：

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

在 Razor pages 應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

`RequestSizeLimitAttribute` 也可以使用[`@attribute`](xref:mvc/views/razor#attribute) Razor 指示詞來套用：

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a>其他 Kestrel 限制

其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：

* [用戶端連線數目上限](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [要求和回應資料速率](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a>IIS 內容長度限制

預設要求限制（`maxAllowedContentLength`）是30000000個位元組，大約是 28.6 MB。 自訂*web.config*檔案中的限制：

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

這個設定只適用於 IIS。 在 Kestrel 上裝載時，預設不會發生此行為。 如需詳細資訊，請參閱[\<requestLimits > 的要求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。

ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。 如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（dotnet/AspNetCore #2711）](https://github.com/dotnet/AspNetCore/issues/2711)。

## <a name="troubleshoot"></a>疑難排解

以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。

### <a name="not-found-error-when-deployed-to-an-iis-server"></a>部署到 IIS 伺服器時找不到錯誤

下列錯誤表示上傳的檔案超過伺服器設定的內容長度：

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。

### <a name="connection-failure"></a>連線失敗

連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。 如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。 Kestrel 用戶端連接限制也可能需要調整。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile 的 Null 參考例外狀況

如果控制器使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 來接受上傳檔案，但 `null`值，請確認 HTML 表單是否指定 `multipart/form-data`的 `enctype` 值。 如果未在 `<form>` 元素上設定這個屬性，就不會進行檔案上傳，而且會 `null`任何系結 <xref:Microsoft.AspNetCore.Http.IFormFile> 引數。 此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。

### <a name="stream-was-too-long"></a>資料流程太長

本主題中的範例依賴 <xref:System.IO.MemoryStream> 來保存所上傳檔案的內容。 `int.MaxValue``MemoryStream` 的大小限制。 如果應用程式的檔案上傳案例需要保留大於 50 MB 的檔案內容，請使用不依賴單一 `MemoryStream` 來保存已上傳檔案內容的替代方法。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 支援針對較小的檔案上傳一個或多個檔案，並針對較大的檔案使用緩衝的串流處理。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>安全性考量

提供使用者將檔案上傳到伺服器的能力時，請務必小心。 攻擊者可能會嘗試：

* 執行[拒絕服務的](/windows-hardware/drivers/ifs/denial-of-service)攻擊。
* 上傳病毒或惡意程式碼。
* 以其他方式危害網路和伺服器。

降低成功攻擊的可能性的安全性步驟如下：

* 將檔案上傳到專用的檔案上傳區域，最好是在非系統磁片磁碟機上。 專用位置可讓您更輕鬆地對上傳的檔案強加安全性限制。 停用檔案上傳位置的 execute 許可權。&dagger;
* 請勿將上傳的**檔案保存在**與應用程式相同的目錄樹狀結構中。&dagger;
* 使用應用程式所決定的安全檔案名。 請勿使用使用者所提供的檔案名或上傳檔案的不受信任檔案名。&dagger; HTML 在顯示不受信任的檔案名時進行編碼。 例如，記錄檔案名或在 UI 中顯示（Razor 會自動以 HTML 編碼輸出）。
* 僅允許應用程式設計規格的已核准副檔名。&dagger; <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* 確認用戶端檢查是在伺服器上執行。&dagger; 用戶端檢查很容易規避。
* 檢查上傳檔案的大小。 設定最大大小限制以防止大量上傳。&dagger;
* 當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。
* **在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**

&dagger;範例應用程式示範符合準則的方法。

> [!WARNING]
> 將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：
>
> * 完全取得系統的控制權。
> * 使用系統損毀的結果來多載系統。
> * 洩漏使用者或系統資料。
> * 將刻套用至公用 UI。
>
> 如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：
>
> * [不受限制的檔案上傳](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

如需有關如何執行安全性措施的詳細資訊，包括範例應用程式的範例，請參閱[驗證](#validation)一節。

## <a name="storage-scenarios"></a>儲存案例

檔案的一般儲存選項包括：

* 資料庫

  * 針對小型檔案上傳，資料庫的速度通常會比實體儲存體（檔案系統或網路共用）選項快。
  * 資料庫通常比實體儲存體選項更方便，因為抓取使用者資料的資料庫記錄可以同時提供檔案內容（例如，頭像影像）。
  * 資料庫的成本可能比使用資料儲存體服務來得低。

* 實體存放裝置（檔案系統或網路共用）

  * 針對大型檔案上傳：
    * 資料庫限制可能會限制上傳的大小。
    * 實體儲存體通常比在資料庫中儲存的成本低。
  * 實體儲存體的成本可能比使用資料儲存體服務來得低。
  * 應用程式的進程必須具有儲存位置的讀取和寫入權限。 **絕對不要授與 execute 許可權。**

* 資料儲存體服務（例如[Azure Blob 儲存體](https://azure.microsoft.com/services/storage/blobs/)）

  * 服務通常會針對內部部署解決方案提供改良的擴充性和彈性，通常會受到單一失敗點的影響。
  * 在大型儲存體基礎結構案例中，服務可能會降低成本。

  如需詳細資訊，請參閱[快速入門：使用 .net 在物件儲存體中建立 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。 本主題示範 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>，但 <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> 可以在使用 <xref:System.IO.Stream>時，用來將 <xref:System.IO.FileStream> 儲存至 blob 儲存體。

## <a name="file-upload-scenarios"></a>檔案上傳案例

上傳檔案的兩個一般方法是緩衝和串流處理。

**緩衝**

系統會將整個檔案讀入 <xref:Microsoft.AspNetCore.Http.IFormFile>，這是用來C#處理或儲存檔案的檔案標記法。

檔案上傳所使用的資源（磁片、記憶體）取決於並行檔案上傳的數目和大小。 如果應用程式嘗試緩衝過多上傳，則當網站用盡記憶體或磁碟空間時，會損毀。 如果檔案上傳的大小或頻率是耗盡應用程式資源，請使用串流。

> [!NOTE]
> 任何超過 64 KB 的已緩衝檔案都會從記憶體移至磁片上的暫存檔案。

此主題的下列各節會涵蓋緩衝處理小型檔案：

* [實體儲存體](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [Database](#upload-small-files-with-buffered-model-binding-to-a-database)

**串流**

此檔案會從多部分要求接收，並由應用程式直接處理或儲存。 串流不會大幅改善效能。 串流處理可減少上傳檔案時記憶體或磁碟空間的需求。

[使用串流上傳大型](#upload-large-files-with-streaming)檔案一節涵蓋串流處理大型檔案。

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a>將具有緩衝模型系結的小型檔案上傳至實體儲存體

若要上傳小型檔案，請使用多部分表單，或使用 JavaScript 來建立 POST 要求。

下列範例示範如何使用 Razor Pages 表單來上傳單一檔案（範例應用程式中的*Pages/BufferedSingleFileUploadPhysical. cshtml* ）：

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

下列範例類似于先前的範例，不同之處在于：

* JavaScript 的（[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)）是用來提交表單的資料。
* 沒有驗證。

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

若要針對[不支援 FETCH API](https://caniuse.com/#feat=fetch)的用戶端，以 JavaScript 執行表單 POST，請使用下列其中一種方法：

* 使用提取 Polyfill （例如，[fetch [Polyfill （github/fetch）]](https://github.com/github/fetch)）。
* 使用 `XMLHttpRequest`。 例如，

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

為了支援檔案上傳，HTML 表單必須指定 `multipart/form-data`的編碼類型（`enctype`）。

若要支援上傳多個檔案的 `files` input 元素，請在 `<input>` 元素上提供 `multiple` 屬性：

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

您可以使用 <xref:Microsoft.AspNetCore.Http.IFormFile>，透過[模型](xref:mvc/models/model-binding)系結來存取上傳至伺服器的個別檔案。 範例應用程式會針對資料庫和實體儲存案例，示範多個已緩衝處理的檔案上傳。

<a name="filename2"></a>

> [!WARNING]
> 除了顯示和記錄之外，**請勿使用 <xref:Microsoft.AspNetCore.Http.IFormFile>** 的 `FileName` 屬性。 當顯示或記錄時，HTML 會將檔案名編碼。 攻擊者可以提供惡意的檔案名，包括完整路徑或相對路徑。 應用程式應該：
>
> * 從使用者提供的檔案名中移除路徑。
> * 針對 UI 或記錄儲存以 HTML 編碼、路徑移除的檔案名。
> * 為儲存區產生新的隨機檔案名。
>
> 下列程式碼會從檔案名中移除路徑：
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> 到目前為止所提供的範例並不考慮安全性考慮。 下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

使用模型系結和 <xref:Microsoft.AspNetCore.Http.IFormFile>上傳檔案時，動作方法可以接受：

* 單一 <xref:Microsoft.AspNetCore.Http.IFormFile>。
* 下列任何代表數個檔案的集合：
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * [列出](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>

> [!NOTE]
> 系結符合依名稱的表單檔案。 例如，`<input type="file" name="formFile">` 中的 HTML `name` 值必須符合C#參數/屬性系結（`FormFile`）。 如需詳細資訊，請參閱[Match name 屬性值與 POST 方法的參數名稱](#match-name-attribute-value-to-parameter-name-of-post-method)一節。

下列範例︰

* 迴圈一或多個已上傳的檔案。
* 會使用[GetTempFileName](xref:System.IO.Path.GetTempFileName*)來傳回檔案的完整路徑，包括檔案名。 
* 使用應用程式所產生的檔案名，將檔案儲存至本機檔案系統。
* 傳回已上傳檔案的總數和大小。

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

使用 `Path.GetRandomFileName` 產生不含路徑的檔案名。 在下列範例中，路徑是從設定取得：

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

傳遞至 <xref:System.IO.FileStream> 的路徑*必須*包含檔案名。 如果未提供檔案名，則會在執行時間擲回 <xref:System.UnauthorizedAccessException>。

使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 技術所上傳的檔案會在處理之前，先緩衝到伺服器上的記憶體或磁片上。 在動作方法內，<xref:Microsoft.AspNetCore.Http.IFormFile> 內容可以 <xref:System.IO.Stream>存取。 除了本機檔案系統之外，檔案也可以儲存至網路共用或檔案儲存體服務，例如[Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)。

如需在多個檔案上傳並使用安全檔案名的另一個範例，請參閱範例應用程式中的*Pages/BufferedMultipleFileUploadPhysical* 。

> [!WARNING]
> 如果建立的檔案超過65535，而未刪除先前的暫存檔案，則[GetTempFileName](xref:System.IO.Path.GetTempFileName*)會擲回 <xref:System.IO.IOException>。 65535檔案的限制是每一伺服器的限制。 如需有關 Windows OS 上此限制的詳細資訊，請參閱下列主題中的備註：
>
> * [GetTempFileNameA 函式](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a>將具有緩衝模型系結的小型檔案上傳至資料庫

若要使用[Entity Framework](/ef/core/index)在資料庫中儲存二進位檔案資料，請在實體上定義 <xref:System.Byte> 陣列屬性：

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

指定包含 <xref:Microsoft.AspNetCore.Http.IFormFile>之類別的 [頁面模型] 屬性：

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
> <xref:Microsoft.AspNetCore.Http.IFormFile> 可以直接當做動作方法參數或系結模型屬性來使用。 先前的範例會使用系結模型屬性。

`FileUpload` 用於 Razor Pages 形式：

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

當表單張貼至伺服器時，請將 <xref:Microsoft.AspNetCore.Http.IFormFile> 複製到資料流程，並將其儲存為資料庫中的位元組陣列。 在下列範例中，`_dbContext` 會儲存應用程式的資料庫內容：

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

前述範例類似于範例應用程式中所示範的案例：

* *Pages/BufferedSingleFileUploadDb. cshtml*
* *Pages/BufferedSingleFileUploadDb. cshtml*

> [!WARNING]
> 將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。
>
> 請勿依賴或信任 <xref:Microsoft.AspNetCore.Http.IFormFile> 的 `FileName` 屬性，而不進行驗證。 `FileName` 屬性只能用於顯示用途，而且只能用於 HTML 編碼。
>
> 提供的範例不會考慮安全性考慮。 下列各節和[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)會提供其他資訊：
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

### <a name="upload-large-files-with-streaming"></a>使用串流上傳大型檔案

下列範例示範如何使用 JavaScript 將檔案串流至控制器動作。 檔案的 antiforgery token 是使用自訂篩選屬性所產生，並傳遞至用戶端 HTTP 標頭，而不是在要求主體中。 由於動作方法會直接處理上傳的資料，因此表單模型系結會由另一個自訂篩選器停用。 在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。 讀取多部分區段之後，動作會執行自己的模型系結。

初始頁面回應會載入表單，並將 antiforgery token 儲存在 cookie 中（透過 `GenerateAntiforgeryTokenCookieAttribute` 屬性）。 屬性會使用 ASP.NET Core 的內建[antiforgery 支援](xref:security/anti-request-forgery)來設定具有要求權杖的 cookie：

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

`DisableFormValueModelBindingAttribute` 可用來停用模型系結：

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

在範例應用程式中，`GenerateAntiforgeryTokenCookieAttribute` 和 `DisableFormValueModelBindingAttribute` 會當做篩選套用至 `/StreamedSingleFileUploadDb` 的頁面應用程式模型，以及 `Startup.ConfigureServices` 中使用[Razor Pages 慣例](xref:razor-pages/razor-pages-conventions)的 `/StreamedSingleFileUploadPhysical`：

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

由於模型系結不會讀取表單，因此從表單系結的參數不會系結（查詢、路由及標頭會繼續正常執行）。 動作方法會直接與 `Request` 屬性搭配運作。 `MultipartReader` 是用來讀取每個區段。 索引鍵/值資料會儲存在 `KeyValueAccumulator`中。 讀取多部分區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料系結至模型類型。

使用 EF Core 串流至資料庫的完整 `StreamingController.UploadDatabase` 方法：

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

`MultipartRequestHelper` （*公用程式/MultipartRequestHelper .cs*）：

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

串流至實體位置的完整 `StreamingController.UploadPhysical` 方法：

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

在範例應用程式中，驗證檢查是由 `FileHelpers.ProcessStreamedFile`處理。

## <a name="validation"></a>驗證

範例應用程式的 `FileHelpers` 類別會示範多個已緩衝處理的 <xref:Microsoft.AspNetCore.Http.IFormFile> 和串流檔案上傳的檢查。 若要處理範例應用程式中 <xref:Microsoft.AspNetCore.Http.IFormFile> 緩衝的檔案上傳，請參閱*公用程式/FileHelpers*檔案中的 `ProcessFormFile` 方法。 如需處理資料流程檔案，請參閱相同檔案中的 `ProcessStreamedFile` 方法。

> [!WARNING]
> 範例應用程式中所示範的驗證處理方法不會掃描已上傳檔案的內容。 在大部分的生產環境案例中，會在檔案上使用病毒/惡意程式碼掃描器 API，才能讓使用者或其他系統使用檔案。
>
> 雖然主題範例提供驗證技術的實用範例，但請勿在生產應用程式中執行 `FileHelpers` 類別，除非您：
>
> * 完全瞭解此實作為。
> * 針對應用程式的環境和規格修改適當的執行。
>
> **絕對不要在應用程式中執行安全驗證，而不需解決這些需求。**

### <a name="content-validation"></a>內容驗證

**在上傳的內容上使用協力廠商的病毒/惡意程式碼掃描 API。**

在大量案例中，掃描檔案對伺服器資源的需求非常嚴苛。 如果要求處理效能因檔案掃描而降低，請考慮將掃描工作卸載至[背景服務](xref:fundamentals/host/hosted-services)，可能是在與應用程式伺服器不同的伺服器上執行的服務。 一般來說，上傳的檔案會保留在隔離的區域中，直到背景病毒掃描程式檢查它們為止。 當檔案通過時，檔案就會移至一般檔案儲存位置。 這些步驟通常會與資料庫記錄一起執行，以指出檔案的掃描狀態。 藉由使用這種方法，應用程式和應用程式伺服器會持續專注于回應要求。

### <a name="file-extension-validation"></a>副檔名驗證

已上傳檔案的延伸模組應針對允許的延伸模組清單進行檢查。 例如，

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a>檔案簽章驗證

檔案的簽章取決於檔案開頭的前幾個位元組。 這些位元組可用來指出延伸模組是否符合檔案的內容。 範例應用程式會檢查檔案簽章是否有幾種常見的檔案類型。 在下列範例中，會針對檔案檢查 JPEG 影像的檔案簽章：

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

若要取得其他檔案簽章，請參閱檔案簽章[資料庫](https://www.filesignatures.net/)和官方檔案規格。

### <a name="file-name-security"></a>檔案名安全性

絕對不要使用用戶端提供的檔案名將檔案儲存至實體存放裝置。 使用[GetRandomFileName](xref:System.IO.Path.GetRandomFileName*)或[GetTempFileName](xref:System.IO.Path.GetTempFileName*)建立檔案的安全檔案名，以建立暫存儲存體的完整路徑（包括檔案名）。

Razor 會自動對屬性值進行 HTML 編碼以供顯示。 以下是可安全使用的程式碼：

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

在 Razor 以外，一律 <xref:System.Net.WebUtility.HtmlEncode*> 來自使用者要求的檔案名內容。

許多的執行都必須包含檔案存在的檢查;否則，檔案會由相同名稱的檔案覆寫。 提供其他邏輯以符合您應用程式的規格。

### <a name="size-validation"></a>大小驗證

限制上傳檔案的大小。

在範例應用程式中，檔案大小限制為 2 MB （以位元組表示）。 此限制是透過 appsettings[檔案中](xref:fundamentals/configuration/index)的設定來提供：

```json
{
  "FileSizeLimit": 2097152
}
```

`FileSizeLimit` 會插入 `PageModel` 類別中：

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

當檔案大小超過限制時，就會拒絕此檔案：

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a>Match name 屬性值與 POST 方法的參數名稱

在張貼表單資料或直接使用 JavaScript `FormData` 的非 Razor 表單中，在表單的元素或 `FormData` 中指定的名稱必須符合控制器動作中參數的名稱。

在下列範例中：

* 使用 `<input>` 元素時，`name` 屬性會設定為 `battlePlans`的值：

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* 在 JavaScript 中使用 `FormData` 時，此名稱會設定為 `battlePlans`的值：

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

針對C#方法的參數使用相符的名稱（`battlePlans`）：

* 針對名為 `Upload`的 Razor Pages 頁面處理常式方法：

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* 針對 MVC POST 控制器動作方法：

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a>伺服器和應用程式設定

### <a name="multipart-body-length-limit"></a>多部分主體長度限制

<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定每個多部分主體的長度限制。 超過此限制的表單區段會在剖析時擲回 <xref:System.IO.InvalidDataException>。 預設值為134217728（128 MB）。 使用 `Startup.ConfigureServices`中的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> 設定自訂限制：

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

<xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> 可用來設定單一頁面或動作的 <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>。

在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：

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

在 Razor Pages 應用程式或 MVC 應用程式中，將篩選套用至頁面模型或動作方法：

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a>Kestrel 要求主體大小上限

針對 Kestrel 所裝載的應用程式，預設的要求主體大小上限為30000000個位元組，大約為 28.6 MB。 使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制：

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

<xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 可用來設定單一頁面或動作的[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) 。

在 Razor Pages 應用程式中，將篩選套用至 `Startup.ConfigureServices`中的[慣例](xref:razor-pages/razor-pages-conventions)：

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

在 Razor pages 應用程式或 MVC 應用程式中，將篩選套用至頁面處理常式類別或動作方法：

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a>其他 Kestrel 限制

其他 Kestrel 限制可能適用于 Kestrel 所裝載的應用程式：

* [用戶端連線數目上限](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [要求和回應資料速率](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a>IIS 內容長度限制

預設要求限制（`maxAllowedContentLength`）是30000000個位元組，大約是 28.6 MB。 自訂*web.config*檔案中的限制：

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

這個設定只適用於 IIS。 在 Kestrel 上裝載時，預設不會發生此行為。 如需詳細資訊，請參閱[\<requestLimits > 的要求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。

ASP.NET Core 模組的限制或 IIS 要求篩選模組的存在，可能會將上傳限制為2或 4 GB。 如需詳細資訊，請參閱[無法上傳大小大於 2 gb 的檔案（dotnet/AspNetCore #2711）](https://github.com/dotnet/AspNetCore/issues/2711)。

## <a name="troubleshoot"></a>疑難排解

以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。

### <a name="not-found-error-when-deployed-to-an-iis-server"></a>部署到 IIS 伺服器時找不到錯誤

下列錯誤表示上傳的檔案超過伺服器設定的內容長度：

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

如需增加限制的詳細資訊，請參閱[IIS 內容長度限制](#iis-content-length-limit)一節。

### <a name="connection-failure"></a>連線失敗

連接錯誤和重設伺服器連接可能表示上傳的檔案超過 Kestrel 的要求主體大小上限。 如需詳細資訊，請參閱[Kestrel 最大要求主體大小](#kestrel-maximum-request-body-size)一節。 Kestrel 用戶端連接限制也可能需要調整。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile 的 Null 參考例外狀況

如果控制器使用 <xref:Microsoft.AspNetCore.Http.IFormFile> 來接受上傳檔案，但 `null`值，請確認 HTML 表單是否指定 `multipart/form-data`的 `enctype` 值。 如果未在 `<form>` 元素上設定這個屬性，就不會進行檔案上傳，而且會 `null`任何系結 <xref:Microsoft.AspNetCore.Http.IFormFile> 引數。 此外，請確認[表單資料中的上傳命名符合應用程式的命名](#match-name-attribute-value-to-parameter-name-of-post-method)。

### <a name="stream-was-too-long"></a>資料流程太長

本主題中的範例依賴 <xref:System.IO.MemoryStream> 來保存所上傳檔案的內容。 `int.MaxValue``MemoryStream` 的大小限制。 如果應用程式的檔案上傳案例需要保留大於 50 MB 的檔案內容，請使用不依賴單一 `MemoryStream` 來保存已上傳檔案內容的替代方法。

::: moniker-end


## <a name="additional-resources"></a>其他資源

* [不受限制的檔案上傳](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [Azure 安全性：安全性框架：輸入驗證 |緩和措施](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [Azure 雲端設計模式： Valet 金鑰模式](/azure/architecture/patterns/valet-key)
