---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: "資料儲存體選項 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 3eb070167c36db7d8fb2e05af89716ee386b8211
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>資料儲存體選項 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


大多數人用於關聯式資料庫，而他們傾向於它們在設計雲端應用程式時，忽略其他資料儲存選項。 此結果會次佳的效能、 高的費用，還是變差，因為[NoSQL](http://en.wikipedia.org/wiki/NoSQL) （非關聯式） 資料庫可與關聯式資料庫更有效率地處理某些工作。 當客戶詢問我們來協助解決的重要資料的儲存體問題時，通常是因為它們具有關聯式資料庫其中 NoSQL 選項的其中一個會處理更好。 在這些情況下客戶已經比較好的方式如果它們必須實作應用程式部署到生產環境之前的 NoSQL 解決方案。

相反地，它也會假設的 NoSQL 資料庫可以執行的所有項目或夠好的錯誤。 沒有單一最佳資料管理選項的所有資料儲存區的工作。不同的資料管理解決方案適合用於不同的工作。 大部分的真實世界雲端應用程式有各種不同的資料儲存需求，並結合多個資料儲存體解決方案通常提供最佳。

本章節的目的是讓您可用的資料儲存體選項的更廣泛的意義上是雲端應用程式和如何挑選適合您案例的一些基本指引。 所以最好了解這些選項可供您和您開發的應用程式之前，思考其優點和缺點。 變更資料儲存體選項，在實際執行應用程式可能很難，像是讓飛機航班時變更 jet 引擎。

## <a name="data-storage-options-on-azure"></a>在 Azure 上的資料儲存體選項

雲端可讓您很容易就能使用各種不同的關聯式和 NoSQL 資料存放區。 以下是一些您可以使用 Azure 中的資料儲存體平台。

![](data-storage-options/_static/image1.png)

下表顯示四種類型的 NoSQL 資料庫：

- [索引鍵/值資料庫](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec7)儲存單一序列化的物件，針對每個索引鍵的值。 適合用來儲存大量資料，您想要針對給定索引鍵的值取得一個項目，而且不需要它們來查詢是根據項目的其他屬性。

    [Azure Blob 儲存體](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/)是函式，例如檔案儲存在雲端中，以對應至資料夾和檔案名稱的索引鍵值的索引鍵/值資料庫。 擷取檔案由資料夾和檔案名稱，而不是依搜尋的檔案內容中的值。

    [Azure 資料表儲存體](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/)也是索引鍵/值的資料庫。 每個值都稱為*實體*（類似於資料列，以資料分割索引鍵和資料列索引鍵所識別） 與包含多個*屬性*(類似於資料行，但並非所有資料表中的實體都必須共用相同資料行）。 查詢以外的索引鍵資料行上非常沒有效率，且應予以避免。 例如，您可以儲存使用者設定檔資料，以一個資料分割的儲存單一使用者的相關資訊。 在個別的一個實體的屬性或相同的資料分割中的個別實體中，您可以儲存資料，例如使用者名稱、 密碼雜湊、 出生日期等等。 但是您不想要查詢給定的出生日期範圍的所有使用者，而且聯結查詢無法執行設定檔資料表與另一個資料表之間。 資料表儲存體是更具擴充性和關聯式資料庫中，成本較低，但它不會啟用複雜的查詢或聯結。
- [Documentdatabases](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec8)索引鍵/值的資料庫中的值是*文件*。 這裡的 「 文件 」 不會使用 Word 或 Excel 的文件的意義，但表示具名的欄位和值，任何一項都可能是子文件的集合。 例如，訂單歷程記錄資料表中的訂單文件可能訂單號碼、 訂單日期，以及客戶的欄位。與客戶欄位有名稱和地址欄位。 資料庫會將編碼的格式，例如 XML、 YAML、 JSON 或 BSON; 中的欄位資料或者，它可以使用純文字。 設定文件資料庫除了資料庫索引鍵/值的其中一項功能是查詢的非索引鍵欄位，並定義要讓查詢更有效率的次要索引的能力。 這項功能可讓您更適合需要擷取資料比文件索引鍵的值更複雜的準則為基礎的應用程式文件資料庫。 例如，銷售訂單歷程記錄文件資料庫中可以查詢的各種欄位，例如產品識別碼、 客戶編號、 客戶名稱等。 [MongoDB](http://www.mongodb.org/)是常用的文件資料庫。
- [資料欄系列資料庫](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec9)是索引鍵/值資料存放區可讓您結構資料儲存成相關的資料行稱為資料行家族的集合。 比方說，人口普查資料庫可能有一個群組的資料行的某個人的名稱 （名字、 中間名上, 一次），一個群組，該人員的位址和一個群組，該人員的設定檔資訊 (DOB、 性別等等。)。 然後資料庫就可以每個資料欄系列儲存在不同的磁碟分割中同時保留所有人與相同的索引鍵相關的資料。 您接著可以讀取所有設定檔資訊而不需要閱讀的所有名稱和位址資訊以及。 [Cassandra](http://cassandra.apache.org/)是常用的資料欄系列資料庫。
- [圖形資料庫](https://msdn.microsoft.com/en-us/library/dn313285.aspx#sec10)儲存資訊的物件和關聯性集合。 圖表資料庫的目的是讓應用程式有效率地執行查詢，兩者之間周遊的網路物件和關聯性。 比方說，物件可能是員工在人力資源資料庫中，而且您可能會想以便查詢這類 「 尋找所有員工直接或間接 scott。 」 [Neo4j](http://www.neo4j.org/)是常用的 graph 資料庫。

相較於關聯式資料庫、 NoSQL 選項會提供更大的延展性和成本效益的儲存體和分析的非結構化資料。 缺點是它們不提供的豐富 queryability 和關聯式資料庫穩固的資料完整性功能。 NoSQL 會適用於 IIS 記錄檔資料，這牽涉到大量使用聯結查詢不需要。 NoSQL 不適合非常銀行的交易，需要絕對資料完整性以及牽涉到許多其他帳戶相關的資料關聯性。

也是較新的資料庫平台呼叫類別[NewSQL](http://en.wikipedia.org/wiki/NewSQL) ，結合的 NoSQL 資料庫延展性 queryability 與關聯式資料庫的交易完整性。 NewSQL 資料庫專為分散式儲存體和查詢處理，因此通常很難在"OldSQL 」 資料庫中實作後者。 [NuoDB](http://www.nuodb.com/) NewSQL 資料庫可以在 Azure 上使用的範例。

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop 和 MapReduce

您可以在 NoSQL 資料庫中儲存的資料量大可能難以及時有效率地分析。 若要執行，您可以使用類似的架構[Hadoop](http://hadoop.apache.org/)實作[MapReduce](http://en.wikipedia.org/wiki/MapReduce)功能。 基本上哪些 MapReduce 程序會如下所示：

- 需要選取資料處理的資料大小只儲存的資料實際上，您需要分析的限制。 例如，您會想要知道您的使用者的出生年份，基底的結構，因此您選取只出生年份，從您的使用者設定檔資料存放區。
- 細分成部分資料，並將它們傳送給不同的電腦進行處理。 電腦 A 計算 1950年 1959年日期的人數，電腦 B 沒有 1960年 1969，等等。此群組的電腦稱為*Hadoop 叢集*。
- 將每個部分結果放回組合在一起的組件在處理完成之後。 您現在有相對較短的出生年份多少人員清單，且可管理此整體清單中的百分比的計算的工作。

在 Azure 上、 [HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/)可讓您處理、 分析和深入探索新巨量資料使用 Hadoop 的電源。 例如，您可以使用它來分析 web 伺服器記錄檔：

- 啟用 web 伺服器記錄，到儲存體帳戶。 這會設定 Azure Blob 服務，以您的應用程式的每個 HTTP 要求寫入記錄檔。 Blob 服務基本上是雲端檔案存放裝置，並運用整合與 HDInsight。 

    ![Blob 儲存體的記錄檔](data-storage-options/_static/image2.png)
- 當應用程式變流量時，網頁伺服器 IIS 記錄檔會寫入至 Blob 儲存體。 

    ![Web 伺服器記錄檔](data-storage-options/_static/image3.png)
- 在入口網站中，按一下**新增** - **Data Services** - **HDInsight** - **快速建立**，並指定 HDInsight 叢集名稱、 叢集大小 （HDInsight 叢集的資料節點的數目），和使用者名稱和密碼的 HDInsight 叢集。 

    ![HDInsight](data-storage-options/_static/image4.png)

您現在可以設定 MapReduce 工作來分析您的記錄檔，並取得問題的解答，例如：

- 每天何時我的應用程式未取得的大部分或最少的流量？
- 哪些國家 （地區） 是來自我的流量？
- 什麼是我的流量所來自的區域的平均上的芳鄰 平均收入。 （沒有公用的資料集，可讓您依 IP 位址的鄰居收入和，符合 web 伺服器記錄檔中的 IP 位址）。
- 上的芳鄰 收入方式關聯特定網頁或網站中的產品？

您接著可以使用像根據客戶有興趣的可能性或可能購買特定產品的目標廣告這類問題的答案。

中所述[一切自動化章](automate-everything.md)，您可以在入口網站的大部分函式則可自動化，以及包含設定及執行 HDInsight 分析工作。 典型的 HDInsight 指令碼可能包含下列步驟：

- 佈建的 HDInsight 叢集，並將它連結至儲存體帳戶針對 Blob 儲存體的輸入。
- 上載到 HDInsight 叢集的 MapReduce 工作可執行檔 （d 或.exe 檔案）。
- 提交 MapReduce 儲存到 Blob 儲存體的輸出資料。
- 等候工作完成。
- 刪除 HDInsight 叢集。
- 存取 Blob 儲存體的輸出。

藉由執行所有指令碼，即可最小的時間佈建 HDInsight 叢集，您的成本降至最低。

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>平台即服務 (PaaS) 與基礎結構即服務 (IaaS)

稍早所列的資料儲存體選項包含平台做為服務 (PaaS) 和基礎結構做為服務 (IaaS) 方案。 PaaS 表示我們管理硬體和軟體基礎結構，而且您只使用服務。 SQL Database 是 Azure PaaS 功能。 您要求的資料庫，而在幕後 Azure 設定和設定 Vm 並設定在其上的資料庫。 您不需要直接存取 Vm，而且不需要管理它們。IaaS 表示設定、 設定及管理我們的資料中心基礎結構，在執行的 Vm，您將任何您想在其上。 我們提供常見的 VM 組態的預先設定的 VM 映像庫。 例如，您可以安裝預先設定的 VM 映像的 Windows Server 2008、 Windows Server 2012、 BizTalk Server、 Oracle WebLogic Server、 Oracle 資料庫等。

Azure 提供的 PaaS 資料解決方案包括：

- Azure SQL Database （先前稱為 SQL Azure）。 雲端關聯式資料庫，SQL Server 為基礎。
- Azure 資料表儲存體。 索引鍵/值 NoSQL 資料庫。
- Azure Blob 儲存體。 在雲端中的檔案存放裝置。

Iaas，您可以執行任何項目可以載入到 VM，例如：

- 例如，SQL Server、 Oracle、 MySQL、 SQL Compact、 SQLite 或 Postgres 的關聯式資料庫。
- 索引鍵/值資料存放區，例如 Memcached、 Redis、 Cassandra 和 Riak。
- 資料行的資料存放區，例如 HBase。
- 例如 MongoDB、 RavenDB 和 CouchDB 文件資料庫。
- 例如 Neo4j 圖形資料庫。

![在 Azure 上的資料儲存體選項](data-storage-options/_static/image5.png)

IaaS 選項可讓您幾乎不受限制的資料儲存體選項，和其中大部分都特別容易使用，因為您可以建立使用預先設定的映像的 Vm。 例如，在管理入口網站中移至**虛擬機器**，按一下 **映像**索引標籤，然後按一下**瀏覽 VM Depot**。

![瀏覽 VM Depot](data-storage-options/_static/image6.png)

您接著會看到一份[數百個預先設定的 VM 映像](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)，而且您可以從已預先安裝的資料庫管理系統映像，例如 MongoDB、 Neo4J、 Redis、 Cassandra 或 CouchDB 來建立 VM:

![VM depot MongoDB](data-storage-options/_static/image7.png)

Azure IaaS 資料儲存體選項，更容易使用，但是 PaaS 供應項目有許多優點，使其更符合成本效益，並用於許多案例實用：

- 您不需要建立 Vm，您只使用入口網站或指令碼來設定資料存放區。 如果您想 200 tb 的資料存放區，您可以只按一下按鈕，或執行命令，並以秒為單位可供您使用。
- 您不需要管理或修補程式的服務; 所使用的 VmMicrosoft 會為您自動。-您不必擔心設定縮放比例] 或 [高可用性的基礎結構Microsoft 為您的處理。
- 您不需要購買授權。授權費用會包含在服務費用。
- 您只需支付您所使用。

在 Azure 中的 PaaS 資料儲存體選項包含由協力廠商提供者的供應項目。 例如，您可以選擇[MongoLab 附加元件](https://azure.microsoft.com/en-us/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/)MongoDB 資料庫做為服務佈建 Azure 市集中。

## <a name="choosing-a-data-storage-option"></a>選擇資料的儲存體選項

沒有方法就是最適合所有案例。 任何人說明這項技術是回應，第一件事，詢問是什麼問題？ 」，因為不同的方案適用於不同的項目。 有明確優點關聯式模型中;這就是為什麼它已久，因此長時間。 但也可以定址 NoSQL 解決方案的 sql 向側邊。

通常我們看到最佳的工作是撰寫方法，其中單一解決方案中使用 SQL 和 NoSQL。 即使當有人說它們採用 NoSQL，是否您向下切入到工作您通常尋找他們所使用數個不同的 NoSQL 架構： 他們所使用[CouchDB](http://wiki.apache.org/couchdb/Introduction)，和[Redis](http://redis.io/)，和[Riak](http://basho.com/riak/)針對不同的項目。 即使 Facebook、 會廣泛地使用 NoSQL，會使用不同的 NoSQL 架構服務的不同部分。 彈性混搭資料儲存的方法是其中一項操作的不錯關於雲端，因為很容易使用多個資料方案，並將它們整合在單一應用程式。

以下是考量當您在選擇適當方法的一些問題：

| 語意資料 | -什麼是的核心資料儲存和資料存取語意 （您儲存關聯式或非結構化資料）？ 非結構化的資料，例如媒體檔案最適合在 blob 儲存體。集合的相關資料，例如產品、 存貨、 供應商、 客戶訂單等等，最適合在關聯式資料庫中。 |
| --- | --- |
| 查詢支援 | -如何輕鬆是可查詢的資料？ -什麼類型的問題可以有效率地要求嗎？ 索引鍵/值資料存放區是在取得指定索引鍵的值的單一資料列很好，但不是複雜的查詢很理想。 使用者設定檔資料儲存區會永遠都會取得一個特定使用者的資料，索引鍵/值資料存放區無法正常運作。要取得各種不同的產品屬性為基礎的不同群組的產品目錄的關聯式資料庫可能效果更好。 NoSQL 資料庫可以有效率地儲存大型資料磁碟區，但您可解決應用程式如何查詢資料，將資料庫和這樣臨機操作查詢更不容易執行。 使用關聯式資料庫中，您可以建立幾乎任何類型的查詢。 |
| 功能投影 | -可以的問題，彙總，以此類推，是執行的伺服器端嗎？ 如果我執行選取的計數 (\*) 從 SQL 中的資料表，它會非常有效率的方式執行伺服器上的所有工作，並且傳回的數字我正在尋找。 如果我想從 NoSQL 資料存放區不支援彙總的相同計算，這會是沒有效率 「 未繫結的查詢 」，而且可能會逾時。即使查詢成功我必須在用戶端從伺服器擷取所有的資料計數的用戶端上的資料列。 -可以使用何種語言或運算式的型別？ 我可以使用 SQL 關聯式資料庫。 使用某些 NoSQL 資料庫例如 Azure 資料表儲存體時，我將使用[OData](http://www.odata.org/)，和所有其他方法是主索引鍵上篩選，並取得的投影 （選取可用的欄位的子集）。 |
| 輕鬆延展性 | -與多少資料需要調整？ -沒有平台原生實作向外延展？ 的簡單方式是加入/移除容量 （大小和輸送量）？ 關聯式資料庫和資料表不是自動進行資料分割，使其可擴充，因此它們不容易擴充超過特定的限制。 NoSQL 資料存放區，例如 Azure 資料表儲存體原本就是資料分割的所有項目，而且沒有加入分割區幾乎沒有限制。 您隨時可以調整資料表儲存體最多 200 tb，但 Azure SQL Database 的最大資料庫大小為 500 gb。 您可以調整關聯式資料分割成多個資料庫，但設定的應用程式，以支援該模型包含許多程式設計工作。 |
| 檢測和管理能力 | -如何輕鬆是要檢測、 監視和管理的平台？ 您必須瞭解有關的健全狀況和效能資料存放區，因此您必須事先知道哪些平台可讓您免費的度量，您必須開發您自己。 |
| 作業 | -如何輕鬆是要部署和 Azure 上執行的平台？ PaaS？ IaaS 嗎？ Linux？ 資料表儲存體和 SQL Database 可輕鬆地在 Azure 上設定項目。 平台不是內建的 Azure PaaS 解決方案需要更多工作。 |
| 應用程式開發介面支援 | -這是提供 API，可讓您輕鬆地使用平台？ Azure 資料表服務沒有使用.NET API 支援.NET 4.5 非同步程式設計模型，透過 SDK。 如果您撰寫.NET 應用程式中，它會是能更輕鬆地撰寫並測試的程式碼相較於另一個索引鍵/值資料行資料存放區平台，已沒有可用的 API 或比較不完整的 Azure 表格服務。 |
| 完整性和資料的交易一致性 | -它是關鍵的平台支援交易，才能保證資料一致性嗎？ 追蹤的傳送，效能和低資料儲存體成本可能比自動支援交易或參考完整性的資料平台，在更重要的大量電子郵件進行 Azure 資料表服務很不錯的選擇。 用於追蹤的銀行帳戶餘額或採購訂單的關聯式資料庫平台，提供強式的交易式保證會是較好的選擇。 |
| 業務續航力 | -如何輕鬆會備份、 還原及災害復原？ 很快實際執行資料將會損毀的原因，您必須復原函式。 關聯式資料庫通常會有更多更細緻的還原功能，例如能夠還原至某個點的時間。 了解還原功能可用於您正在考慮每個平台是需要考慮的重要因素。 |
| 成本 | -如果多個平台可以支援您的資料工作負載，如何比較起來的成本？ 例如，如果您使用 ASP.NET Identity，您可以在 Azure 資料表服務或 Azure SQL Database 中儲存使用者設定檔資料。 如果您不需要查詢 SQL Database 的功能豐富，您可能會選擇 Azure 資料表部分因為成本很少的一段指定的儲存體。 |

通常建議是知道每個類別中問題的答案，再選擇您的資料儲存體解決方案。

此外，您的工作負載可能會有某些平台可支援比其他更好的特定需求。 例如: 

- 您的應用程式需求不會稽核功能？
- 什麼是資料壽命需求-您需要在自動化的封存或清除功能？
- 您是否有特殊的安全性需求？ 例如，資料包含 PII （個人識別資訊），但您必須確定您從查詢結果中排除 PII。
- 如果您有一些資料，無法儲存在雲端中為法規或技術的原因，您可能需要雲端資料儲存體平台，可協助整合與您在內部部署儲存體。

## <a name="demo--using-sql-database-in-azure"></a>示範 – 在 Azure 中使用 SQL Database

修正它應用程式使用關聯式資料庫來儲存工作。 顯示的環境建立 Windows PowerShell 指令碼[一切自動化章](automate-everything.md)建立兩個 SQL Database 執行個體。 您可以按一下來查看這些入口網站中**SQL 資料庫** 索引標籤。

![在入口網站中的 SQL 資料庫](data-storage-options/_static/image8.png)

所以也可以輕鬆地使用入口網站中建立資料庫。

按一下**新-Data Services** -- **SQL Database** -- **快速建立**、 輸入資料庫名稱、 選擇您已經在您的帳戶中的伺服器或建立新的連線，然後按一下**建立 SQL Database**。

![新增 SQL 資料庫](data-storage-options/_static/image9.png)

等候數秒，而且您準備好要使用 Azure 中有一個資料庫。

![建立新的 SQL 資料庫](data-storage-options/_static/image10.png)

讓 Azure 未在幾秒什麼可能要花一天或一週或更長時間才能完成在內部部署環境中。 以及您輕鬆地自動建立資料庫，在指令碼或使用管理 API，因為您可以動態地向外延展所分配到多個 < o: p>< > 資料庫，您的資料，只要該設計您的應用程式。 < /o: p >

這是我們的平台做為服務模型中的範例。 您不需要管理的伺服器，我們會執行它。 您不必擔心備份，我們會執行它。 在高可用性-執行自動複寫到三部伺服器資料庫中的資料。 如果電腦無作用時，我們會自動容錯移轉，您會遺失任何資料。 伺服器會定期修補，您不需要擔心。

按一下按鈕時，取得確切的連接字串需要而且可以立即開始使用新的資料庫。

![連接字串](data-storage-options/_static/image11.png)

儀表板會顯示連線歷程記錄，並使用儲存體數量。

![SQL Database 儀表板](data-storage-options/_static/image12.png)

您可以管理在入口網站的資料庫，或使用 SQL Server 工具您已熟悉，包括 SQL Server Management Studio (SSMS) 和 SQL Server 物件總管 (SSOX) 和伺服器總管的 Visual Studio 工具。

![SSOX](data-storage-options/_static/image13.png)

另一個好處是計價模型。 您可以開始開發使用免費的 20MB 資料庫，並開始大約 $5 每月的實際執行的資料庫。 您付費只是您實際儲存在資料庫中，最大容量的資料量。 您沒有產品購買的授權。

SQL Database 很容易小數位數。 修正它應用程式中，我們會在自動化指令碼中建立的資料庫的上限為 1 gb。 如果您想要調整最多 150 gb，您可以只進入入口網站和變更該設定，或執行 REST API 的命令，並以秒為單位，您有 150 gb 資料庫，您可以部署到的資料。

![SQL 資料庫版本和大小](data-storage-options/_static/image14.png)

這是快速且輕鬆地建立基礎結構，並立即使用此雲端的強大功能。

修正它應用程式使用兩個 SQL 資料庫，一個用於成員資格 （驗證和授權），另一個用於資料，也是您只需要佈建及調整它。 之前看到如何佈建資料庫，透過 Windows PowerShell 指令碼，而且現在也出現在入口網站中是多麼的輕鬆。

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>與直接存取資料庫使用 ADO.NET entity Framework

修正它應用程式使用 Entity Framework 來存取這些資料庫，Microsoft 的.NET 應用程式的建議 ORM （物件關聯式對應工具）。 ORM 很棒的工具，可協助開發人員生產力，但在某些情況下的效能降低的代價產能。 在真實世界雲端應用程式將不會進行選擇使用 EF，或直接使用 ADO.NET--您同時使用的功能。 大部分的情況下當您撰寫程式碼可使用資料庫時，取得最大效能並不重要，而且您可以利用簡化的編碼和測試，您會取得與 Entity Framework。 在 EF 負擔將會導致無法接受的效能的情況下，您可以撰寫，並執行您自己使用 ADO.NET，在理想情況下藉由呼叫預存程序的查詢。

您想要將 「 多話 」 盡量降到最低來存取資料庫，您使用的任何方法。 換句話說，如果在一個較大的查詢結果集而不是數十或數百個較小的可能會發生您需要的所有資料，這通常偏好。 例如，如果您需要清單學生版和中註冊這些課程，它通常會比較好來取得所有一個聯結查詢而取得學生在一個查詢和執行每一位學生課程針對個別查詢中的資料。

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL 資料庫和 Entity Framework 中修正該應用程式

修正它應用程式中`FixItContext`類別，衍生自 Entity Framework`DbContext`類別、 識別資料庫和資料庫中指定的資料表。 內容會指定實體集 （資料表） 的工作，而程式碼會傳入至內容的連接字串名稱。 該名稱會參考 Web.config 檔案中定義的連接字串。

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

中的連接字串*Web.config*檔案名為 appdb （這裡指向本機開發資料庫）：

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework 建立*FixItTasks*資料表中包含的屬性在基礎`FixItTask`實體類別。 這是簡單的 POCO （純舊 CLR 物件） 類別，這表示它不會繼承自或任何依存的 Entity Framework。 但 Entity Framework 知道如何建立它為基礎的資料表，並執行與它的 CRUD （建立讀取-更新-刪除） 作業。

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks 資料表](data-storage-options/_static/image15.png)

修正它應用程式包含的儲存機制介面，它會使用資料存放區的 CRUD 作業。

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

請注意，存放庫方法都是所有非同步處理，讓所有資料存取可以完全非同步的方式來都完成。

儲存機制實作會呼叫 Entity Framework 非同步方法使用的資料，包括 LINQ 查詢也與插入、 更新和刪除作業。 以下是查閱修正它工作的程式碼範例。

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

另外還有一些時間控制，以及錯誤記錄程式碼，我們會探討，稍後您會注意到[監視和遙測章節](monitoring-and-telemetry.md)。

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>選擇 SQL Database (PaaS) 與 Azure 中的 VM (IaaS) 中的 SQL Server

好 SQL Server 和 Azure SQL Database 是核心程式設計模型，兩者都是完全相同。 您可以使用大部分相同技術的兩個環境中。 您甚至可以使用程式開發中的 SQL Server 資料庫和 SQL Database 執行個體，在雲端中，這是設定它修正應用程式的方式。

或者，您可以執行相同的 SQL Server 雲端中執行內部部署安裝 IaaS Vm 上。 某些舊版的應用程式，在 VM 中執行 SQL Server 可能會更好的解決方案。 SQL Server 資料庫在專用的 VM 上執行，因為它有比更多資源可供其使用的共用伺服器執行的 SQL Database 資料庫。 表示 SQL Server 資料庫可以是較大，而且仍然運作正常。 一般情況下，較小的資料庫大小和資料表大小，愈使用案例適用於 SQL Database (PaaS)。

以下是如何選擇兩個模型之間的一些指導方針。

| Azure SQL Database (PaaS) | 虛擬機器 (IaaS) 中的 SQL Server |
| --- | --- |
| **專業人員**-您不必建立或管理 Vm、 更新或修補作業系統或 SQL。Azure 會為您。 -內建高可用性，與資料庫層級 SLA。 -因為您只支付您使用 （不需要權限），低總擁有成本 (TCO)。 -適合處理大量的較小的資料庫 (&lt;= 500 GB)。 -您輕鬆地以動態方式建立新的資料庫，以便向外延展。 | ***專業人員***-在內部部署 SQL Server 功能相容。 -可實作 SQL Server[透過 AlwaysOn 高可用性](https://www.microsoft.com/en-us/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx)2 + Vm，VM 層級 SLA 中。 -您對具有完整控制權 SQL 管理的方式。 -可以重複使用您已擁有，或其中一個每小時付費 SQL 授權。 -適用於處理較少但較大 (1 TB +) 資料庫。 |
| **Cons** -某些功能相較於內部部署 SQL Server 的間距 (缺乏[CLR 整合](https://technet.microsoft.com/en-us/library/ms131102.aspx)， [TDE](https://technet.microsoft.com/en-us/library/bb934049.aspx)，[壓縮支援](https://technet.microsoft.com/en-us/library/cc280449.aspx)， [SQLServer Reporting Services](https://technet.microsoft.com/en-us/library/ms159106.aspx)等等)-500 GB 的資料庫大小限制。 | ***Cons*** -更新/修補程式 （作業系統和 SQL） 是您的責任-建立並管理資料庫使用的是，您必須負責-限於約 8000 （透過 16 個資料磁碟機） 的磁碟 IOPS （每秒輸入/輸出作業）。 |

如果您想要使用 SQL Server 在 VM 中，您可以使用您自己的 SQL Server 授權或支付每小時的其中一個。 例如，在入口網站或透過 REST API 可以建立新的 VM 使用 SQL Server 映像。

![建立具有 SQL Server VM](data-storage-options/_static/image16.png)

![SQL Server VM 映像的清單](data-storage-options/_static/image17.png)

當您建立的 VM 與 SQL Server 映像，我們親速率的 SQL Server 授權成本每小時根據 VM 的使用方式。 如果您有只會在幾個月執行專案，則支付每小時成本較低。 如果您認為您的專案即將最後一年，則成本較低的方式，您通常要購買授權。

## <a name="summary"></a>總結

雲端運算讓實際混合及比對資料儲存的方法符合您的應用程式的需求。 如果您正在建置新的應用程式，請仔細考慮有關此處所列，以便選擇方法，將繼續您的應用程式成長時正常運作的問題。 [下一章](data-partitioning-strategies.md)將說明某些資料分割策略可讓您結合多個資料儲存的方法。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源。

選擇的資料庫平台：

- [資料存取可高度擴充的方案： 使用 SQL、 NoSQL 和 Polyglot 持續性](http://aka.ms/dag-doc)。 電子書由 Microsoft Patterns and Practices，深度進入不同種類的資料會儲存適用於雲端應用程式。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/en-us/library/ff898430.aspx)。 請參閱資料一致性入門、 資料複寫和同步處理的指引，索引資料表的模式、 具體化的檢視模式。
- [基底： Acid 替代](http://queue.acm.org/detail.cfm?id=1394128)。 發行項的相關資料的一致性和延展性之間的權衡取捨。
- [七個週七個資料庫： 新式資料庫和 NoSQL 移動的指引](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921)。 活頁簿 Eric Redmond，Jim R Wilson。 強烈建議簡介自己範圍的目前可用的資料儲存平台。

SQL Server 和 SQL Database 之間選擇：

- [SQL Database 的指引的高階預覽](https://msdn.microsoft.com/en-us/library/windowsazure/dn369873.aspx)。 高階 SQL Database，以及何時選擇透過 SQL Database Web 和 Business edition 指引簡介。
- [方針和限制 (Azure SQL Database)](https://msdn.microsoft.com/en-us/library/windowsazure/ff394102.aspx)。 連結到 SQL Database 的限制相關文件的入口網站頁面上，包括其中一個，著重於 SQL Server 功能的 SQL Database 不支援。
- [Azure 虛擬機器中的 SQL Server](https://msdn.microsoft.com/en-us/library/windowsazure/jj823132.aspx)。 連結文件中有關在 Azure 中執行 SQL Server 的入口網站頁面。
- [Scott Guthrie 說明在 Azure 中的 SQL 資料庫](https://azure.microsoft.com/en-us/documentation/videos/sql-in-azure-scottgu/)。 6 分鐘的介紹影片由 Scott Guthrie 的 SQL 資料庫。
- [應用程式模式和 Azure 虛擬機器中的 SQL Server 的開發策略](https://msdn.microsoft.com/en-us/library/windowsazure/dn574746.aspx)。

ASP.NET Web 應用程式中使用 Entity Framework 和 SQL Database

- [開始使用 EF 6 使用 MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 九部分的教學課程會引導您進行建置使用 EF，並將資料庫部署到 Azure 和 SQL Database 的 MVC 應用程式。
- [使用 Visual Studio 的 ASP.NET Web 部署](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 12 個部分的教學課程，會產生更多有關如何將資料庫部署使用 EF Code First 深度。
- [將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 應用程式部署至 Azure 網站](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)。 逐步引導您建立 web 應用程式的教學課程會使用驗證、 成員資格資料庫中儲存應用程式資料表、 修改資料庫結構描述，和將應用程式部署到 Azure。
- [ASP.NET 資料存取內容對應](https://go.microsoft.com/fwlink/p/?LinkId=282414)。 使用 EF 和 SQL Database 資源的連結。

在 Azure 上使用 MongoDB:

- [MongoLab-在 Azure 上 MongoDB](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/)。 如需在 Azure 上執行 MongoDB 文件入口網站頁面。
- [建立 Azure 網站連接到 Azure 中的虛擬機器上執行的 MongoDB](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/)。 示範如何使用 MongoDB 資料庫中的 ASP.NET web 應用程式的逐步教學課程。

HDInsight (Azure 上的 Hadoop):

- [HDInsight](https://azure.microsoft.com/en-us/documentation/services/hdinsight/)。 入口網站上的 HDInsight 文件[Azure](https://azure.microsoft.com/)網站。
- [Hadoop 和 HDInsight： 在 Azure 中的巨量資料](https://msdn.microsoft.com/en-us/magazine/dn385705.aspx)。 Bruno Terkaly 和 Ricardo Villalobos，介紹在 Azure 上的 Hadoop 的 MSDN Magazine 文件。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/en-us/library/dn568099.aspx)。 請參閱 MapReduce 模式。

>[!div class="step-by-step"]
[上一頁](single-sign-on.md)
[下一頁](data-partitioning-strategies.md)
