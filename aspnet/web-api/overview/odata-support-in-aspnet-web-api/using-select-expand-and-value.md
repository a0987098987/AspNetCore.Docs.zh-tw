---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: 使用 $select，$expand、 與 ASP.NET Web API 2 OData 中的 $value |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>使用 $select，$expand、 與 ASP.NET Web API 2 OData 中的 $value
====================
由[Mike Wasson](https://github.com/MikeWasson)

Web API 2 新增的支援 $ expand $select 和 OData $value 選項。 這些選項可讓用戶端來控制從伺服器取回的表示法。

- **$expand**造成要在回應中的包含的內嵌的相關的實體。
- **$select**選取要包含在回應中的屬性子集。
- **$value**取得屬性的原始值。

## <a name="example-schema"></a>範例結構描述

本文中，我將使用的 OData 服務，會定義三個實體： 產品、 供應商和類別目錄。 每個產品具有一個類別目錄和一個供應商。

![](using-select-expand-and-value/_static/image1.png)

以下是 C# 類別，定義實體模型：

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

請注意，`Product`類別定義導覽屬性`Supplier`和`Category`。 `Category`類別會定義每個類別中之產品的導覽屬性。

若要建立此結構描述的 OData 端點，使用中所述的 Visual Studio 2013 scaffolding，[建立 ASP.NET Web API 中的 OData 端點](odata-v3/creating-an-odata-endpoint.md)。 新增產品、 類別和供應商的個別控制站。

## <a name="enabling-expand-and-select"></a>啟用 $展開和 $select

在 Visual Studio 2013 中，Web API OData scaffolding 該自動支援 $expand 和 $select 建立控制器。 如需參考，以下是支援 $ 需求展開和 $select 控制器中的。

對於集合，控制站的`Get`方法必須傳回**IQueryable**。

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

對於單一實體，會傳回**SingleResult&lt;T&gt;**，其中 T 是**IQueryable** ，其中包含零個或一個實體。

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

此外，裝飾您`Get`方法 **[Queryable]** 屬性，如先前的程式碼片段所示。 或者，呼叫**EnableQuerySupport**上**HttpConfiguration**在啟動時的物件。 (如需詳細資訊，請參閱[啟用 OData 查詢選項](supporting-odata-query-options.md#enable)。)

## <a name="using-expand"></a>使用 $展開

當您查詢 OData 實體或集合時，預設回應不包含相關的實體。 例如，以下是分類實體集的預設回應：

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

如您所見，回應不包含任何產品，即使類別目錄 」 實體包含產品的導覽連結。 不過，用戶端可以使用 $展開來取得每個類別目錄的產品清單。 $Expand 選項則是放在要求的查詢字串：

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

現在伺服器將會包含每個類別目錄，而這些分類則內嵌的產品。 以下是回應裝載：

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

請注意每個項目"value"陣列中的包含產品清單。

$ Expand 選項會採用以逗號分隔清單的導覽屬性，以展開。 下列要求會展開類別目錄和產品的供應商。

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

以下是回應主體：

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

您可以展開瀏覽屬性的多個層級。 下列範例包含的類別目錄的所有產品以及每項產品的供應商。

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

以下是回應主體：

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

根據預設，Web 應用程式開發介面會限制為 2 的最大展開深度。 可避免用戶端傳送複雜的要求，例如`$expand=Orders/OrderDetails/Product/Supplier/Region`，這可能是沒有效率的查詢，並建立大型的回應。 若要覆寫預設值，設定**MaxExpansionDepth**屬性 **[Queryable]** 屬性。

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

如需有關 $expand 選項，請參閱 < [Expand 系統查詢選項 ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand)官方 OData 文件中。

## <a name="using-select"></a>使用 $select

$Select 選項指定要包含在回應主體中的屬性子集。 例如，若要取得只有名稱和每個產品的價格，使用下列查詢：

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

以下是回應主體：

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

您可以結合 $select 和 $expand 中相同的查詢。 請確定已展開的屬性納入 $select 選項。 例如，下列要求會取得供應商與產品名稱。

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

以下是回應主體：

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

您也可以選取中的已展開屬性的屬性。 下列要求會展開產品，並選取類別目錄名稱加上產品名稱。

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

以下是回應主體：

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

如需 $select 選項的詳細資訊，請參閱[選取系統查詢選項 ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select)官方 OData 文件中。

## <a name="getting-individual-properties-of-an-entity-value"></a>取得實體 ($value) 的個別屬性

有兩個實體從取得的個別屬性的 OData 用戶端的方式。 用戶端可以取得 OData 格式的值，或取得屬性的原始值。

下列要求會取得 OData 格式的屬性。

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

以下是範例回應 JSON 格式：

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

若要取得屬性的原始值，請將 $value 附加至的 URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

以下是回應。 請注意，內容類型"text/plain"不是 JSON。

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

若要支援這些查詢在 OData 控制器中，新增名為`GetProperty`，其中`Property`是屬性的名稱。 例如，要取得的名稱屬性的方法就會命名為`GetName`。 這個方法應傳回該屬性的值：

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
