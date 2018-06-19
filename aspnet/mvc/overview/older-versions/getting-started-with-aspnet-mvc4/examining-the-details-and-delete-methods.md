---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: 檢查詳細資料與刪除方法 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 00f7e5d6679f1bd8875931e601c8151049f785ac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868772"
---
<a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="28edc-104">檢查的詳細資料和 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="28edc-104">Examining the Details and Delete Methods</span></span>
====================
<span data-ttu-id="28edc-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="28edc-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="28edc-106">本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="28edc-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="28edc-107">它更安全、 容易遵循，及示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="28edc-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="28edc-108">在本教學課程的這個部分，您將會檢查自動產生`Details`和`Delete`方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-108">In this part of the tutorial, you'll examine the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="28edc-109">檢查的詳細資料和 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="28edc-109">Examining the Details and Delete Methods</span></span>

<span data-ttu-id="28edc-110">開啟`Movie`控制站，並檢查`Details`方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-110">Open the `Movie` controller and examine the `Details` method.</span></span>

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

<span data-ttu-id="28edc-111">MVC scaffolding 引擎，來建立此動作方法中加入註解顯示叫用方法的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="28edc-111">The MVC scaffolding engine that created this action method adds a comment showing a HTTP request that invokes the method.</span></span> <span data-ttu-id="28edc-112">在此情況下它是`GET`具有三個的 URL 區段的要求`Movies`控制站，`Details`方法和`ID`值。</span><span class="sxs-lookup"><span data-stu-id="28edc-112">In this case it's a `GET` request with three URL segments, the `Movies` controller, the `Details` method and a `ID` value.</span></span>

<span data-ttu-id="28edc-113">程式碼第一次輕鬆地搜尋資料使用`Find`方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-113">Code First makes it easy to search for data using the `Find` method.</span></span> <span data-ttu-id="28edc-114">建置至方法的重要安全性功能是，程式碼驗證`Find`方法發現電影，這個程式碼嘗試執行與它的任何項目之前。</span><span class="sxs-lookup"><span data-stu-id="28edc-114">An important security feature built into the method is that the code verifies that the `Find` method has found a movie before the code tries to do anything with it.</span></span> <span data-ttu-id="28edc-115">比方說，駭客可能會導致發生錯誤的站台變更建立者 」 從連結的 URL`http://localhost:xxxx/Movies/Details/1`為類似`http://localhost:xxxx/Movies/Details/12345`（或其他值並不代表實際的影片）。</span><span class="sxs-lookup"><span data-stu-id="28edc-115">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="28edc-116">如果您不會檢查 null 的影片，null 電影會導致資料庫錯誤。</span><span class="sxs-lookup"><span data-stu-id="28edc-116">If you did not check for a null movie, a null movie would result in a database error.</span></span>

<span data-ttu-id="28edc-117">檢查 `Delete` 和 `DeleteConfirmed` 方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-117">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

<span data-ttu-id="28edc-118">請注意，`HTTP Get``Delete`方法並不會刪除指定的影片，它會傳回電影的檢視，您可以提交 (`HttpPost`) 刪除...</span><span class="sxs-lookup"><span data-stu-id="28edc-118">Note that the `HTTP Get``Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (`HttpPost`) the deletion..</span></span> <span data-ttu-id="28edc-119">如果您執行刪除作業以回應 GET 要求 (或是執行相關編輯作業、建立作業或任何會變更資料的其他作業)，則會造成安全性漏洞。</span><span class="sxs-lookup"><span data-stu-id="28edc-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span> <span data-ttu-id="28edc-120">如需詳細資訊，請參閱 Stephen Walther 部落格文章： [ASP.NET MVC 秘訣 #46-請勿使用刪除連結，因為它們會產生安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。</span><span class="sxs-lookup"><span data-stu-id="28edc-120">For more information about this, see Stephen Walther's blog entry [ASP.NET MVC Tip #46 — Don't use Delete Links because they create Security Holes](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span></span>

<span data-ttu-id="28edc-121">我們將可刪除資料的 `HttpPost` 方法命名為 `DeleteConfirmed`，以提供 HTTP POST 方法的唯一簽章或名稱。</span><span class="sxs-lookup"><span data-stu-id="28edc-121">The `HttpPost` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="28edc-122">這兩個方法簽章如下所示：</span><span class="sxs-lookup"><span data-stu-id="28edc-122">The two method signatures are shown below:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

<span data-ttu-id="28edc-123">通用語言執行平台 (CLR) 需要多載方法，以提供唯一的參數簽章 (方法名稱相同但參數清單不同)。</span><span class="sxs-lookup"><span data-stu-id="28edc-123">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="28edc-124">不過，您需要在這裡兩種刪除方法-一個用於 GET-，另一個用於 POST 都具有相同的參數簽章。</span><span class="sxs-lookup"><span data-stu-id="28edc-124">However, here you need two Delete methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="28edc-125">(它們都需要接受單一整數作為參數)。</span><span class="sxs-lookup"><span data-stu-id="28edc-125">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="28edc-126">若要排序時，您可以執行兩件事。</span><span class="sxs-lookup"><span data-stu-id="28edc-126">To sort this out, you can do a couple of things.</span></span> <span data-ttu-id="28edc-127">其中一個是讓方法不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="28edc-127">One is to give the methods different names.</span></span> <span data-ttu-id="28edc-128">這是 scaffolding 機制在上述範例採取的方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-128">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="28edc-129">不過，這麼做會導致一個小問題：ASP.NET 會依名稱將 URL 區段與動作方法對應，一旦您重新命名方法，路由通常就會找不到這個方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-129">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="28edc-130">解決辦法正如您看到的這個範例：將 `ActionName("Delete")` 屬性新增至 `DeleteConfirmed` 方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-130">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="28edc-131">這會有效地執行對應的路由系統，讓 URL 包含<em>/Delete/</em>POST 要求會發現`DeleteConfirmed`方法。</span><span class="sxs-lookup"><span data-stu-id="28edc-131">This effectively performs mapping for the routing system so that a URL that includes <em>/Delete/</em>for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="28edc-132">若要避免具有相同名稱和簽章的方法有問題的另一個常見方法是人工方式變更為包含未使用的參數 POST 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="28edc-132">Another common way to avoid a problem with methods that have identical names and signatures is to artificially change the signature of the POST method to include an unused parameter.</span></span> <span data-ttu-id="28edc-133">比方說，有些開發人員將參數類型`FormCollection`傳遞至 POST 方法，然後只要不使用參數：</span><span class="sxs-lookup"><span data-stu-id="28edc-133">For example, some developers add a parameter type `FormCollection` that is passed to the POST method, and then simply don't use the parameter:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a><span data-ttu-id="28edc-134">總結</span><span class="sxs-lookup"><span data-stu-id="28edc-134">Summary</span></span>

<span data-ttu-id="28edc-135">現在，您會有完整的 ASP.NET MVC 應用程式將資料儲存在本機的 DB 資料庫中。</span><span class="sxs-lookup"><span data-stu-id="28edc-135">You now have a complete ASP.NET MVC application that stores data in a local DB database.</span></span> <span data-ttu-id="28edc-136">您可以建立、 讀取、 更新、 刪除及搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="28edc-136">You can create, read, update, delete, and search for movies.</span></span>

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a><span data-ttu-id="28edc-137">後續步驟</span><span class="sxs-lookup"><span data-stu-id="28edc-137">Next Steps</span></span>

<span data-ttu-id="28edc-138">您已經建置及測試 web 應用程式之後下, 一個步驟是將它提供給其他人透過網際網路使用。</span><span class="sxs-lookup"><span data-stu-id="28edc-138">After you have built and tested a web application, the next step is to make it available to other people to use over the Internet.</span></span> <span data-ttu-id="28edc-139">若要這樣做，您必須將它部署至 web 主控提供者。</span><span class="sxs-lookup"><span data-stu-id="28edc-139">To do that, you have to deploy it to a web hosting provider.</span></span> <span data-ttu-id="28edc-140">Microsoft 提供的免費 web 裝載中的最多 10 個 web sites[免費試用版的 Windows Azure 帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="28edc-140">Microsoft offers free web hosting for up to 10 web sites in a [free Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="28edc-141">我建議您接著執行我的教學課程[將成員資格、 OAuth、 與 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="28edc-141">I suggest you next follow my tutorial [Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to a Windows Azure Web Site](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="28edc-142">絕佳的教學課程是 Tom Dykstra 中繼層級[建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="28edc-142">An excellent tutorial is Tom Dykstra's intermediate-level [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="28edc-143">[Stackoverflow](http://stackoverflow.com/help)和[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)會很大的位置，以詢問的問題。</span><span class="sxs-lookup"><span data-stu-id="28edc-143">[Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are a great places to ask questions.</span></span> <span data-ttu-id="28edc-144">請遵循[我](https://twitter.com/RickAndMSFT)因此我最新的教學課程中的更新可能會發生在 twitter 上。</span><span class="sxs-lookup"><span data-stu-id="28edc-144">Follow [me](https://twitter.com/RickAndMSFT) on twitter so you can get updates on my latest tutorials.</span></span>

<span data-ttu-id="28edc-145">意見反應是 「 歡迎畫面。</span><span class="sxs-lookup"><span data-stu-id="28edc-145">Feedback is welcome.</span></span>

<span data-ttu-id="28edc-146">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="28edc-146">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span></span>  
<span data-ttu-id="28edc-147">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="28edc-147">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="28edc-148">上一步</span><span class="sxs-lookup"><span data-stu-id="28edc-148">Previous</span></span>](adding-validation-to-the-model.md)
