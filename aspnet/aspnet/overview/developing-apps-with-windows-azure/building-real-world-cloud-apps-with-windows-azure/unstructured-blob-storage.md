---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: "非結構化的 Blob 儲存體 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 6cb77e8ef301c2eeef7df3e391e14f4e2c0364e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>非結構化的 Blob 儲存體 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


前面章節中我們探討了資料分割配置，並說明其修正應用程式在 Azure 儲存體 Blob 服務和 Azure SQL Database 中的其他工作資料中所存放的映像檔。 在這一章我們深入 Blob 服務，並顯示如何修正它的專案程式碼中實作。

## <a name="what-is-blob-storage"></a>什麼是 Blob 儲存體？

Azure 儲存體 Blob 服務會提供在雲端中儲存檔案的方法。 Blob 服務的數目將檔案儲存在本機網路檔案系統的優點：

- 它是具有高擴充性。 單一儲存體帳戶可以儲存[數百個 tb 且](https://msdn.microsoft.com/en-us/library/windowsazure/dn249410.aspx)，而且您可以有多個儲存體帳戶。 部分最大的 Azure 客戶儲存數百個 pb。 Microsoft SkyDrive 使用 blob 儲存體。
- 它是持久。 每個檔案儲存在 Blob 服務會自動備份。
- 它提供高可用性。 [儲存體 SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409)承諾 99.9%或 99.99%執行時間，依據您選擇的地理備援性選項。
- 它是平台做為服務 (PaaS) 功能的 Azure，這表示只需要儲存，並擷取只支付您使用時，存放裝置的實際數量的檔案，Azure 會自動設定及管理的所有 Vm 和所需的磁碟機服務。
- 您可以使用 REST API，或使用應用程式開發介面的程式語言來存取 Blob 服務。 Sdk 可供.NET、 Java、 Ruby、 和其他項目。
- 檔案儲存在 Blob 服務時，您可以輕鬆地使其公開可透過網際網路。
- 您可以保護檔案中的 Blob 服務，讓他們可以存取只能由授權的使用者，或您可以提供可供其他人只會針對一段有限的時間的暫時存取語彙基元。

每當您要在 azure 建置應用程式，而且您想要儲存大量資料，在內部部署環境中會放在檔案-例如影像、 影片、 Pdf、 試算表等-請考慮 Blob 服務。

## <a name="creating-a-storage-account"></a>建立儲存體帳戶

若要開始使用 Blob 服務，請在 Azure 中建立儲存體帳戶。 在入口網站中，按一下**新增** -- **Data Services** -- **儲存體** -- **快速建立**，然後輸入 URL 和資料中心位置。 資料中心位置應該與您的 web 應用程式相同。

![建立儲存體 acct](unstructured-blob-storage/_static/image1.png)

您挑選您想要用來儲存內容，而且如果您選擇的主要區域[地理複寫](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)選項，Azure 複本的所有資料中建立不同的資料中心國家/地區的另一個區域中。 例如，如果您選擇美國西部資料中心，當您儲存的檔案，它也會至美國西部的資料中心，但是背景 Azure 中將它複製到其中一個其他美國資料中心。 在災難發生在一個國家/地區的區域中，您的資料仍然安全。

Azure 不會將資料複寫到地理政治界限： 如果主要位置是在美國太平洋時區，在美國太平洋時區; 內的另一個區域僅複寫檔案如果您的主要位置澳洲，澳大利亞的另一個資料中心僅複寫檔案。

當然，您也可以建立儲存體帳戶來執行命令，從指令碼，如上文所述。 以下是建立儲存體帳戶的 Windows PowerShell 命令：

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

儲存體帳戶之後，您可以立即開始將檔案儲存在 Blob 服務。

## <a name="using-blob-storage-in-the-fix-it-app"></a>修正它應用程式中使用 Blob 儲存體

修正它應用程式可讓您上傳相片。

![建立修正它的工作](unstructured-blob-storage/_static/image2.png)

當您按一下**建立 FixIt**，將指定的映像檔案上傳應用程式，並將它儲存在 Blob 服務中。

### <a name="set-up-the-blob-container"></a>設定 Blob 容器

若要將檔案儲存在 Blob 服務，您需要*容器*來將它存放在。 Blob 服務容器會對應至檔案系統資料夾。 我們已檢閱透過在環境建立指令碼[一切自動化章](automate-everything.md)建立儲存體帳戶，但它們不會建立容器。 因此目的`CreateAndConfigure`方法`PhotoService`類別是要建立容器，如果不存在。 這個方法從呼叫`Application_Start`方法中的*Global.asax*。

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

儲存體帳戶名稱和存取金鑰會儲存在`appSettings`集合*Web.config*檔案，然後在程式碼`StorageUtils.StorageAccount`方法會使用這些值來建立連接字串，並建立連接：

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync`方法接著會建立物件，表示 Blob 服務，並且物件，表示容器 （資料夾） 名為 Blob 服務中的 映像 」:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

如果容器名為 「 影像 」 還不存在-將第一次您執行應用程式針對新的儲存體帳戶-程式碼建立容器，並設定權限設為公用，則為 true。 （根據預設，新的 blob 容器是私用和只能由擁有存取儲存體帳戶的權限的使用者存取。）

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>將上傳的相片儲存在 Blob 儲存體

上傳並儲存映像檔，應用程式會使用`IPhotoService`介面和實作的介面中`PhotoService`類別。 *PhotoService.cs*檔案包含所有的程式碼中修正該應用程式與 Blob 服務進行通訊。

下列的 MVC 控制器方法在使用者按一下時呼叫**建立 FixIt**。 在此程式碼，`photoService`的執行個體是指`PhotoService`類別，和`fixittask`的執行個體是指`FixItTask`實體類別，以儲存新的工作資料。

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync`方法中的`PhotoService`類別將上傳的檔案儲存在 Blob 服務，並傳回指向新的 blob 的 URL。

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

在`CreateAndConfigure`方法時，程式碼會連接到儲存體帳戶，並建立物件，表示 「 影像 」 blob 容器，但這裡假設容器已經存在。

然後它會建立要上傳，藉由串連副檔名的新 GUID 值的映像的唯一識別項：

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

程式碼，然後使用 blob 容器物件和新的唯一識別碼來建立 blob 物件，該物件，表示何種檔案，並再將檔案儲存在 blob 儲存體使用 blob 物件上設定屬性。

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

最後，它會取得參考之 blob 的 URL。 此 URL 會儲存在資料庫中，而且可以修正它的 web 網頁中用來顯示上傳的影像。

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

此 URL 會儲存在資料庫中，做為其中一個 FixItTask 資料表的資料行。

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

與在資料庫中，URL 和映像 Blob 儲存體中的，修正它應用程式期間維持資料庫的小型、 可擴充且便宜，影像會儲存儲存體所在廉價能夠處理 tb 或 pb。 一個儲存體帳戶可以儲存數百個 tb 且修正它的相片，並只支付您所使用。 讓您可以開始關閉小付費 9 美分的第一個 gb，並加入其他 gb 每一分多個映像。

### <a name="display-the-uploaded-file"></a>顯示上傳的檔案

顯示工作詳細資料時，修正它應用程式會顯示已上傳映像檔。

![修正相片與工作詳細資料](unstructured-blob-storage/_static/image3.png)

若要顯示的映像，所有 MVC 檢視需要是包含`PhotoUrl`HTML 瀏覽器中的值。 Web 伺服器和資料庫不會使用循環來顯示影像，它們只提供幾個位元組設定影像 URL。 下列的 Razor 程式碼，`Model`的執行個體是指`FixItTask`實體類別。

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

如果您看看顯示的網頁的 HTML，您會看到直接指向在 blob 儲存體，就像這樣的影像的 URL:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>總結

您已經看到如何修正它應用程式會將映像中的 Blob 服務和 SQL database 中的只有影像 Url。 使用 Blob 服務可讓 SQL 資料庫遠低於它否則可讓您以向上調整到幾乎無限數量的工作，而可以完成而不需要撰寫大量程式碼。

儲存體帳戶中可以有數百個 tb 且，且 SQL 資料庫儲存體，開始在大約 3 美分的每個 gb，每個月份加上的小型交易費用成本較低很多的儲存體成本。 並請注意，您不支付的最大容量，但僅針對您實際儲存，因此您的應用程式已準備好縮放，但不是錢這些額外的容量數量。

在[下一章](design-to-survive-failures.md)我們將討論的重要性進行雲端應用程式能夠正常地處理失敗。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源：

- [Azure BLOB 儲存體簡介](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)。 Mike 木材部落格。
- [如何在.NET 中使用 Azure Blob 儲存體服務](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 MicrosoftAzure.com 站台上的官方文件集。 Blob 儲存體，後面接著程式碼範例顯示如何連接到 blob 儲存體，將簡單介紹建立容器、 上傳和下載 blob，依此類推。
- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九部影片系列。 高層級概念與架構原則非常可存取且有趣的方式，呈現劇本取自與實際客戶的 Microsoft 客戶諮詢團隊 (CAT) 體驗。 如需 Azure 儲存體服務和 blob 的討論，請參閱 35:13 時段 5 開始。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/en-us/library/dn568099.aspx)。 請參閱附屬金鑰模式。

>[!div class="step-by-step"]
[上一頁](data-partitioning-strategies.md)
[下一頁](design-to-survive-failures.md)
