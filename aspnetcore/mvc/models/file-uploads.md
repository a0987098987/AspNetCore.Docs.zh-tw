---
title: ASP.NET Core 的檔案上傳
author: ardalis
description: 如何使用模型繫結和資料流在 ASP.NET Core MVC 上傳檔案。
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: 7ba4f6d9e3901c310fe9fa7a70382d9243d8b347
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="a5009-103">ASP.NET Core 的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="a5009-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="a5009-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a5009-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a5009-105">ASP.NET MVC 動作支援上傳一或多個檔案，方法是針對較小的檔案使用簡單模型繫結，或針對較大的檔案使用資料流。</span><span class="sxs-lookup"><span data-stu-id="a5009-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="a5009-106">從 GitHub 檢視或下載範例</span><span class="sxs-lookup"><span data-stu-id="a5009-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="a5009-107">使用模型繫結上傳小型檔案</span><span class="sxs-lookup"><span data-stu-id="a5009-107">Uploading small files with model binding</span></span>

<span data-ttu-id="a5009-108">若要上傳小型檔案，您可以使用多部分 HTML 表單，或使用 JavaScript 建構 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="a5009-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="a5009-109">使用支援多個已上傳檔案之 Razor 的範例表單如下所示：</span><span class="sxs-lookup"><span data-stu-id="a5009-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

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

<span data-ttu-id="a5009-110">若要支援檔案上傳，HTML 表單必須指定 `multipart/form-data` 的 `enctype`。</span><span class="sxs-lookup"><span data-stu-id="a5009-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="a5009-111">上述 `files` 輸入項目支援上傳多個檔案。</span><span class="sxs-lookup"><span data-stu-id="a5009-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="a5009-112">省略此輸入項目上的 `multiple` 屬性，只允許上傳單一檔案。</span><span class="sxs-lookup"><span data-stu-id="a5009-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="a5009-113">上述標記在瀏覽器中會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="a5009-113">The above markup renders in a browser as:</span></span>

![檔案上傳表單](file-uploads/_static/upload-form.png)

<span data-ttu-id="a5009-115">使用 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 介面，可以透過[模型繫結](xref:mvc/models/model-binding)存取已上傳至伺服器的個別檔案。</span><span class="sxs-lookup"><span data-stu-id="a5009-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="a5009-116">`IFormFile` 具有下列結構：</span><span class="sxs-lookup"><span data-stu-id="a5009-116">`IFormFile` has the following structure:</span></span>

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
> <span data-ttu-id="a5009-117">不依賴或不信任無驗證的 `FileName` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a5009-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="a5009-118">`FileName` 屬性只應該用於顯示。</span><span class="sxs-lookup"><span data-stu-id="a5009-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="a5009-119">使用模型繫結和 `IFormFile` 介面上傳檔案時，動作方法可以接受代表數個檔案的單一 `IFormFile` 或 `IEnumerable<IFormFile>` (或 `List<IFormFile>`)。</span><span class="sxs-lookup"><span data-stu-id="a5009-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="a5009-120">下列範例會循環使用一或多個已上傳檔案、將它們儲存至本機檔案系統，並傳回已上傳檔案的總數和大小。</span><span class="sxs-lookup"><span data-stu-id="a5009-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="a5009-121">會先將使用 `IFormFile` 技術所上傳的檔案緩衝至記憶體或是網頁伺服器的磁碟上，再進行處理。</span><span class="sxs-lookup"><span data-stu-id="a5009-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="a5009-122">在動作方法內，`IFormFile` 內容可以當成資料流形式來存取。</span><span class="sxs-lookup"><span data-stu-id="a5009-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="a5009-123">除了本機檔案系統之外，檔案還可以串流至 [Azure Blob 儲存體](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)或 [Entity Framework](https://docs.microsoft.com/ef/core/index)。</span><span class="sxs-lookup"><span data-stu-id="a5009-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="a5009-124">若要使用 Entity Framework 將二進位檔案資料儲存至資料庫，請定義實體上類型 `byte[]` 的屬性：</span><span class="sxs-lookup"><span data-stu-id="a5009-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="a5009-125">指定 `IFormFile` 類型的 viewmodel 屬性：</span><span class="sxs-lookup"><span data-stu-id="a5009-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="a5009-126">`IFormFile` 可以直接用作動作方法參數或 viewmodel 屬性，如上所示。</span><span class="sxs-lookup"><span data-stu-id="a5009-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="a5009-127">將 `IFormFile` 複製至資料流，並將其儲存至位元組陣列：</span><span class="sxs-lookup"><span data-stu-id="a5009-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

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
> <span data-ttu-id="a5009-128">將二進位資料儲存至關聯式資料庫時請小心，因為它可能會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="a5009-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="a5009-129">使用資料流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="a5009-129">Uploading large files with streaming</span></span>

<span data-ttu-id="a5009-130">如果檔案上傳的大小或頻率造成應用程式的資源問題，請考慮串流檔案上傳，而不是緩衝整個檔案，而這與上述模型繫結方法相同。</span><span class="sxs-lookup"><span data-stu-id="a5009-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="a5009-131">雖然使用 `IFormFile` 和模型繫結是比較簡單的解決方案，但是串流需要數個步驟才能正確地實作。</span><span class="sxs-lookup"><span data-stu-id="a5009-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="a5009-132">如果有任何單一緩衝的檔案超過 64 KB，則會將其從 RAM 移至伺服器之磁碟上的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="a5009-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="a5009-133">檔案上傳所使用的資源 (磁碟、RAM) 取決於並行檔案上傳次數和大小。</span><span class="sxs-lookup"><span data-stu-id="a5009-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="a5009-134">串流與效能的關聯不大，而是與規模的關聯較大。</span><span class="sxs-lookup"><span data-stu-id="a5009-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="a5009-135">如果您嘗試要緩衝太多上傳，則記憶體或磁碟空間不足時，會損毀您的網站。</span><span class="sxs-lookup"><span data-stu-id="a5009-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="a5009-136">下列範例示範如何使用 JavaScript/Angular 來串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="a5009-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="a5009-137">使用自訂篩選屬性並傳入 HTTP 標頭來產生檔案的 antiforgery 權杖，而不是傳入要求本文。</span><span class="sxs-lookup"><span data-stu-id="a5009-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="a5009-138">因為動作方法會直接處理已上傳的資料，所以另一個篩選會停用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="a5009-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="a5009-139">在動作內，會使用 `MultipartReader` 來讀取表單內容，以讀取每個個別 `MultipartSection`、處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="a5009-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="a5009-140">讀取所有區段之後，動作會執行它自己的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="a5009-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="a5009-141">初始動作會載入表單，並將 antiforgery 權杖儲存至 Cookie (透過 `GenerateAntiforgeryTokenCookieForAjax` 屬性)：</span><span class="sxs-lookup"><span data-stu-id="a5009-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="a5009-142">此屬性使用 ASP.NET Core 的內建 [Antiforgery](xref:security/anti-request-forgery) 支援，來設定含要求權杖的 Cookie：</span><span class="sxs-lookup"><span data-stu-id="a5009-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="a5009-143">角度會在名為 `X-XSRF-TOKEN` 的要求標頭中自動傳遞 antiforgery 權杖。</span><span class="sxs-lookup"><span data-stu-id="a5009-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="a5009-144">ASP.NET Core MVC 應用程式設定成在 *Startup.cs* 的其組態中參照此標頭：</span><span class="sxs-lookup"><span data-stu-id="a5009-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="a5009-145">上述 `DisableFormValueModelBinding` 屬性是用來停用 `Upload` 動作方法的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="a5009-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="a5009-146">因為已停用模型繫結，所以 `Upload` 動作方法不會接受參數。</span><span class="sxs-lookup"><span data-stu-id="a5009-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="a5009-147">它會直接使用 `ControllerBase` 的 `Request` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a5009-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="a5009-148">`MultipartReader` 是用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="a5009-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="a5009-149">檔案會與 GUID 檔案名稱一起儲存，而索引鍵/值資料會儲存至 `KeyValueAccumulator`。</span><span class="sxs-lookup"><span data-stu-id="a5009-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="a5009-150">讀取所有區段之後，會使用 `KeyValueAccumulator` 的內容，將表單資料繫結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="a5009-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="a5009-151">完整 `Upload` 方法如下：</span><span class="sxs-lookup"><span data-stu-id="a5009-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="a5009-152">疑難排解</span><span class="sxs-lookup"><span data-stu-id="a5009-152">Troubleshooting</span></span>

<span data-ttu-id="a5009-153">以下是使用上傳檔案和其可能解決方案時發現的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="a5009-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="a5009-154">IIS 的未預期找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="a5009-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="a5009-155">下列錯誤指出您的檔案上傳超過伺服器所設定的 `maxAllowedContentLength`：</span><span class="sxs-lookup"><span data-stu-id="a5009-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="a5009-156">預設設定是 `30000000`，其大約是 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="a5009-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="a5009-157">編輯 *web.config*，即可自訂值：</span><span class="sxs-lookup"><span data-stu-id="a5009-157">The value can be customized by editing *web.config*:</span></span>

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

<span data-ttu-id="a5009-158">這個設定只適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="a5009-158">This setting only applies to IIS.</span></span> <span data-ttu-id="a5009-159">在 Kestrel 上裝載時，預設不會發生此行為。</span><span class="sxs-lookup"><span data-stu-id="a5009-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="a5009-160">如需詳細資訊，請參閱[要求限制\<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)。</span><span class="sxs-lookup"><span data-stu-id="a5009-160">For more information, see [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="a5009-161">IFormFile 的 Null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="a5009-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="a5009-162">如果您的控制器接受使用 `IFormFile` 的已上傳檔案，但您發現值一律是 Null，則請確認 HTML 表單將會指定 `multipart/form-data` 的 `enctype` 值。</span><span class="sxs-lookup"><span data-stu-id="a5009-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="a5009-163">如果未在 `<form>` 項目上設定此屬性，則不會進行檔案上傳，而且任何繫結的 `IFormFile` 引數都會是 Null。</span><span class="sxs-lookup"><span data-stu-id="a5009-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
