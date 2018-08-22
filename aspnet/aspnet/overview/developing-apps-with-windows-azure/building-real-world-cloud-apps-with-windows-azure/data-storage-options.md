---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: 資料儲存體選項 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 239592b6e356436e86be1a2ab2f994343884cf90
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830564"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>資料儲存體選項 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


大部分的人會使用關聯式資料庫，而且他們會嘗試尋找其他資料儲存體選項，它們在設計雲端應用程式時。 結果可以是次佳的效能、 高的費用或更糟的是，，因為[NoSQL](http://en.wikipedia.org/wiki/NoSQL) （非關聯式） 的資料庫可以比關聯式資料庫更有效率地處理某些工作。 當客戶尋求協助解決重要的資料儲存體問題時，通常是因為它們有關聯式資料庫所在的 NoSQL 選項的其中一個會成效。 在這些情況下客戶已經好到哪裡去如果它們之前先部署至生產環境的應用程式實作了 NoSQL 解決方案。

相反地，它也會假設，NoSQL 資料庫可以執行的所有項目也或夠好的錯誤。 沒有單一最佳資料管理選項的所有資料儲存體的工作;不同的資料管理解決方案最適合用於不同的工作。 大部分的真實世界的雲端應用程式有各種不同的資料儲存需求，並由多個資料儲存體解決方案的組合通常提供最佳。

本指南的目的是讓您更廣泛了解可用的資料儲存體選項的雲端應用程式，以及如何選擇適合您案例的一些基本指引。 最好是了解這些選項可供您和您開發應用程式之前，仔細考量其優點和缺點。 變更資料儲存體選項，在生產環境應用程式可能會非常困難，像是讓飛機航班時變更 jet 引擎。

## <a name="data-storage-options-on-azure"></a>在 Azure 上的資料儲存體選項

雲端可讓您輕鬆使用各種不同的關聯式和 NoSQL 資料存放區。 以下是一些您可以使用 Azure 中的資料儲存體平台。

![](data-storage-options/_static/image1.png)

下表顯示四種類型的 NoSQL 資料庫：

- [索引鍵/值資料庫](https://msdn.microsoft.com/library/dn313285.aspx#sec7)儲存單一序列化的物件，針對每個索引鍵的值。 它們適合用來儲存大量的資料，其中您想要取得指定索引鍵值的一個項目，而您不需要此項目的其他屬性為基礎的查詢。

    [Azure Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/)是函式，例如檔案儲存體在雲端中，其對應至資料夾和檔案名稱的索引鍵值的索引鍵/值資料庫。 您擷取的檔案，其資料夾和檔案名稱，而不使用搜尋的檔案內容中的值。

    [Azure 資料表儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)也是索引鍵/值資料庫。 每個值都稱為*實體*（類似於資料列，資料分割索引鍵和資料列索引鍵所識別） 和包含多個*屬性*(類似於資料行，但並非所有資料表中的實體都必須共用相同資料行）。 查詢不同於索引鍵資料行是非常沒有效率，應該予以避免。 例如，您可以儲存使用者設定檔資料，以儲存單一使用者的相關資訊的一個資料分割。 在不同的一個實體的屬性，或在相同的資料分割中的個別實體中，您可以儲存資料，例如使用者名稱、 密碼雜湊、 生日等等。 但您不想要與指定之範圍的出生日期的所有使用者的查詢和聯結查詢無法執行設定檔資料表與另一個資料表之間。 資料表儲存體是更具擴充性和關聯式資料庫中，成本較低，但它不會啟用複雜的查詢或聯結。
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8)的值是在其中的索引鍵/值資料庫*文件*。 這裡的 「 文件 」 不會使用 Word 或 Excel 的文件的意義，但表示具名的欄位和值，其中任何一種可能的子文件的集合。 例如，訂單歷程記錄資料表中的訂單文件可能有訂單號碼、 訂購日期，以及客戶欄位;和 [客戶] 欄位可能會有名稱和地址的欄位。 資料庫會將編碼格式，例如 XML、 YAML、 JSON 或 BSON; 中的欄位資料或者，它可以使用純文字。 設定文件資料庫，除了索引鍵/值資料庫的一項功能是能夠查詢非索引鍵欄位，並定義可讓查詢更有效率的次要索引。 這項功能可讓您更適合需要擷取資料的文件索引鍵的值比更複雜的準則為基礎的應用程式的文件資料庫。 例如，銷售訂單歷程記錄文件資料庫中，您可以查詢各種欄位，例如產品識別碼、 客戶編號、 客戶名稱等。 [MongoDB](http://www.mongodb.org/)是熱門的文件資料庫。
- [資料行系列資料庫](https://msdn.microsoft.com/library/dn313285.aspx#sec9)是索引鍵/值可讓您建構資料的儲存體為集合關聯的資料行稱為資料行系列資料存放區。 比方說，人口普查資料庫可能有一組人員名稱的資料行 （第一，中間名、 姓氏），一個群組，該人員的位址和一個群組是個人的設定檔資訊 (DOB、 性別等等。)。 然後資料庫就可以每個資料行系列儲存在個別的資料分割中同時保留所有人與相同的索引鍵相關的資料。 您接著可以讀取所有設定檔資訊而不需要閱讀的所有名稱和位址資訊以及。 [Cassandra](http://cassandra.apache.org/)是普遍的資料行系列資料庫。
- [圖表資料庫](https://msdn.microsoft.com/library/dn313285.aspx#sec10)儲存資訊的物件和關聯性集合。 圖表資料庫的目的是要讓應用程式有效率地執行查詢，它們之間周遊的網路物件和關聯性。 比方說，物件可能是員工在人力資源資料庫中，而您可能想以促進查詢這類 「 尋找直接或間接工作的所有員工 scott 」。 [Neo4j](http://www.neo4j.org/)是受歡迎的圖形資料庫。

相較於關聯式資料庫，NoSQL 選項會提供更佳的延展性和成本效益的儲存體及非結構化資料的分析。 缺點是它們未提供的豐富 queryability 和關聯式資料庫穩固的資料完整性功能。 NoSQL 會適用於的 IIS 記錄檔資料，這牽涉到大量不需要聯結查詢。 NoSQL 會無法運作得很好的銀行交易，它需要絕對資料完整性，並牽涉到許多其他帳戶相關的資料關聯性。

另外還有較新的類別，呼叫的資料庫平台[NewSQL](http://en.wikipedia.org/wiki/NewSQL) ，結合的一種 NoSQL 資料庫延展性 queryability 與關聯式資料庫的交易完整性。 NewSQL 資料庫專為分散式儲存體和查詢處理，通常很難在"OldSQL 」 資料庫中實作。 [NuoDB](http://www.nuodb.com/) NewSQL 資料庫可在 Azure 上的範例。

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop 與 MapReduce

高的磁碟區，您可以在 NoSQL 資料庫中儲存的資料可能很難有效地分析即時的。 若要這麼做，您可以使用類似的架構[Hadoop](http://hadoop.apache.org/)它會實作[MapReduce](http://en.wikipedia.org/wiki/MapReduce)功能。 基本上是哪些 MapReduce 程序會如下所示：

- 需要選取資料處理的資料大小只儲存的資料實際上，您需要分析的限制。 例如，您會想要知道您的使用者的出生年份，基底結構，因此您選取只出生年份，從您的使用者設定檔資料存放區。
- 細分成組件的資料，並將它們傳送到不同的電腦進行處理。 電腦 A 計算 1950年 1959年日期的人數，電腦 B 1960 1969，等等。這個群組的電腦稱為*Hadoop 叢集*。
- 部分在處理完成之後，將結果的每個組件一起。 您現在有多少人生日年份相對較短清單，且可管理計算百分比，此整體的清單中的工作。

在 Azure 上[HDInsight](https://azure.microsoft.com/services/hdinsight/)可讓您處理、 分析及深入了解新的巨量資料使用 Hadoop 的強大功能。 例如，您可以使用它來分析 web 伺服器記錄：

- 啟用 web 伺服器記錄，以您的儲存體帳戶。 這會設定 Azure 將記錄檔寫入 Blob 服務，為您的應用程式的每個 HTTP 要求。 Blob 服務基本上是雲端檔案儲存體，並完美整合 HDInsight 與。 

    ![Blob 儲存體的記錄檔](data-storage-options/_static/image2.png)
- 當應用程式變流量時，網頁伺服器 IIS 記錄檔會寫入 Blob 儲存體。 

    ![Web 伺服器記錄](data-storage-options/_static/image3.png)
- 在入口網站中，按一下**的新** - **Data Services** - **HDInsight** - **快速建立**，指定 HDInsight 叢集名稱、 叢集大小 （HDInsight 叢集資料節點的數目），和使用者名稱和密碼適用於 HDInsight 叢集。 

    ![HDInsight](data-storage-options/_static/image4.png)

您現在可以設定 MapReduce 工作來分析您的記錄檔，並取得問題的解答，例如：

- 每天的哪些時段我的應用程式未取得大部分或最小流量？
- 哪些國家/地區是來自我流量？
- 什麼是我的流量來自之區域的平均網路上的芳鄰平均收入。 （沒有公用資料集，可讓您透過 IP 位址的網路上的芳鄰收入，而且您可以在 web 伺服器記錄檔中的 IP 位址對符合）。
- 網路上的芳鄰收入建立特定的網頁或網站中的產品如何關聯的？

您接著可以使用這些目標廣告根據客戶有興趣的可能性，或可能購買特定產品的問題解答。

中所述[自動執行的所有項目一章](automate-everything.md)、 可以自動化在入口網站中，您可以執行的大部分函式，並包含設定和執行中 HDInsight 分析作業。 典型的 HDInsight 指令碼可能包含下列步驟：

- 佈建 HDInsight 叢集，並將它連結至 Blob 儲存體輸入儲存體帳戶。
- 將 MapReduce 作業可執行檔 （.jar 或.exe 檔案） 上傳至 HDInsight 叢集。
- 提交 MapReduce 會儲存到 Blob 儲存體的輸出資料。
- 等候工作完成。
- 刪除 HDInsight 叢集。
- 存取 Blob 儲存體的輸出。

藉由執行所有指令碼，您最小化佈建 HDInsight 叢集時，您的成本降到最低的時間的量。

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>平台即服務 (PaaS) 與基礎結構即服務 (IaaS)

稍早所列的資料儲存體選項包含平台為-即服務 (PaaS) 和基礎結構做為-即服務 (IaaS) 解決方案。 PaaS 表示我們管理硬體和軟體基礎結構，而且您只使用服務。 SQL Database 是一項 Azure PaaS 功能。 您要求的資料庫，並在幕後 Azure 設定和設定的 Vm，並設定在其上的資料庫。 您不需要直接存取 Vm，並不需要管理它們。IaaS 表示您設定、 設定和管理我們資料中心基礎結構中，在執行的 Vm，而且您將任何您想要在其上。 我們提供預先設定的 VM 映像庫 VM 的常見設定。 例如，您可以安裝預先設定的 VM 映像的 Windows Server 2008、 Windows Server 2012 中，BizTalk Server、 Oracle WebLogic Server、 Oracle 資料庫等。

Azure 提供的 PaaS 資料解決方案包括：

- Azure SQL Database （之前稱為 SQL Azure）。 雲端關聯式資料庫，SQL Server 為基礎。
- Azure 資料表儲存體中。 索引鍵/值的 NoSQL 資料庫。
- Azure Blob 儲存體中。 在雲端中的檔案儲存體。

對於 IaaS，您可以執行任何項目可以載入到 VM，例如：

- SQL Server、 Oracle、 MySQL、 SQL Compact、 SQLite 或 Postgres 之類的關聯式資料庫。
- 索引鍵/值資料存放區，例如 Memcached、 Redis、 Cassandra 和 Riak。
- 資料行 （如 HBase) 的資料存放區。
- 例如，MongoDB，RavenDB、 CouchDB 的文件資料庫。
- 例如 Neo4j 的圖形資料庫。

![在 Azure 上的資料儲存體選項](data-storage-options/_static/image5.png)

IaaS 選項可讓您幾乎無限制的資料儲存體選項，並有許多都是特別簡單易用，因為您可以建立使用預先設定的映像的 Vm。 例如，在管理入口網站中移至**虛擬機器**，按一下**映像**索引標籤，然後按一下**瀏覽 VM Depot**。

![瀏覽 VM Depot](data-storage-options/_static/image6.png)

然後您會看到一份[數百種預先設定的 VM 映像](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)，而且您可以在從映像已預先安裝的資料庫管理系統，例如 MongoDB、 Neo4J、 Redis、 Cassandra 或 CouchDB 建立 VM:

![在 VM Depot 中的 MongoDB](data-storage-options/_static/image7.png)

Azure 讓 IaaS 資料儲存體選項一樣容易使用，但 PaaS 供應項目有許多優點，使其更符合成本效益並適合許多情況下：

- 您不需要建立 Vm，您只使用入口網站或指令碼來設定資料存放區。 如果您想為 200 tb 的資料存放區，您可以只按一下按鈕，或執行命令，並以秒為單位可供您使用。
- 您不需要管理或修補服務; 所使用的 VmMicrosoft 會為您自動。-您不需要擔心設定基礎結構的調整或高可用性;Microsoft 會為您的處理。
- 您不需要購買授權。授權費用已包含在服務費用。
- 您只需支付您所使用的項目。

在 Azure 中的 PaaS 資料儲存體選項包含由協力廠商提供者的供應項目。 例如，您可以選擇[MongoLab 附加元件](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/)從 Azure 市集佈建 MongoDB 資料庫即服務。

## <a name="choosing-a-data-storage-option"></a>選擇的資料儲存選項

任何一個的方法不是最適合所有案例。 如果任何人都說，這項技術就是答案，首先来詢問是 「 什麼是這個問題？ 」，因為不同的解決方案最適合用於不同的項目。 有關聯式模型中，明確的優點這就是為什麼它已經存在因此長時間。 但也有向側邊都無法使用 NoSQL 解決方案的 sql。

通常我們看到的內容最適合是複合的方法，其中在單一解決方案中使用 SQL 和 NoSQL。 即使當人會說，他們要採用 NoSQL，是否您向下切入到他們正在做什麼您通常尋找他們所使用數個不同的 NoSQL 架構： 他們使用[CouchDB](http://wiki.apache.org/couchdb/Introduction)，並[Redis](http://redis.io/)，和[Riak](http://basho.com/riak/)不同的項目。 甚至是 Facebook，廣泛使用 NoSQL，會使用不同的 NoSQL 架構不同部分的服務。 彈性地混合及比對資料儲存的方法是其中一個的雲端的優點，因為它可以輕鬆使用多個資料解決方案，並將它們整合在單一應用程式中的項目。

以下是要考慮當您選擇一種方法的一些問題：

| 語意資料 | -什麼是核心資料儲存體和資料存取語意 （您將儲存關聯式或非結構化資料）？ 非結構化的資料，例如媒體檔案適合在 blob 儲存體;集合的相關資料，例如產品、 清查、 供應商、 客戶訂單等等，最適合關聯式資料庫中。 |
| --- | --- |
| 查詢支援 | -有多麼它來查詢資料？ -哪些類型的問題可以有效率地要求嗎？ 索引鍵/值資料存放區是非常好，可使指定的索引鍵值的單一資料列，但不是複雜的查詢很理想。 使用者設定檔資料存放區會永遠都會取得一個特定的使用者資料，索引鍵/值資料存放區無法適用;您想要用來取得各種不同的產品屬性為基礎的不同群組的產品目錄的關聯式資料庫可能比較適合。 NoSQL 資料庫可以有效率地儲存大量的資料就可將資料庫周圍應用程式如何查詢資料，但這會讓臨機操作查詢執行的工作變得更難。 使用關聯式資料庫時，您可以建置幾乎任何類型的查詢。 |
| 功能的投影 | -可以的問題，等彙總，會執行的伺服器端嗎？ 如果我執行 SELECT COUNT (\*) 從 SQL 中的資料表，它會非常有效率的方式執行伺服器上的所有工作，並傳回的數字我正在尋找。 如果我想從 NoSQL 資料存放區不支援彙總相同的計算，這會是效率不佳的 「 未繫結的查詢 」，而且可能會逾時。即使查詢成功我必須在用戶端從伺服器擷取的所有資料和用戶端上的資料列計數。 -可以使用何種語言或運算式的型別？ 使用關聯式資料庫中，我可以使用 SQL。 使用某些種 NoSQL 資料庫，例如 Azure 資料表儲存體，我將使用[OData](http://www.odata.org/)，而且我可以篩選主索引鍵，並取得的預測 （選取可用的欄位子集）。 |
| 輕鬆延展性 | -與多少資料需要調整嗎？ -沒有平台原生實作向外延展？ -有多麼它來新增/移除 （大小和輸送量） 的容量？ 關聯式資料庫和資料表不自動分割，以使其可調整，因此它們會很難擴充超過特定限制。 Azure 資料表儲存體之類的 NoSQL 資料存放區原本就是分割所有項目，並新增分割區幾乎沒有限制。 您可以進行立即調整資料表儲存體最多 200 tb，但 Azure SQL Database 的最大資料庫大小為 500 gb。 您可以調整關聯式資料分割成多個資料庫，但設定的應用程式，以支援該模型包含許多程式設計工作。 |
| 檢測和管理能力 | -多麼檢測、 監視及管理的平台？ 您必須瞭解有關的健康情況和效能資料存放區，因此您必須事先知道哪些平台可讓您免費的計量，您必須開發您自己。 |
| 作業 | -簡單會部署到在 Azure 上執行的平台？ PaaS？ IaaS？ Linux 嗎？ 資料表儲存體和 SQL Database 可輕鬆地在 Azure 上設定。 不是內建的 Azure PaaS 解決方案的平台需要更多的工作。 |
| API 支援 | -這是提供 API，可讓您輕鬆地使用平台？ Azure 表格服務會支援.NET 4.5 的非同步程式設計模型的.NET API 與 SDK。 如果您要撰寫.NET 應用程式，它會更輕鬆地撰寫及測試 Azure 表格服務相較於另一個索引鍵/值資料行的資料存放區平台，沒有可用的 API 或較不完整的程式碼。 |
| 交易的完整性和資料一致性 | -它是關鍵的平台支援交易，才能保證資料一致性嗎？ 追蹤的傳送，效能和低的資料儲存成本可能比交易或中的資料平台，參考完整性的自動支援更重要的大量電子郵件進行 Azure 表格服務的理想選擇。 用於追蹤銀行帳戶之間取得平衡，或購買訂單關聯式資料庫平台，提供強式的交易式保證會是較好的選擇。 |
| 商務持續性 | -如何輕鬆會備份、 還原及災害復原？ 遲早產量資料將會損毀，您必須復原函式。 關聯式資料庫通常會有更多更細緻的還原功能，例如能夠還原至某個點的時間。 了解還原功能可用於每個平台，您正在考慮是要考慮的重要因素。 |
| 成本 | -如果多個平台可以支援您的資料工作負載，兩相較量成本？ 例如，如果您使用 ASP.NET 身分識別，您就可以在 Azure 表格服務或 Azure SQL Database 中儲存使用者設定檔資料。 如果您不需要的豐富查詢 SQL Database 的功能，您可能會選擇 Azure 資料表部分因為許多成本就少一段指定的儲存體。 |

通常建議是知道在每個分類問題的答案，再選擇您的資料儲存體解決方案。

此外，您的工作負載可能會有某些平台可支援比其他人更好的特定需求。 例如: 

- 您的應用程式需求不會稽核功能？
- 您資料的壽命需求是什麼，您需要自動化封存或清除的功能嗎？
- 您是否有特定的安全性需求？ 比方說，此資料包含 PII （個人識別資訊），但您必須確定會從查詢結果中排除 PII。
- 如果您有一些無法基於法規或技術在雲端中儲存的資料，您可能需要雲端資料儲存體平台，可協助整合您的內部部署儲存體。

## <a name="demo--using-sql-database-in-azure"></a>示範 – 在 Azure 中使用 SQL Database

修正其應用程式使用關聯式資料庫來儲存工作。 環境建立 Windows PowerShell 指令碼所示[自動執行的所有項目一章](automate-everything.md)會建立兩個 SQL Database 執行個體。 您可以看到這些入口網站中，依序按一下**SQL Database**  索引標籤。

![在入口網站中的 SQL 資料庫](data-storage-options/_static/image8.png)

也很容易就能使用入口網站中建立資料庫。

按一下 **新-資料服務** -- **SQL Database** -- **快速建立**、 輸入資料庫名稱、 選擇您已在您的帳戶中的伺服器或建立新的連線，然後按一下**建立 SQL Database**。

![新增 SQL 資料庫](data-storage-options/_static/image9.png)

等候數秒鐘的時間，以及您在準備好要使用的 Azure 資料庫。

![建立新的 SQL Database](data-storage-options/_static/image10.png)

因此 Azure 會在幾個秒什麼可能要花一天或每週或更長時間才能完成內部部署環境中。 和您輕鬆地自動建立資料庫，在指令碼或使用管理 API，因為您可以動態地相應放大藉由將您的資料分散到多個 < o: p>< > 資料庫，只要您的應用程式的設計。 < /o: p >

這是我們的平台為-即服務模型的範例。 您不必管理伺服器，我們執行它。 您不需要擔心備份，我們執行它。 在高可用性 – 執行資料庫中的資料會自動複寫到三部伺服器。 如果機器故障了，我們會自動容錯移轉，您會遺失任何資料。 伺服器會定期修補，您不需要擔心。

按一下按鈕時，取得確切的連接字串，您需要而且可以立即開始使用新的資料庫。

![連接字串](data-storage-options/_static/image11.png)

儀表板會顯示連線歷程記錄，並使用儲存體數量。

![SQL Database 儀表板](data-storage-options/_static/image12.png)

您可以管理在入口網站中的資料庫，或使用 SQL Server 工具您已經熟悉，包括 SQL Server Management Studio (SSMS) 和 SQL Server 物件總管 (SSOX)] 和 [伺服器總管的 Visual Studio 工具。

![SSOX](data-storage-options/_static/image13.png)

另一個好處是計價模式。 您可以開始開發使用免費的 20MB 資料庫，並實際執行資料庫啟動大約美金 $5 每個月。 您只需支付您實際儲存在資料庫中，最大容量的資料。 您不需要購買授權。

SQL Database 是簡單且縮放自如。 修正其應用程式中，我們在我們的自動化指令碼中建立的資料庫上限為 1 gb。 如果您想要調整最多 150 gb，您可以只到入口網站和變更這項設定，或執行 REST API 命令，並以秒為單位，您有 150 gb 資料庫，您可以部署到的資料。

![SQL Database 版本和大小](data-storage-options/_static/image14.png)

這是雲端基礎結構，就能快速且輕鬆地立即開始使用它的功能。

修正其應用程式使用兩個 SQL 資料庫，一個用於成員資格 （驗證和授權），一個用於資料，而這是您只需要佈建和調整其規模。 您稍早看到如何佈建透過 Windows PowerShell 指令碼，而您也已了解如何在入口網站中是多麼的資料庫。

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>與直接資料庫存取使用 ADO.NET entity Framework

修正其應用程式存取這些資料庫藉由使用 Entity Framework、.NET 應用程式的 Microsoft 推薦的 ORM （物件關聯式對應程式）。 ORM 是很棒的工具，可協助開發人員生產力，但是生產力代價是，在某些情況下的效能降低。 在真實世界的雲端應用程式中不會讓您選擇使用 EF，還是直接使用 ADO.NET--您同時使用的功能。 大多時候當您撰寫程式碼可使用資料庫時，取得最大效能並不重要，您可以利用簡化的編碼和測試您取得使用 Entity Framework。 在 EF 額外負荷將會導致無法接受的效能的情況下，可以撰寫，並執行您自己使用 ADO.NET，在理想情況下呼叫預存程序的查詢。

您用來存取資料庫時，任何方法，您想要 「 對話 」 盡量降到最低。 換句話說，如果您可以取得您需要的所有資料在一個較大的查詢結果集而不是數十或數百個較小的這通常偏好。 例如，如果您要在清單中的學生和它們已註冊的課程，它通常會比較好取得所有一個聯結查詢而不是在單一查詢中取得學生和執行個別查詢每個學生的課程中的資料。

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL database 和 Entity Framework，它修正應用程式中

修正其應用程式中`FixItContext`類別，衍生自 Entity Framework`DbContext`類別、 識別資料庫和資料庫中指定的資料表。 這個內容會指定實體集 （資料表） 的工作，以及程式碼會傳遞至內容的連接字串名稱。 該名稱是指在 Web.config 檔案中定義的連接字串。

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

中的連接字串*Web.config*檔案名為 appdb （這裡指向本機開發的資料庫）：

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework 建立*FixItTasks*資料表中包含的屬性為基礎`FixItTask`實體類別。 這是簡單的 POCO （純舊 CLR 物件） 類別，這表示它不會繼承自或有任何相依性 Entity Framework。 但 Entity Framework 知道如何建立以它為基礎的資料表，並執行 CRUD （建立-讀取-更新-刪除） 作業，使用它。

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks 資料表](data-storage-options/_static/image15.png)

修正其應用程式包含的儲存機制介面，它會使用資料存放區的 CRUD 作業。

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

請注意，存放庫方法都是所有的非同步處理，因此可以進行所有的資料存取，完全非同步的方式。

儲存機制實作呼叫 Entity Framework 的非同步方法來處理資料，包括 LINQ 查詢也與插入、 更新和刪除作業。 以下是查閱 Fix It 工作的程式碼的範例。

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

另外還有一些時間，以及錯誤記錄程式碼中，我們將探討的更新版本中，您會發現[監視和遙測章節](monitoring-and-telemetry.md)。

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>選擇與在 Azure 中的 VM (IaaS) 中的 SQL Server 的 SQL Database (PaaS)

SQL Server 和 Azure SQL Database 的好處是相同的核心程式設計模型，這兩者。 在這兩種環境中，您可以使用大部分的相同技術。 您甚至可以使用開發中的 SQL Server 資料庫和 SQL Database 執行個體，在雲端中，這是它修正應用程式的設定方式。

或者，您可以執行相同的 SQL Server 在雲端中執行內部部署 IaaS Vm 上安裝它。 某些舊版的應用程式，在 VM 中執行 SQL Server 可能會更好的解決方案。 SQL Server 資料庫在專用 VM 上執行，因為它會有更多可用資源進行共用的伺服器執行的 SQL Database 資料庫。 這表示 SQL Server 資料庫可以更大的空間，且仍然運作正常。 一般情況下，較小的資料庫大小和資料表大小，越好的使用案例適用於 SQL Database (PaaS)。

以下是有關如何選擇兩個模型之間的一些指導方針。

| Azure SQL Database (PaaS) | 虛擬機器 (IaaS) 中的 SQL Server |
| --- | --- |
| **專業人員**-您不必建立或管理 Vm、 更新或修補作業系統或 SQL;Azure 會為您。 -內建高可用性，具有資料庫層級的 SLA。 -因為您只需支付您使用 （不需要任何授權），低總擁有成本 (TCO)。 -適合用來處理大量的較小的資料庫 (&lt;= 500 GB)。 -輕鬆地以動態方式建立新的資料庫，若要啟用向外延展。 | ***專業人員***-與內部部署 SQL Server 的功能相容。 -可以實作 SQL Server[透過 AlwaysOn 高可用性](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx)2 + Vm，VM 層級的 sla 中。 -您有完整控制 SQL 受管理的方式。 -可以重複使用您已擁有，或支付一個小時的 SQL 授權。 -適合用來處理較少但更大 (1 TB +) 資料庫。 |
| **Cons** -有些功能相較於內部部署 SQL Server 的間距 (缺乏[CLR 整合](https://technet.microsoft.com/library/ms131102.aspx)， [TDE](https://technet.microsoft.com/library/bb934049.aspx)，[壓縮支援](https://technet.microsoft.com/library/cc280449.aspx)， [SQL ServerReporting Services](https://technet.microsoft.com/library/ms159106.aspx)等等)-500 gb 的資料庫大小限制。 | ***Cons*** -更新/修補程式 （作業系統和 SQL） 是您的職責-建立和管理資料庫使用的是您的職責-限制為大約為 8000 （透過 16 個資料磁碟機） 的磁碟 IOPS （每秒輸入/輸出作業）。 |

如果您想要使用 SQL Server VM 中，您可以使用您自己的 SQL Server 授權，或支付一個小時為單位。 比方說，在入口網站或透過 REST API，您可以建立新的 VM，使用 SQL Server 映像。

![使用 SQL Server 中建立 VM](data-storage-options/_static/image16.png)

![SQL Server VM 映像清單](data-storage-options/_static/image17.png)

當您建立 VM 與 SQL Server 映像，我們比例的 SQL Server 授權成本依時數，根據您的 VM 使用方式。 如果您有只會執行幾個月的專案時，它是依時數支付成本較低。 如果您認為您的專案將過去多年來，它是您平常的方式購買的授權成本。

## <a name="summary"></a>總結

雲端運算讓實際混合和比對資料儲存的方法符合您的應用程式的需求。 如果您正在建置新的應用程式，請仔細考慮有關此處所列，以便選擇方法，將繼續您的應用程式成長時正常運作的問題。 [下一步 一章](data-partitioning-strategies.md)將說明一些您可用來結合多個資料儲存體方法的資料分割策略。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源。

選擇的資料庫平台：

- [高度可調整的解決方案的資料存取： 使用 SQL、 NoSQL 和 Polyglot Persistence](http://aka.ms/dag-doc)。 電子書由 Microsoft Patterns and Practices 會深入了解不同種類的資料會儲存適用於雲端應用程式。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/ff898430.aspx)。 請參閱 < 資料一致性入門、 資料複寫和同步處理的指引，索引資料表模式，具體化檢視模式。
- [基底： Acid 替代](http://queue.acm.org/detail.cfm?id=1394128)。 發行項的相關資料的一致性和延展性之間的權衡取捨。
- [七週中的七個資料庫： 最新的資料庫和 NoSQL 運動指南](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921)。 本書由 Eric 雷德蒙和 Jim R Wilson。 強烈建議引進您自己的立即可用的資料儲存體平台範圍。

SQL Server 和 SQL Database 之間進行選擇：

- [SQL Database 指引的高階預覽](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx)。 簡介 SQL Database premium Edition，以及選擇透過 SQL Database Web edition 和 Business edition 的時機的指引。
- [指導方針和限制 (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)。 連結至 SQL Database 的限制相關文件的入口網站頁面上，包括一個著重於 SQL Server 功能的 SQL Database 不支援。
- [Azure 虛擬機器中的 SQL Server](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx)。 關於在 Azure 中執行 SQL Server 文件連結的入口網站頁面。
- [Scott Guthrie 說明在 Azure SQL Database](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/)。 SQL Database，Scott guthrie 介紹 6 分鐘影片。
- [應用程式模式和 Azure 虛擬機器中的 SQL Server 的開發策略](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx)。

使用 Entity Framework 和 SQL Database 中的 ASP.NET Web 應用程式

- [開始使用 MVC 5 的 EF 6](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 九段教學課程系列，逐步引導您建置使用 EF，並將資料庫部署至 Azure 和 SQL Database 的 MVC 應用程式。
- [使用 Visual Studio 的 ASP.NET Web 部署](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 12 個部分教學課程系列，會詳細討論如何使用 EF Code First 部署資料庫。
- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 應用程式部署至 Azure 網站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)。 逐步引導您建立 web 應用程式的教學課程會使用驗證、 成員資格資料庫中儲存應用程式資料表、 修改資料庫結構描述，和將應用程式部署至 Azure。
- [ASP.NET 資料存取內容對應](https://go.microsoft.com/fwlink/p/?LinkId=282414)。 使用 EF 和 SQL Database 資源的連結。

在 Azure 上使用 MongoDB:

- [MongoLab-在 Azure 上的 MongoDB](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/)。 如需在 Azure 上執行 MongoDB 文件的入口網站頁面。
- [建立 Azure 網站連接到 Azure 中的虛擬機器上執行的 MongoDB](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/)。 示範如何使用 ASP.NET web 應用程式中的 MongoDB 資料庫的逐步教學課程。

HDInsight (Azure 上的 Hadoop):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)。 HDInsight 上的文件入口網站[Azure](https://azure.microsoft.com/)網站。
- [Hadoop 與 HDInsight: Azure 中的巨量資料](https://msdn.microsoft.com/magazine/dn385705.aspx)。 Bruno Terkaly 和 Ricardo Villalobos，簡介在 Azure 上的 Hadoop 的 MSDN Magazine 文章。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱 MapReduce 模式。

> [!div class="step-by-step"]
> [上一頁](single-sign-on.md)
> [下一頁](data-partitioning-strategies.md)
