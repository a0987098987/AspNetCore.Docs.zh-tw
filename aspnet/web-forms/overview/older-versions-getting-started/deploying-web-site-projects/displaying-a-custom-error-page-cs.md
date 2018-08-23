---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: 顯示自訂錯誤頁面 (C#) |Microsoft Docs
author: rick-anderson
description: 沒有使用者看到的內容中的 ASP.NET web 應用程式的執行階段錯誤發生時？ 答案取決於如何在網站&lt;customErrors&gt;組態...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 449b8eb26f3f6018fdd6c6dcc1de1c8d58214ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833854"
---
<a name="displaying-a-custom-error-page-c"></a>顯示自訂錯誤頁面 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> 沒有使用者看到的內容中的 ASP.NET web 應用程式的執行階段錯誤發生時？ 答案取決於如何在網站&lt;customErrors&gt;組態。 根據預設，使用者會顯示難懂的黃色螢幕讚揚發生執行階段錯誤。 本教學課程會示範如何自訂這些設定，以符合您網站的外觀與風格的悅耳且具專業水準舒適的顯示自訂錯誤頁面。


## <a name="introduction"></a>簡介

在理想的世界會有任何執行階段錯誤。 程式設計人員撰寫程式碼與 nary bug 和健全的使用者輸入驗證，與外部資源，例如資料庫伺服器和電子郵件伺服器永遠都不會離線。 當然，事實上錯誤是無法避免的。 .NET Framework 中的類別會擲回例外狀況通知發生錯誤。 比方說，呼叫 SqlConnection 物件的開啟方法會建立連接的連接字串所指定的資料庫。 不過，如果資料庫已關閉，或連接字串中的認證不正確則 Open 方法會擲回`SqlException`。 例外狀況可由使用`try/catch/finally`區塊。 如果程式碼內`try`區塊擲回例外狀況，控制權會轉移到適當的 catch 區塊的開發人員可以嘗試從錯誤復原。 如果沒有相符的 catch 區塊，或如果擲回例外狀況的程式碼不是在 try 區塊中，例外狀況 percolates search 的呼叫堆疊`try/catch/finally`區塊。

如果沒有要處理的例外狀況反昇一直到 ASP.NET 執行階段[`HttpApplication`類別](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)的[`Error`事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)引發且設定*錯誤頁面*隨即出現。 根據預設，ASP.NET 會顯示錯誤頁面，露面指[黃色死亡畫面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)(YSOD)。 有兩個版本的 YSOD： 一個會顯示例外狀況詳細資料、 堆疊追蹤，以及其他資訊很有幫助開發人員偵錯應用程式 (請參閱**圖 1**); 另只會指出發生執行階段錯誤 （請參閱**圖 2**)。

例外狀況詳細資料 YSOD 適用於偵錯應用程式，開發人員很有用，但對使用者顯示 YSOD 且認為不夠專業。 相反地，使用者應該前往更方便使用的文字描述的情況下與維護網站的外觀與風格的錯誤頁面。 好消息是，建立這類的自訂錯誤頁面是相當簡單。 本教學課程開始了解 ASP。NET 的不同的錯誤頁面。 然後，它會示範如何設定 web 應用程式，以顯示使用者遇到錯誤時的自訂錯誤頁面。

### <a name="examining-the-three-types-of-error-pages"></a>檢查錯誤網頁的三種類型

當未處理的例外狀況發生於 ASP.NET 應用程式有三種類型的錯誤頁面中會顯示：

- 例外狀況詳細資料黃色死亡畫面錯誤頁面上，
- 執行階段錯誤黃色死亡畫面錯誤頁面上，或
- 自訂錯誤頁面

錯誤網頁開發人員最熟悉使用的是例外狀況詳細資料 YSOD。 根據預設，此頁面會顯示在本機瀏覽的使用者，因此在開發環境中測試網站時，發生錯誤時，您會看到的頁面。 正如其名，例外狀況詳細資料 YSOD 會提供有關例外狀況-詳細資料的類型、 訊息，以及堆疊追蹤。 不僅如此，如果您的 ASP.NET 網頁程式碼後置類別中的程式碼引發例外狀況，而且應用程式已針對偵錯然後例外狀況詳細資料 YSOD 也會顯示這行程式碼 （和上述程式碼和其下的幾行）。

**圖 1**顯示例外狀況詳細資料 YSOD 頁面。 請注意在瀏覽器的位址視窗中的 URL: `http://localhost:62275/Genre.aspx?ID=foo`。 請記得，`Genre.aspx`頁面會列出在特定內容類型的書籍評論。 它要求`GenreId`值 ( `uniqueidentifier`) 傳遞查詢字串; 例如，適當的 URL，以檢視小說評論是`Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`。 如果非`uniqueidentifier`值會傳遞透過查詢字串 （例如"foo") 會擲回例外狀況。

> [!NOTE]
> 若要重現此錯誤在示範 web 應用程式可供下載您可以造訪`Genre.aspx?ID=foo`直接按一下中的 「 產生執行階段錯誤 」 連結或`Default.aspx`。


請注意例外狀況資訊**圖 1**。 例外狀況訊息，「 轉換失敗時從字元字串轉換到 uniqueidentifier 」 會呈現在頁面頂端。 例外狀況類型、 `System.Data.SqlClient.SqlException`，時，也列出。 另外還有堆疊追蹤。

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**圖 1**： 例外狀況詳細資料 YSOD 包含例外狀況的相關資訊  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image3.png))

另一種 YSOD 是執行階段錯誤 YSOD 中，與所示**圖 2**。 執行階段錯誤 YSOD 通知發生執行階段錯誤，訪客，但是它不包含任何擲回的例外狀況的相關資訊。 (它，不過，提供有關如何進行修改的可檢視錯誤詳細資料的指示`Web.config`檔案，這是屬於讓這類 YSOD 看起來不夠專業。)

根據預設，執行階段錯誤 YSOD 顯示給使用者瀏覽遠端 (透過 http://www.yoursite.com) ，在瀏覽器的網址列中的 URL，證明**圖 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`。 在兩個不同 YSOD 畫面存在，因為開發人員有興趣了解錯誤詳細資料，但是因為它可能會揭露潛在的安全性弱點或其他機密資訊来造訪的任何人，這類資訊應該顯示即時站台的程式站台。

> [!NOTE]
> 如果您有並獲得使用與您的 web 主機時，您可能會發現瀏覽即時的網站時，不會不會顯示執行階段錯誤 YSOD。 這是因為獲得的預設設定來顯示例外狀況詳細資料 YSOD 其伺服器。 好消息是，您可以覆寫此預設行為加入`<customErrors>`陳述式，以您`Web.config`檔案。 「 設定的錯誤頁面顯示 」 一節將探討`<customErrors>`詳細資料中的一節。


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**圖 2**： 執行階段錯誤 YSOD 不包含任何錯誤的詳細資料  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image6.png))

錯誤頁面的第三個類型為自訂錯誤頁面上，也就是您所建立的網頁。 自訂錯誤網頁的優點是您可以在頁面的外觀及操作，以及使用者顯示的資訊的完整控制自訂錯誤頁面可以使用相同的主版頁面和樣式，為您其他的頁面。 「 使用自訂錯誤頁面 」 一節逐步解說建立自訂的錯誤頁面，並設定它以發生未處理的例外狀況時顯示。 **圖 3**提供此自訂錯誤頁面，一窺。 如您所見，錯誤頁面的外觀會是更專業的比其中一個黃色畫面死亡數字 1 和 2 中所示。

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**圖 3**： 自訂錯誤頁面提供更客製化的外觀與風格  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image9.png))

花點時間檢查瀏覽器的網址列中**圖 3**。 請注意，[網址] 列會顯示自訂錯誤網頁的 URL (`/ErrorPages/Oops.aspx`)。 數字 1 和 2 中，黃色的人員死亡畫面所示的同一頁所產生的錯誤 (`Genre.aspx`)。 自訂錯誤頁面傳遞透過發生錯誤的所在頁面的 URL `aspxerrorpath` querystring 參數。

## <a name="configuring-which-error-page-is-displayed"></a>設定其 [錯誤] 頁面會顯示

這三個可能的錯誤頁面會顯示，根據兩個變數：

- 中的組態資訊`<customErrors>` 區段中，並
- 是否在本機或遠端使用者會造訪該網站。

[ `<customErrors>`  區段](https://msdn.microsoft.com/library/h0hfz6fc.aspx)中`Web.config`具有會影響顯示錯誤頁面的兩個屬性：`defaultRedirect`和`mode`。 `defaultRedirect` 屬性是選擇性的。 如果提供，它會指定自訂錯誤網頁的 URL，並指出而不是執行階段錯誤 YSOD，應該會顯示自訂錯誤頁面。 `mode`是必要屬性，並接受三個值之一： `On`， `Off`，或`RemoteOnly`。 這些值會有下列行為：

- `On` -表示自訂錯誤頁面或執行階段錯誤 YSOD 顯示所有的訪客，不論它們是本機或遠端。
- `Off` -指定例外狀況詳細資料 YSOD 顯示所有的訪客，不論它們是本機或遠端。
- `RemoteOnly` -表示自訂錯誤頁面或執行階段錯誤 YSOD 顯示為遠端的訪客，雖然例外狀況詳細資料 YSOD 會顯示本機訪客。

除非您另外指定，ASP.NET 行為會如同您已將 mode 屬性設定為`RemoteOnly`且具有未指定`defaultRedirect`值。 換句話說，預設行為是例外狀況詳細資料 YSOD 會顯示本機訪客，雖然執行階段錯誤 YSOD 會顯示為遠端的訪客。 您可以藉由新增覆寫此預設行為`<customErrors>`一節，以您的 web 應用程式 `Web.config file.`

## <a name="using-a-custom-error-page"></a>使用自訂錯誤頁面

每個 web 應用程式應該有一個自訂錯誤頁面。 它提供執行階段錯誤 YSOD 更專業的替代方案，很容易建立，並設定應用程式使用自訂錯誤頁面需要只有幾分鐘的時間。 第一個步驟建立自訂錯誤頁面。 我已加入名為的書籍評論應用程式中的新資料夾`ErrorPages`，並加入新的 ASP.NET 頁面上名為`Oops.aspx`。 具有相同的主版頁面作為網站上頁面的其餘部分，使其自動繼承相同的外觀與風格的頁面。

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**圖 4**： 建立自訂的錯誤頁面

接下來，花幾分鐘的時間，建立 [錯誤] 頁面的內容。 我建立了訊息，指出未預期的錯誤，並連結回到網站的首頁而不是簡單的自訂錯誤頁面。

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**圖 5**： 設計您的自訂錯誤頁面  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image14.png))

使用 [錯誤] 頁面完成的情況下，設定 web 應用程式使用自訂錯誤頁面，執行階段錯誤 YSOD 代替。 這可以藉由指定中的 [錯誤] 頁面的 URL`<customErrors>`一節的`defaultRedirect`屬性。 將下列標記新增至您的應用程式`Web.config`檔案：

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

上述標記會設定要顯示給使用者瀏覽在本機上，從遠端瀏覽這些使用者使用自訂錯誤頁面 Oops.aspx 時的例外狀況詳細資料 YSOD 的應用程式。 若要查看此動作，將您的網站部署至生產環境，然後瀏覽 Genre.aspx 頁面在即時網站具有無效的查詢字串值。 您應該會看到自訂錯誤頁面 (回頭**圖 3**)。

若要確認自訂錯誤頁面只會顯示給遠端使用者，請瀏覽`Genre.aspx`頁面具有無效的查詢字串，從開發環境。 您應該仍會看到例外狀況詳細資料 YSOD (回頭**圖 1**)。 `RemoteOnly`設定可確保在實際執行環境中造訪該網站的使用者在本機上的開發人員繼續以查看例外狀況的詳細資料時，會看到自訂錯誤頁面。

## <a name="notifying-developers-and-logging-error-details"></a>通知開發人員，並記錄錯誤的詳細資料

在開發環境中發生的錯誤所造成開發人員坐在她的電腦。 她顯示例外狀況的詳細資料 YSOD，例外狀況的資訊，且她知道她在發生錯誤時所執行的步驟。 但在實際執行上發生錯誤時，開發人員對此一無所知，除非使用者造訪該網站會以報告錯誤的時間時發生錯誤。 而且，即使使用者超出他開發團隊，發生錯誤，而不需要知道例外狀況類型、 訊息和堆疊追蹤，它可能很難診斷錯誤的原因，讓我們單獨進行修正此問題的方式。

基於這些理由，它是最重要的生產環境中的任何錯誤會記錄至一些 （例如資料庫） 的持續性存放區，並開發人員會收到此錯誤的警示。 自訂錯誤頁面看起來好像不錯的起點，此記錄和通知。 不幸的是，自訂錯誤頁面並沒有錯誤詳細資料的存取權，因此無法用來記錄這項資訊。 好消息是，有數種方式來攔截錯誤詳細資料和記錄，接下來三個教學課程會探索更多詳細資料中的這個主題。

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>不同的 HTTP 錯誤狀態使用不同的自訂錯誤網頁

當 ASP.NET 網頁所擲回例外狀況，而且不會處理時，例外狀況 percolates 至 ASP.NET 執行階段，會顯示設定的錯誤頁面。 如果要求進入 ASP.NET 引擎，但基於某些原因無法處理-可能是要求的檔案是找不到或讀取權限已停用檔案-然後 ASP.NET 引擎會引發`HttpException`。 這個例外狀況，例如從 ASP.NET 頁面中，所引發的例外狀況反昇至執行階段，讓適當的錯誤頁面會顯示。

這表示在生產環境中的 web 應用程式是，如果使用者要求的頁面，找不到，則他們會看見自訂錯誤頁面。 **圖 6**顯示這類範例。 要求不存在的頁面，因此 (`NoSuchPage.aspx`)、`HttpException`就會擲回，並顯示自訂錯誤頁面 (請注意參考`NoSuchPage.aspx`中`aspxerrorpath`querystring 參數)。

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**圖 6**: ASP.NET 執行階段顯示設定錯誤網頁的回應無效的要求 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image17.png))

根據預設，所有類型的錯誤會都導致相同的自訂錯誤頁面，來顯示。 不過，您可以指定特定 HTTP 狀態碼，請使用不同的自訂錯誤頁面`<error>`內的子系項目的`<customErrors>`一節。 例如，若要有不同的錯誤頁面，顯示發生錯誤，其具有 HTTP 狀態碼 404，找不到頁面更新`<customErrors>`節，以加入下列標記：

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

與此變更之後，每當使用者從遠端瀏覽要求 ASP.NET 資源不存在，則會重新導向至`404.aspx`自訂錯誤頁面，而不是`Oops.aspx`。 作為**圖 7**所示，`404.aspx`頁面可以包含比一般的自訂錯誤頁面的更特定的訊息。

> [!NOTE]
> 請參閱[404 錯誤網頁，一個更多時間](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)如需建立有效的 404 錯誤頁面的指引。


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**圖 7**： 自訂 404 錯誤頁面會顯示更具針對性的訊息比 `Oops.aspx`  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image20.png)) 

因為您知道`404.aspx`時使用者提出要求的頁面，找不到，就只能到達頁面，您可以增強這個自訂錯誤頁面，以包含可協助使用者解決這種錯誤的特定類型的功能。 比方說，您可以在其中建置資料庫對應的資料表已知良好的 Url 不正確的 Url 並再有`404.aspx`執行針對查詢的資料表，並建議使用者可能會嘗試連線到的頁面自訂錯誤頁面。

> [!NOTE]
> 由 ASP.NET 引擎處理資源提出要求時，才會顯示自訂錯誤頁面。 如我們所述[差異的核心之間 IIS 和 ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md)教學課程中，web 伺服器可能會自行處理特定要求。 根據預設，IIS 會 web 伺服器處理序要求針對靜態內容，例如映像和 HTML 檔案，而不叫用 ASP.NET 引擎。 因此，如果使用者要求的不存在的映像檔會將得到 IIS 的預設 404 錯誤訊息，而不是 ASP。NET 的設定錯誤頁面。


## <a name="summary"></a>總結

在 ASP.NET 應用程式中處理的例外狀況時，使用者會顯示三個錯誤網頁的其中一個： 例外狀況詳細資料黃色死亡畫面;執行階段錯誤黃色死亡; 的畫面或自訂的錯誤頁面。 會顯示錯誤頁面，取決於應用程式的`<customErrors>`組態和使用者是否已瀏覽本機或遠端。 預設行為是以顯示例外狀況詳細資料 YSOD，本機的訪客和執行階段錯誤 YSOD 遠端的訪客。

雖然執行階段錯誤 YSOD 隱藏可能具機密性的錯誤資訊，從使用者瀏覽的網站，它會從您的網站的外觀及操作中斷的連線，並可讓您尋找錯誤的應用程式。 更好的方法是使用自訂錯誤頁面上，需要建立與設計自訂錯誤頁面，並指定在其 URL`<customErrors>`一節的`defaultRedirect`屬性。 您甚至可以讓多個不同的 HTTP 錯誤狀態的自訂錯誤頁面。

自訂錯誤頁面是完整的錯誤處理策略，在生產環境中網站的第一個步驟。 警示開發人員和記錄其詳細資料也是錯誤的很重要的步驟。 接下來三個教學課程會探索錯誤通知和記錄的技術。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [錯誤頁面、 一次](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [例外狀況的設計方針](https://msdn.microsoft.com/library/ms229014.aspx)
- [使用者易記的錯誤網頁](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [處理和擲回例外狀況](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [適當地使用 ASP.NET 中的 自訂錯誤網頁](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [上一頁](strategies-for-database-development-and-deployment-cs.md)
> [下一頁](processing-unhandled-exceptions-cs.md)
