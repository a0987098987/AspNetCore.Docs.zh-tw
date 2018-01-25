---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "ASP.NET MVC 4 相依性插入 |Microsoft 文件"
author: rick-anderson
description: "注意： 這個實際操作實驗室假設您擁有的 ASP.NET MVC 與 ASP.NET MVC 4 的篩選條件的基本知識。 如果您沒有使用 ASP.NET MVC 4 的篩選器之前，我們建議..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 48a7d7fdb670aebb72450fc4eb12a364ef595c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="6fd56-104">ASP.NET MVC 4 相依性插入</span><span class="sxs-lookup"><span data-stu-id="6fd56-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="6fd56-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="6fd56-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="6fd56-106">這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**和**ASP.NET MVC 4 篩選**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="6fd56-107">如果您不使用**ASP.NET MVC 4 篩選**之前，我們建議您在一段**ASP.NET MVC 自訂動作篩選條件**實際操作實驗室。</span><span class="sxs-lookup"><span data-stu-id="6fd56-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="6fd56-108">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<span data-ttu-id="6fd56-109">在**物件導向程式設計**開發架構時，物件一起運作共同作業模型中有 contributors 和取用者。</span><span class="sxs-lookup"><span data-stu-id="6fd56-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="6fd56-110">當然，此通訊模式會產生物件和元件，變得難以管理複雜度增加之間的相依性。</span><span class="sxs-lookup"><span data-stu-id="6fd56-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="6fd56-111">![相依性的類別和型號複雜性](aspnet-mvc-4-dependency-injection/_static/image1.png "類別相依性，並建立模型的複雜度")</span><span class="sxs-lookup"><span data-stu-id="6fd56-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="6fd56-112">*類別相依性和模型的複雜度*</span><span class="sxs-lookup"><span data-stu-id="6fd56-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="6fd56-113">您可能聽過**原廠模式**和介面和實作使用服務、 用戶端物件通常負責服務位置之間的分離。</span><span class="sxs-lookup"><span data-stu-id="6fd56-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="6fd56-114">相依性插入模式為的逆轉控制的特定實作。</span><span class="sxs-lookup"><span data-stu-id="6fd56-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="6fd56-115">**逆轉控制 (IoC)**表示物件不會建立其他物件，它們所依賴執行其工作。</span><span class="sxs-lookup"><span data-stu-id="6fd56-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="6fd56-116">相反地，他們會收到需要從外部來源 （例如 xml 組態檔） 的物件。</span><span class="sxs-lookup"><span data-stu-id="6fd56-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="6fd56-117">**相依性插入 (DI)**表示這是由物件介入通常會將建構函式參數傳遞的 framework 元件並設定屬性。</span><span class="sxs-lookup"><span data-stu-id="6fd56-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="6fd56-118">相依性插入 (DI) 設計模式</span><span class="sxs-lookup"><span data-stu-id="6fd56-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="6fd56-119">在高層級，相依性插入的目標是，用戶端類別 (例如*高爾夫球*) 必須符合介面的項目 (例如*IClub*)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="6fd56-120">它不在意具象型別為何 (例如*WoodClub，IronClub，WedgeClub*或*PutterClub*)，它想要其他人來處理這種情況 (例如更佳*caddy*)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="6fd56-121">ASP.NET MVC 中的相依性解析程式可讓您註冊您的相依性邏輯其他地方 (例如容器或*梅花包*)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="6fd56-122">![相依性插入圖表](aspnet-mvc-4-dependency-injection/_static/image2.png "相依性插入圖")</span><span class="sxs-lookup"><span data-stu-id="6fd56-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="6fd56-123">*相依性插入-包括業餘高爾夫比喻*</span><span class="sxs-lookup"><span data-stu-id="6fd56-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="6fd56-124">使用相依性插入模式和逆轉控制的優點如下：</span><span class="sxs-lookup"><span data-stu-id="6fd56-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="6fd56-125">減少結合類別</span><span class="sxs-lookup"><span data-stu-id="6fd56-125">Reduces class coupling</span></span>
- <span data-ttu-id="6fd56-126">會增加重複使用程式碼</span><span class="sxs-lookup"><span data-stu-id="6fd56-126">Increases code reusing</span></span>
- <span data-ttu-id="6fd56-127">改善程式碼維護性</span><span class="sxs-lookup"><span data-stu-id="6fd56-127">Improves code maintainability</span></span>
- <span data-ttu-id="6fd56-128">改善應用程式測試</span><span class="sxs-lookup"><span data-stu-id="6fd56-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="6fd56-129">相依性插入有時與抽象 Factory 設計模式，但是沒有這兩種方法之間有些微的差異。</span><span class="sxs-lookup"><span data-stu-id="6fd56-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="6fd56-130">DI 已解決相依性呼叫處理站和已註冊的服務使用背後的架構。</span><span class="sxs-lookup"><span data-stu-id="6fd56-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="6fd56-131">既然您了解相依性插入模式時，您將學習在這個實驗室整個套用在 ASP.NET MVC 4 的方式。</span><span class="sxs-lookup"><span data-stu-id="6fd56-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="6fd56-132">您會開始使用中的相依性插入**控制器**来包含的資料庫存取服務。</span><span class="sxs-lookup"><span data-stu-id="6fd56-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="6fd56-133">接下來，您會將套用至相依性插入**檢視**来使用的服務，並顯示資訊。</span><span class="sxs-lookup"><span data-stu-id="6fd56-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="6fd56-134">最後，您就可以擴充 DI 與 ASP.NET MVC 4 的篩選條件，將方案中的自訂動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="6fd56-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="6fd56-135">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="6fd56-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="6fd56-136">將 ASP.NET MVC 4 與 Unity 整合相依性插入使用 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="6fd56-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="6fd56-137">使用 ASP.NET MVC 控制器內的相依性插入</span><span class="sxs-lookup"><span data-stu-id="6fd56-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="6fd56-138">使用 ASP.NET MVC 檢視內的相依性插入</span><span class="sxs-lookup"><span data-stu-id="6fd56-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="6fd56-139">使用 ASP.NET MVC 動作篩選條件內的相依性插入</span><span class="sxs-lookup"><span data-stu-id="6fd56-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="6fd56-140">這個實驗室使用 Unity.Mvc3 NuGet 封裝相依性解析，但可以調整任何相依性插入架構，才能使用 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="6fd56-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="6fd56-141">必要條件</span><span class="sxs-lookup"><span data-stu-id="6fd56-141">Prerequisites</span></span>

<span data-ttu-id="6fd56-142">您必須具備下列項目才能完成本實驗室：</span><span class="sxs-lookup"><span data-stu-id="6fd56-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="6fd56-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="6fd56-144">安裝程式</span><span class="sxs-lookup"><span data-stu-id="6fd56-144">Setup</span></span>

<span data-ttu-id="6fd56-145">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="6fd56-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="6fd56-146">為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="6fd56-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="6fd56-147">若要安裝執行的程式碼片段**.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="6fd56-148">如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。</span><span class="sxs-lookup"><span data-stu-id="6fd56-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="6fd56-149">練習</span><span class="sxs-lookup"><span data-stu-id="6fd56-149">Exercises</span></span>

<span data-ttu-id="6fd56-150">這個實際操作實驗室是由下列練習所組成：</span><span class="sxs-lookup"><span data-stu-id="6fd56-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="6fd56-151">練習 1： 插入控制器</span><span class="sxs-lookup"><span data-stu-id="6fd56-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="6fd56-152">練習 2： 將，以便檢視</span><span class="sxs-lookup"><span data-stu-id="6fd56-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="6fd56-153">練習 3： 將篩選器</span><span class="sxs-lookup"><span data-stu-id="6fd56-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="6fd56-154">每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。</span><span class="sxs-lookup"><span data-stu-id="6fd56-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="6fd56-155">如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。</span><span class="sxs-lookup"><span data-stu-id="6fd56-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="6fd56-156">完成本實驗室估計時間： **30 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="6fd56-157">練習 1： 插入控制器</span><span class="sxs-lookup"><span data-stu-id="6fd56-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="6fd56-158">在此練習中，您將學習如何使用 ASP.NET MVC 控制器中的相依性插入，方法是整合 Unity 使用 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="6fd56-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="6fd56-159">因此，您將服務納入，邏輯與資料存取分開您 MvcMusicStore 控制站。</span><span class="sxs-lookup"><span data-stu-id="6fd56-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="6fd56-160">服務會建立新的相依於控制器建構函式，會使用相依性插入的協助解決**Unity**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="6fd56-161">這個方法會顯示如何產生較少結合的應用程式，也就是更有彈性且容易維護和測試。</span><span class="sxs-lookup"><span data-stu-id="6fd56-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="6fd56-162">您也將學習如何將 ASP.NET MVC 與 Unity 的整合。</span><span class="sxs-lookup"><span data-stu-id="6fd56-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="6fd56-163">關於 StoreManager 服務</span><span class="sxs-lookup"><span data-stu-id="6fd56-163">About StoreManager Service</span></span>

<span data-ttu-id="6fd56-164">現在開始方案中提供 MVC Music Store 包含服務，可管理存放區控制站資料名為**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="6fd56-165">您將在下面找到儲存區服務實作。</span><span class="sxs-lookup"><span data-stu-id="6fd56-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="6fd56-166">請注意，所有的方法會傳回模型的實體。</span><span class="sxs-lookup"><span data-stu-id="6fd56-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="6fd56-167">**StoreController**從 begin 方案現在會消耗**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="6fd56-168">已從移除所有資料參考**StoreController**，現在可以修改目前的資料存取提供者，而不需要變更的任何方法，會消耗和**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="6fd56-169">您會發現在下方**StoreController**實作具有的相依性**StoreService**類別建構函式內。</span><span class="sxs-lookup"><span data-stu-id="6fd56-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="6fd56-170">在這個練習中導入的相依性與**的逆轉控制**(IoC)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="6fd56-171">**StoreController**類別建構函式收到**IStoreService**型別參數，請務必執行從類別內的服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="6fd56-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="6fd56-172">不過， **StoreController**未實作預設建構函式 （不含參數） 的任何控制站必須具有使用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="6fd56-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="6fd56-173">若要解析相依性，控制器具有建立抽象的處理站 （會傳回指定之任何的類型物件類別）。</span><span class="sxs-lookup"><span data-stu-id="6fd56-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="6fd56-174">類別會嘗試建立 StoreController，而不傳送服務物件，因為沒有宣告沒有無參數建構函式時，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fd56-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="6fd56-175">工作 1-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="6fd56-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="6fd56-176">在這個工作中，您將執行 Begin 應用程式，包括入存放區控制站與應用程式邏輯分隔資料存取服務。</span><span class="sxs-lookup"><span data-stu-id="6fd56-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="6fd56-177">當執行應用程式，您會收到例外狀況，控制器服務不會傳遞做為參數預設為：</span><span class="sxs-lookup"><span data-stu-id="6fd56-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="6fd56-178">開啟**開始**方案位於**Source\Ex01 插入 Controller\Begin**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="6fd56-179">您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="6fd56-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6fd56-180">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6fd56-181">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="6fd56-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6fd56-182">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6fd56-183">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="6fd56-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6fd56-184">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6fd56-185">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="6fd56-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="6fd56-186">按**Ctrl + F5**執行應用程式，但不偵錯。</span><span class="sxs-lookup"><span data-stu-id="6fd56-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="6fd56-187">您會收到錯誤訊息&quot;**沒有為這個物件定義的無參數建構函式**&quot;:</span><span class="sxs-lookup"><span data-stu-id="6fd56-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="6fd56-188">![執行 ASP.NET MVC 開始應用程式時發生錯誤](aspnet-mvc-4-dependency-injection/_static/image3.png "執行 ASP.NET MVC 開始應用程式時發生錯誤")</span><span class="sxs-lookup"><span data-stu-id="6fd56-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="6fd56-189">*執行 ASP.NET MVC 開始應用程式時發生錯誤*</span><span class="sxs-lookup"><span data-stu-id="6fd56-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="6fd56-190">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6fd56-190">Close the browser.</span></span>

<span data-ttu-id="6fd56-191">下列步驟中，您可以使用 [音樂] 存放區方案，將此控制站需要的相依性。</span><span class="sxs-lookup"><span data-stu-id="6fd56-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="6fd56-192">工作 2-包括 Unity MvcMusicStore 解決方案</span><span class="sxs-lookup"><span data-stu-id="6fd56-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="6fd56-193">在這項工作，您會將包含**Unity.Mvc3**至方案的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="6fd56-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="6fd56-194">ASP.NET MVC 3，設計 Unity.Mvc3 封裝，但與 ASP.NET MVC 4 完全相容。</span><span class="sxs-lookup"><span data-stu-id="6fd56-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="6fd56-195">Unity 執行個體是選擇性支援的輕量型、 可延伸的相依性插入容器，然後輸入攔截。</span><span class="sxs-lookup"><span data-stu-id="6fd56-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="6fd56-196">它是任何類型的.NET 應用程式中使用的一般用途容器。</span><span class="sxs-lookup"><span data-stu-id="6fd56-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="6fd56-197">它提供包括相依性插入機制中找到的所有一般功能： 建立物件，藉由指定相依性，在執行階段和彈性，延後至容器的元件組態需求的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="6fd56-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="6fd56-198">安裝**Unity.Mvc3**中 NuGet 套件**MvcMusicStore**專案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="6fd56-199">若要這樣做，請開啟**Package Manager Console**從**檢視** | **其他視窗**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="6fd56-200">執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="6fd56-200">Run the following command.</span></span>

    <span data-ttu-id="6fd56-201">PMC</span><span class="sxs-lookup"><span data-stu-id="6fd56-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="6fd56-202">![正在安裝 Unity.Mvc3 NuGet 封裝](aspnet-mvc-4-dependency-injection/_static/image4.png "安裝 Unity.Mvc3 NuGet 封裝")</span><span class="sxs-lookup"><span data-stu-id="6fd56-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="6fd56-203">*正在安裝 Unity.Mvc3 NuGet 封裝*</span><span class="sxs-lookup"><span data-stu-id="6fd56-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="6fd56-204">一次**Unity.Mvc3**安裝封裝、 瀏覽的檔案和資料夾，它會自動新增以簡化 Unity 組態。</span><span class="sxs-lookup"><span data-stu-id="6fd56-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="6fd56-205">![安裝 Unity.Mvc3 套件](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 封裝安裝")</span><span class="sxs-lookup"><span data-stu-id="6fd56-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="6fd56-206">*Unity.Mvc3 封裝安裝*</span><span class="sxs-lookup"><span data-stu-id="6fd56-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="6fd56-207">工作 3-Global.asax.cs 應用程式中註冊的 Unity\_開始</span><span class="sxs-lookup"><span data-stu-id="6fd56-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="6fd56-208">在這項工作，您將更新**應用程式\_啟動**方法位於**Global.asax.cs**呼叫 Unity 啟動載入器初始設定式，並接著，更新 啟動載入器檔案註冊服務和控制站將用於相依性插入。</span><span class="sxs-lookup"><span data-stu-id="6fd56-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="6fd56-209">現在，您將連結啟動載入器即初始化 Unity 容器的檔案和相依性解析程式。</span><span class="sxs-lookup"><span data-stu-id="6fd56-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="6fd56-210">若要這樣做，請開啟**Global.asax.cs**並加入下列反白顯示的程式碼內**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="6fd56-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="6fd56-211">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-初始化 Unity*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="6fd56-212">開啟**Bootstrapper.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="6fd56-213">包含下列命名空間： **MvcMusicStore.Services**和**MusicStore.Controllers**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="6fd56-214">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-啟動載入器加入命名空間*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="6fd56-215">取代**BuildUnityContainer**方法的內容存放區控制站和存放服務註冊為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fd56-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="6fd56-216">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-暫存器儲存控制站及服務*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="6fd56-217">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="6fd56-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="6fd56-218">在這個工作中，您將執行應用程式，它可以立即載入之後包括 Unity 的確認。</span><span class="sxs-lookup"><span data-stu-id="6fd56-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="6fd56-219">按**F5**執行應用程式，應用程式應該立即載入而不會顯示任何錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="6fd56-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="6fd56-220">![執行應用程式相依性插入](aspnet-mvc-4-dependency-injection/_static/image6.png "執行具有相依性插入應用程式")</span><span class="sxs-lookup"><span data-stu-id="6fd56-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="6fd56-221">*執行具有相依性插入應用程式*</span><span class="sxs-lookup"><span data-stu-id="6fd56-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="6fd56-222">瀏覽至**/儲存**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-222">Browse to **/Store**.</span></span> <span data-ttu-id="6fd56-223">這將會叫用**StoreController**，立即建立使用**Unity**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="6fd56-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="6fd56-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="6fd56-225">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="6fd56-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="6fd56-226">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6fd56-226">Close the browser.</span></span>

<span data-ttu-id="6fd56-227">在下列練習中，您將學習如何擴充至 ASP.NET MVC 檢視和動作篩選條件內使用它的相依性插入範圍。</span><span class="sxs-lookup"><span data-stu-id="6fd56-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="6fd56-228">練習 2： 將，以便檢視</span><span class="sxs-lookup"><span data-stu-id="6fd56-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="6fd56-229">在此練習中，您將學習如何使用 Unity 整合檢視中的新功能的 ASP.NET MVC 4 具有相依性插入。</span><span class="sxs-lookup"><span data-stu-id="6fd56-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="6fd56-230">若要這樣做，您會呼叫存放區瀏覽 檢視中，就會顯示一則訊息以及圖內的自訂服務。</span><span class="sxs-lookup"><span data-stu-id="6fd56-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="6fd56-231">然後，您將會與 Unity 整合專案，並建立自訂的相依性解析程式，將相依性。</span><span class="sxs-lookup"><span data-stu-id="6fd56-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="6fd56-232">工作 1-建立使用服務的檢視</span><span class="sxs-lookup"><span data-stu-id="6fd56-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="6fd56-233">在這項工作，您將建立會執行服務呼叫，以產生新的相依性的檢視。</span><span class="sxs-lookup"><span data-stu-id="6fd56-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="6fd56-234">服務包含簡單的傳訊服務包含在此解決方案中。</span><span class="sxs-lookup"><span data-stu-id="6fd56-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="6fd56-235">開啟**開始**方案位於**Source\Ex02 插入 View\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="6fd56-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="6fd56-236">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="6fd56-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="6fd56-237">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="6fd56-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6fd56-238">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6fd56-239">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="6fd56-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6fd56-240">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6fd56-241">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="6fd56-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6fd56-242">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6fd56-243">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="6fd56-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="6fd56-244">如需詳細資訊，請參閱這篇文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="6fd56-245">包含**MessageService.cs**和**IMessageService.cs**類別位於**來源 \Assets**資料夾中的**/Services**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="6fd56-246">若要這樣做，請以滑鼠右鍵按一下**服務**資料夾，然後選取**加入現有項目**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="6fd56-247">瀏覽至檔案的位置，並將它們包含。</span><span class="sxs-lookup"><span data-stu-id="6fd56-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="6fd56-248">![新增訊息服務和服務介面](aspnet-mvc-4-dependency-injection/_static/image8.png "加入訊息服務與服務介面")</span><span class="sxs-lookup"><span data-stu-id="6fd56-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="6fd56-249">*新增訊息服務和服務介面*</span><span class="sxs-lookup"><span data-stu-id="6fd56-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6fd56-250">**IMessageService**介面會定義兩個屬性實作**MessageService**類別。</span><span class="sxs-lookup"><span data-stu-id="6fd56-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="6fd56-251">這些屬性-**訊息**和**ImageUrl**-儲存要顯示的訊息和影像的 URL。</span><span class="sxs-lookup"><span data-stu-id="6fd56-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="6fd56-252">建立資料夾**/頁面**在專案的根資料夾，並將現有的類別**MyBasePage.cs**從**Source\Assets**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="6fd56-253">您將會繼承自基底頁面具有下列結構。</span><span class="sxs-lookup"><span data-stu-id="6fd56-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="6fd56-254">![Pages 資料夾](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages 資料夾")</span><span class="sxs-lookup"><span data-stu-id="6fd56-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="6fd56-255">開啟**Browse.cshtml**檢視從**/檢視表/存放區**資料夾，並使其繼承自**MyBasePage.cs**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="6fd56-256">在**瀏覽**檢視中，將呼叫加入**MessageService**来顯示的映像和服務所擷取的訊息。</span><span class="sxs-lookup"><span data-stu-id="6fd56-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="6fd56-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="6fd56-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="6fd56-258">工作 2-包括自訂相依性解析程式和自訂檢視頁面啟動項</span><span class="sxs-lookup"><span data-stu-id="6fd56-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="6fd56-259">在先前工作中，您可以插入內執行服務呼叫內文中檢視新的相依性。</span><span class="sxs-lookup"><span data-stu-id="6fd56-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="6fd56-260">現在，您將會解決該相依性，藉由實作 ASP.NET MVC 相依性插入介面**IViewPageActivator**和**IDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="6fd56-261">您會將包含方案的實作**IDependencyResolver** ，將會處理服務擷取使用 Unity。</span><span class="sxs-lookup"><span data-stu-id="6fd56-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="6fd56-262">然後，您將會包含其他自訂實作**IViewPageActivator**都解決此問題的檢視建立的介面。</span><span class="sxs-lookup"><span data-stu-id="6fd56-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="6fd56-263">ASP.NET MVC 3，因為相依性插入的實作已簡化註冊服務的介面。</span><span class="sxs-lookup"><span data-stu-id="6fd56-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="6fd56-264">**IDependencyResolver**和**IViewPageActivator**屬於的 ASP.NET MVC 3 的功能相依性插入。</span><span class="sxs-lookup"><span data-stu-id="6fd56-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="6fd56-265">**-IDependencyResolver**介面取代先前 IMvcServiceLocator。</span><span class="sxs-lookup"><span data-stu-id="6fd56-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="6fd56-266">IDependencyResolver 的實作者必須傳回服務或服務集合的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fd56-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="6fd56-267">**-IViewPageActivator**介面會提供更細微的控制，透過檢視頁面如何透過相依性插入具現。</span><span class="sxs-lookup"><span data-stu-id="6fd56-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="6fd56-268">類別可實作**IViewPageActivator**介面可以建立使用內容資訊的檢視執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fd56-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="6fd56-269">建立 /**工廠**專案的根資料夾中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="6fd56-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="6fd56-270">包含**CustomViewPageActivator.cs**至您的方案從**/來源/資產/**至**工廠**資料夾。</span><span class="sxs-lookup"><span data-stu-id="6fd56-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="6fd56-271">若要這樣做，請以滑鼠右鍵按一下**/Factories**資料夾中，選取**新增 |現有項目**，然後選取  **CustomViewPageActivator.cs**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="6fd56-272">這個類別會實作**IViewPageActivator**來保存 Unity 容器介面。</span><span class="sxs-lookup"><span data-stu-id="6fd56-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="6fd56-273">**CustomViewPageActivator**負責管理檢視的建立使用 Unity 的容器。</span><span class="sxs-lookup"><span data-stu-id="6fd56-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="6fd56-274">包含**UnityDependencyResolver.cs**檔案從**/來源/資產**至**/Factories**資料夾。</span><span class="sxs-lookup"><span data-stu-id="6fd56-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="6fd56-275">若要這樣做，請以滑鼠右鍵按一下**/Factories**資料夾中，選取**新增 |現有項目**，然後選取  **UnityDependencyResolver.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="6fd56-276">**UnityDependencyResolver**類別是針對 Unity 自訂 dependencyresolver 來註冊。</span><span class="sxs-lookup"><span data-stu-id="6fd56-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="6fd56-277">Unity 容器內找不到服務，被 invocated 基底的解析程式。</span><span class="sxs-lookup"><span data-stu-id="6fd56-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="6fd56-278">這兩種實作會在下列工作中註冊，讓模型知道服務和檢視表的位置。</span><span class="sxs-lookup"><span data-stu-id="6fd56-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="6fd56-279">工作 3-Unity 容器內註冊相依性插入</span><span class="sxs-lookup"><span data-stu-id="6fd56-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="6fd56-280">在這項工作，您會將放方式來產生工作的相依性插入的所有先前內容。</span><span class="sxs-lookup"><span data-stu-id="6fd56-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="6fd56-281">到目前為止您方案中有下列項目：</span><span class="sxs-lookup"><span data-stu-id="6fd56-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="6fd56-282">A**瀏覽**檢視繼承自**MyBaseClass**並取用**MessageService**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="6fd56-283">中繼類別-**MyBaseClass**-具有相依性插入宣告服務介面。</span><span class="sxs-lookup"><span data-stu-id="6fd56-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="6fd56-284">服務- **MessageService** -及其介面**IMessageService**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="6fd56-285">自訂相依性解析程式的 Unity- **UnityDependencyResolver** -服務擷取的處理。</span><span class="sxs-lookup"><span data-stu-id="6fd56-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="6fd56-286">檢視頁面啟動項- **CustomViewPageActivator** -建立頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd56-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="6fd56-287">若要插入**瀏覽** 檢視中，您會立即註冊自訂相依性解析程式 Unity 容器中。</span><span class="sxs-lookup"><span data-stu-id="6fd56-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="6fd56-288">開啟**Bootstrapper.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="6fd56-289">註冊執行個體**MessageService**到 Unity 容器初始化服務：</span><span class="sxs-lookup"><span data-stu-id="6fd56-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="6fd56-290">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-暫存器訊息服務*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="6fd56-291">將參考加入**MvcMusicStore.Factories**命名空間。</span><span class="sxs-lookup"><span data-stu-id="6fd56-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="6fd56-292">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-Factory 命名空間*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="6fd56-293">註冊**CustomViewPageActivator** Unity 容器檢視頁面啟動項為：</span><span class="sxs-lookup"><span data-stu-id="6fd56-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="6fd56-294">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-註冊 CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="6fd56-295">ASP.NET MVC 4 預設相依性解析程式取代的執行個體**UnityDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="6fd56-296">若要這樣做，請取代**Initialise**方法內容以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="6fd56-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="6fd56-297">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-更新相依性解析程式*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="6fd56-298">ASP.NET MVC 提供預設相依性解析程式類別。</span><span class="sxs-lookup"><span data-stu-id="6fd56-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="6fd56-299">若要使用自訂的相依性解析程式做為我們已經為 unity 建立，這個解析程式已被取代。</span><span class="sxs-lookup"><span data-stu-id="6fd56-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="6fd56-300">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="6fd56-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="6fd56-301">在這個工作中，您將執行的應用程式，以確認存放區的瀏覽器使用該服務，並顯示的影像和擷取的訊息：</span><span class="sxs-lookup"><span data-stu-id="6fd56-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="6fd56-302">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6fd56-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="6fd56-303">按一下**Rock**內內容類型 功能表，請參閱如何**MessageService**所插入至檢視，並載入歡迎訊息和映像。</span><span class="sxs-lookup"><span data-stu-id="6fd56-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="6fd56-304">在此範例中，我們會輸入到&quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="6fd56-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="6fd56-305">![MVC Music Store 檢視資料隱碼](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store 檢視資料隱碼")</span><span class="sxs-lookup"><span data-stu-id="6fd56-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="6fd56-306">*MVC Music Store 檢視資料隱碼*</span><span class="sxs-lookup"><span data-stu-id="6fd56-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="6fd56-307">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6fd56-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="6fd56-308">練習 3： 將動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="6fd56-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="6fd56-309">在先前的實際操作實驗室**自訂動作篩選條件**您已使用篩選器的自訂及資料隱碼。</span><span class="sxs-lookup"><span data-stu-id="6fd56-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="6fd56-310">在此練習中，您將學習如何將篩選器與相依性插入，使用 Unity 容器。</span><span class="sxs-lookup"><span data-stu-id="6fd56-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="6fd56-311">若要這樣做，您將會加入 Music Store 方案自訂動作篩選條件會追蹤網站的活動。</span><span class="sxs-lookup"><span data-stu-id="6fd56-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="6fd56-312">工作 1-在方案中包含追蹤篩選</span><span class="sxs-lookup"><span data-stu-id="6fd56-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="6fd56-313">在這項工作，您將會包含在 Music Store 中追蹤事件的自訂動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="6fd56-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="6fd56-314">做為自訂動作篩選條件概念已處理的上一個實驗室&quot;自訂動作篩選條件&quot;，您將只包含本實驗室中，從 Assets 資料夾篩選類別，然後再建立 Unity 篩選條件提供者：</span><span class="sxs-lookup"><span data-stu-id="6fd56-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="6fd56-315">開啟**開始**方案位於**Source\Ex03-插入動作 Filter\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="6fd56-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="6fd56-316">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="6fd56-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="6fd56-317">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="6fd56-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6fd56-318">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6fd56-319">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="6fd56-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6fd56-320">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6fd56-321">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="6fd56-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6fd56-322">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="6fd56-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6fd56-323">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="6fd56-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="6fd56-324">如需詳細資訊，請參閱這篇文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="6fd56-325">包含**TraceActionFilter.cs**檔案從**/來源/資產**至**/篩選**資料夾。</span><span class="sxs-lookup"><span data-stu-id="6fd56-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="6fd56-326">此自訂動作篩選條件執行 ASP.NET 追蹤。</span><span class="sxs-lookup"><span data-stu-id="6fd56-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="6fd56-327">您可以檢查&quot;ASP.NET MVC 4 本機和動態的動作篩選條件&quot;實驗室更多的參考。</span><span class="sxs-lookup"><span data-stu-id="6fd56-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="6fd56-328">加入空類別**FilterProvider.cs**資料夾中的專案  **/篩選。**</span><span class="sxs-lookup"><span data-stu-id="6fd56-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="6fd56-329">新增**System.Web.Mvc**和**Microsoft.Practices.Unity**中的命名空間**FilterProvider.cs**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="6fd56-330">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者加入命名空間*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="6fd56-331">使類別繼承自**IFilterProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="6fd56-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="6fd56-332">新增**IUnityContainer**屬性**FilterProvider**類別，然後再建立 指定容器的類別建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fd56-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="6fd56-333">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者的建構函式*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="6fd56-334">篩選條件提供者類別的建構函式不建立**新**物件內。</span><span class="sxs-lookup"><span data-stu-id="6fd56-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="6fd56-335">容器會傳遞做為參數，並將相依性已解決的 Unity。</span><span class="sxs-lookup"><span data-stu-id="6fd56-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="6fd56-336">在**FilterProvider**類別，實作方法**GetFilters**從**IFilterProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="6fd56-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="6fd56-337">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者 GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="6fd56-338">工作 2-註冊及啟用篩選器</span><span class="sxs-lookup"><span data-stu-id="6fd56-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="6fd56-339">在這項工作，您將啟用站台的追蹤。</span><span class="sxs-lookup"><span data-stu-id="6fd56-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="6fd56-340">若要這樣做，請將登錄中的篩選條件**Bootstrapper.cs BuildUnityContainer**方法來啟動追蹤：</span><span class="sxs-lookup"><span data-stu-id="6fd56-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="6fd56-341">開啟**Web.config**位於專案的根目錄和啟用追蹤追蹤在 System.Web 群組。</span><span class="sxs-lookup"><span data-stu-id="6fd56-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="6fd56-342">開啟**Bootstrapper.cs**在專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="6fd56-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="6fd56-343">將參考加入**MvcMusicStore.Filters**命名空間。</span><span class="sxs-lookup"><span data-stu-id="6fd56-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="6fd56-344">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-啟動載入器加入命名空間*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="6fd56-345">選取**BuildUnityContainer**方法和註冊 Unity 容器中的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="6fd56-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="6fd56-346">您必須註冊篩選條件提供者，以及動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="6fd56-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="6fd56-347">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-註冊 FilterProvider 和 ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="6fd56-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="6fd56-348">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="6fd56-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="6fd56-349">在這項工作，您將執行應用程式，並測試自訂動作篩選條件追蹤的活動：</span><span class="sxs-lookup"><span data-stu-id="6fd56-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="6fd56-350">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6fd56-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="6fd56-351">按一下**Rock**內容類型 功能表中。</span><span class="sxs-lookup"><span data-stu-id="6fd56-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="6fd56-352">您可以瀏覽至多個內容類型如果您想要。</span><span class="sxs-lookup"><span data-stu-id="6fd56-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="6fd56-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="6fd56-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="6fd56-354">*Music 市集*</span><span class="sxs-lookup"><span data-stu-id="6fd56-354">*Music Store*</span></span>
3. <span data-ttu-id="6fd56-355">瀏覽至**/Trace.axd**以查看應用程式追蹤頁面，然後再按一下**檢視詳細資料**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="6fd56-356">![應用程式追蹤記錄檔](aspnet-mvc-4-dependency-injection/_static/image12.png "應用程式追蹤記錄檔")</span><span class="sxs-lookup"><span data-stu-id="6fd56-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="6fd56-357">*應用程式追蹤記錄檔*</span><span class="sxs-lookup"><span data-stu-id="6fd56-357">*Application Trace Log*</span></span>

    <span data-ttu-id="6fd56-358">![應用程式追蹤-要求詳細資料](aspnet-mvc-4-dependency-injection/_static/image13.png "追蹤應用程式-要求詳細資料")</span><span class="sxs-lookup"><span data-stu-id="6fd56-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="6fd56-359">*應用程式追蹤-要求詳細資料*</span><span class="sxs-lookup"><span data-stu-id="6fd56-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="6fd56-360">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6fd56-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="6fd56-361">總結</span><span class="sxs-lookup"><span data-stu-id="6fd56-361">Summary</span></span>

<span data-ttu-id="6fd56-362">藉由完成這個實際操作實驗室中，您已經學會如何使用 ASP.NET MVC 4 中的相依性插入，方法是整合 Unity 使用 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="6fd56-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="6fd56-363">若要達成此目標，您使用的相依性插入內控制站、 檢視和動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="6fd56-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="6fd56-364">涵蓋下列概念：</span><span class="sxs-lookup"><span data-stu-id="6fd56-364">The following concepts were covered:</span></span>

- <span data-ttu-id="6fd56-365">ASP.NET MVC 4 相依性插入功能</span><span class="sxs-lookup"><span data-stu-id="6fd56-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="6fd56-366">Unity 整合使用 Unity.Mvc3 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="6fd56-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="6fd56-367">相依性插入控制器</span><span class="sxs-lookup"><span data-stu-id="6fd56-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="6fd56-368">在檢視中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="6fd56-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="6fd56-369">相依性插入的動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="6fd56-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="6fd56-370">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="6fd56-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="6fd56-371">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="6fd56-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="6fd56-372">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="6fd56-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="6fd56-373">移至[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="6fd56-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6fd56-374">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; *Visual Studio Express 2012 for Web 與 Windows Azure SDK*&quot;。</span><span class="sxs-lookup"><span data-stu-id="6fd56-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="6fd56-375">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-375">Click on **Install Now**.</span></span> <span data-ttu-id="6fd56-376">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="6fd56-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6fd56-377">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="6fd56-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6fd56-378">![安裝 Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="6fd56-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="6fd56-379">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="6fd56-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="6fd56-380">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="6fd56-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="6fd56-382">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="6fd56-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6fd56-383">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="6fd56-383">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="6fd56-385">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="6fd56-385">*Installation progress*</span></span>
6. <span data-ttu-id="6fd56-386">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-386">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="6fd56-388">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="6fd56-388">*Installation completed*</span></span>
7. <span data-ttu-id="6fd56-389">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="6fd56-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="6fd56-390">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="6fd56-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="6fd56-392">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="6fd56-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="6fd56-393">附錄 b： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="6fd56-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="6fd56-394">程式碼片段，您會有您需要在您可以的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fd56-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="6fd56-395">實驗室文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="6fd56-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="6fd56-396">![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-dependency-injection/_static/image19.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")</span><span class="sxs-lookup"><span data-stu-id="6fd56-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="6fd56-397">*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="6fd56-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="6fd56-398">***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="6fd56-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="6fd56-399">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fd56-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="6fd56-400">開始鍵入片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="6fd56-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="6fd56-401">觀察 IntelliSense 會顯示比對程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fd56-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="6fd56-402">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="6fd56-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="6fd56-403">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="6fd56-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="6fd56-404">![開始輸入程式碼片段名稱](aspnet-mvc-4-dependency-injection/_static/image20.png "開始鍵入片段名稱")</span><span class="sxs-lookup"><span data-stu-id="6fd56-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="6fd56-405">*開始輸入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="6fd56-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="6fd56-406">![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-dependency-injection/_static/image21.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="6fd56-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="6fd56-407">*按 Tab 鍵選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="6fd56-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="6fd56-408">![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-dependency-injection/_static/image22.png "按 Tab 鍵一次程式碼片段就會擴展")</span><span class="sxs-lookup"><span data-stu-id="6fd56-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="6fd56-409">*按 Tab 鍵一次程式碼片段就會擴展*</span><span class="sxs-lookup"><span data-stu-id="6fd56-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="6fd56-410">***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="6fd56-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="6fd56-411">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="6fd56-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="6fd56-412">選取**插入程式碼片段**後面**我的程式碼片段**。</span><span class="sxs-lookup"><span data-stu-id="6fd56-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="6fd56-413">透過在按一下挑選清單中，從相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="6fd56-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="6fd56-414">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-dependency-injection/_static/image23.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="6fd56-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="6fd56-415">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="6fd56-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="6fd56-416">![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-dependency-injection/_static/image24.png "上它即可挑選清單中，從相關的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="6fd56-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="6fd56-417">*透過在按一下挑選清單中，從相關的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="6fd56-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
