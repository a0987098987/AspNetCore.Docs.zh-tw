---
uid: web-pages/readme/overview
title: WebMatrix 讀我檔案 |Microsoft 文件
author: rick-anderson
description: WebMatrix 和 ASP.NET Web Pages (Razor) 1.0 版讀我檔案
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
ms.locfileid: "30898966"
---
<a name="webmatrix-readme"></a>WebMatrix Readme
====================
13 年 1 月 2011

## <a name="contents"></a>內容

> [!NOTE]
> 此讀我檔案適用於 WebMatrix 1.0 版。


- [概觀](#Overview)
- [安裝](#Installation_Notes)
- [如何發佈應用程式](#InstructionsForPublishingApplications)
- [變更或問題](#ChangesAndIssues)

    - [WebMatrix 1.0 安裝](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [安裝應用程式](#Known_Issues_Installing_Applications)
    - [發行應用程式](#Known_Issues_Publishing_Applications)
- [如需詳細資訊](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>總覽

> Microsoft WebMatrix 1.0 是以分鐘為單位安裝免費的網頁開發堆疊。 它可以整合網頁伺服器與資料庫和程式設計架構來建立單一整合體驗。 您可以使用 WebMatrix 來簡化程式碼、 測試和發佈專屬 ASP.NET 或 PHP 網站的方式，或您可以使用 WebMatrix 來啟動新的網站使用熱門的開放原始碼應用程式，例如 DotNetNuke、 Umbraco、 WordPress 或 Joomla。 WebMatrix 使用相同的功能強大的 web 伺服器、 資料庫引擎和架構環境將在網際網路上，讓從開發轉換至實際執行環境，順利且流暢地執行您的網站。


<a id="Installation_Notes"></a>

## <a name="installation"></a>安裝

> 若要安裝 WebMatrix 1.0，您必須先安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。 安裝 Web Platform Installer 之後，您可以使用它來安裝 WebMatrix。
> 
> 如果您在安裝期間遇到問題，請參閱[疑難排解 Microsoft Web Platform Installer 的問題](https://go.microsoft.com/fwlink/?LinkId=196212)。


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>如何發佈應用程式

> 請參閱[發行應用程式的逐步指示](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>變更或問題

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 安裝問題

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>問題： WebMatrix 1.0 是只適用於支援 Microsoft.NET Framework 4 的平台

> .NET Framework 第 4 版是 WebMatrix 的必要項目。 在某些情況下，WebMatrix 1.0 安裝程式可讓您嘗試安裝不支援的組態集的一部分的平台上。 特別是，Windows Vista SP1 更新不會讓您開始安裝 WebMatrix，但是.NET Framework 4 元件會失敗並封鎖您的安裝。
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>問題： 無法安裝 WebMatrix 1.0，如果未安裝 Microsoft Visual Studio 2008 SP1 已安裝的 Microsoft Visual Studio 2008

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

文件的本節會說明新功能、 變更和已知的問題的 1.0 版的 ASP.NET 網頁使用 Razor 語法。

- [新功能](#NewFeatures)
- [變更](#Changes)
- [問題](#Issues)

#### <a id="NewFeatures"></a>  新功能

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>組態設定加入至停用封裝管理員的新增項目：

> 新`asp:AdminManagerEnabled`金鑰`<appSettings>`中的項目*web.config*檔案，可讓您完全停用封裝管理員。 此項目的預設值為 true，也就是說，如果它不會包含在*web.config*檔案，封裝管理員已啟用。 若要停用封裝管理員，加入下列項目加入*web.config*在網站的根目錄中的檔案：
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  變更

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>變更:"Webpages"索引鍵重新命名為"asp: AdminFolderVirtualPath"

> `webPages:AdminFolderVirtualPath`可以加入的索引鍵*web.config*指定的封裝管理員位置的檔案重新命名為使用`asp:`命名空間，而不是`webPages`命名空間。 如果您已經使用這個項目，您必須在組態檔重新命名它。


#### <a id="Issues"></a>  已知的問題

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>問題： 無法再辨識成員資格使用者的密碼

> 建立和儲存成員資格 （登入） 密碼的演算法已變更為更安全。 如此一來，將無法辨識針對 ASP.NET Razor Beta 版中建立的成員 （使用者） 儲存的密碼。 
> 
> **因應措施**如果網站尚未放到實際執行環境，從成員資格資料庫中移除的使用者記錄。 如果資料庫已上線，以程式設計方式重新產生現有的成員資格資料庫中的密碼。


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>問題： 未預期的行為時使用的自訂使用者資料表的成員資格

> 若要初始化 ASP.NET Razor 網站的成員資格提供者，您呼叫`WebSecurity.InitializeDatabaseConnection`方法。 (在 WebMatrix 中，入門網站範本包含在這個方法的呼叫 *\_AppStart.cshtml*檔案。)如果`autoCreateTables`這個方法的參數設定為 true (根據預設，它設定為入門網站範本中，則為 true)，以及如果無法辨識的資料表名稱傳遞至方法 （第二個參數），此方法不會擲回錯誤。 相反地，它會自動建立資料表。
> 
> 如果您想要使用自訂使用者資料表的成員資格，但傳遞至錯誤的資料表名稱，這可能會有問題`WebSecurity.InitializeDatabaseConnection`方法。 因為方法不會預設產生錯誤如果您指定的資料表不存在，而且它改為建立新的資料表，應用程式會無法運作。 不過，必須使用自訂使用者資料表 （和中的欄位） 的應用程式程式碼最後可能意外的錯誤會失敗。
> 
> **因應措施**  
> 請確定名稱傳入`InitializeDatabaseConnection`方法比對的使用者設定檔資料表中的成員資格資料庫，或請確定`autoCreateTables`參數設定為 false。


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>問題： 錯誤訊息 「 Admin 模組必須以 ~/App\_資料 」

> 在某些情況下，嘗試建立使用者，或使用 ASP.NET 成員資格系統可能會造成顯示錯誤頁面*Admin 模組必須存取 ~/App\_資料*。 會發生這個錯誤的帳戶下執行 IIS 或 IIS Express，並沒有建立和寫入權限*應用程式\_資料*在網站根目錄底下的資料夾。 
> 
> **因應措施**手動建立*應用程式\_資料*網站的資料夾。 請確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有讀取/寫入權限的應用程式的根資料夾以及子資料夾，例如應用程式\_資料。 知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>問題: 「 無法產生 SQL Server 的使用者執行個體 」 錯誤

> 如果 WebMatrix Web 應用程式會使用 SQL Server Express 和 Windows 7 或 Windows Server 2008 R2 上執行 IIS 7.5，您可能會看到錯誤指出 SQL Server 無法在執行階段擷取使用者的本機應用程式路徑。
> 
> **因應措施**請確定執行應用程式 (通常是 NETWORK SERVICE) 的 Windows 帳戶具有這類應用程式的根資料夾以及子資料夾的讀取/寫入權限*應用程式\_資料*. 知識庫文件中會提供更詳細的資訊[SQL Server Express 使用者 instancing 及 ASP.net Web 應用程式專案的問題](https://support.microsoft.com/kb/2002980)。


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>問題： 包含封裝管理員資源或封裝管理員密碼的檔案是版本底下 IIS 6.0 提供及更早版本

> 如果您使用 RC2 版本中，建立 ASP.NET Web Pages (Razor) 應用程式部署，而且應用程式包含*password.txt*或*packagesources.txt*底下 */App\_資料/admin*，IIS 6.0 會提供檔案若有要求，可能會公開您的封裝管理員執行個體的密碼。 
> 
> **因應措施**重新命名*password.txt*或*packagesources.txt*檔案*password.config*或*packagesources.config*.根據預設，IIS 6.0 不符合檔案 *.config*延伸模組。 (在 IIS 7 中中, 沒有任何檔案*應用程式\_資料*資料夾都提供服務，因此您不需要重新命名檔案。)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>問題： 解除安裝使用 Beta 3 版本安裝的封裝並未完全移除封裝元件

> 如果您已安裝的套件，在 Beta 3 版本中使用封裝管理員，並嘗試使用目前的版本解除安裝後，封裝未完全解除安裝。 使用封裝管理員**解除安裝**按鈕移除某些元件，但會保留封裝的程式庫程式碼，而且不會更新*package.config*檔案。
> 
> **因應措施**   
> 執行下列步驟：  
> 1. 刪除*應用程式\_Data\packages*資料夾。 這會移除所有封裝。   
> 2. 刪除*packages.config*在網站的根目錄中的檔案。


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>問題： 在 Visual Studio 中，叫用 web 架構封裝管理員會讓應用程式離線

> 如果您使用 Visual Studio (而非 WebMatrix) 中，而且使用 *\_admin*功能，以啟動封裝管理員，Visual Studio 會將應用程式離線，並將張貼*應用程式\_offline.htm*貼入網站根目錄，這會中斷您使用封裝管理員的能力。
> 
> [!NOTE]
> 如果新增、 移除或修改任何檔案，雖然使用 web 架構封裝管理員介面時，大部分都是會看到這種行為，會發生相同的行為*應用程式\_資料*資料夾。
> 
> **因應措施**   
> 若要使用 Visual Studio 中的封裝，使用 NuGet 延伸模組而非 web 架構封裝管理員。 如需資訊，請參閱[NuGet 文件](https://docs.microsoft.com/nuget/)。 如果您正在使用中的其他檔案*應用程式\_資料*資料夾，請考慮將檔案以避免此問題的其他位置。 如果不可行，請刪除*應用程式\_offline.htm*手動檔案，或等候站台都重新上線時自動 （依預設，在 30 秒後）。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>問題： Visual Studio IntelliSense 和專案範本僅適用於 ASP.NET MVC 3 版

> 安裝 ASP.NET Web Pages 不也會安裝 tools for Visual Studio 等的 ASP.NET Web Pages 應用程式的 IntelliSense 和專案範本。
> 
> **因應措施**要使用 IntelliSense 和專案範本在 Visual Studio 中的 ASP.NET Web Pages 應用程式，安裝 ASP.NET MVC 3 RC 透過 Web Platform Installer 或[獨立安裝程式](https://go.microsoft.com/fwlink/?LinkID=191797)。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>問題： 讀取摘要或其他外部資料透過 proxy 伺服器

> 如果執行站台伺服器是 proxy 伺服器後方，您可能需要設定中的 proxy 資訊*web.config*檔案才能讀取來自網站外部的資訊。 例如，如果您使用`ReCaptcha`helper，協助專家會與 reCAPTCHA 服務通訊，但可能會封鎖您的 proxy 伺服器。 同樣地，用於 ASP.NET Web Pages 中，例如封裝管理員 中，所使用的摘要可能需要 proxy 組態。
> 
> 如果您遇到問題，使用外部服務，或使用封裝摘要中的，將下列項目放入您的應用程式根目錄*web.config*檔案：
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> 如需有關如何設定 proxy 伺服器的詳細資訊，請參閱[ &lt;proxy&gt;項目 （網路設定）](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN 網站上。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>問題： 解除安裝.NET Framework 第 4 版會停用含有 Razor 語法的 ASP.NET Web Pages

> 如果您解除安裝.NET Framework 第 4 版，然後重新安裝時，會停用含有 Razor 語法的 ASP.NET Web Pages。 頁面 *.cshtml*延伸模組無法正確執行。 ASP.NET Web Pages 電腦根目錄中註冊組件*web.config*檔案，並移除.NET Framework 中移除該檔案。 重新安裝.NET Framework 會安裝新版本的組態檔中，但不會新增 ASP.NET Web Pages 組件的參考。
> 
> **因應措施**之後重新安裝.NET Framework，請重新安裝 ASP.NET Web Pages 含有 Razor 語法。 這樣會加入下列項目加入*web.config*電腦根目錄，這通常在下列位置中的檔案：  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>問題： 沒有副檔名的 Url 會找不到在 IIS 7 或 IIS 7.5 上的.cshtml/.vbhtml 檔案

> 在 IIS 7 或 IIS 7.5 上，具有類似下列的 URL 不要求找不到具有頁面 *.cshtml*或 *.vbhtml*延伸模組：  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> 因為 URL 重寫沒有啟用預設的 IIS 7 或 IIS 7.5，就會發生此問題。 Likeliest 案例是看不到問題時測試使用在本機 IIS Express，但它時遇到您將您的網站部署至裝載的網站。
> 
> **因應措施**
> 
> - 如果您有伺服器電腦的控制權，在伺服器電腦上安裝更新中所述[的更新功能可讓某些 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。
> - 如果您沒有在伺服器電腦的控制權 （例如，您要部署至裝載的網站），加上網站的下列*web.config*檔案： 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>問題： 應用程式部署到沒有 SQL Server Compact 安裝的電腦

> 包含 SQL Server Compact 資料庫的應用程式可以執行 SQL Server Compact 不安裝所在的電腦上。 Microsoft WebMatrix 1.0 自動為您複製這些二進位檔，並執行適當*web.config*檔案轉換。
> 
> **因應措施**如果您要複製這些檔案，並進行*web.config*檔案變更，手動執行下列動作：
> 
> 1. 資料庫引擎組件，以複製*Bin*資料夾 （及其子資料夾） 的目標電腦上的應用程式：  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - 複製<em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>至</em></strong>\Bin\x86*
>    - 複製<em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>至</strong><em>\Bin\amd64</em>
> 
> 2. 在網站的根資料夾中，建立或開啟*web.config*檔案。 (在 WebMatrix 1.0，此檔案類型才可用如果您按一下**所有**中**選擇檔案型別** 對話方塊。)
> 3. 將下列項目新增為子系`<configuration>`項目 (不是在內`<system.web>`項目):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>問題: 「 資料庫 」 和 「 WebGrid 「 協助程式無法在中度信任在 Visual Basic 中運作

> 如果您使用 Visual Basic (建立 *.vbhtml*檔案)，則`Database`和`WebGrid`如果應用程式會設定為使用度信任，協助專家將無法運作。
> 
> **因應措施**  
> 如果您使用 Visual Studio 2010，您可以透過安裝 Service Pack 1 版本來解決這個問題。 SP1 版本的最終版本可用之前，您可以下載從 SP1 Beta 版[Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft 下載中心上的頁面。   
>   
> 如果這是不可行，或如果您未使用 Visual Studio 2010，您可以暫時設定應用程式使用完全信任。


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>問題: 「 ApplicationPart 」 資源可供外部存取

> 如果組件包含的物件，衍生自`ApplicationPart`類別、 組件的資源由`ResourceRouteHandler`類別。 例如，請考慮下列 URL：  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> 此要求會下載中的資源字串的所有*System.Web.WebPages.Administration.dll*組件。 下載的所有內嵌資源 （即使不是提供做為靜態內容）。 如果內嵌的資源包含機密資訊，這可代表安全性風險。 
> 
> **因應措施**   
> 如果您建立**ApplicationPart**物件，請確定所內嵌的資源相關聯的**ApplicationPart**物件的組件不包含機密資訊。


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> WebMatrix 安裝問題的相關資訊，請參閱[WebMatrix 安裝問題](#Known_Issues_Installation)稍早在這份文件。


本節的文件描述 for WebMatrix 開發環境的已知的問題。

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>問題： 使用者名稱或密碼的 web.config 檔案中的資料庫連接字串中的變更不會反映在 [資料庫] 工作區

> **因應措施**  
> 
> 1. 在*web.config*檔案中，變更連接字串中的資料庫名稱 （例如，將"1"）。
> 2. 儲存*web.config*檔案。
> 3. 按一下**資料庫**並重新整理。
> 4. 變更中的連接字串中的資料庫名稱*web.config*回到原始的資料庫名稱的檔案。
> 5. 儲存*web.config*檔案。
> 6. 按一下**資料庫**並重新整理。


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>問題： 無法刪除 WebMatrix 所建立的資料夾

> 如果正在執行 WebMatrix，使用提高的權限 (也就是您開始 WebMatrix 使用**系統管理員身分執行**Windows 中的選項)，WebMatrix 所建立的資料夾無法使用 Windows 檔案總管中刪除。
> 
> **因應措施**  
> 執行 Windows 檔案總管 使用提高的權限。 請依照下列步驟：  
> 
> 1. 在 Windows 中，按一下 **啟動**。
> 2. 輸入 「 Windows 檔案總管 」，然後以滑鼠右鍵按一下的項目**Windows 檔案總管**。
> 3. 按一下**系統管理員身分執行**。 然後，您可以刪除資料夾。


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>問題： WebMatrix 1.0 是無法執行需要提高權限的特定工作

> WebMatrix 1.0 是無法執行需要提高權限，例如在下列情況下安裝其他元件的特定工作項目：
> 
> - 在 Windows Vista 或 Windows 7 上，您使用沒有系統管理權限的帳戶登入和使用者帳戶控制 (UAC) 已停用。
> - 您正在使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。
> 
> **因應措施**  
> WebMatrix 1.0 中大部分的工作不需要系統管理權限。 對於執行，您可以執行操作，因為系統管理員，或請遵循下列步驟：
> 
> - 在 Windows Vista 或 Windows 7 上，啟用 UAC。
> - 在 Windows XP 中，將使用者新增至本機 Administrators 安全性群組。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>問題: 「 站台從 Web 組件庫 」 已停用

> **從 Web 組件庫網站**選項已停用，如果未安裝 Web Platform Installer 3.0。
> 
> **因應措施**  
> 安裝[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。


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


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>問題： IntelliSense 不適用於 WebMatrix 的 Razor 語法、 C# 或 Visual Basic

> 在 WebMatrix 支援 IntelliSense 的 HTML 和 CSS。 不過，不供其他語言。 
> 
> **因應措施**   
> 無。


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>問題： 適用於 HTML 和 CSS IntelliSense 建議未當前項目

> 在 WebMatrix 中標記的 IntelliSense 支援使用 HTML [XHTML 1.0 Transitional 結構描述](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional)和 CSS 使用[CSS 2.1 結構描述](http://www.w3.org/TR/CSS2/)。 IntelliSense 會根據這些特定的結構描述，因為某些標記、 屬性可能會建議並非適用於目前頁面或樣式定義。 Html，它也會導致非預期的建議，可能會解譯為格式不正確的 XHTML （例如，當標記未關閉） 的內容中。 此問題可能更值得注意的插入點是否在不完整的標記; 內在此情況下，IntelliSense 可能會建議新開啟的索引標籤，或提供其他錯誤的建議。 
> 
> **因應措施**   
> HTML 中，請確定您使用語式正確、 完整的 XHTML 頁面內。 Css，沒有任何因應措施。


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>問題： IntelliSense 不會叫用輸入時

> 有時候，IntelliSense 可能不會叫用的 HTML 或 CSS 編輯器中輸入。 特別是，這可能會發生插入點位於另一個項目旁邊的直接或檔案結尾處。 
> 
> **因應措施**   
> 請確定是插入點周圍的空白字元，且插入點不在檔案結尾。 您也可以藉由按下 Ctrl + 空格鍵以手動方式啟動 IntelliSense。


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>問題： 沒有使用者介面是適用於停用 IntelliSense

> WebMatrix 1.0 會提供用來停用 IntelliSense 沒有 UI 或軌跡。 
> 
> **因應措施**   
> 啟動 WebMatrix 使用下列命令，其中包括停用 IntelliSense 的交換器：  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express 都有它自己讀我檔案，將會位於下列 URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact 都有它自己讀我檔案，將會位於下列 URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

WebMatrix 的一部分安裝 SQL Server Compact 牽涉到的問題的相關資訊，請參閱[WebMatrix 安裝問題](#Known_Issues_Installation)稍早在這份文件。

### <a id="Known_Issues_Installing_Applications"></a>  安裝應用程式

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>問題： 安裝應用程式可能需要很長的時間。 如果使用者的 [我的文件] 資料夾重新導向到網路共用

> **因應措施**  
> 無。 應用程式可能需要一段，若要安裝，但會正確安裝。


### <a id="Known_Issues_Publishing_Applications"></a>  發行應用程式

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>問題: 「 要求無法取得的權限 」 的錯誤時發佈 SQL Compact 資料庫

> WebMatrix 不完全支援的 SQL Server Compact 的支援的二進位檔部署至伺服器執行.NET Framework 3.5 版與中度信任設定。
> 
> **因應措施**  
> 慣用的因應措施是在伺服器上安裝.NET Framework 4。 或者，執行下列作業：
> 
> 1. 加入下列項目來`SecurityClasses`一節中*Web\_MediumTrust.config*檔案：
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. 建立新的使用權限集合*Web\_MediumTrust.config*下列必要的權限的檔案：
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. 套用權限設定為 SQL Server Compact，將下列項目放入*Web\_MediumTrust.config*檔案：
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>問題： 組件庫和 PhpBB web 應用程式顯示 「 服務無法使用 」 錯誤發行之後

> 在某些情況下，應用程式的發行會導致 「 服務無法使用 」 錯誤。
> 
> **因應措施**  
> 在 WebMatrix 中加入反斜線 (\)中的伺服器名稱的結尾**發行設定**視窗，然後將發行的應用程式。


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>問題： Moodle 網站版面配置和連結已中斷在發行後

> Moodle 應用程式發行之後，應用程式運作不正常運作。
> 
> **因應措施**  
> 在 WebMatrix 中，加入結尾的斜線 （/）**站台名稱**欄位**發行設定**視窗，然後將發行的應用程式。


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>問題： 發行 nopCommerce 資料庫錯誤而失敗

> 發行 nopCommerce 失敗並報告資料庫錯誤，例如 「 插入 nop\_記錄資料表失敗。 」
> 
> **因應措施**  
> 
> 1. 在 WebMatrix 中，按一下 **執行**啟動 nopCommerce 在本機。
> 2. 登入的管理頁面。
> 3. 按一下**系統**功能表。
> 4. 按一下**記錄**選項。
> 5. 按一下**清除記錄檔** 按鈕。
> 6. 重新發行 nopCommerce。


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>問題： Silverstripe CMS 會顯示 「 HTTP 500 PHP FCGI 錯誤 」 下載已發行的站台時

> **因應措施**  
> 按一下 之後**下載發佈站台**，略過`silverstripe-cache/manifest_main`中**發行預覽**。 這個檔案用於快取，且特定的每一部電腦。


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>問題： Subtext 會顯示 「 伺服器錯誤 '/' 應用程式中 」 的已發行的網站的下載時

> **因應措施**  
> 開啟網站的*web.config*檔案，並將使用者識別碼和密碼的資料庫連接字串中使用 SQL Server 系統管理員認證 （"sa"認證）。
> 
> 或者，請遵循下列步驟，才能將提供您用來登入的使用者帳戶`db_owner`權限：
> 
> 1. 安裝 SQL Server Management Studio 中使用 Web Platform Installer。
> 2. 連接到本機的 SQL Server Express 執行個體 (根據預設， `.\SQLEXPRESS`)。
> 3. 按一下**資料庫** &gt; *[localSubtextDatabase]* &gt; **安全性** &gt; **使用者**&gt; *[localSubtextUser*] (預設值是`subtextuser`]，按一下滑鼠右鍵，然後按一下**屬性**。
> 4. 選取**db\_擁有者**中角色成員資格 > 一節。


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


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>問題： 某些連結不會顯示在 DotNetNuke 之後發行或下載網站

> 如果您發行或下載 DotNetNuke 網站，您可能需要清除快取，以取得新的連結會出現在網站上。
> 
> **因應措施**
> 
> 1. 「 主機 」 身分登入。
> 2. 請移至主功能表並選取**主機設定**。
> 3. 捲軸，並在**進階設定**，依序展開**效能設定**。
> 4. 按一下**清除快取**網頁的連結。
> 5. 請移至頁面底部，然後重新啟動應用程式。


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>問題： 某些 AtomSite 中的連結已中斷後下載已發佈的網站

> **因應措施**  
> 在*service.config*檔案， *users.config*檔案，以及所有 *.xml*檔案，取代 URL 字串 (例如， `http://myhost.com/atomsite`) 以本機 (比方說， `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>問題： 如 WordPress MySQL 為基礎的應用程式無法發行，並報告資料庫錯誤

> 根據預設，WebMatrix 會使用 utf-8 字元集安裝 MySQL。 如果您將 MySQL 安裝在您自己的而且字元集不是 utf-8 （例如，它是 Latin1），發行程序的資料庫可能會失敗。
> 
> **因應措施**
> 
> 1. 變更 MySQL 為 utf-8 的字元集。 (如需詳細資訊，請參閱[伺服器字元集和定序](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html)MySQL 網站上。)
> 2. 重新安裝應用程式。
> 3. 重新發行應用程式。


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>問題: 「 下載發佈站台 」 應用程式的瀏覽器為基礎的安裝失敗

> 某些應用程式 (例如，Kentico CMS) 會要求您在瀏覽器中啟動以執行後續安裝設定，例如建立資料庫。 如果您發行像這樣的應用程式，而不會完成瀏覽器為基礎的安裝程式，嘗試從遠端伺服器下載相同的站台將會失敗。
> 
> **因應措施**  
> 發佈站台之前先完成瀏覽器為基礎的安裝。


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>問題: 「 下載發佈站台 」 失敗，發生資料庫錯誤 DotNetNuke 和 Kooboo CMS

> 如果您嘗試從伺服器下載應用程式，而您必須在資料庫連接字串中的系統管理員認證**發行設定** 對話方塊中，您可能會看到發行記錄中的下列錯誤：
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **因應措施**  
> 如果可行的話，重新發行網站 （或將之發行） 資料庫使用非系統管理員認證。


<a id="More_Info"></a>

## <a name="for-more-information"></a>詳細資訊

如需 WebMatrix 1.0 的詳細資訊，請參閱下列網站：

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. All Rights Reserved. [使用規定](https://msdn.microsoft.cos/cc300389.aspx)。
