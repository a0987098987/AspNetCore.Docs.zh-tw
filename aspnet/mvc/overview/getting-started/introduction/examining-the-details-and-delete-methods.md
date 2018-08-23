---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: 檢查 Details 和 Delete 方法 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 3ec5a9a91cf1e2273f91a529779936fd26182fc4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835038"
---
<a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="d1341-102">檢查 Details 和 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="d1341-102">Examining the Details and Delete Methods</span></span>
====================
<span data-ttu-id="d1341-103">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d1341-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="d1341-104">在本教學課程的這個部分，您將會檢查自動產生`Details`和`Delete`方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-104">In this part of the tutorial, you'll examine the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="d1341-105">檢查 Details 和 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="d1341-105">Examining the Details and Delete Methods</span></span>

<span data-ttu-id="d1341-106">開啟`Movie`控制器，並檢查`Details`方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-106">Open the `Movie` controller and examine the `Details` method.</span></span>

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

<span data-ttu-id="d1341-107">建立這個動作方法的 MVC scaffolding 引擎會將顯示 HTTP 要求叫用方法的註解。</span><span class="sxs-lookup"><span data-stu-id="d1341-107">The MVC scaffolding engine that created this action method adds a comment showing a HTTP request that invokes the method.</span></span> <span data-ttu-id="d1341-108">在此情況下很`GET`三個 URL 區段，要求`Movies`控制器`Details`方法和`ID`值。</span><span class="sxs-lookup"><span data-stu-id="d1341-108">In this case it's a `GET` request with three URL segments, the `Movies` controller, the `Details` method and a `ID` value.</span></span>

<span data-ttu-id="d1341-109">程式碼第一次可讓您輕鬆地搜尋資料使用`Find`方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-109">Code First makes it easy to search for data using the `Find` method.</span></span> <span data-ttu-id="d1341-110">方法內建的重要安全性功能是，程式碼會確認`Find`方法已找到電影之前的程式碼嘗試執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="d1341-110">An important security feature built into the method is that the code verifies that the `Find` method has found a movie before the code tries to do anything with it.</span></span> <span data-ttu-id="d1341-111">比方說，駭客可能會導致發生錯誤的站台的連結所建立的 URL 變更`http://localhost:xxxx/Movies/Details/1`為類似`http://localhost:xxxx/Movies/Details/12345`（或不代表實際電影的一些其他值）。</span><span class="sxs-lookup"><span data-stu-id="d1341-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="d1341-112">如果您未檢查是否有 null 的電影，null 的電影會導致資料庫錯誤。</span><span class="sxs-lookup"><span data-stu-id="d1341-112">If you did not check for a null movie, a null movie would result in a database error.</span></span>

<span data-ttu-id="d1341-113">檢查 `Delete` 和 `DeleteConfirmed` 方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

<span data-ttu-id="d1341-114">請注意，HTTP GET`Delete`方法並不會刪除指定的電影，它會傳回電影的檢視設定，您可以提交 (`HttpPost`) 刪除。</span><span class="sxs-lookup"><span data-stu-id="d1341-114">Note that the HTTP GET `Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (`HttpPost`) the deletion.</span></span> <span data-ttu-id="d1341-115">如果您執行刪除作業以回應 GET 要求 (或是執行相關編輯作業、建立作業或任何會變更資料的其他作業)，則會造成安全性漏洞。</span><span class="sxs-lookup"><span data-stu-id="d1341-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span> <span data-ttu-id="d1341-116">如需詳細資訊，請參閱 Stephen walther 所撰寫的部落格項目[ASP.NET MVC 秘訣 #46 — 不用刪除連結，因為它們會造成安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d1341-116">For more information about this, see Stephen Walther's blog entry [ASP.NET MVC Tip #46 — Don't use Delete Links because they create Security Holes](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span></span>

<span data-ttu-id="d1341-117">我們將可刪除資料的 `HttpPost` 方法命名為 `DeleteConfirmed`，以提供 HTTP POST 方法的唯一簽章或名稱。</span><span class="sxs-lookup"><span data-stu-id="d1341-117">The `HttpPost` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="d1341-118">這兩個方法簽章如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1341-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

<span data-ttu-id="d1341-119">通用語言執行平台 (CLR) 需要多載方法，以提供唯一的參數簽章 (方法名稱相同但參數清單不同)。</span><span class="sxs-lookup"><span data-stu-id="d1341-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="d1341-120">不過，此處您需要兩個 Delete 方法，一個用於 GET-和另一個用於 POST 兩者都有相同的參數簽章。</span><span class="sxs-lookup"><span data-stu-id="d1341-120">However, here you need two Delete methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="d1341-121">(它們都需要接受單一整數作為參數)。</span><span class="sxs-lookup"><span data-stu-id="d1341-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="d1341-122">若要排序時，您可以執行兩件事。</span><span class="sxs-lookup"><span data-stu-id="d1341-122">To sort this out, you can do a couple of things.</span></span> <span data-ttu-id="d1341-123">其中一個是為方法提供不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="d1341-123">One is to give the methods different names.</span></span> <span data-ttu-id="d1341-124">這是 scaffolding 機制在上述範例採取的方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-124">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="d1341-125">不過，這麼做會導致一個小問題：ASP.NET 會依名稱將 URL 區段與動作方法對應，一旦您重新命名方法，路由通常就會找不到這個方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-125">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="d1341-126">解決辦法正如您看到的這個範例：將 `ActionName("Delete")` 屬性新增至 `DeleteConfirmed` 方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-126">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="d1341-127">這會有效地執行對應的路由系統，讓 URL，包括 */Delete/* 針對 POST 要求將會找到`DeleteConfirmed`方法。</span><span class="sxs-lookup"><span data-stu-id="d1341-127">This effectively performs mapping for the routing system so that a URL that includes */Delete/* for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="d1341-128">若要避免具有相同名稱和簽章的方法有問題的另一個常見方法是人為方式變更 POST 方法，以包含未使用的參數的簽章。</span><span class="sxs-lookup"><span data-stu-id="d1341-128">Another common way to avoid a problem with methods that have identical names and signatures is to artificially change the signature of the POST method to include an unused parameter.</span></span> <span data-ttu-id="d1341-129">比方說，有些開發人員將參數類型`FormCollection`傳遞至 POST 方法，然後只需不使用參數：</span><span class="sxs-lookup"><span data-stu-id="d1341-129">For example, some developers add a parameter type `FormCollection` that is passed to the POST method, and then simply don't use the parameter:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a><span data-ttu-id="d1341-130">總結</span><span class="sxs-lookup"><span data-stu-id="d1341-130">Summary</span></span>

<span data-ttu-id="d1341-131">您現在有完整的 ASP.NET MVC 應用程式將資料儲存在本機的 DB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="d1341-131">You now have a complete ASP.NET MVC application that stores data in a local DB database.</span></span> <span data-ttu-id="d1341-132">您可以建立、 讀取、 更新、 刪除和搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="d1341-132">You can create, read, update, delete, and search for movies.</span></span>

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a><span data-ttu-id="d1341-133">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d1341-133">Next Steps</span></span>

<span data-ttu-id="d1341-134">在您建置並測試 web 應用程式之後下, 一個步驟是將它提供給其他人透過網際網路使用。</span><span class="sxs-lookup"><span data-stu-id="d1341-134">After you have built and tested a web application, the next step is to make it available to other people to use over the Internet.</span></span> <span data-ttu-id="d1341-135">若要這麼做，您必須將它部署至 web 主控提供者。</span><span class="sxs-lookup"><span data-stu-id="d1341-135">To do that, you have to deploy it to a web hosting provider.</span></span> <span data-ttu-id="d1341-136">Microsoft 提供免費的 web 裝載中的最多 10 個 web sites[免費 Azure 試用帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="d1341-136">Microsoft offers free web hosting for up to 10 web sites in a [free Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="d1341-137">我建議您接下來會依照我教學課程[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="d1341-137">I suggest you next follow my tutorial [Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="d1341-138">絕佳的教學課程是 Tom Dykstra 中繼層級[建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="d1341-138">An excellent tutorial is Tom Dykstra's intermediate-level [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="d1341-139">[Stackoverflow](http://stackoverflow.com/help)而[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)是很大的位置，以詢問問題。</span><span class="sxs-lookup"><span data-stu-id="d1341-139">[Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are a great places to ask questions.</span></span> <span data-ttu-id="d1341-140">請遵循[我](https://twitter.com/RickAndMSFT)twitter，因此您可以取得我最新的教學課程的更新。</span><span class="sxs-lookup"><span data-stu-id="d1341-140">Follow [me](https://twitter.com/RickAndMSFT) on twitter so you can get updates on my latest tutorials.</span></span>

<span data-ttu-id="d1341-141">意見反應非常歡迎畫面。</span><span class="sxs-lookup"><span data-stu-id="d1341-141">Feedback is welcome.</span></span>

<span data-ttu-id="d1341-142">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1341-142">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span></span>  
<span data-ttu-id="d1341-143">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d1341-143">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d1341-144">上一步</span><span class="sxs-lookup"><span data-stu-id="d1341-144">Previous</span></span>](adding-validation.md)
