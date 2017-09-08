---
title: "在 ASP.NET Core 檔案上傳"
author: ardalis
description: "如何使用模型繫結和資料流上傳 ASP.NET Core MVC 中的檔案。"
keywords: "ASP.NET Core，檔案上傳，模型繫結 IFormFile，資料流"
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: ebc98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 78cc9cd846f9b0963dbba9069c86ca295f7a32e4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="bea03-104">在 ASP.NET Core 檔案上傳</span><span class="sxs-lookup"><span data-stu-id="bea03-104">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="bea03-105">由[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="bea03-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="bea03-106">ASP.NET MVC 動作支援的一或多個檔案使用簡單的模型繫結為較小的檔案或資料流處理的較大的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="bea03-106">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="bea03-107">檢視或從 GitHub 下載範例</span><span class="sxs-lookup"><span data-stu-id="bea03-107">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="bea03-108">將小型檔案上傳使用模型繫結</span><span class="sxs-lookup"><span data-stu-id="bea03-108">Uploading small files with model binding</span></span>

<span data-ttu-id="bea03-109">若要上傳的小檔案，您可以使用多部分的 HTML 表單，或建構使用 JavaScript 的 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="bea03-109">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="bea03-110">範例表單中使用 Razor，支援多個已上傳的檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bea03-110">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

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

<span data-ttu-id="bea03-111">若要支援的檔案上傳，必須指定 HTML 表單`enctype`的`multipart/form-data`。</span><span class="sxs-lookup"><span data-stu-id="bea03-111">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="bea03-112">`files`如上所示的輸入項目支援上傳多個檔案。</span><span class="sxs-lookup"><span data-stu-id="bea03-112">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="bea03-113">省略`multiple`允許只將單一檔案上傳這個輸入項目上的屬性。</span><span class="sxs-lookup"><span data-stu-id="bea03-113">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="bea03-114">上述的標記會轉譯為在瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="bea03-114">The above markup renders in a browser as:</span></span>

![檔案上傳的表單](file-uploads/_static/upload-form.png)

<span data-ttu-id="bea03-116">個別檔案上傳至伺服器可以透過存取[模型繫結](xref:mvc/models/model-binding)使用[IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile)介面。</span><span class="sxs-lookup"><span data-stu-id="bea03-116">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="bea03-117">`IFormFile`具有下列結構：</span><span class="sxs-lookup"><span data-stu-id="bea03-117">`IFormFile` has the following structure:</span></span>

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
> <span data-ttu-id="bea03-118">不依賴或不信任`FileName`不需要驗證的屬性。</span><span class="sxs-lookup"><span data-stu-id="bea03-118">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="bea03-119">`FileName`屬性應該只用於顯示用途。</span><span class="sxs-lookup"><span data-stu-id="bea03-119">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="bea03-120">上傳使用模型繫結檔案時，`IFormFile`介面，動作方法可接受單一`IFormFile`或`IEnumerable<IFormFile>`(或`List<IFormFile>`) 代表數個檔案。</span><span class="sxs-lookup"><span data-stu-id="bea03-120">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="bea03-121">下列範例會循環一或多個上傳的檔案、 將它們儲存到本機檔案系統中，並傳回總數與上傳的檔案大小。</span><span class="sxs-lookup"><span data-stu-id="bea03-121">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

<span data-ttu-id="bea03-122">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="bea03-122">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]</span></span>

<span data-ttu-id="bea03-123">使用上傳的檔案`IFormFile`技術緩衝在記憶體中或在 web 伺服器上的磁碟上然後再開始處理。</span><span class="sxs-lookup"><span data-stu-id="bea03-123">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="bea03-124">在動作方法中，內`IFormFile`內容是可存取資料流的形式。</span><span class="sxs-lookup"><span data-stu-id="bea03-124">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="bea03-125">除了本機檔案系統中，檔案可以串流處理至[Azure Blob 儲存體](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs)或[Entity Framework](https://docs.microsoft.com/ef/core/index)。</span><span class="sxs-lookup"><span data-stu-id="bea03-125">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="bea03-126">若要將二進位檔案資料儲存在資料庫中使用 Entity Framework 中，定義 型別屬性`byte[]`實體上：</span><span class="sxs-lookup"><span data-stu-id="bea03-126">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="bea03-127">指定型別的 viewmodel 屬性`IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="bea03-127">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="bea03-128">`IFormFile`可做為動作方法參數，或作為 viewmodel 屬性，如上所示。</span><span class="sxs-lookup"><span data-stu-id="bea03-128">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="bea03-129">複製`IFormFile`至資料流，並將其儲存至位元組陣列：</span><span class="sxs-lookup"><span data-stu-id="bea03-129">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

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
> <span data-ttu-id="bea03-130">請謹慎將二進位資料儲存在關聯式資料庫，因為它可能會造成不良影響效能。</span><span class="sxs-lookup"><span data-stu-id="bea03-130">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="bea03-131">使用資料流上傳大型檔案</span><span class="sxs-lookup"><span data-stu-id="bea03-131">Uploading large files with streaming</span></span>

<span data-ttu-id="bea03-132">如果大小或檔案上傳的頻率會造成應用程式的資源問題，請考慮資料流檔案上傳，而非緩衝整個，如上所示的模型繫結方法一樣。</span><span class="sxs-lookup"><span data-stu-id="bea03-132">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="bea03-133">同時使用`IFormFile`和模型繫結是比較簡單的解決方案，資料流需要的步驟，以實作正確的數字。</span><span class="sxs-lookup"><span data-stu-id="bea03-133">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="bea03-134">任何單一緩衝超過 64KB 會移動檔案從 RAM 磁碟上的暫存檔案伺服器上。</span><span class="sxs-lookup"><span data-stu-id="bea03-134">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="bea03-135">檔案上傳所使用的資源 （磁碟、 RAM） 的數目和並行的檔案上傳的大小而定。</span><span class="sxs-lookup"><span data-stu-id="bea03-135">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="bea03-136">資料流不是這麼多關於效能，了小數位數。</span><span class="sxs-lookup"><span data-stu-id="bea03-136">Streaming is not so much about perf, it's about scale.</span></span> <span data-ttu-id="bea03-137">如果您嘗試要緩衝處理太多上傳，記憶體或磁碟空間不足時，會損毀您的網站。</span><span class="sxs-lookup"><span data-stu-id="bea03-137">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="bea03-138">下列範例會示範使用 JavaScript/Angular 串流至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="bea03-138">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="bea03-139">使用自訂的篩選條件屬性，而不是在要求主體中的 HTTP 標頭中傳遞，就會產生檔案的 antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="bea03-139">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="bea03-140">動作方法會直接處理上傳的資料，因為另一個篩選條件會停用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="bea03-140">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="bea03-141">動作，在表單的內容會使用讀取`MultipartReader`，讀取每個個別`MultipartSection`、 處理檔案，或視需要儲存內容。</span><span class="sxs-lookup"><span data-stu-id="bea03-141">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="bea03-142">一旦已經讀取所有區段，此動作會在執行它自己的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="bea03-142">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="bea03-143">初始化動作載入表單，並儲存在 cookie 中的 antiforgery 的語彙基元 (透過`GenerateAntiforgeryTokenCookieForAjax`屬性):</span><span class="sxs-lookup"><span data-stu-id="bea03-143">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="bea03-144">屬性會使用 ASP.NET Core 內建[Antiforgery](xref:security/anti-request-forgery)設定要求的語彙基元的 cookie 的支援：</span><span class="sxs-lookup"><span data-stu-id="bea03-144">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

<span data-ttu-id="bea03-145">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="bea03-145">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]</span></span>

<span data-ttu-id="bea03-146">角度自動傳遞 antiforgery 的語彙基元中名為要求標頭`X-XSRF-TOKEN`。</span><span class="sxs-lookup"><span data-stu-id="bea03-146">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="bea03-147">ASP.NET Core MVC 應用程式設定為在其設定，此標頭是指*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bea03-147">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

<span data-ttu-id="bea03-148">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="bea03-148">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]</span></span>

<span data-ttu-id="bea03-149">`DisableFormValueModelBinding` ，如下所示的屬性用來停用的模型繫結`Upload`動作方法。</span><span class="sxs-lookup"><span data-stu-id="bea03-149">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

<span data-ttu-id="bea03-150">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="bea03-150">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]</span></span>

<span data-ttu-id="bea03-151">模型繫結已停用，因為`Upload`動作方法不會接受參數。</span><span class="sxs-lookup"><span data-stu-id="bea03-151">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="bea03-152">它適用於直接`Request`屬性`ControllerBase`。</span><span class="sxs-lookup"><span data-stu-id="bea03-152">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="bea03-153">A`MultipartReader`用來讀取每個區段。</span><span class="sxs-lookup"><span data-stu-id="bea03-153">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="bea03-154">GUID 檔案名稱儲存檔案，以及索引鍵/值資料會儲存在`KeyValueAccumulator`。</span><span class="sxs-lookup"><span data-stu-id="bea03-154">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="bea03-155">一旦已經讀取所有區段，內容`KeyValueAccumulator`用來將表單資料繫結的模型型別。</span><span class="sxs-lookup"><span data-stu-id="bea03-155">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="bea03-156">完整`Upload`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="bea03-156">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

<span data-ttu-id="bea03-157">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="bea03-157">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bea03-158">疑難排解</span><span class="sxs-lookup"><span data-stu-id="bea03-158">Troubleshooting</span></span>

<span data-ttu-id="bea03-159">以下是使用上傳的檔案和可能的解決方案時，發現一些常見的問題。</span><span class="sxs-lookup"><span data-stu-id="bea03-159">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="bea03-160">找不到的未預期的錯誤 iis</span><span class="sxs-lookup"><span data-stu-id="bea03-160">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="bea03-161">下列的錯誤指出您的檔案上傳超過伺服器所設定的`maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="bea03-161">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="bea03-162">預設值是`30000000`，這是大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="bea03-162">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="bea03-163">值可以藉由編輯自訂*web.config*:</span><span class="sxs-lookup"><span data-stu-id="bea03-163">The value can be customized by editing *web.config*:</span></span>

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

<span data-ttu-id="bea03-164">這項設定僅適用於 IIS。</span><span class="sxs-lookup"><span data-stu-id="bea03-164">This setting only applies to IIS.</span></span> <span data-ttu-id="bea03-165">裝載 Kestrel 上時，預設不會發生行為。</span><span class="sxs-lookup"><span data-stu-id="bea03-165">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="bea03-166">如需詳細資訊，請參閱[要求限制\<requestLimits\>](https://www.iis.net/configreference/system.webserver/security/requestfiltering/requestlimits)。</span><span class="sxs-lookup"><span data-stu-id="bea03-166">For more information, see [Request Limits \<requestLimits\>](https://www.iis.net/configreference/system.webserver/security/requestfiltering/requestlimits).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="bea03-167">IFormFile null 參考例外狀況</span><span class="sxs-lookup"><span data-stu-id="bea03-167">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="bea03-168">如果您的控制器是接受上傳檔案使用`IFormFile`，但您尋找這個值一律是 null，請確認已指定 HTML 表單`enctype`值`multipart/form-data`。</span><span class="sxs-lookup"><span data-stu-id="bea03-168">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="bea03-169">如果這個屬性未設定上`<form>`項目，檔案上傳不會和任何繫結`IFormFile`引數將會是 null。</span><span class="sxs-lookup"><span data-stu-id="bea03-169">If this attribute is not set on the `<form>` element, the file upload will not occur and any bound `IFormFile` arguments will be null.</span></span>
