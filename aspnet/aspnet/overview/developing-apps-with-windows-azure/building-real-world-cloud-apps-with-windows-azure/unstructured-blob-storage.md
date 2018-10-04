---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: 非結構化的 Blob 儲存體 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1a56a76c9bf27fdd7afb090ca473ebeee4065fe2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577414"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>非結構化的 Blob 儲存體 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson]((https://twitter.com/RickAndMSFT))， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


在上一章中我們探討了資料分割配置，並說明它修正應用程式在 Azure 儲存體 Blob 服務和 Azure SQL Database 中的其他工作資料中所儲存的映像。 在這一章中我們會深入 Blob 服務，並顯示如何修正它的專案程式碼中實作。

## <a name="what-is-blob-storage"></a>什麼是 Blob 儲存體？

Azure 儲存體 Blob 服務會提供在雲端中儲存檔案的方式。 Blob 服務有許多優點，透過將檔案儲存在本機網路檔案系統：

- 它是高擴充性。 單一的儲存體帳戶可儲存[數百個 tb 且](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)，而且您可以有多個儲存體帳戶。 最大的 Azure 客戶的一些儲存以 pb 計的數百。 Microsoft SkyDrive 使用 blob 儲存體。
- 它會是永久性。 自動備份儲存在 Blob 服務的每個檔案。
- 它提供高可用性。 [儲存體 SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409)承諾高達 99.9%或 99.99%執行時間，依據您選擇的異地備援選項。
- 它是 Azure 中，這表示您只是儲存和擷取檔案，只需支付儲存體的實際數量的平台做為服務 (PaaS) 功能，Azure 會自動設定和管理的所有 Vm 和所需的磁碟機服務。
- 使用 REST API 或使用 API 的程式語言，您可以存取 Blob 服務。 Sdk 可供.NET、 Java、 Ruby 及其他項目。
- 當您將檔案儲存在 Blob 服務時，您可以輕鬆地進行其公開可用在網際網路上。
- 您可以保護服務，讓他們可以存取只能由授權的使用者，Blob 中的檔案，或者您可以提供暫時的存取權杖，使其可供人只會針對一段有限的時間。

每當您要建置適用於 Azure 的應用程式，而且您想要儲存大量資料，在內部部署環境中會放置在檔案，例如影像、 影片、 Pdf、 試算表等，請考慮 Blob 服務。

## <a name="creating-a-storage-account"></a>建立儲存體帳戶

若要開始使用 Blob 服務，請在 Azure 中建立儲存體帳戶。 在入口網站中，按一下**的新** -- **Data Services** -- **儲存體** -- **快速建立**，然後輸入 URL 和資料中心位置。 資料中心位置應該與您的 web 應用程式相同。

![建立儲存體 acct](unstructured-blob-storage/_static/image1.png)

您挑選您想要用來儲存內容，而且如果您選擇的主要區域[異地複寫](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)選項時，Azure 中建立複本的所有資料的國家/地區的另一個區域中不同資料中心。 比方說，如果您選擇美國西部資料中心，當您儲存的檔案，也會到美國西部的資料中心，但在背景 Azure 將它複製到其中一個其他美國資料中心。 發生災害時會在國家/地區的一個區域中，是您資料仍安全無虞。

Azure 不會複寫資料，跨越地理政治界限： 如果您的主要位置是在美國，US; 內的另一個區域僅複寫檔案如果您的主要位置是指澳洲，檔案只會複寫至澳洲的另一個資料中心上。

當然，您也可以建立儲存體帳戶來執行命令，從指令碼，如上文所述。 以下是建立儲存體帳戶的 Windows PowerShell 命令：

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

儲存體帳戶之後，您可以立即開始將檔案儲存在 Blob 服務。

## <a name="using-blob-storage-in-the-fix-it-app"></a>修正其應用程式中使用 Blob 儲存體

修正其應用程式可讓您將相片上傳。

![建立修正它的工作](unstructured-blob-storage/_static/image2.png)

當您按一下 **建立 FixIt**，將指定的影像檔案上傳應用程式，並將它儲存在 Blob 服務中。

### <a name="set-up-the-blob-container"></a>設定 Blob 容器

若要將檔案儲存在 Blob 服務，您需要*容器*來儲存它。 Blob 服務容器會對應至檔案系統資料夾。 我們檢閱在環境建立指令碼[自動執行的所有項目一章](automate-everything.md)建立儲存體帳戶，但它們不會建立容器。 因此的目的`CreateAndConfigure`方法的`PhotoService`類別是要建立容器，如果不存在。 這個方法會從呼叫`Application_Start`方法中的*Global.asax*。

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

儲存體帳戶名稱和存取金鑰會儲存在`appSettings`的集合*Web.config*檔案，並在程式碼`StorageUtils.StorageAccount`方法會使用這些值來建立連接字串，並建立連接：

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync`方法接著會建立物件，表示 Blob 服務，以及物件，表示容器 （資料夾） 中名為 Blob 服務中的 「 映像 」:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

如果容器名為 「 映像 」 還不存在，它會是第一次您針對新的儲存體帳戶-執行應用程式程式碼建立容器並設定即可將其公開的權限，則為 true。 （根據預設，新的 blob 容器都是私用和只有可存取您的儲存體帳戶的權限的使用者能夠存取。）

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>將已上傳的相片儲存在 Blob 儲存體

若要上傳並儲存該影像檔時，應用程式會使用`IPhotoService`介面和實作的介面中`PhotoService`類別。 *PhotoService.cs*檔案包含所有的程式碼修正其應用程式中使用 Blob 服務進行通訊。

下列 MVC 控制器方法在使用者按一下時呼叫**建立 FixIt**。 在此程式碼中，`photoService`的執行個體是指`PhotoService`類別，並`fixittask`的執行個體是指`FixItTask`實體類別，以儲存資料的新工作。

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync`方法中的`PhotoService`類別會儲存在 Blob 服務中的上傳的檔案，並傳回指向新的 blob URL。

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

依照`CreateAndConfigure`方法，將程式碼會連接到儲存體帳戶，並建立物件，表示 「 映像 」 blob 容器，但在此它會假設容器已存在。

然後它會建立唯一的識別項，會顯示上傳，藉由串連檔案副檔名為新的 GUID 值，映像：

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

程式碼，然後使用 blob 容器物件和新的唯一識別碼來建立 blob 物件，該物件表示何種檔案，它再用以將檔案儲存在 blob 儲存體的 blob 物件上設定屬性。

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

最後，它會取得參考之 blob 的 URL。 此 URL 會儲存在資料庫，並可以修正它的 web 網頁中用來顯示已上傳的映像。

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

此 URL 會儲存在資料庫中，做為其中一個 FixItTask 資料表的資料行。

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

使用只在資料庫中，URL 」 及 Blob 儲存體中的映像，它修正應用程式可將資料庫小型、 可調整且低成本，而映像儲存所在儲存體成本低廉且能夠處理數 tb 乃至於數 pb。 一個儲存體帳戶可以儲存數百個 tb 且修正它的相片，並只支付您所使用的項目。 因此，您可以在開始時的第一個 gb，小型付費的 9 分，並新增更多映像的其他 gb 每一分。

### <a name="display-the-uploaded-file"></a>顯示上傳的檔案

顯示工作詳細資料時，修正其應用程式就會顯示已上傳的映像檔。

![修正相片與工作詳細資料](unstructured-blob-storage/_static/image3.png)

若要顯示的映像，所有 MVC 檢視只需要為包含`PhotoUrl`HTML 傳送至瀏覽器中的值。 Web 伺服器和資料庫而不要使用循環顯示映像，他們只提供幾個位元組設定影像 URL。 在下列 Razor 程式碼中，`Model`的執行個體是指`FixItTask`實體類別。

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

如果您查看顯示之網頁的 HTML 時，您會看到直接指向影像 blob 的儲存體，如下所示的 URL:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>總結

您已了解如何修正它應用程式會儲存映像中的 Blob 服務，並只在 SQL database 中的影像 Url。 使用 Blob 服務，可讓 SQL 資料庫遠比它否則使得可相應增加，幾乎無限數量的工作，而不撰寫了大批程式碼即可。

您可以在儲存體帳戶中的數百個 tb，而且比 SQL 資料庫儲存體，開始在大約 3 分每 gb / 月加上的小型交易費用更便宜的儲存體成本。 並請記住，您會以最大容量，但只會針對您實際上存放區，因此您的應用程式隨時可調整規模，但您在不需要支付的這些額外的容量數量不需要支付。

在 [[下一步] 一章](design-to-survive-failures.md)我們將討論的重要性進行雲端應用程式能夠正常地處理失敗。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源：

- [Azure BLOB 儲存體簡介](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)。 由 Mike Wood 的部落格。
- [如何在.NET 中使用 Azure Blob 儲存體服務](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 MicrosoftAzure.com 網站上的官方文件集。 Blob 儲存體，後面接著程式碼範例示範如何連接到 blob 儲存體的簡介會建立容器、 上傳和下載 blob，依此類推。
- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部部分影片系列。 提供高階的概念和架構原則非常可存取且有趣的方式，取自 Microsoft 客戶諮詢團隊 (CAT) 體驗，與實際客戶的劇本。 如需 Azure 儲存體服務和 blob 的討論，請參閱第 5 集開始 35:13。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱 Valet 金鑰模式。

> [!div class="step-by-step"]
> [上一頁](data-partitioning-strategies.md)
> [下一頁](design-to-survive-failures.md)
