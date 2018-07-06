---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: 使用 Web API 和 ASP.NET Web Form |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: c9da38d290a812e94ed9473386ba6b897447dac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840884"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="badc6-102">使用 Web API 和 ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="badc6-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="badc6-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="badc6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="badc6-104">雖然 ASP.NET Web API 隨附 ASP.NET MVC，很容易就能將 Web API 新增至傳統的 ASP.NET Web Forms 應用程式。</span><span class="sxs-lookup"><span data-stu-id="badc6-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="badc6-105">本教學課程將逐步引導您逐步完成。</span><span class="sxs-lookup"><span data-stu-id="badc6-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="badc6-106">總覽</span><span class="sxs-lookup"><span data-stu-id="badc6-106">Overview</span></span>

<span data-ttu-id="badc6-107">若要在 Web Form 應用程式中使用 Web API，有兩個主要步驟：</span><span class="sxs-lookup"><span data-stu-id="badc6-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="badc6-108">新增衍生自的 Web API 控制器**ApiController**類別。</span><span class="sxs-lookup"><span data-stu-id="badc6-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="badc6-109">加入路由表與**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="badc6-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="badc6-110">建立 Web Form 專案</span><span class="sxs-lookup"><span data-stu-id="badc6-110">Create a Web Forms Project</span></span>

<span data-ttu-id="badc6-111">啟動 Visual Studio，然後選取**新的專案**從**開始**頁面。</span><span class="sxs-lookup"><span data-stu-id="badc6-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="badc6-112">或從**檔案**功能表上，選取**新增**，然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="badc6-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="badc6-113">在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。</span><span class="sxs-lookup"><span data-stu-id="badc6-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="badc6-114">底下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="badc6-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="badc6-115">在專案範本清單中，選取**ASP.NET Web Forms 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="badc6-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="badc6-116">輸入專案的名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="badc6-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="badc6-117">建立模型和控制器</span><span class="sxs-lookup"><span data-stu-id="badc6-117">Create the Model and Controller</span></span>

<span data-ttu-id="badc6-118">本教學課程使用相同的模型和控制器類別作為[開始使用](tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="badc6-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="badc6-119">首先，新增模型類別。</span><span class="sxs-lookup"><span data-stu-id="badc6-119">First, add a model class.</span></span> <span data-ttu-id="badc6-120">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**加入類別**。</span><span class="sxs-lookup"><span data-stu-id="badc6-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="badc6-121">將類別命名為產品，並加入下列實作：</span><span class="sxs-lookup"><span data-stu-id="badc6-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="badc6-122">接下來，新增至專案。，A 的 Web API 控制器*控制器*是 Web api 處理 HTTP 要求的物件。</span><span class="sxs-lookup"><span data-stu-id="badc6-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="badc6-123">在 [**方案總管] 中**，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="badc6-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="badc6-124">選取 **加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="badc6-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="badc6-125">底下**已安裝的範本**，展開**Visual C#** ，然後選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="badc6-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="badc6-126">然後，從範本清單中，選取**Web API 控制器類別**。</span><span class="sxs-lookup"><span data-stu-id="badc6-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="badc6-127">控制器命名為"ProductsController 」，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="badc6-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="badc6-128">**加入新項目**精靈會建立名為 ProductsController.cs 的檔案。</span><span class="sxs-lookup"><span data-stu-id="badc6-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="badc6-129">刪除精靈包含的方法，並新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="badc6-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="badc6-130">如需有關此控制器中的程式碼的詳細資訊，請參閱[開始使用](tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="badc6-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="badc6-131">新增路由的資訊</span><span class="sxs-lookup"><span data-stu-id="badc6-131">Add Routing Information</span></span>

<span data-ttu-id="badc6-132">接下來，我們將新增的 URI 路由因此該格式的 Uri &quot;/api/產品/&quot;會路由傳送至控制器。</span><span class="sxs-lookup"><span data-stu-id="badc6-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="badc6-133">在 [**方案總管] 中**，連按兩下以開啟程式碼後置檔案 Global.asax.cs Global.asax。</span><span class="sxs-lookup"><span data-stu-id="badc6-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="badc6-134">新增下列**使用**陳述式。</span><span class="sxs-lookup"><span data-stu-id="badc6-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="badc6-135">然後新增下列程式碼**應用程式\_啟動**方法：</span><span class="sxs-lookup"><span data-stu-id="badc6-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="badc6-136">如需有關路由表的詳細資訊，請參閱[ASP.NET Web API 中的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="badc6-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="badc6-137">新增用戶端 AJAX</span><span class="sxs-lookup"><span data-stu-id="badc6-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="badc6-138">這是您只需要建立 web 用戶端可以存取的 API。</span><span class="sxs-lookup"><span data-stu-id="badc6-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="badc6-139">現在讓我們新增 會使用 jQuery 來呼叫 API 的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="badc6-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="badc6-140">請確定您的主版頁面 (例如*Site.Master*) 包含`ContentPlaceHolder`使用`ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="badc6-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="badc6-141">開啟 Default.aspx 檔案。</span><span class="sxs-lookup"><span data-stu-id="badc6-141">Open the file Default.aspx.</span></span> <span data-ttu-id="badc6-142">取代的未定案文字，是在主要內容區段中，如所示：</span><span class="sxs-lookup"><span data-stu-id="badc6-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="badc6-143">接下來，新增 jQuery 原始程式檔中的參考`HeaderContent`區段：</span><span class="sxs-lookup"><span data-stu-id="badc6-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="badc6-144">注意： 您可以輕鬆地加入指令碼參考拖放檔案從**方案總管 中**的程式碼編輯器視窗。</span><span class="sxs-lookup"><span data-stu-id="badc6-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="badc6-145">下列 jQuery 指令碼標記中，新增下列指令碼區塊：</span><span class="sxs-lookup"><span data-stu-id="badc6-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="badc6-146">此指令碼文件載入時，提出 AJAX 要求以&quot;api/產品&quot;。</span><span class="sxs-lookup"><span data-stu-id="badc6-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="badc6-147">要求會以 JSON 格式傳回產品的清單。</span><span class="sxs-lookup"><span data-stu-id="badc6-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="badc6-148">指令碼會將 HTML 表格中的產品資訊。</span><span class="sxs-lookup"><span data-stu-id="badc6-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="badc6-149">當您執行應用程式時，它看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="badc6-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
