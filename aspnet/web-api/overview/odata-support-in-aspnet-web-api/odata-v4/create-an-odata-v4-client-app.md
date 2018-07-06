---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: 建立 OData v4 用戶端應用程式 (C#) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 1e621988b43b4f012f113d7a52250879da56bb1c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818672"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="25fc2-102">建立 OData v4 用戶端應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="25fc2-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="25fc2-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="25fc2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="25fc2-104">在上一個教學課程中，您可以建立支援 CRUD 作業的基本 OData 服務。</span><span class="sxs-lookup"><span data-stu-id="25fc2-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="25fc2-105">現在讓我們來建立服務的用戶端。</span><span class="sxs-lookup"><span data-stu-id="25fc2-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="25fc2-106">啟動 Visual Studio 的新執行個體並建立新的主控台應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="25fc2-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="25fc2-107">在 **新的專案**對話方塊中，選取**已安裝** &gt; **範本** &gt; **Visual C#** &gt; **Windows 桌面**，然後選取**主控台應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="25fc2-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="25fc2-108">將專案命名為&quot;ProductsApp&quot;。</span><span class="sxs-lookup"><span data-stu-id="25fc2-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="25fc2-109">您也可以新增至相同的 Visual Studio 方案，其中包含 OData 服務的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="25fc2-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="25fc2-110">安裝 OData 用戶端程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="25fc2-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="25fc2-111">從**工具**功能表上，選取**擴充功能和更新**。</span><span class="sxs-lookup"><span data-stu-id="25fc2-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="25fc2-112">選取 **線上** &gt; **Visual Studio 元件庫**。</span><span class="sxs-lookup"><span data-stu-id="25fc2-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="25fc2-113">在 [搜尋] 方塊中，搜尋&quot;OData 用戶端程式碼產生器&quot;。</span><span class="sxs-lookup"><span data-stu-id="25fc2-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="25fc2-114">按一下 **下載**来安裝 VSIX。</span><span class="sxs-lookup"><span data-stu-id="25fc2-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="25fc2-115">系統可能會提示您重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="25fc2-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="25fc2-116">在本機執行的 OData 服務</span><span class="sxs-lookup"><span data-stu-id="25fc2-116">Run the OData Service Locally</span></span>

<span data-ttu-id="25fc2-117">從 Visual Studio 執行 ProductService 專案。</span><span class="sxs-lookup"><span data-stu-id="25fc2-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="25fc2-118">根據預設，Visual Studio 會啟動瀏覽器應用程式根目錄。</span><span class="sxs-lookup"><span data-stu-id="25fc2-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="25fc2-119">請注意 URI;您將在下一個步驟中需要此設定。</span><span class="sxs-lookup"><span data-stu-id="25fc2-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="25fc2-120">繼續執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="25fc2-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="25fc2-121">如果您將這兩個專案放在相同的方案時，請務必執行 ProductService 專案但不偵錯。</span><span class="sxs-lookup"><span data-stu-id="25fc2-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="25fc2-122">在下一個步驟中，您必須讓您修改主控台應用程式專案時所執行的服務。</span><span class="sxs-lookup"><span data-stu-id="25fc2-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="25fc2-123">產生服務 Proxy</span><span class="sxs-lookup"><span data-stu-id="25fc2-123">Generate the Service Proxy</span></span>

<span data-ttu-id="25fc2-124">服務 proxy 是一個.NET 類別來定義存取 OData 服務的方法。</span><span class="sxs-lookup"><span data-stu-id="25fc2-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="25fc2-125">Proxy 會轉譯成 HTTP 要求的方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="25fc2-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="25fc2-126">您將建立 proxy 類別，藉由執行[T4 範本](https://msdn.microsoft.com/library/bb126445.aspx)。</span><span class="sxs-lookup"><span data-stu-id="25fc2-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="25fc2-127">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="25fc2-127">Right-click the project.</span></span> <span data-ttu-id="25fc2-128">選取 **新增** &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="25fc2-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="25fc2-129">在 **加入新項目**對話方塊中，選取**Visual C# 項目** &gt; **程式碼** &gt; **OData 用戶端**。</span><span class="sxs-lookup"><span data-stu-id="25fc2-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="25fc2-130">將範本命名&quot;ProductClient.tt&quot;。</span><span class="sxs-lookup"><span data-stu-id="25fc2-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="25fc2-131">按一下 **新增**並點選 安全性警告。</span><span class="sxs-lookup"><span data-stu-id="25fc2-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="25fc2-132">此時，您會收到錯誤，您可以忽略。</span><span class="sxs-lookup"><span data-stu-id="25fc2-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="25fc2-133">Visual Studio 會自動執行此範本中，但此範本需要一些組態設定第一次。</span><span class="sxs-lookup"><span data-stu-id="25fc2-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="25fc2-134">開啟檔案 ProductClient.odata.config。在 `Parameter`項目中，貼上從 ProductService 專案 （上一個步驟） 的 URI。</span><span class="sxs-lookup"><span data-stu-id="25fc2-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="25fc2-135">例如: </span><span class="sxs-lookup"><span data-stu-id="25fc2-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="25fc2-136">再次執行範本。</span><span class="sxs-lookup"><span data-stu-id="25fc2-136">Run the template again.</span></span> <span data-ttu-id="25fc2-137">在 [方案總管] 中，以滑鼠右鍵按一下 ProductClient.tt 檔案，然後選取**執行自訂工具**。</span><span class="sxs-lookup"><span data-stu-id="25fc2-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="25fc2-138">此範本會建立名為 ProductClient.cs 定義 proxy 的程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="25fc2-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="25fc2-139">當您開發您的應用程式，如果您變更 OData 端點，執行一次是要更新的 proxy 之範本。</span><span class="sxs-lookup"><span data-stu-id="25fc2-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="25fc2-140">使用服務 Proxy 來呼叫 OData 服務</span><span class="sxs-lookup"><span data-stu-id="25fc2-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="25fc2-141">開啟 Program.cs 檔案，並以下列內容取代未定案程式碼。</span><span class="sxs-lookup"><span data-stu-id="25fc2-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="25fc2-142">值取代*serviceUri*使用稍早的服務 URI。</span><span class="sxs-lookup"><span data-stu-id="25fc2-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="25fc2-143">當您執行應用程式時，它應輸出下列內容：</span><span class="sxs-lookup"><span data-stu-id="25fc2-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
