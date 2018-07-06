---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: 簡介偵錯 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 偵錯是尋找和修正錯誤，在字碼頁中的程序。 本章示範一些工具和技術可用來偵錯和分析...
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: b8a492b065902fa10d3e4c5cccd50e63ea356709
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823799"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a><span data-ttu-id="dda13-104">簡介偵錯 ASP.NET Web Pages (Razor) 網站</span><span class="sxs-lookup"><span data-stu-id="dda13-104">Introduction to Debugging ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="dda13-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dda13-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dda13-106">這篇文章會說明各種偵錯 ASP.NET Web Pages (Razor) 網站中的網頁。</span><span class="sxs-lookup"><span data-stu-id="dda13-106">This article explains various ways to debug pages in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="dda13-107">偵錯是尋找和修正錯誤，在字碼頁中的程序。</span><span class="sxs-lookup"><span data-stu-id="dda13-107">Debugging is the process of finding and fixing errors in your code pages.</span></span>
> 
> <span data-ttu-id="dda13-108">**您將學到什麼：**</span><span class="sxs-lookup"><span data-stu-id="dda13-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="dda13-109">如何顯示資訊可協助分析和偵錯網頁。</span><span class="sxs-lookup"><span data-stu-id="dda13-109">How to display information that helps analyze and debug pages.</span></span>
> - <span data-ttu-id="dda13-110">如何使用偵錯工具在 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="dda13-110">How to use debugging tools in Visual Studio.</span></span>
>   
> 
> <span data-ttu-id="dda13-111">以下是所引進的發行項 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="dda13-111">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="dda13-112">`ServerInfo`協助程式。</span><span class="sxs-lookup"><span data-stu-id="dda13-112">The `ServerInfo` helper.</span></span>
> - <span data-ttu-id="dda13-113">`ObjectInfo` 協助程式。</span><span class="sxs-lookup"><span data-stu-id="dda13-113">`ObjectInfo` helper.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="dda13-114">軟體版本</span><span class="sxs-lookup"><span data-stu-id="dda13-114">Software versions</span></span>
> 
> 
> - <span data-ttu-id="dda13-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="dda13-115">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="dda13-116">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="dda13-116">Visual Studio 2013</span></span>
>   
> 
> <span data-ttu-id="dda13-117">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="dda13-117">This tutorial also works with ASP.NET Web Pages 2.</span></span> <span data-ttu-id="dda13-118">您可以使用 WebMatrix 3，但不是支援整合式偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="dda13-118">You can use WebMatrix 3 but the integrated debugger is not supported.</span></span>


<span data-ttu-id="dda13-119">疑難排解錯誤和您的程式碼中的問題很重要的層面是在第一時間避免使用它們。</span><span class="sxs-lookup"><span data-stu-id="dda13-119">An important aspect of troubleshooting errors and problems in your code is to avoid them in the first place.</span></span> <span data-ttu-id="dda13-120">您可以這麼做將您的程式碼有可能造成錯誤的各節`try/catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="dda13-120">You can do that by putting sections of your code that are likely to cause errors into `try/catch` blocks.</span></span> <span data-ttu-id="dda13-121">如需詳細資訊，請參閱一節上的錯誤處理[ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890)。</span><span class="sxs-lookup"><span data-stu-id="dda13-121">For more information, see the section on handling errors in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890).</span></span>

<span data-ttu-id="dda13-122">`ServerInfo`協助程式是一種診斷工具，可讓您裝載您的網頁之 web 伺服器環境的相關資訊的概觀。</span><span class="sxs-lookup"><span data-stu-id="dda13-122">The `ServerInfo` helper is a diagnostic tool that gives you an overview of information about the web server environment that hosts your page.</span></span> <span data-ttu-id="dda13-123">它也會顯示您的瀏覽器要求網頁時，會傳送的 HTTP 要求資訊。</span><span class="sxs-lookup"><span data-stu-id="dda13-123">It also shows you HTTP request information that's sent when a browser requests the page.</span></span> <span data-ttu-id="dda13-124">`ServerInfo`協助程式會顯示目前的使用者身分識別，發出要求，瀏覽器的類型，依此類推。</span><span class="sxs-lookup"><span data-stu-id="dda13-124">The `ServerInfo` helper displays the current user identity, the type of browser that made the request, and so on.</span></span> <span data-ttu-id="dda13-125">這類資訊可協助您針對常見問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="dda13-125">This kind of information can help you troubleshoot common issues.</span></span>

1. <span data-ttu-id="dda13-126">建立名為的新網頁*ServerInfo.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="dda13-126">Create a new web page named *ServerInfo.cshtml*.</span></span>
2. <span data-ttu-id="dda13-127">在網頁結束時，只是在關閉前`</body>`標記中加入`@ServerInfo.GetHtml()`:</span><span class="sxs-lookup"><span data-stu-id="dda13-127">At the end of the page, just before the closing `</body>` tag, add `@ServerInfo.GetHtml()`:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    <span data-ttu-id="dda13-128">您可以新增`ServerInfo`頁面中的任何位置的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dda13-128">You can add the `ServerInfo` code anywhere in the page.</span></span> <span data-ttu-id="dda13-129">但是加入結尾會區隔其輸出與您其他的網頁內容，這會讓您更容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="dda13-129">But adding it at the end will keep its output separate from your other page content, which makes it easier to read.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="dda13-130">**重要**網頁移到生產環境伺服器之前，您應該從您的網頁移除任何診斷程式碼。</span><span class="sxs-lookup"><span data-stu-id="dda13-130">**Important** You should remove any diagnostic code from your web pages before you move web pages to a production server.</span></span> <span data-ttu-id="dda13-131">這適用於`ServerInfo`協助程式，以及其他診斷技巧這篇文章中的包含程式碼加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="dda13-131">This applies to the `ServerInfo` helper as well as the other diagnostic techniques in this article that involve adding code to a page.</span></span> <span data-ttu-id="dda13-132">因為這類資訊可能有助於不懷好意的人，您不希望您的網站訪客在您的伺服器和類似的詳細資訊，查看您的伺服器名稱、 使用者名稱、 路徑的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="dda13-132">You don't want your website visitors to see information about your server name, user names, paths on your server, and similar details, because this type of information might be useful to people with malicious intent.</span></span>
3. <span data-ttu-id="dda13-133">儲存頁面，並在瀏覽器中執行它。</span><span class="sxs-lookup"><span data-stu-id="dda13-133">Save the page and run it in a browser.</span></span>

    ![偵錯-1](introduction-to-debugging/_static/image1.jpg)

    <span data-ttu-id="dda13-135">`ServerInfo`協助程式會顯示在頁面中的四個資料表的資訊：</span><span class="sxs-lookup"><span data-stu-id="dda13-135">The `ServerInfo` helper displays four tables of information in the page:</span></span>

   - <span data-ttu-id="dda13-136">伺服器設定。</span><span class="sxs-lookup"><span data-stu-id="dda13-136">Server Configuration.</span></span> <span data-ttu-id="dda13-137">本節提供裝載的 web 伺服器，包括電腦名稱、 您正在執行的 ASP.NET、 網域名稱和伺服器時間的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="dda13-137">This section provides information about the hosting web server, including computer name, the version of ASP.NET you're running, the domain name, and server time.</span></span>
   - <span data-ttu-id="dda13-138">ASP.NET 伺服器變數。</span><span class="sxs-lookup"><span data-stu-id="dda13-138">ASP.NET Server Variables.</span></span> <span data-ttu-id="dda13-139">本節提供許多的 HTTP 通訊協定詳細資料 （稱為 HTTP 變數） 的詳細資料和值是每一個網頁要求的一部分。</span><span class="sxs-lookup"><span data-stu-id="dda13-139">This section provides details about the many HTTP protocol details (called HTTP variables) and values that are part of each web page request.</span></span>
   - <span data-ttu-id="dda13-140">HTTP 執行階段資訊。</span><span class="sxs-lookup"><span data-stu-id="dda13-140">HTTP Runtime Information.</span></span> <span data-ttu-id="dda13-141">本章節提供詳細資料，Microsoft.NET Framework 下執行您的網頁、 路徑、 快取和等等的相關詳細資料的版本。</span><span class="sxs-lookup"><span data-stu-id="dda13-141">This section provides details about that the version of the Microsoft .NET Framework that your web page is running under, the path, details about the cache, and so on.</span></span> <span data-ttu-id="dda13-142">(當您了解[ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890)、 使用的 Razor 語法技術為建置基礎 Microsoft ASP.NET web 伺服器，也就是本身上豐富的軟體建置的 ASP.NET Web Pages開發程式庫呼叫.NET Framework。）</span><span class="sxs-lookup"><span data-stu-id="dda13-142">(As you learned in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web Pages using the Razor syntax are built on Microsoft's ASP.NET web server technology, which is itself built on an extensive software development library called the .NET Framework.)</span></span>
   - <span data-ttu-id="dda13-143">環境變數。</span><span class="sxs-lookup"><span data-stu-id="dda13-143">Environment Variables.</span></span> <span data-ttu-id="dda13-144">本節提供在 web 伺服器上的所有本機環境變數及其值的清單。</span><span class="sxs-lookup"><span data-stu-id="dda13-144">This section provides a list of all the local environment variables and their values on the web server.</span></span>

     <span data-ttu-id="dda13-145">所有的伺服器和要求資訊的完整描述超出本文的範圍，但您可以看到`ServerInfo`協助程式會傳回大量的診斷資訊。</span><span class="sxs-lookup"><span data-stu-id="dda13-145">A full description of all the server and request information is beyond the scope of this article, but you can see that the `ServerInfo` helper returns a lot of diagnostic information.</span></span> <span data-ttu-id="dda13-146">如需值的詳細資訊，`ServerInfo`傳回時，請參閱[辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)Microsoft TechNet 網站上並[IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="dda13-146">For more information about the values that `ServerInfo` returns, see [Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) on the Microsoft TechNet website and [IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) on the MSDN website.</span></span>

## <a name="embedding-output-expressions-to-display-page-values"></a><span data-ttu-id="dda13-147">內嵌的輸出運算式，以顯示頁面的值</span><span class="sxs-lookup"><span data-stu-id="dda13-147">Embedding Output Expressions to Display Page Values</span></span>

<span data-ttu-id="dda13-148">若要查看您的程式碼中的情況的另一個方法是將輸出運算式內嵌在網頁中。</span><span class="sxs-lookup"><span data-stu-id="dda13-148">Another way to see what's happening in your code is to embed output expressions in the page.</span></span> <span data-ttu-id="dda13-149">如您所知，您可以藉由新增類似的內容，直接輸出變數的值`@myVariable`或`@(subTotal * 12)`至頁面。</span><span class="sxs-lookup"><span data-stu-id="dda13-149">As you know, you can directly output the value of a variable by adding something like `@myVariable` or `@(subTotal * 12)` to the page.</span></span> <span data-ttu-id="dda13-150">進行偵錯，您可以在程式碼中將這些輸出運算式放在策略性的點。</span><span class="sxs-lookup"><span data-stu-id="dda13-150">For debugging, you can place these output expressions at strategic points in your code.</span></span> <span data-ttu-id="dda13-151">這可讓您執行您的頁面時，請參閱重要變數或計算結果的值。</span><span class="sxs-lookup"><span data-stu-id="dda13-151">This enables you to see the value of key variables or the result of calculations when your page runs.</span></span> <span data-ttu-id="dda13-152">當您完成偵錯，您可以移除的運算式或註解出。此程序說明使用內嵌的運算式，以協助偵錯 頁面的典型方式。</span><span class="sxs-lookup"><span data-stu-id="dda13-152">When you're done debugging, you can remove the expressions or comment them out. This procedure illustrates a typical way to use embedded expressions to help debug a page.</span></span>

1. <span data-ttu-id="dda13-153">建立新的 WebMatrix 網頁，稱為*OutputExpression.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="dda13-153">Create a new WebMatrix page that's named *OutputExpression.cshtml*.</span></span>
2. <span data-ttu-id="dda13-154">將內容取代為下列頁面：</span><span class="sxs-lookup"><span data-stu-id="dda13-154">Replace the page content with the following:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    <span data-ttu-id="dda13-155">此範例會使用`switch`陳述式來檢查值`weekday`變數，然後顯示它是根據一週的哪一天的不同的輸出訊息。</span><span class="sxs-lookup"><span data-stu-id="dda13-155">The example uses a `switch` statement to check the value of the `weekday` variable and then display a different output message depending on which day of the week it is.</span></span> <span data-ttu-id="dda13-156">在範例中，`if`區塊的第一個程式碼區塊內任意變更的一週天數加一天到目前星期幾的值。</span><span class="sxs-lookup"><span data-stu-id="dda13-156">In the example, the `if` block within the first code block arbitrarily changes the day of the week by adding one day to the current weekday value.</span></span> <span data-ttu-id="dda13-157">這是為了方便說明導入錯誤。</span><span class="sxs-lookup"><span data-stu-id="dda13-157">This is an error introduced for illustration purposes.</span></span>
3. <span data-ttu-id="dda13-158">儲存頁面，並在瀏覽器中執行它。</span><span class="sxs-lookup"><span data-stu-id="dda13-158">Save the page and run it in a browser.</span></span>

    <span data-ttu-id="dda13-159">頁面會顯示錯誤的一天，一週的訊息。</span><span class="sxs-lookup"><span data-stu-id="dda13-159">The page displays the message for the wrong day of the week.</span></span> <span data-ttu-id="dda13-160">任何星期幾實際上是，您會看到訊息一天更新版本。</span><span class="sxs-lookup"><span data-stu-id="dda13-160">Whatever day of the week it actually is, you'll see the message for one day later.</span></span> <span data-ttu-id="dda13-161">雖然在此情況下您知道為什麼訊息已關閉 （因為此程式碼中刻意設定不正確的日期值），但實際上通常很難了解即將在程式碼中錯誤項目。</span><span class="sxs-lookup"><span data-stu-id="dda13-161">Although in this case you know why the message is off (because the code deliberately sets the incorrect day value), in reality it's often hard to know where things are going wrong in the code.</span></span> <span data-ttu-id="dda13-162">若要偵錯，您需要了解這類情況的索引鍵的物件和變數值`weekday`。</span><span class="sxs-lookup"><span data-stu-id="dda13-162">To debug, you need to find out what's happening to the value of key objects and variables such as `weekday`.</span></span>
4. <span data-ttu-id="dda13-163">新增輸出運算式插入`@weekday`程式碼中的註解來表示兩個位置中所示。</span><span class="sxs-lookup"><span data-stu-id="dda13-163">Add output expressions by inserting `@weekday` as shown in the two places indicated by comments in the code.</span></span> <span data-ttu-id="dda13-164">這些輸出運算式會在此時顯示變數的值在執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="dda13-164">These output expressions will display the values of the variable at that point in the code execution.</span></span>

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. <span data-ttu-id="dda13-165">儲存並執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="dda13-165">Save and run the page in a browser.</span></span>

    <span data-ttu-id="dda13-166">此頁面會顯示實際的一天一週的第一次，然後，會將一天，然後按一下 從產生的訊息更新星期幾`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="dda13-166">The page displays the real day of the week first, then the updated day of the week that results from adding one day, and then the resulting message from the `switch` statement.</span></span> <span data-ttu-id="dda13-167">兩個變數運算式的輸出 (`@weekday`) 有天之間沒有空格，因為您未新增任何 HTML`<p>`輸出; 標記運算式是只用於測試。</span><span class="sxs-lookup"><span data-stu-id="dda13-167">The output from the two variable expressions (`@weekday`) has no spaces between the days because you didn't add any HTML `<p>` tags to the output; the expressions are just for testing.</span></span>

    ![偵錯-2](introduction-to-debugging/_static/image2.jpg)

    <span data-ttu-id="dda13-169">現在您可以看到的錯誤時使用。</span><span class="sxs-lookup"><span data-stu-id="dda13-169">Now you can see where the error is.</span></span> <span data-ttu-id="dda13-170">當您第一次顯示`weekday`變數在程式碼，它會顯示正確的日期。</span><span class="sxs-lookup"><span data-stu-id="dda13-170">When you first display the `weekday` variable in the code, it shows the correct day.</span></span> <span data-ttu-id="dda13-171">當您顯示它的第二個的時間之後,`if`在程式碼區塊中，一天已關閉。</span><span class="sxs-lookup"><span data-stu-id="dda13-171">When you display it the second time, after the `if` block in the code, the day is off by one.</span></span> <span data-ttu-id="dda13-172">讓您了解，告訴它發生了第一個和第二個的外觀工作日變數之間。</span><span class="sxs-lookup"><span data-stu-id="dda13-172">So you know that something has happened between the first and second appearance of the weekday variable.</span></span> <span data-ttu-id="dda13-173">如果這是實際的 bug，這種方法會幫助您縮小問題的程式碼的位置。</span><span class="sxs-lookup"><span data-stu-id="dda13-173">If this were a real bug, this kind of approach would help you narrow down the location of the code that's causing the problem.</span></span>
6. <span data-ttu-id="dda13-174">在頁面中修正程式碼，請移除您已新增，兩個輸出運算式並移除變更的當週日期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dda13-174">Fix the code in the page by removing the two output expressions you added, and removing the code that changes the day of the week.</span></span> <span data-ttu-id="dda13-175">程式碼的剩餘、 完整區塊看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="dda13-175">The remaining, complete block of code looks like the following example:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. <span data-ttu-id="dda13-176">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="dda13-176">Run the page in a browser.</span></span> <span data-ttu-id="dda13-177">此時您會看到正確顯示實際的當週日期的訊息。</span><span class="sxs-lookup"><span data-stu-id="dda13-177">This time you see the correct message displayed for the actual day of the week.</span></span>

## <a name="using-the-objectinfo-helper-to-display-object-values"></a><span data-ttu-id="dda13-178">若要顯示物件的值使用 ObjectInfo Helper</span><span class="sxs-lookup"><span data-stu-id="dda13-178">Using the ObjectInfo Helper to Display Object Values</span></span>

<span data-ttu-id="dda13-179">`ObjectInfo`協助程式會顯示類型和您傳遞給它的每個物件的值。</span><span class="sxs-lookup"><span data-stu-id="dda13-179">The `ObjectInfo` helper displays the type and the value of each object you pass to it.</span></span> <span data-ttu-id="dda13-180">您可以使用它來檢視變數和物件的值，在您的程式碼 （如同在上述範例中的輸出運算式），再加上您所見的資料型別物件的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="dda13-180">You can use it to view the value of variables and objects in your code (like you did with output expressions in the previous example), plus you can see data type information about the object.</span></span>

1. <span data-ttu-id="dda13-181">開啟名為的檔案*OutputExpression.cshtml*您稍早建立。</span><span class="sxs-lookup"><span data-stu-id="dda13-181">Open the file named *OutputExpression.cshtml* that you created earlier.</span></span>
2. <span data-ttu-id="dda13-182">取代下列程式碼區塊中的頁面中的所有程式碼：</span><span class="sxs-lookup"><span data-stu-id="dda13-182">Replace all code in the page with the following block of code:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. <span data-ttu-id="dda13-183">儲存並執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="dda13-183">Save and run the page in a browser.</span></span>

    ![偵錯-4](introduction-to-debugging/_static/image3.jpg)

    <span data-ttu-id="dda13-185">在此範例中，`ObjectInfo`協助程式會顯示兩個項目：</span><span class="sxs-lookup"><span data-stu-id="dda13-185">In this example, the `ObjectInfo` helper displays two items:</span></span>

   - <span data-ttu-id="dda13-186">類型。</span><span class="sxs-lookup"><span data-stu-id="dda13-186">The type.</span></span> <span data-ttu-id="dda13-187">第一個變數中，型別是`DayOfWeek`。</span><span class="sxs-lookup"><span data-stu-id="dda13-187">For the first variable, the type is `DayOfWeek`.</span></span> <span data-ttu-id="dda13-188">第二個變數中，型別是`String`。</span><span class="sxs-lookup"><span data-stu-id="dda13-188">For the second variable, the type is `String`.</span></span>
   - <span data-ttu-id="dda13-189">數值。</span><span class="sxs-lookup"><span data-stu-id="dda13-189">The value.</span></span> <span data-ttu-id="dda13-190">在此情況下，由於您已在頁面中顯示的問候語變數的值，該值會再次顯示當您將變數傳遞給`ObjectInfo`。</span><span class="sxs-lookup"><span data-stu-id="dda13-190">In this case, because you already display the value of the greeting variable in the page, the value is displayed again when you pass the variable to `ObjectInfo`.</span></span>

     <span data-ttu-id="dda13-191">對於更複雜的物件，`ObjectInfo`協助程式可以顯示更多的資訊&#8212;基本上，它可以在此對話方塊中顯示的類型和值的所有物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="dda13-191">For more complex objects, the `ObjectInfo` helper can display more information &#8212; basically, it can display the types and values of all of an object's properties.</span></span>

## <a name="using-debugging-tools-in-visual-studio"></a><span data-ttu-id="dda13-192">使用 Visual Studio 中偵錯工具</span><span class="sxs-lookup"><span data-stu-id="dda13-192">Using Debugging Tools in Visual Studio</span></span>

<span data-ttu-id="dda13-193">如需更完整的偵錯體驗，使用 Visual Studio 2013 或免費[Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)。</span><span class="sxs-lookup"><span data-stu-id="dda13-193">For a more comprehensive debugging experience, use Visual Studio 2013 or the free [Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express).</span></span> <span data-ttu-id="dda13-194">使用 Visual Studio 中，您可以在您想要檢查的行程式碼中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="dda13-194">With Visual Studio, you can set a breakpoint in your code at the line that you want to inspect.</span></span>

![設定中斷點](introduction-to-debugging/_static/image1.png)

<span data-ttu-id="dda13-196">當您測試網站時，在中斷點暫止執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dda13-196">When you test the web site, the executing code halts at the breakpoint.</span></span>

![到達中斷點](introduction-to-debugging/_static/image2.png)

<span data-ttu-id="dda13-198">您可以檢查變數及逐步執行程式碼--行的目前值。</span><span class="sxs-lookup"><span data-stu-id="dda13-198">You can examine the current values of the variables, and step through the code line-by-line.</span></span>

![請參閱值](introduction-to-debugging/_static/image3.png)

<span data-ttu-id="dda13-200">如需在 Visual Studio 中使用整合式偵錯工具，來偵錯 ASP.NET Razor 頁面的資訊，請參閱[Programming ASP.NET Web Pages (Razor) 使用 Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)。</span><span class="sxs-lookup"><span data-stu-id="dda13-200">For information about using the integrated debugger in Visual Studio to debug ASP.NET Razor pages, see [Programming ASP.NET Web Pages (Razor) Using Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dda13-201">其他資源</span><span class="sxs-lookup"><span data-stu-id="dda13-201">Additional Resources</span></span>

- [<span data-ttu-id="dda13-202">程式設計使用 Visual Studio 的 ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="dda13-202">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>](https://go.microsoft.com/fwlink/?LinkId=205854)
- <span data-ttu-id="dda13-203">[IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="dda13-203">[IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)</span></span>
- <span data-ttu-id="dda13-204">[辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)</span><span class="sxs-lookup"><span data-stu-id="dda13-204">[Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)</span></span>
