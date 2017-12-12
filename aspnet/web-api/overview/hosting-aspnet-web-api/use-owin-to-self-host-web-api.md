---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "使用 OWIN 自我裝載 ASP.NET Web API 2 |Microsoft 文件"
author: rick-anderson
description: "本教學課程會示範如何使用 OWIN 自我裝載的 Web API framework 主控台應用程式中裝載 ASP.NET Web API。 開啟 Web 介面的.NET (OWIN) d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="92d27-104">使用 OWIN 自我裝載 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="92d27-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="92d27-105">由[Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="92d27-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="92d27-106">本教學課程會示範如何使用 OWIN 自我裝載的 Web API framework 主控台應用程式中裝載 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="92d27-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="92d27-107">[開啟適用於.NET 的 Web 介面](http://owin.org)(OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="92d27-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="92d27-108">OWIN 以減少從伺服器，使得 OWIN 適合自我裝載的 web 應用程式，在您自己的處理序，在 IIS 外部的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="92d27-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="92d27-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="92d27-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="92d27-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （也可用於 Visual Studio 2012）</span><span class="sxs-lookup"><span data-stu-id="92d27-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="92d27-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="92d27-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="92d27-112">您可以在此教學課程中找到的完整原始程式碼[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)。</span><span class="sxs-lookup"><span data-stu-id="92d27-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="92d27-113">建立主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="92d27-113">Create a Console Application</span></span>

<span data-ttu-id="92d27-114">在**檔案**功能表上，按一下 **新增**，然後按一下 **專案**。</span><span class="sxs-lookup"><span data-stu-id="92d27-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="92d27-115">從**已安裝的範本**，在 Visual C# 中，按一下**Windows** ，然後按一下 **主控台應用程式**。</span><span class="sxs-lookup"><span data-stu-id="92d27-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="92d27-116">將專案命名為"OwinSelfhostSample 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="92d27-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="92d27-117">加入 Web 應用程式開發介面和 OWIN 封裝</span><span class="sxs-lookup"><span data-stu-id="92d27-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="92d27-118">從**工具**功能表上，按一下 **程式庫套件管理員**，然後按一下  **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="92d27-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="92d27-119">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="92d27-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="92d27-120">這會安裝 WebAPI OWIN selfhost 封裝和所有必要的 OWIN 封裝。</span><span class="sxs-lookup"><span data-stu-id="92d27-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="92d27-121">設定 Web API 的自我裝載</span><span class="sxs-lookup"><span data-stu-id="92d27-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="92d27-122">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。</span><span class="sxs-lookup"><span data-stu-id="92d27-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="92d27-123">將類別命名為 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="92d27-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="92d27-124">以下列內容取代所有此檔案中的未定案程式碼：</span><span class="sxs-lookup"><span data-stu-id="92d27-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="92d27-125">加入 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="92d27-125">Add a Web API Controller</span></span>

<span data-ttu-id="92d27-126">接下來，加入 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="92d27-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="92d27-127">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。</span><span class="sxs-lookup"><span data-stu-id="92d27-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="92d27-128">將類別命名為 `ValuesController` 。</span><span class="sxs-lookup"><span data-stu-id="92d27-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="92d27-129">以下列內容取代所有此檔案中的未定案程式碼：</span><span class="sxs-lookup"><span data-stu-id="92d27-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="92d27-130">啟動 OWIN 主機，並使用 HttpClient 要求</span><span class="sxs-lookup"><span data-stu-id="92d27-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="92d27-131">以下列內容取代所有在 Program.cs 檔案中的未定案程式碼：</span><span class="sxs-lookup"><span data-stu-id="92d27-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="92d27-132">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="92d27-132">Running the Application</span></span>

<span data-ttu-id="92d27-133">若要執行應用程式，請在 Visual Studio 中按 F5。</span><span class="sxs-lookup"><span data-stu-id="92d27-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="92d27-134">輸出看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="92d27-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="92d27-135">其他資源</span><span class="sxs-lookup"><span data-stu-id="92d27-135">Additional Resources</span></span>

[<span data-ttu-id="92d27-136">專案 Katana 的概觀</span><span class="sxs-lookup"><span data-stu-id="92d27-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="92d27-137">裝載 Azure 背景工作角色中的 ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="92d27-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
