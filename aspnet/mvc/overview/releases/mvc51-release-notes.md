---
uid: mvc/overview/releases/mvc51-release-notes
title: ASP.NET MVC 5.1 中最新消息 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 中最新消息
====================
by [Microsoft](https://github.com/microsoft)

本主題說明 ASP.NET Web MVC 5.1 的新功能。

- [軟體需求](#SoftwareRequirements)
- [下載](#download)
- [文件 (英文)](#documentation)
- [ASP.NET MVC 5.1 的新功能](#new-features)

    - [屬性路由的增強功能](#AttributeRouting)
    - [啟動程序支援編輯器範本](#Bootstrap)
    - [列舉檢視中的支援](#Enum)
    - [不顯眼的 MinLength/MaxLength 屬性驗證](#Unobtrusive)
    - [支援不顯眼的 Ajax 中的 'this' 內容](#thisContext)
- [已知問題和重大變更](#KnownBreakingChanges)- [Bug 修正](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>軟體需求

- 下載 visual Studio 2012: [ASP.NET 及 Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)。
- 下載 visual Studio 2013: [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064)。 編輯 ASP.NET MVC 5.1 Razor 檢視需要此更新。

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。 所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。 最新的 ASP.NET MVC 5.1 RTM 封裝有下列版本:"5.1.2"。 您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。 此版也包含對應當地語系化的 NuGet 套件。

您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文件

教學課程和 ASP.NET MVC 5.1 RTM 的其他資訊都是從 ASP.NET 網站 ( https://www.asp.net)。 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>ASP.NET MVC 5.1 的新功能

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>屬性路由的增強功能

 路由現在支援條件約束，啟用版本控制和標頭的屬性型路由選取。 現在可透過自訂屬性路由的許多層面是`IDirectRouteFactory`介面和`RouteFactoryAttribute`類別。 路由前置字元現在是可透過延伸`IRoutePrefix`介面和`RoutePrefixAttribute`類別。 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>列舉檢視中的支援

1. 新`@Html.EnumDropDownListFor()`helper 方法。 這些應該用像大部分的 HTML helper，但請注意，運算式必須評估為[列舉](https://msdn.microsoft.com/en-us/library/cc138362.aspx)類型或[Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)其中`T`是[列舉](https://msdn.microsoft.com/en-us/library/cc138362.aspx)型別。 使用`EnumHelper.IsValidForEnumHelper()`來檢查這些需求。
2. 新`EnumHelper.GetSelectList()`方法會傳回`IList<SelectListItem>`。 當您需要管理選取清單之前呼叫，例如，這非常有用`@Html.DropDownListFor()`，或當您想要顯示的名稱這`@Html.EnumDropDownListFor()`顯示。

下列程式碼會示範這些 Api。

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

您可以看到完整的範例[這裡](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/)。

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>啟動程序支援編輯器範本

我們現在允許 HTML 屬性中傳遞[EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx)為[匿名物件](https://msdn.microsoft.com/en-us/library/bb397696.aspx)。

例如: 

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>MinLengthAttribute 和 MaxLengthAttribute 不顯眼的驗證

字串和陣列類型的用戶端驗證將現在支援的屬性以裝飾[MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx)和[MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx)屬性。

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>支援不顯眼的 Ajax 中的 'this' 內容

回呼函式 (`OnBegin, OnComplete, OnFailure, OnSuccess`) 現在可以找出透過叫用的項目`this`內容。 例如: 

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

### <a name="attribute-routing"></a>路由屬性

發生錯誤，而不是選擇第一個相符項目，現在將會報告屬性路由相符項目中的模稜兩可。

屬性路由嚴禁`{controller}`參數，並從使用`{action}`路由參數放在 [動作]。 這些參數的用法很可能會導致模稜兩可。 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Scaffolding MVC/Web 應用程式開發介面到 5.1 封裝在中使用結果 5.0 封裝還不存在專案中的專案

ASP.NET MVC 5.1 rtm 更新 NuGet 封裝不會更新的 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。 他們使用的舊版 ASP.NET 執行階段封裝 (5.0.0.0)。 如此一來，ASP.NET scaffolding 會安裝必要的封裝前, 一版 (5.0.0.0)，如果還沒有出現在您的專案。 不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。 如果您使用 ASP.NET scaffolding Web API 2.1 或 ASP.NET MVC 5.1 更新專案中的封裝之後，請確定一致的 Web API 和 ASP.NET MVC 版本。 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 中的 Razor 檢視的反白顯示語法

如果您未更新 Visual Studio 2013 更新至 ASP.NET MVC 5.1 RTM，您才會得到 Visual Studio 編輯器支援語法反白顯示在編輯 Razor 檢視。 您必須更新 Visual Studio 2013，若要取得這項支援。 

### <a name="type-renames"></a>型別重新命名

某些屬性的路由擴充性所使用的型別會重新命名 5.1 RTM 的侷限。

| **舊的型別名稱 (5.1 RC)** | **新的類型名稱 (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Bug 修正

此版本也包含數個 bug 修正。 您可以找到完整的清單：

- [5.1.0 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 封裝包含 IntelliSense 的更新，但沒有 bug 修正。
