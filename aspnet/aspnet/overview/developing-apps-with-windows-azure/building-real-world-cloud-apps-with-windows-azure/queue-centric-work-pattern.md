---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: 以佇列為主的工作模式 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c454023c26d73df174a43b3a8ad6745ef8654fd2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834641"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>以佇列為主的工作模式 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


稍早，我們所見，使用多個服務可能會導致 「 複合 」 的 SLA，應用程式的有效 SLA 所在*產品*的個別的 Sla。 比方說，它修正應用程式所使用的網站、 儲存體和 SQL Database。 如果這些服務的任何一個失敗，應用程式會傳回錯誤給使用者。

快取是以處理暫時性失敗的唯讀內容的好方法。 但是，如果您的應用程式需要執行工作？ 比方說，當使用者提交新修正它的工作，應用程式不能只將工作放入快取。 必須為持續性資料存放區中，修正它工作寫入，因此它可以處理應用程式。

這是以佇列為主的工作模式之處。 此模式可讓 web 層和後端服務之間的鬆散結合。

以下是模式的運作方式。 當應用程式取得要求時，它會將工作項目放入佇列，並立即傳回回應。 然後個別的後端程序會從佇列的工作項目，並完成工作。

以佇列為主的工作模式適合用來：

- 很多時間 （高延遲） 的工作。
- 需要外部的服務可能不一定有足夠的工作。
- 也就是使用大量資源 (高 CPU)。
- 調節 （受限於突然負載高載） 的速率而獲益的工作。

## <a name="reduced-latency"></a>較低的延遲

佇列是的每當您執行耗時的工作很有用。 如果工作需要幾秒鐘或更久，而不會封鎖使用者，將工作項目放入佇列。 「 我們正努力，"會告訴使用者，然後使用佇列接聽程式來處理在背景工作。

比方說，當您購買項目在線上零售店，網站會確認您的訂單立即。 但這並不表示您的資料已在 「 卡車 」 傳遞。 他們將工作放在佇列中，以及在背景中進行信用查核，準備您的項目進行傳送，和這些等。

短暫延遲的情況下，總計的端對端時間可能較長的使用佇列，相較於以同步方式執行該工作。 但即使如此，其他優點會勝的缺點。

## <a name="increased-reliability"></a>增加的可靠性

修正它，我們探討了目前的版本，在 web 前端會緊密結合的 SQL Database 後端。 如果 SQL database 服務無法使用時，使用者會收到錯誤。 如果重試無法運作 （也就是失敗是多個暫時性），您可以執行的唯一會顯示錯誤，並要求使用者稍後再試。

![圖表顯示的 web 前端失敗的 SQL Database 後端的失敗時](queue-centric-work-pattern/_static/image1.png)

當使用者提交 Fix It 工作，請使用佇列，應用程式可以將訊息寫入佇列。 訊息承載[JSON](http://json.org/)工作的表示法。 只要將訊息寫入至佇列，應用程式就會傳回，並立即向使用者顯示成功訊息。

如果任何後端服務 – 例如，SQL database 或佇列接聽程式--離線，使用者仍然可以提交新修正它的工作。 後端服務再度變成可用之前，只是排入佇列的訊息。 此時後, 端服務會趕上進度待辦項目。

![此圖顯示 web 前端繼續函式 SQL Database 錯誤時](queue-centric-work-pattern/_static/image2.png)

此外，現在您可以新增更多的後端邏輯而不需擔心前端的恢復功能。 比方說，您可能要將電子郵件或 SMS 訊息傳送至擁有者，只要指派新修正它。 如果電子郵件或 SMS 服務變成無法使用，可處理所有項目，然後再將訊息放入個別的佇列，以傳送電子郵件/簡訊。

以前，我們有效的 SLA 是 Web Apps&times;儲存體&times;SQL Database = 99.7%。 (請參閱[設計存留失敗](design-to-survive-failures.md)。)

當我們變更應用程式使用佇列時，web 前端僅取決於 Web 應用程式和儲存體，複合 99.8%的 SLA。 （請注意，佇列是 Azure 儲存體服務的一部分，因此它們會包含在相同的 SLA，做為 blob 儲存體）。

如果您需要更棒的是比 99.8%，則您可以在兩個不同區域中建立兩個佇列。 將做為主要伺服器上，而另一個指定為次要資料庫。 在您的應用程式中，容錯移轉至次要佇列如果主要佇列無法使用。 這兩個變成無法使用一次的機會就非常渺小。

## <a name="rate-leveling-and-independent-scaling"></a>速率調節，以及獨立調整

佇列也可用於所謂*速率調節*或是*負載調節*。

Web 應用程式很容易在流量突然暴增。 雖然您可以使用自動調整，自動將 web 伺服器，以處理增加的網路流量，自動調整可能無法回應速度不夠快處理突然尖峰負載。 如果 web 伺服器可以卸載某些工作，他們只需要藉由將訊息寫入佇列，它們能夠處理更多流量。 後端服務可以接著從佇列讀取訊息並加以處理。 佇列的深度會放大或縮小的傳入的負載互異。

大部分的耗時工作卸載給後端服務，web 層可以更輕鬆地回應突然出現流量尖峰。 和您節省成本，因為較少的 web 伺服器可以處理任何指定的數量的流量。

您可以在 web 層和後端服務會分別調整。 例如，您可能需要三部 web 伺服器，但只有一個伺服器處理佇列訊息。 或者，如果您在背景中執行運算密集工作，您可能需要更多的後端伺服器。

![](queue-centric-work-pattern/_static/image3.png)

自動調整運作方式與後端服務以及 web 層。 您可以相應增加或相應減少處理的工作在佇列中後, 端 Vm 的 CPU 使用量為基礎的 Vm 數目。 或者，您可以在佇列中有多少項目為基礎的自動調整。 比方說，您可以告訴自動調整功能再嘗試將佇列中保留最多 10 個項目。 如果佇列有 10 個以上的項目，會將 Vm 新增自動調整規模。 跟上時自動調整規模將會卸除額外的 Vm。

## <a name="adding-queues-to-the-fix-it-application"></a>將它排入佇列的修正程式的應用程式

若要實作的佇列模式，我們需要修正其應用程式進行兩項變更。

- 當使用者提交新修正它的工作時，請將工作放在佇列中，而不是寫入至資料庫。
- 建立後端服務，以處理佇列中的訊息。

我們將使用佇列[Azure 佇列儲存體服務](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/)。 另一個選項是使用[Azure 服務匯流排](https://docs.microsoft.com/azure/service-bus/)。

若要決定要使用的佇列服務，請考慮您的應用程式需要的方式傳送和接收佇列中的訊息：

- 如果您合作的產生者和競爭取用者，請考慮使用 Azure 佇列儲存體服務。 「 Cooperating 生產者 」 表示多個處理序會將訊息新增至佇列。 「 競爭取用者 」 表示多個處理序提取訊息從佇列處理它們，但任何指定的訊息只能處理一個 「 取用者。 」 如果您需要更多的輸送量比可以使用單一佇列，請使用其他的佇列和/或其他儲存體帳戶。
- 如果您需要[發佈/訂閱模型](http://en.wikipedia.org/wiki/Publish/subscribe)，請考慮使用 Azure 服務匯流排佇列。

修正其應用程式符合合作的產生者和競爭取用者模型。

另一個考量是應用程式的可用性。 佇列儲存體服務屬於相同的服務，我們會使用 blob 儲存體，因此使用它不會影響我們的 SLA。 Azure 服務匯流排是個別的服務，以它自己的 SLA。 如果我們使用服務匯流排佇列時，我們必須納入其他的 SLA 百分比，以及複合 SLA 會降低。 當您選擇的佇列服務時，請確定您了解您選擇對應用程式可用性的影響。 如需詳細資訊，請參閱 <<c0> [ 資源](#resources)一節。

## <a name="creating-queue-messages"></a>建立佇列訊息

若要將 Fix It 」 工作放在佇列上，web 前端，請執行下列步驟：

1. 建立[CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx)執行個體。 `CloudQueueClient`執行個體用來執行對佇列服務的要求。
2. 如果尚不存在，請建立佇列。
3. 將序列化 Fix It 工作。
4. 呼叫[CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx)將訊息放到佇列。

我們會進行這項工作的建構函式和`SendMessageAsync`的新方法`FixItQueueManager`類別。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

我們將使用以下[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)序列化為 JSON 格式 fixit 的程式庫。 您可以使用任何您偏好的序列化方法。 JSON 具有的優點是人類看得懂，同時不比 XML 簡潔。

生產環境品質的程式碼會新增錯誤處理邏輯，暫停如果資料庫變成無法使用、 更簡潔地處理復原、 在應用程式啟動時，建立佇列及管理 「[有害 」 訊息](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx)。 （有害訊息是因為某些原因無法處理的訊息。 您不想要放入佇列，其中背景工作角色會持續嘗試進行處理、 失敗、 再試一次、 失敗，等等的有害訊息。）

在前端的 MVC 應用程式中，我們需要更新建立新工作的程式碼。 而不是將工作放入存放庫中，呼叫`SendMessageAsync`如上所示的方法。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>處理佇列訊息

若要處理佇列中的訊息，我們將建立後端服務。 後端服務會執行一個無限迴圈，執行下列步驟：

1. 從佇列取得下一個訊息。
2. 修正它工作將訊息還原序列化。
3. 寫入資料庫中的 It 工作。

若要裝載的後端服務，我們將建立 Azure 雲端服務，其中包含*背景工作角色*。 背景工作角色可以執行後端處理的一或多個 Vm 所組成。 在這些 Vm 中執行的程式碼會提取從佇列中可用的訊息。 對於每個訊息中，我們會還原序列化的 JSON 承載，並修正它工作實體的執行個體寫入至資料庫，使用我們稍早在 web 層中使用的相同儲存機制。

下列步驟示範如何新增背景工作角色專案，以具有標準的 web 專案的方案。 已修正它在專案中，您可以下載完成這些步驟。

第一次將雲端服務專案新增至 Visual Studio 方案中。 以滑鼠右鍵按一下方案，然後選取**新增**，然後**新的專案**。 在左窗格中，依序展開**Visual C#** ，然後選取**雲端**。

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

在 [**新的 Azure 雲端服務**] 對話方塊中，展開**Visual C#** 在左窗格中的節點。 選取 **背景工作角色**，然後按一下向右箭號圖示。

![](queue-centric-work-pattern/_static/image6.png)

(請注意，您也可以加入*web 角色*。 我們可以執行修正它在相同的雲端服務，而不是執行它在 Azure 網站前端。 具有一些優點，讓您更輕鬆地協調前端與後端之間的連線。 不過，為了簡化這段示範影片，正在將前端放在 Azure App Service Web 應用程式，並只執行雲端服務中的 後端。）

將預設名稱被指派給背景工作角色。 若要變更名稱，滑鼠停留在背景工作角色，在右窗格中，然後按一下鉛筆圖示。

![](queue-centric-work-pattern/_static/image7.png)

按一下 **確定**來完成對話方塊。 這會將兩個專案加入至 Visual Studio 方案中。

- Azure 專案定義雲端服務，包括組態資訊。
- 定義背景工作角色的背景工作角色專案。

![](queue-centric-work-pattern/_static/image8.png)

如需詳細資訊，請參閱[使用 Visual Studio 建立 Azure 專案。](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

在背景工作角色中，我們輪詢訊息藉由呼叫`ProcessMessageAsync`方法的`FixItQueueManager`上文所述的類別。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync`方法會檢查是否有訊息等待。 如果有的話，它將訊息還原序列化成`FixItTask`實體，並將實體儲存在資料庫中。 它會循環直到佇列是空的。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

輪詢的佇列訊息時，會產生小型的交易收費，所以在沒有訊息等待處理，背景工作角色`RunAsync`方法會等候一秒再輪詢一次呼叫`Task.Delay(1000)`。

在 web 專案中，新增非同步程式碼可以自動改善效能，因為 IIS 管理有限的執行緒集區。 不在背景工作角色專案中的案例。 若要改善延展性的背景工作角色，您可以撰寫多執行緒程式碼，或使用非同步程式碼來實作[平行程式設計](https://msdn.microsoft.com/library/ff963553.aspx)。 此範例不會實作平行程式設計，但示範如何讓程式碼非同步，因此您可以實作平行程式設計。

## <a name="summary"></a>總結

在這一章中，您已了解如何改善應用程式回應速度、 可靠性和延展性，藉由實作以佇列為主的工作模式。

這是本電子書，涵蓋 13 模式的最後一個，但當然還有許多其他的模式和作法，可協助您建置成功的雲端應用程式。 [最後一章](more-patterns-and-guidance.md)的主題未涵蓋的這些 13 的模式，提供資源的連結。

<a id="resources"></a>
## <a name="resources"></a>資源

如需有關佇列的詳細資訊，請參閱下列資源。

文件：

- [Microsoft Azure 儲存體佇列第 1 部分： 開始使用](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/)。 Roman Schacherl 文章。
- [執行背景工作](https://msdn.microsoft.com/library/ff803365.aspx)，第 5 章[應用程式移至雲端，也就是第 3 版](https://msdn.microsoft.com/library/ff728592.aspx)從 Microsoft Patterns and Practices。 (特別是，一節[「 使用 Azure 儲存體佇列 」](https://msdn.microsoft.com/library/ff803365.aspx#sec7)。)
- [最大化延展性和成本的效益的 Azure 上的佇列架構傳訊解決方案的最佳作法](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx)。 由 Valery Mizonov 詘躩裛。
- [比較 Azure 佇列和服務匯流排佇列](https://msdn.microsoft.com/magazine/jj159884.aspx)。 MSDN Magazine 文章會提供可協助您選擇要使用的佇列服務的其他資訊。 本文章提及服務匯流排是相依於 ACS 進行驗證，表示無法使用 ACS 時，SB 佇列可能會無法使用。 不過，因為發行項所撰寫，SB 已變更為可讓您使用[SAS 權杖](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx)作為 ACS 的替代方案。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱非同步傳訊入門 」、 「 管道和篩選條件模式 」、 「 補償交易模式 」、 「 競爭取用者模式、 「 CQRS 模式。
- [CQRS 旅程](https://msdn.microsoft.com/library/jj554200)。 由 Microsoft Patterns and Practices CQRS 相關的電子書。

影片：

- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部部分影片系列。 提供高階的概念和架構原則非常可存取且有趣的方式，取自 Microsoft 客戶諮詢團隊 (CAT) 體驗，與實際客戶的劇本。 Azure 儲存體服務和佇列的簡介，請參閱第 5 集開始 35:13。

> [!div class="step-by-step"]
> [上一頁](distributed-caching.md)
> [下一頁](more-patterns-and-guidance.md)
