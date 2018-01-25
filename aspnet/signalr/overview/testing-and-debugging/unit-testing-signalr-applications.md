---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "單元測試 SignalR 應用程式 |Microsoft 文件"
author: pfletcher
description: "本文說明如何使用 SignalR 2.0 的單元測試功能。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="aaf56-103">單元測試的 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="aaf56-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="aaf56-104">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="aaf56-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="aaf56-105">本文說明如何使用 SignalR 2 的單元測試功能。</span><span class="sxs-lookup"><span data-stu-id="aaf56-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="aaf56-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="aaf56-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="aaf56-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="aaf56-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="aaf56-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="aaf56-108">.NET 4.5</span></span>
> - <span data-ttu-id="aaf56-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="aaf56-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="aaf56-110">問題和註解</span><span class="sxs-lookup"><span data-stu-id="aaf56-110">Questions and comments</span></span>
> 
> <span data-ttu-id="aaf56-111">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="aaf56-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="aaf56-112">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="aaf56-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="aaf56-113">單元測試 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="aaf56-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="aaf56-114">您可以使用 SignalR 2 中的單元測試功能，來建立 SignalR 應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="aaf56-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="aaf56-115">SignalR 2 還包含[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)介面，可用來建立模擬物件來模擬測試中樞的方法。</span><span class="sxs-lookup"><span data-stu-id="aaf56-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="aaf56-116">在本節中，您會將應用程式中建立的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用[XUnit.net](https://github.com/xunit/xunit)和[Moq](https://github.com/Moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="aaf56-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="aaf56-117">XUnit.net 會用來控制測試。Moq 將用來建立[模擬](http://en.wikipedia.org/wiki/Mock_object)進行測試的物件。</span><span class="sxs-lookup"><span data-stu-id="aaf56-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="aaf56-118">如果預期; 可以使用其他模擬架構[NSubstitute](http://nsubstitute.github.io/)也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="aaf56-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="aaf56-119">本教學課程示範如何設定兩種方式模擬物件： 首先，使用`dynamic`物件 （.NET Framework 4 中引入） 和第二個，使用的介面。</span><span class="sxs-lookup"><span data-stu-id="aaf56-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="aaf56-120">內容</span><span class="sxs-lookup"><span data-stu-id="aaf56-120">Contents</span></span>

<span data-ttu-id="aaf56-121">本教學課程包含下列各節。</span><span class="sxs-lookup"><span data-stu-id="aaf56-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="aaf56-122">使用動態執行單元測試</span><span class="sxs-lookup"><span data-stu-id="aaf56-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="aaf56-123">單元測試的類型</span><span class="sxs-lookup"><span data-stu-id="aaf56-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="aaf56-124">使用動態執行單元測試</span><span class="sxs-lookup"><span data-stu-id="aaf56-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="aaf56-125">在本節中，您會加入應用程式中建立的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用動態物件。</span><span class="sxs-lookup"><span data-stu-id="aaf56-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="aaf56-126">安裝[XUnit 執行器延伸模組](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)for Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="aaf56-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="aaf56-127">完成[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，或下載完成的應用程式從[MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。</span><span class="sxs-lookup"><span data-stu-id="aaf56-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="aaf56-128">如果您使用下載的入門應用程式版本，開啟**Package Manager Console**按一下**還原**SignalR 封裝加入專案。</span><span class="sxs-lookup"><span data-stu-id="aaf56-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![還原封裝](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="aaf56-130">將專案加入至單元測試的方案。</span><span class="sxs-lookup"><span data-stu-id="aaf56-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="aaf56-131">以滑鼠右鍵按一下方案中的**方案總管 中**選取**新增**，**新的專案...**.在下**C#**節點中，選取**Windows**節點。</span><span class="sxs-lookup"><span data-stu-id="aaf56-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="aaf56-132">選取**類別庫**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-132">Select **Class Library**.</span></span> <span data-ttu-id="aaf56-133">將新專案**TestLibrary**按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![建立測試程式庫](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="aaf56-135">在測試程式庫專案中加入 SignalRChat 專案的參考。</span><span class="sxs-lookup"><span data-stu-id="aaf56-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="aaf56-136">以滑鼠右鍵按一下**TestLibrary**專案，然後選取**新增**，**參考...**.選取**專案**節點下的**方案**節點，然後核取**SignalRChat**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="aaf56-137">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="aaf56-137">Click **OK**.</span></span>

    ![加入專案參考](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="aaf56-139">將 SignalR、 Moq 和 XUnit 封裝加入**TestLibrary**專案。</span><span class="sxs-lookup"><span data-stu-id="aaf56-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="aaf56-140">在**Package Manager Console**，將**預設專案**下拉式清單來**TestLibrary**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="aaf56-141">在主控台視窗中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="aaf56-141">Run the following commands in the console window:</span></span>

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![安裝封裝](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="aaf56-143">建立測試檔案。</span><span class="sxs-lookup"><span data-stu-id="aaf56-143">Create the test file.</span></span> <span data-ttu-id="aaf56-144">以滑鼠右鍵按一下**TestLibrary**專案，然後按一下**新增...**，**類別**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="aaf56-145">將新類別**Tests.cs**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="aaf56-146">Tests.cs 的內容取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="aaf56-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="aaf56-147">在上述程式碼測試用戶端會建立使用`Mock`物件從[Moq](https://github.com/Moq/moq4)型別的文件庫[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`類型參數。）`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。</span><span class="sxs-lookup"><span data-stu-id="aaf56-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="aaf56-148">`broadcastMessage`以便可由呼叫然後函式定義模擬用戶端`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="aaf56-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="aaf56-149">測試引擎會呼叫`Send`方法`ChatHub`類別，依序呼叫 模擬`broadcastMessage`函式。</span><span class="sxs-lookup"><span data-stu-id="aaf56-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="aaf56-150">按下建置此方案**F6**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="aaf56-151">執行單元測試。</span><span class="sxs-lookup"><span data-stu-id="aaf56-151">Run the unit test.</span></span> <span data-ttu-id="aaf56-152">在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="aaf56-153">在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic**選取**執行選取的測試**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![測試總管](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="aaf56-155">請確認測試成功藉由在測試總管 視窗的下方窗格。</span><span class="sxs-lookup"><span data-stu-id="aaf56-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="aaf56-156">視窗會顯示測試成功。</span><span class="sxs-lookup"><span data-stu-id="aaf56-156">The window will show that the test passed.</span></span>

    ![成功的測試](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="aaf56-158">單元測試的類型</span><span class="sxs-lookup"><span data-stu-id="aaf56-158">Unit testing by type</span></span>

<span data-ttu-id="aaf56-159">在本節中，您要加入的測試中建立的應用程式[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用包含要測試之方法的介面。</span><span class="sxs-lookup"><span data-stu-id="aaf56-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="aaf56-160">完成步驟 1-7[單元測試與動態](#dynamic)上述的教學課程。</span><span class="sxs-lookup"><span data-stu-id="aaf56-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="aaf56-161">Tests.cs 的內容取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="aaf56-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="aaf56-162">上述程式碼，已建立介面定義的簽章`broadcastMessage`測試引擎會建立模擬用戶端的方法。</span><span class="sxs-lookup"><span data-stu-id="aaf56-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="aaf56-163">模擬用戶端接著會建立使用`Mock`型別的物件[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`型別參數。)`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。</span><span class="sxs-lookup"><span data-stu-id="aaf56-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="aaf56-164">測試接著會建立的執行個體`ChatHub`，然後建立的模擬版本`broadcastMessage`方法中，依序叫用呼叫`Send`集線器上的方法。</span><span class="sxs-lookup"><span data-stu-id="aaf56-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="aaf56-165">按下建置此方案**F6**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="aaf56-166">執行單元測試。</span><span class="sxs-lookup"><span data-stu-id="aaf56-166">Run the unit test.</span></span> <span data-ttu-id="aaf56-167">在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="aaf56-168">在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic**選取**執行選取的測試**。</span><span class="sxs-lookup"><span data-stu-id="aaf56-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![測試總管](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="aaf56-170">請確認測試成功藉由在測試總管 視窗的下方窗格。</span><span class="sxs-lookup"><span data-stu-id="aaf56-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="aaf56-171">視窗會顯示測試成功。</span><span class="sxs-lookup"><span data-stu-id="aaf56-171">The window will show that the test passed.</span></span>

    ![成功的測試](unit-testing-signalr-applications/_static/image8.png)
