---
title: 將檔案上傳至 ASP.NET Core 的 Razor 頁面
author: guardrex
description: 了解如何將檔案上傳至 Razor Page。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/11/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 92e72869967b6e3202c97b92e341ea22adc69651
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912497"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a>將檔案上傳至 ASP.NET Core 的 Razor 頁面

作者：[Luke Latham](https://github.com/guardrex)

本主題為建置基礎[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample)在<xref:tutorials/razor-pages/razor-pages-start>。

本主題說明如何使用簡單的模型繫結檔案上, 傳適用於上傳小型檔案。 如需串流大型檔案的資訊，請參閱[使用串流上傳大型檔案](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)。

在下列步驟中，電影排程檔案上傳功能會新增至範例應用程式。 電影排程是由 `Schedule` 類別表示。 此類別包含兩個版本的排程。 `PublicSchedule` 為提供給客戶的版本， 而另一個 `PrivateSchedule` 版本則用於公司員工。 每個版本都會以個別的檔案上傳。 本教學課程示範如何使用單一 POST 將兩個檔案從頁面上傳到伺服器。

## <a name="security-considerations"></a>安全性考量

為使用者提供將檔案上傳到伺服器的能力時，請特別注意。 攻擊者可能會對系統發動[拒絕服務](/windows-hardware/drivers/ifs/denial-of-service)等攻擊。 以下安全性步驟可減少攻擊成功的可能性：

* 將檔案上傳至系統上的專用檔案上傳區域，可讓您更輕鬆地對上傳內容實施安全性措施。 允許檔案上傳時，請確保上傳位置的執行權限已停用。
* 使用應用程式認為安全的檔案名稱，而不使用使用者輸入或上傳檔的檔名。
* 僅允許特定一組核准的副檔名。
* 確認用戶端檢查在伺服器上執行。 用戶端檢查很容易規避。
* 檢查上傳項目的大小，並防止上傳超過預期的大小。
* 對上傳內容執行病毒/惡意程式碼掃描器。

> [!WARNING]
> 將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：
> * 完全侵占系統。
> * 使系統超載，導致系統全面失效。
> * 洩漏使用者或系統資料。
> * 在公用介面上塗鴉。

## <a name="add-a-fileupload-class"></a>新增 FileUpload 類別

您可以建立 Razor Page 來處理一對檔案上傳。 新增 `FileUpload` 類別，以繫結至頁面並取得排程資料。 以滑鼠右鍵按一下 *Models* 資料夾。 選取 [新增] > [類別]。 將類別命名為 **FileUpload** 並新增下列屬性：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

針對這兩個版本的排程，此類別具有每種版本的排程標題和屬性。 所有三個屬性都是必要項目，且標題長度必須為 3-60 個字元。

## <a name="add-a-helper-method-to-upload-files"></a>新增可上傳檔案的 Helper 方法

若要避免處理已上傳之排程檔案的程式碼有所重複，您可以新增靜態 Helper 方法。 在應用程式中，建立 *Utilities* 資料夾，並使用下列內容新增 *FileHelpers.cs* 檔案。 `ProcessFormFile` Helper 方法會採用 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary)，並傳回包含檔案大小及內容的字串。 系統會檢查內容類型和長度。 如果檔案未通過驗證檢查，`ModelState` 就會新增一項錯誤。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a>將檔案儲存至磁碟

範例應用程式會將上傳的檔案儲存到資料庫欄位。 若要將檔案儲存到磁碟，請使用 [FileStream](/dotnet/api/system.io.filestream)。 下列範例會將 `FileUpload.UploadPublicSchedule` 所保存的檔案複製到 `OnPostAsync` 方法的 `FileStream` 中。 `FileStream` 則會將檔案寫入磁碟所提供的 `<PATH-AND-FILE-NAME>`：

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

背景工作處理序必須具備 `filePath` 指定之位置的寫入權限。

> [!NOTE]
> `filePath`「必須」包含檔案名稱。 如果未提供檔案名稱，則會在執行階段擲回 [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception)。

> [!WARNING]
> 永遠不要將上傳的檔案保存在與應用程式相同的目錄樹狀結構中。
>
> 程式碼範例不提供防禦惡意檔案上傳的任何伺服器端保護。 如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：
>
> * [不受限制的檔案上傳](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a>將檔案儲存至 Azure Blob 儲存體

若要將檔案內容上傳至 Azure Blob 儲存體，請參閱[以 .NET 開始使用 Azure Blob 儲存體](/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 本主題會示範如何使用 [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) 將 [FileStream](/dotnet/api/system.io.filestream) 儲存至 Blob 儲存體。

## <a name="add-the-schedule-class"></a>新增 Schedule 類別

以滑鼠右鍵按一下 *Models* 資料夾。 選取 [新增] > [類別]。 將類別命名為 **Schedule** 並新增下列屬性：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

此類別會使用 `Display` 和 `DisplayFormat` 屬性，以在呈現排程資料時產生易記的標題和格式設定。

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a>更新 RazorPagesMovieContext

在 `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) 中為排程指定 `DbSet`：

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a>更新 MovieContext

在 `MovieContext` 中指定 `DbSet` (*Models/MovieContext.cs*) 以進行排程：

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a>將 Schedule 資料表新增至資料庫

開啟 [套件管理員主控台] (PMC)：[工具] > [NuGet 套件管理員] > [套件管理員主控台]。

![PMC 功能表](upload-files/_static/pmc.png)

在 PMC 中，執行下列命令。 這些命令可將 `Schedule` 資料表新增至資料庫：

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>新增檔案上傳 Razor Page

在 *Pages* 資料夾中，建立 *Schedules* 資料夾。 在 *Schedules* 資料夾中，建立名為 *Index.cshtml* 的頁面，以上傳排程與下列內容：

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

每個表單群組都包含 **\<標籤 >**，其會顯示每個類別屬性的名稱。 `FileUpload` 模型中的 `Display` 屬性提供標籤的顯示值。 例如，系統會將 `UploadPublicSchedule` 屬性的顯示名稱設為 `[Display(Name="Public Schedule")]`，因此當表單呈現時會顯示 "Public Schedule"。

每個表單群組包含驗證 **\<範圍>**。 如果使用者的輸入不符合 `FileUpload` 類別中所設的內容屬性，或若有任何 `ProcessFormFile` 方法的檔案驗證檢查失敗，則模型驗證會失敗。 模型驗證失敗時，系統會顯示對使用者很有幫助的驗證訊息。 例如，系統會將 `Title` 屬性附註 `[Required]` 和 `[StringLength(60, MinimumLength = 3)]`。 如果使用者未提供標題，則會收到訊息，指出該值為必要項目。 如果使用者輸入的值少於 3 個字元或超過 60 個字元，則會收到訊息，指出該值的長度有誤。 如果提供的檔案沒有任何內容，則會顯示訊息，指出檔案為空白。

## <a name="add-the-page-model"></a>新增頁面模型

將頁面模型 (*Index.cshtml.cs*) 新增至 *Schedules* 資料夾：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

頁面模型 (*Index.cshtml.cs* 中的 `IndexModel`) 會繫結 `FileUpload` 類別：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

模型也會使用排程的清單 (`IList<Schedule>`) 來顯示儲存在頁面資料庫中的排程：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

當頁面載入 `OnGetAsync` 時，會從資料庫填入 `Schedules`，並用其來產生已載入之排程的 HTML 資料表：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

當表單張貼至伺服器時，系統即會檢查 `ModelState`。 如果無效，就會重建 `Schedule`，且頁面會顯示一或多則驗證訊息，指出頁面驗證失敗的原因。 如果有效，即會在 *OnPostAsync* 中使用 `FileUpload` 屬性，以完成上傳這兩個排程版本的檔案，並建立新的 `Schedule` 物件來儲存資料。 接著，系統就會將排程儲存到資料庫中：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a>連結檔案上傳 Razor 頁面

開啟 *Pages/Shared/_Layout.cshtml*，然後將連結新增至導覽列以連至排程頁面：

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a>新增用來確認刪除排程的頁面

當使用者按一下 [刪除排程] 時，系統會提供取消作業的機會。 將刪除確認頁面 (*Delete.cshtml*) 新增至 *Schedules* 資料夾：

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

頁面模型 (*Delete.cshtml.cs*) 會將 `id` 所識別的單一排程載入要求的路由資料中。 將 *Delete.cshtml.cs* 檔案新增至 *Schedules* 資料夾：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

`OnPostAsync` 方法會依據排程的 `id` 來處理其刪除作業：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

成功刪除排程之後，`RedirectToPage` 會將使用者送回排程的 *Index.cshtml* 頁面。

## <a name="the-working-schedules-razor-page"></a>運作中的排程 Razor 頁面

載入頁面時，排程標題的標籤和輸入、公用排程和私用排程都會顯示提交按鈕：

![初始載入的排程 Razor 頁面，其中不含任何驗證錯誤和空白欄位](upload-files/_static/browser1.png)

如果任何欄位未填妥就選取 [上傳] 按鈕，即會違反模型上的 `[Required]` 屬性。 `ModelState` 無效。 使用者會看到驗證錯誤訊息：

![每個輸入控制項旁會出現驗證錯誤訊息](upload-files/_static/browser2.png)

在 [標題] 欄位中鍵入兩個字母。 驗證訊息會變更為指出標題必須介於 3-60 個字元：

![標題驗證訊息已變更](upload-files/_static/browser3.png)

當上傳一或多個排程時，[Loaded Schedules] (載入排程) 區段會顯示載入的排程：

![載入的排程表，其中顯示每個排程的標題、上傳日期 (UTC)、公用版本的檔案大小和私用版本的檔案大小](upload-files/_static/browser4.png)

當使用者按一下這裡的 [刪除] 連結時，可前往刪除確認檢視，以選擇確認或取消刪除作業。

## <a name="troubleshooting"></a>疑難排解

如需疑難排解資訊`IFormFile`上傳，請參閱[ASP.NET Core 的檔案上傳： 疑難排解](xref:mvc/models/file-uploads#troubleshooting)。
