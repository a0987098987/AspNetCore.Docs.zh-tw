---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: 使用 ASP.NET Web API OData v4 中開啟類型 |Microsoft Docs
author: microsoft
description: OData v4 中開啟的型別是 stuctured 型別包含動態屬性，除了任何型別定義中宣告的屬性。 開啟...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 77771d85532b8b622c2ad4ca219a38990e474c9c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834608"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>使用 ASP.NET Web API OData v4 中開啟類型
====================
by [Microsoft](https://github.com/microsoft)

> 在 OData v4*開啟輸入*stuctured 型別，其中包含動態屬性，除了任何型別定義中宣告的屬性。 開放式類型可讓您將新增至您的資料模型的彈性。 本教學課程會示範如何使用 ASP.NET Web API OData 中的開放型別。
> 
> 本教學課程假設您已經知道如何建立 ASP.NET Web API 中的 OData 端點。 如果沒有，請先閱讀[建立 OData v4 端點](create-an-odata-v4-endpoint.md)第一次。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API OData 5.3
> - OData v4


首先，OData 的一些術語：

- 實體類型： 具有索引鍵的結構化型別。
- 複雜型別： 結構化的型別沒有索引鍵。
- 開啟類型： 具有動態屬性的型別。 實體類型和複雜型別都可以開啟。

動態屬性的值可以是基本型別、 複雜型別或列舉型別;或任何一種類型的集合。 如需開啟類型的詳細資訊，請參閱[OData v4 規格](http://www.odata.org/documentation/odata-version-4-0/)。

## <a name="install-the-web-odata-libraries"></a>安裝 Web OData 程式庫

使用 NuGet 套件管理員來安裝最新的 Web API OData 程式庫。 從 [套件管理員主控台] 視窗中：

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>定義 CLR 類型

從以 CLR 型別定義的 EDM 模型開始。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

建立 Entity Data Model (EDM) 時，

- `Category` 是列舉類型。
- `Address` 是複雜類型。 （它並沒有索引鍵，因此它不是實體類型。）
- `Customer` 是實體類型。 （它有索引鍵）。
- `Press` 是開放式的複雜型別。
- `Book` 是為開放實體類型。

若要建立開啟的型別，CLR 型別必須具有型別的屬性`IDictionary<string, object>`，其會保存動態屬性。

## <a name="build-the-edm-model"></a>建置的 EDM 模型

如果您使用**ODataConventionModelBuilder**若要建立 EDM 中，`Press`並`Book`會自動新增為開啟類型時，根據是否存在`IDictionary<string, object>`屬性。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

您也可以建置 EDM 明確地使用**ODataModelBuilder**。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>加入 OData 控制器

接下來，新增 OData 控制器。 本教學課程中，我們將使用簡化的控制器，只支援 GET 和 POST 要求時，並使用記憶體中清單來儲存實體。

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

請注意，第一個`Book`執行個體有沒有動態屬性。 第二個`Book`執行個體有下列的動態屬性：

- 「 發佈 」: 基本類型
- 「 著作人 」: 基本類型的集合
- 「 OtherCategories": 列舉型別的集合。

此外，`Press`屬性，`Book`執行個體有下列的動態屬性：

- "部落格": 基本類型
- "Address": 複雜型別

## <a name="query-the-metadata"></a>查詢的中繼資料

若要取得 OData 中繼資料文件，將 GET 要求傳送至`~/$metadata`。 回應主體看起來應該如下所示：

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

中繼資料文件中，您可以看到：

- 針對`Book`並`Press`類型的值`OpenType`屬性為 true。 `Customer`和`Address`型別沒有這個屬性。
- `Book`實體類型有三個宣告的屬性： ISBN、 標題和按。 OData 中繼資料不包含`Book.Properties`從 CLR 類別的屬性。
- 同樣地，`Press`複雜型別具有只有兩個宣告的屬性： 名稱和類別目錄。 中繼資料不包含`Press.DynamicProperties`從 CLR 類別的屬性。

## <a name="query-an-entity"></a>查詢實體

若要取得 ISBN 的書本等於"978-0-7356-7942-9"，將 GET 要求傳送至`~/Books('978-0-7356-7942-9')`。 回應主體看起來應該如下所示。 （縮排，以使其易於讀取。）

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

請注意，動態屬性內嵌包含宣告的屬性。

## <a name="post-an-entity"></a>POST 實體

若要加入活頁簿的實體，傳送 POST 要求至`~/Books`。 用戶端可以要求裝載中設定動態屬性。

以下是範例要求。 請注意 「 價格 」 和 「 已發佈 」 屬性。

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

如果您是在控制器方法中設定中斷點，您可以看到 Web API 新增這些屬性，以`Properties`字典。

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>其他資源

[OData 開放類型範例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
