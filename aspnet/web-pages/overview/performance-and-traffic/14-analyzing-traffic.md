---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: 追蹤的 ASP.NET Web Pages (Razor) 網站訪客資訊 （分析） |Microsoft 文件
author: tfitzmac
description: 您您之後將您的網站之後，您可能想要分析網站流量。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528757"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>追蹤的 ASP.NET Web Pages (Razor) 網站訪客資訊 （分析）
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用協助程式，將網站分析加入 ASP.NET Web Pages (Razor) 網站中的網頁。
> 
> 您將學習：
> 
> - 如何將您的網站流量的相關資訊傳送至分析提供者。
> 
> 這些是 ASP.NET 程式設計文件中所引進的功能：
> 
> - `Analytics`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library （NuGet 套件）


分析是技術，測量您的網站上的流量，因此您可以了解使用者如何使用站台的一般詞彙。 許多分析可用的服務，包括 Google、 Yahoo、 StatCounter，和其他服務。

想要追蹤的運作方式是確認您登入帳戶分析提供者使用，讓您在其中註冊站台，您的方式分析。提供者會傳送包含識別碼或追蹤程式碼，為您的帳戶的 JavaScript 程式碼的程式碼片段。 您可以將 JavaScript 程式碼片段加入您想要追蹤的站台上的網頁。（您通常將分析的程式碼片段加入頁尾或版面配置頁或其他 HTML 標記出現在網站中，每個頁面上。）當使用者要求網頁包含其中一種這些 JavaScript 程式碼片段時，程式碼片段會傳送給的分析提供者，會記錄各種詳細資料頁面的目前頁面的相關資訊。

當您想要看看您的網站統計資料時，您登入分析提供者的網站。 然後，您可以檢視有關您站台，各種報表類似：

- 個別頁面的頁面檢視數目。 這會告訴您大致上有多人造訪網站，而且最受歡迎網站上的頁面。
- 多久人員花上的特定頁面。 這會告訴您等您的首頁是否保持人感興趣。
- 站台人們為何上之前造訪您的網站。 這可協助您了解您的流量是否來自連結，從搜尋，依此類推。
- 當使用者造訪您的網站，他們保持多久。
- 為您的訪客哪些國家/地區。
- 何種瀏覽器和作業系統會使用您的訪客。

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>使用 Helper 將分析加入頁面

ASP.NET Web Pages 包含數個分析 helper (`Analytics.GetGoogleHtml`， `Analytics.GetYahooHtml`，和`Analytics.GetStatCounterHtml`)，讓您輕鬆管理用於分析的 JavaScript 程式碼片段。 而方式和位置，找出不是將 JavaScript 程式碼中，您只需要為協助專家將加入頁面。 您需要提供的唯一資訊是您的帳戶名稱、 識別碼或追蹤程式碼。 （如 StatCounter，您也必須提供幾個額外的值。）

在此程序，您將建立使用的版面配置頁`GetGoogleHtml`協助程式。 如果您已經與其他廠商分析提供者的其中一個帳戶，您可以改為使用該帳戶，並視需要進行了些微調整。

> [!NOTE]
> 當您建立 analytics 帳戶時，您會註冊您想要追蹤的網站 URL。 如果您要測試的所有項目在本機電腦上，您將不會追蹤實際流量 （只流量您），因此您無法再記錄和檢視的網站統計資料。 但是，此程序示範如何將分析 helper 加入頁面。 當您發行您的網站時，即時網站會將您的分析提供者資訊。


1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您尚未新增它。
2. 建立 Google Analytics 帳戶，並記錄的帳戶名稱。
3. 建立名為的版面配置頁面*Analytics.cshtml*並加入下列標記：

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > 您必須將呼叫`Analytics`網頁主體中的協助程式 (之前`</body>`標記)。 否則，瀏覽器不會執行指令碼。

    如果您使用不同的分析提供者，請改用下列 helper 的其中一個：

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. 取代`myaccount`帳戶、 識別碼或您在步驟 1 中建立的追蹤程式碼的名稱。
5. 執行網頁瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)
6. 在瀏覽器中檢視網頁原始檔。 您可以查看轉譯的分析程式碼：

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. 登入 Google Analytics 網站，並檢查您的站台的統計資料。 如果您即時的站台上執行的頁面，您會看到記錄到您的網頁瀏覽的項目。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

- [Google Analytics 網站](https://www.google.com/analytics/)
- [Yahoo ！網站分析](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter 站台](http://statcounter.com/)
