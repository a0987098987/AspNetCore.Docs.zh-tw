---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "內含項目中 OData v4 使用 Web 應用程式開發介面 2.2 |Microsoft 文件"
author: rick-anderson
description: "傳統上，如果它封裝內的實體集，可能只存取實體。 但是，OData v4 提供另外兩個選項，單一和 Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>內含項目中 OData v4 使用 Web 應用程式開發介面 2.2
====================
由 Jinfu Tan

> 傳統上，如果它封裝內的實體集，可能只存取實體。 但是 OData v4 提供另外兩個選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。


本主題說明如何在 WebApi 2.2 中的 OData 端點中定義的內含項目。 如需內含項目，請參閱[內含項目來自與 OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)。 若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。

首先，我們將建立內含項目網域模型中的 OData 服務，請使用此資料模型：

![資料模型](odata-containment-in-web-api-22/_static/image1.png)

帳戶包含許多 PaymentInstruments (PI)，但我們不會定義實體集 PI。 相反地，Pi 只能透過帳戶存取。

您可以下載從本主題中所用之解決方案[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)。

## <a name="defining-the-data-model"></a>定義資料模型

1. 定義 CLR 類型。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained`屬性用於內含項目導覽屬性。
2. 產生的 CLR 型別為基礎的 EDM 模型。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder`建置的 EDM 模型，如果將處理`Contained`屬性加入至對應的導覽屬性。 如果屬性是集合型別，`GetCount(string NameContains)`也會建立函式。

    產生的中繼資料將會如下所示：

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget`屬性會指出的導覽屬性是否將內含項目。

## <a name="define-the-containing-entity-set-controller"></a>定義包含實體集控制器

包含的實體沒有自己的控制器。動作被定義中包含的實體集控制器。 在此範例中，沒有 AccountsController，但沒有 PaymentInstrumentsController。

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

如果 OData 路徑區段 4 或更多，僅屬性路由的運作方式，例如`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上述控制器中。 否則，屬性和傳統路由的運作方式： 例如，`GetPayInPIs(int key)`符合`GET ~/Accounts(1)/PayinPIs`。

*由於原始內容的這篇文章 Leo Hu。*
