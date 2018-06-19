---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET 拒絕存取 IIS 目錄 |Microsoft 文件
author: rick-anderson
description: 本白皮書描述所需的動作如果 ASP.NET 應用程式的要求會傳回錯誤，也就是 「 拒絕存取 DirectoryName 目錄。 無法為 s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30070767"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="db364-104">ASP.NET 拒絕 IIS 目錄的存取</span><span class="sxs-lookup"><span data-stu-id="db364-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="db364-105">本白皮書說明若您的 ASP.NET 應用程式的要求會傳回錯誤，您須執行的作業 「 拒絕存取*DirectoryName*目錄。</span><span class="sxs-lookup"><span data-stu-id="db364-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="db364-106">若要開始監視目錄變更失敗。 」</span><span class="sxs-lookup"><span data-stu-id="db364-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="db364-107">適用於 ASP.NET 1.0 和 1.1 的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="db364-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="db364-108">ASP.NET V1 RTM 現在執行時使用較小權限的 windows 帳戶-註冊為本機電腦上的"ASPNET 」 帳戶。</span><span class="sxs-lookup"><span data-stu-id="db364-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="db364-109">在某些系統已鎖定，此帳戶可能並非預設讀取的網站內容目錄、 應用程式的根目錄，或網站根目錄的安全性存取權。</span><span class="sxs-lookup"><span data-stu-id="db364-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="db364-110">在此情況下您會在從給定的 web 應用程式要求頁面時，收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="db364-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="db364-111">若要修正此問題，您必須變更適當的目錄的安全性權限。</span><span class="sxs-lookup"><span data-stu-id="db364-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="db364-112">具體而言，ASP.NET 需要讀取、 執行和清單的網站根目錄的 ASPNET 帳戶存取權 (例如： c:\inetpub\wwwroot 或可能會在 IIS 中設定任何其他站台目錄)，內容的目錄和應用程式根目錄若要監視的組態檔變更。</span><span class="sxs-lookup"><span data-stu-id="db364-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="db364-113">應用程式根目錄對應至 IIS 系統管理工具 (inetmgr) 中的應用程式虛擬目錄相關聯的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="db364-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="db364-114">例如，請考慮下列應用程式階層下的 wwwroot 資料夾。</span><span class="sxs-lookup"><span data-stu-id="db364-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="db364-115">例如，ASPNET 帳戶需要以上定義的 myapp 和 wwwroot 目錄內容的讀取權限。</span><span class="sxs-lookup"><span data-stu-id="db364-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="db364-116">根資料夾上的單一繼承的 ACL 也會選擇性地用於兩個目錄如果它們巢狀。</span><span class="sxs-lookup"><span data-stu-id="db364-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="db364-117">若要加入目錄的權限，執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="db364-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="db364-118">使用 Windows 檔案總管，瀏覽至目錄</span><span class="sxs-lookup"><span data-stu-id="db364-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="db364-119">目錄資料夾上按一下滑鼠右鍵，然後選擇 [屬性]</span><span class="sxs-lookup"><span data-stu-id="db364-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="db364-120">瀏覽至 [屬性] 對話方塊的 [安全性] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="db364-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="db364-121">按一下 [新增] 按鈕並輸入電腦名稱，後面接著 ASPNET 帳戶名稱。</span><span class="sxs-lookup"><span data-stu-id="db364-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="db364-122">比方說，在電腦上名為"webdev 」，您會輸入 webdev\ASPNET，然後點擊 [確定]。</span><span class="sxs-lookup"><span data-stu-id="db364-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="db364-123">請確認 ASPNET 帳戶具有 「 讀取&amp;Execute"，「 列出資料夾內容 」 和 「 讀取 」 核取的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="db364-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="db364-124">點擊 [確定] 以關閉對話方塊並儲存變更。</span><span class="sxs-lookup"><span data-stu-id="db364-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="db364-125">如有需要，可以使用指令碼或隨附的 「 cacls.exe 」 工具與 Windows 自動化這些變更。</span><span class="sxs-lookup"><span data-stu-id="db364-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="db364-126">如需有關 ASPNET 帳戶的詳細資訊，請參閱[常見問題集文件](https://go.microsoft.com/fwlink/?LinkId=5828)。</span><span class="sxs-lookup"><span data-stu-id="db364-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="db364-127">如果給定的 web 應用程式依賴具有寫入或修改特定資料夾或檔案的權限，這可以授與相同的程序，並檢查 「 寫入 」 和/或 「 修改 」 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="db364-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="db364-128">在電腦上，可以讓每個人或使用者群組讀取權限，這些目錄 （這是預設組態），會發生任何問題，並不需要上述步驟。</span><span class="sxs-lookup"><span data-stu-id="db364-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
