---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 簡介 ASP.NET Web Pages-刪除資料庫的資料 |Microsoft Docs
author: tfitzmac
description: 本教學課程會示範如何刪除個別的資料庫項目。 它會假設您已完成透過 ASP.NET Web 的 pa 中的 更新資料庫資料數列...
ms.author: aspnetcontent
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 234b5f99c5d5f580316204c88ea1ab8c1269d452
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815352"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="16cce-104">ASP.NET Web Pages 簡介-刪除資料庫資料</span><span class="sxs-lookup"><span data-stu-id="16cce-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="16cce-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="16cce-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="16cce-106">本教學課程會示範如何刪除個別的資料庫項目。</span><span class="sxs-lookup"><span data-stu-id="16cce-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="16cce-107">它假設您已完成透過數列[更新 ASP.NET Web Pages 中的資料庫資料](updating-data.md)。</span><span class="sxs-lookup"><span data-stu-id="16cce-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="16cce-108">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="16cce-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="16cce-109">如何從記錄清單中選取個別的記錄。</span><span class="sxs-lookup"><span data-stu-id="16cce-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="16cce-110">如何刪除資料庫中的單一記錄。</span><span class="sxs-lookup"><span data-stu-id="16cce-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="16cce-111">如何檢查特定按鈕已按下表單中。</span><span class="sxs-lookup"><span data-stu-id="16cce-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="16cce-112">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="16cce-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="16cce-113">`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="16cce-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="16cce-114">SQL`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="16cce-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="16cce-115">`Database.Execute`方法來執行 SQL`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="16cce-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="16cce-116">您將建置</span><span class="sxs-lookup"><span data-stu-id="16cce-116">What You'll Build</span></span>

<span data-ttu-id="16cce-117">在上一個教學課程中，您已了解如何更新現有的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="16cce-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="16cce-118">本教學課程是類似，只不過是而不是更新記錄，您將會刪除它。</span><span class="sxs-lookup"><span data-stu-id="16cce-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="16cce-119">處理程序也大致相同，不同之處在於刪除是更簡單，因此本教學課程中會很短。</span><span class="sxs-lookup"><span data-stu-id="16cce-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="16cce-120">在*電影*頁面上，您將更新`WebGrid`協助程式，因此它會顯示**刪除**隨附的每個電影旁邊的連結**編輯**您先前加入的連結。</span><span class="sxs-lookup"><span data-stu-id="16cce-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![顯示每一部電影的刪除連結的影片頁面](deleting-data/_static/image1.png)

<span data-ttu-id="16cce-122">與編輯，當您按一下 **刪除**連結，它會帶您到不同的頁面中，電影資訊已經在表單中：</span><span class="sxs-lookup"><span data-stu-id="16cce-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![刪除顯示對影片的影片頁面](deleting-data/_static/image2.png)

<span data-ttu-id="16cce-124">您可以按一下按鈕以永久刪除的記錄。</span><span class="sxs-lookup"><span data-stu-id="16cce-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="16cce-125">將刪除連結新增至電影清單</span><span class="sxs-lookup"><span data-stu-id="16cce-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="16cce-126">您會開始藉由新增**刪除**連結至`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="16cce-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="16cce-127">這個連結是類似**編輯**您在上一個教學課程中加入的連結。</span><span class="sxs-lookup"><span data-stu-id="16cce-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="16cce-128">開啟*Movies.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="16cce-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="16cce-129">變更`WebGrid`標記中加入資料行頁面的主體。</span><span class="sxs-lookup"><span data-stu-id="16cce-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="16cce-130">以下是修改過的標記：</span><span class="sxs-lookup"><span data-stu-id="16cce-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="16cce-131">新的資料行是如下：</span><span class="sxs-lookup"><span data-stu-id="16cce-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="16cce-132">方格中的設定的方式**編輯**資料行是在方格中最左邊，**刪除**資料行是最右邊。</span><span class="sxs-lookup"><span data-stu-id="16cce-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="16cce-133">(沒有逗號之後`Year`資料行現在，如果您沒有注意。)沒有特別關於這些連結資料行在哪裡，以及您可以輕鬆將它們放彼此相鄰。</span><span class="sxs-lookup"><span data-stu-id="16cce-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="16cce-134">在此情況下，它們是個別使它們更難以取得混。</span><span class="sxs-lookup"><span data-stu-id="16cce-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![標示為顯示並未彼此相鄰的 movies 頁面，使用 [編輯] 和 [詳細資料] 連結](deleting-data/_static/image3.png)

<span data-ttu-id="16cce-136">新的資料行顯示的連結 (`<a>`項目) 的文字會顯示 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="16cce-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="16cce-137">連結的目標 (及其`href`屬性) 是最終會解析為類似此 URL 中，使用的程式碼`id`每一部電影的不同值：</span><span class="sxs-lookup"><span data-stu-id="16cce-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="16cce-138">此連結會叫用名為的頁面*DeleteMovie*並將它傳遞您所選取的電影識別碼。</span><span class="sxs-lookup"><span data-stu-id="16cce-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="16cce-139">本教學課程不會探討如何建構此連結，因為它幾乎等同於**編輯**從上一個教學課程的連結 ([更新 ASP.NET Web Pages 中的資料庫資料](updating-data.md))。</span><span class="sxs-lookup"><span data-stu-id="16cce-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="16cce-140">建立 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="16cce-140">Creating the Delete Page</span></span>

<span data-ttu-id="16cce-141">現在您可以建立將會針對目標頁面**刪除**方格中的連結。</span><span class="sxs-lookup"><span data-stu-id="16cce-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="16cce-142">**重要**先選取 刪除的記錄，然後確認此程序中使用不同的網頁和按鈕的技術是非常重要的安全性。</span><span class="sxs-lookup"><span data-stu-id="16cce-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="16cce-143">當您已閱讀先前教學課程中，進行*任何*變更至您的網站應該*一律*完成使用表單&mdash;也就使用 HTTP POST 作業。</span><span class="sxs-lookup"><span data-stu-id="16cce-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="16cce-144">如果您所做變更站台，只要按一下的連結 （亦即，使用 「 取得 」 作業），人無法對您的網站進行簡單的要求，並刪除您的資料。</span><span class="sxs-lookup"><span data-stu-id="16cce-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="16cce-145">即使您的網站在索引為搜尋引擎編目程式可能只是透過下列連結在無意間刪除資料。</span><span class="sxs-lookup"><span data-stu-id="16cce-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="16cce-146">當您的應用程式可讓您變更記錄的人員時，您必須向使用者顯示的記錄，仍要編輯的。</span><span class="sxs-lookup"><span data-stu-id="16cce-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="16cce-147">不過，您可能想要跳過此步驟，刪除記錄。</span><span class="sxs-lookup"><span data-stu-id="16cce-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="16cce-148">請勿略過該步驟，不過。</span><span class="sxs-lookup"><span data-stu-id="16cce-148">Don't skip that step, though.</span></span> <span data-ttu-id="16cce-149">（它也是讓使用者看到的記錄，並確認它們要刪除的記錄，它們能幫助。）</span><span class="sxs-lookup"><span data-stu-id="16cce-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="16cce-150">在後續的教學課程組中，您會看到如何將登入功能，因此使用者必須登入，再刪除記錄。</span><span class="sxs-lookup"><span data-stu-id="16cce-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="16cce-151">建立名為的頁面*DeleteMovie.cshtml*和取代功能的檔案，以下列標記：</span><span class="sxs-lookup"><span data-stu-id="16cce-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="16cce-152">此標記就像是*EditMovie*頁面，除了，而不是使用文字方塊中 (`<input type="text">`)，標記包括`<span>`項目。</span><span class="sxs-lookup"><span data-stu-id="16cce-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="16cce-153">這裡沒有編輯。</span><span class="sxs-lookup"><span data-stu-id="16cce-153">There's nothing here to edit.</span></span> <span data-ttu-id="16cce-154">您必須執行的只是顯示電影詳細資料，以便使用者可以確定他們要刪除正確的影片。</span><span class="sxs-lookup"><span data-stu-id="16cce-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="16cce-155">標記已包含可讓使用者返回電影清單頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="16cce-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="16cce-156">依照*EditMovie*  頁面上，選取的影片識別碼儲存在隱藏欄位。</span><span class="sxs-lookup"><span data-stu-id="16cce-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="16cce-157">（它會傳遞至頁面一開始作為查詢字串值。）沒有`Html.ValidationSummary`會顯示驗證錯誤的呼叫。</span><span class="sxs-lookup"><span data-stu-id="16cce-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="16cce-158">在此情況下，錯誤可能是任何影片識別碼已傳遞至網頁或影片識別碼無效。</span><span class="sxs-lookup"><span data-stu-id="16cce-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="16cce-159">如果某個人必須先選取影片，以在執行此頁面，可能會發生這種情況下*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="16cce-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="16cce-160">是按鈕標題**刪除電影**，且其名稱屬性設為`buttonDelete`。</span><span class="sxs-lookup"><span data-stu-id="16cce-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="16cce-161">`name`屬性會在程式碼中用來識別按鈕送出表單。</span><span class="sxs-lookup"><span data-stu-id="16cce-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="16cce-162">您必須撰寫 1） 讀取電影詳細資料，當第一次顯示網頁的程式碼，以及 2） 實際刪除電影，當使用者按一下按鈕。</span><span class="sxs-lookup"><span data-stu-id="16cce-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="16cce-163">新增程式碼來讀取單一影片</span><span class="sxs-lookup"><span data-stu-id="16cce-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="16cce-164">在頂端*DeleteMovie.cshtml*頁面上，新增下列程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="16cce-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="16cce-165">此標記是與對應的程式碼，在相同*EditMovie*頁面。</span><span class="sxs-lookup"><span data-stu-id="16cce-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="16cce-166">它會取得從查詢字串的電影識別碼，並會使用從資料庫讀取記錄的識別碼。</span><span class="sxs-lookup"><span data-stu-id="16cce-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="16cce-167">程式碼包含驗證測試 (`IsInt()`和`row != null`) 以確定傳遞至頁面的電影識別碼無效。</span><span class="sxs-lookup"><span data-stu-id="16cce-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="16cce-168">請記住此程式碼應該只執行此頁面會執行第一次。</span><span class="sxs-lookup"><span data-stu-id="16cce-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="16cce-169">您不打算重新讀取資料庫中的電影記錄，當使用者按一下**刪除電影** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="16cce-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="16cce-170">因此，程式碼來讀取內部測試指出電影`if(!IsPost)`&mdash;亦即*要求不是 post 作業 （表單提交）*。</span><span class="sxs-lookup"><span data-stu-id="16cce-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="16cce-171">加入程式碼以刪除選取的影片</span><span class="sxs-lookup"><span data-stu-id="16cce-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="16cce-172">使用者按一下按鈕時，請刪除電影，新增下列程式碼的右括號之內`@`區塊：</span><span class="sxs-lookup"><span data-stu-id="16cce-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="16cce-173">此程式碼是類似的程式碼更新現有的記錄，但更簡單。</span><span class="sxs-lookup"><span data-stu-id="16cce-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="16cce-174">程式碼基本上會執行 SQL`Delete`陳述式。</span><span class="sxs-lookup"><span data-stu-id="16cce-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="16cce-175">依照*EditMovie*頁面上，程式碼是在`if(IsPost)`區塊。</span><span class="sxs-lookup"><span data-stu-id="16cce-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="16cce-176">此時，`if()`條件是稍微複雜一點：</span><span class="sxs-lookup"><span data-stu-id="16cce-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="16cce-177">有以下兩種情況。</span><span class="sxs-lookup"><span data-stu-id="16cce-177">There are two conditions here.</span></span> <span data-ttu-id="16cce-178">第一個是，正在提交頁面，如您所見之前&mdash; `if(IsPost)`。</span><span class="sxs-lookup"><span data-stu-id="16cce-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="16cce-179">第二個條件是`!Request["buttonDelete"].IsEmpty()`，這表示該要求具有名為物件`buttonDelete`。</span><span class="sxs-lookup"><span data-stu-id="16cce-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="16cce-180">無可否認地，這是間接的方式，測試哪一個按鈕送出表單。</span><span class="sxs-lookup"><span data-stu-id="16cce-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="16cce-181">如果表單包含多個提交按鈕，只有按下按鈕的名稱會出現在要求中。</span><span class="sxs-lookup"><span data-stu-id="16cce-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="16cce-182">因此，邏輯上來說，如果特定按鈕的名稱會出現在要求&mdash;中所述的程式碼中，如果該按鈕不是空的或&mdash;是提交表單的按鈕。</span><span class="sxs-lookup"><span data-stu-id="16cce-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="16cce-183">`&&`運算子表示 「 和 」 (邏輯 AND)。</span><span class="sxs-lookup"><span data-stu-id="16cce-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="16cce-184">因此整個`if`條件是...</span><span class="sxs-lookup"><span data-stu-id="16cce-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="16cce-185">*此要求是某篇文章 （而不是第一次要求）*</span><span class="sxs-lookup"><span data-stu-id="16cce-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="16cce-186">AND</span><span class="sxs-lookup"><span data-stu-id="16cce-186">AND</span></span>  
  
<span data-ttu-id="16cce-187"> `buttonDelete`*按鈕已提交表單的按鈕。*</span><span class="sxs-lookup"><span data-stu-id="16cce-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="16cce-188">（事實上，此頁面） 此表單包含只有一個按鈕，以便其他測試`buttonDelete`技術上來說並不需要。</span><span class="sxs-lookup"><span data-stu-id="16cce-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="16cce-189">儘管如此，您要執行的作業，將會永久移除資料。</span><span class="sxs-lookup"><span data-stu-id="16cce-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="16cce-190">因此，您想要為確保越好，您要執行此作業，使用者已明確要求它時，才。</span><span class="sxs-lookup"><span data-stu-id="16cce-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="16cce-191">例如，假設您於稍後展開此頁面，並加入其他按鈕。</span><span class="sxs-lookup"><span data-stu-id="16cce-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="16cce-192">即使如此，刪除影片的程式碼才會執行`buttonDelete`所按的按鈕。</span><span class="sxs-lookup"><span data-stu-id="16cce-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="16cce-193">依照*EditMovie* ] 頁面上，您取得的識別碼，從隱藏的欄位，並接著執行 [SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="16cce-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="16cce-194">語法`Delete`陳述式：</span><span class="sxs-lookup"><span data-stu-id="16cce-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="16cce-195">請務必包含`WHERE`子句和識別碼。</span><span class="sxs-lookup"><span data-stu-id="16cce-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="16cce-196">如果您省略 WHERE 子句中，*資料表中的所有記錄將會都刪除*。</span><span class="sxs-lookup"><span data-stu-id="16cce-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="16cce-197">如您所見，您傳遞的識別碼值給 SQL 命令所使用的預留位置。</span><span class="sxs-lookup"><span data-stu-id="16cce-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="16cce-198">測試影片的刪除程序</span><span class="sxs-lookup"><span data-stu-id="16cce-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="16cce-199">現在您可以測試。</span><span class="sxs-lookup"><span data-stu-id="16cce-199">Now you can test.</span></span> <span data-ttu-id="16cce-200">執行*電影*頁面，然後按一下**刪除**電影旁邊。</span><span class="sxs-lookup"><span data-stu-id="16cce-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="16cce-201">當*DeleteMovie*  頁面出現時，按一下**刪除電影**。</span><span class="sxs-lookup"><span data-stu-id="16cce-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![反白顯示的刪除電影按鈕刪除影片頁面](deleting-data/_static/image4.png)

<span data-ttu-id="16cce-203">當您按一下按鈕時，程式碼會刪除電影，並傳回的電影清單。</span><span class="sxs-lookup"><span data-stu-id="16cce-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="16cce-204">您可以在那里搜尋已刪除的影片，並確認它已被刪除。</span><span class="sxs-lookup"><span data-stu-id="16cce-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="16cce-205">接下來的下一步</span><span class="sxs-lookup"><span data-stu-id="16cce-205">Coming Up Next</span></span>

<span data-ttu-id="16cce-206">下一個教學課程會示範如何在您的網站上提供的所有頁面，常見的外觀和版面配置。</span><span class="sxs-lookup"><span data-stu-id="16cce-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="16cce-207">（更新與刪除連結） 的電影頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="16cce-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="16cce-208">DeleteMovie 頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="16cce-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="16cce-209">其他資源</span><span class="sxs-lookup"><span data-stu-id="16cce-209">Additional Resources</span></span>

- [<span data-ttu-id="16cce-210">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="16cce-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="16cce-211">[SQL DELETE 陳述式](http://www.w3schools.com/sql/sql_delete.asp)W3Schools 站台上</span><span class="sxs-lookup"><span data-stu-id="16cce-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16cce-212">[上一頁](updating-data.md)
> [下一頁](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="16cce-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
