---
uid: signalr/overview/advanced/dependency-injection
title: SignalR 中的相依性插入 |Microsoft Docs
author: MikeWasson
description: 軟體使用本主題的 Visual Studio 2013.NET 4.5 SignalR 本主題的第 2 版上一個版本的版本較早版本的相關資訊...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 2f9b8eeb87882a686df5f35b2e7048a8518c8d4b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831191"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="96aa9-103">SignalR 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="96aa9-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="96aa9-104">藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="96aa9-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="96aa9-105">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="96aa9-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="96aa9-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="96aa9-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="96aa9-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="96aa9-107">.NET 4.5</span></span>
> - <span data-ttu-id="96aa9-108">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="96aa9-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="96aa9-109">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="96aa9-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="96aa9-110">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="96aa9-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="96aa9-111">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="96aa9-111">Questions and comments</span></span>
> 
> <span data-ttu-id="96aa9-112">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="96aa9-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="96aa9-113">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="96aa9-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="96aa9-114">相依性插入是移除硬式編碼物件，方便您將物件的相依性，針對測試 （使用模擬 （mock） 物件），或變更執行階段行為之間的相依性的方法。</span><span class="sxs-lookup"><span data-stu-id="96aa9-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="96aa9-115">本教學課程會示範如何對 SignalR 中樞的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="96aa9-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="96aa9-116">它也會示範如何使用 SignalR 使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="96aa9-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="96aa9-117">IoC 容器是一般架構的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="96aa9-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="96aa9-118">相依性插入是什麼？</span><span class="sxs-lookup"><span data-stu-id="96aa9-118">What is Dependency Injection?</span></span>

<span data-ttu-id="96aa9-119">如果您已經熟悉如何使用相依性插入，請略過本節。</span><span class="sxs-lookup"><span data-stu-id="96aa9-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="96aa9-120">*相依性插入*(DI) 是一種模式的物件不負責建立自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="96aa9-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="96aa9-121">以下是激發 DI 的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="96aa9-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="96aa9-122">假設您有需要記錄訊息的物件。</span><span class="sxs-lookup"><span data-stu-id="96aa9-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="96aa9-123">您可以定義記錄介面：</span><span class="sxs-lookup"><span data-stu-id="96aa9-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="96aa9-124">在您的物件，您可以建立`ILogger`記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="96aa9-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="96aa9-125">可以運作，但它不是最好的設計。</span><span class="sxs-lookup"><span data-stu-id="96aa9-125">This works, but it's not the best design.</span></span> <span data-ttu-id="96aa9-126">如果您想要取代`FileLogger`彼此`ILogger`實作中，您就必須修改`SomeComponent`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="96aa9-127">內的其他許多物件所使用`FileLogger`，您必須將所有這些變更。</span><span class="sxs-lookup"><span data-stu-id="96aa9-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="96aa9-128">或如果您決定要`FileLogger`單一值，您也需要變更整個應用程式。</span><span class="sxs-lookup"><span data-stu-id="96aa9-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="96aa9-129">更好的方法是 「 插入 」`ILogger`物件 — 例如，藉由使用建構函式引數：</span><span class="sxs-lookup"><span data-stu-id="96aa9-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="96aa9-130">現在物件不是負責選取`ILogger`使用。</span><span class="sxs-lookup"><span data-stu-id="96aa9-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="96aa9-131">您可以 swich`ILogger`而不需要變更依存於此物件的實作。</span><span class="sxs-lookup"><span data-stu-id="96aa9-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="96aa9-132">這個模式稱為[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。</span><span class="sxs-lookup"><span data-stu-id="96aa9-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="96aa9-133">另一個模式是 setter 資料隱碼攻擊，您用來設定透過 setter 方法或屬性的相依性。</span><span class="sxs-lookup"><span data-stu-id="96aa9-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="96aa9-134">SignalR 中簡單的相依性插入</span><span class="sxs-lookup"><span data-stu-id="96aa9-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="96aa9-135">交談應用程式，從教學課程，請考慮[開始使用 SignalR](../getting-started/tutorial-getting-started-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="96aa9-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="96aa9-136">以下是從該應用程式的中樞類別：</span><span class="sxs-lookup"><span data-stu-id="96aa9-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="96aa9-137">假設您想要儲存在伺服器上的交談訊息，然後才傳送。</span><span class="sxs-lookup"><span data-stu-id="96aa9-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="96aa9-138">您可以定義介面，用來擷取這項功能，並使用 DI 插入至介面`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="96aa9-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="96aa9-139">唯一的問題是，SignalR 應用程式並不直接建立中樞;SignalR 會為您建立它們。</span><span class="sxs-lookup"><span data-stu-id="96aa9-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="96aa9-140">根據預設，SignalR 預期中樞類別具有無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="96aa9-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="96aa9-141">不過，您可以輕鬆地註冊函式來建立中樞執行個體，並用此函式執行 DI。</span><span class="sxs-lookup"><span data-stu-id="96aa9-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="96aa9-142">藉由呼叫註冊函式**GlobalHost.DependencyResolver.Register**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="96aa9-143">現在 SignalR 會叫用此匿名函式，每當它需要建立`ChatHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="96aa9-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="96aa9-144">IoC 容器</span><span class="sxs-lookup"><span data-stu-id="96aa9-144">IoC Containers</span></span>

<span data-ttu-id="96aa9-145">先前的程式碼是簡單的情況下正常的。</span><span class="sxs-lookup"><span data-stu-id="96aa9-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="96aa9-146">但您還是必須撰寫下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="96aa9-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="96aa9-147">在具有許多相依性的複雜應用程式，您可能需要撰寫大量的這個 「 連接 」 程式碼。</span><span class="sxs-lookup"><span data-stu-id="96aa9-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="96aa9-148">此程式碼很難維護，尤其是相依性巢狀。</span><span class="sxs-lookup"><span data-stu-id="96aa9-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="96aa9-149">此外，也難以使用單元測試。</span><span class="sxs-lookup"><span data-stu-id="96aa9-149">It is also hard to unit test.</span></span>

<span data-ttu-id="96aa9-150">其中一個解決方案是使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="96aa9-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="96aa9-151">IoC 容器是負責管理相依性的軟體元件。您向容器註冊類型，然後再使用 建立物件的 容器。</span><span class="sxs-lookup"><span data-stu-id="96aa9-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="96aa9-152">容器會自動找出相依性關聯性。</span><span class="sxs-lookup"><span data-stu-id="96aa9-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="96aa9-153">許多 IoC 容器也可讓您控制物件存留期和範圍等項目。</span><span class="sxs-lookup"><span data-stu-id="96aa9-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="96aa9-154">「 IoC 」 代表 「 的控制項反轉 」，這是一種架構會呼叫應用程式程式碼的一般模式。</span><span class="sxs-lookup"><span data-stu-id="96aa9-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="96aa9-155">IoC 容器建構您的物件，其中 「 反轉 」 一般控制流程。</span><span class="sxs-lookup"><span data-stu-id="96aa9-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="96aa9-156">使用 signalr 的 IoC 容器</span><span class="sxs-lookup"><span data-stu-id="96aa9-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="96aa9-157">交談應用程式可能是太簡單才會受益於 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="96aa9-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="96aa9-158">相反地，讓我們看看[Stockservices.asmx](http://nuget.org/packages/microsoft.aspnet.signalr.sample)範例。</span><span class="sxs-lookup"><span data-stu-id="96aa9-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="96aa9-159">Stockservices.asmx 範例會定義兩個主要類別：</span><span class="sxs-lookup"><span data-stu-id="96aa9-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="96aa9-160">`StockTickerHub`： 管理用戶端連線 hub 類別。</span><span class="sxs-lookup"><span data-stu-id="96aa9-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="96aa9-161">`StockTicker`: 保存股票的價格，而且會定期更新它們的 singleton。</span><span class="sxs-lookup"><span data-stu-id="96aa9-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="96aa9-162">`StockTickerHub` 保留的參考`StockTicker`單一值，雖然`StockTicker`保存參考**IHubConnectionContext**的`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="96aa9-163">它會使用此介面來與通訊`StockTickerHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="96aa9-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="96aa9-164">(如需詳細資訊，請參閱 <<c0> [ 伺服器廣播與 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)</span><span class="sxs-lookup"><span data-stu-id="96aa9-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="96aa9-165">若要更隔離這些相依性，我們可以使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="96aa9-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="96aa9-166">首先，讓我們簡化`StockTickerHub`和`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="96aa9-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="96aa9-167">下列程式碼中，我已標記為註解的部分，我們不需要。</span><span class="sxs-lookup"><span data-stu-id="96aa9-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="96aa9-168">移除無參數建構函式從`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="96aa9-169">相反地，我們一律會使用 DI 建立中樞。</span><span class="sxs-lookup"><span data-stu-id="96aa9-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="96aa9-170">對於 Stockservices.asmx，移除單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="96aa9-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="96aa9-171">稍後，我們將使用 IoC 容器來控制 Stockservices.asmx 存留期。</span><span class="sxs-lookup"><span data-stu-id="96aa9-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="96aa9-172">此外，請建構函式公開。</span><span class="sxs-lookup"><span data-stu-id="96aa9-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="96aa9-173">接下來，我們可以重構程式碼所建立的介面`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="96aa9-174">我們將使用此介面來分離`StockTickerHub`從`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="96aa9-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="96aa9-175">Visual Studio 可讓這種重構很容易。</span><span class="sxs-lookup"><span data-stu-id="96aa9-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="96aa9-176">開啟檔案 StockTicker.cs，以滑鼠右鍵按一下`StockTicker`類別宣告，然後選取**重構**...**擷取介面**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="96aa9-177">在 [**擷取介面**] 對話方塊中，按一下**全選**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="96aa9-178">保留其他預設值。</span><span class="sxs-lookup"><span data-stu-id="96aa9-178">Leave the other defaults.</span></span> <span data-ttu-id="96aa9-179">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="96aa9-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="96aa9-180">Visual Studio 會建立名為的新介面`IStockTicker`，也會變更`StockTicker`衍生自`IStockTicker`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="96aa9-181">開啟檔案 IStockTicker.cs，並變更至介面**公開**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="96aa9-182">在 `StockTickerHub`類別中變更兩個執行個體`StockTicker`到`IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="96aa9-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="96aa9-183">建立`IStockTicker`介面不是絕對必要，但是我想要顯示 DI 如何協助減少在您的應用程式元件之間的結合。</span><span class="sxs-lookup"><span data-stu-id="96aa9-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="96aa9-184">新增 Ninject 程式庫</span><span class="sxs-lookup"><span data-stu-id="96aa9-184">Add the Ninject Library</span></span>

<span data-ttu-id="96aa9-185">有許多適用於.NET 的開放原始碼 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="96aa9-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="96aa9-186">本教學課程中，我將使用[Ninject](http://www.ninject.org/)。</span><span class="sxs-lookup"><span data-stu-id="96aa9-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="96aa9-187">(其他熱門的程式庫包含[Castle Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Unity](https://github.com/unitycontainer/unity)，和[StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="96aa9-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="96aa9-188">使用 NuGet 套件管理員來安裝[Ninject 文件庫](https://nuget.org/packages/Ninject/3.0.1.10)。</span><span class="sxs-lookup"><span data-stu-id="96aa9-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="96aa9-189">在 Visual Studio 中，從**工具**功能表中，選取**程式庫套件管理員** | **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="96aa9-190">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="96aa9-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="96aa9-191">將 SignalR 相依性解析程式</span><span class="sxs-lookup"><span data-stu-id="96aa9-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="96aa9-192">若要使用 Ninject SignalR 內，建立衍生自類別**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="96aa9-193">此類別會覆寫**GetService**並**GetServices**方法**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="96aa9-194">SignalR 呼叫這些方法來建立在執行階段，包括中樞執行個體，以及各種服務供內部使用 SignalR 的各種物件。</span><span class="sxs-lookup"><span data-stu-id="96aa9-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="96aa9-195">**GetService**方法會建立類型的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="96aa9-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="96aa9-196">覆寫此方法以呼叫 Ninject 核心**TryGet**方法。</span><span class="sxs-lookup"><span data-stu-id="96aa9-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="96aa9-197">如果該方法會傳回 null，切換回預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="96aa9-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="96aa9-198">**GetServices**方法會建立指定型別的物件的集合。</span><span class="sxs-lookup"><span data-stu-id="96aa9-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="96aa9-199">覆寫這個方法，以串連結果 Ninject 與預設的解析程式的結果。</span><span class="sxs-lookup"><span data-stu-id="96aa9-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="96aa9-200">設定 Ninject 繫結</span><span class="sxs-lookup"><span data-stu-id="96aa9-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="96aa9-201">現在我們將使用 Ninject 宣告型別繫結。</span><span class="sxs-lookup"><span data-stu-id="96aa9-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="96aa9-202">開啟您的應用程式的 Startup.cs 類別 (您是建立以手動方式根據套件的指示在`readme.txt`，或將驗證新增至您的專案中建立)。</span><span class="sxs-lookup"><span data-stu-id="96aa9-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="96aa9-203">在 `Startup.Configuration`方法，建立 Ninject 容器，它會呼叫 Ninject*核心*。</span><span class="sxs-lookup"><span data-stu-id="96aa9-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="96aa9-204">建立我們的自訂相依性解析程式的執行個體：</span><span class="sxs-lookup"><span data-stu-id="96aa9-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="96aa9-205">建立的繫結`IStockTicker`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="96aa9-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="96aa9-206">此程式碼會指出兩件事。</span><span class="sxs-lookup"><span data-stu-id="96aa9-206">This code is saying two things.</span></span> <span data-ttu-id="96aa9-207">首先，每當應用程式需要`IStockTicker`，核心應該建立的執行個體`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="96aa9-208">第二個，`StockTicker`類別應該是建立為單一物件。</span><span class="sxs-lookup"><span data-stu-id="96aa9-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="96aa9-209">Ninject 會建立一個執行個體的物件，並傳回每個要求的相同執行個體。</span><span class="sxs-lookup"><span data-stu-id="96aa9-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="96aa9-210">建立的繫結**IHubConnectionContext** ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="96aa9-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="96aa9-211">此程式碼 creatres 匿名函式會傳回**IHubConnection**。</span><span class="sxs-lookup"><span data-stu-id="96aa9-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="96aa9-212">**WhenInjectedInto**方法會告訴只有在建立時，才使用這個函式的 Ninject`IStockTicker`執行個體。</span><span class="sxs-lookup"><span data-stu-id="96aa9-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="96aa9-213">原因是，會建立 SignalR **IHubConnectionContext**執行個體就內部而言，而且我們不想要覆寫如何 SignalR 便會建立它們。</span><span class="sxs-lookup"><span data-stu-id="96aa9-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="96aa9-214">此函式僅適用於我們`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="96aa9-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="96aa9-215">傳遞至相依性解析程式**MapSignalR**藉由新增中樞設定的方法：</span><span class="sxs-lookup"><span data-stu-id="96aa9-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="96aa9-216">更新 Startup.ConfigureSignalR 方法，在此範例的啟動類別的新參數：</span><span class="sxs-lookup"><span data-stu-id="96aa9-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="96aa9-217">SignalR 將使用在指定的解決器現在**MapSignalR**，而不是預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="96aa9-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="96aa9-218">以下是完整的程式碼清單`Startup.Configuration`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="96aa9-219">若要在 Visual Studio 中執行 Stockservices.asmx 應用程式，請按 F5。</span><span class="sxs-lookup"><span data-stu-id="96aa9-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="96aa9-220">在瀏覽器視窗中，瀏覽至`http://localhost:*port*/SignalR.Sample/StockTicker.html`。</span><span class="sxs-lookup"><span data-stu-id="96aa9-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="96aa9-221">應用程式有功能仍然與之前完全相同。</span><span class="sxs-lookup"><span data-stu-id="96aa9-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="96aa9-222">(如需說明，請參閱[伺服器廣播與 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)我們尚未變更行為。剛才您更輕鬆地測試、 維護及發展的程式碼。</span><span class="sxs-lookup"><span data-stu-id="96aa9-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
