---
title: "在 ASP.NET Core 檔案上傳"
author: ardalis
description: "如何使用模型繫結和資料流上傳 ASP.NET Core MVC 中的檔案。"
keywords: "ASP.NET Core，檔案上傳，模型繫結 IFormFile，資料流"
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: ebc98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 3d114f8d13345cb260430847e80a61500de170b4
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2017
---
# <a name="file-uploads-in-aspnet-core"></a>在 ASP.NET Core 檔案上傳

由[Steve Smith](https://ardalis.com/)

ASP.NET MVC 動作支援的一或多個檔案使用簡單的模型繫結為較小的檔案或資料流處理的較大的檔案上傳。

[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>將小型檔案上傳使用模型繫結

若要上傳的小檔案，您可以使用多部分的 HTML 表單，或建構使用 JavaScript 的 POST 要求。 範例表單中使用 Razor，支援多個已上傳的檔案，如下所示：

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

若要支援的檔案上傳，必須指定 HTML 表單`enctype`的`multipart/form-data`。 `files`如上所示的輸入項目支援上傳多個檔案。 省略`multiple`允許只將單一檔案上傳這個輸入項目上的屬性。 上述的標記會轉譯為在瀏覽器中：

![檔案上傳的表單](file-uploads/_static/upload-form.png)

個別檔案上傳至伺服器可以透過存取[模型繫結](xref:mvc/models/model-binding)使用[IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile)介面。 `IFormFile`具有下列結構：

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
> 不依賴或不信任`FileName`不需要驗證的屬性。 `FileName`屬性應該只用於顯示用途。

上傳使用模型繫結檔案時，`IFormFile`介面，動作方法可接受單一`IFormFile`或`IEnumerable<IFormFile>`(或`List<IFormFile>`) 代表數個檔案。 下列範例會循環一或多個上傳的檔案、 將它們儲存到本機檔案系統中，並傳回總數與上傳的檔案大小。

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

使用上傳的檔案`IFormFile`技術緩衝在記憶體中或在 web 伺服器上的磁碟上然後再開始處理。 在動作方法中，內`IFormFile`內容是可存取資料流的形式。 除了本機檔案系統中，檔案可以串流處理至[Azure Blob 儲存體](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)或[Entity Framework](https://docs.microsoft.com/ef/core/index)。

若要將二進位檔案資料儲存在資料庫中使用 Entity Framework 中，定義 型別屬性`byte[]`實體上：

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

指定型別的 viewmodel 屬性`IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile`可做為動作方法參數，或作為 viewmodel 屬性，如上所示。

複製`IFormFile`至資料流，並將其儲存至位元組陣列：

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
        var user = new ApplicationUser {
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
> 請謹慎將二進位資料儲存在關聯式資料庫，因為它可能會造成不良影響效能。

## <a name="uploading-large-files-with-streaming"></a>使用資料流上傳大型檔案

如果大小或檔案上傳的頻率會造成應用程式的資源問題，請考慮資料流檔案上傳，而非緩衝整個，如上所示的模型繫結方法一樣。 同時使用`IFormFile`和模型繫結是比較簡單的解決方案，資料流需要的步驟，以實作正確的數字。

> [!NOTE]
> 任何單一緩衝超過 64KB 會移動檔案從 RAM 磁碟上的暫存檔案伺服器上。 檔案上傳所使用的資源 （磁碟、 RAM） 的數目和並行的檔案上傳的大小而定。 資料流不是這麼多關於效能，了小數位數。 如果您嘗試要緩衝處理太多上傳，記憶體或磁碟空間不足時，會損毀您的網站。

下列範例會示範使用 JavaScript/Angular 串流至控制器動作。 使用自訂的篩選條件屬性，而不是在要求主體中的 HTTP 標頭中傳遞，就會產生檔案的 antiforgery 語彙基元。 動作方法會直接處理上傳的資料，因為另一個篩選條件會停用模型繫結。 動作，在表單的內容會使用讀取`MultipartReader`，讀取每個個別`MultipartSection`、 處理檔案，或視需要儲存內容。 一旦已經讀取所有區段，此動作會在執行它自己的模型繫結。

初始化動作載入表單，並儲存在 cookie 中的 antiforgery 的語彙基元 (透過`GenerateAntiforgeryTokenCookieForAjax`屬性):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

屬性會使用 ASP.NET Core 內建[Antiforgery](xref:security/anti-request-forgery)設定要求的語彙基元的 cookie 的支援：

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

角度自動傳遞 antiforgery 的語彙基元中名為要求標頭`X-XSRF-TOKEN`。 ASP.NET Core MVC 應用程式設定為在其設定，此標頭是指*Startup.cs*:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

`DisableFormValueModelBinding` ，如下所示的屬性用來停用的模型繫結`Upload`動作方法。

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

模型繫結已停用，因為`Upload`動作方法不會接受參數。 它適用於直接`Request`屬性`ControllerBase`。 A`MultipartReader`用來讀取每個區段。 GUID 檔案名稱儲存檔案，以及索引鍵/值資料會儲存在`KeyValueAccumulator`。 一旦已經讀取所有區段，內容`KeyValueAccumulator`用來將表單資料繫結的模型型別。

完整`Upload`方法如下所示：

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>疑難排解

以下是使用上傳的檔案和可能的解決方案時，發現一些常見的問題。

### <a name="unexpected-not-found-error-with-iis"></a>找不到的未預期的錯誤 iis

下列的錯誤指出您的檔案上傳超過伺服器所設定的`maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

預設值是`30000000`，這是大約 28.6 MB。 值可以藉由編輯自訂*web.config*:

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

這項設定僅適用於 IIS。 裝載 Kestrel 上時，預設不會發生行為。 如需詳細資訊，請參閱[要求限制\<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile null 參考例外狀況

如果您的控制器是接受上傳檔案使用`IFormFile`，但您尋找這個值一律是 null，請確認已指定 HTML 表單`enctype`值`multipart/form-data`。 如果這個屬性未設定上`<form>`項目，檔案上傳不會和任何繫結`IFormFile`引數將會是 null。
