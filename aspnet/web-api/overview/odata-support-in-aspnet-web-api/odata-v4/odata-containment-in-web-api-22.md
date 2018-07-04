---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: 內含項目在 OData v4 中的使用 Web API 2.2 |Microsoft Docs
author: rick-anderson
description: 傳統上，如果它封裝在實體集，可能只能存取實體。 但 OData v4 提供兩個額外的選項，單一和 Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 33ff49f69d70dd3a8179445d2895c418d2185e49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365603"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>內含項目在 OData v4 中使用 Web API 2.2
====================
由 Jinfu Tan

> 傳統上，如果它封裝在實體集，可能只能存取實體。 但 OData v4 提供兩個額外的選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。


本主題說明如何在 WebApi 2.2 中的 OData 端點中定義將內含項目。 如需有關內含項目的詳細資訊，請參閱[內含項目即將與 OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)。 若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。

首先，我們會建立內含項目領域模型中的 OData 服務，使用此資料模型：

![資料模型](odata-containment-in-web-api-22/_static/image1.png)

帳戶包含許多 PaymentInstruments (PI)，但我們未定義的實體集的 PI。 相反地，Pi 只能透過帳戶存取。

您可以下載從本主題中所使用的解決方案[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)。

## <a name="defining-the-data-model"></a>定義資料模型

1. 定義的 CLR 型別。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained`屬性適用於內含項目導覽屬性。
2. 產生的 CLR 型別為基礎的 EDM 模型。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder`建置的 EDM 模型，如果處理`Contained`屬性新增至對應的導覽屬性。 如果屬性是集合型別，`GetCount(string NameContains)`也會建立函式。

    產生的中繼資料將會如下所示：

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget`屬性表示的導覽屬性會將內含項目。

## <a name="define-the-containing-entity-set-controller"></a>定義包含實體集控制器

沒有自己的控制器; 包含的實體。動作是此控制器中定義包含實體集。 在此範例中，沒有 AccountsController，但沒有 PaymentInstrumentsController。

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

如果 4 個或多個區段的 OData 路徑，屬性路由的運作方式，例如`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上述控制器中。 否則，屬性與慣例路由的運作方式： 例如，`GetPayInPIs(int key)`符合`GET ~/Accounts(1)/PayinPIs`。

*感謝您來代表 Leo Hu 這篇文章的原始內容。*
