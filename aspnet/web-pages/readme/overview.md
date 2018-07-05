---
uid: web-pages/readme/overview
title: WebMatrix 讀我檔案 |Microsoft Docs
author: rick-anderson
description: WebMatrix 及 ASP.NET Web Pages (Razor) 1.0 版讀我檔案
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 924bf04772a1d73c7fdfb1168090daef388438ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398796"
---
<a name="webmatrix-readme"></a><span data-ttu-id="e7b9a-103">WebMatrix 讀我檔案</span><span class="sxs-lookup"><span data-stu-id="e7b9a-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="e7b9a-104">13 年 1 月 2011</span><span class="sxs-lookup"><span data-stu-id="e7b9a-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="e7b9a-105">內容</span><span class="sxs-lookup"><span data-stu-id="e7b9a-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="e7b9a-106">此讀我檔案適用於 WebMatrix 1.0 版。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="e7b9a-107">概觀</span><span class="sxs-lookup"><span data-stu-id="e7b9a-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="e7b9a-108">安裝</span><span class="sxs-lookup"><span data-stu-id="e7b9a-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="e7b9a-109">如何發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="e7b9a-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="e7b9a-110">變更與問題</span><span class="sxs-lookup"><span data-stu-id="e7b9a-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="e7b9a-111">WebMatrix 1.0 安裝</span><span class="sxs-lookup"><span data-stu-id="e7b9a-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="e7b9a-112">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="e7b9a-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="e7b9a-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e7b9a-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="e7b9a-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e7b9a-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="e7b9a-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="e7b9a-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="e7b9a-116">安裝應用程式</span><span class="sxs-lookup"><span data-stu-id="e7b9a-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="e7b9a-117">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="e7b9a-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="e7b9a-118">如需詳細資訊</span><span class="sxs-lookup"><span data-stu-id="e7b9a-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="e7b9a-119">總覽</span><span class="sxs-lookup"><span data-stu-id="e7b9a-119">Overview</span></span>

> <span data-ttu-id="e7b9a-120">Microsoft WebMatrix 1.0 是免費的 web 開發堆疊，以分鐘為單位進行安裝。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="e7b9a-121">它整合了 web 伺服器與資料庫和程式設計架構來建立單一的整合體驗。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="e7b9a-122">您可以使用 WebMatrix 來簡化的方式撰寫程式碼、 測試及發佈您自己的 ASP.NET 或 PHP 網站，或您可以使用 WebMatrix 來啟動新的網站使用熱門的開放原始碼應用程式，例如 DotNetNuke、 Umbraco、 WordPress 或 Joomla。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="e7b9a-123">WebMatrix 使用相同的功能強大的 web 伺服器、 資料庫引擎和架構環境，將會在網際網路上，讓從開發轉換到生產環境，順利且流暢地執行您的網站。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="e7b9a-124">安裝</span><span class="sxs-lookup"><span data-stu-id="e7b9a-124">Installation</span></span>

> <span data-ttu-id="e7b9a-125">若要安裝 WebMatrix 1.0，您必須先安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="e7b9a-126">您已安裝 Web Platform Installer 之後，您可以使用它來安裝 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="e7b9a-127">如果您在安裝期間遇到問題，請參閱[疑難排解 Microsoft Web Platform Installer 的問題](https://go.microsoft.com/fwlink/?LinkId=196212)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="e7b9a-128">如何發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="e7b9a-128">How to Publish Applications</span></span>

> <span data-ttu-id="e7b9a-129">請參閱[發行應用程式的逐步指示](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="e7b9a-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="e7b9a-130">變更與問題</span><span class="sxs-lookup"><span data-stu-id="e7b9a-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="e7b9a-131">WebMatrix 1.0 安裝問題</span><span class="sxs-lookup"><span data-stu-id="e7b9a-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="e7b9a-132">問題： WebMatrix 1.0 是只適用於支援 Microsoft.NET Framework 4 的平台</span><span class="sxs-lookup"><span data-stu-id="e7b9a-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="e7b9a-133">.NET Framework 第 4 版是 WebMatrix 的必要項目。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="e7b9a-134">在某些情況下，WebMatrix 1.0 安裝程式，也可讓您嘗試安裝不支援的組態集的一部分的平台上。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="e7b9a-135">特別是，Windows Vista SP1 更新不會讓您開始安裝 WebMatrix，但是.NET Framework 4 元件會失敗並封鎖您的安裝。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="e7b9a-136">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-136">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-137">安裝在支援的平台，包括：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="e7b9a-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="e7b9a-138">Windows 7</span></span>
> - <span data-ttu-id="e7b9a-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="e7b9a-139">Windows Server 2008</span></span>
> - <span data-ttu-id="e7b9a-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="e7b9a-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="e7b9a-141">Windows Vista SP1 (含) 以後版本</span><span class="sxs-lookup"><span data-stu-id="e7b9a-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="e7b9a-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="e7b9a-142">Windows XP SP3</span></span>
> - <span data-ttu-id="e7b9a-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="e7b9a-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="e7b9a-144">問題︰ 無法安裝 WebMatrix 1.0，如果未使用 Microsoft Visual Studio 2008 SP1 安裝 Microsoft Visual Studio 2008</span><span class="sxs-lookup"><span data-stu-id="e7b9a-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="e7b9a-145">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-145">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-146">安裝[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en)從 Microsoft 下載中心取得。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="e7b9a-147">問題： SQL Server Compact 4.0 的某些組件不會安裝在 GAC 中</span><span class="sxs-lookup"><span data-stu-id="e7b9a-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="e7b9a-148">當您在 64 位元電腦上安裝 SQL Server Compact 4.0，以及電腦只有.NET Framework 3.5 SP1 用戶端安裝設定檔時，SQL Server Compact 4.0 的 managed 組件不會放在全域組件快取 (GAC) 中。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="e7b9a-149">不會安裝在 GAC 中的 managed 組件包括：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="e7b9a-150">*System.Data.SqlServerCe.dll* (ado.net)</span><span class="sxs-lookup"><span data-stu-id="e7b9a-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="e7b9a-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="e7b9a-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="e7b9a-152">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-152">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-153">解除安裝 SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="e7b9a-154">下載並安裝完整版的.NET Framework 3.5 SP1，請從下列位置：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="e7b9a-155">Microsoft.NET Framework 3.5 Service pack 1 （完整套件）</span><span class="sxs-lookup"><span data-stu-id="e7b9a-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="e7b9a-156">然後重新安裝 SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="e7b9a-157">問題： 無法解除安裝 SQL Server Compact 使用命令列</span><span class="sxs-lookup"><span data-stu-id="e7b9a-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="e7b9a-158">解除安裝的 SQL Server Compact 使用命令列選項在此版本中無法運作。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="e7b9a-159">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-159">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-160">使用*程式和功能*Windows 控制台中解除安裝 Microsoft SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="e7b9a-161">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="e7b9a-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="e7b9a-162">本節的文件說明新功能、 變更和已知的問題的 1.0 版的 ASP.NET Web Pages 含有 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="e7b9a-163">新功能</span><span class="sxs-lookup"><span data-stu-id="e7b9a-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="e7b9a-164">變更</span><span class="sxs-lookup"><span data-stu-id="e7b9a-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="e7b9a-165">問題</span><span class="sxs-lookup"><span data-stu-id="e7b9a-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="e7b9a-166">新功能</span><span class="sxs-lookup"><span data-stu-id="e7b9a-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="e7b9a-167">若要停用封裝管理員加入組態設定的新功能：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="e7b9a-168">新`asp:AdminManagerEnabled`金鑰可供`<appSettings>`中的項目*web.config*檔案，可讓您完全停用封裝管理員。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="e7b9a-169">此項目的預設值為 true，也就是說，如果它不會納入*web.config*檔案，套件管理員已啟用。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="e7b9a-170">若要停用封裝管理員，請新增下列元素*web.config*網站的根目錄中的檔案：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="e7b9a-171">變更</span><span class="sxs-lookup"><span data-stu-id="e7b9a-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="e7b9a-172">重新命名為"asp: AdminFolderVirtualPath"的變更:"Webpages"索引鍵</span><span class="sxs-lookup"><span data-stu-id="e7b9a-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="e7b9a-173">`webPages:AdminFolderVirtualPath`可以加入的索引鍵*web.config*若要使用已重新命名檔案，以指定的封裝管理員位置`asp:`命名空間，而不是`webPages`命名空間。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="e7b9a-174">如果您已使用這個項目，您必須將它重新命名組態檔中。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="e7b9a-175">已知的問題</span><span class="sxs-lookup"><span data-stu-id="e7b9a-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="e7b9a-176">問題： 無法再辨識成員資格使用者的密碼</span><span class="sxs-lookup"><span data-stu-id="e7b9a-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="e7b9a-177">建立及儲存成員資格 （登入） 密碼的演算法已變更為更安全。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="e7b9a-178">如此一來，將無法辨識針對 ASP.NET Razor Beta 版中建立的成員 （使用者） 儲存的密碼。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="e7b9a-179">**因應措施**如果網站尚未放到生產環境，從 成員資格資料庫中移除使用者記錄。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="e7b9a-180">如果資料庫已上線，以程式設計方式重新產生現有成員資格資料庫中的密碼。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="e7b9a-181">問題： 未預期的行為時使用的自訂使用者資料表的成員資格</span><span class="sxs-lookup"><span data-stu-id="e7b9a-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="e7b9a-182">若要初始化 ASP.NET Razor 網站的成員資格提供者，請呼叫`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="e7b9a-183">(在 WebMatrix 中，入門網站範本會包含在這個方法呼叫 *\_AppStart.cshtml*檔案。)如果`autoCreateTables`這個方法的參數設定為 true (根據預設，它設定為在入門網站範本，則為 true)，以及無法辨識的資料表名稱傳遞至方法 （第二個參數），如果方法沒有擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="e7b9a-184">相反地，它會自動建立資料表。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="e7b9a-185">如果您想要使用自訂使用者資料表的成員資格，但傳遞至錯誤的資料表名稱，這可能會有問題`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="e7b9a-186">因為方法不會預設引發錯誤如果您指定的資料表不存在，而且它改為建立新的資料表，應用程式可以在運作中。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="e7b9a-187">不過，依賴您的自訂使用者資料表 （和中的欄位） 的應用程式程式碼最後可能發生未預期的錯誤會失敗。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="e7b9a-188">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-188">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-189">請確定名稱傳入`InitializeDatabaseConnection`方法比對的使用者設定檔資料表中的成員資格資料庫中，或請確定`autoCreateTables`參數設定為 false。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="e7b9a-190">問題： 錯誤訊息 「 Admin 模組必須以 ~/App\_資料 」</span><span class="sxs-lookup"><span data-stu-id="e7b9a-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="e7b9a-191">在某些情況下，嘗試建立使用者，或使用 ASP.NET 成員資格系統可能會造成顯示錯誤頁面*Admin 模組必須存取 ~/App\_資料*。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="e7b9a-192">發生此情況下執行 IIS 或 IIS Express 的帳戶沒有建立和寫入權限*應用程式\_資料*在網站根目錄底下的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="e7b9a-193">**因應措施**手動建立*應用程式\_資料*網站的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="e7b9a-194">請確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有讀取/寫入權限的應用程式的根資料夾以及子資料夾，例如應用程式\_資料。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="e7b9a-195">知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="e7b9a-196">問題: 「 無法產生 SQL Server 的使用者執行個體 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e7b9a-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="e7b9a-197">如果 WebMatrix Web 應用程式會使用 SQL Server Express，而且在 Windows 7 或 Windows Server 2008 R2 上執行 IIS 7.5，您可能會看到錯誤指出 SQL Server 無法在執行階段擷取使用者的本機應用程式路徑。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="e7b9a-198">**因應措施**確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有這類的應用程式根資料夾以及子資料夾的讀取/寫入權限*應用程式\_資料*.</span><span class="sxs-lookup"><span data-stu-id="e7b9a-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="e7b9a-199">知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="e7b9a-200">問題： 包含封裝管理員資源或封裝管理員密碼的檔案是可提供在 IIS 6.0 和更早版本</span><span class="sxs-lookup"><span data-stu-id="e7b9a-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="e7b9a-201">如果您部署使用 RC2 版本中，所建置的 ASP.NET Web Pages (Razor) 應用程式，並包含應用程式*password.txt*或是*packagesources.txt*檔案下 */App\_資料/admin*，IIS 6.0 會提供此檔案要求，可能會公開您的封裝管理員執行個體的密碼。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="e7b9a-202">**因應措施**重新命名*password.txt*或是*packagesources.txt*檔案*password.config*或*packagesources.config*.根據預設，IIS 6.0 不會不會提供檔案具有 *.config*延伸模組。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="e7b9a-203">(在 IIS 7 中沒有任何檔案*應用程式\_資料*提供資料夾，因此您不需要重新命名檔案。)</span><span class="sxs-lookup"><span data-stu-id="e7b9a-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="e7b9a-204">問題： 解除安裝使用 Beta 3 版本安裝的套件不會完全移除封裝元件</span><span class="sxs-lookup"><span data-stu-id="e7b9a-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="e7b9a-205">如果您已安裝的套件，使用 package manager Beta 3 版本中，然後再次嘗試將它使用目前版本解除安裝套件未完全解除安裝。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="e7b9a-206">使用套件管理員**解除安裝** 按鈕移除某些元件，但會保留封裝的程式庫程式碼，而且不會更新*package.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="e7b9a-207">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="e7b9a-207">**Workaround** </span></span>  
> <span data-ttu-id="e7b9a-208">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="e7b9a-209">刪除*應用程式\_Data\packages*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="e7b9a-210">這會移除所有封裝。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="e7b9a-211">刪除*packages.config*網站的根目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="e7b9a-212">問題： 在 Visual Studio 中，叫用 web 架構封裝管理員會讓應用程式離線</span><span class="sxs-lookup"><span data-stu-id="e7b9a-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="e7b9a-213">如果您使用 Visual Studio (而非 WebMatrix) 中，並且使用 *\_admin*啟動套件管理員]，[Visual Studio 的功能會讓應用程式離線，並將張貼*應用程式\_offline.htm*貼入網站根目錄，這會中斷您使用封裝管理員的能力。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="e7b9a-214">如果您新增、 移除或修改任何檔案，雖然使用 web 架構封裝管理員介面時，通常會看到此行為，會發生相同的行為*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="e7b9a-215">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="e7b9a-215">**Workaround** </span></span>  
> <span data-ttu-id="e7b9a-216">若要使用 Visual Studio 中的封裝，使用 NuGet 延伸模組而非 web 架構封裝管理員。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="e7b9a-217">如需資訊，請參閱[NuGet 文件](https://docs.microsoft.com/nuget/)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="e7b9a-218">如果您正在使用中的其他檔案*應用程式\_資料*資料夾，請考慮將檔案保留要避免這個問題的其他位置。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="e7b9a-219">如果這不可行，刪除*應用程式\_offline.htm*手動檔案，或等候直到網站自動 （根據預設，在 30 秒後） 恢復上線。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="e7b9a-220">問題： Visual Studio IntelliSense 和專案範本僅適用於 ASP.NET MVC 3 版</span><span class="sxs-lookup"><span data-stu-id="e7b9a-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="e7b9a-221">安裝 ASP.NET Web Pages 不會也安裝 tools for Visual Studio ASP.NET Web Pages 應用程式的 IntelliSense 和專案範本等。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="e7b9a-222">**因應措施**若要使用 IntelliSense 和專案範本在 Visual Studio 中的 ASP.NET Web Pages 應用程式，安裝 ASP.NET MVC 3 RC 透過 Web Platform Installer 或[獨立安裝](https://go.microsoft.com/fwlink/?LinkID=191797)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="e7b9a-223">問題： 讀取摘要或其他外部的資料，透過 proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="e7b9a-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="e7b9a-224">如果執行站台的伺服器是在 proxy 伺服器後方，您可能需要設定中的 proxy 資訊*web.config*檔案以讀取來自網站外部的資訊。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="e7b9a-225">例如，如果您使用`ReCaptcha`協助程式，協助專家會與 reCAPTCHA 服務通訊，但可能會封鎖您的 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="e7b9a-226">同樣地，使用 ASP.NET Web Pages 中，例如套件管理員 中，所使用的摘要的摘要可能需要 proxy 組態。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="e7b9a-227">如果您遇到問題，使用外部服務，或使用套件摘要中的，將下列項目放到您的應用程式根目錄*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="e7b9a-228">如需有關如何設定 proxy 伺服器的詳細資訊，請參閱 < [ &lt;proxy&gt;項目 （網路設定）](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="e7b9a-229">問題： 解除安裝.NET Framework 第 4 版會停用含有 Razor 語法的 ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="e7b9a-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="e7b9a-230">如果您解除安裝.NET Framework 第 4 版，然後重新安裝時，會停用含有 Razor 語法的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="e7b9a-231">頁面 *.cshtml*延伸模組無法正確執行。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="e7b9a-232">ASP.NET 網頁註冊組件在電腦根目錄*web.config*檔案，並移除.NET Framework 中移除該檔案。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="e7b9a-233">重新安裝.NET Framework 會安裝新版本的組態檔中，但不會新增 ASP.NET Web Pages 組件的參考。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="e7b9a-234">**因應措施**之後重新安裝.NET Framework，請重新安裝 ASP.NET Web Pages 含有 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="e7b9a-235">這會將新增到下列項目*web.config*電腦根目錄，這通常是在下列位置中的檔案：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="e7b9a-236">問題： 無副檔名 Url 找.cshtml/.vbhtml 檔案，在 IIS 7 或 IIS 7.5 上的不到</span><span class="sxs-lookup"><span data-stu-id="e7b9a-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="e7b9a-237">在 IIS 7 或 IIS 7.5 上，要求如下所示的 url 不能找到網頁具有 *.cshtml*或是 *.vbhtml*延伸模組：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="e7b9a-238">因為 URL 重寫未啟用預設的 IIS 7 或 IIS 7.5，就會發生問題。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="e7b9a-239">Likeliest 案例是看不到問題時測試在本機上使用 IIS Express，但它時遇到您將您的網站部署至裝載的網站。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="e7b9a-240">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="e7b9a-241">如果您有伺服器電腦的控制權時，在伺服器電腦上安裝更新中所述[的更新功能，可讓特定 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="e7b9a-242">如果您沒有在伺服器電腦的控制權 （例如，您要部署到裝載的網站），加上網站的下列*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="e7b9a-243">問題： 應用程式部署至沒有 SQL Server Compact 安裝的電腦</span><span class="sxs-lookup"><span data-stu-id="e7b9a-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="e7b9a-244">包含 SQL Server Compact 資料庫的應用程式可以執行 SQL Server Compact 不安裝所在的電腦上。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="e7b9a-245">Microsoft WebMatrix 1.0 自動為您複製這些二進位檔，並執行適當*web.config*檔案轉換。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="e7b9a-246">**因應措施**如果您需要複製這些檔案，並使*web.config*檔案變更以手動的方式，執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="e7b9a-247">資料庫引擎組件，以複製*Bin*目標電腦上的應用程式的資料夾 （和子資料夾）：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="e7b9a-248">複製*C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="e7b9a-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="e7b9a-249">**to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="e7b9a-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="e7b9a-250">複製<em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>來</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="e7b9a-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="e7b9a-251">複製<em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>來</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="e7b9a-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="e7b9a-252">在網站的根資料夾中，建立或開啟*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="e7b9a-253">(在 WebMatrix 1.0，這種檔案類型是可用，如果您按一下**所有**中**選擇 [檔案類型**] 對話方塊。)</span><span class="sxs-lookup"><span data-stu-id="e7b9a-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="e7b9a-254">將下列項目新增為子系`<configuration>`項目 (不是在內`<system.web>`項目):</span><span class="sxs-lookup"><span data-stu-id="e7b9a-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="e7b9a-255">問題: 「 資料庫 」 和 「 WebGrid 」 的協助程式不適用於在 Visual Basic 中的中度信任</span><span class="sxs-lookup"><span data-stu-id="e7b9a-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="e7b9a-256">如果您使用 Visual Basic (建立 *.vbhtml*檔案)，則`Database`和`WebGrid`如果應用程式會設定為使用中度信任，協助程式將無法運作。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="e7b9a-257">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-257">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-258">如果您使用 Visual Studio 2010，您可以透過安裝 Service Pack 1 版本中解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="e7b9a-259">SP1 版本的最終版本可用之前，您可以下載從 SP1 的 Beta 版[Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft Download Center 上的頁面。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="e7b9a-260">如果這不是很實用，或如果您不使用 Visual Studio 2010，您可以暫時設定應用程式使用完全信任。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="e7b9a-261">問題: 「 ApplicationPart 」 資源是從外部存取</span><span class="sxs-lookup"><span data-stu-id="e7b9a-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="e7b9a-262">如果組件包含的物件，衍生自`ApplicationPart`類別，組件的資源由`ResourceRouteHandler`類別。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="e7b9a-263">例如，請考慮下列 URL：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="e7b9a-264">此要求下載中的資源字串的所有*System.Web.WebPages.Administration.dll*組件。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="e7b9a-265">下載的所有內嵌資源 （即使不打算提供做為靜態內容）。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="e7b9a-266">如果內嵌的資源包含機密資訊，這可以代表安全性風險。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="e7b9a-267">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="e7b9a-267">**Workaround** </span></span>  
> <span data-ttu-id="e7b9a-268">如果您建立**ApplicationPart**物件，請確認與它相關的內嵌的資源**ApplicationPart**物件的組件不包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="e7b9a-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e7b9a-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="e7b9a-270">WebMatrix 針對安裝問題的相關資訊，請參閱[WebMatrix 安裝問題](#Known_Issues_Installation)稍早在這份文件。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="e7b9a-271">本節的文件說明 WebMatrix 開發環境的已知的的問題。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="e7b9a-272">問題： 使用者名稱或密碼的 web.config 檔案中的資料庫連接字串中的變更不會反映在 [資料庫] 工作區</span><span class="sxs-lookup"><span data-stu-id="e7b9a-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="e7b9a-273">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="e7b9a-274">在  *web.config*檔案中，變更連接字串中的資料庫名稱 （例如，加入"1"，）。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="e7b9a-275">儲存*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="e7b9a-276">按一下 **資料庫**並重新整理。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="e7b9a-277">變更中的連接字串中的資料庫名稱*web.config*回到原始的資料庫名稱的檔案。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="e7b9a-278">儲存*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="e7b9a-279">按一下 **資料庫**並重新整理。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="e7b9a-280">問題： 無法刪除 WebMatrix 所建立的資料夾</span><span class="sxs-lookup"><span data-stu-id="e7b9a-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="e7b9a-281">如果執行 WebMatrix，使用提高的權限 (也就是您開始使用 WebMatrix**系統管理員身分執行**在 Windows 中的選項)，WebMatrix 所建立的資料夾無法使用 Windows 檔案總管來刪除。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="e7b9a-282">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-282">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-283">執行使用提高的權限的 Windows 檔案總管。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="e7b9a-284">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="e7b9a-285">在 Windows 中，按一下**啟動**。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="e7b9a-286">輸入 「 Windows 檔案總管 」，然後以滑鼠右鍵按一下的項目**Windows 檔案總管**。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="e7b9a-287">按一下 **系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="e7b9a-288">此外，您就可以刪除資料夾。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="e7b9a-289">問題： WebMatrix 1.0 是無法執行需要提高權限的特定工作</span><span class="sxs-lookup"><span data-stu-id="e7b9a-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="e7b9a-290">WebMatrix 1.0 是無法執行需要提高權限，例如在下列情況下安裝其他元件的特定工作項目：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="e7b9a-291">在 Windows Vista 或 Windows 7 上，您登入不具系統管理權限的帳戶，並已停用使用者帳戶控制 (UAC)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="e7b9a-292">您使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="e7b9a-293">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-293">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-294">WebMatrix 1.0 中的大部分工作不需要系統管理權限。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="e7b9a-295">對於這些，您可以執行此作業，身為管理員，或遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="e7b9a-296">在 Windows Vista 或 Windows 7 上，啟用 UAC。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="e7b9a-297">在 Windows XP 上加入 Administrators 安全性群組中的使用者。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="e7b9a-298">問題: 「 站台從 Web 組件庫 「 已停用</span><span class="sxs-lookup"><span data-stu-id="e7b9a-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="e7b9a-299">**從 Web 組件庫網站**選項會停用，如果未安裝 Web Platform Installer 3.0。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="e7b9a-300">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-300">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-301">安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="e7b9a-302">問題： Google Chrome 不提供執行選項</span><span class="sxs-lookup"><span data-stu-id="e7b9a-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="e7b9a-303">Google Chrome 不會顯示在清單下方的瀏覽器**執行**上**首頁** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="e7b9a-304">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-304">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-305">Google Chrome 的某些版本中請勿自行正確向註冊在 Windows 中的預設程式 功能。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="e7b9a-306">因應措施，請啟動 Google Chrome，請按一下*自訂和控制 Google Chrome*功能表上，按一下*選項*，然後按一下*讓 Google Chrome 我的預設瀏覽器*。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="e7b9a-307">問題: 「 外部索引鍵 」 對話方塊中不允許輸入主索引鍵</span><span class="sxs-lookup"><span data-stu-id="e7b9a-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="e7b9a-308">**外部索引鍵**對話方塊不允許您輸入主索引鍵的資料表主索引鍵的名稱。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="e7b9a-309">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-309">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-310">這是刻意設計的。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-310">This is intentional.</span></span> <span data-ttu-id="e7b9a-311">您不需要輸入主索引鍵資料表的主索引鍵的名稱。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="e7b9a-312">問題： IntelliSense 不適用於 WebMatrix 的 Razor 語法、 C# 或 Visual Basic</span><span class="sxs-lookup"><span data-stu-id="e7b9a-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="e7b9a-313">WebMatrix 支援 IntelliSense 的 HTML 和 CSS。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="e7b9a-314">不過，它不適用於其他語言。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="e7b9a-315">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="e7b9a-315">**Workaround** </span></span>  
> <span data-ttu-id="e7b9a-316">無。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="e7b9a-317">問題： 適用於 HTML 和 CSS IntelliSense 建議不符合當前項目</span><span class="sxs-lookup"><span data-stu-id="e7b9a-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="e7b9a-318">在 WebMatrix 中標記的 IntelliSense 支援使用 HTML [XHTML 1.0 Transitional 的結構描述](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional)CSS，並用[CSS 2.1 結構描述](http://www.w3.org/TR/CSS2/)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="e7b9a-319">IntelliSense 會根據這些特定的結構描述，因為特定的標記、 屬性可能會建議不適合目前的頁面或樣式定義。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="e7b9a-320">Html，它可能也會導致非預期的建議，可能會被解譯為格式不正確的 XHTML，（例如，當標記未關閉） 的內容中。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="e7b9a-321">此問題可能會更明顯，如果插入點位於不完整的標記;在此情況下，IntelliSense 可能會建議新開啟的索引標籤，或提供其他錯誤的建議。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="e7b9a-322">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="e7b9a-322">**Workaround** </span></span>  
> <span data-ttu-id="e7b9a-323">HTML 中，請確定您正在語式正確且完整的 XHTML 頁面內。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="e7b9a-324">Css，沒有任何因應措施。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="e7b9a-325">問題： IntelliSense 不會叫用時您所輸入</span><span class="sxs-lookup"><span data-stu-id="e7b9a-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="e7b9a-326">有些時候，IntelliSense 可能不會叫用，在編輯器中輸入 HTML 或 CSS。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="e7b9a-327">特別是，這可能會發生插入點位於另一個項目旁邊的直接或檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="e7b9a-328">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="e7b9a-328">**Workaround** </span></span>  
> <span data-ttu-id="e7b9a-329">請確定是插入點周圍的空白字元，並將插入點不在檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="e7b9a-330">您也可以藉由按下 Ctrl + 空格鍵以手動方式啟動 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="e7b9a-331">問題： 沒有 UI 可供停用 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="e7b9a-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="e7b9a-332">WebMatrix 1.0 提供停用 IntelliSense 的任何 UI 或軌跡。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="e7b9a-333">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="e7b9a-333">**Workaround** </span></span>  
> <span data-ttu-id="e7b9a-334">啟動 WebMatrix 使用下列命令，其中包括停用 IntelliSense 的交換器：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="e7b9a-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e7b9a-335">IIS Express</span></span>

<span data-ttu-id="e7b9a-336">IIS Express 有自己的讀我檔案，將會位於下列 URL:</span><span class="sxs-lookup"><span data-stu-id="e7b9a-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="e7b9a-337">https://go.microsoft.com/fwlink/?LinkID=207675&ampclcid = 0x409</span><span class="sxs-lookup"><span data-stu-id="e7b9a-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="e7b9a-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="e7b9a-338">SQL Server Compact</span></span>

<span data-ttu-id="e7b9a-339">SQL Server Compact 有自己的讀我檔案，將會位於下列 URL:</span><span class="sxs-lookup"><span data-stu-id="e7b9a-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="e7b9a-340">如需涉及 WebMatrix 的過程中安裝 SQL Server Compact 的問題的資訊，請參閱[WebMatrix 安裝問題](#Known_Issues_Installation)稍早在這份文件。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="e7b9a-341">安裝應用程式</span><span class="sxs-lookup"><span data-stu-id="e7b9a-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="e7b9a-342">問題： 安裝應用程式可能需要很長的時間如果使用者的 [我的文件] 資料夾重新導向至網路共用</span><span class="sxs-lookup"><span data-stu-id="e7b9a-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="e7b9a-343">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-343">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-344">無。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-344">None.</span></span> <span data-ttu-id="e7b9a-345">應用程式可能需要一段時間，若要安裝，但會正確安裝。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="e7b9a-346">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="e7b9a-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="e7b9a-347">問題: 「 要求無法取得權限 」 的錯誤時發佈 SQL Compact 資料庫</span><span class="sxs-lookup"><span data-stu-id="e7b9a-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="e7b9a-348">WebMatrix 不完全支援 SQL Server Compact 的部署支援的二進位檔，以中度信任組態中執行.NET Framework 3.5 版的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="e7b9a-349">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-349">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-350">慣用的解決方法是在伺服器上安裝.NET Framework 4。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="e7b9a-351">或者，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="e7b9a-352">加入下列項目，來`SecurityClasses`一節*Web\_MediumTrust.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="e7b9a-353">建立新的權限，在中設定*Web\_MediumTrust.config*下列必要的權限的檔案：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="e7b9a-354">套用權限，藉由將下列項目放在設定為 SQL Server Compact *Web\_MediumTrust.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="e7b9a-355">問題： 組件庫和 PhpBB web 應用程式顯示 「 服務無法使用 」 錯誤發行之後</span><span class="sxs-lookup"><span data-stu-id="e7b9a-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="e7b9a-356">在某些情況下，發行應用程式，將會導致 「 服務無法使用 」 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="e7b9a-357">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-357">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-358">在 WebMatrix 中，新增反斜線 (\)中的伺服器名稱的結尾**發佈設定**視窗並再發佈一次的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="e7b9a-359">問題： Moodle 網站配置和連結已中斷之後發行</span><span class="sxs-lookup"><span data-stu-id="e7b9a-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="e7b9a-360">在您發行的 Moodle 應用程式之後，應用程式無法正常運作。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="e7b9a-361">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-361">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-362">在 WebMatrix 中，將斜線 （/） 新增至結尾**站台名稱**欄位中**發佈設定**視窗並再發佈一次的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="e7b9a-363">問題： 發行 nopCommerce 資料庫錯誤而失敗</span><span class="sxs-lookup"><span data-stu-id="e7b9a-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="e7b9a-364">發行 nopCommerce 失敗並報告資料庫錯誤，例如 「 插入 nop\_記錄資料表失敗。 」</span><span class="sxs-lookup"><span data-stu-id="e7b9a-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="e7b9a-365">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="e7b9a-366">在 WebMatrix 中，按一下**執行**啟動 nopCommerce 在本機。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="e7b9a-367">登入的管理頁面。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="e7b9a-368">按一下 **系統**功能表。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="e7b9a-369">按一下 **記錄**選項。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="e7b9a-370">按一下 [**清除記錄檔**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="e7b9a-371">重新發佈 nopCommerce。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="e7b9a-372">問題： Silverstripe CMS 會顯示 「 HTTP 500 PHP FCGI 錯誤 」 時下載已發佈的網站</span><span class="sxs-lookup"><span data-stu-id="e7b9a-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="e7b9a-373">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-373">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-374">按一下 之後**下載已發佈的站台**，請略過`silverstripe-cache/manifest_main`中**發佈預覽**。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="e7b9a-375">這個檔案用於快取，且每一部電腦的特定。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="e7b9a-376">問題： 子文字會顯示 「 伺服器錯誤 '/' 應用程式中 」 當您下載已發佈的網站</span><span class="sxs-lookup"><span data-stu-id="e7b9a-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="e7b9a-377">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-377">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-378">開啟 站台*web.config*檔案，並將使用者識別碼和密碼的資料庫連接字串中使用 SQL Server 系統管理員認證 （"sa"認證）。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="e7b9a-379">或者，遵循這些步驟，可讓您登入的使用者帳戶`db_owner`權限：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="e7b9a-380">安裝 SQL Server Management Studio 中使用 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="e7b9a-381">連接到本機的 SQL Server Express 執行個體 (根據預設， `.\SQLEXPRESS`)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="e7b9a-382">按一下 **資料庫** &gt; *[localSubtextDatabase]* &gt; **安全性** &gt; **使用者**&gt; *[localSubtextUser*] (預設值是`subtextuser`]，按一下滑鼠右鍵，然後按一下 [**屬性**。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="e7b9a-383">選取 [ **db\_擁有者**中角色的成員資格] 區段中。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="e7b9a-384">問題： 站台後可能無法運作如果 [目的地 URL] 欄位不以 http:// 或 https:// 前置發行</span><span class="sxs-lookup"><span data-stu-id="e7b9a-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="e7b9a-385">在 [**發佈設定**] 對話方塊中，如果目的地 URL 開頭不是`http://`或`https://`，部署後，網站可能無法運作。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="e7b9a-386">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-386">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-387">請確定發行的網站中的目的地 URL 之前，先**發佈設定** 對話方塊中的開頭`http://`或`https://`。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="e7b9a-388">問題： 發行的 MySQL 資料庫失敗並出現錯誤 「 無法發行資料庫。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="e7b9a-389">這就可能發生遠端資料庫無法執行指令碼。 」</span><span class="sxs-lookup"><span data-stu-id="e7b9a-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="e7b9a-390">有許多原因可能會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="e7b9a-391">您可以看到此錯誤的其中一個原因是如果將資料庫指令碼包含單引號字元 （'），而且目的地 MySQL 資料庫的預設字元集不為 utf-8。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="e7b9a-392">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-392">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-393">設定預設遠端 MySQL 資料庫設定為 utf-8 的字元。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="e7b9a-394">問題： 部分連結不會顯示在 DotNetNuke 之後發佈或下載網站</span><span class="sxs-lookup"><span data-stu-id="e7b9a-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="e7b9a-395">如果您發行或下載 DotNetNuke 網站時，您可能需要清除快取，以取得新的連結，才會出現在網站上。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="e7b9a-396">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="e7b9a-397">「 主機 」 身分登入。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="e7b9a-398">請移至 [主機] 功能表，然後選取**主控件設定**。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="e7b9a-399">向下的捲動底下**進階設定**，展開**效能設定**。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="e7b9a-400">按一下 **清除快取**頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="e7b9a-401">請移至頁面底部，並重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="e7b9a-402">問題： 某些連結 AtomSite 中的不能用在您下載已發佈的網站之後，</span><span class="sxs-lookup"><span data-stu-id="e7b9a-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="e7b9a-403">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-403">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-404">在  *service.config*檔案， *users.config*檔案，以及所有 *.xml*檔案，取代 URL 字串 (比方說， `http://myhost.com/atomsite`) 以本機 (比方說， `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="e7b9a-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="e7b9a-405">發佈和報表資料庫錯誤的問題： WordPress 這類以 MySQL 為基礎的應用程式失敗</span><span class="sxs-lookup"><span data-stu-id="e7b9a-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="e7b9a-406">根據預設，WebMatrix 會使用 utf-8 字元集安裝 MySQL。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="e7b9a-407">如果您自行安裝 MySQL，並將字元集不是 utf-8 （比方說，它是 Latin1），資料庫的發佈程序可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="e7b9a-408">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="e7b9a-409">變更 MySQL 為 utf-8 的字元集。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="e7b9a-410">(如需詳細資訊，請參閱 <<c0> [ 伺服器字元集和定序](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html)MySQL 網站上。)</span><span class="sxs-lookup"><span data-stu-id="e7b9a-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="e7b9a-411">重新安裝應用程式。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="e7b9a-412">重新發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="e7b9a-413">問題: 「 下載發佈站台 」 的應用程式，讓瀏覽器為基礎的安裝程式失敗</span><span class="sxs-lookup"><span data-stu-id="e7b9a-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="e7b9a-414">有些應用程式 (例如 Kentico CMS) 會要求您在瀏覽器中啟動以執行後續安裝設定，例如建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="e7b9a-415">如果您發行這類的應用程式，而不會完成瀏覽器為基礎的安裝程式時，嘗試從遠端伺服器下載相同的站台將會失敗。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="e7b9a-416">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-416">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-417">發佈站台之前先完成瀏覽器為基礎的安裝。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="e7b9a-418">問題: 「 下載發佈站台 」 錯誤而失敗資料庫 DotNetNuke 和 Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="e7b9a-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="e7b9a-419">如果您嘗試從伺服器下載應用程式，而且您必須在資料庫連接字串中的系統管理員認證**發佈設定** 對話方塊中，您可能會看到下列錯誤，發行記錄檔中：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="e7b9a-420">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="e7b9a-420">**Workaround**</span></span>  
> <span data-ttu-id="e7b9a-421">如果可行的話，重新發行網站 （或將它發行） 資料庫使用非系統管理員認證。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="e7b9a-422">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="e7b9a-422">For More Information</span></span>

<span data-ttu-id="e7b9a-423">如需有關 WebMatrix 1.0 的詳細資訊，請參閱下列網站：</span><span class="sxs-lookup"><span data-stu-id="e7b9a-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="e7b9a-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="e7b9a-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="e7b9a-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e7b9a-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="e7b9a-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="e7b9a-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="e7b9a-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="e7b9a-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="e7b9a-428">All Rights Reserved.</span><span class="sxs-lookup"><span data-stu-id="e7b9a-428">All Rights Reserved.</span></span> <span data-ttu-id="e7b9a-429">[使用規定](https://msdn.microsoft.cos/cc300389.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e7b9a-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
