---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: 安全性指導方針，ASP.NET web API 2 OData |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868704"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>安全性指導方針，ASP.NET web API 2 OData
====================
由[Mike Wasson](https://github.com/MikeWasson)

本主題描述一些公開 OData 透過資料集時，您應該考慮的安全性問題。

## <a name="edm-security"></a>EDM 安全性

查詢語意會根據實體資料模型 (EDM) 中，沒有基礎的模型型別。 您可以排除 EDM 中的屬性，它將看不到查詢。 例如，假設您的模型包含的員工薪資屬性類型。 您可能想要排除這個屬性，以便隱藏用戶端在 EDM 中。

有兩種方式可以排除在 EDM 中的屬性。 您可以設定 **[IgnoreDataMember]** 模型類別中的屬性上的屬性：

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

您也可以移除此屬性在 EDM 中以程式設計的方式：

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>查詢的安全性

惡意或貝氏的用戶端可以建構花很長的時間執行的查詢。 最糟的情況中，這可能會中斷您服務的存取權。

**[Queryable]** 屬性是剖析、 驗證以及將查詢套用的動作篩選條件。 篩選器會將查詢選項轉換成 LINQ 運算式。 當 OData 控制器傳回**IQueryable**型別， **IQueryable** LINQ 提供者會將 LINQ 運算式轉換成一個查詢。 因此，效能取決於 LINQ 提供者使用，以及您的資料集或資料庫結構描述的特定特性。

如需在 ASP.NET Web API 中使用 OData 查詢選項的詳細資訊，請參閱[支援 OData 查詢選項](supporting-odata-query-options.md)。

如果您知道所有用戶端所信任 （例如，在企業環境中），或您的資料集很小，查詢效能可能不會產生問題。 否則，您應該考慮下列建議。

- 測試您的服務提供各種查詢和分析資料庫。
- 啟用伺服器導向的分頁，若要避免在單一查詢中傳回大型資料集。 如需詳細資訊，請參閱[Server-Driven 分頁](supporting-odata-query-options.md#server-paging)。 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- 您需要 $filter 和 $orderby 嗎？ 某些應用程式可能會允許用戶端分頁、 使用 $top 和 $skip，但停用其他查詢選項。 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- 請考慮限制 $orderby 到叢集索引中的屬性。 排序沒有叢集索引的大型資料速度很慢。 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- 最大節點計數：**為 MaxNodeCount**屬性 **[Queryable]** 設定最大的數字節點允許 $filter 語法樹狀目錄中。 預設值是 100，但可能會想要設定較低的值，因為大量節點可能會慢而無法編譯。 特別是如果您使用 LINQ to Objects （亦即，在記憶體中，而不使用中繼的 LINQ 提供者集合的 LINQ 查詢）。 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- 請考慮停用的 any （） 和 all （） 函數，這些可能會很慢。 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- 如果任何字串屬性包含大型字串 & #8212for 範例、 產品描述或部落格文章： & #8212consider 停用字串函式。 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- 請考慮不允許的導覽屬性進行篩選。 導覽屬性上的篩選可能會導致聯結，可能會變慢，視您的資料庫結構描述。 下列程式碼會示範查詢驗證程式，可防止的導覽屬性進行篩選。 如需查詢驗證程式的詳細資訊，請參閱[查詢驗證](supporting-odata-query-options.md#query-validation)。 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- 請考慮限制 $filter 查詢撰寫已針對您的資料庫進行自訂的驗證程式。 例如，請考慮這兩個查詢： 

  - 最後一個名稱開頭為 'A' 的執行者的所有電影。
  - 所有的影片，1994 年發行。

    除非電影會由動作項目編製索引，第一個查詢可能需要掃描整個清單的電影資料庫引擎。 而第二個查詢都可能是可接受的此時是假設的電影的索引是由發行年份。

    下列程式碼顯示的驗證程式，可讓您針對 「 ReleaseYear"和"Title"屬性，但沒有其他屬性進行篩選。

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- 一般情況下，請考慮您需要哪些 $filter 函式。 如果您的用戶端不需要 $filter 完整表達，您可以限制允許的函式。
