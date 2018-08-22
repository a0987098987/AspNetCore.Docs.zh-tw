---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter 協助程式與 ASP.NET Web Pages |Microsoft Docs
author: tfitzmac
description: 此主題和應用程式會示範如何將 Twitter 的協助程式新增至您的 WebMatrix 3 專案。 它包含的 Twitter Helper 程式碼，並示範如何呼叫協助程式...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 06223826c2682ffd62d5a1717f34242f39be5eda
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830557"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="5a144-104">Twitter 協助程式與 ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="5a144-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="5a144-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5a144-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5a144-106">此主題和應用程式會示範如何將 Twitter 的協助程式新增至您的 WebMatrix 3 專案。</span><span class="sxs-lookup"><span data-stu-id="5a144-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="5a144-107">它包含的 Twitter Helper 程式碼，並示範如何呼叫 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="5a144-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="5a144-108">此程式碼 Twitter.cshtml 檔案由開發**Tian 取景位置調整**的 Microsoft。</span><span class="sxs-lookup"><span data-stu-id="5a144-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5a144-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="5a144-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5a144-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5a144-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5a144-111">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="5a144-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="5a144-112">簡介</span><span class="sxs-lookup"><span data-stu-id="5a144-112">Introduction</span></span>

<span data-ttu-id="5a144-113">本主題示範如何將 Twitter 的協助程式新增至您的應用程式，並使用 Razor 語法來呼叫 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="5a144-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="5a144-114">Twitter 協助程式可讓您輕鬆地將 Twitter 按鈕和應用程式中的 widget。</span><span class="sxs-lookup"><span data-stu-id="5a144-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="5a144-115">若要使用 Twitter widget，例如使用者的動態時報或搜尋結果的雜湊標記，您必須先建立[在 Twitter 上的小工具](https://twitter.com/settings/widgets)。</span><span class="sxs-lookup"><span data-stu-id="5a144-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="5a144-116">建立小工具之後, 您會收到小工具識別碼。呼叫顯示小工具的協助程式方法時，您可以傳遞做為參數的這個小工具識別碼。</span><span class="sxs-lookup"><span data-stu-id="5a144-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="5a144-117">本主題所撰寫的 Twitter API 的 1.1 版。</span><span class="sxs-lookup"><span data-stu-id="5a144-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="5a144-118">藉由直接新增至您的專案的 Twitter Helper 程式碼，您可以更新協助程式程式碼，如果 Twitter API 變更。</span><span class="sxs-lookup"><span data-stu-id="5a144-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="5a144-119">如需安裝 WebMatrix 的資訊，請參閱[Introducing ASP.NET Web Pages 2-開始使用](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="5a144-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="5a144-120">將 Twitter 協助程式新增至您的專案</span><span class="sxs-lookup"><span data-stu-id="5a144-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="5a144-121">若要新增 Twitter 協助程式，首先，新增名為的資料夾**應用程式\_程式碼**至您的專案。</span><span class="sxs-lookup"><span data-stu-id="5a144-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="5a144-122">然後，建立名為**Twitter.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="5a144-122">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code 資料夾](twitter-helper/_static/image1.png)

<span data-ttu-id="5a144-124">Twitter.cshtml 的預設程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="5a144-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="5a144-125">從網頁呼叫 Twitter 方法</span><span class="sxs-lookup"><span data-stu-id="5a144-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="5a144-126">下列範例示範如何在專案中使用 Twitter Helper 方法，從頁面。</span><span class="sxs-lookup"><span data-stu-id="5a144-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="5a144-127">在專案中，您會想要的參數值取代為您的需求與相關的值。</span><span class="sxs-lookup"><span data-stu-id="5a144-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="5a144-128">您可以使用提供的小工具識別碼來探索方法的運作方式，但您會想要產生您自己的 widget，為您的專案。</span><span class="sxs-lookup"><span data-stu-id="5a144-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="5a144-129">不是所有的參數，如下所示的需要。</span><span class="sxs-lookup"><span data-stu-id="5a144-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="5a144-130">選擇性參數用來自訂按鈕或小工具顯示的方式。</span><span class="sxs-lookup"><span data-stu-id="5a144-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="5a144-131">比方說，遵循 按鈕只需要使用者名稱，才能執行，但此範例示範如何顯示數字的粉絲，和指定的按鈕和語言大小的方式。</span><span class="sxs-lookup"><span data-stu-id="5a144-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="5a144-132">查看結果</span><span class="sxs-lookup"><span data-stu-id="5a144-132">See the results</span></span>

<span data-ttu-id="5a144-133">上述程式碼會產生下列的按鈕和小工具。</span><span class="sxs-lookup"><span data-stu-id="5a144-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="5a144-134">這些按鈕和 widget 都能完整運作不螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="5a144-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="5a144-135">遵循] 按鈕會顯示在 [西班牙文，因為語言參數已設為**es**。</span><span class="sxs-lookup"><span data-stu-id="5a144-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="5a144-136">請依照下列按鈕</span><span class="sxs-lookup"><span data-stu-id="5a144-136">Follow Button</span></span>

<span data-ttu-id="5a144-137">[請依照下列@aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="5a144-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="5a144-138">推文的按鈕</span><span class="sxs-lookup"><span data-stu-id="5a144-138">Tweet Button</span></span>

<span data-ttu-id="5a144-139">[推文](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="5a144-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="5a144-140">使用者時間軸 （設定檔）</span><span class="sxs-lookup"><span data-stu-id="5a144-140">User Timeline (Profile)</span></span>

<span data-ttu-id="5a144-141">[推文者 @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="5a144-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="5a144-142">我的最愛</span><span class="sxs-lookup"><span data-stu-id="5a144-142">Favorites</span></span>

<span data-ttu-id="5a144-143">[最愛的推文者 @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="5a144-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="5a144-144">清單</span><span class="sxs-lookup"><span data-stu-id="5a144-144">List</span></span>

<span data-ttu-id="5a144-145">[從推文@Microsoft/MS\_消費者\_群組列](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="5a144-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="5a144-146">搜尋</span><span class="sxs-lookup"><span data-stu-id="5a144-146">Search</span></span>

<span data-ttu-id="5a144-147">[相關推文&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="5a144-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
