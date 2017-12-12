---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: "Twitter Helper 與 ASP.NET Web Pages |Microsoft 文件"
author: tfitzmac
description: "本主題和應用程式顯示如何新增至 WebMatrix 3 專案的 Twitter Helper。 它包含的 Twitter Helper 程式碼，並示範如何呼叫的協助程式..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="2627e-104">ASP.NET Web 網頁的 twitter Helper</span><span class="sxs-lookup"><span data-stu-id="2627e-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="2627e-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2627e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2627e-106">本主題和應用程式顯示如何新增至 WebMatrix 3 專案的 Twitter Helper。</span><span class="sxs-lookup"><span data-stu-id="2627e-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="2627e-107">它包含的 Twitter Helper 程式碼，並示範如何呼叫 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="2627e-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="2627e-108">此程式碼 Twitter.cshtml 檔案由開發**Tian 取景位置調整**的 Microsoft。</span><span class="sxs-lookup"><span data-stu-id="2627e-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2627e-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="2627e-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2627e-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2627e-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2627e-111">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="2627e-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="2627e-112">簡介</span><span class="sxs-lookup"><span data-stu-id="2627e-112">Introduction</span></span>

<span data-ttu-id="2627e-113">本主題示範如何加入您的應用程式的 Twitter Helper，然後使用 Razor 語法呼叫 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="2627e-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="2627e-114">Twitter Helper 輕鬆地將 Twitter 按鈕與您的應用程式中的 widget。</span><span class="sxs-lookup"><span data-stu-id="2627e-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="2627e-115">若要使用 Twitter widget，例如使用者的時間軸或搜尋結果的說明，您必須先建立[Twitter 上的小工具](https://twitter.com/settings/widgets)。</span><span class="sxs-lookup"><span data-stu-id="2627e-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="2627e-116">建立您的 widget 之後, 您會收到小工具 id。顯示 widget 的 helper 方法的呼叫時，您可以傳遞做為參數的這個小工具 id。</span><span class="sxs-lookup"><span data-stu-id="2627e-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="2627e-117">本主題是針對 1.1 版的 Twitter API 所撰寫。</span><span class="sxs-lookup"><span data-stu-id="2627e-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="2627e-118">將 Twitter Helper 程式碼直接新增至您的專案中，您可以更新協助程式程式碼，如果 Twitter API 變更。</span><span class="sxs-lookup"><span data-stu-id="2627e-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="2627e-119">如需安裝 WebMatrix，請參閱[簡介 ASP.NET Web Pages 2-快速入門](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="2627e-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="2627e-120">將加入至專案的 Twitter Helper</span><span class="sxs-lookup"><span data-stu-id="2627e-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="2627e-121">若要加入的 Twitter Helper，首先，加入名為的資料夾**應用程式\_程式碼**至您的專案。</span><span class="sxs-lookup"><span data-stu-id="2627e-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="2627e-122">然後，建立名為**Twitter.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="2627e-122">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code 資料夾](twitter-helper/_static/image1.png)

<span data-ttu-id="2627e-124">Twitter.cshtml 中的預設程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="2627e-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="2627e-125">從您網頁呼叫 Twitter 方法</span><span class="sxs-lookup"><span data-stu-id="2627e-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="2627e-126">下列範例會示範如何使用專案中的頁面從 Twitter Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="2627e-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="2627e-127">在專案中，您要取代的參數值與您的需求與相關的值。</span><span class="sxs-lookup"><span data-stu-id="2627e-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="2627e-128">您可以使用提供的小工具 id 瀏覽這些方法的工作，但您會想要產生您自己的 widget，為您的專案。</span><span class="sxs-lookup"><span data-stu-id="2627e-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="2627e-129">不是所有的參數如下所示的需要。</span><span class="sxs-lookup"><span data-stu-id="2627e-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="2627e-130">選擇性參數用於自訂按鈕或 widget 的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="2627e-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="2627e-131">比方說，遵循 按鈕只會要求使用者名稱，若要依照，但此範例示範如何包含數目實行項目，並指定的按鈕和語言大小的方式。</span><span class="sxs-lookup"><span data-stu-id="2627e-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="2627e-132">查看結果</span><span class="sxs-lookup"><span data-stu-id="2627e-132">See the results</span></span>

<span data-ttu-id="2627e-133">上述程式碼會產生下列按鈕和 widget。</span><span class="sxs-lookup"><span data-stu-id="2627e-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="2627e-134">這些按鈕和 widget 都能完整運作不螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="2627e-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="2627e-135">請遵循 按鈕顯示在西班牙文，因為語言參數已設為**es**。</span><span class="sxs-lookup"><span data-stu-id="2627e-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="2627e-136">請依照下列按鈕</span><span class="sxs-lookup"><span data-stu-id="2627e-136">Follow Button</span></span>

<span data-ttu-id="2627e-137">[請遵循@aspnet)](https://twitter.com/aspnet)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore js (fjs）;}}（文件、 'script'、 ' twitter wjs'）;</script></span><span class="sxs-lookup"><span data-stu-id="2627e-137">[Follow @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="tweet-button"></a><span data-ttu-id="2627e-138">推文 」 按鈕</span><span class="sxs-lookup"><span data-stu-id="2627e-138">Tweet Button</span></span>

<span data-ttu-id="2627e-139">[推文](https://twitter.com/share)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore js (fjs）;}}（文件、 'script'、 ' twitter wjs'）;</script></span><span class="sxs-lookup"><span data-stu-id="2627e-139">[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="2627e-140">使用者時間軸 （設定檔）</span><span class="sxs-lookup"><span data-stu-id="2627e-140">User Timeline (Profile)</span></span>

<span data-ttu-id="2627e-141">[由推文@aspnet ](https://twitter.com/aspnet) <script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script></span><span class="sxs-lookup"><span data-stu-id="2627e-141">[Tweets by @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="favorites"></a><span data-ttu-id="2627e-142">我的最愛</span><span class="sxs-lookup"><span data-stu-id="2627e-142">Favorites</span></span>

<span data-ttu-id="2627e-143">[由我的最愛推文@Microsoft ](https://twitter.com/Microsoft/favorites) <script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script></span><span class="sxs-lookup"><span data-stu-id="2627e-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="list"></a><span data-ttu-id="2627e-144">清單</span><span class="sxs-lookup"><span data-stu-id="2627e-144">List</span></span>

<span data-ttu-id="2627e-145">[從推文@Microsoft/MS\_消費者\_帶](https://twitter.com/microsoft/ms-consumer-brands/)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script></span><span class="sxs-lookup"><span data-stu-id="2627e-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="search"></a><span data-ttu-id="2627e-146">搜尋</span><span class="sxs-lookup"><span data-stu-id="2627e-146">Search</span></span>

<span data-ttu-id="2627e-147">[相關推文&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script></span><span class="sxs-lookup"><span data-stu-id="2627e-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>
