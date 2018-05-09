---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: ASP.NET Web API OData 5.3 中最新消息 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>ASP.NET Web API OData 5.3 中最新消息
====================
由[Microsoft](https://github.com/microsoft)

本主題說明什麼是新的 ASP.NET Web API OData 5.3。

- [下載](#download)
- [文件 (英文)](#documentation)
- [OData 核心程式庫](#corelib)
- [新功能](#newf)
- [已知的問題和重大變更](#known-issues)
- [Bug 修正](#bug-fixes)
- [ASP.NET Web API OData 第 5.3.1 章](#OD)

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。 您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文件

您可以找到教學課程和其他相關文件在 ASP.NET Web API OData [ASP.NET 網站](../odata-support-in-aspnet-web-api/index.md)。

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData 核心程式庫

針對 OData v4、 Web API 現在使用 ODataLib 版本 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>新功能，在 ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>支援在 $ $levels 展開

您可以使用 $levels 查詢選項擴充查詢。 例如: 

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

這個查詢相當於：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>開啟實體類型的支援

*開啟輸入*stuctured 型別，其中包含動態內容，以及任何型別定義中宣告的屬性。 開放式類型可讓您加入的彈性資料模型。 如需詳細資訊，請參閱 xxxx。

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>在開放式類型的動態集合屬性的支援

之前，動態屬性必須是單一值。 在 5.3，動態屬性可以有集合值。 例如，在下列 JSON 承載，`Emails`屬性是動態屬性，而且屬於字串類型的集合：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>針對複雜型別繼承支援

現在複雜類型可以繼承自基底類型。 例如，OData 服務可以定義下列複雜類型：

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

以下是此範例中的 EDM:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

如需詳細資訊，請參閱[OData 複雜型別繼承範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

本章節描述的已知的問題和 ASP.NET Web API OData 5.3 中的突破性變更。

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>查詢選項

問題： 使用巢狀的 $展開含有 $levels = max 導致不正確的展開深度。

例如，假設下列要求：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

如果`MaxExpansionDepth`為 5，此查詢會產生 6 的展開深度。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Bug 修正和次要功能更新

此版本也包含數個 bug 修正和次要功能更新。 您可以找到完整的清單：

- [Bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 第 5.3.1 章

在此版本中我們組成[bug 修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All)某些 AllowedFunctions 列舉。 此版本沒有任何其他 bug 修正或新的功能。
