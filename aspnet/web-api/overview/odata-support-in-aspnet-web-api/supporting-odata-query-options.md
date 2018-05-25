---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: 支援 OData 查詢選項中 ASP.NET Web API 2 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>支援在 ASP.NET Web API 2 OData 查詢選項
====================
由[Mike Wasson](https://github.com/MikeWasson)

OData 會定義可以用來修改 OData 查詢的參數。 用戶端在要求 URI 查詢字串中傳送這些參數。 例如，若要排序的結果，用戶端會使用 $orderby 參數：

`http://localhost/Products?$orderby=Name`

OData 規格會呼叫這些參數*查詢選項*。 您可以在您的專案 & #8212; 啟動任何 Web API 控制器的 OData 查詢選項控制站不需要是 OData 端點。 這可讓您加入功能，例如篩選和排序任何 Web API 應用程式的便利方式。

啟用查詢選項，請閱讀本主題之前[OData 安全性指導方針](odata-security-guidance.md)。

- [啟用 OData 查詢選項](#enable)
- [範例查詢](#examples)
- [伺服器導向的分頁](#server-paging)
- [限制的查詢選項](#limiting_query_options)
- [直接叫用查詢選項](#ODataQueryOptions)
- [查詢驗證](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>啟用 OData 查詢選項

Web 應用程式開發介面支援下列 OData 查詢選項：

| 選項 | 說明 |
| --- | --- |
| $ expand | 展開相關的實體內嵌。 |
| $filter | 篩選結果，根據 Boolean 條件。 |
| $inlinecount | 告知伺服器應在回應中包含的相符實體總計數。 （適用於伺服器端分頁。） |
| $orderby | 排序結果。 |
| $select | 選取要在回應中包含的屬性。 |
| $skip | 略過第一組 n 個結果。 |
| $top | 傳回只有前 n 的結果。 |

若要使用 OData 查詢選項，您必須明確啟用它們。 您可以全域啟用整個應用程式，或啟用以進行特定的控制站或特定的動作。

若要全域啟用 OData 查詢選項，呼叫**EnableQuerySupport**上**HttpConfiguration**在啟動時的類別：

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport**方法可讓查詢選項，會傳回任何控制器動作的全域**IQueryable**型別。 如果您不想啟用整個應用程式的查詢選項，您可以啟用以進行特定控制器的動作加入 **[Queryable]** 屬性至動作方法。

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>範例查詢

這個部分顯示的查詢可能使用的 OData 查詢選項的類型。 如特定的查詢選項的詳細資訊，請參閱 OData 文件，網址[www.odata.org](http://www.odata.org/)。

如需 $展開和 $select，請參閱[使用 $select，$expand、 與 ASP.NET Web API OData 中的 $value](using-select-expand-and-value.md)。

**用戶端導向的分頁**

為大型實體集，用戶端可能會想要限制的結果數目。 例如，用戶端可能會顯示 10 個項目一次 下一步 」 連結來取得結果的下一頁。 若要這樣做，用戶端會使用 $top 和 $skip 選項。

`http://localhost/Products?$top=10&$skip=20`

$Top 選項提供項目，以傳回，最大數目，並且 $skip 選項提供略過的項目數。 先前的範例會擷取項目到 30 21。

**篩選**

$Filter 選項可讓用戶端藉由套用布林運算式篩選結果。 篩選條件運算式是相當強大。它們包含的邏輯和算術運算子、 字串函數和日期函數。

| 傳回等於"Toys 」 類別目錄與所有產品。 | `http://localhost/Products?$filter=Category`eq 'Toys' |
| --- | --- |
| 傳回標價小於 10 的所有產品。 | `http://localhost/Products?$filter=Price`lt 10 |
| 邏輯運算子： 傳回所有產品，價格 > = 5 且價格 < = 15。 | `http://localhost/Products?$filter=Price`ge 5 和價格 le 15 |
| 字串函數： 傳回與"zz"的所有產品的名稱。 | `http://localhost/Products?$filter=substringof('zz',Name)` |
| 日期函數： 2005年之後傳回 ReleaseDate 與所有產品。 | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**排序**

若要排序的結果，請使用 $orderby 篩選器。

| 排序依據價格。 | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| 排序依據以遞減順序 （最高到最低） 的價格。 | `http://localhost/Products?$orderby=Price desc` |
| 依據類別排序，然後依價格遞減分類內的順序排序。 | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>伺服器導向的分頁

如果您的資料庫包含數百萬筆記錄，您不想將它們傳送單一承載中的所有。 若要防止這個情況，伺服器可以限制單一回應中傳送的項目數。 若要啟用伺服器分頁， **PageSize**屬性**Queryable**屬性。 值是要傳回的項目數目上限。

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

如果您的控制器傳回 OData 格式時，回應主體會包含下一頁資料的連結：

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

用戶端可以使用此連結來擷取下一個頁面。 若要深入了解結果集中的項目總數，用戶端可以設定 $inlinecount 查詢選項，以值"allpages"。

`http://localhost/Products?$inlinecount=allpages`

值"allpages"告知伺服器應在回應中包含的總計數：

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> 下一步 頁面的連結和內嵌計數都需要 OData 格式。 原因是 OData 定義的特殊欄位，來保存的連結和計數的回應主體中。


對於非 OData 格式，是支援的文繞圖中的查詢結果的下一頁連結和內嵌計數，仍然可以**PageResult&lt;T&gt;** 物件。 不過，它需要更多的程式碼。 請看以下範例：

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

以下是範例 JSON 回應：

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>限制的查詢選項

查詢選項可讓用戶端大量控制伺服器執行的查詢。 在某些情況下，您可能想要限制的可用選項的安全性或效能的考量。 **[Queryable]** 屬性具有某些內建屬性這個。 以下是一些範例。

允許只 $skip 和 $top，以支援分頁而無其他內容：

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

允許排序只能由特定屬性，以防止在資料庫中建立索引的屬性上的排序：

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

允許"eq"邏輯函式，但沒有其他邏輯函數：

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

不允許任何算術運算子：

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

您可以藉由建構全域限制選項**根據 QueryableAttribute**執行個體，並將其傳遞至**EnableQuerySupport**函式：

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>直接叫用查詢選項

而不是使用 **[Queryable]** 屬性，您可以直接在您的控制器中叫用的查詢選項。 若要這樣做，請加入**ODataQueryOptions**控制器方法的參數。 在此情況下，您不需要 **[Queryable]** 屬性。

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API 填入**ODataQueryOptions**從 URI 查詢字串。 若要將查詢套用，傳遞**IQueryable**至**ApplyTo**方法。 方法會傳回另一個**IQueryable**。

進階案例中，如果您不需要**IQueryable**查詢提供者，您可以檢查**ODataQueryOptions**並轉譯成另一種形式的查詢選項。 (例如，請參閱 RaghuRam Nadiminti 部落格文章[轉譯的 OData 查詢翻譯為 HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx)，其中也包括[範例](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt)。)

<a id="query-validation"></a>
## <a name="query-validation"></a>查詢驗證

**[Queryable]** 屬性之前先驗證查詢。 驗證步驟會在中執行**QueryableAttribute.ValidateQuery**方法。 您也可以自訂驗證程序。

另請參閱[OData 安全性指導方針](odata-security-guidance.md)。

首先，覆寫其中一個驗證器的類別也就是定義在**Web.Http.OData.Query.Validators**命名空間。 例如，下列的驗證程式類別會停用 $orderby 選項的 'desc' 選項。

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

子類別 **[Queryable]** 屬性來覆寫**ValidateQuery**方法。

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

然後設定您的自訂屬性是全域或每個控制站：

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

如果您使用**ODataQueryOptions**直接設定之選項的驗證程式：

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
