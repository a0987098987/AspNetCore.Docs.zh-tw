---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: 與 SignalR 的頻率高的即時 1.x |Microsoft 文件
author: pfletcher
description: 本教學課程會示範如何建立 web 應用程式使用 ASP.NET SignalR 提供高頻率的傳訊功能。 頻率高的內送訊息...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="high-frequency-realtime-with-signalr-1x"></a><span data-ttu-id="0ed81-104">與 SignalR 的頻率高的即時 1.x</span><span class="sxs-lookup"><span data-stu-id="0ed81-104">High-Frequency Realtime with SignalR 1.x</span></span>
====================
<span data-ttu-id="0ed81-105">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="0ed81-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="0ed81-106">本教學課程會示範如何建立 web 應用程式使用 ASP.NET SignalR 提供高頻率的傳訊功能。</span><span class="sxs-lookup"><span data-stu-id="0ed81-106">This tutorial shows how to create a web application that uses ASP.NET SignalR to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="0ed81-107">在此情況下高頻率訊息表示傳送以固定費率; 的更新如果這個應用程式，最多 10 個第二個訊息。</span><span class="sxs-lookup"><span data-stu-id="0ed81-107">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
> 
> <span data-ttu-id="0ed81-108">您會在本教學課程中建立應用程式會顯示使用者可以拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="0ed81-108">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="0ed81-109">所有其他連接的瀏覽器中圖形的位置就會更新以符合使用定時的更新 「 拖曳 」 圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="0ed81-109">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
> 
> <span data-ttu-id="0ed81-110">此教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-110">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
> 
> <span data-ttu-id="0ed81-111">在本教學課程的註解是歡迎畫面。</span><span class="sxs-lookup"><span data-stu-id="0ed81-111">Comments on the tutorial are welcome.</span></span> <span data-ttu-id="0ed81-112">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com)。</span><span class="sxs-lookup"><span data-stu-id="0ed81-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com).</span></span>


## <a name="overview"></a><span data-ttu-id="0ed81-113">概觀</span><span class="sxs-lookup"><span data-stu-id="0ed81-113">Overview</span></span>

<span data-ttu-id="0ed81-114">本教學課程會示範如何建立共用物件的狀態與即時其他瀏覽器的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-114">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="0ed81-115">我們將建立的應用程式會呼叫 MoveShape。</span><span class="sxs-lookup"><span data-stu-id="0ed81-115">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="0ed81-116">MoveShape 頁面會顯示使用者可以拖曳; HTML Div 項目當使用者拖曳 Div，新的位置將傳送到伺服器，就會通知所有其他連線的用戶端更新以符合圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="0ed81-116">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="0ed81-118">在本教學課程所建立的應用程式根據由 Damian Edwards 示範。</span><span class="sxs-lookup"><span data-stu-id="0ed81-118">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="0ed81-119">包含此示範影片可以看到[這裡](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。</span><span class="sxs-lookup"><span data-stu-id="0ed81-119">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="0ed81-120">開始本教學課程會示範如何從每個引發之事件的圖形拖曳時傳送 SignalR 訊息。</span><span class="sxs-lookup"><span data-stu-id="0ed81-120">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="0ed81-121">每個連線的用戶端會更新位置的本機版本 」 圖形的每次收到一則訊息。</span><span class="sxs-lookup"><span data-stu-id="0ed81-121">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="0ed81-122">應用程式將使用此方法來運作，而這不建議使用的程式設計模型，因為有可能取得傳送，因此用戶端與伺服器無法取得爆訊息，並會降低效能的訊息數目沒有上限.</span><span class="sxs-lookup"><span data-stu-id="0ed81-122">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="0ed81-123">脫離，圖形會立即移動的每個方法，而非移動到每個新的位置順暢地也是在用戶端上顯示的動畫。</span><span class="sxs-lookup"><span data-stu-id="0ed81-123">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="0ed81-124">本教學課程稍後的章節將示範如何建立會限制用戶端或伺服器所傳送訊息的最大速率計時器函式，以及如何順暢移動位置之間的圖形。</span><span class="sxs-lookup"><span data-stu-id="0ed81-124">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="0ed81-125">在本教學課程所建立的應用程式的最終版本可以從下載[Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。</span><span class="sxs-lookup"><span data-stu-id="0ed81-125">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).</span></span>

<span data-ttu-id="0ed81-126">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="0ed81-126">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="0ed81-127">必要條件</span><span class="sxs-lookup"><span data-stu-id="0ed81-127">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="0ed81-128">建立專案</span><span class="sxs-lookup"><span data-stu-id="0ed81-128">Create the project</span></span>](#createtheproject)
- [<span data-ttu-id="0ed81-129">新增 ASP.NET SignalR 和 JQuery.UI NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="0ed81-129">Add the ASP.NET SignalR and JQuery.UI NuGet packages</span></span>](#nugetpackages)
- [<span data-ttu-id="0ed81-130">建立基底應用程式</span><span class="sxs-lookup"><span data-stu-id="0ed81-130">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="0ed81-131">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="0ed81-131">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="0ed81-132">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="0ed81-132">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="0ed81-133">用戶端上新增 smooth 動畫</span><span class="sxs-lookup"><span data-stu-id="0ed81-133">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="0ed81-134">進一步的步驟</span><span class="sxs-lookup"><span data-stu-id="0ed81-134">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="0ed81-135">必要條件</span><span class="sxs-lookup"><span data-stu-id="0ed81-135">Prerequisites</span></span>

<span data-ttu-id="0ed81-136">本教學課程需要 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="0ed81-136">This tutorial requires Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="0ed81-137">如果使用 Visual Studio 2010，則專案會使用.NET Framework 4，而不是.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="0ed81-137">If Visual Studio 2010 is used, the project will use .NET Framework 4 rather than .NET Framework 4.5.</span></span>

<span data-ttu-id="0ed81-138">如果您使用 Visual Studio 2012，建議您安裝[ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。</span><span class="sxs-lookup"><span data-stu-id="0ed81-138">If you are using Visual Studio 2012, it's recommended that you install the [ASP.NET and Web Tools 2012.2 update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="0ed81-139">此更新包含新功能，例如發行，新功能的增強功能和新的範本。</span><span class="sxs-lookup"><span data-stu-id="0ed81-139">This update contains new features such as enhancements to publishing, new functionality, and new templates.</span></span>

<span data-ttu-id="0ed81-140">如果您有 Visual Studio 2010，請確定[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安裝。</span><span class="sxs-lookup"><span data-stu-id="0ed81-140">If you have Visual Studio 2010, make sure that [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) is installed.</span></span>

<a id="createtheproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="0ed81-141">建立專案</span><span class="sxs-lookup"><span data-stu-id="0ed81-141">Create the project</span></span>

<span data-ttu-id="0ed81-142">在本節中，我們將建立在 Visual Studio 中的專案。</span><span class="sxs-lookup"><span data-stu-id="0ed81-142">In this section, we'll create the project in Visual Studio.</span></span>

1. <span data-ttu-id="0ed81-143">從**檔案**功能表上，按一下**新專案**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-143">From the **File** menu click **New Project**.</span></span>
2. <span data-ttu-id="0ed81-144">在**新專案**對話方塊方塊中，展開  **C#** 下**範本**選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-144">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="0ed81-145">選取**ASP.NET 空 Web 應用程式**範本，將專案*MoveShapeDemo*，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-145">Select the **ASP.NET Empty Web Application** template, name the project *MoveShapeDemo*, and click **OK**.</span></span>

    ![建立新的專案](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a><span data-ttu-id="0ed81-147">將 SignalR 和 JQuery.UI NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="0ed81-147">Add the SignalR and JQuery.UI NuGet Packages</span></span>

<span data-ttu-id="0ed81-148">您可以將加入專案的 SignalR 功能安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0ed81-148">You can add SignalR functionality to a project by installing a NuGet package.</span></span> <span data-ttu-id="0ed81-149">本教學課程也會使用 JQuery.UI 封裝的允許拖曳並繪製圖形。</span><span class="sxs-lookup"><span data-stu-id="0ed81-149">This tutorial will also use the JQuery.UI package for allowing the shape to be dragged and animated.</span></span>

1. <span data-ttu-id="0ed81-150">按一下**工具 |程式庫套件管理員 |Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-150">Click **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="0ed81-151">封裝管理員中，輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="0ed81-151">Enter the following command in the package manager.</span></span>

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    <span data-ttu-id="0ed81-152">SignalR 套件會安裝其他的 NuGet 套件的數字，做為相依性。</span><span class="sxs-lookup"><span data-stu-id="0ed81-152">The SignalR package installs a number of other NuGet packages as dependencies.</span></span> <span data-ttu-id="0ed81-153">安裝完成時可能會有所有 SignalR 使用 ASP.NET 應用程式所需的伺服器和用戶端元件。</span><span class="sxs-lookup"><span data-stu-id="0ed81-153">When the installation is finished you have all of the server and client components required to use SignalR in an ASP.NET application.</span></span>
3. <span data-ttu-id="0ed81-154">輸入下列命令到封裝管理員主控台安裝 JQuery 和 JQuery.UI 的封裝。</span><span class="sxs-lookup"><span data-stu-id="0ed81-154">Enter the following command into the package manager console to install the JQuery and JQuery.UI packages.</span></span>

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="0ed81-155">建立基底應用程式</span><span class="sxs-lookup"><span data-stu-id="0ed81-155">Create the base application</span></span>

<span data-ttu-id="0ed81-156">在本節中，我們將建立每個滑鼠移動事件期間，將圖形的位置傳送到伺服器的瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-156">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="0ed81-157">伺服器再廣播給所有其他連接的用戶端的這項資訊在接收時。</span><span class="sxs-lookup"><span data-stu-id="0ed81-157">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="0ed81-158">我們將在稍後的章節中的這個應用程式上展開。</span><span class="sxs-lookup"><span data-stu-id="0ed81-158">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="0ed81-159">在**方案總管 中**，以滑鼠右鍵按一下專案，然後選取**新增**，**類別...**.將類別**MoveShapeHub**按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-159">In **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="0ed81-160">在新的程式碼取代**MoveShapeHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="0ed81-160">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    <span data-ttu-id="0ed81-161">`MoveShapeHub`上述類別是 SignalR 中樞的實作。</span><span class="sxs-lookup"><span data-stu-id="0ed81-161">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="0ed81-162">在[入門 SignalR](index.md)教學課程中，中樞包含用戶端會直接呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="0ed81-162">As in the [Getting Started with SignalR](index.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="0ed81-163">在此情況下，用戶端會傳送物件，其中包含新的伺服器，然後取得傳播到所有其他連線的用戶端 圖形的 X 和 Y 座標。</span><span class="sxs-lookup"><span data-stu-id="0ed81-163">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="0ed81-164">SignalR 將自動序列化這個物件使用 JSON。</span><span class="sxs-lookup"><span data-stu-id="0ed81-164">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="0ed81-165">將傳送至用戶端的物件 (`ShapeModel`) 包含要儲存的位置圖形的成員。</span><span class="sxs-lookup"><span data-stu-id="0ed81-165">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="0ed81-166">在伺服器上之物件的版本也包含成員，才能追蹤用戶端的資料儲存，，以便他們自己的資料將不會傳送指定的用戶端。</span><span class="sxs-lookup"><span data-stu-id="0ed81-166">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="0ed81-167">這個成員會使用`JsonIgnore`屬性，以防止被序列化並傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="0ed81-167">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>
3. <span data-ttu-id="0ed81-168">接下來，我們會設定中樞應用程式啟動時。</span><span class="sxs-lookup"><span data-stu-id="0ed81-168">Next, we'll set up the hub when the application starts.</span></span> <span data-ttu-id="0ed81-169">在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |全域應用程式類別**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-169">In **Solution Explorer**, right-click the project, then click **Add | Global Application Class**.</span></span> <span data-ttu-id="0ed81-170">接受預設名稱*Global*按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-170">Accept the default name of *Global* and click **OK**.</span></span>

    ![加入全域應用程式類別](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. <span data-ttu-id="0ed81-172">加入下列`using`陳述式之後提供**使用**Global.asax.cs 類別中的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-172">Add the following `using` statement after the provided **using** statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. <span data-ttu-id="0ed81-173">加入下列行中的程式碼`Application_Start`註冊預設路由 SignalR 的通用類別的方法。</span><span class="sxs-lookup"><span data-stu-id="0ed81-173">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="0ed81-174">Global.asax 檔案看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="0ed81-174">Your global.asax file should look like the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. <span data-ttu-id="0ed81-175">接下來，我們要加入用戶端。</span><span class="sxs-lookup"><span data-stu-id="0ed81-175">Next, we'll add the client.</span></span> <span data-ttu-id="0ed81-176">在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |新項目**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-176">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="0ed81-177">在**加入新項目**對話方塊中，選取**Html 網頁**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-177">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="0ed81-178">指定頁面的適當名稱 (例如**Default.html**) 按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-178">Give the page an appropriate name (like **Default.html**) and click **Add**.</span></span>
7. <span data-ttu-id="0ed81-179">在**方案總管 中**，以滑鼠右鍵按一下您剛建立的網頁，然後按一下**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="0ed81-179">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
8. <span data-ttu-id="0ed81-180">HTML 網頁中的預設程式碼取代為下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="0ed81-180">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ed81-181">確認指令碼參考以下相符項目新增至指令碼 資料夾中的專案的套件。</span><span class="sxs-lookup"><span data-stu-id="0ed81-181">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span> <span data-ttu-id="0ed81-182">在 Visual Studio 2010、 JQuery 和 SignalR 加入至專案的版本可能不符合版本編號。</span><span class="sxs-lookup"><span data-stu-id="0ed81-182">In Visual Studio 2010, the version of JQuery and SignalR added to the project may not match the version numbers below.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    <span data-ttu-id="0ed81-183">上述的 HTML 和 JavaScript 程式碼會建立稱為圖形紅色 Div、 啟用圖形的拖曳行為使用 jQuery 程式庫，並使用圖形的`drag`事件傳送至伺服器的圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="0ed81-183">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
9. <span data-ttu-id="0ed81-184">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-184">Start the application by pressing F5.</span></span> <span data-ttu-id="0ed81-185">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0ed81-185">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0ed81-186">將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0ed81-186">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="0ed81-188">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="0ed81-188">Add the client loop</span></span>

<span data-ttu-id="0ed81-189">由於傳送上每個滑鼠移動事件圖形的位置將會建立不需要網路傳輸量，從用戶端的訊息必須進行節流處理。</span><span class="sxs-lookup"><span data-stu-id="0ed81-189">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="0ed81-190">我們會使用 javascript`setInterval`函式來設定新的位置資訊傳送到伺服器時以固定費率的迴圈。</span><span class="sxs-lookup"><span data-stu-id="0ed81-190">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="0ed81-191">這個迴圈是 「 遊戲迴圈 」，重複呼叫的函式的磁碟機的所有遊戲或其他的模擬功能非常基本表示法。</span><span class="sxs-lookup"><span data-stu-id="0ed81-191">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="0ed81-192">更新以符合下列程式碼片段的 HTML 網頁上的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="0ed81-192">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    <span data-ttu-id="0ed81-193">上述的更新將新增`updateServerModel`函式，以取得在固定頻率上呼叫。</span><span class="sxs-lookup"><span data-stu-id="0ed81-193">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="0ed81-194">此函式會將位置資料傳送到伺服器時`moved`旗標，表示沒有要傳送的新位置資料。</span><span class="sxs-lookup"><span data-stu-id="0ed81-194">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="0ed81-195">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-195">Start the application by pressing F5.</span></span> <span data-ttu-id="0ed81-196">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0ed81-196">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0ed81-197">將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0ed81-197">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="0ed81-198">取得傳送到伺服器的訊息數目進行節流處理，因為動畫不會出現能順利，如前一節所示。</span><span class="sxs-lookup"><span data-stu-id="0ed81-198">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="0ed81-200">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="0ed81-200">Add the server loop</span></span>

<span data-ttu-id="0ed81-201">在目前的應用程式，從伺服器傳送至用戶端的訊息移通常會將收到。</span><span class="sxs-lookup"><span data-stu-id="0ed81-201">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="0ed81-202">這顯現出驗證類似的問題已在用戶端所示超過所需，而且連接可能會變得湧入因此通常可以傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="0ed81-202">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="0ed81-203">本節說明如何更新伺服器實作的計時器，節流處理外寄訊息的速率。</span><span class="sxs-lookup"><span data-stu-id="0ed81-203">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="0ed81-204">取代內容`MoveShapeHub.cs`藉由下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="0ed81-204">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    <span data-ttu-id="0ed81-205">上述程式碼會展開，以新增用戶端`Broadcaster`類別，節流處理外寄訊息使用`Timer`從.NET framework 類別。</span><span class="sxs-lookup"><span data-stu-id="0ed81-205">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="0ed81-206">由於中樞本身是暫時性 （它會建立每次需要它），`Broadcaster`會建立為單一值。</span><span class="sxs-lookup"><span data-stu-id="0ed81-206">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="0ed81-207">延遲初始設定 （.NET 4 中引進） 用來延遲其建立之前，確保在計時器啟動之前，完全建立第一個中樞執行個體。</span><span class="sxs-lookup"><span data-stu-id="0ed81-207">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="0ed81-208">用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法，所以它不再接收內送訊息時，立即呼叫。</span><span class="sxs-lookup"><span data-stu-id="0ed81-208">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="0ed81-209">相反地，將傳送給用戶端的訊息速率為 25 秒的呼叫受`_broadcastLoop`計時器從`Broadcaster`類別。</span><span class="sxs-lookup"><span data-stu-id="0ed81-209">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="0ed81-210">最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的中樞的參考 (`_hubContext`) 使用`GlobalHost`。</span><span class="sxs-lookup"><span data-stu-id="0ed81-210">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="0ed81-211">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-211">Start the application by pressing F5.</span></span> <span data-ttu-id="0ed81-212">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0ed81-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0ed81-213">將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0ed81-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="0ed81-214">不會從上一節中，瀏覽器中看到的差別，但是會被調整到用戶端傳送的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="0ed81-214">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="0ed81-216">用戶端上新增 smooth 動畫</span><span class="sxs-lookup"><span data-stu-id="0ed81-216">Add smooth animation on the client</span></span>

<span data-ttu-id="0ed81-217">應用程式幾乎完成，但我們無法讓一個詳細改進，圖形用戶端上的影片中移動以回應伺服器訊息。</span><span class="sxs-lookup"><span data-stu-id="0ed81-217">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="0ed81-218">而不是將圖形的位置設定為給定伺服器的新位置中，我們將使用 JQuery UI 程式庫的`animate`順暢移動圖形，它目前和新的位置之間的函式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-218">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="0ed81-219">更新用戶端`updateShape`方法，以便尋找類似以下的反白顯示程式碼：</span><span class="sxs-lookup"><span data-stu-id="0ed81-219">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    <span data-ttu-id="0ed81-220">上述程式碼會將圖形從舊位置移到新伺服器所提供的動畫間隔 （此案例中是 100 毫秒） 的過程。</span><span class="sxs-lookup"><span data-stu-id="0ed81-220">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="0ed81-221">新的動畫開始之前，會清除在圖形上執行任何先前動畫。</span><span class="sxs-lookup"><span data-stu-id="0ed81-221">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="0ed81-222">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-222">Start the application by pressing F5.</span></span> <span data-ttu-id="0ed81-223">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0ed81-223">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0ed81-224">將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0ed81-224">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="0ed81-225">其他視窗中的圖形的移動應該會出現較不為一段時間，而非一次設定每個內送訊息會以內插值取代其移動不穩定。</span><span class="sxs-lookup"><span data-stu-id="0ed81-225">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="0ed81-227">進一步的步驟</span><span class="sxs-lookup"><span data-stu-id="0ed81-227">Further Steps</span></span>

<span data-ttu-id="0ed81-228">在本教學課程中，您學到如何進行用戶端與伺服器之間的頻率高的訊息將傳送的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ed81-228">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="0ed81-229">此通訊範例可用於開發線上遊戲和其他模擬，例如[ShootR 遊戲建立與 SignalR](http://shootr.signalr.net)。</span><span class="sxs-lookup"><span data-stu-id="0ed81-229">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](http://shootr.signalr.net).</span></span>

<span data-ttu-id="0ed81-230">完成本教學課程建立的應用程式可以從下載[Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。</span><span class="sxs-lookup"><span data-stu-id="0ed81-230">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).</span></span>

<span data-ttu-id="0ed81-231">若要了解有關 SignalR 開發概念的詳細資訊，請瀏覽下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="0ed81-231">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="0ed81-232">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="0ed81-232">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="0ed81-233">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="0ed81-233">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="0ed81-234">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="0ed81-234">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
