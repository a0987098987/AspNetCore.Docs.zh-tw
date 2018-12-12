---
uid: signalr/overview/older-versions/dependency-injection
title: 相依性插入 signalr 1.x |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 2035b3feebfa32dd7ec4d6adf715a7fee5e7b74f
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287352"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="5f6a9-102">相依性插入 signalr 1.x</span><span class="sxs-lookup"><span data-stu-id="5f6a9-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="5f6a9-103">藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5f6a9-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="5f6a9-104">相依性插入是移除硬式編碼物件，方便您將物件的相依性，針對測試 （使用模擬 （mock） 物件），或變更執行階段行為之間的相依性的方法。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="5f6a9-105">本教學課程會示範如何對 SignalR 中樞的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="5f6a9-106">它也會示範如何使用 SignalR 使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="5f6a9-107">IoC 容器是一般架構的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="5f6a9-108">相依性插入是什麼？</span><span class="sxs-lookup"><span data-stu-id="5f6a9-108">What is Dependency Injection?</span></span>

<span data-ttu-id="5f6a9-109">如果您已經熟悉如何使用相依性插入，請略過本節。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="5f6a9-110">*相依性插入*(DI) 是一種模式的物件不負責建立自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="5f6a9-111">以下是激發 DI 的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="5f6a9-112">假設您有需要記錄訊息的物件。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="5f6a9-113">您可以定義記錄介面：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="5f6a9-114">在您的物件，您可以建立`ILogger`記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="5f6a9-115">可以運作，但它不是最好的設計。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-115">This works, but it's not the best design.</span></span> <span data-ttu-id="5f6a9-116">如果您想要取代`FileLogger`彼此`ILogger`實作中，您就必須修改`SomeComponent`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="5f6a9-117">內的其他許多物件所使用`FileLogger`，您必須將所有這些變更。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="5f6a9-118">或如果您決定要`FileLogger`單一值，您也需要變更整個應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="5f6a9-119">更好的方法是 「 插入 」`ILogger`物件 — 例如，藉由使用建構函式引數：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="5f6a9-120">現在物件不是負責選取`ILogger`使用。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="5f6a9-121">您可以 swich`ILogger`而不需要變更依存於此物件的實作。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="5f6a9-122">這個模式稱為[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="5f6a9-123">另一個模式是 setter 資料隱碼攻擊，您用來設定透過 setter 方法或屬性的相依性。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="5f6a9-124">SignalR 中簡單的相依性插入</span><span class="sxs-lookup"><span data-stu-id="5f6a9-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="5f6a9-125">交談應用程式，從教學課程，請考慮[開始使用 SignalR](../getting-started/tutorial-getting-started-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="5f6a9-126">以下是從該應用程式的中樞類別：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="5f6a9-127">假設您想要儲存在伺服器上的交談訊息，然後才傳送。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="5f6a9-128">您可以定義介面，用來擷取這項功能，並使用 DI 插入至介面`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="5f6a9-129">唯一的問題是，SignalR 應用程式並不直接建立中樞;SignalR 會為您建立它們。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="5f6a9-130">根據預設，SignalR 預期中樞類別具有無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="5f6a9-131">不過，您可以輕鬆地註冊函式來建立中樞執行個體，並用此函式執行 DI。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="5f6a9-132">藉由呼叫註冊函式**GlobalHost.DependencyResolver.Register**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="5f6a9-133">現在 SignalR 會叫用此匿名函式，每當它需要建立`ChatHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="5f6a9-134">IoC 容器</span><span class="sxs-lookup"><span data-stu-id="5f6a9-134">IoC Containers</span></span>

<span data-ttu-id="5f6a9-135">先前的程式碼是簡單的情況下正常的。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="5f6a9-136">但您還是必須撰寫下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="5f6a9-137">在具有許多相依性的複雜應用程式，您可能需要撰寫大量的這個 「 連接 」 程式碼。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="5f6a9-138">此程式碼很難維護，尤其是相依性巢狀。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="5f6a9-139">此外，也難以使用單元測試。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-139">It is also hard to unit test.</span></span>

<span data-ttu-id="5f6a9-140">其中一個解決方案是使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="5f6a9-141">IoC 容器是負責管理相依性的軟體元件。您向容器註冊類型，然後再使用 建立物件的 容器。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="5f6a9-142">容器會自動找出相依性關聯性。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="5f6a9-143">許多 IoC 容器也可讓您控制物件存留期和範圍等項目。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="5f6a9-144">「 IoC 」 代表 「 的控制項反轉 」，這是一種架構會呼叫應用程式程式碼的一般模式。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="5f6a9-145">IoC 容器建構您的物件，其中 「 反轉 」 一般控制流程。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="5f6a9-146">使用 signalr 的 IoC 容器</span><span class="sxs-lookup"><span data-stu-id="5f6a9-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="5f6a9-147">交談應用程式可能是太簡單才會受益於 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="5f6a9-148">相反地，讓我們看看[Stockservices.asmx](http://nuget.org/packages/microsoft.aspnet.signalr.sample)範例。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="5f6a9-149">Stockservices.asmx 範例會定義兩個主要類別：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="5f6a9-150">`StockTickerHub`：中樞的類別，用來管理用戶端連線。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="5f6a9-151">`StockTicker`：單一保留股價，並定期更新它們。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="5f6a9-152">`StockTickerHub` 保留的參考`StockTicker`單一值，雖然`StockTicker`保存參考**IHubConnectionContext**的`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="5f6a9-153">它會使用此介面來與通訊`StockTickerHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="5f6a9-154">(如需詳細資訊，請參閱 <<c0> [ 伺服器廣播與 ASP.NET SignalR](index.md)。)</span><span class="sxs-lookup"><span data-stu-id="5f6a9-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="5f6a9-155">若要更隔離這些相依性，我們可以使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="5f6a9-156">首先，讓我們簡化`StockTickerHub`和`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="5f6a9-157">下列程式碼中，我已標記為註解的部分，我們不需要。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="5f6a9-158">移除無參數建構函式從`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="5f6a9-159">相反地，我們一律會使用 DI 建立中樞。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="5f6a9-160">對於 Stockservices.asmx，移除單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="5f6a9-161">稍後，我們將使用 IoC 容器來控制 Stockservices.asmx 存留期。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="5f6a9-162">此外，請建構函式公開。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="5f6a9-163">接下來，我們可以重構程式碼所建立的介面`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="5f6a9-164">我們將使用此介面來分離`StockTickerHub`從`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="5f6a9-165">Visual Studio 可讓這種重構很容易。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="5f6a9-166">開啟檔案 StockTicker.cs，以滑鼠右鍵按一下`StockTicker`類別宣告，然後選取**重構**...**擷取介面**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="5f6a9-167">在 [**擷取介面**] 對話方塊中，按一下**全選**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="5f6a9-168">保留其他預設值。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-168">Leave the other defaults.</span></span> <span data-ttu-id="5f6a9-169">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="5f6a9-170">Visual Studio 會建立名為的新介面`IStockTicker`，也會變更`StockTicker`衍生自`IStockTicker`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="5f6a9-171">開啟檔案 IStockTicker.cs，並變更至介面**公開**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="5f6a9-172">在 `StockTickerHub`類別中變更兩個執行個體`StockTicker`到`IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="5f6a9-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="5f6a9-173">建立`IStockTicker`介面不是絕對必要，但是我想要顯示 DI 如何協助減少在您的應用程式元件之間的結合。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="5f6a9-174">新增 Ninject 程式庫</span><span class="sxs-lookup"><span data-stu-id="5f6a9-174">Add the Ninject Library</span></span>

<span data-ttu-id="5f6a9-175">有許多適用於.NET 的開放原始碼 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="5f6a9-176">本教學課程中，我將使用[Ninject](http://www.ninject.org/)。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="5f6a9-177">(其他熱門的程式庫包含[Castle Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Unity](https://github.com/unitycontainer/unity)，和[StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="5f6a9-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="5f6a9-178">使用 NuGet 套件管理員來安裝[Ninject 文件庫](https://nuget.org/packages/Ninject/3.0.1.10)。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="5f6a9-179">在 Visual Studio 中，從**工具**功能表中，選取**NuGet 套件管理員** > **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-179">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="5f6a9-180">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="5f6a9-181">將 SignalR 相依性解析程式</span><span class="sxs-lookup"><span data-stu-id="5f6a9-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="5f6a9-182">若要使用 Ninject SignalR 內，建立衍生自類別**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="5f6a9-183">此類別會覆寫**GetService**並**GetServices**方法**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="5f6a9-184">SignalR 呼叫這些方法來建立在執行階段，包括中樞執行個體，以及各種服務供內部使用 SignalR 的各種物件。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="5f6a9-185">**GetService**方法會建立類型的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="5f6a9-186">覆寫此方法以呼叫 Ninject 核心**TryGet**方法。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="5f6a9-187">如果該方法會傳回 null，切換回預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="5f6a9-188">**GetServices**方法會建立指定型別的物件的集合。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="5f6a9-189">覆寫這個方法，以串連結果 Ninject 與預設的解析程式的結果。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="5f6a9-190">設定 Ninject 繫結</span><span class="sxs-lookup"><span data-stu-id="5f6a9-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="5f6a9-191">現在我們將使用 Ninject 宣告型別繫結。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="5f6a9-192">開啟檔案 RegisterHubs.cs。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="5f6a9-193">在 `RegisterHubs.Start`方法，建立 Ninject 容器，它會呼叫 Ninject*核心*。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="5f6a9-194">建立我們的自訂相依性解析程式的執行個體：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="5f6a9-195">建立的繫結`IStockTicker`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="5f6a9-196">此程式碼會指出兩件事。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-196">This code is saying two things.</span></span> <span data-ttu-id="5f6a9-197">首先，每當應用程式需要`IStockTicker`，核心應該建立的執行個體`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="5f6a9-198">第二個，`StockTicker`類別應該是建立為單一物件。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="5f6a9-199">Ninject 會建立一個執行個體的物件，並傳回每個要求的相同執行個體。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="5f6a9-200">建立的繫結**IHubConnectionContext** ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="5f6a9-201">此程式碼 creatres 匿名函式會傳回**IHubConnection**。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="5f6a9-202">**WhenInjectedInto**方法會告訴只有在建立時，才使用這個函式的 Ninject`IStockTicker`執行個體。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="5f6a9-203">原因是，會建立 SignalR **IHubConnectionContext**執行個體就內部而言，而且我們不想要覆寫如何 SignalR 便會建立它們。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="5f6a9-204">此函式僅適用於我們`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="5f6a9-205">傳遞至相依性解析程式**MapHubs**方法：</span><span class="sxs-lookup"><span data-stu-id="5f6a9-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="5f6a9-206">SignalR 將使用在指定的解決器現在**MapHubs**，而不是預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="5f6a9-207">以下是完整的程式碼清單`RegisterHubs.Start`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="5f6a9-208">若要在 Visual Studio 中執行 Stockservices.asmx 應用程式，請按 F5。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="5f6a9-209">在瀏覽器視窗中，瀏覽至`http://localhost:*port*/SignalR.Sample/StockTicker.html`。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="5f6a9-210">應用程式有功能仍然與之前完全相同。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="5f6a9-211">(如需說明，請參閱[伺服器廣播與 ASP.NET SignalR](index.md)。)我們尚未變更行為。剛才您更輕鬆地測試、 維護及發展的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5f6a9-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
