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
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: fe3536f3a40f3a74df47bcc1ca0d0b8416895169
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381674"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [概觀](#overview)
- [安裝注意事項](#installation-notes)
- [軟體需求](#software-requirements)
- [文件](#documentation)
- [支援](#support)
- [將 ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 工具更新](#upgrading)
- [ASP.NET MVC 3 Tools Update (2011 年 4 月 12 日)](#tu-changes)

    - [[加入控制器] 對話方塊現在可以 scaffold 控制器使用檢視和資料存取程式碼](#tu-AddControllerDialog)
    - [改善 「 ASP.NET MVC 3 新增專案 」 對話方塊](#tu-ImprovementsNewDialogBox)
    - [專案範本現在包含 Modernizr 1.7](#tu-Modernizr)
    - [專案範本包含 jQuery、 jQuery UI 和 jQuery 的更新的版本驗證](#tu-UpdatedJQuery)
    - [專案範本現在包含 ADO.NET Entity Framework 4.1 的預先安裝的 NuGet 封裝](#tu-EF)
    - [專案範本會納入預先安裝的 NuGet 套件中的 JavaScript 程式庫](#tu-JavaScriptLibsNuget)
    - [已知問題](#tu-KI)
- [ASP.NET MVC 3 RTM (2011 年 1 月 13 日)](#MVC3RTM)

    - [變更： 更新的版本的 jQuery UI 1.8.7](#RTM-1)
    - [變更： 變更預設值 ModelMetadataProvider 回到 DataAnnotationsModelMetadataProvider](#RTM-2)
    - [已修正： 貼上含有空白結果，裡面要反轉的 Razor 運算式的一部分](#RTM-3)
    - [已修正︰ 重新命名 Razor 檔案在編輯器中開啟時，會停用語法顏色標示和 IntelliSense](#RTM-4)
    - [已知問題](#RTM-KI)
    - [重大變更](#RTM-BC)
- [ASP.NET MVC 3 的候選版 2 (2010 年 12 月 10 日)](#_Toc2)

    - [專案範本變更為包括 jQuery 1.4.4、 jQuery 驗證 1.7 和 jQuery UI 1.8.6y UI 1.8.6](#_Toc2_1)
    - [已新增 「 AdditionalMetadataAttribute"類別](#_Toc2_2)
    - [改善的檢視 Scaffolding](#_Toc2_3)
    - [加入的 Html.Raw 方法](#_Toc2_3)
    - [已重新命名 「 Controller.ViewModel"屬性，「 檢視 」 屬性設定為"ViewBag 」](#_Toc2_4)
    - [已重新命名 「 ControllerSessionStateAttribute 「 類別 」 SessionStateAttribute"](#_Toc2_5)
    - [已重新命名的 RemoteAttribute 「 欄位 」 屬性，以 「 AdditionalFields"](#_Toc2_6)
    - [重新命名為"SkipRequestValidationAttribute"到"AllowHtmlAttribute 」](#_Toc2_7)
    - [已變更 」 Html.ValidationMessage 」 方法，以顯示第一個的實用錯誤訊息](#_Toc2_8)
    - [固定@model宣告，以將空白新增至文件](#_Toc2_9)
    - [已新增 「 FileExtensions"屬性，以支援特定引擎的檔案名稱的檢視引擎](#_Toc2_10)
    - [要發出的"For"屬性的正確值的固定"LabelFor 」 協助程式](#_Toc2_11)
    - [已修正 「 RenderAction"的方法，讓模型繫結期間明確的值優先順序](#_Toc2_12)
    - [重大變更](#_Toc2_BC)
    - [已知問題](#_Toc2_KI)
- [ASP.NET MVC 3 的候選版 (2010 年 11 月 9 日)](#TOC_ASP_NET_3_RC)

    - [在 ASP.NET MVC 3 RC 的新功能](#_Toc276711785)
    - [NuGet 套件管理員](#_Toc276711786)
    - [改善 [新增專案] 對話方塊](#_Toc276711787)
    - [無工作階段的控制站](#_Toc276711788)
    - [新的驗證屬性](#_Toc276711789)
    - [新的多載，如 「 LabelFor"和"LabelForModel 」 方法](#_Toc276711790)
    - [子系動作的輸出快取](#_Toc276711791)
    - [[新增檢視] 對話方塊改進](#_Toc276711792)
    - [詳細的要求驗證](#_Toc276711793)
    - [重大變更](#_Toc276711794)
    - [已知問題](#_Toc276711795)
- [ASP。MVC 3 Beta 資訊 (2010 年 10 月 6 日)](#TOC_ASP_NET_3_Beta)

    - [ASP.NET MVC 3 beta 版的新功能](#0.1__Toc274034215)
    - [NuPack 套件管理員](#0.1__Toc274034216)
    - [改善 [新增專案] 對話方塊](#0.1__Toc274034217)
    - [簡化的方式，來指定強型別在 Razor 檢視中的模型](#0.1__Toc274034218)
    - [支援新的 ASP.NET Web 網頁的 Helper 方法](#0.1__Toc274034219)
    - [其他相依性插入支援](#0.1__Toc274034220)
    - [新的支援不顯眼的 Jquery ajax](#0.1__Toc274034221)
    - [新的支援，適用於不顯眼的 jQuery 驗證](#0.1__Toc274034222)
    - [新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1__Toc274034223)
    - [新的支援，針對檢視執行前執行的程式碼](#0.1__Toc274034224)
    - [新 VBHTML Razor 語法的支援](#0.1__Toc274034225)
    - [Validateinputattribute 套用更精細地控制](#0.1__Toc274034226)
    - [協助程式將底線轉換為連字號使用匿名物件指定的 HTML 屬性名稱](#0.1__Toc274034227)
    - [Bug 修正](#0.1__Toc274034228)
    - [重大變更](#0.1__Toc274034229)
    - [已知問題](#0.1__Toc274034230)
- [免責聲明](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>總覽

本文件說明適用於 Visual Studio 2010 版本的 ASP.NET MVC 3 RTM。 ASP.NET MVC 是使用 「 模型-檢視-控制器 (MVC) 模式開發 Web 應用程式架構。 ASP.NET MVC 3 安裝程式包含下列元件：

- ASP.NET MVC 3 執行階段元件
- ASP.NET MVC 3 Visual Studio 2010 工具
- ASP.NET 網頁的執行階段元件
- ASP.NET Web Pages Visual Studio 2010 工具
- Microsoft Package Manager for.NET (NuGet)
- Visual Studio 2010，可讓 Razor 語法的支援更新。 （如需詳細資訊，請參閱知識庫文件 2483190）。

完整的每個發行前版本的 ASP.NET MVC 3 版本資訊可在 ASP.NET 網站，下列 URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>安裝注意事項

若要安裝 ASP.NET MVC 3 RTM 使用 Web Platform Installer (Web PI)，請瀏覽下列網頁：

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

或者，您可以下載適用於 Visual Studio 2010 的 ASP.NET MVC 3 rtm 安裝程式，從下列頁面：

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 可以安裝，而且可以執行並排顯示使用 ASP.NET MVC 2。

<a id="software-requirements"></a>
## <a name="software-requirements"></a>軟體需求

ASP.NET MVC 3 執行階段元件需要下列軟體：

- .NET framework 第 4 版。 

    ASP.NET MVC 3 Visual Studio 2010 工具需要下列軟體：
- Visual Studio 2010 或 Visual Web Developer 2010 Express。

<a id="documentation"></a>
## <a name="documentation"></a>文件

ASP.NET mvc 的文件可在 MSDN 網站上下列 url:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

教學課程和 ASP.NET MVC 的其他資訊可在下列 URL 在 ASP.NET 網站的 [MVC] 頁面上：

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>支援

這是受到完整支援的版本。 取得技術支援的相關資訊可從[Microsoft 支援服務網站](https://support.microsoft.com/)。

也請隨意關於此版本的問題在 ASP.NET MVC 論壇張貼，其中的 ASP.NET 社群成員可經常提供非正式的支援：

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>將 ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 工具更新

ASP.NET MVC 3 可以與 ASP.NET MVC 2 並存安裝在同一部電腦，讓您彈性地選擇何時要升級至 ASP.NET MVC 3 的 ASP.NET MVC 2 應用程式。

若要手動升級至版本 3 中現有的 ASP.NET MVC 2 應用程式，執行下列作業：

1. 在您的電腦上建立新的空白 ASP.NET MVC 3 專案。 這個專案會包含所需的升級某些檔案。
2. 將下列檔案從 ASP.NET MVC 3 專案複製到您的 ASP.NET MVC 2 專案的對應位置。 您將需要更新任何參考 jQuery 程式庫，以說明新的檔案名稱 (jQuery-1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /內容/主題/\*。\*
3. 複製*封裝*資料夾根目錄中的空白 ASP.NET MVC 3 專案方案，您的方案，方案的.sln 檔案所在的目錄中的根目錄。
4. 如果您的 ASP.NET MVC 2 專案會包含任何區域，/Views/Web.config 檔案複製到*檢視*的每個區域的資料夾。
5. 在這兩個 Web.config 檔案在 ASP.NET MVC 2 專案中，全域搜尋和取代 ASP.NET MVC 版本。 發現下列項目： 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    您可以將它取代為下列：

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. 在 [方案總管] 中，刪除其參考*System.Web.Mvc* （指向 DLL 從第 2 版），然後將參考加入*System.Web.Mvc* (v3.0.0.0)。
7. 將參考加入至 System.Web.WebPages.dll 和 system.web.helpers.dll 的參考。 這些組件位於下列資料夾： 

    - %ProgramFiles%\Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，並選取 卸載專案。 然後再次以滑鼠右鍵按一下專案名稱，然後選取 編輯*ProjectName*.csproj。
9. 找出*ProjectTypeGuids*項目，並取代 {F85E285D-A4E0-4152-9332-AB1D724D3325} 與 {E53F8FEA-EAE0-44A6-8774-FFD645390401}。
10. 儲存變更，以滑鼠右鍵按一下專案，，然後選取 重新載入專案。
11. 在應用程式的根 Web.config 檔案中，新增下列設定，才能*組件*一節。 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. 如果專案參考使用 ASP.NET MVC 2 編譯的任何協力廠商程式庫，新增下列醒目提示*bindingRedirect*下的應用程式根目錄中的 Web.config 檔案的項目*設定*區段： 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>變更在 ASP.NET MVC 3 工具更新

本章節說明自 ASP.NET MVC 3 RTM 發行以來，在 ASP.NET MVC 3 Tools Update 版本中所做的變更。

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>[加入控制器] 對話方塊現在可以 scaffold 控制器使用檢視和資料存取程式碼

Scaffolding 可用來快速產生控制器和檢視您的應用程式。 在產生的程式碼之後，您可以加以編輯以符合您的專案需求。

若要啟動*新增控制器*對話方塊中，在 ASP.NET MVC 3 中，以滑鼠右鍵按一下*控制站*資料夾中的*方案總管] 中*，按一下 [*新增*，然後按一下*控制器*。 已增強 [] 對話方塊中提供額外的 scaffolding 選項。

![](mvc3-release-notes/_static/image1.png)

預設情況下有三個 scaffolding 範本可用。

#### <a name="empty-controller"></a>空白控制器

此範本會產生空白的控制器檔案。 此範本就相當於不勾選*新增建立、 編輯、 詳細資料的動作，請刪除案例*在舊版的 ASP.NET MVC。 如果您選擇此選項，沒有還有其他選項可用。

#### <a name="controller-with-empty-readwrite-actions"></a>具有空白讀取/寫入動作的控制器

此範本會產生具有所有必要的動作方法，但沒有實作程式碼的方法中的控制器檔案。 此範本就相當於檢查*新增建立、 編輯、 詳細資料的動作，請刪除案例*在舊版的 ASP.NET MVC。 如果您選擇此選項，沒有還有其他選項可用。

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>具有讀取/寫入動作和檢視，使用 Entity Framework 的控制器

此範本可讓您快速建立工作的資料輸入的使用者介面。 它會產生處理一組通用需求和案例，如下所示的程式碼：

- *資料存取*。 產生的程式碼會讀取並寫入資料庫中的實體。 它使用 Entity Framework Code First 方法如果您選擇現有的資料內容類別，或是讓範本產生的新*DbContext*類別。 它也可以使用 Entity Framework 資料庫優先 」 或 「 模型優先方法如果您選擇的現有*ObjectContext*類別。
- *驗證*。 產生的程式碼使用以 ASP.NET MVC 模型繫結和中繼資料功能，讓表單提交作業會根據您的模型類別上宣告的規則進行驗證。 這包括內建驗證規則，例如*所需*並*StringLength*屬性和自訂驗證規則。
- *一對多關聯性*。 如果您的模型類別之間定義一對多外部索引鍵關聯性，則產生的程式碼會產生下拉式清單中選取相關的實體。 例如，您可以定義下列模型類別遵循 Entity Framework Code First 慣例： 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    當您接著 scaffold 控制器*產品*類別，它的檢視可讓使用者選擇*分類*物件給每個*產品*執行個體。

    此範本可讓其他選項中的*新增控制器* 對話方塊。 針對*模型類別*，您可以選擇任何模型類別，在您方案中，用來決定使用者將能夠建立或編輯的資料類型：
- 如果您想要使用 Entity Framework Code First 時，您可以選擇任何模型類別。
- 如果您使用 Entity Framework 資料庫優先 」 或 「 Entity Framework 模型優先，請務必選擇您概念模型中定義的實體類別。

針對*的資料內容類別*，您可進行下列選擇：

- 如果您想要使用的程式碼優先 」 而沒有現有的資料內容類別中，選擇 新的資料內容 * *。 然後會為您產生的資料內容類別。
- 如果您想要使用 Code First 與現有的資料內容類別，它在這裡選擇。 它會更新以保存您選取的模型類別。
- 如果您使用 Database First 或 Model First，選擇您的物件內容類別。

對於檢視，請選擇您要使用的檢視引擎，或如果您不要 scaffold 任何檢視，請選擇 None。

您可以選取進階你指定其他選項來產生的檢視表。 例如，您可以選擇要使用的主版頁面的版面配置。

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>改善 「 ASP.NET MVC 3 新增專案 」 對話方塊

您用來建立新的 ASP.NET MVC 3 專案的對話方塊會包含多項改良，如下所示。

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>新的 「 內部網路專案 」 範本

專案範本清單中包含新的內部網路應用程式範本。 此範本包含建置 web 應用程式使用 Windows 驗證，而不表單驗證的設定。 因為內部網路應用程式需要一些無法封裝專案範本中的 IIS 設定，所以範本包含讀我檔案說明如何讓工作在 IIS 中的專案範本。 文件新的內部網路應用程式範本可在 MSDN 網站，下列 URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>已啟用 HTML5 專案範本。

[新增專案] 對話方塊現在包含將 HTML5 專屬功能加入至專案範本的選項。 選取的選項可讓要產生的檢視包含新的 HTML5 `<header>`， `<footer>`，和`<navigation>`項目。

請注意，舊版的瀏覽器不支援 HTML5 專屬標記。 若要解決這項限制，HTML5 專案範本會包含 Modernizr 程式庫的參考。 （請參閱下一節。）

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>專案範本現在包含 Modernizr 1.7

Modernizr 是 JavaScript 程式庫，可讓 CSS 3 和 HTML5 中尚不支援這些功能的瀏覽器的支援。 包含預先安裝的 NuGet 套件，適用於 ASP.NET MVC 3 專案範本中為此文件庫。 如需 Modernizr 的詳細資訊，請參閱[ http://www.modernizr.com/ ](http://www.modernizr.com/)。

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>專案範本包含 jQuery、 jQuery UI 和 jQuery 的更新的版本驗證

專案範本現在包含下列 jQuery 指令碼版本：

- jQuery 1.5.1
- jQuery 驗證 1.8
- jQuery UI 1.8.11

這些程式庫會以預先安裝的 NuGet 套件形式包含項目。

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>專案範本現在包含 ADO.NET Entity Framework 4.1 的預先安裝的 NuGet 封裝

ADO.NET Entity Framework 4.1 包含的 Code First 功能。 程式碼第一次是 ADO.NET Entity Framework 提供的現有的 Database First 和 Model First 模式的替代方案的新開發模式。

程式碼第一次是把焦點放在定義模型使用以 Visual Basic 或 C# 撰寫的 POCO 類別 （「 純舊 CLR 物件 」）。 這些類別接著可以對應至現有的資料庫，或用來產生資料庫結構描述。 可以使用提供額外的組態*DataAnnotations*屬性，或使用 fluent Api。

使用程式碼 Firstwith ASP.NET MVC 的文件位於 ASP.NET 網站，在下列 Url:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>專案範本會納入預先安裝的 NuGet 套件中的 JavaScript 程式庫

當您建立新的 ASP.NET MVC 3 專案時，專案會包含先前所述的 JavaScript 檔案 （例如 Modernizr 程式庫） 組件安裝使用 NuGet 而非直接在專案範本中的指令碼 資料夾中新增指令碼內容。 這可讓您使用的指令碼的新版本發行時，將指令碼更新為最新版本的 NuGet。

比方說，假設新的 jQuery 版本的頻率，專案範本中所包含的 jQuery 版本在某個時間點會過期。 不過，因為 jQuery 是隨附安裝的 NuGet 套件，將會通知您在 [NuGet] 對話方塊中的 jQuery 的較新版本可用時。

因為 jQuery 在檔案名稱中包含的版本號碼，jQuery 更新為最新版本也需要更新`<script>`參考 jQuery 檔案，以便使用新的檔案名稱的標記。 其他內含的指令碼程式庫不包含版本號碼在指令碼名稱中，因此他們可以更輕鬆地為其最新版本更新。

<a id="tu-KI"></a>
## <a name="known-issues"></a>已知問題

- 在某些情況下，安裝可能無法使用錯誤訊息 「 安裝失敗，錯誤碼為 (0x80070643) 」。 如需有關如何解決此問題的資訊，請參閱 <<c0> [ 知識庫文件 2531566](https://support.microsoft.com/kb/2531566)。
- 用於加入控制器的 scaffolding 不會 scaffold 利用 Entity Framework 實體繼承支援的實體。 例如，假設基底*Person*類別，繼承自*學生*類別，scaffolding*學生*類別將會導致產生的程式碼無法編譯。
- 建立新的 ASP.NET MVC 3 專案的方案資料夾內的原因*NullReferenceException*時發生錯誤。 因應措施是解決方案的根目錄中建立 ASP.NET MVC 3 專案，然後將它移入方案資料夾。
- 時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri 部落格，其中討論了如何一起搭配使用今天。
- 在安裝期間，EULA 接受對話方塊會顯示在視窗比預期較小授權條款。
- 當您要在其中編輯 Razor 檢視 (.cshtml 或。*vbhtml*檔案)，檢視。 ASP.NET MVC 3 不包含 Razor 檢視的任何程式碼片段...aspxselecting ASP.NET MVC 的程式碼片段會顯示程式碼的片段
- 如果您安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。 如果您沒有 Visual Web Developer Express，然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，則會套用相同的問題。

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>在 ASP.NET MVC 3 rtm 版本中的變更

本章節描述的變更和 RC2 版本之後，在 ASP.NET MVC 3 RTM 版本中所做的 bug 修正。

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>變更： 更新的版本的 jQuery UI 1.8.7

適用於 Visual Studio 的 ASP.NET MVC 專案範本已更新以包含最新版本的 jQuery UI 程式庫。 範本也包含最少的 jQuery UI，例如相關聯的 CSS 和影像檔所需的資源檔案。

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>變更： 變更預設值 ModelMetadataProvider 回到 DataAnnotationsModelMetadataProvider

導入的 ASP.NET MVC 3 的 RC2 版本*CachedDataAnnotationsMetadataProvider*類別，提供快取上的現有*DataAnnotationsModelMetadataProvider*類別做為效能改善。 不過，某些 bug 回報與這個實作中，因此變更已還原並移至 MVC Futures 專案中，將會位於[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>已修正： 貼上含有空白結果，裡面要反轉的 Razor 運算式的一部分

中的 ASP.NET MVC 3 的發行前版本，當您貼上至 Razor 檔案中，包含空白字元的 Razor 運算式的一部分會反轉所產生的運算式。 例如，請考慮下列 Razor 程式碼區塊：

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

如果您選取的文字 「 第一個參數 「 第一種方法，並將其做為引數，第二個方法，結果如下所示：

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

正確的行為是，貼上作業應該會導致下列：

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

以便正確地貼上作業期間保留運算式已在 RTM 版本中修正此問題。

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>已修正︰ 重新命名 Razor 檔案在編輯器中開啟時，會停用語法顏色標示和 IntelliSense

重新命名 Razor 檔案在編輯器視窗中開啟檔案時，請使用方案總管 中，會導致語法醒目提示和 IntelliSense 停止運作，該檔案。 這個問題已修正，以便反白顯示，而 IntelliSense 將會保留在重新命名後。

<a id="RTM-KI"></a>
## <a name="known-issues"></a>已知問題

- 如果您關閉 Visual Studio 2010 SP1 beta 版，開啟 NuGet 套件管理員主控台時，Visual Studio 會損毀，並嘗試重新啟動。 這將會修正 RTM 版本的 Visual Studio 2010 SP1 中。
- ASP.NET MVC 3 安裝程式才能夠安裝 NuGet 套件管理員的初始版本。 您已安裝的初始版本之後，可以安裝 NuGet，並使用 Visual Studio 延伸模組管理員來更新。 如果您已經有安裝 NuGet，請移至 Visual Studio 延伸模組組件庫更新至最新版的 NuGet。
- 建立新的 ASP.NET MVC 3 專案的方案資料夾內的原因*NullReferenceException*時發生錯誤。 因應措施是解決方案的根目錄中建立 ASP.NET MVC 3 專案，然後將它移入方案資料夾。
- 安裝程式可能會花更長的時間比舊版 ASP.NET MVC 到完成的。 這是因為它會更新 Visual Studio 2010 的元件。
- 時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri 部落格，其中討論了如何一起搭配使用今天。
- 使用 ASP.NET MVC 3 的 Beta 版建立的 CCSHTML 」 和 「 VBHTML 檢視並沒有正確設定其建置動作，與這些檢視的結果在發行專案時，會省略類型。 這些檔案的建置動作值應該設定為 「 內容 」 中。 ASP.NET MVC 3 RTM 修正此問題的新檔案，但不會更正現有的檔案，使用發行前版本所建立之專案的設定。
- ![](mvc3-release-notes/_static/image3.png)
- 在安裝期間，EULA 接受對話方塊會顯示在視窗比預期較小授權條款。
- 當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器中功能表項目，在 Visual Studio 中的將無法使用，而且有任何程式碼片段。
- 如果您安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。 如果您沒有 Visual Web Developer Express，然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，則會套用相同的問題。

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>重大變更

- 在舊版 ASP.NET MVC 中，動作篩選條件會建立每個要求除了在少數情況下。 此行為是永遠不會保證的行為，但只是實作詳細資料，並篩選條件的合約，就是將它們視為無狀態。 在 ASP.NET MVC 3 中，篩選會更積極地快取。 因此，您可能會中斷任何自訂動作篩選條件未正確儲存執行個體的狀態。
- 執行例外狀況篩選條件的順序已變更為具有相同的例外狀況篩選條件*順序*值。 在 ASP.NET MVC 2 和舊版中，例外狀況的篩選具有相同的控制器*順序*值上執行的動作方法上的動作方法的例外狀況篩選條件之前執行時。 這通常會發生這個狀況，例外狀況篩選條件套用時沒有指定之*順序*值。 在 ASP.NET MVC 3 中，此順序已顛倒，因此最特定的例外狀況處理常式會先執行。 如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。
- 名為的新屬性*FileExtensions*已加入至*VirtualPathProviderViewEngine*基底類別。 當 ASP.NET 尋找檢視的路徑 （不是依名稱） 時，會被視為唯一的檢視，以這個新屬性所指定的清單中所包含的副檔名。 若要讓 Web 表單檢視的自訂副檔名註冊自訂的組建提供者，而且提供者使用的完整路徑，而不是名稱參考這些檢視，這是在應用程式中的重大變更。 因應措施是要修改的值*FileExtensions*屬性，以加入自訂的副檔名。
- 直接實作的自訂控制器 factory 實作*IControllerFactory*介面必須提供新的實作*GetControllerSessionBehavior*方法加入此版本中的介面。 一般情況下，建議，不直接實作此介面，並改為衍生您的類別，從*DefaultControllerFactory*。

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>ASP.NET MVC 3 RC2 中的變更

本章節描述 （新功能和 bug 修正） 所做的變更在 ASP.NET MVC 3 RC2 版本自從 RC 版本。

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>專案範本變更為包括 jQuery 1.4.4、 jQuery 驗證 1.7 和 jQuery UI 1.8.6

ASP.NET MVC 3 專案範本現在包含最新版的 jQuery、 jQuery 驗證和 jQuery UI。 jQuery UI 是新的專案範本，並提供實用的使用者介面小工具。 如需有關的 jQuery UI 的詳細資訊，請瀏覽其首頁： [ http://jqueryui.com/ ](http://jqueryui.com/)。

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>已新增 「 AdditionalMetadataAttribute"類別

您可以使用*AdditionalMetadataAttribute*類別，以填入*ModelMetadata.AdditionalValues*模型屬性的字典。

例如，假設檢視模型有應該僅向系統管理員顯示的屬性。 該模型可以標註，以作為索引鍵和 true 使用 AdminOnly，做為值，如下列範例所示的新屬性：

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

此中繼資料開放給任何的顯示器或編輯器範本呈現產品檢視模型時。 您可以決定為應用程式開發人員若要解譯的中繼資料資訊。

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>改善的檢視 Scaffolding

現在使用 scaffolding 檢視的 T4 範本產生範本協助程式方法的呼叫這類*EditorFor*而不是協助程式，例如*TextBoxFor*。 [新增檢視] 對話方塊產生的檢視時，這項變更可改善支援中的資料註解屬性形式的模型上的中繼資料。

新增檢視 scaffolding 也包含改良的偵測和慣例為基礎的模型上的主索引鍵資訊的使用方式。 例如，[新增檢視] 對話方塊中會使用此資訊，以確保主索引鍵值不會包含 scaffold 的可編輯的表單欄位。

預設編輯和建立範本包括 jQuery 指令碼，用戶端驗證所需的參考。

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>加入的 Html.Raw 方法

根據預設，Razor 檢視引擎將 HTML 編碼所有的值。 比方說，下列程式碼片段會將編碼的問候語變數內的 HTML，以便顯示在頁面中做`<strong>Hello World!</strong>`。

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

新*Html.Raw*方法提供簡單的方式，已知為安全的內容時，顯示未編碼的 HTML。 下列範例會顯示相同的字串，但是字串會轉譯為標記：

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>已重新命名 「 Controller.ViewModel"屬性，「 檢視 」 屬性設定為"ViewBag 」

先前*ViewModel*屬性*控制站*採納*檢視*檢視的屬性。 這兩個屬性可用來存取值*ViewDataDictionary*物件使用動態屬性存取子的語法。 已重新命名這兩個屬性必須相同，以避免混淆，並更加一致。

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>已重新命名 「 ControllerSessionStateAttribute 「 類別 」 SessionStateAttribute"

*ControllerSessionStateAttribute*類別 ASP.NET MVC 3 RC 版本中引進。 屬性已重新命名為更簡潔。

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>已重新命名的 RemoteAttribute 「 欄位 」 屬性，以 「 AdditionalFields"

*RemoteAttribute*類別的*欄位*屬性造成了些許混淆，在使用者之間。 重新命名這個屬性，以*AdditionalFields*釐清其意圖。

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>重新命名為"SkipRequestValidationAttribute"到"AllowHtmlAttribute 」

*SkipRequestValidationAttribute*屬性已重新命名為*AllowHtmlAttribute*為了更能代表其預定使用方式。

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>已變更 」 Html.ValidationMessage 」 方法，以顯示第一個的實用錯誤訊息

*Html.ValidationMessage*方法已修正顯示第一個有用的錯誤訊息，而不是只顯示第一個錯誤。

模型繫結，期間*ModelState*字典可以填入多個來源的屬性，包括從模型本身相關的錯誤訊息 (如果它會實作*IValidatableObject*)，從驗證屬性套用至屬性，以及在存取屬性時擲回例外狀況。

當*Html.ValidationMessage*方法會顯示驗證訊息，它會略過包含例外狀況的模型狀態項目，因為這些通常不適合的使用者。 相反地，此方法會尋找所關聯的例外狀況，並顯示該訊息的第一個驗證訊息。 如果不找到任何這類訊息時，它會預設為第一個例外狀況相關聯的一般錯誤訊息。

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>固定@model宣告，以將空白新增至文件

在舊版中， <em>@model</em>檢視頂端的宣告會加入呈現的 HTML 輸出中的空白行。 這個問題已修正，以便宣告不會產生空白字元。

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>已新增 「 FileExtensions"屬性，以支援特定引擎的檔案名稱的檢視引擎

檢視引擎可以傳回檢視，使用明確的檢視路徑，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

第一個檢視引擎一律會嘗試將檢視呈現。 根據預設，Web Form 檢視引擎是第一次的檢視引擎;Web Form 引擎無法轉譯 Razor 檢視，因為發生錯誤。 檢視引擎已經*FileExtensions*它們支援的屬性，用來指定哪些檔案副檔名。 ASP.NET 會判斷是否檢視引擎可以轉譯檔案時，會檢查這個屬性。 這是一項重大變更，而且包含更多詳細資料[的重大變更](#_Toc2_BC)這份文件的區段。

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>要發出的"For"屬性的正確值的固定"LabelFor 」 協助程式

已修正的 bug 的位置*LabelFor*方法轉譯*如*符合的屬性*輸入*項目的*名稱*屬性改為識別碼 根據 W3C， *for*屬性應該符合*輸入*項目的識別碼。

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>已修正 「 RenderAction"的方法，讓模型繫結期間明確的值優先順序

在舊版中，明確的值已傳遞給*RenderAction*方法已由目前的表單值的子系動作內的模型繫結期間忽略。 此修正可確保明確的值優先模型繫結期間。

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>重大變更

- 在舊版的 ASP.NET MVC 中，每個要求除了在少數情況下建立動作篩選條件。 此行為是永遠不會保證的行為，但只是實作詳細資料，並篩選條件的合約，就是將它們視為無狀態。 在 ASP.NET MVC 3 中，篩選會更積極地快取。 因此，您可能會中斷任何自訂動作篩選條件未正確儲存執行個體的狀態。
- 執行例外狀況篩選條件的順序已變更為具有相同的例外狀況篩選條件*順序*值。 在 ASP.NET MVC 2 和舊版中，例外狀況的篩選具有相同的控制器*順序*值，在動作方法上所執行的動作方法的例外狀況篩選條件之前。 這通常會發生這個狀況，例外狀況篩選條件已套用時沒有指定之*順序*值。 在 ASP.NET MVC 3 中，此順序已顛倒，因此最特定的例外狀況處理常式會先執行。 如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。
- 名為的新屬性*FileExtensions*已加入至*VirtualPathProviderViewEngine*基底類別。 當 ASP.NET 尋找檢視的路徑 （不是依名稱） 時，會被視為唯一的檢視，以這個新屬性所指定的清單中所包含的副檔名。 若要讓 Web 表單檢視的自訂副檔名註冊自訂的組建提供者，而且提供者使用的完整路徑，而不是名稱參考這些檢視，這是在應用程式中的重大變更。 因應措施是要修改的值*FileExtensions*屬性，以加入自訂的副檔名。
- 直接實作的自訂控制器 factory 實作<em>IControllerFactory</em>介面必須提供新的實作<em>GetControllerSessionBehavior</em> <em>已新增至這個版本中的介面方法</em>。 一般情況下，建議，不直接實作此介面，並改為衍生您的類別，從<em>DefaultControllerFactory</em>。

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>已知問題

- ASP.NET MVC 3 安裝程式才能夠安裝 NuGet 套件管理員的初始版本。 您已安裝的初始版本之後，可以安裝 NuGet，並使用 Visual Studio 延伸模組管理員來更新。 如果您已經有安裝 NuGet，請移至 Visual Studio 延伸模組組件庫更新至最新版的 NuGet。
- 建立新的 ASP.NET MVC 3 專案的方案資料夾內的原因*NullReferenceException*時發生錯誤。 因應措施是解決方案的根目錄中建立 ASP.NET MVC 3 專案，然後將它移入方案資料夾。
- 安裝程式可能會花更長的時間比舊版 ASP.NET MVC 到完成的。 這是因為它會更新 Visual Studio 2010 的元件。
- 時已安裝 ReSharper，Razor 語法的 IntelliSense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 ASP.NET MVC 3 RC2 中的 Razor IntelliSense 支援，請參閱文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri 部落格，其中討論了如何一起搭配使用今天。
- 使用 ASP.NET MVC 3 的 Beta 版建立的 CSHTML 與 VBHTML 檢視並沒有正確設定其建置動作，與這些檢視的結果在發行專案時，會省略類型。 *建置動作*值這些檔案應該設定為內容 」。 ASP.NET MVC 3 RC2 修正此問題的新檔案，但不修正與搶鮮版所建立之專案的現有檔案的設定。![](mvc3-release-notes/_static/image4.png)
- 在安裝期間，EULA 接受對話方塊會顯示在視窗比預期較小授權條款。
- 當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器中功能表項目，在 Visual Studio 中的將無法使用，而且有任何程式碼片段。
- 如果您安裝 ASP.NET MVC 3 for Visual Web Developer Express，其中未安裝 Visual Studio，在電腦上，然後再安裝 Visual Studio，您必須重新安裝 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共用的 ASP.NET MVC 3 安裝程式會升級的元件。 如果您沒有 Visual Web Developer Express，然後再安裝 Visual Web Developer Express 的電腦上安裝適用於 Visual Studio ASP.NET MVC 3，則會套用相同的問題。
- 如果您已經安裝，安裝 ASP.NET MVC 3 RC 2 並不會更新 NuGet。 若要升級 NuGet，請移至 Visual Studio 擴充功能管理員和它應該顯示為可用的更新。 您可以從該處的最新發行版本升級 NuGet。

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 的候選版

ASP.NET MVC 發行候選版本已於 2010 年 11 月 9 日發行。

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>在 ASP.NET MVC 3 RC 的新功能

本節說明已引進的功能在 beta 版發行以來的 ASP.NET MVC 3 RC 版本。

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet 封裝管理員

ASP.NET MVC 3 包含 NuGet 套件管理員 （前身為 NuPack），也就是 Visual Studio 專案中加入程式庫和工具的整合式的套件管理工具。 此工具會自動執行開發人員需要採取立即進入其來源樹狀結構中的文件庫的步驟。

您可以使用 NuGet 命令列工具、 整合式的主控台視窗在 Visual Studio 2010 內，從 Visual Studio 內容功能表中，和一組 PowerShell cmdlet。

如需 NuGet 的詳細資訊，請參閱[Nuget 文件](https://docs.microsoft.com/nuget/)。

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>改善 [新增專案] 對話方塊

當您建立新的專案時，[新增專案] 對話方塊現在可讓您指定的檢視引擎，以及 ASP.NET MVC 專案類型。

![](mvc3-release-notes/_static/image5.png)

支援修改的範本和對話方塊中的引擎所列出的檢視清單包含在此版本中。

預設範本，如下所示：

空的。 包含最基本的 ASP.NET MVC 專案中，包括預設的目錄結構針對 ASP.NET MVC 專案，包含 ASP.NET MVC 樣式的預設值，而且包含預設 JavaScript 檔案的指令碼目錄 Site.css 檔案的檔案。

網際網路應用程式。 包含範例示範如何使用 ASP.NET MVC 中的成員資格提供者的功能。

在 Windows 登錄中指定，會顯示在對話方塊中的專案範本清單。

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>無工作階段的控制站

新*ControllerSessionStateAttribute*讓您進一步控制工作階段狀態行為的控制站藉由指定[System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx)列舉值。

下列範例示範如何關閉控制站的所有要求的工作階段狀態。

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

下列範例示範如何將所有要求的唯讀工作階段狀態設定為一個控制站。

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>新的驗證屬性

#### <a name="compareattribute"></a>CompareAttribute

新*CompareAttribute*驗證屬性可讓您比較兩個不同的模型屬性的值。 在下列範例中， *ComparePassword*屬性必須符合*密碼*欄位才會生效。

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

新*RemoteAttribute*驗證屬性會充分利用 jQuery 驗證外掛程式中的遠端驗證程式，可讓用戶端驗證，以呼叫執行實際的驗證邏輯的伺服器上的方法。

在下列範例中，*使用者名稱*屬性具有*RemoteAttribute*套用。 編輯這個屬性，在 編輯檢視中的，用戶端驗證將會呼叫名為動作*UserNameAvailable*上*UsersController*以驗證這個欄位的類別。

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

下列範例會顯示對應的控制器。

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

根據預設，屬性會套用至屬性名稱會做為查詢字串參數傳送至動作方法。

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>新的多載，如 「 LabelFor"和"LabelForModel 」 方法

已新增新的多載*LabelFor*並*LabelForModel*方法可讓您指定的標籤文字。 下列範例示範如何使用這些多載。

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>子系動作的輸出快取

*OutputCacheAttribute*支援的使用會呼叫的子系動作的輸出快取*Html.RenderAction*或是*Html.Action* helper 方法。 下列範例會顯示呼叫另一個動作的檢視。

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate*附註動作*OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

此程式碼執行時，會快取 100 秒 Html.Action("GetDate") 呼叫的結果。

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>[新增檢視] 對話方塊改進

當您新增的強型別的檢視時，[新增檢視] 對話方塊現在會篩選出多個不適用類型比在舊版中，例如許多核心.NET Framework 型別。 此外，清單現在會排序類別名稱，而不是完整限定的類型名稱，讓您更輕鬆地尋找類型。 例如，型別名稱現在會顯示如下列範例所示：

類別名稱 （命名空間）

在舊版中，這會顯示如下所示：

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>詳細的要求驗證

*排除*屬性*validateinputattribute 套用*已不存在。 相反地，若要在模型繫結期間略過之模型的特定屬性的要求驗證，請使用 新*SkipRequestValidationAttribute*。

例如，假設動作方法用來編輯部落格文章：

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

下列範例顯示的檢視模型的部落格文章。

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

當使用者送出某些描述屬性的標記時，模型繫結會因要求驗證。 若要停用要求驗證的部落格文章描述的模型繫結期間，套用*SkipRequpestValidationAttribute*屬性，在此範例中所示:。

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

或者，若要關閉之模型的每個屬性的要求驗證，套用*validateinputattribute 套用*值是*false*至動作方法：

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>重大變更

- 執行例外狀況篩選條件的順序已變更為具有相同的例外狀況篩選條件*順序*值。 在 ASP.NET MVC 2 和舊版中，例外狀況的篩選具有相同的控制器*順序*動作方法上所執行的動作方法的例外狀況篩選條件之前。 這通常會發生這個狀況，例外狀況篩選條件已套用時沒有指定之*順序*值。 在 ASP.NET MVC 3 中，此順序已顛倒，因此最特定的例外狀況處理常式會先執行。 如同舊版本中，如果*順序*屬性明確指定，則篩選條件會執行指定的順序。
- 新增名為的新屬性*FileExtensions*要*VirtualPathProviderViewEngine*基底類別。 當查詢檢視的路徑 （而不是名稱），只檢視中所包含的檔案副檔名為視為這個新屬性所指定的清單。 這是一項重大變更的人註冊自訂的組建提供者若要啟用 web 表單檢視的自訂副檔名，並會使用完整路徑，而不是名稱來參考這些檢視表。 因應措施是要修改的值*FileExtensions*屬性，以加入自訂的副檔名。

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>已知問題

- 安裝程式可能會花更長的時間比舊版 ASP.NET MVC 到完成，因為它會更新 Visual Studio 2010 元件的。
- 選取 astrongly 時加入檢視樣板型別檢視表會 scaffold 唯寫屬性。 這些應該一律 scaffolding 所忽略。 [新增檢視] 對話方塊產生的 [編輯] 或 [建立] 檢視時，也則唯讀屬性。 唯讀屬性應該只包含 scaffold 的 顯示 和 清單檢視。
- ASP.NET MVC 3 會連同 Async CTP 安裝時，無法運作偵錯。 ASP.NET MVC 3 不能是已安裝的並行與非同步版 CTP。 解除安裝 Async CTP 修復偵錯。 如需詳細資訊，請參閱[此部落格文章](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)需解除安裝 ASP.NET MVC 3 rc 的所有項目。
- 時已安裝 Resharper，razor Intellisense 不會無法運作。 如果您已安裝 ReSharper 且想要利用 Razor intellisense 支援在 ASP.NET MVC 3 RC，請閱讀[此部落格文章](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)jetbrains 其中討論如何一起搭配使用今天。
- 使用 ASP.NET MVC 3 的 beta 版建立的 CSHTML 與 VBHTML 檢視並沒有其建置動作正確會省略它們發佈。 *建置動作*這些檔案應該設定為 「 內容 」。 ASP.NET MVC 3 RC 修正此問題的新檔案，但不修正與 beta 版所建立之專案的現有檔案的設定。
- 安裝程式可能會花更長的時間比舊版 ASP.NET MVC 到完成，因為它會更新 Visual Studio 2010 元件的。
- 新增檢視 scaffolding 時選取 編輯，強型別 檢視會 scaffold 唯讀屬性。 同樣地，唯寫屬性會包含 scaffold 的 「 顯示 」 檢視。
- 在安裝期間，EULA 接受對話方塊會顯示在視窗比預期較小授權條款。
- 安裝 Visual Studio Async CTP 包含的工具安裝 ASP.NET MVC 3 Razor 隨著導致衝突。 請確定您未嘗試在同一部電腦上安裝 Visual Studio Async CTP 和 Razor 版本。
- 當您在編輯 Razor 檢視 （.cshtml 檔案） 時，請移至控制器中功能表項目，在 Visual Studio 中的將無法使用，而且有任何程式碼片段。

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 beta 版

ASP.NET MVC 3 Beta 已於 2010 年 10 月 6 日發行。 下列資訊僅供 Beta 版本中，並受到任何更新或參考 ASP.NET MVC 3 Release Candidate 節中的變更。

## <a id="0.1__Toc274034215"></a>  新的 Featuresin ASP.NET MVC 3 beta 版

<a id="0.1__Default_validation_system"></a>本節說明已引進的功能的 ASP.NET MVC 3 Beta 版。

### <a id="0.1__Toc274034216"></a>  NuGet 套件管理員

ASP.NET MVC 3 包含 NuGet 套件管理員，也就是新增程式庫的整合式的套件管理工具和 Visual Studio 專案的工具。 大部分的情況下，它會自動化開發人員需要採取立即進入其來源樹狀結構中的文件庫的步驟。

您可以使用 NuGet 命令列工具、 整合式的主控台視窗在 Visual Studio 2010 內，從 Visual Studio 內容功能表中，和一組 PowerShell cmdlet。

如需 NuGet 的詳細資訊，請參閱[NuGet 文件](https://docs.microsoft.com/nuget/)。

### <a id="0.1__Toc274034217"></a>  改善 [新增專案] 對話方塊

當您建立新的專案時，[新增專案] 對話方塊現在可讓您指定的檢視引擎，以及 ASP.NET MVC 專案類型。

![](mvc3-release-notes/_static/image6.png)

在此版本中不包含修改範本清單檢視引擎的對話方塊中所列出的支援。

預設範本，如下所示：

空的。 包含最基本的 ASP.NET MVC 專案中，包括預設的目錄結構針對 ASP.NET MVC 專案中，小型的 Site.css 檔案，其中包含 ASP.NET MVC 樣式的預設值和指令碼目錄的預設 JavaScript 檔案所在的檔案。

網際網路應用程式。 包含範例示範如何使用 ASP.NET MVC 中的成員資格提供者的功能。

### <a id="0.1__Toc274034218"></a>  簡化的方式，來指定強型別在 Razor 檢視中的模型

指定強型別 Razor 檢視的模型類型的方法已經過簡化，使用新@modelCSHTML 檢視指示詞和@ModelTypeVBHTML 檢視指示詞。 在舊版的 ASP.NET MVC 中，您將指定的強型別的模型的 Razor 檢視這種方式：

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

在此版本中，您可以使用下列語法：

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  支援新的 ASP.NET Web 網頁的 Helper 方法

新的 ASP.NET Web Pages 技術包含一組可用於加入檢視及控制器的常用的功能的協助程式方法。 ASP.NET MVC 3 支援使用這些 helper 方法，在控制器和檢視 （適當的話）。 這些方法都包含在 System.Web.Helpers 組件。 下表列出數個 ASP.NET Web Pages helper 方法。

| **協助程式** | **描述** |
| --- | --- |
| 圖表 | 呈現的圖表檢視中。 包含 Chart.ToWebImage、 Chart.Save，等 Chart.Write 的方法。 |
| 密碼編譯 | 它使用雜湊演算法，以建立正確 salted 和雜湊密碼。 |
| WebGrid | 以方格呈現物件 （一般而言，資料庫中的資料） 的集合。 分頁和排序的支援。 |
| Icloudblobstreambinder | 呈現影像。 |
| WebMail | 傳送電子郵件訊息。 |

快速參考主題將列出的協助程式 」 和 「 基本語法是使用 ASP.NET Razor 語法會位於下列 URL 的一部分：

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  其他相依性插入支援

建置 ASP.NET MVC 3 Preview 1 版本上，目前的版本會包含已新增兩個新的服務和四個現有的服務，支援並改善對相依性解析和通用服務定位程式支援。

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>更細緻的控制站的具現化新 IControllerActivator 介面

新的 IControllerActivator 介面會提供更細微的控制，透過控制器會執行個體化的方式透過相依性插入。 下列範例顯示的介面：

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

與此相反角色的控制器 factory。 控制器 factory 會負責尋找控制器類型和具現化該控制器型別的執行個體的 IControllerFactory 介面的實作。

控制器啟動項會只負責具現化控制器型別的執行個體。 它們不會執行查閱控制器類型。 找到適當的控制器類型之後, 控制器 factory 應該委派 IControllerActivator 的執行個體，以處理實際的具現化控制器。

DefaultControllerFactory 類別具有新的建構函式可接受 IControllerFactory 執行個體。 這可讓您套用管理控制站建立的這個部分，而不需要覆寫預設的控制器類型查閱行為的相依性插入。

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>IServiceLocator 介面取代 IDependencyResolver

根據社群意見反應，ASP.NET MVC 3 Beta 版已取代 IServiceLocator 介面使用特定的 ASP.NET MVC 需求精簡 IDependencyResolver 介面。 下列範例示範新的介面：

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

隨著這項變更的詳細資訊，也已取代 dependencyresolver 來註冊類別的 servicelocator，使其類別。 註冊相依性解析程式的作業類似於舊版的 ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

要註冊為提供服務要求的型別，這個介面的實作只應該委派到基礎的相依性插入容器。

所要求型別的任何註冊的服務時，ASP.NET MVC 預期會實作這個介面從 GetService 傳回 null，並從 GetServices 傳回空集合。

新的 dependencyresolver 來註冊類別可讓您註冊新的 IDependencyResolver 介面或通用服務定位程式介面 (IServiceLocator) 實作的類別。 如需有關通用服務定位程式的詳細資訊，請參閱[GitHub 上的 CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)。

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>更細緻的檢視頁面具現化新 IViewActivator 介面

新的 IViewPageActivator 介面會提供更細微的控制，透過檢視頁面會執行個體化的方式透過相依性插入。 這適用於 WebFormView 執行個體和 RazorView 執行個體。 下列範例示範新的介面：

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

這些類別現在接受 IViewPageActivator 建構函式引數，可讓您使用相依性插入來控制 ViewPage、 ViewUserControl 和 WebViewPage 類型執行個體化的方式。

#### <a name="new-dependency-resolver-support-for-existing-services"></a>新的現有服務的相依性解析程式支援

新的版本包括相依性解析支援下列服務：

- 模型驗證提供者。 類別可實作 ModelValidatorProvider 可以註冊相依性解析程式中，系統會使用它們來支援用戶端和伺服器端驗證。
- 模型中繼資料提供者。 單一類別可實作 ModelMetadataProvider 可以註冊相依性解析程式中，系統會使用它來提供中繼資料範本化和驗證系統。
- 值提供者。 類別可實作 ValueProviderFactory 可以註冊相依性解析程式中，系統會使用它們來建立控制器和模型繫結期間所使用的值提供者。
- 模型繫結器。 類別可實作 Bytearraymodelbinderprovider 可以註冊相依性解析程式中，系統會使用它們來建立模型繫結器所使用的模型繫結系統。

### <a id="0.1__Toc274034221"></a>  新的支援不顯眼的 Jquery ajax

ASP.NET MVC 包括 Ajax helper 方法，如下所示：

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

這些方法會使用 JavaScript 來叫用動作方法上的伺服器，而不是使用完整的回傳。 這項功能已更新為利用 jQuery 低調的方式。 而不干擾的方式發出內嵌用戶端指令碼，這些 helper 方法不同的行為從標記發出使用 HTML5 屬性*資料 ajax*前置詞。 然後行為會套用至該標記以藉由參考適當的 JavaScript 檔案。 請確定會參考下列 JavaScript 檔案：

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

中的 Web.config 檔案中的 ASP.NET MVC 3 新專案範本，預設會啟用此功能，但對於現有的專案預設會停用。 如需詳細資訊，請參閱 <<c0> [ 加入應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1_AddedApplicationWideFlagsForClientValida)本文件稍後的。

### <a id="0.1__Toc274034222"></a>  新的支援，適用於不顯眼的 jQuery 驗證

根據預設，ASP.NET MVC 3 beta 版會使用 jQuery 驗證以執行用戶端驗證以不顯眼的方式。 若要啟用不顯眼的用戶端驗證，請如下所示從檢視中的呼叫：

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

這需要 ViewContext.UnobtrusiveJavaScriptEnabled 屬性設為 true 時，您可以這麼做，請執行下列呼叫：

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

也請確定下列 JavaScript 檔案參考。

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

這項功能預設會在 Web.config 檔案中的 ASP.NET MVC 3 新專案範本，在已啟用，但對於現有的專案預設會停用。 如需詳細資訊，請參閱 <<c0> [ 新的應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript](#0.1_AddedApplicationWideFlagsForClientValida)本文件稍後的。

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  新應用程式範圍的旗標，用戶端驗證和不顯眼的 JavaScript

您可以啟用或停用用戶端驗證和不顯眼的 JavaScript 全域使用 HtmlHelper 類別，如下列範例所示的靜態成員：

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

預設專案範本預設會啟用不顯眼的 JavaScript。 您也可以啟用或停用這些功能在您使用下列設定的應用程式的根 Web.config 檔案中：

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

因為您可以啟用這些功能，根據預設，新的多載引進 HtmlHelper 類別，可讓您覆寫預設設定，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

回溯相容性，這兩種功能預設為停用。

### <a id="0.1__Toc274034224"></a>  新的支援，針對檢視執行前執行的程式碼

您現在可以將名為的檔案\_viewstart.cshtml (或\_viewstart.vbhtml) Views 目錄並將該目錄及其子目錄中的 在多個檢視之間共用，以在新增程式碼。 例如，將下列程式碼可能會將放\_viewstart.cshtml ~/Views 資料夾中的頁面：

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

這會設定 [Views] 資料夾內的每個檢視和其所有子資料夾遞迴的版面配置頁面。 檢視正在呈現時中的程式碼\_viewstart.cshtml 檔案執行檢視程式碼執行之前。 \_Viewstart.cshtml 程式碼套用至該資料夾中的每個檢視。

根據預設中的程式碼\_viewstart.cshtml 檔案也會套用至任何子資料夾中的檢視。 不過，個別的子資料夾可以有其自己的版本\_viewstart.cshtml 檔案;，因為案例中，本機版本會優先。 例如，若要執行通用於所有檢視 HomeController 的程式碼，放入\_viewstart.cshtml ~/Views/Home 資料夾中的檔案。

### <a id="0.1__Toc274034225"></a>  新 VBHTML Razor 語法的支援

先前的 ASP.NET MVC preview 包含支援使用以 C# Razor 語法的檢視。 這些檢視會使用.cshtml 檔案的副檔名。 進行中的工作，以支援 Razor 的一部分，ASP.NET MVC 3 beta 版導入了使用.vbhtml 檔擴充功能的 Visual Basic 中的 Razor 語法的支援。

如需使用 Visual Basic 語法 VBHTML 頁面中，請參閱教學課程中的，下列 url:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Validateinputattribute 套用更精細地控制

ASP.NET MVC 一定會包含 validateinputattribute 套用類別，這會叫用核心 ASP.NET 要求驗證基礎結構，藉此確定傳入的要求不包含可能是惡意的輸入。 根據預設，會啟用輸入的驗證。 您可停用要求驗證，使用 validateinputattribute 套用屬性，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

不過，許多 web 應用程式有需要可讓 HTML，而其餘的欄位不應該個別表單欄位。 Validateinputattribute 套用類別現在可讓您指定應該不會包含在要求驗證的欄位清單。

比方說，如果您正在開發的部落格引擎，您可能想要允許在主體和摘要欄位的標記。 這些欄位可能會以兩個輸入項目，每個都有對應的屬性名稱 （「 主體 」 和 「 摘要 」） 的名稱屬性。 若要停用要求驗證這些欄位，指定 （以逗號分隔） 在 ValidateInput 類別，如下列範例所示的 [排除] 屬性的名稱：

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  協助程式將底線轉換為連字號使用匿名物件指定的 HTML 屬性名稱

協助程式方法可讓您指定屬性名稱/值組，使用匿名物件，如下列範例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

這個方法不會讓您使用連字號，屬性名稱，因為連字號不能用在 ASP.NET 中的屬性名稱。 不過，連字號是很重要的自訂 HTML5 屬性;比方說，HTML5 會使用"的資料-"前置詞。

在此同時，底線不能用於在 HTML 中，屬性名稱，但會在屬性名稱中有效。 因此，如果您指定使用的匿名物件的屬性和屬性名稱包含底線、 helper 方法會將轉換的底線連字號。 例如，下列協助程式語法會使用底線：

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

協助程式執行時前, 一個範例就會呈現下列標記：

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Bug 修正

EditorFor 和 DisplayFor 範本協助程式的預設物件範本現在支援 DisplayAttribute.Order 屬性中指定的順序。 （在舊版中，順序設定未使用。）

用戶端驗證現在支援驗證覆寫已套用的驗證屬性的屬性。

JsonValueProviderFactory 現在已註冊的預設值。

## <a id="0.1__Toc274034229"></a>  重大變更

具有相同的順序值的例外狀況篩選條件已變更的例外狀況篩選條件的執行順序。 在 ASP.NET MVC 2 和舊版中，例外狀況篩選，控制站相同的順序與動作方法上所執行的動作方法的例外狀況篩選條件之前。 例外狀況篩選條件已套用指定的順序值時，這通常會是大小寫。 在 ASP.NET MVC 3 中，此順序已顛倒，因此最特定的例外狀況處理常式會先執行。 和較舊的版本，如果明確指定 Order 屬性，篩選會執行指定的順序。

## <a id="0.1__Toc274034230"></a>  已知的問題

在安裝期間，EULA 接受對話方塊會顯示在視窗比預期較小授權條款。

Razor 檢視沒有 IntelliSense 支援和語法反白顯示。 預期在 Visual Studio 中的 Razor 語法的支援，是較新版本的一部分。

當您在編輯 Razor 檢視 （CSHTML 檔案）， <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a>移至控制器在 Visual Studio 中的功能表項目將無法使用，而且有任何程式碼片段。

當使用@model語法來指定強型別的 CSHTML 檢視中，無法辨識的類型的特定語言的快速鍵。 例如， @model int 將無法運作，但@modelInt32 會運作。 這個錯誤的因應措施是當您指定的模型型別時使用的實際型別名稱。

使用時@model語法來指定強型別的 CSHTML 檢視 (或@ModelType指定強型別的 VBHTML 檢視)，不支援可為 null 的型別和陣列宣告。 比方說， @model int？ 不支援。 請改用`@model Nullable<Int32>`。 語法@modelstring [] 也不支援，請改用`@model IList<string>`。

當您將 ASP.NET MVC 2 專案升級至 ASP.NET MVC 3 時，請確定 Web.config 檔的 appSettings 區段新增下列內容：

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

沒有會造成永遠將未經驗證的使用者重新導向至 ~/Account/登入，並忽略在 Web.config 中使用表單驗證設定的表單驗證的已知的問題。因應措施是將下列應用程式設定。

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  免責聲明

© 2011 Microsoft Corporation. 著作權所有，並保留一切權利。 本文件係依 「 做為-是。 」 資訊和檢視，以表示此文件，包括 URL 及其他網際網路網站參考資料，可能會變更恕不另行通知。 貴用戶須自行承擔使用風險。

本文件不提供　貴用戶任何 Microsoft 產品之智慧財產權的法定權利。 貴用戶可以複製本文件供內部參考之用。
