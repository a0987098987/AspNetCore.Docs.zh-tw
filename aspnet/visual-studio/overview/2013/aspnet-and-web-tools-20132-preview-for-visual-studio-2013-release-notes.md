---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊 2013.2 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036020"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET 及 Web Tools 2013.2 for Visual Studio 2013 版本資訊
====================
by [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>安裝注意事項

ASP.NET 及 Web Tools for Visual Studio 2013.2 會集結在主要安裝程式，而且可以下載的一部分[Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)。

## <a name="documentation"></a>文件

教學課程和其他資訊 ASP.NET 及 Web Tools for Visual Studio 2013.2 都是從[ASP.NET 網站](https://www.asp.net/)。

## <a name="software-requirements"></a>軟體需求

ASP.NET 及 Web Tools for Visual Studio 2013.2 需要 Visual Studio 2013。

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>在 ASP.NET 和 Web Tools for Visual Studio 2013.2 中的新功能

下列章節說明在版本中已引進的功能。

- [一個 ASP.NET 專案範本](#oneaspnet)
- [啟動 IIS express Web 應用程式時，支援 SSL](#ssl)
- [Visual Studio Web 編輯器增強功能](#vswebeditor)
- [瀏覽器連結](#browserlink)
- [Visual Studio 中的 Azure App Service Web 應用程式的支援](#waws)
- [建立新的 Web 專案時建立遠端的 Azure 資源](#AzureResources)
- [Web 發行的增強功能](#webpublish)
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
### <a name="one-aspnet-project-templates"></a>一個 ASP.NET 專案範本

- 若要支援帳戶確認和密碼重設的 ASP.NET 專案範本的更新。
- 更新 ASP.NET Web API 範本，以支援使用在內部部署組織帳戶進行驗證。
- ASP.NET SPA</範本現在包含 MVC 和伺服器端檢視為基礎的驗證。 該範本含有 WebAPI 控制器只有經過驗證的使用者可存取。

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>啟動 IIS express Web 應用程式時，支援 SSL

若要消除安全性警告，瀏覽及在 localhost 上偵錯 HTTPS 時，我們加入對話方塊，讓 Internet Explorer 和 Chrome 信任自我簽署的 IIS express SSL 憑證。

例如，web 專案屬性可以設定為使用 SSL。 按 F4 顯示 [屬性] 對話方塊。 變更**啟用 SSL**為 true。 複製 SSL URL。

![SSL 已啟用屬性](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

設定 web 專案屬性頁面 web 索引標籤為使用 HTTPS 基礎 URL (將 SSL URL`https://localhost:44300/`除非您先前已建立 SSL 的網站。)

![設定專案 URL (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

按 CTRL+F5 執行應用程式。 依照要信任 IIS Express 產生的自我簽署的憑證的指示。

![SSL 警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

讀取**安全性警告**對話方塊，然後按一下**是**如果您想要安裝憑證，表示 localhost。

![安全性警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

站台將會顯示在 IE 或 Chrome 不在瀏覽器中憑證警告。

![沒有警告的 HTTPS 頁面](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox 會使用自己的憑證存放區，因此它會顯示警告。

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web 編輯器增強功能

- **新的 JSON 專案項目和編輯器**： 我們新增了 JSON 專案項目和編輯器以 Visual Studio。 目前的 JSON 編輯器功能包括顏色標示、 語法驗證、 大括號完成、 大綱、 工具選項設定等等。

    ![JSON 編輯器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense 現在支援[JSON 結構描述](http://json-schema.org/)v3 和 v4。 結構描述下拉式方塊來選擇現有的結構描述，請編輯本機結構描述路徑，或只是拖曳卸除專案的 JSON 檔案，以便取得相對路徑。

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON 結構描述編輯器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **新的 Sass (SCSS) 編輯器**： 我們加入小於 VS2013 rtm 中，我們現在有 Sass 專案項目和編輯器。 Sass 編輯器功能相當於較少的編輯器中，而且包含顏色標示、 變數和 Mixins IntelliSense、 註解/取消註解、 快速諮詢、 格式化、 語法驗證、 大綱、 移至定義、 色彩選擇器，工具選項等設定。

    ![加入新項目： SCSS 樣式表](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![樣式表編輯器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **HTML、 Razor，在 CSS 中較少的新 URL 選擇器和 Sass 文件：** VS 2013 隨附沒有外部 Web Form 網頁的 URL 選擇器。 HTML、 Razor、 css、 較少的新 URL 選擇器和 Sass 編輯器是對話方塊釋放、 fluent 應用程式開發類型選擇器，了解 '..' 並篩選檔列出適當的 img 標記和連結。

    ![影像標記的 URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![檢視 URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![URL 選擇器 css](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **藉由新增更多功能的 LESS 編輯器的更新**
- **Knockout Intellisense 升級**： 我們加入了非標準的 KnockOut 語法 VS intellisense"ko vs 編輯器 viewModel:"語法。 它可以用來繫結至表單中使用註解在頁面上的多個檢視模型：

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    我們也加入巢狀的 ViewModel IntelliSense 支援，因此您可以切入 ViewModel 深的巢狀物件。

    `<div data-bind="text: foo.bar.baz.etc" />`

    顯示 IntelilSense 是 JavaScript 物件在完整的 IntelliSense。

    ![Intellisense 顯示完整的 JavaScript 物件](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **HTML、 Razor，在 CSS 中較少的新 URL 選擇器和 Sass 文件**: VS 2013 隨附沒有外部 Web Form 網頁的 URL 選擇器。 HTML、 Razor、 css、 較少的新 URL 選擇器和 Sass 編輯器是對話方塊釋放、 fluent 應用程式開發類型選擇器，了解 '..' 並篩選檔列出適當的 img 標記和連結。

    ![影像標記的 URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![檢視 URL 選擇器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![URL 選擇器 css](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>瀏覽器連結

- 瀏覽器連結現在支援 HTTPS 連線，並會列出，儀表板中的其他連接，只要瀏覽器所信任的憑證。
- 靜態的 HTML 原始檔對應
- SPA 支援的對應資料
- 自動更新對應資料

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Visual Studio 中的 Azure App Service Web 應用程式的支援

- **支援 Azure 登入。**
- **遠端偵錯和遠端 web 應用程式檢視**： 我們現在支援[Azure App Service 中的 web 應用程式的遠端偵錯](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)和遠端 web 應用程式內容檔案在伺服器總管中的檢視。

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>建立新的 Web 專案時建立遠端的 Azure 資源

我們加入了 Azure ["建立遠端資源 」](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)在新的 web 應用程式 對話方塊上的核取方塊。 藉由選擇它，您可以建立新的 web 應用程式，設定測試和幾個簡單步驟中建立發行設定檔的 Azure 發佈站台的體驗整合。

![Azure 資源與新的專案](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![發行至 Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web 發行的增強功能

- 改善使用者體驗進行發佈。

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

- **列舉支援：** 如果您的模型使用列舉，則 MVC Scaffolder 會產生下拉式清單中的列舉。 這會在 MVC 中使用的列舉 helper。
- **啟動支援**： 更新在 MVC Scaffolding EditorFor 範本，讓它們使用的啟動程序的類別。
- **封裝支援**: MVC Web 應用程式開發介面 Scaffolder 會將 5.1 封裝之 MVC 和 Web API

下列螢幕擷取畫面示範 scaffolding 模型。

- 模型的程式碼：

     ![模型程式碼](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- 編譯模型程式碼中，按一下滑鼠右鍵，然後選取**新增**，**新的 Scaffold 項目**。

     ![加入新的 Scaffold 項目](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- 選擇**MVC5 控制器與檢視，使用 Entity Framework**:

     ![加入新的 MVC5 控制器與檢視](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **加入控制器**使用模型：

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- 檢查產生的程式碼，例如 Views/WeekdayModels/Edit.cshtml 包含`@Html.EnumDropDownListFor`:![檢視包含 EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- 執行頁面，請參閱產生列舉下拉式方塊中，請注意，是否值可以是 null，空的字串可以選擇下拉式方塊。 例如，**建立**頁面會顯示下列：

    ![下拉式方塊中允許空字串](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM 將於 2014 年 4 月發行。 以下是重點從版本資訊，但請檢查[完整版本資訊](http://docs.nuget.org/docs/release-notes/nuget-2.8)如需有關這些變更。

- **Windows Phone 8.1 應用程式為目標**: NuGet 2.8.1 現在可支援針對 Windows Phone 8.1 應用程式使用的目標 framework moniker 'WindowsPhoneApp'、 'WPA'、 'WindowsPhoneApp81，' 和 'WPA81'。
- **相依性的修補程式解析**： 解析封裝相依性，NuGet 在過去已實作的選取最低的主要和次要的封裝版本能符合封裝上的相依性的策略。 不同於主要和次要版本中，不過，修補程式的版本是一定要解析為最高的版本。 雖然行為是本意良好，但，它會建立缺乏決定性具有相依性安裝封裝。
- **DependencyVersion 交換器**： 雖然 NuGet 2.8 變更*預設*解析相依項的行為，它也會加入相依性解析程序中的-DependencyVersion 交換器透過更精確地控制封裝管理員主控台。 參數可讓解析的相依性，以最低的可能版本 （預設行為），可能最高的版本，或最高次要或修補程式的版本。 這個參數僅適用於安裝套件中的 powershell 命令。
- **DependencyVersion 屬性**:-DependencyVersion 交換器以上詳述的情況下，除了 NuGet 也有提供設定新的屬性中定義的預設值為何，如果-DependencyVersion 切換 nuget.config 檔案的能力未安裝套件的引動過程中指定。 這個值也會遵守任何安裝封裝作業的 [NuGet 套件管理員] 對話方塊。 若要設定此值，請先 nuget.config 檔案中加入以下屬性：

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **預覽 NuGet 作業與-whatif**： 某些 NuGet 封裝可以有深入的相依性圖形，並在這種情況，它可以在安裝期間會有幫助、 解除安裝或更新作業來第一次看到 會發生什麼事。 NuGet 2.8 加入標準 PowerShell-如果切換到啟用視覺化整個封閉區段的命令將會套用的封裝的安裝套件、 解除安裝套件和更新套件的命令。
- **降級封裝**： 是不常見的安裝套件的發行前版本，以便調查新的功能，然後決定要回復到最後一個穩定版本。 NuGet 2.8 之前，這是多重步驟的程序解除安裝發行前版本的封裝和其相依性，然後安裝較舊版本。 使用 NuGet 2.8，不過，更新套件現在會回復整個封裝封閉區段 （例如封裝的相依性樹狀目錄） 到之前的版本。
- **開發相依性**： 許多不同類型的功能可以傳遞做為 NuGet 套件-包括用來最佳化開發程序的工具。 雖然可以很有幫助開發新的封裝，而這些元件不應視為發行新的封裝更新時的相依性。 NuGet 2.8 可讓封裝在 developmentDependency.nuspec 檔案中識別本身。 安裝時，此中繼資料也會加入 packages.config 檔案到其中安裝封裝的專案。 當稍後 nuget.exe 套件期間 NuGet 相依性分析該 packages.config 檔案時，它會排除這些標示為開發相依性的相依性。
- **不同的平台的個別 packages.config 檔案**： 時開發多個目標平台的應用程式，通常會針對每個個別的建置環境中有不同的專案檔。 它通常也可以使用不同的專案檔中的不同 NuGet 套件封裝有各種不同平台的支援層級。 NuGet 2.8 中建立不同的 packages.config 檔案不同的平台專屬專案檔，此案例中提供改善的支援。
- **後援至本機快取**： 雖然 NuGet 封裝通常使用從遠端組件庫例如[NuGet gallery](http://www.nuget.org)使用網路連線，有許多用戶端無法連線的案例。 如果沒有網路連接，NuGet 用戶端無法成功安裝封裝-即使這些封裝工作已在本機的 NuGet 快取的用戶端電腦上。 NuGet 2.8 會將後援自動快取加入至封裝管理員主控台。

    快取後援功能不需要任何特定的命令列引數。 此外，後援的快取目前只適用於封裝管理員主控台-行為目前不適用於 [封裝管理員] 對話方塊。
- **Bug 修正**： 有一個主要的 bug 修正進行已更新套件中的效能改進層重新安裝命令。

    除了這些功能與前述效能修正程式，這個版本的 NuGet 也包含許多其他 bug 修正。 有一些版本中解決的 181 總問題。 如需完整清單的工作項目中修正 NuGet 2.8，請檢視[NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all)這一版。

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web Form

- Web Form 範本現在會顯示如何針對 ASP.NET Identity 執行帳戶確認和密碼重設。
- 實體資料的原始檔控制和動態的資料提供者的 Entity Framework 6。 如需詳細資訊，請參閱下列 MSDN 部落格：[動態的資料提供者與 Entity Framework 6 EntityDataSource 控制](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)。

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [屬性路由的增強功能](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [啟動程序支援編輯器範本](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [列舉檢視中的支援](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Unobstrusive 支援 MinLength / MaxLength 屬性](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [支援不顯眼的 Ajax 中的 'this' 內容](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- 各種[bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [全域錯誤處理](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [屬性路由的增強功能](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [說明頁面增強功能](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute 支援](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON 的媒體類型格式器](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [非同步篩選器更佳的支援](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [查詢剖析格式媒體櫃用戶端](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- 各種[bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- 各種[bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework 已更新為 6.1 版執行階段和工具。 Entity Framework (EF) 6.1 是 Entity Framework 6 次要更新，而包含數個 bug 修正和新功能。 如需 EF6.1，包括文件的新功能，連結的詳細資訊請參閱[Entity Framework 版本記錄](https://msdn.microsoft.com/data/jj574253)。 在此版本中的新功能包括：

- **Tooling 合併**提供一致的方式，以建立新的 EF 模型。 此功能會延伸 ADO.NET 實體資料模型精靈，可讓您建立的 Code First 模型，包括從現有的資料庫的反向工程。 這些功能是先前使用的 EF Power Tools 中的 Beta 品質。
- **處理的交易認可失敗**提供新[System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx)是會攔截交易作業使用的新引入的功能。 **CommitFailureHandler**儘管認可交易，允許從連線失敗的自動復原。
- **IndexAttribute**允許將屬性 （或屬性） 上的屬性放在 Code First 模型中指定的索引。 程式碼第一次將接著建立對應的索引在資料庫中。
- **應用程式開發介面的公用對應**提供 EF 對於如何對應到資料行和資料表在資料庫中的屬性及型別資訊的存取權。 在過去的發行此應用程式開發介面是內部。
- **若要設定攔截器，透過 App/Web.config 檔案的能力**（允許加入，而不需要重新編譯應用程式的攔截器）。
- **DatabaseLogger**是新的攔截器，以簡化資料庫的所有作業都記錄到檔案。 舊版的功能結合，這可讓您輕鬆地切換為已部署的應用程式，而不需要重新編譯的資料庫作業的記錄。
- **移轉模型變更偵測**已經過改良，以便更精確; 變更偵測處理程序的效能也已經大幅增強 scaffold 的移轉。
- **效能改進**初始化期間減少的資料庫作業，包括 null 的相等比較中的 LINQ 查詢的最佳化更快速檢視產生 （模型建立） 在多個案例中，且更有效率具有多個關聯的追蹤實體的具體化。

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **雙因素驗證**: ASP.NET Identity 現在支援雙因素驗證。 雙因素驗證提供一層額外的安全性，以您的密碼遭到入侵，萬一您的使用者帳戶。 另外還有兩個因素碼的暴力攻擊的保護。
- **帳戶鎖定：** 提供方法來鎖定使用者，如果使用者可輸入其密碼或兩個因素碼不正確。 無效的嘗試次數和 timespan 的數目的使用者遭到鎖定可以設定。 開發人員可以選擇性地關閉帳戶鎖定的特定使用者帳戶應該要。
- **帳戶確認：** ASP.NET Identity 系統現在支援帳戶確認。 這是很常見的案例中今日大多數的網站，當您註冊新帳戶在網站上，您需要先在執行任何網站中確認您的電子郵件。 電子郵件確認是很有用，因為它可防止假的帳戶建立。 這是網站的非常有用，如果您使用電子郵件做為與您，例如論壇站台、 金融交易、 電子商務或社交網站的使用者進行通訊的方法。
- **密碼重設：** 密碼重設為其中使用者可以重設其密碼如果忘記其密碼的功能。
- **安全性戳記 （各處號）：** 支援重新產生安全性權杖的案例中的使用者，當使用者變更其密碼或任何其他安全性相關資訊，例如移除關聯的登入 （例如 Facebook、 Google、Microsoft 帳戶等等）。 需要這個動作，以確保任何使用舊密碼產生的語彙基元會失效。 在範例專案中，如果您變更使用者的密碼為使用者產生新的權杖，然後任何先前的語彙基元會失效。 這項功能提供一層額外的安全性，您的應用程式，因為當您變更您的密碼時，您會從每個地方 （所有其他瀏覽器） 您已登入此應用程式記錄。
- **讓使用者與角色的擴充性的主索引鍵類型**： 在 ASP.NET Identity 1.0 中，資料表的使用者和角色的主索引鍵的型別為字串。 這表示當 ASP.NET 識別系統永續性的 SQL Server 中使用 Entity Framework 中，我們使用 nvarchar。 共有的堆疊溢位的這個預設實作的許多討論根據內送的意見反應。 我們已提供的擴充攔截，讓您指定應在您的使用者和角色的資料表的主索引鍵。 此擴充攔截，就特別有用的是如果您要移轉您的應用程式，而且應用程式已經儲存的使用者 Id 是 Guid 或將 int 轉換。
- **在使用者和角色支援 IQueryable**： 加入的 UsersStore 和 RolesStore IQueryable 的支援，您可以輕鬆地取得使用者和角色的清單。
- **支援透過 UserManager 的刪除作業**
- **編製索引的使用者名稱**: ASP.NET Identity Entity Framework 中的實作中，我們新增了唯一索引的使用者名稱的新的 IndexAttribute 使用 EF 6.1.0 中。 這可確保永遠是唯一的使用者名稱，並沒有任何競爭，在其中您最後可能會含有重複的使用者名稱。
- **增強式密碼驗證程式：** 密碼驗證程式隨附在 ASP.NET Identity 1.0 是相當基本密碼驗證程式僅已驗證的最小長度。 沒有新的密碼驗證程式，可讓您更充分掌控密碼的複雜性。 請注意，即使您開啟這個密碼中的所有設定，請勿建議您啟用使用者帳戶的雙因素驗證。
- **IdentityFactory 中介軟體 / CreatePerOwinContext**:

    - **使用者管理員**： 您可以使用處理站實作的 OWIN 內容從取得 UserManager 的執行個體。 此模式相當類似於我們用於 AuthenticationManager 獲得登入和登出的 OWIN 內容。 這是建議的方法來取得每個應用程式要求 UserManager 的執行個體。
    - **DbContextFactory**: ASP.NET Identity 保存在 SQL Server 中的識別系統中使用 Entity Framework。 若要這樣做識別系統有 ApplicationDbContext 的參考。 DbContextFactory 中介軟體傳回每個您可以使用您的應用程式中的要求 ApplicationDbContext 執行的個體。
- **ASP.NET Identity 範例 NuGet 封裝**： 範例 NuGet 套件可讓您更輕鬆地安裝並執行範例 ASP.NET 識別的最佳作法。 這是 ASP.NET MVC 應用程式的範例。 請修改以符合您的應用程式，才能在生產中部署此程式碼。 此範例應該安裝在空的 ASP.NET 應用程式。 如需有關封裝的詳細資訊，請移至下列部落格文章：[宣佈適用於 RTM 的 ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN 元件

沒有很多這一版中已修正的 bug。 請參閱[的版本資訊 2.1.0 發行](https://katanaproject.codeplex.com/releases/view/113281)如需詳細資訊。

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

沒有很多這一版中已修正的 bug。 請參閱[的版本資訊 2.0.2 發行](https://github.com/SignalR/SignalR/releases/tag/2.0.2)如需詳細資訊。
