---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "使用 ASP.NET Web API OData v4 中的複雜型別繼承 |Microsoft 文件"
author: microsoft
description: "根據 OData v4 規格中，複雜類型可以繼承自另一個複雜類型。 （複雜型別是結構化的型別沒有索引鍵）。Web API..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>使用 ASP.NET Web API OData v4 中的複雜型別繼承
====================
由[Microsoft](https://github.com/microsoft)

> 根據 OData v4[規格](http://www.odata.org/documentation/odata-version-4-0/)，複雜類型可以繼承自另一個複雜類型。 (A*複雜*類型是結構化型別沒有索引鍵。)Web API OData 5.3 支援複雜類型的繼承。
> 
> 本主題示範如何建置繼承複雜類型的實體資料模型 (EDM)。 完整的原始程式碼，請參閱[OData 複雜型別繼承範例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>模型階層

為了說明複雜型別繼承，我們將使用下列類別階層架構。

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`是抽象的複雜類型。 `Rectangle``Triangle`，和`Circle`複雜型別衍生自`Shape`，和`RoundRectangle`衍生自`Rectangle`。 `Window`是實體類型，包含`Shape`執行個體。

以下是定義這些類型的 CLR 類別。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>建置的 EDM 模型

若要建立 EDM，您可以使用**ODataConventionModelBuilder**，這會推斷從 CLR 類型的繼承關聯性。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

您也可以建置 EDM 明確地使用**ODataModelBuilder**。 這會採用更多的程式碼，但讓您更充分掌控 EDM。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

這兩個範例會建立相同的 EDM 結構描述。

## <a name="metadata-document"></a>中繼資料文件

以下是 OData 中繼資料文件，顯示複雜型別繼承。

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

中繼資料文件中，您可以看到：

- `Shape`複雜類型是抽象的。
- `Rectangle`， `Triangle`，和`Circle`複雜類型具有基底型別`Shape`。
- `RoundRectangle`類型具有基底類型`Rectangle`。

## <a name="casting-complex-types"></a>將複雜型別轉型

現在支援複雜類型上進行轉換。 例如，下列查詢會轉換`Shape`至`Rectangle`。

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

以下是回應裝載：

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
