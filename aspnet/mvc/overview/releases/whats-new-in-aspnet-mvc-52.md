---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: 什麼是 ASP.NET MVC 5.2 的新功能 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 76e1c13453c45a1b8ca71dd6435dcbfbd8bec66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801537"
---
<a name="whats-new-in-aspnet-mvc-52"></a>什麼是 ASP.NET MVC 5.2 的新功能
====================
by [Microsoft](https://github.com/microsoft)

本主題描述適用於 ASP.NET MVC 5.2，最新消息[Microsoft.AspNet.MVC 5.2.2](#52)和[ASP.NET MVC 5.2.3 beta 版](#mvc523Beta)

- [軟體需求](#softRequire)
- [下載](#download)
- [文件](#documentation)
- [ASP.NET mvc 5.2 的新功能](#new-features)

    - [屬性路由的增強功能](#attributerouting)
- [已知的問題和重大變更](#knownbreakingchanges)
- [Bug 修正](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>軟體需求

- Visual Studio 2012： 下載[ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062)。
- Visual Studio 2013： 下載[Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)或更高版本。 此更新所需的編輯 ASP.NET MVC 5.2 Razor 檢視。

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 套件，NuGet gallery 上發行。 所有執行階段套件遵循[語意版本設定](http://semver.org/)規格。 最新的 ASP.NET MVC 5.2 套件有下列版本:"5.2.0 」。 您可以安裝或更新這些套件，透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。 此版也包含在 NuGet 上的對應當地語系化的套件。

您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：

安裝套件 Microsoft.AspNet.Mvc-5.2.0 版

<a id="documentation"></a>
## <a name="documentation"></a>文件

教學課程和 ASP.NET MVC 5.2 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/mvc](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>ASP.NET mvc 5.2 的新功能

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>屬性路由的增強功能

屬性路由現在提供稱為 IDirectRouteProvider，可讓您完整控制如何屬性路由會探索及設定的擴充點。 IDirectRouteProvider 負責提供一份動作與控制器相關聯的路由資訊，來指定這些動作只需要何種路由的設定。 呼叫 MapAttributes/MapHttpAttributeRoutes 時，可能會指定 IDirectRouteProvider 實作。

自訂 IDirectRouteProvider 會最簡單方法是將我們的預設實作，DefaultDirectRouteProvider 延伸。 這個類別會提供不同可覆寫的虛擬方法，以變更邏輯探索屬性、 建立路由項目，以及探索路由前置詞和區域前置詞。

使用新屬性路由的擴充性 IDirectRouteProvider，使用者可以執行下列作業：

1. 支援的屬性路由的繼承。 例如，在下列案例中的部落格和存放區的控制站使用 BaseController 所定義的屬性路由慣例。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. 自動產生屬性路由的路由的名稱。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. 路由加入至路由表之前，請修改路由前置詞，在一個集中位置。
4. 篩選出您想要尋找的屬性路由的控制器。 我們很快就希望部落格上 3 和 4。

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook 的修正程式已變更的 API 介面

MVC Facebook 封裝[已中斷](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)因為 Facebook 所做的一些 API 變更。 我們也將推出新的 Facebook 封裝來修正這些問題的 (Microsoft.AspNet.Facebook 1.0.0)。

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Scaffolding MVC/Web API，到使用 5.2.0 專案不存在專案中的封裝的封裝導致 5.1.2

ASP.NET mvc 5.2.0 更新 NuGet 套件不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。 他們使用舊版 ASP.NET 執行階段套件 (例如在 Update 2 的 5.1.2)。 如此一來，ASP.NET scaffolding 會安裝必要的套件，先前的版本 (例如在 Update 2 的 5.1.2)，如果它們還不在您的專案中。 不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET 樣板不能覆寫您在專案中最新的套件。 如果您使用 ASP.NET scaffolding Web API 2.2 或 ASP.NET MVC 5.2 更新您的專案的封裝之後，請確定 Web API 和 ASP.NET MVC 的版本不一致。

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Microsoft.jQuery.Unobtrusive.Validation NuGet 套件安裝失敗，因為它是找不到 Microsoft.jQuery.Unobtrusive.Validation 的版本相容，jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation 需要 jQuery &gt;= 1.8 和 jQuery.Validation &gt;= 1.8。 But,jQuery.Validation (1.8) 需要 jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6)。 因為這個緣故，如果 NuGet 所安裝的 JQuery 1.8 和 jQuery.Validation 1.8 在此同時，就會失敗。 當您看到此問題時，則只要更新至 jQuery.Validation 新版&gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)具有固定的 jQuery 上限前，您應該能夠安裝Microsoft.jQuery.Unobtrusive.Validation。

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery。驗證 nuget 套件版本 1.13.0 無法辨識某些國際電子郵件地址

jQuery.Validation nuget 套件版本 1.11.1 是最後一個已知的版本可辨識下列有效的電子郵件地址。 任何更新的版本可能無法辨識它們。 例如: 

電子郵件地址國際化 (EAI) 標準 (例如[ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + 國際化資源識別項 (Iri) (例如。， [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;。&#1088;&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

在報告的問題 [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 中的 Razor 檢視的語法醒目提示

如果您將更新 ASP.NET MVC 5.2，而不需要更新 Visual Studio 2013，您會取得 Visual Studio 編輯器支援語法反白顯示在編輯 Razor 檢視。 您必須更新以取得這項支援的 Visual Studio 2013。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Bug 修正和次要功能更新

此版本也包含數個 bug 修正和次要功能更新。 您可以找到完整的清單：

- [5.2 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

在 MVC 中沒有任何新功能或 bug 修正這一版。 我們所做[網頁中變更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)顯著的效能改進，隨後更新現存相依於這個新版網頁的所有其他相依套件。

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 beta 版

您可以閱讀發行[此處](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。 此版本包含只 bug 修正。 您可以使用[這項查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。
