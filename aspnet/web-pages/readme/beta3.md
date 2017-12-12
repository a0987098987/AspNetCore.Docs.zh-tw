---
uid: web-pages/readme/beta3
title: "Web 矩陣和 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案 |Microsoft 文件"
author: rick-anderson
description: "Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5fad4b659dafe5470aeb84d320ff711b8840d1e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="00b18-103">Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案</span><span class="sxs-lookup"><span data-stu-id="00b18-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="00b18-104">Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案</span><span class="sxs-lookup"><span data-stu-id="00b18-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="00b18-105">9 年 11 月 2010</span><span class="sxs-lookup"><span data-stu-id="00b18-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="00b18-106">內容</span><span class="sxs-lookup"><span data-stu-id="00b18-106">Contents</span></span>

- [<span data-ttu-id="00b18-107">概觀</span><span class="sxs-lookup"><span data-stu-id="00b18-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="00b18-108">安裝</span><span class="sxs-lookup"><span data-stu-id="00b18-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="00b18-109">新功能、 變更和 Beta 3 版本中的已知問題</span><span class="sxs-lookup"><span data-stu-id="00b18-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="00b18-110">WebMatrix 安裝問題</span><span class="sxs-lookup"><span data-stu-id="00b18-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="00b18-111">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="00b18-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="00b18-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="00b18-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="00b18-113">安裝應用程式</span><span class="sxs-lookup"><span data-stu-id="00b18-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="00b18-114">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="00b18-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="00b18-115">其他問題</span><span class="sxs-lookup"><span data-stu-id="00b18-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="00b18-116">如需詳細資訊</span><span class="sxs-lookup"><span data-stu-id="00b18-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="00b18-117">概觀</span><span class="sxs-lookup"><span data-stu-id="00b18-117">Overview</span></span>

> <span data-ttu-id="00b18-118">Microsoft WebMatrix Beta 是以分鐘為單位安裝免費的網頁開發堆疊。</span><span class="sxs-lookup"><span data-stu-id="00b18-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="00b18-119">它可以整合網頁伺服器與資料庫和程式設計架構來建立單一整合體驗。</span><span class="sxs-lookup"><span data-stu-id="00b18-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="00b18-120">您可以使用 WebMatrix Beta 來簡化程式碼、 測試和發佈專屬 ASP.NET 或 PHP 網站的方式，或您可以使用 WebMatrix Beta 來啟動新的網站使用熱門的開放原始碼應用程式，例如 DotNetNuke、 Umbraco、 WordPress 或 Joomla。</span><span class="sxs-lookup"><span data-stu-id="00b18-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="00b18-121">WebMatrix beta 版會使用相同的功能強大的 web 伺服器、 資料庫引擎和架構環境將在網際網路上，讓從開發轉換至實際執行環境，順利且流暢地執行您的網站。</span><span class="sxs-lookup"><span data-stu-id="00b18-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="00b18-122">安裝</span><span class="sxs-lookup"><span data-stu-id="00b18-122">Installation</span></span>

> <span data-ttu-id="00b18-123">若要安裝 WebMatrix Beta 3，您使用[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。</span><span class="sxs-lookup"><span data-stu-id="00b18-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="00b18-124">安裝 Web Platform Installer 之後，您可以使用它來安裝 WebMatrix Beta 3。</span><span class="sxs-lookup"><span data-stu-id="00b18-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="00b18-125">如果您在安裝期間遇到問題，請參閱[疑難排解 Microsoft Web Platform Installer 的問題](https://go.microsoft.com/fwlink/?LinkId=196212)。</span><span class="sxs-lookup"><span data-stu-id="00b18-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="00b18-126">發行應用程式的指示</span><span class="sxs-lookup"><span data-stu-id="00b18-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="00b18-127">請參閱[發行應用程式的逐步指示](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="00b18-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="00b18-128">新功能，變更，andKnown 問題</span><span class="sxs-lookup"><span data-stu-id="00b18-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="00b18-129">WebMatrix Beta 3 安裝</span><span class="sxs-lookup"><span data-stu-id="00b18-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="00b18-130">問題： WebMatrix Beta 3 才支援 Microsoft.NET Framework 4 平台上可用</span><span class="sxs-lookup"><span data-stu-id="00b18-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="00b18-131">WebMatrix Beta 需要.NET Framework 第 4 版。</span><span class="sxs-lookup"><span data-stu-id="00b18-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="00b18-132">在某些情況下，WebMatrix beta 版安裝程式可讓您嘗試安裝不支援的組態集的一部分的平台上。</span><span class="sxs-lookup"><span data-stu-id="00b18-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="00b18-133">特別是，Windows Vista SP1 更新不會讓您開始安裝 WebMatrix Beta，但是.NET Framework 4 元件會失敗並封鎖您的安裝。</span><span class="sxs-lookup"><span data-stu-id="00b18-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="00b18-134">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-134">**Workaround**</span></span>  
> <span data-ttu-id="00b18-135">安裝在支援的平台，其中包括：</span><span class="sxs-lookup"><span data-stu-id="00b18-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="00b18-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="00b18-136">Windows 7</span></span>
> - <span data-ttu-id="00b18-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="00b18-137">Windows Server 2008</span></span>
> - <span data-ttu-id="00b18-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="00b18-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="00b18-139">Windows Vista SP1 (含) 以後版本</span><span class="sxs-lookup"><span data-stu-id="00b18-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="00b18-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="00b18-140">Windows XP SP3</span></span>
> - <span data-ttu-id="00b18-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="00b18-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="00b18-142">如果未安裝 Microsoft Visual Studio 2008 SP1 已安裝的 Microsoft Visual Studio 2008 問題： 無法安裝 WebMatrix Beta 3</span><span class="sxs-lookup"><span data-stu-id="00b18-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="00b18-143">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-143">**Workaround**</span></span>  
> <span data-ttu-id="00b18-144">安裝[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en)從 Microsoft 下載中心取得。</span><span class="sxs-lookup"><span data-stu-id="00b18-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="00b18-145">問題： SQL Server Compact 4.0 的某些組件不會安裝在 GAC 中</span><span class="sxs-lookup"><span data-stu-id="00b18-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="00b18-146">當您在 64 位元電腦上安裝 SQL Server Compact 4.0 而電腦只有.NET Framework 3.5 SP1 Client Profile 安裝時，SQL Server Compact 4.0 的 managed 組件不會放在全域組件快取 (GAC) 中。</span><span class="sxs-lookup"><span data-stu-id="00b18-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="00b18-147">不會安裝在 GAC 中的 managed 組件如下：</span><span class="sxs-lookup"><span data-stu-id="00b18-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="00b18-148">*System.Data.SqlServerCe.dll* （ADO.NET 提供者）</span><span class="sxs-lookup"><span data-stu-id="00b18-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="00b18-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="00b18-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="00b18-150">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-150">**Workaround**</span></span>  
> <span data-ttu-id="00b18-151">解除安裝 SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="00b18-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="00b18-152">下載並安裝.NET Framework 3.5 SP1 的完整版本，從下列位置：</span><span class="sxs-lookup"><span data-stu-id="00b18-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="00b18-153">Microsoft.NET Framework 3.5 Service pack 1 （完整套件）</span><span class="sxs-lookup"><span data-stu-id="00b18-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="00b18-154">然後重新安裝 SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="00b18-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="00b18-155">問題： 無法解除安裝 SQL Server Compact 使用命令列</span><span class="sxs-lookup"><span data-stu-id="00b18-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="00b18-156">解除安裝的 SQL Server Compact 使用命令列選項在此版本中無法運作。</span><span class="sxs-lookup"><span data-stu-id="00b18-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="00b18-157">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-157">**Workaround**</span></span>  
> <span data-ttu-id="00b18-158">使用*程式和功能*Windows 控制台中解除安裝 Microsoft SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="00b18-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="00b18-159">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="00b18-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="00b18-160">文件的本節會說明新功能、 變更和 Beta 3 版本的 ASP.NET 網頁使用 Razor 語法的已知的問題。</span><span class="sxs-lookup"><span data-stu-id="00b18-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="00b18-161">新功能</span><span class="sxs-lookup"><span data-stu-id="00b18-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="00b18-162">變更</span><span class="sxs-lookup"><span data-stu-id="00b18-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="00b18-163">問題</span><span class="sxs-lookup"><span data-stu-id="00b18-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="00b18-164">含有 Razor 語法的 beta 版 3 的 ASP.NET Web Pages 中的新功能</span><span class="sxs-lookup"><span data-stu-id="00b18-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="00b18-165">新增項目: 「 Html.Raw"方法會呈現未編碼的標記</span><span class="sxs-lookup"><span data-stu-id="00b18-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="00b18-166">新`Html.Raw`方法可讓您為標記，而不是呈現編碼的輸出中呈現 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="00b18-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="00b18-167">（根據預設，ASP.NET Razor 編碼字串後再呈現它們。）其語法為：</span><span class="sxs-lookup"><span data-stu-id="00b18-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="00b18-168">下列範例顯示如何使用 `Html.Raw`：</span><span class="sxs-lookup"><span data-stu-id="00b18-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="00b18-169">含有 Razor 語法的 beta 版 3 的 ASP.NET Web Pages 中的變更</span><span class="sxs-lookup"><span data-stu-id="00b18-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="00b18-170">移除的變更:"HrefAttribute"方法</span><span class="sxs-lookup"><span data-stu-id="00b18-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="00b18-171">`HrefAttribute`方法`WebPage`類別已被移除。</span><span class="sxs-lookup"><span data-stu-id="00b18-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="00b18-172">此協助程式用來編碼 Url 中不安全的字元。</span><span class="sxs-lookup"><span data-stu-id="00b18-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="00b18-173">它不再需要因為 ASP.NET Razor 自動編碼字串。</span><span class="sxs-lookup"><span data-stu-id="00b18-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="00b18-174">(使用新`Html.Raw`方法以呈現未編碼的字串。)</span><span class="sxs-lookup"><span data-stu-id="00b18-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="00b18-175">變更： 語法宣告式 」@helper」 變更的協助程式</span><span class="sxs-lookup"><span data-stu-id="00b18-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="00b18-176">在 Beta 3 版本中，ASP.NET 會變更它會使用建立的協助程式的剖析`@helper`語法。</span><span class="sxs-lookup"><span data-stu-id="00b18-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="00b18-177">基本上，`@helper`語法現在會剖析為而不是做為區塊可以包含程式碼標記的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="00b18-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="00b18-178">因此，內部協助程式程式碼不需要括住`@{ }`區塊。</span><span class="sxs-lookup"><span data-stu-id="00b18-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="00b18-179">相反地，協助專家內部標記必須為明確包含在 HTML 項目或 ASP.NET Razor`<text></text>`標記。</span><span class="sxs-lookup"><span data-stu-id="00b18-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="00b18-180">例如，下列`@helper`Beta 3 版本中運作的語法：</span><span class="sxs-lookup"><span data-stu-id="00b18-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="00b18-181">在 Beta 3 版本中，您必須變更此協助程式，以看起來與下列範例：</span><span class="sxs-lookup"><span data-stu-id="00b18-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="00b18-182">請注意，`@{ }`不再使用中的協助程式的初始程式碼周圍的字元。</span><span class="sxs-lookup"><span data-stu-id="00b18-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="00b18-183">這是因為將協助程式的內容會視為預設做為程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="00b18-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="00b18-184">Helper 會呈現開頭為開頭的標記`<a>`標記。</span><span class="sxs-lookup"><span data-stu-id="00b18-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="00b18-185">如果協助專家必須在純文字或不含結尾標記的標記呈現 (例如，`<meta>`標記)，要呈現的內容必須在`<text></text>`標記。</span><span class="sxs-lookup"><span data-stu-id="00b18-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="00b18-186">「 WebPageContext.HttpContext"中移除變更：</span><span class="sxs-lookup"><span data-stu-id="00b18-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="00b18-187">`WebPageContext.HttpContext`屬性已經移除。</span><span class="sxs-lookup"><span data-stu-id="00b18-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="00b18-188">請改用 `HttpContext.Current` 。</span><span class="sxs-lookup"><span data-stu-id="00b18-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="00b18-189">(`WebPageContext.HttpContext`屬性只包裝這。)</span><span class="sxs-lookup"><span data-stu-id="00b18-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="00b18-190">移至新的封裝變更:"Facebook"helper</span><span class="sxs-lookup"><span data-stu-id="00b18-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="00b18-191">`Facebook` Helper 已移至*Facebook.Helper*程式庫，包括`Facebook`helper 和其他功能。</span><span class="sxs-lookup"><span data-stu-id="00b18-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="00b18-192">您必須安裝此文件庫個別封裝時，「 安裝協助程式使用封裝管理員 」 中所述教學課程中[開始使用 ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202889)。</span><span class="sxs-lookup"><span data-stu-id="00b18-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="00b18-193">變更： 移至新的組件的成員資格、 角色和安全性的類型</span><span class="sxs-lookup"><span data-stu-id="00b18-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="00b18-194">下列類型移到`WebMatrix.WebData`組件：</span><span class="sxs-lookup"><span data-stu-id="00b18-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="00b18-195">移至 System.Web.WebPages.dll 組件的變更:"TagBuilder 」 類別</span><span class="sxs-lookup"><span data-stu-id="00b18-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="00b18-196">`TagBuilde` r 類別已移至 System.Web.WebPages.dll 組件。</span><span class="sxs-lookup"><span data-stu-id="00b18-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="00b18-197">先前，這是 ASP.NET MVC 的一部分之組件中。</span><span class="sxs-lookup"><span data-stu-id="00b18-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="00b18-198">這項變更表示您沒有安裝 ASP.NET MVC，才能使用`TagBuilder`類別。</span><span class="sxs-lookup"><span data-stu-id="00b18-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="00b18-199">不過，類別仍處於`System.Web.Mvc`命名空間。</span><span class="sxs-lookup"><span data-stu-id="00b18-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="00b18-200">若要使用`TagBuilder`類別 （例如，在自訂 ASP.NET Razor helper)，您必須參考命名空間 (例如，藉由加入`@using System.Web.Mvc`您的程式碼)。</span><span class="sxs-lookup"><span data-stu-id="00b18-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="00b18-201">變更： 要求驗證語法已變更。「 驗證 」 類別中移除</span><span class="sxs-lookup"><span data-stu-id="00b18-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="00b18-202">在 Beta 3 版本中，若要停用驗證的個別欄位或一組欄位，您可以呼叫`Validation.Exclude`方法，傳入要從驗證排除之欄位的名稱。</span><span class="sxs-lookup"><span data-stu-id="00b18-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="00b18-203">Beta 3 版本，略過驗證提供一種新語法。</span><span class="sxs-lookup"><span data-stu-id="00b18-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="00b18-204">`Validation` Beta 3 中使用的方法被移除了。</span><span class="sxs-lookup"><span data-stu-id="00b18-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="00b18-205">如果有使用者嘗試上載的 HTML 標記 （例如，藉由使用豐富的文字編輯器 頁面上） 未停用要求驗證，網站就會報告類似錯誤*偵測到具有潛在危險性的 Request.Form 值從用戶端*，而且無法接受使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="00b18-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="00b18-206">如果您停用要求驗證，您必須手動檢查並確定它不會不包含有潛在危險的標記或指令碼使用類似的使用者輸入[Microsoft 反跨網站指令碼程式庫 V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)。</span><span class="sxs-lookup"><span data-stu-id="00b18-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="00b18-207">若要停用自動要求驗證，請呼叫`Request.Unvalidated`方法，將欄位或其他您想要略過要求驗證的 post 物件的名稱傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="00b18-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="00b18-208">您可以使用這個方法來略過驗證的任何項目中`Form`， `QueryString`， `Cookies`，和`ServerVariables`集合。</span><span class="sxs-lookup"><span data-stu-id="00b18-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="00b18-209">下列範例顯示如何使用`Unvalidated`方法：</span><span class="sxs-lookup"><span data-stu-id="00b18-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="00b18-210">含有 Razor 語法的 ASP.NET Web Pages 的已知的問題</span><span class="sxs-lookup"><span data-stu-id="00b18-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="00b18-211">問題： 未預期的行為時使用的自訂使用者資料表的成員資格</span><span class="sxs-lookup"><span data-stu-id="00b18-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="00b18-212">若要初始化 ASP.NET Razor 網站的成員資格提供者，您呼叫`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="00b18-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="00b18-213">(在 WebMatrix 中，入門網站範本包含在這個方法的呼叫 *\_AppStart.cshtml*檔案。)如果`autoCreateTables`這個方法的參數設定為 true (根據預設，它設定為入門網站範本中，則為 true)，以及如果無法辨識的資料表名稱傳遞至方法 （第二個參數），此方法不會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="00b18-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="00b18-214">相反地，它會自動建立資料表。</span><span class="sxs-lookup"><span data-stu-id="00b18-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="00b18-215">如果您想要使用自訂使用者資料表的成員資格，但傳遞至錯誤的資料表名稱，這可能會有問題`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="00b18-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="00b18-216">因為方法不會預設產生錯誤如果您指定的資料表不存在，而且它改為建立新的資料表，應用程式會無法運作。</span><span class="sxs-lookup"><span data-stu-id="00b18-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="00b18-217">不過，必須使用自訂使用者資料表 （和中的欄位） 的應用程式程式碼最後可能意外的錯誤會失敗。</span><span class="sxs-lookup"><span data-stu-id="00b18-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="00b18-218">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-218">**Workaround**</span></span>  
> <span data-ttu-id="00b18-219">請確定名稱傳入`InitializeDatabaseConnection`方法比對的使用者設定檔資料表中的成員資格資料庫，或請確定`autoCreateTables`參數設定為 false。</span><span class="sxs-lookup"><span data-stu-id="00b18-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="00b18-220">問題: 「 無法產生 SQL Server 的使用者執行個體 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="00b18-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="00b18-221">如果 WebMatrix Web 應用程式會使用 SQL Server Express 和 Windows 7 或 Windows Server 2008 R2 上執行 IIS 7.5，您可能會看到錯誤指出 SQL Server 無法在執行階段擷取使用者的本機應用程式路徑。</span><span class="sxs-lookup"><span data-stu-id="00b18-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="00b18-222">**因應措施**請確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有這類應用程式的根資料夾以及子資料夾的讀取/寫入權限*應用程式\_資料*.</span><span class="sxs-lookup"><span data-stu-id="00b18-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="00b18-223">知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。</span><span class="sxs-lookup"><span data-stu-id="00b18-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="00b18-224">問題： 在 Visual Studio 中的自訂組件 (Dll) 的命名空間不自動匯入</span><span class="sxs-lookup"><span data-stu-id="00b18-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="00b18-225">如果您在 Visual Studio 中的專案中使用自訂組件，這些組件中宣告的命名空間不自動匯入在設計階段。</span><span class="sxs-lookup"><span data-stu-id="00b18-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="00b18-226">如此一來，自訂類型的參考可能無法辨識在設計階段，並會標示為無法辨識中的 Visual Studio （使用 「 波浪線 」）。</span><span class="sxs-lookup"><span data-stu-id="00b18-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="00b18-227">只能在 Visual Studio; 中的設計階段就會發生此問題應用程式本身會正確地執行。</span><span class="sxs-lookup"><span data-stu-id="00b18-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="00b18-228">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-228">**Workaround**</span></span>  
> <span data-ttu-id="00b18-229">包含`using`陳述式 (`imports`在 Visual Basic 中) 會參考在設計階段無法辨識的實體。</span><span class="sxs-lookup"><span data-stu-id="00b18-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="00b18-230">問題： Visual Studio IntelliSense 和專案範本僅適用於 ASP.NET MVC 3 版</span><span class="sxs-lookup"><span data-stu-id="00b18-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="00b18-231">安裝 ASP.NET Web Pages 不也會安裝 tools for Visual Studio 等的 ASP.NET Web Pages 應用程式的 IntelliSense 和專案範本。</span><span class="sxs-lookup"><span data-stu-id="00b18-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="00b18-232">**因應措施**要使用 IntelliSense 和專案範本在 Visual Studio 中的 ASP.NET Web Pages 應用程式，安裝 ASP.NET MVC 3 RC 透過 Web Platform Installer 或[獨立安裝程式](https://go.microsoft.com/fwlink/?LinkID=191797)。</span><span class="sxs-lookup"><span data-stu-id="00b18-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="00b18-233">問題: 「&lt;helper&gt;找不到類別 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="00b18-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="00b18-234">在升級到 Beta 3 之後，您可能會看到錯誤的協助程式類別 (比方說，`Facebook`類別) 不到。</span><span class="sxs-lookup"><span data-stu-id="00b18-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="00b18-235">從 Beta 2 開始，並繼續在 Beta 3，協助專家都已移至，您必須明確地安裝封裝中。</span><span class="sxs-lookup"><span data-stu-id="00b18-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="00b18-236">現有的站台不會升級到包含這些封裝。這包括中的站台*\My Documents\IISExpress*或*\My Documents\My 網站*資料夾。</span><span class="sxs-lookup"><span data-stu-id="00b18-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="00b18-237">特別是，您會看到此錯誤如果您使用中的預設站台*我網站*(網站 1)，其中包含的參考`Twitter`協助程式。</span><span class="sxs-lookup"><span data-stu-id="00b18-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="00b18-238">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-238">**Workaround**</span></span>  
> <span data-ttu-id="00b18-239">註解呼叫任何 helper 在網站中，執行 *\_Admin*頁面，然後安裝封裝或封裝包含您想要使用的 helper。</span><span class="sxs-lookup"><span data-stu-id="00b18-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="00b18-240">安裝封裝之後，您可以參考協助程式行取消註解。</span><span class="sxs-lookup"><span data-stu-id="00b18-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="00b18-241">問題： Beta 3 ASP.NET Razor 組件部署至 Bin 資料夾可能不適用於裝載站台</span><span class="sxs-lookup"><span data-stu-id="00b18-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="00b18-242">如果您將 ASP.NET Web Pages 網站部署至裝載站台，而且您將 ASP.NET Razor Beta 3 組件部署至站台的*Bin*資料夾中，您可能會遇到錯誤，包括下列：</span><span class="sxs-lookup"><span data-stu-id="00b18-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="00b18-243">如果主機服務提供者已安裝 ASP.NET Web Pages Beta 1 組件伺服器的全域應用程式快取 (GAC)，也可能會發生。</span><span class="sxs-lookup"><span data-stu-id="00b18-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="00b18-244">在 GAC 中的組件透過組件安裝在本機以取得優先順序*Bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="00b18-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="00b18-245">**因應措施**請連絡您裝載的提供者，以確認您看到的錯誤是因為提供者的版本之間的衝突組件的內容。</span><span class="sxs-lookup"><span data-stu-id="00b18-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="00b18-246">若是如此，要求主機服務提供者更新伺服器的 GAC 中的組件。</span><span class="sxs-lookup"><span data-stu-id="00b18-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="00b18-247">問題： 讀取摘要或其他外部資料透過 proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="00b18-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="00b18-248">如果執行站台伺服器是 proxy 伺服器後方，您可能需要設定中的 proxy 資訊*Web.config*檔案才能讀取來自網站外部的資訊。</span><span class="sxs-lookup"><span data-stu-id="00b18-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="00b18-249">例如，如果您使用`ReCaptcha`helper，協助專家會與 reCAPTCHA 服務通訊，但可能會封鎖您的 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="00b18-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="00b18-250">同樣地，用於 ASP.NET Web Pages 中，例如封裝管理員 中，所使用的摘要可能需要 proxy 組態。</span><span class="sxs-lookup"><span data-stu-id="00b18-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="00b18-251">如果您遇到問題，使用外部服務，或使用封裝摘要中的，將下列項目放入您的應用程式根目錄*Web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="00b18-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="00b18-252">如需有關如何設定 proxy 伺服器的詳細資訊，請參閱[ &lt;proxy&gt;項目 （網路設定）](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="00b18-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="00b18-253">問題: 「 無法載入 Microsoft.Web.Infrastructure.dll 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="00b18-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="00b18-254">如果您先前安裝的 Beta 1 版本的 ASP.NET 網頁使用 Razor 語法，然後再安裝 Beta 3 版本，除了在 GAC 中安裝所有適當的組件*Microsoft.Web.Infrastructure.dll*。</span><span class="sxs-lookup"><span data-stu-id="00b18-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="00b18-255">因此，當您執行 ASP.NET Razor 頁面時，您看到錯誤指出， *Microsoft.Web.Infrastructure.dll*無法載入。</span><span class="sxs-lookup"><span data-stu-id="00b18-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="00b18-256">如果您載入 Beta 3 版本乾淨的電腦上，則不會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="00b18-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="00b18-257">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-257">**Workaround**</span></span>  
> <span data-ttu-id="00b18-258">在 [控制台] 解除安裝 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="00b18-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="00b18-259">然後重新安裝 Beta 3 版本。</span><span class="sxs-lookup"><span data-stu-id="00b18-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="00b18-260">問題： 解除安裝.NET Framework 第 4 版會停用含有 Razor 語法的 ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="00b18-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="00b18-261">如果您解除安裝.NET Framework 第 4 版，然後重新安裝時，會停用含有 Razor 語法的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="00b18-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="00b18-262">頁面*.cshtml*延伸模組無法正確執行。</span><span class="sxs-lookup"><span data-stu-id="00b18-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="00b18-263">ASP.NET Web Pages 電腦根目錄中註冊組件*Web.config*檔案，並移除.NET Framework 中移除該檔案。</span><span class="sxs-lookup"><span data-stu-id="00b18-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="00b18-264">重新安裝.NET Framework 會安裝新版本的組態檔中，但不會新增 ASP.NET Web Pages 組件的參考。</span><span class="sxs-lookup"><span data-stu-id="00b18-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="00b18-265">**因應措施**之後重新安裝.NET Framework，請重新安裝 ASP.NET Web Pages 含有 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="00b18-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="00b18-266">這樣會加入下列項目加入*Web.config*電腦根目錄，這通常在下列位置中的檔案：</span><span class="sxs-lookup"><span data-stu-id="00b18-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="00b18-267">問題： 使用 ASP.NET 的 Bin 資料夾中的組件先前部署的應用程式發生錯誤</span><span class="sxs-lookup"><span data-stu-id="00b18-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="00b18-268">在部署期間，ASP.NET Web Pages 組件複本 (例如， *Microsoft.WebPages.dll*) 至*Bin*資料夾的伺服器上的網站。</span><span class="sxs-lookup"><span data-stu-id="00b18-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="00b18-269">(這可能會發生自動部署，或因為開發人員明確複製組件。)不過，安裝 Beta 3 版本時，錯誤就會發生，例如找不到特定類型的錯誤。</span><span class="sxs-lookup"><span data-stu-id="00b18-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="00b18-270">這是因為 Beta 3 版本的 ASP.NET Web Pages 類型數目移動到不同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="00b18-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="00b18-271">**因應措施** </span><span class="sxs-lookup"><span data-stu-id="00b18-271">**Workaround** </span></span>  
> <span data-ttu-id="00b18-272">清除*Bin*資料夾部署的應用程式，將新的組件複製到資料夾 （或重新部署應用程式），然後重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="00b18-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="00b18-273">問題： 沒有副檔名的 Url 會找不到在 IIS 7 或 IIS 7.5 上的.cshtml/.vbhtml 檔案</span><span class="sxs-lookup"><span data-stu-id="00b18-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="00b18-274">在 IIS 7 或 IIS 7.5 上，具有類似下列的 URL 不要求找不到具有頁面*.cshtml*或*.vbhtml*延伸模組：</span><span class="sxs-lookup"><span data-stu-id="00b18-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> <span data-ttu-id="00b18-275">因為 URL 重寫沒有啟用預設的 IIS 7 或 IIS 7.5，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="00b18-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="00b18-276">Likeliest 案例是看不到問題時測試使用在本機 IIS Express，但它時遇到您將您的網站部署至裝載的網站。</span><span class="sxs-lookup"><span data-stu-id="00b18-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="00b18-277">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="00b18-278">如果您有伺服器電腦的控制權，在伺服器電腦上安裝更新中所述[的更新功能可讓某些 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。</span><span class="sxs-lookup"><span data-stu-id="00b18-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="00b18-279">如果您沒有在伺服器電腦的控制權 （例如，您要部署至裝載的網站），加上網站的下列*Web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="00b18-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="00b18-280">問題： 使用相同的應用程式中的 Web 應用程式專案或 ASP.NET MVC 與 ASP.NET Web 網頁</span><span class="sxs-lookup"><span data-stu-id="00b18-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="00b18-281">如果您使用 ASP.NET Web Pages Web 應用程式專案或 ASP.NET MVC 應用程式中，您可能會看到錯誤， *WebPageHttpApplication*找不到。</span><span class="sxs-lookup"><span data-stu-id="00b18-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="00b18-282">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-282">**Workaround**</span></span>  
> <span data-ttu-id="00b18-283">如果您收到這個錯誤，請變更應用程式所衍生自的基底類別。</span><span class="sxs-lookup"><span data-stu-id="00b18-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="00b18-284">在*Global.asax*檔案中，將以下這一行：</span><span class="sxs-lookup"><span data-stu-id="00b18-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="00b18-285">為此值：</span><span class="sxs-lookup"><span data-stu-id="00b18-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="00b18-286">這實際上會反轉變更導入 Beta 1 版本的 ASP.NET Web Pages 中含有 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="00b18-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="00b18-287">問題： 應用程式部署到沒有 SQL Server Compact 安裝的電腦</span><span class="sxs-lookup"><span data-stu-id="00b18-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="00b18-288">包含 SQL Server Compact 資料庫的應用程式可以執行 SQL Server Compact 不安裝所在的電腦上。</span><span class="sxs-lookup"><span data-stu-id="00b18-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="00b18-289">Microsoft WebMatrix Beta 3 自動為您複製這些二進位檔，並執行適當*Web.config*檔案轉換。</span><span class="sxs-lookup"><span data-stu-id="00b18-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="00b18-290">**因應措施**如果您要複製這些檔案，並進行*Web.config*檔案變更，手動執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="00b18-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="00b18-291">資料庫引擎組件，以複製*Bin*資料夾 （及其子資料夾） 的目標電腦上的應用程式：</span><span class="sxs-lookup"><span data-stu-id="00b18-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="00b18-292">複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **至** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="00b18-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="00b18-293">複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **至** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="00b18-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="00b18-294">複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **至** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="00b18-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="00b18-295">在網站的根資料夾中，建立或開啟*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="00b18-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="00b18-296">(在 WebMatrix Beta 3 中，此檔案類型才可用如果您按一下**所有**中**選擇檔案型別** 對話方塊。)</span><span class="sxs-lookup"><span data-stu-id="00b18-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="00b18-297">將下列項目新增為子系**&lt;組態&gt;**項目 (不是在內 **&lt;system.web&gt;** 項目):</span><span class="sxs-lookup"><span data-stu-id="00b18-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="00b18-298">問題： 中度信任在 Visual Basic 中無法運作的資料庫和 WebGrid 協助程式</span><span class="sxs-lookup"><span data-stu-id="00b18-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="00b18-299">如果您使用 Visual Basic (建立*.vbhtml*檔案)，則`Database`和`WebGrid`如果應用程式會設定為使用度信任，協助專家將無法運作。</span><span class="sxs-lookup"><span data-stu-id="00b18-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="00b18-300">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-300">**Workaround**</span></span>  
> <span data-ttu-id="00b18-301">暫時設定應用程式使用完全信任。</span><span class="sxs-lookup"><span data-stu-id="00b18-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="00b18-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="00b18-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="00b18-303">問題： 無法辨識"Encrypt"屬性</span><span class="sxs-lookup"><span data-stu-id="00b18-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="00b18-304">無法辨識 SQL Server Compact 4.0`Encrypt`屬性`SqlCeConnection`類別。</span><span class="sxs-lookup"><span data-stu-id="00b18-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="00b18-305">您不應該使用這個屬性來加密資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="00b18-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="00b18-306">`Encrypt`屬性在 SQL Server Compact 3.5 版本中已被取代，並僅為回溯相容性已保留。</span><span class="sxs-lookup"><span data-stu-id="00b18-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="00b18-307">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-307">**Workaround**</span></span>  
> <span data-ttu-id="00b18-308">使用`Encryption Mode`屬性`SqlCeConnection`類別來加密 SQL Server Compact 4.0 資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="00b18-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="00b18-309">下列範例示範如何建立加密的 SQL Server Compact 4.0 資料庫使用`Encryption Mode`屬性：</span><span class="sxs-lookup"><span data-stu-id="00b18-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
>  
> [!code-csharp[Main](beta3/samples/sample11.cs)]
>  
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="00b18-310">若要變更現有的 SQL Server Compact 4.0 資料庫的加密模式，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="00b18-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
>  
> [!code-csharp[Main](beta3/samples/sample13.cs)]
>  
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="00b18-311">若要加密的未加密的 SQL Server Compact 4.0 資料庫，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="00b18-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
>  
> [!code-csharp[Main](beta3/samples/sample15.cs)]
>  
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="00b18-312">Microsoft Visual c + + 2008年執行階段程式庫所需的問題：</span><span class="sxs-lookup"><span data-stu-id="00b18-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="00b18-313">原生 Dll 的 SQL Server Compact 4.0 需要 Microsoft Visual c + + 2008年執行階段程式庫 （x86、 IA64 和 x64）、 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="00b18-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="00b18-314">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-314">**Workaround**</span></span>  
> <span data-ttu-id="00b18-315">安裝.NET Framework 3.5 SP1。</span><span class="sxs-lookup"><span data-stu-id="00b18-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="00b18-316">這也會安裝 Visual c + + 2008年執行階段程式庫 SP1。</span><span class="sxs-lookup"><span data-stu-id="00b18-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="00b18-317">您可以從下列位置下載的程式庫：</span><span class="sxs-lookup"><span data-stu-id="00b18-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="00b18-318">Microsoft Visual c + + 2008 Service Pack 1 可轉散發套件 ATL Security Update</span><span class="sxs-lookup"><span data-stu-id="00b18-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="00b18-319">請注意，安裝.NET Framework 2.0，3.0，或 4 不*不*安裝 SP1 的 windows Visual c + + 2008年執行階段程式庫。</span><span class="sxs-lookup"><span data-stu-id="00b18-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="00b18-320">問題： 如果 SQL Server Compact 安裝在電腦上安裝.NET Framework 之前，其提供者非變異名稱未註冊.NET Framework machine.config 檔案中</span><span class="sxs-lookup"><span data-stu-id="00b18-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="00b18-321">SQL Server Compact 可以安裝在未安裝.NET Framework 安裝，因為 SQL Server Compact 一定需要.NET framework 的電腦上。</span><span class="sxs-lookup"><span data-stu-id="00b18-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="00b18-322">如果再安裝 SQL Server Compact 安裝既不.NET Framework 3.5 版或 4，SQL Server Compact 安裝程式不會註冊在其提供者非變異名稱*machine.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="00b18-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="00b18-323">任何應用程式所使用的 SQL Server Compact 中的項目*machine.config*檔案將會失敗。</span><span class="sxs-lookup"><span data-stu-id="00b18-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="00b18-324">中的非變異名稱登錄項目*machine.config*看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="00b18-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="00b18-325">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-325">**Workaround**</span></span>  
> <span data-ttu-id="00b18-326">解除安裝 SQL Server Compact 4.0 CTP1。</span><span class="sxs-lookup"><span data-stu-id="00b18-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="00b18-327">下載並安裝.NET Framework 的完整版本，從下列位置：</span><span class="sxs-lookup"><span data-stu-id="00b18-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="00b18-328">Microsoft.NET Framework 3.5 Service pack 1 （完整套件）</span><span class="sxs-lookup"><span data-stu-id="00b18-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="00b18-329">Microsoft.NET Framework 4.0 版本 （完整套件）</span><span class="sxs-lookup"><span data-stu-id="00b18-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="00b18-330">然後重新安裝[SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)。</span><span class="sxs-lookup"><span data-stu-id="00b18-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="00b18-331">安裝應用程式</span><span class="sxs-lookup"><span data-stu-id="00b18-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="00b18-332">問題： 安裝應用程式可能需要很長的時間。 如果使用者的 [我的文件] 資料夾重新導向到網路共用</span><span class="sxs-lookup"><span data-stu-id="00b18-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="00b18-333">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-333">**Workaround**</span></span>  
> <span data-ttu-id="00b18-334">無。</span><span class="sxs-lookup"><span data-stu-id="00b18-334">None.</span></span> <span data-ttu-id="00b18-335">應用程式可能需要一段，若要安裝，但會正確安裝。</span><span class="sxs-lookup"><span data-stu-id="00b18-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="00b18-336">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="00b18-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="00b18-337">問題： 站台後可能無法運作如果 「 目的地 URL 」 欄位不以 http:// 或 https:// 前置發行</span><span class="sxs-lookup"><span data-stu-id="00b18-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="00b18-338">在**發佈設定**對話方塊中，如果目的地 URL 開頭不是`http://`或`https://`，在部署之後，站台可能無法運作。</span><span class="sxs-lookup"><span data-stu-id="00b18-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="00b18-339">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-339">**Workaround**</span></span>  
> <span data-ttu-id="00b18-340">請確定發行的網站中的目的地 URL 之前，先**發行設定**對話方塊開頭`http://`或`https://`。</span><span class="sxs-lookup"><span data-stu-id="00b18-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="00b18-341">問題： 發行 MySQL 資料庫失敗並出現錯誤 「 無法發行資料庫。</span><span class="sxs-lookup"><span data-stu-id="00b18-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="00b18-342">這種情況的遠端資料庫無法執行指令碼。 」</span><span class="sxs-lookup"><span data-stu-id="00b18-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="00b18-343">針對數個原因會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="00b18-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="00b18-344">您可以看到此錯誤的其中一個原因是，如果將資料庫指令碼包含單引號字元 （'），而且目的地 MySQL 資料庫的預設字元集不為 utf-8。</span><span class="sxs-lookup"><span data-stu-id="00b18-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="00b18-345">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-345">**Workaround**</span></span>  
> <span data-ttu-id="00b18-346">設定遠端的 MySQL 資料庫設定為 utf-8 的預設字元。</span><span class="sxs-lookup"><span data-stu-id="00b18-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="00b18-347">其他問題</span><span class="sxs-lookup"><span data-stu-id="00b18-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="00b18-348">問題： 搜尋/篩選器無法用於報表中分組： 問題類型排序</span><span class="sxs-lookup"><span data-stu-id="00b18-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="00b18-349">當您執行報表針對站台，如果您輸入的文字*URL 所篩選*方塊，然後按一下*搜尋*，會發生任何事。</span><span class="sxs-lookup"><span data-stu-id="00b18-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="00b18-350">這是因為這個控制項沒有作用時*Group By*報告的狀態會設為*問題類型排序*，預設值。</span><span class="sxs-lookup"><span data-stu-id="00b18-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="00b18-351">**因應措施**中*Group By*  索引標籤的 功能區中，按一下  *URL*來分組的項目由其來源 URL。</span><span class="sxs-lookup"><span data-stu-id="00b18-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="00b18-352">處於此狀態中的文字方塊和按鈕來篩選項目會運作。</span><span class="sxs-lookup"><span data-stu-id="00b18-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="00b18-353">問題： 無法執行 IIS Express 與 WCF 應用程式</span><span class="sxs-lookup"><span data-stu-id="00b18-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="00b18-354">瀏覽至 WCF 應用程式會產生類似下列的其中一個錯誤：</span><span class="sxs-lookup"><span data-stu-id="00b18-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="00b18-355">*無法載入檔案或組件 ' Microsoft.Web.Administration，Version = 7.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 或其中一個相依性。系統找不到指定的檔案。*</span><span class="sxs-lookup"><span data-stu-id="00b18-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="00b18-356">這是因為 IIS Express 的 Beta 版本不支援預設的 WCF。</span><span class="sxs-lookup"><span data-stu-id="00b18-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="00b18-357">**因應措施**使用任何一項因應措施 (因應措施 #2 需要 Microsoft Windows Vista 或更新版本):</span><span class="sxs-lookup"><span data-stu-id="00b18-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="00b18-358">複製*Microsoft.Web.dll*和*Microsoft.Web.Administration.dll* WebMatrix 安裝位置，若要從組件*bin* WCF 目錄應用程式。</span><span class="sxs-lookup"><span data-stu-id="00b18-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="00b18-359">根據預設，在安裝 WebMatrix *Microsoft WebMatrix*下系統的子資料夾*Program Files*資料夾。</span><span class="sxs-lookup"><span data-stu-id="00b18-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="00b18-360">在 Microsoft Windows Vista 或更新版本中，建立組件中的符號連結*bin*使用下列命令的目錄。</span><span class="sxs-lookup"><span data-stu-id="00b18-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="00b18-361">（這種方法有優點在於它不會建立一份組件）。</span><span class="sxs-lookup"><span data-stu-id="00b18-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="00b18-362">在 GAC 中安裝兩個組件。</span><span class="sxs-lookup"><span data-stu-id="00b18-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="00b18-363">從提升權限的提示字元，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="00b18-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="00b18-364">問題： WebMatrix Beta 3 不能執行需要提高權限的特定工作</span><span class="sxs-lookup"><span data-stu-id="00b18-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="00b18-365">WebMatrix Beta 3 是無法執行需要提高權限，例如在下列情況下安裝其他元件的特定工作項目：</span><span class="sxs-lookup"><span data-stu-id="00b18-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="00b18-366">在 Windows Vista 或 Windows 7 上，您使用沒有系統管理權限的帳戶登入和使用者帳戶控制 (UAC) 已停用。</span><span class="sxs-lookup"><span data-stu-id="00b18-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="00b18-367">您正在使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。</span><span class="sxs-lookup"><span data-stu-id="00b18-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="00b18-368">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-368">**Workaround**</span></span>  
> <span data-ttu-id="00b18-369">WebMatrix Beta 3 中大部分的工作不需要系統管理權限。</span><span class="sxs-lookup"><span data-stu-id="00b18-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="00b18-370">對於執行，您可以執行操作，因為系統管理員，或請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="00b18-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="00b18-371">在 Windows Vista 或 Windows 7 上，啟用 UAC。</span><span class="sxs-lookup"><span data-stu-id="00b18-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="00b18-372">在 Windows XP 中，將使用者新增至本機 Administrators 安全性群組。</span><span class="sxs-lookup"><span data-stu-id="00b18-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="00b18-373">問題: 「 站台從 Web 組件庫 」 已停用</span><span class="sxs-lookup"><span data-stu-id="00b18-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="00b18-374">**從 Web 組件庫網站**選項已停用，如果未安裝 Web Platform Installer 3.0。</span><span class="sxs-lookup"><span data-stu-id="00b18-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="00b18-375">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-375">**Workaround**</span></span>  
> <span data-ttu-id="00b18-376">安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。</span><span class="sxs-lookup"><span data-stu-id="00b18-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="00b18-377">問題： 在 Windows Server 2003 上 IIS Express 不會啟動為非系統管理使用者</span><span class="sxs-lookup"><span data-stu-id="00b18-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="00b18-378">Windows Server 2003，當您啟動頁面，或啟動 IIS Express，IIS Express 不會啟動。</span><span class="sxs-lookup"><span data-stu-id="00b18-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="00b18-379">Web 網頁會顯示錯誤，指出已由非系統管理使用者啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="00b18-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="00b18-380">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-380">**Workaround**</span></span>  
> <span data-ttu-id="00b18-381">系統管理使用者身分來啟動 WebMatrix Beta 3。</span><span class="sxs-lookup"><span data-stu-id="00b18-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="00b18-382">如需詳細資訊，請參閱下列知識庫文件：</span><span class="sxs-lookup"><span data-stu-id="00b18-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="00b18-383">非系統管理使用者啟動的應用程式無法接聽應用程式執行 Windows Vista、 Windows Server 2003 或 Windows XP 中所在之電腦的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="00b18-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="00b18-384">問題： Google Chrome 並不是執行選項</span><span class="sxs-lookup"><span data-stu-id="00b18-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="00b18-385">Google Chrome 瀏覽器在清單中未顯示**執行**上**首頁** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="00b18-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="00b18-386">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-386">**Workaround**</span></span>  
> <span data-ttu-id="00b18-387">Google Chrome 的某些版本中請勿本身正確向註冊 Windows 中的預設程式 功能。</span><span class="sxs-lookup"><span data-stu-id="00b18-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="00b18-388">因應措施，啟動 Google Chrome，請按一下*自訂和控制 Google Chrome*功能表上，按一下 *選項*，然後按一下 *請 Google Chrome 預設瀏覽器*。</span><span class="sxs-lookup"><span data-stu-id="00b18-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="00b18-389">問題: 「 外部索引鍵 」 對話方塊中不允許輸入主要金鑰</span><span class="sxs-lookup"><span data-stu-id="00b18-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="00b18-390">**外部索引鍵**對話方塊不允許輸入從主索引鍵資料表的主索引鍵的名稱。</span><span class="sxs-lookup"><span data-stu-id="00b18-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="00b18-391">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-391">**Workaround**</span></span>  
> <span data-ttu-id="00b18-392">這是有意如此。</span><span class="sxs-lookup"><span data-stu-id="00b18-392">This is intentional.</span></span> <span data-ttu-id="00b18-393">您不需要輸入主索引鍵資料表的主索引鍵的名稱。</span><span class="sxs-lookup"><span data-stu-id="00b18-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="00b18-394">問題: 「 關聯性 」 按鈕已停用</span><span class="sxs-lookup"><span data-stu-id="00b18-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="00b18-395">**關聯性**按鈕底下**資料表**索引標籤中**資料庫**工作區中已停用 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="00b18-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="00b18-396">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-396">**Workaround**</span></span>  
> <span data-ttu-id="00b18-397">無。</span><span class="sxs-lookup"><span data-stu-id="00b18-397">None.</span></span> <span data-ttu-id="00b18-398">SQL Server Compact 不支援資料表之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="00b18-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="00b18-399">問題： 參數化的 SQL 查詢會擲回例外狀況</span><span class="sxs-lookup"><span data-stu-id="00b18-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="00b18-400">在 SQL Server Compact 4.0，如果您未指定資料類型例如`SqlDbType`或`DbType`執行查詢時，發生例外狀況參數化查詢中的參數。</span><span class="sxs-lookup"><span data-stu-id="00b18-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="00b18-401">**因應措施**</span><span class="sxs-lookup"><span data-stu-id="00b18-401">**Workaround**</span></span>  
> <span data-ttu-id="00b18-402">明確地設定參數的資料類型，例如`SqlDbType`或`DbType`。</span><span class="sxs-lookup"><span data-stu-id="00b18-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="00b18-403">這一點十分重要，在 BLOB 資料類型的情況下 (`image`和`ntext`)。</span><span class="sxs-lookup"><span data-stu-id="00b18-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="00b18-404">使用程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="00b18-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="00b18-405">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="00b18-405">For More Information</span></span>

<span data-ttu-id="00b18-406">如需 WebMatrix Beta 3 的詳細資訊，請參閱下列網站：</span><span class="sxs-lookup"><span data-stu-id="00b18-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="00b18-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="00b18-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="00b18-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="00b18-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="00b18-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="00b18-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="00b18-410">© 2010 Microsoft Corporation。</span><span class="sxs-lookup"><span data-stu-id="00b18-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="00b18-411">All Rights Reserved.</span><span class="sxs-lookup"><span data-stu-id="00b18-411">All Rights Reserved.</span></span> <span data-ttu-id="00b18-412">[使用規定](https://msdn.microsoft.com/en-us/cc300389.aspx)。</span><span class="sxs-lookup"><span data-stu-id="00b18-412">[Terms of Use](https://msdn.microsoft.com/en-us/cc300389.aspx).</span></span>
