---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Security Guidance for ASP.NET Web API 2 OData |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41f8a84285579e4aea35b8b1ed7aeb5cc8d5e385
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806599"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Security Guidance for ASP.NET Web API 2 OData
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

本主題描述一些您應該考慮公開透過 OData 資料集時的安全性問題。

## <a name="edm-security"></a>EDM 安全性

查詢語意根據 entity data model (EDM) 中，不基礎的模型型別。 您可以排除 EDM 中的屬性，將不會顯示查詢。 例如，假設您的模型包含員工具有的型別薪資屬性。 您可能想要隱藏來自用戶端在 EDM 中排除此屬性。

有兩種方式可以排除在 EDM 中的屬性。 您可以設定 **[IgnoreDataMember]** 模型類別中的屬性上的屬性：

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

您也可以移除該屬性在 EDM 中以程式設計的方式：

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>查詢安全性

惡意或單純的用戶端可以建構花很長的時間執行的查詢。 最糟的情況中，這可能會中斷服務的存取。

**[Queryable]** 是動作篩選條件，以剖析、 驗證，並將查詢套用的屬性。 篩選條件會將查詢選項轉換成 LINQ 運算式。 當 OData 控制器會傳回**IQueryable**型別**IQueryable** LINQ 提供者會將 LINQ 運算式轉換成一個查詢。 因此，效能取決於使用時，LINQ 提供者同時也會在您的資料集或資料庫結構描述的特定特性。

如需 ASP.NET Web API 中使用 OData 查詢選項的詳細資訊，請參閱 <<c0> [ 支援的 OData 查詢選項](supporting-odata-query-options.md)。

如果您知道所有用戶端所信任 （例如，在企業環境中），或您的資料集很小，查詢效能可能不會有問題。 否則，您應該考慮下列建議。

- 測試您的服務，使用各種不同的查詢和分析資料庫。
- 啟用伺服器驅動型分頁，來避免傳回大型資料集在單一查詢中。 如需詳細資訊，請參閱 < [Server-Driven 分頁](supporting-odata-query-options.md#server-paging)。 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- 您需要 $filter 和 $orderby 嗎？ 有些應用程式可能會允許用戶端分頁、 使用 $top 和 $skip，但停用其他查詢選項。 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- 請考慮限制 $orderby 叢集索引中的屬性。 沒有叢集索引的大型資料的排序速度很慢。 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- 節點計數上限： **MaxNodeCount**屬性上的 **[Queryable]** 設定 $filter 語法樹狀目錄中允許的最大數目節點。 預設值是 100，但因為大量節點可能會慢而無法編譯，您可能想要設定較低的值。 特別是如果您使用 LINQ to Objects （亦即，在記憶體中，而不使用中繼 LINQ 提供者的集合上的 LINQ 查詢）。 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- 請考慮停用的 any （） 和 all （） 函式，這些可能會很慢。 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- 如果任何字串屬性包含大型字串 & #8212for 範例、 產品描述或部落格 & #8212consider 停用的字串函數。 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- 請考慮不允許的導覽屬性進行篩選。 導覽屬性的篩選可能會導致聯結，可能會變慢，視您的資料庫結構描述。 下列程式碼會顯示查詢驗證程式，以防止篩選導覽屬性。 如需有關查詢驗證程式的詳細資訊，請參閱 <<c0> [ 查詢驗證](supporting-odata-query-options.md#query-validation)。 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- 請考慮限制 $filter 查詢藉由撰寫自訂資料庫的驗證程式。 例如，請考慮這兩個查詢： 

  - 與動作項目以 'A' 開頭的姓氏的所有影片。
  - 1994 年發行的所有影片。

    除非電影會由動作項目編製索引，第一個查詢可能需要掃描整個清單的電影資料庫引擎。 而第二個的查詢可能會是可接受的則假設電影會編製索引的發行年份。

    下列程式碼顯示的驗證程式，可讓您的 「 ReleaseYear"和"Title"屬性，但沒有其他屬性的篩選。

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- 一般情況下，請考慮您需要哪些 $filter 函式。 如果您的用戶端不需要 $filter 完整表達，您可以限制允許的函式。
