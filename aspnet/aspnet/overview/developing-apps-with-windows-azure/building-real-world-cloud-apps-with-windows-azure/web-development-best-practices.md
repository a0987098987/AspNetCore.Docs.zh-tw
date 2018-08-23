---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web 開發最佳作法 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: be014eabcc7f88f229722b2d565262c9025c5ad3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833193"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web 開發最佳作法 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


前三個模式所需設定敏捷式開發程序;其餘部分是關於架構和程式碼。 這是 web 開發最佳作法的集合：

- [無狀態的 web 伺服器](#stateless)智慧負載平衡器後方。
- [避免工作階段狀態](#sessionstate)（或如果您無法避開它，請使用分散式快取，而不是資料庫）。
- [使用 CDN](#cdn)邊緣快取靜態檔案資產 （映像、 指令碼）。
- [使用.NET 4.5 的非同步支援](#async)以避免封鎖呼叫。

這些作法適用於所有的 web 程式開發，不只是針對雲端應用程式，但它們的雲端應用程式特別重要。 它們共同運作以協助您充分運用雲端環境所提供的彈性調整。 如果您不遵循這些作法，您會遇到限制當您嘗試調整您的應用程式。

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>智慧負載平衡器後方的無狀態 web 層

*無狀態 web 層*表示您不將任何應用程式資料儲存在 web 伺服器記憶體或檔案系統。 讓您的 web 層保持無狀態，可讓您同時提供更佳的客戶經驗並節省成本：

- 如果 web 層為無狀態，而且它位於負載平衡器後方，您可以快速地回應中的應用程式流量的變更藉由以動態方式新增或移除伺服器。 在雲端環境中，您只需支付的資源伺服器，只要您實際使用，這項功能來回應需求的變更可能會轉變成省下更不在話下。
- 無狀態 web 層是在架構上更容易相應放大應用程式。 也可讓您回應速度更快，調整需求並花費更少金錢上的開發和測試程序中。
- 雲端伺服器，如同在內部部署伺服器，需要修補並重新啟動偶爾;而且，如果無狀態 web 層，當伺服器暫時關閉 read-only routing 流量不會造成錯誤或非預期的行為。

大部分的真實世界應用程式需要儲存狀態的 web 工作階段;這裡的重點不是將它儲存在 web 伺服器上。 您可以在 cookie 或關閉處理序伺服器端 ASP.NET 工作階段狀態使用的快取提供者中用戶端上，例如儲存狀態，以其他方式。 您可以在檔案儲存[Windows Azure Blob 儲存體](unstructured-blob-storage.md)而不是本機檔案系統。

調整應用程式在 Windows Azure 網站中，如果您的 web 層為無狀態是多麼的範例，請參閱**擴展** 索引標籤上的 Windows Azure 網站在管理入口網站：

![調整 索引標籤](web-development-best-practices/_static/image1.png)

如果您想要新增 web 伺服器，您可以只將執行個體計數滑桿拖曳至右邊。 將它設定為 5，然後按一下 **儲存**，而且您可以在數秒內有 5 部 web 伺服器來處理您的網站流量的 Windows Azure 中。

![五個執行個體](web-development-best-practices/_static/image2.png)

您可以輕鬆地設定到 3 或縮減成 1 的執行個體計數。 當您調整後時，您就會開始省錢立即在 Windows Azure 不會依時數費用的分鐘數，因為。

您也可以告訴 Windows Azure，以自動增加或減少 CPU 使用量為基礎的 web 伺服器數目。 在下列範例中，當 CPU 使用率低於 60%時，web 伺服器的數目會減少為最小值為 2，，和 web 伺服器的數目上限為 4，如果 CPU 使用率高於 80%時，就會增加。

![依 CPU 使用量調整規模](web-development-best-practices/_static/image3.png)

或者，如果您知道您的網站將只會忙碌在上班時間？ 您可以告訴白天執行多部伺服器，並減少 evenings、 加班，單一伺服器，而不包含週末的 Windows Azure。 下面一系列螢幕擷取畫面示範如何將網站設定為執行期間從上午 8 點到下午 5 點的工作時數的下班時間在一部伺服器和 4 部伺服器。

![依排程調整規模](web-development-best-practices/_static/image4.png)

![設定排程時間](web-development-best-practices/_static/image5.png)

![白天的排程](web-development-best-practices/_static/image6.png)

![Weeknight 排程](web-development-best-practices/_static/image7.png)

![週末排程](web-development-best-practices/_static/image8.png)

當然這一切都可以完成以及指令碼與入口網站中。

只要避免阻礙以動態方式加入或移除伺服器 Vm，藉由讓 web 層保持無狀態，您的應用程式向外延展的能力是幾乎無限制的 Windows Azure 中。

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>避免工作階段狀態

它通常不適合在真實世界的雲端應用程式中避免使用者工作階段中，儲存某種形式的狀態，但某些方法影響相較於其他效能和延展性。 如果您有儲存狀態，最好的解決方案是狀態的保留量小，並將其儲存在 cookie 中。 如果此方法不可行，次佳的方法是使用的提供者的 ASP.NET 工作階段狀態[分散式、 記憶體中快取](distributed-caching.md#sessionstate)。 從效能和延展性的觀點來看最差的解決方法是使用資料庫備份的工作階段狀態提供者。

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>使用 CDN 來快取靜態檔案資產

CDN 是內容傳遞網路的首字母縮寫。 您提供靜態檔案資產，例如影像和指令碼檔案至 CDN 提供者和提供者快取這些檔案在世界各地的資料中心，讓人存取您的應用程式，只要它們取得相對快速的回應和低延遲的快取資產。 此站台整體的載入時間並加速降低您的 web 伺服器上的負載。 Cdn 是特別重要，如果您已達到廣泛地理位置分散的對象。

Windows Azure CDN，而且您可以在 Windows Azure 或任何 web 裝載環境中執行的應用程式中使用其他的 Cdn。

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>若要避免封鎖呼叫中使用.NET 4.5 的非同步支援

.NET 4.5 增強的 C# 和 VB 的程式設計語言，以使其更容易以非同步方式處理工作。 非同步程式設計的優點不只是平行處理的情況下，例如當您想要同時啟動多個 web 服務呼叫。 它也可讓您的 web 伺服器，更有效率地執行高負載情況下可靠。 Web 伺服器中只有少數的執行緒可用，而且在高負載狀況下的所有執行緒使用時，連入要求都必須等候，直到執行緒空出來。 如果您的應用程式程式碼不會處理工作，例如資料庫查詢和 web 服務呼叫以非同步方式，許多執行緒是不必要地繫結起來，當伺服器在等候 I/O 回應。 這會限制伺服器可處理高負載情況下的流量。 使用非同步程式設計中，web 服務或傳回資料的資料庫正在等候的執行緒之前，釋放的最新要求提供服務資料接收。 在忙碌的 web 伺服器中，數百或數千個要求可以接著處理立即否則會等候執行緒釋出它。

如您稍早所見，很容易減少處理您的網站，因為它是以提高它們的 web 伺服器的數目。 因此如果伺服器可以達到更高的輸送量，您不需要為許多，而且可以減少您的成本，因為您需要較少的伺服器指定的流量磁碟區比您否則。

支援.NET 4.5 的非同步程式設計模型納入 ASP.NET 4.5 Web Form、 MVC、 和 Web API，在 Entity Framework 6，並在[Windows Azure 儲存體 API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx)。

### <a name="async-support-in-aspnet-45"></a>ASP.NET 4.5 中的非同步支援

在 ASP.NET 4.5 中，支援不只是為語言也是 MVC、 Web Form 和 Web API 架構已新增非同步程式設計。 例如，ASP.NET MVC 控制器動作方法接收來自 web 要求的資料，並將資料傳遞至檢視，然後建立要傳送至瀏覽器的 HTML。 經常在動作方法需要從資料庫或 web 服務取得資料，才能顯示在網頁上，或將儲存在網頁中所輸入的資料。 在這些情況下就可以輕鬆地進行非同步動作方法： 而不是傳回*ActionResult*物件，您會傳回*工作&lt;ActionResult&gt;* 和標記方法具有*非同步*關鍵字。 在方法中，一行程式碼開始的作業牽涉到的等候時間，當您標示為使用 await 關鍵字。

以下是簡單的動作方法的儲存機制方法呼叫的資料庫查詢：

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

而以下是以非同步方式處理資料庫呼叫的相同方法：

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

實際上，編譯器會產生適當的非同步程式碼。 當應用程式所進行的呼叫`FindTaskByIdAsync`，ASP.NET 則`FindTask`要求然後回溯背景工作執行緒，並使其可處理其他要求。 當`FindTask`動作的要求，以繼續處理的程式碼，該呼叫之後重新啟動執行緒。 之間過渡期間`FindTask`起始要求，並傳回資料時，可提供要有用的工作會等待回應就會繫結的執行緒。

非同步程式碼中，一些額外負荷，但在低負載情況下額外的負荷非常微小; 同時在您能夠處理要求，否則會等待可用的執行緒持有的高負載狀況下。

已可進行非同步程式設計，因為 ASP.NET 1.1 中，這種，但很難撰寫、 容易發生錯誤，且很難偵錯。 既然我們已簡化撰寫程式碼，ASP.NET 4.5 中，沒有理由再就不要執行。

### <a name="async-support-in-entity-framework-6"></a>在 Entity Framework 6 中的非同步支援

4.5 中的非同步支援的一部分提供非同步支援 web 服務呼叫、 通訊端，以及檔案系統 I/O，web 應用程式的最常見模式是叫用的資料庫，但我們資料的程式庫不支援非同步處理。 現在 Entity Framework 6 加入資料庫存取權的非同步支援。

在 Entity Framework 6 中會造成查詢或命令，以傳送至資料庫的所有方法都有非同步版本。 此範例中示範的非同步版本*尋找*方法。

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

與這個非同步支援的運作方式不只是針對插入、 刪除、 更新和簡單的尋找，它也可以使用 LINQ 查詢：

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

沒有`Async`新版`ToList`方法因為在此程式碼中，這是會使查詢傳送至資料庫的方法。 `Where`並`OrderByDescending`方法只會設定查詢，而`ToListAsync`方法執行查詢，並將回應的`result`變數。

## <a name="summary"></a>總結

您可以實作此處所述任何 web 程式設計架構和任何雲端環境中，web 的開發最佳作法，但我們有在 ASP.NET 和 Windows Azure 可讓您輕鬆的工具。 如果您遵循這些模式，您可以輕鬆地相應放大您的 web 層，因為每一部伺服器可以處理更多流量，您將會盡可能減少支出。

[下一步 一章](single-sign-on.md)會查看雲端如何讓單一登入案例。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源。

無狀態的 web 伺服器：

- [Microsoft Patterns and Practices-自動調整指引](https://msdn.microsoft.com/library/dn589774.aspx)。
- [停用 ARR 執行個體在 Windows Azure 網站中的同質](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)。 Erez benari 部落格文章說明工作階段親和性在 Windows Azure 網站。

CDN:

- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部部分影片系列。 請參閱第 3 集啟動 1:34:00 CDN 討論。
- [Microsoft 模式和作法靜態內容裝載模式](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN 檢閱](http://www.cdnreviews.com/)。 許多 Cdn 的概觀。

非同步程式設計：

- [ASP.NET MVC 4 中使用非同步方法](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。 由 Rick anderson 發表的教學課程。
- [非同步程式設計使用 Async 和 Await （C# 和 Visual Basic）](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)。 說明進行非同步程式設計的基本原理，ASP.NET 4.5 中的運作方式，以及如何撰寫程式碼來實作它的 MSDN 技術白皮書。
- [Entity Framework 的非同步查詢和儲存](https://msdn.microsoft.com/data/jj819165)
- [如何建置 ASP.NET Web 應用程式使用 Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7)。 Rowan Miller 所的視訊簡報。 包含圖形的示範如何非同步程式設計可以協助在高負載狀況下的 web 伺服器輸送量大幅提升。
- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部部分影片系列。 相關影響延展性的非同步程式設計的討論，請參閱影片 4 和 8 影片。
- [使用 ASP.NET 4.5，再加上重要的第個中的非同步方法的 Magic](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx)。 Scott Hanselman 的部落格文章主要是有關在 ASP.NET Web Forms 應用程式中使用非同步。

如需其他 web 開發最佳作法，請參閱下列資源：

- [修正範例應用程式-最佳作法](the-fix-it-sample-application.md#bestpractices)。 本電子書的 < 附錄列出一些修正其應用程式中實作的最佳作法。
- [Web 開發人員檢查清單](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [上一頁](continuous-integration-and-continuous-delivery.md)
> [下一頁](single-sign-on.md)
