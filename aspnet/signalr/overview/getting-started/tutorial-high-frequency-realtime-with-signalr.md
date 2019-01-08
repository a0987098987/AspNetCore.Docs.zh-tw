---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 教學課程：使用 SignalR 2 建立高頻率即時應用程式 |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098586"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="fec41-103">教學課程：使用 SignalR 2 建立高頻率即時應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="fec41-104">本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 2 提供高頻率的傳訊功能。</span><span class="sxs-lookup"><span data-stu-id="fec41-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="fec41-105">在此情況下，「 高頻率傳訊 」，表示伺服器會以固定費率來傳送更新。</span><span class="sxs-lookup"><span data-stu-id="fec41-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="fec41-106">您傳送第二個最多 10 個訊息。</span><span class="sxs-lookup"><span data-stu-id="fec41-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="fec41-107">您所建立的應用程式會顯示使用者可以拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="fec41-108">伺服器會更新所有連線的瀏覽器，以符合使用定時的更新拖曳圖形的位置中圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="fec41-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="fec41-109">本教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。</span><span class="sxs-lookup"><span data-stu-id="fec41-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="fec41-110">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="fec41-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fec41-111">設定專案</span><span class="sxs-lookup"><span data-stu-id="fec41-111">Set up the project</span></span>
> * <span data-ttu-id="fec41-112">建立基底的應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-112">Create the base application</span></span>
> * <span data-ttu-id="fec41-113">應用程式啟動時，對應到中樞</span><span class="sxs-lookup"><span data-stu-id="fec41-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="fec41-114">新增用戶端</span><span class="sxs-lookup"><span data-stu-id="fec41-114">Add the client</span></span>
> * <span data-ttu-id="fec41-115">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-115">Run the app</span></span>
> * <span data-ttu-id="fec41-116">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="fec41-116">Add the client loop</span></span>
> * <span data-ttu-id="fec41-117">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="fec41-117">Add the server loop</span></span>
> * <span data-ttu-id="fec41-118">新增動畫更為順暢</span><span class="sxs-lookup"><span data-stu-id="fec41-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="fec41-119">必要條件</span><span class="sxs-lookup"><span data-stu-id="fec41-119">Prerequisites</span></span>

* <span data-ttu-id="fec41-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)具有**ASP.NET 和 web 開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="fec41-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="fec41-121">設定專案</span><span class="sxs-lookup"><span data-stu-id="fec41-121">Set up the project</span></span>

<span data-ttu-id="fec41-122">在本節中，您可以建立 Visual Studio 2017 中的專案。</span><span class="sxs-lookup"><span data-stu-id="fec41-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="fec41-123">本節說明如何使用 Visual Studio 2017 來建立空白的 ASP.NET Web 應用程式並新增之 SignalR 和 jQuery.UI 程式庫。</span><span class="sxs-lookup"><span data-stu-id="fec41-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="fec41-124">在 Visual Studio 中建立 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fec41-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![建立 web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="fec41-126">在 [**新增 ASP.NET Web 應用程式-MoveShapeDemo** ] 視窗中，保持**空白**，並選取 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="fec41-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="fec41-127">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。</span><span class="sxs-lookup"><span data-stu-id="fec41-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="fec41-128">在 **加入新項目-MoveShapeDemo**，選取**已安裝** > **Visual C#**   >  **Web**  > **SignalR** ，然後選取**SignalR Hub 類別 (v2)**。</span><span class="sxs-lookup"><span data-stu-id="fec41-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="fec41-129">將類別命名為*MoveShapeHub*並將它新增至專案。</span><span class="sxs-lookup"><span data-stu-id="fec41-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="fec41-130">這個步驟會建立*MoveShapeHub.cs*類別檔案。</span><span class="sxs-lookup"><span data-stu-id="fec41-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="fec41-131">同時，它會新增一組指令碼檔案和專案支援 SignalR 的組件參考。</span><span class="sxs-lookup"><span data-stu-id="fec41-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="fec41-132">選取 **工具** > **NuGet 套件管理員** > **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="fec41-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="fec41-133">在  **Package Manager Console**，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fec41-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="fec41-134">此命令會安裝 jQuery UI 程式庫。</span><span class="sxs-lookup"><span data-stu-id="fec41-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="fec41-135">您可以使用它來製作圖案動畫。</span><span class="sxs-lookup"><span data-stu-id="fec41-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="fec41-136">在 [**方案總管] 中**，展開 [指令碼] 節點。</span><span class="sxs-lookup"><span data-stu-id="fec41-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![指令碼程式庫參考](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="fec41-138">適用於 jQuery、 jQueryUI 及 SignalR 的指令碼程式庫會顯示在專案中。</span><span class="sxs-lookup"><span data-stu-id="fec41-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="fec41-139">建立基底的應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-139">Create the base application</span></span>

<span data-ttu-id="fec41-140">在本節中，您可以建立瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="fec41-140">In this section, you create a browser application.</span></span> <span data-ttu-id="fec41-141">應用程式會每個滑鼠移動事件期間，將形狀的位置傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="fec41-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="fec41-142">伺服器會廣播到所有其他連線的用戶端即時的這項資訊。</span><span class="sxs-lookup"><span data-stu-id="fec41-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="fec41-143">您進一步了解此應用程式在稍後的章節。</span><span class="sxs-lookup"><span data-stu-id="fec41-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="fec41-144">開啟*MoveShapeHub.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="fec41-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="fec41-145">中的程式碼取代*MoveShapeHub.cs*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="fec41-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="fec41-146">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="fec41-146">Save the file.</span></span>

<span data-ttu-id="fec41-147">`MoveShapeHub`類別是實作 SignalR hub。</span><span class="sxs-lookup"><span data-stu-id="fec41-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="fec41-148">依照[開始使用 SignalR](tutorial-getting-started-with-signalr.md)教學課程中，中樞都有在用戶端直接呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="fec41-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="fec41-149">在此情況下，於物件，使用新的 X 和 Y 座標的圖形，以伺服器的用戶端傳送。</span><span class="sxs-lookup"><span data-stu-id="fec41-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="fec41-150">這些座標取得傳播到所有其他連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="fec41-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="fec41-151">SignalR 會自動將序列化此物件，使用 JSON。</span><span class="sxs-lookup"><span data-stu-id="fec41-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="fec41-152">應用程式傳送`ShapeModel`給用戶端的物件。</span><span class="sxs-lookup"><span data-stu-id="fec41-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="fec41-153">它有成員，以儲存圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="fec41-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="fec41-154">在伺服器上物件的版本也有成員，才能追蹤用戶端的資料儲存。</span><span class="sxs-lookup"><span data-stu-id="fec41-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="fec41-155">此物件會防止伺服器將用戶端的資料傳送至本身。</span><span class="sxs-lookup"><span data-stu-id="fec41-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="fec41-156">這個成員會使用`JsonIgnore`屬性將序列化資料，將它傳送回用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="fec41-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="fec41-157">應用程式啟動時，對應到中樞</span><span class="sxs-lookup"><span data-stu-id="fec41-157">Map to the hub when app starts</span></span>

<span data-ttu-id="fec41-158">接下來，您對應至中樞時設定應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="fec41-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="fec41-159">SignalR 2 中新增 OWIN 啟動類別，將會建立對應。</span><span class="sxs-lookup"><span data-stu-id="fec41-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="fec41-160">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。</span><span class="sxs-lookup"><span data-stu-id="fec41-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="fec41-161">在 **加入新項目-MoveShapeDemo**選取**已安裝** > **Visual C#**   >  **Web** ，然後選取  **OWIN 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="fec41-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="fec41-162">將類別命名為*啟始*，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="fec41-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="fec41-163">取代預設的程式碼中*Startup.cs*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="fec41-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="fec41-164">OWIN 啟動類別呼叫`MapSignalR`應用程式在執行時`Configuration`方法。</span><span class="sxs-lookup"><span data-stu-id="fec41-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="fec41-165">應用程式新增至 OWIN 的啟動類別處理使用`OwinStartup`組件屬性。</span><span class="sxs-lookup"><span data-stu-id="fec41-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="fec41-166">新增用戶端</span><span class="sxs-lookup"><span data-stu-id="fec41-166">Add the client</span></span>

<span data-ttu-id="fec41-167">加入 HTML 網頁用戶端。</span><span class="sxs-lookup"><span data-stu-id="fec41-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="fec41-168">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **HTML 網頁**。</span><span class="sxs-lookup"><span data-stu-id="fec41-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="fec41-169">將頁面命名**預設**，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="fec41-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="fec41-170">在 [**方案總管] 中**，以滑鼠右鍵按一下*Default.html* ，然後選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="fec41-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="fec41-171">取代預設的程式碼中*Default.html*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="fec41-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="fec41-172">在 **方案總管**，展開**指令碼**。</span><span class="sxs-lookup"><span data-stu-id="fec41-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="fec41-173">JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。</span><span class="sxs-lookup"><span data-stu-id="fec41-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fec41-174">套件管理員安裝新版 SignalR 指令碼。</span><span class="sxs-lookup"><span data-stu-id="fec41-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="fec41-175">更新程式碼區塊，以對應至專案中的指令碼檔案的版本中的指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="fec41-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="fec41-176">此 HTML 和 JavaScript 程式碼會建立紅色`div`稱為`shape`。</span><span class="sxs-lookup"><span data-stu-id="fec41-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="fec41-177">它可讓使用 jQuery 程式庫的圖形的拖曳行為，並使用`drag`圖形的位置傳送至伺服器的事件。</span><span class="sxs-lookup"><span data-stu-id="fec41-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="fec41-178">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-178">Run the app</span></span>

<span data-ttu-id="fec41-179">您可以執行應用程式到 se'e 它運作。</span><span class="sxs-lookup"><span data-stu-id="fec41-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="fec41-180">當您瀏覽器視窗周圍拖曳圖形時，圖案會移動其他瀏覽器中太。</span><span class="sxs-lookup"><span data-stu-id="fec41-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="fec41-181">在工具列中，開啟**指令碼偵錯**，然後選取 偵錯模式中執行應用程式的 播放 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fec41-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![使用者開啟偵錯模式，然後選取 [播放] 的螢幕擷取畫面。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="fec41-183">瀏覽器視窗隨即開啟的右上角的紅色圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="fec41-184">複製頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="fec41-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="fec41-185">開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。</span><span class="sxs-lookup"><span data-stu-id="fec41-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="fec41-186">其中一種瀏覽器視窗中拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="fec41-187">遵循其他瀏覽器視窗中的圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="fec41-188">雖然應用程式使用這個方法的函式，它不是建議的程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="fec41-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="fec41-189">取得傳送的訊息數目沒有上限。</span><span class="sxs-lookup"><span data-stu-id="fec41-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="fec41-190">如此一來，用戶端和伺服器無法取得應付訊息，並降低效能。</span><span class="sxs-lookup"><span data-stu-id="fec41-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="fec41-191">此外，應用程式會顯示在用戶端上脫離的動畫。</span><span class="sxs-lookup"><span data-stu-id="fec41-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="fec41-192">因為，圖形會在每個方法所立即移，則會發生此不穩定的動畫。</span><span class="sxs-lookup"><span data-stu-id="fec41-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="fec41-193">最好是如果，圖形會順暢地移至每個新的位置。</span><span class="sxs-lookup"><span data-stu-id="fec41-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="fec41-194">接下來，您會了解如何修正這些問題。</span><span class="sxs-lookup"><span data-stu-id="fec41-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="fec41-195">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="fec41-195">Add the client loop</span></span>

<span data-ttu-id="fec41-196">傳送每一個滑鼠移動事件形狀的位置，會建立不必要的網路傳輸量。</span><span class="sxs-lookup"><span data-stu-id="fec41-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="fec41-197">應用程式必須進行節流處理來自用戶端的訊息。</span><span class="sxs-lookup"><span data-stu-id="fec41-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="fec41-198">使用 javascript`setInterval`函式來設定迴圈，將新的位置資訊傳送到伺服器以固定費率。</span><span class="sxs-lookup"><span data-stu-id="fec41-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="fec41-199">這個迴圈是基本的表示法的 「 遊戲迴圈 」。</span><span class="sxs-lookup"><span data-stu-id="fec41-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="fec41-200">它會重複呼叫的函式的磁碟機的遊戲的所有功能。</span><span class="sxs-lookup"><span data-stu-id="fec41-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="fec41-201">中的用戶端程式碼取代*Default.html*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="fec41-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="fec41-202">您必須取代指令碼參考一次。</span><span class="sxs-lookup"><span data-stu-id="fec41-202">You have to replace the script references again.</span></span> <span data-ttu-id="fec41-203">它們必須符合專案中的指令碼的版本。</span><span class="sxs-lookup"><span data-stu-id="fec41-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="fec41-204">這個新的程式碼加入`updateServerModel`函式。</span><span class="sxs-lookup"><span data-stu-id="fec41-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="fec41-205">它會呼叫在固定的頻率。</span><span class="sxs-lookup"><span data-stu-id="fec41-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="fec41-206">函式會將位置資料傳送到伺服器時`moved`旗標指出是要傳送的新位置資料。</span><span class="sxs-lookup"><span data-stu-id="fec41-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="fec41-207">選取 [播放] 按鈕，啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="fec41-208">複製頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="fec41-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="fec41-209">開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。</span><span class="sxs-lookup"><span data-stu-id="fec41-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="fec41-210">其中一種瀏覽器視窗中拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="fec41-211">遵循其他瀏覽器視窗中的圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="fec41-212">因為應用程式節流處理的訊息傳送到伺服器時，動畫不會顯示為 smooth 數目未在第一個。</span><span class="sxs-lookup"><span data-stu-id="fec41-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="fec41-213">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="fec41-213">Add the server loop</span></span>

<span data-ttu-id="fec41-214">在目前的應用程式，從伺服器傳送至用戶端的訊息移通常在收到。</span><span class="sxs-lookup"><span data-stu-id="fec41-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="fec41-215">如我們在用戶端上所見，此網路流量會提供類似的問題。</span><span class="sxs-lookup"><span data-stu-id="fec41-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="fec41-216">應用程式可以比在需要更常傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="fec41-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="fec41-217">如此一來連接可能會淹沒。</span><span class="sxs-lookup"><span data-stu-id="fec41-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="fec41-218">本節說明如何更新伺服器，以加入計時器，針對外寄訊息的速率進行節流。</span><span class="sxs-lookup"><span data-stu-id="fec41-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="fec41-219">內容取代`MoveShapeHub.cs`以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fec41-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="fec41-220">選取 啟動應用程式的 播放 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fec41-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="fec41-221">複製頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="fec41-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="fec41-222">開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。</span><span class="sxs-lookup"><span data-stu-id="fec41-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="fec41-223">其中一種瀏覽器視窗中拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="fec41-224">此程式碼會展開要新增的用戶端`Broadcaster`類別。</span><span class="sxs-lookup"><span data-stu-id="fec41-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="fec41-225">新的類別節流處理外寄訊息使用`Timer`從.NET framework 類別。</span><span class="sxs-lookup"><span data-stu-id="fec41-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="fec41-226">您最好了解中樞本身是暫時性。</span><span class="sxs-lookup"><span data-stu-id="fec41-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="fec41-227">它會建立每次需要它。</span><span class="sxs-lookup"><span data-stu-id="fec41-227">It's created every time it's needed.</span></span> <span data-ttu-id="fec41-228">因此，應用程式會建立`Broadcaster`為 singleton。</span><span class="sxs-lookup"><span data-stu-id="fec41-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="fec41-229">延遲的情況下，它在使用延遲初始設定`Broadcaster`的建立，直到需要為止。</span><span class="sxs-lookup"><span data-stu-id="fec41-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="fec41-230">如此一來，可保證應用程式完全建立第一個 「 中樞 」 執行個體，再開始計時器。</span><span class="sxs-lookup"><span data-stu-id="fec41-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="fec41-231">用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法。</span><span class="sxs-lookup"><span data-stu-id="fec41-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="fec41-232">它不會再呼叫應用程式接收內送訊息時，立即。</span><span class="sxs-lookup"><span data-stu-id="fec41-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="fec41-233">相反地，應用程式會將訊息傳送速率為每秒 25 呼叫用戶端。</span><span class="sxs-lookup"><span data-stu-id="fec41-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="fec41-234">此程序由管理`_broadcastLoop`從計時器`Broadcaster`類別。</span><span class="sxs-lookup"><span data-stu-id="fec41-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="fec41-235">最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的參考`_hubContext`中樞。</span><span class="sxs-lookup"><span data-stu-id="fec41-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="fec41-236">它會取得與參考`GlobalHost`。</span><span class="sxs-lookup"><span data-stu-id="fec41-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="fec41-237">新增動畫更為順暢</span><span class="sxs-lookup"><span data-stu-id="fec41-237">Add smooth animation</span></span>

<span data-ttu-id="fec41-238">應用程式幾乎已完成，但我們無法讓多個的其中一項改進。</span><span class="sxs-lookup"><span data-stu-id="fec41-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="fec41-239">應用程式移動以回應伺服器訊息用戶端上的圖案。</span><span class="sxs-lookup"><span data-stu-id="fec41-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="fec41-240">而不是給定伺服器的新位置來設定圖案的位置，使用 JQuery UI 程式庫的`animate`函式。</span><span class="sxs-lookup"><span data-stu-id="fec41-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="fec41-241">它可以將圖案移動其目前的和新的位置之間的順暢。</span><span class="sxs-lookup"><span data-stu-id="fec41-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="fec41-242">更新用戶端`updateShape`方法中的*Default.html*檔案看起來像是醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="fec41-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="fec41-243">選取 啟動應用程式的 播放 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fec41-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="fec41-244">複製頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="fec41-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="fec41-245">開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。</span><span class="sxs-lookup"><span data-stu-id="fec41-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="fec41-246">其中一種瀏覽器視窗中拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="fec41-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="fec41-247">移動的另一個視窗中的圖形會顯示較不穩定。</span><span class="sxs-lookup"><span data-stu-id="fec41-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="fec41-248">應用程式進行插補其移動移轉時間，而不是每個內送訊息一次設定。</span><span class="sxs-lookup"><span data-stu-id="fec41-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="fec41-249">此程式碼會將圖形從舊位置移至新的。</span><span class="sxs-lookup"><span data-stu-id="fec41-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="fec41-250">伺服器會提供形狀的位置的動畫時間間隔期間。</span><span class="sxs-lookup"><span data-stu-id="fec41-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="fec41-251">在此情況下，這會是 100 毫秒。</span><span class="sxs-lookup"><span data-stu-id="fec41-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="fec41-252">應用程式會清除任何新的動畫開始之前，在圖形上執行的上一個動畫。</span><span class="sxs-lookup"><span data-stu-id="fec41-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fec41-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="fec41-253">Additional resources</span></span>

<span data-ttu-id="fec41-254">只是您已了解的通訊架構可用於開發線上遊戲，以及其他的模擬，像是[ShootR 遊戲建立與 SignalR](https://shootr.azurewebsites.net/)。</span><span class="sxs-lookup"><span data-stu-id="fec41-254">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="fec41-255">如需 SignalR 相關資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="fec41-255">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="fec41-256">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="fec41-256">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="fec41-257">SignalR GitHub 和範例</span><span class="sxs-lookup"><span data-stu-id="fec41-257">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="fec41-258">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="fec41-258">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="fec41-259">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fec41-259">Next steps</span></span>

<span data-ttu-id="fec41-260">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="fec41-260">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fec41-261">設定專案</span><span class="sxs-lookup"><span data-stu-id="fec41-261">Set up the project</span></span>
> * <span data-ttu-id="fec41-262">建立基本的應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-262">Created the base application</span></span>
> * <span data-ttu-id="fec41-263">應用程式啟動時，對應到中樞</span><span class="sxs-lookup"><span data-stu-id="fec41-263">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="fec41-264">新增用戶端</span><span class="sxs-lookup"><span data-stu-id="fec41-264">Added the client</span></span>
> * <span data-ttu-id="fec41-265">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fec41-265">Ran the app</span></span>
> * <span data-ttu-id="fec41-266">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="fec41-266">Added the client loop</span></span>
> * <span data-ttu-id="fec41-267">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="fec41-267">Added the server loop</span></span>
> * <span data-ttu-id="fec41-268">已新增的動畫更為順暢</span><span class="sxs-lookup"><span data-stu-id="fec41-268">Added smooth animation</span></span>

<span data-ttu-id="fec41-269">請前往下一篇文章，以了解如何建立會使用 ASP.NET SignalR 2 提供伺服器廣播的功能的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fec41-269">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="fec41-270">SignalR 2 及伺服器廣播</span><span class="sxs-lookup"><span data-stu-id="fec41-270">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)