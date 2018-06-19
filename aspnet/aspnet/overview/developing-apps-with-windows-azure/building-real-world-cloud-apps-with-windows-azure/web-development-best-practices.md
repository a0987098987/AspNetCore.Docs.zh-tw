---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web 開發最佳作法 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件
author: MikeWasson
description: Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 4c43b256018d91e89b3427f90fc5c6cd018641f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873517"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web 開發最佳做法 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


前三個模式是有關設定敏捷式開發流程。其餘的架構，以及程式碼相關。 這是 web 開發最佳作法的集合：

- [無狀態的 web 伺服器](#stateless)智慧負載平衡器後方。
- [避免工作階段狀態](#sessionstate)（或如果您無法避開它，請使用分散式快取，而非資料庫）。
- [使用 CDN](#cdn)邊緣快取靜態檔案資產 （映像、 指令碼）。
- [使用.NET 4.5 的非同步支援](#async)以避免封鎖呼叫。

這些做法適用於所有的 web 程式開發，不只是針對雲端應用程式，但它們對於雲端應用程式特別重要。 它們共同運作以協助您充分運用雲端環境所提供的彈性調整。 如果您不要遵循這些作法，您將執行的限制到當您嘗試調整應用程式。

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>智慧負載平衡器後方的無狀態 web 層

*無狀態 web 層*表示您不要在 web 伺服器記憶體或檔案系統中儲存任何應用程式資料。 讓您的 web 層保持無狀態可讓您同時提供較佳的客戶經驗並節省成本：

- 如果 web 層無狀態，而且它位於負載平衡器後方，您可以快速回應應用程式資料流中的變更以動態方式加入或移除伺服器。 在雲端環境中，您只需支付的資源伺服器，只要您實際使用，這項功能來回應需求變更轉譯為可節省大量的。
- 無狀態 web 層是向外擴充應用程式在架構上更簡單。 此外，太可讓您回應速度更快，調整需求，並花少錢開發和測試程序中。
- 雲端伺服器，如同在內部部署伺服器，需要修補和偶爾; 重新開機而且，如果無狀態 web 層，當伺服器暫時關閉經常流量不會造成錯誤或非預期的行為。

大部分的真實世界應用程式需要儲存狀態的 web 工作階段。此處的重點是不將它儲存在 web 伺服器上。 您可以在 cookie 或關閉處理序伺服器端使用快取提供者的 ASP.NET 工作階段狀態中用戶端，例如儲存狀態，以其他方式。 您可以在檔案儲存[Windows Azure Blob 儲存體](unstructured-blob-storage.md)而不是本機檔案系統。

為調整應用程式在 Windows Azure 網站中，如果您的 web 層無狀態是多麼簡單的範例，請參閱**標尺**索引標籤上的 Windows Azure 網站在管理入口網站中：

![調整 索引標籤](web-development-best-practices/_static/image1.png)

如果您想要新增 web 伺服器，您可以只將執行個體計數滑桿拖曳到右邊。 將它設定為 5，按一下 **儲存**，並在數秒內可 5 的 web 伺服器在處理您的網站流量的 Windows Azure 中。

![五個執行個體](web-development-best-practices/_static/image2.png)

您可以輕鬆地設定向 3 或回到 1 的執行個體計數。 當您調整後時，您就會啟動節省立即因為 Windows Azure 以分鐘數，不是依小時費用。

您也可以告知 Microsoft Azure 會自動增加或減少 CPU 使用量為基礎的 web 伺服器的數目。 在下列範例中，數個 web 伺服器時的 CPU 使用量低於 60%，將會降低為最小值為 2，以及如果 CPU 使用率超過 80%時，web 伺服器的數目會增加最多 4 個。

![調整的 CPU 使用量](web-development-best-practices/_static/image3.png)

或者，如果您知道您的網站將只會忙碌中工作期間？ 您可以告訴執行完整的多部伺服器，並減少 evenings、 nights，在單一伺服器和週末的 Windows Azure。 一系列如下的螢幕擷取畫面顯示如何將網站設定為執行在工作時間內從上午 8 點到下午 5 點的一部伺服器在離峰時間和 4 部伺服器。

![縮放所依據的排程](web-development-best-practices/_static/image4.png)

![設定排程時間](web-development-best-practices/_static/image5.png)

![白天的排程](web-development-best-practices/_static/image6.png)

![Weeknight 排程](web-development-best-practices/_static/image7.png)

![週末排程](web-development-best-practices/_static/image8.png)

這全都當然進行中的指令碼也如同在入口網站。

向外延展應用程式的功能，只要您避免阻礙以動態方式加入或移除伺服器的 Vm，讓 web 層保持無狀態是在 Windows Azure 中，幾乎沒有限制。

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>避免工作階段狀態

通常不是實際的真實世界雲端應用程式，以避免使用者工作階段中，儲存某種形式的狀態，但某些方法會影響效能和延展性比其他更多。 如果您有儲存狀態，最好的解決方案是狀態的小數量，並將其儲存在 cookie 中。 如果此方法不可行下, 一個最佳解決方案是使用 ASP.NET 工作階段狀態提供者[分散式、 記憶體中快取](distributed-caching.md#sessionstate)。 從效能和延展性的觀點而言最差的解決方式是使用資料庫備份的工作階段狀態提供者。

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>使用 CDN 快取靜態檔案資產

CDN 是內容傳遞網路的縮略字。 您提供靜態檔案資產，例如影像和指令碼檔案至 CDN 提供者和提供者會快取在全球資料中心中的這些檔案這樣的人員存取您的應用程式，只要他們取得相對較快速的回應和低延遲的快取資產。 這可加速站台整體的載入時間，並減少您的 web 伺服器上的負載。 Cdn 是達到廣泛地理位置分散的對象時尤其重要。

Windows Azure 有 CDN，而且您可以使用其他 Cdn 會在 Windows Azure 或任何 web 主機環境中執行的應用程式中。

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>為避免封鎖的呼叫中使用.NET 4.5 的非同步支援

.NET 4.5 增強的 C# 和 VB 的程式設計語言，以使其更容易以非同步方式處理工作。 非同步程式設計的優點不只適用於平行處理的情況下，例如，當您想要同時開始多個 web 服務呼叫。 它也可讓您的網頁伺服器來執行更有效率且可靠的高負載情況下。 Web 伺服器只具有有限的數目的執行緒可用，並在高負載情況下的所有執行緒在使用時，連入要求，必須等候，直到執行緒釋放。 如果您的應用程式程式碼未處理的工作，像是資料庫查詢和 web 服務呼叫以非同步方式，許多執行緒是不必要地繫結起來，當伺服器等候 I/O 回應時。 這會限制伺服器可以處理在高負載狀況下的流量。 非同步程式設計，web 服務或傳回資料的資料庫正在等候的執行緒釋放的最新要求提供服務資料之前收到。 在忙碌的 web 伺服器中，數百或數千個要求進行處理立即否則會等待執行緒釋放出它。

如稍早所見，很容易處理您的網站，因為它是以增加其 web 伺服器的數目會降低。 因此如果伺服器可以達到較大的輸送量，您不需要許多，而且因為您需要較少的伺服器指定的流量磁碟區比否則會降低成本。

.NET 4.5 非同步程式設計模型的支援包含在 ASP.NET 4.5 Web Form、 MVC、 和 Web API。在 Entity Framework 6，和[Windows Azure 儲存體 API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx)。

### <a name="async-support-in-aspnet-45"></a>ASP.NET 4.5 中的非同步支援

在 ASP.NET 4.5 支援非同步程式設計已新增語言到但 MVC、 Web Form 和 Web API 架構。 例如，ASP.NET MVC 控制器動作方法接收來自 web 要求的資料，並將資料傳遞至檢視，然後建立要傳送至瀏覽器的 HTML。 經常動作方法需要從資料庫或 web 服務取得資料，以顯示在網頁中，或是儲存在網頁中的輸入資料。 在這些案例中很容易使非同步動作方法： 而不是傳回*ActionResult*物件，傳回*工作&lt;ActionResult&gt;* 和標記方法與*非同步*關鍵字。 在方法內，當牽涉等候時間的作業程式碼行一開始您標示為使用 await 關鍵字。

以下是資料庫查詢呼叫的儲存機制方法的簡單動作方法：

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

此外，這裡有相同的方法會以非同步方式處理資料庫呼叫：

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

在幕後編譯器會產生適當的非同步程式碼。 當應用程式提出呼叫`FindTaskByIdAsync`，ASP.NET 使`FindTask`要求然後回溯背景工作執行緒，並使其可處理其他要求。 當`FindTask`動作的要求，繼續處理的程式碼，該呼叫之後重新啟動執行緒。 之間過渡期間`FindTask`起始要求，並傳回資料時，可讓執行緒可執行有用的工作，這也會停滯不前等待回應。

非同步程式碼中，某些額外負荷，但在低負載情況下，額外的負荷是微不足道時您能夠處理要求，否則會等待可用的執行緒持有高負載情況下。

一直執行 ASP.NET 1.1 中，因為非同步程式設計的這類但難以撰寫、 易發生錯誤且難以偵錯。 既然我們已簡化 ASP.NET 4.5 中的編碼，沒有理由就不再不要執行。

### <a name="async-support-in-entity-framework-6"></a>Entity Framework 6 中的非同步支援

一部分 4.5 中的非同步支援，我們隨附非同步支援 web 服務呼叫、 通訊端，以及檔案系統 I/O，web 應用程式最常見的模式是叫用的資料庫，但資料程式庫，我們不支援非同步。 現在 Entity Framework 6 新增資料庫存取權的非同步支援。

在 Entity Framework 6 會造成查詢或命令，以傳送至資料庫的所有方法都有非同步版本。 此處的範例將示範非同步版本的*尋找*方法。

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

支援這個非同步處理的運作方式不只是用於插入、 刪除、 更新和簡單，會發現，它也可以使用 LINQ 查詢：

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

沒有`Async`版本`ToList`方法，所以這個程式碼中，會使查詢傳送至資料庫的方法。 `Where`和`OrderByDescending`方法只會設定查詢，而`ToListAsync`方法執行查詢，並將儲存在回應`result`變數。

## <a name="summary"></a>總結

您可以實作 web 開發最佳作法此處所述任何 web 程式設計架構和任何雲端環境，但我們有工具可讓您輕鬆的 ASP.NET 和 Windows Azure。 如果您遵循這些模式，您可以輕鬆地向外擴充您的 web 層，您將最小化支出因為每一部伺服器可以處理更多流量。

[下一章](single-sign-on.md)會查看如何在雲端啟用單一登入案例。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源。

無狀態的 web 伺服器：

- [Microsoft Patterns and Practices-自動調整指引](https://msdn.microsoft.com/library/dn589774.aspx)。
- [停用 ARR 執行個體的 Windows Azure 網站中的同質](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)。 Erez Benari 部落格文章說明工作階段親和性在 Windows Azure 網站。

CDN:

- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九部影片系列。 1:34:00 時段 3 中的 CDN 討論，請參閱。
- [Microsoft 模式和作法靜態內容託管模式](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN 檢閱](http://www.cdnreviews.com/)。 許多 Cdn 的概觀。

非同步程式設計：

- [ASP.NET MVC 4 中使用非同步方法](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。 由 Rick anderson 發表的教學課程。
- [非同步程式設計使用 Async 和 Await （C# 和 Visual Basic）](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)。 說明非同步程式設計的基本原理、 ASP.NET 4.5 中的運作方式以及如何撰寫程式碼來實作它的 MSDN 技術白皮書。
- [Entity Framework 非同步查詢和儲存](https://msdn.microsoft.com/data/jj819165)
- [如何建立 ASP.NET Web 應用程式使用非同步](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7)。 由 Rowan Miller 的視訊簡報。 包含示範圖形，說明如何非同步程式設計也有助於大幅增加在高負載狀況下的 web 伺服器輸送量。
- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九部影片系列。 延展性的非同步程式設計的相關討論，請參閱時段 4 和 8 的時段。
- [使用 ASP.NET 4.5，再加上重要的陷阱中的非同步方法的識別常數](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx)。 部落格文章由 Scott Hanselman 主要是有關使用非同步方式在 ASP.NET Web Form 應用程式中。

針對其他 web 開發最佳作法，請參閱下列資源：

- [修正它範例應用程式的最佳作法](the-fix-it-sample-application.md#bestpractices)。 附錄 e-本書列出一些已修正它應用程式中實作的最佳作法。
- [Web 開發人員檢查清單](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [上一頁](continuous-integration-and-continuous-delivery.md)
> [下一頁](single-sign-on.md)
