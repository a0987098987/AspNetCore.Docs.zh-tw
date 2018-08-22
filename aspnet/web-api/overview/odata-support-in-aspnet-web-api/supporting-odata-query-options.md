---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: 支援 OData 查詢選項中 ASP.NET Web API 2 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 8745183125c9dd1dcc7cb0e146367a893bdb0170
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824722"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中支援 OData 查詢選項
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

OData 會定義可用來修改 OData 查詢的參數。 用戶端在要求 URI 查詢字串中傳送這些參數。 例如，若要排序的結果，用戶端會使用 $orderby 參數：

`http://localhost/Products?$orderby=Name`

OData 規格會呼叫這些參數*查詢選項*。 您可以啟用您專案中的任何 Web API 控制器的 OData 查詢選項&#8212;控制器不需要是 OData 端點。 這會讓您加入功能，例如篩選和排序任何 Web API 應用程式的便利方式。

啟用查詢選項，請參閱本主題之前[OData 安全性指導](odata-security-guidance.md)。

- [啟用 OData 查詢選項](#enable)
- [範例查詢](#examples)
- [伺服器驅動型分頁](#server-paging)
- [限制的查詢選項](#limiting_query_options)
- [直接叫用查詢選項](#ODataQueryOptions)
- [查詢驗證](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>啟用 OData 查詢選項

Web API 支援下列 OData 查詢選項：

| 選項 | 描述 |
| --- | --- |
| $ expand | 展開相關的實體內嵌。 |
| $filter | 篩選結果，根據布林條件。 |
| $inlinecount | 告知伺服器應回應中包含的相符實體總計數。 （適用於伺服器端分頁。） |
| $orderby | 排序結果。 |
| $select | 選取要包含在回應中的屬性。 |
| $skip | 略過前 n 個結果。 |
| $top | 傳回前 n 個結果。 |

若要使用的 OData 查詢選項，您必須明確啟用它們。 您可以全域啟用整個應用程式，或針對特定控制器或特定的動作啟用它們。

若要全域啟用 OData 查詢選項，請呼叫**EnableQuerySupport**上**HttpConfiguration**在啟動類別：

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport**方法可讓查詢選項，會傳回任何控制器動作的全域**IQueryable**型別。 如果您不想啟用整個應用程式的查詢選項，您可以讓它們適用於特定的控制器動作加 **[Queryable]** 屬性加入動作方法。

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>範例查詢

本節說明是可能使用的 OData 查詢選項的查詢的類型。 如特定的查詢選項的詳細資訊，請參閱 OData 文件，網址[www.odata.org](http://www.odata.org/)。

如需 $展開和 $select，請參閱[使用 $select，$expand、 和 ASP.NET Web API OData 中的 $value](using-select-expand-and-value.md)。

**用戶端驅動型分頁**

為大型實體集，用戶端可能會想要限制結果數目。 例如，用戶端可能會顯示 10 個項目一次 下一步 」 的連結，來取得下一頁的結果。 若要這樣做，用戶端會使用 $top] 和 [$skip 選項。

`http://localhost/Products?$top=10&$skip=20`

$Top 選項提供項目，以傳回，最大數目，以及 $skip 選項以略過的項目數。 前一個範例會擷取項目 21 到 30。

**篩選**

$Filter 選項可讓用戶端所套用的布林運算式篩選結果。 篩選條件運算式是相當強大;其中包括邏輯和算術運算子、 字串函數，以及日期函式。

| 會傳回等於 「 玩具 」 類別的所有產品。 | `http://localhost/Products?$filter=Category` eq 'Toys' |
| --- | --- |
| 會傳回標價小於 10 的所有產品。 | `http://localhost/Products?$filter=Price` l 10 |
| 邏輯運算子： 傳回所有產品，價格 > = 5 且價格 < = 15。 | `http://localhost/Products?$filter=Price` ge 5 和價格 le 15 |
| 字串函數： 傳回與"zz"的所有產品名稱中。 | `http://localhost/Products?$filter=substringof('zz',Name)` |
| 日期函數： 傳回所有產品 ReleaseDate 2005 之後。 | `http://localhost/Products?$filter=year(ReleaseDate)` t 2005 |

**排序**

若要排序的結果，請使用 $orderby 篩選。

| 價格依排序。 | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| 排序依據以遞減順序 （最高到最低） 的價格。 | `http://localhost/Products?$orderby=Price desc` |
| 依類別排序，然後依價格遞減類別內的順序排序。 | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>伺服器驅動型分頁

如果您的資料庫包含數百萬筆記錄，您不想傳送這些全都放在一個裝載。 若要避免這個問題，伺服器可以限制單一回應中傳送的項目數。 若要啟用伺服器分頁，將**PageSize**中的屬性**Queryable**屬性。 值是要傳回的項目數目上限。

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

如果您的控制器傳回 OData 格式時，回應主體會包含下一頁資料的連結：

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

用戶端可以使用此連結，以擷取下一個頁面。 若要了解結果集中的項目總數，用戶端可以設定 $inlinecount 查詢選項的值"allpages 」。

`http://localhost/Products?$inlinecount=allpages`

值"allpages 」 會要求伺服器在回應中包含的總數：

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> 下一頁連結和內嵌計數都需要 OData 格式。 原因是，OData 會定義特殊欄位來保存的連結和計數的回應主體中。


對於非 OData 格式，就仍然可以支援藉由包裝的查詢結果中的下一頁連結和內嵌計數**PageResult&lt;T&gt;** 物件。 不過，它需要更多的程式碼。 請看以下範例：

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

以下是範例 JSON 回應：

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>限制的查詢選項

查詢選項會導致在用戶端控制伺服器執行的查詢很多。 在某些情況下，您可能想要限制可用的選項，基於安全性或效能的考量。 **[Queryable]** 屬性有部分內建屬性的。 以下是一些範例。

僅允許 $skip 和 $top，以支援分頁並沒有其他項目：

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

若要避免在資料庫中建立索引的屬性上的排序順序只能由特定的屬性，可讓：

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

允許"eq"的邏輯函式，但沒有其他的邏輯函數：

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

不允許任何的算術運算子：

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

您可以藉由建構，全域限制選項**QueryableAttribute**執行個體，並將其傳遞給**EnableQuerySupport**函式：

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>直接叫用查詢選項

而不是使用 **[Queryable]** 屬性，您可以直接在您的控制器中叫用的查詢選項。 若要這樣做，請新增**ODataQueryOptions**控制器方法的參數。 在此情況下，您不需要 **[Queryable]** 屬性。

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API 會填入**ODataQueryOptions**從 URI 查詢字串。 若要套用查詢，請傳遞**IQueryable**要**ApplyTo**方法。 此方法會傳回另一個**IQueryable**。

對於進階案例，如果您不需要**IQueryable**查詢提供者，您可以檢查**ODataQueryOptions**並轉譯成另一種形式的查詢選項。 (例如，請參閱 RaghuRam Nadiminti 部落格文章[轉譯的 OData 查詢，為 HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx)，其中也包括[範例](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt)。)

<a id="query-validation"></a>
## <a name="query-validation"></a>查詢驗證

**[Queryable]** 屬性驗證的查詢，然後再執行它。 驗證步驟在中執行**QueryableAttribute.ValidateQuery**方法。 您也可以自訂驗證程序。

另請參閱[OData 安全性指導](odata-security-guidance.md)。

覆寫其中一個驗證程式也就是類別定義中的第一次， **Web.Http.OData.Query.Validators**命名空間。 例如，下列的驗證程式類別會停用 $orderby 選項的 'desc' 選項。

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

子類別 **[Queryable]** 屬性來覆寫**ValidateQuery**方法。

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

然後設定您的自訂屬性是全域或每個控制站：

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

如果您使用**ODataQueryOptions**直接設定選項中的 驗證程式：

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
