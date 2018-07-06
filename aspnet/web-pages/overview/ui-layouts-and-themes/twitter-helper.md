---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter 協助程式與 ASP.NET Web Pages |Microsoft Docs
author: tfitzmac
description: 此主題和應用程式會示範如何將 Twitter 的協助程式新增至您的 WebMatrix 3 專案。 它包含的 Twitter Helper 程式碼，並示範如何呼叫協助程式...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 1824b677a7ba96ea6fc5119610725a30d472764e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817651"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Twitter 協助程式與 ASP.NET Web Pages
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 此主題和應用程式會示範如何將 Twitter 的協助程式新增至您的 WebMatrix 3 專案。 它包含的 Twitter Helper 程式碼，並示範如何呼叫 helper 方法。
> 
> 此程式碼 Twitter.cshtml 檔案由開發**Tian 取景位置調整**的 Microsoft。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


## <a name="introduction"></a>簡介

本主題示範如何將 Twitter 的協助程式新增至您的應用程式，並使用 Razor 語法來呼叫 helper 方法。 Twitter 協助程式可讓您輕鬆地將 Twitter 按鈕和應用程式中的 widget。 若要使用 Twitter widget，例如使用者的動態時報或搜尋結果的雜湊標記，您必須先建立[在 Twitter 上的小工具](https://twitter.com/settings/widgets)。 建立小工具之後, 您會收到小工具識別碼。呼叫顯示小工具的協助程式方法時，您可以傳遞做為參數的這個小工具識別碼。

本主題所撰寫的 Twitter API 的 1.1 版。 藉由直接新增至您的專案的 Twitter Helper 程式碼，您可以更新協助程式程式碼，如果 Twitter API 變更。

如需安裝 WebMatrix 的資訊，請參閱[Introducing ASP.NET Web Pages 2-開始使用](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)。

## <a name="add-twitter-helper-to-your-project"></a>將 Twitter 協助程式新增至您的專案

若要新增 Twitter 協助程式，首先，新增名為的資料夾**應用程式\_程式碼**至您的專案。 然後，建立名為**Twitter.cshtml**。

![App_Code 資料夾](twitter-helper/_static/image1.png)

Twitter.cshtml 的預設程式碼取代為下列程式碼。

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>從網頁呼叫 Twitter 方法

下列範例示範如何在專案中使用 Twitter Helper 方法，從頁面。 在專案中，您會想要的參數值取代為您的需求與相關的值。 您可以使用提供的小工具識別碼來探索方法的運作方式，但您會想要產生您自己的 widget，為您的專案。

不是所有的參數，如下所示的需要。 選擇性參數用來自訂按鈕或小工具顯示的方式。 比方說，遵循 按鈕只需要使用者名稱，才能執行，但此範例示範如何顯示數字的粉絲，和指定的按鈕和語言大小的方式。

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>查看結果

上述程式碼會產生下列的按鈕和小工具。 這些按鈕和 widget 都能完整運作不螢幕擷取畫面。 遵循] 按鈕會顯示在 [西班牙文，因為語言參數已設為**es**。

### <a name="follow-button"></a>請依照下列按鈕

[請依照下列@aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>推文的按鈕

[推文](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>使用者時間軸 （設定檔）

[推文者 @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>我的最愛

[最愛的推文者 @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>清單

[從推文@Microsoft/MS\_消費者\_群組列](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>搜尋

[相關推文&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
