---
title: ASP.NET Core 的檔案上傳
author: ardalis
description: 如何使用模型繫結和資料流在 ASP.NET Core MVC 上傳檔案。
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 3db2e08acc1552957f28c7015f9a75af4a57fdcd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65084855"
---
# <a name="file-uploads-in-aspnet-core"></a>ASP.NET Core 的檔案上傳

作者：[Steve Smith](https://ardalis.com/)

ASP.NET MVC 動作支援上傳一或多個檔案，方法是針對較小的檔案使用簡單模型繫結，或針對較大的檔案使用資料流。

[從 GitHub 檢視或下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>使用模型繫結上傳小型檔案

若要上傳小型檔案，您可以使用多部分 HTML 表單，或使用 JavaScript 建構 POST 要求。 使用支援多個已上傳檔案之 Razor 的範例表單如下所示：

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple>
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload">
        </div>
    </div>
</form>
```

若要支援檔案上傳，HTML 表單必須指定 `multipart/form-data` 的 `enctype`。 上述 `files` 輸入項目支援上傳多個檔案。 省略此輸入項目上的 `multiple` 屬性，只允許上傳單一檔案。 上述標記在瀏覽器中會轉譯為：

![檔案上傳表單](file-uploads/_static/upload-form.png)

使用 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 介面，可以透過[模型繫結](xref:mvc/models/model-binding)存取已上傳至伺服器的個別檔案。 `IFormFile` 具有下列結構：

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> 不依賴或不信任無驗證的 `FileName` 屬性。 `FileName` 屬性只應該用於顯示。

使用模型繫結和 `IFormFile` 介面上傳檔案時，動作方法可以接受代表數個檔案的單一 `IFormFile` 或 `IEnumerable<IFormFile>` (或 `List<IFormFile>`)。 下列範例會循環使用一或多個已上傳檔案、將它們儲存至本機檔案系統，並傳回已上傳檔案的總數和大小。

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

會先將使用 `IFormFile` 技術所上傳的檔案緩衝至記憶體或是網頁伺服器的磁碟上，再進行處理。 在動作方法內，`IFormFile` 內容可以當成資料流形式來存取。 除了本機檔案系統之外，檔案還可以串流至 [Azure Blob 儲存體](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)或 [Entity Framework](/ef/core/index)。

若要使用 Entity Framework 將二進位檔案資料儲存至資料庫，請定義實體上類型 `byte[]` 的屬性：

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

指定 `IFormFile` 類型的 viewmodel 屬性：

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` 可以直接用作動作方法參數或 viewmodel 屬性，如上所示。

將 `IFormFile` 複製至資料流，並將其儲存至位元組陣列：

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser 
        {
            UserName = model.Email,
            Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted

    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> 將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。

## <a name="uploading-large-files-with-streaming"></a>使用資料流上傳大型檔案

如果檔案上傳的大小或頻率造成應用程式的資源問題，請考慮串流檔案上傳，而不是緩衝整個檔案，而這與上述模型繫結方法相同。 雖然使用 `IFormFile` 和模型繫結是比較簡單的解決方案，但是串流需要數個步驟才能正確地實作。

> [!NOTE]
> 如果有任何單一緩衝的檔案超過 64 KB，則會將其從 RAM 移至伺服器之磁碟上的暫存檔案。 檔案上傳所使用的資源 (磁碟、RAM) 取決於並行檔案上傳次數和大小。 串流與效能的關聯不大，而是與規模的關聯較大。 如果您嘗試要緩衝太多上傳，則記憶體或磁碟空間不足時，會損毀您的網站。

下列範例示範如何使用 JavaScript/Angular 來串流至控制器動作。 使用自訂篩選屬性並傳入 HTTP 標頭來產生檔案的 antiforgery 權杖，而不是傳入要求本文。 因為動作方法會直接處理已上傳的資料，所以另一個篩選會停用模型繫結。 在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。 讀取所有區段之後，動作會執行它自己的模型繫結。

初始動作會載入表單，並將 antiforgery 權杖儲存至 Cookie (透過 `GenerateAntiforgeryTokenCookieForAjax` 屬性)：

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

此屬性使用 ASP.NET Core 的內建 [Antiforgery](xref:security/anti-request-forgery) 支援，來設定含要求權杖的 Cookie：

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

角度會在名為 `X-XSRF-TOKEN` 的要求標頭中自動傳遞 antiforgery 權杖。 ASP.NET Core MVC 應用程式設定成在 *Startup.cs* 的其組態中參照此標頭：

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

下述中的 `DisableFormValueModelBinding` 屬性是用來停用 `Upload` 動作方法的模型繫結。 

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

因為已停用模型繫結，所以 `Upload` 動作方法不會接受參數。 它會直接使用 `ControllerBase` 的 `Request` 屬性。 `MultipartReader` 是用來讀取每個區段。 檔案會與 GUID 檔案名稱一起儲存，而索引鍵/值資料會儲存至 `KeyValueAccumulator`。 讀取所有區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料繫結至模型類型。

完整 `Upload` 方法如下：

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>疑難排解

以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。

### <a name="unexpected-not-found-error-with-iis"></a>IIS 的未預期找不到錯誤

下列錯誤指出您的檔案上傳超過伺服器所設定的 `maxAllowedContentLength`：

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

預設設定是 `30000000`，其大約是 28.6 MB。 編輯 *web.config*，即可自訂值：

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

這個設定只適用於 IIS。 在 Kestrel 上裝載時，預設不會發生此行為。 如需詳細資訊，請參閱[要求限制\<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile 的 Null 參考例外狀況

如果您的控制器接受使用 `IFormFile` 的已上傳檔案，但您發現值一律是 Null，則請確認 HTML 表單將會指定 `multipart/form-data` 的 `enctype` 值。 如果未在 `<form>` 項目上設定此屬性，則不會進行檔案上傳，而且任何繫結的 `IFormFile` 引數都會是 Null。
