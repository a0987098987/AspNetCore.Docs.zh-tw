---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 相依性插入 |Microsoft Docs
author: rick-anderson
description: 注意： 這個實際操作實驗室中假設您有 ASP.NET MVC 和 ASP.NET MVC 4 的篩選器的基本知識。 如果您未曾使用 ASP.NET MVC 4 的篩選器之前，我們建議...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834642"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="7c64c-104">ASP.NET MVC 4 相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="7c64c-105">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7c64c-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7c64c-106">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="7c64c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7c64c-107">這個實際操作實驗室會假設您有的基本知識**ASP.NET MVC**並**ASP.NET MVC 4 篩選**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="7c64c-108">如果您未曾使用**ASP.NET MVC 4 會篩選**之前，我們建議您將逐一**ASP.NET MVC 自訂動作篩選條件**實際操作實驗室。</span><span class="sxs-lookup"><span data-stu-id="7c64c-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="7c64c-109">Web 研討會訓練套件中，在可從包含所有的範例程式碼和程式碼片段[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7c64c-110">這個實驗室中的特定專案將會位於[ASP.NET MVC 4 相依性插入](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="7c64c-111">在 **物件導向程式設計**開發架構時，物件一起運作共同作業模型有參與者和取用者。</span><span class="sxs-lookup"><span data-stu-id="7c64c-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="7c64c-112">當然，這套通訊模型產生物件和元件，變得難以管理時就會增加複雜性之間的相依性。</span><span class="sxs-lookup"><span data-stu-id="7c64c-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="7c64c-113">![類別的相依性，並建立模型的複雜性](aspnet-mvc-4-dependency-injection/_static/image1.png "類別相依性，並建立複雜的模型")</span><span class="sxs-lookup"><span data-stu-id="7c64c-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="7c64c-114">*類別的相依性和模型的複雜度*</span><span class="sxs-lookup"><span data-stu-id="7c64c-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="7c64c-115">您可能聽過有關**Factory 模式**和之間的介面和實作使用服務，用戶端物件通常適用於服務位置的責任區隔。</span><span class="sxs-lookup"><span data-stu-id="7c64c-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="7c64c-116">相依性插入模式是控制反轉的特定實作。</span><span class="sxs-lookup"><span data-stu-id="7c64c-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="7c64c-117">**逆轉控制 (IoC)** 表示物件不會建立其他物件，它們都依賴執行其工作。</span><span class="sxs-lookup"><span data-stu-id="7c64c-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="7c64c-118">相反地，他們可以從外部來源 （例如 xml 組態檔） 所需的物件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="7c64c-119">**相依性插入 (DI)** 表示這是由物件介入，通常會將建構函式參數傳遞的 framework 元件並設定屬性。</span><span class="sxs-lookup"><span data-stu-id="7c64c-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="7c64c-120">相依性插入 (DI) 設計模式</span><span class="sxs-lookup"><span data-stu-id="7c64c-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="7c64c-121">概括而言，相依性插入的目標是，用戶端類別 (例如*高爾夫球*) 必須符合介面的項目 (例如*IClub*)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="7c64c-122">它不在意具象型別為何 (例如*WoodClub，IronClub，WedgeClub*或*PutterClub*)，它想要處理的其他人 (例如更佳*caddy*)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="7c64c-123">在 ASP.NET MVC 中的相依性解析程式可讓您註冊您的相依性邏輯的任一處 (例如容器或*包梅花的么點*)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="7c64c-124">![相依性插入圖表](aspnet-mvc-4-dependency-injection/_static/image2.png "相依性插入圖")</span><span class="sxs-lookup"><span data-stu-id="7c64c-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="7c64c-125">*相依性插入-Golf 比喻*</span><span class="sxs-lookup"><span data-stu-id="7c64c-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="7c64c-126">使用相依性插入模式和 「 控制項反轉的優點如下所示：</span><span class="sxs-lookup"><span data-stu-id="7c64c-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="7c64c-127">減少類別結合程度</span><span class="sxs-lookup"><span data-stu-id="7c64c-127">Reduces class coupling</span></span>
- <span data-ttu-id="7c64c-128">增加程式碼重複使用</span><span class="sxs-lookup"><span data-stu-id="7c64c-128">Increases code reusing</span></span>
- <span data-ttu-id="7c64c-129">改善程式碼維護性</span><span class="sxs-lookup"><span data-stu-id="7c64c-129">Improves code maintainability</span></span>
- <span data-ttu-id="7c64c-130">改善應用程式測試</span><span class="sxs-lookup"><span data-stu-id="7c64c-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="7c64c-131">使用抽象 Factory 設計模式時，有時比較相依性插入，但這兩種方法之間的些微差異。</span><span class="sxs-lookup"><span data-stu-id="7c64c-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="7c64c-132">DI 有背後努力解決相依性呼叫之處理站和已註冊的服務的架構。</span><span class="sxs-lookup"><span data-stu-id="7c64c-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="7c64c-133">既然您了解相依性插入模式時，您將學習在這個實驗室中如何套用 ASP.NET MVC 4 中。</span><span class="sxs-lookup"><span data-stu-id="7c64c-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="7c64c-134">您會開始使用中的相依性插入**控制器**来包含的資料庫存取服務。</span><span class="sxs-lookup"><span data-stu-id="7c64c-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="7c64c-135">接下來，您會將套用至相依性插入**檢視**來取用服務，並顯示資訊。</span><span class="sxs-lookup"><span data-stu-id="7c64c-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="7c64c-136">最後，您就可以擴充 DI ASP.NET MVC 4 的篩選條件，將方案中的自訂動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="7c64c-137">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="7c64c-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="7c64c-138">將 ASP.NET MVC 4 與 Unity 整合使用 NuGet 套件的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="7c64c-139">使用 ASP.NET MVC 控制器內的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="7c64c-140">使用 ASP.NET MVC 檢視內的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="7c64c-141">使用 ASP.NET MVC 動作篩選條件內的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="7c64c-142">這個實驗室用於 Unity.Mvc3 NuGet 套件相依性解析，但是可以採用任何相依性插入架構，才能使用 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="7c64c-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7c64c-143">必要條件</span><span class="sxs-lookup"><span data-stu-id="7c64c-143">Prerequisites</span></span>

<span data-ttu-id="7c64c-144">您必須具備下列項目，即可完成此實驗室：</span><span class="sxs-lookup"><span data-stu-id="7c64c-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7c64c-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7c64c-146">安裝程式</span><span class="sxs-lookup"><span data-stu-id="7c64c-146">Setup</span></span>

<span data-ttu-id="7c64c-147">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="7c64c-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="7c64c-148">為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="7c64c-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7c64c-149">若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7c64c-150">如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。</span><span class="sxs-lookup"><span data-stu-id="7c64c-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7c64c-151">練習</span><span class="sxs-lookup"><span data-stu-id="7c64c-151">Exercises</span></span>

<span data-ttu-id="7c64c-152">這個實際操作實驗室是由下列練習所組成的：</span><span class="sxs-lookup"><span data-stu-id="7c64c-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="7c64c-153">練習 1： 插入控制器</span><span class="sxs-lookup"><span data-stu-id="7c64c-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="7c64c-154">練習 2： 將檢視</span><span class="sxs-lookup"><span data-stu-id="7c64c-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="7c64c-155">練習 3： 將篩選器</span><span class="sxs-lookup"><span data-stu-id="7c64c-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="7c64c-156">每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c64c-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7c64c-157">如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。</span><span class="sxs-lookup"><span data-stu-id="7c64c-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="7c64c-158">完成這個實驗室估計時間： **30 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="7c64c-159">練習 1： 插入控制器</span><span class="sxs-lookup"><span data-stu-id="7c64c-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="7c64c-160">在此練習中，您將學習如何在 ASP.NET MVC 控制器中使用相依性插入，藉由整合 Unity 使用 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="7c64c-161">基於這個理由，您將會包含您 MvcMusicStore 控制站，以區隔邏輯與資料存取服務。</span><span class="sxs-lookup"><span data-stu-id="7c64c-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="7c64c-162">服務會建立新的相依性，在控制器建構函式，會搭配使用相依性插入來解決**Unity**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="7c64c-163">這種方法將會說明如何產生較低結合的應用程式，也就是更有彈性且容易維護和測試。</span><span class="sxs-lookup"><span data-stu-id="7c64c-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="7c64c-164">您也將學習如何使用 Unity 整合 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="7c64c-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="7c64c-165">關於 StoreManager 服務</span><span class="sxs-lookup"><span data-stu-id="7c64c-165">About StoreManager Service</span></span>

<span data-ttu-id="7c64c-166">MVC Music 市集現在提供開始方案中包含管理名為的存放控制器資料的服務**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="7c64c-167">您將在下面找到存放區服務實作。</span><span class="sxs-lookup"><span data-stu-id="7c64c-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="7c64c-168">請注意，所有方法會都傳回模型的實體。</span><span class="sxs-lookup"><span data-stu-id="7c64c-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="7c64c-169">**StoreController**從 begin 方案現在取用**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="7c64c-170">資料的所有參考都移除了**StoreController**，以及現在能夠修改目前的資料存取提供者，而不需要變更任何方法，來解決**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="7c64c-171">您會發現下面**StoreController**實作有相依性**StoreService**類別建構函式內。</span><span class="sxs-lookup"><span data-stu-id="7c64c-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="7c64c-172">在這個練習中導入的相依性相關**控制反轉**(IoC)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="7c64c-173">**StoreController**類別建構函式收到**IStoreService**型別參數，請務必將執行從類別內的服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="7c64c-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="7c64c-174">不過， **StoreController**未實作預設建構函式 （不含任何參數） 的任何控制器必須能夠使用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="7c64c-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="7c64c-175">若要解決相依性，控制器必須抽象的處理站 （會傳回指定之型別的任何物件的類別） 所建立。</span><span class="sxs-lookup"><span data-stu-id="7c64c-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="7c64c-176">因為沒有宣告沒有無參數建構函式，而不需要傳送的服務物件，建立 StoreController 嘗試類別時，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c64c-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="7c64c-177">工作 1-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7c64c-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="7c64c-178">在這個工作中，您將執行 Begin 應用程式，包括到分開的應用程式邏輯中的資料存取的存放控制器的服務。</span><span class="sxs-lookup"><span data-stu-id="7c64c-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="7c64c-179">當執行應用程式，您會收到例外狀況，控制器服務不會傳遞做為參數預設為：</span><span class="sxs-lookup"><span data-stu-id="7c64c-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="7c64c-180">開啟**開始**解決方案位於**Source\Ex01 插入 Controller\Begin**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="7c64c-181">您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="7c64c-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c64c-182">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c64c-183">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c64c-184">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c64c-185">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="7c64c-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c64c-186">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c64c-187">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="7c64c-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c64c-188">按下**Ctrl + F5**執行應用程式，但不偵錯。</span><span class="sxs-lookup"><span data-stu-id="7c64c-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="7c64c-189">您會收到錯誤訊息&quot;**沒有為這個物件定義的無參數建構函式**&quot;:</span><span class="sxs-lookup"><span data-stu-id="7c64c-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="7c64c-190">![執行 ASP.NET MVC 開始應用程式時的錯誤](aspnet-mvc-4-dependency-injection/_static/image3.png "執行 ASP.NET MVC 開始應用程式時的錯誤")</span><span class="sxs-lookup"><span data-stu-id="7c64c-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="7c64c-191">*執行 ASP.NET MVC 開始應用程式時的錯誤*</span><span class="sxs-lookup"><span data-stu-id="7c64c-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="7c64c-192">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7c64c-192">Close the browser.</span></span>

<span data-ttu-id="7c64c-193">在下列步驟中您會使用音樂的儲存解決方案，將此控制站所需要的相依性。</span><span class="sxs-lookup"><span data-stu-id="7c64c-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="7c64c-194">工作 2-將 Unity 包括 MvcMusicStore 解決方案</span><span class="sxs-lookup"><span data-stu-id="7c64c-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="7c64c-195">在這個工作中，您會將包含**Unity.Mvc3**至方案的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="7c64c-196">Unity.Mvc3 套件專為 ASP.NET MVC 3，但它是與 ASP.NET MVC 4 完全相容。</span><span class="sxs-lookup"><span data-stu-id="7c64c-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="7c64c-197">Unity 執行個體是使用選擇性支援輕量型、 可擴充的相依性插入容器，然後輸入攔截。</span><span class="sxs-lookup"><span data-stu-id="7c64c-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="7c64c-198">它是任何類型的.NET 應用程式中使用的一般用途容器。</span><span class="sxs-lookup"><span data-stu-id="7c64c-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="7c64c-199">它提供包括相依性插入的機制中找到的所有一般功能： 建立物件，藉由指定相依性，在執行階段和彈性，延後至容器的元件組態需求的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="7c64c-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="7c64c-200">安裝**Unity.Mvc3**中的 NuGet 套件**MvcMusicStore**專案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="7c64c-201">若要這樣做，請開啟**Package Manager Console**從**檢視** | **其他 Windows**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="7c64c-202">執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="7c64c-202">Run the following command.</span></span>

    <span data-ttu-id="7c64c-203">PMC</span><span class="sxs-lookup"><span data-stu-id="7c64c-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="7c64c-204">![安裝 Unity.Mvc3 NuGet 封裝](aspnet-mvc-4-dependency-injection/_static/image4.png "安裝 Unity.Mvc3 NuGet 套件")</span><span class="sxs-lookup"><span data-stu-id="7c64c-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="7c64c-205">*安裝 Unity.Mvc3 NuGet 套件*</span><span class="sxs-lookup"><span data-stu-id="7c64c-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="7c64c-206">一次**Unity.Mvc3**安裝套件時，瀏覽的檔案和資料夾會自動新增以簡化 Unity 組態。</span><span class="sxs-lookup"><span data-stu-id="7c64c-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="7c64c-207">![安裝套件的 Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 套件安裝")</span><span class="sxs-lookup"><span data-stu-id="7c64c-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="7c64c-208">*Unity.Mvc3 套件安裝*</span><span class="sxs-lookup"><span data-stu-id="7c64c-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="7c64c-209">工作 3-在 Global.asax.cs 應用程式中註冊的 Unity\_開始</span><span class="sxs-lookup"><span data-stu-id="7c64c-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="7c64c-210">在這個工作中，您將會更新**應用程式\_開始**方法位於**Global.asax.cs**呼叫 Unity 啟動載入器的初始設定式，並接著，更新啟動載入器檔案註冊「 服務 」 和 「 控制站將用於相依性插入。</span><span class="sxs-lookup"><span data-stu-id="7c64c-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="7c64c-211">現在，您將會連結啟動載入器這是初始化 Unity 容器的檔案和相依性解析程式。</span><span class="sxs-lookup"><span data-stu-id="7c64c-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="7c64c-212">若要這樣做，請開啟**Global.asax.cs** ，並新增下列醒目提示的程式碼內**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="7c64c-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="7c64c-213">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-初始化 Unity*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="7c64c-214">開啟**Bootstrapper.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="7c64c-215">包含下列命名空間： **MvcMusicStore.Services**並**MusicStore.Controllers**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="7c64c-216">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-啟動載入器加入命名空間*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="7c64c-217">取代**BuildUnityContainer**方法的內容存放區控制站和存放服務會註冊為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c64c-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="7c64c-218">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex01-暫存存放區控制站和服務*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7c64c-219">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7c64c-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="7c64c-220">在這個工作中，您將執行的應用程式，以確認，它可以立即載入包含 Unity 之後。</span><span class="sxs-lookup"><span data-stu-id="7c64c-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="7c64c-221">按下**F5**執行應用程式，應用程式應該立即載入而不會顯示任何錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7c64c-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="7c64c-222">![執行應用程式使用 Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "執行應用程式使用相依性插入")</span><span class="sxs-lookup"><span data-stu-id="7c64c-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="7c64c-223">*使用相依性插入執行中應用程式*</span><span class="sxs-lookup"><span data-stu-id="7c64c-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="7c64c-224">瀏覽至 **/儲存**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-224">Browse to **/Store**.</span></span> <span data-ttu-id="7c64c-225">這將會叫用**StoreController**，它現在已建立使用**Unity**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="7c64c-226">![MVC Music 市集](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music 市集")</span><span class="sxs-lookup"><span data-stu-id="7c64c-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="7c64c-227">*MVC Music 市集*</span><span class="sxs-lookup"><span data-stu-id="7c64c-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="7c64c-228">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7c64c-228">Close the browser.</span></span>

<span data-ttu-id="7c64c-229">在下列練習中，您會了解如何擴充至 ASP.NET MVC 檢視和動作篩選條件內使用它的相依性插入範圍。</span><span class="sxs-lookup"><span data-stu-id="7c64c-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="7c64c-230">練習 2： 將檢視</span><span class="sxs-lookup"><span data-stu-id="7c64c-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="7c64c-231">在此練習中，您將學習如何在 ASP.NET MVC 4 的新功能與檢視中的相依性插入用於 Unity 的整合。</span><span class="sxs-lookup"><span data-stu-id="7c64c-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="7c64c-232">若要這樣做，您會呼叫自訂服務存放區瀏覽 檢視中，會顯示一則訊息和下列映像內。</span><span class="sxs-lookup"><span data-stu-id="7c64c-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="7c64c-233">然後，您將會與 Unity 整合專案，並建立自訂的相依性解析程式，以插入相依性。</span><span class="sxs-lookup"><span data-stu-id="7c64c-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="7c64c-234">工作 1： 建立使用服務的檢視</span><span class="sxs-lookup"><span data-stu-id="7c64c-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="7c64c-235">在這個工作中，您將建立會執行的服務呼叫，以產生新的相依性的檢視。</span><span class="sxs-lookup"><span data-stu-id="7c64c-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="7c64c-236">服務包含一個簡單的傳訊服務，包含在此解決方案中。</span><span class="sxs-lookup"><span data-stu-id="7c64c-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="7c64c-237">開啟**開始**解決方案位於**Source\Ex02 插入 View\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c64c-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="7c64c-238">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="7c64c-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c64c-239">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="7c64c-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c64c-240">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c64c-241">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c64c-242">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c64c-243">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="7c64c-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c64c-244">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c64c-245">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="7c64c-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="7c64c-246">如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="7c64c-247">包含**MessageService.cs**並**IMessageService.cs**類別位於**來源 \Assets**資料夾中的 **/服務**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="7c64c-248">若要這樣做，請以滑鼠右鍵按一下**Services**資料夾，然後選取**加入現有項目**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="7c64c-249">瀏覽至檔案的位置，並將它們包含。</span><span class="sxs-lookup"><span data-stu-id="7c64c-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="7c64c-250">![新增訊息服務和服務介面](aspnet-mvc-4-dependency-injection/_static/image8.png "新增訊息服務和服務介面")</span><span class="sxs-lookup"><span data-stu-id="7c64c-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="7c64c-251">*新增訊息服務和服務介面*</span><span class="sxs-lookup"><span data-stu-id="7c64c-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c64c-252">**IMessageService**介面會定義所實作的兩個屬性**MessageService**類別。</span><span class="sxs-lookup"><span data-stu-id="7c64c-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="7c64c-253">這些屬性-**訊息**並**ImageUrl**-儲存要顯示的訊息和影像的 URL。</span><span class="sxs-lookup"><span data-stu-id="7c64c-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="7c64c-254">建立資料夾 **/pages**在專案的根資料夾，然後新增現有的類別**MyBasePage.cs**從**Source\Assets**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="7c64c-255">您將會繼承自基底頁面具有下列結構。</span><span class="sxs-lookup"><span data-stu-id="7c64c-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="7c64c-256">![Pages 資料夾](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages 資料夾")</span><span class="sxs-lookup"><span data-stu-id="7c64c-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="7c64c-257">開啟**Browse.cshtml**從檢視 **/檢視/Store**資料夾，並使它繼承自**MyBasePage.cs**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="7c64c-258">在 **瀏覽**檢視中，將呼叫加入**MessageService**来顯示的映像和服務擷取的訊息。</span><span class="sxs-lookup"><span data-stu-id="7c64c-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="7c64c-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="7c64c-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="7c64c-260">工作 2-包括自訂相依性解析程式和自訂檢視頁面啟動項</span><span class="sxs-lookup"><span data-stu-id="7c64c-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="7c64c-261">在先前工作中，您可以插入新的相依性，來執行其內部服務呼叫在檢視內。</span><span class="sxs-lookup"><span data-stu-id="7c64c-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="7c64c-262">現在，您將會解決該相依性，藉由實作 ASP.NET MVC 相依性插入介面**IViewPageActivator**並**IDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="7c64c-263">您會在解決方案中包含的實作**IDependencyResolver** ，將會使用 Unity 處理服務擷取。</span><span class="sxs-lookup"><span data-stu-id="7c64c-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="7c64c-264">然後，您將會包含另一個自訂實作**IViewPageActivator**將解決建立檢視的介面。</span><span class="sxs-lookup"><span data-stu-id="7c64c-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="7c64c-265">ASP.NET MVC 3，因為相依性插入的實作已簡化註冊服務的介面。</span><span class="sxs-lookup"><span data-stu-id="7c64c-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="7c64c-266">**IDependencyResolver**並**IViewPageActivator**屬於相依性插入的 ASP.NET MVC 3 的功能。</span><span class="sxs-lookup"><span data-stu-id="7c64c-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="7c64c-267">**-IDependencyResolver**介面會取代先前 IMvcServiceLocator。</span><span class="sxs-lookup"><span data-stu-id="7c64c-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="7c64c-268">IDependencyResolver 的實作者必須傳回服務或服務集合的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7c64c-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="7c64c-269">**-IViewPageActivator**介面提供更細微的控制，透過檢視頁面會執行個體化的方式透過相依性插入。</span><span class="sxs-lookup"><span data-stu-id="7c64c-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="7c64c-270">類別可實作**IViewPageActivator**介面可以建立使用內容資訊的檢視執行個體。</span><span class="sxs-lookup"><span data-stu-id="7c64c-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="7c64c-271">建立 /**Factory**專案的根資料夾中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c64c-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="7c64c-272">包含**CustomViewPageActivator.cs**至您的方案，從 **/來源/資產/** 來**Factory**資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c64c-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="7c64c-273">若要這樣做，請以滑鼠右鍵按一下 **/Factories**資料夾中，選取**新增 |現有的項目**，然後選取**CustomViewPageActivator.cs**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="7c64c-274">這個類別會實作**IViewPageActivator**來保存 Unity 容器的介面。</span><span class="sxs-lookup"><span data-stu-id="7c64c-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c64c-275">**CustomViewPageActivator**負責使用 Unity 容器管理檢視的建立。</span><span class="sxs-lookup"><span data-stu-id="7c64c-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="7c64c-276">包含**UnityDependencyResolver.cs**檔案 **/來源/資產**來 **/Factories**資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c64c-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="7c64c-277">若要這樣做，請以滑鼠右鍵按一下 **/Factories**資料夾中，選取**新增 |現有的項目**，然後選取**UnityDependencyResolver.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c64c-278">**UnityDependencyResolver**類別是自訂的 Unity 的 dependencyresolver 來註冊。</span><span class="sxs-lookup"><span data-stu-id="7c64c-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="7c64c-279">當 Unity 容器內找不到服務時，被 invocated 基底的解析程式。</span><span class="sxs-lookup"><span data-stu-id="7c64c-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="7c64c-280">在下列工作中這兩種實作將會登錄，讓模型知道服務] 和 [檢視表的位置。</span><span class="sxs-lookup"><span data-stu-id="7c64c-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="7c64c-281">工作 3-Unity 容器內註冊相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="7c64c-282">在這個工作中，您會將放方式來產生工作的相依性插入所有先前的項目。</span><span class="sxs-lookup"><span data-stu-id="7c64c-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="7c64c-283">到目前為止您的解決方案會有下列項目：</span><span class="sxs-lookup"><span data-stu-id="7c64c-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="7c64c-284">A**瀏覽**檢視繼承自**MyBaseClass**並取用**MessageService**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="7c64c-285">中繼類別-**MyBaseClass**-具有相依性插入，宣告服務介面。</span><span class="sxs-lookup"><span data-stu-id="7c64c-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="7c64c-286">服務層**MessageService** -及其介面**IMessageService**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="7c64c-287">Unity-自訂相依性解析程式**UnityDependencyResolver** -可處理服務擷取。</span><span class="sxs-lookup"><span data-stu-id="7c64c-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="7c64c-288">檢視頁面啟動項- **CustomViewPageActivator** -建立頁面。</span><span class="sxs-lookup"><span data-stu-id="7c64c-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="7c64c-289">若要插入**瀏覽** 檢視中，您將會現在註冊自訂相依性解析程式 Unity 容器中。</span><span class="sxs-lookup"><span data-stu-id="7c64c-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="7c64c-290">開啟**Bootstrapper.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="7c64c-291">註冊執行個體**MessageService** Unity 容器中，以初始化服務：</span><span class="sxs-lookup"><span data-stu-id="7c64c-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="7c64c-292">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-註冊訊息服務*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="7c64c-293">將參考加入**MvcMusicStore.Factories**命名空間。</span><span class="sxs-lookup"><span data-stu-id="7c64c-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="7c64c-294">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-工廠命名空間*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="7c64c-295">註冊**CustomViewPageActivator** Unity 容器內檢視頁面啟動項為：</span><span class="sxs-lookup"><span data-stu-id="7c64c-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="7c64c-296">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-註冊 CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="7c64c-297">取代 ASP.NET MVC 4 預設相依性解析程式的執行個體**UnityDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="7c64c-298">若要這樣做，請取代**Initialise**內容為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="7c64c-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="7c64c-299">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex02-更新相依性解析程式*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c64c-300">ASP.NET MVC 提供的預設相依性解析程式類別。</span><span class="sxs-lookup"><span data-stu-id="7c64c-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="7c64c-301">若要使用自訂的相依性解析程式，我們已經為 unity 建立一個，此解析程式已被取代。</span><span class="sxs-lookup"><span data-stu-id="7c64c-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7c64c-302">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7c64c-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="7c64c-303">在這個工作中，您要執行的應用程式，以確認存放區瀏覽器取用服務，並顯示的影像和擷取的訊息：</span><span class="sxs-lookup"><span data-stu-id="7c64c-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="7c64c-304">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c64c-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="7c64c-305">按一下 [ **Rock**內的內容類型] 功能表並查看如何**MessageService**插入至檢視，並載入的歡迎訊息和映像。</span><span class="sxs-lookup"><span data-stu-id="7c64c-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="7c64c-306">在此範例中，我們會輸入到&quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="7c64c-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="7c64c-307">![MVC Music 市集檢視插入](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music 市集檢視插入")</span><span class="sxs-lookup"><span data-stu-id="7c64c-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="7c64c-308">*MVC Music 市集檢視插入*</span><span class="sxs-lookup"><span data-stu-id="7c64c-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="7c64c-309">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7c64c-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="7c64c-310">練習 3： 將動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="7c64c-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="7c64c-311">在先前的實際操作實驗室**自訂動作篩選條件**您已使用篩選器的自訂或資料隱碼。</span><span class="sxs-lookup"><span data-stu-id="7c64c-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="7c64c-312">在此練習中，您將學習如何使用相依性插入插入篩選器，使用 Unity 容器。</span><span class="sxs-lookup"><span data-stu-id="7c64c-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="7c64c-313">若要這樣做，請將音樂市集 」 解決方案會追蹤活動的站台的自訂動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="7c64c-314">工作 1-包括在方案中的追蹤篩選器</span><span class="sxs-lookup"><span data-stu-id="7c64c-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="7c64c-315">在這個工作中，您會將包含音樂市集的自訂動作篩選條件，以追蹤事件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="7c64c-316">做為自訂動作篩選條件概念已經處理在前一個實驗室&quot;自訂動作篩選條件&quot;，您將只包含這個實驗室中，從 Assets 資料夾的篩選條件類別，然後建立 Unity 篩選條件提供者：</span><span class="sxs-lookup"><span data-stu-id="7c64c-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="7c64c-317">開啟**開始**解決方案位於**Source\Ex03-插入動作 Filter\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c64c-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="7c64c-318">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="7c64c-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c64c-319">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="7c64c-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c64c-320">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c64c-321">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c64c-322">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c64c-323">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="7c64c-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c64c-324">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="7c64c-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c64c-325">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="7c64c-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="7c64c-326">如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="7c64c-327">包含**TraceActionFilter.cs**檔案 **/來源/資產**來 **/篩選**資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c64c-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c64c-328">此自訂動作篩選條件會執行 ASP.NET 追蹤。</span><span class="sxs-lookup"><span data-stu-id="7c64c-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="7c64c-329">您可以檢查&quot;ASP.NET MVC 4 本機和動態動作篩選條件&quot;實驗室以進行更多的參考。</span><span class="sxs-lookup"><span data-stu-id="7c64c-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="7c64c-330">新增空的類別**FilterProvider.cs**資料夾中的專案  **/篩選。**</span><span class="sxs-lookup"><span data-stu-id="7c64c-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="7c64c-331">新增**System.Web.Mvc**並**Microsoft.Practices.Unity**中的命名空間**FilterProvider.cs**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="7c64c-332">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者加入命名空間*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="7c64c-333">讓類別繼承自**IFilterProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="7c64c-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="7c64c-334">新增**IUnityContainer**中的屬性**FilterProvider**類別，並再建立的類別建構函式指派的容器。</span><span class="sxs-lookup"><span data-stu-id="7c64c-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="7c64c-335">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者的建構函式*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c64c-336">不建立篩選條件提供者類別的建構函式**新**物件內。</span><span class="sxs-lookup"><span data-stu-id="7c64c-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="7c64c-337">容器會做為參數，傳遞和 Unity 來解決相依性。</span><span class="sxs-lookup"><span data-stu-id="7c64c-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="7c64c-338">在  **FilterProvider**類別中，實作方法**GetFilters**從**IFilterProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="7c64c-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="7c64c-339">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-篩選條件提供者 GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="7c64c-340">工作 2-註冊和啟用篩選條件</span><span class="sxs-lookup"><span data-stu-id="7c64c-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="7c64c-341">在這個工作中，您將啟用站台的追蹤。</span><span class="sxs-lookup"><span data-stu-id="7c64c-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="7c64c-342">若要這樣做，您將要註冊中的篩選條件**Bootstrapper.cs BuildUnityContainer**方法來啟動追蹤：</span><span class="sxs-lookup"><span data-stu-id="7c64c-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="7c64c-343">開啟**Web.config**位於專案根目錄和啟用追蹤的追蹤在 System.Web 群組。</span><span class="sxs-lookup"><span data-stu-id="7c64c-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="7c64c-344">開啟**Bootstrapper.cs**位於專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="7c64c-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="7c64c-345">將參考加入**MvcMusicStore.Filters**命名空間。</span><span class="sxs-lookup"><span data-stu-id="7c64c-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="7c64c-346">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-啟動載入器加入命名空間*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="7c64c-347">選取  **BuildUnityContainer**方法，並註冊 Unity 容器中的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="7c64c-348">您必須註冊篩選條件提供者，以及動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="7c64c-349">(程式碼片段- *ASP.NET 相依性插入實驗室-Ex03-註冊 FilterProvider 和 ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="7c64c-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7c64c-350">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7c64c-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="7c64c-351">在這個工作中，您將執行應用程式，並測試自訂動作篩選條件追蹤的活動：</span><span class="sxs-lookup"><span data-stu-id="7c64c-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="7c64c-352">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c64c-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="7c64c-353">按一下 [ **Rock**內容類型] 功能表中。</span><span class="sxs-lookup"><span data-stu-id="7c64c-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="7c64c-354">您可以瀏覽至 更多的內容類型如果您想要。</span><span class="sxs-lookup"><span data-stu-id="7c64c-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="7c64c-355">![音樂市集](aspnet-mvc-4-dependency-injection/_static/image11.png "Music 市集")</span><span class="sxs-lookup"><span data-stu-id="7c64c-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="7c64c-356">*Music 市集*</span><span class="sxs-lookup"><span data-stu-id="7c64c-356">*Music Store*</span></span>
3. <span data-ttu-id="7c64c-357">瀏覽至 **/Trace.axd**若要查看應用程式追蹤頁面，然後再按一下**檢視詳細資料**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="7c64c-358">![應用程式追蹤記錄檔](aspnet-mvc-4-dependency-injection/_static/image12.png "應用程式追蹤記錄檔")</span><span class="sxs-lookup"><span data-stu-id="7c64c-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="7c64c-359">*應用程式追蹤記錄檔*</span><span class="sxs-lookup"><span data-stu-id="7c64c-359">*Application Trace Log*</span></span>

    <span data-ttu-id="7c64c-360">![應用程式追蹤-要求詳細資料](aspnet-mvc-4-dependency-injection/_static/image13.png "追蹤應用程式-要求詳細資料")</span><span class="sxs-lookup"><span data-stu-id="7c64c-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="7c64c-361">*應用程式追蹤-要求詳細資料*</span><span class="sxs-lookup"><span data-stu-id="7c64c-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="7c64c-362">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7c64c-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7c64c-363">總結</span><span class="sxs-lookup"><span data-stu-id="7c64c-363">Summary</span></span>

<span data-ttu-id="7c64c-364">藉由完成這個實際操作實驗室中，您已了解如何使用 ASP.NET MVC 4 中的相依性插入，藉由整合 Unity 使用 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7c64c-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="7c64c-365">為了達成此目標，您使用相依性插入控制器、 檢視和動作篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="7c64c-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="7c64c-366">已涵蓋下列概念：</span><span class="sxs-lookup"><span data-stu-id="7c64c-366">The following concepts were covered:</span></span>

- <span data-ttu-id="7c64c-367">ASP.NET MVC 4 相依性插入功能</span><span class="sxs-lookup"><span data-stu-id="7c64c-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="7c64c-368">Unity 整合使用 Unity.Mvc3 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="7c64c-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="7c64c-369">控制器中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="7c64c-370">在檢視中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="7c64c-371">動作篩選條件的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7c64c-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7c64c-372">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="7c64c-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7c64c-373">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="7c64c-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7c64c-374">下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="7c64c-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7c64c-375">移至[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="7c64c-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7c64c-376">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="7c64c-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7c64c-377">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-377">Click on **Install Now**.</span></span> <span data-ttu-id="7c64c-378">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="7c64c-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7c64c-379">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="7c64c-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7c64c-380">![安裝 Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7c64c-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7c64c-381">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7c64c-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7c64c-382">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="7c64c-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="7c64c-384">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="7c64c-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7c64c-385">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="7c64c-385">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="7c64c-387">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="7c64c-387">*Installation progress*</span></span>
6. <span data-ttu-id="7c64c-388">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-388">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="7c64c-390">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="7c64c-390">*Installation completed*</span></span>
7. <span data-ttu-id="7c64c-391">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="7c64c-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7c64c-392">若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。</span><span class="sxs-lookup"><span data-stu-id="7c64c-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 圖格](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="7c64c-394">*VS Express for Web 圖格*</span><span class="sxs-lookup"><span data-stu-id="7c64c-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="7c64c-395">附錄 b： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="7c64c-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="7c64c-396">使用程式碼片段，您會有您需要在隨手可得的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c64c-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7c64c-397">實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="7c64c-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7c64c-398">![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-dependency-injection/_static/image19.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="7c64c-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7c64c-399">*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="7c64c-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7c64c-400">***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="7c64c-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7c64c-401">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c64c-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7c64c-402">開始輸入程式碼片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="7c64c-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7c64c-403">觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="7c64c-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7c64c-404">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="7c64c-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7c64c-405">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="7c64c-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7c64c-406">![開始鍵入程式碼片段名稱](aspnet-mvc-4-dependency-injection/_static/image20.png "開始輸入程式碼片段名稱")</span><span class="sxs-lookup"><span data-stu-id="7c64c-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7c64c-407">*開始鍵入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="7c64c-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="7c64c-408">![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-dependency-injection/_static/image21.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="7c64c-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7c64c-409">*按 Tab 鍵以選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="7c64c-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7c64c-410">![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-dependency-injection/_static/image22.png "再次按 Tab 鍵和程式碼片段會展開")</span><span class="sxs-lookup"><span data-stu-id="7c64c-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7c64c-411">*再次按 Tab 鍵和程式碼片段會展開*</span><span class="sxs-lookup"><span data-stu-id="7c64c-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7c64c-412">***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="7c64c-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7c64c-413">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="7c64c-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7c64c-414">選取 **插入程式碼片段**後面**My Code Snippets**。</span><span class="sxs-lookup"><span data-stu-id="7c64c-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7c64c-415">選擇相關的程式片段，從清單中，對它按一下。</span><span class="sxs-lookup"><span data-stu-id="7c64c-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7c64c-416">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-dependency-injection/_static/image23.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="7c64c-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7c64c-417">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="7c64c-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7c64c-418">![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-dependency-injection/_static/image24.png "對它按一下挑選清單中，相關程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="7c64c-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7c64c-419">*對它按一下挑選清單中，相關程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="7c64c-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
