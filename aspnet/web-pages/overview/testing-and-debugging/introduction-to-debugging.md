---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introduction to 偵錯 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 偵錯是尋找和修正錯誤字碼頁中的程序。 本章示範的一些工具和技巧可用來偵錯並 analyz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: c28d63acda6e585f4aa64f294049c1790faac850
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897501"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a><span data-ttu-id="63a69-104">Introduction to 偵錯 ASP.NET Web Pages (Razor) 站台</span><span class="sxs-lookup"><span data-stu-id="63a69-104">Introduction to Debugging ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="63a69-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="63a69-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="63a69-106">本文說明偵錯 ASP.NET Web Pages (Razor) 網站中的網頁的各種方法。</span><span class="sxs-lookup"><span data-stu-id="63a69-106">This article explains various ways to debug pages in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="63a69-107">偵錯是尋找和修正錯誤字碼頁中的程序。</span><span class="sxs-lookup"><span data-stu-id="63a69-107">Debugging is the process of finding and fixing errors in your code pages.</span></span>
> 
> <span data-ttu-id="63a69-108">**您將學習：**</span><span class="sxs-lookup"><span data-stu-id="63a69-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="63a69-109">如何顯示資訊可協助分析和偵錯網頁。</span><span class="sxs-lookup"><span data-stu-id="63a69-109">How to display information that helps analyze and debug pages.</span></span>
> - <span data-ttu-id="63a69-110">如何使用偵錯工具 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="63a69-110">How to use debugging tools in Visual Studio.</span></span>
>   
> 
> <span data-ttu-id="63a69-111">這些是發行項中所引進的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="63a69-111">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="63a69-112">`ServerInfo`協助程式。</span><span class="sxs-lookup"><span data-stu-id="63a69-112">The `ServerInfo` helper.</span></span>
> - <span data-ttu-id="63a69-113">`ObjectInfo` 協助程式。</span><span class="sxs-lookup"><span data-stu-id="63a69-113">`ObjectInfo` helper.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="63a69-114">軟體版本</span><span class="sxs-lookup"><span data-stu-id="63a69-114">Software versions</span></span>
> 
> 
> - <span data-ttu-id="63a69-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="63a69-115">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="63a69-116">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="63a69-116">Visual Studio 2013</span></span>
>   
> 
> <span data-ttu-id="63a69-117">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="63a69-117">This tutorial also works with ASP.NET Web Pages 2.</span></span> <span data-ttu-id="63a69-118">您可以使用 WebMatrix 3，但不是支援整合式偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="63a69-118">You can use WebMatrix 3 but the integrated debugger is not supported.</span></span>


<span data-ttu-id="63a69-119">疑難排解錯誤和問題，您的程式碼中的重要層面是避免發生在第一次。</span><span class="sxs-lookup"><span data-stu-id="63a69-119">An important aspect of troubleshooting errors and problems in your code is to avoid them in the first place.</span></span> <span data-ttu-id="63a69-120">您可以執行此動作將您的程式碼有可能造成錯誤的各節`try/catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="63a69-120">You can do that by putting sections of your code that are likely to cause errors into `try/catch` blocks.</span></span> <span data-ttu-id="63a69-121">如需詳細資訊，請參閱節上處理中的錯誤[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890)。</span><span class="sxs-lookup"><span data-stu-id="63a69-121">For more information, see the section on handling errors in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890).</span></span>

<span data-ttu-id="63a69-122">`ServerInfo`協助程式是一種診斷工具，可讓您裝載您的網頁之 web 伺服器環境的相關資訊的概觀。</span><span class="sxs-lookup"><span data-stu-id="63a69-122">The `ServerInfo` helper is a diagnostic tool that gives you an overview of information about the web server environment that hosts your page.</span></span> <span data-ttu-id="63a69-123">它也會顯示在瀏覽器要求網頁時，會傳送的 HTTP 要求資訊。</span><span class="sxs-lookup"><span data-stu-id="63a69-123">It also shows you HTTP request information that's sent when a browser requests the page.</span></span> <span data-ttu-id="63a69-124">`ServerInfo` Helper 會顯示目前的使用者身分識別，發出要求，瀏覽器的類型，依此類推。</span><span class="sxs-lookup"><span data-stu-id="63a69-124">The `ServerInfo` helper displays the current user identity, the type of browser that made the request, and so on.</span></span> <span data-ttu-id="63a69-125">此類資訊可協助您疑難排解常見的問題。</span><span class="sxs-lookup"><span data-stu-id="63a69-125">This kind of information can help you troubleshoot common issues.</span></span>

1. <span data-ttu-id="63a69-126">建立名為的新網頁*ServerInfo.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="63a69-126">Create a new web page named *ServerInfo.cshtml*.</span></span>
2. <span data-ttu-id="63a69-127">在頁面的結束時，只在關閉前`</body>`標記中加入`@ServerInfo.GetHtml()`:</span><span class="sxs-lookup"><span data-stu-id="63a69-127">At the end of the page, just before the closing `</body>` tag, add `@ServerInfo.GetHtml()`:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    <span data-ttu-id="63a69-128">您可以加入`ServerInfo`頁面中的任何位置的程式碼。</span><span class="sxs-lookup"><span data-stu-id="63a69-128">You can add the `ServerInfo` code anywhere in the page.</span></span> <span data-ttu-id="63a69-129">但是加入結尾會讓其輸出分開其他網頁內容，使其更容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="63a69-129">But adding it at the end will keep its output separate from your other page content, which makes it easier to read.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="63a69-130">**重要**web 網頁移到實際伺服器之前，您應該從您網頁移除任何診斷程式碼。</span><span class="sxs-lookup"><span data-stu-id="63a69-130">**Important** You should remove any diagnostic code from your web pages before you move web pages to a production server.</span></span> <span data-ttu-id="63a69-131">這適用於`ServerInfo`協助程式，以及其他診斷技術在本文中包含程式碼加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="63a69-131">This applies to the `ServerInfo` helper as well as the other diagnostic techniques in this article that involve adding code to a page.</span></span> <span data-ttu-id="63a69-132">您不想您的網站訪客在您的伺服器和類似的詳細資訊，請參閱您的伺服器名稱、 使用者名稱、 路徑的相關資訊，因為這種類型的資訊可能會被用於惡意用途的人士很有用。</span><span class="sxs-lookup"><span data-stu-id="63a69-132">You don't want your website visitors to see information about your server name, user names, paths on your server, and similar details, because this type of information might be useful to people with malicious intent.</span></span>
3. <span data-ttu-id="63a69-133">儲存頁面，並在瀏覽器中執行。</span><span class="sxs-lookup"><span data-stu-id="63a69-133">Save the page and run it in a browser.</span></span>

    ![偵錯 1](introduction-to-debugging/_static/image1.jpg)

    <span data-ttu-id="63a69-135">`ServerInfo` Helper 在頁面中顯示四個資料表的資訊：</span><span class="sxs-lookup"><span data-stu-id="63a69-135">The `ServerInfo` helper displays four tables of information in the page:</span></span>

   - <span data-ttu-id="63a69-136">伺服器組態。</span><span class="sxs-lookup"><span data-stu-id="63a69-136">Server Configuration.</span></span> <span data-ttu-id="63a69-137">本節提供裝載的 web 伺服器，包括電腦名稱、 您正在執行的 ASP.NET、 網域名稱和伺服器時間的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="63a69-137">This section provides information about the hosting web server, including computer name, the version of ASP.NET you're running, the domain name, and server time.</span></span>
   - <span data-ttu-id="63a69-138">ASP.NET 伺服器變數。</span><span class="sxs-lookup"><span data-stu-id="63a69-138">ASP.NET Server Variables.</span></span> <span data-ttu-id="63a69-139">本節提供有關許多的 HTTP 通訊協定詳細資料 （稱為 HTTP 變數） 及值是每個網頁要求的一部分。</span><span class="sxs-lookup"><span data-stu-id="63a69-139">This section provides details about the many HTTP protocol details (called HTTP variables) and values that are part of each web page request.</span></span>
   - <span data-ttu-id="63a69-140">HTTP 執行階段資訊。</span><span class="sxs-lookup"><span data-stu-id="63a69-140">HTTP Runtime Information.</span></span> <span data-ttu-id="63a69-141">本章節提供詳細的 Microsoft.NET Framework 下執行您的網頁、 路徑、 詳細資料快取，等等的新版。</span><span class="sxs-lookup"><span data-stu-id="63a69-141">This section provides details about that the version of the Microsoft .NET Framework that your web page is running under, the path, details about the cache, and so on.</span></span> <span data-ttu-id="63a69-142">(因為您了解在[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890)、 使用的 Razor 語法在 Microsoft ASP.NET web 伺服器技術，也就是根據廣泛的軟體本身建立的 ASP.NET Web Pages開發程式庫呼叫.NET Framework。）</span><span class="sxs-lookup"><span data-stu-id="63a69-142">(As you learned in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web Pages using the Razor syntax are built on Microsoft's ASP.NET web server technology, which is itself built on an extensive software development library called the .NET Framework.)</span></span>
   - <span data-ttu-id="63a69-143">環境變數。</span><span class="sxs-lookup"><span data-stu-id="63a69-143">Environment Variables.</span></span> <span data-ttu-id="63a69-144">本節提供在 web 伺服器上的所有本機的環境變數及其值的清單。</span><span class="sxs-lookup"><span data-stu-id="63a69-144">This section provides a list of all the local environment variables and their values on the web server.</span></span>

     <span data-ttu-id="63a69-145">所有伺服器及要求資訊的完整說明超出本文的範圍，但您可以看到`ServerInfo`協助程式傳回的診斷資訊很多。</span><span class="sxs-lookup"><span data-stu-id="63a69-145">A full description of all the server and request information is beyond the scope of this article, but you can see that the `ServerInfo` helper returns a lot of diagnostic information.</span></span> <span data-ttu-id="63a69-146">如需值的詳細資訊，`ServerInfo`傳回時，請參閱[辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)Microsoft TechNet 網站上和[IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="63a69-146">For more information about the values that `ServerInfo` returns, see [Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) on the Microsoft TechNet website and [IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) on the MSDN website.</span></span>

## <a name="embedding-output-expressions-to-display-page-values"></a><span data-ttu-id="63a69-147">內嵌的輸出運算式，以顯示頁面的值</span><span class="sxs-lookup"><span data-stu-id="63a69-147">Embedding Output Expressions to Display Page Values</span></span>

<span data-ttu-id="63a69-148">若要查看您的程式碼中發生的另一個方法是將輸出運算式內嵌在網頁中。</span><span class="sxs-lookup"><span data-stu-id="63a69-148">Another way to see what's happening in your code is to embed output expressions in the page.</span></span> <span data-ttu-id="63a69-149">您知道，您可以直接加入類似輸出變數的值`@myVariable`或`@(subTotal * 12)`至頁面。</span><span class="sxs-lookup"><span data-stu-id="63a69-149">As you know, you can directly output the value of a variable by adding something like `@myVariable` or `@(subTotal * 12)` to the page.</span></span> <span data-ttu-id="63a69-150">偵錯時，您可以在程式碼中將這些輸出運算式放在策略的點。</span><span class="sxs-lookup"><span data-stu-id="63a69-150">For debugging, you can place these output expressions at strategic points in your code.</span></span> <span data-ttu-id="63a69-151">這可讓您執行您的頁面時，請參閱重要變數或計算結果的值。</span><span class="sxs-lookup"><span data-stu-id="63a69-151">This enables you to see the value of key variables or the result of calculations when your page runs.</span></span> <span data-ttu-id="63a69-152">當您完成偵錯，您可以移除的運算式或排除這些註解。此程序說明使用內嵌的運算式，以協助偵錯網頁的典型方式。</span><span class="sxs-lookup"><span data-stu-id="63a69-152">When you're done debugging, you can remove the expressions or comment them out. This procedure illustrates a typical way to use embedded expressions to help debug a page.</span></span>

1. <span data-ttu-id="63a69-153">建立新的 WebMatrix 網頁名為*OutputExpression.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="63a69-153">Create a new WebMatrix page that's named *OutputExpression.cshtml*.</span></span>
2. <span data-ttu-id="63a69-154">取代內容以下列頁面：</span><span class="sxs-lookup"><span data-stu-id="63a69-154">Replace the page content with the following:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    <span data-ttu-id="63a69-155">此範例會使用`switch`陳述式來檢查值`weekday`變數，然後顯示它是根據一週的哪一天的不同的輸出訊息。</span><span class="sxs-lookup"><span data-stu-id="63a69-155">The example uses a `switch` statement to check the value of the `weekday` variable and then display a different output message depending on which day of the week it is.</span></span> <span data-ttu-id="63a69-156">在範例中，`if`區塊的第一個程式碼區塊內的任意變更的一週天數加一天到目前的週間日值。</span><span class="sxs-lookup"><span data-stu-id="63a69-156">In the example, the `if` block within the first code block arbitrarily changes the day of the week by adding one day to the current weekday value.</span></span> <span data-ttu-id="63a69-157">這是為了提供說明導入錯誤。</span><span class="sxs-lookup"><span data-stu-id="63a69-157">This is an error introduced for illustration purposes.</span></span>
3. <span data-ttu-id="63a69-158">儲存頁面，並在瀏覽器中執行。</span><span class="sxs-lookup"><span data-stu-id="63a69-158">Save the page and run it in a browser.</span></span>

    <span data-ttu-id="63a69-159">頁面會顯示錯誤星期幾的訊息。</span><span class="sxs-lookup"><span data-stu-id="63a69-159">The page displays the message for the wrong day of the week.</span></span> <span data-ttu-id="63a69-160">任何一週的星期幾實際上是，您會看到訊息一天更新版本。</span><span class="sxs-lookup"><span data-stu-id="63a69-160">Whatever day of the week it actually is, you'll see the message for one day later.</span></span> <span data-ttu-id="63a69-161">雖然在此情況下您知道為什麼訊息已關閉 （因為此程式碼刻意設定不正確的日期值），實際上通常很難了解即將在程式碼中錯誤項目。</span><span class="sxs-lookup"><span data-stu-id="63a69-161">Although in this case you know why the message is off (because the code deliberately sets the incorrect day value), in reality it's often hard to know where things are going wrong in the code.</span></span> <span data-ttu-id="63a69-162">若要偵錯，您必須找出發生什麼事的索引鍵的物件和變數值這類`weekday`。</span><span class="sxs-lookup"><span data-stu-id="63a69-162">To debug, you need to find out what's happening to the value of key objects and variables such as `weekday`.</span></span>
4. <span data-ttu-id="63a69-163">藉由將加入輸出運算式`@weekday`兩個程式碼中的註解所指出的位置中所示。</span><span class="sxs-lookup"><span data-stu-id="63a69-163">Add output expressions by inserting `@weekday` as shown in the two places indicated by comments in the code.</span></span> <span data-ttu-id="63a69-164">這些輸出運算式會在該點顯示變數的值，在執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="63a69-164">These output expressions will display the values of the variable at that point in the code execution.</span></span>

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. <span data-ttu-id="63a69-165">儲存並執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="63a69-165">Save and run the page in a browser.</span></span>

    <span data-ttu-id="63a69-166">頁面會顯示實際星期幾第一次，然後再更新一週的星期幾，這樣會產生一天，然後按一下 從產生的訊息中新增`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="63a69-166">The page displays the real day of the week first, then the updated day of the week that results from adding one day, and then the resulting message from the `switch` statement.</span></span> <span data-ttu-id="63a69-167">從兩個變數的運算式輸出 (`@weekday`) 具有天之間沒有空格，因為您沒有加入任何 HTML`<p>`輸出、 標記運算式是僅供測試。</span><span class="sxs-lookup"><span data-stu-id="63a69-167">The output from the two variable expressions (`@weekday`) has no spaces between the days because you didn't add any HTML `<p>` tags to the output; the expressions are just for testing.</span></span>

    ![偵錯-2](introduction-to-debugging/_static/image2.jpg)

    <span data-ttu-id="63a69-169">現在您可以看到的錯誤時使用。</span><span class="sxs-lookup"><span data-stu-id="63a69-169">Now you can see where the error is.</span></span> <span data-ttu-id="63a69-170">當您第一次顯示`weekday`變數在程式碼中，它會顯示正確的日期。</span><span class="sxs-lookup"><span data-stu-id="63a69-170">When you first display the `weekday` variable in the code, it shows the correct day.</span></span> <span data-ttu-id="63a69-171">當您顯示它的第二個時間之後,`if`封鎖程式碼中，日期是關閉的其中一個。</span><span class="sxs-lookup"><span data-stu-id="63a69-171">When you display it the second time, after the `if` block in the code, the day is off by one.</span></span> <span data-ttu-id="63a69-172">因此您知道發生有問題之間的週間日變數的第一個和第二個外觀。</span><span class="sxs-lookup"><span data-stu-id="63a69-172">So you know that something has happened between the first and second appearance of the weekday variable.</span></span> <span data-ttu-id="63a69-173">如果這是實際的錯誤，這種方法可協助您縮小問題的程式碼的位置。</span><span class="sxs-lookup"><span data-stu-id="63a69-173">If this were a real bug, this kind of approach would help you narrow down the location of the code that's causing the problem.</span></span>
6. <span data-ttu-id="63a69-174">修正在頁面的程式碼，藉由移除您加入兩個輸出運算式及移除的程式碼變更的一週天數。</span><span class="sxs-lookup"><span data-stu-id="63a69-174">Fix the code in the page by removing the two output expressions you added, and removing the code that changes the day of the week.</span></span> <span data-ttu-id="63a69-175">其餘的完成區塊的程式碼看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="63a69-175">The remaining, complete block of code looks like the following example:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. <span data-ttu-id="63a69-176">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="63a69-176">Run the page in a browser.</span></span> <span data-ttu-id="63a69-177">此時您會看到正確的訊息顯示實際的一週天數。</span><span class="sxs-lookup"><span data-stu-id="63a69-177">This time you see the correct message displayed for the actual day of the week.</span></span>

## <a name="using-the-objectinfo-helper-to-display-object-values"></a><span data-ttu-id="63a69-178">若要顯示物件的值使用 ObjectInfo Helper</span><span class="sxs-lookup"><span data-stu-id="63a69-178">Using the ObjectInfo Helper to Display Object Values</span></span>

<span data-ttu-id="63a69-179">`ObjectInfo` Helper 顯示類型和您傳遞給每個物件的值。</span><span class="sxs-lookup"><span data-stu-id="63a69-179">The `ObjectInfo` helper displays the type and the value of each object you pass to it.</span></span> <span data-ttu-id="63a69-180">您可以使用它來檢視變數和物件的值，在您的程式碼 （例如您未使用在上述範例中的輸出運算式），加上您所見的資料型別物件的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="63a69-180">You can use it to view the value of variables and objects in your code (like you did with output expressions in the previous example), plus you can see data type information about the object.</span></span>

1. <span data-ttu-id="63a69-181">開啟的檔案命名*OutputExpression.cshtml*您稍早建立的。</span><span class="sxs-lookup"><span data-stu-id="63a69-181">Open the file named *OutputExpression.cshtml* that you created earlier.</span></span>
2. <span data-ttu-id="63a69-182">在頁面中的所有程式碼取代為下列程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="63a69-182">Replace all code in the page with the following block of code:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. <span data-ttu-id="63a69-183">儲存並執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="63a69-183">Save and run the page in a browser.</span></span>

    ![偵錯-4](introduction-to-debugging/_static/image3.jpg)

    <span data-ttu-id="63a69-185">在此範例中， `ObjectInfo` helper 會顯示兩個項目：</span><span class="sxs-lookup"><span data-stu-id="63a69-185">In this example, the `ObjectInfo` helper displays two items:</span></span>

   - <span data-ttu-id="63a69-186">類型。</span><span class="sxs-lookup"><span data-stu-id="63a69-186">The type.</span></span> <span data-ttu-id="63a69-187">第一個變數，型別是`DayOfWeek`。</span><span class="sxs-lookup"><span data-stu-id="63a69-187">For the first variable, the type is `DayOfWeek`.</span></span> <span data-ttu-id="63a69-188">針對第二個變數的類型是`String`。</span><span class="sxs-lookup"><span data-stu-id="63a69-188">For the second variable, the type is `String`.</span></span>
   - <span data-ttu-id="63a69-189">數值。</span><span class="sxs-lookup"><span data-stu-id="63a69-189">The value.</span></span> <span data-ttu-id="63a69-190">在此情況下，因為您已經在頁面中顯示的問候語變數的值，該值會再次顯示當您將變數傳遞給`ObjectInfo`。</span><span class="sxs-lookup"><span data-stu-id="63a69-190">In this case, because you already display the value of the greeting variable in the page, the value is displayed again when you pass the variable to `ObjectInfo`.</span></span>

     <span data-ttu-id="63a69-191">對於更複雜的物件，`ObjectInfo`協助程式可以顯示的詳細資訊&#8212;基本上，它可以顯示的類型和值的所有物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="63a69-191">For more complex objects, the `ObjectInfo` helper can display more information &#8212; basically, it can display the types and values of all of an object's properties.</span></span>

## <a name="using-debugging-tools-in-visual-studio"></a><span data-ttu-id="63a69-192">使用 Visual Studio 中偵錯工具</span><span class="sxs-lookup"><span data-stu-id="63a69-192">Using Debugging Tools in Visual Studio</span></span>

<span data-ttu-id="63a69-193">如需更完整的偵錯經驗，使用 Visual Studio 2013 或免費[Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)。</span><span class="sxs-lookup"><span data-stu-id="63a69-193">For a more comprehensive debugging experience, use Visual Studio 2013 or the free [Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express).</span></span> <span data-ttu-id="63a69-194">使用 Visual Studio 中，您可以在您想要檢查的行程式碼中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="63a69-194">With Visual Studio, you can set a breakpoint in your code at the line that you want to inspect.</span></span>

![設定中斷點](introduction-to-debugging/_static/image1.png)

<span data-ttu-id="63a69-196">當您測試網站時，執行程式碼會在中斷點中止。</span><span class="sxs-lookup"><span data-stu-id="63a69-196">When you test the web site, the executing code halts at the breakpoint.</span></span>

![到達中斷點](introduction-to-debugging/_static/image2.png)

<span data-ttu-id="63a69-198">您可以檢查變數及逐步執行程式碼--逐行的目前值。</span><span class="sxs-lookup"><span data-stu-id="63a69-198">You can examine the current values of the variables, and step through the code line-by-line.</span></span>

![請參閱值](introduction-to-debugging/_static/image3.png)

<span data-ttu-id="63a69-200">如需使用整合式偵錯工具 Visual Studio 中偵錯 ASP.NET Razor 頁面資訊，請參閱[程式設計 ASP.NET Web Pages (Razor) 使用 Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)。</span><span class="sxs-lookup"><span data-stu-id="63a69-200">For information about using the integrated debugger in Visual Studio to debug ASP.NET Razor pages, see [Programming ASP.NET Web Pages (Razor) Using Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63a69-201">其他資源</span><span class="sxs-lookup"><span data-stu-id="63a69-201">Additional Resources</span></span>

- [<span data-ttu-id="63a69-202">使用 Visual Studio 程式設計 ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="63a69-202">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>](https://go.microsoft.com/fwlink/?LinkId=205854)
- <span data-ttu-id="63a69-203">[IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="63a69-203">[IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)</span></span>
- <span data-ttu-id="63a69-204">[辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)</span><span class="sxs-lookup"><span data-stu-id="63a69-204">[Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)</span></span>
