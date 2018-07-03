---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: 非結構化的 Blob 儲存體 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1840bea3b4183838ffdfe710e987b864a05d53fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364180"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="0154d-104">非結構化的 Blob 儲存體 （使用 Azure 建置真實世界的雲端應用程式）</span><span class="sxs-lookup"><span data-stu-id="0154d-104">Unstructured Blob Storage (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="0154d-105">藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0154d-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0154d-106">[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="0154d-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="0154d-107">**建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。</span><span class="sxs-lookup"><span data-stu-id="0154d-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="0154d-108">它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0154d-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="0154d-109">電子書的相關資訊，請參閱[第 1 章](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="0154d-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="0154d-110">在上一章中我們探討了資料分割配置，並說明它修正應用程式在 Azure 儲存體 Blob 服務和 Azure SQL Database 中的其他工作資料中所儲存的映像。</span><span class="sxs-lookup"><span data-stu-id="0154d-110">In the previous chapter we looked at partitioning schemes and explained how the Fix It app stores images in the Azure Storage Blob service, and other task data in Azure SQL Database.</span></span> <span data-ttu-id="0154d-111">在這一章中我們會深入 Blob 服務，並顯示如何修正它的專案程式碼中實作。</span><span class="sxs-lookup"><span data-stu-id="0154d-111">In this chapter we go deeper into the Blob service and show how it's implemented in Fix It project code.</span></span>

## <a name="what-is-blob-storage"></a><span data-ttu-id="0154d-112">什麼是 Blob 儲存體？</span><span class="sxs-lookup"><span data-stu-id="0154d-112">What is Blob storage?</span></span>

<span data-ttu-id="0154d-113">Azure 儲存體 Blob 服務會提供在雲端中儲存檔案的方式。</span><span class="sxs-lookup"><span data-stu-id="0154d-113">The Azure Storage Blob service provides a way to store files in the cloud.</span></span> <span data-ttu-id="0154d-114">Blob 服務有許多優點，透過將檔案儲存在本機網路檔案系統：</span><span class="sxs-lookup"><span data-stu-id="0154d-114">The Blob service has a number of advantages over storing files in a local network file system:</span></span>

- <span data-ttu-id="0154d-115">它是高擴充性。</span><span class="sxs-lookup"><span data-stu-id="0154d-115">It's highly scalable.</span></span> <span data-ttu-id="0154d-116">單一的儲存體帳戶可儲存[數百個 tb 且](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)，而且您可以有多個儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="0154d-116">A single Storage account can store [hundreds of terabytes](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), and you can have multiple Storage accounts.</span></span> <span data-ttu-id="0154d-117">最大的 Azure 客戶的一些儲存以 pb 計的數百。</span><span class="sxs-lookup"><span data-stu-id="0154d-117">Some of the biggest Azure customers store hundreds of petabytes.</span></span> <span data-ttu-id="0154d-118">Microsoft SkyDrive 使用 blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="0154d-118">Microsoft SkyDrive uses blob storage.</span></span>
- <span data-ttu-id="0154d-119">它會是永久性。</span><span class="sxs-lookup"><span data-stu-id="0154d-119">It's durable.</span></span> <span data-ttu-id="0154d-120">自動備份儲存在 Blob 服務的每個檔案。</span><span class="sxs-lookup"><span data-stu-id="0154d-120">Every file you store in the Blob service is automatically backed up.</span></span>
- <span data-ttu-id="0154d-121">它提供高可用性。</span><span class="sxs-lookup"><span data-stu-id="0154d-121">It provides high availability.</span></span> <span data-ttu-id="0154d-122">[儲存體 SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409)承諾高達 99.9%或 99.99%執行時間，依據您選擇的異地備援選項。</span><span class="sxs-lookup"><span data-stu-id="0154d-122">The [SLA for Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promises 99.9% or 99.99% uptime, depending on which geo-redundancy option you choose.</span></span>
- <span data-ttu-id="0154d-123">它是 Azure 中，這表示您只是儲存和擷取檔案，只需支付儲存體的實際數量的平台做為服務 (PaaS) 功能，Azure 會自動設定和管理的所有 Vm 和所需的磁碟機服務。</span><span class="sxs-lookup"><span data-stu-id="0154d-123">It's a platform-as-a-service (PaaS) feature of Azure, which means you just store and retrieve files, paying only for the actual amount of storage you use, and Azure automatically takes care of setting up and managing all of the VMs and disk drives required for the service.</span></span>
- <span data-ttu-id="0154d-124">使用 REST API 或使用 API 的程式語言，您可以存取 Blob 服務。</span><span class="sxs-lookup"><span data-stu-id="0154d-124">You can access the Blob service by using a REST API or by using a programming language API.</span></span> <span data-ttu-id="0154d-125">Sdk 可供.NET、 Java、 Ruby 及其他項目。</span><span class="sxs-lookup"><span data-stu-id="0154d-125">SDKs are available for .NET, Java, Ruby, and others.</span></span>
- <span data-ttu-id="0154d-126">當您將檔案儲存在 Blob 服務時，您可以輕鬆地進行其公開可用在網際網路上。</span><span class="sxs-lookup"><span data-stu-id="0154d-126">When you store a file in the Blob service, you can easily make it publicly available over the Internet.</span></span>
- <span data-ttu-id="0154d-127">您可以保護服務，讓他們可以存取只能由授權的使用者，Blob 中的檔案，或者您可以提供暫時的存取權杖，使其可供人只會針對一段有限的時間。</span><span class="sxs-lookup"><span data-stu-id="0154d-127">You can secure files in the Blob service so they can accessed only by authorized users, or you can provide temporary access tokens that makes them available to someone only for a limited period of time.</span></span>

<span data-ttu-id="0154d-128">每當您要建置適用於 Azure 的應用程式，而且您想要儲存大量資料，在內部部署環境中會放置在檔案，例如影像、 影片、 Pdf、 試算表等，請考慮 Blob 服務。</span><span class="sxs-lookup"><span data-stu-id="0154d-128">Anytime you're building an app for Azure and you want to store a lot of data that in an on-premises environment would go in files -- such as images, videos, PDFs, spreadsheets, etc. -- consider the Blob service.</span></span>

## <a name="creating-a-storage-account"></a><span data-ttu-id="0154d-129">建立儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="0154d-129">Creating a Storage account</span></span>

<span data-ttu-id="0154d-130">若要開始使用 Blob 服務，請在 Azure 中建立儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="0154d-130">To get started with the Blob service you create a Storage account in Azure.</span></span> <span data-ttu-id="0154d-131">在入口網站中，按一下**的新** -- **Data Services** -- **儲存體** -- **快速建立**，然後輸入 URL 和資料中心位置。</span><span class="sxs-lookup"><span data-stu-id="0154d-131">In the portal, click **New** -- **Data Services** -- **Storage** -- **Quick Create**, and then enter a URL and a data center location.</span></span> <span data-ttu-id="0154d-132">資料中心位置應該與您的 web 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="0154d-132">The data center location should be the same as your web app.</span></span>

![建立儲存體 acct](unstructured-blob-storage/_static/image1.png)

<span data-ttu-id="0154d-134">您挑選您想要用來儲存內容，而且如果您選擇的主要區域[異地複寫](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)選項時，Azure 中建立複本的所有資料的國家/地區的另一個區域中不同資料中心。</span><span class="sxs-lookup"><span data-stu-id="0154d-134">You pick the primary region where you want to store the content, and if you choose the [geo-replication](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) option, Azure creates replicas of all your data in a different data center in another region of the country.</span></span> <span data-ttu-id="0154d-135">比方說，如果您選擇美國西部資料中心，當您儲存的檔案，也會到美國西部的資料中心，但在背景 Azure 將它複製到其中一個其他美國資料中心。</span><span class="sxs-lookup"><span data-stu-id="0154d-135">For example, if you choose the Western US data center, when you store a file it goes to the Western US data center, but in the background Azure also copies it to one of the other US data centers.</span></span> <span data-ttu-id="0154d-136">發生災害時會在國家/地區的一個區域中，是您資料仍安全無虞。</span><span class="sxs-lookup"><span data-stu-id="0154d-136">If a disaster happens in one region of the country, your data is still safe.</span></span>

<span data-ttu-id="0154d-137">Azure 不會複寫資料，跨越地理政治界限： 如果您的主要位置是在美國，US; 內的另一個區域僅複寫檔案如果您的主要位置是指澳洲，檔案只會複寫至澳洲的另一個資料中心上。</span><span class="sxs-lookup"><span data-stu-id="0154d-137">Azure won't replicate data across geo-political boundaries: if your primary location is in the U.S., your files are only replicated to another region within the U.S.; if your primary location is Australia, your files are only replicated to another data center in Australia.</span></span>

<span data-ttu-id="0154d-138">當然，您也可以建立儲存體帳戶來執行命令，從指令碼，如上文所述。</span><span class="sxs-lookup"><span data-stu-id="0154d-138">Of course, you can also create a Storage account by executing commands from a script, as we saw earlier.</span></span> <span data-ttu-id="0154d-139">以下是建立儲存體帳戶的 Windows PowerShell 命令：</span><span class="sxs-lookup"><span data-stu-id="0154d-139">Here's a Windows PowerShell command to create a Storage account:</span></span>

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

<span data-ttu-id="0154d-140">儲存體帳戶之後，您可以立即開始將檔案儲存在 Blob 服務。</span><span class="sxs-lookup"><span data-stu-id="0154d-140">Once you have a Storage account, you can immediately start storing files in the Blob service.</span></span>

## <a name="using-blob-storage-in-the-fix-it-app"></a><span data-ttu-id="0154d-141">修正其應用程式中使用 Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="0154d-141">Using Blob storage in the Fix It app</span></span>

<span data-ttu-id="0154d-142">修正其應用程式可讓您將相片上傳。</span><span class="sxs-lookup"><span data-stu-id="0154d-142">The Fix It app enables you to upload photos.</span></span>

![建立修正它的工作](unstructured-blob-storage/_static/image2.png)

<span data-ttu-id="0154d-144">當您按一下 **建立 FixIt**，將指定的影像檔案上傳應用程式，並將它儲存在 Blob 服務中。</span><span class="sxs-lookup"><span data-stu-id="0154d-144">When you click **Create the FixIt**, the application uploads the specified image file and stores it in the Blob service.</span></span>

### <a name="set-up-the-blob-container"></a><span data-ttu-id="0154d-145">設定 Blob 容器</span><span class="sxs-lookup"><span data-stu-id="0154d-145">Set up the Blob container</span></span>

<span data-ttu-id="0154d-146">若要將檔案儲存在 Blob 服務，您需要*容器*來儲存它。</span><span class="sxs-lookup"><span data-stu-id="0154d-146">In order to store a file in the Blob service you need a *container* to store it in.</span></span> <span data-ttu-id="0154d-147">Blob 服務容器會對應至檔案系統資料夾。</span><span class="sxs-lookup"><span data-stu-id="0154d-147">A Blob service container corresponds to a file system folder.</span></span> <span data-ttu-id="0154d-148">我們檢閱在環境建立指令碼[自動執行的所有項目一章](automate-everything.md)建立儲存體帳戶，但它們不會建立容器。</span><span class="sxs-lookup"><span data-stu-id="0154d-148">The environment creation scripts that we reviewed in the [Automate Everything chapter](automate-everything.md) create the Storage account, but they don't create a container.</span></span> <span data-ttu-id="0154d-149">因此的目的`CreateAndConfigure`方法的`PhotoService`類別是要建立容器，如果不存在。</span><span class="sxs-lookup"><span data-stu-id="0154d-149">So the purpose of the `CreateAndConfigure` method of the `PhotoService` class is to create a container if it doesn't already exist.</span></span> <span data-ttu-id="0154d-150">這個方法會從呼叫`Application_Start`方法中的*Global.asax*。</span><span class="sxs-lookup"><span data-stu-id="0154d-150">This method is called from the `Application_Start` method in *Global.asax*.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

<span data-ttu-id="0154d-151">儲存體帳戶名稱和存取金鑰會儲存在`appSettings`的集合*Web.config*檔案，並在程式碼`StorageUtils.StorageAccount`方法會使用這些值來建立連接字串，並建立連接：</span><span class="sxs-lookup"><span data-stu-id="0154d-151">The storage account name and access key are stored in the `appSettings` collection of the *Web.config* file, and code in the `StorageUtils.StorageAccount` method uses those values to build a connection string and establish a connection:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

<span data-ttu-id="0154d-152">`CreateAndConfigureAsync`方法接著會建立物件，表示 Blob 服務，以及物件，表示容器 （資料夾） 中名為 Blob 服務中的 「 映像 」:</span><span class="sxs-lookup"><span data-stu-id="0154d-152">The `CreateAndConfigureAsync` method then creates an object that represents the Blob service, and an object that represents a container (folder) named "images" in the Blob service:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

<span data-ttu-id="0154d-153">如果容器名為 「 映像 」 還不存在，它會是第一次您針對新的儲存體帳戶-執行應用程式程式碼建立容器並設定即可將其公開的權限，則為 true。</span><span class="sxs-lookup"><span data-stu-id="0154d-153">If a container named "images" doesn't exist yet -- which will be true the first time you run the app against a new storage account -- the code creates the container and sets permissions to make it public.</span></span> <span data-ttu-id="0154d-154">（根據預設，新的 blob 容器都是私用和只有可存取您的儲存體帳戶的權限的使用者能夠存取。）</span><span class="sxs-lookup"><span data-stu-id="0154d-154">(By default, new blob containers are private and are accessible only to users who have permission to access your storage account.)</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a><span data-ttu-id="0154d-155">將已上傳的相片儲存在 Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="0154d-155">Store the uploaded photo in Blob storage</span></span>

<span data-ttu-id="0154d-156">若要上傳並儲存該影像檔時，應用程式會使用`IPhotoService`介面和實作的介面中`PhotoService`類別。</span><span class="sxs-lookup"><span data-stu-id="0154d-156">To upload and save the image file, the app uses an `IPhotoService` interface and an implementation of the interface in the `PhotoService` class.</span></span> <span data-ttu-id="0154d-157">*PhotoService.cs*檔案包含所有的程式碼修正其應用程式中使用 Blob 服務進行通訊。</span><span class="sxs-lookup"><span data-stu-id="0154d-157">The *PhotoService.cs* file contains all of the code in the Fix It app that communicates with the Blob service.</span></span>

<span data-ttu-id="0154d-158">下列 MVC 控制器方法在使用者按一下時呼叫**建立 FixIt**。</span><span class="sxs-lookup"><span data-stu-id="0154d-158">The following MVC controller method is called when the user clicks **Create the FixIt**.</span></span> <span data-ttu-id="0154d-159">在此程式碼中，`photoService`的執行個體是指`PhotoService`類別，並`fixittask`的執行個體是指`FixItTask`實體類別，以儲存資料的新工作。</span><span class="sxs-lookup"><span data-stu-id="0154d-159">In this code, `photoService` refers to an instance of the `PhotoService` class, and `fixittask` refers to an instance of the `FixItTask` entity class that stores data for a new task.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

<span data-ttu-id="0154d-160">`UploadPhotoAsync`方法中的`PhotoService`類別會儲存在 Blob 服務中的上傳的檔案，並傳回指向新的 blob URL。</span><span class="sxs-lookup"><span data-stu-id="0154d-160">The `UploadPhotoAsync` method in the `PhotoService` class stores the uploaded file in the Blob service and returns a URL that points to the new blob.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

<span data-ttu-id="0154d-161">依照`CreateAndConfigure`方法，將程式碼會連接到儲存體帳戶，並建立物件，表示 「 映像 」 blob 容器，但在此它會假設容器已存在。</span><span class="sxs-lookup"><span data-stu-id="0154d-161">As in the `CreateAndConfigure` method, the code connects to the storage account and creates an object that represents the "images" blob container, except here it assumes the container already exists.</span></span>

<span data-ttu-id="0154d-162">然後它會建立唯一的識別項，會顯示上傳，藉由串連檔案副檔名為新的 GUID 值，映像：</span><span class="sxs-lookup"><span data-stu-id="0154d-162">Then it creates a unique identifier for the image about to be uploaded, by concatenating a new GUID value with the file extension:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

<span data-ttu-id="0154d-163">程式碼，然後使用 blob 容器物件和新的唯一識別碼來建立 blob 物件，該物件表示何種檔案，它再用以將檔案儲存在 blob 儲存體的 blob 物件上設定屬性。</span><span class="sxs-lookup"><span data-stu-id="0154d-163">The code then uses the blob container object and the new unique identifier to create a blob object, sets an attribute on that object indicating what kind of file it is, and then uses the blob object to store the file in blob storage.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

<span data-ttu-id="0154d-164">最後，它會取得參考之 blob 的 URL。</span><span class="sxs-lookup"><span data-stu-id="0154d-164">Finally, it gets a URL that references the blob.</span></span> <span data-ttu-id="0154d-165">此 URL 會儲存在資料庫，並可以修正它的 web 網頁中用來顯示已上傳的映像。</span><span class="sxs-lookup"><span data-stu-id="0154d-165">This URL will be stored in the database and can be used in Fix It web pages to display the uploaded image.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

<span data-ttu-id="0154d-166">此 URL 會儲存在資料庫中，做為其中一個 FixItTask 資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="0154d-166">This URL is stored in the database as one of the columns of the FixItTask table.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

<span data-ttu-id="0154d-167">使用只在資料庫中，URL 」 及 Blob 儲存體中的映像，它修正應用程式可將資料庫小型、 可調整且低成本，而映像儲存所在儲存體成本低廉且能夠處理數 tb 乃至於數 pb。</span><span class="sxs-lookup"><span data-stu-id="0154d-167">With only the URL in the database, and images in Blob storage, the Fix It app keeps the database small, scalable, and inexpensive, while the images are stored where storage is cheap and capable of handling terabytes or petabytes.</span></span> <span data-ttu-id="0154d-168">一個儲存體帳戶可以儲存數百個 tb 且修正它的相片，並只支付您所使用的項目。</span><span class="sxs-lookup"><span data-stu-id="0154d-168">One storage account can store hundreds of terabytes of Fix It photos, and you only pay for what you use.</span></span> <span data-ttu-id="0154d-169">因此，您可以在開始時的第一個 gb，小型付費的 9 分，並新增更多映像的其他 gb 每一分。</span><span class="sxs-lookup"><span data-stu-id="0154d-169">So you can start off small paying 9 cents for the first gigabyte, and add more images for pennies per additional gigabyte.</span></span>

### <a name="display-the-uploaded-file"></a><span data-ttu-id="0154d-170">顯示上傳的檔案</span><span class="sxs-lookup"><span data-stu-id="0154d-170">Display the uploaded file</span></span>

<span data-ttu-id="0154d-171">顯示工作詳細資料時，修正其應用程式就會顯示已上傳的映像檔。</span><span class="sxs-lookup"><span data-stu-id="0154d-171">The Fix It application displays the uploaded image file when it displays details for a task.</span></span>

![修正相片與工作詳細資料](unstructured-blob-storage/_static/image3.png)

<span data-ttu-id="0154d-173">若要顯示的映像，所有 MVC 檢視只需要為包含`PhotoUrl`HTML 傳送至瀏覽器中的值。</span><span class="sxs-lookup"><span data-stu-id="0154d-173">To display the image, all the MVC view has to do is include the `PhotoUrl` value in the HTML sent to the browser.</span></span> <span data-ttu-id="0154d-174">Web 伺服器和資料庫而不要使用循環顯示映像，他們只提供幾個位元組設定影像 URL。</span><span class="sxs-lookup"><span data-stu-id="0154d-174">The web server and the database are not using cycles to display the image, they are only serving up a few bytes to the image URL.</span></span> <span data-ttu-id="0154d-175">在下列 Razor 程式碼中，`Model`的執行個體是指`FixItTask`實體類別。</span><span class="sxs-lookup"><span data-stu-id="0154d-175">In the following Razor code, `Model` refers to an instance of the `FixItTask` entity class.</span></span>

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

<span data-ttu-id="0154d-176">如果您查看顯示之網頁的 HTML 時，您會看到直接指向影像 blob 的儲存體，如下所示的 URL:</span><span class="sxs-lookup"><span data-stu-id="0154d-176">If you look at the HTML of the page that displays, you see the URL pointing directly to the image in blob storage, something like this:</span></span>

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a><span data-ttu-id="0154d-177">總結</span><span class="sxs-lookup"><span data-stu-id="0154d-177">Summary</span></span>

<span data-ttu-id="0154d-178">您已了解如何修正它應用程式會儲存映像中的 Blob 服務，並只在 SQL database 中的影像 Url。</span><span class="sxs-lookup"><span data-stu-id="0154d-178">You've seen how the Fix It app stores images in the Blob service and only image URLs in the SQL database.</span></span> <span data-ttu-id="0154d-179">使用 Blob 服務，可讓 SQL 資料庫遠比它否則使得可相應增加，幾乎無限數量的工作，而不撰寫了大批程式碼即可。</span><span class="sxs-lookup"><span data-stu-id="0154d-179">Using the Blob service keeps the SQL database much smaller than it otherwise would be, makes it possible to scale up to an almost unlimited number of tasks, and can be done without writing a lot of code.</span></span>

<span data-ttu-id="0154d-180">您可以在儲存體帳戶中的數百個 tb，而且比 SQL 資料庫儲存體，開始在大約 3 分每 gb / 月加上的小型交易費用更便宜的儲存體成本。</span><span class="sxs-lookup"><span data-stu-id="0154d-180">You can have hundreds of terabytes in a storage account, and the storage cost is much less expensive than SQL Database storage, starting at about 3 cents per gigabyte per month plus a small transaction charge.</span></span> <span data-ttu-id="0154d-181">並請記住，您會以最大容量，但只會針對您實際上存放區，因此您的應用程式隨時可調整規模，但您在不需要支付的這些額外的容量數量不需要支付。</span><span class="sxs-lookup"><span data-stu-id="0154d-181">And keep in mind that you're not paying for the maximum capacity but only for the amount you actually store, so your app is ready to scale but you're not paying for all that extra capacity.</span></span>

<span data-ttu-id="0154d-182">在 [[下一步] 一章](design-to-survive-failures.md)我們將討論的重要性進行雲端應用程式能夠正常地處理失敗。</span><span class="sxs-lookup"><span data-stu-id="0154d-182">In the [next chapter](design-to-survive-failures.md) we'll talk about the importance of making a cloud app capable of gracefully handling failures.</span></span>

## <a name="resources"></a><span data-ttu-id="0154d-183">資源</span><span class="sxs-lookup"><span data-stu-id="0154d-183">Resources</span></span>

<span data-ttu-id="0154d-184">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="0154d-184">For more information see the following resources:</span></span>

- <span data-ttu-id="0154d-185">[Azure BLOB 儲存體簡介](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)。</span><span class="sxs-lookup"><span data-stu-id="0154d-185">[An Introduction to Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/).</span></span> <span data-ttu-id="0154d-186">由 Mike Wood 的部落格。</span><span class="sxs-lookup"><span data-stu-id="0154d-186">Blog by Mike Wood.</span></span>
- <span data-ttu-id="0154d-187">[如何在.NET 中使用 Azure Blob 儲存體服務](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。</span><span class="sxs-lookup"><span data-stu-id="0154d-187">[How to use the Azure Blob Storage Service in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="0154d-188">MicrosoftAzure.com 網站上的官方文件集。</span><span class="sxs-lookup"><span data-stu-id="0154d-188">Official documentation on the MicrosoftAzure.com site.</span></span> <span data-ttu-id="0154d-189">Blob 儲存體，後面接著程式碼範例示範如何連接到 blob 儲存體的簡介會建立容器、 上傳和下載 blob，依此類推。</span><span class="sxs-lookup"><span data-stu-id="0154d-189">A brief introduction to blob storage followed by code examples showing how to connect to blob storage, create containers, upload and download blobs, etc.</span></span>
- <span data-ttu-id="0154d-190">[FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。</span><span class="sxs-lookup"><span data-stu-id="0154d-190">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="0154d-191">Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部部分影片系列。</span><span class="sxs-lookup"><span data-stu-id="0154d-191">Nine-part video series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="0154d-192">提供高階的概念和架構原則非常可存取且有趣的方式，取自 Microsoft 客戶諮詢團隊 (CAT) 體驗，與實際客戶的劇本。</span><span class="sxs-lookup"><span data-stu-id="0154d-192">Presents high-level concepts and architectural principles in a very accessible and interesting way, with stories drawn from Microsoft Customer Advisory Team (CAT) experience with actual customers.</span></span> <span data-ttu-id="0154d-193">如需 Azure 儲存體服務和 blob 的討論，請參閱第 5 集開始 35:13。</span><span class="sxs-lookup"><span data-stu-id="0154d-193">For a discussion of Azure Storage service and blobs, see episode 5 starting at 35:13.</span></span>
- <span data-ttu-id="0154d-194">[Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0154d-194">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx).</span></span> <span data-ttu-id="0154d-195">請參閱 Valet 金鑰模式。</span><span class="sxs-lookup"><span data-stu-id="0154d-195">See Valet Key pattern.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0154d-196">[上一頁](data-partitioning-strategies.md)
> [下一頁](design-to-survive-failures.md)</span><span class="sxs-lookup"><span data-stu-id="0154d-196">[Previous](data-partitioning-strategies.md)
[Next](design-to-survive-failures.md)</span></span>
