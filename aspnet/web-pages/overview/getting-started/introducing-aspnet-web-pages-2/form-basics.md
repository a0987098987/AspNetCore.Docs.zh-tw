---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: 簡介 ASP.NET Web 網頁的 HTML 表單的基本概念 |Microsoft 文件
author: tfitzmac
description: 本教學課程會示範如何建立一個輸入的表單，以及如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。 現在您...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 6f44f74774c2fa6338524987779e15f3940d1830
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898917"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a><span data-ttu-id="680fc-104">簡介 ASP.NET Web 網頁的 HTML 表單的基本概念</span><span class="sxs-lookup"><span data-stu-id="680fc-104">Introducing ASP.NET Web Pages - HTML Form Basics</span></span>
====================
<span data-ttu-id="680fc-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="680fc-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="680fc-106">本教學課程會示範如何建立一個輸入的表單，以及如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="680fc-106">This tutorial shows you the basics of how to create an input form and how to handle the user's input when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="680fc-107">以及，既然了資料庫，請您將使用表單技術，讓使用者在資料庫中找到特定的影片。</span><span class="sxs-lookup"><span data-stu-id="680fc-107">And now that you've got a database, you'll use your form skills to let users find specific movies in the database.</span></span> <span data-ttu-id="680fc-108">它會假設您已完成透過數列[簡介來顯示資料使用 ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)。</span><span class="sxs-lookup"><span data-stu-id="680fc-108">It assumes you have completed the series through [Introduction to Displaying Data Using ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).</span></span>
> 
> <span data-ttu-id="680fc-109">您將學習：</span><span class="sxs-lookup"><span data-stu-id="680fc-109">What you'll learn:</span></span>
> 
> - <span data-ttu-id="680fc-110">如何建立使用標準 HTML 項目表單。</span><span class="sxs-lookup"><span data-stu-id="680fc-110">How to create a form by using standard HTML elements.</span></span>
> - <span data-ttu-id="680fc-111">如何讀取使用者的輸入表單中。</span><span class="sxs-lookup"><span data-stu-id="680fc-111">How to read the user's input in a form.</span></span>
> - <span data-ttu-id="680fc-112">提供如何建立 SQL 查詢，選擇性地取得資料所使用的搜尋詞彙的使用者。</span><span class="sxs-lookup"><span data-stu-id="680fc-112">How to create a SQL query that selectively gets data by using a search term that the user supplies.</span></span>
> - <span data-ttu-id="680fc-113">如何在頁面中 [記住] 使用者的輸入欄位。</span><span class="sxs-lookup"><span data-stu-id="680fc-113">How to have fields in the page "remember" what the user entered.</span></span>
>   
> 
> <span data-ttu-id="680fc-114">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="680fc-114">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="680fc-115">`Request` 物件。</span><span class="sxs-lookup"><span data-stu-id="680fc-115">The `Request` object.</span></span>
> - <span data-ttu-id="680fc-116">SQL`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="680fc-116">The SQL `Where` clause.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="680fc-117">您將建置</span><span class="sxs-lookup"><span data-stu-id="680fc-117">What You'll Build</span></span>

<span data-ttu-id="680fc-118">在上一個教學課程中，您建立資料庫、 加入資料，並接著使用`WebGrid`helper 來顯示資料。</span><span class="sxs-lookup"><span data-stu-id="680fc-118">In the previous tutorial, you created a database, added data to it, and then used the `WebGrid` helper to display the data.</span></span> <span data-ttu-id="680fc-119">在本教學課程中，您將新增搜尋方塊，可讓您尋找特定類型的電影，或其標題包含您輸入任何文字。</span><span class="sxs-lookup"><span data-stu-id="680fc-119">In this tutorial, you'll add a search box that lets you find movies of a specific genre or whose title contains whatever word you enter.</span></span> <span data-ttu-id="680fc-120">（例如，您可以尋找內容類型為 「 動作 」 或其標題包含"Harry"Adventure"。 所有的影片）</span><span class="sxs-lookup"><span data-stu-id="680fc-120">(For example, you'll be able to find all movies whose genre is "Action" or whose title contains "Harry" or "Adventure.")</span></span>

<span data-ttu-id="680fc-121">當您完成本教學課程時，則必須與下列類似的頁面：</span><span class="sxs-lookup"><span data-stu-id="680fc-121">When you're done with this tutorial, you'll have a page like this one:</span></span>

![影片內容類型和標題的搜尋頁面](form-basics/_static/image1.png)

<span data-ttu-id="680fc-123">頁面的清單部分是與上一個教學課程中的相同&mdash;方格。</span><span class="sxs-lookup"><span data-stu-id="680fc-123">The listing part of the page is the same as in the last tutorial &mdash; a grid.</span></span> <span data-ttu-id="680fc-124">差異將會確認此方格會顯示只有電影您搜尋的。</span><span class="sxs-lookup"><span data-stu-id="680fc-124">The difference will be that the grid will show only the movies that you searched for.</span></span>

## <a name="about-html-forms"></a><span data-ttu-id="680fc-125">關於 HTML 表單</span><span class="sxs-lookup"><span data-stu-id="680fc-125">About HTML Forms</span></span>

<span data-ttu-id="680fc-126">(如果您有建立 HTML 表單與之間差異的經驗`GET`和`POST`，您可以略過這一節。)</span><span class="sxs-lookup"><span data-stu-id="680fc-126">(If you've got experience with creating HTML forms and with the difference between `GET` and `POST`, you can skip this section.)</span></span>

<span data-ttu-id="680fc-127">表單具有使用者輸入項目的&mdash;文字方塊、 按鈕、 選項按鈕、 核取方塊、 下拉式清單等等。</span><span class="sxs-lookup"><span data-stu-id="680fc-127">A form has user input elements &mdash; text boxes, buttons, radio buttons, check boxes, drop-down lists, and so on.</span></span> <span data-ttu-id="680fc-128">使用者填入這些控制項或進行選擇，然後按一下按鈕來送出表單。</span><span class="sxs-lookup"><span data-stu-id="680fc-128">Users fill in these controls or make selections and then submit the form by clicking a button.</span></span>

<span data-ttu-id="680fc-129">此範例所說明的表單的基本 HTML 語法：</span><span class="sxs-lookup"><span data-stu-id="680fc-129">The basic HTML syntax of a form is illustrated by this example:</span></span>

[!code-html[Main](form-basics/samples/sample1.html)]

<span data-ttu-id="680fc-130">在頁面中，執行此標記，它會建立簡單的表單看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="680fc-130">When this markup runs in a page, it creates a simple form that looks like this illustration:</span></span>

![基本的 HTML 表單，做為呈現在瀏覽器](form-basics/_static/image2.png)

<span data-ttu-id="680fc-132">`<form>`元素會括住提交的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="680fc-132">The `<form>` element encloses HTML elements to be submitted.</span></span> <span data-ttu-id="680fc-133">(進行簡單的錯誤是將項目加入至頁面，但忘記放在`<form>`項目。</span><span class="sxs-lookup"><span data-stu-id="680fc-133">(An easy mistake to make is to add elements to the page but then forget to put them inside a `<form>` element.</span></span> <span data-ttu-id="680fc-134">在此情況下，執行任何動作提交。）`method`屬性會告知瀏覽器如何提交使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="680fc-134">In that case, nothing is submitted.) The `method` attribute tells the browser how to submit the user input.</span></span> <span data-ttu-id="680fc-135">將此設`post`如果您將執行的更新，在伺服器上，或者`get`如果您只從伺服器擷取資料。</span><span class="sxs-lookup"><span data-stu-id="680fc-135">You set this to `post` if you're performing an update on the server or to `get` if you're just fetching data from the server.</span></span>

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> <span data-ttu-id="680fc-136">**GET、 POST 和 HTTP 動詞命令安全性**</span><span class="sxs-lookup"><span data-stu-id="680fc-136">**GET, POST, and HTTP Verb Safety**</span></span>
> 
> <span data-ttu-id="680fc-137">HTTP，瀏覽器和伺服器用來交換資訊的通訊協定是非常簡單，其基本作業。</span><span class="sxs-lookup"><span data-stu-id="680fc-137">HTTP, the protocol that browsers and servers use to exchange information, is remarkably simple in its basic operations.</span></span> <span data-ttu-id="680fc-138">瀏覽器會使用只有少數的動詞命令，對伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="680fc-138">Browsers use only a few verbs to make requests to servers.</span></span> <span data-ttu-id="680fc-139">當您撰寫的 web 的程式碼時，最好先了解這些動詞命令，以及它們如何使用瀏覽器和伺服器。</span><span class="sxs-lookup"><span data-stu-id="680fc-139">When you write code for the web, it's helpful to understand these verbs and how the browser and server use them.</span></span> <span data-ttu-id="680fc-140">毫無疑問，最常使用的動詞命令如下：</span><span class="sxs-lookup"><span data-stu-id="680fc-140">Far and away the most commonly used verbs are these:</span></span>
> 
> - <span data-ttu-id="680fc-141">`GET`.</span><span class="sxs-lookup"><span data-stu-id="680fc-141">`GET`.</span></span> <span data-ttu-id="680fc-142">瀏覽器會使用此動詞命令，從伺服器擷取的項目。</span><span class="sxs-lookup"><span data-stu-id="680fc-142">The browser uses this verb to fetch something from the server.</span></span> <span data-ttu-id="680fc-143">例如，當您將 URL 輸入瀏覽器，瀏覽器執行`GET`作業要求您要的頁面。</span><span class="sxs-lookup"><span data-stu-id="680fc-143">For example, when you type a URL into your browser, the browser performs a `GET` operation to request the page you want.</span></span> <span data-ttu-id="680fc-144">如果此頁面包含圖形、 瀏覽器就會執行其他`GET`取得映像的作業。</span><span class="sxs-lookup"><span data-stu-id="680fc-144">If the page includes graphics, the browser performs additional `GET` operations to get the images.</span></span> <span data-ttu-id="680fc-145">如果`GET`作業已將資訊傳遞至伺服器，將資訊傳遞做為查詢字串中的 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="680fc-145">If the `GET` operation has to pass information to the server, the information is passed as part of the URL in the query string.</span></span>
> - <span data-ttu-id="680fc-146">`POST`.</span><span class="sxs-lookup"><span data-stu-id="680fc-146">`POST`.</span></span> <span data-ttu-id="680fc-147">瀏覽器傳送`POST`要求才能送出要新增或變更伺服器上的資料。</span><span class="sxs-lookup"><span data-stu-id="680fc-147">The browser sends a `POST` request in order to submit data to be added or changed on the server.</span></span> <span data-ttu-id="680fc-148">例如，`POST`動詞命令用來在資料庫中建立記錄，或變更現有的。</span><span class="sxs-lookup"><span data-stu-id="680fc-148">For example, the `POST` verb is used to create records in a database or change existing ones.</span></span> <span data-ttu-id="680fc-149">大部分的情況下，當您填寫表單，然後按一下 [提交] 按鈕，瀏覽器執行`POST`作業。</span><span class="sxs-lookup"><span data-stu-id="680fc-149">Most of the time, when you fill in a form and click the submit button, the browser performs a `POST` operation.</span></span> <span data-ttu-id="680fc-150">在`POST`作業，正在傳遞至伺服器的資料是在頁面主體。</span><span class="sxs-lookup"><span data-stu-id="680fc-150">In a `POST` operation, the data being passed to the server is in the body of the page.</span></span>
> 
> <span data-ttu-id="680fc-151">這些動詞命令中重要的區別在於`GET`作業不會變更伺服器上的任何項目，或將其置於稍微更抽象的方式，`GET`作業並不會導致在伺服器上的狀態變更。</span><span class="sxs-lookup"><span data-stu-id="680fc-151">An important distinction between these verbs is that a `GET` operation is not supposed to change anything on the server — or to put it in a slightly more abstract way, a `GET` operation does not result in a change in state on the server.</span></span> <span data-ttu-id="680fc-152">您可以執行`GET`依需求多次，也不會變更這些資源相同的資源上的作業。</span><span class="sxs-lookup"><span data-stu-id="680fc-152">You can perform a `GET` operation on the same resources as many times as you like, and those resources don't change.</span></span> <span data-ttu-id="680fc-153">(A`GET`作業通常稱為 「 安全 」，或使用技術的詞彙，是*等冪*。)相較之下，當然`POST`要求變更在伺服器上的項目每次執行作業。</span><span class="sxs-lookup"><span data-stu-id="680fc-153">(A `GET` operation is often said to be "safe," or to use a technical term, is *idempotent*.) In contrast, of course, a `POST` request changes something on the server each time you perform the operation.</span></span>
> 
> <span data-ttu-id="680fc-154">兩個範例將說明這種區別。</span><span class="sxs-lookup"><span data-stu-id="680fc-154">Two examples will help illustrate this distinction.</span></span> <span data-ttu-id="680fc-155">當您執行搜尋時使用例如 Bing 或 Google、 引擎您填寫表單包含一個文字方塊中，，然後按一下 [搜尋] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="680fc-155">When you perform a search using an engine like Bing or Google, you fill in a form that consists of one text box, and then you click the search button.</span></span> <span data-ttu-id="680fc-156">瀏覽器執行`GET`作業，並具有您做為 URL 的一部分傳遞的方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-156">The browser performs a `GET` operation, with the value you entered into the box passed as part of the URL.</span></span> <span data-ttu-id="680fc-157">使用`GET`作業因為搜尋作業不會變更伺服器上的任何資源，為沒問題，這種類型的表單，它只會擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="680fc-157">Using a `GET` operation for this type of form is fine, because a search operation doesn't change any resources on the server, it just fetches information.</span></span>
> 
> <span data-ttu-id="680fc-158">現在請試想排序東西的程序。</span><span class="sxs-lookup"><span data-stu-id="680fc-158">Now consider the process of ordering something online.</span></span> <span data-ttu-id="680fc-159">您填寫訂單詳細資料，然後按一下 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="680fc-159">You fill in the order details and then click the submit button.</span></span> <span data-ttu-id="680fc-160">這項作業將會`POST`要求，因為作業會導致在伺服器上，例如新的訂單記錄、 變更您的帳戶資訊，以及可能是其他多項變更的變更。</span><span class="sxs-lookup"><span data-stu-id="680fc-160">This operation will be a `POST` request, because the operation will result in changes on the server, such as a new order record, a change in your account information, and perhaps many other changes.</span></span> <span data-ttu-id="680fc-161">不同於`GET`不能重複作業，您`POST`要求 — 如果您這樣做，每次您重新提交要求，您會產生新的順序，在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="680fc-161">Unlike the `GET` operation, you cannot repeat your `POST` request — if you did, each time you resubmitted the request, you'd generate a new order on the server.</span></span> <span data-ttu-id="680fc-162">（在這種情況下，網站通常會警告您不必一次以上，按一下 [提交] 按鈕或將停用 [提交] 按鈕，使您不小心重新提交表單）。</span><span class="sxs-lookup"><span data-stu-id="680fc-162">(In cases like this, websites will often warn you not to click a submit button more than once, or will disable the submit button so that you don't resubmit the form accidentally.)</span></span>
> 
> <span data-ttu-id="680fc-163">在此教學課程中，您將使用兩者`GET`作業和`POST`作業來處理 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="680fc-163">In the course of this tutorial, you'll use both a `GET` operation and a `POST` operation to work with HTML forms.</span></span> <span data-ttu-id="680fc-164">我們將說明在每個案例為何您使用的動詞命令是適當的一個。</span><span class="sxs-lookup"><span data-stu-id="680fc-164">We'll explain in each case why the verb you use is the appropriate one.</span></span>
> 
> <span data-ttu-id="680fc-165">(若要深入了解 HTTP 動詞命令，請參閱[方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的發行項。)</span><span class="sxs-lookup"><span data-stu-id="680fc-165">(To learn more about HTTP verbs, see the [Method Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article on the W3C site.)</span></span>


<span data-ttu-id="680fc-166">大部分的使用者輸入項目會以 HTML`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="680fc-166">Most user input elements are HTML `<input>` elements.</span></span> <span data-ttu-id="680fc-167">它們看起來像是`<input type="type" name="name">,`其中*類型*表示您想要的使用者輸入控制項的類型。</span><span class="sxs-lookup"><span data-stu-id="680fc-167">They look like `<input type="type" name="name">,` where *type* indicates the kind of user input control you want.</span></span> <span data-ttu-id="680fc-168">這些項目是常見的：</span><span class="sxs-lookup"><span data-stu-id="680fc-168">These elements are the common ones:</span></span>

- <span data-ttu-id="680fc-169">文字方塊中： `<input type="text">`</span><span class="sxs-lookup"><span data-stu-id="680fc-169">Text box: `<input type="text">`</span></span>
- <span data-ttu-id="680fc-170">核取方塊： `<input type="check">`</span><span class="sxs-lookup"><span data-stu-id="680fc-170">Check box: `<input type="check">`</span></span>
- <span data-ttu-id="680fc-171">選項按鈕： `<input type="radio">`</span><span class="sxs-lookup"><span data-stu-id="680fc-171">Radio button: `<input type="radio">`</span></span>
- <span data-ttu-id="680fc-172">按鈕： `<input type="button">`</span><span class="sxs-lookup"><span data-stu-id="680fc-172">Button: `<input type="button">`</span></span>
- <span data-ttu-id="680fc-173">送出按鈕： `<input type="submit">`</span><span class="sxs-lookup"><span data-stu-id="680fc-173">Submit button: `<input type="submit">`</span></span>

<span data-ttu-id="680fc-174">您也可以使用`<textarea>`項目來建立多行文字方塊和`<select>`建立下拉式清單或可捲動的清單項目。</span><span class="sxs-lookup"><span data-stu-id="680fc-174">You can also use the `<textarea>` element to create a multiline text box and the `<select>` element to create a drop-down list or scrollable list.</span></span> <span data-ttu-id="680fc-175">(如需有關 HTML 表單項目，請參閱[HTML 表單和輸入](http://www.w3schools.com/html/html_forms.asp)W3Schools 站台上。)</span><span class="sxs-lookup"><span data-stu-id="680fc-175">(For more about HTML form elements, see [HTML Forms and Input](http://www.w3schools.com/html/html_forms.asp) on the W3Schools site.)</span></span>

<span data-ttu-id="680fc-176">`name`屬性是很重要，因為名稱就是將取得的項目更新版本中，值的方式不久，您會看到。</span><span class="sxs-lookup"><span data-stu-id="680fc-176">The `name` attribute is very important, because the name is how you'll get the value of the element later, as you'll see shortly.</span></span>

<span data-ttu-id="680fc-177">有趣的部分是網頁開發人員，您將使用者輸入具有執行的動作。</span><span class="sxs-lookup"><span data-stu-id="680fc-177">The interesting part is what you, the page developer, do with the user's input.</span></span> <span data-ttu-id="680fc-178">沒有任何與這些項目相關聯的內建行為。</span><span class="sxs-lookup"><span data-stu-id="680fc-178">There's no built-in behavior associated with these elements.</span></span> <span data-ttu-id="680fc-179">相反地，您必須取得使用者輸入或選取的值，並運用它們。</span><span class="sxs-lookup"><span data-stu-id="680fc-179">Instead, you have to get the values that the user has entered or selected and do something with them.</span></span> <span data-ttu-id="680fc-180">這是您將在本教學課程中學習。</span><span class="sxs-lookup"><span data-stu-id="680fc-180">That's what you'll learn in this tutorial.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="680fc-181">**HTML5 和輸入的表單**</span><span class="sxs-lookup"><span data-stu-id="680fc-181">**HTML5 and Input Forms**</span></span>
> 
> <span data-ttu-id="680fc-182">您可能知道，HTML 是轉換中，並最新版本 (HTML5) 包含支援更直覺的方式，讓使用者輸入資訊。</span><span class="sxs-lookup"><span data-stu-id="680fc-182">As you might know, HTML is in transition and the latest version (HTML5) includes support for more intuitive ways for users to enter information.</span></span> <span data-ttu-id="680fc-183">比方說，html5 格式，您 （網頁開發人員） 可以通知網頁您想讓使用者輸入的日期。</span><span class="sxs-lookup"><span data-stu-id="680fc-183">For example, in HTML5, you (the page developer) can tell the page that you want the user to enter a date.</span></span> <span data-ttu-id="680fc-184">行事曆，而不是需要使用者手動輸入日期，然後可以自動顯示瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="680fc-184">The browser can then automatically display a calendar rather than requiring the user to enter a date manually.</span></span> <span data-ttu-id="680fc-185">不過，HTML5 新，而且尚不支援在所有瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="680fc-185">However, HTML5 is new and is not supported in all browsers yet.</span></span>
> 
> <span data-ttu-id="680fc-186">ASP.NET Web Pages 支援 HTML5 輸入使用者的瀏覽器未確認。</span><span class="sxs-lookup"><span data-stu-id="680fc-186">ASP.NET Web Pages supports HTML5 input to the extent that the user's browser does.</span></span> <span data-ttu-id="680fc-187">以新屬性的了解`<input>`元素，html5 格式，請參閱[HTML&lt;輸入&gt;輸入屬性](http://www.w3schools.com/html/html_form_input_types.asp)W3Schools 站台上。</span><span class="sxs-lookup"><span data-stu-id="680fc-187">For an idea of the new attributes for the `<input>` element in HTML5, see [HTML &lt;input&gt; type Attribute](http://www.w3schools.com/html/html_form_input_types.asp) on the W3Schools site.</span></span>


## <a name="creating-the-form"></a><span data-ttu-id="680fc-188">建立表單</span><span class="sxs-lookup"><span data-stu-id="680fc-188">Creating the Form</span></span>

<span data-ttu-id="680fc-189">在 WebMatrix 中，在**檔案**工作區中，開啟*Movies.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="680fc-189">In WebMatrix, in the **Files** workspace, open the *Movies.cshtml* page.</span></span>

<span data-ttu-id="680fc-190">關閉後`</h1>`標記和開頭之前`<div>`標記`grid.GetHtml`呼叫中，加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="680fc-190">After the closing `</h1>` tag and before the opening `<div>` tag of the `grid.GetHtml` call, add the following markup:</span></span>

[!code-html[Main](form-basics/samples/sample2.html)]

<span data-ttu-id="680fc-191">這個標記會建立名為文字方塊的表單`searchGenre`及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="680fc-191">This markup creates a form that has a text box named `searchGenre` and a submit button.</span></span> <span data-ttu-id="680fc-192">文字並提交 按鈕會括住`<form>`項目其`method`屬性設為`get`。</span><span class="sxs-lookup"><span data-stu-id="680fc-192">The text box and submit button are enclosed in a `<form>` element whose `method` attribute is set to `get`.</span></span> <span data-ttu-id="680fc-193">(請記住，如果沒有將放在文字方塊中，然後送出按鈕內`<form>`項目，當您按一下按鈕會提交做任何動作。)您使用`GET`動詞這裡因為您要建立表單，不會進行任何變更在伺服器上，則只會導致搜尋。</span><span class="sxs-lookup"><span data-stu-id="680fc-193">(Remember that if you don't put the text box and submit button inside a `<form>` element, nothing will be submitted when you click the button.) You use the `GET` verb here because you're creating a form that does not make any changes on the server — it just results in a search.</span></span> <span data-ttu-id="680fc-194">(在上一個教學課程中，您已經使用`post`方法，這是如何您將變更提交至伺服器。</span><span class="sxs-lookup"><span data-stu-id="680fc-194">(In the previous tutorial, you used a `post` method, which is how you submit changes to the server.</span></span> <span data-ttu-id="680fc-195">您會看到下一個教學課程中一次。）</span><span class="sxs-lookup"><span data-stu-id="680fc-195">You'll see that in the next tutorial again.)</span></span>

<span data-ttu-id="680fc-196">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="680fc-196">Run the page.</span></span> <span data-ttu-id="680fc-197">您尚未定義任何形式的行為，雖然您可以看到它的外觀：</span><span class="sxs-lookup"><span data-stu-id="680fc-197">Although you haven't defined any behavior for the form, you can see what it looks like:</span></span>

![「 內容類型的 [搜尋] 方塊的影片頁面](form-basics/_static/image3.png)

<span data-ttu-id="680fc-199">輸入的值，在文字方塊中，像是 「 喜劇。 」</span><span class="sxs-lookup"><span data-stu-id="680fc-199">Enter a value into the text box, like "Comedy."</span></span> <span data-ttu-id="680fc-200">然後按一下 **搜尋內容類型**。</span><span class="sxs-lookup"><span data-stu-id="680fc-200">Then click **Search Genre**.</span></span>

<span data-ttu-id="680fc-201">記下的網頁 URL。</span><span class="sxs-lookup"><span data-stu-id="680fc-201">Take note of the URL of the page.</span></span> <span data-ttu-id="680fc-202">因為您將設定`<form>`項目的`method`屬性`get`，您輸入的值現在是在 URL 中，像這樣的查詢字串的一部分：</span><span class="sxs-lookup"><span data-stu-id="680fc-202">Because you set the `<form>` element's `method` attribute to `get`, the value you entered is now part of the query string in the URL, like this:</span></span>

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a><span data-ttu-id="680fc-203">讀取表單值</span><span class="sxs-lookup"><span data-stu-id="680fc-203">Reading Form Values</span></span>

<span data-ttu-id="680fc-204">此頁面已包含一些程式碼，取得資料庫資料，並將結果顯示在方格中。</span><span class="sxs-lookup"><span data-stu-id="680fc-204">The page already contains some code that gets database data and displays the results in a grid.</span></span> <span data-ttu-id="680fc-205">現在，您必須加入讀取文字方塊的值，因此您可以執行 SQL 查詢，以包含搜尋詞彙的一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="680fc-205">Now you have to add some code that reads the value of the text box so you can run a SQL query that includes the search term.</span></span>

<span data-ttu-id="680fc-206">因為您設定表單的方法為`get`，您可以閱讀使用類似下列程式碼到文字方塊中輸入的值：</span><span class="sxs-lookup"><span data-stu-id="680fc-206">Because you set the form's method to `get`, you can read the value that was entered into the text box by using code like the following:</span></span>

`var searchTerm = Request.QueryString["searchGenre"];`

<span data-ttu-id="680fc-207">`Request.QueryString`物件 (`QueryString`屬性`Request`物件) 包含已送出的項目值`GET`作業。</span><span class="sxs-lookup"><span data-stu-id="680fc-207">The `Request.QueryString` object (the `QueryString` property of the `Request` object) includes the values of elements that were submitted as part of the `GET` operation.</span></span> <span data-ttu-id="680fc-208">`Request.QueryString`屬性包含*集合*（清單） 會提交表單中的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-208">The `Request.QueryString` property contains a *collection* (a list) of the values that are submitted in the form.</span></span> <span data-ttu-id="680fc-209">若要取得任何個別值，您可以指定您想要的項目名稱。</span><span class="sxs-lookup"><span data-stu-id="680fc-209">To get any individual value, you specify the name of the element that you want.</span></span> <span data-ttu-id="680fc-210">這就是為什麼您必須擁有`name`屬性`<input>`元素 (`searchTerm`) 會建立在文字方塊。</span><span class="sxs-lookup"><span data-stu-id="680fc-210">That's why you have to have a `name` attribute on the `<input>` element (`searchTerm`) that creates the text box.</span></span> <span data-ttu-id="680fc-211">(如需詳細資訊`Request`物件，請參閱[資訊看板](#BKMK_TheRequestObject)更新版本。)</span><span class="sxs-lookup"><span data-stu-id="680fc-211">(For more about the `Request` object, see the [sidebar](#BKMK_TheRequestObject) later.)</span></span>

<span data-ttu-id="680fc-212">它是簡單讀取文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-212">It's simple enough to read the value of the text box.</span></span> <span data-ttu-id="680fc-213">但是，如果使用者未輸入任何項目完全在文字方塊中，但已按下**搜尋**，忽略按一下的因為沒有要搜尋項目。</span><span class="sxs-lookup"><span data-stu-id="680fc-213">But if the user didn't enter anything at all in the text box but clicked **Search** anyway, you can ignore that click, since there's nothing to search.</span></span>

<span data-ttu-id="680fc-214">下列程式碼是範例，示範如何實作這些條件。</span><span class="sxs-lookup"><span data-stu-id="680fc-214">The following code is an example that shows how to implement these conditions.</span></span> <span data-ttu-id="680fc-215">（您不需要尚未加入此程式碼，您將會立即執行的）。</span><span class="sxs-lookup"><span data-stu-id="680fc-215">(You don't have to add this code yet; you'll do that in a moment.)</span></span>

[!code-csharp[Main](form-basics/samples/sample3.cs)]

<span data-ttu-id="680fc-216">測試細分以這種方式：</span><span class="sxs-lookup"><span data-stu-id="680fc-216">The test breaks down in this way:</span></span>

- <span data-ttu-id="680fc-217">取得值的`Request.QueryString["searchGenre"]`，也就是輸入的值`<input>`名為項目`searchGenre`。</span><span class="sxs-lookup"><span data-stu-id="680fc-217">Get the value of `Request.QueryString["searchGenre"]`, namely the value that was entered into the `<input>` element named `searchGenre`.</span></span>
- <span data-ttu-id="680fc-218">找出是否為空白使用`IsEmpty`方法。</span><span class="sxs-lookup"><span data-stu-id="680fc-218">Find out if it's empty by using the `IsEmpty` method.</span></span> <span data-ttu-id="680fc-219">這個方法是以判斷項目 （例如，表單項目） 是否包含值的標準方式。</span><span class="sxs-lookup"><span data-stu-id="680fc-219">This method is the standard way to determine whether something (for example, a form element) contains a value.</span></span> <span data-ttu-id="680fc-220">但是，您不在乎只有當*不*空的因此...</span><span class="sxs-lookup"><span data-stu-id="680fc-220">But really, you care only if it's *not* empty, therefore ...</span></span>
- <span data-ttu-id="680fc-221">新增`!`運算子的前面`IsEmpty`測試。</span><span class="sxs-lookup"><span data-stu-id="680fc-221">Add the `!` operator in front of the `IsEmpty` test.</span></span> <span data-ttu-id="680fc-222">(`!`運算子代表邏輯 NOT)。</span><span class="sxs-lookup"><span data-stu-id="680fc-222">(The `!` operator means logical NOT).</span></span>

<span data-ttu-id="680fc-223">一般的語言，將整個`if`條件會轉譯成下列：*如果表單的 searchGenre 項目不是空的然後...*</span><span class="sxs-lookup"><span data-stu-id="680fc-223">In plain English, the entire `if` condition translates into the following: *If the form's searchGenre element is not empty, then ...*</span></span>

<span data-ttu-id="680fc-224">此區塊會設定為建立使用搜尋詞彙的查詢。</span><span class="sxs-lookup"><span data-stu-id="680fc-224">This block sets the stage for creating a query that uses the search term.</span></span> <span data-ttu-id="680fc-225">您要進行的下一節。</span><span class="sxs-lookup"><span data-stu-id="680fc-225">You'll do that in the next section.</span></span>

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> <span data-ttu-id="680fc-226">**要求物件**</span><span class="sxs-lookup"><span data-stu-id="680fc-226">**The Request Object**</span></span>
> 
> <span data-ttu-id="680fc-227">`Request`物件包含瀏覽器時，會傳送至您的應用程式頁面要求或送出的所有資訊。</span><span class="sxs-lookup"><span data-stu-id="680fc-227">The `Request` object contains all the information that the browser sends to your application when a page is requested or submitted.</span></span> <span data-ttu-id="680fc-228">此物件包含使用者提供，例如文字方塊值或要上傳檔案的任何資訊。</span><span class="sxs-lookup"><span data-stu-id="680fc-228">This object includes any information that the user provides, like text box values or a file to upload.</span></span> <span data-ttu-id="680fc-229">它也包含各式各樣的其他資訊，例如 cookie 值中的 URL 查詢字串 （如果有的話），頁面是否正在執行，型別瀏覽器的使用者所使用的瀏覽器中設定的語言清單的檔案路徑以及其他更多功能。</span><span class="sxs-lookup"><span data-stu-id="680fc-229">It also includes all sorts of additional information, like cookies, values in the URL query string (if any), the file path of the page that is running, the type of browser that the user is using, the list of languages that are set in the browser, and much more.</span></span>
> 
> <span data-ttu-id="680fc-230">`Request`物件是*集合*（清單） 的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-230">The `Request` object is a *collection* (list) of values.</span></span> <span data-ttu-id="680fc-231">您藉由指定其名稱取得集合的個別值：</span><span class="sxs-lookup"><span data-stu-id="680fc-231">You get an individual value out of the collection by specifying its name:</span></span>
> 
> `var someValue = Request["name"];`
> 
> <span data-ttu-id="680fc-232">`Request`物件實際上會公開數個子集。</span><span class="sxs-lookup"><span data-stu-id="680fc-232">The `Request` object actually exposes several subsets.</span></span> <span data-ttu-id="680fc-233">例如: </span><span class="sxs-lookup"><span data-stu-id="680fc-233">For example:</span></span>
> 
> - <span data-ttu-id="680fc-234">`Request.Form` 可讓您從項目內已提交的值`<form>`項目，如果要求是`POST`要求。</span><span class="sxs-lookup"><span data-stu-id="680fc-234">`Request.Form` gives you values from elements inside the submitted `<form>` element if the request is a `POST` request.</span></span>
> - <span data-ttu-id="680fc-235">`Request.QueryString` URL 查詢字串中，提供您剛才的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-235">`Request.QueryString` gives you just the values in the URL's query string.</span></span> <span data-ttu-id="680fc-236">(例如 URL 中`http://mysite/myapp/page?searchGenre=action&page=2`、 `?searchGenre=action&page=2` URL 區段是查詢字串。)</span><span class="sxs-lookup"><span data-stu-id="680fc-236">(In a URL like `http://mysite/myapp/page?searchGenre=action&page=2`, the `?searchGenre=action&page=2` section of the URL is the query string.)</span></span>
> - <span data-ttu-id="680fc-237">`Request.Cookies` 集合可讓您存取瀏覽器已傳送的 cookie。</span><span class="sxs-lookup"><span data-stu-id="680fc-237">`Request.Cookies` collection gives you access to cookies that the browser has sent.</span></span>
> 
> <span data-ttu-id="680fc-238">若要取得您知道值是在送出表單，您可以使用`Request["name"]`。</span><span class="sxs-lookup"><span data-stu-id="680fc-238">To get a value that you know is in the submitted form, you can use `Request["name"]`.</span></span> <span data-ttu-id="680fc-239">或者，您可以使用更特定的版本`Request.Form["name"]`(如`POST`要求) 或`Request.QueryString["name"]`(如`GET`要求)。</span><span class="sxs-lookup"><span data-stu-id="680fc-239">Alternatively, you can use the more specific versions `Request.Form["name"]` (for `POST` requests) or `Request.QueryString["name"]` (for `GET` requests).</span></span> <span data-ttu-id="680fc-240">當然，*名稱*是要取得之項目的名稱。</span><span class="sxs-lookup"><span data-stu-id="680fc-240">Of course, *name* is the name of the item to get.</span></span>
> 
> <span data-ttu-id="680fc-241">您想要取得的項目名稱必須是唯一您正在使用在集合中。</span><span class="sxs-lookup"><span data-stu-id="680fc-241">The name of the item you want to get has to be unique within the collection you're using.</span></span> <span data-ttu-id="680fc-242">這就是為什麼`Request`物件所提供的子集喜歡`Request.Form`和`Request.QueryString`。</span><span class="sxs-lookup"><span data-stu-id="680fc-242">That's why the `Request` object provides the subsets like `Request.Form` and `Request.QueryString`.</span></span> <span data-ttu-id="680fc-243">假設您的頁面包含名為表單項目`userName`和*也*包含名為 cookie `userName`。</span><span class="sxs-lookup"><span data-stu-id="680fc-243">Suppose that your page contains a form element named `userName` and *also* contains a cookie named `userName`.</span></span> <span data-ttu-id="680fc-244">如果您收到`Request["userName"]`，模稜兩可是否要讓表單值或 cookie。</span><span class="sxs-lookup"><span data-stu-id="680fc-244">If you get `Request["userName"]`, it's ambiguous whether you want the form value or the cookie.</span></span> <span data-ttu-id="680fc-245">不過，如果您收到`Request.Form["userName"]`或`Request.Cookie["userName"]`，您要明確了解要取得的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-245">However, if you get `Request.Form["userName"]` or `Request.Cookie["userName"]`, you're being explicit about which value to get.</span></span>
> 
> <span data-ttu-id="680fc-246">它是最好的作法是特定，且使用的子集`Request`，您感興趣，例如`Request.Form`或`Request.QueryString`。</span><span class="sxs-lookup"><span data-stu-id="680fc-246">It's a good practice to be specific and use the subset of `Request` that you're interested in, like `Request.Form` or `Request.QueryString`.</span></span> <span data-ttu-id="680fc-247">在本教學課程，您要建立簡單的頁面，它可能不會真的會有任何影響。</span><span class="sxs-lookup"><span data-stu-id="680fc-247">For the simple pages that you're creating in this tutorial, it probably doesn't really make any difference.</span></span> <span data-ttu-id="680fc-248">不過，當您建立更複雜的頁面，使用明確的版本`Request.Form`或`Request.QueryString`可協助您避免當頁面包含表單 （或多個表單），可以發生的問題，cookie、 查詢字串值等等。</span><span class="sxs-lookup"><span data-stu-id="680fc-248">However, as you create more complex pages, using the explicit version `Request.Form` or `Request.QueryString` can help you avoid problems that can arise when the page contains a form (or multiple forms), cookies, query string values, and so on.</span></span>


## <a name="creating-a-query-by-using-a-search-term"></a><span data-ttu-id="680fc-249">建立查詢所使用的搜尋詞彙</span><span class="sxs-lookup"><span data-stu-id="680fc-249">Creating a Query by Using a Search Term</span></span>

<span data-ttu-id="680fc-250">您現在知道如何取得使用者輸入搜尋詞彙，您可以建立使用它的查詢。</span><span class="sxs-lookup"><span data-stu-id="680fc-250">Now that you know how to get the search term that the user entered, you can create a query that uses it.</span></span> <span data-ttu-id="680fc-251">請記住，若要取得移出資料庫的所有電影項目，您正在使用 SQL 查詢，看起來像此陳述式：</span><span class="sxs-lookup"><span data-stu-id="680fc-251">Remember that to get all the movie items out of the database, you're using a SQL query that looks like this statement:</span></span>

`SELECT * FROM Movies`

<span data-ttu-id="680fc-252">若要取得特定的電影，您必須使用包含查詢`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="680fc-252">To get only certain movies, you have to use a query that includes a `Where` clause.</span></span> <span data-ttu-id="680fc-253">這個子句可讓您設定的條件的查詢所傳回資料列。</span><span class="sxs-lookup"><span data-stu-id="680fc-253">This clause lets you set a condition on which rows are returned by the query.</span></span> <span data-ttu-id="680fc-254">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="680fc-254">Here's an example:</span></span>

`SELECT * FROM Movies WHERE Genre = 'Action'`

<span data-ttu-id="680fc-255">基本格式是`WHERE column = value`。</span><span class="sxs-lookup"><span data-stu-id="680fc-255">The basic format is `WHERE column = value`.</span></span> <span data-ttu-id="680fc-256">您可以使用不同的運算子，只除了`=`、 like `>` （大於）、 `<` （小於）、 `<>` （不等於）、 `<=` （小於或等於）、 等等，根據您要尋找的。</span><span class="sxs-lookup"><span data-stu-id="680fc-256">You can use different operators besides just `=`, like `>` (greater than), `<` (less than), `<>` (not equal to), `<=` (less than or equal to), etc., depending on what you're looking for.</span></span>

<span data-ttu-id="680fc-257">如果您想知道，SQL 陳述式不區分大小寫&mdash;`SELECT`相同`Select`(或甚至`select`)。</span><span class="sxs-lookup"><span data-stu-id="680fc-257">In case you're wondering, SQL statements are not case sensitive &mdash; `SELECT` is the same as `Select` (or even `select`).</span></span> <span data-ttu-id="680fc-258">不過，人們通常大寫 SQL 陳述式中的關鍵字 like`SELECT`和`WHERE`，讓您更輕鬆地閱讀。</span><span class="sxs-lookup"><span data-stu-id="680fc-258">However, people often capitalize keywords in a SQL statement, like `SELECT` and `WHERE`, to make it easier to read.</span></span>

### <a name="passing-the-search-term-as-a-parameter"></a><span data-ttu-id="680fc-259">傳遞做為參數的搜尋詞彙</span><span class="sxs-lookup"><span data-stu-id="680fc-259">Passing the search term as a parameter</span></span>

<span data-ttu-id="680fc-260">搜尋特定的內容類型也很簡單 (`WHERE Genre = 'Action'`)，但您想要能夠搜尋任何使用者輸入的內容類型。</span><span class="sxs-lookup"><span data-stu-id="680fc-260">Searching for a specific genre is easy enough (`WHERE Genre = 'Action'`), but you want to be able to search for any genre that the user enters.</span></span> <span data-ttu-id="680fc-261">若要這樣做，請您建立為包含要搜尋的預留位置值的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="680fc-261">To do that, you create as SQL query that includes a placeholder for the value to search.</span></span> <span data-ttu-id="680fc-262">它看起來像下列命令：</span><span class="sxs-lookup"><span data-stu-id="680fc-262">It will look like this command:</span></span>

`SELECT * FROM Movies WHERE Genre = @0`

<span data-ttu-id="680fc-263">預留位置是`@`字元後面接著零。</span><span class="sxs-lookup"><span data-stu-id="680fc-263">The placeholder is the `@` character followed by zero.</span></span> <span data-ttu-id="680fc-264">您可能會猜測，查詢可以包含多個 「 預留位置 」，以及會命名為`@0`， `@1`，`@2`等等。</span><span class="sxs-lookup"><span data-stu-id="680fc-264">As you might guess, a query can contain multiple placeholders, and they'd be named `@0`, `@1`, `@2`, etc.</span></span>

<span data-ttu-id="680fc-265">若要設定查詢和實際傳遞的值，您可以使用如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="680fc-265">To set up the query and actually pass it the value, you use the code like the following:</span></span>

[!code-sql[Main](form-basics/samples/sample4.sql)]

<span data-ttu-id="680fc-266">此程式碼很類似什麼您已完成在方格中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="680fc-266">This code is similar to what you've already done to display data in the grid.</span></span> <span data-ttu-id="680fc-267">唯一的差異如下：</span><span class="sxs-lookup"><span data-stu-id="680fc-267">The only differences are:</span></span>

- <span data-ttu-id="680fc-268">查詢包含預留位置 (`WHERE Genre = @0"`)。</span><span class="sxs-lookup"><span data-stu-id="680fc-268">The query contains a placeholder (`WHERE Genre = @0"`).</span></span>
- <span data-ttu-id="680fc-269">查詢會放入變數 (`selectCommand`); 之前，您查詢直接傳遞至`db.Query`方法。</span><span class="sxs-lookup"><span data-stu-id="680fc-269">The query is put into a variable (`selectCommand`); before, you passed the query directly to the `db.Query` method.</span></span>
- <span data-ttu-id="680fc-270">當您呼叫`db.Query`方法，您將傳遞查詢和要使用的預留位置的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-270">When you call the `db.Query` method, you pass both the query and the value to use for the placeholder.</span></span> <span data-ttu-id="680fc-271">(如果查詢在多個預留位置，您會將其傳遞做為個別值的方法。)</span><span class="sxs-lookup"><span data-stu-id="680fc-271">(If the query had multiple placeholders, you'd pass them all as separate values to the method.)</span></span>

<span data-ttu-id="680fc-272">如果您同時將所有這些項目，您會得到下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="680fc-272">If you put all these elements together, you get the following code:</span></span>

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="680fc-273">**重要！**</span><span class="sxs-lookup"><span data-stu-id="680fc-273">**Important!**</span></span> <span data-ttu-id="680fc-274">使用預留位置 (例如`@0`) 將值傳遞至 SQL 命令為*極為重要*安全性。</span><span class="sxs-lookup"><span data-stu-id="680fc-274">Using placeholders (like `@0`) to pass values to a SQL command is *extremely important* for security.</span></span> <span data-ttu-id="680fc-275">您看到該元件，和預留位置的變數，方法是您應該建構 SQL 命令的唯一方式。</span><span class="sxs-lookup"><span data-stu-id="680fc-275">The way you see it here, with placeholders for variable data, is the only way you should construct SQL commands.</span></span>
> 
> <span data-ttu-id="680fc-276">永遠不會以建構 SQL 陳述式放在一起，（串連） 的常值文字和您從使用者取得的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-276">Never construct a SQL statement by putting together (concatenating) literal text and values you get from the user.</span></span> <span data-ttu-id="680fc-277">串連使用者輸入 SQL 陳述式會開啟您的站台*SQL 資料隱碼攻擊*當惡意使用者送出至您的頁面 hack 資料庫的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-277">Concatenating user input into a SQL statement opens your site to a *SQL injection attack* where a malicious user submits values to your page that hack your database.</span></span> <span data-ttu-id="680fc-278">(您可以讀取多個發行項中[SQL 資料隱碼](https://msdn.microsoft.com/library/ms161953.aspx)MSDN 網站。)</span><span class="sxs-lookup"><span data-stu-id="680fc-278">(You can read more in the article [SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) the MSDN website.)</span></span>


## <a name="updating-the-movies-page-with-search-code"></a><span data-ttu-id="680fc-279">搜尋程式碼以更新 [影片] 頁面</span><span class="sxs-lookup"><span data-stu-id="680fc-279">Updating the Movies Page with Search Code</span></span>

<span data-ttu-id="680fc-280">現在您可以更新中的程式碼*Movies.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="680fc-280">Now you can update the code in the *Movies.cshtml* file.</span></span> <span data-ttu-id="680fc-281">若要開始，請在頁面頂端的程式碼區塊中的程式碼取代這段程式碼：</span><span class="sxs-lookup"><span data-stu-id="680fc-281">To begin, replace the code in the code block at the top of the page with this code:</span></span>

[!code-csharp[Main](form-basics/samples/sample6.cs)]

<span data-ttu-id="680fc-282">此處的差異在於您已將放入查詢`selectCommand`變數，您會將它傳遞給`db.Query`更新版本。</span><span class="sxs-lookup"><span data-stu-id="680fc-282">The difference here is that you've put the query into the `selectCommand` variable, which you'll pass to `db.Query` later.</span></span> <span data-ttu-id="680fc-283">將放入變數的 SQL 陳述式，可讓您變更陳述式，也就是您將會執行執行搜尋。</span><span class="sxs-lookup"><span data-stu-id="680fc-283">Putting the SQL statement into a variable lets you change the statement, which is what you'll do to perform the search.</span></span>

<span data-ttu-id="680fc-284">您也已移除這兩行，您將會放回更新版本：</span><span class="sxs-lookup"><span data-stu-id="680fc-284">You've also removed these two lines, which you'll put back in later:</span></span>

[!code-csharp[Main](form-basics/samples/sample7.cs)]

<span data-ttu-id="680fc-285">您不想要尚未執行查詢 (也就是呼叫`db.Query`) 而您不想要初始化`WebGrid`協助程式，但其中。</span><span class="sxs-lookup"><span data-stu-id="680fc-285">You don't want to run the query yet (that is, call `db.Query`) and you don't want to initialize the `WebGrid` helper yet either.</span></span> <span data-ttu-id="680fc-286">說已經找出哪些 SQL 陳述式都必須執行之後，您要進行這些工作。</span><span class="sxs-lookup"><span data-stu-id="680fc-286">You'll do those things after you've figured out which SQL statement has to run.</span></span>

<span data-ttu-id="680fc-287">之後重寫區塊中，您可以加入新的邏輯來處理的搜尋。</span><span class="sxs-lookup"><span data-stu-id="680fc-287">After this rewritten block, you can add the new logic for handling the search.</span></span> <span data-ttu-id="680fc-288">已完成的程式碼看起來如下所示。</span><span class="sxs-lookup"><span data-stu-id="680fc-288">The completed code will look like the following.</span></span> <span data-ttu-id="680fc-289">更新您的頁面中的程式碼，使其符合此範例中：</span><span class="sxs-lookup"><span data-stu-id="680fc-289">Update the code in your page so it matches this example:</span></span>

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

<span data-ttu-id="680fc-290">頁面現在的運作方式如下。</span><span class="sxs-lookup"><span data-stu-id="680fc-290">The page now works like this.</span></span> <span data-ttu-id="680fc-291">每次執行網頁時，程式碼會開啟該資料庫和`selectCommand`變數會設為 SQL 陳述式可取得所有記錄`Movies`資料表。</span><span class="sxs-lookup"><span data-stu-id="680fc-291">Every time the page runs, the code opens the database and the `selectCommand` variable is set to the SQL statement that gets all the records from the `Movies` table.</span></span> <span data-ttu-id="680fc-292">程式碼也會初始化`searchTerm`變數。</span><span class="sxs-lookup"><span data-stu-id="680fc-292">The code also initializes the `searchTerm` variable.</span></span>

<span data-ttu-id="680fc-293">不過，如果目前的要求中包含的值`searchGenre`項目、 程式碼集`selectCommand`至不同的查詢，也就是要包含`Where`子句以搜尋內容類型。</span><span class="sxs-lookup"><span data-stu-id="680fc-293">However, if the current request includes a value for the `searchGenre` element, the code sets `selectCommand` to a different query — namely, to one that includes the `Where` clause to search for a genre.</span></span> <span data-ttu-id="680fc-294">它也會設定`searchTerm`任何已傳遞的 [搜尋] 方塊 （這可能是 nothing）。</span><span class="sxs-lookup"><span data-stu-id="680fc-294">It also sets `searchTerm` to whatever was passed for the search box (which might be nothing).</span></span>

<span data-ttu-id="680fc-295">不論哪一個 SQL 陳述式是在`selectCommand`，程式碼接著會呼叫`db.Query`為執行查詢時，將其傳遞任何處於`searchTerm`。</span><span class="sxs-lookup"><span data-stu-id="680fc-295">Regardless of which SQL statement is in `selectCommand`, the code then calls `db.Query` to run the query, passing it whatever is in `searchTerm`.</span></span> <span data-ttu-id="680fc-296">如果沒有任何`searchTerm`，它並不重要，因為在此情況下沒有任何參數將值傳遞至`selectCommand`嗎。</span><span class="sxs-lookup"><span data-stu-id="680fc-296">If there's nothing in `searchTerm`, it doesn't matter, because in that case there's no parameter to pass the value to `selectCommand` anyway.</span></span>

<span data-ttu-id="680fc-297">最後，程式碼會初始化`WebGrid`使用查詢結果，就像之前的協助程式。</span><span class="sxs-lookup"><span data-stu-id="680fc-297">Finally, the code initializes the `WebGrid` helper by using the query results, just like before.</span></span>

<span data-ttu-id="680fc-298">您可以看到將放入 SQL 陳述式，並搜尋詞彙到變數中，您已加入彈性的程式碼。</span><span class="sxs-lookup"><span data-stu-id="680fc-298">You can see that by putting the SQL statement and the search term into variables, you've added flexibility to the code.</span></span> <span data-ttu-id="680fc-299">您會發現稍後在本教學課程，您可以使用這個基本架構，並保留加入不同類型的搜尋邏輯。</span><span class="sxs-lookup"><span data-stu-id="680fc-299">As you'll see later in this tutorial, you can use this basic framework and keep adding logic for different types of searches.</span></span>

## <a name="testing-the-search-by-genre-feature"></a><span data-ttu-id="680fc-300">測試的內容類型的搜尋功能</span><span class="sxs-lookup"><span data-stu-id="680fc-300">Testing the Search-by-Genre Feature</span></span>

<span data-ttu-id="680fc-301">在 WebMatrix 中，執行*Movies.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="680fc-301">In WebMatrix, run the *Movies.cshtml* page.</span></span> <span data-ttu-id="680fc-302">您看到的頁面類型的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="680fc-302">You see the page with the text box for genre.</span></span>

<span data-ttu-id="680fc-303">輸入，您已為您的測試記錄的其中一個輸入，然後按一下 內容類型**搜尋**。</span><span class="sxs-lookup"><span data-stu-id="680fc-303">Enter a genre that you've entered for one of your test records, then click **Search**.</span></span> <span data-ttu-id="680fc-304">此時您會看到剛才電影符合該類型的清單：</span><span class="sxs-lookup"><span data-stu-id="680fc-304">This time you see a listing of just the movies that match that genre:</span></span>

![列出搜尋內容類型 'Comedies' 之後影片頁面](form-basics/_static/image4.png)

<span data-ttu-id="680fc-306">輸入不同的內容類型，然後再搜尋一次。</span><span class="sxs-lookup"><span data-stu-id="680fc-306">Enter a different genre and search again.</span></span> <span data-ttu-id="680fc-307">請嘗試使用所有小寫或全部大寫的字母，讓您能夠查看搜尋不區分大小寫輸入的內容類型。</span><span class="sxs-lookup"><span data-stu-id="680fc-307">Try entering the genre by using all lowercase or all uppercase letters so that you can see that the search is not case sensitive.</span></span>

## <a name="remembering-what-the-user-entered"></a><span data-ttu-id="680fc-308">「 記住 」 使用者的輸入</span><span class="sxs-lookup"><span data-stu-id="680fc-308">"Remembering" What the User Entered</span></span>

<span data-ttu-id="680fc-309">您可能已注意到，之後您輸入的內容類型，然後再按下**搜尋內容類型**，您會看到該內容類型的清單。</span><span class="sxs-lookup"><span data-stu-id="680fc-309">You might have noticed that after you entered a genre and clicked **Search Genre**, you saw a listing for that genre.</span></span> <span data-ttu-id="680fc-310">不過，[搜尋] 方塊是空的&mdash;換句話說，它不記得您已輸入。</span><span class="sxs-lookup"><span data-stu-id="680fc-310">However, the search text box was empty &mdash; in other words, it didn't remember what you'd entered.</span></span>

<span data-ttu-id="680fc-311">請務必了解為什麼會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="680fc-311">It's important to understand why this behavior occurs.</span></span> <span data-ttu-id="680fc-312">當您提交頁面時，瀏覽器會傳送要求至 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="680fc-312">When you submit a page, the browser sends a request to the web server.</span></span> <span data-ttu-id="680fc-313">當 ASP.NET 要求時，建立網頁的全新執行個體、 執行的程式碼，以及接著再次轉譯至瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="680fc-313">When ASP.NET gets the request, it creates a brand-new instance of the page, runs the code in it, and then renders the page to the browser again.</span></span> <span data-ttu-id="680fc-314">作用中，不過，頁面並不知道，您只使用本身的先前版本。</span><span class="sxs-lookup"><span data-stu-id="680fc-314">In effect, though, the page doesn't know that you were just working with a previous version of itself.</span></span> <span data-ttu-id="680fc-315">它知道它取得具有一些要求所有表單中的資料。</span><span class="sxs-lookup"><span data-stu-id="680fc-315">All it knows is that it got a request that had some form data in it.</span></span>

<span data-ttu-id="680fc-316">每次要求頁面&mdash;第一次還是送出&mdash;您收到新的頁面。</span><span class="sxs-lookup"><span data-stu-id="680fc-316">Every time you request a page &mdash; whether for the first time or by submitting it &mdash; you're getting a new page.</span></span> <span data-ttu-id="680fc-317">網頁伺服器有沒有您的最後一個要求的記憶體。</span><span class="sxs-lookup"><span data-stu-id="680fc-317">The web server has no memory of your last request.</span></span> <span data-ttu-id="680fc-318">都不會 ASP.NET 中，而且都不會在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="680fc-318">Neither does ASP.NET, and neither does the browser.</span></span> <span data-ttu-id="680fc-319">頁面的這些個別執行個體之間的唯一連接是您在它們之間傳送的任何資料。</span><span class="sxs-lookup"><span data-stu-id="680fc-319">The only connection between these separate instances of the page is any data that you transmit between them.</span></span> <span data-ttu-id="680fc-320">比方說，如果送出頁面之後，新的頁面執行個體可以取得舊版的執行個體所送出表單資料。</span><span class="sxs-lookup"><span data-stu-id="680fc-320">If you submit a page, for example, the new page instance can get the form data that was sent by the earlier instance.</span></span> <span data-ttu-id="680fc-321">（頁面之間傳遞資料的另一種方式是使用 cookie）。</span><span class="sxs-lookup"><span data-stu-id="680fc-321">(Another way to pass data between pages is to use cookies.)</span></span>

<span data-ttu-id="680fc-322">型式的方式來描述這種情況是說出網頁是*無狀態*。</span><span class="sxs-lookup"><span data-stu-id="680fc-322">A formal way to describe this situation is to say that web pages are *stateless*.</span></span> <span data-ttu-id="680fc-323">Web 伺服器本身的頁面和頁面中的項目不會維護任何先前的頁面狀態的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="680fc-323">Web servers and the pages themselves and the elements in the page do not maintain any information about the previous state of a page.</span></span> <span data-ttu-id="680fc-324">網站的設計是這種方式，因為維護個別要求的狀態會快速耗盡資源的網頁伺服器，通常可能處理上千，甚至數百個，每秒要求數。</span><span class="sxs-lookup"><span data-stu-id="680fc-324">The web was designed this way because maintaining state for individual requests would quickly exhaust the resources of web servers, which often handle thousands, maybe even hundreds of thousands, of requests per second.</span></span>

<span data-ttu-id="680fc-325">因此，這就是為什麼文字方塊是空的。</span><span class="sxs-lookup"><span data-stu-id="680fc-325">So that's why the text box was empty.</span></span> <span data-ttu-id="680fc-326">送出頁面之後，ASP.NET 會建立頁面的新執行個體，並執行透過程式碼和標記。</span><span class="sxs-lookup"><span data-stu-id="680fc-326">After you submitted the page, ASP.NET created a new instance of the page and ran through the code and markup.</span></span> <span data-ttu-id="680fc-327">沒有任何會告知 ASP.NET，若要將值置入文字方塊中之程式碼中。</span><span class="sxs-lookup"><span data-stu-id="680fc-327">There was nothing in that code that told ASP.NET to put a value into the text box.</span></span> <span data-ttu-id="680fc-328">因此 ASP.NET 並不執行，並沒有在它的值已轉譯的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="680fc-328">So ASP.NET didn't do anything, and the text box was rendered without a value in it.</span></span>

<span data-ttu-id="680fc-329">沒有實際上可以輕鬆地取得解決此問題。</span><span class="sxs-lookup"><span data-stu-id="680fc-329">There's actually an easy way to get around this issue.</span></span> <span data-ttu-id="680fc-330">您在文字方塊中輸入的內容類型*是*為您提供的程式碼&mdash;處於`Request.QueryString["searchGenre"]`。</span><span class="sxs-lookup"><span data-stu-id="680fc-330">The genre that you entered into the text box *is* available to you in code &mdash; it's in `Request.QueryString["searchGenre"]`.</span></span>

<span data-ttu-id="680fc-331">更新在文字方塊中的標記以便`value`屬性取得其值從`searchTerm`，類似這個範例：</span><span class="sxs-lookup"><span data-stu-id="680fc-331">Update the markup for the text box so that the `value` attribute gets its value from `searchTerm`, like this example:</span></span>

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

<span data-ttu-id="680fc-332">在此頁面上，您可能有也設定`value`屬性`searchTerm`您輸入變數，因為該變數也包含內容類型。</span><span class="sxs-lookup"><span data-stu-id="680fc-332">In this page, you could have also set the `value` attribute to the `searchTerm` variable, since that variable also contains the genre you entered.</span></span> <span data-ttu-id="680fc-333">但使用`Request`物件來設定`value`屬性所示如下的標準方式完成這項工作。</span><span class="sxs-lookup"><span data-stu-id="680fc-333">But using the `Request` object to set the `value` attribute as shown here is the standard way to accomplish this task.</span></span> <span data-ttu-id="680fc-334">(假設要執行這個動作&mdash;在某些情況下，您可能想要呈現頁面*沒有*欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-334">(Assuming you even want to do this &mdash; in some situations, you might want to render the page *without* values in the fields.</span></span> <span data-ttu-id="680fc-335">這所有取決於您的應用程式使用狀況。）</span><span class="sxs-lookup"><span data-stu-id="680fc-335">It all depends on what's going on with your app.)</span></span>

> [!NOTE]
> <span data-ttu-id="680fc-336">您無法 [記住] 文字方塊中所使用的密碼值。</span><span class="sxs-lookup"><span data-stu-id="680fc-336">You can't "remember" the value of a text box that's used for passwords.</span></span> <span data-ttu-id="680fc-337">將安全性漏洞，來允許人員填寫密碼欄位，使用程式碼。</span><span class="sxs-lookup"><span data-stu-id="680fc-337">It would be a security hole to allow people to fill in a password field by using code.</span></span>


<span data-ttu-id="680fc-338">再次執行頁面，輸入類型，然後按一下**搜尋內容類型**。</span><span class="sxs-lookup"><span data-stu-id="680fc-338">Run the page again, enter a genre, and click **Search Genre**.</span></span> <span data-ttu-id="680fc-339">這次不只執行您看到搜尋的結果，但文字方塊會記住您輸入上一次：</span><span class="sxs-lookup"><span data-stu-id="680fc-339">This time not only do you see the results of the search, but the text box remembers what you entered last time:</span></span>

![顯示在文字方塊中有 '記住' 先前的項目頁面](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a><span data-ttu-id="680fc-341">搜尋標題中的任何文字</span><span class="sxs-lookup"><span data-stu-id="680fc-341">Searching for Any Word in the Title</span></span>

<span data-ttu-id="680fc-342">您現在可以搜尋的任何內容類型，但您也可以搜尋的標題。</span><span class="sxs-lookup"><span data-stu-id="680fc-342">You can now search for any genre, but you might also want to search for a title.</span></span> <span data-ttu-id="680fc-343">很難取得標題正確，當您搜尋，而您可搜尋的任何位置出現在標題文字。</span><span class="sxs-lookup"><span data-stu-id="680fc-343">It's hard to get a title exactly right when you search, so instead you can search for a word that appears anywhere inside a title.</span></span> <span data-ttu-id="680fc-344">若要這樣做，在 SQL 中，您使用`LIKE`運算子和語法，例如下列：</span><span class="sxs-lookup"><span data-stu-id="680fc-344">To do that in SQL, you use the `LIKE` operator and syntax like the following:</span></span>

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

<span data-ttu-id="680fc-345">此命令會取得其標題包含"adventure"的所有電影。</span><span class="sxs-lookup"><span data-stu-id="680fc-345">This command gets all the movies whose titles contain "adventure".</span></span> <span data-ttu-id="680fc-346">當您使用`LIKE`運算子，包括萬用字元`%`做為搜尋詞彙的一部分。</span><span class="sxs-lookup"><span data-stu-id="680fc-346">When you use the `LIKE` operator, you include the wildcard character `%` as part of the search term.</span></span> <span data-ttu-id="680fc-347">搜尋`LIKE 'adventure%'`表示 「 開頭 'adventure' 」。</span><span class="sxs-lookup"><span data-stu-id="680fc-347">The search `LIKE 'adventure%'` means "starting with 'adventure'".</span></span> <span data-ttu-id="680fc-348">（技術上來說，表示 「 字串 'adventure' 後面的任何項目。 」）同樣地，搜尋詞彙`LIKE '%adventure'`表示 「 任何項目後面的字串 'adventure'"，這是另一種方式說 「 結束與 'adventure' 」。</span><span class="sxs-lookup"><span data-stu-id="680fc-348">(Technically, it means "The string 'adventure' followed by anything.") Similarly, the search term `LIKE '%adventure'` means "anything followed by the string 'adventure'", which is another way to say "ending with 'adventure'".</span></span>

<span data-ttu-id="680fc-349">搜尋詞彙`LIKE '%adventure%'`因此表示 「 具"adventure' 標題中任何位置。"</span><span class="sxs-lookup"><span data-stu-id="680fc-349">The search term `LIKE '%adventure%'` therefore means "with 'adventure' anywhere in the title."</span></span> <span data-ttu-id="680fc-350">（技術上來說，「 任何項目在標題中，後面接著 'adventure'，後面接著任何項目。 」）</span><span class="sxs-lookup"><span data-stu-id="680fc-350">(Technically, "anything in the title, followed by 'adventure', followed by anything.")</span></span>

<span data-ttu-id="680fc-351">內部`<form>`項目，加入下列標記結尾下的`</div>`類型搜尋的標記 (的結束`</form>`項目):</span><span class="sxs-lookup"><span data-stu-id="680fc-351">Inside the `<form>` element, add the following markup right under the closing `</div>` tag for the genre search (just before the closing `</form>` element):</span></span>

[!code-html[Main](form-basics/samples/sample10.html)]

<span data-ttu-id="680fc-352">處理此搜尋的程式碼是類似的程式碼類型的搜尋，只不過您必須組合`LIKE`搜尋。</span><span class="sxs-lookup"><span data-stu-id="680fc-352">The code to handle this search is similar to the code for the genre search, except that you have to assemble the `LIKE` search.</span></span> <span data-ttu-id="680fc-353">在頁面頂端的程式碼區塊內加入下列`if`後方封鎖`if`類型搜尋的區塊：</span><span class="sxs-lookup"><span data-stu-id="680fc-353">Inside the code block at the top of the page, add this `if` block just after the `if` block for the genre search:</span></span>

[!code-csharp[Main](form-basics/samples/sample11.cs)]

<span data-ttu-id="680fc-354">此程式碼使用您稍早所見的相同邏輯，不同之處在於會使用搜尋`LIKE`運算子和程式碼將 「`%`"之前和之後的搜尋字詞。</span><span class="sxs-lookup"><span data-stu-id="680fc-354">This code uses the same logic you saw earlier, except that the search uses a `LIKE` operator and the code puts "`%`" before and after the search term.</span></span>

<span data-ttu-id="680fc-355">請注意如何很容易就能加入網頁中的另一個搜尋。</span><span class="sxs-lookup"><span data-stu-id="680fc-355">Notice how it was easy to add another search to the page.</span></span> <span data-ttu-id="680fc-356">您必須是：</span><span class="sxs-lookup"><span data-stu-id="680fc-356">All you had to do was:</span></span>

- <span data-ttu-id="680fc-357">建立`if`測試，以查看相關的搜尋方塊是否具有值的區塊。</span><span class="sxs-lookup"><span data-stu-id="680fc-357">Create an `if` block that tested to see whether the relevant search box had a value.</span></span>
- <span data-ttu-id="680fc-358">設定`selectCommand`變數設為新的 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="680fc-358">Set the `selectCommand` variable to a new SQL statement.</span></span>
- <span data-ttu-id="680fc-359">設定`searchTerm`變數設為要傳遞給查詢的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-359">Set the `searchTerm` variable to the value to pass to the query.</span></span>

<span data-ttu-id="680fc-360">以下是完整的程式碼區塊，其中包含項目的搜尋的新邏輯：</span><span class="sxs-lookup"><span data-stu-id="680fc-360">Here's the complete code block, which contains the new logic for a title search:</span></span>

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

<span data-ttu-id="680fc-361">此程式碼的用途的摘要如下：</span><span class="sxs-lookup"><span data-stu-id="680fc-361">Here's a summary of what this code does:</span></span>

- <span data-ttu-id="680fc-362">變數`searchTerm`和`selectCommand`頂端會初始化。</span><span class="sxs-lookup"><span data-stu-id="680fc-362">The variables `searchTerm` and `selectCommand` are initialized at the top.</span></span> <span data-ttu-id="680fc-363">您要將這些變數設定為適當的搜尋詞彙 （如果有的話），使用者沒有在頁面根據適當的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="680fc-363">You're going to set these variables to the appropriate search term (if any) and appropriate SQL command based on what the user does in the page.</span></span> <span data-ttu-id="680fc-364">預設搜尋是簡單的情況下，從資料庫取得所有影片。</span><span class="sxs-lookup"><span data-stu-id="680fc-364">The default search is the simple case of getting all the movies from the database.</span></span>
- <span data-ttu-id="680fc-365">中的測試`searchGenre`和`searchTitle`，程式碼集`searchTerm`您想要搜尋的值。</span><span class="sxs-lookup"><span data-stu-id="680fc-365">In the tests for `searchGenre` and `searchTitle`, the code sets `searchTerm` to the value you want to search for.</span></span> <span data-ttu-id="680fc-366">這些程式碼區塊也設定`selectCommand`到該項搜尋適當的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="680fc-366">Those code blocks also set `selectCommand` to an appropriate SQL command for that search.</span></span>
- <span data-ttu-id="680fc-367">`db.Query`方法叫用一次，使用任何 SQL 命令`selectedCommand`和任何值存在於`searchTerm`。</span><span class="sxs-lookup"><span data-stu-id="680fc-367">The `db.Query` method is invoked only once, using whatever SQL command is in `selectedCommand` and whatever value is in `searchTerm`.</span></span> <span data-ttu-id="680fc-368">如果沒有任何搜尋詞彙 （沒有內容類型和任何標題 word） 的值`searchTerm`為空字串。</span><span class="sxs-lookup"><span data-stu-id="680fc-368">If there is no search term (no genre and no title word), the value of `searchTerm` is an empty string.</span></span> <span data-ttu-id="680fc-369">不過，，並不重要，因為查詢不在此情況下需要參數。</span><span class="sxs-lookup"><span data-stu-id="680fc-369">However, that doesn't matter, because in that case the query doesn't require a parameter.</span></span>

## <a name="testing-the-title-search-feature"></a><span data-ttu-id="680fc-370">測試的標題搜尋功能</span><span class="sxs-lookup"><span data-stu-id="680fc-370">Testing the Title Search Feature</span></span>

<span data-ttu-id="680fc-371">現在您可以測試您已完成的搜尋頁面。</span><span class="sxs-lookup"><span data-stu-id="680fc-371">Now you can test your completed search page.</span></span> <span data-ttu-id="680fc-372">執行*Movies.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="680fc-372">Run *Movies.cshtml*.</span></span>

<span data-ttu-id="680fc-373">輸入的內容類型，然後按一下**搜尋內容類型**。</span><span class="sxs-lookup"><span data-stu-id="680fc-373">Enter a genre and click **Search Genre**.</span></span> <span data-ttu-id="680fc-374">方格會顯示類似之前電影的該內容類型。</span><span class="sxs-lookup"><span data-stu-id="680fc-374">The grid displays movies of that genre, like before.</span></span>

<span data-ttu-id="680fc-375">輸入標題文字，然後按一下**搜尋標題**。</span><span class="sxs-lookup"><span data-stu-id="680fc-375">Enter a title word and click **Search Title**.</span></span> <span data-ttu-id="680fc-376">方格會顯示在標題中有該單字的電影。</span><span class="sxs-lookup"><span data-stu-id="680fc-376">The grid displays movies that have that word in the title.</span></span>

![列出搜尋之後 '' 標題中的影片頁面](form-basics/_static/image6.png)

<span data-ttu-id="680fc-378">將這兩個文字方塊保留空白，然後按一下其中一個按鈕。</span><span class="sxs-lookup"><span data-stu-id="680fc-378">Leave both text boxes blank and click either button.</span></span> <span data-ttu-id="680fc-379">方格會顯示所有影片。</span><span class="sxs-lookup"><span data-stu-id="680fc-379">The grid displays all the movies.</span></span>

## <a name="combining-the-queries"></a><span data-ttu-id="680fc-380">結合查詢</span><span class="sxs-lookup"><span data-stu-id="680fc-380">Combining the Queries</span></span>

<span data-ttu-id="680fc-381">您可能會注意到，您可以執行的搜尋會獨佔。</span><span class="sxs-lookup"><span data-stu-id="680fc-381">You might notice that the searches you can perform are exclusive.</span></span> <span data-ttu-id="680fc-382">您無法搜尋標題與內容類型在相同的時間，即使這兩個搜尋方塊中有值。</span><span class="sxs-lookup"><span data-stu-id="680fc-382">You can't search the title and the genre at the same time, even if both search boxes have values in them.</span></span> <span data-ttu-id="680fc-383">例如，您無法搜尋其標題包含"Adventure"的所有動作影片。</span><span class="sxs-lookup"><span data-stu-id="680fc-383">For example, you can't search for all action movies whose title contains "Adventure".</span></span> <span data-ttu-id="680fc-384">（如頁面自動程式碼現在，如果您輸入值的內容類型和標題，標題搜尋取得優先順序。）若要建立結合條件的搜尋，您就必須建立 SQL 查詢的語法如下所示：</span><span class="sxs-lookup"><span data-stu-id="680fc-384">(As the page is coded now, if you enter values for both genre and title, the title search gets precedence.) To create a search that combines the conditions, you would have to create a SQL query that has syntax like the following:</span></span>

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

<span data-ttu-id="680fc-385">而且您必須使用類似下面的陳述式來執行查詢 （大致來說）：</span><span class="sxs-lookup"><span data-stu-id="680fc-385">And you'd have to run the query by using a statement like the following (roughly speaking):</span></span>

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

<span data-ttu-id="680fc-386">建立邏輯，可讓許多的搜尋準則的排列方式可讓有點複雜，您可以看到。</span><span class="sxs-lookup"><span data-stu-id="680fc-386">Creating logic to allow many permutations of search criteria can get a bit involved, as you can see.</span></span> <span data-ttu-id="680fc-387">因此，我們將在此停止。</span><span class="sxs-lookup"><span data-stu-id="680fc-387">Therefore, we'll stop here.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="680fc-388">接下來</span><span class="sxs-lookup"><span data-stu-id="680fc-388">Coming Up Next</span></span>

<span data-ttu-id="680fc-389">在下一個教學課程中，您將建立使用表單來讓使用者資料庫中新增電影的頁面。</span><span class="sxs-lookup"><span data-stu-id="680fc-389">In the next tutorial, you'll create a page that uses a form to let users add movies to the database.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-search"></a><span data-ttu-id="680fc-390">電影頁面 （使用搜尋更新） 的完整清單</span><span class="sxs-lookup"><span data-stu-id="680fc-390">Complete Listing for Movie Page (Updated with Search)</span></span>

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="680fc-391">其他資源</span><span class="sxs-lookup"><span data-stu-id="680fc-391">Additional Resources</span></span>

- [<span data-ttu-id="680fc-392">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="680fc-392">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- <span data-ttu-id="680fc-393">[SQL WHERE 子句](http://www.w3schools.com/sql/sql_where.asp)W3Schools 站台上</span><span class="sxs-lookup"><span data-stu-id="680fc-393">[SQL WHERE Clause](http://www.w3schools.com/sql/sql_where.asp) on the W3Schools site</span></span>
- <span data-ttu-id="680fc-394">[方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的發行項</span><span class="sxs-lookup"><span data-stu-id="680fc-394">[Method Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article on the W3C site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="680fc-395">[上一頁](displaying-data.md)
> [下一頁](entering-data.md)</span><span class="sxs-lookup"><span data-stu-id="680fc-395">[Previous](displaying-data.md)
[Next](entering-data.md)</span></span>
