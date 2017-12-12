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
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案
====================
> Web Matrix 及 ASP.NET Web Pages (Razor) Beta 3 版讀我檔案

9 年 11 月 2010

## <a name="contents"></a>內容

- [概觀](#Overview)
- [安裝](#Installation_Notes)
- [新功能、 變更和 Beta 3 版本中的已知問題](#Known_Issues)

    - [WebMatrix 安裝問題](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [安裝應用程式](#Known_Issues_Installing_Applications)
    - [發行應用程式](#Known_Issues_Publishing_Applications)
    - [其他問題](#Known_Issues_Other_Issues)
- [如需詳細資訊](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>概觀

> Microsoft WebMatrix Beta 是以分鐘為單位安裝免費的網頁開發堆疊。 它可以整合網頁伺服器與資料庫和程式設計架構來建立單一整合體驗。 您可以使用 WebMatrix Beta 來簡化程式碼、 測試和發佈專屬 ASP.NET 或 PHP 網站的方式，或您可以使用 WebMatrix Beta 來啟動新的網站使用熱門的開放原始碼應用程式，例如 DotNetNuke、 Umbraco、 WordPress 或 Joomla。 WebMatrix beta 版會使用相同的功能強大的 web 伺服器、 資料庫引擎和架構環境將在網際網路上，讓從開發轉換至實際執行環境，順利且流暢地執行您的網站。


<a id="Installation_Notes"></a>

## <a name="installation"></a>安裝

> 若要安裝 WebMatrix Beta 3，您使用[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。 安裝 Web Platform Installer 之後，您可以使用它來安裝 WebMatrix Beta 3。
> 
> 如果您在安裝期間遇到問題，請參閱[疑難排解 Microsoft Web Platform Installer 的問題](https://go.microsoft.com/fwlink/?LinkId=196212)。


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>發行應用程式的指示

> 請參閱[發行應用程式的逐步指示](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>新功能，變更，andKnown 問題

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix Beta 3 安裝

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>問題： WebMatrix Beta 3 才支援 Microsoft.NET Framework 4 平台上可用

> WebMatrix Beta 需要.NET Framework 第 4 版。 在某些情況下，WebMatrix beta 版安裝程式可讓您嘗試安裝不支援的組態集的一部分的平台上。 特別是，Windows Vista SP1 更新不會讓您開始安裝 WebMatrix Beta，但是.NET Framework 4 元件會失敗並封鎖您的安裝。
> 
> **因應措施**  
> 安裝在支援的平台，其中包括：
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 (含) 以後版本
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>如果未安裝 Microsoft Visual Studio 2008 SP1 已安裝的 Microsoft Visual Studio 2008 問題： 無法安裝 WebMatrix Beta 3

> **因應措施**  
> 安裝[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en)從 Microsoft 下載中心取得。


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>問題： SQL Server Compact 4.0 的某些組件不會安裝在 GAC 中

> 當您在 64 位元電腦上安裝 SQL Server Compact 4.0 而電腦只有.NET Framework 3.5 SP1 Client Profile 安裝時，SQL Server Compact 4.0 的 managed 組件不會放在全域組件快取 (GAC) 中。 不會安裝在 GAC 中的 managed 組件如下：
> 
> - *System.Data.SqlServerCe.dll* （ADO.NET 提供者）
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **因應措施**  
> 解除安裝 SQL Server Compact 4.0。 下載並安裝.NET Framework 3.5 SP1 的完整版本，從下列位置：  
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

文件的本節會說明新功能、 變更和 Beta 3 版本的 ASP.NET 網頁使用 Razor 語法的已知的問題。

- [新功能](#NewFeatures)
- [變更](#Changes)
- [問題](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>含有 Razor 語法的 beta 版 3 的 ASP.NET Web Pages 中的新功能

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>新增項目: 「 Html.Raw"方法會呈現未編碼的標記

> 新`Html.Raw`方法可讓您為標記，而不是呈現編碼的輸出中呈現 HTML 標記。 （根據預設，ASP.NET Razor 編碼字串後再呈現它們。）其語法為：
> 
> `Html.Raw(value)`
> 
> 下列範例顯示如何使用 `Html.Raw`：
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>含有 Razor 語法的 beta 版 3 的 ASP.NET Web Pages 中的變更

#### <a name="change-hrefattribute-method-removed"></a>移除的變更:"HrefAttribute"方法

> `HrefAttribute`方法`WebPage`類別已被移除。 此協助程式用來編碼 Url 中不安全的字元。 它不再需要因為 ASP.NET Razor 自動編碼字串。 (使用新`Html.Raw`方法以呈現未編碼的字串。)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>變更： 語法宣告式 」@helper」 變更的協助程式

> 在 Beta 3 版本中，ASP.NET 會變更它會使用建立的協助程式的剖析`@helper`語法。 基本上，`@helper`語法現在會剖析為而不是做為區塊可以包含程式碼標記的程式碼區塊。 因此，內部協助程式程式碼不需要括住`@{ }`區塊。 相反地，協助專家內部標記必須為明確包含在 HTML 項目或 ASP.NET Razor`<text></text>`標記。
> 
> 例如，下列`@helper`Beta 3 版本中運作的語法：
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> 在 Beta 3 版本中，您必須變更此協助程式，以看起來與下列範例：
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> 請注意，`@{ }`不再使用中的協助程式的初始程式碼周圍的字元。 這是因為將協助程式的內容會視為預設做為程式碼區塊。 Helper 會呈現開頭為開頭的標記`<a>`標記。 如果協助專家必須在純文字或不含結尾標記的標記呈現 (例如，`<meta>`標記)，要呈現的內容必須在`<text></text>`標記。


#### <a name="change-webpagecontexthttpcontext-removed"></a>「 WebPageContext.HttpContext"中移除變更：

> `WebPageContext.HttpContext`屬性已經移除。 請改用 `HttpContext.Current` 。 (`WebPageContext.HttpContext`屬性只包裝這。)


#### <a name="change-facebook-helper-moved-to-new-package"></a>移至新的封裝變更:"Facebook"helper

> `Facebook` Helper 已移至*Facebook.Helper*程式庫，包括`Facebook`helper 和其他功能。 您必須安裝此文件庫個別封裝時，「 安裝協助程式使用封裝管理員 」 中所述教學課程中[開始使用 ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202889)。


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>變更： 移至新的組件的成員資格、 角色和安全性的類型

> 下列類型移到`WebMatrix.WebData`組件：
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>移至 System.Web.WebPages.dll 組件的變更:"TagBuilder 」 類別

> `TagBuilde` r 類別已移至 System.Web.WebPages.dll 組件。 先前，這是 ASP.NET MVC 的一部分之組件中。 這項變更表示您沒有安裝 ASP.NET MVC，才能使用`TagBuilder`類別。
> 
> 不過，類別仍處於`System.Web.Mvc`命名空間。 若要使用`TagBuilder`類別 （例如，在自訂 ASP.NET Razor helper)，您必須參考命名空間 (例如，藉由加入`@using System.Web.Mvc`您的程式碼)。


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>變更： 要求驗證語法已變更。「 驗證 」 類別中移除

> 在 Beta 3 版本中，若要停用驗證的個別欄位或一組欄位，您可以呼叫`Validation.Exclude`方法，傳入要從驗證排除之欄位的名稱。 Beta 3 版本，略過驗證提供一種新語法。 `Validation` Beta 3 中使用的方法被移除了。
> 
> > [!NOTE]
> > 如果有使用者嘗試上載的 HTML 標記 （例如，藉由使用豐富的文字編輯器 頁面上） 未停用要求驗證，網站就會報告類似錯誤*偵測到具有潛在危險性的 Request.Form 值從用戶端*，而且無法接受使用者輸入。 如果您停用要求驗證，您必須手動檢查並確定它不會不包含有潛在危險的標記或指令碼使用類似的使用者輸入[Microsoft 反跨網站指令碼程式庫 V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)。
> 
> 
> 若要停用自動要求驗證，請呼叫`Request.Unvalidated`方法，將欄位或其他您想要略過要求驗證的 post 物件的名稱傳遞給它。 您可以使用這個方法來略過驗證的任何項目中`Form`， `QueryString`， `Cookies`，和`ServerVariables`集合。 下列範例顯示如何使用`Unvalidated`方法：
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>含有 Razor 語法的 ASP.NET Web Pages 的已知的問題

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>問題： 未預期的行為時使用的自訂使用者資料表的成員資格

> 若要初始化 ASP.NET Razor 網站的成員資格提供者，您呼叫`WebSecurity.InitializeDatabaseConnection`方法。 (在 WebMatrix 中，入門網站範本包含在這個方法的呼叫 *\_AppStart.cshtml*檔案。)如果`autoCreateTables`這個方法的參數設定為 true (根據預設，它設定為入門網站範本中，則為 true)，以及如果無法辨識的資料表名稱傳遞至方法 （第二個參數），此方法不會擲回錯誤。 相反地，它會自動建立資料表。
> 
> 如果您想要使用自訂使用者資料表的成員資格，但傳遞至錯誤的資料表名稱，這可能會有問題`WebSecurity.InitializeDatabaseConnection`方法。 因為方法不會預設產生錯誤如果您指定的資料表不存在，而且它改為建立新的資料表，應用程式會無法運作。 不過，必須使用自訂使用者資料表 （和中的欄位） 的應用程式程式碼最後可能意外的錯誤會失敗。
> 
> **因應措施**  
> 請確定名稱傳入`InitializeDatabaseConnection`方法比對的使用者設定檔資料表中的成員資格資料庫，或請確定`autoCreateTables`參數設定為 false。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>問題: 「 無法產生 SQL Server 的使用者執行個體 」 錯誤

> 如果 WebMatrix Web 應用程式會使用 SQL Server Express 和 Windows 7 或 Windows Server 2008 R2 上執行 IIS 7.5，您可能會看到錯誤指出 SQL Server 無法在執行階段擷取使用者的本機應用程式路徑。
> 
> **因應措施**請確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有這類應用程式的根資料夾以及子資料夾的讀取/寫入權限*應用程式\_資料*. 知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>問題： 在 Visual Studio 中的自訂組件 (Dll) 的命名空間不自動匯入

> 如果您在 Visual Studio 中的專案中使用自訂組件，這些組件中宣告的命名空間不自動匯入在設計階段。 如此一來，自訂類型的參考可能無法辨識在設計階段，並會標示為無法辨識中的 Visual Studio （使用 「 波浪線 」）。 只能在 Visual Studio; 中的設計階段就會發生此問題應用程式本身會正確地執行。
> 
> **因應措施**  
> 包含`using`陳述式 (`imports`在 Visual Basic 中) 會參考在設計階段無法辨識的實體。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>問題： Visual Studio IntelliSense 和專案範本僅適用於 ASP.NET MVC 3 版

> 安裝 ASP.NET Web Pages 不也會安裝 tools for Visual Studio 等的 ASP.NET Web Pages 應用程式的 IntelliSense 和專案範本。
> 
> **因應措施**要使用 IntelliSense 和專案範本在 Visual Studio 中的 ASP.NET Web Pages 應用程式，安裝 ASP.NET MVC 3 RC 透過 Web Platform Installer 或[獨立安裝程式](https://go.microsoft.com/fwlink/?LinkID=191797)。


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>問題: 「&lt;helper&gt;找不到類別 」 錯誤

> 在升級到 Beta 3 之後，您可能會看到錯誤的協助程式類別 (比方說，`Facebook`類別) 不到。 從 Beta 2 開始，並繼續在 Beta 3，協助專家都已移至，您必須明確地安裝封裝中。 現有的站台不會升級到包含這些封裝。這包括中的站台*\My Documents\IISExpress*或*\My Documents\My 網站*資料夾。 特別是，您會看到此錯誤如果您使用中的預設站台*我網站*(網站 1)，其中包含的參考`Twitter`協助程式。
> 
> **因應措施**  
> 註解呼叫任何 helper 在網站中，執行 *\_Admin*頁面，然後安裝封裝或封裝包含您想要使用的 helper。 安裝封裝之後，您可以參考協助程式行取消註解。


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>問題： Beta 3 ASP.NET Razor 組件部署至 Bin 資料夾可能不適用於裝載站台

> 如果您將 ASP.NET Web Pages 網站部署至裝載站台，而且您將 ASP.NET Razor Beta 3 組件部署至站台的*Bin*資料夾中，您可能會遇到錯誤，包括下列：
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> 如果主機服務提供者已安裝 ASP.NET Web Pages Beta 1 組件伺服器的全域應用程式快取 (GAC)，也可能會發生。 在 GAC 中的組件透過組件安裝在本機以取得優先順序*Bin*資料夾。
> 
> **因應措施**請連絡您裝載的提供者，以確認您看到的錯誤是因為提供者的版本之間的衝突組件的內容。 若是如此，要求主機服務提供者更新伺服器的 GAC 中的組件。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>問題： 讀取摘要或其他外部資料透過 proxy 伺服器

> 如果執行站台伺服器是 proxy 伺服器後方，您可能需要設定中的 proxy 資訊*Web.config*檔案才能讀取來自網站外部的資訊。 例如，如果您使用`ReCaptcha`helper，協助專家會與 reCAPTCHA 服務通訊，但可能會封鎖您的 proxy 伺服器。 同樣地，用於 ASP.NET Web Pages 中，例如封裝管理員 中，所使用的摘要可能需要 proxy 組態。
> 
> 如果您遇到問題，使用外部服務，或使用封裝摘要中的，將下列項目放入您的應用程式根目錄*Web.config*檔案：
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> 如需有關如何設定 proxy 伺服器的詳細資訊，請參閱[ &lt;proxy&gt;項目 （網路設定）](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) MSDN 網站上。


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>問題: 「 無法載入 Microsoft.Web.Infrastructure.dll 」 錯誤

> 如果您先前安裝的 Beta 1 版本的 ASP.NET 網頁使用 Razor 語法，然後再安裝 Beta 3 版本，除了在 GAC 中安裝所有適當的組件*Microsoft.Web.Infrastructure.dll*。 因此，當您執行 ASP.NET Razor 頁面時，您看到錯誤指出， *Microsoft.Web.Infrastructure.dll*無法載入。
> 
> 如果您載入 Beta 3 版本乾淨的電腦上，則不會發生此問題。
> 
> **因應措施**  
> 在 [控制台] 解除安裝 ASP.NET Web Pages。 然後重新安裝 Beta 3 版本。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>問題： 解除安裝.NET Framework 第 4 版會停用含有 Razor 語法的 ASP.NET Web Pages

> 如果您解除安裝.NET Framework 第 4 版，然後重新安裝時，會停用含有 Razor 語法的 ASP.NET Web Pages。 頁面*.cshtml*延伸模組無法正確執行。 ASP.NET Web Pages 電腦根目錄中註冊組件*Web.config*檔案，並移除.NET Framework 中移除該檔案。 重新安裝.NET Framework 會安裝新版本的組態檔中，但不會新增 ASP.NET Web Pages 組件的參考。
> 
> **因應措施**之後重新安裝.NET Framework，請重新安裝 ASP.NET Web Pages 含有 Razor 語法。 這樣會加入下列項目加入*Web.config*電腦根目錄，這通常在下列位置中的檔案：  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>問題： 使用 ASP.NET 的 Bin 資料夾中的組件先前部署的應用程式發生錯誤

> 在部署期間，ASP.NET Web Pages 組件複本 (例如， *Microsoft.WebPages.dll*) 至*Bin*資料夾的伺服器上的網站。 (這可能會發生自動部署，或因為開發人員明確複製組件。)不過，安裝 Beta 3 版本時，錯誤就會發生，例如找不到特定類型的錯誤。 這是因為 Beta 3 版本的 ASP.NET Web Pages 類型數目移動到不同的命名空間。
> 
> **因應措施**   
> 清除*Bin*資料夾部署的應用程式，將新的組件複製到資料夾 （或重新部署應用程式），然後重新啟動應用程式。


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>問題： 沒有副檔名的 Url 會找不到在 IIS 7 或 IIS 7.5 上的.cshtml/.vbhtml 檔案

> 在 IIS 7 或 IIS 7.5 上，具有類似下列的 URL 不要求找不到具有頁面*.cshtml*或*.vbhtml*延伸模組：  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> 因為 URL 重寫沒有啟用預設的 IIS 7 或 IIS 7.5，就會發生此問題。 Likeliest 案例是看不到問題時測試使用在本機 IIS Express，但它時遇到您將您的網站部署至裝載的網站。
> 
> **因應措施**
> 
> - 如果您有伺服器電腦的控制權，在伺服器電腦上安裝更新中所述[的更新功能可讓某些 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。
> - 如果您沒有在伺服器電腦的控制權 （例如，您要部署至裝載的網站），加上網站的下列*Web.config*檔案：
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>問題： 使用相同的應用程式中的 Web 應用程式專案或 ASP.NET MVC 與 ASP.NET Web 網頁

> 如果您使用 ASP.NET Web Pages Web 應用程式專案或 ASP.NET MVC 應用程式中，您可能會看到錯誤， *WebPageHttpApplication*找不到。
> 
> **因應措施**  
> 如果您收到這個錯誤，請變更應用程式所衍生自的基底類別。 在*Global.asax*檔案中，將以下這一行：
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> 為此值：
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> 這實際上會反轉變更導入 Beta 1 版本的 ASP.NET Web Pages 中含有 Razor 語法。


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>問題： 應用程式部署到沒有 SQL Server Compact 安裝的電腦

> 包含 SQL Server Compact 資料庫的應用程式可以執行 SQL Server Compact 不安裝所在的電腦上。 Microsoft WebMatrix Beta 3 自動為您複製這些二進位檔，並執行適當*Web.config*檔案轉換。
> 
> **因應措施**如果您要複製這些檔案，並進行*Web.config*檔案變更，手動執行下列動作：
> 
> 1. 資料庫引擎組件，以複製*Bin*資料夾 （及其子資料夾） 的目標電腦上的應用程式： 
> 
>     - 複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **至** *\Bin*
>     - 複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **至** *\Bin\x86*
>     - 複製*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **至** *\Bin\amd64*
> 2. 在網站的根資料夾中，建立或開啟*Web.config*檔案。 (在 WebMatrix Beta 3 中，此檔案類型才可用如果您按一下**所有**中**選擇檔案型別** 對話方塊。)
> 3. 將下列項目新增為子系**&lt;組態&gt;**項目 (不是在內 **&lt;system.web&gt;** 項目):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>問題： 中度信任在 Visual Basic 中無法運作的資料庫和 WebGrid 協助程式

> 如果您使用 Visual Basic (建立*.vbhtml*檔案)，則`Database`和`WebGrid`如果應用程式會設定為使用度信任，協助專家將無法運作。
> 
> **因應措施**  
> 暫時設定應用程式使用完全信任。

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>問題： 無法辨識"Encrypt"屬性

> 無法辨識 SQL Server Compact 4.0`Encrypt`屬性`SqlCeConnection`類別。 您不應該使用這個屬性來加密資料庫檔案。 `Encrypt`屬性在 SQL Server Compact 3.5 版本中已被取代，並僅為回溯相容性已保留。 
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

> 原生 Dll 的 SQL Server Compact 4.0 需要 Microsoft Visual c + + 2008年執行階段程式庫 （x86、 IA64 和 x64）、 Service Pack 1。
> 
> **因應措施**  
> 安裝.NET Framework 3.5 SP1。 這也會安裝 Visual c + + 2008年執行階段程式庫 SP1。 您可以從下列位置下載的程式庫：   
>   
> [Microsoft Visual c + + 2008 Service Pack 1 可轉散發套件 ATL Security Update](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> 請注意，安裝.NET Framework 2.0，3.0，或 4 不*不*安裝 SP1 的 windows Visual c + + 2008年執行階段程式庫。


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>問題： 如果 SQL Server Compact 安裝在電腦上安裝.NET Framework 之前，其提供者非變異名稱未註冊.NET Framework machine.config 檔案中

> SQL Server Compact 可以安裝在未安裝.NET Framework 安裝，因為 SQL Server Compact 一定需要.NET framework 的電腦上。 如果再安裝 SQL Server Compact 安裝既不.NET Framework 3.5 版或 4，SQL Server Compact 安裝程式不會註冊在其提供者非變異名稱*machine.config*檔案。 任何應用程式所使用的 SQL Server Compact 中的項目*machine.config*檔案將會失敗。 中的非變異名稱登錄項目*machine.config*看起來像下列的範例：
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **因應措施**  
> 解除安裝 SQL Server Compact 4.0 CTP1。 下載並安裝.NET Framework 的完整版本，從下列位置：
> 
> [Microsoft.NET Framework 3.5 Service pack 1 （完整套件）](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft.NET Framework 4.0 版本 （完整套件）](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> 然後重新安裝[SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)。


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>安裝應用程式

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>問題： 安裝應用程式可能需要很長的時間。 如果使用者的 [我的文件] 資料夾重新導向到網路共用

> **因應措施**  
> 無。 應用程式可能需要一段，若要安裝，但會正確安裝。


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>發行應用程式

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>問題： 站台後可能無法運作如果 「 目的地 URL 」 欄位不以 http:// 或 https:// 前置發行

> 在**發佈設定**對話方塊中，如果目的地 URL 開頭不是`http://`或`https://`，在部署之後，站台可能無法運作。
> 
> **因應措施**  
> 請確定發行的網站中的目的地 URL 之前，先**發行設定**對話方塊開頭`http://`或`https://`。


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>問題： 發行 MySQL 資料庫失敗並出現錯誤 「 無法發行資料庫。 這種情況的遠端資料庫無法執行指令碼。 」

> 針對數個原因會發生錯誤。 您可以看到此錯誤的其中一個原因是，如果將資料庫指令碼包含單引號字元 （'），而且目的地 MySQL 資料庫的預設字元集不為 utf-8。
> 
> **因應措施**  
> 設定遠端的 MySQL 資料庫設定為 utf-8 的預設字元。


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>其他問題

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>問題： 搜尋/篩選器無法用於報表中分組： 問題類型排序

> 當您執行報表針對站台，如果您輸入的文字*URL 所篩選*方塊，然後按一下*搜尋*，會發生任何事。 這是因為這個控制項沒有作用時*Group By*報告的狀態會設為*問題類型排序*，預設值。
> 
> **因應措施**中*Group By*  索引標籤的 功能區中，按一下  *URL*來分組的項目由其來源 URL。 處於此狀態中的文字方塊和按鈕來篩選項目會運作。


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>問題： 無法執行 IIS Express 與 WCF 應用程式

> 瀏覽至 WCF 應用程式會產生類似下列的其中一個錯誤：
> 
> *無法載入檔案或組件 ' Microsoft.Web.Administration，Version = 7.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 或其中一個相依性。系統找不到指定的檔案。*
> 
> 這是因為 IIS Express 的 Beta 版本不支援預設的 WCF。
> 
> **因應措施**使用任何一項因應措施 (因應措施 #2 需要 Microsoft Windows Vista 或更新版本):
> 
> 
> 1. 複製*Microsoft.Web.dll*和*Microsoft.Web.Administration.dll* WebMatrix 安裝位置，若要從組件*bin* WCF 目錄應用程式。 根據預設，在安裝 WebMatrix *Microsoft WebMatrix*下系統的子資料夾*Program Files*資料夾。
> 2. 在 Microsoft Windows Vista 或更新版本中，建立組件中的符號連結*bin*使用下列命令的目錄。 （這種方法有優點在於它不會建立一份組件）。
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. 在 GAC 中安裝兩個組件。 從提升權限的提示字元，執行下列命令：
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>問題： WebMatrix Beta 3 不能執行需要提高權限的特定工作

> WebMatrix Beta 3 是無法執行需要提高權限，例如在下列情況下安裝其他元件的特定工作項目：
> 
> - 在 Windows Vista 或 Windows 7 上，您使用沒有系統管理權限的帳戶登入和使用者帳戶控制 (UAC) 已停用。
> - 您正在使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。
> 
> **因應措施**  
> WebMatrix Beta 3 中大部分的工作不需要系統管理權限。 對於執行，您可以執行操作，因為系統管理員，或請遵循下列步驟：
> 
> - 在 Windows Vista 或 Windows 7 上，啟用 UAC。
> - 在 Windows XP 中，將使用者新增至本機 Administrators 安全性群組。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>問題: 「 站台從 Web 組件庫 」 已停用

> **從 Web 組件庫網站**選項已停用，如果未安裝 Web Platform Installer 3.0。
> 
> **因應措施**  
> 安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>問題： 在 Windows Server 2003 上 IIS Express 不會啟動為非系統管理使用者

> Windows Server 2003，當您啟動頁面，或啟動 IIS Express，IIS Express 不會啟動。 Web 網頁會顯示錯誤，指出已由非系統管理使用者啟動應用程式。
> 
> **因應措施**  
> 系統管理使用者身分來啟動 WebMatrix Beta 3。 如需詳細資訊，請參閱下列知識庫文件：  
>   
> [非系統管理使用者啟動的應用程式無法接聽應用程式執行 Windows Vista、 Windows Server 2003 或 Windows XP 中所在之電腦的 HTTP 流量。](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>問題： Google Chrome 並不是執行選項

> Google Chrome 瀏覽器在清單中未顯示**執行**上**首頁** 索引標籤。
> 
> **因應措施**  
> Google Chrome 的某些版本中請勿本身正確向註冊 Windows 中的預設程式 功能。 因應措施，啟動 Google Chrome，請按一下*自訂和控制 Google Chrome*功能表上，按一下 *選項*，然後按一下 *請 Google Chrome 預設瀏覽器*。


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>問題: 「 外部索引鍵 」 對話方塊中不允許輸入主要金鑰

> **外部索引鍵**對話方塊不允許輸入從主索引鍵資料表的主索引鍵的名稱。
> 
> **因應措施**  
> 這是有意如此。 您不需要輸入主索引鍵資料表的主索引鍵的名稱。


#### <a name="issue-the-relationships-button-is-disabled"></a>問題: 「 關聯性 」 按鈕已停用

> **關聯性**按鈕底下**資料表**索引標籤中**資料庫**工作區中已停用 SQL Server Compact 資料庫。
> 
> **因應措施**  
> 無。 SQL Server Compact 不支援資料表之間的關聯性。


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>問題： 參數化的 SQL 查詢會擲回例外狀況

> 在 SQL Server Compact 4.0，如果您未指定資料類型例如`SqlDbType`或`DbType`執行查詢時，發生例外狀況參數化查詢中的參數。
> 
> **因應措施**  
> 明確地設定參數的資料類型，例如`SqlDbType`或`DbType`。 這一點十分重要，在 BLOB 資料類型的情況下 (`image`和`ntext`)。 使用程式碼如下所示：
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>詳細資訊

如需 WebMatrix Beta 3 的詳細資訊，請參閱下列網站：

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation。 All Rights Reserved. [使用規定](https://msdn.microsoft.com/en-us/cc300389.aspx)。
