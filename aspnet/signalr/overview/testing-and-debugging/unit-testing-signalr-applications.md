---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: 單元測試 SignalR 應用程式 |Microsoft Docs
author: pfletcher
description: 本文說明如何使用 SignalR 2.0 的單元測試功能。
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: f46c6edd2002bcc6e62f4e4fe313c9e50e35aaff
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840901"
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="00949-103">單元測試的 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="00949-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="00949-104">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="00949-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="00949-105">這篇文章說明如何使用 SignalR 2 的單元測試功能。</span><span class="sxs-lookup"><span data-stu-id="00949-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="00949-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="00949-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="00949-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="00949-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="00949-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="00949-108">.NET 4.5</span></span>
> - <span data-ttu-id="00949-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="00949-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="00949-110">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="00949-110">Questions and comments</span></span>
> 
> <span data-ttu-id="00949-111">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="00949-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="00949-112">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="00949-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="00949-113">單元測試 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="00949-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="00949-114">您可以使用 SignalR 2 中的單元測試功能，建立您的 SignalR 應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="00949-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="00949-115">SignalR 2 包含[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)介面，可用來建立模擬 （mock） 物件，以模擬您的中樞方法的測試。</span><span class="sxs-lookup"><span data-stu-id="00949-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="00949-116">在本節中，您將建立的應用程式的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用[XUnit.net](https://github.com/xunit/xunit)並[Moq](https://github.com/Moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="00949-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="00949-117">XUnit.net 會用來控制測試;將用來建立的 Moq[模擬](http://en.wikipedia.org/wiki/Mock_object)進行測試的物件。</span><span class="sxs-lookup"><span data-stu-id="00949-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="00949-118">如有需要，可以使用其他 （mock） 架構[NSubstitute](http://nsubstitute.github.io/)也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="00949-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="00949-119">本教學課程示範如何設定兩種方式的模擬 （mock） 物件： 首先，使用`dynamic`物件 （.NET Framework 4 中引入） 和第二，使用的介面。</span><span class="sxs-lookup"><span data-stu-id="00949-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="00949-120">內容</span><span class="sxs-lookup"><span data-stu-id="00949-120">Contents</span></span>

<span data-ttu-id="00949-121">本教學課程包含下列各節。</span><span class="sxs-lookup"><span data-stu-id="00949-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="00949-122">動態使用單元測試</span><span class="sxs-lookup"><span data-stu-id="00949-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="00949-123">單元測試類型</span><span class="sxs-lookup"><span data-stu-id="00949-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="00949-124">動態使用單元測試</span><span class="sxs-lookup"><span data-stu-id="00949-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="00949-125">在本節中，您將建立的應用程式的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用動態物件。</span><span class="sxs-lookup"><span data-stu-id="00949-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="00949-126">安裝[XUnit 執行器延伸模組](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)適用於 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="00949-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="00949-127">請完成[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，或下載已完成的應用程式，從[MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。</span><span class="sxs-lookup"><span data-stu-id="00949-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="00949-128">如果您使用下載的快速入門應用程式版本，開啟**Package Manager Console**然後按一下**還原**SignalR 封裝加入專案。</span><span class="sxs-lookup"><span data-stu-id="00949-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![還原套件](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="00949-130">將專案加入至單元測試的方案。</span><span class="sxs-lookup"><span data-stu-id="00949-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="00949-131">以滑鼠右鍵按一下 在您的解決方案**方案總管**，然後選取**新增**，**新專案...**.底下**C#** 節點中，選取**Windows**節點。</span><span class="sxs-lookup"><span data-stu-id="00949-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="00949-132">選取 **類別庫**。</span><span class="sxs-lookup"><span data-stu-id="00949-132">Select **Class Library**.</span></span> <span data-ttu-id="00949-133">將新專案命名**TestLibrary**然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="00949-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![建立測試程式庫](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="00949-135">在測試程式庫專案中加入 SignalRChat 專案的參考。</span><span class="sxs-lookup"><span data-stu-id="00949-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="00949-136">以滑鼠右鍵按一下**TestLibrary**專案，然後選取**新增**，**參考...**.選取 **專案**下方的節點**解決方案**節點，然後核取**SignalRChat**。</span><span class="sxs-lookup"><span data-stu-id="00949-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="00949-137">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="00949-137">Click **OK**.</span></span>

    ![加入專案參考](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="00949-139">將 SignalR、 Moq 和 XUnit 的封裝，加入**TestLibrary**專案。</span><span class="sxs-lookup"><span data-stu-id="00949-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="00949-140">在  **Package Manager Console**，將**預設專案**下拉式清單來**TestLibrary**。</span><span class="sxs-lookup"><span data-stu-id="00949-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="00949-141">在主控台視窗中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="00949-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![安裝套件](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="00949-143">建立測試檔案。</span><span class="sxs-lookup"><span data-stu-id="00949-143">Create the test file.</span></span> <span data-ttu-id="00949-144">以滑鼠右鍵按一下**TestLibrary**專案，然後按一下 **加入...**，**類別**。</span><span class="sxs-lookup"><span data-stu-id="00949-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="00949-145">新類別命名**Tests.cs**。</span><span class="sxs-lookup"><span data-stu-id="00949-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="00949-146">Tests.cs 的內容取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="00949-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="00949-147">在上述程式碼，測試用戶端會使用建立`Mock`物件從[Moq](https://github.com/Moq/moq4)類型的文件庫[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`類型參數。）`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。</span><span class="sxs-lookup"><span data-stu-id="00949-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="00949-148">`broadcastMessage`函式會接著定義模擬 （mock） 的用戶端，讓它可以呼叫`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="00949-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="00949-149">測試引擎接著會呼叫`Send`方法`ChatHub`類別，而呼叫模擬 （mock）`broadcastMessage`函式。</span><span class="sxs-lookup"><span data-stu-id="00949-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="00949-150">建置方案，藉由按下**F6**。</span><span class="sxs-lookup"><span data-stu-id="00949-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="00949-151">執行單元測試。</span><span class="sxs-lookup"><span data-stu-id="00949-151">Run the unit test.</span></span> <span data-ttu-id="00949-152">在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。</span><span class="sxs-lookup"><span data-stu-id="00949-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="00949-153">在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic** ，然後選取**執行選取的測試**。</span><span class="sxs-lookup"><span data-stu-id="00949-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![測試總管](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="00949-155">請確認測試成功藉由檢查下方的窗格，在 [測試總管] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="00949-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="00949-156">視窗會顯示測試成功。</span><span class="sxs-lookup"><span data-stu-id="00949-156">The window will show that the test passed.</span></span>

    ![成功的測試](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="00949-158">單元測試類型</span><span class="sxs-lookup"><span data-stu-id="00949-158">Unit testing by type</span></span>

<span data-ttu-id="00949-159">在本節中，您將建立的應用程式的測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用介面，包含要測試的方法。</span><span class="sxs-lookup"><span data-stu-id="00949-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="00949-160">完成步驟 1-7 中[單元測試與 Dynamic](#dynamic)上述的教學課程。</span><span class="sxs-lookup"><span data-stu-id="00949-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="00949-161">Tests.cs 的內容取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="00949-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="00949-162">在上述程式碼會建立介面定義的簽章`broadcastMessage`測試引擎會建立模擬 （mock） 的用戶端的方法。</span><span class="sxs-lookup"><span data-stu-id="00949-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="00949-163">模擬 （mock） 的用戶端接著會建立使用`Mock`類型的物件[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`做為 type 參數。)`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。</span><span class="sxs-lookup"><span data-stu-id="00949-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="00949-164">測試再建立的執行個體`ChatHub`，然後建立模擬版本`broadcastMessage`方法中，依序叫用呼叫`Send`集線器上的方法。</span><span class="sxs-lookup"><span data-stu-id="00949-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="00949-165">建置方案，藉由按下**F6**。</span><span class="sxs-lookup"><span data-stu-id="00949-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="00949-166">執行單元測試。</span><span class="sxs-lookup"><span data-stu-id="00949-166">Run the unit test.</span></span> <span data-ttu-id="00949-167">在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。</span><span class="sxs-lookup"><span data-stu-id="00949-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="00949-168">在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic** ，然後選取**執行選取的測試**。</span><span class="sxs-lookup"><span data-stu-id="00949-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![測試總管](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="00949-170">請確認測試成功藉由檢查下方的窗格，在 [測試總管] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="00949-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="00949-171">視窗會顯示測試成功。</span><span class="sxs-lookup"><span data-stu-id="00949-171">The window will show that the test passed.</span></span>

    ![成功的測試](unit-testing-signalr-applications/_static/image8.png)
