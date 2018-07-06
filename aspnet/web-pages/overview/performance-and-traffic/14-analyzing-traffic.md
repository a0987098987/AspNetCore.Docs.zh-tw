---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: 追蹤 ASP.NET Web Pages (Razor) 網站訪客資訊 （分析） |Microsoft Docs
author: tfitzmac
description: 您覺得您要的網站之後，您可能想要分析網站流量。
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 4e065e5223d2f996779ab47de4823962a9aa852e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831092"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>追蹤 ASP.NET Web Pages (Razor) 網站的造訪者資訊 （分析）
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用協助程式，在 ASP.NET Web Pages (Razor) 網站中的頁面加入網站分析。
> 
> 您將學到什麼：
> 
> - 如何將您的網站流量的相關資訊傳送給分析提供者。
> 
> 這些是 ASP.NET 程式設計文章中所引進的功能：
> 
> - `Analytics`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library （NuGet 套件）


Analytics 是一個通稱測量您的網站上的流量，因此您可以了解使用者如何使用站台的技術。 許多分析服務已推出，包括 Google、 Yahoo、 StatCounter，和其他服務。

想要追蹤的運作方式是，您登入帳戶與分析提供者，讓您註冊站台時，您的方式分析。提供者會傳送包含 ID 或追蹤您的帳戶的程式碼的 JavaScript 程式碼片段。 您新增至您想要追蹤之站台上的網頁的 JavaScript 程式碼片段。（您通常新增分析程式碼片段的頁尾或版面配置頁面或其他網站中的每個頁面出現的 HTML 標記）。當使用者要求網頁包含其中一個這些 JavaScript 程式碼片段時，程式碼片段會將目前頁面的相關資訊傳送分析提供者，會記錄各種詳細資料頁面。

當您想要看看您的網站統計資料時，您會登入分析提供者的網站。 然後，您可以檢視有關您的網站，各種報表類似：

- 個別頁面的頁面檢視數目。 這會告訴您 （大約） 有多少人正在瀏覽網站，並在您的網站上的哪些分頁是最受歡迎。
- 多久人員花在特定的頁面上。 這會告訴您像是您的首頁上是否讓人的感興趣。
- 哪些站台有人上前客戶造訪您的網站。 這可協助您了解您的流量是否來自連結，從搜尋，依此類推。
- 當您的網站和其停留多久，請瀏覽的人員。
- 為您的訪客哪些國家/地區。
- 哪些瀏覽器與作業系統會使用您的訪客。

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>若要將分析加入頁面中使用協助程式

ASP.NET Web 網頁包含數個分析協助程式 (`Analytics.GetGoogleHtml`， `Analytics.GetYahooHtml`，和`Analytics.GetStatCounterHtml`)，讓您輕鬆管理用於分析的 JavaScript 程式碼片段。 而方式和位置，找出不是把 JavaScript 程式碼中，您只需要只將協助程式新增至頁面。 您必須提供的唯一資訊是您的帳戶名稱、 識別碼或追蹤程式碼。 （如 StatCounter，您也必須提供一些其他的值。）

在此程序中，您將建立會使用的版面配置頁`GetGoogleHtml`協助程式。 如果您已經有帳戶，與其中一個其他分析提供者，您可以改為使用該帳戶，並視需要進行些微調整。

> [!NOTE]
> 當您建立 analytics 帳戶時，您會註冊您想要追蹤之網站的 URL。 如果您要測試的所有項目在您的本機電腦上，您將不會追蹤實際的資料傳輸 （唯一的流量是您），因此您無法再記錄及檢視網站統計資料。 但此程序示範如何將分析協助程式新增至頁面。 當您發行您的網站時，即時網站會將資訊傳送給您的分析提供者。


1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有新增它。
2. 建立具有 Google Analytics 帳戶，並記下帳戶名稱。
3. 建立名為版面配置頁*Analytics.cshtml*並新增下列標記：

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > 您必須將放在呼叫`Analytics`在您的網頁主體中的協助程式 (之前`</body>`標記)。 否則，瀏覽器不會執行指令碼。

    如果您使用不同的分析提供者，請改用下列協助程式的其中一個：

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. 取代`myaccount`帳戶、 識別碼或您在步驟 1 中建立的追蹤程式碼的名稱。
5. 執行網頁瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)
6. 在瀏覽器中檢視網頁原始檔。 您將能夠看到轉譯的分析程式碼：

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. 登入 Google Analytics 網站，並檢查您的站台的統計資料。 如果您即時站台上執行的頁面，您會看到記錄到您的網頁瀏覽的項目。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

- [Google Analytics 網站](https://www.google.com/analytics/)
- [Yahoo ！Web Analytics 網站](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter 站台](http://statcounter.com/)
