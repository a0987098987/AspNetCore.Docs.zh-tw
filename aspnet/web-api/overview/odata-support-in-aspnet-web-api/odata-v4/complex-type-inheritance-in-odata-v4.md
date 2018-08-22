---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: 使用 ASP.NET Web API OData v4 中的複雜類型繼承 |Microsoft Docs
author: microsoft
description: 根據 OData v4 規格中，複雜型別可以繼承自另一個複雜類型。 （複雜型別是結構化的型別沒有索引鍵）。Web API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 8dbf7dc4cfc70e1ea1ed6f72ffc0751a56809c09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825548"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>使用 ASP.NET Web API OData v4 中的複雜類型繼承
====================
by [Microsoft](https://github.com/microsoft)

> 根據 OData v4[規格](http://www.odata.org/documentation/odata-version-4-0/)，複雜型別可以繼承自另一個複雜類型。 (A*複雜*型別是結構化的型別沒有索引鍵。)Web API OData 5.3 支援複雜類型繼承。
> 
> 本主題說明如何建置具有複雜繼承類型的實體資料模型 (EDM)。 如需完整的原始程式碼，請參閱[OData 複雜類型繼承範例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>模型階層

為了說明複雜類型繼承，我們將使用下列的類別階層架構。

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` 是抽象的複雜類型。 `Rectangle``Triangle`，並`Circle`複雜型別衍生自`Shape`，以及`RoundRectangle`衍生自`Rectangle`。 `Window` 是實體類型，包含`Shape`執行個體。

以下是定義這些類型的 CLR 類別。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>建置的 EDM 模型

若要建立 EDM 中，您可以使用**ODataConventionModelBuilder**，這會推斷從 CLR 類型的繼承關聯性。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

您也可以建置 EDM 明確地使用**ODataModelBuilder**。 這會讓更多的程式碼，但可讓您更充分掌控 EDM。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

這兩個範例會建立相同的 EDM 結構描述。

## <a name="metadata-document"></a>中繼資料文件

以下是 OData 中繼資料顯示的文件，複雜類型繼承。

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

中繼資料文件中，您可以看到：

- `Shape`複雜型別為抽象。
- `Rectangle`， `Triangle`，並`Circle`複雜類型具有基底型別`Shape`。
- `RoundRectangle`類型具有基底類型`Rectangle`。

## <a name="casting-complex-types"></a>將複雜型別轉型

現在支援上的複雜型別進行轉換。 例如，下列查詢會轉換`Shape`至`Rectangle`。

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

以下是回應承載：

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
