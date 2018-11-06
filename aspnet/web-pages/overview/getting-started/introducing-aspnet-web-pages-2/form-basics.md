---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: 簡介 ASP.NET Web Pages-HTML 表單基本概念 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何建立一個輸入的表單如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。 而現在您...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 28385ac2244ab0bfb38ee5fcbc64e6e11804612b
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021673"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a><span data-ttu-id="ac97f-104">ASP.NET Web Pages 簡介-HTML 表單基本概念</span><span class="sxs-lookup"><span data-stu-id="ac97f-104">Introducing ASP.NET Web Pages - HTML Form Basics</span></span>
====================
<span data-ttu-id="ac97f-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ac97f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ac97f-106">本教學課程會示範如何建立一個輸入的表單如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="ac97f-106">This tutorial shows you the basics of how to create an input form and how to handle the user's input when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="ac97f-107">現在您有資料庫，您將使用您的表單技術，讓使用者在資料庫中尋找特定的影片。</span><span class="sxs-lookup"><span data-stu-id="ac97f-107">And now that you've got a database, you'll use your form skills to let users find specific movies in the database.</span></span> <span data-ttu-id="ac97f-108">它假設您已完成透過數列[若要顯示的資料使用 ASP.NET Web Pages 簡介](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)。</span><span class="sxs-lookup"><span data-stu-id="ac97f-108">It assumes you have completed the series through [Introduction to Displaying Data Using ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).</span></span>
> 
> <span data-ttu-id="ac97f-109">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="ac97f-109">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ac97f-110">如何使用標準的 HTML 項目建立的表單。</span><span class="sxs-lookup"><span data-stu-id="ac97f-110">How to create a form by using standard HTML elements.</span></span>
> - <span data-ttu-id="ac97f-111">如何讀取使用者的輸入表單中。</span><span class="sxs-lookup"><span data-stu-id="ac97f-111">How to read the user's input in a form.</span></span>
> - <span data-ttu-id="ac97f-112">提供如何建立 SQL 查詢，選擇性地取得資料所使用的搜尋詞彙的使用者。</span><span class="sxs-lookup"><span data-stu-id="ac97f-112">How to create a SQL query that selectively gets data by using a search term that the user supplies.</span></span>
> - <span data-ttu-id="ac97f-113">如何在網頁中 「 記住 」 使用者的輸入欄位。</span><span class="sxs-lookup"><span data-stu-id="ac97f-113">How to have fields in the page "remember" what the user entered.</span></span>
>   
> 
> <span data-ttu-id="ac97f-114">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="ac97f-114">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="ac97f-115">`Request` 物件。</span><span class="sxs-lookup"><span data-stu-id="ac97f-115">The `Request` object.</span></span>
> - <span data-ttu-id="ac97f-116">SQL`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="ac97f-116">The SQL `Where` clause.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ac97f-117">您將建置</span><span class="sxs-lookup"><span data-stu-id="ac97f-117">What You'll Build</span></span>

<span data-ttu-id="ac97f-118">在上一個教學課程中，您建立資料庫、 資料加入至它，並接著使用`WebGrid`helper 來顯示資料。</span><span class="sxs-lookup"><span data-stu-id="ac97f-118">In the previous tutorial, you created a database, added data to it, and then used the `WebGrid` helper to display the data.</span></span> <span data-ttu-id="ac97f-119">在本教學課程中，您將新增搜尋方塊中，可讓您尋找特定的內容類型的電影，或其標題會包含任何您所輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="ac97f-119">In this tutorial, you'll add a search box that lets you find movies of a specific genre or whose title contains whatever word you enter.</span></span> <span data-ttu-id="ac97f-120">（比方說，您將能夠找到所有的影片，內容類型是 「 動作 」，或其標題包含"Harry"Adventure"。）</span><span class="sxs-lookup"><span data-stu-id="ac97f-120">(For example, you'll be able to find all movies whose genre is "Action" or whose title contains "Harry" or "Adventure.")</span></span>

<span data-ttu-id="ac97f-121">當您完成本教學課程中時，您會有如下的頁面：</span><span class="sxs-lookup"><span data-stu-id="ac97f-121">When you're done with this tutorial, you'll have a page like this one:</span></span>

![使用內容類型和標題搜尋影片頁面](form-basics/_static/image1.png)

<span data-ttu-id="ac97f-123">頁面的清單部分是最後一個教學課程中相同&mdash;方格。</span><span class="sxs-lookup"><span data-stu-id="ac97f-123">The listing part of the page is the same as in the last tutorial &mdash; a grid.</span></span> <span data-ttu-id="ac97f-124">的差別是，此方格會顯示只有電影您搜尋的。</span><span class="sxs-lookup"><span data-stu-id="ac97f-124">The difference will be that the grid will show only the movies that you searched for.</span></span>

## <a name="about-html-forms"></a><span data-ttu-id="ac97f-125">關於 HTML 表單</span><span class="sxs-lookup"><span data-stu-id="ac97f-125">About HTML Forms</span></span>

<span data-ttu-id="ac97f-126">(如果您有經驗，建立 HTML 表單，並且之間的差異`GET`和`POST`，您可以略過這一節。)</span><span class="sxs-lookup"><span data-stu-id="ac97f-126">(If you've got experience with creating HTML forms and with the difference between `GET` and `POST`, you can skip this section.)</span></span>

<span data-ttu-id="ac97f-127">表單中的使用者輸入項目的&mdash;文字方塊、 按鈕、 選項按鈕、 核取方塊、 下拉式清單等等。</span><span class="sxs-lookup"><span data-stu-id="ac97f-127">A form has user input elements &mdash; text boxes, buttons, radio buttons, check boxes, drop-down lists, and so on.</span></span> <span data-ttu-id="ac97f-128">使用者填入這些控制項或進行選擇，然後再按一下按鈕來提交表單。</span><span class="sxs-lookup"><span data-stu-id="ac97f-128">Users fill in these controls or make selections and then submit the form by clicking a button.</span></span>

<span data-ttu-id="ac97f-129">此範例說明表單的基本 HTML 語法：</span><span class="sxs-lookup"><span data-stu-id="ac97f-129">The basic HTML syntax of a form is illustrated by this example:</span></span>

[!code-html[Main](form-basics/samples/sample1.html)]

<span data-ttu-id="ac97f-130">當在頁面中，執行此標記時，它會建立一個簡單的表單看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="ac97f-130">When this markup runs in a page, it creates a simple form that looks like this illustration:</span></span>

![基本的 HTML 表單，以瀏覽器中轉譯](form-basics/_static/image2.png)

<span data-ttu-id="ac97f-132">`<form>`元素會括住提交的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="ac97f-132">The `<form>` element encloses HTML elements to be submitted.</span></span> <span data-ttu-id="ac97f-133">(進行簡單的錯誤是將項目新增至頁面，但務必放在`<form>`項目。</span><span class="sxs-lookup"><span data-stu-id="ac97f-133">(An easy mistake to make is to add elements to the page but then forget to put them inside a `<form>` element.</span></span> <span data-ttu-id="ac97f-134">在此情況下，不會送出。）`method`屬性會告知瀏覽器如何提交使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="ac97f-134">In that case, nothing is submitted.) The `method` attribute tells the browser how to submit the user input.</span></span> <span data-ttu-id="ac97f-135">您將此設`post`如果您執行在伺服器上，或更新`get`如果您只從伺服器擷取資料。</span><span class="sxs-lookup"><span data-stu-id="ac97f-135">You set this to `post` if you're performing an update on the server or to `get` if you're just fetching data from the server.</span></span>

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> <span data-ttu-id="ac97f-136">**GET、 POST 和 HTTP 動詞命令安全性**</span><span class="sxs-lookup"><span data-stu-id="ac97f-136">**GET, POST, and HTTP Verb Safety**</span></span>
> 
> <span data-ttu-id="ac97f-137">HTTP 通訊協定的瀏覽器和伺服器用來交換資訊，是其基本的作業非常簡單。</span><span class="sxs-lookup"><span data-stu-id="ac97f-137">HTTP, the protocol that browsers and servers use to exchange information, is remarkably simple in its basic operations.</span></span> <span data-ttu-id="ac97f-138">瀏覽器會使用只有少數的動詞命令，對伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="ac97f-138">Browsers use only a few verbs to make requests to servers.</span></span> <span data-ttu-id="ac97f-139">當您撰寫適用於 web 的程式碼時，最好先了解這些動詞，以及它們如何使用瀏覽器和伺服器。</span><span class="sxs-lookup"><span data-stu-id="ac97f-139">When you write code for the web, it's helpful to understand these verbs and how the browser and server use them.</span></span> <span data-ttu-id="ac97f-140">毫無疑問，最常使用的動詞命令為：</span><span class="sxs-lookup"><span data-stu-id="ac97f-140">Far and away the most commonly used verbs are these:</span></span>
> 
> - <span data-ttu-id="ac97f-141">`GET`.</span><span class="sxs-lookup"><span data-stu-id="ac97f-141">`GET`.</span></span> <span data-ttu-id="ac97f-142">瀏覽器會使用此動詞命令，從伺服器擷取的項目。</span><span class="sxs-lookup"><span data-stu-id="ac97f-142">The browser uses this verb to fetch something from the server.</span></span> <span data-ttu-id="ac97f-143">例如，當您在瀏覽器輸入 URL，瀏覽器執行`GET`作業要求您想要的頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-143">For example, when you type a URL into your browser, the browser performs a `GET` operation to request the page you want.</span></span> <span data-ttu-id="ac97f-144">當頁面包括圖形、 瀏覽器會執行其他`GET`作業，以取得映像。</span><span class="sxs-lookup"><span data-stu-id="ac97f-144">If the page includes graphics, the browser performs additional `GET` operations to get the images.</span></span> <span data-ttu-id="ac97f-145">如果`GET`作業已將資訊傳遞至伺服器，將資訊傳遞做為查詢字串中的 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="ac97f-145">If the `GET` operation has to pass information to the server, the information is passed as part of the URL in the query string.</span></span>
> - <span data-ttu-id="ac97f-146">`POST`.</span><span class="sxs-lookup"><span data-stu-id="ac97f-146">`POST`.</span></span> <span data-ttu-id="ac97f-147">瀏覽器傳送`POST`要求，以便將資料提交至要加入或變更伺服器上。</span><span class="sxs-lookup"><span data-stu-id="ac97f-147">The browser sends a `POST` request in order to submit data to be added or changed on the server.</span></span> <span data-ttu-id="ac97f-148">比方說，`POST`動詞命令用來在資料庫中建立記錄，或變更現有的。</span><span class="sxs-lookup"><span data-stu-id="ac97f-148">For example, the `POST` verb is used to create records in a database or change existing ones.</span></span> <span data-ttu-id="ac97f-149">大部分的情況下，當您填寫表單，並按一下 [提交] 按鈕，瀏覽器執行`POST`作業。</span><span class="sxs-lookup"><span data-stu-id="ac97f-149">Most of the time, when you fill in a form and click the submit button, the browser performs a `POST` operation.</span></span> <span data-ttu-id="ac97f-150">在 `POST`作業，傳遞至伺服器的資料是在頁面的主體。</span><span class="sxs-lookup"><span data-stu-id="ac97f-150">In a `POST` operation, the data being passed to the server is in the body of the page.</span></span>
> 
> <span data-ttu-id="ac97f-151">這些動詞重大的差別在於`GET`作業不應變更伺服器上的任何項目，或將它放在稍微更抽象的方式，`GET`作業不會導致在伺服器上的狀態變更。</span><span class="sxs-lookup"><span data-stu-id="ac97f-151">An important distinction between these verbs is that a `GET` operation is not supposed to change anything on the server — or to put it in a slightly more abstract way, a `GET` operation does not result in a change in state on the server.</span></span> <span data-ttu-id="ac97f-152">您可以執行`GET`上相同的資源的次數，而不會變更這些資源的作業。</span><span class="sxs-lookup"><span data-stu-id="ac97f-152">You can perform a `GET` operation on the same resources as many times as you like, and those resources don't change.</span></span> <span data-ttu-id="ac97f-153">(A`GET`作業通常稱為 「 安全 」，或使用的技術的詞彙，是*具有等冪性*。)相較之下，當然，`POST`要求會變更伺服器上的項目，每次執行作業。</span><span class="sxs-lookup"><span data-stu-id="ac97f-153">(A `GET` operation is often said to be "safe," or to use a technical term, is *idempotent*.) In contrast, of course, a `POST` request changes something on the server each time you perform the operation.</span></span>
> 
> <span data-ttu-id="ac97f-154">兩個範例將說明這項區別。</span><span class="sxs-lookup"><span data-stu-id="ac97f-154">Two examples will help illustrate this distinction.</span></span> <span data-ttu-id="ac97f-155">當您執行搜尋時使用的引擎，例如 Bing 或 Google，您填寫表單，包含一個文字方塊中，然後按一下 [搜尋] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ac97f-155">When you perform a search using an engine like Bing or Google, you fill in a form that consists of one text box, and then you click the search button.</span></span> <span data-ttu-id="ac97f-156">瀏覽器執行`GET`作業，您做為 URL 的一部分傳遞的方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-156">The browser performs a `GET` operation, with the value you entered into the box passed as part of the URL.</span></span> <span data-ttu-id="ac97f-157">使用`GET`作業因為搜尋作業不會變更伺服器上的任何資源，是沒問題，這種類型的表單，它只會擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-157">Using a `GET` operation for this type of form is fine, because a search operation doesn't change any resources on the server, it just fetches information.</span></span>
> 
> <span data-ttu-id="ac97f-158">現在，請考慮線上項目排序程的序。</span><span class="sxs-lookup"><span data-stu-id="ac97f-158">Now consider the process of ordering something online.</span></span> <span data-ttu-id="ac97f-159">您填寫訂單詳細資料，然後按一下 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ac97f-159">You fill in the order details and then click the submit button.</span></span> <span data-ttu-id="ac97f-160">這項作業將會`POST`要求，因為此作業會導致在伺服器上，例如新的訂單記錄、 變更您的帳戶資訊，以及可能是許多其他變更的變更。</span><span class="sxs-lookup"><span data-stu-id="ac97f-160">This operation will be a `POST` request, because the operation will result in changes on the server, such as a new order record, a change in your account information, and perhaps many other changes.</span></span> <span data-ttu-id="ac97f-161">不同於`GET`作業，您不能重複您`POST`要求 — 如果您這樣做，每次您重新提交要求，您會產生新的順序，在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="ac97f-161">Unlike the `GET` operation, you cannot repeat your `POST` request — if you did, each time you resubmitted the request, you'd generate a new order on the server.</span></span> <span data-ttu-id="ac97f-162">（在這種情況下，網站通常會警告您不要一次以上，按一下 [提交] 按鈕或將會停用 [提交] 按鈕，使您不小心重新提交表單。）</span><span class="sxs-lookup"><span data-stu-id="ac97f-162">(In cases like this, websites will often warn you not to click a submit button more than once, or will disable the submit button so that you don't resubmit the form accidentally.)</span></span>
> 
> <span data-ttu-id="ac97f-163">在本教學課程的過程中，您將兩者`GET`作業和`POST`作業來處理 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="ac97f-163">In the course of this tutorial, you'll use both a `GET` operation and a `POST` operation to work with HTML forms.</span></span> <span data-ttu-id="ac97f-164">我們將說明在每個案例的原因您所使用的動詞命令是適當。</span><span class="sxs-lookup"><span data-stu-id="ac97f-164">We'll explain in each case why the verb you use is the appropriate one.</span></span>
> 
> <span data-ttu-id="ac97f-165">(若要深入了解 HTTP 動詞命令，請參閱[方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的文章。)</span><span class="sxs-lookup"><span data-stu-id="ac97f-165">(To learn more about HTTP verbs, see the [Method Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article on the W3C site.)</span></span>


<span data-ttu-id="ac97f-166">大部分的使用者輸入項目會以 HTML`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="ac97f-166">Most user input elements are HTML `<input>` elements.</span></span> <span data-ttu-id="ac97f-167">它們看起來像是`<input type="type" name="name">,`何處*型別*表示您想要的使用者輸入控制項的類型。</span><span class="sxs-lookup"><span data-stu-id="ac97f-167">They look like `<input type="type" name="name">,` where *type* indicates the kind of user input control you want.</span></span> <span data-ttu-id="ac97f-168">這些項目是常見的：</span><span class="sxs-lookup"><span data-stu-id="ac97f-168">These elements are the common ones:</span></span>

- <span data-ttu-id="ac97f-169">文字方塊中： `<input type="text">`</span><span class="sxs-lookup"><span data-stu-id="ac97f-169">Text box: `<input type="text">`</span></span>
- <span data-ttu-id="ac97f-170">核取方塊： `<input type="check">`</span><span class="sxs-lookup"><span data-stu-id="ac97f-170">Check box: `<input type="check">`</span></span>
- <span data-ttu-id="ac97f-171">選項按鈕： `<input type="radio">`</span><span class="sxs-lookup"><span data-stu-id="ac97f-171">Radio button: `<input type="radio">`</span></span>
- <span data-ttu-id="ac97f-172">按鈕： `<input type="button">`</span><span class="sxs-lookup"><span data-stu-id="ac97f-172">Button: `<input type="button">`</span></span>
- <span data-ttu-id="ac97f-173">提交按鈕： `<input type="submit">`</span><span class="sxs-lookup"><span data-stu-id="ac97f-173">Submit button: `<input type="submit">`</span></span>

<span data-ttu-id="ac97f-174">您也可以使用`<textarea>`元素，來建立多行文字方塊和`<select>`来建立之下拉式清單或可捲動的清單項目。</span><span class="sxs-lookup"><span data-stu-id="ac97f-174">You can also use the `<textarea>` element to create a multiline text box and the `<select>` element to create a drop-down list or scrollable list.</span></span> <span data-ttu-id="ac97f-175">(如更多有關 HTML 表單項目，請參閱 < [HTML 表單和輸入](http://www.w3schools.com/html/html_forms.asp)W3Schools 站台上。)</span><span class="sxs-lookup"><span data-stu-id="ac97f-175">(For more about HTML form elements, see [HTML Forms and Input](http://www.w3schools.com/html/html_forms.asp) on the W3Schools site.)</span></span>

<span data-ttu-id="ac97f-176">`name`屬性是很重要，因為名稱會得到更新版本中，元素的值的方式，您很快會看到。</span><span class="sxs-lookup"><span data-stu-id="ac97f-176">The `name` attribute is very important, because the name is how you'll get the value of the element later, as you'll see shortly.</span></span>

<span data-ttu-id="ac97f-177">有趣的部分是網頁開發人員的您，請勿與使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="ac97f-177">The interesting part is what you, the page developer, do with the user's input.</span></span> <span data-ttu-id="ac97f-178">沒有任何與這些項目相關聯的內建行為。</span><span class="sxs-lookup"><span data-stu-id="ac97f-178">There's no built-in behavior associated with these elements.</span></span> <span data-ttu-id="ac97f-179">相反地，您必須取得使用者輸入或選取的值，並運用它們。</span><span class="sxs-lookup"><span data-stu-id="ac97f-179">Instead, you have to get the values that the user has entered or selected and do something with them.</span></span> <span data-ttu-id="ac97f-180">這是您將了解在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="ac97f-180">That's what you'll learn in this tutorial.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="ac97f-181">**HTML5 和輸入的表單**</span><span class="sxs-lookup"><span data-stu-id="ac97f-181">**HTML5 and Input Forms**</span></span>
> 
> <span data-ttu-id="ac97f-182">您可能已經知道，HTML 是轉換中，最新版本 (HTML5) 支援更直覺的方式，讓使用者輸入資訊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-182">As you might know, HTML is in transition and the latest version (HTML5) includes support for more intuitive ways for users to enter information.</span></span> <span data-ttu-id="ac97f-183">比方說，在 HTML5，您 （網頁開發人員） 所見頁面您希望使用者輸入日期。</span><span class="sxs-lookup"><span data-stu-id="ac97f-183">For example, in HTML5, you (the page developer) can tell the page that you want the user to enter a date.</span></span> <span data-ttu-id="ac97f-184">行事曆，而不需要使用者手動輸入日期，然後可以自動顯示瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ac97f-184">The browser can then automatically display a calendar rather than requiring the user to enter a date manually.</span></span> <span data-ttu-id="ac97f-185">不過，HTML5 新功能，而且尚不支援在所有瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="ac97f-185">However, HTML5 is new and is not supported in all browsers yet.</span></span>
> 
> <span data-ttu-id="ac97f-186">ASP.NET Web Pages 支援 HTML5 輸入使用者的瀏覽器所沒有的。</span><span class="sxs-lookup"><span data-stu-id="ac97f-186">ASP.NET Web Pages supports HTML5 input to the extent that the user's browser does.</span></span> <span data-ttu-id="ac97f-187">以了解的新屬性`<input>`項目以 HTML5，請參閱[HTML&lt;輸入&gt;type 屬性](http://www.w3schools.com/html/html_form_input_types.asp)W3Schools 站台上。</span><span class="sxs-lookup"><span data-stu-id="ac97f-187">For an idea of the new attributes for the `<input>` element in HTML5, see [HTML &lt;input&gt; type Attribute](http://www.w3schools.com/html/html_form_input_types.asp) on the W3Schools site.</span></span>


## <a name="creating-the-form"></a><span data-ttu-id="ac97f-188">建立表單</span><span class="sxs-lookup"><span data-stu-id="ac97f-188">Creating the Form</span></span>

<span data-ttu-id="ac97f-189">在 WebMatrix 中，在**檔案**工作區中，開啟*Movies.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-189">In WebMatrix, in the **Files** workspace, open the *Movies.cshtml* page.</span></span>

<span data-ttu-id="ac97f-190">在結尾後面`</h1>`標記之前開頭`<div>`標記`grid.GetHtml`呼叫時，加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="ac97f-190">After the closing `</h1>` tag and before the opening `<div>` tag of the `grid.GetHtml` call, add the following markup:</span></span>

[!code-html[Main](form-basics/samples/sample2.html)]

<span data-ttu-id="ac97f-191">此標記會建立名為文字方塊的表單`searchGenre`和提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="ac97f-191">This markup creates a form that has a text box named `searchGenre` and a submit button.</span></span> <span data-ttu-id="ac97f-192">文字並提交 按鈕會括住`<form>`項目其`method`屬性設為`get`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-192">The text box and submit button are enclosed in a `<form>` element whose `method` attribute is set to `get`.</span></span> <span data-ttu-id="ac97f-193">(請記住，如果您不將文字方塊和提交按鈕內`<form>`項目，當您按一下按鈕時將會提交執行任何動作。)您使用`GET`動詞這裡因為您要建立表單，不會進行任何變更伺服器上 — 它只會造成在搜尋中。</span><span class="sxs-lookup"><span data-stu-id="ac97f-193">(Remember that if you don't put the text box and submit button inside a `<form>` element, nothing will be submitted when you click the button.) You use the `GET` verb here because you're creating a form that does not make any changes on the server — it just results in a search.</span></span> <span data-ttu-id="ac97f-194">(在上一個教學課程中，您已使用`post`方法，這是您要如何提交到伺服器的變更。</span><span class="sxs-lookup"><span data-stu-id="ac97f-194">(In the previous tutorial, you used a `post` method, which is how you submit changes to the server.</span></span> <span data-ttu-id="ac97f-195">您會看到，在下一個教學課程中一次。）</span><span class="sxs-lookup"><span data-stu-id="ac97f-195">You'll see that in the next tutorial again.)</span></span>

<span data-ttu-id="ac97f-196">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="ac97f-196">Run the page.</span></span> <span data-ttu-id="ac97f-197">雖然您尚未定義表單的任何行為，您可以看到它的外觀：</span><span class="sxs-lookup"><span data-stu-id="ac97f-197">Although you haven't defined any behavior for the form, you can see what it looks like:</span></span>

![「 內容類型的 [搜尋] 方塊的影片頁面](form-basics/_static/image3.png)

<span data-ttu-id="ac97f-199">輸入的值，在文字方塊中，例如"喜劇。 」</span><span class="sxs-lookup"><span data-stu-id="ac97f-199">Enter a value into the text box, like "Comedy."</span></span> <span data-ttu-id="ac97f-200">然後按一下**搜尋內容類型**。</span><span class="sxs-lookup"><span data-stu-id="ac97f-200">Then click **Search Genre**.</span></span>

<span data-ttu-id="ac97f-201">記下的網頁 URL。</span><span class="sxs-lookup"><span data-stu-id="ac97f-201">Take note of the URL of the page.</span></span> <span data-ttu-id="ac97f-202">因為您設定`<form>`項目的`method`屬性設定為`get`，您所輸入的值現在是在 URL 中，像這樣的查詢字串的一部分：</span><span class="sxs-lookup"><span data-stu-id="ac97f-202">Because you set the `<form>` element's `method` attribute to `get`, the value you entered is now part of the query string in the URL, like this:</span></span>

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a><span data-ttu-id="ac97f-203">讀取表單值</span><span class="sxs-lookup"><span data-stu-id="ac97f-203">Reading Form Values</span></span>

<span data-ttu-id="ac97f-204">頁面已經包含一些程式碼，取得資料庫的資料，並將結果顯示在方格中。</span><span class="sxs-lookup"><span data-stu-id="ac97f-204">The page already contains some code that gets database data and displays the results in a grid.</span></span> <span data-ttu-id="ac97f-205">現在，您必須新增一些程式碼，讀取文字方塊的值，因此您可以執行 SQL 查詢，其中包含搜尋詞彙。</span><span class="sxs-lookup"><span data-stu-id="ac97f-205">Now you have to add some code that reads the value of the text box so you can run a SQL query that includes the search term.</span></span>

<span data-ttu-id="ac97f-206">因為您設定表單的方法為`get`，您可以閱讀使用如下所示的程式碼輸入到文字方塊中的值：</span><span class="sxs-lookup"><span data-stu-id="ac97f-206">Because you set the form's method to `get`, you can read the value that was entered into the text box by using code like the following:</span></span>

`var searchTerm = Request.QueryString["searchGenre"];`

<span data-ttu-id="ac97f-207">`Request.QueryString`物件 (`QueryString`屬性`Request`物件) 包含已提交為一部分的項目值`GET`作業。</span><span class="sxs-lookup"><span data-stu-id="ac97f-207">The `Request.QueryString` object (the `QueryString` property of the `Request` object) includes the values of elements that were submitted as part of the `GET` operation.</span></span> <span data-ttu-id="ac97f-208">`Request.QueryString`屬性包含*集合*（清單） 的形式送出的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-208">The `Request.QueryString` property contains a *collection* (a list) of the values that are submitted in the form.</span></span> <span data-ttu-id="ac97f-209">若要取得任何個別值，您可以指定您想要的項目名稱。</span><span class="sxs-lookup"><span data-stu-id="ac97f-209">To get any individual value, you specify the name of the element that you want.</span></span> <span data-ttu-id="ac97f-210">這就是為什麼您必須擁有`name`屬性`<input>`項目 (`searchTerm`)，會建立文字方塊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-210">That's why you have to have a `name` attribute on the `<input>` element (`searchTerm`) that creates the text box.</span></span> <span data-ttu-id="ac97f-211">(如需詳細資訊`Request`物件，請參閱 <<c2> [ 資訊看板](#BKMK_TheRequestObject)更新版本。)</span><span class="sxs-lookup"><span data-stu-id="ac97f-211">(For more about the `Request` object, see the [sidebar](#BKMK_TheRequestObject) later.)</span></span>

<span data-ttu-id="ac97f-212">很容易閱讀的文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-212">It's simple enough to read the value of the text box.</span></span> <span data-ttu-id="ac97f-213">但是，如果使用者未輸入任何項目完全在文字方塊中，但按下**搜尋**無論如何，您可以忽略，按一下，因為沒有要搜尋項目。</span><span class="sxs-lookup"><span data-stu-id="ac97f-213">But if the user didn't enter anything at all in the text box but clicked **Search** anyway, you can ignore that click, since there's nothing to search.</span></span>

<span data-ttu-id="ac97f-214">下列程式碼是範例，示範如何實作這些條件。</span><span class="sxs-lookup"><span data-stu-id="ac97f-214">The following code is an example that shows how to implement these conditions.</span></span> <span data-ttu-id="ac97f-215">（您不需要尚未新增此程式碼，在您稍後將執行的）。</span><span class="sxs-lookup"><span data-stu-id="ac97f-215">(You don't have to add this code yet; you'll do that in a moment.)</span></span>

[!code-csharp[Main](form-basics/samples/sample3.cs)]

<span data-ttu-id="ac97f-216">測試會細分以這種方式：</span><span class="sxs-lookup"><span data-stu-id="ac97f-216">The test breaks down in this way:</span></span>

- <span data-ttu-id="ac97f-217">取得的值`Request.QueryString["searchGenre"]`，也就是輸入的值`<input>`名為的項目`searchGenre`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-217">Get the value of `Request.QueryString["searchGenre"]`, namely the value that was entered into the `<input>` element named `searchGenre`.</span></span>
- <span data-ttu-id="ac97f-218">了解是否空白，便使用`IsEmpty`方法。</span><span class="sxs-lookup"><span data-stu-id="ac97f-218">Find out if it's empty by using the `IsEmpty` method.</span></span> <span data-ttu-id="ac97f-219">這個方法會判斷項目 （例如，表單項目） 是否包含值的標準方式。</span><span class="sxs-lookup"><span data-stu-id="ac97f-219">This method is the standard way to determine whether something (for example, a form element) contains a value.</span></span> <span data-ttu-id="ac97f-220">但是，您在意有它才*不*空的因此...</span><span class="sxs-lookup"><span data-stu-id="ac97f-220">But really, you care only if it's *not* empty, therefore ...</span></span>
- <span data-ttu-id="ac97f-221">新增`!`運算子前面`IsEmpty`測試。</span><span class="sxs-lookup"><span data-stu-id="ac97f-221">Add the `!` operator in front of the `IsEmpty` test.</span></span> <span data-ttu-id="ac97f-222">(`!`運算子代表邏輯 NOT)。</span><span class="sxs-lookup"><span data-stu-id="ac97f-222">(The `!` operator means logical NOT).</span></span>

<span data-ttu-id="ac97f-223">以純文字的英文，整個`if`條件會轉譯成下列：*表單的 searchGenre 項目不是空的則如果...*</span><span class="sxs-lookup"><span data-stu-id="ac97f-223">In plain English, the entire `if` condition translates into the following: *If the form's searchGenre element is not empty, then ...*</span></span>

<span data-ttu-id="ac97f-224">此區塊會設定來建立使用搜尋詞彙的查詢階段。</span><span class="sxs-lookup"><span data-stu-id="ac97f-224">This block sets the stage for creating a query that uses the search term.</span></span> <span data-ttu-id="ac97f-225">您將在下一節來這麼做。</span><span class="sxs-lookup"><span data-stu-id="ac97f-225">You'll do that in the next section.</span></span>

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> <span data-ttu-id="ac97f-226">**要求物件**</span><span class="sxs-lookup"><span data-stu-id="ac97f-226">**The Request Object**</span></span>
> 
> <span data-ttu-id="ac97f-227">`Request`物件包含當您要求或送出頁面時，瀏覽器傳送您的應用程式的所有資訊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-227">The `Request` object contains all the information that the browser sends to your application when a page is requested or submitted.</span></span> <span data-ttu-id="ac97f-228">此物件包含使用者提供，例如文字方塊值或上傳檔案的任何資訊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-228">This object includes any information that the user provides, like text box values or a file to upload.</span></span> <span data-ttu-id="ac97f-229">它也包含各式各樣的其他資訊，例如 cookie 值中的 URL 查詢字串 （如果有的話），正在執行的瀏覽器類型的使用者使用，會在瀏覽器中設定的語言的清單頁面的檔案路徑以及其他更多功能。</span><span class="sxs-lookup"><span data-stu-id="ac97f-229">It also includes all sorts of additional information, like cookies, values in the URL query string (if any), the file path of the page that is running, the type of browser that the user is using, the list of languages that are set in the browser, and much more.</span></span>
> 
> <span data-ttu-id="ac97f-230">`Request`物件是否*集合*（清單） 的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-230">The `Request` object is a *collection* (list) of values.</span></span> <span data-ttu-id="ac97f-231">您可以指定其名稱，以取得集合的個別值：</span><span class="sxs-lookup"><span data-stu-id="ac97f-231">You get an individual value out of the collection by specifying its name:</span></span>
> 
> `var someValue = Request["name"];`
> 
> <span data-ttu-id="ac97f-232">`Request`物件實際上會公開數個子集。</span><span class="sxs-lookup"><span data-stu-id="ac97f-232">The `Request` object actually exposes several subsets.</span></span> <span data-ttu-id="ac97f-233">例如: </span><span class="sxs-lookup"><span data-stu-id="ac97f-233">For example:</span></span>
> 
> - <span data-ttu-id="ac97f-234">`Request.Form` 可讓您從項目內已提交的值`<form>`項目，如果要求是`POST`要求。</span><span class="sxs-lookup"><span data-stu-id="ac97f-234">`Request.Form` gives you values from elements inside the submitted `<form>` element if the request is a `POST` request.</span></span>
> - <span data-ttu-id="ac97f-235">`Request.QueryString` 提供值的 URL 查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ac97f-235">`Request.QueryString` gives you just the values in the URL's query string.</span></span> <span data-ttu-id="ac97f-236">(在之類的 URL `http://mysite/myapp/page?searchGenre=action&page=2`，則`?searchGenre=action&page=2`URL 區段是查詢字串。)</span><span class="sxs-lookup"><span data-stu-id="ac97f-236">(In a URL like `http://mysite/myapp/page?searchGenre=action&page=2`, the `?searchGenre=action&page=2` section of the URL is the query string.)</span></span>
> - <span data-ttu-id="ac97f-237">`Request.Cookies` 集合可讓您存取瀏覽器所傳送的 cookie。</span><span class="sxs-lookup"><span data-stu-id="ac97f-237">`Request.Cookies` collection gives you access to cookies that the browser has sent.</span></span>
> 
> <span data-ttu-id="ac97f-238">若要取得值，您知道處於 已提交表單，您可以使用`Request["name"]`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-238">To get a value that you know is in the submitted form, you can use `Request["name"]`.</span></span> <span data-ttu-id="ac97f-239">或者，您可以使用更特定的版本`Request.Form["name"]`(如`POST`要求) 或`Request.QueryString["name"]`(如`GET`要求)。</span><span class="sxs-lookup"><span data-stu-id="ac97f-239">Alternatively, you can use the more specific versions `Request.Form["name"]` (for `POST` requests) or `Request.QueryString["name"]` (for `GET` requests).</span></span> <span data-ttu-id="ac97f-240">當然*名稱*是要取得之項目的名稱。</span><span class="sxs-lookup"><span data-stu-id="ac97f-240">Of course, *name* is the name of the item to get.</span></span>
> 
> <span data-ttu-id="ac97f-241">您想要取得的項目的名稱必須是唯一您使用在集合中。</span><span class="sxs-lookup"><span data-stu-id="ac97f-241">The name of the item you want to get has to be unique within the collection you're using.</span></span> <span data-ttu-id="ac97f-242">這就是為什麼`Request`物件所提供的子集喜歡`Request.Form`和`Request.QueryString`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-242">That's why the `Request` object provides the subsets like `Request.Form` and `Request.QueryString`.</span></span> <span data-ttu-id="ac97f-243">假設您的頁面包含名為表單項目`userName`並*也*包含名為 cookie `userName`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-243">Suppose that your page contains a form element named `userName` and *also* contains a cookie named `userName`.</span></span> <span data-ttu-id="ac97f-244">如果您收到`Request["userName"]`，模稜兩可您要的表單值或 cookie。</span><span class="sxs-lookup"><span data-stu-id="ac97f-244">If you get `Request["userName"]`, it's ambiguous whether you want the form value or the cookie.</span></span> <span data-ttu-id="ac97f-245">不過，如果您收到`Request.Form["userName"]`或`Request.Cookie["userName"]`，您要瞭如指掌要取得的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-245">However, if you get `Request.Form["userName"]` or `Request.Cookie["userName"]`, you're being explicit about which value to get.</span></span>
> 
> <span data-ttu-id="ac97f-246">它是個不錯的做法是特定且使用的子集`Request`感興趣，例如`Request.Form`或`Request.QueryString`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-246">It's a good practice to be specific and use the subset of `Request` that you're interested in, like `Request.Form` or `Request.QueryString`.</span></span> <span data-ttu-id="ac97f-247">如您在本教學課程中建立簡單的頁面，它可能不會真的會造成任何差異。</span><span class="sxs-lookup"><span data-stu-id="ac97f-247">For the simple pages that you're creating in this tutorial, it probably doesn't really make any difference.</span></span> <span data-ttu-id="ac97f-248">不過，當您建立更複雜的頁面時，使用明確的版本`Request.Form`或`Request.QueryString`可協助您避免發生問題時的頁面包含表單 （或多個表單），可能發生的 cookie、 查詢字串值等等。</span><span class="sxs-lookup"><span data-stu-id="ac97f-248">However, as you create more complex pages, using the explicit version `Request.Form` or `Request.QueryString` can help you avoid problems that can arise when the page contains a form (or multiple forms), cookies, query string values, and so on.</span></span>


## <a name="creating-a-query-by-using-a-search-term"></a><span data-ttu-id="ac97f-249">建立查詢利用搜尋詞彙</span><span class="sxs-lookup"><span data-stu-id="ac97f-249">Creating a Query by Using a Search Term</span></span>

<span data-ttu-id="ac97f-250">既然您已經知道如何取得使用者輸入搜尋詞彙，您可以建立使用它的查詢。</span><span class="sxs-lookup"><span data-stu-id="ac97f-250">Now that you know how to get the search term that the user entered, you can create a query that uses it.</span></span> <span data-ttu-id="ac97f-251">請記住，若要取得移出資料庫的所有影片項目，您使用 SQL 查詢，看起來像這個陳述式：</span><span class="sxs-lookup"><span data-stu-id="ac97f-251">Remember that to get all the movie items out of the database, you're using a SQL query that looks like this statement:</span></span>

`SELECT * FROM Movies`

<span data-ttu-id="ac97f-252">若要取得特定的電影，您必須使用包含的查詢`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="ac97f-252">To get only certain movies, you have to use a query that includes a `Where` clause.</span></span> <span data-ttu-id="ac97f-253">此子句可讓您設定的條件的查詢所傳回的資料列。</span><span class="sxs-lookup"><span data-stu-id="ac97f-253">This clause lets you set a condition on which rows are returned by the query.</span></span> <span data-ttu-id="ac97f-254">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="ac97f-254">Here's an example:</span></span>

`SELECT * FROM Movies WHERE Genre = 'Action'`

<span data-ttu-id="ac97f-255">基本格式`WHERE column = value`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-255">The basic format is `WHERE column = value`.</span></span> <span data-ttu-id="ac97f-256">Besides 只是，您可以使用不同的運算子`=`，例如`>`（大於）、 `<` （小於）、 `<>` （不等於）、 `<=` （小於或等於）、 逗號等等，視您要尋找的內容。</span><span class="sxs-lookup"><span data-stu-id="ac97f-256">You can use different operators besides just `=`, like `>` (greater than), `<` (less than), `<>` (not equal to), `<=` (less than or equal to), etc., depending on what you're looking for.</span></span>

<span data-ttu-id="ac97f-257">您可能會感到奇怪，SQL 陳述式都不會區分大小寫&mdash;`SELECT`等同於`Select`(或甚至`select`)。</span><span class="sxs-lookup"><span data-stu-id="ac97f-257">In case you're wondering, SQL statements are not case sensitive &mdash; `SELECT` is the same as `Select` (or even `select`).</span></span> <span data-ttu-id="ac97f-258">不過，人們通常利用 SQL 陳述式中的關鍵字類似`SELECT`和`WHERE`，以便於讀取。</span><span class="sxs-lookup"><span data-stu-id="ac97f-258">However, people often capitalize keywords in a SQL statement, like `SELECT` and `WHERE`, to make it easier to read.</span></span>

### <a name="passing-the-search-term-as-a-parameter"></a><span data-ttu-id="ac97f-259">傳遞做為參數的搜尋字詞</span><span class="sxs-lookup"><span data-stu-id="ac97f-259">Passing the search term as a parameter</span></span>

<span data-ttu-id="ac97f-260">搜尋特定的內容類型也很簡單 (`WHERE Genre = 'Action'`)，但您想要能夠搜尋任何使用者輸入的內容類型。</span><span class="sxs-lookup"><span data-stu-id="ac97f-260">Searching for a specific genre is easy enough (`WHERE Genre = 'Action'`), but you want to be able to search for any genre that the user enters.</span></span> <span data-ttu-id="ac97f-261">若要這樣做，您會建立為包含要搜尋的預留位置值的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="ac97f-261">To do that, you create as SQL query that includes a placeholder for the value to search.</span></span> <span data-ttu-id="ac97f-262">它看起來像這個命令：</span><span class="sxs-lookup"><span data-stu-id="ac97f-262">It will look like this command:</span></span>

`SELECT * FROM Movies WHERE Genre = @0`

<span data-ttu-id="ac97f-263">預留位置是`@`字元後面接著零。</span><span class="sxs-lookup"><span data-stu-id="ac97f-263">The placeholder is the `@` character followed by zero.</span></span> <span data-ttu-id="ac97f-264">您可能會猜想，查詢可以包含多個預留位置，並會命名為`@0`， `@1`，`@2`等等。</span><span class="sxs-lookup"><span data-stu-id="ac97f-264">As you might guess, a query can contain multiple placeholders, and they'd be named `@0`, `@1`, `@2`, etc.</span></span>

<span data-ttu-id="ac97f-265">若要設定查詢，並實際將它傳遞值，您可以使用如下所示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac97f-265">To set up the query and actually pass it the value, you use the code like the following:</span></span>

[!code-sql[Main](form-basics/samples/sample4.sql)]

<span data-ttu-id="ac97f-266">此程式碼很類似什麼您已經完成此方格中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="ac97f-266">This code is similar to what you've already done to display data in the grid.</span></span> <span data-ttu-id="ac97f-267">唯一的差異如下：</span><span class="sxs-lookup"><span data-stu-id="ac97f-267">The only differences are:</span></span>

- <span data-ttu-id="ac97f-268">此查詢會包含預留位置 (`WHERE Genre = @0"`)。</span><span class="sxs-lookup"><span data-stu-id="ac97f-268">The query contains a placeholder (`WHERE Genre = @0"`).</span></span>
- <span data-ttu-id="ac97f-269">查詢會放入變數 (`selectCommand`); 之前，您也可以直接將查詢傳遞`db.Query`方法。</span><span class="sxs-lookup"><span data-stu-id="ac97f-269">The query is put into a variable (`selectCommand`); before, you passed the query directly to the `db.Query` method.</span></span>
- <span data-ttu-id="ac97f-270">當您呼叫`db.Query`方法中，您傳送查詢和要使用的預留位置的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-270">When you call the `db.Query` method, you pass both the query and the value to use for the placeholder.</span></span> <span data-ttu-id="ac97f-271">(如果查詢有多個預留位置，您會將其傳遞做為個別值的方法。)</span><span class="sxs-lookup"><span data-stu-id="ac97f-271">(If the query had multiple placeholders, you'd pass them all as separate values to the method.)</span></span>

<span data-ttu-id="ac97f-272">如果您同時將所有這些項目，您會取得下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac97f-272">If you put all these elements together, you get the following code:</span></span>

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="ac97f-273">**重要！**</span><span class="sxs-lookup"><span data-stu-id="ac97f-273">**Important!**</span></span> <span data-ttu-id="ac97f-274">使用預留位置 (例如`@0`) 若要將值傳遞給 SQL 命令*極為重要*安全性。</span><span class="sxs-lookup"><span data-stu-id="ac97f-274">Using placeholders (like `@0`) to pass values to a SQL command is *extremely important* for security.</span></span> <span data-ttu-id="ac97f-275">您在此見報，具有預留位置變數的資料，方法是您應該建構 SQL 命令的唯一方法。</span><span class="sxs-lookup"><span data-stu-id="ac97f-275">The way you see it here, with placeholders for variable data, is the only way you should construct SQL commands.</span></span>
> 
> <span data-ttu-id="ac97f-276">永遠不會建構 SQL 陳述式放在一起，（串連） 的常值文字和您從使用者取得的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-276">Never construct a SQL statement by putting together (concatenating) literal text and values you get from the user.</span></span> <span data-ttu-id="ac97f-277">串連使用者輸入 SQL 陳述式會開啟您的站台*SQL 資料隱碼攻擊*當惡意使用者送出至您的頁面 hack 您資料庫的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-277">Concatenating user input into a SQL statement opens your site to a *SQL injection attack* where a malicious user submits values to your page that hack your database.</span></span> <span data-ttu-id="ac97f-278">(您可以閱讀更多文件中[SQL 資料隱碼](https://msdn.microsoft.com/library/ms161953.aspx)MSDN 網站。)</span><span class="sxs-lookup"><span data-stu-id="ac97f-278">(You can read more in the article [SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) the MSDN website.)</span></span>


## <a name="updating-the-movies-page-with-search-code"></a><span data-ttu-id="ac97f-279">使用 搜尋程式碼更新影片頁面</span><span class="sxs-lookup"><span data-stu-id="ac97f-279">Updating the Movies Page with Search Code</span></span>

<span data-ttu-id="ac97f-280">現在您可以更新中的程式碼*Movies.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="ac97f-280">Now you can update the code in the *Movies.cshtml* file.</span></span> <span data-ttu-id="ac97f-281">若要開始，請在頁面頂端的程式碼區塊中的程式碼取代此程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac97f-281">To begin, replace the code in the code block at the top of the page with this code:</span></span>

[!code-csharp[Main](form-basics/samples/sample6.cs)]

<span data-ttu-id="ac97f-282">此處的差異在於，您將查詢貼入`selectCommand`變數，您要傳遞至`db.Query`更新版本。</span><span class="sxs-lookup"><span data-stu-id="ac97f-282">The difference here is that you've put the query into the `selectCommand` variable, which you'll pass to `db.Query` later.</span></span> <span data-ttu-id="ac97f-283">將放入變數的 SQL 陳述式，可讓您變更陳述式，也就是您需要執行搜尋。</span><span class="sxs-lookup"><span data-stu-id="ac97f-283">Putting the SQL statement into a variable lets you change the statement, which is what you'll do to perform the search.</span></span>

<span data-ttu-id="ac97f-284">您也移除了下列兩行，您會將放回在更新版本：</span><span class="sxs-lookup"><span data-stu-id="ac97f-284">You've also removed these two lines, which you'll put back in later:</span></span>

[!code-csharp[Main](form-basics/samples/sample7.cs)]

<span data-ttu-id="ac97f-285">您不打算尚未執行查詢 (也就是呼叫`db.Query`)，您不想要初始化`WebGrid`協助程式，但其中一個。</span><span class="sxs-lookup"><span data-stu-id="ac97f-285">You don't want to run the query yet (that is, call `db.Query`) and you don't want to initialize the `WebGrid` helper yet either.</span></span> <span data-ttu-id="ac97f-286">您已了解哪些 SQL 陳述式已執行之後，您會執行這些項目。</span><span class="sxs-lookup"><span data-stu-id="ac97f-286">You'll do those things after you've figured out which SQL statement has to run.</span></span>

<span data-ttu-id="ac97f-287">之後重寫區塊中，您可以加入新的邏輯來處理搜尋。</span><span class="sxs-lookup"><span data-stu-id="ac97f-287">After this rewritten block, you can add the new logic for handling the search.</span></span> <span data-ttu-id="ac97f-288">已完成的程式碼看起來如下所示。</span><span class="sxs-lookup"><span data-stu-id="ac97f-288">The completed code will look like the following.</span></span> <span data-ttu-id="ac97f-289">使它符合此範例中，請更新您的頁面中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac97f-289">Update the code in your page so it matches this example:</span></span>

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

<span data-ttu-id="ac97f-290">頁面現在的運作方式如下。</span><span class="sxs-lookup"><span data-stu-id="ac97f-290">The page now works like this.</span></span> <span data-ttu-id="ac97f-291">每次執行網頁時，程式碼會開啟資料庫和`selectCommand`變數設為 SQL 陳述式可取得所有記錄`Movies`資料表。</span><span class="sxs-lookup"><span data-stu-id="ac97f-291">Every time the page runs, the code opens the database and the `selectCommand` variable is set to the SQL statement that gets all the records from the `Movies` table.</span></span> <span data-ttu-id="ac97f-292">程式碼也會初始化`searchTerm`變數。</span><span class="sxs-lookup"><span data-stu-id="ac97f-292">The code also initializes the `searchTerm` variable.</span></span>

<span data-ttu-id="ac97f-293">不過，如果目前的要求中包含的值`searchGenre`項目、 程式碼集`selectCommand`不同的查詢 — 那就是，為包含`Where`子句來搜尋的內容類型。</span><span class="sxs-lookup"><span data-stu-id="ac97f-293">However, if the current request includes a value for the `searchGenre` element, the code sets `selectCommand` to a different query — namely, to one that includes the `Where` clause to search for a genre.</span></span> <span data-ttu-id="ac97f-294">它也會設定`searchTerm`任何已傳遞的搜尋方塊中 （這可能是 nothing）。</span><span class="sxs-lookup"><span data-stu-id="ac97f-294">It also sets `searchTerm` to whatever was passed for the search box (which might be nothing).</span></span>

<span data-ttu-id="ac97f-295">不論 SQL 陳述式是在`selectCommand`，然後呼叫程式碼`db.Query`若要執行查詢時，將它傳遞任何位於`searchTerm`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-295">Regardless of which SQL statement is in `selectCommand`, the code then calls `db.Query` to run the query, passing it whatever is in `searchTerm`.</span></span> <span data-ttu-id="ac97f-296">如果沒有`searchTerm`，它並不重要，因為在此情況下沒有任何參數，將值傳遞至`selectCommand`嗎。</span><span class="sxs-lookup"><span data-stu-id="ac97f-296">If there's nothing in `searchTerm`, it doesn't matter, because in that case there's no parameter to pass the value to `selectCommand` anyway.</span></span>

<span data-ttu-id="ac97f-297">最後，程式碼會初始化`WebGrid`使用查詢結果中的，像以前一樣的協助程式。</span><span class="sxs-lookup"><span data-stu-id="ac97f-297">Finally, the code initializes the `WebGrid` helper by using the query results, just like before.</span></span>

<span data-ttu-id="ac97f-298">您所見，將 SQL 陳述式，並搜尋詞彙，到變數中，您已加入彈性的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ac97f-298">You can see that by putting the SQL statement and the search term into variables, you've added flexibility to the code.</span></span> <span data-ttu-id="ac97f-299">如您所見本教學課程稍後，您可以使用此基本架構，並持續新增不同類型的搜尋邏輯。</span><span class="sxs-lookup"><span data-stu-id="ac97f-299">As you'll see later in this tutorial, you can use this basic framework and keep adding logic for different types of searches.</span></span>

## <a name="testing-the-search-by-genre-feature"></a><span data-ttu-id="ac97f-300">要測試的內容類型的搜尋功能</span><span class="sxs-lookup"><span data-stu-id="ac97f-300">Testing the Search-by-Genre Feature</span></span>

<span data-ttu-id="ac97f-301">在 WebMatrix 中，執行*Movies.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-301">In WebMatrix, run the *Movies.cshtml* page.</span></span> <span data-ttu-id="ac97f-302">您會看到內容類型的文字方塊中的頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-302">You see the page with the text box for genre.</span></span>

<span data-ttu-id="ac97f-303">輸入，您已為您的測試記錄的其中一個輸入，然後按一下 內容類型**搜尋**。</span><span class="sxs-lookup"><span data-stu-id="ac97f-303">Enter a genre that you've entered for one of your test records, then click **Search**.</span></span> <span data-ttu-id="ac97f-304">此時您會看到只比對該內容類型的影片清單：</span><span class="sxs-lookup"><span data-stu-id="ac97f-304">This time you see a listing of just the movies that match that genre:</span></span>

![列出搜尋的內容類型 'Comedies' 之後的影片頁面](form-basics/_static/image4.png)

<span data-ttu-id="ac97f-306">輸入不同的內容類型，然後再搜尋一次。</span><span class="sxs-lookup"><span data-stu-id="ac97f-306">Enter a different genre and search again.</span></span> <span data-ttu-id="ac97f-307">請嘗試輸入的內容類型，藉由使用全部小寫或全部大寫的字母，如此一來，您可以看到搜尋不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ac97f-307">Try entering the genre by using all lowercase or all uppercase letters so that you can see that the search is not case sensitive.</span></span>

## <a name="remembering-what-the-user-entered"></a><span data-ttu-id="ac97f-308">「 記住 」 使用者所輸入的內容</span><span class="sxs-lookup"><span data-stu-id="ac97f-308">"Remembering" What the User Entered</span></span>

<span data-ttu-id="ac97f-309">您可能已經注意到，在您輸入的內容類型，並按下之後**搜尋內容類型**，您會看到該內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="ac97f-309">You might have noticed that after you entered a genre and clicked **Search Genre**, you saw a listing for that genre.</span></span> <span data-ttu-id="ac97f-310">不過，[搜尋] 方塊是空&mdash;換句話說，它不記得您必須輸入。</span><span class="sxs-lookup"><span data-stu-id="ac97f-310">However, the search text box was empty &mdash; in other words, it didn't remember what you'd entered.</span></span>

<span data-ttu-id="ac97f-311">請務必了解為什麼會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="ac97f-311">It's important to understand why this behavior occurs.</span></span> <span data-ttu-id="ac97f-312">當您提交頁面時，瀏覽器會傳送要求到 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ac97f-312">When you submit a page, the browser sends a request to the web server.</span></span> <span data-ttu-id="ac97f-313">當 ASP.NET 取得要求時，它會建立頁面的全新執行個體、 執行的程式碼，並再一次呈現至瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-313">When ASP.NET gets the request, it creates a brand-new instance of the page, runs the code in it, and then renders the page to the browser again.</span></span> <span data-ttu-id="ac97f-314">實際上，不過，頁面並不知道，您就已使用的舊版本身。</span><span class="sxs-lookup"><span data-stu-id="ac97f-314">In effect, though, the page doesn't know that you were just working with a previous version of itself.</span></span> <span data-ttu-id="ac97f-315">所有它知道它有要求，其中有一些表單中的資料。</span><span class="sxs-lookup"><span data-stu-id="ac97f-315">All it knows is that it got a request that had some form data in it.</span></span>

<span data-ttu-id="ac97f-316">每次要求頁面&mdash;第一次還是將其提交出去&mdash;您收到新的頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-316">Every time you request a page &mdash; whether for the first time or by submitting it &mdash; you're getting a new page.</span></span> <span data-ttu-id="ac97f-317">Web 伺服器會有上次要求的任何記憶體。</span><span class="sxs-lookup"><span data-stu-id="ac97f-317">The web server has no memory of your last request.</span></span> <span data-ttu-id="ac97f-318">兩者都不會執行 ASP.NET，並都不會在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ac97f-318">Neither does ASP.NET, and neither does the browser.</span></span> <span data-ttu-id="ac97f-319">這些個別的執行個體的頁面之間唯一的連線是您傳送其間的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ac97f-319">The only connection between these separate instances of the page is any data that you transmit between them.</span></span> <span data-ttu-id="ac97f-320">如果您送出頁面，比方說，新的頁面執行個體可以取得舊版的執行個體送出表單資料。</span><span class="sxs-lookup"><span data-stu-id="ac97f-320">If you submit a page, for example, the new page instance can get the form data that was sent by the earlier instance.</span></span> <span data-ttu-id="ac97f-321">（另一種頁面之間傳遞資料的方式是使用 cookie）。</span><span class="sxs-lookup"><span data-stu-id="ac97f-321">(Another way to pass data between pages is to use cookies.)</span></span>

<span data-ttu-id="ac97f-322">型式的方式，來說明這種情況下為網頁都*無狀態*。</span><span class="sxs-lookup"><span data-stu-id="ac97f-322">A formal way to describe this situation is to say that web pages are *stateless*.</span></span> <span data-ttu-id="ac97f-323">Web 伺服器和網頁本身和頁面中的項目不會維護任何先前的頁面狀態的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-323">Web servers and the pages themselves and the elements in the page do not maintain any information about the previous state of a page.</span></span> <span data-ttu-id="ac97f-324">Web 的設計是這種方式，因為維護個別要求的狀態會快速耗盡資源的 web 伺服器，通常可能處理上千條，甚至數百個，每秒的要求。</span><span class="sxs-lookup"><span data-stu-id="ac97f-324">The web was designed this way because maintaining state for individual requests would quickly exhaust the resources of web servers, which often handle thousands, maybe even hundreds of thousands, of requests per second.</span></span>

<span data-ttu-id="ac97f-325">所以這就是為什麼文字方塊為空白。</span><span class="sxs-lookup"><span data-stu-id="ac97f-325">So that's why the text box was empty.</span></span> <span data-ttu-id="ac97f-326">送出頁面之後，ASP.NET 會建立頁面的新執行個體，並執行程式碼和標記。</span><span class="sxs-lookup"><span data-stu-id="ac97f-326">After you submitted the page, ASP.NET created a new instance of the page and ran through the code and markup.</span></span> <span data-ttu-id="ac97f-327">沒有任何在該告知 ASP.NET 將值放入文字方塊中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ac97f-327">There was nothing in that code that told ASP.NET to put a value into the text box.</span></span> <span data-ttu-id="ac97f-328">因此，不執行任何動作，ASP.NET 並沒有值，以在其中呈現文字方塊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-328">So ASP.NET didn't do anything, and the text box was rendered without a value in it.</span></span>

<span data-ttu-id="ac97f-329">沒有實際輕鬆地取得解決此問題。</span><span class="sxs-lookup"><span data-stu-id="ac97f-329">There's actually an easy way to get around this issue.</span></span> <span data-ttu-id="ac97f-330">您在文字方塊中輸入的內容類型*已*為您提供的程式碼&mdash;處於`Request.QueryString["searchGenre"]`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-330">The genre that you entered into the text box *is* available to you in code &mdash; it's in `Request.QueryString["searchGenre"]`.</span></span>

<span data-ttu-id="ac97f-331">更新 [] 文字方塊中的標記以便`value`取得其值的屬性， `searchTerm`，如這個範例所示：</span><span class="sxs-lookup"><span data-stu-id="ac97f-331">Update the markup for the text box so that the `value` attribute gets its value from `searchTerm`, like this example:</span></span>

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

<span data-ttu-id="ac97f-332">在此頁面上，您可能有也設定`value`屬性設定為`searchTerm`您輸入變數，因為該變數也包含內容類型。</span><span class="sxs-lookup"><span data-stu-id="ac97f-332">In this page, you could have also set the `value` attribute to the `searchTerm` variable, since that variable also contains the genre you entered.</span></span> <span data-ttu-id="ac97f-333">但使用`Request`物件來設定`value`屬性所示，以下是完成這項工作的標準方式。</span><span class="sxs-lookup"><span data-stu-id="ac97f-333">But using the `Request` object to set the `value` attribute as shown here is the standard way to accomplish this task.</span></span> <span data-ttu-id="ac97f-334">(假設您甚至想要執行這項操作&mdash;在某些情況下，您可能想要呈現的頁面*而不需要*中欄位的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-334">(Assuming you even want to do this &mdash; in some situations, you might want to render the page *without* values in the fields.</span></span> <span data-ttu-id="ac97f-335">一切取決於發生什麼情況與您的應用程式。）</span><span class="sxs-lookup"><span data-stu-id="ac97f-335">It all depends on what's going on with your app.)</span></span>

> [!NOTE]
> <span data-ttu-id="ac97f-336">您無法 「 記住 」 所使用的密碼 文字方塊中的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-336">You can't "remember" the value of a text box that's used for passwords.</span></span> <span data-ttu-id="ac97f-337">它會讓人可以使用程式碼填入在密碼欄位中的安全性漏洞。</span><span class="sxs-lookup"><span data-stu-id="ac97f-337">It would be a security hole to allow people to fill in a password field by using code.</span></span>


<span data-ttu-id="ac97f-338">再次執行頁面，輸入內容類型，然後按一下**搜尋內容類型**。</span><span class="sxs-lookup"><span data-stu-id="ac97f-338">Run the page again, enter a genre, and click **Search Genre**.</span></span> <span data-ttu-id="ac97f-339">這次不只您看的搜尋結果，但文字方塊會記住您上次輸入：</span><span class="sxs-lookup"><span data-stu-id="ac97f-339">This time not only do you see the results of the search, but the text box remembers what you entered last time:</span></span>

![顯示文字方塊具有 '記住' 先前的項目頁面](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a><span data-ttu-id="ac97f-341">搜尋標題中的任何文字</span><span class="sxs-lookup"><span data-stu-id="ac97f-341">Searching for Any Word in the Title</span></span>

<span data-ttu-id="ac97f-342">您現在可以搜尋任何內容類型，但您也可以在搜尋標題。</span><span class="sxs-lookup"><span data-stu-id="ac97f-342">You can now search for any genre, but you might also want to search for a title.</span></span> <span data-ttu-id="ac97f-343">很難取得標題沒錯，當您搜尋，因此您可搜尋的任何位置出現在標題文字。</span><span class="sxs-lookup"><span data-stu-id="ac97f-343">It's hard to get a title exactly right when you search, so instead you can search for a word that appears anywhere inside a title.</span></span> <span data-ttu-id="ac97f-344">若要在 SQL 中這麼做，您使用`LIKE`運算子和語法如下所示：</span><span class="sxs-lookup"><span data-stu-id="ac97f-344">To do that in SQL, you use the `LIKE` operator and syntax like the following:</span></span>

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

<span data-ttu-id="ac97f-345">此命令會取得標題中含有"adventure"的所有影片。</span><span class="sxs-lookup"><span data-stu-id="ac97f-345">This command gets all the movies whose titles contain "adventure".</span></span> <span data-ttu-id="ac97f-346">當您使用`LIKE`運算子，包括萬用字元`%`做為搜尋條件的一部分。</span><span class="sxs-lookup"><span data-stu-id="ac97f-346">When you use the `LIKE` operator, you include the wildcard character `%` as part of the search term.</span></span> <span data-ttu-id="ac97f-347">搜尋`LIKE 'adventure%'`表示 「 開頭為 'adventure' 」。</span><span class="sxs-lookup"><span data-stu-id="ac97f-347">The search `LIKE 'adventure%'` means "starting with 'adventure'".</span></span> <span data-ttu-id="ac97f-348">（技術上來說，這表示 「 字串 'adventure' 後面的任何項目。 」）同樣地，搜尋字詞`LIKE '%adventure'`表示 「 任何項目後面的字串 'adventure' 」，這是說: 「 'adventure' 做為結尾 」 的另一種方式。</span><span class="sxs-lookup"><span data-stu-id="ac97f-348">(Technically, it means "The string 'adventure' followed by anything.") Similarly, the search term `LIKE '%adventure'` means "anything followed by the string 'adventure'", which is another way to say "ending with 'adventure'".</span></span>

<span data-ttu-id="ac97f-349">搜尋字詞`LIKE '%adventure%'`因此表示 「 具有 'adventure' 標題中任何地方。 」</span><span class="sxs-lookup"><span data-stu-id="ac97f-349">The search term `LIKE '%adventure%'` therefore means "with 'adventure' anywhere in the title."</span></span> <span data-ttu-id="ac97f-350">（技術上來說，「 任何標題，後面接著 'adventure'，後面接著的任何項目中。"）</span><span class="sxs-lookup"><span data-stu-id="ac97f-350">(Technically, "anything in the title, followed by 'adventure', followed by anything.")</span></span>

<span data-ttu-id="ac97f-351">內`<form>`項目，加入下列標記正下方的右`</div>`內容類型搜尋的標記 (只在關閉前`</form>`項目):</span><span class="sxs-lookup"><span data-stu-id="ac97f-351">Inside the `<form>` element, add the following markup right under the closing `</div>` tag for the genre search (just before the closing `</form>` element):</span></span>

[!code-html[Main](form-basics/samples/sample10.html)]

<span data-ttu-id="ac97f-352">程式碼來處理此搜尋是內容類型搜尋的程式碼類似，不同之處在於您必須組合`LIKE`搜尋。</span><span class="sxs-lookup"><span data-stu-id="ac97f-352">The code to handle this search is similar to the code for the genre search, except that you have to assemble the `LIKE` search.</span></span> <span data-ttu-id="ac97f-353">程式碼區塊頂端的頁面中，新增這`if`正後方封鎖`if`內容類型搜尋的區塊：</span><span class="sxs-lookup"><span data-stu-id="ac97f-353">Inside the code block at the top of the page, add this `if` block just after the `if` block for the genre search:</span></span>

[!code-csharp[Main](form-basics/samples/sample11.cs)]

<span data-ttu-id="ac97f-354">此程式碼會使用您先前看到的相同邏輯，不同之處在於會使用`LIKE`運算子，而且程式碼將 「`%`"之前和之後的搜尋字詞。</span><span class="sxs-lookup"><span data-stu-id="ac97f-354">This code uses the same logic you saw earlier, except that the search uses a `LIKE` operator and the code puts "`%`" before and after the search term.</span></span>

<span data-ttu-id="ac97f-355">請注意如何很容易就能新增至頁面的另一項搜尋。</span><span class="sxs-lookup"><span data-stu-id="ac97f-355">Notice how it was easy to add another search to the page.</span></span> <span data-ttu-id="ac97f-356">那就行了：</span><span class="sxs-lookup"><span data-stu-id="ac97f-356">All you had to do was:</span></span>

- <span data-ttu-id="ac97f-357">建立`if`測試，以查看相關的搜尋方塊中是否有值的區塊。</span><span class="sxs-lookup"><span data-stu-id="ac97f-357">Create an `if` block that tested to see whether the relevant search box had a value.</span></span>
- <span data-ttu-id="ac97f-358">設定`selectCommand`變數設為新的 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="ac97f-358">Set the `selectCommand` variable to a new SQL statement.</span></span>
- <span data-ttu-id="ac97f-359">設定`searchTerm`變數的值，以傳遞給查詢。</span><span class="sxs-lookup"><span data-stu-id="ac97f-359">Set the `searchTerm` variable to the value to pass to the query.</span></span>

<span data-ttu-id="ac97f-360">以下是完整的程式碼區塊，其中包含項目的搜尋的新邏輯：</span><span class="sxs-lookup"><span data-stu-id="ac97f-360">Here's the complete code block, which contains the new logic for a title search:</span></span>

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

<span data-ttu-id="ac97f-361">此程式碼所執行的作業的摘要如下：</span><span class="sxs-lookup"><span data-stu-id="ac97f-361">Here's a summary of what this code does:</span></span>

- <span data-ttu-id="ac97f-362">變數`searchTerm`和`selectCommand`頂端會初始化。</span><span class="sxs-lookup"><span data-stu-id="ac97f-362">The variables `searchTerm` and `selectCommand` are initialized at the top.</span></span> <span data-ttu-id="ac97f-363">您要將這些變數設定為適當的搜尋字詞 （如果有的話），使用者會在頁面根據適當的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="ac97f-363">You're going to set these variables to the appropriate search term (if any) and appropriate SQL command based on what the user does in the page.</span></span> <span data-ttu-id="ac97f-364">預設搜尋是簡單的情況下，從資料庫取得所有影片。</span><span class="sxs-lookup"><span data-stu-id="ac97f-364">The default search is the simple case of getting all the movies from the database.</span></span>
- <span data-ttu-id="ac97f-365">中的測試`searchGenre`並`searchTitle`，程式碼集`searchTerm`您想要搜尋的值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-365">In the tests for `searchGenre` and `searchTitle`, the code sets `searchTerm` to the value you want to search for.</span></span> <span data-ttu-id="ac97f-366">這些程式碼區塊也設定`selectCommand`該搜尋適當的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="ac97f-366">Those code blocks also set `selectCommand` to an appropriate SQL command for that search.</span></span>
- <span data-ttu-id="ac97f-367">`db.Query`方法叫用一次，使用 SQL 命令`selectedCommand`中的任何值，而且`searchTerm`。</span><span class="sxs-lookup"><span data-stu-id="ac97f-367">The `db.Query` method is invoked only once, using whatever SQL command is in `selectedCommand` and whatever value is in `searchTerm`.</span></span> <span data-ttu-id="ac97f-368">如果沒有搜尋詞彙 （沒有內容類型和任何標題 word） 的值`searchTerm`為空字串。</span><span class="sxs-lookup"><span data-stu-id="ac97f-368">If there is no search term (no genre and no title word), the value of `searchTerm` is an empty string.</span></span> <span data-ttu-id="ac97f-369">不過，，並不重要，因為在此情況下查詢並不需要參數。</span><span class="sxs-lookup"><span data-stu-id="ac97f-369">However, that doesn't matter, because in that case the query doesn't require a parameter.</span></span>

## <a name="testing-the-title-search-feature"></a><span data-ttu-id="ac97f-370">測試的標題搜尋功能</span><span class="sxs-lookup"><span data-stu-id="ac97f-370">Testing the Title Search Feature</span></span>

<span data-ttu-id="ac97f-371">現在您可以測試您的已完成的搜尋頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-371">Now you can test your completed search page.</span></span> <span data-ttu-id="ac97f-372">執行*Movies.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="ac97f-372">Run *Movies.cshtml*.</span></span>

<span data-ttu-id="ac97f-373">輸入的內容類型，然後按一下**搜尋內容類型**。</span><span class="sxs-lookup"><span data-stu-id="ac97f-373">Enter a genre and click **Search Genre**.</span></span> <span data-ttu-id="ac97f-374">方格會顯示影片的該內容類型，例如之前。</span><span class="sxs-lookup"><span data-stu-id="ac97f-374">The grid displays movies of that genre, like before.</span></span>

<span data-ttu-id="ac97f-375">輸入 標題文字，然後按一下**搜尋標題**。</span><span class="sxs-lookup"><span data-stu-id="ac97f-375">Enter a title word and click **Search Title**.</span></span> <span data-ttu-id="ac97f-376">方格會顯示在標題中有該單字的電影。</span><span class="sxs-lookup"><span data-stu-id="ac97f-376">The grid displays movies that have that word in the title.</span></span>

!['' 中搜尋標題之後列出影片頁面](form-basics/_static/image6.png)

<span data-ttu-id="ac97f-378">將兩個文字方塊保留空白，然後按一下任一個按鈕。</span><span class="sxs-lookup"><span data-stu-id="ac97f-378">Leave both text boxes blank and click either button.</span></span> <span data-ttu-id="ac97f-379">方格會顯示所有影片。</span><span class="sxs-lookup"><span data-stu-id="ac97f-379">The grid displays all the movies.</span></span>

## <a name="combining-the-queries"></a><span data-ttu-id="ac97f-380">合併查詢</span><span class="sxs-lookup"><span data-stu-id="ac97f-380">Combining the Queries</span></span>

<span data-ttu-id="ac97f-381">您可能會注意到，您可以執行的搜尋會獨佔。</span><span class="sxs-lookup"><span data-stu-id="ac97f-381">You might notice that the searches you can perform are exclusive.</span></span> <span data-ttu-id="ac97f-382">您無法搜尋標題和內容類型在此同時，即使這兩個搜尋方塊中有值。</span><span class="sxs-lookup"><span data-stu-id="ac97f-382">You can't search the title and the genre at the same time, even if both search boxes have values in them.</span></span> <span data-ttu-id="ac97f-383">例如，您無法搜尋其標題包含"Adventure"的所有動作電影。</span><span class="sxs-lookup"><span data-stu-id="ac97f-383">For example, you can't search for all action movies whose title contains "Adventure".</span></span> <span data-ttu-id="ac97f-384">（頁面是程式碼現在，如果您輸入的內容類型和標題值項目的搜尋取得優先順序。）若要建立結合條件的搜尋，您必須建立 SQL 查詢，如下所示的語法：</span><span class="sxs-lookup"><span data-stu-id="ac97f-384">(As the page is coded now, if you enter values for both genre and title, the title search gets precedence.) To create a search that combines the conditions, you would have to create a SQL query that has syntax like the following:</span></span>

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

<span data-ttu-id="ac97f-385">而且您必須使用如下的陳述式來執行查詢 （粗略地說，）：</span><span class="sxs-lookup"><span data-stu-id="ac97f-385">And you'd have to run the query by using a statement like the following (roughly speaking):</span></span>

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

<span data-ttu-id="ac97f-386">建立邏輯，以允許的搜尋準則的許多排列方式可讓有點複雜，如您所見。</span><span class="sxs-lookup"><span data-stu-id="ac97f-386">Creating logic to allow many permutations of search criteria can get a bit involved, as you can see.</span></span> <span data-ttu-id="ac97f-387">因此，我們將在此停止。</span><span class="sxs-lookup"><span data-stu-id="ac97f-387">Therefore, we'll stop here.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="ac97f-388">接下來的下一步</span><span class="sxs-lookup"><span data-stu-id="ac97f-388">Coming Up Next</span></span>

<span data-ttu-id="ac97f-389">在下一個教學課程中，您將建立使用表單，讓使用者可將影片新增到資料庫的頁面。</span><span class="sxs-lookup"><span data-stu-id="ac97f-389">In the next tutorial, you'll create a page that uses a form to let users add movies to the database.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-search"></a><span data-ttu-id="ac97f-390">（使用搜尋服務更新） 的電影頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="ac97f-390">Complete Listing for Movie Page (Updated with Search)</span></span>

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="ac97f-391">其他資源</span><span class="sxs-lookup"><span data-stu-id="ac97f-391">Additional Resources</span></span>

- [<span data-ttu-id="ac97f-392">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="ac97f-392">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- <span data-ttu-id="ac97f-393">[SQL WHERE 子句](http://www.w3schools.com/sql/sql_where.asp)W3Schools 站台上</span><span class="sxs-lookup"><span data-stu-id="ac97f-393">[SQL WHERE Clause](http://www.w3schools.com/sql/sql_where.asp) on the W3Schools site</span></span>
- <span data-ttu-id="ac97f-394">[方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的文章</span><span class="sxs-lookup"><span data-stu-id="ac97f-394">[Method Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article on the W3C site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ac97f-395">[上一頁](displaying-data.md)
> [下一頁](entering-data.md)</span><span class="sxs-lookup"><span data-stu-id="ac97f-395">[Previous](displaying-data.md)
[Next](entering-data.md)</span></span>
