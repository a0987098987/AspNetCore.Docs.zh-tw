---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Web API 和 ASP.NET Web Form 搭配使用 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="5dfb9-102">Web API 和 ASP.NET Web Form 搭配使用</span><span class="sxs-lookup"><span data-stu-id="5dfb9-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="5dfb9-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5dfb9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5dfb9-104">雖然 ASP.NET Web API 搭配 ASP.NET MVC 封裝時，所以可以輕鬆地加入傳統 ASP.NET Web Form 應用程式中 Web API。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="5dfb9-105">本教學課程將引導您逐步完成。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="5dfb9-106">概觀</span><span class="sxs-lookup"><span data-stu-id="5dfb9-106">Overview</span></span>

<span data-ttu-id="5dfb9-107">若要使用 Web API Web Form 應用程式中，有兩個主要步驟：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="5dfb9-108">加入衍生自的 Web API 控制器**ApiController**類別。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="5dfb9-109">加入路由表新增至**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="5dfb9-110">建立 Web Form 專案</span><span class="sxs-lookup"><span data-stu-id="5dfb9-110">Create a Web Forms Project</span></span>

<span data-ttu-id="5dfb9-111">啟動 Visual Studio，然後選取**新專案**從**啟動**頁面。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="5dfb9-112">或從**檔案**功能表上，選取**新增**然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="5dfb9-113">在**範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="5dfb9-114">在下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="5dfb9-115">在專案範本清單中選取**ASP.NET Web Form 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="5dfb9-116">輸入專案的名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="5dfb9-117">建立模型和控制器</span><span class="sxs-lookup"><span data-stu-id="5dfb9-117">Create the Model and Controller</span></span>

<span data-ttu-id="5dfb9-118">本教學課程使用相同的模型和控制器類別做為[入門](tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="5dfb9-119">首先，將模型類別。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-119">First, add a model class.</span></span> <span data-ttu-id="5dfb9-120">在**方案總管 中**，以滑鼠右鍵按一下專案，然後選取**加入類別**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="5dfb9-121">將類別的產品，並加入下列實作：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="5dfb9-122">接下來，將 Web API 控制器加入至專案。，*控制器*是負責處理 HTTP 要求 Web 應用程式開發介面的物件。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="5dfb9-123">在**方案總管 中**，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="5dfb9-124">選取**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="5dfb9-125">在下**已安裝的範本**，依序展開**Visual C#** 選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="5dfb9-126">然後，從範本清單中，選取**Web API 控制器類別**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="5dfb9-127">將控制器"ProductsController 」，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="5dfb9-128">**加入新項目**精靈會建立名為 ProductsController.cs 的檔案。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="5dfb9-129">刪除精靈包含的方法，並加入下列方法：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="5dfb9-130">如需此控制器中的程式碼的詳細資訊，請參閱[入門](tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="5dfb9-131">新增路由資訊</span><span class="sxs-lookup"><span data-stu-id="5dfb9-131">Add Routing Information</span></span>

<span data-ttu-id="5dfb9-132">接下來，我們要加入的 URI 路由因此的該 Uri &quot;/api/產品/&quot;會路由傳送至控制器。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="5dfb9-133">在**方案總管 中**，連按兩下以開啟程式碼後置檔案 Global.asax.cs Global.asax。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="5dfb9-134">加入下列**使用**陳述式。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="5dfb9-135">然後加入下列程式碼加入**應用程式\_啟動**方法：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="5dfb9-136">如需有關路由表的詳細資訊，請參閱[中 ASP.NET Web API 的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="5dfb9-137">新增用戶端 AJAX</span><span class="sxs-lookup"><span data-stu-id="5dfb9-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="5dfb9-138">這就是您要建立的 web 用戶端可以存取的 API。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="5dfb9-139">現在讓我們加入用來呼叫 API 的 jQuery HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="5dfb9-140">請確定您的主版頁面 (例如， *Site.Master*) 包含`ContentPlaceHolder`與`ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="5dfb9-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="5dfb9-141">開啟 Default.aspx 檔案。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-141">Open the file Default.aspx.</span></span> <span data-ttu-id="5dfb9-142">取代的未定案文字，主要內容 > 一節中所示：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="5dfb9-143">接下來，將參考加入 jQuery 原始程式檔中`HeaderContent`> 一節：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="5dfb9-144">注意： 您可以輕鬆地加入指令碼參考透過拖放方式將檔案從**方案總管 中**到程式碼編輯器視窗。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="5dfb9-145">下方的 jQuery 指令碼標記中，加入下列指令碼區塊：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="5dfb9-146">此指令碼文件載入時，會以 AJAX 要求&quot;api/產品&quot;。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="5dfb9-147">要求會傳回 JSON 格式的產品清單。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="5dfb9-148">指令碼會將產品資訊加入至 HTML 表格。</span><span class="sxs-lookup"><span data-stu-id="5dfb9-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="5dfb9-149">當您執行應用程式時，它看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="5dfb9-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
