---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: 顯示自訂錯誤網頁 (C#) |Microsoft 文件
author: rick-anderson
description: 沒有使用者看到的內容中的 ASP.NET web 應用程式發生執行階段錯誤時？ 答案需視如何網站&lt;customErrors&gt;組態...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: f01a0f3af3680d53639512d7a86ac1a8645d00e2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
ms.locfileid: "30889068"
---
<a name="displaying-a-custom-error-page-c"></a>顯示自訂錯誤網頁 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> 沒有使用者看到的內容中的 ASP.NET web 應用程式發生執行階段錯誤時？ 答案需視如何網站&lt;customErrors&gt;組態。 根據預設，會使用者顯示的機動性的黃色螢幕 proclaiming 發生執行階段錯誤。 本教學課程會示範如何自訂這些設定，以符合您的網站的外觀及操作-悅耳顯示自訂錯誤網頁。


## <a name="introduction"></a>簡介

在理想的世界裡中會有任何執行階段錯誤。 程式設計人員可以撰寫程式碼與 nary bug，健全的使用者輸入驗證，與外部資源，例如資料庫伺服器和電子郵件伺服器將會永遠不會進入離線。 當然，但實際上是無法避免錯誤。 .NET Framework 中的類別會通知發生錯誤，所擲回例外狀況。 物件的開啟方法呼叫 SqlConnection，例如建立資料庫連接字串所指定的連接。 不過，如果資料庫已關閉，或連接字串中的認證不正確然後 Open 方法擲回`SqlException`。 可以處理例外狀況，方法是使用`try/catch/finally`區塊。 如果內的程式碼`try`區塊擲回例外狀況，控制權會轉移到適當的 catch 區塊，開發人員可以嘗試從錯誤中復原。 如果沒有相符的 catch 區塊，或例外狀況擲回例外狀況的程式碼不是在 try 區塊中，如果 percolates search 的呼叫堆疊向上`try/catch/finally`區塊。

如果沒有要處理的例外狀況反昇一直 ASP.NET 執行階段[`HttpApplication`類別](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)的[`Error`事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)，就會引發和設定*錯誤頁面*隨即出現。 根據預設，ASP.NET 會顯示錯誤頁面，affectionately 稱為[死亡畫面黃色](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)(YSOD)。 有兩個版本的 YSOD： 其中一個會顯示例外狀況詳細資料、 堆疊追蹤，以及其他資訊給偵錯應用程式的開發人員很有幫助 (請參閱**圖 1**); 其他只要狀態發生執行階段錯誤 （請參閱**圖 2**)。

例外狀況詳細資料 YSOD 偵錯應用程式，開發人員很有用，但 YSOD 顯示給使用者，而且 tacky 不夠專業。 相反地，一般使用者，應該採取以維護站台的外觀與風格與更加易懂易記 prose 描述這種情況的錯誤頁面。 好消息是建立自訂錯誤頁面，是相當簡單。 本教學課程開頭 ASP 看。網路的不同的錯誤頁面。 然後，它會示範如何設定 web 應用程式，以顯示使用者自訂錯誤網頁遇到錯誤時。

### <a name="examining-the-three-types-of-error-pages"></a>檢查錯誤網頁的三種類型

當未處理的例外狀況，就會發生的錯誤網頁的三種類型之一的 ASP.NET 應用程式中會顯示：

- 例外狀況詳細資料黃色螢幕的致命錯誤頁面上，
- 執行階段錯誤黃色螢幕的致命錯誤頁面上，或
- 自訂錯誤網頁

錯誤網頁開發人員是最熟悉的是例外狀況詳細資料 YSOD。 根據預設，此頁面會顯示在本機瀏覽的使用者，並因此是在開發環境中測試網站時，就會發生錯誤時所看到的頁面。 正如其名，例外狀況詳細資料 YSOD 會提供有關例外狀況-詳細資料的類型、 訊息，以及堆疊追蹤。 而且，如果例外狀況由 ASP.NET 網頁的程式碼後置類別中的程式碼，而且應用程式設定進行偵錯然後例外狀況詳細資料 YSOD 也會顯示這一行程式碼 （與上述程式碼和其下的幾行）。

**圖 1**顯示例外狀況詳細資料 YSOD 頁面。 記下 URL 在瀏覽器的位址視窗： `http://localhost:62275/Genre.aspx?ID=foo`。 請記得，`Genre.aspx`頁面會列出活頁簿中的檢閱特定內容類型。 它要求`GenreId`值 ( `uniqueidentifier`) 傳遞查詢字串; 例如，若要檢視小說檢閱適當的 URL 是`Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`。 如果將非`uniqueidentifier`傳遞值中 （例如"foo") 的 querystring 透過擲回例外狀況。

> [!NOTE]
> 若要重現示範 web 應用程式中的這個錯誤可供下載您可以請造訪`Genre.aspx?ID=foo`直接按一下中的 [產生執行階段錯誤] 連結或`Default.aspx`。


請注意中呈現的例外狀況資訊**圖 1**。 例外狀況訊息，「 轉換失敗時從字元字串轉換到 uniqueidentifier 」 是出現在頁面頂端。 例外狀況類型、 `System.Data.SqlClient.SqlException`，列出，以及。 另外還有堆疊追蹤。

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**圖 1**： 例外狀況詳細資料 YSOD 包含例外狀況的相關資訊  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image3.png))

另一種 YSOD 是執行階段錯誤 YSOD 中，並如下所示**圖 2**。 執行階段錯誤 YSOD 通知執行階段錯誤的訪客，但它不包含任何擲回的例外狀況的相關資訊。 (它，不過，提供有關如何進行修改的可檢視錯誤詳細資料的指示`Web.config`檔案，即構成這類 YSOD 看起來不夠專業的一部分。)

根據預設，執行階段錯誤 YSOD 顯示給使用者的瀏覽遠端 (透過http://www.yoursite.com)，如在瀏覽器的網址列中的 url 辨識項**圖 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`。 兩個不同的 YSOD 畫面存在，因為開發人員有興趣了解錯誤詳細資料，但這類資訊不應該顯示即時的站台上為潛在的安全性漏洞或其他機密資訊的訪客的任何人，它可能會顯示您站台。

> [!NOTE]
> 如果步驟一路做，而且使用 DiscountASP.NET 做 web 主機，您可能會注意到，執行階段錯誤 YSOD 時不會顯示瀏覽即時網站。 這是因為 DiscountASP.NET 具有其預設設定來顯示例外狀況詳細資料 YSOD 的伺服器。 好消息是，您可以覆寫此預設行為加入`<customErrors>`區段您`Web.config`檔案。 「 設定的錯誤網頁顯示 」 一節會檢查`<customErrors>`> 一節中詳細資料。


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**圖 2**： 執行階段錯誤 YSOD 不包含任何錯誤的詳細資料  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image6.png))

錯誤頁面的第三個類型是自訂錯誤頁面上，是您所建立的網頁。 自訂錯誤網頁的好處是： 您可以在頁面的外觀及操作; 以及使用者顯示的資訊的完整控制自訂錯誤網頁可以做為您的其他頁面使用相同的主版頁面和樣式。 使用自訂錯誤頁面 」 一節逐步解說建立自訂錯誤網頁，並加以設定來顯示期間發生未處理的例外狀況。 **圖 3**提供觀看此自訂錯誤頁面。 如您所見，外觀與風格的錯誤頁面是死的更專業比其中一個黃色螢幕圖 1 和 2 所示。

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**圖 3**： 自訂錯誤頁面提供了更量身訂做的外觀與風格  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image9.png))

請花一點時間檢查瀏覽器的網址列中**圖 3**。 請注意，[網址] 列會顯示自訂錯誤網頁的 URL (`/ErrorPages/Oops.aspx`)。 在圖 1 和 2 黃色螢幕的死會顯示在相同的頁面，從產生的錯誤 (`Genre.aspx`)。 自訂錯誤網頁傳遞透過發生錯誤網頁的 URL `aspxerrorpath` querystring 參數。

## <a name="configuring-which-error-page-is-displayed"></a>設定的錯誤頁面會顯示

這三個可能的錯誤頁面會顯示根據兩個變數：

- 中的組態資訊`<customErrors>` 區段中，並
- 是否在本機或遠端使用者造訪網站。

[ `<customErrors>`區段](https://msdn.microsoft.com/library/h0hfz6fc.aspx)中`Web.config`具有兩個屬性會影響哪些錯誤頁面會顯示：`defaultRedirect`和`mode`。 `defaultRedirect` 屬性是選擇性的。 如果提供，它會指定自訂錯誤網頁的 URL，並指出自訂錯誤頁面應該會顯示而不是執行階段錯誤 YSOD。 `mode`必要屬性，並接受三個值之一： `On`， `Off`，或`RemoteOnly`。 這些值具有下列行為：

- `On` -表示自訂錯誤網頁或執行階段錯誤 YSOD，會顯示所有造訪者，不論是本機或遠端。
- `Off` -指定例外狀況詳細資料 YSOD 會顯示所有造訪者，不論是本機或遠端。
- `RemoteOnly` -表示自訂錯誤網頁或執行階段錯誤 YSOD 顯示為遠端的訪客，而例外狀況詳細資料 YSOD 則顯示為本機的訪客。

除非您另外指定，ASP.NET 會像是您已將 mode 屬性設定為`RemoteOnly`且具有未指定`defaultRedirect`值。 換句話說的預設行為是例外狀況詳細資料 YSOD 會顯示為本機的訪客，而執行階段錯誤 YSOD 就會顯示遠端訪客。 您可以藉由新增覆寫此預設行為`<customErrors>`區段，以您的 web 應用程式 `Web.config file.`

## <a name="using-a-custom-error-page"></a>使用自訂錯誤網頁

每個 web 應用程式應該有自訂錯誤頁面。 它可提供執行階段錯誤 YSOD 更專業的替代選項，很容易建立，並設定要使用自訂錯誤網頁的應用程式會採用只有幾分鐘的時間。 第一個步驟建立自訂錯誤網頁。 我已將新的資料夾加入至活頁簿檢閱應用程式名為`ErrorPages`並加入新的 ASP.NET 網頁名為`Oops.aspx`。 具有相同的主版頁面作為網站上頁面的其餘部分，使其自動繼承相同的外觀及操作的頁面。

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**圖 4**： 建立自訂錯誤頁面

接下來，花幾分鐘的時間，建立錯誤網頁的內容。 我建立了訊息，指出有未預期的錯誤，並連結回到網站的首頁而不是簡單的自訂錯誤網頁。

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**圖 5**： 設計您的自訂錯誤頁面  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image14.png))

已完成的錯誤網頁，web 應用程式設定為使用自訂錯誤頁面，就不需執行階段錯誤 YSOD。 這可以藉由指定之 URL 中的錯誤頁面`<customErrors>`區段的`defaultRedirect`屬性。 將下列標記加入至您的應用程式`Web.config`檔案：

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

上述的標記會設定要顯示給使用者瀏覽在本機從遠端瀏覽這些使用者使用自訂錯誤網頁 Oops.aspx 時的例外狀況詳細資料 YSOD 的應用程式。 若要查看此動作中，將您的網站部署到實際執行環境，然後瀏覽 Genre.aspx 頁面上即時網站具有無效的查詢字串值。 您應該會看到自訂錯誤網頁 (回頭參考**圖 3**)。

若要確認自訂錯誤頁面僅會顯示遠端使用者，請瀏覽`Genre.aspx`頁面具有無效的查詢字串，從開發環境。 您仍應該會看到例外狀況詳細資料 YSOD (回頭參考**圖 1**)。 `RemoteOnly`設定可確保在實際執行環境瀏覽網站的使用者請參閱開發人員在本機繼續以查看例外狀況的詳細資料時，將自訂錯誤網頁。

## <a name="notifying-developers-and-logging-error-details"></a>通知開發人員，並記錄錯誤的詳細資料

在開發環境中發生的錯誤所引起坐在電腦的開發人員。 她會顯示例外狀況的資訊中的例外狀況的詳細資料 YSOD，而且她知道錯誤發生時，她正在執行哪些步驟。 但在生產環境上發生錯誤時，開發人員不認得使用者瀏覽的網站會報告錯誤的時間，除非發生錯誤。 而且，即使使用者超出他警示發生錯誤，而不需要知道例外狀況類型、 訊息和堆疊追蹤它很難進行診斷錯誤的原因更別說修正此問題的開發團隊的方法。

這些理由，最重要的生產環境中的任何錯誤會記錄某些永續性存放區時 （例如資料庫），以及開發人員提供此錯誤會發出警示。 自訂錯誤網頁看起來好像沒有此記錄和通知的好地方。 不幸的是，自訂錯誤網頁並沒有錯誤詳細資料的存取權，因此無法用來記錄這項資訊。 好消息是有數種方式以攔截錯誤詳細資料和記錄，而接下來三個教學課程瀏覽此主題的更多詳細資料。

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>使用不同的 HTTP 錯誤狀態的不同的自訂錯誤網頁

當 ASP.NET 網頁所擲回例外狀況，而且不會處理時，例外狀況 percolates 至 ASP.NET 執行階段，會顯示設定的錯誤頁面。 如果進入 ASP.NET 引擎的要求，但基於某些原因無法處理-可能是要求的檔案是找不到或讀取檔案-已停用權限，則 ASP.NET 引擎會引發`HttpException`。 這個例外狀況，例如從 ASP.NET 頁面中，所引發的例外狀況反昇至執行階段，導致顯示適當的錯誤頁面。

這表示在生產環境中的 web 應用程式是，如果使用者要求找不到它們將會看到自訂錯誤頁面的頁面。 **圖 6**顯示這類範例。 因為要求不存在頁面 (`NoSuchPage.aspx`)、`HttpException`就會擲回，並顯示自訂錯誤頁面 (請注意的參考`NoSuchPage.aspx`中`aspxerrorpath`querystring 參數)。

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**圖 6**: ASP.NET 執行階段會顯示的設定錯誤頁面中回應不正確的要求 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image17.png))

根據預設，所有類型的錯誤會都導致相同的自訂錯誤頁面，才會顯示。 不過，您可以指定特定 HTTP 狀態碼，請使用不同的自訂錯誤頁面`<error>`內的子系項目的`<customErrors>`> 一節。 例如，若要有不同的錯誤網頁顯示發生錯誤，已經 404 HTTP 狀態碼，找不到頁面更新`<customErrors>`節，以加入下列標記：

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

完成這項變更的地方，每當使用者瀏覽遠端要求的 ASP.NET 資源不存在，則會重新導向至`404.aspx`自訂錯誤網頁，而不是`Oops.aspx`。 做為**圖 7**說明，`404.aspx`頁面可以包含一般的自訂錯誤頁面之外的更特定訊息。

> [!NOTE]
> 簽出[404 錯誤網頁，一個多階段](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)如需建立有效的 404 錯誤網頁的指引。


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**圖 7**： 自訂 404 錯誤頁面會顯示比更目標的訊息 `Oops.aspx`  
 ([按一下以檢視完整大小的影像](displaying-a-custom-error-page-cs/_static/image20.png)) 

因為您已了解`404.aspx`使用者找不到頁面的要求時，只有到達頁面中，您可以加強這個自訂錯誤網頁包含可協助使用者解決這種錯誤的特定類型的功能。 例如，您無法建置對應已知良好的 Url 不正確的 Url 資料庫資料表，而則有`404.aspx`執行針對查詢的資料表，並建議使用者可能會嘗試連線到的網頁的自訂錯誤網頁。

> [!NOTE]
> 由 ASP.NET 引擎所處理的資源來提出要求時，才會顯示自訂錯誤網頁。 如我們所述[核心差異之間 IIS 和 ASP.NET 程式開發伺服器](core-differences-between-iis-and-the-asp-net-development-server-cs.md)教學課程，網頁伺服器可能本身處理特定要求。 根據預設，IIS 會 web 映像與 HTML 檔案之類的靜態內容的伺服器處理序要求，而不叫用 ASP.NET 引擎。 因此，如果使用者要求的不存在的映像檔它們將會傳回 IIS 的預設 404 錯誤訊息，而不是 ASP。網路的設定錯誤網頁。


## <a name="summary"></a>總結

當 ASP.NET 應用程式中發生未處理的例外狀況時，使用者會顯示三個錯誤網頁的其中一個： 例外狀況詳細資料黃色螢幕的死;執行階段錯誤黃色死亡; 畫面或自訂錯誤網頁。 顯示的錯誤頁面取決於應用程式的`<customErrors>`組態和使用者是否為瀏覽本機或遠端。 預設行為是顯示為本機的訪客例外狀況詳細資料 YSOD 和執行階段錯誤 YSOD 遠端訪客。

雖然執行階段錯誤 YSOD 隱藏使用者瀏覽的網站可能含有機密的錯誤資訊，它會中斷從您的網站的外觀及操作，並可讓您尋找錯誤的應用程式。 更好的方法是使用自訂錯誤網頁，需要建立及設計自訂錯誤網頁，並指定其 URL 中的`<customErrors>`區段的`defaultRedirect`屬性。 您甚至可以擁有多個不同的 HTTP 錯誤狀態的自訂錯誤頁面。

自訂錯誤頁面是完整的錯誤處理策略，在生產環境中網站的第一個步驟。 警示開發人員，並記錄其詳細資料也是錯誤的重要的步驟。 接下來三個教學課程中，瀏覽錯誤通知和記錄的技術。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [錯誤頁面，一次](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [例外狀況的設計方針](https://msdn.microsoft.com/library/ms229014.aspx)
- [使用者易記的錯誤網頁](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [處理和擲回例外狀況](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [正確地在 ASP.NET 中使用自訂錯誤網頁](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [上一頁](strategies-for-database-development-and-deployment-cs.md)
> [下一頁](processing-unhandled-exceptions-cs.md)
