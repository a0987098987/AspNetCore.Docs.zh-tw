---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (包含 2011 年 4 月更新工具)ASP.NET MVC 3 是用於建置使用信譽良好的設計模式的可擴充、 以標準為基礎的 web 應用程式的架構...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034733"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(包含 2011 年 4 月更新工具)*
> 
> ASP.NET MVC 3 是用來建置可延展、 標準為基礎的 web 應用程式使用信譽良好的設計模式與強大的 ASP.NET 和.NET Framework 的架構。
> 
> 它會由並行安裝與 ASP.NET MVC 2，以便開始使用它現在 ！
> 
> 下載[這裡安裝程式](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>最上層的功能

- 整合的 Scaffolding 系統可透過 NuGet 延伸
- HTML 5 啟用的專案範本
- 具表達能力以外的檢視，包括新的 Razor 檢視引擎
- 功能強大的攔截程序相依性插入和全域動作篩選條件
- 豐富 JavaScript 支援使用不顯眼的 JavaScript、 jQuery、 驗證和 JSON 繫結
- *讀取完整的功能清單[下方](#overview)*

## <a name="top-links"></a>頂端的連結

ASP.NET MVC 3 中最新消息

- Phil Haack: [ASP.NET MVC 3 的發行](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3、 WebMatrix、 NuGet、 IIS Express 及 Orchard 發行-Microsoft 年 1 月 Web 內容中發行](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie:[宣佈適用於 ASP.NET MVC 3、 IIS Express、 SQL CE 4、 Web 伺服陣列架構、 Orchard、 WebMatrix 發行](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

安裝和說明

- 安裝 ASP.NET MVC 3 使用[Web Platform Installer （建議選項）](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- 安裝 ASP.NET MVC 3 使用[安裝程式可執行檔](https://go.microsoft.com/fwlink/?LinkID=208140)
- 安裝[ASP.NET MVC 3 Visual Studio 11 開發人員預覽](https://go.microsoft.com/fwlink/?LinkID=208140)
- 讀取[ASP.NET MVC 3 教學課程簡介](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- 如需協助，並討論在 ASP.NET MVC 3[論壇](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 概觀

ASP.NET MVC 3 建置在 ASP.NET MVC 1 和 2，加入很棒的功能，同時簡化您的程式碼，並允許更深入的擴充性。 本主題提供許多新功能包含在此版本中，組織成下列各區段的概觀：

- [可延伸的 Scaffolding MvcScaffold 整合](#BM_MvcScaffolding)
- [HTML 5 啟用的專案範本](#BM_HTML5)
- [Razor 檢視引擎](#BM_TheRazorViewEngine)
- [支援多個檢視引擎](#BM_Support_for_Multiple_View_Engines)
- [控制器的增強功能](#BM_Controller_Improvements)
- [JavaScript 和 Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [模型驗證增強功能](#BM_Model_Validation_Improvements)
- [相依性插入增強功能](#BM_Dependency_Injection_Improvements)
- [其他新功能](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>可延伸的 Scaffolding MvcScaffold 整合

新的 Scaffold 系統容易來拾取及開始全新的 framework 中，您是否有效率地使用並自動化一般開發工作，如果您是有經驗，並已經知道您所做。

支援這種新的 nuget *scaffolding*呼叫封裝**MvcScaffolding**。 許多軟體技術 」 的 Scaffolding 」 用來表示 「 快速產生您的軟體，您可以基本大綱然後編輯和自訂 」 一詞。 正在建立 ASP.NET MVC 的 scaffolding 封裝是在許多狀況下大有幫助：

- **如果您第一次學習 ASP.NET MVC**，因為它可讓您快速取得一些有用，工作程式碼，您可以編輯並根據您的需求調整。 它可讓您查看空白頁，且不知道要從何處開始創傷 ！
- **如果您很了解 ASP.NET MVC，並立即瀏覽一些新的附加元件技術**物件關聯式對應工具，檢視引擎，測試程式庫，例如等，因為該技術的建立者可能也建立了 scaffolding 封裝它。
- **如果您的工作包括重複建立類似的類別或某種類型的檔案**，因為您可以建立自訂 scaffolder 測試裝置、 部署指令碼，或您所需要的其他任何輸出。 您的小組中的所有成員也可以使用您的自訂 scaffolder。

MvcScaffolding 中的其他功能包括：

- C# 和 VB 專案的支援
- ASPX 與 Razor 檢視引擎的支援
- 支援 ASP.NET MVC 區域的 scaffolding 及使用自訂檢視配置母片
- 您可以輕鬆地編輯來自訂輸出 T4 範本
- 您可以新增全新 scaffolder 使用 PowerShell 的自訂邏輯和自訂 T4 範本。 這些 （和任何您已獲得它們的自訂參數） 會自動出現在主控台 tab 鍵自動完成清單。
- 您可以取得 NuGet 套件包含適用於不同技術的其他 scaffolder （例如，目前已的概念證明一個 linq to SQL） 並混用，而且它們一起與相符

ASP.NET MVC 3 Tools Update 包含絕佳的 Visual Studio 支援此 scaffolding 系統，例如：

- 加入控制器 對話方塊現在支援建立、 讀取、 更新和刪除控制器動作和對應的檢視表的完整自動 scaffolding。 根據預設，這 scaffold 使用 EF Code First 資料存取程式碼。
- 加入控制器 對話方塊支援*可擴充 scaffold*透過 NuGet 套件*MvcScaffolding*。 這允許插入到對話方塊可讓您使用 ODBCDirect 建立其他的資料存取技術，例如 NHibernate 或甚至 JET scaffold，如果您正在因此比較傾向自訂 scaffold ！

如需 ASP.NET MVC 3 中的 Scaffolding 的詳細資訊，請參閱下列資源：

- Steve Sanderson 張貼序列，包括： 

    1. [簡介： Scaffold ASP.NET MVC 3 專案包含 MvcScaffolding 套件](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [標準的使用方式： 一般使用案例和選項](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [一對多關聯性](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Scaffolding 動作和單元測試](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [覆寫 T4 範本](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [這篇文章： 建立自訂 scaffolder](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- 他 PDC 2010 的工作階段 Scott Hanselman 文章[建置與 Microsoft 「 未命名套件的 Web 愛 」 部落格](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 專案範本

新增專案 對話方塊包含核取方塊啟用的 html5 版本的專案範本。 這些範本會利用提供的舊版瀏覽器中的相容性支援 HTML 5 和 CSS 3 Modernizr 1.7。

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor 檢視引擎

ASP.NET MVC 3 隨附新的檢視引擎，名為 Razor，提供下列優點：

- Razor 語法是全新且精確，需要最少的按鍵輸入。
- Razor 很容易了解，部分因為它根據現有的語言，例如 C# 和 Visual Basic。
- Visual Studio 包含 Razor 語法的 IntelliSense 和程式碼顏色標示。
- Razor 檢視可進行單元測試而不需要您執行應用程式，或啟動 web 伺服器。

某些新的 Razor 功能包括下列各項：

- `@model` 指定要傳遞到檢視類型的語法。
- `@* *@` 註解語法。
- 指定預設值的功能 (例如`layoutpage`) 一次完整的站台。
- `Html.Raw`方法而不進行 HTML 編碼的文字顯示它。
- 支援多個檢視之間共用程式碼 (*\_viewstart.cshtml*或 *\_viewstart.vbhtml*檔案)。

Razor 還包含新的 HTML helper，如下所示：

- `Chart`. 呈現的圖表，提供 ASP.NET 4 中的 chart 控制項與相同的功能。
- `WebGrid`. 轉譯資料方格中，完成，但分頁和排序功能。
- `Crypto`. 雜湊演算法來建立正確的使用 salt 和密碼雜湊處理。
- `WebImage`. 呈現影像。
- `WebMail`. 傳送電子郵件訊息。

如需 Razor 的詳細資訊，請參閱下列資源：

- [Razor 簡介 Scott Guthrie 的部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie 的部落格文章簡介@model關鍵字](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie 的部落格文章介紹 Razor 版面配置](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API 的快速參考](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>支援多個檢視引擎

**加入檢視**ASP.NET MVC 3 中的對話方塊可讓您選擇您想要使用的檢視引擎和**新專案** 對話方塊可讓您指定專案的預設檢視引擎。 您可以選擇 Web 表單檢視引擎 (ASPX)、 Razor、 或開放原始碼檢視引擎例如[Spark](http://sparkviewengine.com/)， [NHaml](https://code.google.com/p/nhaml/)，或[NDjango](http://ndjango.org/)。

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>控制器的增強功能

### <a name="global-action-filters"></a>全域動作篩選條件

有時候您會想要執行的邏輯在動作方法執行之前或之後執行的動作方法。 若要支援此作業，ASP.NET MVC 2 會提供動作篩選條件。 動作篩選條件是自訂屬性，提供將動作前和動作後的行為加入至特定控制器動作方法的宣告式方法。 不過，在某些情況下，您可能想要指定動作前或後置動作套用至所有動作方法的行為。 MVC 3 可讓您藉由加入指定全域篩選`GlobalFilters`集合。 如需全域動作篩選條件的詳細資訊，請參閱下列資源：

- [MVC 3 Preview 上 Scott Guthrie 的部落格](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [ASP.NET MVC 中的篩選](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>新的 「 ViewBag 」 屬性

MVC 2 控制器支援`ViewData`可讓您將資料傳遞給使用晚期繫結字典 API 檢視範本的屬性。 在 MVC 3 中，您也可以使用較為簡單的語法與`ViewBag`可以完成相同工作的屬性。 例如，反而比撰寫`ViewData["Message"]="text"`，您可以撰寫`ViewBag.Message="text"`。 您不需要定義要使用的任何強型別類別`ViewBag`屬性。 因為它是動態屬性，您可以改為直接取得或設定屬性，並將它們以動態方式在執行階段解析。 就內部而言，`ViewBag`屬性會儲存成名稱/值組中`ViewData`字典。 (注意： 在大部分的發行前版本的 MVC 3`ViewBag`屬性名為`ViewModel`屬性。)

### <a name="new-actionresult-types"></a>新的 「 ActionResult 」 型別

下列`ActionResult`型別和對應的 helper 方法是新的或增強 MVC 3 中：

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx)。 傳回用戶端 404 的 HTTP 狀態碼。
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx)。 傳回暫時重新導向 （HTTP 302 狀態碼） 或根據布林值參數的永久重新導向 （HTTP 301 狀態碼）。 這項變更，搭配[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx)類別現在具有三個方法來執行永久重新導向： `RedirectPermanent`， `RedirectToRoutePermanent`，和`RedirectToActionPermanent`。 這些方法會傳回的執行個體`RedirectResult`與`Permanent`屬性設定為`true`。
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx)。 傳回使用者指定的 HTTP 狀態碼。

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript 和 Ajax 增強功能

根據預設，MVC 3 中的 Ajax 和驗證 helper 會使用不顯眼的 JavaScript 方法。 不顯眼的 JavaScript 可避免將 HTML 插入，以便內嵌 JavaScript。 這會使您的 HTML 較小且小於雜亂，並且輕鬆地交換或自訂 JavaScript 程式庫。 驗證 helper 在 MVC 3 中的也使用`jQueryValidate`預設的外掛程式。 如果您想要 MVC 2 行為，您可以停用不顯眼的 JavaScript 使用*web.config*檔案設定。 如需 JavaScript 和 Ajax 改進的詳細資訊，請參閱下列資源：

- [維基百科站台上不顯眼的 JavaScript 的基本簡介](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson 的不顯眼的 JavaScript 文章](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson 的不顯眼的 JavaScript 驗證文章](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [使用 Razor 和不顯眼的 JavaScript 建立 MVC 3 應用程式](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md)（ASP.NET 網站上教學課程）
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>依預設啟用的用戶端驗證

在舊版的 MVC 中，您需要明確地呼叫`Html.EnableClientValidation`從檢視表，以啟用用戶端驗證方法。 MVC 3 中這是不再需要因為預設會啟用用戶端驗證。 (您可以停用此選項使用中的設定*web.config*檔案。)

為了讓用戶端驗證運作，您仍然需要參考適當的 jQuery 和您站台中的 jQuery 驗證程式庫。 您可以在您自己的伺服器上裝載這些文件庫或從像是向 Microsoft 或 Google Cdn 的內容傳遞網路 (CDN) 中參考它們。

### <a name="remote-validator"></a>遠端驗證程式

ASP.NET MVC 3 會支援新[RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx)類別，可讓您利用 jQuery 驗證外掛程式中的遠端驗證程式的支援。 這可讓用戶端驗證程式庫伺服器端自動呼叫您在伺服器定義才能執行只能進行的驗證邏輯的自訂方法。

在下列範例中，`Remote`屬性會指定用戶端驗證，會呼叫名為動作`UserNameAvailable`上`UsersController`類別以驗證`UserName`欄位。

[!code-csharp[Main](mvc3/samples/sample1.cs)]

下列範例顯示相對應的控制項。

[!code-csharp[Main](mvc3/samples/sample2.cs)]

如需有關如何使用`Remote`屬性，請參閱[How to： 在 ASP.NET MVC 中實作遠端驗證](https://msdn.microsoft.com/library/gg508808(VS.98).aspx)MSDN library 中。

### <a name="json-binding-support"></a>繫結的 JSON 支援

ASP.NET MVC 3 包含內建的 JSON 繫結支援，可讓動作方法接收 JSON 編碼的資料和模型繫結它至動作方法參數。 此功能會在用戶端範本和資料繫結案例中很有用。 （用戶端範本可讓您格式化及顯示資料的單一項目或一組資料的項目使用的用戶端執行的範本。）MVC 3 可讓您輕鬆地連接與伺服器上的動作方法傳送和接收 JSON 資料的用戶端範本。 如需繫結的 JSON 支援的詳細資訊，請參閱**JavaScript 和 AJAX 改進**區段[Scott Guthrie 的 MVC 3 Preview 部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>模型驗證增強功能

### <a name="dataannotations-metadata-attributes"></a>「 DataAnnotations"中繼資料屬性

ASP.NET MVC 3 支援`DataAnnotations`等中繼資料屬性`DisplayAttribute`。

### <a name="validationattribute-class"></a>「 ValidationAttribute 」 類別

`ValidationAttribute`類別已提升在.NET Framework 4，以支援新`IsValid`提供目前驗證內容，例如哪些物件要驗證的詳細資訊的多載。 這可讓您可以在此驗證根據模型的另一個屬性的目前值更豐富的案例。 例如，新`CompareAttribute`屬性可讓您比較兩個模型的屬性的值。 在下列範例中，`ComparePassword`屬性必須符合`Password`欄位才會生效。

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>驗證介面

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx)介面可讓您執行模型層級驗證，而且可讓您提供的驗證錯誤訊息所特有的整體模型中，或在模型中的兩個屬性之間的狀態. MVC 3 現在擷取錯誤`IValidatableObject`介面介面會在模型繫結，並會自動旗標或反白顯示受影響欄位內使用內建 HTML 表單 helper 的檢視。

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx)介面可讓 ASP.NET MVC，若要在執行階段探索驗證程式是否支援用戶端驗證。 這個介面設計，讓它可以與各種不同的驗證架構整合。

如需驗證介面的詳細資訊，請參閱**模型驗證改進**區段[Scott Guthrie 的 MVC 3 Preview 部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。 （不過請注意 「 IValidateObject"部落格中的參考應該是"IValidatableObject"）。

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>相依性插入增強功能

套用相依性插入 (DI) 和整合與相依性插入或的控制項反轉 (IOC) 容器，ASP.NET MVC 3 提供更佳的支援。 DI 支援已新增下列區域：

- 控制站 （註冊和插入控制器 factory，插入控制器）。
- 檢視 （註冊和插入檢視引擎，將插入檢視頁面的 相依性）。
- 動作篩選條件 （尋找和插入篩選）。
- 模型繫結器 （註冊和插入）。
- （登錄及插入），模型驗證提供者。
- （登錄及插入），模型中繼資料提供者。
- 值提供者 （註冊和插入）。

MVC 3 支援[通用服務定位程式](https://github.com/unitycontainer/commonservicelocator)程式庫和支援該媒體櫃的任何 DI 容器`IServiceLocator`介面。 它也支援新`IDependencyResolver`介面，可讓您更輕鬆地整合 DI 架構。

如需 DI MVC 3 中的詳細資訊，請參閱下列資源：

- [服務位置有關的 Brad Wilson 的一系列部落格文章](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>其他新功能

### <a name="nuget-integration"></a>NuGet 整合

ASP.NET MVC 3 自動安裝，並將其安裝程序可讓 NuGet。 NuGet 是免費的開放原始碼封裝管理員，可讓您輕鬆地尋找、 安裝及使用您在專案中的.NET 程式庫和工具。 它可搭配所有 Visual Studio 專案類型 （包括 ASP.NET Web Form 和 ASP.NET MVC）。

NuGet 可讓開發人員維護封裝其程式庫，然後在線上組件庫中註冊的開放原始碼專案 （例如，專案 Moq、 NHibernate、 Ninject、 StructureMap、 NUnit、 Windsor、 RhinoMocks 和 Elmah 等）。 然後很容易的.NET 開發人員想要使用其中一個程式庫來尋找套件，並將它安裝在他們正在處理的專案。

與 ASP.NET 3 Tools Update 中，專案範本包含 JavaScript 程式庫預先安裝的 NuGet 套件，因此它們會透過 NuGet 可更新。 Entity Framework Code First 也會預先安裝以 NuGet 套件。

如需 NuGet 的詳細資訊，請參閱 [NuGet 文件](https://docs.microsoft.com/nuget/) (英文)。

### <a name="partial-page-output-caching"></a>部分頁面輸出快取

ASP.NET MVC 有支援第 1 版後輸出快取的完整頁面回應。 MVC 3 也支援局部網頁輸出快取，可讓您輕鬆地快取區域或回應的片段。 如需有關快取的詳細資訊，請參閱**部分的頁面輸出快取**區段[Scott Guthrie 的部落格文章，在 MVC 3 發行候選版本](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)和**子動作輸出快取**區段[MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)。

### <a name="granular-control-over-request-validation"></a>更精確地控制要求驗證

ASP.NET MVC 有自動可協助防止 XSS 和 HTML 資料隱碼攻擊的內建的要求驗證。 不過，有時候您想要明確停用要求驗證，例如，如果您想要讓使用者張貼 HTML 內容 （例如，在部落格文章或 CMS 內容）。 您現在可以新增[AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx)屬性模型或檢視停用模型繫結期間的要求驗證每個屬性為基礎的模型。 如需有關要求驗證的詳細資訊，請參閱下列資源：

- **不顯眼的 JavaScript 和驗證**一節中[Scott Guthrie 的部落格文章，在 MVC 3 發行候選版本](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)。
- [MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>可延伸的 [新增專案] 對話方塊

在 ASP.NET MVC 3 中，您可以加入專案範本，檢視引擎，和單元測試專案的架構，**新專案** 對話方塊。

### <a name="template-scaffolding-improvements"></a>範本 Scaffolding 增強功能

ASP.NET MVC 3 scaffolding 範本工作表現較佳的識別模型上的主索引鍵屬性，並處理這些適當地比在舊版的 MVC。 （例如，scaffolding 範本現在確定主索引鍵不會建立結構做為可編輯的表單欄位。）

根據預設，建立與編輯 scaffold 現在使用`Html.EditorFor`協助程式，而不是`Html.TextBoxFor`協助程式。 這可改善資料的表單中的模型上的中繼資料支援附註屬性時**加入檢視**對話方塊就會產生檢視。

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>「 Html.LabelFor"和"Html.LabelForModel"的新多載

已新增新的方法多載`LabelFor`和`LabelForModel`helper 方法。 新的多載可讓您指定或覆寫的標籤文字。

### <a name="sessionless-controller-support"></a>無工作階段的控制站支援

在 ASP.NET MVC 3，您可以指出是否想要使用工作階段狀態的控制器類別，而且如果是的話，是否工作階段狀態應該是讀/寫或唯讀狀態。 如需 sessionless 控制器支援的詳細資訊，請參閱[MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)。

### <a name="new-additionalmetadataattribute-class"></a>新的 「 AdditionalMetadataAttribute 」 類別

您可以使用[AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx)屬性來填入`ModelMetadata.AdditionalValues`模型屬性的字典。 例如，如果檢視模型應該只會顯示系統管理員的屬性，您可以標註該屬性在下列範例所示：

[!code-csharp[Main](mvc3/samples/sample4.cs)]

產品檢視模型呈現時，才可以提供給任何顯示或編輯器範本此中繼資料。 這是由您解譯的中繼資料資訊。

### <a name="accountcontroller-improvements"></a>AccountController 增強功能

AccountController 網際網路專案範本中的已大幅改善。

### <a name="new-intranet-project-template"></a>新的內部網路專案範本

新的內部網路專案範本隨附的啟用 Windows 驗證，並移除 AccountController。
