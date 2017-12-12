---
uid: whitepapers/aspnet-and-iis6
title: "執行 IIS 6.0 與 ASP.NET 1.1 |Microsoft 文件"
author: rick-anderson
description: "雖然 Windows Server 2003 包括 IIS 6.0 與 ASP.NET 1.1，預設會停用這些元件。 本白皮書說明如何啟用 IIS 6.0..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="37a37-104">執行 IIS 6.0 與 ASP.NET 1.1</span><span class="sxs-lookup"><span data-stu-id="37a37-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="37a37-105">雖然 Windows Server 2003 包括 IIS 6.0 與 ASP.NET 1.1，預設會停用這些元件。</span><span class="sxs-lookup"><span data-stu-id="37a37-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="37a37-106">本白皮書說明如何啟用 IIS 6.0 與 ASP.NET 1.1，並建議數個組態設定從 IIS 和 ASP.NET 中取得最佳效能。</span><span class="sxs-lookup"><span data-stu-id="37a37-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="37a37-107">適用於 ASP.NET 1.1 版和 IIS 6.0。</span><span class="sxs-lookup"><span data-stu-id="37a37-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="37a37-108">ASP.NET 1.1 隨附於 Windows Server 2003，也會包含最新版的 Internet Information Server (IIS) 6.0 版的版本。</span><span class="sxs-lookup"><span data-stu-id="37a37-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="37a37-109">IIS 6.0 與 ASP.NET 1.1 專為緊密整合，ASP.NET 現在預設為新的 IIS 6.0 工作者處理序模型。</span><span class="sxs-lookup"><span data-stu-id="37a37-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="37a37-110">根據預設不會安裝 ASP.NET 1.1</span><span class="sxs-lookup"><span data-stu-id="37a37-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="37a37-111">不同於舊版的 Microsoft 的伺服器作業系統，Internet Information Server (IIS) 是預設不啟用。也不是 ASP.NET 1.1。</span><span class="sxs-lookup"><span data-stu-id="37a37-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="37a37-112">有兩個選項可啟用 IIS:</span><span class="sxs-lookup"><span data-stu-id="37a37-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="37a37-113">啟用 IIS，選項 #1-設定您的伺服器精靈</span><span class="sxs-lookup"><span data-stu-id="37a37-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="37a37-114">Windows Server 2003 隨附新 ' 設定您的伺服器精靈 可協助您正確設定您的伺服器中的所需的模式。</span><span class="sxs-lookup"><span data-stu-id="37a37-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="37a37-115">若要啟動精靈-請注意，若要執行精靈，您必須以系統管理員的身分登入，請移至： 啟動 |程式 |系統管理工具] 並選取 [設定您的伺服器 '。</span><span class="sxs-lookup"><span data-stu-id="37a37-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="37a37-116">您應該會看到 ' 設定您的伺服器精靈 開啟螢幕一次選取：</span><span class="sxs-lookup"><span data-stu-id="37a37-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="37a37-117">按一下 下一步&gt;':</span><span class="sxs-lookup"><span data-stu-id="37a37-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="37a37-118">按一下 下一步&gt;'</span><span class="sxs-lookup"><span data-stu-id="37a37-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="37a37-119">在這個畫面上，您將需要選取 ' 的應用程式伺服器 （IIS，ASP.NET） 來設定選項。</span><span class="sxs-lookup"><span data-stu-id="37a37-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="37a37-120">按一下 下一步&gt;'。</span><span class="sxs-lookup"><span data-stu-id="37a37-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="37a37-121">選取後將伺服器設定為應用程式伺服器，這個畫面將會顯示提示應該安裝哪些其他功能。</span><span class="sxs-lookup"><span data-stu-id="37a37-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="37a37-122">預設會選取這兩個選項。</span><span class="sxs-lookup"><span data-stu-id="37a37-122">Neither option is selected by default.</span></span> <span data-ttu-id="37a37-123">若要自動啟用 ASP.NET，您必須選取 ' 啟用 ASP。NET'。</span><span class="sxs-lookup"><span data-stu-id="37a37-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="37a37-124">按一下 下一步&gt;'。</span><span class="sxs-lookup"><span data-stu-id="37a37-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="37a37-125">此畫面顯示選項會安裝。</span><span class="sxs-lookup"><span data-stu-id="37a37-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="37a37-126">按一下 下一步&gt;'。</span><span class="sxs-lookup"><span data-stu-id="37a37-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="37a37-127">正在安裝您選取的選項時，您會看到這個畫面。</span><span class="sxs-lookup"><span data-stu-id="37a37-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="37a37-128">這是正常看到其他對話方塊會出現方塊，因為正在安裝服務。</span><span class="sxs-lookup"><span data-stu-id="37a37-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="37a37-129">您可能此外會提示您在 Windows 2003 Server 安裝光碟的位置。</span><span class="sxs-lookup"><span data-stu-id="37a37-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="37a37-130">按一下 下一步&gt;' 時完成。</span><span class="sxs-lookup"><span data-stu-id="37a37-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="37a37-131">按一下 [完成]-Windows Server 2003 現在已設定為支援 IIS 6.0 與 ASP.NET 1.1。</span><span class="sxs-lookup"><span data-stu-id="37a37-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="37a37-132">啟用 IIS，選項 #2-手動設定 IIS 和 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="37a37-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="37a37-133">如果您不想使用 ' 設定您的伺服器精靈 可以選擇性地從控制台安裝 IIS 6.0 與 ASP.NET 1.1，使用 新增或移除程式。</span><span class="sxs-lookup"><span data-stu-id="37a37-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="37a37-134">第一次開啟 控制台：</span><span class="sxs-lookup"><span data-stu-id="37a37-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="37a37-135">接下來，按一下 新增/移除 Windows 元件 ' 它將會開啟 「 Windows 元件精靈 」:</span><span class="sxs-lookup"><span data-stu-id="37a37-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="37a37-136">反白顯示及檢查 「 應用程式伺服器 」，然後按一下 [詳細資料]？</span><span class="sxs-lookup"><span data-stu-id="37a37-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="37a37-137">按鈕：</span><span class="sxs-lookup"><span data-stu-id="37a37-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="37a37-138">若要安裝 ASP.NET，請檢查 ' ASP。NET'。</span><span class="sxs-lookup"><span data-stu-id="37a37-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="37a37-139">按一下 [確定] 以返回 [Windows 元件精靈]。</span><span class="sxs-lookup"><span data-stu-id="37a37-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="37a37-140">按一下 下一步&gt;' 從 Windows 元件精靈，開始安裝：</span><span class="sxs-lookup"><span data-stu-id="37a37-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="37a37-141">這是正常看到其他對話方塊會出現方塊，因為正在安裝服務。</span><span class="sxs-lookup"><span data-stu-id="37a37-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="37a37-142">您可能此外會提示您在 Windows 2003 Server 安裝光碟的位置。</span><span class="sxs-lookup"><span data-stu-id="37a37-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="37a37-143">安裝完成時，您會看到 Windows 元件精靈的最後一個畫面：</span><span class="sxs-lookup"><span data-stu-id="37a37-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="37a37-144">現在設定 IIS 6.0 與 ASP.NET 1.1 及可用。</span><span class="sxs-lookup"><span data-stu-id="37a37-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="37a37-145">建議的設定</span><span class="sxs-lookup"><span data-stu-id="37a37-145">Recommended Settings</span></span>

<span data-ttu-id="37a37-146">IIS 6.0 中執行 ASP.NET 1.1 時有數個建議使用它們以取得最佳效能，從 ASP.NET 的組態設定：</span><span class="sxs-lookup"><span data-stu-id="37a37-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="37a37-147">設定背景工作處理序記憶體限制</span><span class="sxs-lookup"><span data-stu-id="37a37-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="37a37-148">設定背景工作處理序回收</span><span class="sxs-lookup"><span data-stu-id="37a37-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="37a37-149">設定背景工作處理序記憶體限制</span><span class="sxs-lookup"><span data-stu-id="37a37-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="37a37-150">預設 IIS 6.0 不會設定限制的允許 IIS 使用的記憶體數量。</span><span class="sxs-lookup"><span data-stu-id="37a37-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="37a37-151">ASP。網路的快取功能會依賴記憶體的限制，所以從記憶體中快取可以主動移除未使用的項目。</span><span class="sxs-lookup"><span data-stu-id="37a37-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="37a37-152">建議您設定的記憶體回收的 IIS 6.0 的功能。</span><span class="sxs-lookup"><span data-stu-id="37a37-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="37a37-153">若要設定這個開啟的 Internet Information Services 管理員 (啟動 |程式 |系統管理工具 |網際網路資訊服務）。</span><span class="sxs-lookup"><span data-stu-id="37a37-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="37a37-154">一旦開啟，請展開 '應用程式集區' 資料夾：</span><span class="sxs-lookup"><span data-stu-id="37a37-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="37a37-155">每個應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="37a37-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="37a37-156">以滑鼠右鍵按一下應用程式集區，例如：'DefaultAppPool'，並選取 [內容]:</span><span class="sxs-lookup"><span data-stu-id="37a37-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="37a37-157">接下來，讓記憶體回收，請按一下 ' 使用的記憶體上限 （以 mb 為單位）:'。</span><span class="sxs-lookup"><span data-stu-id="37a37-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="37a37-158">值不能超過伺服器上的實體 （不是虛擬的） 的記憶體數量，很好的近似值是 60%的實體記憶體，也就是具有 512 MB 的實體記憶體選取 310 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="37a37-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="37a37-159">也建議使用 2 GB 的位址空間時，最多不超過 800 MB。</span><span class="sxs-lookup"><span data-stu-id="37a37-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="37a37-160">如果伺服器的記憶體位址空間為 3 GB，可以是 1，800 MB 最高工作者處理序的最大記憶體限制：</span><span class="sxs-lookup"><span data-stu-id="37a37-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="37a37-161">按一下 [套用] 和 [確定] 以結束 [屬性] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="37a37-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="37a37-162">所有可用的應用程式集區重複此步驟。</span><span class="sxs-lookup"><span data-stu-id="37a37-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="37a37-163">設定回收背景工作</span><span class="sxs-lookup"><span data-stu-id="37a37-163">Configuring worker recycling</span></span>

<span data-ttu-id="37a37-164">預設會設定 IIS 6.0，以回收其工作者處理序每 29 小時。</span><span class="sxs-lookup"><span data-stu-id="37a37-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="37a37-165">這是一個位元積極執行 ASP.NET 應用程式，並建議，停用自動背景工作處理序回收。</span><span class="sxs-lookup"><span data-stu-id="37a37-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="37a37-166">若要停用自動背景工作處理序回收，請先開啟 Internet Information Services 管理員 (啟動 |程式 |系統管理工具 |網際網路資訊服務）。</span><span class="sxs-lookup"><span data-stu-id="37a37-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="37a37-167">一旦開啟，請展開 '應用程式集區' 資料夾：</span><span class="sxs-lookup"><span data-stu-id="37a37-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="37a37-168">每個應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="37a37-168">For each application pool:</span></span>

1. <span data-ttu-id="37a37-169">以滑鼠右鍵按一下應用程式集區，例如：'DefaultAppPool'，並選取 [內容]:</span><span class="sxs-lookup"><span data-stu-id="37a37-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="37a37-170">取消選取 ' 回收工作者處理序 （以分鐘為單位）:':</span><span class="sxs-lookup"><span data-stu-id="37a37-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="37a37-171">按一下 [套用] 和 [確定] 以結束 [屬性] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="37a37-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="37a37-172">所有可用的應用程式集區重複此步驟。</span><span class="sxs-lookup"><span data-stu-id="37a37-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="37a37-173">授與檔案系統的寫入權限</span><span class="sxs-lookup"><span data-stu-id="37a37-173">Granting write access to the file system</span></span>

<span data-ttu-id="37a37-174">如果您的應用程式需要在檔案系統的寫入權限，而且您正在使用 NTFS 您必須修改存取控制清單 (ACL) 上的資料夾或檔案授與 ASP.NET 存取權。</span><span class="sxs-lookup"><span data-stu-id="37a37-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="37a37-175">比方說，若要授與 ASP.NET c:\inetpub\wwwroot 的寫入權限第一次開啟檔案總管並瀏覽至目錄：</span><span class="sxs-lookup"><span data-stu-id="37a37-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="37a37-176">接下來，以滑鼠右鍵按一下目錄，例如 'wwwroot' 並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="37a37-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="37a37-177">[屬性] 對話方塊開啟之後，選取 [安全性] 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="37a37-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="37a37-178">C:\inetpub\wwwroot\ 目錄是特殊的目錄中的特殊 IIS 6.0 群組 ' IIS\_WPG' 已經授與讀取&amp;執行、 列出資料夾內容、 及讀取權限。</span><span class="sxs-lookup"><span data-stu-id="37a37-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="37a37-179">不過，若要授與寫入權限，您需要寫入中按一下 [允許] 核取方塊：</span><span class="sxs-lookup"><span data-stu-id="37a37-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="37a37-180">IIS 6.0 現在會有此資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="37a37-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="37a37-181">若要授與寫入權限，在其他資料夾，請遵循下列步驟-請注意，您可能需要加入 IIS\_WPG 群組，如果不存在。</span><span class="sxs-lookup"><span data-stu-id="37a37-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="37a37-182">授與寫入權限 IIS\_WPG 可讓任何可寫入這個目錄的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="37a37-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="37a37-183">支援與 SQL Server 的整合式的驗證</span><span class="sxs-lookup"><span data-stu-id="37a37-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="37a37-184">整合式的驗證可讓 SQL Server，以運用 Windows NT 驗證以驗證 SQL Server 登入帳戶。</span><span class="sxs-lookup"><span data-stu-id="37a37-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="37a37-185">這可讓使用者略過標準的 SQL Server 登入程序。</span><span class="sxs-lookup"><span data-stu-id="37a37-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="37a37-186">使用此方法時，網路使用者可以存取 SQL Server 資料庫，而不需要提供不同的登入識別碼或密碼，因為 SQL Server 中取得使用者和密碼資訊從 Windows NT 網路安全性的程序。</span><span class="sxs-lookup"><span data-stu-id="37a37-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="37a37-187">選擇 ASP.NET 應用程式的整合式的驗證是不錯的選擇，因為沒有認證永遠會儲存在您的應用程式的連接字串。</span><span class="sxs-lookup"><span data-stu-id="37a37-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="37a37-188">而是用來連接到 SQL 連接字串將如下所示：</span><span class="sxs-lookup"><span data-stu-id="37a37-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="37a37-189">這個連接字串會告知 SQL Server 使用嘗試存取 SQL Server 的應用程式的 Windows 認證。</span><span class="sxs-lookup"><span data-stu-id="37a37-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="37a37-190">在 ASP.NET/IIS 6 的情況下，這會是在 IIS 中的帳戶\_WPG 群組。</span><span class="sxs-lookup"><span data-stu-id="37a37-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="37a37-191">若要啟用 SQL Server 與 ASP.NET 之間的整合式的驗證，您必須先確認 SQL Server 針對整合式的驗證或混合模式驗證-設定與判斷 DBA 檢查。</span><span class="sxs-lookup"><span data-stu-id="37a37-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="37a37-192">如果 SQL Server 是在這兩種模式的其中一個，您可以使用整合式的驗證。</span><span class="sxs-lookup"><span data-stu-id="37a37-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="37a37-193">開啟 SQL Server Enterprise Manager (開始 |程式 |Microsoft SQL Server |Enterprise Manager)，選取適當的伺服器，並展開 [安全性] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="37a37-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="37a37-194">如果 ' BUILTINT\IIS\_WPG' 未列出群組，以滑鼠右鍵按一下 登入並選取 新的登入:</span><span class="sxs-lookup"><span data-stu-id="37a37-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="37a37-195">在 '名稱:'，或者輸入的文字方塊中' [伺服器/網域名稱] \IIS\_WPG' 或按一下省略符號按鈕，以開啟 Windows NT 使用者/群組選擇器：</span><span class="sxs-lookup"><span data-stu-id="37a37-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="37a37-196">選取 在目前電腦的 IIS\_WPG 群組，然後按一下 新增 和 確定 以關閉選擇器。</span><span class="sxs-lookup"><span data-stu-id="37a37-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="37a37-197">然後，您需要也是預設的資料庫以及存取資料庫的權限設定。</span><span class="sxs-lookup"><span data-stu-id="37a37-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="37a37-198">若要設定的預設資料庫選擇從下拉式清單中，如下列 Northwind 選取：</span><span class="sxs-lookup"><span data-stu-id="37a37-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="37a37-199">接下來，按一下 [資料庫存取權] 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="37a37-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="37a37-200">按一下您想要允許存取的每個資料庫的允許核取方塊。</span><span class="sxs-lookup"><span data-stu-id="37a37-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="37a37-201">您也必須選擇資料庫角色檢查 db\_擁有者可確保您的登入具有所有必要權限管理，並使用選取的資料庫。</span><span class="sxs-lookup"><span data-stu-id="37a37-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="37a37-202">按一下 [確定] 結束 [屬性] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="37a37-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="37a37-203">ASP.NET 應用程式現在已設定為支援整合式的 SQL Server 驗證。</span><span class="sxs-lookup"><span data-stu-id="37a37-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="37a37-204">不在 IIS 6.0 原生模式下執行 ASP.NET 1.0</span><span class="sxs-lookup"><span data-stu-id="37a37-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="37a37-205">IIS 5 相容性模式中才支援 ASP.NET 1.0 IIS 6.0 上。</span><span class="sxs-lookup"><span data-stu-id="37a37-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="37a37-206">若要設定 IIS 5.0 相容性模式中執行 ASP.NET 1.0，開啟網際網路服務管理員，以滑鼠右鍵按一下 [網站] 並選取 [屬性]:</span><span class="sxs-lookup"><span data-stu-id="37a37-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="37a37-207">切換至 [服務] 索引標籤，然後檢查嗎？在 IIS 5.0 隔離模式中執行 WWW 服務？</span><span class="sxs-lookup"><span data-stu-id="37a37-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
