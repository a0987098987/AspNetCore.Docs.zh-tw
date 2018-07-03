---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: 使用 OWIN 自我裝載 ASP.NET Web API 2 |Microsoft Docs
author: rick-anderson
description: 本教學課程會示範如何使用 OWIN 自我裝載 Web API 架構的主控台應用程式中裝載 ASP.NET Web API。 Open Web Interface for.NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 73757b50c15c6c65dbde4b61179b2d453673cfad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389556"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="537f6-104">使用 OWIN 自我裝載 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="537f6-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="537f6-105">藉由[Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="537f6-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="537f6-106">本教學課程會示範如何使用 OWIN 自我裝載 Web API 架構的主控台應用程式中裝載 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="537f6-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="537f6-107">[Open Web Interface for.NET](http://owin.org) (OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="537f6-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="537f6-108">OWIN 可分隔的伺服器，讓 OWIN 很適合用於自我裝載的 web 應用程式，在您自己的程序，IIS 外部的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="537f6-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="537f6-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="537f6-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="537f6-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （也可用於 Visual Studio 2012）</span><span class="sxs-lookup"><span data-stu-id="537f6-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="537f6-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="537f6-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="537f6-112">您可以在本教學課程中找到完整的原始程式碼[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)。</span><span class="sxs-lookup"><span data-stu-id="537f6-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="537f6-113">建立主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="537f6-113">Create a Console Application</span></span>

<span data-ttu-id="537f6-114">在上**檔案**功能表上，按一下**新增**，然後按一下**專案**。</span><span class="sxs-lookup"><span data-stu-id="537f6-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="537f6-115">從**已安裝的範本**，在 Visual C# 中，按一下**Windows** ，然後按一下 **主控台應用程式**。</span><span class="sxs-lookup"><span data-stu-id="537f6-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="537f6-116">將專案命名為"OwinSelfhostSample 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="537f6-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="537f6-117">新增 Web API 和 OWIN 套件</span><span class="sxs-lookup"><span data-stu-id="537f6-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="537f6-118">從**工具**功能表上，按一下**程式庫套件管理員**，然後按一下**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="537f6-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="537f6-119">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="537f6-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="537f6-120">這會安裝 WebAPI OWIN selfhost 封裝和所有必要的 OWIN 套件。</span><span class="sxs-lookup"><span data-stu-id="537f6-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="537f6-121">設定 Web API 的自我裝載</span><span class="sxs-lookup"><span data-stu-id="537f6-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="537f6-122">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。</span><span class="sxs-lookup"><span data-stu-id="537f6-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="537f6-123">將類別命名為 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="537f6-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="537f6-124">您可以將所有未定案程式碼，此檔案中取代為下列：</span><span class="sxs-lookup"><span data-stu-id="537f6-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="537f6-125">新增 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="537f6-125">Add a Web API Controller</span></span>

<span data-ttu-id="537f6-126">接下來，新增 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="537f6-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="537f6-127">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。</span><span class="sxs-lookup"><span data-stu-id="537f6-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="537f6-128">將類別命名為 `ValuesController` 。</span><span class="sxs-lookup"><span data-stu-id="537f6-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="537f6-129">您可以將所有未定案程式碼，此檔案中取代為下列：</span><span class="sxs-lookup"><span data-stu-id="537f6-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="537f6-130">啟動 OWIN 主機，並使用 HttpClient 提出要求</span><span class="sxs-lookup"><span data-stu-id="537f6-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="537f6-131">您可以將所有的 Program.cs 檔案中的未定案程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="537f6-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="537f6-132">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="537f6-132">Running the Application</span></span>

<span data-ttu-id="537f6-133">若要執行應用程式，請在 Visual Studio 中按 F5。</span><span class="sxs-lookup"><span data-stu-id="537f6-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="537f6-134">輸出看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="537f6-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="537f6-135">其他資源</span><span class="sxs-lookup"><span data-stu-id="537f6-135">Additional Resources</span></span>

[<span data-ttu-id="537f6-136">Katana 專案概觀</span><span class="sxs-lookup"><span data-stu-id="537f6-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="537f6-137">裝載的 Azure 背景工作角色中的 ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="537f6-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
