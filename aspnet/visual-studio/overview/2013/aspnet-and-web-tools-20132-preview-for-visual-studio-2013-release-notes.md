---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET 和 Web 工具 2013.2 for Visual Studio 2013 版本資訊 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 59e33d9bb1cef79e6d0e83fb3560e964723ad5e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391440"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET 和 Web 工具 2013.2 for Visual Studio 2013 版本資訊
====================
by [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>安裝注意事項

ASP.NET 及 Web Tools for Visual Studio 2013.2 會配套在主要安裝程式，並可以下載的一部分[Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)。

## <a name="documentation"></a>文件

教學課程和其他資訊關於 ASP.NET 和 Web Tools for Visual Studio 2013.2 都可從[ASP.NET 網站](https://www.asp.net/)。

## <a name="software-requirements"></a>軟體需求

ASP.NET 及 Web Tools for Visual Studio 2013.2 需要 Visual Studio 2013。

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>在 ASP.NET 和 Web Tools for Visual Studio 2013.2 中的新功能

下列各節會說明已在版本中引進的功能。

- [ASP.NET 專案範本](#oneaspnet)
- [當您啟動在 IIS Express 上的 Web 應用程式時，支援 SSL](#ssl)
- [Visual Studio Web 編輯器增強功能](#vswebeditor)
- [瀏覽器連結](#browserlink)
- [在 Visual Studio 中的 Azure App Service Web Apps 支援](#waws)
- [建立新的 Web 專案時，請建立遠端的 Azure 資源](#AzureResources)
- [Web 發行增強功能](#webpublish)
- [ASP.NET Scaffolding](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web Form](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Microsoft OWIN 元件](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>ASP.NET 專案範本

- 若要支援帳戶確認和密碼重設的 ASP.NET 專案範本的更新。
- 更新 ASP.NET Web API 範本，以支援使用在內部部署組織帳戶進行驗證。
- ASP.NET SPA 範本現在包含 MVC 和伺服器端檢視為基礎的驗證。 該範本含有 WebAPI 控制器並僅可透過已驗證的使用者存取。

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>當您啟動在 IIS Express 上的 Web 應用程式時，支援 SSL

若要消除安全性警告時瀏覽和偵錯在 localhost 上的 HTTPS，我們新增了一個對話方塊，以允許 Internet Explorer 和 Chrome，信任的自我簽署的 IIS express SSL 憑證。

例如，web 專案屬性可以設定為使用 SSL。 按 F4 顯示 [屬性] 對話方塊。 變更**啟用 SSL**設為 true。 複製 SSL URL。

![SSL 已啟用屬性](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

集合 [web 專案屬性頁 web] 索引標籤為使用 HTTPS 基礎 URL (的 SSL URL 將是`https://localhost:44300/`除非您先前已建立 SSL Web Sites。)

![設定專案 URL (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

按 CTRL+F5 執行應用程式。 遵循指示來信任 IIS Express 產生的自我簽署的憑證。

![SSL 警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

讀取**安全性警告**對話方塊，然後按一下**是**如果您想要安裝憑證，代表 localhost。

![安全性警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

站台將會顯示在 IE 或 Chrome 中沒有瀏覽器中的憑證警告。

![HTTPS 頁面上，而不發出警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox 會使用自己的憑證存放區，因此它會顯示警告。

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web 編輯器增強功能

- **新的 JSON 專案項目和編輯器**： 我們已新增 JSON 專案項目和編輯器，Visual studio。 目前的 JSON 編輯器功能包括顏色標示、 語法驗證、 大括號完成、 大綱、 工具選項的設定和更多功能。

    ![JSON 編輯器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense 現在支援[JSON 結構描述](http://json-schema.org/)v3 和 v4。 結構描述下拉式方塊來選擇現有的結構描述，請編輯本機結構描述路徑，或只是將卸除專案的 JSON 檔案，以取得相對路徑。

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON 結構描述編輯器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Sass (SCSS) 的新編輯器**： 我們已新增小於 VS2013 rtm，而且我們現在有 Sass 專案項目和編輯器。 Sass 編輯器功能相當於 LESS 編輯器中，並包含顏色標示、 變數和 Mixin IntelliSense、 註解/取消註解、 快速諮詢、 格式化、 語法驗證、 大綱、 移至定義、 色彩選擇器中，工具選項等設定。

    ![加入新項目： SCSS 樣式表](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![樣式表編輯器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **在 HTML、 Razor、 CSS、 較少的新 URL 選擇器和 Sass 文件：** VS 2013 隨附於 Web Form 頁面之外的任何 URL 選擇器。 Razor 的 HTML，css，較少的新 URL 選擇器和 Sass 編輯器是對話方塊免費、 fluent 類型選擇器，了解 '..' 並篩選檔列出適當的 img 標記和連結。

    ![影像標記的 URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![URL 選擇器檢視](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Css URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **藉由新增更多功能的 LESS 編輯器的更新**
- **Knockout Intellisense 升級**： 我們已新增 VS intelliSense 的非標準 KnockOut 語法"ko vs 編輯器 viewModel:"語法。 它可以用來繫結至在表單中使用註解 頁面上的多個檢視模型：

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    我們也加入巢狀的 ViewModel IntelliSense 支援，因此您可能會向下切入在 ViewModel 上深的巢狀物件。

    `<div data-bind="text: foo.bar.baz.etc" />`

    顯示 IntelilSense 是 JavaScript 物件的完整的 IntelliSense。

    ![Intellisense 顯示完整的 JavaScript 物件](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **在 HTML、 Razor、 CSS、 較少的新 URL 選擇器和 Sass 文件**: VS 2013 隨附於 Web Form 頁面之外的任何 URL 選擇器。 Razor 的 HTML，css，較少的新 URL 選擇器和 Sass 編輯器是對話方塊免費、 fluent 類型選擇器，了解 '..' 並篩選檔列出適當的 img 標記和連結。

    ![影像標記的 URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![URL 選擇器檢視](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Css URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>瀏覽器連結

- 瀏覽器連結現在支援 HTTPS 連線，並會列出，在儀表板與其他連線，只要瀏覽器信任的憑證。
- 靜態的 HTML 原始檔對應
- SPA 支援的對應資料
- 自動更新對應資料

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>在 Visual Studio 中的 Azure App Service Web Apps 支援

- **Azure 支援登入。**
- **遠端偵錯] 和 [web 應用程式的遠端檢視**： 我們現在支援[在 Azure App Service 中的 web 應用程式的遠端偵錯](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)和遠端的 web 應用程式內容檔案在伺服器總管 中的檢視。

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>建立新的 Web 專案時，請建立遠端的 Azure 資源

我們已新增 Azure [[建立遠端資源]](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)在新的 web 應用程式 對話方塊上的核取方塊。 藉由選擇它，您將能夠整合建立新的 web 應用程式，測試，以及一些簡單的步驟中建立發行設定檔的 Azure 發行網站所設定的體驗。

![使用 Azure 資源的新專案](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![發行至 Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web 發行增強功能

- 改善使用者體驗進行發佈。

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

- **列舉支援：** 如果您的模型使用列舉，則 MVC Scaffolder 會產生下拉式清單中列舉。 這會在 MVC 中使用的列舉 helper。
- **啟動程序支援**： 更新 MVC Scaffolding EditorFor 範本，讓它們使用的啟動程序的類別。
- **封裝支援**: MVC 和 Web API Scaffolder 會在 MVC 和 Web API 新增 5.1 套件

下列螢幕擷取畫面示範 scaffolding 模型。

- 模型的程式碼：

     ![模型的程式碼](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- 編譯模型程式碼、 按一下滑鼠右鍵，然後選取**新增**，**新增 Scaffold 項目**。

     ![加入新的 Scaffold 項目](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- 選擇**MVC5 控制器與檢視，使用 Entity Framework**:

     ![新增 MVC5 控制器與檢視](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **新增控制器**使用模型：

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- 檢查產生的程式碼，例如包含 Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`:![檢視包含 EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- 執行若要查看產生的列舉下拉式方塊，請注意，是否值可以是 null，空字串可以選擇下拉式方塊的頁面。 例如，**建立**頁面會顯示下列：

    ![允許空字串的下拉式方塊](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM 將會發行在 2014 年 4 月。 以下是主要重點，從版本資訊，但請檢查[完整的版本資訊](http://docs.nuget.org/docs/release-notes/nuget-2.8)如需有關這些變更。

- **Windows Phone 8.1 應用程式為目標**: NuGet 2.8.1 現在支援以 Windows Phone 8.1 應用程式使用目標 framework moniker 'WindowsPhoneApp'、 'WPA'、 'WindowsPhoneApp81，' 和 'WPA81' 為目標。
- **相依性的修補程式解決**： 時解析套件相依性，NuGet 在過去已實作的選取最低的主要和次要封裝版本符合封裝上的相依性的策略。 不同於主要和次要版本，不過，修補程式版本已一律解決的最高版本。 行為是用意良好，但它會建立決定性具有相依性安裝套件的缺乏。
- **DependencyVersion 參數**： 雖然 NuGet 2.8 變更*預設*解析相依性的行為，同時也增加了更精確地控制透過-DependencyVersion 參數中的相依性解析程序套件管理員主控台。 參數可讓您解析的相依性，以最低的可能版本 （預設行為）、 最高的可能版本，或最高次要或修補程式版本。 此參數僅適用於安裝套件中的 powershell 命令。
- **DependencyVersion 屬性**： 除了-DependencyVersion 參數以上詳述的情況下，NuGet 也可讓能夠設定新的屬性中定義的預設值為何，如果-DependencyVersion 切換 nuget.config 檔案未安裝套件的引動過程中指定。 此值也會遵守任何安裝封裝作業的 [NuGet 套件管理員] 對話方塊。 若要設定此值，請先 nuget.config 檔案中加入以下屬性：

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **預覽 NuGet 作業與-whatif**： 某些 NuGet 套件可以有深入的相依性圖形，而此情況下，它可以在安裝期間會有幫助、 解除安裝或更新作業，先查看 會發生什麼事。 NuGet 2.8 新增標準的 PowerShell-如果切換到安裝套件、 解除安裝套件和更新套件的命令，以啟用視覺化的命令將會套用的封裝整個關閉。
- **降級封裝**： 是不常見的安裝套件的發行前版本，以便調查的新功能，但後來回復到最新穩定版本。 NuGet 2.8 以前這會是多重步驟的程序解除安裝發行前版本套件和其相依性，然後安裝 先前的版本。 使用 NuGet 2.8，不過，更新套件現在會回復整個封裝結束 （例如套件的相依性樹狀結構） 到之前的版本。
- **開發相依性**： 許多不同類型的功能可以傳遞為 NuGet 套件-包括用來最佳化開發過程的工具。 這些元件，雖然也很有幫助開發新封裝時，不應視為已發佈的更新版本時，新的封裝相依性。 NuGet 2.8 可讓封裝本身識別為 developmentDependency.nuspec 檔案中。 安裝時，此中繼資料也會加入至專案的套件安裝所在的 packages.config 檔案。 Packages.config 檔案稍後分析的 NuGet 相依性期間 nuget.exe 組件，它會排除這些標示為開發相依性的相依性。
- **適用於不同平台的個別 packages.config 檔案**： 開發適用於多個目標平台的應用程式，通常會每個個別的建置環境有不同的專案檔。 它也很常見使用不同的 NuGet 封裝，在不同的專案檔中，因為套件會有各種不同平台的支援層級。 NuGet 2.8 會提供改進的支援此案例中藉由建立不同的平台專屬專案檔的不同的 packages.config 檔案。
- **切換回本機快取**： 雖然 NuGet 套件通常都是從遠端資源庫這類[NuGet 資源庫](http://www.nuget.org)使用網路連線，有許多用戶端未連接的案例。 沒有網路連線，NuGet 用戶端無法成功安裝封裝-，即使這些套件已在本機 NuGet 快取的用戶端的電腦上。 NuGet 2.8 會將套件管理員主控台中的自動後援的快取。

    快取後援功能不需要任何特定的命令列引數。 此外，快取後援目前只適用於套件管理員主控台-行為目前不適用於 [套件管理員] 對話方塊。
- **Bug 修正**： 其中一個所做的重大錯誤修正已更新套件中的效能改進-重新安裝命令。

    除了這些功能和修正上述的效能，這一版的 NuGet 也會包含其他許多 bug 修正。 發生在版本中解決的 181 總問題。 如需完整的工作清單項目中已修正 NuGet 2.8，請檢視[NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all)此版本。

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web Form

- Web Form 範本現在會顯示如何執行帳戶確認和密碼重設作業的 ASP.NET 身分識別。
- 實體資料的原始檔控制和動態的資料提供者，針對 Entity Framework 6。 如需詳細資訊，請參閱下列 MSDN 部落格：[動態資料提供者和 Entity Framework 6 的 EntityDataSource 控制項](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)。

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [屬性路由的增強功能](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [編輯器範本的啟動程序支援](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [在檢視中的列舉支援](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Unobstrusive 支援 MinLength / MaxLength 屬性](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [支援不顯眼的 Ajax 中的 'this' 內容](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- 各種[bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [全域錯誤處理](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [屬性路由的增強功能](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [說明頁面改善](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute 支援](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON 的媒體類型格式器](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [非同步篩選條件有更好的支援](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [查詢剖析為格式化的媒體櫃用戶端](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- 各種[bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- 各種[bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework 已更新為版本 6.1 執行階段和工具。 Entity Framework (EF) 6.1 是 Entity Framework 6 次要更新，而包含數個 bug 修正和新功能。 如需 EF6.1，包括文件的新功能，連結的詳細資訊，請參閱[Entity Framework 版本歷程記錄](https://msdn.microsoft.com/data/jj574253)。 在此版本中的新功能包括：

- **工具 彙總**提供一致的方式，來建立新的 EF 模型。 這項功能會擴充 ADO.NET 實體資料模型精靈，可讓您建立的 Code First 模型，包括反向工程，從現有的資料庫。 這些功能是先前在 EF Power Tools 中的 Beta 品質中提供。
- **處理交易認可失敗**提供的新[System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx)哪些會利用新引進的功能來攔截交易作業。 **CommitFailureHandler**認可交易的同時，允許從連接失敗的自動復原。
- **IndexAttribute**允許藉由將屬性 （或屬性） 上的屬性放在 Code First 模型中指定的索引。 程式碼第一次將接著建立對應的索引在資料庫中。
- **公開的對應 API**提供 EF 對如何對應到資料行和資料表在資料庫中的屬性及型別資訊的存取權。 在過去版本此 API 的內部。
- **若要設定攔截器，透過 App/Web.config 檔案的能力**（允許新增，而不需要重新編譯應用程式的攔截器）。
- **DatabaseLogger**是新的攔截器，可讓您輕鬆地記錄到檔案中的所有資料庫作業。 舊版的功能結合，這可讓您輕鬆地切換為已部署的應用程式，而不需要重新編譯的資料庫作業的記錄。
- **移轉模型變更偵測**已經過改良，以便包含 scaffold 的移轉會變得更精確; 變更偵測程序的效能也已大幅增強。
- **效能改進**在初始化期間降低的資料庫作業，包括 LINQ 查詢中的 null 的相等比較的最佳化更快速檢視層代 （模型建立） 的更多案例，且更有效率具有多個關聯的追蹤實體的具體化。

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **雙因素驗證**: ASP.NET 身分識別現在支援雙因素驗證。 雙因素驗證提供一層額外的安全性，以在其中您的密碼遭到入侵的情況下您的使用者帳戶。 另外還有兩個因素碼的暴力密碼破解攻擊的保護。
- **帳戶鎖定：** 可用來鎖定使用者，如果使用者可輸入其密碼或兩個因素碼不正確。 無效的嘗試次數和時間範圍的數目如使用者遭到鎖定可以設定。 開發人員可以選擇性地關閉帳戶鎖定特定使用者帳戶應該要。
- **帳戶確認：** ASP.NET 身分識別系統現在支援帳戶確認。 這是很常見的案例在今日大部分網站中，當您註冊網站上新的帳戶，您需要確認您的電子郵件，然後才可以執行之網站中的任何項目。 電子郵件確認是很有用，因為它可防止假帳戶建立。 這是網站的非常有用，如果您使用電子郵件做為與您，例如論壇網站、 銀行業務、 電子商務或社交網站的使用者進行通訊的方法。
- **密碼重設：** 密碼重設為其中使用者可以重設其密碼如果忘記其密碼的功能。
- **安全性戳記 （登出所有位置）：** 支援的方式重新產生安全性權杖案例中的使用者，當使用者變更其密碼或任何其他安全性相關資訊，例如移除相關聯的登入 （例如 Facebook、 Google、Microsoft 帳戶等等）。 這被需要以確保任何使用舊密碼所產生的權杖都會失效。 在範例專案中，如果您變更使用者的密碼為使用者產生新的權杖，然後任何先前的權杖都會失效。 這項功能提供一層額外的安全性，以您的應用程式，因為當您變更您的密碼時，會將您登出從任何位置 （所有其他瀏覽器） 已在此登入此應用程式。
- **進行可延伸的使用者和角色的主索引鍵類型**： 在 ASP.NET 身分識別 1.0 中，資料表的使用者和角色的主索引鍵的型別為字串。 這表示當 ASP.NET 身分識別系統時，已保存在 SQL Server 上，使用 Entity Framework，我們使用 nvarchar。 發生在 Stack Overflow 上的這個預設實作的許多討論並根據內送的意見反應。 我們提供的擴充攔截，其中您可以在其中指定應該為何您的使用者和角色的資料表的主索引鍵。 此擴充性攔截程序會特別有用的是如果您要移轉您的應用程式，而且應用程式已儲存的使用者 Id 是 Guid 或將 int。
- **在 使用者和角色支援 IQueryable**： 已新增的對 UsersStore 和 RolesStore IQueryable 的詳細資訊，您可以輕鬆地取得使用者和角色的清單。
- **支援透過 UserManager 的刪除作業**
- **編製索引的使用者名稱**： 在 ASP.NET 身分識別 Entity Framework 實作中，我們新增了唯一索引的使用者名稱使用新的 IndexAttribute EF 6.1.0 中。 這可確保永遠是唯一的使用者名稱，並沒有任何競爭條件，在其中您最後可能會含有重複的使用者名稱。
- **增強的密碼驗證程式：** 隨附於 ASP.NET 身分識別 1.0 密碼驗證程式是一個相當基本密碼驗證程式，只有已驗證的最小長度。 沒有新的密碼驗證程式，可讓您更充分掌控密碼的複雜性。 請注意，即使您開啟此密碼中的所有設定，執行建議您啟用雙因素驗證的使用者帳戶。
- **IdentityFactory 中介軟體 / CreatePerOwinContext**:

    - **使用者管理員**： 您可以使用處理站實作的 OWIN 內容從取得 UserManager 的執行個體。 此模式相當類似於我們用於 AuthenticationManager 獲得登入和登出的 OWIN 內容。 這是建議的方法來取得 UserManager 的執行個體，每個應用程式的要求。
    - **DbContextFactory**: ASP.NET 身分識別會保存在 SQL Server 中的身分識別系統中使用 Entity Framework。 若要這樣做的身份識別系統有 ApplicationDbContext 的參考。 DbContextFactory 中介軟體會傳回每個您可以使用您的應用程式中的要求 ApplicationDbContext 的執行個體。
- **ASP.NET 身分識別範例 NuGet 套件**： 範例 NuGet 套件可以輕鬆地安裝和執行範例 ASP.NET 身分識別和遵循的最佳作法。 這是範例 ASP.NET MVC 應用程式。 請修改程式碼以符合您的應用程式，這在生產環境中部署之前。 此範例應該安裝在空的 ASP.NET 應用程式。 如需有關封裝的詳細資訊，請移至下列部落格文章：[宣布 RTM 的 ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN 元件

發生大量已在此版本中修正的 bug。 請參閱[版本資訊 2.1.0 版本](https://katanaproject.codeplex.com/releases/view/113281)如需詳細資訊。

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

發生大量已在此版本中修正的 bug。 請參閱[2.0.2 的版本資訊版本](https://github.com/SignalR/SignalR/releases/tag/2.0.2)如需詳細資訊。
