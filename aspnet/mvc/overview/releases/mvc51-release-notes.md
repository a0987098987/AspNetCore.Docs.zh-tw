---
uid: mvc/overview/releases/mvc51-release-notes
title: 什麼是 ASP.NET MVC 5.1 的新功能 |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830581"
---
<a name="whats-new-in-aspnet-mvc-51"></a>什麼是 ASP.NET MVC 5.1 的新功能
====================
by [Microsoft](https://github.com/microsoft)

本主題會描述什麼是 ASP.NET Web MVC 5.1 的新功能。

- [軟體需求](#SoftwareRequirements)
- [下載](#download)
- [文件](#documentation)
- [ASP.NET mvc 5.1 的新功能](#new-features)

    - [屬性路由的增強功能](#AttributeRouting)
    - [編輯器範本的啟動程序支援](#Bootstrap)
    - [在檢視中的列舉支援](#Enum)
    - [低調驗證 MinLength/MaxLength 屬性](#Unobtrusive)
    - [支援不顯眼的 Ajax 中的 'this' 內容](#thisContext)
- [已知問題和重大變更](#KnownBreakingChanges)- [Bug 修正](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>軟體需求

- Visual Studio 2012： 下載[ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062)。
- Visual Studio 2013： 下載[Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064)。 此更新所需的編輯 ASP.NET MVC 5.1 Razor 檢視。

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 套件，NuGet gallery 上發行。 所有執行階段套件遵循[語意版本設定](http://semver.org/)規格。 最新的 ASP.NET MVC 5.1 RTM 套件有下列版本:"5.1.2"。 您可以安裝或更新這些套件，透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。 此版也包含在 NuGet 上的對應當地語系化的套件。

您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文件

教學課程和 ASP.NET MVC 5.1 RTM 的其他資訊都是從 ASP.NET 網站 ( https://www.asp.net)。 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>ASP.NET mvc 5.1 的新功能

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>屬性路由的增強功能

 屬性路由現在支援條件約束，啟用版本控制和標頭型路由選取。 現在已可透過自訂屬性路由的許多層面`IDirectRouteFactory`介面和`RouteFactoryAttribute`類別。 路由前置字元現在是透過可延伸`IRoutePrefix`介面和`RoutePrefixAttribute`類別。 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>在檢視中的列舉支援

1. 新`@Html.EnumDropDownListFor()`helper 方法。 這些應該用的運算式必須評估為注意 HTML 協助程式的最[enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx)類型或[Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)其中`T`是[enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx)型別。 使用`EnumHelper.IsValidForEnumHelper()`檢查這些需求。
2. 新`EnumHelper.GetSelectList()`方法會傳回`IList<SelectListItem>`。 當您需要操作之前呼叫，比方說，在選取清單時，這是很有用`@Html.DropDownListFor()`，或當您想要顯示名稱的`@Html.EnumDropDownListFor()`顯示。

下列程式碼示範這些 Api。

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

您可以看到完整的範例[此處](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/)。

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>編輯器範本的啟動程序支援

我們現在允許 HTML 屬性中傳入[EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx)作為[匿名物件](https://msdn.microsoft.com/en-us/library/bb397696.aspx)。

例如: 

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>低調驗證 MinLengthAttribute 和 MaxLengthAttribute

字串和陣列類型的用戶端驗證將現在支援的屬性裝飾[MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx)並[MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx)屬性。

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>支援不顯眼的 Ajax 中的 'this' 內容

回呼函式 (`OnBegin, OnComplete, OnFailure, OnSuccess`) 現在可以找出透過叫用的項目`this`內容。 例如: 

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

### <a name="attribute-routing"></a>屬性路由

屬性路由的相符項目中的模稜兩可時，現在將會回報錯誤，而不是選擇第一個相符項目。

屬性路由會嚴禁`{controller}`參數，以及從使用`{action}`路由參數放在動作上。 這些參數的用法很可能會導致模稜兩可。 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>到 5.1 封裝在中使用結果 5.0 封裝不存在專案中之專案的 scaffolding MVC/Web API

ASP.NET MVC 5.1 rtm 更新 NuGet 套件不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。 他們使用舊版 ASP.NET 執行階段套件 (version=5.0.0.0)。 如此一來，ASP.NET scaffolding 會安裝必要的套件，先前的版本 (version=5.0.0.0)，如果它們還不在您的專案中。 不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET 樣板不能覆寫您在專案中最新的套件。 如果您使用 ASP.NET scaffolding Web API 2.1 或 ASP.NET MVC 5.1 更新您的專案的封裝之後，請確定 Web API 和 ASP.NET MVC 的版本不一致。 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 中的 Razor 檢視的語法醒目提示

如果您沒有更新 Visual Studio 2013 更新至 ASP.NET MVC 5.1 RTM，您會取得 Visual Studio 編輯器支援語法反白顯示在編輯 Razor 檢視。 您必須更新以取得這項支援的 Visual Studio 2013。 

### <a name="type-renames"></a>型別重新命名

5.1 RTM 中重新命名一些用於屬性路由的擴充性的類型。

| **舊的型別名稱 (5.1 RC)** | **新的型別名稱 (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Bug 修正

此版本也包含數個 bug 修正。 您可以找到完整的清單：

- [5.1.0 套件](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 封裝包含 IntelliSense 的更新，但任何錯誤修正。
