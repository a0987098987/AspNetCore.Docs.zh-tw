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
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案
====================
> Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案

9 年 11 月 2010

## <a name="contents"></a>內容

- [概觀](#Overview)
- [安裝](#Installation_Notes)
- [新的功能、 變更和 Beta 3 版本的已知問題](#Known_Issues)

    - [WebMatrix 安裝問題](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [安裝應用程式](#Known_Issues_Installing_Applications)
    - [發行應用程式](#Known_Issues_Publishing_Applications)
    - [其他問題](#Known_Issues_Other_Issues)
- [如需詳細資訊](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>總覽

> Microsoft WebMatrix beta 版是免費的 web 開發堆疊，以分鐘為單位的安裝。 它整合了 web 伺服器與資料庫和程式設計架構來建立單一的整合體驗。 您可以使用 WebMatrix Beta 來簡化的方式撰寫程式碼、 測試及發佈您自己的 ASP.NET 或 PHP 網站，或您可以使用 WebMatrix beta 版來開始使用 DotNetNuke、 Umbraco、 WordPress 或 Joomla 等熱門的開放原始碼應用程式建立新網站。 WebMatrix beta 版會使用相同的功能強大的 web 伺服器、 資料庫引擎和架構環境，將會在網際網路上，讓從開發轉換到生產環境，順利且流暢地執行您的網站。


<a id="Installation_Notes"></a>

## <a name="installation"></a>安裝

> 若要安裝 WebMatrix Beta 3，您使用[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。 您已安裝 Web Platform Installer 之後，您可以使用它來安裝 WebMatrix Beta 3。
> 
> 如果您在安裝期間遇到問題，請參閱[疑難排解 Microsoft Web Platform Installer 的問題](https://go.microsoft.com/fwlink/?LinkId=196212)。


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>發行應用程式的指示

> 請參閱[發行應用程式的逐步指示](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>新的功能，變更，andKnown 問題

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix Beta 3 的安裝

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>問題： WebMatrix Beta 3 現在只適用於支援 Microsoft.NET Framework 4 的平台

> WebMatrix Beta 需要.NET Framework 第 4 版。 在某些情況下，WebMatrix Beta 安裝程式，也可讓您嘗試安裝不支援的組態集的一部分的平台上。 特別是，Windows Vista SP1 更新不會讓您開始安裝 WebMatrix beta 版，但.NET Framework 4 元件會失敗並封鎖您的安裝。
> 
> **因應措施**  
> 安裝在支援的平台，包括：
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 (含) 以後版本
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>問題︰ 無法安裝 WebMatrix Beta 3，如果未使用 Microsoft Visual Studio 2008 SP1 安裝 Microsoft Visual Studio 2008

> **因應措施**  
> 安裝[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en)從 Microsoft 下載中心取得。


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>問題： SQL Server Compact 4.0 的某些組件不會安裝在 GAC 中

> 當您在 64 位元電腦上安裝 SQL Server Compact 4.0，以及電腦只有.NET Framework 3.5 SP1 用戶端安裝設定檔時，SQL Server Compact 4.0 的 managed 組件不會放在全域組件快取 (GAC) 中。 不會安裝在 GAC 中的 managed 組件包括：
> 
> - *System.Data.SqlServerCe.dll* (ado.net)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **因應措施**  
> 解除安裝 SQL Server Compact 4.0。 下載並安裝完整版的.NET Framework 3.5 SP1，請從下列位置：  
>   
> [Microsoft.NET Framework 3.5 Service pack 1 （完整套件）](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> 然後重新安裝 SQL Server Compact 4.0。


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>問題： 無法解除安裝 SQL Server Compact 使用命令列

> 解除安裝的 SQL Server Compact 使用命令列選項在此版本中無法運作。
> 
> **因應措施**  
> 使用*程式和功能*Windows 控制台中解除安裝 Microsoft SQL Server Compact 4.0。


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

本節的文件說明新功能、 變更和 Beta 3 版本的 ASP.NET Web Pages 含有 Razor 語法的已知的問題。

- [新功能](#NewFeatures)
- [變更](#Changes)
- [問題](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Beta 3 for ASP.NET 網頁使用 Razor 語法的新功能

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>新功能: 「 Html.Raw"方法會呈現未編碼的標記

> 新`Html.Raw`方法可讓您為標記，而不是轉譯編碼的輸出中呈現 HTML 標記。 （根據預設，ASP.NET Razor 將字串編碼再轉譯它們。）其語法為：
> 
> `Html.Raw(value)`
> 
> 下列範例顯示如何使用 `Html.Raw`：
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Beta 3 for ASP.NET 網頁使用 Razor 語法中的變更

#### <a name="change-hrefattribute-method-removed"></a>移除的變更:"HrefAttribute 」 方法

> `HrefAttribute`方法的`WebPage`類別已移除。 此協助程式用來編碼在 Url 中不安全的字元。 它不再需要因為 ASP.NET Razor 自動編碼字串。 (使用新`Html.Raw`方法以呈現未編碼的字串。)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>變更： 語法宣告式 「@helper」 變更的協助程式

> 在 Beta 3 版本中，ASP.NET 會變更它會使用所建立的協助程式的剖析`@helper`語法。 基本上，`@helper`語法現在會剖析為程式碼區塊而不是做為區塊的標記，可以包含程式碼。 因此，在協助程式碼不需要括在`@{ }`區塊。 相反地，標記協助程式必須明確包含在 HTML 項目或 ASP.NET Razor`<text></text>`標記。
> 
> 例如，下列`@helper`Beta 3 版本中運作的語法：
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> 在 Beta 3 版本中，您必須變更此協助程式，以下列範例所示：
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> 請注意，`@{ }`不再使用協助程式中的初始程式碼周圍的字元。 這是因為內容的協助程式預設情況下被視為程式碼區塊。 Helper 會呈現開啟的開頭的標記`<a>`標記。 如果協助專家必須轉譯純文字] 或 [不包含結尾標記的標記 (例如`<meta>`標記)，要呈現的內容必須位於`<text></text>`標記。


#### <a name="change-webpagecontexthttpcontext-removed"></a>變更:"WebPageContext.HttpContext 」 移除

> `WebPageContext.HttpContext`已移除屬性。 請改用 `HttpContext.Current`。 (`WebPageContext.HttpContext`屬性直接包裝這。)


#### <a name="change-facebook-helper-moved-to-new-package"></a>移至新的封裝變更:"Facebook"協助程式

> `Facebook`協助程式已移至*Facebook.Helper*程式庫，其中包含`Facebook`helper 和其他功能。 您必須安裝此程式庫為個別的套件，「 安裝協助程式使用封裝管理員 」 中所述在本教學課程[Getting Started with ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202889)。


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>變更： 移至新的組件的成員資格、 角色和安全性的類型

> 下列類型移到`WebMatrix.WebData`組件：
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>移至 System.Web.WebPages.dll 組件的變更:"TagBuilder 」 類別

> `TagBuilde` r 類別已移至 System.Web.WebPages.dll 組件。 這在以前是 ASP.NET MVC 的一部分的組件中。 這項變更表示您沒有安裝，才能使用 ASP.NET MVC`TagBuilder`類別。
> 
> 不過，此類別是仍在`System.Web.Mvc`命名空間。 若要使用`TagBuilder`類別 （例如，在自訂 ASP.NET Razor 協助程式），您必須參考的命名空間 (例如，藉由加入`@using System.Web.Mvc`您的程式碼)。


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>變更： 要求驗證語法已變更;移除的 「 驗證 」 類別

> 在 Beta 3 版本中，若要停用驗證個別欄位或一組欄位，您可以呼叫`Validation.Exclude`方法並傳入要從驗證排除的欄位名稱。 略過驗證的 Beta 3 版本提供一種新語法。 `Validation` Beta 3 中所使用的方法已被移除。
> 
> > [!NOTE]
> > 如果有使用者嘗試使用上傳的 HTML 標記 （例如 rtf 編輯器頁面上） 未停用要求驗證，網站會報告類似的錯誤*從用戶端的偵測到有潛在危險的Request.Form值*，而且無法接受使用者輸入。 如果您停用要求驗證，您必須手動檢查並確定它不會不包含有潛在危險的標記或指令碼使用類似的使用者輸入[Microsoft 反跨網站指令碼程式庫 V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)。
> 
> 
> 若要停用自動要求驗證，請呼叫`Request.Unvalidated`方法，將欄位或其他您希望略過要求驗證的 post 物件的名稱傳遞給它。 您可以使用這個方法來略過驗證中的任何項目`Form`， `QueryString`， `Cookies`，和`ServerVariables`集合。 下列範例示範如何使用`Unvalidated`方法：
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>含有 Razor 語法的 ASP.NET Web 網頁的已知的問題

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>問題： 未預期的行為時使用的自訂使用者資料表的成員資格

> 若要初始化 ASP.NET Razor 網站的成員資格提供者，請呼叫`WebSecurity.InitializeDatabaseConnection`方法。 (在 WebMatrix 中，入門網站範本會包含在這個方法呼叫 *\_AppStart.cshtml*檔案。)如果`autoCreateTables`這個方法的參數設定為 true (根據預設，它設定為在入門網站範本，則為 true)，以及無法辨識的資料表名稱傳遞至方法 （第二個參數），如果方法沒有擲回錯誤。 相反地，它會自動建立資料表。
> 
> 如果您想要使用自訂使用者資料表的成員資格，但傳遞至錯誤的資料表名稱，這可能會有問題`WebSecurity.InitializeDatabaseConnection`方法。 因為方法不會預設引發錯誤如果您指定的資料表不存在，而且它改為建立新的資料表，應用程式可以在運作中。 不過，依賴您的自訂使用者資料表 （和中的欄位） 的應用程式程式碼最後可能發生未預期的錯誤會失敗。
> 
> **因應措施**  
> 請確定名稱傳入`InitializeDatabaseConnection`方法比對的使用者設定檔資料表中的成員資格資料庫中，或請確定`autoCreateTables`參數設定為 false。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>問題: 「 無法產生 SQL Server 的使用者執行個體 」 錯誤

> 如果 WebMatrix Web 應用程式會使用 SQL Server Express，而且在 Windows 7 或 Windows Server 2008 R2 上執行 IIS 7.5，您可能會看到錯誤指出 SQL Server 無法在執行階段擷取使用者的本機應用程式路徑。
> 
> **因應措施**確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有這類的應用程式根資料夾以及子資料夾的讀取/寫入權限*應用程式\_資料*. 知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>問題： 在 Visual Studio 中，自訂組件 (Dll) 的命名空間會不自動匯入

> 如果您在 Visual Studio 專案中使用自訂組件，這些組件中宣告的命名空間不會自動匯入在設計階段。 如此一來，自訂類型參考可能無法辨識在設計階段，且會標示為無法辨識在 Visual Studio 中 （使用 「 波浪線 」）。 只能在設計階段，在 Visual Studio 中，就會發生此問題應用程式本身可以正確執行。
> 
> **因應措施**  
> 包含`using`陳述式 (`imports` Visual Basic 中)，參考在設計階段無法辨識的實體。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>問題： Visual Studio IntelliSense 和專案範本僅適用於 ASP.NET MVC 3 版

> 安裝 ASP.NET Web Pages 不會也安裝 tools for Visual Studio ASP.NET Web Pages 應用程式的 IntelliSense 和專案範本等。
> 
> **因應措施**若要使用 IntelliSense 和專案範本在 Visual Studio 中的 ASP.NET Web Pages 應用程式，安裝 ASP.NET MVC 3 RC 透過 Web Platform Installer 或[獨立安裝](https://go.microsoft.com/fwlink/?LinkID=191797)。


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>問題: 「&lt;協助程式&gt;找不到類別 」 錯誤

> 在升級到 Beta 3 之後，您可能會看到錯誤，協助程式類別 (例如`Facebook`類別) 找不到不。 從 Beta 2 開始，並繼續在 Beta 3 中，協助程式都已移至您必須明確安裝的套件中。 現有的站台並不會升級為包含這些封裝中;這包括中的站台 *\My Documents\IISExpress* 或是 *\My Documents\My 網站* 資料夾。 特別是，您會看到這個錯誤如果您使用中的預設站台*My Sites* (網站 1)，其中包含的參考`Twitter`協助程式。
> 
> **因應措施**  
> 在網站中，執行任何協助程式的呼叫註解 *\_Admin*頁面上，並安裝包含您想要使用的協助程式的封裝。 您已安裝封裝之後，您可以參考協助程式行取消註解。


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>問題： Beta 3 ASP.NET Razor 組件部署至 Bin 資料夾可能不適用於裝載站台

> 如果您裝載的網站，部署將 ASP.NET Web Pages 網站，而且您將 ASP.NET Razor Beta 3 的組件部署至站台的*Bin*資料夾中，您可能會遇到錯誤，包括下列：
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> 如果裝載提供者已安裝到伺服器的全域應用程式快取 (GAC 中) 的 ASP.NET Web Pages Beta 1 組件，會發生這項目。 組件在 GAC 中的透過本機上安裝的組件中取得優先順序*Bin*資料夾。
> 
> **因應措施**連絡以確認您看到的錯誤是提供者的版本之間發生衝突，因此您裝載提供者的組件和您。 如果是這樣，要求主機服務提供者更新伺服器的 GAC 中的組件。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>問題： 讀取摘要或其他外部的資料，透過 proxy 伺服器

> 如果執行站台的伺服器是在 proxy 伺服器後方，您可能需要設定中的 proxy 資訊*Web.config*檔案以讀取來自網站外部的資訊。 例如，如果您使用`ReCaptcha`協助程式，協助專家會與 reCAPTCHA 服務通訊，但可能會封鎖您的 proxy 伺服器。 同樣地，使用 ASP.NET Web Pages 中，例如套件管理員 中，所使用的摘要的摘要可能需要 proxy 組態。
> 
> 如果您遇到問題，使用外部服務，或使用套件摘要中的，將下列項目放到您的應用程式根目錄*Web.config*檔案：
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> 如需有關如何設定 proxy 伺服器的詳細資訊，請參閱 < [ &lt;proxy&gt;項目 （網路設定）](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN 網站上。


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>問題: 「 無法載入 Microsoft.Web.Infrastructure.dll 」 錯誤

> 如果您先前安裝含有 Razor 語法的 Beta 1 版本的 ASP.NET Web Pages，然後再安裝 Beta 3 版本，所有適當的組件會安裝在 GAC 除外*Microsoft.Web.Infrastructure.dll*。 如此一來，當您執行 ASP.NET Razor 頁面，您會看到錯誤，指出*Microsoft.Web.Infrastructure.dll*無法載入。
> 
> 如果您載入 Beta 3 版本乾淨的電腦上，則不會發生此問題。
> 
> **因應措施**  
> 在 [控制台] 解除安裝 ASP.NET Web Pages。 然後重新安裝 Beta 3 版本。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>問題： 解除安裝.NET Framework 第 4 版會停用含有 Razor 語法的 ASP.NET Web Pages

> 如果您解除安裝.NET Framework 第 4 版，然後重新安裝時，會停用含有 Razor 語法的 ASP.NET Web Pages。 頁面 *.cshtml*延伸模組無法正確執行。 ASP.NET 網頁註冊組件在電腦根目錄*Web.config*檔案，並移除.NET Framework 中移除該檔案。 重新安裝.NET Framework 會安裝新版本的組態檔中，但不會新增 ASP.NET Web Pages 組件的參考。
> 
> **因應措施**之後重新安裝.NET Framework，請重新安裝 ASP.NET Web Pages 含有 Razor 語法。 這會將新增到下列項目*Web.config*電腦根目錄，這通常是在下列位置中的檔案：  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>問題： 先前使用 ASP.NET 的 Bin 資料夾中的組件部署的應用程式遇到錯誤

> 在部署期間，ASP.NET 網頁組件複本 (例如*Microsoft.WebPages.dll*) 來*Bin*網站伺服器上的資料夾。 (這在部署期間可能會自動發生，或因為開發人員明確複製的組件。)不過，安裝 Beta 3 版本時，錯誤就會發生，例如找不到特定類型的錯誤。 這是因為許多 ASP.NET Web Pages 類型時，已移至不同的命名空間上，針對 Beta 3 版本。
> 
> **因應措施**   
> 清除*Bin*資料夾部署的應用程式，將新的組件複製到資料夾 （或重新部署應用程式），然後重新啟動應用程式。


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>問題： 無副檔名 Url 找.cshtml/.vbhtml 檔案，在 IIS 7 或 IIS 7.5 上的不到

> 在 IIS 7 或 IIS 7.5 上，要求如下所示的 url 不能找到網頁具有 *.cshtml*或是 *.vbhtml*延伸模組：  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> 因為 URL 重寫未啟用預設的 IIS 7 或 IIS 7.5，就會發生問題。 Likeliest 案例是看不到問題時測試在本機上使用 IIS Express，但它時遇到您將您的網站部署至裝載的網站。
> 
> **因應措施**
> 
> - 如果您有伺服器電腦的控制權時，在伺服器電腦上安裝更新中所述[的更新功能，可讓特定 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。
> - 如果您沒有在伺服器電腦的控制權 （例如，您要部署到裝載的網站），加上網站的下列*Web.config*檔案：
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>問題： 會在相同的應用程式中使用 「 Web 應用程式專案或 ASP.NET MVC 和 ASP.NET 網頁

> 如果您已使用 ASP.NET Web Pages 中的 Web 應用程式專案或 ASP.NET MVC 應用程式，您可能會看到錯誤， *WebPageHttpApplication*找不到。
> 
> **因應措施**  
> 如果您收到這個錯誤，請變更應用程式所衍生自的基底類別。 在  *Global.asax*檔案中，變更下列這一行：
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> 為此值：
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> 這實際上會反轉變更導入 Beta 1 版本的 ASP.NET Web Pages 中含有 Razor 語法。


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>問題： 應用程式部署至沒有 SQL Server Compact 安裝的電腦

> 包含 SQL Server Compact 資料庫的應用程式可以執行 SQL Server Compact 不安裝所在的電腦上。 Microsoft WebMatrix Beta 3 自動為您複製這些二進位檔，並執行適當*Web.config*檔案轉換。
> 
> **因應措施**如果您需要複製這些檔案，並使*Web.config*檔案變更以手動的方式，執行下列動作：
> 
> 1. 資料庫引擎組件，以複製*Bin*目標電腦上的應用程式的資料夾 （和子資料夾）： 
> 
>     - 複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **要** *\Bin*
>     - 複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **來** *\Bin\x86*
>     - 複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **來** *\Bin\amd64*
> 2. 在網站的根資料夾中，建立或開啟*Web.config*檔案。 (在 WebMatrix Beta 3 中，這種檔案類型是如果您按一下可用**所有**中**選擇 [檔案類型**] 對話方塊。)
> 3. 將下列項目新增為子系 **&lt;configuration&gt;** 項目 (不是在內**&lt;system.web&gt;** 項目):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>問題： 中度信任在 Visual Basic 中無法運作資料庫和 WebGrid 協助程式

> 如果您使用 Visual Basic (建立 *.vbhtml*檔案)，則`Database`和`WebGrid`如果應用程式會設定為使用中度信任，協助程式將無法運作。
> 
> **因應措施**  
> 暫時設定應用程式使用完全信任。

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>問題： 無法辨識"Encrypt"屬性

> 無法辨識 SQL Server Compact 4.0`Encrypt`屬性`SqlCeConnection`類別。 您不應該使用這個屬性來加密資料庫檔案。 `Encrypt`屬性已被取代 SQL Server Compact 3.5 的版本中，並保留僅為回溯相容性。 
> 
> **因應措施**  
> 使用`Encryption Mode`屬性`SqlCeConnection`類別來加密 SQL Server Compact 4.0 資料庫檔案。 下列範例示範如何建立加密的 SQL Server Compact 4.0 資料庫使用`Encryption Mode`屬性：
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> 若要變更現有的 SQL Server Compact 4.0 資料庫的加密模式，執行下列作業：
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> 若要加密的未加密的 SQL Server Compact 4.0 資料庫，執行下列作業：
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Microsoft Visual c + + 2008年執行階段程式庫所需的問題：

> 原生 Dll 的 SQL Server Compact 4.0 需要 Microsoft Visual c + + 2008年執行階段程式庫 （IA64，x86 和 x64）、 Service Pack 1。
> 
> **因應措施**  
> 安裝.NET Framework 3.5 SP1。 這也會安裝 Visual c + + 2008年執行階段程式庫 SP1。 您可以從下列位置下載的程式庫：   
>   
> [Microsoft Visual c + + 2008 Service Pack 1 可轉散發套件的 ATL 安全性更新](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> 請注意，安裝.NET Framework 2.0、 3.0 或 4*不*安裝 SP1 的 windows Visual c + + 2008年執行階段程式庫。


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>問題： 如果 SQL Server Compact 安裝在電腦上安裝.NET Framework 之前，其提供者非變異名稱未註冊.NET Framework 的 machine.config 檔案中

> SQL Server Compact 可以安裝在沒有安裝，因為 SQL Server Compact 一定需要.NET framework 的.NET Framework 的電腦上。 如果您安裝 SQL Server Compact 安裝既不的.NET Framework 版本 3.5 或 4，SQL Server Compact 安裝程式不會註冊在其提供者非變異名稱*machine.config*檔案。 中的 SQL Server Compact 項目所依賴的任何應用程式*machine.config*檔案將會失敗。 中的非變異名稱註冊項目*machine.config*如下列範例所示：
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **因應措施**  
> 解除安裝 SQL Server Compact 4.0 CTP1。 下載並安裝.NET Framework 的完整版本，從下列位置：
> 
> [Microsoft.NET Framework 3.5 Service pack 1 （完整套件）](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft.NET Framework 4.0 版 （完整套件）](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> 然後重新安裝[SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)。


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>安裝應用程式

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>問題： 安裝應用程式可能需要很長的時間如果使用者的 [我的文件] 資料夾重新導向至網路共用

> **因應措施**  
> 無。 應用程式可能需要一段時間，若要安裝，但會正確安裝。


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>發行應用程式

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>問題： 站台後可能無法運作如果 [目的地 URL] 欄位不以 http:// 或 https:// 前置發行

> 在 [**發佈設定**] 對話方塊中，如果目的地 URL 開頭不是`http://`或`https://`，部署後，網站可能無法運作。
> 
> **因應措施**  
> 請確定發行的網站中的目的地 URL 之前，先**發佈設定** 對話方塊中的開頭`http://`或`https://`。


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>問題： 發行的 MySQL 資料庫失敗並出現錯誤 「 無法發行資料庫。 這就可能發生遠端資料庫無法執行指令碼。 」

> 有許多原因可能會發生錯誤。 您可以看到此錯誤的其中一個原因是如果將資料庫指令碼包含單引號字元 （'），而且目的地 MySQL 資料庫的預設字元集不為 utf-8。
> 
> **因應措施**  
> 設定預設遠端 MySQL 資料庫設定為 utf-8 的字元。


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>其他問題

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>問題： 搜尋/篩選無法用於報表中群組依據： 問題類型

> 當您執行報表網站，如果您輸入的文字*依 URL 篩選*方塊，然後按一下*搜尋*，會發生任何事。 這是因為此控制項無法運作*Group By*報告的狀態會設為*問題類型*，這是預設值。
> 
> **因應措施**中*Group By*  索引標籤的 功能區中，按一下  *URL*將由其來源 URL 的項目。 處於此狀態中的文字方塊和按鈕，以篩選項目會運作。


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>問題： WCF 應用程式會使用 IIS Express 來執行失敗

> 瀏覽至 WCF 應用程式會產生類似下列的其中一個錯誤：
> 
> *無法載入檔案或組件 ' Microsoft.Web.Administration，版本 = 7.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 或其中一個相依性。系統找不到指定的檔案。*
> 
> 這是因為 IIS Express 的 Beta 版本不支援預設的 WCF。
> 
> **因應措施**使用任何其中一項因應措施 (因應措施 2 需要 Microsoft Windows Vista 或更新版本):
> 
> 
> 1. 複製*Microsoft.Web.dll*並*Microsoft.Web.Administration.dll*組件，WebMatrix 安裝位置，以從*bin* WCF 目錄應用程式。 根據預設，在安裝 WebMatrix *Microsoft WebMatrix*子資料夾下的系統*Program Files*資料夾。
> 2. Microsoft Windows Vista 或更新版本中，建立中的組件的符號連結*bin*目錄使用下列命令。 （這種方法有它不會建立一份組件的優點）。
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. 安裝到 GAC 中的兩個組件。 從提升權限的提示字元，執行下列命令：
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>問題： WebMatrix Beta 3 現在無法執行需要提高權限的特定工作

> WebMatrix Beta 3 已無法執行需要提高權限，例如在下列情況下安裝其他元件的特定工作項目：
> 
> - 在 Windows Vista 或 Windows 7 上，您登入不具系統管理權限的帳戶，並已停用使用者帳戶控制 (UAC)。
> - 您使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。
> 
> **因應措施**  
> 在 WebMatrix Beta 3 中的大部分工作不需要系統管理權限。 對於這些，您可以執行此作業，身為管理員，或遵循下列步驟：
> 
> - 在 Windows Vista 或 Windows 7 上，啟用 UAC。
> - 在 Windows XP 上加入 Administrators 安全性群組中的使用者。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>問題: 「 站台從 Web 組件庫 「 已停用

> **從 Web 組件庫網站**選項會停用，如果未安裝 Web Platform Installer 3.0。
> 
> **因應措施**  
> 安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>問題： 在 Windows Server 2003 上 IIS Express 不會啟動非系統管理使用者

> Windows Server 2003，當您啟動頁面，或啟動 IIS Express，IIS Express 不會啟動。 針對 Web 網頁會顯示錯誤，指出已由非系統管理使用者啟動應用程式。
> 
> **因應措施**  
> 啟動 WebMatrix Beta 3 的系統管理使用者身分。 如需詳細資訊，請參閱下列知識庫文件：  
>   
> [非系統管理使用者所啟動的應用程式無法接聽 HTTP 流量的應用程式正在執行 Windows Vista、 Windows Server 2003 或 Windows XP 的電腦。](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>問題： Google Chrome 不提供執行選項

> Google Chrome 不會顯示在清單下方的瀏覽器**執行**上**首頁** 索引標籤。
> 
> **因應措施**  
> Google Chrome 的某些版本中請勿自行正確向註冊在 Windows 中的預設程式 功能。 因應措施，請啟動 Google Chrome，請按一下*自訂和控制 Google Chrome*功能表上，按一下*選項*，然後按一下*讓 Google Chrome 我的預設瀏覽器*。


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>問題: 「 外部索引鍵 」 對話方塊中不允許輸入主索引鍵

> **外部索引鍵**對話方塊不允許您輸入主索引鍵的資料表主索引鍵的名稱。
> 
> **因應措施**  
> 這是刻意設計的。 您不需要輸入主索引鍵資料表的主索引鍵的名稱。


#### <a name="issue-the-relationships-button-is-disabled"></a>問題: 「 關聯性 」 按鈕已停用

> **關聯性**下方的按鈕**資料表**索引標籤**資料庫**工作區已停用 SQL Server Compact 資料庫。
> 
> **因應措施**  
> 無。 SQL Server Compact 不支援資料表之間的關聯性。


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>問題： 參數化的 SQL 查詢會擲回例外狀況

> 在 SQL Server Compact 4.0，如果您未指定的資料類型這類`SqlDbType`或`DbType`執行查詢時，發生例外狀況的參數化查詢中的參數。
> 
> **因應措施**  
> 明確地設定這類參數的資料類型`SqlDbType`或`DbType`。 這點很重要，如果 BLOB 資料類型 (`image`和`ntext`)。 使用程式碼，如下所示：
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>詳細資訊

如需有關 WebMatrix Beta 3 的詳細資訊，請參閱下列網站：

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. All Rights Reserved. [使用規定](https://msdn.microsoft.cos/cc300389.aspx)。
