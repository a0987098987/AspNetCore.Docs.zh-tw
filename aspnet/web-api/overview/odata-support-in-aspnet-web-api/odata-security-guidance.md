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
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="5afae-102">安全性指導方針，ASP.NET web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="5afae-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="5afae-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5afae-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5afae-104">本主題描述一些公開 OData 透過資料集時，您應該考慮的安全性問題。</span><span class="sxs-lookup"><span data-stu-id="5afae-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="5afae-105">EDM 安全性</span><span class="sxs-lookup"><span data-stu-id="5afae-105">EDM Security</span></span>

<span data-ttu-id="5afae-106">查詢語意會根據實體資料模型 (EDM) 中，沒有基礎的模型型別。</span><span class="sxs-lookup"><span data-stu-id="5afae-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="5afae-107">您可以排除 EDM 中的屬性，它將看不到查詢。</span><span class="sxs-lookup"><span data-stu-id="5afae-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="5afae-108">例如，假設您的模型包含的員工薪資屬性類型。</span><span class="sxs-lookup"><span data-stu-id="5afae-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="5afae-109">您可能想要排除這個屬性，以便隱藏用戶端在 EDM 中。</span><span class="sxs-lookup"><span data-stu-id="5afae-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="5afae-110">有兩種方式可以排除在 EDM 中的屬性。</span><span class="sxs-lookup"><span data-stu-id="5afae-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="5afae-111">您可以設定**[IgnoreDataMember]**模型類別中的屬性上的屬性：</span><span class="sxs-lookup"><span data-stu-id="5afae-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="5afae-112">您也可以移除此屬性在 EDM 中以程式設計的方式：</span><span class="sxs-lookup"><span data-stu-id="5afae-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="5afae-113">查詢的安全性</span><span class="sxs-lookup"><span data-stu-id="5afae-113">Query Security</span></span>

<span data-ttu-id="5afae-114">惡意或貝氏的用戶端可以建構花很長的時間執行的查詢。</span><span class="sxs-lookup"><span data-stu-id="5afae-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="5afae-115">最糟的情況中，這可能會中斷您服務的存取權。</span><span class="sxs-lookup"><span data-stu-id="5afae-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="5afae-116">**[Queryable]**屬性是剖析、 驗證以及將查詢套用的動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="5afae-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="5afae-117">篩選器會將查詢選項轉換成 LINQ 運算式。</span><span class="sxs-lookup"><span data-stu-id="5afae-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="5afae-118">當 OData 控制器傳回**IQueryable**型別， **IQueryable** LINQ 提供者會將 LINQ 運算式轉換成一個查詢。</span><span class="sxs-lookup"><span data-stu-id="5afae-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="5afae-119">因此，效能取決於 LINQ 提供者使用，以及您的資料集或資料庫結構描述的特定特性。</span><span class="sxs-lookup"><span data-stu-id="5afae-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="5afae-120">如需在 ASP.NET Web API 中使用 OData 查詢選項的詳細資訊，請參閱[支援 OData 查詢選項](supporting-odata-query-options.md)。</span><span class="sxs-lookup"><span data-stu-id="5afae-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="5afae-121">如果您知道所有用戶端所信任 （例如，在企業環境中），或您的資料集很小，查詢效能可能不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="5afae-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="5afae-122">否則，您應該考慮下列建議。</span><span class="sxs-lookup"><span data-stu-id="5afae-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="5afae-123">測試您的服務提供各種查詢和分析資料庫。</span><span class="sxs-lookup"><span data-stu-id="5afae-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="5afae-124">啟用伺服器導向的分頁，若要避免在單一查詢中傳回大型資料集。</span><span class="sxs-lookup"><span data-stu-id="5afae-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="5afae-125">如需詳細資訊，請參閱[Server-Driven 分頁](supporting-odata-query-options.md#server-paging)。</span><span class="sxs-lookup"><span data-stu-id="5afae-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="5afae-126">您需要 $filter 和 $orderby 嗎？</span><span class="sxs-lookup"><span data-stu-id="5afae-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="5afae-127">某些應用程式可能會允許用戶端分頁、 使用 $top 和 $skip，但停用其他查詢選項。</span><span class="sxs-lookup"><span data-stu-id="5afae-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="5afae-128">請考慮限制 $orderby 到叢集索引中的屬性。</span><span class="sxs-lookup"><span data-stu-id="5afae-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="5afae-129">排序沒有叢集索引的大型資料速度很慢。</span><span class="sxs-lookup"><span data-stu-id="5afae-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="5afae-130">最大節點計數：**為 MaxNodeCount**屬性**[Queryable]**設定最大的數字節點允許 $filter 語法樹狀目錄中。</span><span class="sxs-lookup"><span data-stu-id="5afae-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="5afae-131">預設值是 100，但可能會想要設定較低的值，因為大量節點可能會慢而無法編譯。</span><span class="sxs-lookup"><span data-stu-id="5afae-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="5afae-132">特別是如果您使用 LINQ to Objects （亦即，在記憶體中，而不使用中繼的 LINQ 提供者集合的 LINQ 查詢）。</span><span class="sxs-lookup"><span data-stu-id="5afae-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="5afae-133">請考慮停用的 any （） 和 all （） 函數，這些可能會很慢。</span><span class="sxs-lookup"><span data-stu-id="5afae-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="5afae-134">如果任何字串屬性包含大型字串 & #8212for 範例、 產品描述或部落格文章： & #8212consider 停用字串函式。</span><span class="sxs-lookup"><span data-stu-id="5afae-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="5afae-135">請考慮不允許的導覽屬性進行篩選。</span><span class="sxs-lookup"><span data-stu-id="5afae-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="5afae-136">導覽屬性上的篩選可能會導致聯結，可能會變慢，視您的資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="5afae-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="5afae-137">下列程式碼會示範查詢驗證程式，可防止的導覽屬性進行篩選。</span><span class="sxs-lookup"><span data-stu-id="5afae-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="5afae-138">如需查詢驗證程式的詳細資訊，請參閱[查詢驗證](supporting-odata-query-options.md#query-validation)。</span><span class="sxs-lookup"><span data-stu-id="5afae-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="5afae-139">請考慮限制 $filter 查詢撰寫已針對您的資料庫進行自訂的驗證程式。</span><span class="sxs-lookup"><span data-stu-id="5afae-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="5afae-140">例如，請考慮這兩個查詢：</span><span class="sxs-lookup"><span data-stu-id="5afae-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="5afae-141">最後一個名稱開頭為 'A' 的執行者的所有電影。</span><span class="sxs-lookup"><span data-stu-id="5afae-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="5afae-142">所有的影片，1994 年發行。</span><span class="sxs-lookup"><span data-stu-id="5afae-142">All movies released in 1994.</span></span>

    <span data-ttu-id="5afae-143">除非電影會由動作項目編製索引，第一個查詢可能需要掃描整個清單的電影資料庫引擎。</span><span class="sxs-lookup"><span data-stu-id="5afae-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="5afae-144">而第二個查詢都可能是可接受的此時是假設的電影的索引是由發行年份。</span><span class="sxs-lookup"><span data-stu-id="5afae-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="5afae-145">下列程式碼顯示的驗證程式，可讓您針對 「 ReleaseYear"和"Title"屬性，但沒有其他屬性進行篩選。</span><span class="sxs-lookup"><span data-stu-id="5afae-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="5afae-146">一般情況下，請考慮您需要哪些 $filter 函式。</span><span class="sxs-lookup"><span data-stu-id="5afae-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="5afae-147">如果您的用戶端不需要 $filter 完整表達，您可以限制允許的函式。</span><span class="sxs-lookup"><span data-stu-id="5afae-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
