---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: "資料分割策略 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: dca016cb6293a346f5622cc272e510b182c86d58
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>資料分割策略 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 數列的相關資訊，請參閱[第一章](introduction.md)。


我們稍早看到調整 web 層的雲端應用程式，透過加入和移除 web 伺服器是多麼的輕鬆。 但如果它們全部遇到相同的資料存放區，您的應用程式瓶頸將從前端移至後端，並為資料層不難調整。 在本章中我們會探討如何讓您的資料層可擴充的資料分割成多個關聯式資料庫，或結合其他資料儲存選項中的關聯式資料庫儲存體。

設定資料分割配置，最好完成先前所述的相同原因事先： 應用程式處於生產環境後，變更您的儲存體策略極為困難。 如果您認為硬碟最前面位置之不同方式，您可以避免在"Twitter 立即 「 當您的應用程式損毀，或當您重新組織您的應用程式資料和資料存取程式碼需要停機一段時間。

## <a name="the-three-vs-of-data-storage"></a>資料儲存在三個 Vs

若要判斷您是否需要資料分割策略，而且應該是什麼，請考慮有關資料的三個問題：

- 最後儲存磁碟區-您將會多少資料？ 幾個 gb？ 一些數百 gb？ Tb 嗎？ Pb 嗎？
- 速度 – 在您的資料將會成長的速度為何？ 它是不會產生大量資料的內部應用程式嗎？ 將上傳的客戶，影像和視訊到外部應用程式嗎？
- 各種 – 什麼類型的資料將儲存嗎？ 關聯式、 影像、 索引鍵-值組、 社交圖形？

如果您認為您將有很多的磁碟區、 速度或各種不同，您必須仔細考慮何種資料分割配置最可讓您的應用程式及來調整有效率且有效地成長，以確保您不必執行到的任何瓶頸。

基本上，有三種資料分割的方法：

- 垂直資料分割
- 水平資料分割
- 混合資料分割

## <a name="vertical-partitioning"></a>垂直資料分割

垂直 portioning 就像是資料表分割資料行： 資料行的一個集合分至一個資料存放區，和另一個資料行集便會進入不同的資料存放區。

例如，假設我的應用程式儲存人員，包括映像相關的資料：

![資料表格](data-partitioning-strategies/_static/image1.png)

當您為資料表資料，並查看不同種類的資料時，您可以看到三個資料行，在左邊有可以有效率地儲存關聯式資料庫中，在右側的兩個資料行時基本上位元組陣列的字串資料 come 從映像檔案。 您可在關聯式資料庫中，儲存體映像檔案資料，許多人這樣做，因為它們不想要將資料儲存至檔案系統。 不具備能夠儲存的資料所需的磁碟區的檔案系統，或它們可能不想要管理個別的備份和還原系統。 這種方法適用於內部部署資料庫和雲端資料庫中的資料量。 在內部部署環境中，可能很容易就讓 DBA 負責處理的所有項目。

但在雲端資料庫中，儲存體是相當耗費資源，以大量的映像無法成長超出限制，它可以有效的操作資料庫的大小。 您可以解決這些問題的垂直分割的資料表示，您可以選擇最適當的資料存放區中，每個資料行資料的資料表中。 什麼可能最適合用於這個範例是將字串資料放在關聯式資料庫和 Blob 儲存體中的映像中。

![垂直資料分割的資料表](data-partitioning-strategies/_static/image2.png)

將影像儲存在 Blob 儲存體，而非資料庫是比在內部部署環境中在雲端中更為實用，因為您不必擔心設定檔案伺服器或管理備份和還原儲存在關聯式資料庫外部的資料： 所有會為您自動處理 Blob 儲存體服務。

這是資料分割方法，我們實作修正它，應用程式中，我們會探討程式碼中的[Blob 儲存體章](unstructured-blob-storage.md)。 而不需要此資料分割配置，並假設平均映像的大小 3 mb，修正它應用程式會只能儲存大約 40,000 工作才按下的資料庫大小上限為 150 gb。 移除映像之後，資料庫可以儲存 10 倍為許多工作;您必須思考如何實作水平資料分割配置之前，您可以移更長的時間。 而且因為應用程式比例，支出速度更慢成長因為大量的儲存體需求會進入極低的 Blob 儲存體。

## <a name="horizontal-partitioning-sharding"></a>水平資料分割 （分區化）

水平 portioning 就像是分割資料表的資料列所： 的資料列的一個集合分至一個資料存放區，以及資料列的其他集合分至不同的資料存放區。

指定相同的資料集，另一個選項是儲存在不同資料庫中的不同範圍的客戶名稱。

![水平資料分割的資料表](data-partitioning-strategies/_static/image3.png)

您想要特別小心確定為了避免作用點的資料會平均分散分區化配置。 此簡單範例中使用的最後一個名稱的第一個字母不符合這項需求，因為許多人有某些常見的字母為開頭的姓氏。 就會早於您可能預期會因為某些資料庫會得到非常大，但大部分仍會保持小型叫用資料表的大小限制。

項目的水平資料分割的缺點是，它可能是硬碟上的所有資料都進行的查詢。 在此範例中，查詢必須繪製於最多 26 個不同的資料庫，若要取得所有應用程式所儲存的資料。

## <a name="hybrid-partitioning"></a>混合資料分割

您可以結合垂直和水平資料分割。 例如，範例資料中，可以將影像儲存在 Blob 儲存體，水平資料分割。

![資料分割的資料表混合式](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>資料分割的實際執行應用程式

在概念上很容易看到資料分割配置如何運作，但任何分割區的配置會增加程式碼複雜度及會引進許多新的複雜性，您必須處理。 當您移動至 blob 儲存體的映像，該怎麼辦的儲存體服務已關閉時？ 您要如何處理 blob 安全性？ 如果資料庫和 blob 儲存體取得同步會怎樣？ 如果您是分區化，您要如何處理跨所有資料庫都查詢？

複雜功能是可管理的只要您移至生產環境之前，您計劃。 許多人沒有這樣做，想要它們在更新版本。 我們的客戶諮詢團隊 (CAT) 小組來自客戶的應用程式就可以非常大的方式，取得有關每月一次驚慌失措撥打電話的平均，它們沒有執行這項規劃。 他們說類似: 「 協助 ！ 我將所有項目放在一個資料存放區和在 45 天我將在其上執行空間不足 」 ！ 如果您有許多的內建資料存放區的存取方式的商務邏輯，而且您有使用您的應用程式的客戶，是當您移轉每日當機沒有好時機。 我們最後其資料 herculean 投入時間，以協助客戶資料分割經過即時而不需要停機。 它是非常讓人動心，而且非常嚇人，而非您想要參與如果您可以避免這樣 ！ 考慮這最前面位置，並將其整合至您的應用程式才會進行您的生活中來得簡單許多應用程式稍後成長。

## <a name="summary"></a>總結

有效的資料分割配置，可以啟用您的雲端應用程式擴充，以 pb 計的瓶頸不在雲端中的資料。 且您不必支付最前面位置龐大的機器或廣泛的基礎結構如同您在內部部署資料中心執行應用程式一樣。 在雲端中您可以以累加方式增加容量，您需要它，並只錢就像您使用當您使用它。

在[下一章](unstructured-blob-storage.md)我們會看到它修正應用程式如何實作透過 Blob 儲存體中儲存影像的垂直資料分割。

## <a name="resources"></a>資源

如需有關資料分割策略的詳細資訊，請參閱下列資源。

文件集：

- [Windows Azure 雲端服務中大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。
- [Microsoft Patterns and Practices-雲端設計模式](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱資料分割的指導方針，分區化模式。

影片：

- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 九部分 Ulrich Homann、 Marc Mercuri 和 Mark Simms 的系列。 高層級概念與架構原則非常可存取且有趣的方式，呈現劇本取自與實際客戶的 Microsoft 客戶諮詢團隊 (CAT) 體驗。 在事件 7 中分割的討論，請參閱。
- [建置大： 從 Windows Azure 客戶的第 I 部分學](https://channel9.msdn.com/Events/Build/2012/3-029)。Mark Simms 討論資料分割配置，分區化策略如何實作分區化和 SQL Database 同盟，19:49 處開始。 類似於保全數列但進入詳細的使用說明。

範例程式碼：

- [雲端服務基本概念，在 Windows Azure 中的](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 包含分區化資料庫的範例應用程式。 如需實作的分區化配置的說明，請參閱[DAL – 分區化的 RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) Windows Azure 部落格上。

>[!div class="step-by-step"]
[上一頁](data-storage-options.md)
[下一頁](unstructured-blob-storage.md)
