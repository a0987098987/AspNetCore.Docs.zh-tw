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
# <a name="upload-files-in-aspnet-core"></a>上傳ASP.NET核心中的檔案

由[史蒂夫史密斯](https://ardalis.com/)和[魯特格風暴](https://github.com/rutix)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 支援使用緩衝模型綁定上傳一個或多個檔,用於較小的檔,以及針對較大檔的未緩衝流。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)([如何下載](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>安全性考量

在為使用者提供將檔上傳到伺服器的能力時,請謹慎。 攻擊者可能嘗試:

* 執行[拒絕服務](/windows-hardware/drivers/ifs/denial-of-service)攻擊。
* 上傳病毒或惡意軟體。
* 以其他方式危害網路和伺服器。

降低成功攻擊可能性的安全步驟包括:

* 將檔上載到專用檔上傳區域,最好將其上傳到非系統驅動器。 專用位置使對上載的文件實施安全限制變得更加容易。 禁用對檔上載位置執行許可權。&dagger;
* **不要**將上載的檔保留在同一目錄樹中。&dagger;
* 使用由應用確定的安全檔名。 不要使用使用者提供的檔案名或上傳檔的不受信任的檔名。&dagger; HTML 在顯示不受信任的檔名時對其進行編碼。 例如,在 UI 中記錄檔名或顯示(Razor 自動 HTML 編碼輸出)。
* 僅允許應用設計規範的已批准文件副檔名。&dagger; <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* 驗證在伺服器上是否執行客戶端檢查。&dagger;客戶端檢查很容易規避。
* 檢查上載檔的大小。 設置最大大小限制以防止大型上載。&dagger;
* 當檔不應被同名上傳的文件覆蓋時,請在上載檔之前對照資料庫或物理存儲檢查檔名。
* **在存儲檔之前,對上載的內容運行病毒/惡意軟體掃描程式。**

&dagger;示例應用演示了滿足條件的方法。

> [!WARNING]
> 將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：
>
> * 完全控制系統。
> * 使系統過載,導致系統崩潰。
> * 洩漏使用者或系統資料。
> * 將塗鴉應用於公共 UI。
>
> 如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：
>
> * [不受限制的檔案上傳](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
> * [Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

有關實現安全措施的詳細資訊(包括示例應用的示例),請參閱[驗證](#validation)部分。

## <a name="storage-scenarios"></a>儲存機制

檔案的常見記憶體選項包括:

* 資料庫

  * 對於小型檔上載,資料庫通常比實體存儲(檔案系統或網路共用)選項更快。
  * 資料庫通常比物理存儲選項更方便,因為檢索用戶數據的資料庫記錄可以同時提供文件內容(例如,頭像映射)。
  * 與使用數據存儲服務相比,資料庫的成本可能更低。

* 實體儲存(檔案系統或網路分享)

  * 對大型檔案上載:
    * 資料庫限制可能會限制上載的大小。
    * 物理存儲通常比資料庫中的存儲經濟程度低。
  * 物理存儲可能比使用數據存儲服務的成本更低。
  * 應用的進程必須具有存儲位置的讀取和寫入許可權。 **切勿授予執行許可權。**

* 資料儲存服務(例如[,Azure Blob 儲存](https://azure.microsoft.com/services/storage/blobs/))

  * 與通常單點故障的本地解決方案,服務通常提供改進的可擴充性和彈性。
  * 在大型存儲基礎結構方案中,服務的成本可能會更低。

  有關詳細資訊,請參閱[快速入門:使用 .NET 在物件存儲中創建 Blob。](/azure/storage/blobs/storage-quickstart-blobs-dotnet)

## <a name="file-upload-scenarios"></a>檔案上傳機制

上傳檔的兩種常規方法是緩衝和流式傳輸。

**緩衝**

整個檔被讀入一<xref:Microsoft.AspNetCore.Http.IFormFile>個 ,它是用於處理或保存檔的檔的 C# 表示形式。

檔上載使用的資源(磁碟、記憶體)取決於併發檔上載的數量和大小。 如果應用嘗試緩衝過多的上載,則當網站記憶體或磁碟空間不足時,該網站將崩潰。 如果檔上載的大小或頻率耗盡了應用資源,請使用流式處理。

> [!NOTE]
> 任何超過 64 KB 的單個緩衝檔將從記憶體移動到磁碟上的臨時檔。

本主題的以下部分介紹了緩衝小檔:

* [實體儲存](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [Database](#upload-small-files-with-buffered-model-binding-to-a-database)

**串流**

該檔從多部分請求接收,並由應用直接處理或保存。 流式處理不會顯著提高性能。 流式傳輸減少了上傳檔時對記憶體或磁碟空間的需求。

流式處理大型檔在[「上傳包含流式處理的大型檔](#upload-large-files-with-streaming)」部分中介紹。

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a>將具有緩衝模型的小型檔案載入物理儲存

要上載小檔,請使用多部分窗體或使用 JAVAScript 構造 POST 請求。

下面的範例展示使用 Razor Pages 表單上載單檔案(例如範*單中的網頁/緩衝單檔案上傳物理.cshtml):*

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

以下示例與以前的示例類似,只不過:

* JavaScript 的 ([提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) 用於提交表單的資料。
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

要對[不支援 Fetch API 的](https://caniuse.com/#feat=fetch)用戶端在 JavaScript 執行表單 POST,請使用以下方法之一:

* 使用「擷取」多邊形填充(例如,[視窗.fetch 多邊填充(github/提取)。](https://github.com/github/fetch)
* 使用 `XMLHttpRequest`。 例如：

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

為了支援檔案中載,HTML 窗型必須指定的編碼型`enctype`態`multipart/form-data`( )

對於支援`files`上載多個檔案的輸入元素,`multiple``<input>`該 元素在元素上提供屬性:

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

上傳到伺服器的各個檔案可以使用[模型連結](xref:mvc/models/model-binding)<xref:Microsoft.AspNetCore.Http.IFormFile>。 範例應用示範了多個用於資料庫和實體存儲方案的緩衝檔上載。

<a name="filename"></a>

> [!WARNING]
> **請勿**`FileName`使用 顯示和日<xref:Microsoft.AspNetCore.Http.IFormFile>誌記錄 以外的屬性。 顯示或紀錄記錄時,HTML 對檔案名進行編碼。 攻擊者可以提供惡意檔名,包括完整路徑或相對路徑。 應用程式應:
>
> * 從使用者提供的檔名中刪除路徑。
> * 儲存 HTML 編碼的路徑刪除的檔名,用於 UI 或紀錄記錄。
> * 生成新的隨機檔名進行存儲。
>
> 以下代碼從檔案名中刪除路徑:
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> 到目前為止提供的示例沒有考慮到安全注意事項。 其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

使用模型綁定<xref:Microsoft.AspNetCore.Http.IFormFile>和 上載檔時,操作方法可以接受:

* 單個<xref:Microsoft.AspNetCore.Http.IFormFile>。
* 表示多個檔案的以下任何集合:
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * [清單](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>

> [!NOTE]
> 綁定依名稱匹配表單檔。 例如,中的`name``<input type="file" name="formFile">`HTML 值必須與 C# 參數`FormFile`/ 屬性繫結 ( ) 符合。 關於詳細資訊,請參閱 POST[方法部份的參數名稱的比名稱屬性值](#match-name-attribute-value-to-parameter-name-of-post-method)。

下列範例將：

* 循環訪問一個或多個上傳的檔。
* 使用[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)傳回檔案的完整路徑,包括檔名。 
* 使用應用生成的檔案名將檔儲存到本地檔案系統。
* 返回上載的檔的總數和大小。

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

用於`Path.GetRandomFileName`生成沒有路徑的檔名。 在下面的範例中,路徑是從設定中取得的:

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

傳遞到<xref:System.IO.FileStream>的路徑*必須*包括檔名。 如果未提供檔案名稱,則在執行時引發<xref:System.UnauthorizedAccessException>。

在處理之前,使用<xref:Microsoft.AspNetCore.Http.IFormFile>該技術上載的檔將緩衝在記憶體中或伺服器上的磁碟上。 在操作方法中,<xref:Microsoft.AspNetCore.Http.IFormFile>內容可作為<xref:System.IO.Stream>可訪問。 除了本地檔案系統之外,檔案還可以保存到網路共享或檔案存儲服務(如[Azure Blob 儲存](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs))。

有關迴圈存取多個檔以進行上載並使用安全檔名的範例,請參閱範例應用中*的頁面/緩衝倍數檔檔物理.cshtml.cs。*

> [!WARNING]
> [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)<xref:System.IO.IOException>會引發一個,如果創建了超過 65,535 個檔,而不刪除以前的臨時檔。 65,535 個檔的限制是每個伺服器的限制。 有關 Windows 作業系統上此限制的詳細資訊,請參閱以下主題中的備註:
>
> * [取得tempFilenameA函式](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a>將具有緩衝模型的小檔案載到資料庫

要使用[實體框架](/ef/core/index)將二進位檔案資料儲存在資料庫中,<xref:System.Byte>請在實體上定義數位屬性:

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

指定的類別指定頁面模型屬性<xref:Microsoft.AspNetCore.Http.IFormFile>:

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
> <xref:Microsoft.AspNetCore.Http.IFormFile>可以直接用作操作方法參數或綁定模型屬性。 上一個示例使用綁定模型屬性。

在`FileUpload`剃刀頁窗體中使用:

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

將表單 POSTed 複製到伺服器時<xref:Microsoft.AspNetCore.Http.IFormFile>,將複製到流並將其另存為資料庫中的位元組。 在以下範例中,`_dbContext`儲存應用的資料庫內容:

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

前面的範例類似於範例應用中展示的方案:

* *頁面/緩衝單檔案上傳Db.cshtml*
* *頁面/緩衝單檔案上傳Db.cshtml.cs*

> [!WARNING]
> 將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。
>
> 未經驗證,不要依賴或信任`FileName`的屬性。 <xref:Microsoft.AspNetCore.Http.IFormFile> 該`FileName`屬性應僅用於顯示目的,並且僅在 HTML 編碼後使用。
>
> 提供的示例不考慮安全注意事項。 其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

### <a name="upload-large-files-with-streaming"></a>使用流式上傳大型檔案

下面的範例展示如何使用 JavaScript 將檔案流式傳輸到控制器操作。 該檔的反偽造權杖使用自定義篩選器屬性生成,並傳遞到客戶端 HTTP 標頭而不是請求正文中。 由於操作方法直接處理上載的數據,因此表單模型綁定被另一個自定義篩選器禁用。 在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。 讀取多部分部分后,操作將執行其自己的模型綁定。

初始頁面回應載入表單,並將反偽造權杖保存在 Cookie 中`GenerateAntiforgeryTokenCookieAttribute`(通過屬性 )。 該屬性使用 ASP.NET Core 的內建[反偽造支援](xref:security/anti-request-forgery)來設定具有請求權杖的 Cookie:

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

`DisableFormValueModelBindingAttribute`關閉模型的系統:

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

在`GenerateAntiforgeryTokenCookieAttribute`範例應用中,並`DisableFormValueModelBindingAttribute`作為篩選器應用`/StreamedSingleFileUploadDb``/StreamedSingleFileUploadPhysical``Startup.ConfigureServices`於使用 Razor Pages 約定的 頁面應用程式模型和 中的[頁面](xref:razor-pages/razor-pages-conventions)應用程式模型:

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

由於模型綁定不讀取窗體,因此從窗體綁定的參數不會綁定(查詢、路由和標頭繼續工作)。 操作方法直接與 屬性`Request`一起工作。 `MultipartReader` 是用來讀取每個區段。 鍵/數值資料儲存在`KeyValueAccumulator`中 。 讀取多部分部分後,使用的內容`KeyValueAccumulator`將窗體數據綁定到模型類型。

使用`StreamingController.UploadDatabase`EF Core 串流式傳輸到資料庫的完整方法:

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

`MultipartRequestHelper`(*公用程式/ 多部份要求説明者.cs*):

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

串流式傳輸`StreamingController.UploadPhysical`到 物理位置的完整方法:

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

在範例應用中,驗證檢查由`FileHelpers.ProcessStreamedFile`處理 。

## <a name="validation"></a>驗證

示例應用的`FileHelpers`類演示了對緩<xref:Microsoft.AspNetCore.Http.IFormFile>衝 和流式檔上載的多次檢查。 有關在<xref:Microsoft.AspNetCore.Http.IFormFile>示例應用中處理緩衝檔上傳,請參閱`ProcessFormFile`*實用程式/FileHelpers.cs*檔中的方法。 有關處理流式處理檔,`ProcessStreamedFile`請參閱同一檔中的方法。

> [!WARNING]
> 範例應用中演示的驗證處理方法不會掃描上載文件的內容。 在大多數生產方案中,在將檔可供使用者或其他系統使用之前,在檔上使用病毒/惡意軟體掃描器 API。
>
> 儘管主題示例提供了驗證技術的工作示例,但除非在生產應用中實現`FileHelpers`類,否則不要實現該類:
>
> * 完全理解實施情況。
> * 根據需要修改應用的環境和規範的實現。
>
> **如果不滿足這些要求,切勿在應用中不加區別地實施安全代碼。**

### <a name="content-validation"></a>內容驗證

**對上傳的內容使用第三方病毒/惡意軟體掃描 API。**

在高容量方案中,掃描檔對伺服器資源的要求很高。 如果由於文件掃描而使請求處理性能降低,請考慮將掃描工作卸載到[後台服務](xref:fundamentals/host/hosted-services),可能是在伺服器上運行的服務不同於應用的伺服器。 通常,上傳的檔在隔離區域中,直到背景病毒掃描程式檢查它們。 當檔通過時,檔將移動到正常檔案存儲位置。 這些步驟通常與指示檔的掃描狀態的資料庫記錄一起執行。 通過使用這種方法,應用和應用程式伺服器仍然專注於回應請求。

### <a name="file-extension-validation"></a>檔案副檔名驗證

應根據允許的擴展清單檢查上載的檔案副檔名。 例如：

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a>檔案簽章驗證

檔的簽名由檔開頭的前幾個位元組決定。 這些位元組可用於指示擴展名是否與文件的內容匹配。 範例應用檢查檔簽名中有幾個常見的文件類型。 在下面的範例中,針對該檔案檢查 JPEG 影像的檔案簽名:

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

要獲取其他文件簽名,請參閱[文件簽名資料庫](https://www.filesignatures.net/)和官方文件規範。

### <a name="file-name-security"></a>檔案名稱安全性

切勿使用用戶端提供的檔案名將檔儲存到物理存儲。 使用[Path.GetRandomFilename](xref:System.IO.Path.GetRandomFileName*)或[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)為檔案建立安全性的檔名,以創建用於暫時儲存的完整路徑(包括檔名)。

Razor 會自動對顯示的屬性值進行編碼。 以下代碼可安全使用:

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

在 Razor<xref:System.Net.WebUtility.HtmlEncode*>之外 ,始終從使用者請求中提交姓名內容。

許多實現必須包括檢查該檔是否存在;否則,該檔將被同名文件覆蓋。 提供其他邏輯以滿足應用的規範。

### <a name="size-validation"></a>大小驗證

限制上載檔的大小。

在範例應用中,檔的大小限制為2 MB(以位元組為單位)。 限制透過來自*應用程式設定.json*檔的[設定](xref:fundamentals/configuration/index)提供:

```json
{
  "FileSizeLimit": 2097152
}
```

註`FileSizeLimit`入 :`PageModel`

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

當檔案大小超過限制時,檔案將被拒絕:

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a>將名稱屬性值與 POST 方法的參數名稱符合

在 POST 形成資料或使用 JavaScript`FormData`的非 Razor 窗體中,在窗體`FormData`元素中指定的名稱或 必須與控制器操作中參數的名稱匹配。

在下例中︰

* 使用`<input>`元素時,屬性`name`設定為值`battlePlans`:

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* `FormData`在 JavaScript 中使用時,名稱設定`battlePlans`為 :

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

描述 C# 方法的參數使用符合`battlePlans`名稱 ( :

* 對於名為的`Upload`Razor 頁面頁面處理程式方法:

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* 對於 MVC POST 控制器操作方法:

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a>伺服器和應用程式設定

### <a name="multipart-body-length-limit"></a>多部份體長度限制

<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>設置每個多部分實體的長度限制。 超過此限制的表單的元件在<xref:System.IO.InvalidDataException>分析 時會引發 。 默認值為 134,217,728 (128 MB)。 使用的<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>`Startup.ConfigureServices`設定自訂限制:

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

<xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>為單一頁面或<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>操作設定 。

在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:

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

在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面模型或操作方法:

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a>凱斯特雷爾最大請求正文大小

對於 Kestrel 託管的應用,預設的最大請求正文大小為 30,000,000 位元組,約為 28.6 MB。 使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制:

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

<xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>用於為單個頁面或操作設置[最大請求博量。](xref:fundamentals/servers/kestrel#maximum-request-body-size)

在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:

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

在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面處理程式類或操作方法:

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

`RequestSizeLimitAttribute`也可以使用[`@attribute`](xref:mvc/views/razor#attribute)Razor 指令應用:

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a>其他凱斯特雷爾限制

其他凱斯特雷爾限制可能適用於由凱斯特雷爾託管的應用程式:

* [用戶端連線數目上限](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [要求與回應資料速率](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a>IIS 內容長度限制

默認請求限制`maxAllowedContentLength`( ) 是 30,000,000 位元組,約為 28.6MB。 自訂*Web.config*檔案中的限制:

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

這個設定只適用於 IIS。 在 Kestrel 上裝載時，預設不會發生此行為。 有關詳細資訊,請參閱[要求\<限制 請求限制>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。

ASP.NET核心模組或IIS請求篩選模組的存在的限制可能會將上載限制為2或4 GB。 有關詳細資訊,請參閱[無法上載大小大於 2GB 的檔(dotnet/AspNetCore #2711)。](https://github.com/dotnet/AspNetCore/issues/2711)

## <a name="troubleshoot"></a>疑難排解

以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。

### <a name="not-found-error-when-deployed-to-an-iis-server"></a>部署到 IIS 伺服器時找不到錯誤

以下錯誤指示上載的檔案超過伺服器設定的內容長度:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

有關增加限制的詳細資訊,請參閱[IIS內容長度限制](#iis-content-length-limit)部分。

### <a name="connection-failure"></a>連線失敗

連接錯誤和重置伺服器連接可能表示上載的文件超過 Kestrel 的最大請求正文大小。 有關詳細資訊,請參閱[Kestrel 最大請求正文大小](#kestrel-maximum-request-body-size)部分。 Kestrel 用戶端連接限制可能還需要調整。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile 的 Null 參考例外狀況

如果控制器<xref:Microsoft.AspNetCore.Http.IFormFile>正在使用 接受上載的檔案,但`null`值為 ,請確認`enctype`HTML`multipart/form-data`窗體指定的值 。 如果未在`<form>`元素上設定此屬性,則不會發生檔案上載,並且任何繫<xref:Microsoft.AspNetCore.Http.IFormFile>結參數`null`都是 。 您可以確認[表單資料中的上傳命名與套用的命名符合](#match-name-attribute-value-to-parameter-name-of-post-method)。

### <a name="stream-was-too-long"></a>流太長

本主題中的範例依賴於<xref:System.IO.MemoryStream>保存上載的文件的內容。 的大小限制`MemoryStream`為`int.MaxValue`。 如果應用的檔上載方案需要保存大於 50 MB 的檔內容,請使用不`MemoryStream`依賴單個 檔案來保存上載檔內容的替代方法。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 支援使用緩衝模型綁定上傳一個或多個檔,用於較小的檔,以及針對較大檔的未緩衝流。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)([如何下載](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>安全性考量

在為使用者提供將檔上傳到伺服器的能力時,請謹慎。 攻擊者可能嘗試:

* 執行[拒絕服務](/windows-hardware/drivers/ifs/denial-of-service)攻擊。
* 上傳病毒或惡意軟體。
* 以其他方式危害網路和伺服器。

降低成功攻擊可能性的安全步驟包括:

* 將檔上載到專用檔上傳區域,最好將其上傳到非系統驅動器。 專用位置使對上載的文件實施安全限制變得更加容易。 禁用對檔上載位置執行許可權。&dagger;
* **不要**將上載的檔保留在同一目錄樹中。&dagger;
* 使用由應用確定的安全檔名。 不要使用使用者提供的檔案名或上傳檔的不受信任的檔名。&dagger; HTML 在顯示不受信任的檔名時對其進行編碼。 例如,在 UI 中記錄檔名或顯示(Razor 自動 HTML 編碼輸出)。
* 僅允許應用設計規範的已批准文件副檔名。&dagger; <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* 驗證在伺服器上是否執行客戶端檢查。&dagger;客戶端檢查很容易規避。
* 檢查上載檔的大小。 設置最大大小限制以防止大型上載。&dagger;
* 當檔不應被同名上傳的文件覆蓋時,請在上載檔之前對照資料庫或物理存儲檢查檔名。
* **在存儲檔之前,對上載的內容運行病毒/惡意軟體掃描程式。**

&dagger;示例應用演示了滿足條件的方法。

> [!WARNING]
> 將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：
>
> * 完全控制系統。
> * 使系統過載,導致系統崩潰。
> * 洩漏使用者或系統資料。
> * 將塗鴉應用於公共 UI。
>
> 如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：
>
> * [不受限制的檔案上傳](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
> * [Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

有關實現安全措施的詳細資訊(包括示例應用的示例),請參閱[驗證](#validation)部分。

## <a name="storage-scenarios"></a>儲存機制

檔案的常見記憶體選項包括:

* 資料庫

  * 對於小型檔上載,資料庫通常比實體存儲(檔案系統或網路共用)選項更快。
  * 資料庫通常比物理存儲選項更方便,因為檢索用戶數據的資料庫記錄可以同時提供文件內容(例如,頭像映射)。
  * 與使用數據存儲服務相比,資料庫的成本可能更低。

* 實體儲存(檔案系統或網路分享)

  * 對大型檔案上載:
    * 資料庫限制可能會限制上載的大小。
    * 物理存儲通常比資料庫中的存儲經濟程度低。
  * 物理存儲可能比使用數據存儲服務的成本更低。
  * 應用的進程必須具有存儲位置的讀取和寫入許可權。 **切勿授予執行許可權。**

* 資料儲存服務(例如[,Azure Blob 儲存](https://azure.microsoft.com/services/storage/blobs/))

  * 與通常單點故障的本地解決方案,服務通常提供改進的可擴充性和彈性。
  * 在大型存儲基礎結構方案中,服務的成本可能會更低。

  有關詳細資訊,請參閱[快速入門:使用 .NET 在物件存儲中創建 Blob。](/azure/storage/blobs/storage-quickstart-blobs-dotnet) 本主題示範<xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>,<xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*>但可用於在使用<xref:System.IO.FileStream><xref:System.IO.Stream>時儲存到 blob 儲存。

## <a name="file-upload-scenarios"></a>檔案上傳機制

上傳檔的兩種常規方法是緩衝和流式傳輸。

**緩衝**

整個檔被讀入一<xref:Microsoft.AspNetCore.Http.IFormFile>個 ,它是用於處理或保存檔的檔的 C# 表示形式。

檔上載使用的資源(磁碟、記憶體)取決於併發檔上載的數量和大小。 如果應用嘗試緩衝過多的上載,則當網站記憶體或磁碟空間不足時,該網站將崩潰。 如果檔上載的大小或頻率耗盡了應用資源,請使用流式處理。

> [!NOTE]
> 任何超過 64 KB 的單個緩衝檔將從記憶體移動到磁碟上的臨時檔。

本主題的以下部分介紹了緩衝小檔:

* [實體儲存](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [Database](#upload-small-files-with-buffered-model-binding-to-a-database)

**串流**

該檔從多部分請求接收,並由應用直接處理或保存。 流式處理不會顯著提高性能。 流式傳輸減少了上傳檔時對記憶體或磁碟空間的需求。

流式處理大型檔在[「上傳包含流式處理的大型檔](#upload-large-files-with-streaming)」部分中介紹。

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a>將具有緩衝模型的小型檔案載入物理儲存

要上載小檔,請使用多部分窗體或使用 JAVAScript 構造 POST 請求。

下面的範例展示使用 Razor Pages 表單上載單檔案(例如範*單中的網頁/緩衝單檔案上傳物理.cshtml):*

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

以下示例與以前的示例類似,只不過:

* JavaScript 的 ([提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) 用於提交表單的資料。
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

要對[不支援 Fetch API 的](https://caniuse.com/#feat=fetch)用戶端在 JavaScript 執行表單 POST,請使用以下方法之一:

* 使用「擷取」多邊形填充(例如,[視窗.fetch 多邊填充(github/提取)。](https://github.com/github/fetch)
* 使用 `XMLHttpRequest`。 例如：

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

為了支援檔案中載,HTML 窗型必須指定的編碼型`enctype`態`multipart/form-data`( )

對於支援`files`上載多個檔案的輸入元素,`multiple``<input>`該 元素在元素上提供屬性:

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

上傳到伺服器的各個檔案可以使用[模型連結](xref:mvc/models/model-binding)<xref:Microsoft.AspNetCore.Http.IFormFile>。 範例應用示範了多個用於資料庫和實體存儲方案的緩衝檔上載。

<a name="filename2"></a>

> [!WARNING]
> **請勿**`FileName`使用 顯示和日<xref:Microsoft.AspNetCore.Http.IFormFile>誌記錄 以外的屬性。 顯示或紀錄記錄時,HTML 對檔案名進行編碼。 攻擊者可以提供惡意檔名,包括完整路徑或相對路徑。 應用程式應:
>
> * 從使用者提供的檔名中刪除路徑。
> * 儲存 HTML 編碼的路徑刪除的檔名,用於 UI 或紀錄記錄。
> * 生成新的隨機檔名進行存儲。
>
> 以下代碼從檔案名中刪除路徑:
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> 到目前為止提供的示例沒有考慮到安全注意事項。 其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

使用模型綁定<xref:Microsoft.AspNetCore.Http.IFormFile>和 上載檔時,操作方法可以接受:

* 單個<xref:Microsoft.AspNetCore.Http.IFormFile>。
* 表示多個檔案的以下任何集合:
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * [清單](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>

> [!NOTE]
> 綁定依名稱匹配表單檔。 例如,中的`name``<input type="file" name="formFile">`HTML 值必須與 C# 參數`FormFile`/ 屬性繫結 ( ) 符合。 關於詳細資訊,請參閱 POST[方法部份的參數名稱的比名稱屬性值](#match-name-attribute-value-to-parameter-name-of-post-method)。

下列範例將：

* 循環訪問一個或多個上傳的檔。
* 使用[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)傳回檔案的完整路徑,包括檔名。 
* 使用應用生成的檔案名將檔儲存到本地檔案系統。
* 返回上載的檔的總數和大小。

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

用於`Path.GetRandomFileName`生成沒有路徑的檔名。 在下面的範例中,路徑是從設定中取得的:

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

傳遞到<xref:System.IO.FileStream>的路徑*必須*包括檔名。 如果未提供檔案名稱,則在執行時引發<xref:System.UnauthorizedAccessException>。

在處理之前,使用<xref:Microsoft.AspNetCore.Http.IFormFile>該技術上載的檔將緩衝在記憶體中或伺服器上的磁碟上。 在操作方法中,<xref:Microsoft.AspNetCore.Http.IFormFile>內容可作為<xref:System.IO.Stream>可訪問。 除了本地檔案系統之外,檔案還可以保存到網路共享或檔案存儲服務(如[Azure Blob 儲存](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs))。

有關迴圈存取多個檔以進行上載並使用安全檔名的範例,請參閱範例應用中*的頁面/緩衝倍數檔檔物理.cshtml.cs。*

> [!WARNING]
> [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)<xref:System.IO.IOException>會引發一個,如果創建了超過 65,535 個檔,而不刪除以前的臨時檔。 65,535 個檔的限制是每個伺服器的限制。 有關 Windows 作業系統上此限制的詳細資訊,請參閱以下主題中的備註:
>
> * [取得tempFilenameA函式](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a>將具有緩衝模型的小檔案載到資料庫

要使用[實體框架](/ef/core/index)將二進位檔案資料儲存在資料庫中,<xref:System.Byte>請在實體上定義數位屬性:

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

指定的類別指定頁面模型屬性<xref:Microsoft.AspNetCore.Http.IFormFile>:

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
> <xref:Microsoft.AspNetCore.Http.IFormFile>可以直接用作操作方法參數或綁定模型屬性。 上一個示例使用綁定模型屬性。

在`FileUpload`剃刀頁窗體中使用:

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

將表單 POSTed 複製到伺服器時<xref:Microsoft.AspNetCore.Http.IFormFile>,將複製到流並將其另存為資料庫中的位元組。 在以下範例中,`_dbContext`儲存應用的資料庫內容:

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

前面的範例類似於範例應用中展示的方案:

* *頁面/緩衝單檔案上傳Db.cshtml*
* *頁面/緩衝單檔案上傳Db.cshtml.cs*

> [!WARNING]
> 將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。
>
> 未經驗證,不要依賴或信任`FileName`的屬性。 <xref:Microsoft.AspNetCore.Http.IFormFile> 該`FileName`屬性應僅用於顯示目的,並且僅在 HTML 編碼後使用。
>
> 提供的示例不考慮安全注意事項。 其他資訊由以下部分和[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)提供:
>
> * [安全性考量](#security-considerations)
> * [驗證](#validation)

### <a name="upload-large-files-with-streaming"></a>使用流式上傳大型檔案

下面的範例展示如何使用 JavaScript 將檔案流式傳輸到控制器操作。 該檔的反偽造權杖使用自定義篩選器屬性生成,並傳遞到客戶端 HTTP 標頭而不是請求正文中。 由於操作方法直接處理上載的數據,因此表單模型綁定被另一個自定義篩選器禁用。 在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。 讀取多部分部分后,操作將執行其自己的模型綁定。

初始頁面回應載入表單,並將反偽造權杖保存在 Cookie 中`GenerateAntiforgeryTokenCookieAttribute`(通過屬性 )。 該屬性使用 ASP.NET Core 的內建[反偽造支援](xref:security/anti-request-forgery)來設定具有請求權杖的 Cookie:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

`DisableFormValueModelBindingAttribute`關閉模型的系統:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

在`GenerateAntiforgeryTokenCookieAttribute`範例應用中,並`DisableFormValueModelBindingAttribute`作為篩選器應用`/StreamedSingleFileUploadDb``/StreamedSingleFileUploadPhysical``Startup.ConfigureServices`於使用 Razor Pages 約定的 頁面應用程式模型和 中的[頁面](xref:razor-pages/razor-pages-conventions)應用程式模型:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

由於模型綁定不讀取窗體,因此從窗體綁定的參數不會綁定(查詢、路由和標頭繼續工作)。 操作方法直接與 屬性`Request`一起工作。 `MultipartReader` 是用來讀取每個區段。 鍵/數值資料儲存在`KeyValueAccumulator`中 。 讀取多部分部分後,使用的內容`KeyValueAccumulator`將窗體數據綁定到模型類型。

使用`StreamingController.UploadDatabase`EF Core 串流式傳輸到資料庫的完整方法:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

`MultipartRequestHelper`(*公用程式/ 多部份要求説明者.cs*):

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

串流式傳輸`StreamingController.UploadPhysical`到 物理位置的完整方法:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

在範例應用中,驗證檢查由`FileHelpers.ProcessStreamedFile`處理 。

## <a name="validation"></a>驗證

示例應用的`FileHelpers`類演示了對緩<xref:Microsoft.AspNetCore.Http.IFormFile>衝 和流式檔上載的多次檢查。 有關在<xref:Microsoft.AspNetCore.Http.IFormFile>示例應用中處理緩衝檔上傳,請參閱`ProcessFormFile`*實用程式/FileHelpers.cs*檔中的方法。 有關處理流式處理檔,`ProcessStreamedFile`請參閱同一檔中的方法。

> [!WARNING]
> 範例應用中演示的驗證處理方法不會掃描上載文件的內容。 在大多數生產方案中,在將檔可供使用者或其他系統使用之前,在檔上使用病毒/惡意軟體掃描器 API。
>
> 儘管主題示例提供了驗證技術的工作示例,但除非在生產應用中實現`FileHelpers`類,否則不要實現該類:
>
> * 完全理解實施情況。
> * 根據需要修改應用的環境和規範的實現。
>
> **如果不滿足這些要求,切勿在應用中不加區別地實施安全代碼。**

### <a name="content-validation"></a>內容驗證

**對上傳的內容使用第三方病毒/惡意軟體掃描 API。**

在高容量方案中,掃描檔對伺服器資源的要求很高。 如果由於文件掃描而使請求處理性能降低,請考慮將掃描工作卸載到[後台服務](xref:fundamentals/host/hosted-services),可能是在伺服器上運行的服務不同於應用的伺服器。 通常,上傳的檔在隔離區域中,直到背景病毒掃描程式檢查它們。 當檔通過時,檔將移動到正常檔案存儲位置。 這些步驟通常與指示檔的掃描狀態的資料庫記錄一起執行。 通過使用這種方法,應用和應用程式伺服器仍然專注於回應請求。

### <a name="file-extension-validation"></a>檔案副檔名驗證

應根據允許的擴展清單檢查上載的檔案副檔名。 例如：

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a>檔案簽章驗證

檔的簽名由檔開頭的前幾個位元組決定。 這些位元組可用於指示擴展名是否與文件的內容匹配。 範例應用檢查檔簽名中有幾個常見的文件類型。 在下面的範例中,針對該檔案檢查 JPEG 影像的檔案簽名:

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

要獲取其他文件簽名,請參閱[文件簽名資料庫](https://www.filesignatures.net/)和官方文件規範。

### <a name="file-name-security"></a>檔案名稱安全性

切勿使用用戶端提供的檔案名將檔儲存到物理存儲。 使用[Path.GetRandomFilename](xref:System.IO.Path.GetRandomFileName*)或[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*)為檔案建立安全性的檔名,以創建用於暫時儲存的完整路徑(包括檔名)。

Razor 會自動對顯示的屬性值進行編碼。 以下代碼可安全使用:

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

在 Razor<xref:System.Net.WebUtility.HtmlEncode*>之外 ,始終從使用者請求中提交姓名內容。

許多實現必須包括檢查該檔是否存在;否則,該檔將被同名文件覆蓋。 提供其他邏輯以滿足應用的規範。

### <a name="size-validation"></a>大小驗證

限制上載檔的大小。

在範例應用中,檔的大小限制為2 MB(以位元組為單位)。 限制透過來自*應用程式設定.json*檔的[設定](xref:fundamentals/configuration/index)提供:

```json
{
  "FileSizeLimit": 2097152
}
```

註`FileSizeLimit`入 :`PageModel`

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

當檔案大小超過限制時,檔案將被拒絕:

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a>將名稱屬性值與 POST 方法的參數名稱符合

在 POST 形成資料或使用 JavaScript`FormData`的非 Razor 窗體中,在窗體`FormData`元素中指定的名稱或 必須與控制器操作中參數的名稱匹配。

在下例中︰

* 使用`<input>`元素時,屬性`name`設定為值`battlePlans`:

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* `FormData`在 JavaScript 中使用時,名稱設定`battlePlans`為 :

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

描述 C# 方法的參數使用符合`battlePlans`名稱 ( :

* 對於名為的`Upload`Razor 頁面頁面處理程式方法:

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* 對於 MVC POST 控制器操作方法:

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a>伺服器和應用程式設定

### <a name="multipart-body-length-limit"></a>多部份體長度限制

<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>設置每個多部分實體的長度限制。 超過此限制的表單的元件在<xref:System.IO.InvalidDataException>分析 時會引發 。 默認值為 134,217,728 (128 MB)。 使用的<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>`Startup.ConfigureServices`設定自訂限制:

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

<xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>為單一頁面或<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>操作設定 。

在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:

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

在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面模型或操作方法:

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a>凱斯特雷爾最大請求正文大小

對於 Kestrel 託管的應用,預設的最大請求正文大小為 30,000,000 位元組,約為 28.6 MB。 使用[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel 伺服器選項自訂限制:

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

<xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>用於為單個頁面或操作設置[最大請求博量。](xref:fundamentals/servers/kestrel#maximum-request-body-size)

在 Razor Pages 應用中[convention](xref:razor-pages/razor-pages-conventions)`Startup.ConfigureServices`,在 中 應用具有約定的篩選器:

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

在 Razor 頁面應用程式或 MVC 應用中,將篩選器應用於頁面處理程式類或操作方法:

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a>其他凱斯特雷爾限制

其他凱斯特雷爾限制可能適用於由凱斯特雷爾託管的應用程式:

* [用戶端連線數目上限](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [要求與回應資料速率](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a>IIS 內容長度限制

默認請求限制`maxAllowedContentLength`( ) 是 30,000,000 位元組,約為 28.6MB。 自訂*Web.config*檔案中的限制:

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

這個設定只適用於 IIS。 在 Kestrel 上裝載時，預設不會發生此行為。 有關詳細資訊,請參閱[要求\<限制 請求限制>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。

ASP.NET核心模組或IIS請求篩選模組的存在的限制可能會將上載限制為2或4 GB。 有關詳細資訊,請參閱[無法上載大小大於 2GB 的檔(dotnet/AspNetCore #2711)。](https://github.com/dotnet/AspNetCore/issues/2711)

## <a name="troubleshoot"></a>疑難排解

以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。

### <a name="not-found-error-when-deployed-to-an-iis-server"></a>部署到 IIS 伺服器時找不到錯誤

以下錯誤指示上載的檔案超過伺服器設定的內容長度:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

有關增加限制的詳細資訊,請參閱[IIS內容長度限制](#iis-content-length-limit)部分。

### <a name="connection-failure"></a>連線失敗

連接錯誤和重置伺服器連接可能表示上載的文件超過 Kestrel 的最大請求正文大小。 有關詳細資訊,請參閱[Kestrel 最大請求正文大小](#kestrel-maximum-request-body-size)部分。 Kestrel 用戶端連接限制可能還需要調整。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile 的 Null 參考例外狀況

如果控制器<xref:Microsoft.AspNetCore.Http.IFormFile>正在使用 接受上載的檔案,但`null`值為 ,請確認`enctype`HTML`multipart/form-data`窗體指定的值 。 如果未在`<form>`元素上設定此屬性,則不會發生檔案上載,並且任何繫<xref:Microsoft.AspNetCore.Http.IFormFile>結參數`null`都是 。 您可以確認[表單資料中的上傳命名與套用的命名符合](#match-name-attribute-value-to-parameter-name-of-post-method)。

### <a name="stream-was-too-long"></a>流太長

本主題中的範例依賴於<xref:System.IO.MemoryStream>保存上載的文件的內容。 的大小限制`MemoryStream`為`int.MaxValue`。 如果應用的檔上載方案需要保存大於 50 MB 的檔內容,請使用不`MemoryStream`依賴單個 檔案來保存上載檔內容的替代方法。

::: moniker-end


## <a name="additional-resources"></a>其他資源

* [不受限制的檔案上傳](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
* [Azure 安全性:安全框架:輸入驗證 |緩解措施](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [Azure 雲設計模式:代客金鑰模式](/azure/architecture/patterns/valet-key)
