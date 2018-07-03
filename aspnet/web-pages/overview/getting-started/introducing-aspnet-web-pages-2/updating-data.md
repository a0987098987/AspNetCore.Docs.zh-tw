---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: 簡介 ASP.NET Web Pages-更新資料庫的資料 |Microsoft Docs
author: tfitzmac
description: 本教學課程會示範如何更新 （變更） 現有的資料庫項目，當您使用 ASP.NET Web Pages (Razor)。 它假設您已完成序列個...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 259034a795b9dff7001165a182bc0e84bb690491
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377047"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a><span data-ttu-id="03f67-104">ASP.NET Web Pages 簡介-更新資料庫資料</span><span class="sxs-lookup"><span data-stu-id="03f67-104">Introducing ASP.NET Web Pages - Updating Database Data</span></span>
====================
<span data-ttu-id="03f67-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="03f67-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="03f67-106">本教學課程會示範如何更新 （變更） 現有的資料庫項目，當您使用 ASP.NET Web Pages (Razor)。</span><span class="sxs-lookup"><span data-stu-id="03f67-106">This tutorial shows you how to update (change) an existing database entry when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="03f67-107">它假設您已完成透過數列[輸入資料所使用的表單使用 ASP.NET Web Pages](entering-data.md)。</span><span class="sxs-lookup"><span data-stu-id="03f67-107">It assumes you have completed the series through [Entering Data by Using Forms Using ASP.NET Web Pages](entering-data.md).</span></span>
> 
> <span data-ttu-id="03f67-108">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="03f67-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="03f67-109">如何選取中的個別記錄`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="03f67-109">How to select an individual record in the `WebGrid` helper.</span></span>
> - <span data-ttu-id="03f67-110">如何從資料庫讀取單一記錄。</span><span class="sxs-lookup"><span data-stu-id="03f67-110">How to read a single record from a database.</span></span>
> - <span data-ttu-id="03f67-111">如何預先載入表單，以從資料庫記錄的值。</span><span class="sxs-lookup"><span data-stu-id="03f67-111">How to preload a form with values from the database record.</span></span>
> - <span data-ttu-id="03f67-112">如何更新資料庫中的現有記錄。</span><span class="sxs-lookup"><span data-stu-id="03f67-112">How to update an existing record in a database.</span></span>
> - <span data-ttu-id="03f67-113">如何將資訊儲存在頁面中，而不會顯示它。</span><span class="sxs-lookup"><span data-stu-id="03f67-113">How to store information in the page without displaying it.</span></span>
> - <span data-ttu-id="03f67-114">如何使用隱藏的欄位來儲存資訊。</span><span class="sxs-lookup"><span data-stu-id="03f67-114">How to use a hidden field to store information.</span></span>
>   
> 
> <span data-ttu-id="03f67-115">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="03f67-115">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="03f67-116">`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="03f67-116">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="03f67-117">SQL`Update`命令。</span><span class="sxs-lookup"><span data-stu-id="03f67-117">The SQL `Update` command.</span></span>
> - <span data-ttu-id="03f67-118">`Database.Execute` 方法</span><span class="sxs-lookup"><span data-stu-id="03f67-118">The `Database.Execute` method.</span></span>
> - <span data-ttu-id="03f67-119">隱藏的欄位 (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="03f67-119">Hidden fields (`<input type="hidden">`).</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="03f67-120">您將建置</span><span class="sxs-lookup"><span data-stu-id="03f67-120">What You'll Build</span></span>

<span data-ttu-id="03f67-121">在上一個教學課程中，您已了解如何將記錄新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="03f67-121">In the previous tutorial, you learned how to add a record to a database.</span></span> <span data-ttu-id="03f67-122">在這裡，您將了解如何顯示供編輯的記錄。</span><span class="sxs-lookup"><span data-stu-id="03f67-122">Here, you'll learn how to display a record for editing.</span></span> <span data-ttu-id="03f67-123">在 *電影*頁面上，您將更新`WebGrid`協助程式，因此它會顯示**編輯**每部電影旁邊的連結：</span><span class="sxs-lookup"><span data-stu-id="03f67-123">In the *Movies* page, you'll update the `WebGrid` helper so that it displays an **Edit** link next to each movie:</span></span>

![WebGrid 顯示包括每一部電影的 [編輯] 連結](updating-data/_static/image1.png)

<span data-ttu-id="03f67-125">當您按一下 **編輯**連結，它會帶您到不同的頁面中，電影資訊已經在表單中：</span><span class="sxs-lookup"><span data-stu-id="03f67-125">When you click the **Edit** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![編輯顯示要編輯影片的影片頁面](updating-data/_static/image2.png)

<span data-ttu-id="03f67-127">您可以變更任何值。</span><span class="sxs-lookup"><span data-stu-id="03f67-127">You can change any of the values.</span></span> <span data-ttu-id="03f67-128">當您提交的變更時，頁面中的程式碼會更新資料庫，並帶您回到的電影清單。</span><span class="sxs-lookup"><span data-stu-id="03f67-128">When you submit the changes, the code in the page updates the database and takes you back to the movie listing.</span></span>

<span data-ttu-id="03f67-129">這部分的程序的方式幾乎完全相同*AddMovie.cshtml*您建立在上一個教學課程中，因此本教學課程中的大部分都很熟悉的頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-129">This part of the process works almost exactly like the *AddMovie.cshtml* page you created in the previous tutorial, so much of this tutorial will be familiar.</span></span>

<span data-ttu-id="03f67-130">有數種方式，您可以實作編輯個別的電影的方法。</span><span class="sxs-lookup"><span data-stu-id="03f67-130">There are several ways you could implement a way to edit an individual movie.</span></span> <span data-ttu-id="03f67-131">已選擇所顯示的方式，因為很容易實作且容易理解。</span><span class="sxs-lookup"><span data-stu-id="03f67-131">The approach shown was chosen because it's easy to implement and easy to understand.</span></span>

## <a name="adding-an-edit-link-to-the-movie-listing"></a><span data-ttu-id="03f67-132">加入的電影清單的編輯連結</span><span class="sxs-lookup"><span data-stu-id="03f67-132">Adding an Edit Link to the Movie Listing</span></span>

<span data-ttu-id="03f67-133">若要開始，您將更新*電影*頁面上，使每個電影清單也會一併包含**編輯**連結。</span><span class="sxs-lookup"><span data-stu-id="03f67-133">To begin, you'll update the *Movies* page so that each movie listing also contains an **Edit** link.</span></span>

<span data-ttu-id="03f67-134">開啟*Movies.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="03f67-134">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="03f67-135">在頁面的主體中，變更`WebGrid`標記加入資料行。</span><span class="sxs-lookup"><span data-stu-id="03f67-135">In the body of the page, change the `WebGrid` markup by adding a column.</span></span> <span data-ttu-id="03f67-136">以下是修改過的標記：</span><span class="sxs-lookup"><span data-stu-id="03f67-136">Here's the modified markup:</span></span>

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

<span data-ttu-id="03f67-137">新的資料行是如下：</span><span class="sxs-lookup"><span data-stu-id="03f67-137">The new column is this one:</span></span>

[!code-html[Main](updating-data/samples/sample2.html)]

<span data-ttu-id="03f67-138">本專欄的重點是要示範的連結 (`<a>`項目) 的文字會顯示 [編輯]。</span><span class="sxs-lookup"><span data-stu-id="03f67-138">The point of this column is to show a link (`<a>` element) whose text says "Edit".</span></span> <span data-ttu-id="03f67-139">我們之後建立時執行網頁時，看起來類似下列的連結是與`id`每一部電影的不同值：</span><span class="sxs-lookup"><span data-stu-id="03f67-139">What we're after is to create a link that looks like the following when the page runs, with the `id` value different for each movie:</span></span>

[!code-css[Main](updating-data/samples/sample3.css)]

<span data-ttu-id="03f67-140">此連結會叫用名為的頁面*EditMovie*，它會將傳遞查詢字串和`?id=7`至該頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-140">This link will invoke a page named *EditMovie*, and it will pass the query string `?id=7` to that page.</span></span>

<span data-ttu-id="03f67-141">新的資料行的語法看起來可能有點複雜，但這只是因為它會將它們集結數個項目。</span><span class="sxs-lookup"><span data-stu-id="03f67-141">The syntax for the new column might look a bit complex, but that's only because it puts together several elements.</span></span> <span data-ttu-id="03f67-142">每個個別的項目很簡單。</span><span class="sxs-lookup"><span data-stu-id="03f67-142">Each individual element is straightforward.</span></span> <span data-ttu-id="03f67-143">如果您專注於只`<a>`項目，您會看到此標記：</span><span class="sxs-lookup"><span data-stu-id="03f67-143">If you concentrate on just the `<a>` element, you see this markup:</span></span>

[!code-html[Main](updating-data/samples/sample4.html)]

<span data-ttu-id="03f67-144">方格的運作方式的一些背景： 方格會顯示資料列，其中每個資料庫記錄，並在資料庫記錄中顯示每個欄位的資料行。</span><span class="sxs-lookup"><span data-stu-id="03f67-144">Some background about how the grid works: the grid displays rows, one for each database record, and it displays columns for each field in the database record.</span></span> <span data-ttu-id="03f67-145">建構每個格線資料列時，雖然`item`物件包含該資料列的資料庫記錄 （項目）。</span><span class="sxs-lookup"><span data-stu-id="03f67-145">While each grid row is being constructed, the `item` object contains the database record (item) for that row.</span></span> <span data-ttu-id="03f67-146">這種安排可讓您在程式碼，以取得該資料列的資料。</span><span class="sxs-lookup"><span data-stu-id="03f67-146">This arrangement gives you a way in code to get at the data for that row.</span></span> <span data-ttu-id="03f67-147">這是這裡所示： 運算式`item.ID`會取得目前的資料庫項目識別碼值。</span><span class="sxs-lookup"><span data-stu-id="03f67-147">That's what you see here: the expression `item.ID` is getting the ID value of the current database item.</span></span> <span data-ttu-id="03f67-148">若要取得的任何資料庫值 （標題、 內容類型或年） 相同的方式，使用`item.Title`， `item.Genre`，或`item.Year`。</span><span class="sxs-lookup"><span data-stu-id="03f67-148">You could get any of the database values (title, genre, or year) the same way by using `item.Title`, `item.Genre`, or `item.Year`.</span></span>

<span data-ttu-id="03f67-149">運算式`"~/EditMovie?id=@item.ID`結合的目標 URL 的硬式編碼部分 (`~/EditMovie?id=`) 使用此動態衍生的識別碼。</span><span class="sxs-lookup"><span data-stu-id="03f67-149">The expression `"~/EditMovie?id=@item.ID` combines the hard-coded part of the target URL (`~/EditMovie?id=`) with this dynamically derived ID.</span></span> <span data-ttu-id="03f67-150">(您所見`~`運算子在上一個教學課程中，這是一個 ASP.NET 運算子，表示目前的網站根目錄。)</span><span class="sxs-lookup"><span data-stu-id="03f67-150">(You saw the `~` operator in the previous tutorial; it's an ASP.NET operator that represents the current website root.)</span></span>

<span data-ttu-id="03f67-151">結果是標記的，這一部分的資料行中只會產生類似下列的標記在執行階段：</span><span class="sxs-lookup"><span data-stu-id="03f67-151">The result is that this part of the markup in the column simply produces something like the following markup at run time:</span></span>

[!code-xml[Main](updating-data/samples/sample5.xml)]

<span data-ttu-id="03f67-152">當然，實際值的`id`會不同，每個資料列。</span><span class="sxs-lookup"><span data-stu-id="03f67-152">Naturally, the actual value of `id` will be different for each row.</span></span>

## <a name="creating-a-custom-display-for-a-grid-column"></a><span data-ttu-id="03f67-153">建立自訂的顯示方格資料行</span><span class="sxs-lookup"><span data-stu-id="03f67-153">Creating a Custom Display for a Grid Column</span></span>

<span data-ttu-id="03f67-154">現在我們回到方格資料行。</span><span class="sxs-lookup"><span data-stu-id="03f67-154">Now back to the grid column.</span></span> <span data-ttu-id="03f67-155">三個資料行最初所顯示的格線唯一資料值 （title、 genre 和年）。</span><span class="sxs-lookup"><span data-stu-id="03f67-155">The three columns you originally had in the grid displayed only data values (title, genre, and year).</span></span> <span data-ttu-id="03f67-156">您可以指定此顯示的資料庫資料行名稱傳遞&mdash;比方說， `grid.Column("Title")`。</span><span class="sxs-lookup"><span data-stu-id="03f67-156">You specified this display by passing the name of the database column &mdash; for example, `grid.Column("Title")`.</span></span>

<span data-ttu-id="03f67-157">這個新**編輯**連結資料行是不同。</span><span class="sxs-lookup"><span data-stu-id="03f67-157">This new **Edit** link column is different.</span></span> <span data-ttu-id="03f67-158">而不指定的資料行名稱，您只要傳遞`format`參數。</span><span class="sxs-lookup"><span data-stu-id="03f67-158">Instead of specifying a column name, you're passing a `format` parameter.</span></span> <span data-ttu-id="03f67-159">此參數可讓您定義標記所`WebGrid`helper 會呈現連同`item`粗體或綠色顯示資料行的資料，或在預定的格式，您想要的值。</span><span class="sxs-lookup"><span data-stu-id="03f67-159">This parameter lets you define markup that the `WebGrid` helper will render along with the `item` value to display the column data as bold or green or in whatever format that you want.</span></span> <span data-ttu-id="03f67-160">比方說，如果您想要顯示為粗體標題時，您可以建立的資料行，如這個範例所示：</span><span class="sxs-lookup"><span data-stu-id="03f67-160">For example, if you wanted the title to appear bold, you could create a column like this example:</span></span>

[!code-html[Main](updating-data/samples/sample6.html)]

<span data-ttu-id="03f67-161">(各種`@`中所見的字元`format`屬性標記的標記和程式碼的值之間的轉換。)</span><span class="sxs-lookup"><span data-stu-id="03f67-161">(The various `@` characters you see in the `format` property mark the transition between markup and a code value.)</span></span>

<span data-ttu-id="03f67-162">一旦您已了解`format`屬性，會比較容易了解如何新增**編輯**連結資料行放在一起：</span><span class="sxs-lookup"><span data-stu-id="03f67-162">Once you know about the `format` property, it's easier to understand how the new **Edit** link column is put together:</span></span>

[!code-html[Main](updating-data/samples/sample7.html)]

<span data-ttu-id="03f67-163">資料行包含*只*的轉譯連結標記，再加上一些資訊 (ID)，從擷取的資料列的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="03f67-163">The column consists *only* of the markup that renders the link, plus some information (the ID) that's extracted from the database record for the row.</span></span>

> [!TIP]
> 
> <span data-ttu-id="03f67-164">**具名的參數和方法的位置參數**</span><span class="sxs-lookup"><span data-stu-id="03f67-164">**Named Parameters and Positional Parameters for a Method**</span></span>
> 
> <span data-ttu-id="03f67-165">許多次時已呼叫方法並傳遞參數給它，您已只被列以逗號分隔的參數值。</span><span class="sxs-lookup"><span data-stu-id="03f67-165">Many times when you've called a method and passed parameters to it, you've simply listed the parameter values separated by commas.</span></span> <span data-ttu-id="03f67-166">以下提供幾個範例：</span><span class="sxs-lookup"><span data-stu-id="03f67-166">Here are a couple of examples:</span></span>
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> <span data-ttu-id="03f67-167">當您第一次看到此程式碼，但在每個案例中，要將參數傳遞給特定的順序中的方法時，我們沒有提到問題&mdash;也就是定義在該方法之參數的順序。</span><span class="sxs-lookup"><span data-stu-id="03f67-167">We didn't mention the issue when you first saw this code, but in each case, you're passing parameters to the methods in a specific order &mdash; namely, the order in which the parameters are defined in that method.</span></span> <span data-ttu-id="03f67-168">針對`db.Execute`和`Validation.RequireFields`，如果您混您傳遞值的順序時，您會得到錯誤訊息時執行網頁時或至少一些奇怪的結果。</span><span class="sxs-lookup"><span data-stu-id="03f67-168">For `db.Execute` and `Validation.RequireFields`, if you mixed up the order of the values you pass, you'd get an error message when the page runs, or at least some strange results.</span></span> <span data-ttu-id="03f67-169">很明顯地，您必須知道要傳遞的參數順序。</span><span class="sxs-lookup"><span data-stu-id="03f67-169">Clearly, you have to know the order to pass the parameters in.</span></span> <span data-ttu-id="03f67-170">（在 WebMatrix 中，IntelliSense 可以幫助您了解釐清名稱、 類型和參數的順序）。</span><span class="sxs-lookup"><span data-stu-id="03f67-170">(In WebMatrix, IntelliSense can help you learn figure out the name, type, and order of the parameters.)</span></span>
> 
> <span data-ttu-id="03f67-171">為將值傳遞順序的替代方案，您可以使用*具名參數*。</span><span class="sxs-lookup"><span data-stu-id="03f67-171">As an alternative to passing values in order, you can use *named parameters*.</span></span> <span data-ttu-id="03f67-172">(為了傳遞參數就是使用*位置參數*。)用於具名參數，您明確地包含參數的名稱傳遞它的值時。</span><span class="sxs-lookup"><span data-stu-id="03f67-172">(Passing parameters in order is known as using *positional parameters*.) For named parameters, you explicitly include the name of the parameter when passing its value.</span></span> <span data-ttu-id="03f67-173">您已使用具名的參數已經數次，在這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="03f67-173">You've used named parameters already a number of times in these tutorials.</span></span> <span data-ttu-id="03f67-174">例如: </span><span class="sxs-lookup"><span data-stu-id="03f67-174">For example:</span></span>
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> <span data-ttu-id="03f67-175">和</span><span class="sxs-lookup"><span data-stu-id="03f67-175">and</span></span>
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> <span data-ttu-id="03f67-176">尤其是當方法會接受許多參數，是實用的幾個情況下，具名的參數。</span><span class="sxs-lookup"><span data-stu-id="03f67-176">Named parameters are handy for a couple of situations, especially when a method takes many parameters.</span></span> <span data-ttu-id="03f67-177">其中一個時，您想要將一個或兩個參數傳遞，但您想要傳遞的值不是參數清單中的第一個位置。</span><span class="sxs-lookup"><span data-stu-id="03f67-177">One is when you want to pass only one or two parameters, but the values you want to pass are not among the first positions in the parameter list.</span></span> <span data-ttu-id="03f67-178">另一種情況時，您想要對您最有意義的順序傳遞的參數，讓您的程式碼更容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="03f67-178">Another situation is when you want to make your code more readable by passing the parameters in the order that makes the most sense to you.</span></span>
> 
> <span data-ttu-id="03f67-179">很明顯地，若要使用具名的參數，您必須知道參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="03f67-179">Obviously, to use named parameters, you have to know the names of the parameters.</span></span> <span data-ttu-id="03f67-180">WebMatrix IntelliSense 可以*顯示*您的名稱，但它無法目前他們為您填入。</span><span class="sxs-lookup"><span data-stu-id="03f67-180">WebMatrix IntelliSense can *show* you the names, but it cannot currently fill them in for you.</span></span>


## <a name="creating-the-edit-page"></a><span data-ttu-id="03f67-181">建立 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="03f67-181">Creating the Edit Page</span></span>

<span data-ttu-id="03f67-182">現在您可以建立*EditMovie*頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-182">Now you can create the *EditMovie* page.</span></span> <span data-ttu-id="03f67-183">當使用者按一下**編輯**連結，他們就可以得到此頁面上。</span><span class="sxs-lookup"><span data-stu-id="03f67-183">When users click the **Edit** link, they'll end up on this page.</span></span>

<span data-ttu-id="03f67-184">建立名為的頁面*EditMovie.cshtml*和取代功能的檔案，以下列標記：</span><span class="sxs-lookup"><span data-stu-id="03f67-184">Create a page named *EditMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

<span data-ttu-id="03f67-185">此標記和程式碼是類似於您在*AddMovie*頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-185">This markup and code is similar to what you have in the *AddMovie* page.</span></span> <span data-ttu-id="03f67-186">沒有有些許不同的 [提交] 按鈕的文字。</span><span class="sxs-lookup"><span data-stu-id="03f67-186">There's a small difference in the text for the submit button.</span></span> <span data-ttu-id="03f67-187">如同*AddMovie*頁面上，有`Html.ValidationSummary`如果有的話，會顯示驗證錯誤的呼叫。</span><span class="sxs-lookup"><span data-stu-id="03f67-187">As with the *AddMovie* page, there's an `Html.ValidationSummary` call that will display validation errors if there are any.</span></span> <span data-ttu-id="03f67-188">此時，我們會留下任何呼叫`Validation.Message`，因為錯誤會顯示在驗證摘要。</span><span class="sxs-lookup"><span data-stu-id="03f67-188">This time we're leaving out calls to `Validation.Message`, since errors will be displayed in the validation summary.</span></span> <span data-ttu-id="03f67-189">如先前的教學課程中所述，您可以使用各種組合中的驗證摘要和個別的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="03f67-189">As noted in the previous tutorial, you can use the validation summary and the individual error messages in various combinations.</span></span>

<span data-ttu-id="03f67-190">請注意再次`method`的屬性`<form>`元素設定為`post`。</span><span class="sxs-lookup"><span data-stu-id="03f67-190">Notice again that the `method` attribute of the `<form>` element is set to `post`.</span></span> <span data-ttu-id="03f67-191">如同*AddMovie.cshtml*  頁面上，此頁面可變更資料庫。</span><span class="sxs-lookup"><span data-stu-id="03f67-191">As with the *AddMovie.cshtml* page, this page makes changes to the database.</span></span> <span data-ttu-id="03f67-192">因此，此表單應該執行`POST`作業。</span><span class="sxs-lookup"><span data-stu-id="03f67-192">Therefore, this form should perform a `POST` operation.</span></span> <span data-ttu-id="03f67-193">(如需詳細資訊之間的差異`GET`並`POST`作業，請參閱[GET、 POST 和 HTTP 動詞命令安全性](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety)HTML 表單的教學課程中的資訊看板。)</span><span class="sxs-lookup"><span data-stu-id="03f67-193">(For more about the difference between `GET` and `POST` operations, see the [GET, POST, and HTTP Verb Safety](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) sidebar in the tutorial on HTML forms.)</span></span>

<span data-ttu-id="03f67-194">如您在先前的教學課程中所見`value`文字方塊中的屬性 Razor 程式碼以它們預先載入設定。</span><span class="sxs-lookup"><span data-stu-id="03f67-194">As you saw in an earlier tutorial, the `value` attributes of the text boxes are being set with Razor code in order to preload them.</span></span> <span data-ttu-id="03f67-195">此時，不過，您使用變數，例如`title`並`genre`而不是該工作`Request.Form["title"]`:</span><span class="sxs-lookup"><span data-stu-id="03f67-195">This time, though, you're using variables like `title` and `genre` for that task instead of `Request.Form["title"]`:</span></span>

`<input type="text" name="title" value="@title" />`

<span data-ttu-id="03f67-196">為之前，此標記會預先載入的電影值的文字方塊值。</span><span class="sxs-lookup"><span data-stu-id="03f67-196">As before, this markup will preload the text box values with the movie values.</span></span> <span data-ttu-id="03f67-197">您會看到它為什麼很方便地這次使用變數，而不是使用稍後`Request`物件。</span><span class="sxs-lookup"><span data-stu-id="03f67-197">You'll see in a moment why it's handy to use variables this time instead of using the `Request` object.</span></span>

<span data-ttu-id="03f67-198">另外還有`<input type="hidden">`此頁面上的項目。</span><span class="sxs-lookup"><span data-stu-id="03f67-198">There's also a `<input type="hidden">` element on this page.</span></span> <span data-ttu-id="03f67-199">這個項目而不讓它顯示在頁面上儲存的電影識別碼。</span><span class="sxs-lookup"><span data-stu-id="03f67-199">This element stores the movie ID without making it visible on the page.</span></span> <span data-ttu-id="03f67-200">識別碼一開始傳遞至頁面所使用的查詢字串值 (`?id=7`或類似的 URL 中)。</span><span class="sxs-lookup"><span data-stu-id="03f67-200">The ID is initially passed to the page by using a query string value (`?id=7` or similar in the URL).</span></span> <span data-ttu-id="03f67-201">藉由將的識別碼值放入隱藏的欄位中，您可以確保它可供當提交表單時，即使您不再需要存取的頁面以叫用的原始 URL。</span><span class="sxs-lookup"><span data-stu-id="03f67-201">By putting the ID value into a hidden field, you can make sure that it's available when the form is submitted, even if you no longer have access to the original URL that the page was invoked with.</span></span>

<span data-ttu-id="03f67-202">不同於*AddMovie*頁面上的程式碼*EditMovie*頁面有兩個不同的功能。</span><span class="sxs-lookup"><span data-stu-id="03f67-202">Unlike the *AddMovie* page, the code for the *EditMovie* page has two distinct functions.</span></span> <span data-ttu-id="03f67-203">第一個函式是第一次顯示網頁時 (和*只*然後)，此程式碼從查詢字串取得的電影識別碼。</span><span class="sxs-lookup"><span data-stu-id="03f67-203">The first function is that when the page is displayed for the first time (and *only* then), the code gets the movie ID from the query string.</span></span> <span data-ttu-id="03f67-204">接著，程式碼會使用識別碼來讀取對應影片移出資料庫和顯示 （預先載入） 它的文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="03f67-204">The code then uses the ID to read the corresponding movie out of the database and display (preload) it in the text boxes.</span></span>

<span data-ttu-id="03f67-205">第二個函式是，當使用者按下**送出變更** 按鈕，程式碼會讀取這些值的文字方塊中，並驗證它們。</span><span class="sxs-lookup"><span data-stu-id="03f67-205">The second function is that when the user clicks the **Submit Changes** button, the code has to read the values of the text boxes and validate them.</span></span> <span data-ttu-id="03f67-206">程式碼也會對新的值來更新資料庫項目。</span><span class="sxs-lookup"><span data-stu-id="03f67-206">The code also has to update the database item with the new values.</span></span> <span data-ttu-id="03f67-207">如您所見的就像是加入一筆記錄，這項技術*AddMovie*。</span><span class="sxs-lookup"><span data-stu-id="03f67-207">This technique is similar to adding a record, as you saw in *AddMovie*.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="03f67-208">新增程式碼來讀取單一影片</span><span class="sxs-lookup"><span data-stu-id="03f67-208">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="03f67-209">若要執行第一個函式，請將此程式碼加入頁面頂端：</span><span class="sxs-lookup"><span data-stu-id="03f67-209">To perform the first function, add this code to the top of the page:</span></span>

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

<span data-ttu-id="03f67-210">大部分的這段程式碼會啟動在區塊內`if(!IsPost)`。</span><span class="sxs-lookup"><span data-stu-id="03f67-210">Most of this code is inside a block that starts `if(!IsPost)`.</span></span> <span data-ttu-id="03f67-211">`!`運算子代表"not，"所以運算式表示*如果此要求不是 post 提交*，這是間接的一種說法*此要求是否已執行此頁面第一次*。</span><span class="sxs-lookup"><span data-stu-id="03f67-211">The `!` operator means "not," so the expression means *if this request is not a post submission*, which is an indirect way of saying *if this request is the first time that this page has been run*.</span></span> <span data-ttu-id="03f67-212">如先前所述，應該執行此程式碼*只*此頁面會執行第一次。</span><span class="sxs-lookup"><span data-stu-id="03f67-212">As noted earlier, this code should run *only* the first time the page runs.</span></span> <span data-ttu-id="03f67-213">如果您沒有使用中的程式碼`if(!IsPost)`，它會每次頁面叫用時，是否在第一次執行，或按一下 [回應] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="03f67-213">If you didn't enclose the code in `if(!IsPost)`, it would run every time the page is invoked, whether the first time or in response to a button click.</span></span>

<span data-ttu-id="03f67-214">請注意，程式碼包含`else`封鎖此時間。</span><span class="sxs-lookup"><span data-stu-id="03f67-214">Notice that the code includes an `else` block this time.</span></span> <span data-ttu-id="03f67-215">我們說過當我們引進了`if`區塊，有時您想要執行替代程式碼，如果您要測試的條件不是，則為 true。</span><span class="sxs-lookup"><span data-stu-id="03f67-215">As we said when we introduced `if` blocks, sometimes you want to run alternative code if the condition you're testing isn't true.</span></span> <span data-ttu-id="03f67-216">這種情況如下。</span><span class="sxs-lookup"><span data-stu-id="03f67-216">That's the case here.</span></span> <span data-ttu-id="03f67-217">如果符合條件 （亦即，如果傳遞至頁面的識別碼為 [確定]），您會從資料庫讀取資料列。</span><span class="sxs-lookup"><span data-stu-id="03f67-217">If the condition passes (that is, if the ID passed to the page is ok), you read a row from the database.</span></span> <span data-ttu-id="03f67-218">不過，如果條件未通過，`else`區塊會執行，並將程式碼設定錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="03f67-218">However, if the condition doesn't pass, the `else` block runs and the code sets an error message.</span></span>

## <a name="validating-a-value-passed-to-the-page"></a><span data-ttu-id="03f67-219">驗證傳遞至頁面的值</span><span class="sxs-lookup"><span data-stu-id="03f67-219">Validating a Value Passed to the Page</span></span>

<span data-ttu-id="03f67-220">此程式碼使用`Request.QueryString["id"]`取得傳遞至頁面的識別碼。</span><span class="sxs-lookup"><span data-stu-id="03f67-220">The code uses `Request.QueryString["id"]` to get the ID that's passed to the page.</span></span> <span data-ttu-id="03f67-221">程式碼可確保值實際上傳遞做為識別碼。</span><span class="sxs-lookup"><span data-stu-id="03f67-221">The code makes sure that a value was actually passed for the ID.</span></span> <span data-ttu-id="03f67-222">如果不傳遞任何值，此程式碼會設定驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="03f67-222">If no value was passed, the code sets a validation error.</span></span>

<span data-ttu-id="03f67-223">此程式碼會示範不同的方法來驗證資訊。</span><span class="sxs-lookup"><span data-stu-id="03f67-223">This code shows a different way to validate information.</span></span> <span data-ttu-id="03f67-224">在上一個教學課程中，您用過`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="03f67-224">In the previous tutorial, you worked with the `Validation` helper.</span></span> <span data-ttu-id="03f67-225">註冊要驗證，欄位和 ASP.NET 自動未驗證，並使用顯示錯誤`Html.ValidationMessage`和`Html.ValidationSummary`。</span><span class="sxs-lookup"><span data-stu-id="03f67-225">You registered fields to validate, and ASP.NET automatically did the validation and displayed errors by using `Html.ValidationMessage` and `Html.ValidationSummary`.</span></span> <span data-ttu-id="03f67-226">在此情況下，不過，您不是真正驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="03f67-226">In this case, however, you're not really validating user input.</span></span> <span data-ttu-id="03f67-227">相反地，您要驗證已傳遞至頁面上，從其他位置的值。</span><span class="sxs-lookup"><span data-stu-id="03f67-227">Instead, you're validating a value that was passed to the page from elsewhere.</span></span> <span data-ttu-id="03f67-228">`Validation`協助程式為您不會進行。</span><span class="sxs-lookup"><span data-stu-id="03f67-228">The `Validation` helper doesn't do that for you.</span></span>

<span data-ttu-id="03f67-229">因此，您檢查的值，藉由測試它與`if(!Request.QueryString["ID"].IsEmpty()`)。</span><span class="sxs-lookup"><span data-stu-id="03f67-229">Therefore, you check the value yourself, by testing it with `if(!Request.QueryString["ID"].IsEmpty()`).</span></span> <span data-ttu-id="03f67-230">如果沒有問題，您可以使用來顯示錯誤`Html.ValidationSummary`，當您使用`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="03f67-230">If there's a problem, you can display the error by using `Html.ValidationSummary`, as you did with the `Validation` helper.</span></span> <span data-ttu-id="03f67-231">若要這樣做，請呼叫`Validation.AddFormError`並將它傳遞要顯示的訊息。</span><span class="sxs-lookup"><span data-stu-id="03f67-231">To do that, you call `Validation.AddFormError` and pass it a message to display.</span></span> <span data-ttu-id="03f67-232">`Validation.AddFormError` 是一種內建方法，可讓您定義自訂的訊息，您已經熟悉的驗證系統相連結。</span><span class="sxs-lookup"><span data-stu-id="03f67-232">`Validation.AddFormError` is a built-in method that lets you define custom messages that tie in with the validation system you're already familiar with.</span></span> <span data-ttu-id="03f67-233">（稍後在本教學課程將討論如何進行此驗證程序更健全。）</span><span class="sxs-lookup"><span data-stu-id="03f67-233">(Later in this tutorial we'll talk about how to make this validation process a little more robust.)</span></span>

<span data-ttu-id="03f67-234">確定您影片的識別碼之後, 程式碼會讀取資料庫中，尋找只有單一資料庫的項目。</span><span class="sxs-lookup"><span data-stu-id="03f67-234">After making sure that there's an ID for the movie, the code reads the database, looking for only a single database item.</span></span> <span data-ttu-id="03f67-235">(您可能已經注意到一般資料庫作業模式： 開啟資料庫、 定義 SQL 陳述式，並執行陳述式。)此時，SQL`Select`陳述式包含`WHERE ID = @0`。</span><span class="sxs-lookup"><span data-stu-id="03f67-235">(You probably have noticed the general pattern for database operations: open the database, define a SQL statement, and run the statement.) This time, the SQL `Select` statement includes `WHERE ID = @0`.</span></span> <span data-ttu-id="03f67-236">因為識別碼是唯一的可傳回只有一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="03f67-236">Because the ID is unique, only one record can be returned.</span></span>

<span data-ttu-id="03f67-237">查詢透過執行`db.QuerySingle`(不`db.Query`，因為您使用的電影清單)，並將程式碼將放入結果`row`變數。</span><span class="sxs-lookup"><span data-stu-id="03f67-237">The query is performed by using `db.QuerySingle` (not `db.Query`, as you used for the movie listing), and the code puts the result into the `row` variable.</span></span> <span data-ttu-id="03f67-238">名稱`row`是任意的; 您可以隨意命名的變數。</span><span class="sxs-lookup"><span data-stu-id="03f67-238">The name `row` is arbitrary; you can name the variables anything you like.</span></span> <span data-ttu-id="03f67-239">頂端初始化的變數則會填入電影詳細資料，這些值可以顯示的文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="03f67-239">The variables initialized at the top are then filled with the movie details so that these values can be displayed in the text boxes.</span></span>

## <a name="testing-the-edit-page-so-far"></a><span data-ttu-id="03f67-240">測試 [編輯] 頁面 （目前）</span><span class="sxs-lookup"><span data-stu-id="03f67-240">Testing the Edit Page (So Far)</span></span>

<span data-ttu-id="03f67-241">如果您想要測試您的頁面，執行*電影*現在頁面，然後按一下**編輯**旁邊任何影片的連結。</span><span class="sxs-lookup"><span data-stu-id="03f67-241">If you'd like to test your page, run the *Movies* page now and click an **Edit** link next to any movie.</span></span> <span data-ttu-id="03f67-242">您會看到*EditMovie*詳細資料已填入資料頁面您選取的電影：</span><span class="sxs-lookup"><span data-stu-id="03f67-242">You'll see the *EditMovie* page with the details filled in for the movie you selected:</span></span>

![編輯顯示要編輯影片的影片頁面](updating-data/_static/image3.png)

<span data-ttu-id="03f67-244">請注意，頁面的 URL 會包含類似`?id=10`（或其他數字）。</span><span class="sxs-lookup"><span data-stu-id="03f67-244">Notice that the URL of the page includes something like `?id=10` (or some other number).</span></span> <span data-ttu-id="03f67-245">到目前為止，測試**編輯**中的連結*電影*頁面上的工作，您的頁面從查詢字串中，讀取識別碼，而且資料庫取得單一影片記錄查詢運作。</span><span class="sxs-lookup"><span data-stu-id="03f67-245">So far you've tested that **Edit** links in the *Movie* page work, that your page is reading the ID from the query string, and that the database query to get a single movie record is working.</span></span>

<span data-ttu-id="03f67-246">您可以變更的電影資訊，但當您按一下時，會發生任何事**送出變更**。</span><span class="sxs-lookup"><span data-stu-id="03f67-246">You can change the movie information, but nothing happens when you click **Submit Changes**.</span></span>

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a><span data-ttu-id="03f67-247">加入程式碼以使用使用者的變更來更新影片</span><span class="sxs-lookup"><span data-stu-id="03f67-247">Adding Code to Update the Movie with the User's Changes</span></span>

<span data-ttu-id="03f67-248">在  *EditMovie.cshtml*檔案中，若要實作 （儲存變更） 的第二個函式，新增下列程式碼的右括號之內`@`區塊。</span><span class="sxs-lookup"><span data-stu-id="03f67-248">In the *EditMovie.cshtml* file, to implement the second function (saving changes), add the following code just inside the closing brace of the `@` block.</span></span> <span data-ttu-id="03f67-249">(如果您不確定確實要放置程式碼的位置，您可以看看[完整的程式碼清單編輯影片頁面](#Complete_Page_Listing_for_EditMovie)出現在本教學課程結尾處。)</span><span class="sxs-lookup"><span data-stu-id="03f67-249">(If you're not sure exactly where to put the code, you can look at the [complete code listing for the Edit Movie page](#Complete_Page_Listing_for_EditMovie) that appears at the end of this tutorial.)</span></span>

[!code-csharp[Main](updating-data/samples/sample12.cs)]

<span data-ttu-id="03f67-250">同樣地，此標記和程式碼是中的程式碼類似*AddMovie*。</span><span class="sxs-lookup"><span data-stu-id="03f67-250">Again, this markup and code is similar to the code in *AddMovie*.</span></span> <span data-ttu-id="03f67-251">程式碼位於`if(IsPost)`封鎖，因為只有在使用者按一下時，才執行此程式碼**送出變更** 按鈕&mdash;也就是當 （並時，才） 已張貼的表單。</span><span class="sxs-lookup"><span data-stu-id="03f67-251">The code is in an `if(IsPost)` block, because this code runs only when the user clicks the **Submit Changes** button &mdash; that is, when (and only when) the form has been posted.</span></span> <span data-ttu-id="03f67-252">在此情況下，您不使用類似的測試`if(IsPost && Validation.IsValid())`— 也就無法結合這兩項測試使用 and。</span><span class="sxs-lookup"><span data-stu-id="03f67-252">In this case, you're not using a test like `if(IsPost && Validation.IsValid())`— that is, you're not combining both tests by using AND.</span></span> <span data-ttu-id="03f67-253">在此頁面中，您先判斷是否有表單提交 (`if(IsPost)`)，然後只再登錄進行驗證的欄位。</span><span class="sxs-lookup"><span data-stu-id="03f67-253">In this page, you first determine whether there's a form submission (`if(IsPost)`), and only then register the fields for validation.</span></span> <span data-ttu-id="03f67-254">然後您可以測試驗證結果 (`if(Validation.IsValid()`)。</span><span class="sxs-lookup"><span data-stu-id="03f67-254">Then you can test the validation results (`if(Validation.IsValid()`).</span></span> <span data-ttu-id="03f67-255">流程會稍有不同*AddMovie.cshtml*  頁面上，但效果是一樣。</span><span class="sxs-lookup"><span data-stu-id="03f67-255">The flow is slightly different than in the *AddMovie.cshtml* page, but the effect is the same.</span></span>

<span data-ttu-id="03f67-256">使用取得的文字方塊中的值`Request.Form["title"]`和其他類似的程式碼`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="03f67-256">You get the values of the text boxes by using `Request.Form["title"]` and similar code for the other `<input>` elements.</span></span> <span data-ttu-id="03f67-257">請注意，此時，程式碼會取得電影識別碼從隱藏的欄位 (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="03f67-257">Notice that this time, the code gets the movie ID out of the hidden field (`<input type="hidden">`).</span></span> <span data-ttu-id="03f67-258">當頁面執行第一次時，程式碼會取得從查詢字串的識別碼。</span><span class="sxs-lookup"><span data-stu-id="03f67-258">When the page ran the first time, the code got the ID out of the query string.</span></span> <span data-ttu-id="03f67-259">您從隱藏的欄位，藉此確定您取得識別碼的影片，原本顯示，以防之後以某種方式修改查詢字串取得的值。</span><span class="sxs-lookup"><span data-stu-id="03f67-259">You get the value from the hidden field to make sure that you're getting the ID of the movie that was originally displayed, in case the query string was somehow altered since then.</span></span>

<span data-ttu-id="03f67-260">之間非常重要的差異*AddMovie*程式碼，此程式碼是在這段程式碼中使用 SQL`Update`陳述式來取代`Insert Into`陳述式。</span><span class="sxs-lookup"><span data-stu-id="03f67-260">The really important difference between the *AddMovie* code and this code is that in this code you use the SQL `Update` statement instead of the `Insert Into` statement.</span></span> <span data-ttu-id="03f67-261">下列範例示範的 SQL 語法`Update`陳述式：</span><span class="sxs-lookup"><span data-stu-id="03f67-261">The following example shows the syntax of the SQL `Update` statement:</span></span>

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

<span data-ttu-id="03f67-262">您可以依任何順序指定的任何資料行，而且不一定需要更新期間的每個資料行`Update`作業。</span><span class="sxs-lookup"><span data-stu-id="03f67-262">You can specify any columns in any order, and you don't necessarily have to update every column during an `Update` operation.</span></span> <span data-ttu-id="03f67-263">(因為，實際上會為新的記錄，來儲存記錄，而且不允許的無法更新識別碼本身，`Update`作業。)</span><span class="sxs-lookup"><span data-stu-id="03f67-263">(You cannot update the ID itself, because that would in effect save the record as a new record, and that's not allowed for an `Update` operation.)</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="03f67-264">**重要**`Where`識別碼子句是很重要，因為這是資料庫如何知道哪一個資料庫記錄您想要更新。</span><span class="sxs-lookup"><span data-stu-id="03f67-264">**Important** The `Where` clause with the ID is very important, because that's how the database knows which database record you want to update.</span></span> <span data-ttu-id="03f67-265">如果您離開`Where`子句中，資料庫會更新*每個*記錄資料庫中。</span><span class="sxs-lookup"><span data-stu-id="03f67-265">If you left off the `Where` clause, the database would update *every* record in the database.</span></span> <span data-ttu-id="03f67-266">在大部分情況下，是嚴重損壞。</span><span class="sxs-lookup"><span data-stu-id="03f67-266">In most cases, that would be a disaster.</span></span>


<span data-ttu-id="03f67-267">在程式碼中，若要更新的值會傳遞至 SQL 陳述式使用預留位置。</span><span class="sxs-lookup"><span data-stu-id="03f67-267">In the code, the values to update are passed to the SQL statement by using placeholders.</span></span> <span data-ttu-id="03f67-268">若要重複我們剛才說過： 基於安全性理由*只*使用預留位置來將值傳遞至 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="03f67-268">To repeat what we've said before: for security reasons, *only* use placeholders to pass values to a SQL statement.</span></span>

<span data-ttu-id="03f67-269">程式碼會使用後`db.Execute`執行`Update`陳述式，它會重新導向回到清單頁面上，您可以在其中看到所做的變更。</span><span class="sxs-lookup"><span data-stu-id="03f67-269">After the code uses `db.Execute` to run the `Update` statement, it redirects back to the listing page, where you can see the changes.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="03f67-270">**不同的 SQL 陳述式，不同的方法**</span><span class="sxs-lookup"><span data-stu-id="03f67-270">**Different SQL Statements, Different Methods**</span></span>
> 
> <span data-ttu-id="03f67-271">您可能已經注意到，您會使用稍微不同的方法來執行不同的 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="03f67-271">You might have noticed that you use slightly different methods to run different SQL statements.</span></span> <span data-ttu-id="03f67-272">若要執行`Select`可能查詢會傳回多筆記錄，您使用`Query`方法。</span><span class="sxs-lookup"><span data-stu-id="03f67-272">To run a `Select` query that potentially returns multiple records, you use the `Query` method.</span></span> <span data-ttu-id="03f67-273">若要執行`Select`您知道的查詢會傳回只有一個資料庫項目，則使用`QuerySingle`方法。</span><span class="sxs-lookup"><span data-stu-id="03f67-273">To run a `Select` query that you know will return only one database item, you use the `QuerySingle` method.</span></span> <span data-ttu-id="03f67-274">若要執行的命令，進行變更，但不傳回資料庫項目，您使用`Execute`方法。</span><span class="sxs-lookup"><span data-stu-id="03f67-274">To run commands that make changes but that don't return database items, you use the `Execute` method.</span></span>
> 
> <span data-ttu-id="03f67-275">您必須有不同的方法，因為每個會傳回不同的結果，如您所見已經在之間的差異`Query`和`QuerySingle`。</span><span class="sxs-lookup"><span data-stu-id="03f67-275">You have to have different methods because each of them returns different results, as you saw already in the difference between `Query` and `QuerySingle`.</span></span> <span data-ttu-id="03f67-276">(`Execute`方法實際上會傳回值也&mdash;亦即已受到該命令的資料庫資料列數目&mdash;您已被忽略，到目前為止，但。)</span><span class="sxs-lookup"><span data-stu-id="03f67-276">(The `Execute` method actually returns a value also &mdash; namely, the number of database rows that were affected by the command &mdash; but you've been ignoring that so far.)</span></span>
> 
> <span data-ttu-id="03f67-277">當然，`Query`方法可能會傳回只有一個資料庫的資料列。</span><span class="sxs-lookup"><span data-stu-id="03f67-277">Of course, the `Query` method might return only one database row.</span></span> <span data-ttu-id="03f67-278">不過，ASP.NET 一律會將結果`Query`為集合的方法。</span><span class="sxs-lookup"><span data-stu-id="03f67-278">However, ASP.NET always treats the results of the `Query` method as a collection.</span></span> <span data-ttu-id="03f67-279">即使方法傳回只有一個資料列，您必須從集合擷取該單一資料列。</span><span class="sxs-lookup"><span data-stu-id="03f67-279">Even if the method returns just one row, you have to extract that single row from the collection.</span></span> <span data-ttu-id="03f67-280">因此，在情況下，您*知道*您會得到只有一個資料列，更方便使用的位元`QuerySingle`。</span><span class="sxs-lookup"><span data-stu-id="03f67-280">Therefore, in situations where you *know* you'll get back only one row, it's a bit more convenient to use `QuerySingle`.</span></span>
> 
> <span data-ttu-id="03f67-281">有幾個其他執行特定類型的資料庫作業的方法。</span><span class="sxs-lookup"><span data-stu-id="03f67-281">There are a few other methods that perform specific types of database operations.</span></span> <span data-ttu-id="03f67-282">您可以找到資料庫中的方法清單[ASP.NET Web Pages API 的快速參考](../../api-reference/asp-net-web-pages-api-reference.md#Data)。</span><span class="sxs-lookup"><span data-stu-id="03f67-282">You can find a listing of database methods in the [ASP.NET Web Pages API Quick Reference](../../api-reference/asp-net-web-pages-api-reference.md#Data).</span></span>


## <a name="making-validation-for-the-id-more-robust"></a><span data-ttu-id="03f67-283">讓穩固，識別碼越多的驗證</span><span class="sxs-lookup"><span data-stu-id="03f67-283">Making Validation for the ID More Robust</span></span>

<span data-ttu-id="03f67-284">初次執行網頁時，您取得的電影識別碼從查詢字串，以便您即可從資料庫取得該影片。</span><span class="sxs-lookup"><span data-stu-id="03f67-284">The first time that the page runs, you get the movie ID from the query string so that you can go get that movie from the database.</span></span> <span data-ttu-id="03f67-285">您很確定實際發生去尋找，使用此程式碼的值：</span><span class="sxs-lookup"><span data-stu-id="03f67-285">You made sure that there actually was a value to go look for, which you did by using this code:</span></span>

[!code-csharp[Main](updating-data/samples/sample13.cs)]

<span data-ttu-id="03f67-286">使用此代碼可確保當使用者到達*EditMovies*頁面上，必須先選取電影*電影* 頁面上，頁面會顯示使用者易記錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="03f67-286">You used this code to make sure that if a user gets to the *EditMovies* page without first selecting a movie in the *Movies* page, the page would display a user-friendly error message.</span></span> <span data-ttu-id="03f67-287">（否則使用者會看到錯誤，可能會可能混淆。）</span><span class="sxs-lookup"><span data-stu-id="03f67-287">(Otherwise, users would see an error that would probably just confuse them.)</span></span>

<span data-ttu-id="03f67-288">不過，這項驗證不是非常強大。</span><span class="sxs-lookup"><span data-stu-id="03f67-288">However, this validation isn't very robust.</span></span> <span data-ttu-id="03f67-289">只有這些錯誤也可能會叫用頁面：</span><span class="sxs-lookup"><span data-stu-id="03f67-289">The page might also be invoked with these errors:</span></span>

- <span data-ttu-id="03f67-290">識別碼不是數字。</span><span class="sxs-lookup"><span data-stu-id="03f67-290">The ID isn't a number.</span></span> <span data-ttu-id="03f67-291">例如，無法之類的 URL 叫用頁面`http://localhost:nnnnn/EditMovie?id=abc`。</span><span class="sxs-lookup"><span data-stu-id="03f67-291">For example, the page could be invoked with a URL like `http://localhost:nnnnn/EditMovie?id=abc`.</span></span>
- <span data-ttu-id="03f67-292">識別碼是數字，但它會參考不存在的影片 (例如`http://localhost:nnnnn/EditMovie?id=100934`)。</span><span class="sxs-lookup"><span data-stu-id="03f67-292">The ID is a number, but it references a movie that doesn't exist (for example, `http://localhost:nnnnn/EditMovie?id=100934`).</span></span>

<span data-ttu-id="03f67-293">如果您想知道，請參閱錯誤所造成的這些 Url 當中，執行*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-293">If you're curious to see the errors that result from these URLs, run the *Movies* page.</span></span> <span data-ttu-id="03f67-294">選取要編輯的影片，然後變更 的 URL *EditMovie*頁面，即可為 URL，內含字母識別碼或不存在電影識別碼。</span><span class="sxs-lookup"><span data-stu-id="03f67-294">Select a movie to edit, and then change the URL of the *EditMovie* page to a URL that contains an alphabetic ID or the ID of a non-existent movie.</span></span>

<span data-ttu-id="03f67-295">因此您怎麼辦？</span><span class="sxs-lookup"><span data-stu-id="03f67-295">So what should you do?</span></span> <span data-ttu-id="03f67-296">第一次的修正方法是確保，不只識別碼傳遞至頁面上，但識別碼是整數。</span><span class="sxs-lookup"><span data-stu-id="03f67-296">The first fix is to make sure that not only is an ID passed to the page, but that the ID is an integer.</span></span> <span data-ttu-id="03f67-297">變更的程式碼`!IsPost`測試此範例所示：</span><span class="sxs-lookup"><span data-stu-id="03f67-297">Change the code for the `!IsPost` test to look like this example:</span></span>

[!code-csharp[Main](updating-data/samples/sample14.cs)]

<span data-ttu-id="03f67-298">您已新增第二個條件`IsEmpty`測試，與連結`&&`(邏輯 AND):</span><span class="sxs-lookup"><span data-stu-id="03f67-298">You've added a second condition to the `IsEmpty` test, linked with `&&` (logical AND):</span></span>

[!code-csharp[Main](updating-data/samples/sample15.cs)]

<span data-ttu-id="03f67-299">從大概還記得[ASP.NET Web Pages 程式設計簡介](../introducing-razor-syntax-c.md)等方法的教學課程`AsBool``AsInt`將字元字串轉換成其他資料類型。</span><span class="sxs-lookup"><span data-stu-id="03f67-299">You might remember from the [Introduction to ASP.NET Web Pages Programming](../introducing-razor-syntax-c.md) tutorial that methods like `AsBool` an `AsInt` convert a character string to some other data type.</span></span> <span data-ttu-id="03f67-300">`IsInt`方法 (和其他項目，例如`IsBool`和`IsDateTime`) 類似。</span><span class="sxs-lookup"><span data-stu-id="03f67-300">The `IsInt` method (and others, like `IsBool` and `IsDateTime`) are similar.</span></span> <span data-ttu-id="03f67-301">不過，僅限測試是否您*可以*轉換的字串，而不實際執行轉換。</span><span class="sxs-lookup"><span data-stu-id="03f67-301">However, they test only whether you *can* convert the string, without actually performing the conversion.</span></span> <span data-ttu-id="03f67-302">這裡您基本上就表示*如果查詢字串值可以轉換成整數...*.</span><span class="sxs-lookup"><span data-stu-id="03f67-302">So here you're essentially saying *If the query string value can be converted to an integer ...*.</span></span>

<span data-ttu-id="03f67-303">不存在的電影會尋找潛在的問題。</span><span class="sxs-lookup"><span data-stu-id="03f67-303">The other potential problem is looking for a movie that doesn't exist.</span></span> <span data-ttu-id="03f67-304">取得影片的程式碼看起來像此程式碼：</span><span class="sxs-lookup"><span data-stu-id="03f67-304">The code to get a movie looks like this code:</span></span>

[!code-csharp[Main](updating-data/samples/sample16.cs)]

<span data-ttu-id="03f67-305">如果您傳遞`movieId`值加入`QuerySingle`未對應到實際電影的方法，沒有傳回任何內容和陳述式，遵循 (比方說， `title=row.Title`) 會導致錯誤。</span><span class="sxs-lookup"><span data-stu-id="03f67-305">If you pass a `movieId` value to the `QuerySingle` method that doesn't correspond to an actual movie, nothing is returned and the statements that follow (for example, `title=row.Title`) result in errors.</span></span>

<span data-ttu-id="03f67-306">一次很容易解決。</span><span class="sxs-lookup"><span data-stu-id="03f67-306">Again there's an easy fix.</span></span> <span data-ttu-id="03f67-307">如果`db.QuerySingle`方法會傳回任何結果，`row`變數將會是 null。</span><span class="sxs-lookup"><span data-stu-id="03f67-307">If the `db.QuerySingle` method returns no results, the `row` variable will be null.</span></span> <span data-ttu-id="03f67-308">因此您可以檢查是否`row`變數為 null，然後再從它取得的值。</span><span class="sxs-lookup"><span data-stu-id="03f67-308">So you can check whether the `row` variable is null before you try to get values from it.</span></span> <span data-ttu-id="03f67-309">下列程式碼加入`if`取得的值的陳述式前後的區塊`row`物件：</span><span class="sxs-lookup"><span data-stu-id="03f67-309">The following code adds an `if` block around the statements that get the values out of the `row` object:</span></span>

[!code-csharp[Main](updating-data/samples/sample17.cs)]

<span data-ttu-id="03f67-310">這兩個額外的驗證測試，網頁變得更確實。</span><span class="sxs-lookup"><span data-stu-id="03f67-310">With these two additional validation tests, the page becomes more bullet-proof.</span></span> <span data-ttu-id="03f67-311">完整程式碼`!IsPost`分支現在看起來像此範例中：</span><span class="sxs-lookup"><span data-stu-id="03f67-311">The complete code for the `!IsPost` branch now looks like this example:</span></span>

[!code-csharp[Main](updating-data/samples/sample18.cs)]

<span data-ttu-id="03f67-312">我們會請注意一次，這項工作的好用途`else`區塊。</span><span class="sxs-lookup"><span data-stu-id="03f67-312">We'll note once more that this task is a good use for an `else` block.</span></span> <span data-ttu-id="03f67-313">如果測試不成功，`else`區塊設定錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="03f67-313">If the tests don't pass, the `else` blocks set error messages.</span></span>

## <a name="adding-a-link-to-return-to-the-movies-page"></a><span data-ttu-id="03f67-314">新增傳回至 Movies 頁面的連結</span><span class="sxs-lookup"><span data-stu-id="03f67-314">Adding a Link to Return to the Movies Page</span></span>

<span data-ttu-id="03f67-315">最後也很有幫助的詳細資料是要將連結新增回到*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-315">A final and helpful detail is to add a link back to the *Movies* page.</span></span> <span data-ttu-id="03f67-316">在一般的事件流程中，使用者將會在啟動*電影*頁面，然後按一下**編輯**連結。</span><span class="sxs-lookup"><span data-stu-id="03f67-316">In the ordinary flow of events, users will start at the *Movies* page and click an **Edit** link.</span></span> <span data-ttu-id="03f67-317">予以更正，使*EditMovie* ] 頁面上，他們可以在此編輯影片，然後按一下 [[] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="03f67-317">That brings them to the *EditMovie* page, where they can edit the movie and click the button.</span></span> <span data-ttu-id="03f67-318">程式碼會處理變更之後，它會重新導向回到*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-318">After the code processes the change, it redirects back to the *Movies* page.</span></span>

<span data-ttu-id="03f67-319">但是：</span><span class="sxs-lookup"><span data-stu-id="03f67-319">However:</span></span>

- <span data-ttu-id="03f67-320">使用者不可能會決定在變更任何項目。</span><span class="sxs-lookup"><span data-stu-id="03f67-320">The user might decide not to change anything.</span></span>
- <span data-ttu-id="03f67-321">使用者可能沒有先按一下收到此網頁**編輯**連結*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-321">The user might have gotten to this page without first clicking an **Edit** link in the *Movies* page.</span></span>

<span data-ttu-id="03f67-322">無論如何，您會想要讓他們可以輕鬆將返回主要清單。</span><span class="sxs-lookup"><span data-stu-id="03f67-322">Either way, you want to make it easy for them to return to the main listing.</span></span> <span data-ttu-id="03f67-323">它很容易解決&mdash;後方結尾加入下列標記`</form>`在標記中的標記：</span><span class="sxs-lookup"><span data-stu-id="03f67-323">It's an easy fix &mdash; add the following markup just after the closing `</form>` tag in the markup:</span></span>

[!code-html[Main](updating-data/samples/sample19.html)]

<span data-ttu-id="03f67-324">此標記會使用的相同語法`<a>`您在其他地方看到的項目。</span><span class="sxs-lookup"><span data-stu-id="03f67-324">This markup uses the same syntax for an `<a>` element that you've seen elsewhere.</span></span> <span data-ttu-id="03f67-325">URL 包含`~`來表示 「 根網站的 」。</span><span class="sxs-lookup"><span data-stu-id="03f67-325">The URL includes `~` to mean "root of the website."</span></span>

## <a name="testing-the-movie-update-process"></a><span data-ttu-id="03f67-326">測試影片的更新程序</span><span class="sxs-lookup"><span data-stu-id="03f67-326">Testing the Movie Update Process</span></span>

<span data-ttu-id="03f67-327">現在您可以測試。</span><span class="sxs-lookup"><span data-stu-id="03f67-327">Now you can test.</span></span> <span data-ttu-id="03f67-328">執行*電影*頁面，然後按一下**編輯**電影旁邊。</span><span class="sxs-lookup"><span data-stu-id="03f67-328">Run the *Movies* page, and click **Edit** next to a movie.</span></span> <span data-ttu-id="03f67-329">當*EditMovie*頁面隨即出現，變更至影片，然後按一下**送出變更**。</span><span class="sxs-lookup"><span data-stu-id="03f67-329">When the *EditMovie* page appears, make changes to the movie and click **Submit Changes**.</span></span> <span data-ttu-id="03f67-330">當電影清單隨即出現時，請確定您的變更，會顯示。</span><span class="sxs-lookup"><span data-stu-id="03f67-330">When the movie listing appears, make sure that your changes are shown.</span></span>

<span data-ttu-id="03f67-331">若要確定驗證是否運作，請按一下**編輯**另一部電影。</span><span class="sxs-lookup"><span data-stu-id="03f67-331">To make sure that validation is working, click **Edit** for another movie.</span></span> <span data-ttu-id="03f67-332">當您進入*EditMovie*頁面上，清除**內容類型**欄位 (或**年** 欄位中，或兩者)，並嘗試提交您的變更。</span><span class="sxs-lookup"><span data-stu-id="03f67-332">When you get to the *EditMovie* page, clear the **Genre** field (or **Year** field, or both) and try to submit your changes.</span></span> <span data-ttu-id="03f67-333">如您所預期，您會看到錯誤：</span><span class="sxs-lookup"><span data-stu-id="03f67-333">You'll see an error, as you'd expect:</span></span>

![編輯影片頁面顯示驗證錯誤](updating-data/_static/image4.png)

<span data-ttu-id="03f67-335">按一下 **傳回的電影清單**連結，以放棄變更並返回*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="03f67-335">Click the **Return to movie listing** link to abandon your changes and return to the *Movies* page.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="03f67-336">接下來的下一步</span><span class="sxs-lookup"><span data-stu-id="03f67-336">Coming Up Next</span></span>

<span data-ttu-id="03f67-337">在下一個教學課程中，您會看到如何刪除電影錄製。</span><span class="sxs-lookup"><span data-stu-id="03f67-337">In the next tutorial, you'll see how to delete a movie record.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a><span data-ttu-id="03f67-338">（更新的編輯連結） 的電影頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="03f67-338">Complete Listing for Movie Page (Updated with Edit Links)</span></span>

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a><span data-ttu-id="03f67-339">完成編輯影片頁面的頁面清單</span><span class="sxs-lookup"><span data-stu-id="03f67-339">Complete Page Listing for Edit Movie Page</span></span>

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="03f67-340">其他資源</span><span class="sxs-lookup"><span data-stu-id="03f67-340">Additional Resources</span></span>

- [<span data-ttu-id="03f67-341">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="03f67-341">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../../getting-started/introducing-razor-syntax-c.md)
- <span data-ttu-id="03f67-342">[SQL UPDATE 陳述式](http://www.w3schools.com/sql/sql_update.asp)W3Schools 站台上</span><span class="sxs-lookup"><span data-stu-id="03f67-342">[SQL UPDATE Statement](http://www.w3schools.com/sql/sql_update.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="03f67-343">[上一頁](entering-data.md)
> [下一頁](deleting-data.md)</span><span class="sxs-lookup"><span data-stu-id="03f67-343">[Previous](entering-data.md)
[Next](deleting-data.md)</span></span>
