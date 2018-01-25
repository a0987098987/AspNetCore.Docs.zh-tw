---
uid: visual-studio/overview/2013/using-browser-link
title: "在 Visual Studio 2013 中使用瀏覽器連結 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="fa5b2-102">在 Visual Studio 2013 中使用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="fa5b2-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="fa5b2-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fa5b2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fa5b2-104">瀏覽器連結是建立在開發環境和一或多個網頁瀏覽器之間的通訊通道的 Visual Studio 2013 中的新功能。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="fa5b2-105">您可以使用瀏覽器連結，以重新整理 web 應用程式在數個瀏覽器中的，這是適用於跨瀏覽器測試。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="fa5b2-106">重新整理瀏覽器</span><span class="sxs-lookup"><span data-stu-id="fa5b2-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="fa5b2-107">檢視瀏覽器連結儀表板</span><span class="sxs-lookup"><span data-stu-id="fa5b2-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="fa5b2-108">啟用靜態 HTML 檔案的瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="fa5b2-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="fa5b2-109">停用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="fa5b2-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="fa5b2-110">它如何運作？</span><span class="sxs-lookup"><span data-stu-id="fa5b2-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="fa5b2-111">重新整理瀏覽器</span><span class="sxs-lookup"><span data-stu-id="fa5b2-111">Browser Refresh</span></span>

<span data-ttu-id="fa5b2-112">使用瀏覽器重新整理，您可以重新整理多個瀏覽器連接到 Visual Studio 透過瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="fa5b2-113">若要使用瀏覽器重新整理，請先建立任何專案範本使用的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="fa5b2-114">按下 F5 或按一下工具列中的箭號圖示，偵錯應用程式：</span><span class="sxs-lookup"><span data-stu-id="fa5b2-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="fa5b2-115">您也可以使用下拉式清單中選取特定瀏覽器的偵錯。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="fa5b2-116">若要使用多個瀏覽器進行偵錯，請選取**瀏覽**。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="fa5b2-117">在**瀏覽** 對話方塊中，按住 CTRL 鍵來選取多個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="fa5b2-118">按一下**瀏覽**與所選的瀏覽器偵錯。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="fa5b2-119">如果您啟動從 Visual Studio 外部瀏覽器並瀏覽至應用程式 URL，也適用於瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="fa5b2-120">在下拉式清單中有循環的箭號圖示位於瀏覽器連結控制項。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="fa5b2-121">箭號圖示，則為**重新整理** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="fa5b2-122">若要查看哪些瀏覽器連線，將滑鼠游標**重新整理**偵錯時的按鈕。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="fa5b2-123">已連線的瀏覽器會顯示在工具提示 視窗中。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="fa5b2-124">若要重新整理連接的瀏覽器，請按一下**重新整理**按鈕或按 CTRL + ALT + ENTER。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="fa5b2-125">例如，下列螢幕擷取畫面會顯示在 ASP.NET 專案中使用 MVC 5 專案範本建立。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="fa5b2-126">您可以看到在頂端的兩個瀏覽器中執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="fa5b2-127">在底部，專案是在 Visual Studio 中開啟。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="fa5b2-128">在 Visual Studio 中，變更&lt;h1&gt;標題首頁：</span><span class="sxs-lookup"><span data-stu-id="fa5b2-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="fa5b2-129">當我按下**重新整理**按鈕，變更出現在這兩個瀏覽器視窗中：</span><span class="sxs-lookup"><span data-stu-id="fa5b2-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="fa5b2-130">**備註**</span><span class="sxs-lookup"><span data-stu-id="fa5b2-130">**Notes**</span></span>

- <span data-ttu-id="fa5b2-131">若要啟用瀏覽器連結，將`debug=true`中[&lt;編譯&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx)專案的 Web.config 檔案中的項目。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="fa5b2-132">應用程式必須在 localhost 上執行。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="fa5b2-133">應用程式必須以.NET 4.0 或更新版本為目標。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="fa5b2-134">檢視瀏覽器連結儀表板</span><span class="sxs-lookup"><span data-stu-id="fa5b2-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="fa5b2-135">瀏覽器連結儀表板會顯示瀏覽器連結連線的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="fa5b2-136">若要檢視儀表板，請選取 [瀏覽器連結] 下拉式功能表 (旁邊的小箭頭**重新整理**按鈕)。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="fa5b2-137">然後按一下 **瀏覽器連結儀表板**。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="fa5b2-138">儀表板會列出已連線的瀏覽器和每個瀏覽器已瀏覽的 URL。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="fa5b2-139">**必要條件**區段會顯示該專案啟用瀏覽器連結所需的任何步驟。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="fa5b2-140">例如，下列螢幕擷取畫面顯示的專案之 「 偵錯 」 設定為 false 的 Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="fa5b2-141">啟用靜態 HTML 檔案的瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="fa5b2-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="fa5b2-142">若要啟用瀏覽器連結為靜態的 HTML 檔案，請將下列加入 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="fa5b2-143">基於效能考量，請移除此設定，當您發行專案。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="fa5b2-144">停用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="fa5b2-144">Disabling Browser Link</span></span>

<span data-ttu-id="fa5b2-145">預設會啟用瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="fa5b2-146">有數種方式可將它停用：</span><span class="sxs-lookup"><span data-stu-id="fa5b2-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="fa5b2-147">在瀏覽器連結 下拉式功能表中，取消核取**啟用瀏覽器連結**。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="fa5b2-148">在 Web.config 檔案中，加入名為"vs: EnableBrowserLink"與"false"值的 appSettings 區段中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="fa5b2-149">在 Web.config 檔案中，設定為 false 的偵錯。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="fa5b2-150">它如何運作？</span><span class="sxs-lookup"><span data-stu-id="fa5b2-150">How Does It Work?</span></span>

<span data-ttu-id="fa5b2-151">使用瀏覽器連結[SignalR](../../../signalr/index.md)來建立 Visual Studio 和瀏覽器之間的通訊通道。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="fa5b2-152">啟用瀏覽器連結時，Visual Studio 就會當做多個用戶端 （瀏覽器使用） 可以連接到的 SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="fa5b2-153">瀏覽器連結也會使用 ASP.NET 註冊 HTTP 模組。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="fa5b2-154">此模組會插入特殊&lt;指令碼&gt;到每個頁面要求來自伺服器的參考。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="fa5b2-155">您可以看到指令碼參考在瀏覽器中選取 [檢視來源]。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="fa5b2-156">不會修改原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-156">Your source files are not modified.</span></span> <span data-ttu-id="fa5b2-157">HTTP 模組會以動態方式插入指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="fa5b2-158">因為瀏覽器端程式碼是所有 JavaScript，所以它適用於所有瀏覽器，[支援 SignalR](../../../signalr/overview/getting-started/supported-platforms.md)，而不需要任何瀏覽器外掛程式。</span><span class="sxs-lookup"><span data-stu-id="fa5b2-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
