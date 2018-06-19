---
uid: signalr/overview/older-versions/dependency-injection
title: SignalR 中的相依性插入 1.x |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505487"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="1f6bd-102">SignalR 中的相依性插入 1.x</span><span class="sxs-lookup"><span data-stu-id="1f6bd-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="1f6bd-103">由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1f6bd-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="1f6bd-104">相依性插入是以移除物件，以更輕鬆，來取代物件的相依性，針對測試 （使用模擬物件），或變更執行階段行為之間的硬式編碼相依性的方法。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="1f6bd-105">本教學課程會示範如何執行相依性插入 SignalR 中樞上。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="1f6bd-106">它也會示範如何使用 SignalR IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="1f6bd-107">相依性插入的一般架構 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="1f6bd-108">相依性插入是什麼？</span><span class="sxs-lookup"><span data-stu-id="1f6bd-108">What is Dependency Injection?</span></span>

<span data-ttu-id="1f6bd-109">如果您已經熟悉相依性插入，請略過本節。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="1f6bd-110">*相依性插入*(DI) 是一種模式的物件不負責建立自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="1f6bd-111">以下是我們 DI 的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="1f6bd-112">假設您有需要記錄訊息的物件。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="1f6bd-113">您可以定義記錄介面：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="1f6bd-114">在您的物件，您可以建立`ILogger`記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="1f6bd-115">這麼做，但它不是最佳設計。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-115">This works, but it's not the best design.</span></span> <span data-ttu-id="1f6bd-116">如果您想要取代`FileLogger`與另一個`ILogger`實作中，您就必須修改`SomeComponent`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="1f6bd-117">內許多其他物件使用`FileLogger`，您必須變更它們全部。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="1f6bd-118">或如果您決定是否要進行`FileLogger`單一值，您還需要進行整個應用程式的變更。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="1f6bd-119">更好的方法是 「 插入 」`ILogger`的物件，例如，藉由使用建構函式引數：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="1f6bd-120">現在物件不是負責選取其中`ILogger`使用。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="1f6bd-121">您可以 swich`ILogger`實作，而不需要變更依存於此物件。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="1f6bd-122">此模式呼叫[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="1f6bd-123">另一個模式是 setter 資料隱碼，您用來設定透過 setter 方法或屬性的相依性。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="1f6bd-124">簡單的相依性插入 SignalR 中</span><span class="sxs-lookup"><span data-stu-id="1f6bd-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="1f6bd-125">交談應用程式，從教學課程，請考慮[入門 SignalR](../getting-started/tutorial-getting-started-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="1f6bd-126">以下是來自該應用程式的中樞類別：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="1f6bd-127">假設您想要儲存在伺服器上的交談訊息之前傳送。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="1f6bd-128">您可能會定義一個介面來擷取這項功能，並將介面使用 DI`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="1f6bd-129">唯一的問題是，SignalR 應用程式並不直接建立中樞;SignalR 為您建立它們。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="1f6bd-130">根據預設，SignalR 預期中樞類別具有無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="1f6bd-131">不過，您可以輕鬆地註冊函式來建立中樞執行個體，及使用這個函數來執行 DI。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="1f6bd-132">註冊此函數，藉由呼叫**GlobalHost.DependencyResolver.Register**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="1f6bd-133">SignalR 會叫用此匿名函式，每當需要建立現在`ChatHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="1f6bd-134">IoC 容器</span><span class="sxs-lookup"><span data-stu-id="1f6bd-134">IoC Containers</span></span>

<span data-ttu-id="1f6bd-135">先前的程式碼，就可以簡單的案例。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="1f6bd-136">但您還是必須撰寫這：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="1f6bd-137">在具有許多相依性的複雜應用程式，您可能需要撰寫大量的這個 「 情節 」 程式碼。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="1f6bd-138">此程式碼可能難以維護，特別是當巢狀相依性。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="1f6bd-139">此外，也難以單元測試。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-139">It is also hard to unit test.</span></span>

<span data-ttu-id="1f6bd-140">其中一種解決方案是使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="1f6bd-141">IoC 容器是負責管理相依性的軟體元件。您使用容器，註冊型別，並將容器來建立物件。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="1f6bd-142">容器會自動找出的相依性關聯性。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="1f6bd-143">許多 IoC 容器也可讓您控制等物件存留期範圍。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="1f6bd-144">"IoC 「 代表 」 的控制項反轉 」，這是一般模式架構會呼叫應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="1f6bd-145">IoC 容器建構您的物件，其中"反轉 」 一般控制流程。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="1f6bd-146">SignalR 中使用 IoC 容器</span><span class="sxs-lookup"><span data-stu-id="1f6bd-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="1f6bd-147">交談的應用程式可能是過於簡單受益於 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="1f6bd-148">相反地，讓我們看看[StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample)範例。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="1f6bd-149">StockTicker 範例會定義兩個主要類別：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="1f6bd-150">`StockTickerHub`： 管理用戶端連線中樞類別。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="1f6bd-151">`StockTicker`： 單一保存股票價格並定期更新它們。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="1f6bd-152">`StockTickerHub`保存的參考，`StockTicker`單一值，而`StockTicker`保存的參考， **IHubConnectionContext**如`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="1f6bd-153">它會使用這個介面來與通訊`StockTickerHub`執行個體。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="1f6bd-154">(如需詳細資訊，請參閱[伺服器廣播透過 ASP.NET SignalR](index.md)。)</span><span class="sxs-lookup"><span data-stu-id="1f6bd-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="1f6bd-155">若要隔離這些相依性的位元，我們可以使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="1f6bd-156">首先，讓我們來簡化`StockTickerHub`和`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="1f6bd-157">下列程式碼中，我已標記為註解組件，我們不需要。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="1f6bd-158">移除從無參數建構函式`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="1f6bd-159">相反地，我們將一律使用 DI 建立中樞中。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="1f6bd-160">對於 StockTicker，移除單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="1f6bd-161">更新版本中，我們將使用 IoC 容器控制 StockTicker 存留期。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="1f6bd-162">此外，請在建構函式公開。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="1f6bd-163">接下來，我們可以重構程式碼所建立的介面`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="1f6bd-164">我們將使用此介面來分離`StockTickerHub`從`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="1f6bd-165">Visual Studio 可讓此種類型的重構很容易。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="1f6bd-166">開啟檔案 StockTicker.cs、 以滑鼠右鍵按一下`StockTicker`類別宣告，並選取**重構**...**擷取介面**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="1f6bd-167">在**擷取介面**] 對話方塊中，按一下 [**全選**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="1f6bd-168">保留其他預設值。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-168">Leave the other defaults.</span></span> <span data-ttu-id="1f6bd-169">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="1f6bd-170">Visual Studio 會建立名為的新介面`IStockTicker`，也會變更`StockTicker`衍生自`IStockTicker`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="1f6bd-171">開啟檔案 IStockTicker.cs 和變更的介面**公用**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="1f6bd-172">在`StockTickerHub`類別中變更兩個執行個體`StockTicker`至`IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="1f6bd-173">建立`IStockTicker`介面不是絕對必要，但我想顯示如何 DI 可協助減少在您的應用程式元件之間的結合。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="1f6bd-174">加入 Ninject 程式庫</span><span class="sxs-lookup"><span data-stu-id="1f6bd-174">Add the Ninject Library</span></span>

<span data-ttu-id="1f6bd-175">有許多適用於.NET 的開放原始碼 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="1f6bd-176">此教學課程中，我將會使用[Ninject](http://www.ninject.org/)。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="1f6bd-177">(其他常用的程式庫包含[城堡 Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Unity](https://github.com/unitycontainer/unity)，和[StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="1f6bd-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="1f6bd-178">使用 NuGet 套件管理員來安裝[Ninject 文件庫](https://nuget.org/packages/Ninject/3.0.1.10)。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="1f6bd-179">在 Visual Studio 中，從**工具**功能表中選取**程式庫套件管理員** | **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="1f6bd-180">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="1f6bd-181">取代 SignalR 相依性解析程式</span><span class="sxs-lookup"><span data-stu-id="1f6bd-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="1f6bd-182">若要使用 Ninject SignalR 內，建立衍生自類別**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="1f6bd-183">這個類別會覆寫**GetService**和**GetServices**方法**DefaultDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="1f6bd-184">SignalR 呼叫這些方法來建立在執行階段，包括中樞執行個體，以及由 SignalR 內部使用的各種服務的各種物件。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="1f6bd-185">**GetService**方法會建立類型的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="1f6bd-186">覆寫此方法，呼叫 Ninject 核心**TryGet**方法。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="1f6bd-187">如果該方法會傳回 null，切換回預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="1f6bd-188">**GetServices**方法會建立指定類型之物件的集合。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="1f6bd-189">覆寫這個方法，以串連 Ninject 的結果與預設的解析程式的結果。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="1f6bd-190">設定 Ninject 繫結</span><span class="sxs-lookup"><span data-stu-id="1f6bd-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="1f6bd-191">現在我們將使用 Ninject 宣告型別繫結。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="1f6bd-192">開啟檔案 RegisterHubs.cs。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="1f6bd-193">在`RegisterHubs.Start`方法，建立 Ninject 容器，它會呼叫 Ninject*核心*。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="1f6bd-194">建立我們自訂相依性解析程式的執行個體：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="1f6bd-195">建立的繫結`IStockTicker`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="1f6bd-196">此程式碼會指出模式兩件事。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-196">This code is saying two things.</span></span> <span data-ttu-id="1f6bd-197">首先，每當應用程式需要`IStockTicker`，核心應該建立的執行個體`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="1f6bd-198">第二個，`StockTicker`類別應該是建立為單一物件。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="1f6bd-199">Ninject 會建立一個執行個體的物件，並傳回每個要求的相同執行個體。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="1f6bd-200">建立的繫結**IHubConnectionContext** ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="1f6bd-201">此程式碼 creatres 匿名函式會傳回**IHubConnection**。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="1f6bd-202">**WhenInjectedInto**方法會告知使用此函式只有在建立時 Ninject`IStockTicker`執行個體。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="1f6bd-203">原因是，會建立 SignalR **IHubConnectionContext**執行個體內部，而我們不想要覆寫 SignalR 如何建立它們。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="1f6bd-204">此函式僅適用於我們`StockTicker`類別。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="1f6bd-205">傳遞至相依性解析程式**MapHubs**方法：</span><span class="sxs-lookup"><span data-stu-id="1f6bd-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="1f6bd-206">現在 SignalR 會使用在指定的解決器**MapHubs**，而不是預設的解析程式。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="1f6bd-207">以下是完整的程式碼清單`RegisterHubs.Start`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="1f6bd-208">若要執行 Visual Studio StockTicker 應用程式，請按 F5。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="1f6bd-209">在瀏覽器視窗中，瀏覽至`http://localhost:*port*/SignalR.Sample/StockTicker.html`。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="1f6bd-210">應用程式具有完全相同的功能為之前。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="1f6bd-211">(如需說明，請參閱[伺服器廣播透過 ASP.NET SignalR](index.md)。)我們尚未改變行為。剛剛您更輕鬆地測試、 維護及發展的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1f6bd-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
