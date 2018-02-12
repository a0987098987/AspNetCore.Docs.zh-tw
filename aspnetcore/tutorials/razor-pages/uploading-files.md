---
title: "將檔案上傳至 ASP.NET Core 的 Razor 頁面"
author: guardrex
description: "了解如何將檔案上傳至 Razor 頁面。"
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 24eaa0dd9293cc932c51d280300308e835a0840e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>將檔案上傳至 ASP.NET Core 的 Razor 頁面

作者：[Luke Latham](https://github.com/guardrex)

本節會示範如何使用 Razor 頁面上傳檔案。

在本教學課程中，[Razor 頁面電影範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)會使用簡單的模型繫結上傳檔案；這種方法很適合用來上傳小型檔案。 如需串流大型檔案的資訊，請參閱[使用串流上傳大型檔案](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)。

在下列步驟中，您可以將電影排程檔案上傳功能新增至範例應用程式。 電影排程是由 `Schedule` 類別表示。 此類別包含兩個版本的排程。 `PublicSchedule` 為提供給客戶的版本， 而另一個 `PrivateSchedule` 版本則用於公司員工。 每個版本都會以個別的檔案上傳。 本教學課程示範如何使用單一 POST 將兩個檔案從頁面上傳到伺服器。

## <a name="add-a-fileupload-class"></a>新增 FileUpload 類別

在本節中，您將建立 Razor 頁面來處理一對檔案上傳。 新增 `FileUpload` 類別，以繫結至頁面並取得排程資料。 以滑鼠右鍵按一下 *Models* 資料夾。 選取 [新增] > [類別]。 將類別命名為 **FileUpload** 並新增下列屬性：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

針對這兩個版本的排程，此類別具有每種版本的排程標題和屬性。 所有三個屬性都是必要項目，且標題長度必須為 3-60 個字元。

## <a name="add-a-helper-method-to-upload-files"></a>新增可上傳檔案的 Helper 方法

若要避免處理已上傳之排程檔案的程式碼有所重複，您可以新增靜態 Helper 方法。 在應用程式中，建立 *Utilities* 資料夾，並使用下列內容新增 *FileHelpers.cs* 檔案。 `ProcessFormFile` Helper 方法會採用 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary)，並傳回包含檔案大小及內容的字串。 系統會檢查內容類型和長度。 如果檔案未通過驗證檢查，`ModelState` 就會新增一項錯誤。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a>新增 Schedule 類別

以滑鼠右鍵按一下 *Models* 資料夾。 選取 [新增] > [類別]。 將類別命名為 **Schedule** 並新增下列屬性：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

此類別會使用 `Display` 和 `DisplayFormat` 屬性，以在呈現排程資料時產生易記的標題和格式設定。

## <a name="update-the-moviecontext"></a>更新 MovieContext

在 `MovieContext` 中指定 `DbSet` (*Models/MovieContext.cs*) 以進行排程：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>將 Schedule 資料表新增至資料庫

開啟 [套件管理員主控台] (PMC)：[工具] > [NuGet 套件管理員] > [套件管理員主控台]。

![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，執行下列命令。 這些命令可將 `Schedule` 資料表新增至資料庫：

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>新增檔案上傳 Razor 頁面

在 *Pages* 資料夾中，建立 *Schedules* 資料夾。 在 *Schedules* 資料夾中，建立名為 *Index.cshtml* 的頁面，以上傳排程與下列內容：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

每個表單群組都包含 **\<標籤 >**，其會顯示每個類別屬性的名稱。 `FileUpload` 模型中的 `Display` 屬性提供標籤的顯示值。 例如，系統會將 `UploadPublicSchedule` 屬性的顯示名稱設為 `[Display(Name="Public Schedule")]`，因此當表單呈現時會顯示 "Public Schedule"。

每個表單群組包含驗證 **\<範圍>**。 如果使用者的輸入不符合 `FileUpload` 類別中所設的內容屬性，或若有任何 `ProcessFormFile` 方法的檔案驗證檢查失敗，則模型驗證會失敗。 模型驗證失敗時，系統會顯示對使用者很有幫助的驗證訊息。 例如，系統會將 `Title` 屬性附註 `[Required]` 和 `[StringLength(60, MinimumLength = 3)]`。 如果使用者未提供標題，則會收到訊息，指出該值為必要項目。 如果使用者輸入的值少於 3 個字元或超過 60 個字元，則會收到訊息，指出該值的長度有誤。 如果提供的檔案沒有任何內容，則會顯示訊息，指出檔案為空白。

## <a name="add-the-page-model"></a>新增頁面模型

將頁面模型 (*Index.cshtml.cs*) 新增至 *Schedules* 資料夾：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

頁面模型 (*Index.cshtml.cs* 中的 `IndexModel`) 會繫結 `FileUpload` 類別：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

模型也會使用排程的清單 (`IList<Schedule>`) 來顯示儲存在頁面資料庫中的排程：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

當頁面載入 `OnGetAsync` 時，會從資料庫填入 `Schedules`，並用其來產生已載入之排程的 HTML 資料表：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

當表單張貼至伺服器時，系統即會檢查 `ModelState`。 如果無效，就會重建 `Schedule`，且頁面會顯示一或多則驗證訊息，指出頁面驗證失敗的原因。 如果有效，即會在 *OnPostAsync* 中使用 `FileUpload` 屬性，以完成上傳這兩個排程版本的檔案，並建立新的 `Schedule` 物件來儲存資料。 接著，系統就會將排程儲存到資料庫中：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>連結檔案上傳 Razor 頁面

開啟 *_Layout.cshtml*，然後將連結新增至導覽列以存取檔案上傳頁面：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>新增用來確認刪除排程的頁面

當使用者按一下刪除排程時，您會希望他們有機會取消作業。 將刪除確認頁面 (*Delete.cshtml*) 新增至 *Schedules* 資料夾：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

頁面模型 (*Delete.cshtml.cs*) 會將 `id` 所識別的單一排程載入要求的路由資料中。 將 *Delete.cshtml.cs* 檔案新增至 *Schedules* 資料夾：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

`OnPostAsync` 方法會依據排程的 `id` 來處理其刪除作業：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

成功刪除排程之後，`RedirectToPage` 會將使用者送回排程的 *Index.cshtml* 頁面。

## <a name="the-working-schedules-razor-page"></a>運作中的排程 Razor 頁面

載入頁面時，排程標題的標籤和輸入、公用排程和私用排程都會顯示提交按鈕：

![初始載入的排程 Razor 頁面，其中不含任何驗證錯誤和空白欄位](uploading-files/_static/browser1.png)

如果任何欄位未填妥就選取 [上傳] 按鈕，即會違反模型上的 `[Required]` 屬性。 `ModelState` 無效。 使用者會看到驗證錯誤訊息：

![每個輸入控制項旁會出現驗證錯誤訊息](uploading-files/_static/browser2.png)

在 [標題] 欄位中鍵入兩個字母。 驗證訊息會變更為指出標題必須介於 3-60 個字元：

![標題驗證訊息已變更](uploading-files/_static/browser3.png)

當上傳一或多個排程時，[Loaded Schedules] (載入排程) 區段會顯示載入的排程：

![載入的排程表，其中顯示每個排程的標題、上傳日期 (UTC)、公用版本的檔案大小和私用版本的檔案大小](uploading-files/_static/browser4.png)

當使用者按一下這裡的 [刪除] 連結時，可前往刪除確認檢視，以選擇確認或取消刪除作業。

## <a name="troubleshooting"></a>疑難排解

如需針對 `IFormFile` 上傳進行疑難排解的資訊，請參閱 [ASP.NET Core 的檔案上傳：疑難排解](xref:mvc/models/file-uploads#troubleshooting)。

感謝您看完這份 Razor 頁面簡介。 歡迎您提供任何指教。 完成本教學課程之後，非常建議您繼續參閱 [MVC 和 EF Core 使用者入門](xref:data/ef-mvc/intro)。

## <a name="additional-resources"></a>其他資源

* [ASP.NET Core 的檔案上傳](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[上一步：驗證](xref:tutorials/razor-pages/validation)
