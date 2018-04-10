---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 0bfe9cdc215226457ccfafff2b85ace87325b91b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [概觀](#overview)
- [安裝注意事項](#installation-notes)
- [軟體需求](#software-requirements)
- [文件 (英文)](#documentation)
- [支援](#support)
- [ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 Tools Update](#upgrading)
- [ASP.NET MVC 3 Tools Update (2011 年 4 月 12 日)](#tu-changes)

    - [[加入控制器] 對話方塊現在可以 scaffold 控制器與檢視和資料存取程式碼](#tu-AddControllerDialog)
    - [改善 「 ASP.NET MVC 3 新專案 」 對話方塊](#tu-ImprovementsNewDialogBox)
    - [專案範本現在包含 Modernizr 1.7](#tu-Modernizr)
    - [專案範本包含 jQuery、 jQuery UI 和 jQuery 的更新的版本驗證](#tu-UpdatedJQuery)
    - [專案範本現在包含 ADO.NET Entity Framework 4.1 以預先安裝的 NuGet 套件](#tu-EF)
    - [專案範本會納入預先安裝的 NuGet 套件中的 JavaScript 程式庫](#tu-JavaScriptLibsNuget)
    - [已知問題](#tu-KI)
- [ASP.NET MVC 3 RTM (2011 年 1 月 13 日)](#MVC3RTM)

    - [變更： 更新至 1.8.7 jQuery UI 的版本](#RTM-1)
    - [變更： 變更的預設 ModelMetadataProvider 回 DataAnnotationsModelMetadataProvider](#RTM-2)
    - [已修正︰ 貼上含有空白結果中要反轉的 Razor 運算式的一部分](#RTM-3)
    - [已修正︰ 重新命名 Razor 檔案在編輯器中開啟時，會停用語法顏色標示和 IntelliSense](#RTM-4)
    - [已知問題](#RTM-KI)
    - [重大變更](#RTM-BC)
- [ASP.NET MVC 3 候選版 2 (2010 年 12 月 10 日)](#_Toc2)

    - [專案範本變更為包含 jQuery 1.4.4、 jQuery 驗證 1.7 和 jQuery UI 1.8.6y UI 1.8.6](#_Toc2_1)
    - [已新增 「 AdditionalMetadataAttribute 」 類別](#_Toc2_2)
    - [改善的檢視 Scaffolding](#_Toc2_3)
    - [加入的 Html.Raw 方法](#_Toc2_3)
    - [重新命名 「 Controller.ViewModel"屬性，「 檢視 」 屬性設定為"ViewBag"](#_Toc2_4)
    - [重新命名 「 ControllerSessionStateAttribute 「 類別 」 SessionStateAttribute"](#_Toc2_5)
    - [重新命名的 RemoteAttribute 「 欄位 」 屬性 「 AdditionalFields"](#_Toc2_6)
    - [重新命名 「 SkipRequestValidationAttribute"至"AllowHtmlAttribute"](#_Toc2_7)
    - [已變更 」 Html.ValidationMessage"方法，以顯示第一個有用的錯誤訊息](#_Toc2_8)
    - [固定@model不要將空白字元加入至文件的宣告](#_Toc2_9)
    - [已新增 「 FileExtensions"屬性，以檢視引擎，以支援引擎特定檔案名稱](#_Toc2_10)
    - [發出正確的值為"For"屬性的固定"LabelFor 「 協助程式](#_Toc2_11)
    - [固定"RenderAction"方法，來提供模型繫結期間明確的值優先順序](#_Toc2_12)
    - [重大變更](#_Toc2_BC)
    - [已知問題](#_Toc2_KI)
- [ASP.NET MVC 3 候選版 (2010 年 11 月 9 日)](#TOC_ASP_NET_3_RC)

    - [ASP.NET MVC 3 RC 的新功能](#_Toc276711785)
    - [NuGet 封裝管理員](#_Toc276711786)
    - [改善 [新增專案] 對話方塊](#_Toc276711787)
    - [無工作階段的控制站](#_Toc276711788)
    - [新的驗證屬性](#_Toc276711789)
    - [新的 「 LabelFor"和"LabelForModel"方法的多載](#_Toc276711790)
    - [子系動作輸出快取](#_Toc276711791)
    - [[新增檢視] 對話方塊改進](#_Toc276711792)
    - [詳細的要求驗證](#_Toc276711793)
    - [重大變更](#_Toc276711794)
    - [已知問題](#_Toc276711795)
- [ASP。MVC 3 Beta 資訊 (2010 年 10 月 6 日)](#TOC_ASP_NET_3_Beta)

    - [ASP.NET MVC 3 Beta 的新功能](#0.1__Toc274034215)
    - [NuPack 封裝管理員](#0.1__Toc274034216)
    - [改善 新增專案對話方塊](#0.1__Toc274034217)
    - [簡化的方式指定強類型模型在 Razor 檢視](#0.1__Toc274034218)
    - [Helper 方法支援新的 ASP.NET Web 網頁](#0.1__Toc274034219)
    - [其他相依性插入的支援](#0.1__Toc274034220)
    - [新的不顯眼的 jQuery 為基礎的 Ajax 支援](#0.1__Toc274034221)
    - [不顯眼的 jQuery 驗證的新支援](#0.1__Toc274034222)
    - [新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1__Toc274034223)
    - [檢視執行之前，所執行的程式碼的新支援](#0.1__Toc274034224)
    - [新 VBHTML Razor 語法的支援](#0.1__Toc274034225)
    - [更精確地控制從而 validateinputattribute 套用](#0.1__Toc274034226)
    - [協助程式轉換成底線連字號為使用匿名物件指定的 HTML 屬性名稱](#0.1__Toc274034227)
    - [Bug 修正](#0.1__Toc274034228)
    - [重大變更](#0.1__Toc274034229)
    - [已知問題](#0.1__Toc274034230)
- [免責聲明](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>總覽

本文件描述 for Visual Studio 2010 版本的 ASP.NET MVC 3 RTM。 ASP.NET MVC 是使用 「 模型檢視控制器 (MVC) 模式開發 Web 應用程式架構。 ASP.NET MVC 3 安裝程式包含下列元件：

- ASP.NET MVC 3 執行階段元件
- ASP.NET MVC 3 Visual Studio 2010 工具
- ASP.NET Web 網頁的執行階段元件
- ASP.NET Web Pages Visual Studio 2010 工具
- Microsoft Package Manager for.NET (NuGet)
- 啟用 Razor 語法支援的 Visual Studio 2010 的更新。 （如需詳細資訊，請參閱知識庫文件 2483190）。

下列 url 的 ASP.NET 網站上可以找到一組完整的每個發行前版本的 ASP.NET MVC 3 版本資訊：

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>安裝注意事項

若要安裝 ASP.NET MVC 3 RTM 使用 Web Platform Installer (Web PI)，請造訪下列網頁：

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

或者，您可以下載適用於 Visual Studio 2010 的 ASP.NET MVC 3 RTM 的安裝程式，從下列頁面：

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 可以安裝並可執行的並行與 ASP.NET MVC 2。

<a id="software-requirements"></a>
## <a name="software-requirements"></a>軟體需求

ASP.NET MVC 3 執行階段元件需要下列軟體：

- .NET framework 第 4 版。 

    ASP.NET MVC 3 Visual Studio 2010 工具需要下列軟體：
- Visual Studio 2010 或 Visual Web Developer 2010 Express。

<a id="documentation"></a>
## <a name="documentation"></a>文件

ASP.NET MVC 文件會提供在 MSDN 網站上下列 url:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

教學課程和 ASP.NET MVC 的其他資訊可在 ASP.NET 網站的下列 URL [MVC] 頁面上：

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>支援

這是完整支援的版本。 取得技術支援人員的相關資訊，請參閱[Microsoft 技術支援網站](https://support.microsoft.com/)。

也請放心將這一版的相關的問題張貼至 ASP.NET MVC 論壇，其中的 ASP.NET 社群成員可經常提供非正式的支援：

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 可以在同一部電腦，讓您彈性選擇何時要升級至 ASP.NET MVC 3 的 ASP.NET MVC 2 應用程式的 ASP.NET MVC 2 並存安裝。

若要手動升級現有的 ASP.NET MVC 2 應用程式版本 3，執行下列作業：

1. 在電腦上建立新的空白 ASP.NET MVC 3 專案。 這個專案會包含所需升級的某些檔案。
2. 將下列檔案從 ASP.NET MVC 3 專案複製到您的 ASP.NET MVC 2 專案的對應位置。 您將需要更新任何參考 jQuery 程式庫來說明新的檔案名稱 (jquery-1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /內容/主題/\*。\*
3. 複製*封裝*根目錄中的空白 ASP.NET MVC 3 專案方案根目錄中的方案時，這是方案的.sln 檔案所在的目錄中的資料夾。
4. 如果您的 ASP.NET MVC 2 專案會包含任何區域，將 /Views/Web.config 檔案複製*檢視*之每個區域的資料夾。
5. 在這兩個 Web.config 檔案在 ASP.NET MVC 2 專案中，全域搜尋並取代 ASP.NET MVC 版本。 搜尋下列文字： 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    使用下列加以取代：

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. 在 [方案總管] 中，刪除其參考*System.Web.Mvc* （指向 DLL 從第 2 版），然後將參考加入*System.Web.Mvc* (v3.0.0.0)。
7. 將參考加入至 System.Web.WebPages.dll 和 system.web.helpers.dll 的參考。 這些組件位於下列資料夾： 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，並選取 卸載專案。 專案名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。
9. 找出*ProjectTypeGuids*項目和取代 {F85E285D-A4E0-4152-9332-AB1D724D3325} 為 {E53F8FEA-EAE0-44A6-8774-FFD645390401}。
10. 儲存變更，以滑鼠右鍵按一下專案，然後選取重新載入專案。
11. 在應用程式的根 Web.config 檔案中，新增下列設定，才能*組件*> 一節。 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. 如果專案參考使用 ASP.NET MVC 2 編譯任何協力廠商程式庫，加入下列反白顯示*bindingRedirect*  下的應用程式根目錄中的 Web.config 檔案的項目*組態*> 一節： 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>變更在 ASP.NET MVC 3 Tools Update

本節說明 ASP.NET MVC 3 RTM 發行版本以來，在 ASP.NET MVC 3 Tools Update 版本中所做的變更。

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>[加入控制器] 對話方塊現在可以 scaffold 控制器與檢視和資料存取程式碼

Scaffolding 是一種快速產生的控制器和檢視您的應用程式。 在產生的程式碼之後，您可以編輯它以符合您的專案需求。

若要啟動*加入控制器*對話方塊中，在 ASP.NET MVC 3，以滑鼠右鍵按一下*控制器*資料夾中的*方案總管] 中*，按一下*新增*，然後按一下 [*控制器*。 對話方塊中已經增強，可提供額外的 scaffolding 選項。

![](mvc3-release-notes/_static/image1.png)

預設情況下有三個 scaffolding 範本可用。

#### <a name="empty-controller"></a>空白控制器

此範本會產生空白的控制器檔案。 此範本就相當於不檢查*新增動作的建立、 編輯、 詳細資料、 刪除案例*在舊版 ASP.NET MVC。 如果您選擇此選項，沒有進一步的選項使用。

#### <a name="controller-with-empty-readwrite-actions"></a>具有空白讀取/寫入動作的控制器

此範本會產生在方法中有所有必要的動作方法，但沒有實作程式碼的控制器檔案。 此範本就相當於檢查*新增動作的建立、 編輯、 詳細資料、 刪除案例*在舊版 ASP.NET MVC。 如果您選擇此選項，沒有進一步的選項使用。

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>控制器會讀取/寫入動作和檢視表、 使用 Entity Framework

此範本可讓您快速地建立工作資料輸入的使用者介面。 它會產生處理一組通用需求和案例，如下所示的程式碼：

- *資料存取*。 產生的程式碼會讀取並寫入資料庫中的實體。 如果您選擇現有的資料內容類別，或是讓範本產生新的 Entity Framework Code First 方法運作*DbContext*類別。 它也可以使用的 Entity Framework 資料庫優先 」 或 「 模型優先 」 方法如果您選擇現有*ObjectContext*類別。
- *驗證*。 產生的程式碼會使用 ASP.NET MVC 模型繫結和中繼資料功能，以便提交表單會根據模型類別上宣告的規則進行驗證。 這包含內建驗證規則，例如*需要*和*StringLength*屬性和自訂驗證規則。
- *一對多關聯性*。 如果模型類別之間定義一對多外部索引鍵關聯性，則產生的程式碼會產生下拉式清單中的選取相關的實體。 例如，您可能會定義下列模型類別遵循 Entity Framework Code First 慣例： 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    當您接著 scaffold 控制器*產品*類別，它的檢視可讓使用者選擇*類別*物件給每個*產品*執行個體。

    此範本可讓中的其他選項*加入控制器* 對話方塊。 如*模型類別*，您可以選擇任何模型類別，在您方案中，用來決定的使用者將能夠建立或編輯的資料類型：
- 如果您想要使用 Entity Framework Code First 時，您可以選擇任何模型類別。
- 如果您使用 Entity Framework 資料庫優先 」 或 「 Entity Framework 模型優先，請務必選擇您概念模型中定義的實體類別。

如*資料內容類別*，您可以進行下列選擇：

- 如果您想要使用程式碼優先 」 而沒有現有的資料內容類別中，選擇 * * 新的資料內容 * *。 然後會為您產生資料內容類別。
- 如果您想要使用 Code First 和現有的資料內容類別，請在此選擇。 它會進行更新以保存您選取的模型類別。
- 如果您使用 Database First 或 Model First，選擇您的物件內容類別。

對於檢視，請選擇您要使用的檢視引擎，或選擇 [無] 如果您不要 scaffold 任何檢視。

您可以選取進階 Optionsto 指定其他選項來產生的檢視表。 例如，您可以選擇版面配置頁或主版頁面，即可使用。

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>改善 「 ASP.NET MVC 3 新專案 」 對話方塊

您用來建立新的 ASP.NET MVC 3 專案的對話方塊包含多項改良，如下所示。

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>新的 「 內部網路專案 」 範本

專案範本清單中包含新的內部網路應用程式範本。 此範本包含設定來建置 web 應用程式使用 Windows 驗證，而不表單驗證。 因為內部網路應用程式需要部分 IIS 設定無法封裝專案範本中，範本包含讀我檔案，其中包含如何在 IIS 中運作，專案範本的指示。 文件新的內部網路應用程式範本位於 MSDN 網站，位於下列 URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>專案範本現在都已啟用 HTML5

[新增專案] 對話方塊現在包含一個選項以將 HTML5 專屬功能加入至專案範本。 選取此選項可讓要產生的檢視包含新的 HTML5 `<header>`， `<footer>`，和`<navigation>`項目。

請注意，舊版的瀏覽器不支援 HTML5 專屬標記。 若要解決這項限制，HTML5 專案範本包含 Modernizr 程式庫的參考。 （請參閱下一節）。

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>專案範本現在包含 Modernizr 1.7

Modernizr 是 JavaScript 程式庫可支援 CSS 3 和 HTML5 的瀏覽器目前還不支援這些功能。 此程式庫是在 ASP.NET MVC 3 專案範本中預先安裝的 NuGet 套件。 如需 Modernizr 的詳細資訊，請參閱[ http://www.modernizr.com/ ](http://www.modernizr.com/)。

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>專案範本包含 jQuery、 jQuery UI 和 jQuery 的更新的版本驗證

專案範本現在包含下列 jQuery 指令碼版本：

- 1.5.1 jQuery
- jQuery 驗證 1.8
- jQuery UI 1.8.11

這些程式庫會包含為預先安裝的 NuGet 套件。

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>專案範本現在包含 ADO.NET Entity Framework 4.1 以預先安裝的 NuGet 套件

ADO.NET Entity Framework 4.1 包含第一個程式碼的功能。 程式碼第一次是為現有的第一個資料庫和 Model First 模式提供替代的 ADO.NET Entity Framework 的新開發模式。

程式碼第一次 」 的重點定義模型使用在 Visual Basic 或 C# 撰寫的 POCO 類別 （「 純舊 CLR 物件 」）。 這些類別接著可以對應到現有的資料庫，或用來產生資料庫結構描述。 可以提供額外的設定，使用*DataAnnotations*屬性，或使用 fluent 應用程式開發的應用程式開發介面。

使用程式碼 Firstwith ASP.NET MVC 的文件位於 ASP.NET 網站，在下列 Url:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>專案範本會納入預先安裝的 NuGet 套件中的 JavaScript 程式庫

當您建立新的 ASP.NET MVC 3 專案時，專案會先安裝包含之前提到的 JavaScript 檔案 （例如 Modernizr 程式庫） 使用 NuGet 而非直接在專案範本中的指令碼 資料夾中新增指令碼內容。 這可讓您使用 NuGet 將指令碼更新為最新版本發行新版本的指令碼時。

例如，提供新的 jQuery 版本的頻率，專案範本中所包含的 jQuery 版本在某個時間點會過期。 不過，因為 jQuery 是隨附安裝的 NuGet 套件，系統會通知您 NuGet 對話方塊中可用的 jQuery 新版時。

因為 jQuery 在檔案名稱中包含的版本號碼，jQuery 更新為最新版本也需要更新`<script>`參考 jQuery 檔案，才能使用新的檔案名稱的標記。 其他內含的指令碼程式庫不包含版本號碼中的指令碼名稱，因此可以更輕鬆地其最新版本更新。

<a id="tu-KI"></a>
## <a name="known-issues"></a>已知問題

- 在某些情況下，安裝可能會因錯誤訊息 「 安裝失敗，錯誤碼為 (0x80070643) 」。 如需如何解決此問題的資訊，請參閱[知識庫文件 2531566](https://support.microsoft.com/kb/2531566)。
- 用於加入控制器的 scaffolding 不會 scaffold 利用 Entity Framework 實體繼承支援的實體。 例如，假設基底*人員*類別所繼承*學生*類別，scaffolding*學生*類別將導致產生不會編譯的程式碼。
- 建立新的 ASP.NET MVC 3 專案的方案資料夾內原因*NullReferenceException*錯誤。 因應措施是在方案根目錄中建立 ASP.NET MVC 3 專案，然後將它移至方案資料夾。
- 時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) （英文） 部落格上的討論如何一起搭配使用今天。
- 在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。
- 當您編輯 Razor 檢視 (.cshtml 或。*vbhtml*檔案)，檢視。 ASP.NET MVC 3 不包含 Razor 檢視的任何程式碼片段...aspxselecting ASP.NET MVC 的程式碼片段會顯示程式碼的片段
- 如果在安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。 如果您沒有 Visual Web Developer Express 以及然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，就會發生相同問題。

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>在 ASP.NET MVC 3 rtm 版本中的變更

本章節描述變更及 RC2 版本之後進行 ASP.NET MVC 3 RTM 版本中的 bug 修正。

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>變更： 更新至 1.8.7 jQuery UI 的版本

Visual Studio 的 ASP.NET MVC 專案範本已更新以包含 jQuery UI 程式庫的最新版本。 範本也包含最少的 jQuery UI，例如相關聯的 CSS 和影像檔所需的資源檔案集。

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>變更： 變更的預設 ModelMetadataProvider 回 DataAnnotationsModelMetadataProvider

RC2 版本的 ASP.NET MVC 3 引進*CachedDataAnnotationsMetadataProvider*類別提供的快取之上現有*DataAnnotationsModelMetadataProvider*類別做為效能改善。 不過，某些錯誤報告與這個實作中，讓變更已還原，且已移至 MVC 未來專案中，將會位於[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>已修正︰ 貼上含有空白結果中要反轉的 Razor 運算式的一部分

在 ASP.NET MVC 3 的發行前版本中，當您將貼到 Razor 檔案，包含空格的 Razor 運算式的一部分會反轉所產生的運算式。 例如，請考慮下列的 Razor 程式碼區塊：

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

如果您選取文字 「 第一個參數 「 第一個方法，並將它做為引數貼到第二種方法，結果如下所示：

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

正確的行為是，貼上作業應該會導致下列：

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

以便正確貼上作業期間保留運算式已經在 RTM 版本中修正此問題。

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>已修正︰ 重新命名 Razor 檔案在編輯器中開啟時，會停用語法顏色標示和 IntelliSense

重新命名 Razor 檔案在編輯器視窗中開啟的檔案時，使用方案總管 會反白顯示語法和 IntelliSense 無法運作，該檔案。 此問題已修正，以便反白顯示，IntelliSense 會維護，重新命名後。

<a id="RTM-KI"></a>
## <a name="known-issues"></a>已知問題

- 如果您關閉 Visual Studio 2010 SP1 Beta，NuGet 封裝管理員主控台開啟時，Visual Studio 會損毀，並嘗試重新啟動。 將會修正此問題在 RTM 版本的 Visual Studio 2010 SP1。
- ASP.NET MVC 3 安裝程式，才能夠安裝的 NuGet 套件管理員的初始版本。 您已安裝的初始版本之後，可以安裝 NuGet，並使用 Visual Studio 擴充功能管理員更新。 如果您已經安裝 NuGet，請移至更新至最新版的 NuGet Visual Studio 擴充程式庫。
- 建立新的 ASP.NET MVC 3 專案的方案資料夾內原因*NullReferenceException*錯誤。 因應措施是在方案根目錄中建立 ASP.NET MVC 3 專案，然後將它移至方案資料夾。
- 安裝程式可能需要更長的時間比舊版 ASP.NET MVC，才能完成。 這是因為它會更新 Visual Studio 2010 的元件。
- 時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) （英文） 部落格上的討論如何一起搭配使用今天。
- 建立 ASP.NET MVC 3 的 Beta 版本的 CCSHTML 和 VBHTML 檢視並沒有正確地設定其建置動作，與這些檢視的結果類型會省略發行專案時。 這些檔案的建置動作值應該設定為 「 內容 」 中。 ASP.NET MVC 3 RTM 修正此問題，對於新的檔案，但不會更正專案發行前版本建立的現有檔案的設定。
- ![](mvc3-release-notes/_static/image3.png)
- 在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。
- 當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器功能表項目，在 Visual Studio 中的將無法使用，而且沒有任何程式碼片段。
- 如果在安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。 如果您沒有 Visual Web Developer Express 以及然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，就會發生相同問題。

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>重大變更

- 在舊版 ASP.NET MVC 中，每個要求除了在少數情況下建立的篩選條件的動作。 這種行為是不保證的行為，但只是實作詳細資料，就是把它視為無狀態篩選器的合約。 在 ASP.NET MVC 3 中，篩選是更積極地快取。 因此，任何自訂動作篩選條件不正確儲存執行個體的狀態可能已損壞。
- 例外狀況篩選條件的執行順序已變更為具有相同的例外狀況篩選條件*順序*值。 在 ASP.NET MVC 2 和舊版中，例外狀況篩選條件具有相同的控制站上*順序*值上執行的動作方法執行時，可以在動作方法的例外狀況篩選條件之前。 這通常會發生此情況時例外狀況篩選條件會套用但未指定*順序*值。 在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。 如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。
- 新的屬性，名為*FileExtensions*已新增至*VirtualPathProviderViewEngine*基底類別。 當 ASP.NET 尋找檢視的路徑 （不是依名稱） 時，則會被視為只有檢視與此新屬性所指定的清單中所包含的副檔名。 這是應用程式中的重大變更，自訂組建提供者註冊以啟用 Web 表單檢視自訂檔案的副檔名，而且提供者所使用的完整路徑，而不是名稱參考這些檢視表。 因應措施是要修改的值*FileExtensions*屬性設定為包含自訂檔案的副檔名。
- 直接實作的自訂控制器 factory 實作*IControllerFactory*介面必須提供新的實作*GetControllerSessionBehavior*方法加入至這一版中的介面。 一般情況下，建議您不要不直接實作這個介面，並且改為衍生您的類別，從*DefaultControllerFactory*。

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>ASP.NET MVC 3 RC2 中的變更

本章節描述 （新功能及 bug 修正） 所做的變更在 ASP.NET MVC 3 RC2 版本的 RC 版本。

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>專案範本變更為包含 jQuery 1.4.4、 jQuery 驗證 1.7 和 jQuery UI 1.8.6

ASP.NET MVC 3 的專案範本現在包含最新版的 jQuery、 jQuery、 驗證和 jQuery UI。 jQuery UI 是新增至專案範本，並提供實用的使用者介面的 widget。 如需 jQuery UI 的詳細資訊，請瀏覽其/首頁： [ http://jqueryui.com/ ](http://jqueryui.com/)。

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>已新增 「 AdditionalMetadataAttribute 」 類別

您可以使用*AdditionalMetadataAttribute*類別，以填入*ModelMetadata.AdditionalValues*模型屬性的字典。

例如，假設 「 檢視模型 」 具有應該只會顯示系統管理員的屬性。 該模型可以標註具有新屬性作為索引鍵和 true 使用 AdminOnly 當做值，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

產品檢視模型呈現時，才可以提供給任何顯示或編輯器範本此中繼資料。 它是由您決定將解譯的中繼資料資訊的應用程式開發人員。

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>改善的檢視 Scaffolding

現在使用 scaffolding 檢視 T4 範本產生範本協助程式方法的呼叫例如*EditorFor*而不是協助程式，例如*TextBoxFor*。 這項變更可改善資料附註屬性的形式在模型上的中繼資料的支援時加入檢視 對話方塊產生的檢視。

加入檢視 scaffolding 也會包含改良的偵測和慣例為基礎的模型上的主索引鍵資訊的使用方式。 例如，加入檢視 對話方塊會使用此資訊以確保主要金鑰的值不會建立結構做為可編輯的表單欄位。

預設編輯和建立範本包含 jQuery 指令碼所需的用戶端驗證的參考。

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>加入的 Html.Raw 方法

根據預設，Razor 檢視引擎將 HTML 編碼所有的值。 例如，下列程式碼片段將編碼的問候語變數內的 HTML，使它顯示在網頁中做`<strong>Hello World!</strong>`。

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

新*Html.Raw*方法提供簡單的方法的內容是否已知為安全時，顯示未編碼的 HTML。 下列範例顯示相同的字串，但是字串會轉譯為標記：

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>重新命名 「 Controller.ViewModel"屬性，「 檢視 」 屬性設定為"ViewBag"

先前， *ViewModel*屬性*控制器*採納*檢視*檢視的屬性。 這兩個屬性提供一個方式來存取值*ViewDataDictionary*物件使用動態屬性存取子語法。 這兩個屬性已重新命名，以避免混淆，並會更一致必須相同。

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>重新命名 「 ControllerSessionStateAttribute 「 類別 」 SessionStateAttribute"

*ControllerSessionStateAttribute*類別在 ASP.NET MVC 3 RC 版本中引進。 屬性已重新命名為更簡潔。

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>重新命名的 RemoteAttribute 「 欄位 」 屬性 「 AdditionalFields"

*RemoteAttribute*類別的*欄位*屬性造成使用者混淆。 重新命名此屬性為*AdditionalFields*將釐清其意圖。

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>重新命名 「 SkipRequestValidationAttribute"至"AllowHtmlAttribute"

*SkipRequestValidationAttribute*屬性已重新命名為*AllowHtmlAttribute*到更能代表其預定使用方式。

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>已變更 」 Html.ValidationMessage"方法，以顯示第一個有用的錯誤訊息

*Html.ValidationMessage*方法已修正顯示第一個有用的錯誤訊息，而不是只顯示第一個錯誤。

模型繫結，期間*ModelState*字典可以擴展多個來源的屬性，包括從模型本身相關的錯誤訊息 (如果它實作*IValidatableObject*)，從套用至屬性的驗證屬性和屬性在存取時擲回例外狀況。

當*Html.ValidationMessage*方法會顯示驗證訊息，它會略過包含例外狀況的模型狀態項目，因為這些通常不適合終端使用者。 相反地，方法會尋找相關聯的例外狀況並不會顯示該訊息的第一個驗證訊息。 如果不找到任何這類訊息，則會預設為第一個例外狀況相關聯的一般錯誤訊息。

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>固定@model不要將空白字元加入至文件的宣告

在舊版中， <em>@model</em>檢視頂端的宣告新增至呈現的 HTML 輸出的空白行。 此問題已修正，讓宣告不會產生空白字元。

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>已新增 「 FileExtensions"屬性，以檢視引擎，以支援引擎特定檔案名稱

檢視引擎可以傳回的檢視使用明確的檢視路徑，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

第一個檢視引擎一律會嘗試將檢視呈現。 根據預設，Web Form 檢視引擎是第一個檢視引擎。Web Form 引擎無法轉譯 Razor 檢視，因為發生錯誤。 檢視引擎現在有*FileExtensions*支援用來指定哪些檔案延伸模組的屬性。 ASP.NET 會判斷是否要檢視引擎可以轉譯檔案時，會檢查這個屬性。 這是一項重大變更和更多詳細資料中包含[的重大變更](#_Toc2_BC)本文件一節。

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>發出正確的值為"For"屬性的固定"LabelFor 「 協助程式

Bug 的修正 where *LabelFor*方法呈現*如*屬性符合*輸入*項目的*名稱*屬性，屬性識別碼 根據 W3C*如*屬性應該符合*輸入*項目的識別碼。

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>固定"RenderAction"方法，來提供模型繫結期間明確的值優先順序

在舊版中，明確的值傳遞至*RenderAction*方法會改用目前的表單值在內部子系動作的模型繫結期間忽略。 修正可確保明確的值優先模型繫結期間。

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>重大變更

- 在舊版 ASP.NET MVC 中，每個要求除了在少數情況下建立動作篩選條件。 這種行為是不保證的行為，但只是實作詳細資料，就是把它視為無狀態篩選器的合約。 在 ASP.NET MVC 3 中，篩選是更積極地快取。 因此，任何自訂動作篩選條件不正確儲存執行個體的狀態可能已損壞。
- 例外狀況篩選條件的執行順序已變更為具有相同的例外狀況篩選條件*順序*值。 在 ASP.NET MVC 2 和舊版中，例外狀況篩選條件具有相同的控制站上*順序*動作方法上所執行的動作方法上的例外狀況篩選條件之前的值。 這通常會發生此情況時套用例外狀況篩選條件但未指定*順序*值。 在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。 如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。
- 新的屬性，名為*FileExtensions*已新增至*VirtualPathProviderViewEngine*基底類別。 當 ASP.NET 尋找檢視的路徑 （不是依名稱） 時，則會被視為只有檢視與此新屬性所指定的清單中所包含的副檔名。 這是應用程式中的重大變更，自訂組建提供者註冊以啟用 Web 表單檢視自訂檔案的副檔名，而且提供者所使用的完整路徑，而不是名稱參考這些檢視表。 因應措施是要修改的值*FileExtensions*屬性設定為包含自訂檔案的副檔名。
- 直接實作的自訂控制器 factory 實作<em>IControllerFactory</em>介面必須提供新的實作<em>GetControllerSessionBehavior</em> <em>已新增至這一版中的介面方法</em>。 一般情況下，建議您不要不直接實作這個介面，並且改為衍生您的類別，從<em>DefaultControllerFactory</em>。

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>已知問題

- ASP.NET MVC 3 安裝程式，才能夠安裝的 NuGet 套件管理員的初始版本。 您已安裝的初始版本之後，可以安裝 NuGet，並使用 Visual Studio 擴充功能管理員更新。 如果您已經安裝 NuGet，請移至更新至最新版的 NuGet Visual Studio 擴充程式庫。
- 建立新的 ASP.NET MVC 3 專案的方案資料夾內原因*NullReferenceException*錯誤。 因應措施是在方案根目錄中建立 ASP.NET MVC 3 專案，然後將它移至方案資料夾。
- 安裝程式可能需要更長的時間比舊版 ASP.NET MVC，才能完成。 這是因為它會更新 Visual Studio 2010 的元件。
- 時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 rc2 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) （英文） 部落格上的討論如何一起搭配使用今天。
- 建立 ASP.NET MVC 3 的 Beta 版本的 CSHTML 和 VBHTML 檢視並沒有正確地設定其建置動作，與這些檢視的結果類型會省略發行專案時。 *建置動作*這些檔案應該設定為內容的值 」。 ASP.NET MVC 3 RC2 修正此問題，對於新的檔案，但不正確的 Beta 版本以建立專案的現有檔案設定。![](mvc3-release-notes/_static/image4.png)
- 在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。
- 當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器功能表項目，在 Visual Studio 中的將無法使用，而且沒有任何程式碼片段。
- 如果在安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。 如果您沒有 Visual Web Developer Express 以及然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，就會發生相同問題。
- 如果您已經安裝，安裝 ASP.NET MVC 3 RC 2 並不會更新 NuGet。 若要升級 NuGet，請移至 Visual Studio 擴充功能管理員和它應該顯示為可用的更新。 您可以將 NuGet 升級為最新版本，從該處。

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 發行候選版本

ASP.NET MVC 發行候選版本已於 2010 年 11 月 9 日發行。

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>ASP.NET MVC 3 RC 的新功能

本節說明已引進的功能測試版自 ASP.NET MVC 3 RC 版本中。

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet 封裝管理員

ASP.NET MVC 3 包含 NuGet 封裝管理員 （之前稱為 NuPack），也就是將程式庫和工具加入至 Visual Studio 專案的整合式的套件管理工具。 此工具會自動取得其來源樹狀結構的程式庫現在開發人員，採取的步驟。

命令列工具、 Visual Studio 2010、 內部整合的主控台視窗，從 Visual Studio 內容功能表中，和一組 PowerShell 指令程式，您可以使用 NuGet。

如需 NuGet 的詳細資訊，請參閱[Nuget 文件](https://docs.microsoft.com/nuget/)。

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>改善 [新增專案] 對話方塊

當您建立新的專案時，[新增專案] 對話方塊現在可讓您指定的檢視引擎，以及 ASP.NET MVC 專案類型。

![](mvc3-release-notes/_static/image5.png)

支援修改的範本及引擎列在對話方塊中的檢視清單包含在此版本中。

預設範本，如下所示：

空的。 包含最基本的 ASP.NET MVC 專案，包括預設的目錄結構 ASP.NET MVC 專案中，包含 ASP.NET MVC 樣式時，預設值，而且包含預設 JavaScript 檔案的指令碼目錄 Site.css 檔案的檔案。

網際網路應用程式。 包含範例示範如何使用 ASP.NET MVC 中的成員資格提供者的功能。

在 Windows 登錄中指定會顯示在對話方塊中的專案範本清單。

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>無工作階段的控制站

新*ControllerSessionStateAttribute*可讓您更充分掌控工作階段狀態行為的控制站藉由指定[System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx)列舉值。

下列範例會示範如何關閉控制站的所有要求的工作階段狀態。

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

下列範例會示範如何設定唯讀的工作階段狀態的所有要求的控制站。

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>新的驗證屬性

#### <a name="compareattribute"></a>CompareAttribute

新*CompareAttribute*驗證屬性可讓您比較兩個不同模型的屬性值。 在下列範例中， *ComparePassword*屬性必須符合*密碼*欄位才會生效。

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

新*RemoteAttribute*驗證屬性會利用 jQuery 驗證外掛程式中的遠端驗證程式，可讓用戶端驗證，以呼叫執行實際驗證邏輯的伺服器上的方法。

在下列範例中， *UserName*屬性具有*RemoteAttribute*套用。 編輯時編輯的檢視，這個屬性，用戶端驗證將會呼叫名為動作*UserNameAvailable*上*UsersController*類別以驗證這個欄位。

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

下列範例顯示相對應的控制項。

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

根據預設，屬性會套用至屬性名稱會做為查詢字串參數傳送至動作方法。

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>新的 「 LabelFor"和"LabelForModel"方法的多載

已新增新的多載*LabelFor*和*LabelForModel*方法可讓您指定的標籤文字。 下列範例會示範如何使用這些多載。

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>子系動作輸出快取

*OutputCacheAttribute*支援輸出快取子系動作，透過呼叫*Html.RenderAction*或*Html.Action* helper 方法。 下列範例會顯示呼叫另一個動作的檢視。

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate*動作以註解*OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

執行此程式碼，Html.Action("GetDate") 要呼叫的結果是針對快取 100 秒。

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>[新增檢視] 對話方塊改進

當您將強型別的檢視時，[新增檢視] 對話方塊現在會篩選出更多非適用類型比在舊版中，例如許多核心.NET Framework 型別。 此外，現在排序清單的類別名稱，而不是完整限定的類型名稱，使其更容易尋找到型別。 例如，型別名稱現在會顯示如下列範例所示：

類別名稱 （命名空間）

在舊版中，這會顯示如下所示：

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>詳細的要求驗證

*排除*屬性*validateinputattribute 套用*不再存在。 相反地，若要在模型繫結期間略過之模型的特定屬性的要求驗證，請使用 新*SkipRequestValidationAttribute*。

例如，假設動作方法用來編輯部落格文章：

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

下列範例顯示的檢視模型的部落格文章。

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

使用者送出某些描述屬性的標記，將會失敗模型繫結，因為要求驗證。 若要停用部落格文章描述的模型繫結期間要求驗證，請套用*SkipRequpestValidationAttribute*屬性，如本範例所示:。

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

或者，若要關閉此模型的每個屬性的要求驗證，套用*validateinputattribute 套用*值是*false*至動作方法：

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>重大變更

- 例外狀況篩選條件的執行順序已變更為具有相同的例外狀況篩選條件*順序*值。 在 ASP.NET MVC 2 和舊版中，例外狀況篩選條件具有相同的控制站上*順序*動作方法上所執行的動作方法上的例外狀況篩選條件之前。 這通常會發生此情況時套用例外狀況篩選條件但未指定*順序*值。 在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。 如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。
- 加入新的屬性，名為*FileExtensions*至*VirtualPathProviderViewEngine*基底類別。 查閱時檢視路徑 （而不是名稱），只檢視中所包含的檔案副檔名為會被視為新的屬性所指定的清單。 這是一項重大變更的人員註冊自訂的組建提供者啟用自訂檔案延伸模組的 web 表單檢視和使用完整路徑，而不是名稱參考這些檢視表。 因應措施是要修改的值*FileExtensions*屬性設定為包含自訂檔案的副檔名。

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>已知問題

- 安裝程式可能需要更長的時間比舊版 ASP.NET MVC 完成，因為它會更新 Visual Studio 2010 的元件。
- 當您選取 astrongly 時加入檢視 scaffolding 型別的檢視 scaffold 唯寫屬性。 這些應該一律忽略 scaffolding。 [新增檢視] 對話方塊也 scaffold 唯讀屬性時產生的 「 編輯 」 或 「 建立 」 檢視。 唯讀屬性只應針對顯示和清單檢視建立結構。
- 偵錯不適用於 ASP.NET MVC 3 與非同步 CTP 一起安裝。 ASP.NET MVC 3 不能安裝-並存與非同步 CTP。 解除安裝非同步 CTP 修復偵錯。 如需詳細資訊，請參閱[此部落格文章](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)需解除安裝 ASP.NET MVC 3 RC 的所有項目。
- 安裝 Resharper，razor Intellisense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 Razor intellisense 支援在 ASP.NET MVC 3 RC，請閱讀[此部落格文章](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)從 JetBrains 其中討論如何一起搭配使用今天。
- 建立 ASP.NET MVC 3 Beta 的 CSHTML 和 VBHTML 檢視並沒有其建置動作正確，從發行會省略它們。 *建置動作*這些檔案應該設定為 「 內容 」。 ASP.NET MVC 3 RC 修正此問題，對於新的檔案，但不會正確設定現有的檔案，如使用 beta 版建立的專案。
- 安裝程式可能需要更長的時間比舊版 ASP.NET MVC 完成，因為它會更新 Visual Studio 2010 的元件。
- 當選取"Edit"的強型別檢視 scaffold 加入檢視 scaffolding 唯讀屬性。 同樣地，唯寫屬性會建立 「 顯示 」 檢視的結構。
- 在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。
- 安裝 Visual Studio 非同步 CTP 中包含的工具安裝 ASP.NET MVC 3 Razor 版本導致衝突。 請確定您未嘗試在同一部電腦上安裝 Visual Studio 非同步 CTP 和 Razor 版本。
- 當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器功能表項目，在 Visual Studio 中的將無法使用，而且沒有任何程式碼片段。

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta 已於 2010 年 10 月 6 日發行。 下列附註僅供測試版，，而且受限於的任何更新或變更上述 ASP.NET MVC 3 發行候選版本 > 一節中所參考。

## <a id="0.1__Toc274034215"></a>  新的 Featuresin ASP.NET MVC 3 Beta

<a id="0.1__Default_validation_system"></a>本節說明已引進的功能在 ASP.NET MVC 3 Beta 版本。

### <a id="0.1__Toc274034216"></a>  NuGet 封裝管理員

ASP.NET MVC 3 包含 NuGet 封裝管理員，也就是新增程式庫的整合式的封裝管理工具和 Visual Studio 專案的工具。 大部分的情況下，它會自動取得其來源樹狀結構的程式庫現在開發人員，採取的步驟。

命令列工具、 Visual Studio 2010 內從 Visual Studio 內容功能表中，整合的主控台 視窗和組 PowerShell 指令程式，您可以使用 NuGet。

如需 NuGet 的詳細資訊，請參閱[NuGet 文件](https://docs.microsoft.com/nuget/)。

### <a id="0.1__Toc274034217"></a>  改善 新增專案對話方塊

當您建立新的專案時，[新增專案] 對話方塊現在可讓您指定的檢視引擎，以及 ASP.NET MVC 專案類型。

![](mvc3-release-notes/_static/image6.png)

在此版本中不支援修改的範本及引擎列在對話方塊中的檢視清單。

預設範本，如下所示：

空的。 包含 ASP.NET MVC 專案，包括預設的目錄結構 ASP.NET MVC 專案小型 Site.css 檔案，其中包含 ASP.NET MVC 樣式時，預設值，而且包含預設 JavaScript 檔案的指令碼目錄檔案的最少。

網際網路應用程式。 包含範例示範如何使用 ASP.NET MVC 中的成員資格提供者的功能。

### <a id="0.1__Toc274034218"></a>  簡化的方式指定強類型模型在 Razor 檢視

指定強型別 Razor 檢視的模型類型的方法已經過簡化，使用新@modelCSHTML 檢視的指示詞和@ModelTypeVBHTML 檢視的指示詞。 在舊版 ASP.NET MVC 中，您會指定 Razor 的強型別的模型檢視這種方式：

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

在此版本中，您可以使用下列語法：

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Helper 方法支援新的 ASP.NET Web 網頁

新的 ASP.NET Web Pages 技術包含一組可用於將常用的功能加入至檢視和控制器的 helper 方法。 ASP.NET MVC 3 支援使用中控制器和檢視這些 helper 方法 （如適用）。 這些方法都包含在 System.Web.Helpers 組件。 下表列出幾個 ASP.NET Web Pages helper 方法。

| **Helper** | **描述** |
| --- | --- |
| 圖表 | 呈現於檢視內的圖表。 包含例如 Chart.ToWebImage、 Chart.Save 和 Chart.Write 方法。 |
| 密碼編譯 | 雜湊演算法來建立正確的使用 salt 和密碼雜湊處理。 |
| WebGrid | 以方格呈現物件 （通常，從資料庫的資料） 的集合。 支援分頁和排序。 |
| WebImage | 呈現影像。 |
| WebMail | 傳送電子郵件訊息。 |

快速參考主題，列出的協助程式及基本語法是使用 ASP.NET Razor 語法文件集，下列 url 的一部分：

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  其他相依性插入的支援

建置在 ASP.NET MVC 3 Preview 1 版本上，目前的版本會包含已加入的支援的兩個新的服務和四個現有的服務和改良的相依性解析和通用服務定位程式支援。

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>新的 IControllerActivator 介面的更細緻的控制站執行個體化

新的 IControllerActivator 介面還提供如何控制站會具現化透過相依性插入更多更細微的控制。 下列範例示範介面：

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

以此對照控制器 factory 的角色。 控制器 factory 會負責尋找控制器型別以及具現化該控制器型別的執行個體的 IControllerFactory 介面的實作。

控制器啟動項只負責具現化控制器型別的執行個體。 它們不執行控制器的型別查閱。 找到適當的控制器類型之後, 控制器 factory 應該委派到 IControllerActivator 的執行個體，以處理控制站的實際的具現化。

DefaultControllerFactory 類別具有新的建構函式可接受 IControllerFactory 執行個體。 這可讓您將管理此部分的控制站建立不必覆寫預設的控制器類型查閱行為的相依性插入套用。

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>IServiceLocator 介面取代 IDependencyResolver

根據社群意見反應，ASP.NET MVC 3 版已取代 IServiceLocator 介面的使用電腦類 IDependencyResolver 介面適用於 ASP.NET MVC 的需求。 下列範例示範新的介面：

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

這項變更的一部分，ServiceLocator 類別也已經取代 dependencyresolver 來註冊類別。 相依性解析程式註冊會類似於舊版 ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

提供已註冊的服務要求類型，此介面的實作只應該委派到基礎的相依性插入容器。

要求類型的任何註冊的服務時，ASP.NET MVC 預期會實作這個介面，可從 GetService 傳回 null，而且從 GetServices 傳回空集合。

新的 dependencyresolver 來註冊類別可讓您註冊新的 IDependencyResolver 介面或通用服務定位程式介面 (IServiceLocator) 實作的類別。 如需通用服務定位程式的詳細資訊，請參閱[GitHub 上的 CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)。

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>新的 IViewActivator 介面的更細緻的檢視頁面具現化

新的 IViewPageActivator 介面還提供更細微的控制，透過檢視頁面如何透過相依性插入具現。 這適用於 WebFormView 執行個體和 RazorView 執行個體。 下列範例示範新的介面：

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

這些類別現在接受 IViewPageActivator 建構函式引數，可讓您使用相依性插入來控制如何 ViewPage、 ViewUserControl 和 WebViewPage 型別會具現化。

#### <a name="new-dependency-resolver-support-for-existing-services"></a>新的相依性解析程式支援現有的服務

新的版本會包含相依性解析支援下列服務：

- 模型驗證提供者。 類別可實作 ModelValidatorProvider 可以註冊相依性解析程式中，系統會使用它們以支援用戶端和伺服器端驗證。
- 模型中繼資料提供者。 單一類別可實作 ModelMetadataProvider 可以註冊相依性解析程式中，系統會使用樣板化及驗證系統提供的中繼資料。
- 值提供者。 類別可實作 ValueProviderFactory 可以註冊相依性解析程式中，系統會使用它們來建立控制器和模型繫結期間所使用的值提供者。
- 模型繫結器。 類別可實作 IModelBinderProvider 可以註冊相依性解析程式中，系統會使用它們來建立模型繫結器所使用的模型繫結系統。

### <a id="0.1__Toc274034221"></a>  新的不顯眼的 jQuery 為基礎的 Ajax 支援

ASP.NET MVC 包括 Ajax helper 方法，如下所示：

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

這些方法會使用 JavaScript，叫用動作方法上的伺服器，而不是使用完整的回傳。 這項功能已更新為利用 jQuery 不顯眼的方式。 而不是 intrusively 發出內嵌用戶端指令碼，這些 helper 方法分隔行為從標記發出使用 HTML5 屬性*資料 ajax*前置詞。 然後行為會套用至標記以參考適當的 JavaScript 檔案。 請確定所參考 下列 JavaScript 檔案：

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

在 ASP.NET MVC 3 新專案範本中，在 Web.config 檔案中預設會啟用此功能，但現有的專案預設會停用。 如需詳細資訊，請參閱[加入應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1_AddedApplicationWideFlagsForClientValida)本文件後面。

### <a id="0.1__Toc274034222"></a>  不顯眼的 jQuery 驗證的新支援

根據預設，ASP.NET MVC 3 Beta 會使用 jQuery 驗證以執行用戶端驗證不顯眼的方式。 若要啟用不顯眼的用戶端驗證，請如下所示從檢視表中的呼叫：

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

這需要 ViewContext.UnobtrusiveJavaScriptEnabled 屬性設定為 true，您可以藉由下列呼叫：

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

也請確定下列 JavaScript 檔案參考。

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

這項功能預設會在 Web.config 檔案在 ASP.NET MVC 3 新專案範本中，已啟用，但現有的專案預設會停用。 如需詳細資訊，請參閱[新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1_AddedApplicationWideFlagsForClientValida)本文件後面。

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript

您可以啟用或停用用戶端驗證和不顯眼的 JavaScript 全域使用靜態成員的 HtmlHelper 類別，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

預設的專案範本預設會啟用不顯眼的 JavaScript。 您也可以啟用或停用這些功能使用下列設定的應用程式的根 Web.config 檔案中：

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

根據預設，您可以啟用這些功能，因為新的多載引進 HtmlHelper 類別，可讓您覆寫預設設定，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

回溯相容性，這兩種功能預設為停用。

### <a id="0.1__Toc274034224"></a>  檢視執行之前，所執行的程式碼的新支援

您現在可以將名為\_viewstart.cshtml (或\_viewstart.vbhtml) 檢視目錄中，請加入程式碼，將該目錄及其子目錄中的 在多個檢視之間共用。 例如，下列程式碼可能會放入\_viewstart.cshtml 頁面 ~/Views 資料夾中：

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

這會設定在 [檢視] 資料夾內的每個檢視和其所有子資料夾遞迴的版面配置頁。 當檢視所呈現中的程式碼\_viewstart.cshtml 檔案執行檢視程式碼執行之前。 \_Viewstart.cshtml 程式碼適用於該資料夾中的每個檢視。

根據預設中的程式碼\_viewstart.cshtml 檔案也會套用至任何子資料夾中的檢視。 不過，個別的子資料夾可以有自己版本的\_viewstart.cshtml 檔案; 該中的情況下，會優先使用本機版本。 例如，若要執行 HomeController 所有檢視都通用的程式碼，放入\_viewstart.cshtml ~/Views/Home 資料夾中的檔案。

### <a id="0.1__Toc274034225"></a>  新 VBHTML Razor 語法的支援

先前的 ASP.NET MVC preview 包含檢視使用根據 C# 的 Razor 語法的支援。 這些檢視會使用.cshtml 檔案的副檔名。 支援 Razor 的進行中工作的一部分，ASP.NET MVC 3 beta 版導入了在 Visual Basic，會使用.vbhtml 副檔名 Razor 語法的支援。

如需使用 Visual Basic 語法 VBHTML 頁面中的簡介，請參閱教學課程中的，下列 url:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  更精確地控制從而 validateinputattribute 套用

ASP.NET MVC 一律具有包含 validateinputattribute 套用類別，可叫用核心 ASP.NET 要求驗證基礎結構，並確定內送要求不包含可能是惡意的輸入。 根據預設，會啟用輸入的驗證。 您可使用 validateinputattribute 套用屬性，如下列範例所示，停用要求驗證：

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

不過，許多 web 應用程式有需要時剩下的欄位不應該允許 HTML，個別的表單欄位。 Validateinputattribute 套用類別現在可讓您指定不應該包含在要求驗證的欄位清單。

比方說，如果您正在開發部落格引擎，您可能想要允許主體和摘要欄位中的標記。 這些欄位可能會表示兩個輸入項目，每個都有對應的屬性名稱 （「 主體 」 和 「 摘要 」） 的名稱屬性。 若要停用要求驗證這些欄位，指定名稱 （以逗號分隔） 中排除 ValidateInput 類別屬性，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  協助程式轉換成底線連字號為使用匿名物件指定的 HTML 屬性名稱

Helper 方法可讓您指定屬性名稱/值組，使用匿名物件，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

這個方法不會讓您使用屬性名稱中的連字號因為連字號不能用在 ASP.NET 中的屬性名稱。 不過，連字號是很重要的自訂 HTML5 屬性;比方說，HTML5 會使用 「 資料-」 前置詞。

同時，底線不能用於在 HTML 中，屬性名稱，但會在屬性名稱中有效。 因此，如果您指定使用匿名的物件、 屬性和屬性名稱包含底線、 helper 方法會將轉換底線連字號。 例如，下列 helper 語法會使用底線：

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

協助程式執行時前, 一個範例就會呈現下列標記：

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Bug 修正

EditorFor 和 DisplayFor 範本協助程式的預設物件範本現在支援 DisplayAttribute.Order 屬性中指定的順序。 （在舊版中，順序設定已無法使用。）

現在，用戶端驗證支援驗證的覆寫已套用的驗證屬性的屬性。

預設現在會註冊的 JsonValueProviderFactory。

## <a id="0.1__Toc274034229"></a>  重大變更

具有相同的順序值的例外狀況篩選條件已經變更的例外狀況篩選條件執行順序。 在 ASP.NET MVC 2 和舊版中，例外狀況的篩選具有相同的順序控制器動作方法上所執行的動作方法上的例外狀況篩選條件之前。 例外狀況篩選條件已套用沒有指定的順序值，這通常會是大小寫。 在 ASP.NET MVC 3 中，這個順序已顛倒，因此最特定的例外狀況處理常式會先執行。 如同舊版本中，如果明確指定 Order 屬性，篩選條件會執行指定的順序。

## <a id="0.1__Toc274034230"></a>  已知的問題

在安裝期間，EULA 接受對話方塊會比預期較小視窗中顯示的授權條款。

Razor 檢視沒有 IntelliSense 支援，也不語法反白顯示。 它預期會在 Visual Studio 中的 Razor 語法支援的是較新版本的一部分。

當您在編輯 Razor 檢視 （CSHTML 檔案）、 <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a>移至控制器在 Visual Studio 中的功能表項目將無法使用，而且沒有程式碼片段。

當使用@model語法來指定強型別的 CSHTML 檢視中，類型的語言特定快速鍵無法辨識。 例如， @model int 將無法運作，但@modelInt32 運作。 此錯誤的因應措施是當您指定的模型型別時使用的實際型別名稱。

當使用@model語法來指定強型別的 CSHTML 檢視 (或@ModelType指定強型別的 VBHTML 檢視)，不支援可為 null 的類型和陣列宣告。 例如， @model int？ 不支援。 請改用`@model Nullable<Int32>`。 語法@modelstring [] 也不支援; 請改用`@model IList<string>`。

當您將 ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 時，請確定 Web.config 檔的 appSettings 區段中加入下列：

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

沒有會造成永遠將未經驗證的使用者重新導向至 ~/Account/登入，略過在 Web.config 中使用表單驗證設定表單驗證的已知的問題。因應措施是將下列應用程式設定。

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  免責聲明

© 2011 Microsoft Corporation. 著作權所有，並保留一切權利。 本文件係"做為-是。 」 資訊及檢視在此文件，包括 URL 及其他網際網路網站參考資料，可能會變更恕不另行通知。 貴用戶須自行承擔使用風險。

本文件不提供　貴用戶任何 Microsoft 產品之智慧財產權的法定權利。 貴用戶可以複製本文件供內部參考之用。
