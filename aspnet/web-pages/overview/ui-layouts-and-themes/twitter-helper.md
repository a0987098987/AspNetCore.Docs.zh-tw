---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter 協助程式與 ASP.NET Web Pages |Microsoft Docs
author: Rick-Anderson
description: 此主題和應用程式會示範如何將 Twitter 的協助程式新增至您的 WebMatrix 3 專案。 它包含的 Twitter Helper 程式碼，並示範如何呼叫協助程式...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299426"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="b357f-104">Twitter 協助程式與 ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="b357f-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="b357f-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b357f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b357f-106">Twitter 協助程式已過時。</span><span class="sxs-lookup"><span data-stu-id="b357f-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="b357f-107">Twitter 的最新 engagement 工具網站，請參閱 < [Twitter 網站概觀](https://developer.twitter.com/en/docs/twitter-for-websites/overview)。</span><span class="sxs-lookup"><span data-stu-id="b357f-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="b357f-108">此主題和應用程式會示範如何將 Twitter 的協助程式新增至您的 WebMatrix 3 專案。</span><span class="sxs-lookup"><span data-stu-id="b357f-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="b357f-109">它包含的 Twitter Helper 程式碼，並示範如何呼叫 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="b357f-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="b357f-110">此程式碼 Twitter.cshtml 檔案由開發**Tian 取景位置調整**的 Microsoft。</span><span class="sxs-lookup"><span data-stu-id="b357f-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b357f-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="b357f-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b357f-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="b357f-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="b357f-113">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="b357f-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="b357f-114">簡介</span><span class="sxs-lookup"><span data-stu-id="b357f-114">Introduction</span></span>

<span data-ttu-id="b357f-115">本主題示範如何將 Twitter 的協助程式新增至您的應用程式，並使用 Razor 語法來呼叫 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="b357f-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="b357f-116">Twitter 協助程式可讓您輕鬆地將 Twitter 按鈕和應用程式中的 widget。</span><span class="sxs-lookup"><span data-stu-id="b357f-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="b357f-117">若要使用 Twitter widget，例如使用者的動態時報或搜尋結果的雜湊標記，您必須先建立[在 Twitter 上的小工具](https://twitter.com/settings/widgets)。</span><span class="sxs-lookup"><span data-stu-id="b357f-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="b357f-118">建立小工具之後, 您會收到小工具識別碼。呼叫顯示小工具的協助程式方法時，您可以傳遞做為參數的這個小工具識別碼。</span><span class="sxs-lookup"><span data-stu-id="b357f-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="b357f-119">本主題所撰寫的 Twitter API 的 1.1 版。</span><span class="sxs-lookup"><span data-stu-id="b357f-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="b357f-120">藉由直接新增至您的專案的 Twitter Helper 程式碼，您可以更新協助程式程式碼，如果 Twitter API 變更。</span><span class="sxs-lookup"><span data-stu-id="b357f-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="b357f-121">如需安裝 WebMatrix 的資訊，請參閱[Introducing ASP.NET Web Pages 2-開始使用](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="b357f-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="b357f-122">將 Twitter 協助程式新增至您的專案</span><span class="sxs-lookup"><span data-stu-id="b357f-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="b357f-123">若要新增 Twitter 協助程式，首先，新增名為的資料夾**應用程式\_程式碼**至您的專案。</span><span class="sxs-lookup"><span data-stu-id="b357f-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="b357f-124">然後，建立名為**Twitter.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="b357f-124">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code 資料夾](twitter-helper/_static/image1.png)

<span data-ttu-id="b357f-126">Twitter.cshtml 的預設程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="b357f-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="b357f-127">從網頁呼叫 Twitter 方法</span><span class="sxs-lookup"><span data-stu-id="b357f-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="b357f-128">下列範例示範如何在專案中使用 Twitter Helper 方法，從頁面。</span><span class="sxs-lookup"><span data-stu-id="b357f-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="b357f-129">在專案中，您會想要的參數值取代為您的需求與相關的值。</span><span class="sxs-lookup"><span data-stu-id="b357f-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="b357f-130">您可以使用提供的小工具識別碼來探索方法的運作方式，但您會想要產生您自己的 widget，為您的專案。</span><span class="sxs-lookup"><span data-stu-id="b357f-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="b357f-131">不是所有的參數，如下所示的需要。</span><span class="sxs-lookup"><span data-stu-id="b357f-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="b357f-132">選擇性參數用來自訂按鈕或小工具顯示的方式。</span><span class="sxs-lookup"><span data-stu-id="b357f-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="b357f-133">比方說，遵循 按鈕只需要使用者名稱，才能執行，但此範例示範如何顯示數字的粉絲，和指定的按鈕和語言大小的方式。</span><span class="sxs-lookup"><span data-stu-id="b357f-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="b357f-134">查看結果</span><span class="sxs-lookup"><span data-stu-id="b357f-134">See the results</span></span>

<span data-ttu-id="b357f-135">上述程式碼會產生下列的按鈕和小工具。</span><span class="sxs-lookup"><span data-stu-id="b357f-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="b357f-136">這些按鈕和 widget 都能完整運作不螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="b357f-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="b357f-137">遵循] 按鈕會顯示在 [西班牙文，因為語言參數已設為**es**。</span><span class="sxs-lookup"><span data-stu-id="b357f-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="b357f-138">請依照下列按鈕</span><span class="sxs-lookup"><span data-stu-id="b357f-138">Follow Button</span></span>

<span data-ttu-id="b357f-139">[請依照下列@aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="b357f-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="b357f-140">推文的按鈕</span><span class="sxs-lookup"><span data-stu-id="b357f-140">Tweet Button</span></span>

<span data-ttu-id="b357f-141">[推文](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="b357f-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="b357f-142">使用者時間軸 （設定檔）</span><span class="sxs-lookup"><span data-stu-id="b357f-142">User Timeline (Profile)</span></span>

<span data-ttu-id="b357f-143">[推文者 @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="b357f-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="b357f-144">我的最愛</span><span class="sxs-lookup"><span data-stu-id="b357f-144">Favorites</span></span>

<span data-ttu-id="b357f-145">[最愛的推文者 @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="b357f-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="b357f-146">清單</span><span class="sxs-lookup"><span data-stu-id="b357f-146">List</span></span>

<span data-ttu-id="b357f-147">[從推文@Microsoft/MS\_消費者\_群組列](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="b357f-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="b357f-148">搜尋</span><span class="sxs-lookup"><span data-stu-id="b357f-148">Search</span></span>

<span data-ttu-id="b357f-149">[相關推文&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="b357f-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
