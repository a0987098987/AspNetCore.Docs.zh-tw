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
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="41356-103">將檔案上傳至 ASP.NET Core 的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="41356-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="41356-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="41356-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="41356-105">本節會示範如何使用 Razor 頁面上傳檔案。</span><span class="sxs-lookup"><span data-stu-id="41356-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="41356-106">在本教學課程中，[Razor 頁面電影範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)會使用簡單的模型繫結上傳檔案；這種方法很適合用來上傳小型檔案。</span><span class="sxs-lookup"><span data-stu-id="41356-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="41356-107">如需串流大型檔案的資訊，請參閱[使用串流上傳大型檔案](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)。</span><span class="sxs-lookup"><span data-stu-id="41356-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="41356-108">在下列步驟中，您可以將電影排程檔案上傳功能新增至範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="41356-108">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="41356-109">電影排程是由 `Schedule` 類別表示。</span><span class="sxs-lookup"><span data-stu-id="41356-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="41356-110">此類別包含兩個版本的排程。</span><span class="sxs-lookup"><span data-stu-id="41356-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="41356-111">`PublicSchedule` 為提供給客戶的版本，</span><span class="sxs-lookup"><span data-stu-id="41356-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="41356-112">而另一個 `PrivateSchedule` 版本則用於公司員工。</span><span class="sxs-lookup"><span data-stu-id="41356-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="41356-113">每個版本都會以個別的檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="41356-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="41356-114">本教學課程示範如何使用單一 POST 將兩個檔案從頁面上傳到伺服器。</span><span class="sxs-lookup"><span data-stu-id="41356-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="41356-115">新增 FileUpload 類別</span><span class="sxs-lookup"><span data-stu-id="41356-115">Add a FileUpload class</span></span>

<span data-ttu-id="41356-116">在本節中，您將建立 Razor 頁面來處理一對檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="41356-116">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="41356-117">新增 `FileUpload` 類別，以繫結至頁面並取得排程資料。</span><span class="sxs-lookup"><span data-stu-id="41356-117">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="41356-118">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="41356-118">Right click the *Models* folder.</span></span> <span data-ttu-id="41356-119">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="41356-119">Select **Add** > **Class**.</span></span> <span data-ttu-id="41356-120">將類別命名為 **FileUpload** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="41356-120">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="41356-121">針對這兩個版本的排程，此類別具有每種版本的排程標題和屬性。</span><span class="sxs-lookup"><span data-stu-id="41356-121">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="41356-122">所有三個屬性都是必要項目，且標題長度必須為 3-60 個字元。</span><span class="sxs-lookup"><span data-stu-id="41356-122">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="41356-123">新增可上傳檔案的 Helper 方法</span><span class="sxs-lookup"><span data-stu-id="41356-123">Add a helper method to upload files</span></span>

<span data-ttu-id="41356-124">若要避免處理已上傳之排程檔案的程式碼有所重複，您可以新增靜態 Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="41356-124">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="41356-125">在應用程式中，建立 *Utilities* 資料夾，並使用下列內容新增 *FileHelpers.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="41356-125">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="41356-126">`ProcessFormFile` Helper 方法會採用 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary)，並傳回包含檔案大小及內容的字串。</span><span class="sxs-lookup"><span data-stu-id="41356-126">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="41356-127">系統會檢查內容類型和長度。</span><span class="sxs-lookup"><span data-stu-id="41356-127">The content type and length are checked.</span></span> <span data-ttu-id="41356-128">如果檔案未通過驗證檢查，`ModelState` 就會新增一項錯誤。</span><span class="sxs-lookup"><span data-stu-id="41356-128">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="41356-129">新增 Schedule 類別</span><span class="sxs-lookup"><span data-stu-id="41356-129">Add the Schedule class</span></span>

<span data-ttu-id="41356-130">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="41356-130">Right click the *Models* folder.</span></span> <span data-ttu-id="41356-131">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="41356-131">Select **Add** > **Class**.</span></span> <span data-ttu-id="41356-132">將類別命名為 **Schedule** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="41356-132">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="41356-133">此類別會使用 `Display` 和 `DisplayFormat` 屬性，以在呈現排程資料時產生易記的標題和格式設定。</span><span class="sxs-lookup"><span data-stu-id="41356-133">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="41356-134">更新 MovieContext</span><span class="sxs-lookup"><span data-stu-id="41356-134">Update the MovieContext</span></span>

<span data-ttu-id="41356-135">在 `MovieContext` 中指定 `DbSet` (*Models/MovieContext.cs*) 以進行排程：</span><span class="sxs-lookup"><span data-stu-id="41356-135">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="41356-136">將 Schedule 資料表新增至資料庫</span><span class="sxs-lookup"><span data-stu-id="41356-136">Add the Schedule table to the database</span></span>

<span data-ttu-id="41356-137">開啟 [套件管理員主控台] (PMC)：[工具] > [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="41356-137">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="41356-139">在 PMC 中，執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="41356-139">In the PMC, execute the following commands.</span></span> <span data-ttu-id="41356-140">這些命令可將 `Schedule` 資料表新增至資料庫：</span><span class="sxs-lookup"><span data-stu-id="41356-140">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="41356-141">新增檔案上傳 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="41356-141">Add a file upload Razor Page</span></span>

<span data-ttu-id="41356-142">在 *Pages* 資料夾中，建立 *Schedules* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="41356-142">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="41356-143">在 *Schedules* 資料夾中，建立名為 *Index.cshtml* 的頁面，以上傳排程與下列內容：</span><span class="sxs-lookup"><span data-stu-id="41356-143">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="41356-144">每個表單群組都包含 **\<標籤 >**，其會顯示每個類別屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="41356-144">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="41356-145">`FileUpload` 模型中的 `Display` 屬性提供標籤的顯示值。</span><span class="sxs-lookup"><span data-stu-id="41356-145">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="41356-146">例如，系統會將 `UploadPublicSchedule` 屬性的顯示名稱設為 `[Display(Name="Public Schedule")]`，因此當表單呈現時會顯示 "Public Schedule"。</span><span class="sxs-lookup"><span data-stu-id="41356-146">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="41356-147">每個表單群組包含驗證 **\<範圍>**。</span><span class="sxs-lookup"><span data-stu-id="41356-147">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="41356-148">如果使用者的輸入不符合 `FileUpload` 類別中所設的內容屬性，或若有任何 `ProcessFormFile` 方法的檔案驗證檢查失敗，則模型驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="41356-148">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="41356-149">模型驗證失敗時，系統會顯示對使用者很有幫助的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="41356-149">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="41356-150">例如，系統會將 `Title` 屬性附註 `[Required]` 和 `[StringLength(60, MinimumLength = 3)]`。</span><span class="sxs-lookup"><span data-stu-id="41356-150">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="41356-151">如果使用者未提供標題，則會收到訊息，指出該值為必要項目。</span><span class="sxs-lookup"><span data-stu-id="41356-151">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="41356-152">如果使用者輸入的值少於 3 個字元或超過 60 個字元，則會收到訊息，指出該值的長度有誤。</span><span class="sxs-lookup"><span data-stu-id="41356-152">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="41356-153">如果提供的檔案沒有任何內容，則會顯示訊息，指出檔案為空白。</span><span class="sxs-lookup"><span data-stu-id="41356-153">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="41356-154">新增頁面模型</span><span class="sxs-lookup"><span data-stu-id="41356-154">Add the page model</span></span>

<span data-ttu-id="41356-155">將頁面模型 (*Index.cshtml.cs*) 新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="41356-155">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="41356-156">頁面模型 (*Index.cshtml.cs* 中的 `IndexModel`) 會繫結 `FileUpload` 類別：</span><span class="sxs-lookup"><span data-stu-id="41356-156">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="41356-157">模型也會使用排程的清單 (`IList<Schedule>`) 來顯示儲存在頁面資料庫中的排程：</span><span class="sxs-lookup"><span data-stu-id="41356-157">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="41356-158">當頁面載入 `OnGetAsync` 時，會從資料庫填入 `Schedules`，並用其來產生已載入之排程的 HTML 資料表：</span><span class="sxs-lookup"><span data-stu-id="41356-158">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="41356-159">當表單張貼至伺服器時，系統即會檢查 `ModelState`。</span><span class="sxs-lookup"><span data-stu-id="41356-159">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="41356-160">如果無效，就會重建 `Schedule`，且頁面會顯示一或多則驗證訊息，指出頁面驗證失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="41356-160">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="41356-161">如果有效，即會在 *OnPostAsync* 中使用 `FileUpload` 屬性，以完成上傳這兩個排程版本的檔案，並建立新的 `Schedule` 物件來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="41356-161">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="41356-162">接著，系統就會將排程儲存到資料庫中：</span><span class="sxs-lookup"><span data-stu-id="41356-162">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="41356-163">連結檔案上傳 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="41356-163">Link the file upload Razor Page</span></span>

<span data-ttu-id="41356-164">開啟 *_Layout.cshtml*，然後將連結新增至導覽列以存取檔案上傳頁面：</span><span class="sxs-lookup"><span data-stu-id="41356-164">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="41356-165">新增用來確認刪除排程的頁面</span><span class="sxs-lookup"><span data-stu-id="41356-165">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="41356-166">當使用者按一下刪除排程時，您會希望他們有機會取消作業。</span><span class="sxs-lookup"><span data-stu-id="41356-166">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="41356-167">將刪除確認頁面 (*Delete.cshtml*) 新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="41356-167">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="41356-168">頁面模型 (*Delete.cshtml.cs*) 會將 `id` 所識別的單一排程載入要求的路由資料中。</span><span class="sxs-lookup"><span data-stu-id="41356-168">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="41356-169">將 *Delete.cshtml.cs* 檔案新增至 *Schedules* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="41356-169">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="41356-170">`OnPostAsync` 方法會依據排程的 `id` 來處理其刪除作業：</span><span class="sxs-lookup"><span data-stu-id="41356-170">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="41356-171">成功刪除排程之後，`RedirectToPage` 會將使用者送回排程的 *Index.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="41356-171">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="41356-172">運作中的排程 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="41356-172">The working Schedules Razor Page</span></span>

<span data-ttu-id="41356-173">載入頁面時，排程標題的標籤和輸入、公用排程和私用排程都會顯示提交按鈕：</span><span class="sxs-lookup"><span data-stu-id="41356-173">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![初始載入的排程 Razor 頁面，其中不含任何驗證錯誤和空白欄位](uploading-files/_static/browser1.png)

<span data-ttu-id="41356-175">如果任何欄位未填妥就選取 [上傳] 按鈕，即會違反模型上的 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="41356-175">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="41356-176">`ModelState` 無效。</span><span class="sxs-lookup"><span data-stu-id="41356-176">The `ModelState` is invalid.</span></span> <span data-ttu-id="41356-177">使用者會看到驗證錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="41356-177">The validation error messages are displayed to the user:</span></span>

![每個輸入控制項旁會出現驗證錯誤訊息](uploading-files/_static/browser2.png)

<span data-ttu-id="41356-179">在 [標題] 欄位中鍵入兩個字母。</span><span class="sxs-lookup"><span data-stu-id="41356-179">Type two letters into the **Title** field.</span></span> <span data-ttu-id="41356-180">驗證訊息會變更為指出標題必須介於 3-60 個字元：</span><span class="sxs-lookup"><span data-stu-id="41356-180">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![標題驗證訊息已變更](uploading-files/_static/browser3.png)

<span data-ttu-id="41356-182">當上傳一或多個排程時，[Loaded Schedules] (載入排程) 區段會顯示載入的排程：</span><span class="sxs-lookup"><span data-stu-id="41356-182">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![載入的排程表，其中顯示每個排程的標題、上傳日期 (UTC)、公用版本的檔案大小和私用版本的檔案大小](uploading-files/_static/browser4.png)

<span data-ttu-id="41356-184">當使用者按一下這裡的 [刪除] 連結時，可前往刪除確認檢視，以選擇確認或取消刪除作業。</span><span class="sxs-lookup"><span data-stu-id="41356-184">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="41356-185">疑難排解</span><span class="sxs-lookup"><span data-stu-id="41356-185">Troubleshooting</span></span>

<span data-ttu-id="41356-186">如需針對 `IFormFile` 上傳進行疑難排解的資訊，請參閱 [ASP.NET Core 的檔案上傳：疑難排解](xref:mvc/models/file-uploads#troubleshooting)。</span><span class="sxs-lookup"><span data-stu-id="41356-186">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="41356-187">感謝您看完這份 Razor 頁面簡介。</span><span class="sxs-lookup"><span data-stu-id="41356-187">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="41356-188">歡迎您提供任何指教。</span><span class="sxs-lookup"><span data-stu-id="41356-188">We appreciate any comments you leave.</span></span> <span data-ttu-id="41356-189">完成本教學課程之後，非常建議您繼續參閱 [MVC 和 EF Core 使用者入門](xref:data/ef-mvc/intro)。</span><span class="sxs-lookup"><span data-stu-id="41356-189">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41356-190">其他資源</span><span class="sxs-lookup"><span data-stu-id="41356-190">Additional resources</span></span>

* [<span data-ttu-id="41356-191">ASP.NET Core 的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="41356-191">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="41356-192">IFormFile</span><span class="sxs-lookup"><span data-stu-id="41356-192">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="41356-193">上一步：驗證</span><span class="sxs-lookup"><span data-stu-id="41356-193">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
