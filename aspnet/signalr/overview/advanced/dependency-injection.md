---
uid: signalr/overview/advanced/dependency-injection
title: "SignalR 中的相依性插入 |Microsoft 文件"
author: MikeWasson
description: "軟體版本本主題中使用 Visual Studio 2013.NET 4.5 SignalR 第 2 版舊版的此主題的較早版本的相關資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="6fc1d-103">SignalR 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="6fc1d-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="6fc1d-104">由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="6fc1d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6fc1d-105">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="6fc1d-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="6fc1d-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6fc1d-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6fc1d-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6fc1d-107">.NET 4.5</span></span>
> - <span data-ttu-id="6fc1d-108">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="6fc1d-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6fc1d-109">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="6fc1d-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="6fc1d-110">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6fc1d-111">問題和註解</span><span class="sxs-lookup"><span data-stu-id="6fc1d-111">Questions and comments</span></span>
> 
> <span data-ttu-id="6fc1d-112">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6fc1d-113">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="6fc1d-114">相依性插入是以移除物件，以更輕鬆，來取代物件的相依性，針對測試 （使用模擬物件），或變更執行階段行為之間的硬式編碼相依性的方法。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="6fc1d-115">本教學課程會示範如何執行相依性插入 SignalR 中樞上。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="6fc1d-116">它也會示範如何使用 SignalR IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="6fc1d-117">相依性插入的一般架構 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="6fc1d-118">相依性插入是什麼？</span><span class="sxs-lookup"><span data-stu-id="6fc1d-118">What is Dependency Injection?</span></span>

<span data-ttu-id="6fc1d-119">如果您已經熟悉相依性插入，請略過本節。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="6fc1d-120">*相依性插入*(DI) 是一種模式的物件不負責建立自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="6fc1d-121">以下是我們 DI 的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="6fc1d-122">假設您有需要記錄訊息的物件。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="6fc1d-123">您可以定義記錄介面：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="6fc1d-124">在您的物件，您可以建立`ILogger`記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="6fc1d-125">這麼做，但它不是最佳設計。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-125">This works, but it's not the best design.</span></span> <span data-ttu-id="6fc1d-126">如果您想要取代`FileLogger`與另一個`ILogger`實作中，您就必須修改`SomeComponent`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="6fc1d-127">內許多其他物件使用`FileLogger`，您必須變更它們全部。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="6fc1d-128">或如果您決定是否要進行`FileLogger`單一值，您還需要進行整個應用程式的變更。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="6fc1d-129">更好的方法是 「 插入 」`ILogger`的物件，例如，藉由使用建構函式引數：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="6fc1d-130">現在物件不是負責選取其中`ILogger`使用。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="6fc1d-131">您可以 swich`ILogger`實作，而不需要變更依存於此物件。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="6fc1d-132">此模式呼叫[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="6fc1d-133">另一個模式是 setter 資料隱碼，您用來設定透過 setter 方法或屬性的相依性。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="6fc1d-134">簡單的相依性插入 SignalR 中</span><span class="sxs-lookup"><span data-stu-id="6fc1d-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="6fc1d-135">交談應用程式，從教學課程，請考慮[入門 SignalR](../getting-started/tutorial-getting-started-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="6fc1d-136">以下是來自該應用程式的中樞類別：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="6fc1d-137">假設您想要儲存在伺服器上的交談訊息之前傳送。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="6fc1d-138">您可能會定義一個介面來擷取這項功能，並將介面使用 DI`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="6fc1d-139">唯一的問題是，SignalR 應用程式並不直接建立中樞;SignalR 為您建立它們。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="6fc1d-140">根據預設，SignalR 預期中樞類別具有無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="6fc1d-141">不過，您可以輕鬆地註冊函式來建立中樞執行個體，及使用這個函數來執行 DI。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="6fc1d-142">註冊此函數，藉由呼叫**GlobalHost.DependencyResolver.Register**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="6fc1d-143">SignalR 會叫用此匿名函式，每當需要建立現在`ChatHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="6fc1d-144">IoC 容器</span><span class="sxs-lookup"><span data-stu-id="6fc1d-144">IoC Containers</span></span>

<span data-ttu-id="6fc1d-145">先前的程式碼，就可以簡單的案例。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="6fc1d-146">但您還是必須撰寫這：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="6fc1d-147">在具有許多相依性的複雜應用程式，您可能需要撰寫大量的這個 「 情節 」 程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="6fc1d-148">此程式碼可能難以維護，特別是當巢狀相依性。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="6fc1d-149">此外，也難以單元測試。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-149">It is also hard to unit test.</span></span>

<span data-ttu-id="6fc1d-150">其中一種解決方案是使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="6fc1d-151">IoC 容器是負責管理相依性的軟體元件。您使用容器，註冊型別，並將容器來建立物件。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="6fc1d-152">容器會自動找出的相依性關聯性。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="6fc1d-153">許多 IoC 容器也可讓您控制等物件存留期範圍。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc1d-154">"IoC 「 代表 」 的控制項反轉 」，這是一般模式架構會呼叫應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="6fc1d-155">IoC 容器建構您的物件，其中"反轉 」 一般控制流程。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="6fc1d-156">SignalR 中使用 IoC 容器</span><span class="sxs-lookup"><span data-stu-id="6fc1d-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="6fc1d-157">交談的應用程式可能是過於簡單受益於 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="6fc1d-158">相反地，讓我們看看[StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample)範例。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="6fc1d-159">StockTicker 範例會定義兩個主要類別：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="6fc1d-160">`StockTickerHub`： 管理用戶端連線中樞類別。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="6fc1d-161">`StockTicker`： 單一保存股票價格並定期更新它們。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="6fc1d-162">`StockTickerHub`保存的參考，`StockTicker`單一值，而`StockTicker`保存的參考， **IHubConnectionContext**如`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="6fc1d-163">它會使用這個介面來與通訊`StockTickerHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="6fc1d-164">(如需詳細資訊，請參閱[伺服器廣播透過 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)</span><span class="sxs-lookup"><span data-stu-id="6fc1d-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="6fc1d-165">若要隔離這些相依性的位元，我們可以使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="6fc1d-166">首先，讓我們來簡化`StockTickerHub`和`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="6fc1d-167">下列程式碼中，我已標記為註解組件，我們不需要。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="6fc1d-168">移除從無參數建構函式`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="6fc1d-169">相反地，我們將一律使用 DI 建立中樞中。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="6fc1d-170">對於 StockTicker，移除單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="6fc1d-171">更新版本中，我們將使用 IoC 容器控制 StockTicker 存留期。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="6fc1d-172">此外，請在建構函式公開。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="6fc1d-173">接下來，我們可以重構程式碼所建立的介面`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="6fc1d-174">我們將使用此介面來分離`StockTickerHub`從`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="6fc1d-175">Visual Studio 可讓此種類型的重構很容易。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="6fc1d-176">開啟檔案 StockTicker.cs、 以滑鼠右鍵按一下`StockTicker`類別宣告，並選取**重構**...**擷取介面**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="6fc1d-177">在**擷取介面**] 對話方塊中，按一下 [**全選**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="6fc1d-178">保留其他預設值。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-178">Leave the other defaults.</span></span> <span data-ttu-id="6fc1d-179">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="6fc1d-180">Visual Studio 會建立名為的新介面`IStockTicker`，也會變更`StockTicker`衍生自`IStockTicker`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="6fc1d-181">開啟檔案 IStockTicker.cs 和變更的介面**公用**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="6fc1d-182">在`StockTickerHub`類別中變更兩個執行個體`StockTicker`至`IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="6fc1d-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="6fc1d-183">建立`IStockTicker`介面不是絕對必要，但我想顯示如何 DI 可協助減少在您的應用程式元件之間的結合。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="6fc1d-184">加入 Ninject 程式庫</span><span class="sxs-lookup"><span data-stu-id="6fc1d-184">Add the Ninject Library</span></span>

<span data-ttu-id="6fc1d-185">有許多適用於.NET 的開放原始碼 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="6fc1d-186">此教學課程中，我將會使用[Ninject](http://www.ninject.org/)。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="6fc1d-187">(其他常用的程式庫包含[城堡 Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Unity](https://github.com/unitycontainer/unity)，和[StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="6fc1d-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="6fc1d-188">使用 NuGet 套件管理員來安裝[Ninject 文件庫](https://nuget.org/packages/Ninject/3.0.1.10)。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="6fc1d-189">在 Visual Studio 中，從**工具**功能表中選取**程式庫套件管理員** | **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="6fc1d-190">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="6fc1d-191">取代 SignalR 相依性解析程式</span><span class="sxs-lookup"><span data-stu-id="6fc1d-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="6fc1d-192">若要使用 Ninject SignalR 內，建立衍生自類別**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="6fc1d-193">這個類別會覆寫**GetService**和**GetServices**方法**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="6fc1d-194">SignalR 呼叫這些方法來建立在執行階段，包括中樞執行個體，以及由 SignalR 內部使用的各種服務的各種物件。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="6fc1d-195">**GetService**方法會建立類型的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="6fc1d-196">覆寫此方法，呼叫 Ninject 核心**TryGet**方法。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="6fc1d-197">如果該方法會傳回 null，切換回預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="6fc1d-198">**GetServices**方法會建立指定類型之物件的集合。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="6fc1d-199">覆寫這個方法，以串連 Ninject 的結果與預設的解析程式的結果。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="6fc1d-200">設定 Ninject 繫結</span><span class="sxs-lookup"><span data-stu-id="6fc1d-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="6fc1d-201">現在我們將使用 Ninject 宣告型別繫結。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="6fc1d-202">開啟您的應用程式 Startup.cs 類別 (確認您是手動建立依照中的封裝指示`readme.txt`，或將驗證新增至您的專案中建立)。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="6fc1d-203">在`Startup.Configuration`方法，建立 Ninject 容器，它會呼叫 Ninject*核心*。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="6fc1d-204">建立我們自訂相依性解析程式的執行個體：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="6fc1d-205">建立的繫結`IStockTicker`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="6fc1d-206">此程式碼會指出模式兩件事。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-206">This code is saying two things.</span></span> <span data-ttu-id="6fc1d-207">首先，每當應用程式需要`IStockTicker`，核心應該建立的執行個體`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="6fc1d-208">第二個，`StockTicker`類別應該是建立為單一物件。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="6fc1d-209">Ninject 會建立一個執行個體的物件，並傳回每個要求的相同執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="6fc1d-210">建立的繫結**IHubConnectionContext** ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="6fc1d-211">此程式碼 creatres 匿名函式會傳回**IHubConnection**。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="6fc1d-212">**WhenInjectedInto**方法會告知使用此函式只有在建立時 Ninject`IStockTicker`執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="6fc1d-213">原因是，會建立 SignalR **IHubConnectionContext**執行個體內部，而我們不想要覆寫 SignalR 如何建立它們。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="6fc1d-214">此函式僅適用於我們`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="6fc1d-215">傳遞至相依性解析程式**MapSignalR**加中樞組態的方法：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="6fc1d-216">更新 Startup.ConfigureSignalR 類別中的方法範例的啟動新的參數：</span><span class="sxs-lookup"><span data-stu-id="6fc1d-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="6fc1d-217">現在 SignalR 會使用在指定的解決器**MapSignalR**，而不是預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="6fc1d-218">以下是完整的程式碼清單`Startup.Configuration`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="6fc1d-219">若要執行 Visual Studio StockTicker 應用程式，請按 F5。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="6fc1d-220">在瀏覽器視窗中，瀏覽至`http://localhost:*port*/SignalR.Sample/StockTicker.html`。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="6fc1d-221">應用程式具有完全相同的功能為之前。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="6fc1d-222">(如需說明，請參閱[伺服器廣播透過 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)我們尚未改變行為。剛剛您更輕鬆地測試、 維護及發展的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fc1d-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
