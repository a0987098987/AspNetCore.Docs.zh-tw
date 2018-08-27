---
uid: web-pages/readme/beta3
title: Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案 |Microsoft Docs
author: rick-anderson
description: Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 3d729d1b0615533dddceff484acb3d42247f6cab
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831407"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="a184a-103">Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案</span><span class="sxs-lookup"><span data-stu-id="a184a-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="a184a-104">Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案</span><span class="sxs-lookup"><span data-stu-id="a184a-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="a184a-105">9 年 11 月 2010</span><span class="sxs-lookup"><span data-stu-id="a184a-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="a184a-106">內容</span><span class="sxs-lookup"><span data-stu-id="a184a-106">Contents</span></span>

- [<span data-ttu-id="a184a-107">概觀</span><span class="sxs-lookup"><span data-stu-id="a184a-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="a184a-108">安裝</span><span class="sxs-lookup"><span data-stu-id="a184a-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="a184a-109">新的功能、 變更和 Beta 3 版本的已知問題</span><span class="sxs-lookup"><span data-stu-id="a184a-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="a184a-110">WebMatrix 安裝問題</span><span class="sxs-lookup"><span data-stu-id="a184a-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="a184a-111">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="a184a-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="a184a-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="a184a-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="a184a-113">安裝應用程式</span><span class="sxs-lookup"><span data-stu-id="a184a-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="a184a-114">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="a184a-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="a184a-115">其他問題</span><span class="sxs-lookup"><span data-stu-id="a184a-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="a184a-116">如需詳細資訊</span><span class="sxs-lookup"><span data-stu-id="a184a-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="a184a-117">總覽</span><span class="sxs-lookup"><span data-stu-id="a184a-117">Overview</span></span>

> <span data-ttu-id="a184a-118">Microsoft WebMatrix beta 版是免費的 web 開發堆疊，以分鐘為單位的安裝。</span><span class="sxs-lookup"><span data-stu-id="a184a-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="a184a-119">它整合了 web 伺服器與資料庫和程式設計架構來建立單一的整合體驗。</span><span class="sxs-lookup"><span data-stu-id="a184a-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="a184a-120">您可以使用 WebMatrix Beta 來簡化的方式撰寫程式碼、 測試及發佈您自己的 ASP.NET 或 PHP 網站，或您可以使用 WebMatrix beta 版來開始使用 DotNetNuke、 Umbraco、 WordPress 或 Joomla 等熱門的開放原始碼應用程式建立新網站。</span><span class="sxs-lookup"><span data-stu-id="a184a-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="a184a-121">WebMatrix beta 版會使用相同的功能強大的 web 伺服器、 資料庫引擎和架構環境，將會在網際網路上，讓從開發轉換到生產環境，順利且流暢地執行您的網站。</span><span class="sxs-lookup"><span data-stu-id="a184a-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="a184a-122">安裝</span><span class="sxs-lookup"><span data-stu-id="a184a-122">Installation</span></span>

> <span data-ttu-id="a184a-123">若要安裝 WebMatrix Beta 3，您使用[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。</span><span class="sxs-lookup"><span data-stu-id="a184a-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="a184a-124">您已安裝 Web Platform Installer 之後，您可以使用它來安裝 WebMatrix Beta 3。</span><span class="sxs-lookup"><span data-stu-id="a184a-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="a184a-125">如果您在安裝期間遇到問題，請參閱[疑難排解 Microsoft Web Platform Installer 的問題](https://go.microsoft.com/fwlink/?LinkId=196212)。</span><span class="sxs-lookup"><span data-stu-id="a184a-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="a184a-126">發行應用程式的指示</span><span class="sxs-lookup"><span data-stu-id="a184a-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="a184a-127">請參閱[發行應用程式的逐步指示](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="a184a-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="a184a-128">新的功能，變更，andKnown 問題</span><span class="sxs-lookup"><span data-stu-id="a184a-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="a184a-129">WebMatrix Beta 3 的安裝</span><span class="sxs-lookup"><span data-stu-id="a184a-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="a184a-130">問題： WebMatrix Beta 3 現在只適用於支援 Microsoft.NET Framework 4 的平台</span><span class="sxs-lookup"><span data-stu-id="a184a-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="a184a-131">WebMatrix Beta 需要.NET Framework 第 4 版。</span><span class="sxs-lookup"><span data-stu-id="a184a-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="a184a-132">在某些情況下，WebMatrix Beta 安裝程式，也可讓您嘗試安裝不支援的組態集的一部分的平台上。</span><span class="sxs-lookup"><span data-stu-id="a184a-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="a184a-133">特別是，Windows Vista SP1 更新不會讓您開始安裝 WebMatrix beta 版，但.NET Framework 4 元件會失敗並封鎖您的安裝。</span><span class="sxs-lookup"><span data-stu-id="a184a-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="a184a-134">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-134">**Workaround**</span></span>  
> <span data-ttu-id="a184a-135">安裝在支援的平台，包括：</span><span class="sxs-lookup"><span data-stu-id="a184a-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="a184a-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="a184a-136">Windows 7</span></span>
> - <span data-ttu-id="a184a-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="a184a-137">Windows Server 2008</span></span>
> - <span data-ttu-id="a184a-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="a184a-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="a184a-139">Windows Vista SP1 (含) 以後版本</span><span class="sxs-lookup"><span data-stu-id="a184a-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="a184a-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="a184a-140">Windows XP SP3</span></span>
> - <span data-ttu-id="a184a-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="a184a-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="a184a-142">問題︰ 無法安裝 WebMatrix Beta 3，如果未使用 Microsoft Visual Studio 2008 SP1 安裝 Microsoft Visual Studio 2008</span><span class="sxs-lookup"><span data-stu-id="a184a-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="a184a-143">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-143">**Workaround**</span></span>  
> <span data-ttu-id="a184a-144">安裝[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en)從 Microsoft 下載中心取得。</span><span class="sxs-lookup"><span data-stu-id="a184a-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="a184a-145">問題： SQL Server Compact 4.0 的某些組件不會安裝在 GAC 中</span><span class="sxs-lookup"><span data-stu-id="a184a-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="a184a-146">當您在 64 位元電腦上安裝 SQL Server Compact 4.0，以及電腦只有.NET Framework 3.5 SP1 用戶端安裝設定檔時，SQL Server Compact 4.0 的 managed 組件不會放在全域組件快取 (GAC) 中。</span><span class="sxs-lookup"><span data-stu-id="a184a-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="a184a-147">不會安裝在 GAC 中的 managed 組件包括：</span><span class="sxs-lookup"><span data-stu-id="a184a-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="a184a-148">*System.Data.SqlServerCe.dll* (ado.net)</span><span class="sxs-lookup"><span data-stu-id="a184a-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="a184a-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="a184a-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="a184a-150">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-150">**Workaround**</span></span>  
> <span data-ttu-id="a184a-151">解除安裝 SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="a184a-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="a184a-152">下載並安裝完整版的.NET Framework 3.5 SP1，請從下列位置：</span><span class="sxs-lookup"><span data-stu-id="a184a-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="a184a-153">Microsoft.NET Framework 3.5 Service pack 1 （完整套件）</span><span class="sxs-lookup"><span data-stu-id="a184a-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="a184a-154">然後重新安裝 SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="a184a-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="a184a-155">問題： 無法解除安裝 SQL Server Compact 使用命令列</span><span class="sxs-lookup"><span data-stu-id="a184a-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="a184a-156">解除安裝的 SQL Server Compact 使用命令列選項在此版本中無法運作。</span><span class="sxs-lookup"><span data-stu-id="a184a-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="a184a-157">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-157">**Workaround**</span></span>  
> <span data-ttu-id="a184a-158">使用*程式和功能*Windows 控制台中解除安裝 Microsoft SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="a184a-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="a184a-159">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="a184a-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="a184a-160">本節的文件說明新功能、 變更和 Beta 3 版本的 ASP.NET Web Pages 含有 Razor 語法的已知的問題。</span><span class="sxs-lookup"><span data-stu-id="a184a-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="a184a-161">新功能</span><span class="sxs-lookup"><span data-stu-id="a184a-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="a184a-162">變更</span><span class="sxs-lookup"><span data-stu-id="a184a-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="a184a-163">問題</span><span class="sxs-lookup"><span data-stu-id="a184a-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="a184a-164">Beta 3 for ASP.NET 網頁使用 Razor 語法的新功能</span><span class="sxs-lookup"><span data-stu-id="a184a-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="a184a-165">新功能: 「 Html.Raw"方法會呈現未編碼的標記</span><span class="sxs-lookup"><span data-stu-id="a184a-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="a184a-166">新`Html.Raw`方法可讓您為標記，而不是轉譯編碼的輸出中呈現 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="a184a-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="a184a-167">（根據預設，ASP.NET Razor 將字串編碼再轉譯它們。）其語法為：</span><span class="sxs-lookup"><span data-stu-id="a184a-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="a184a-168">下列範例顯示如何使用 `Html.Raw`：</span><span class="sxs-lookup"><span data-stu-id="a184a-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="a184a-169">Beta 3 for ASP.NET 網頁使用 Razor 語法中的變更</span><span class="sxs-lookup"><span data-stu-id="a184a-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="a184a-170">移除的變更:"HrefAttribute 」 方法</span><span class="sxs-lookup"><span data-stu-id="a184a-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="a184a-171">`HrefAttribute`方法的`WebPage`類別已移除。</span><span class="sxs-lookup"><span data-stu-id="a184a-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="a184a-172">此協助程式用來編碼在 Url 中不安全的字元。</span><span class="sxs-lookup"><span data-stu-id="a184a-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="a184a-173">它不再需要因為 ASP.NET Razor 自動編碼字串。</span><span class="sxs-lookup"><span data-stu-id="a184a-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="a184a-174">(使用新`Html.Raw`方法以呈現未編碼的字串。)</span><span class="sxs-lookup"><span data-stu-id="a184a-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="a184a-175">變更： 語法宣告式 「@helper」 變更的協助程式</span><span class="sxs-lookup"><span data-stu-id="a184a-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="a184a-176">在 Beta 3 版本中，ASP.NET 會變更它會使用所建立的協助程式的剖析`@helper`語法。</span><span class="sxs-lookup"><span data-stu-id="a184a-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="a184a-177">基本上，`@helper`語法現在會剖析為程式碼區塊而不是做為區塊的標記，可以包含程式碼。</span><span class="sxs-lookup"><span data-stu-id="a184a-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="a184a-178">因此，在協助程式碼不需要括在`@{ }`區塊。</span><span class="sxs-lookup"><span data-stu-id="a184a-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="a184a-179">相反地，標記協助程式必須明確包含在 HTML 項目或 ASP.NET Razor`<text></text>`標記。</span><span class="sxs-lookup"><span data-stu-id="a184a-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="a184a-180">例如，下列`@helper`Beta 3 版本中運作的語法：</span><span class="sxs-lookup"><span data-stu-id="a184a-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="a184a-181">在 Beta 3 版本中，您必須變更此協助程式，以下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a184a-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="a184a-182">請注意，`@{ }`不再使用協助程式中的初始程式碼周圍的字元。</span><span class="sxs-lookup"><span data-stu-id="a184a-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="a184a-183">這是因為內容的協助程式預設情況下被視為程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="a184a-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="a184a-184">Helper 會呈現開啟的開頭的標記`<a>`標記。</span><span class="sxs-lookup"><span data-stu-id="a184a-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="a184a-185">如果協助專家必須轉譯純文字] 或 [不包含結尾標記的標記 (例如`<meta>`標記)，要呈現的內容必須位於`<text></text>`標記。</span><span class="sxs-lookup"><span data-stu-id="a184a-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="a184a-186">變更:"WebPageContext.HttpContext 」 移除</span><span class="sxs-lookup"><span data-stu-id="a184a-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="a184a-187">`WebPageContext.HttpContext`已移除屬性。</span><span class="sxs-lookup"><span data-stu-id="a184a-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="a184a-188">請改用 `HttpContext.Current`。</span><span class="sxs-lookup"><span data-stu-id="a184a-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="a184a-189">(`WebPageContext.HttpContext`屬性直接包裝這。)</span><span class="sxs-lookup"><span data-stu-id="a184a-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="a184a-190">移至新的封裝變更:"Facebook"協助程式</span><span class="sxs-lookup"><span data-stu-id="a184a-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="a184a-191">`Facebook`協助程式已移至*Facebook.Helper*程式庫，其中包含`Facebook`helper 和其他功能。</span><span class="sxs-lookup"><span data-stu-id="a184a-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="a184a-192">您必須安裝此程式庫為個別的套件，「 安裝協助程式使用封裝管理員 」 中所述在本教學課程[Getting Started with ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202889)。</span><span class="sxs-lookup"><span data-stu-id="a184a-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="a184a-193">變更： 移至新的組件的成員資格、 角色和安全性的類型</span><span class="sxs-lookup"><span data-stu-id="a184a-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="a184a-194">下列類型移到`WebMatrix.WebData`組件：</span><span class="sxs-lookup"><span data-stu-id="a184a-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="a184a-195">移至 System.Web.WebPages.dll 組件的變更:"TagBuilder 」 類別</span><span class="sxs-lookup"><span data-stu-id="a184a-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="a184a-196">`TagBuilde` r 類別已移至 System.Web.WebPages.dll 組件。</span><span class="sxs-lookup"><span data-stu-id="a184a-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="a184a-197">這在以前是 ASP.NET MVC 的一部分的組件中。</span><span class="sxs-lookup"><span data-stu-id="a184a-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="a184a-198">這項變更表示您沒有安裝，才能使用 ASP.NET MVC`TagBuilder`類別。</span><span class="sxs-lookup"><span data-stu-id="a184a-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="a184a-199">不過，此類別是仍在`System.Web.Mvc`命名空間。</span><span class="sxs-lookup"><span data-stu-id="a184a-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="a184a-200">若要使用`TagBuilder`類別 （例如，在自訂 ASP.NET Razor 協助程式），您必須參考的命名空間 (例如，藉由加入`@using System.Web.Mvc`您的程式碼)。</span><span class="sxs-lookup"><span data-stu-id="a184a-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="a184a-201">變更： 要求驗證語法已變更;移除的 「 驗證 」 類別</span><span class="sxs-lookup"><span data-stu-id="a184a-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="a184a-202">在 Beta 3 版本中，若要停用驗證個別欄位或一組欄位，您可以呼叫`Validation.Exclude`方法並傳入要從驗證排除的欄位名稱。</span><span class="sxs-lookup"><span data-stu-id="a184a-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="a184a-203">略過驗證的 Beta 3 版本提供一種新語法。</span><span class="sxs-lookup"><span data-stu-id="a184a-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="a184a-204">`Validation` Beta 3 中所使用的方法已被移除。</span><span class="sxs-lookup"><span data-stu-id="a184a-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="a184a-205">如果有使用者嘗試使用上傳的 HTML 標記 （例如 rtf 編輯器頁面上） 未停用要求驗證，網站會報告類似的錯誤*從用戶端的偵測到有潛在危險的Request.Form值*，而且無法接受使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a184a-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="a184a-206">如果您停用要求驗證，您必須手動檢查並確定它不會不包含有潛在危險的標記或指令碼使用類似的使用者輸入[Microsoft 反跨網站指令碼程式庫 V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)。</span><span class="sxs-lookup"><span data-stu-id="a184a-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="a184a-207">若要停用自動要求驗證，請呼叫`Request.Unvalidated`方法，將欄位或其他您希望略過要求驗證的 post 物件的名稱傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="a184a-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="a184a-208">您可以使用這個方法來略過驗證中的任何項目`Form`， `QueryString`， `Cookies`，和`ServerVariables`集合。</span><span class="sxs-lookup"><span data-stu-id="a184a-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="a184a-209">下列範例示範如何使用`Unvalidated`方法：</span><span class="sxs-lookup"><span data-stu-id="a184a-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="a184a-210">含有 Razor 語法的 ASP.NET Web 網頁的已知的問題</span><span class="sxs-lookup"><span data-stu-id="a184a-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="a184a-211">問題： 未預期的行為時使用的自訂使用者資料表的成員資格</span><span class="sxs-lookup"><span data-stu-id="a184a-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="a184a-212">若要初始化 ASP.NET Razor 網站的成員資格提供者，請呼叫`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="a184a-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="a184a-213">(在 WebMatrix 中，入門網站範本會包含在這個方法呼叫 *\_AppStart.cshtml*檔案。)如果`autoCreateTables`這個方法的參數設定為 true (根據預設，它設定為在入門網站範本，則為 true)，以及無法辨識的資料表名稱傳遞至方法 （第二個參數），如果方法沒有擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="a184a-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="a184a-214">相反地，它會自動建立資料表。</span><span class="sxs-lookup"><span data-stu-id="a184a-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="a184a-215">如果您想要使用自訂使用者資料表的成員資格，但傳遞至錯誤的資料表名稱，這可能會有問題`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="a184a-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="a184a-216">因為方法不會預設引發錯誤如果您指定的資料表不存在，而且它改為建立新的資料表，應用程式可以在運作中。</span><span class="sxs-lookup"><span data-stu-id="a184a-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="a184a-217">不過，依賴您的自訂使用者資料表 （和中的欄位） 的應用程式程式碼最後可能發生未預期的錯誤會失敗。</span><span class="sxs-lookup"><span data-stu-id="a184a-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="a184a-218">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-218">**Workaround**</span></span>  
> <span data-ttu-id="a184a-219">請確定名稱傳入`InitializeDatabaseConnection`方法比對的使用者設定檔資料表中的成員資格資料庫中，或請確定`autoCreateTables`參數設定為 false。</span><span class="sxs-lookup"><span data-stu-id="a184a-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="a184a-220">問題: 「 無法產生 SQL Server 的使用者執行個體 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="a184a-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="a184a-221">如果 WebMatrix Web 應用程式會使用 SQL Server Express，而且在 Windows 7 或 Windows Server 2008 R2 上執行 IIS 7.5，您可能會看到錯誤指出 SQL Server 無法在執行階段擷取使用者的本機應用程式路徑。</span><span class="sxs-lookup"><span data-stu-id="a184a-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="a184a-222">**因應措施**確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有這類的應用程式根資料夾以及子資料夾的讀取/寫入權限*應用程式\_資料*.</span><span class="sxs-lookup"><span data-stu-id="a184a-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="a184a-223">知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。</span><span class="sxs-lookup"><span data-stu-id="a184a-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="a184a-224">問題： 在 Visual Studio 中，自訂組件 (Dll) 的命名空間會不自動匯入</span><span class="sxs-lookup"><span data-stu-id="a184a-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="a184a-225">如果您在 Visual Studio 專案中使用自訂組件，這些組件中宣告的命名空間不會自動匯入在設計階段。</span><span class="sxs-lookup"><span data-stu-id="a184a-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="a184a-226">如此一來，自訂類型參考可能無法辨識在設計階段，且會標示為無法辨識在 Visual Studio 中 （使用 「 波浪線 」）。</span><span class="sxs-lookup"><span data-stu-id="a184a-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="a184a-227">只能在設計階段，在 Visual Studio 中，就會發生此問題應用程式本身可以正確執行。</span><span class="sxs-lookup"><span data-stu-id="a184a-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="a184a-228">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-228">**Workaround**</span></span>  
> <span data-ttu-id="a184a-229">包含`using`陳述式 (`imports` Visual Basic 中)，參考在設計階段無法辨識的實體。</span><span class="sxs-lookup"><span data-stu-id="a184a-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="a184a-230">問題： Visual Studio IntelliSense 和專案範本僅適用於 ASP.NET MVC 3 版</span><span class="sxs-lookup"><span data-stu-id="a184a-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="a184a-231">安裝 ASP.NET Web Pages 不會也安裝 tools for Visual Studio ASP.NET Web Pages 應用程式的 IntelliSense 和專案範本等。</span><span class="sxs-lookup"><span data-stu-id="a184a-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="a184a-232">**因應措施**若要使用 IntelliSense 和專案範本在 Visual Studio 中的 ASP.NET Web Pages 應用程式，安裝 ASP.NET MVC 3 RC 透過 Web Platform Installer 或[獨立安裝](https://go.microsoft.com/fwlink/?LinkID=191797)。</span><span class="sxs-lookup"><span data-stu-id="a184a-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="a184a-233">問題: 「&lt;協助程式&gt;找不到類別 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="a184a-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="a184a-234">在升級到 Beta 3 之後，您可能會看到錯誤，協助程式類別 (例如`Facebook`類別) 找不到不。</span><span class="sxs-lookup"><span data-stu-id="a184a-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="a184a-235">從 Beta 2 開始，並繼續在 Beta 3 中，協助程式都已移至您必須明確安裝的套件中。</span><span class="sxs-lookup"><span data-stu-id="a184a-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="a184a-236">現有的站台並不會升級為包含這些封裝中;這包括中的站台*\My Documents\IISExpress* 或是 *\My Documents\My 網站* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a184a-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="a184a-237">特別是，您會看到這個錯誤如果您使用中的預設站台*My Sites* (網站 1)，其中包含的參考`Twitter`協助程式。</span><span class="sxs-lookup"><span data-stu-id="a184a-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="a184a-238">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-238">**Workaround**</span></span>  
> <span data-ttu-id="a184a-239">在網站中，執行任何協助程式的呼叫註解 *\_Admin*頁面上，並安裝包含您想要使用的協助程式的封裝。</span><span class="sxs-lookup"><span data-stu-id="a184a-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="a184a-240">您已安裝封裝之後，您可以參考協助程式行取消註解。</span><span class="sxs-lookup"><span data-stu-id="a184a-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="a184a-241">問題： Beta 3 ASP.NET Razor 組件部署至 Bin 資料夾可能不適用於裝載站台</span><span class="sxs-lookup"><span data-stu-id="a184a-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="a184a-242">如果您裝載的網站，部署將 ASP.NET Web Pages 網站，而且您將 ASP.NET Razor Beta 3 的組件部署至站台的*Bin*資料夾中，您可能會遇到錯誤，包括下列：</span><span class="sxs-lookup"><span data-stu-id="a184a-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="a184a-243">如果裝載提供者已安裝到伺服器的全域應用程式快取 (GAC 中) 的 ASP.NET Web Pages Beta 1 組件，會發生這項目。</span><span class="sxs-lookup"><span data-stu-id="a184a-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="a184a-244">組件在 GAC 中的透過本機上安裝的組件中取得優先順序*Bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a184a-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="a184a-245">**因應措施**連絡以確認您看到的錯誤是提供者的版本之間發生衝突，因此您裝載提供者的組件和您。</span><span class="sxs-lookup"><span data-stu-id="a184a-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="a184a-246">如果是這樣，要求主機服務提供者更新伺服器的 GAC 中的組件。</span><span class="sxs-lookup"><span data-stu-id="a184a-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="a184a-247">問題： 讀取摘要或其他外部的資料，透過 proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="a184a-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="a184a-248">如果執行站台的伺服器是在 proxy 伺服器後方，您可能需要設定中的 proxy 資訊*Web.config*檔案以讀取來自網站外部的資訊。</span><span class="sxs-lookup"><span data-stu-id="a184a-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="a184a-249">例如，如果您使用`ReCaptcha`協助程式，協助專家會與 reCAPTCHA 服務通訊，但可能會封鎖您的 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a184a-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="a184a-250">同樣地，使用 ASP.NET Web Pages 中，例如套件管理員 中，所使用的摘要的摘要可能需要 proxy 組態。</span><span class="sxs-lookup"><span data-stu-id="a184a-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="a184a-251">如果您遇到問題，使用外部服務，或使用套件摘要中的，將下列項目放到您的應用程式根目錄*Web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="a184a-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="a184a-252">如需有關如何設定 proxy 伺服器的詳細資訊，請參閱 < [ &lt;proxy&gt;項目 （網路設定）](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="a184a-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="a184a-253">問題: 「 無法載入 Microsoft.Web.Infrastructure.dll 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="a184a-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="a184a-254">如果您先前安裝含有 Razor 語法的 Beta 1 版本的 ASP.NET Web Pages，然後再安裝 Beta 3 版本，所有適當的組件會安裝在 GAC 除外*Microsoft.Web.Infrastructure.dll*。</span><span class="sxs-lookup"><span data-stu-id="a184a-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="a184a-255">如此一來，當您執行 ASP.NET Razor 頁面，您會看到錯誤，指出*Microsoft.Web.Infrastructure.dll*無法載入。</span><span class="sxs-lookup"><span data-stu-id="a184a-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="a184a-256">如果您載入 Beta 3 版本乾淨的電腦上，則不會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="a184a-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="a184a-257">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-257">**Workaround**</span></span>  
> <span data-ttu-id="a184a-258">在 [控制台] 解除安裝 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="a184a-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="a184a-259">然後重新安裝 Beta 3 版本。</span><span class="sxs-lookup"><span data-stu-id="a184a-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="a184a-260">問題： 解除安裝.NET Framework 第 4 版會停用含有 Razor 語法的 ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="a184a-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="a184a-261">如果您解除安裝.NET Framework 第 4 版，然後重新安裝時，會停用含有 Razor 語法的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="a184a-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="a184a-262">頁面 *.cshtml*延伸模組無法正確執行。</span><span class="sxs-lookup"><span data-stu-id="a184a-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="a184a-263">ASP.NET 網頁註冊組件在電腦根目錄*Web.config*檔案，並移除.NET Framework 中移除該檔案。</span><span class="sxs-lookup"><span data-stu-id="a184a-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="a184a-264">重新安裝.NET Framework 會安裝新版本的組態檔中，但不會新增 ASP.NET Web Pages 組件的參考。</span><span class="sxs-lookup"><span data-stu-id="a184a-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="a184a-265">**因應措施**之後重新安裝.NET Framework，請重新安裝 ASP.NET Web Pages 含有 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="a184a-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="a184a-266">這會將新增到下列項目*Web.config*電腦根目錄，這通常是在下列位置中的檔案：</span><span class="sxs-lookup"><span data-stu-id="a184a-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="a184a-267">問題： 先前使用 ASP.NET 的 Bin 資料夾中的組件部署的應用程式遇到錯誤</span><span class="sxs-lookup"><span data-stu-id="a184a-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="a184a-268">在部署期間，ASP.NET 網頁組件複本 (例如*Microsoft.WebPages.dll*) 來*Bin*網站伺服器上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a184a-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="a184a-269">(這在部署期間可能會自動發生，或因為開發人員明確複製的組件。)不過，安裝 Beta 3 版本時，錯誤就會發生，例如找不到特定類型的錯誤。</span><span class="sxs-lookup"><span data-stu-id="a184a-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="a184a-270">這是因為許多 ASP.NET Web Pages 類型時，已移至不同的命名空間上，針對 Beta 3 版本。</span><span class="sxs-lookup"><span data-stu-id="a184a-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="a184a-271">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="a184a-271">**Workaround** </span></span>  
> <span data-ttu-id="a184a-272">清除*Bin*資料夾部署的應用程式，將新的組件複製到資料夾 （或重新部署應用程式），然後重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a184a-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="a184a-273">問題： 無副檔名 Url 找.cshtml/.vbhtml 檔案，在 IIS 7 或 IIS 7.5 上的不到</span><span class="sxs-lookup"><span data-stu-id="a184a-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="a184a-274">在 IIS 7 或 IIS 7.5 上，要求如下所示的 url 不能找到網頁具有 *.cshtml*或是 *.vbhtml*延伸模組：</span><span class="sxs-lookup"><span data-stu-id="a184a-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="a184a-275">因為 URL 重寫未啟用預設的 IIS 7 或 IIS 7.5，就會發生問題。</span><span class="sxs-lookup"><span data-stu-id="a184a-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="a184a-276">Likeliest 案例是看不到問題時測試在本機上使用 IIS Express，但它時遇到您將您的網站部署至裝載的網站。</span><span class="sxs-lookup"><span data-stu-id="a184a-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="a184a-277">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="a184a-278">如果您有伺服器電腦的控制權時，在伺服器電腦上安裝更新中所述[的更新功能，可讓特定 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。</span><span class="sxs-lookup"><span data-stu-id="a184a-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="a184a-279">如果您沒有在伺服器電腦的控制權 （例如，您要部署到裝載的網站），加上網站的下列*Web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="a184a-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="a184a-280">問題： 會在相同的應用程式中使用 「 Web 應用程式專案或 ASP.NET MVC 和 ASP.NET 網頁</span><span class="sxs-lookup"><span data-stu-id="a184a-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="a184a-281">如果您已使用 ASP.NET Web Pages 中的 Web 應用程式專案或 ASP.NET MVC 應用程式，您可能會看到錯誤， *WebPageHttpApplication*找不到。</span><span class="sxs-lookup"><span data-stu-id="a184a-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="a184a-282">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-282">**Workaround**</span></span>  
> <span data-ttu-id="a184a-283">如果您收到這個錯誤，請變更應用程式所衍生自的基底類別。</span><span class="sxs-lookup"><span data-stu-id="a184a-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="a184a-284">在  *Global.asax*檔案中，變更下列這一行：</span><span class="sxs-lookup"><span data-stu-id="a184a-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="a184a-285">為此值：</span><span class="sxs-lookup"><span data-stu-id="a184a-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="a184a-286">這實際上會反轉變更導入 Beta 1 版本的 ASP.NET Web Pages 中含有 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="a184a-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="a184a-287">問題： 應用程式部署至沒有 SQL Server Compact 安裝的電腦</span><span class="sxs-lookup"><span data-stu-id="a184a-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="a184a-288">包含 SQL Server Compact 資料庫的應用程式可以執行 SQL Server Compact 不安裝所在的電腦上。</span><span class="sxs-lookup"><span data-stu-id="a184a-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="a184a-289">Microsoft WebMatrix Beta 3 自動為您複製這些二進位檔，並執行適當*Web.config*檔案轉換。</span><span class="sxs-lookup"><span data-stu-id="a184a-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="a184a-290">**因應措施**如果您需要複製這些檔案，並使*Web.config*檔案變更以手動的方式，執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="a184a-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="a184a-291">資料庫引擎組件，以複製*Bin*目標電腦上的應用程式的資料夾 （和子資料夾）：</span><span class="sxs-lookup"><span data-stu-id="a184a-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="a184a-292">複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **要** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="a184a-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="a184a-293">複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **來** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="a184a-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="a184a-294">複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **來** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="a184a-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="a184a-295">在網站的根資料夾中，建立或開啟*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a184a-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="a184a-296">(在 WebMatrix Beta 3 中，這種檔案類型是如果您按一下可用**所有**中**選擇 [檔案類型**] 對話方塊。)</span><span class="sxs-lookup"><span data-stu-id="a184a-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="a184a-297">將下列項目新增為子系 **&lt;configuration&gt;** 項目 (不是在內**&lt;system.web&gt;** 項目):</span><span class="sxs-lookup"><span data-stu-id="a184a-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="a184a-298">問題： 中度信任在 Visual Basic 中無法運作資料庫和 WebGrid 協助程式</span><span class="sxs-lookup"><span data-stu-id="a184a-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="a184a-299">如果您使用 Visual Basic (建立 *.vbhtml*檔案)，則`Database`和`WebGrid`如果應用程式會設定為使用中度信任，協助程式將無法運作。</span><span class="sxs-lookup"><span data-stu-id="a184a-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="a184a-300">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-300">**Workaround**</span></span>  
> <span data-ttu-id="a184a-301">暫時設定應用程式使用完全信任。</span><span class="sxs-lookup"><span data-stu-id="a184a-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="a184a-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="a184a-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="a184a-303">問題： 無法辨識"Encrypt"屬性</span><span class="sxs-lookup"><span data-stu-id="a184a-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="a184a-304">無法辨識 SQL Server Compact 4.0`Encrypt`屬性`SqlCeConnection`類別。</span><span class="sxs-lookup"><span data-stu-id="a184a-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="a184a-305">您不應該使用這個屬性來加密資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="a184a-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="a184a-306">`Encrypt`屬性已被取代 SQL Server Compact 3.5 的版本中，並保留僅為回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="a184a-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="a184a-307">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-307">**Workaround**</span></span>  
> <span data-ttu-id="a184a-308">使用`Encryption Mode`屬性`SqlCeConnection`類別來加密 SQL Server Compact 4.0 資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="a184a-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="a184a-309">下列範例示範如何建立加密的 SQL Server Compact 4.0 資料庫使用`Encryption Mode`屬性：</span><span class="sxs-lookup"><span data-stu-id="a184a-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="a184a-310">若要變更現有的 SQL Server Compact 4.0 資料庫的加密模式，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="a184a-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="a184a-311">若要加密的未加密的 SQL Server Compact 4.0 資料庫，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="a184a-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="a184a-312">Microsoft Visual c + + 2008年執行階段程式庫所需的問題：</span><span class="sxs-lookup"><span data-stu-id="a184a-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="a184a-313">原生 Dll 的 SQL Server Compact 4.0 需要 Microsoft Visual c + + 2008年執行階段程式庫 （IA64，x86 和 x64）、 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="a184a-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="a184a-314">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-314">**Workaround**</span></span>  
> <span data-ttu-id="a184a-315">安裝.NET Framework 3.5 SP1。</span><span class="sxs-lookup"><span data-stu-id="a184a-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="a184a-316">這也會安裝 Visual c + + 2008年執行階段程式庫 SP1。</span><span class="sxs-lookup"><span data-stu-id="a184a-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="a184a-317">您可以從下列位置下載的程式庫：</span><span class="sxs-lookup"><span data-stu-id="a184a-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="a184a-318">Microsoft Visual c + + 2008 Service Pack 1 可轉散發套件的 ATL 安全性更新</span><span class="sxs-lookup"><span data-stu-id="a184a-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="a184a-319">請注意，安裝.NET Framework 2.0、 3.0 或 4*不*安裝 SP1 的 windows Visual c + + 2008年執行階段程式庫。</span><span class="sxs-lookup"><span data-stu-id="a184a-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="a184a-320">問題： 如果 SQL Server Compact 安裝在電腦上安裝.NET Framework 之前，其提供者非變異名稱未註冊.NET Framework 的 machine.config 檔案中</span><span class="sxs-lookup"><span data-stu-id="a184a-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="a184a-321">SQL Server Compact 可以安裝在沒有安裝，因為 SQL Server Compact 一定需要.NET framework 的.NET Framework 的電腦上。</span><span class="sxs-lookup"><span data-stu-id="a184a-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="a184a-322">如果您安裝 SQL Server Compact 安裝既不的.NET Framework 版本 3.5 或 4，SQL Server Compact 安裝程式不會註冊在其提供者非變異名稱*machine.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a184a-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="a184a-323">中的 SQL Server Compact 項目所依賴的任何應用程式*machine.config*檔案將會失敗。</span><span class="sxs-lookup"><span data-stu-id="a184a-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="a184a-324">中的非變異名稱註冊項目*machine.config*如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a184a-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="a184a-325">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-325">**Workaround**</span></span>  
> <span data-ttu-id="a184a-326">解除安裝 SQL Server Compact 4.0 CTP1。</span><span class="sxs-lookup"><span data-stu-id="a184a-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="a184a-327">下載並安裝.NET Framework 的完整版本，從下列位置：</span><span class="sxs-lookup"><span data-stu-id="a184a-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="a184a-328">Microsoft.NET Framework 3.5 Service pack 1 （完整套件）</span><span class="sxs-lookup"><span data-stu-id="a184a-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="a184a-329">Microsoft.NET Framework 4.0 版 （完整套件）</span><span class="sxs-lookup"><span data-stu-id="a184a-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="a184a-330">然後重新安裝[SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)。</span><span class="sxs-lookup"><span data-stu-id="a184a-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="a184a-331">安裝應用程式</span><span class="sxs-lookup"><span data-stu-id="a184a-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="a184a-332">問題： 安裝應用程式可能需要很長的時間如果使用者的 [我的文件] 資料夾重新導向至網路共用</span><span class="sxs-lookup"><span data-stu-id="a184a-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="a184a-333">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-333">**Workaround**</span></span>  
> <span data-ttu-id="a184a-334">無。</span><span class="sxs-lookup"><span data-stu-id="a184a-334">None.</span></span> <span data-ttu-id="a184a-335">應用程式可能需要一段時間，若要安裝，但會正確安裝。</span><span class="sxs-lookup"><span data-stu-id="a184a-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="a184a-336">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="a184a-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="a184a-337">問題： 站台後可能無法運作如果 [目的地 URL] 欄位不以 http:// 或 https:// 前置發行</span><span class="sxs-lookup"><span data-stu-id="a184a-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="a184a-338">在 [**發佈設定**] 對話方塊中，如果目的地 URL 開頭不是`http://`或`https://`，部署後，網站可能無法運作。</span><span class="sxs-lookup"><span data-stu-id="a184a-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="a184a-339">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-339">**Workaround**</span></span>  
> <span data-ttu-id="a184a-340">請確定發行的網站中的目的地 URL 之前，先**發佈設定** 對話方塊中的開頭`http://`或`https://`。</span><span class="sxs-lookup"><span data-stu-id="a184a-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="a184a-341">問題： 發行的 MySQL 資料庫失敗並出現錯誤 「 無法發行資料庫。</span><span class="sxs-lookup"><span data-stu-id="a184a-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="a184a-342">這就可能發生遠端資料庫無法執行指令碼。 」</span><span class="sxs-lookup"><span data-stu-id="a184a-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="a184a-343">有許多原因可能會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="a184a-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="a184a-344">您可以看到此錯誤的其中一個原因是如果將資料庫指令碼包含單引號字元 （'），而且目的地 MySQL 資料庫的預設字元集不為 utf-8。</span><span class="sxs-lookup"><span data-stu-id="a184a-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="a184a-345">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-345">**Workaround**</span></span>  
> <span data-ttu-id="a184a-346">設定預設遠端 MySQL 資料庫設定為 utf-8 的字元。</span><span class="sxs-lookup"><span data-stu-id="a184a-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="a184a-347">其他問題</span><span class="sxs-lookup"><span data-stu-id="a184a-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="a184a-348">問題： 搜尋/篩選無法用於報表中群組依據： 問題類型</span><span class="sxs-lookup"><span data-stu-id="a184a-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="a184a-349">當您執行報表網站，如果您輸入的文字*依 URL 篩選*方塊，然後按一下*搜尋*，會發生任何事。</span><span class="sxs-lookup"><span data-stu-id="a184a-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="a184a-350">這是因為此控制項無法運作*Group By*報告的狀態會設為*問題類型*，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="a184a-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="a184a-351">**因應措施**中*Group By*  索引標籤的 功能區中，按一下  *URL*將由其來源 URL 的項目。</span><span class="sxs-lookup"><span data-stu-id="a184a-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="a184a-352">處於此狀態中的文字方塊和按鈕，以篩選項目會運作。</span><span class="sxs-lookup"><span data-stu-id="a184a-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="a184a-353">問題： WCF 應用程式會使用 IIS Express 來執行失敗</span><span class="sxs-lookup"><span data-stu-id="a184a-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="a184a-354">瀏覽至 WCF 應用程式會產生類似下列的其中一個錯誤：</span><span class="sxs-lookup"><span data-stu-id="a184a-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="a184a-355">*無法載入檔案或組件 ' Microsoft.Web.Administration，版本 = 7.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 或其中一個相依性。系統找不到指定的檔案。*</span><span class="sxs-lookup"><span data-stu-id="a184a-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="a184a-356">這是因為 IIS Express 的 Beta 版本不支援預設的 WCF。</span><span class="sxs-lookup"><span data-stu-id="a184a-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="a184a-357">**因應措施**使用任何其中一項因應措施 (因應措施 2 需要 Microsoft Windows Vista 或更新版本):</span><span class="sxs-lookup"><span data-stu-id="a184a-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="a184a-358">複製*Microsoft.Web.dll*並*Microsoft.Web.Administration.dll*組件，WebMatrix 安裝位置，以從*bin* WCF 目錄應用程式。</span><span class="sxs-lookup"><span data-stu-id="a184a-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="a184a-359">根據預設，在安裝 WebMatrix *Microsoft WebMatrix*子資料夾下的系統*Program Files*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a184a-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="a184a-360">Microsoft Windows Vista 或更新版本中，建立中的組件的符號連結*bin*目錄使用下列命令。</span><span class="sxs-lookup"><span data-stu-id="a184a-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="a184a-361">（這種方法有它不會建立一份組件的優點）。</span><span class="sxs-lookup"><span data-stu-id="a184a-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="a184a-362">安裝到 GAC 中的兩個組件。</span><span class="sxs-lookup"><span data-stu-id="a184a-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="a184a-363">從提升權限的提示字元，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a184a-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="a184a-364">問題： WebMatrix Beta 3 現在無法執行需要提高權限的特定工作</span><span class="sxs-lookup"><span data-stu-id="a184a-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="a184a-365">WebMatrix Beta 3 已無法執行需要提高權限，例如在下列情況下安裝其他元件的特定工作項目：</span><span class="sxs-lookup"><span data-stu-id="a184a-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="a184a-366">在 Windows Vista 或 Windows 7 上，您登入不具系統管理權限的帳戶，並已停用使用者帳戶控制 (UAC)。</span><span class="sxs-lookup"><span data-stu-id="a184a-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="a184a-367">您使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。</span><span class="sxs-lookup"><span data-stu-id="a184a-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="a184a-368">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-368">**Workaround**</span></span>  
> <span data-ttu-id="a184a-369">在 WebMatrix Beta 3 中的大部分工作不需要系統管理權限。</span><span class="sxs-lookup"><span data-stu-id="a184a-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="a184a-370">對於這些，您可以執行此作業，身為管理員，或遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="a184a-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="a184a-371">在 Windows Vista 或 Windows 7 上，啟用 UAC。</span><span class="sxs-lookup"><span data-stu-id="a184a-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="a184a-372">在 Windows XP 上加入 Administrators 安全性群組中的使用者。</span><span class="sxs-lookup"><span data-stu-id="a184a-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="a184a-373">問題: 「 站台從 Web 組件庫 「 已停用</span><span class="sxs-lookup"><span data-stu-id="a184a-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="a184a-374">**從 Web 組件庫網站**選項會停用，如果未安裝 Web Platform Installer 3.0。</span><span class="sxs-lookup"><span data-stu-id="a184a-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="a184a-375">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-375">**Workaround**</span></span>  
> <span data-ttu-id="a184a-376">安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。</span><span class="sxs-lookup"><span data-stu-id="a184a-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="a184a-377">問題： 在 Windows Server 2003 上 IIS Express 不會啟動非系統管理使用者</span><span class="sxs-lookup"><span data-stu-id="a184a-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="a184a-378">Windows Server 2003，當您啟動頁面，或啟動 IIS Express，IIS Express 不會啟動。</span><span class="sxs-lookup"><span data-stu-id="a184a-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="a184a-379">針對 Web 網頁會顯示錯誤，指出已由非系統管理使用者啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a184a-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="a184a-380">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-380">**Workaround**</span></span>  
> <span data-ttu-id="a184a-381">啟動 WebMatrix Beta 3 的系統管理使用者身分。</span><span class="sxs-lookup"><span data-stu-id="a184a-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="a184a-382">如需詳細資訊，請參閱下列知識庫文件：</span><span class="sxs-lookup"><span data-stu-id="a184a-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="a184a-383">非系統管理使用者所啟動的應用程式無法接聽 HTTP 流量的應用程式正在執行 Windows Vista、 Windows Server 2003 或 Windows XP 的電腦。</span><span class="sxs-lookup"><span data-stu-id="a184a-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="a184a-384">問題： Google Chrome 不提供執行選項</span><span class="sxs-lookup"><span data-stu-id="a184a-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="a184a-385">Google Chrome 不會顯示在清單下方的瀏覽器**執行**上**首頁** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="a184a-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="a184a-386">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-386">**Workaround**</span></span>  
> <span data-ttu-id="a184a-387">Google Chrome 的某些版本中請勿自行正確向註冊在 Windows 中的預設程式 功能。</span><span class="sxs-lookup"><span data-stu-id="a184a-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="a184a-388">因應措施，請啟動 Google Chrome，請按一下*自訂和控制 Google Chrome*功能表上，按一下*選項*，然後按一下*讓 Google Chrome 我的預設瀏覽器*。</span><span class="sxs-lookup"><span data-stu-id="a184a-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="a184a-389">問題: 「 外部索引鍵 」 對話方塊中不允許輸入主索引鍵</span><span class="sxs-lookup"><span data-stu-id="a184a-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="a184a-390">**外部索引鍵**對話方塊不允許您輸入主索引鍵的資料表主索引鍵的名稱。</span><span class="sxs-lookup"><span data-stu-id="a184a-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="a184a-391">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-391">**Workaround**</span></span>  
> <span data-ttu-id="a184a-392">這是刻意設計的。</span><span class="sxs-lookup"><span data-stu-id="a184a-392">This is intentional.</span></span> <span data-ttu-id="a184a-393">您不需要輸入主索引鍵資料表的主索引鍵的名稱。</span><span class="sxs-lookup"><span data-stu-id="a184a-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="a184a-394">問題: 「 關聯性 」 按鈕已停用</span><span class="sxs-lookup"><span data-stu-id="a184a-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="a184a-395">**關聯性**下方的按鈕**資料表**索引標籤**資料庫**工作區已停用 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="a184a-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="a184a-396">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-396">**Workaround**</span></span>  
> <span data-ttu-id="a184a-397">無。</span><span class="sxs-lookup"><span data-stu-id="a184a-397">None.</span></span> <span data-ttu-id="a184a-398">SQL Server Compact 不支援資料表之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="a184a-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="a184a-399">問題： 參數化的 SQL 查詢會擲回例外狀況</span><span class="sxs-lookup"><span data-stu-id="a184a-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="a184a-400">在 SQL Server Compact 4.0，如果您未指定的資料類型這類`SqlDbType`或`DbType`執行查詢時，發生例外狀況的參數化查詢中的參數。</span><span class="sxs-lookup"><span data-stu-id="a184a-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="a184a-401">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="a184a-401">**Workaround**</span></span>  
> <span data-ttu-id="a184a-402">明確地設定這類參數的資料類型`SqlDbType`或`DbType`。</span><span class="sxs-lookup"><span data-stu-id="a184a-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="a184a-403">這點很重要，如果 BLOB 資料類型 (`image`和`ntext`)。</span><span class="sxs-lookup"><span data-stu-id="a184a-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="a184a-404">使用程式碼，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a184a-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="a184a-405">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="a184a-405">For More Information</span></span>

<span data-ttu-id="a184a-406">如需有關 WebMatrix Beta 3 的詳細資訊，請參閱下列網站：</span><span class="sxs-lookup"><span data-stu-id="a184a-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="a184a-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="a184a-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="a184a-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a184a-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="a184a-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="a184a-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="a184a-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="a184a-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="a184a-411">All Rights Reserved.</span><span class="sxs-lookup"><span data-stu-id="a184a-411">All Rights Reserved.</span></span> <span data-ttu-id="a184a-412">[使用規定](https://msdn.microsoft.cos/cc300389.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a184a-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
