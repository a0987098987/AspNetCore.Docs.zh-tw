---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: 高頻率即時與 SignalR 1.x |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。 高頻率訊息處理中...
ms.author: riande
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 53cc35d819c0d3a9bd84e8bfc44098a3b62e6db3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832641"
---
<a name="high-frequency-realtime-with-signalr-1x"></a><span data-ttu-id="0e0f9-104">高頻率即時與 SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="0e0f9-104">High-Frequency Realtime with SignalR 1.x</span></span>
====================
<span data-ttu-id="0e0f9-105">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="0e0f9-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="0e0f9-106">本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-106">This tutorial shows how to create a web application that uses ASP.NET SignalR to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="0e0f9-107">在此情況下頻繁訊息表示會以固定費率; 的更新在此應用程式，最多 10 個訊息，第二個。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-107">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
> 
> <span data-ttu-id="0e0f9-108">您會在本教學課程中建立應用程式會顯示使用者可以拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-108">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="0e0f9-109">圖形的位置，所有其他已連線的瀏覽器中就會更新以符合使用定時的更新拖曳圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-109">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
> 
> <span data-ttu-id="0e0f9-110">本教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-110">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
> 
> <span data-ttu-id="0e0f9-111">在本教學課程的註解是歡迎畫面。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-111">Comments on the tutorial are welcome.</span></span> <span data-ttu-id="0e0f9-112">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com)。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com).</span></span>


## <a name="overview"></a><span data-ttu-id="0e0f9-113">總覽</span><span class="sxs-lookup"><span data-stu-id="0e0f9-113">Overview</span></span>

<span data-ttu-id="0e0f9-114">本教學課程會示範如何建立與其他瀏覽器即時分享物件狀態的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-114">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="0e0f9-115">我們將建立的應用程式稱為 MoveShape。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-115">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="0e0f9-116">MoveShape 頁面會顯示使用者可以拖曳; HTML Div 項目當使用者拖曳 Div，則其新位置將傳送到伺服器，然後就可以知道所有其他連線的用戶端更新以符合圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-116">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="0e0f9-118">在本教學課程所建立的應用程式根據示範中，由 Damian Edwards 主講。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-118">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="0e0f9-119">可以看到包含這段示範影片的影片[此處](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-119">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="0e0f9-120">本教學課程會示範如何從每個引發拖曳圖形的事件傳送 SignalR 訊息啟動。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-120">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="0e0f9-121">每個連線的用戶端會更新圖形的本機版本的位置收到訊息的每一次。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-121">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="0e0f9-122">儘管應用程式將使用此方法來運作，這不是建議的程式設計模型，因為會開始傳送，因此用戶端和伺服器無法取得應付訊息，並會降低效能的訊息數目沒有上限.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-122">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="0e0f9-123">脫離，圖形會立即移動每個方法，由，而不是移動順暢到每個新的位置，也會是在用戶端上顯示的動畫。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-123">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="0e0f9-124">本教學課程後面幾節將示範如何建立計時器函式，將由用戶端或伺服器，傳送的訊息最大速率限制以及如何將圖案移動位置之間的順暢。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-124">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="0e0f9-125">在本教學課程所建立的應用程式的最終版本可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-125">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).</span></span>

<span data-ttu-id="0e0f9-126">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="0e0f9-126">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="0e0f9-127">必要條件</span><span class="sxs-lookup"><span data-stu-id="0e0f9-127">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="0e0f9-128">建立專案</span><span class="sxs-lookup"><span data-stu-id="0e0f9-128">Create the project</span></span>](#createtheproject)
- [<span data-ttu-id="0e0f9-129">新增 ASP.NET SignalR 和 JQuery.UI NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="0e0f9-129">Add the ASP.NET SignalR and JQuery.UI NuGet packages</span></span>](#nugetpackages)
- [<span data-ttu-id="0e0f9-130">建立基底的應用程式</span><span class="sxs-lookup"><span data-stu-id="0e0f9-130">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="0e0f9-131">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="0e0f9-131">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="0e0f9-132">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="0e0f9-132">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="0e0f9-133">用戶端上新增動畫更為順暢</span><span class="sxs-lookup"><span data-stu-id="0e0f9-133">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="0e0f9-134">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0e0f9-134">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="0e0f9-135">必要條件</span><span class="sxs-lookup"><span data-stu-id="0e0f9-135">Prerequisites</span></span>

<span data-ttu-id="0e0f9-136">本教學課程需要 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-136">This tutorial requires Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="0e0f9-137">如果使用 Visual Studio 2010，則專案會使用.NET Framework 4，而不是.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-137">If Visual Studio 2010 is used, the project will use .NET Framework 4 rather than .NET Framework 4.5.</span></span>

<span data-ttu-id="0e0f9-138">如果您使用 Visual Studio 2012，建議您安裝[ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-138">If you are using Visual Studio 2012, it's recommended that you install the [ASP.NET and Web Tools 2012.2 update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="0e0f9-139">此更新包含新功能，例如發行、 新功能的增強功能和新的範本。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-139">This update contains new features such as enhancements to publishing, new functionality, and new templates.</span></span>

<span data-ttu-id="0e0f9-140">如果您有 Visual Studio 2010，請確定[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安裝。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-140">If you have Visual Studio 2010, make sure that [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) is installed.</span></span>

<a id="createtheproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="0e0f9-141">建立專案</span><span class="sxs-lookup"><span data-stu-id="0e0f9-141">Create the project</span></span>

<span data-ttu-id="0e0f9-142">在本節中，我們將建立 Visual Studio 中的專案。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-142">In this section, we'll create the project in Visual Studio.</span></span>

1. <span data-ttu-id="0e0f9-143">從**檔案**功能表上，按一下**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-143">From the **File** menu click **New Project**.</span></span>
2. <span data-ttu-id="0e0f9-144">在**新的專案**對話方塊方塊中，展開**C#** 之下**範本**，然後選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-144">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="0e0f9-145">選取  **ASP.NET 空白 Web 應用程式**範本，將專案命名為*MoveShapeDemo*，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-145">Select the **ASP.NET Empty Web Application** template, name the project *MoveShapeDemo*, and click **OK**.</span></span>

    ![建立新的專案](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a><span data-ttu-id="0e0f9-147">將 SignalR 和 JQuery.UI NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="0e0f9-147">Add the SignalR and JQuery.UI NuGet Packages</span></span>

<span data-ttu-id="0e0f9-148">您可以加入至專案的 SignalR 功能，藉由安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-148">You can add SignalR functionality to a project by installing a NuGet package.</span></span> <span data-ttu-id="0e0f9-149">本教學課程也會使用 JQuery.UI 套件，可允許為了能夠拖放以動畫顯示圖形。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-149">This tutorial will also use the JQuery.UI package for allowing the shape to be dragged and animated.</span></span>

1. <span data-ttu-id="0e0f9-150">按一下 **工具 |程式庫套件管理員 |套件管理員主控台**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-150">Click **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="0e0f9-151">在 套件管理員中，輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-151">Enter the following command in the package manager.</span></span>

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    <span data-ttu-id="0e0f9-152">SignalR 套件會安裝一些其他 NuGet 套件作為相依項目。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-152">The SignalR package installs a number of other NuGet packages as dependencies.</span></span> <span data-ttu-id="0e0f9-153">安裝完成時可能會有所有的伺服器和用戶端所需的元件使用 SignalR 的 ASP.NET 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-153">When the installation is finished you have all of the server and client components required to use SignalR in an ASP.NET application.</span></span>
3. <span data-ttu-id="0e0f9-154">套件管理員主控台安裝 JQuery 和 JQuery.UI 套件中輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-154">Enter the following command into the package manager console to install the JQuery and JQuery.UI packages.</span></span>

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="0e0f9-155">建立基底的應用程式</span><span class="sxs-lookup"><span data-stu-id="0e0f9-155">Create the base application</span></span>

<span data-ttu-id="0e0f9-156">在本節中，我們將建立每個滑鼠移動事件期間，將形狀的位置傳送至伺服器的瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-156">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="0e0f9-157">伺服器然後廣播給所有其他連接的用戶端的這項資訊是在接收。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-157">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="0e0f9-158">我們將探討在稍後的章節中的此應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-158">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="0e0f9-159">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增**，**類別...**.將類別命名為**MoveShapeHub**然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-159">In **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="0e0f9-160">在新程式碼取代**MoveShapeHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-160">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    <span data-ttu-id="0e0f9-161">`MoveShapeHub`上述類別會實作 SignalR hub。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-161">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="0e0f9-162">依照[開始使用 SignalR](index.md)教學課程中，中樞都有在用戶端會直接呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-162">As in the [Getting Started with SignalR](index.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="0e0f9-163">在此情況下，用戶端會傳送物件，其中包含新的伺服器，然後取得傳播到所有其他連線的用戶端 圖形的 X 和 Y 座標。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-163">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="0e0f9-164">SignalR 將自動序列化此物件，使用 JSON。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-164">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="0e0f9-165">物件，將會傳送至用戶端 (`ShapeModel`) 包含要儲存圖形的位置的成員。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-165">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="0e0f9-166">在伺服器上物件的版本也包含成員，才能追蹤用戶端的資料儲存，以便指定用戶端將不會傳送自己的資料。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-166">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="0e0f9-167">這個成員會使用`JsonIgnore`以防止它被序列化並傳送至用戶端的屬性。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-167">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>
3. <span data-ttu-id="0e0f9-168">接下來，我們會設定中樞的應用程式啟動時。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-168">Next, we'll set up the hub when the application starts.</span></span> <span data-ttu-id="0e0f9-169">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |全域應用程式類別**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-169">In **Solution Explorer**, right-click the project, then click **Add | Global Application Class**.</span></span> <span data-ttu-id="0e0f9-170">接受預設名稱*Global*然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-170">Accept the default name of *Global* and click **OK**.</span></span>

    ![新增全域應用程式類別](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. <span data-ttu-id="0e0f9-172">新增下列`using`之後所提供的陳述式**使用**Global.asax.cs 類別中的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-172">Add the following `using` statement after the provided **using** statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. <span data-ttu-id="0e0f9-173">加入下列行中的程式碼`Application_Start`要報名 SignalR 的預設路由的全域類別的方法。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-173">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="0e0f9-174">Global.asax 檔案看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="0e0f9-174">Your global.asax file should look like the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. <span data-ttu-id="0e0f9-175">接下來，我們將新增用戶端。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-175">Next, we'll add the client.</span></span> <span data-ttu-id="0e0f9-176">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |新的項目**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-176">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="0e0f9-177">在 **加入新項目**對話方塊中，選取**Html 網頁**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-177">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="0e0f9-178">指定頁面的適當名稱 (例如**Default.html**)，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-178">Give the page an appropriate name (like **Default.html**) and click **Add**.</span></span>
7. <span data-ttu-id="0e0f9-179">在 **方案總管**，以滑鼠右鍵按一下您剛建立的網頁，然後按一下**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-179">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
8. <span data-ttu-id="0e0f9-180">HTML 網頁中的預設程式碼取代為下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-180">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e0f9-181">確認指令碼參照以下相符項目新增至您的專案，在指令碼 資料夾中的封裝。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-181">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span> <span data-ttu-id="0e0f9-182">在 Visual Studio 2010 中，JQuery 和 SignalR 加入至專案的版本可能不符合以下的版本號碼。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-182">In Visual Studio 2010, the version of JQuery and SignalR added to the project may not match the version numbers below.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    <span data-ttu-id="0e0f9-183">上述的 HTML 和 JavaScript 程式碼會建立紅色的 Div 稱為圖形、 啟用圖形的拖曳行為使用 jQuery 程式庫，並使用圖形的`drag`圖形的位置傳送至伺服器的事件。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-183">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
9. <span data-ttu-id="0e0f9-184">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-184">Start the application by pressing F5.</span></span> <span data-ttu-id="0e0f9-185">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-185">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0e0f9-186">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-186">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="0e0f9-188">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="0e0f9-188">Add the client loop</span></span>

<span data-ttu-id="0e0f9-189">由於傳送每一個滑鼠移動事件形狀的位置，會建立網路流量不需要數量，將訊息從用戶端就必須進行節流處理。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-189">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="0e0f9-190">我們將使用 javascript`setInterval`函式來設定迴圈，將新的位置資訊傳送到伺服器以固定費率。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-190">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="0e0f9-191">這個迴圈是功能的 「 遊戲迴圈 」，重複呼叫的函式的磁碟機的所有遊戲或其他模擬非常基本表示法。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-191">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="0e0f9-192">更新用戶端中的程式碼的 HTML 網頁，以符合下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-192">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    <span data-ttu-id="0e0f9-193">上述的更新新增了`updateServerModel`函式，都會呼叫固定的頻率。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-193">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="0e0f9-194">此函式會將位置資料傳送到伺服器時`moved`旗標指出是要傳送的新位置資料。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-194">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="0e0f9-195">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-195">Start the application by pressing F5.</span></span> <span data-ttu-id="0e0f9-196">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-196">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0e0f9-197">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-197">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="0e0f9-198">取得傳送到伺服器的訊息數目會受到節流控制，因為動畫不會顯示為 smooth 上一節。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-198">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="0e0f9-200">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="0e0f9-200">Add the server loop</span></span>

<span data-ttu-id="0e0f9-201">在目前的應用程式，從伺服器傳送至用戶端的訊息移通常會將收到。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-201">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="0e0f9-202">這會呈現類似的問題，因為出現在用戶端中;訊息可以傳送頻率比所需之連接如此一來可能會淹沒。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-202">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="0e0f9-203">本節說明如何更新伺服器實作的計時器，針對外寄訊息的速率進行節流。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-203">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="0e0f9-204">內容取代`MoveShapeHub.cs`取代為下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-204">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    <span data-ttu-id="0e0f9-205">上述程式碼會展開要新增的用戶端`Broadcaster`類別，節流處理外寄訊息使用`Timer`從.NET framework 類別。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-205">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="0e0f9-206">由於中樞本身是暫時性 （它會建立每次需要它），`Broadcaster`會建立為單一值。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-206">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="0e0f9-207">延遲初始設定 （.NET 4 中引入） 用來延遲其建立，直到有需要確保計時器啟動之前，完全建立第一個 「 中樞 」 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-207">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="0e0f9-208">用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法，所以它不再接收內送訊息時，立即呼叫。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-208">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="0e0f9-209">相反地，您會將用戶端訊息傳送速率為每秒 25 呼叫受`_broadcastLoop`從計時器`Broadcaster`類別。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-209">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="0e0f9-210">最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的中樞的參考 (`_hubContext`) 使用`GlobalHost`。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-210">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="0e0f9-211">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-211">Start the application by pressing F5.</span></span> <span data-ttu-id="0e0f9-212">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0e0f9-213">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="0e0f9-214">不會從上一節中，瀏覽器中的可見差異，但會進行節流處理至用戶端傳送的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-214">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="0e0f9-216">用戶端上新增動畫更為順暢</span><span class="sxs-lookup"><span data-stu-id="0e0f9-216">Add smooth animation on the client</span></span>

<span data-ttu-id="0e0f9-217">應用程式幾乎已完成，但我們無法讓一個更多改進，圖形用戶端上的移動中移動以回應伺服器的訊息時。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-217">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="0e0f9-218">而不是給定伺服器的新位置來設定圖案的位置，我們會使用 JQuery UI 程式庫的`animate`順暢移動圖形，其目前的和新的位置之間的函式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-218">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="0e0f9-219">更新用戶端`updateShape`方法看起來像下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="0e0f9-219">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    <span data-ttu-id="0e0f9-220">上述程式碼會將圖形從舊位置移到新伺服器所指定的期間 （在本例中為 100 毫秒） 的動畫間隔。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-220">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="0e0f9-221">新動畫開始之前，會清除任何在圖形上執行的上一個動畫。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-221">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="0e0f9-222">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-222">Start the application by pressing F5.</span></span> <span data-ttu-id="0e0f9-223">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-223">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="0e0f9-224">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-224">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="0e0f9-225">移動的另一個視窗中的圖形應該會出現較不為移轉的時間，而不是一次設定每個內送訊息會以內插值取代其移動不穩定。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-225">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="0e0f9-227">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0e0f9-227">Further Steps</span></span>

<span data-ttu-id="0e0f9-228">在本教學課程中，您已了解如何以程式設計的 SignalR 應用程式，將用戶端與伺服器之間的頻率高的訊息傳送方式。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-228">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="0e0f9-229">此通訊架構可用於開發線上遊戲，以及其他的模擬，例如[ShootR 遊戲建立與 SignalR](http://shootr.signalr.net)。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-229">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](http://shootr.signalr.net).</span></span>

<span data-ttu-id="0e0f9-230">在本教學課程中建立完整的應用程式可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。</span><span class="sxs-lookup"><span data-stu-id="0e0f9-230">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).</span></span>

<span data-ttu-id="0e0f9-231">若要深入了解 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="0e0f9-231">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="0e0f9-232">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="0e0f9-232">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="0e0f9-233">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="0e0f9-233">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="0e0f9-234">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="0e0f9-234">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
