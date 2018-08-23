---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: 資料分割策略 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 1a19b5b8ce47439c39359a805e16ab4e7631deec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835063"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>資料分割策略 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 這個系列的相關資訊，請參閱[第 1 章](introduction.md)。


稍早我們看到調整的 web 層的雲端應用程式中，新增和移除網頁伺服器是多麼容易。 但如果它們全部遇到相同的資料存放區，您的應用程式瓶頸將移到前端到後端，然後資料層是最難調整。 在這一章中我們會探討如何讓您的資料層可調整的資料分割成多個關聯式資料庫，或藉由結合關聯式資料庫儲存體與其他資料儲存體選項。

設定資料分割配置是最佳完成先前所述的相同原因做好： 它是很難在生產環境中應用程式之後，變更您的資料儲存策略。 如果您思考硬碟最前面位置不同的方法，您可以避免在 「 Twitter 一下 「 當您的應用程式當機或停止長時間時您重新組織您的應用程式資料和資料存取程式碼。

## <a name="the-three-vs-of-data-storage"></a>在三個 Vs 的資料存放區

若要判斷您是否需要資料分割策略，而且應該是什麼，請考慮您的資料相關的三個問題：

- 最終存放區的磁碟區-您將會多少資料？ 幾個 gb 嗎？ 幾個數百 gb 嗎？ Tb 規模嗎？ Pb 規模嗎？
- 速度 – 在您的資料將會成長的速度為何？ 是內部應用程式，不會產生大量資料嗎？ 將上傳的客戶，影像和影片到外部應用程式嗎？
- 各種不同 – 什麼類型的資料會儲存嗎？ 關聯式、 影像、 索引鍵 / 值組、 社交圖形？

如果您認為你有很多的磁碟區、 速度或各種不同，您必須仔細考慮何種資料分割配置最適合可讓您的應用程式調整有效率且有效地成長，以及以確保您沒有遇到任何瓶頸。

基本上有三種資料分割的方法：

- 垂直資料分割
- 水平資料分割
- 混合資料分割

## <a name="vertical-partitioning"></a>垂直資料分割

垂直 portioning 就像是將資料表分割資料行： 資料行的一個集合分至一個資料存放區，以及資料行的其他集合分至不同的資料存放區。

例如，假設我的應用程式會將儲存的人員，包括映像的相關資料：

![資料表](data-partitioning-strategies/_static/image1.png)

當您代表此資料，為資料表，並看看不同的資料時，您可以看到在左側的三個資料行有可以有效率地儲存關聯式資料庫中，而右邊的兩個資料行則基本上是位元組陣列的字串資料的 come 從映像檔案。 您可在關聯式資料庫中，儲存體映像檔案的資料，很多人這麼做，因為他們不想將資料儲存至檔案系統。 不具備能夠儲存的資料所需的磁碟區的檔案系統，或它們可能不想要管理個別的備份和還原系統。 這種方法適用於內部部署資料庫和少量的雲端資料庫中的資料。 在內部部署環境中，它可能只是讓 DBA 處理好一切變得更容易。

在雲端資料庫中，儲存體是相當耗成本，但有大量的映像可以進行成長超過限制的它可以有效率地操作資料庫的大小。 您可以解決這些問題，由分割資料以垂直方式，這表示您選擇最適當的資料存放區中，每個資料行資料的資料表中。 什麼可能最適合用於此範例會將字串資料放在關聯式資料庫和 Blob 儲存體中的映像。

![垂直分割的資料表](data-partitioning-strategies/_static/image2.png)

將映像儲存在 Blob 儲存體，而非資料庫是定域機組中比在內部部署環境中更為實用，因為您不必擔心如何設定檔案伺服器或管理備份與還原儲存在關聯式資料庫外部的資料： 所有會為您自動處理 Blob 儲存體服務。

這是資料分割方法，我們實作修正其應用程式中，而且我們將探討程式碼，在[Blob 儲存體章](unstructured-blob-storage.md)。 如果沒有這個資料分割配置，和假設平均的映像的大小 3 mb，它修正應用程式只會能夠儲存約 40,000 個工作之前達到資料庫大小上限為 150 gb 資料。 移除映像之後，資料庫可以儲存 10 倍的工作;您需要思考如何實作水平資料分割配置之前，您即可更長的時間。 而且因為應用程式擴展，補貼速度更慢成長因為大量的儲存體需求會到極低的 Blob 儲存體。

## <a name="horizontal-partitioning-sharding"></a>水平資料分割 （分區化）

水平 portioning 就像是依資料列分割資料表： 資料列的一個集合分至一個資料存放區，和的資料列的其他集合分至不同的資料存放區。

指定相同的資料集，另一個選項是將不同範圍的客戶名稱儲存在不同的資料庫。

![水平分割的資料表](data-partitioning-strategies/_static/image3.png)

您想要特別小心處理您的分區化配置，請確定資料平均分佈以避免作用點。 此簡單範例中使用的最後一個名稱的第一個字母不符合這項需求，因為很多人有某些常見的字母為開頭的姓氏。 您會早於您可能預期，因為某些資料庫會非常大，而大部分會維持小型達資料表大小限制。

項目的水平資料分割的缺點是，它可能難以在所有的資料進行查詢。 在此範例中，查詢必須以繪製於最多 26 個不同的資料庫，若要取得所有的應用程式所儲存的資料。

## <a name="hybrid-partitioning"></a>混合資料分割

您可以結合垂直和水平資料分割。 比方說，在此範例資料中，您可以將影像儲存在 Blob 儲存體並水平資料分割的字串資料。

![資料分割的資料表混合式](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>資料分割的實際執行應用程式

在概念上很容易了解資料分割配置會運作，但任何資料分割配置會增加複雜性，並會引進許多新的複雜性，您需要處理。 如果您要移動到 blob 儲存體的映像，該怎麼辦的儲存體服務已關閉時？ 您要如何處理 blob 安全性？ 如果資料庫和 blob 儲存體取得同步發生什麼事？ 如果您是分區化時，您要如何處理跨所有資料庫都查詢？

複雜功能是可管理的只要您計劃，您移至生產環境之前。 不這麼做的很多人希望他們有更新版本。 我們的客戶諮詢團隊 (CAT) 小組從客戶的應用程式企業顯然開始採用非常大的方式，取得驚慌失措的電話撥號，約一次一個月的平均和它們並沒有進行此計劃。 和他們說: 「 協助 ！ 我將所有東西放在單一資料存放區，並在 45 天我要在其中執行空間不足 」 ！ 而且如果您有許多的內建於您如何存取您的資料存放區的商務邏輯，而且您必須使用您的應用程式的客戶，沒有任何移轉時需要停機一天的好時機。 我們得到其資料 herculean 致力於協助客戶分割區進行即時而不需要停機。 它非常令人期待且很嚇人，而且非您想要參與能避免 ！ 日復一日提前，並將它整合到您的應用程式可讓工作輕鬆許多如果應用程式增加到更新版本。

## <a name="summary"></a>總結

有效的資料分割配置可以讓您的雲端應用程式擴充到 pb 的資料在雲端取得，沒有瓶頸。 您不必支付事先大規模的機器或廣泛的基礎結構就像如果您已在內部部署資料中心內執行應用程式。 在雲端中您可以以累加方式新增容量，您需要它，以及只錢就像您使用當您使用它。

在 [[下一步] 一章](unstructured-blob-storage.md)我們會看到如何修正它應用程式實作垂直資料分割，將映像儲存在 Blob 儲存體。

## <a name="resources"></a>資源

如需有關資料分割策略的詳細資訊，請參閱下列資源。

文件：

- [Windows Azure 雲端服務上大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。
- [Microsoft Patterns and Practices-雲端設計模式](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱資料分割指引，分區化模式。

影片：

- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部組件系列。 提供高階的概念和架構原則非常可存取且有趣的方式，取自 Microsoft 客戶諮詢團隊 (CAT) 體驗，與實際客戶的劇本。 請參閱資料分割的討論內容，在集中 7。
- [建置大型： 從第一部 Windows Azure 客戶群 Lessons learned](https://channel9.msdn.com/Events/Build/2012/3-029)。Mark Simms 討論資料分割配置，分區化策略如何實作分區化，以及 SQL Database 同盟，開始 19:49。 類似於保全數列但作法的詳細說明。

範例程式碼：

- [雲端服務基本概念，在 Windows Azure 中的](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 包含分區化資料庫的範例應用程式。 如需實作分區化配置的說明，請參閱 < [DAL-分區化的 RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) Windows Azure 部落格上。

> [!div class="step-by-step"]
> [上一頁](data-storage-options.md)
> [下一頁](unstructured-blob-storage.md)
