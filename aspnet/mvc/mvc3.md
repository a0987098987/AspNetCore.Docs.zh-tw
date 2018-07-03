---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (包含 2011 年 4 月工具更新)ASP.NET MVC 3 是一個來建置可調整、 以標準為基礎的 web 應用程式，使用堅實的設計模式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: ed3bf81943ac2ffa143cf169c87b3012269d9e77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375787"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(包含 2011 年 4 月工具更新)*
> 
> ASP.NET MVC 3 是用來建置可調整、 以標準為基礎的 web 應用程式使用完善的設計模式，以及 ASP.NET 和.NET Framework 的強大功能的架構。
> 
> 它會並排顯示安裝 ASP.NET MVC 2，因此開始立即使用它 ！
> 
> 下載[這裡安裝程式](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>精選功能

- 整合式的 Scaffolding 系統可透過 NuGet 延伸
- HTML 5 已啟用的專案範本
- 易懂的檢視，包括新的 Razor 檢視引擎
- 功能強大的攔截程序相依性插入和全域動作篩選條件
- 不顯眼的 JavaScript、 jQuery 驗證，與 JSON 繫結的豐富 JavaScript 支援
- *閱讀完整的功能清單[下方](#overview)*

## <a name="top-links"></a>上方的連結

ASP.NET MVC 3 中最新消息

- Phil Haack: [ASP.NET MVC 3 的發行](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3、 WebMatrix、 NuGet、 IIS Express 和 Orchard 推出-Microsoft 年 1 月 Web 版本，在內容中](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie:[宣布新版 ASP.NET MVC 3 中，IIS Express、 SQL CE 4，Web 伺服陣列架構、 Orchard，WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

安裝和說明

- 安裝 ASP.NET MVC 3 使用[Web Platform Installer （建議選項）](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- 安裝 ASP.NET MVC 3 使用[安裝程式可執行檔](https://go.microsoft.com/fwlink/?LinkID=208140)
- 安裝[ASP.NET MVC 3 for Visual Studio 11 開發人員預覽](https://go.microsoft.com/fwlink/?LinkID=208140)
- 讀取[ASP.NET MVC 3 教學課程簡介](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- 取得說明並討論在 ASP.NET MVC 3[論壇](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 概觀

ASP.NET MVC 3 是根據 ASP.NET MVC 1 和 2，新增最棒的功能，同時簡化您的程式碼，並允許更深入的擴充性。 本主題會提供許多包含在此版本中，組織成下列各節中的新功能的概觀：

- [可延伸的 Scaffolding MvcScaffold 整合](#BM_MvcScaffolding)
- [HTML 5 已啟用的專案範本](#BM_HTML5)
- [Razor 檢視引擎](#BM_TheRazorViewEngine)
- [支援多個檢視引擎](#BM_Support_for_Multiple_View_Engines)
- [控制器的改進](#BM_Controller_Improvements)
- [JavaScript 和 Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [模型驗證增強功能](#BM_Model_Validation_Improvements)
- [相依性插入增強功能](#BM_Dependency_Injection_Improvements)
- [其他新功能](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>可延伸的 Scaffolding MvcScaffold 整合

新 Scaffolding 系統容易挑選並開始有效率地使用，如果您不熟悉整個 framework，和一般開發工作自動化，如果您是經驗豐富，並已經知道您的所作所為。

這新的 NuGet 支援*scaffolding*套件，稱為**MvcScaffolding**。 "Scaffolding"由許多軟體技術來表示 「 快速產生基本的外框的軟體，您可以接著編輯和自訂 」 一詞。 我們將建立 ASP.NET mvc scaffolding 封裝是數種案例中大幅有幫助：

- **如果您第一次學習 ASP.NET MVC**，因為它可讓您快速取得一些很有用，工作程式碼，您可以編輯並根據您的需求調整。 它可讓您查看空白頁面，且不知道要從何處開始創傷 ！
- **如果您很了解 ASP.NET MVC，現在瀏覽一些新的附加元件技術**一種物件關聯式對應程式、 檢視引擎、 測試的程式庫，例如等等，因為這項技術的建立者可能也建立了 scaffolding 封裝它。
- **如果您的工作涉及重複建立類似的類別或某種類型的檔案**，因為您可以建立自訂 scaffolder 測試裝置、 部署指令碼，或您所需要的其他任何輸出。 您的小組上的所有人也可以使用您的自訂 scaffolder。

MvcScaffolding 中的其他功能包括：

- C# 和 VB 專案的支援
- ASPX 與 Razor 檢視引擎的支援
- 支援 ASP.NET MVC 區域 scaffolding 並使用自訂檢視版面配置/主機
- 您可以輕鬆地編輯來自訂輸出 T4 範本
- 您可以新增全新 scaffolder 使用 PowerShell 的自訂邏輯和自訂的 T4 範本。 這些 （以及您提供使用者任何自訂參數） 會自動出現在主控台 tab 鍵自動完成清單。
- 您可以取得 NuGet 套件，其中包含適用於不同技術的其他 scaffolder （例如，-概念的其中一個 linq to SQL 目前沒有） 和混合和比對，在一起

ASP.NET MVC 3 Tools Update 包含很棒的 Visual Studio 支援此 scaffolding 系統中，例如：

- 新增控制器 對話方塊現在支援建立、 讀取、 更新和刪除的控制器動作和對應的檢視表的完整自動 scaffolding。 根據預設，這則使用 EF Code First 資料存取程式碼。
- 新增控制器 對話方塊支援*可延伸的架構*透過 NuGet 封裝這類*MvcScaffolding*。 這可讓插入自訂的架構，可讓您使用 ODBCDirect 建立其他的資料存取技術，例如 NHibernate 或甚至是 JET 的架構，如果您因此比較傾向對話方塊 ！

如需有關 ASP.NET MVC 3 中的 Scaffolding 的詳細資訊，請參閱下列資源：

- Steve Sanderson 文章系列，包括： 

    1. [簡介： 建立含有 MvcScaffolding 套件的 ASP.NET MVC 3 專案的結構](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [標準的使用方式： 一般使用案例和選項](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [一對多關聯性](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Scaffolding 動作和單元測試](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [覆寫的 T4 範本](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [這篇文章： 建立自訂 scaffolder](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman 的 post，從他的 PDC 2010 講習會[建置與 Microsoft 「 未具名套件的 Web Love 」 部落格](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 專案範本

新增專案 對話方塊會包含專案範本的核取方塊啟用 HTML 5 版本。 這些範本會利用以舊版瀏覽器中的相容性支援的 html5 和 CSS 3 的 Modernizr 1.7。

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor 檢視引擎

ASP.NET MVC 3 隨附新的檢視引擎，名為 Razor 提供下列優點：

- Razor 語法是整齊精簡，需要最少的按鍵輸入。
- Razor 是容易學習，部分因為它根據現有的語言，例如 C# 和 Visual Basic。
- Visual Studio 包含 Razor 語法的 IntelliSense 和程式碼顏色標示。
- Razor 檢視可以進行單元測試而不需要您執行應用程式，或啟動 web 伺服器。

一些新 Razor 功能包括：

- `@model` 指定要傳遞至檢視的型別語法。
- `@* *@` 註解語法。
- 指定預設值的功能 (例如`layoutpage`) 整個站台的一次。
- `Html.Raw`方法，來顯示文字，而不進行 HTML 編碼它。
- 支援多個檢視之間共用程式碼 (*\_viewstart.cshtml*或是 *\_viewstart.vbhtml*檔案)。

Razor 也包含新的 HTML 協助程式，如下所示：

- `Chart`. 呈現的圖表，提供與 ASP.NET 4 中的 chart 控制項相同的功能。
- `WebGrid`. 呈現的資料格，完整的分頁和排序功能。
- `Crypto`. 它使用雜湊演算法，以建立正確 salted 和雜湊密碼。
- `WebImage`. 呈現影像。
- `WebMail`. 傳送電子郵件訊息。

如需 Razor 的詳細資訊，請參閱下列資源：

- [Razor 簡介 Scott Guthrie 的部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie 的部落格文章簡介@model關鍵字](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie 的部落格文章簡介 Razor 版面配置](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API 快速參考](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>支援多個檢視引擎

**加入檢視**ASP.NET MVC 3 中的對話方塊可讓您選擇您想要使用的檢視引擎，**新的專案**對話方塊可讓您指定專案的預設檢視引擎。 您可以選擇 Web Form 檢視引擎 (ASPX)，Razor 或開放原始碼檢視引擎例如[Spark](http://sparkviewengine.com/)， [NHaml](https://code.google.com/p/nhaml/)，或[NDjango](http://ndjango.org/)。

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>控制器的改進

### <a name="global-action-filters"></a>全域動作篩選條件

有時您想要執行的邏輯在動作方法執行之前或之後的動作方法執行。 若要支援此功能，ASP.NET MVC 2 會提供動作篩選條件。 動作篩選條件會提供宣告式方法，將前置動作和動作後的行為加入至特定的控制器動作方法的自訂屬性。 不過，在某些情況下，您可能想要指定前置動作或後續動作套用至所有動作方法的行為。 MVC 3 可讓您指定全域篩選器加入至`GlobalFilters`集合。 如需全域動作篩選條件的詳細資訊，請參閱下列資源：

- [Scott Guthrie 的部落格上的 MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [ASP.NET MVC 中的篩選](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>新的 「 ViewBag 」 屬性

MVC 2 控制器支援`ViewData`屬性，可讓您將資料傳遞至使用晚期繫結字典 API 檢視範本。 在 MVC 3 中，您也可以使用較為簡單的語法，與`ViewBag`屬性來達成相同目的。 比方說，反而比撰寫`ViewData["Message"]="text"`，您可以撰寫`ViewBag.Message="text"`。 您不需要定義任何強型別類別，才能使用`ViewBag`屬性。 因為它是動態屬性時，您可以改為直接取得或設定屬性，並將它們以動態方式在執行階段解析。 就內部而言，`ViewBag`屬性會儲存成名稱/值組中`ViewData`字典。 (注意： 在大多數的 MVC 3 的發行前版本`ViewBag`屬性名為`ViewModel`屬性。)

### <a name="new-actionresult-types"></a>新的 「 ActionResult"類型

下列`ActionResult`類型和對應的協助程式方法都是新的或增強 MVC 3 中：

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx)。 傳回 「 404 的 HTTP 狀態碼給用戶端。
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx)。 傳回暫時重新導向 （HTTP 302 狀態碼） 或根據布林參數成為永久重新導向 （HTTP 301 狀態碼）。 這項變更，搭配[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx)類別現在有三種方法來執行永久重新導向： `RedirectPermanent`， `RedirectToRoutePermanent`，和`RedirectToActionPermanent`。 這些方法會傳回的執行個體`RedirectResult`具有`Permanent`屬性設定為`true`。
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx)。 傳回使用者指定的 HTTP 狀態碼。

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript 和 Ajax 的增強功能

根據預設，MVC 3 中的 Ajax 和驗證協助程式會使用不顯眼的 JavaScript 方法。 不顯眼的 JavaScript 中，可避免插入 HTML 中的內嵌 JavaScript。 這可讓您的 HTML 小且較不雜亂，並可讓您更輕鬆地交換或自訂的 JavaScript 程式庫。 在 MVC 3 驗證協助程式也使用`jQueryValidate`預設的外掛程式。 如果您想要 MVC 2 的行為，您可以停用使用不顯眼的 JavaScript *web.config*檔案設定。 如需有關 JavaScript 和 Ajax 的增強功能的詳細資訊，請參閱下列資源：

- [不顯眼的 JavaScript 維基百科網站上的基本簡介](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson 的不顯眼的 JavaScript 文章](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson 的不顯眼的 JavaScript 驗證 Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [使用 Razor 和不顯眼的 JavaScript 建立 MVC 3 應用程式](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md)（ASP.NET 網站上教學課程）
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>依預設啟用的用戶端驗證

在舊版的 MVC 中，您需要明確地呼叫`Html.EnableClientValidation`從檢視表，以啟用用戶端驗證的方法。 MVC 3 中這是不再需要因為預設會啟用用戶端驗證。 (您可以停用此使用中的設定*web.config*檔案。)

為了讓用戶端驗證運作，您仍然需要參考適當的 jQuery 和 jQuery 驗證程式庫，在您的網站。 您可以裝載您自己的伺服器上的這些程式庫，或從像是來自 Microsoft 或 Google Cdn 的內容傳遞網路 (CDN) 中參考它們。

### <a name="remote-validator"></a>遠端驗證程式

支援新的 ASP.NET MVC 3 [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx)類別，可讓您充分利用 jQuery 驗證外掛程式中的遠端驗證支援。 這可讓用戶端驗證程式庫會自動呼叫伺服器端以執行只能進行的驗證邏輯伺服器定義的自訂方法。

在下列範例中，`Remote`屬性會指定用戶端驗證將會呼叫名為動作`UserNameAvailable`上`UsersController`類別，以驗證`UserName`欄位。

[!code-csharp[Main](mvc3/samples/sample1.cs)]

下列範例會顯示對應的控制器。

[!code-csharp[Main](mvc3/samples/sample2.cs)]

如需有關如何使用`Remote`屬性，請參閱[How to: ASP.NET MVC 中實作遠端驗證](https://msdn.microsoft.com/library/gg508808(VS.98).aspx)MSDN library 中。

### <a name="json-binding-support"></a>JSON 繫結支援

ASP.NET MVC 3 包含內建的 JSON 繫結支援，可讓動作方法接收 JSON 編碼的資料和模型繫結它至動作方法參數。 這項功能可用於涉及用戶端範本與資料繫結案例。 （用戶端範本可讓您進行格式化並顯示單一資料項目的集合，使用用戶端執行的範本。）MVC 3 可讓您輕鬆地在伺服器上的動作方法，傳送和接收 JSON 資料連接用戶端範本。 如需 JSON 繫結支援的詳細資訊，請參閱**JavaScript 和 AJAX 改進**一節[Scott Guthrie 的 MVC 3 Preview 部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>模型驗證增強功能

### <a name="dataannotations-metadata-attributes"></a>「 DataAnnotations"中繼資料屬性

ASP.NET MVC 3 支援`DataAnnotations`等中繼資料屬性`DisplayAttribute`。

### <a name="validationattribute-class"></a>「 ValidationAttribute"類別

`ValidationAttribute`類別已改進在.NET Framework 4 中，以支援新`IsValid`提供目前的驗證內容，例如哪些物件正在驗證的詳細資訊的多載。 這可讓您可以在此驗證目前模型的另一個屬性為基礎的值更豐富的案例。 比方說，新`CompareAttribute`屬性可讓您比較模型的兩個屬性的值。 在下列範例中，`ComparePassword`屬性必須符合`Password`欄位才會生效。

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>驗證介面

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx)介面可讓您執行模型層級驗證，並可讓您提供的驗證錯誤訊息所特有的整體模型中，或在模型內的兩個屬性之間的狀態. MVC 3 現在會擷取錯誤`IValidatableObject`介面時，模型繫結，並自動旗標或反白顯示受影響的檢視，使用內建的 HTML 表單協助程式中的欄位。

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx)介面可讓 ASP.NET MVC 驗證程式是否支援用戶端驗證，在執行階段探索。 此介面，讓它可以與各種不同的驗證架構整合設計。

如需有關驗證介面的詳細資訊，請參閱**模型驗證改進**一節[Scott Guthrie 的 MVC 3 Preview 部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。 （不過請注意"IValidateObject 」 部落格中的參考，應該是 「 IValidatableObject"）。

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>相依性插入增強功能

套用相依性插入 (DI) 和整合與相依性插入或的控制反轉 (IOC) 容器，則 ASP.NET MVC 3 提供更佳的支援。 DI 支援已新增下列方面：

- 控制站 （註冊和插入控制器 factory，將控制器）。
- 檢視 （註冊和插入檢視引擎，相依性插入檢視頁面）。
- 動作篩選條件 （尋找和插入篩選）。
- 模型繫結器 （註冊和插入）。
- 模型驗證提供者 （註冊和插入）。
- 模型中繼資料提供者 （註冊和插入）。
- 值提供者 （註冊和插入）。

MVC 3 還支援[通用服務定位程式](https://github.com/unitycontainer/commonservicelocator)程式庫和任何支援該程式庫的 DI 容器`IServiceLocator`介面。 它也支援新`IDependencyResolver`介面可讓您更輕鬆地整合 DI 架構。

如需有關 MVC 3 中的 DI 的詳細資訊，請參閱下列資源：

- [服務位置有關的 Brad Wilson 的部落格系列文章](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>其他新功能

### <a name="nuget-integration"></a>NuGet 整合

ASP.NET MVC 3 自動安裝，並將其安裝程序讓 NuGet。 NuGet 是免費的開放原始碼套件管理員，可讓您輕鬆地尋找、 安裝及使用您在專案中的.NET 程式庫和工具。 它適用於所有的 Visual Studio 專案類型 （包括 ASP.NET Web Form 和 ASP.NET MVC）。

NuGet 可讓開發人員維護其程式庫封裝，並在線上組件庫中註冊它們的開放原始碼專案 （例如，專案等 Moq、 NHibernate、 Ninject、 StructureMap、 NUnit，Windsor、 RhinoMocks，Elmah）。 然後是簡單的.NET 開發人員想要使用這些程式庫的其中一個找到的套件，並將它安裝在他們正在處理的專案。

使用 ASP.NET 3 Tools Update 中，專案範本包含 JavaScript 程式庫預先安裝的 NuGet 套件，讓它們可透過 NuGet 更新。 Entity Framework Code First 也會預先安裝為 NuGet 套件。

如需 NuGet 的詳細資訊，請參閱 [NuGet 文件](https://docs.microsoft.com/nuget/) (英文)。

### <a name="partial-page-output-caching"></a>部分頁面輸出快取

ASP.NET MVC 有支援第 1 版自輸出快取的整頁回應。 MVC 3 也支援部分頁面輸出快取，這可讓您輕鬆地快取區域或回應的片段。 如需有關快取的詳細資訊，請參閱**部分的頁面輸出快取**一節[Scott Guthrie 的部落格文章，在 MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)而**子動作的輸出快取**一節[MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)。

### <a name="granular-control-over-request-validation"></a>更精確地控制要求驗證

ASP.NET MVC 有內建的要求驗證，會自動可協助防止 XSS 和 HTML 資料隱碼攻擊。 不過，有時候您想要明確停用要求驗證，例如，如果您想要讓使用者張貼 HTML 內容 （例如，在部落格文章或 CMS 內容）。 您現在可以新增[AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx)屬性加入模型或檢視模型，以停用模型繫結期間以每個屬性為基礎的要求驗證。 如需有關要求驗證的詳細資訊，請參閱下列資源：

- **不顯眼的 JavaScript 和驗證**一節[Scott Guthrie 的部落格文章，在 MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)。
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>可延伸的 [新增專案] 對話方塊

在 ASP.NET MVC 3 中，您可以加入專案範本，檢視引擎和單元測試專案的架構**新的專案** 對話方塊。

### <a name="template-scaffolding-improvements"></a>範本 Scaffolding 增強功能

ASP.NET MVC 3 scaffolding 範本工作表現較佳的識別模型上的主索引鍵屬性，並處理它們適當地比在舊版的 MVC。 （例如，scaffolding 範本現在請確定主索引鍵不會包含 scaffold 的可編輯的表單欄位。）

根據預設，建立與編輯架構現在使用`Html.EditorFor`協助程式，而不是`Html.TextBoxFor`協助程式。 這可改善支援中的資料形式的模型上的中繼資料屬性註釋**加入檢視**對話方塊就會產生一個檢視。

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>「 Html.LabelFor"和"Html.LabelForModel 」 的新多載

已新增新的方法多載`LabelFor`和`LabelForModel`helper 方法。 新的多載可讓您指定或覆寫的標籤文字。

### <a name="sessionless-controller-support"></a>無工作階段的控制站支援

在 ASP.NET MVC 3，您可以指定是否要使用工作階段狀態的控制器類別，而如果是的話，不論工作階段狀態應該是讀取/寫入或唯寫。 如需有關 sessionless 控制器支援的詳細資訊，請參閱[MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)。

### <a name="new-additionalmetadataattribute-class"></a>新的 「 AdditionalMetadataAttribute"類別

您可以使用[AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx)屬性來填入`ModelMetadata.AdditionalValues`模型屬性的字典。 例如，如果檢視模型應該僅向系統管理員顯示的屬性，您可以標註該屬性在下列範例所示：

[!code-csharp[Main](mvc3/samples/sample4.cs)]

此中繼資料開放給任何的顯示器或編輯器範本呈現產品檢視模型時。 它可以決定要解譯的中繼資料資訊。

### <a name="accountcontroller-improvements"></a>AccountController 增強功能

AccountController 網際網路專案範本中的已大幅改善。

### <a name="new-intranet-project-template"></a>新的內部網路專案範本

新的內部網路專案範本已納入將會啟用 Windows 驗證，然後移除 AccountController。
