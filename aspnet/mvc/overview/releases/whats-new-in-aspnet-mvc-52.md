---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 中最新消息 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504097"
---
<a name="whats-new-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 中最新消息
====================
由[Microsoft](https://github.com/microsoft)

本主題說明什麼是新的 ASP.NET MVC 5.2， [Microsoft.AspNet.MVC 5.2.2 章](#52)和[ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [軟體需求](#softRequire)
- [下載](#download)
- [文件 (英文)](#documentation)
- [ASP.NET MVC 5.2 中的新功能](#new-features)

    - [屬性路由的增強功能](#attributerouting)
- [已知的問題和重大變更](#knownbreakingchanges)
- [Bug 修正](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2 章](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>軟體需求

- 下載 visual Studio 2012: [ASP.NET 及 Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)。
- 下載 visual Studio 2013: [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)或更高版本。 編輯 ASP.NET MVC 5.2 Razor 檢視需要此更新。

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。 所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。 最新的 ASP.NET MVC 5.2 封裝有下列版本:"5.2.0"。 您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。 此版也包含對應當地語系化的 NuGet 套件。

您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：

安裝套件 Microsoft.AspNet.Mvc-5.2.0 版

<a id="documentation"></a>
## <a name="documentation"></a>文件

教學課程和其他關於 ASP.NET MVC 5.2 資訊都是從 ASP.NET 網站 ([https://www.asp.net/mvc](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 中的新功能

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>屬性路由的增強功能

屬性現在路由提供稱為 IDirectRouteProvider，可讓您探索及設定屬性路由的方式的完整控制權的擴充點。 IDirectRouteProvider 負責提供動作與控制器以及相關聯的路由資訊來指定完全何種路由組態所需的那些動作的清單。 呼叫 MapAttributes/MapHttpAttributeRoutes 時，可能指定 IDirectRouteProvider 實作。

自訂 IDirectRouteProvider 會最簡單方法是將我們的預設實作、 DefaultDirectRouteProvider 延伸。 這個類別會提供個別可覆寫的虛擬方法，若要變更探索屬性、 建立路由項目，以及探索路由前置字元和區域前置詞的邏輯。

使用新屬性路由擴充功能的 IDirectRouteProvider，使用者可以執行下列作業：

1. 支援的屬性路由的繼承。 例如，在下列案例中部落格和存放區的控制站使用 BaseController 所定義的屬性路由慣例。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. 自動產生屬性路由的路由名稱。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. 修改路由前置詞在單一中央位置，才能路由加入至路由表。
4. 篩選掉您想要尋找的屬性路由的控制器。 我們希望 3 和 4 上的部落格推出。

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook 修正已變更的 API 介面

MVC Facebook 封裝[中斷](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)因為 Facebook 所做的一些應用程式開發介面變更。 我們也將推出新的 Facebook 封裝以修正這些問題的 (Microsoft.AspNet.Facebook 1.0.0)。

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>在專案中使用 5.2.0 到 scaffolding MVC/Web API 找出已不存在於專案中封裝的封裝導致 5.1.2

ASP.NET mvc 5.2.0 更新 NuGet 封裝不會更新的 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。 他們使用的舊版 ASP.NET 執行階段封裝 (例如 5.1.2 在 Update 2)。 如此一來，ASP.NET scaffolding 會安裝舊版 (例如 5.1.2 在 Update 2) 必要的封裝，如果還沒有出現在您的專案。 不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。 如果您使用 ASP.NET scaffolding Web API 2.2 或 ASP.NET MVC 5.2 更新專案中的封裝之後，請確定一致的 Web API 和 ASP.NET MVC 版本。

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Microsoft.jQuery.Unobtrusive.Validation NuGet 套件安裝失敗，因為它是找不到 Microsoft.jQuery.Unobtrusive.Validation 的版本相容，jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation 需要 jQuery &gt;= 1.8 和 jQuery.Validation &gt;= 1.8。 But,jQuery.Validation (1.8) 需要 jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6)。 因為這個緣故，NuGet 會 JQuery 1.8 和 jQuery.Validation 1.8 安裝在相同的時間，它就會失敗。 當您看到這個問題時，您僅更新至 jQuery.Validation 版本&gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)具有固定的 jQuery cap 首先，您應該能夠安裝Microsoft.jQuery.Unobtrusive.Validation。

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery。驗證 nuget 封裝版本 1.13.0 無法辨識某些國際電子郵件地址

jQuery.Validation nuget 封裝版本 1.11.1 是最後已知的版本可辨識下列有效的電子郵件地址。 任何更新的版本可能無法辨識它們。 例如: 

電子郵件地址國際化 (EAI) 標準 (例如[&#29992; &#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))   
 EAI + 國際化資源識別元 （光圈） (例如。， [（& s) #29992; &#25143; @ （& s) #1076; （& s) #1086; （& s) #1084; （& s) #1077; （& s) #1085;。&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

在報告的問題[https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 中的 Razor 檢視的反白顯示語法

如果您將更新 ASP.NET MVC 5.2，但未更新 Visual Studio 2013，您會取得 Visual Studio 編輯器支援語法反白顯示在編輯 Razor 檢視。 您必須更新 Visual Studio 2013，若要取得這項支援。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Bug 修正和次要功能更新

此版本也包含數個 bug 修正和次要功能更新。 您可以找到完整的清單：

- [5.2 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2 章

在 MVC 中沒有任何新功能或 bug 修正這一版。 我們做了[網頁中變更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)顯著的效能改進及後續更新我們擁有相依於這個新版本的 Web 網頁的所有其他相依套件。

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

您可以閱讀發行[這裡](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。 此版本包含只 bug 修正。 您可以使用[此查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。
