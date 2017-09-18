---
title: "更新產生的頁面"
author: rick-anderson
description: "以更好的顯示方式更新產生的網頁。"
keywords: "ASP.NET Core, Razor 頁面"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 39b65f8af8304fabc6cf8d9a27992043f1e381a0
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="1f627-104">更新產生的頁面</span><span class="sxs-lookup"><span data-stu-id="1f627-104">Updating the generated pages</span></span>

<span data-ttu-id="1f627-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f627-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f627-106">我們開始使用電影應用程式的情況很不錯，但簡報卻不理想。</span><span class="sxs-lookup"><span data-stu-id="1f627-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="1f627-107">我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是 **Release Date** (兩個分開的字)。</span><span class="sxs-lookup"><span data-stu-id="1f627-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="1f627-109">更新產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="1f627-109">Update the generated code</span></span>

<span data-ttu-id="1f627-110">開啟 *Models/Movie.cs* 檔案，然後新增下列程式碼中顯示的醒目提示行：</span><span class="sxs-lookup"><span data-stu-id="1f627-110">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

<span data-ttu-id="1f627-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="1f627-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span></span>

<span data-ttu-id="1f627-112">以滑鼠右鍵按一下紅色曲線 > **[快速動作與重構]**。</span><span class="sxs-lookup"><span data-stu-id="1f627-112">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![操作功能表隨即顯示 **> [Quick Actions and Refactorings] (快速控制項目及重構)**。](da1/qa.png)


<span data-ttu-id="1f627-114">選取 `using System.ComponentModel.DataAnnotations;`。</span><span class="sxs-lookup"><span data-stu-id="1f627-114">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![使用清單頂端的 System.ComponentModel.DataAnnotations](da1/da.png)

  <span data-ttu-id="1f627-116">Visual Studio 即會新增 `using System.ComponentModel.DataAnnotations;`。</span><span class="sxs-lookup"><span data-stu-id="1f627-116">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="1f627-117">接下來的教學課程中將涵蓋 [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)。</span><span class="sxs-lookup"><span data-stu-id="1f627-117">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="1f627-118">[Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 屬性指定要顯示的欄位名稱 (在本例中為 "Release Date"，而不是 "ReleaseDate")。</span><span class="sxs-lookup"><span data-stu-id="1f627-118">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="1f627-119">[DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 屬性指定資料的型別 (Date)，因此不會顯示儲存在欄位中的時間資訊。</span><span class="sxs-lookup"><span data-stu-id="1f627-119">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="1f627-120">瀏覽至 Pages/Movies，然後將滑鼠停留在 **Edit** 連結，以查看目標 URL。</span><span class="sxs-lookup"><span data-stu-id="1f627-120">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![滑鼠停留在 Edit 連結並顯示 http://localhost:1234/Movies/Edit/5 的 Url 的瀏覽器視窗](da1/edit7.png)

<span data-ttu-id="1f627-122">在 *Pages/Movies/Index.cshtml* 檔案中，**Edit**、**Details ** 和 **Delete** 連結是由[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)所產生。</span><span class="sxs-lookup"><span data-stu-id="1f627-122">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) in the *Pages/Movies/Index.cshtml* file.</span></span>

<span data-ttu-id="1f627-123">[!code-cshtml[Main](razor-pages-start\snapshot_sample\RazorPagesMovie\Pages\Movie\Index.cshtml?highlight=16-18&range=32-)]</span><span class="sxs-lookup"><span data-stu-id="1f627-123">[!code-cshtml[Main](razor-pages-start\snapshot_sample\RazorPagesMovie\Pages\Movie\Index.cshtml?highlight=16-18&range=32-)]</span></span>

<span data-ttu-id="1f627-124">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="1f627-124">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1f627-125">在上述程式碼中，`AnchorTagHelper` 會從 Razor 頁面 (路由是相對路由)、`asp-page` 和路由識別碼 (`asp-route-id`) 動態產生 HTML `href` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="1f627-125">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="1f627-126">如需詳細資訊，請參閱[頁面的 URL 產生](xref:mvc/razor-pages/index#url-generation-for-pages)。</span><span class="sxs-lookup"><span data-stu-id="1f627-126">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="1f627-127">從您最愛的瀏覽器中使用 [檢視原始檔] 來檢查產生的標記。</span><span class="sxs-lookup"><span data-stu-id="1f627-127">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="1f627-128">產生的 HTML 部分如下所示：</span><span class="sxs-lookup"><span data-stu-id="1f627-128">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>

```

<span data-ttu-id="1f627-129">動態產生的連結會傳遞含有查詢字串的電影識別碼 (例如，`http://localhost:5000/Movies/Details?id=2`)。</span><span class="sxs-lookup"><span data-stu-id="1f627-129">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="1f627-130">更新 Edit、Details 和 Delete Razor 頁面，以使用 "{id:int}" 路由範本。</span><span class="sxs-lookup"><span data-stu-id="1f627-130">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="1f627-131">將這些頁面每一頁的頁面指示詞變更為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="1f627-131">Change the page directive for each of these pages to `@page "{id:int}"`.</span></span> <span data-ttu-id="1f627-132">執行應用程式，然後檢視原始檔。</span><span class="sxs-lookup"><span data-stu-id="1f627-132">Run the app and then view source.</span></span> <span data-ttu-id="1f627-133">產生的 HTML 將識別碼新增至 URL 的路徑部分：</span><span class="sxs-lookup"><span data-stu-id="1f627-133">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="1f627-134">對使用 "{id:int}" 路由範本的頁面提出的要求若**未**包含整數，將傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="1f627-134">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1f627-135">例如，`http://localhost:5000/Movies/Details` 會傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="1f627-135">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="1f627-136">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="1f627-136">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="1f627-137">更新並行例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="1f627-137">Update concurrency exception handling</span></span>

<span data-ttu-id="1f627-138">在 *Pages/Movies/Edit.cshtml.cs* 檔案中更新 `OnPostAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="1f627-138">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="1f627-139">下列醒目提示的程式碼示範這些變更：</span><span class="sxs-lookup"><span data-stu-id="1f627-139">The following highlighted code shows the changes:</span></span>

<span data-ttu-id="1f627-140">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet1&highlight=17-24)]</span><span class="sxs-lookup"><span data-stu-id="1f627-140">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet1&highlight=17-24)]</span></span>

<span data-ttu-id="1f627-141">當第一個並行用戶端刪除電影，而第二個並行用戶端發佈對電影的變更時，先前的程式碼只會偵測並行存取例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1f627-141">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="1f627-142">若要測試 `catch` 區段：</span><span class="sxs-lookup"><span data-stu-id="1f627-142">To test the `catch` block:</span></span>

* <span data-ttu-id="1f627-143">在 `catch (DbUpdateConcurrencyException)` 上設定中斷點</span><span class="sxs-lookup"><span data-stu-id="1f627-143">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="1f627-144">編輯電影。</span><span class="sxs-lookup"><span data-stu-id="1f627-144">Edit a movie.</span></span>
* <span data-ttu-id="1f627-145">在另一個瀏覽器視窗中，選取相同電影的 **Delete** 連結，然後刪除電影。</span><span class="sxs-lookup"><span data-stu-id="1f627-145">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="1f627-146">在先前的瀏覽器視窗中，發佈對電影的變更。</span><span class="sxs-lookup"><span data-stu-id="1f627-146">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="1f627-147">生產環境程式碼通常會在兩個或多個用戶端同時更新一筆記錄時，偵測並行存取衝突。</span><span class="sxs-lookup"><span data-stu-id="1f627-147">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="1f627-148">如需詳細資訊，請參閱[處理並行存取衝突](xref:data/ef-mvc/concurrency)。</span><span class="sxs-lookup"><span data-stu-id="1f627-148">See [Handling concurrency conflicts](xref:data/ef-mvc/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="1f627-149">發佈和繫結檢閱內容</span><span class="sxs-lookup"><span data-stu-id="1f627-149">Posting and binding review</span></span>

<span data-ttu-id="1f627-150">檢查 *Pages/Movies/Edit.cshtml.cs* 檔案：[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="1f627-150">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="1f627-151">對 Movies/Edit 頁面提出 HTTP GET 要求時 (例如，`http://localhost:5000/Movies/Edit/2`)：</span><span class="sxs-lookup"><span data-stu-id="1f627-151">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="1f627-152">`OnGetAsync` 方法會從資料庫擷取電影，並傳回 `Page` 方法。</span><span class="sxs-lookup"><span data-stu-id="1f627-152">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="1f627-153">`Page` 方法會轉譯 *Pages/Movies/Edit.cshtml* Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="1f627-153">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="1f627-154">*Pages/Movies/Edit.cshtml* 檔案包含模型指示詞 (`@model RazorPagesMovie.Pages.Movies.EditModel`)，這會讓電影模型可以在頁面上使用。</span><span class="sxs-lookup"><span data-stu-id="1f627-154">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the the movie model available on the page.</span></span>
* <span data-ttu-id="1f627-155">Edit 表單會顯示來自電影的值。</span><span class="sxs-lookup"><span data-stu-id="1f627-155">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="1f627-156">發佈 Movies/Edit 頁面時：</span><span class="sxs-lookup"><span data-stu-id="1f627-156">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="1f627-157">頁面上的表單值會繫結至 `Movie` 屬性。</span><span class="sxs-lookup"><span data-stu-id="1f627-157">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="1f627-158">`[BindProperty]` 屬性可讓[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="1f627-158">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

```csharp
[BindProperty]
public Movie Movie { get; set; }
```

* <span data-ttu-id="1f627-159">如果模型狀態中有錯誤 (例如，`ReleaseDate`無法轉換為 date)，則會以提交的值再次發佈表單。</span><span class="sxs-lookup"><span data-stu-id="1f627-159">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="1f627-160">如果沒有任何模型錯誤，則會儲存電影。</span><span class="sxs-lookup"><span data-stu-id="1f627-160">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="1f627-161">Index、Create 和 Delete Razor 頁面中的 HTTP GET 方法都會依循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="1f627-161">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="1f627-162">Create Razor 頁面中的 HTTP POST `OnPostAsync` 方法，會依循與 Edit Razor 頁面中的 `OnPostAsync` 方法類似的模式。</span><span class="sxs-lookup"><span data-stu-id="1f627-162">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="1f627-163">搜尋會在接下來的教學課程中新增。</span><span class="sxs-lookup"><span data-stu-id="1f627-163">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1f627-164">[上一步：使用 SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[新增搜尋](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="1f627-164">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
