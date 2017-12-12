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
<a name="twitter-helper-with-aspnet-web-pages"></a>ASP.NET Web 網頁的 twitter Helper
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本主題和應用程式顯示如何新增至 WebMatrix 3 專案的 Twitter Helper。 它包含的 Twitter Helper 程式碼，並示範如何呼叫 helper 方法。
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

本主題示範如何加入您的應用程式的 Twitter Helper，然後使用 Razor 語法呼叫 helper 方法。 Twitter Helper 輕鬆地將 Twitter 按鈕與您的應用程式中的 widget。 若要使用 Twitter widget，例如使用者的時間軸或搜尋結果的說明，您必須先建立[Twitter 上的小工具](https://twitter.com/settings/widgets)。 建立您的 widget 之後, 您會收到小工具 id。顯示 widget 的 helper 方法的呼叫時，您可以傳遞做為參數的這個小工具 id。

本主題是針對 1.1 版的 Twitter API 所撰寫。 將 Twitter Helper 程式碼直接新增至您的專案中，您可以更新協助程式程式碼，如果 Twitter API 變更。

如需安裝 WebMatrix，請參閱[簡介 ASP.NET Web Pages 2-快速入門](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)。

## <a name="add-twitter-helper-to-your-project"></a>將加入至專案的 Twitter Helper

若要加入的 Twitter Helper，首先，加入名為的資料夾**應用程式\_程式碼**至您的專案。 然後，建立名為**Twitter.cshtml**。

![App_Code 資料夾](twitter-helper/_static/image1.png)

Twitter.cshtml 中的預設程式碼取代為下列程式碼。

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>從您網頁呼叫 Twitter 方法

下列範例會示範如何使用專案中的頁面從 Twitter Helper 方法。 在專案中，您要取代的參數值與您的需求與相關的值。 您可以使用提供的小工具 id 瀏覽這些方法的工作，但您會想要產生您自己的 widget，為您的專案。

不是所有的參數如下所示的需要。 選擇性參數用於自訂按鈕或 widget 的顯示方式。 比方說，遵循 按鈕只會要求使用者名稱，若要依照，但此範例示範如何包含數目實行項目，並指定的按鈕和語言大小的方式。

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>查看結果

上述程式碼會產生下列按鈕和 widget。 這些按鈕和 widget 都能完整運作不螢幕擷取畫面。 請遵循 按鈕顯示在西班牙文，因為語言參數已設為**es**。

### <a name="follow-button"></a>請依照下列按鈕

[請遵循@aspnet)](https://twitter.com/aspnet)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore js (fjs）;}}（文件、 'script'、 ' twitter wjs'）;</script>

### <a name="tweet-button"></a>推文 」 按鈕

[推文](https://twitter.com/share)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore js (fjs）;}}（文件、 'script'、 ' twitter wjs'）;</script>

### <a name="user-timeline-profile"></a>使用者時間軸 （設定檔）

[由推文@aspnet ](https://twitter.com/aspnet) <script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script>

### <a name="favorites"></a>我的最愛

[由我的最愛推文@Microsoft ](https://twitter.com/Microsoft/favorites) <script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script>

### <a name="list"></a>清單

[從推文@Microsoft/MS\_消費者\_帶](https://twitter.com/microsoft/ms-consumer-brands/)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script>

### <a name="search"></a>搜尋

[相關推文&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>！ （d、 s、 識別碼） 的函式 {var js fjs = d.getElementsByTagName(s) [0]、 p = /^http:/.test(d.location) 嗎？'http': 'https';如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = 識別碼; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore js (fjs）;}}（文件、"script"，"twitter wjs"）;</script>
