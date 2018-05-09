---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: 新 ASP.NET Web API 2.2 功能 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 中最新消息
====================
由[Microsoft](https://github.com/microsoft)

本主題說明 ASP.NET Web API 2.2 的新功能。

- [下載](#download)
- [文件 (英文)](#documentation)
- [ASP.NET Web API 2.2 的新功能](#newf)

    - [OData v4](#OData)
    - [屬性路由的增強功能](#ARI)
    - [適用於 Windows Phone 8.1 的 web API 的用戶端支援](#phone)
- [已知的問題和重大變更](#known-issues)
- [Bug 修正](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1 章](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2 章](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。 所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。 最新的 ASP.NET Web API 2.2 封裝有下列版本:"5.2.0"。 您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。 此版也包含對應當地語系化的 NuGet 套件。

您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文件

教學課程和 ASP.NET Web API 2.2 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/web-api](../../index.md))。

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 的新功能

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

此版本中加入 OData v4 通訊協定的支援。 如需詳細資訊，請參閱[Web API OData v4 文件。](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

以下是一些主要功能與 OData v4 的變更：

- [OData 模型中的別名屬性的支援](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [ComplexTypeAttribute、 AssociationAttribute、 TimesTampAttribute 和 ConcurrencyCheckAttribute ODataConventionModelBuilder 中支援](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [提供功能，提供易記的標題的動作](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- ODL UriParser 與整合
- 支援[列舉](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/)，[內含項目](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)和[單一](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- 支援的基本型別轉換
- [已新增的 OData 函數支援](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [函式呼叫的支援參數別名](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [在模型中支援 camel 命名法的大小寫命名慣例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- 支援在 $filter cast （）
- 開啟的複雜類型的支援
- 移除的 EntitySetController 和 AsyncEntitySetController
- [變更至 $ref $link](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [已新增的屬性路由支援](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- 使用 OData 核心程式庫 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>屬性路由的增強功能

屬性現在路由提供稱為 IDirectRouteProvider，可讓您探索及設定屬性路由的方式的完整控制權的擴充點。 IDirectRouteProvider 負責提供動作與控制器以及相關聯的路由資訊來指定完全何種路由組態所需的那些動作的清單。 呼叫 MapAttributes/MapHttpAttributeRoutes 時，可能指定 IDirectRouteProvider 實作。

自訂 IDirectRouteProvider 會最簡單方法是將我們的預設實作、 DefaultDirectRouteProvider 延伸。 這個類別會提供個別可覆寫的虛擬方法，若要變更探索屬性、 建立路由項目，以及探索路由前置字元和區域前置詞的邏輯。

以下是有關您可以執行與此新的擴充性點的部分範例：

1. 支援路由屬性的繼承

    範例：

    這裡 like"/ api/值 10"的要求會成功傳回"成功： 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. 提供屬性路由的預設路由名稱遵循您喜歡的一些慣例。 根據預設，屬性路由不會自動建立屬性路由的名稱。
3. 它們在路由表結束之前，請修改在同一個地方屬性路由的路由範本。

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>適用於 Windows Phone 8.1 的 web API 的用戶端支援

您現在可以使用 Web API 的用戶端 NuGet 封裝來實作您的 Web API 用戶端邏輯，以 Windows Phone 8.1 為目標時，或從通用應用程式中。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

本章節描述的已知的問題和 ASP.NET Web API 2.2 中的突破性變更。

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>模型產生器

問題： 多載函式可能不會公開為 FunctionImport

如果有 2 多載函式，它們也是 FunctionImport 然後要求在 System.InvalidOperationException ~/GetAllConventionCustomers(CustomerName={customerName}) 結果如下所示。

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

因應措施： 此問題的因應措施是將這兩個函式多載為 FunctionImports。

#### <a name="odata-routing"></a>OData 路由

字串常值包含 URL 編碼斜線 (%2f)，而 backslash(%5C) 用於 OData 資源路徑時，會導致 404 錯誤。

例如，字串常值可以當做 OData 資源路徑中的函式的參數或實體集的索引鍵值。

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

當服務收到這類要求主機將會取消逸出這些逸出序列，然後將它們傳遞至 Web API 執行階段。 這樣可防止攻擊，如下所示：  
  
 http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:

這會導致 Web API OData 堆疊傳回 404 錯誤 （找不到）。 若要避免這個錯誤，您的用戶端應該使用雙重逸出序列的斜線 （%252f) 和反斜線 （%255 C)。 這不會發生的查詢字串，例如 /Employees？ $filter = Name eq '名稱 %2f'

**請注意未逸出的斜線 （'/') 和反斜線 （"） 是不合法的 OData 資源路徑字串常值。斜線應只做為路徑分隔符號，反斜線不應該完全出現在 OData 資源路徑。（兩者都可用於 OData 查詢字串的某些部分。）**

因應措施： 您無法覆寫來逸出斜線和字串常值反斜線，實際上會剖析它們之前 DefaultODataPathHandler 的剖析方法。 您可以找到這種方法的範例。

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[查詢]

[Queryable] 屬性已被取代。 新的 OData v3 應用程式應該使用**System.Web.Http.OData.EnableQueryAttribute**。

**ODataHttpConfigurationExtensions.EnableQuerySupport**擴充方法現在將**EnableQueryAttribute**至全域篩選集合。 如果有任何控制站 **[Queryable]** 屬性，呼叫`config.EnableQuerySupport()`會導致 **[Queryable]** 屬性失敗

若要解決此問題的建議的方式是要取代的所有執行個體**根據 QueryableAttribute**與**System.Web.Http.OData.EnableQueryAttribute**。

一種解決方法是在您的 Web API 設定中使用下列程式碼：

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>路由屬性

問題： 使用 FromUri 屬性裝飾的複雜類型的模型繫結時的行為以不同的方式使用屬性的路由。

下列連結會追蹤問題，而且也有因應措施的相關詳細資料。  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

問題： Scaffolding MVC/Web API，在專案中使用 5.2.0 到找出已不存在於專案中封裝的封裝導致 5.1.2

ASP.NET MVC 5.2 更新 NuGet 封裝不會更新的 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。 他們使用的舊版 ASP.NET 執行階段封裝 (例如 5.1.2 在 Update 2)。 如此一來，ASP.NET scaffolding 會安裝舊版 (例如 5.1.2 在 Update 2) 必要的封裝，如果還沒有出現在您的專案。 不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。 如果您使用 ASP.NET scaffolding Web API 2.2 或 ASP.NET MVC 5.2 更新專案中的封裝之後，請確定一致的 Web API 和 ASP.NET MVC 版本。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Bug 修正和次要功能更新

此版本也包含數個 bug 修正和次要功能更新。 您可以找到完整的清單：

- [5.2 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1 章

Microsoft.AspNet.OData 5.2.1 章封裝包含 NuGet 相依性更新，但沒有 bug 修正。 透過這項更新，但是再也不嚴格的相依性上 Microsoft.OData.Core 6.4.0，但其中一個可以升級到 6.4.0 和 7.0.0 之間的任何版本。

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2 章

在此版本中我們所做變更的相依性`Json.Net 6.0.4`。 如需有關在此版本中的最新消息`Json.NET`，請參閱[Json.NET 6.0 版本 4-JSON 合併，相依性插入](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection)。 在 Web 應用程式開發介面中沒有任何其他新功能或 bug 修正這一版。 接下來，我們已更新我們擁有相依於這個新版本的 Web API 的所有其他相依套件。

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

您可以閱讀發行[這裡](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。 此版本包含只 bug 修正。 您可以使用[此查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。
