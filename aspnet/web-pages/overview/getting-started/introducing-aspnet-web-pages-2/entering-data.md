---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: 導入 ASP.NET Web Pages-輸入資料庫的資料，使用表單 |Microsoft 文件
author: tfitzmac
description: 本教學課程會示範如何建立項目表單，然後輸入 得到的資料從表單到資料庫資料表時使用 ASP.NET Web Pages （...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: bbccf8134e90c19e29efaa5afe1e46e15320c189
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897958"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a><span data-ttu-id="0421d-103">導入 ASP.NET Web Pages-使用表單中輸入資料庫的資料</span><span class="sxs-lookup"><span data-stu-id="0421d-103">Introducing ASP.NET Web Pages - Entering Database Data by Using Forms</span></span>
====================
<span data-ttu-id="0421d-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0421d-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0421d-105">本教學課程會示範如何建立項目表單，然後輸入 得到的資料從表單到資料庫資料表時使用 ASP.NET Web Pages (Razor)。</span><span class="sxs-lookup"><span data-stu-id="0421d-105">This tutorial shows you how to create an entry form and then enter the data that you get from the form into a database table when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="0421d-106">它會假設您已完成透過數列[基本概念的 HTML 表單中的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581)。</span><span class="sxs-lookup"><span data-stu-id="0421d-106">It assumes you have completed the series through [Basics of HTML Forms in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).</span></span>
> 
> <span data-ttu-id="0421d-107">您將學習：</span><span class="sxs-lookup"><span data-stu-id="0421d-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="0421d-108">深入了解如何處理項目表單。</span><span class="sxs-lookup"><span data-stu-id="0421d-108">More about how to process entry forms.</span></span>
> - <span data-ttu-id="0421d-109">如何在資料庫中加入資料 (insert)。</span><span class="sxs-lookup"><span data-stu-id="0421d-109">How to add (insert) data in a database.</span></span>
> - <span data-ttu-id="0421d-110">如何確定使用者有 （如何驗證使用者輸入） 表單中輸入必要的值。</span><span class="sxs-lookup"><span data-stu-id="0421d-110">How to make sure that users have entered a required value in a form (how to validate user input).</span></span>
> - <span data-ttu-id="0421d-111">如何顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="0421d-111">How to display validation errors.</span></span>
> - <span data-ttu-id="0421d-112">如何從目前的頁面跳至另一個頁面。</span><span class="sxs-lookup"><span data-stu-id="0421d-112">How to jump to another page from the current page.</span></span>
>   
> 
> <span data-ttu-id="0421d-113">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="0421d-113">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="0421d-114">`Database.Execute` 方法</span><span class="sxs-lookup"><span data-stu-id="0421d-114">The `Database.Execute` method.</span></span>
> - <span data-ttu-id="0421d-115">SQL`Insert Into`陳述式</span><span class="sxs-lookup"><span data-stu-id="0421d-115">The SQL `Insert Into` statement</span></span>
> - <span data-ttu-id="0421d-116">`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="0421d-116">The `Validation` helper.</span></span>
> - <span data-ttu-id="0421d-117">`Response.Redirect` 方法</span><span class="sxs-lookup"><span data-stu-id="0421d-117">The `Response.Redirect` method.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="0421d-118">您將建置</span><span class="sxs-lookup"><span data-stu-id="0421d-118">What You'll Build</span></span>

<span data-ttu-id="0421d-119">教學課程中先前的範例說明了如何建立資料庫，您必須輸入資料庫的資料藉由編輯直接在 WebMatrix 中，使用資料庫**資料庫**工作區。</span><span class="sxs-lookup"><span data-stu-id="0421d-119">In the tutorial earlier that showed you how to create a database, you entered database data by editing the database directly in WebMatrix, working in the **Database** workspace.</span></span> <span data-ttu-id="0421d-120">在大部分的應用程式，這是不可行的方式，可將資料放入資料庫，不過。</span><span class="sxs-lookup"><span data-stu-id="0421d-120">In most apps, that's not a practical way to put data into the database, though.</span></span> <span data-ttu-id="0421d-121">因此在本教學課程中，您將建立的 web 型介面，可讓您或任何人輸入的資料，並將它儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="0421d-121">So in this tutorial, you'll create a web-based interface that lets you or anyone enter data and save it to the database.</span></span>

<span data-ttu-id="0421d-122">您要建立的頁面，您可以在此輸入新的電影。</span><span class="sxs-lookup"><span data-stu-id="0421d-122">You'll create a page where you can enter new movies.</span></span> <span data-ttu-id="0421d-123">頁面會包含具有的欄位 （文字方塊），您可以在其中輸入電影標題、 類型和年份的項目表單。</span><span class="sxs-lookup"><span data-stu-id="0421d-123">The page will contain an entry form that has fields (text boxes) where you can enter a movie title, genre, and year.</span></span> <span data-ttu-id="0421d-124">網頁看起來像此頁面：</span><span class="sxs-lookup"><span data-stu-id="0421d-124">The page will look like this page:</span></span>

![瀏覽器中的 [新增電影] 頁面](entering-data/_static/image1.png)

<span data-ttu-id="0421d-126">文字方塊中，將會是 HTML`<input>`項目，看起來像此標記：</span><span class="sxs-lookup"><span data-stu-id="0421d-126">The text boxes will be HTML `<input>` elements that will look like this markup:</span></span>

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a><span data-ttu-id="0421d-127">建立基本的項目表單</span><span class="sxs-lookup"><span data-stu-id="0421d-127">Creating the Basic Entry Form</span></span>

<span data-ttu-id="0421d-128">建立名為的頁面*AddMovie.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0421d-128">Create a page named *AddMovie.cshtml*.</span></span>

<span data-ttu-id="0421d-129">取代什麼是以下列標記檔案中。</span><span class="sxs-lookup"><span data-stu-id="0421d-129">Replace what's in the file with the following markup.</span></span> <span data-ttu-id="0421d-130">覆寫所有項目。您稍後要加入的程式碼區塊頂端。</span><span class="sxs-lookup"><span data-stu-id="0421d-130">Overwrite everything; you'll add a code block at the top shortly.</span></span>

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

<span data-ttu-id="0421d-131">這個範例示範典型的 HTML 建立表單。</span><span class="sxs-lookup"><span data-stu-id="0421d-131">This example shows typical HTML for creating a form.</span></span> <span data-ttu-id="0421d-132">它會使用`<input>`[提交] 按鈕和文字方塊中的項目。</span><span class="sxs-lookup"><span data-stu-id="0421d-132">It uses `<input>` elements for the text boxes and for the submit button.</span></span> <span data-ttu-id="0421d-133">使用標準所建立之文字方塊的標題`<label>`項目。</span><span class="sxs-lookup"><span data-stu-id="0421d-133">The captions for the text boxes are created by using standard `<label>` elements.</span></span> <span data-ttu-id="0421d-134">`<fieldset>`和`<legend>`項目放置在表單上四處 nice 方塊。</span><span class="sxs-lookup"><span data-stu-id="0421d-134">The `<fieldset>` and `<legend>` elements put a nice box around the form.</span></span>

<span data-ttu-id="0421d-135">請注意，在此頁面上，`<form>`項目使用`post`做為值`method`屬性。</span><span class="sxs-lookup"><span data-stu-id="0421d-135">Notice that in this page, the `<form>` element uses `post` as the value for the `method` attribute.</span></span> <span data-ttu-id="0421d-136">在上一個教學課程中，您已建立一個表單，用`get`方法。</span><span class="sxs-lookup"><span data-stu-id="0421d-136">In the previous tutorial, you created a form that used the `get` method.</span></span> <span data-ttu-id="0421d-137">因為雖然表單送出至伺服器的值，但要求並未進行任何變更，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="0421d-137">That was correct, because although the form submitted values to the server, the request did not make any changes.</span></span> <span data-ttu-id="0421d-138">一樣是擷取資料以不同的方式。</span><span class="sxs-lookup"><span data-stu-id="0421d-138">All it did was fetch data in different ways.</span></span> <span data-ttu-id="0421d-139">不過，在這個頁面上您*將*進行變更，您要加入新的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="0421d-139">However, in this page you *will* make changes—you're going to add new database records.</span></span> <span data-ttu-id="0421d-140">因此，應該使用這種形式`post`方法。</span><span class="sxs-lookup"><span data-stu-id="0421d-140">Therefore, this form should use the `post` method.</span></span> <span data-ttu-id="0421d-141">(如需詳細資訊之間的差異`GET`和`POST`作業，請參閱[GET、 POST 和 HTTP 動詞命令安全性](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety)在上一個教學課程中的 [資訊看板]。)</span><span class="sxs-lookup"><span data-stu-id="0421d-141">(For more about the difference between `GET` and `POST` operations, see the[GET, POST, and HTTP Verb Safety](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) sidebar in the previous tutorial.)</span></span>

<span data-ttu-id="0421d-142">請注意，每個文字方塊具有`name`元素 (`title`， `genre`， `year`)。</span><span class="sxs-lookup"><span data-stu-id="0421d-142">Note that each text box has a `name` element (`title`, `genre`, `year`).</span></span> <span data-ttu-id="0421d-143">如同上一個教學課程中，這些名稱很重要，因為您必須有這些名稱，因此您可以稍後取得使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="0421d-143">As you saw in the previous tutorial, these names are important because you must have those names so you can get the user's input later.</span></span> <span data-ttu-id="0421d-144">您可以使用任何名稱。</span><span class="sxs-lookup"><span data-stu-id="0421d-144">You can use any names.</span></span> <span data-ttu-id="0421d-145">最好先使用有意義的名稱，協助您記住您正在使用哪些資料。</span><span class="sxs-lookup"><span data-stu-id="0421d-145">It's helpful to use meaningful names that help you remember what data you're working with.</span></span>

<span data-ttu-id="0421d-146">`value`每個屬性`<input>`項目包含的位元的 Razor 程式碼 (例如， `Request.Form["title"]`)。</span><span class="sxs-lookup"><span data-stu-id="0421d-146">The `value` attribute of each `<input>` element contains a bit of Razor code (for example, `Request.Form["title"]`).</span></span> <span data-ttu-id="0421d-147">在上一個教學課程，以保留 （如果有的話） 在文字方塊中輸入的值中，在提交表單之後，您學到這個技巧的版本。</span><span class="sxs-lookup"><span data-stu-id="0421d-147">You learned a version of this trick in the previous tutorial to preserve the value entered into the text box (if any) after the form has been submitted.</span></span>

## <a name="getting-the-form-values"></a><span data-ttu-id="0421d-148">取得表單值</span><span class="sxs-lookup"><span data-stu-id="0421d-148">Getting the Form Values</span></span>

<span data-ttu-id="0421d-149">接著，您將處理表單的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0421d-149">Next, you add code that processes the form.</span></span> <span data-ttu-id="0421d-150">您將在大綱中，進行下列項目：</span><span class="sxs-lookup"><span data-stu-id="0421d-150">In outline, you'll do the following:</span></span>

1. <span data-ttu-id="0421d-151">檢查網頁是否正在回傳 （提交）。</span><span class="sxs-lookup"><span data-stu-id="0421d-151">Check whether the page is being posted (was submitted).</span></span> <span data-ttu-id="0421d-152">您想要您的程式碼只有在使用者按下按鈕，不會在頁面上第一次執行時，才執行。</span><span class="sxs-lookup"><span data-stu-id="0421d-152">You want to your code to run only when users have clicked the button, not when the page first runs.</span></span>
2. <span data-ttu-id="0421d-153">取得使用者在文字方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="0421d-153">Get the values that the user entered into the text boxes.</span></span> <span data-ttu-id="0421d-154">在此情況下，因為使用的表單`POST`動詞命令，取得從表單值`Request.Form`集合。</span><span class="sxs-lookup"><span data-stu-id="0421d-154">In this case, because the form is using the `POST` verb, you get the form values from the `Request.Form` collection.</span></span>
3. <span data-ttu-id="0421d-155">為新記錄中插入值*電影*資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="0421d-155">Insert the values as a new record in the *Movies* database table.</span></span>

<span data-ttu-id="0421d-156">在檔案頂端加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0421d-156">At the top of the file, add the following code:</span></span>

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

<span data-ttu-id="0421d-157">前幾行建立的變數 (`title`， `genre`，和`year`) 來保存從文字方塊中的值。</span><span class="sxs-lookup"><span data-stu-id="0421d-157">The first few lines create variables (`title`, `genre`, and `year`) to hold the values from the text boxes.</span></span> <span data-ttu-id="0421d-158">線條`if(IsPost)`可確保所設定的變數*只*當使用者按一下**新增電影**按鈕 — 也就是當表單張貼。</span><span class="sxs-lookup"><span data-stu-id="0421d-158">The line `if(IsPost)` makes sure that the variables are set *only* when users click the **Add Movie** button — that is, when the form has been posted.</span></span>

<span data-ttu-id="0421d-159">如同先前的教學課程中，您會取得文字方塊的值使用如下的運算式`Request.Form["name"]`，其中*名稱*名稱`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="0421d-159">As you saw in an earlier tutorial, you get the value of a text box by using an expression like `Request.Form["name"]`, where *name* is the name of the `<input>` element.</span></span>

<span data-ttu-id="0421d-160">變數的名稱 (`title`， `genre`，和`year`) 是任意的。</span><span class="sxs-lookup"><span data-stu-id="0421d-160">The names of the variables (`title`, `genre`, and `year`) are arbitrary.</span></span> <span data-ttu-id="0421d-161">要指派給名稱`<input>`項目，您要的任何項目呼叫。</span><span class="sxs-lookup"><span data-stu-id="0421d-161">Like the names that you assign to `<input>` elements, you can call them anything you like.</span></span> <span data-ttu-id="0421d-162">(變數的名稱不一定要比對的 name 屬性`<input>`項目表單上的。)但是如同`<input>`項目，最好使用變數的名稱，以反映它們所包含的資料。</span><span class="sxs-lookup"><span data-stu-id="0421d-162">(The names of the variables don't have to match the name attributes of `<input>` elements on the form.) But as with the `<input>` elements, it's a good idea to use variable names that reflect the data that they contain.</span></span> <span data-ttu-id="0421d-163">當您撰寫程式碼時，則 一致的名稱讓您更輕鬆地記住您正在使用哪些資料。</span><span class="sxs-lookup"><span data-stu-id="0421d-163">When you write code, consistent names make it easier for you to remember what data you're working with.</span></span>

## <a name="adding-data-to-the-database"></a><span data-ttu-id="0421d-164">將資料加入至資料庫</span><span class="sxs-lookup"><span data-stu-id="0421d-164">Adding Data to the Database</span></span>

<span data-ttu-id="0421d-165">在程式碼中封鎖您剛加入，請直接*內*右大括號 ( `}` ) 的`if`封鎖 （不只是在程式碼區塊） 內，加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0421d-165">In the code block you just added, just *inside* the closing brace ( `}` ) of the `if` block (not just inside the code block), add the following code:</span></span>

[!code-csharp[Main](entering-data/samples/sample3.cs)]

<span data-ttu-id="0421d-166">這個範例是類似於您用來擷取和顯示資料的上一個教學課程中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0421d-166">This example is similar to the code you used in a previous tutorial to fetch and display data.</span></span> <span data-ttu-id="0421d-167">開頭的行`db =`像之前，資料庫和下一行定義 SQL 陳述式，一次開啟你之前。</span><span class="sxs-lookup"><span data-stu-id="0421d-167">The line that starts with `db =` opens the database, like before, and the next line defines a SQL statement, again as you saw before.</span></span> <span data-ttu-id="0421d-168">不過，此時它會定義 SQL`Insert Into`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0421d-168">However, this time it defines a SQL `Insert Into` statement.</span></span> <span data-ttu-id="0421d-169">下列範例顯示的一般語法`Insert Into`陳述式：</span><span class="sxs-lookup"><span data-stu-id="0421d-169">The following example shows the general syntax of the `Insert Into` statement:</span></span>

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

<span data-ttu-id="0421d-170">換句話說，您指定的資料表中插入，則列出的資料行中插入，並列出要插入之值。</span><span class="sxs-lookup"><span data-stu-id="0421d-170">In other words, you specify the table to insert into, then list the columns to insert into, and then list the values to insert.</span></span> <span data-ttu-id="0421d-171">（如之前所述，SQL 是不區分大小寫，但有些人大寫的關鍵字，讓您更輕鬆地讀取命令）。</span><span class="sxs-lookup"><span data-stu-id="0421d-171">(As noted before, SQL is not case sensitive but some people capitalize the keywords to make it easier to read the command.)</span></span>

<span data-ttu-id="0421d-172">要插入的資料行已經列在命令中 — `(Title, Genre, Year)`。</span><span class="sxs-lookup"><span data-stu-id="0421d-172">The columns that you're inserting into are already listed in the command — `(Title, Genre, Year)`.</span></span> <span data-ttu-id="0421d-173">有趣的部分，就是您從文字方塊中，到取得的值`VALUES`命令的一部分。</span><span class="sxs-lookup"><span data-stu-id="0421d-173">The interesting part is how you get the values from the text boxes into the `VALUES` part of the command.</span></span> <span data-ttu-id="0421d-174">而不是實際的值，您會看到`@0`， `@1`，和`@2`，當然，這是預留位置。</span><span class="sxs-lookup"><span data-stu-id="0421d-174">Instead of actual values, you see `@0`, `@1`, and `@2`, which are of course placeholders.</span></span> <span data-ttu-id="0421d-175">當您執行命令 (在`db.Execute`列)，傳遞所得的文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="0421d-175">When you run the command (on the `db.Execute` line), you pass the values that you got from the text boxes.</span></span>

<span data-ttu-id="0421d-176">**重要！**</span><span class="sxs-lookup"><span data-stu-id="0421d-176">**Important!**</span></span> <span data-ttu-id="0421d-177">請記住，您應該永遠包含線上使用者輸入的 SQL 陳述式中的資料的唯一方式是使用預留位置，如下所示 (`VALUES(@0, @1, @2)`)。</span><span class="sxs-lookup"><span data-stu-id="0421d-177">Remember that the only way you should ever include data entered online by a user in a SQL statement is to use placeholders, as you see here (`VALUES(@0, @1, @2)`).</span></span> <span data-ttu-id="0421d-178">串連成 SQL 陳述式的使用者輸入時，如果您自行開啟 SQL 插入式攻擊，如所述[表單基本 ASP.NET Web Pages 中](https://go.microsoft.com/fwlink/?LinkId=251581)（上一個教學課程）。</span><span class="sxs-lookup"><span data-stu-id="0421d-178">If you concatenate user input into a SQL statement, you open yourself to a SQL injection attack, as explained in [Form Basics in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (the previous tutorial).</span></span>

<span data-ttu-id="0421d-179">仍內部`if`封鎖，加入下列一行之後`db.Execute`行：</span><span class="sxs-lookup"><span data-stu-id="0421d-179">Still inside the `if` block, add the following line after the `db.Execute` line:</span></span>

[!code-css[Main](entering-data/samples/sample4.css)]

<span data-ttu-id="0421d-180">新的電影已插入至資料庫之後，這一行就會將您 （重新導向） 跳到*電影*頁面上，才可看到您剛才輸入的電影。</span><span class="sxs-lookup"><span data-stu-id="0421d-180">After the new movie has been inserted into the database, this line jumps you (redirects) to the *Movies* page so you can see the movie you just entered.</span></span> <span data-ttu-id="0421d-181">`~`運算子表示 「 網站的根。 」</span><span class="sxs-lookup"><span data-stu-id="0421d-181">The `~` operator means "root of the website."</span></span> <span data-ttu-id="0421d-182">(`~`運算子只適用於不在 HTML 中的 ASP.NET 網頁通常。)</span><span class="sxs-lookup"><span data-stu-id="0421d-182">(The `~` operator works only in ASP.NET pages, not in HTML generally.)</span></span>

<span data-ttu-id="0421d-183">此範例看起來的完整程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="0421d-183">The complete code block looks like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a><span data-ttu-id="0421d-184">測試插入命令 （目前）</span><span class="sxs-lookup"><span data-stu-id="0421d-184">Testing the Insert Command (So Far)</span></span>

<span data-ttu-id="0421d-185">您未完成，但現在已測試的好時機。</span><span class="sxs-lookup"><span data-stu-id="0421d-185">You're not done yet, but now is a good time to test.</span></span>

<span data-ttu-id="0421d-186">在 WebMatrix 中的檔案樹狀結構檢視中，以滑鼠右鍵按一下*AddMovie.cshtml*頁面，然後按一下**瀏覽器中啟動**。</span><span class="sxs-lookup"><span data-stu-id="0421d-186">In the tree view of files in WebMatrix, right-click the *AddMovie.cshtml* page and then click **Launch in browser**.</span></span>

![瀏覽器中的 [新增電影] 頁面](entering-data/_static/image2.png)

<span data-ttu-id="0421d-188">(如果您得到不同的網頁瀏覽器中，請確定 URL 是`http://localhost:nnnnn/AddMovie`)，其中*nnnnn*是您所使用的連接埠號碼。)</span><span class="sxs-lookup"><span data-stu-id="0421d-188">(If you end up with a different page in the browser, make sure that the URL is `http://localhost:nnnnn/AddMovie`), where *nnnnn* is the port number that you're using.)</span></span>

<span data-ttu-id="0421d-189">您未取得錯誤網頁嗎？</span><span class="sxs-lookup"><span data-stu-id="0421d-189">Did you get an error page?</span></span> <span data-ttu-id="0421d-190">如果是的話，請仔細閱讀，並確定該程式碼看起來完全什麼先前已列出。</span><span class="sxs-lookup"><span data-stu-id="0421d-190">If so, read it carefully and make sure that the code looks exactly what was listed earlier.</span></span>

<span data-ttu-id="0421d-191">在表單中輸入電影&mdash;比方說，使用"公民 Kane"、"戲劇 」 和 「 1941"。</span><span class="sxs-lookup"><span data-stu-id="0421d-191">Enter a movie in the form &mdash; for example, use "Citizen Kane", "Drama", and "1941".</span></span> <span data-ttu-id="0421d-192">（或任何）。然後按一下 **新增電影**。</span><span class="sxs-lookup"><span data-stu-id="0421d-192">(Or whatever.) Then click **Add Movie**.</span></span>

<span data-ttu-id="0421d-193">如果一切順利，您要重新導向至*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="0421d-193">If all goes well, you're redirected to the *Movies* page.</span></span> <span data-ttu-id="0421d-194">請確定其中已列出新的電影。</span><span class="sxs-lookup"><span data-stu-id="0421d-194">Make sure that your new movie is listed.</span></span>

![顯示新的電影頁面加入影片](entering-data/_static/image3.png)

## <a name="validating-user-input"></a><span data-ttu-id="0421d-196">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="0421d-196">Validating User Input</span></span>

<span data-ttu-id="0421d-197">請返回*AddMovie*頁面上，或執行一次。</span><span class="sxs-lookup"><span data-stu-id="0421d-197">Go back to the *AddMovie* page, or run it again.</span></span> <span data-ttu-id="0421d-198">輸入另一部影片，但這次，請輸入只標題&mdash;，例如輸入"登 ' 中 Rain"。</span><span class="sxs-lookup"><span data-stu-id="0421d-198">Enter another movie, but this time, enter only the title &mdash; for example, enter "Singin' in the Rain".</span></span> <span data-ttu-id="0421d-199">然後按一下 **新增電影**。</span><span class="sxs-lookup"><span data-stu-id="0421d-199">Then click **Add Movie**.</span></span>

<span data-ttu-id="0421d-200">正在重新導向至*電影*頁面上一次。</span><span class="sxs-lookup"><span data-stu-id="0421d-200">You're redirected to the *Movies* page again.</span></span> <span data-ttu-id="0421d-201">您可以找到新的電影，但不完整。</span><span class="sxs-lookup"><span data-stu-id="0421d-201">You can find the new movie, but it's incomplete.</span></span>

![影片頁面上顯示新的電影遺失某些值，](entering-data/_static/image4.png)

<span data-ttu-id="0421d-203">當您建立*電影*資料表中，您明確地說，欄位不可以是 null。</span><span class="sxs-lookup"><span data-stu-id="0421d-203">When you created the *Movies* table, you explicitly said that none of the fields could be null.</span></span> <span data-ttu-id="0421d-204">這裡您有新的電影，項目表單時，您要將欄位保留空白。</span><span class="sxs-lookup"><span data-stu-id="0421d-204">Here you have an entry form for new movies, and you're leaving fields blank.</span></span> <span data-ttu-id="0421d-205">這是錯誤。</span><span class="sxs-lookup"><span data-stu-id="0421d-205">That's an error.</span></span>

<span data-ttu-id="0421d-206">在此情況下，並未實際引發資料庫 (或*擲回*) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="0421d-206">In this case, the database didn't actually raise (or *throw*) an error.</span></span> <span data-ttu-id="0421d-207">您沒有因此提供內容類型或年中的程式碼*AddMovie*頁面被視為這些值所謂*空白字串*。</span><span class="sxs-lookup"><span data-stu-id="0421d-207">You didn't supply a genre or year, so the code in the *AddMovie* page treated those values as so-called *empty strings*.</span></span> <span data-ttu-id="0421d-208">當 SQL`Insert Into`命令執行、 內容類型及年欄位沒有有用的資料，但不為 null。</span><span class="sxs-lookup"><span data-stu-id="0421d-208">When the SQL `Insert Into` command ran, the genre and year fields didn't have useful data in them, but they weren't null.</span></span>

<span data-ttu-id="0421d-209">很明顯地，您不想要讓使用者輸入半空白電影資訊到資料庫。</span><span class="sxs-lookup"><span data-stu-id="0421d-209">Obviously, you don't want to let users enter half-empty movie information into the database.</span></span> <span data-ttu-id="0421d-210">解決方法是驗證使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="0421d-210">The solution is to validate the user's input.</span></span> <span data-ttu-id="0421d-211">一開始，驗證將會只是確定使用者已為所有欄位都輸入的值 （也就是無一包含空字串）。</span><span class="sxs-lookup"><span data-stu-id="0421d-211">Initially, the validation will simply make sure that the user has entered a value for all of the fields (that is, that none of them contains an empty string).</span></span>

> [!TIP]
> 
> <span data-ttu-id="0421d-212">**Null 和空字串**</span><span class="sxs-lookup"><span data-stu-id="0421d-212">**Null and Empty Strings**</span></span>
> 
> <span data-ttu-id="0421d-213">在程式設計中，沒有區分不同的概念的 「 任何值 」。</span><span class="sxs-lookup"><span data-stu-id="0421d-213">In programming, there's a distinction between different notions of "no value."</span></span> <span data-ttu-id="0421d-214">值，一般是*null*如果從未已設定或以任何方式初始化。</span><span class="sxs-lookup"><span data-stu-id="0421d-214">In general, a value is *null* if it has never been set or initialized in any way.</span></span> <span data-ttu-id="0421d-215">相反地，預期字元資料 （字串） 的變數可以將*空字串*。</span><span class="sxs-lookup"><span data-stu-id="0421d-215">In contrast, a variable that expects character data (strings) can be set to an *empty string*.</span></span> <span data-ttu-id="0421d-216">在此情況下，值不是 null。它只已明確設定為字串，其長度為零的字元。</span><span class="sxs-lookup"><span data-stu-id="0421d-216">In that case, the value is not null; it's just been explicitly set to a string of characters whose length is zero.</span></span> <span data-ttu-id="0421d-217">這些兩個陳述式顯示的差異：</span><span class="sxs-lookup"><span data-stu-id="0421d-217">These two statements show the difference:</span></span>
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> <span data-ttu-id="0421d-218">這有點較為複雜，但是很重要的一點是`null`代表一種不明的狀態。</span><span class="sxs-lookup"><span data-stu-id="0421d-218">It's a little more complicated than that, but the important point is that `null` represents a sort of undetermined state.</span></span>
> 
> <span data-ttu-id="0421d-219">現在，然後務必了解剛好有值為 null 時，而只是空字串時。</span><span class="sxs-lookup"><span data-stu-id="0421d-219">Now and then it's important to understand exactly when a value is null and when it's just an empty string.</span></span> <span data-ttu-id="0421d-220">在程式碼*AddMovie*  頁面上，您收到的值的文字方塊使用`Request.Form["title"]`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="0421d-220">In the code for the *AddMovie* page, you get the values of the text boxes by using `Request.Form["title"]` and so on.</span></span> <span data-ttu-id="0421d-221">當頁面第一次執行時 （在您按一下按鈕時）、 值`Request.Form["title"]`為 null。</span><span class="sxs-lookup"><span data-stu-id="0421d-221">When the page first runs (before you click the button), the value of `Request.Form["title"]` is null.</span></span> <span data-ttu-id="0421d-222">當您送出表單，但是`Request.Form["title"]`取得的值`title`文字方塊。</span><span class="sxs-lookup"><span data-stu-id="0421d-222">But when you submit the form, `Request.Form["title"]` gets the value of the `title` text box.</span></span> <span data-ttu-id="0421d-223">並不明顯，但空白的文字方塊不是 null。它只會包含在它的空字串。</span><span class="sxs-lookup"><span data-stu-id="0421d-223">It's not obvious, but an empty text box is not null; it just has an empty string in it.</span></span> <span data-ttu-id="0421d-224">因此當程式碼執行以回應 [] 按鈕按一下，`Request.Form["title"]`中有為空字串。</span><span class="sxs-lookup"><span data-stu-id="0421d-224">So when the code runs in response to the button click, `Request.Form["title"]` has an empty string in it.</span></span>
> 
> <span data-ttu-id="0421d-225">這個區別為何如此重要？</span><span class="sxs-lookup"><span data-stu-id="0421d-225">Why is this distinction important?</span></span> <span data-ttu-id="0421d-226">當您建立*電影*資料表中，您明確地說，欄位不可以是 null。</span><span class="sxs-lookup"><span data-stu-id="0421d-226">When you created the *Movies* table, you explicitly said that none of the fields could be null.</span></span> <span data-ttu-id="0421d-227">但是這裡您有新的電影，項目表單，而且您正在將欄位保留空白。</span><span class="sxs-lookup"><span data-stu-id="0421d-227">But here you have an entry form for new movies, and you're leaving fields blank.</span></span> <span data-ttu-id="0421d-228">您會合理預期資料庫會抱怨當您嘗試要儲存新的電影是沒有內容類型或年份的值。</span><span class="sxs-lookup"><span data-stu-id="0421d-228">You would reasonably expect the database to complain when you tried to save new movies that didn't have values for genre or year.</span></span> <span data-ttu-id="0421d-229">但是，這是點&mdash;即使這些文字方塊留白，值不為 null，而空字串。</span><span class="sxs-lookup"><span data-stu-id="0421d-229">But that's the point &mdash; even if you leave those text boxes blank, the values aren't null; they're empty strings.</span></span> <span data-ttu-id="0421d-230">如此一來，您就可以將空白這些資料行的資料庫來儲存新電影&mdash;但不是 null ！</span><span class="sxs-lookup"><span data-stu-id="0421d-230">As a result, you're able to save new movies to the database with these columns empty &mdash; but not null!</span></span> <span data-ttu-id="0421d-231">&mdash; 值。</span><span class="sxs-lookup"><span data-stu-id="0421d-231">&mdash; values.</span></span> <span data-ttu-id="0421d-232">因此，您必須確定使用者不會提交空的字串，您可以先驗證使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="0421d-232">Therefore, you have to make sure that users don't submit an empty string, which you can do by validating the user's input.</span></span>


### <a name="the-validation-helper"></a><span data-ttu-id="0421d-233">驗證 Helper</span><span class="sxs-lookup"><span data-stu-id="0421d-233">The Validation Helper</span></span>

<span data-ttu-id="0421d-234">ASP.NET 網頁包括 helper &mdash; `Validation` helper&mdash;可用來確定使用者輸入符合您需求的資料。</span><span class="sxs-lookup"><span data-stu-id="0421d-234">ASP.NET Web Pages includes a helper &mdash; the `Validation` helper &mdash; that you can use to make sure that users enter data that meets your requirements.</span></span> <span data-ttu-id="0421d-235">`Validation`協助程式是其中一個內建到 ASP.NET Web Pages 中，所以您不需要使用 NuGet，您在先前的教學課程安裝 Gravatar 協助程式的方式將它安裝為封裝的協助程式。</span><span class="sxs-lookup"><span data-stu-id="0421d-235">The `Validation` helper is one of the helpers that's built in to ASP.NET Web Pages, so you don't have to install it as a package by using NuGet, the way you installed the Gravatar helper in an earlier tutorial.</span></span>

<span data-ttu-id="0421d-236">若要驗證使用者的輸入，將會執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="0421d-236">To validate the user's input, you'll do the following:</span></span>

- <span data-ttu-id="0421d-237">您可以使用程式碼，指定您想要要求在頁面上的文字方塊中的值。</span><span class="sxs-lookup"><span data-stu-id="0421d-237">Use code to specify that you want to require values in the text boxes on the page.</span></span>
- <span data-ttu-id="0421d-238">使電影資訊加入至資料庫的所有項目正確驗證時，才放置測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0421d-238">Put a test into the code so that the movie information is added to the database only if everything validates properly.</span></span>
- <span data-ttu-id="0421d-239">加入程式碼來顯示錯誤訊息的標記。</span><span class="sxs-lookup"><span data-stu-id="0421d-239">Add code into the markup to display error messages.</span></span>

<span data-ttu-id="0421d-240">中的程式碼區塊中*AddMovie*頁面上，往右上在變數宣告之前，最上方加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0421d-240">In the code block in the *AddMovie* page, right up at the top before the variable declarations, add the following code:</span></span>

[!code-csharp[Main](entering-data/samples/sample7.cs)]

<span data-ttu-id="0421d-241">您呼叫`Validation.RequireField`一次是針對每個欄位 (`<input>`項目) 您要需要項目。</span><span class="sxs-lookup"><span data-stu-id="0421d-241">You call `Validation.RequireField` once for each field (`<input>` element) where you want to require an entry.</span></span> <span data-ttu-id="0421d-242">您也可以新增自訂錯誤訊息，每個呼叫，您在這裡看到的一樣。</span><span class="sxs-lookup"><span data-stu-id="0421d-242">You can also add a custom error message for each call, like you see here.</span></span> <span data-ttu-id="0421d-243">（我們改變只是為了顯示，您可以將任何您喜歡的項目那里訊息）。</span><span class="sxs-lookup"><span data-stu-id="0421d-243">(We varied the messages just to show that you can put anything you like there.)</span></span>

<span data-ttu-id="0421d-244">如果沒有問題，您會想要避免新的電影資訊插入資料庫。</span><span class="sxs-lookup"><span data-stu-id="0421d-244">If there's a problem, you want to prevent the new movie information from being inserted into the database.</span></span> <span data-ttu-id="0421d-245">在`if(IsPost)`封鎖，請使用`&&`(邏輯 AND) 若要加入測試的另一個條件`Validation.IsValid()`。</span><span class="sxs-lookup"><span data-stu-id="0421d-245">In the `if(IsPost)` block, use `&&` (logical AND) to add another condition that tests `Validation.IsValid()`.</span></span> <span data-ttu-id="0421d-246">當您完成時，整個`if(IsPost)`區塊看起來像此程式碼：</span><span class="sxs-lookup"><span data-stu-id="0421d-246">When you're done, the whole `if(IsPost)` block looks like this code:</span></span>

[!code-csharp[Main](entering-data/samples/sample8.cs)]

<span data-ttu-id="0421d-247">如果沒有與任何使用您註冊之欄位的驗證錯誤`Validation`helper，`Validation.IsValid`方法會傳回 false。</span><span class="sxs-lookup"><span data-stu-id="0421d-247">If there's a validation error with any of the fields that you registered by using the `Validation` helper, the `Validation.IsValid` method returns false.</span></span> <span data-ttu-id="0421d-248">在此情況下，無該區塊中的程式碼會執行，並讓任何無效的電影項目會不插入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="0421d-248">And in that case, none of the code in that block will run, so no invalid movie entries will be inserted into the database.</span></span> <span data-ttu-id="0421d-249">當然您不到重新導向時*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="0421d-249">And of course you're not redirected to the *Movies* page.</span></span>

<span data-ttu-id="0421d-250">完整的程式碼區塊中，包括驗證程式碼現在看起來像此範例中：</span><span class="sxs-lookup"><span data-stu-id="0421d-250">The complete code block, including the validation code, now looks like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a><span data-ttu-id="0421d-251">顯示驗證錯誤</span><span class="sxs-lookup"><span data-stu-id="0421d-251">Displaying Validation Errors</span></span>

<span data-ttu-id="0421d-252">最後一個步驟是顯示任何錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="0421d-252">The last step is to display any error messages.</span></span> <span data-ttu-id="0421d-253">您可以顯示個別的訊息，每個驗證錯誤，或您可以顯示摘要，或兩者。</span><span class="sxs-lookup"><span data-stu-id="0421d-253">You can display individual messages for each validation error, or you can display a summary, or both.</span></span> <span data-ttu-id="0421d-254">此教學課程中，您將會執行同時，讓您能夠查看它的運作方式。</span><span class="sxs-lookup"><span data-stu-id="0421d-254">For this tutorial, you'll do both so that you can see how it works.</span></span>

<span data-ttu-id="0421d-255">每個旁邊`<input>`正在驗證的項目，請呼叫`Html.ValidationMessage`方法並將它傳遞的名稱`<input>`正在驗證的項目。</span><span class="sxs-lookup"><span data-stu-id="0421d-255">Next to each `<input>` element that you're validating, call the `Html.ValidationMessage` method and pass it the name of the `<input>` element you're validating.</span></span> <span data-ttu-id="0421d-256">您將`Html.ValidationMessage`方法正確的地方出現的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="0421d-256">You put the `Html.ValidationMessage` method right where you want the error message to appear.</span></span> <span data-ttu-id="0421d-257">當頁面執行，則`Html.ValidationMessage`方法呈現`<span>`位置驗證錯誤的項目。</span><span class="sxs-lookup"><span data-stu-id="0421d-257">When the page runs, the `Html.ValidationMessage` method renders a `<span>` element where the validation error will go.</span></span> <span data-ttu-id="0421d-258">(如果沒有發生錯誤，`<span>`項目會呈現，但在它沒有任何文字。)</span><span class="sxs-lookup"><span data-stu-id="0421d-258">(If there's no error, the `<span>` element is rendered, but there's no text in it.)</span></span>

<span data-ttu-id="0421d-259">變更頁面中的標記，使其包含`Html.ValidationMessage`每三個方法`<input>`元素在頁面上，例如此範例中：</span><span class="sxs-lookup"><span data-stu-id="0421d-259">Change the markup in the page so that it includes an `Html.ValidationMessage` method for each of the three `<input>` elements on the page, like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

<span data-ttu-id="0421d-260">若要查看摘要的運作方式，也加入下列標記和程式碼後緊接著`<h1>Add a Movie</h1>`頁面上的元素：</span><span class="sxs-lookup"><span data-stu-id="0421d-260">To see how the summary works, also add the following markup and code right after the `<h1>Add a Movie</h1>` element on the page:</span></span>

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

<span data-ttu-id="0421d-261">根據預設，`Html.ValidationSummary`方法會顯示在清單中的所有驗證訊息 (`<ul>`內的項目`<div>`項目)。</span><span class="sxs-lookup"><span data-stu-id="0421d-261">By default, the `Html.ValidationSummary` method displays all the validation messages in a list (a `<ul>` element that's inside a `<div>` element).</span></span> <span data-ttu-id="0421d-262">如同`Html.ValidationMessage`方法時，永遠會轉譯驗證摘要的標記; 如果沒有任何錯誤，會呈現任何清單項目。</span><span class="sxs-lookup"><span data-stu-id="0421d-262">As with the `Html.ValidationMessage` method, the markup for the validation summary is always rendered; if there are no errors, no list items are rendered.</span></span>

<span data-ttu-id="0421d-263">摘要可以是顯示驗證訊息，而不是使用的替代方式`Html.ValidationMessage`方法，以顯示每個欄位特有的錯誤。</span><span class="sxs-lookup"><span data-stu-id="0421d-263">The summary can be an alternative way to display validation messages instead of by using the `Html.ValidationMessage` method to display each field-specific error.</span></span> <span data-ttu-id="0421d-264">或者，您可以使用的摘要和詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0421d-264">Or you can use both a summary and the details.</span></span> <span data-ttu-id="0421d-265">您可以使用或`Html.ValidationSummary`方法來顯示一般錯誤，然後使用 個別`Html.ValidationMessage`呼叫，以顯示詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0421d-265">Or you can use the `Html.ValidationSummary` method to display a generic error and then use individual `Html.ValidationMessage` calls to display details.</span></span>

<span data-ttu-id="0421d-266">[完成] 頁面現在看起來像此範例中：</span><span class="sxs-lookup"><span data-stu-id="0421d-266">The complete page now looks like this example:</span></span>

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

<span data-ttu-id="0421d-267">就這麼容易！</span><span class="sxs-lookup"><span data-stu-id="0421d-267">That's it.</span></span> <span data-ttu-id="0421d-268">您現在可以測試頁面新增電影，但留下其中一個或多個欄位。</span><span class="sxs-lookup"><span data-stu-id="0421d-268">You can now test the page by adding a movie but leaving out one or more of the fields.</span></span> <span data-ttu-id="0421d-269">當您這樣做時，您會看到顯示下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="0421d-269">When you do, you see the following error display:</span></span>

![新增電影的畫面，顯示驗證錯誤訊息](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a><span data-ttu-id="0421d-271">設定樣式的驗證錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="0421d-271">Styling the Validation Error Messages</span></span>

<span data-ttu-id="0421d-272">您可以看到錯誤訊息，但是它們不突顯很好。</span><span class="sxs-lookup"><span data-stu-id="0421d-272">You can see that there are error messages, but they don't really stand out very well.</span></span> <span data-ttu-id="0421d-273">沒有簡單的方法，以設定樣式的錯誤訊息，不過。</span><span class="sxs-lookup"><span data-stu-id="0421d-273">There's an easy way to style the error messages, though.</span></span>

<span data-ttu-id="0421d-274">若要設定個別的錯誤訊息所顯示的樣式`Html.ValidationMessage`，建立名為的 CSS 樣式類別`field-validation-error`。</span><span class="sxs-lookup"><span data-stu-id="0421d-274">To style the individual error messages that are displayed by `Html.ValidationMessage`, create a CSS style class named `field-validation-error`.</span></span> <span data-ttu-id="0421d-275">若要定義驗證摘要尋找，建立名為的 CSS 樣式類別`validation-summary-errors`。</span><span class="sxs-lookup"><span data-stu-id="0421d-275">To define the look for the validation summary, create a CSS style class named `validation-summary-errors`.</span></span>

<span data-ttu-id="0421d-276">若要查看這項技術的運作方式，加入`<style>`元素內`<head>`頁面區段。</span><span class="sxs-lookup"><span data-stu-id="0421d-276">To see how this technique works, add a `<style>` element inside the `<head>` section of the page.</span></span> <span data-ttu-id="0421d-277">然後定義名為的樣式類別`field-validation-error`和`validation-summary-errors`，其中包含下列規則：</span><span class="sxs-lookup"><span data-stu-id="0421d-277">Then define style classes named `field-validation-error` and `validation-summary-errors` that contain the following rules:</span></span>

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

<span data-ttu-id="0421d-278">通常您可能會放置到不同的樣式資訊 *.css*檔案，但為求簡化您可以將它們放在頁面現在。</span><span class="sxs-lookup"><span data-stu-id="0421d-278">Normally you'd probably put style information into a separate *.css* file, but for simplicity you can put them in the page for now.</span></span> <span data-ttu-id="0421d-279">(稍後在此教學課程的集中，您將會移動 CSS 規則到個別 *.css*檔案。)</span><span class="sxs-lookup"><span data-stu-id="0421d-279">(Later in this tutorial set, you'll move the CSS rules to a separate *.css* file.)</span></span>

<span data-ttu-id="0421d-280">如果沒有驗證錯誤時，`Html.ValidationMessage`方法呈現`<span>`包含的項目`class="field-validation-error"`。</span><span class="sxs-lookup"><span data-stu-id="0421d-280">If there's a validation error, the `Html.ValidationMessage` method renders a `<span>` element that includes `class="field-validation-error"`.</span></span> <span data-ttu-id="0421d-281">藉由加入該類別的樣式定義，您可以設定訊息的外觀。</span><span class="sxs-lookup"><span data-stu-id="0421d-281">By adding a style definition for that class, you can configure what the message looks like.</span></span> <span data-ttu-id="0421d-282">如果發生錯誤，`ValidationSummary`方法同樣的動態呈現屬性`class="validation-summary-errors"`。</span><span class="sxs-lookup"><span data-stu-id="0421d-282">If there are errors, the `ValidationSummary` method likewise dynamically renders the attribute `class="validation-summary-errors"`.</span></span>

<span data-ttu-id="0421d-283">再次執行頁面，刻意省略幾個欄位。</span><span class="sxs-lookup"><span data-stu-id="0421d-283">Run the page again and deliberately leave out a couple of the fields.</span></span> <span data-ttu-id="0421d-284">錯誤現在是更值得注意。</span><span class="sxs-lookup"><span data-stu-id="0421d-284">The errors are now more noticeable.</span></span> <span data-ttu-id="0421d-285">（事實上，它們做得太過火，但這只是為了顯示您可以執行）。</span><span class="sxs-lookup"><span data-stu-id="0421d-285">(In fact, they're overdone, but that's just to show what you can do.)</span></span>

![新增電影的畫面，顯示驗證錯誤已設定樣式](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a><span data-ttu-id="0421d-287">將連結加入至 [影片] 頁面</span><span class="sxs-lookup"><span data-stu-id="0421d-287">Adding a Link to the Movies Page</span></span>

<span data-ttu-id="0421d-288">最後一個步驟是方便您取得*AddMovie*頁面從原始的電影清單。</span><span class="sxs-lookup"><span data-stu-id="0421d-288">One final step is to make it convenient to get to the *AddMovie* page from the original movie listing.</span></span>

<span data-ttu-id="0421d-289">開啟*電影*頁面上一次。</span><span class="sxs-lookup"><span data-stu-id="0421d-289">Open the *Movies* page again.</span></span> <span data-ttu-id="0421d-290">關閉後`</div>`標記後面`WebGrid`helper，加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="0421d-290">After the closing `</div>` tag that follows the `WebGrid` helper, add the following markup:</span></span>

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

<span data-ttu-id="0421d-291">如同之前，ASP.NET 會解譯`~`運算子作為網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="0421d-291">As you saw before, ASP.NET interprets the `~` operator as the root of the website.</span></span> <span data-ttu-id="0421d-292">您不必使用`~`運算子; 您可以使用標記`<a href="./AddMovie">Add a movie</a>`或其他方式來定義 HTML 的了解的路徑。</span><span class="sxs-lookup"><span data-stu-id="0421d-292">You don't have to use the `~` operator; you could use the markup `<a href="./AddMovie">Add a movie</a>` or some other way to define the path that HTML understands.</span></span> <span data-ttu-id="0421d-293">但是`~`運算子是良好的一般方法建立 Razor 頁面的連結時因為這會讓網站更有彈性 — 如果您將目前的頁面移至子資料夾時，連結仍將會移至*AddMovie*頁面。</span><span class="sxs-lookup"><span data-stu-id="0421d-293">But the `~` operator is a good general approach when you create links for Razor pages, because it makes the site more flexible — if you move the current page to a subfolder, the link will still go to the *AddMovie* page.</span></span> <span data-ttu-id="0421d-294">(請記住，`~`運算子僅適用於 *.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="0421d-294">(Remember that the `~` operator only works in *.cshtml* pages.</span></span> <span data-ttu-id="0421d-295">ASP.NET 了解，但它不是標準 HTML）。</span><span class="sxs-lookup"><span data-stu-id="0421d-295">ASP.NET understands it, but it's not standard HTML.)</span></span>

<span data-ttu-id="0421d-296">當您完成時，執行*電影*頁面。</span><span class="sxs-lookup"><span data-stu-id="0421d-296">When you're done, run the *Movies* page.</span></span> <span data-ttu-id="0421d-297">它看起來像此頁面：</span><span class="sxs-lookup"><span data-stu-id="0421d-297">It will look like this page:</span></span>

![新增電影的頁面連結的影片頁面](entering-data/_static/image7.png)

<span data-ttu-id="0421d-299">按一下**新增電影**連結，以確定它會移至*AddMovie*頁面。</span><span class="sxs-lookup"><span data-stu-id="0421d-299">Click the **Add a movie** link to make sure that it goes to the *AddMovie* page.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="0421d-300">接下來</span><span class="sxs-lookup"><span data-stu-id="0421d-300">Coming Up Next</span></span>

<span data-ttu-id="0421d-301">在下一個教學課程中，您將學習如何讓使用者編輯已在資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="0421d-301">In the next tutorial, you'll learn how to let users edit data that's already in the database.</span></span>

## <a name="complete-listing-for-addmovie-page"></a><span data-ttu-id="0421d-302">AddMovie 頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="0421d-302">Complete Listing for AddMovie Page</span></span>

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="0421d-303">其他資源</span><span class="sxs-lookup"><span data-stu-id="0421d-303">Additional Resources</span></span>

- [<span data-ttu-id="0421d-304">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="0421d-304">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- <span data-ttu-id="0421d-305">[SQL INSERT INTO 陳述式](http://www.w3schools.com/sql/sql_insert.asp)W3Schools 站台上</span><span class="sxs-lookup"><span data-stu-id="0421d-305">[SQL INSERT INTO Statement](http://www.w3schools.com/sql/sql_insert.asp) on the W3Schools site</span></span>
- <span data-ttu-id="0421d-306">[驗證使用者輸入，在 ASP.NET Web Pages 站台](https://go.microsoft.com/fwlink/?LinkId=253002)。</span><span class="sxs-lookup"><span data-stu-id="0421d-306">[Validating User Input in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=253002).</span></span> <span data-ttu-id="0421d-307">使用的詳細資訊`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="0421d-307">More information about working with the `Validation` helper.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0421d-308">[上一頁](form-basics.md)
> [下一頁](updating-data.md)</span><span class="sxs-lookup"><span data-stu-id="0421d-308">[Previous](form-basics.md)
[Next](updating-data.md)</span></span>
