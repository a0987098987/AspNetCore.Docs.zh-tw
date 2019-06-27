---
title: 發佈 ASP.NET Core SignalR 應用程式至 Azure App Service
author: bradygaster
description: 了解如何將 ASP.NET Core SignalR 應用程式發行至 Azure App Service。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406105"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a><span data-ttu-id="41062-103">發佈 ASP.NET Core SignalR 應用程式至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="41062-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="41062-104">藉由[Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="41062-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="41062-105">[Azure App Service](/azure/app-service/app-service-web-overview)已[Microsoft 雲端運算](https://azure.microsoft.com/)裝載 web 應用程式，包括 ASP.NET Core 的平台服務。</span><span class="sxs-lookup"><span data-stu-id="41062-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="41062-106">這篇文章是指從 Visual Studio 發佈的 ASP.NET Core SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="41062-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="41062-107">如需詳細資訊，請參閱 <<c0> [ 適用於 Azure SignalR 服務](https://azure.microsoft.com/services/signalr-service)。</span><span class="sxs-lookup"><span data-stu-id="41062-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="41062-108">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="41062-108">Publish the app</span></span>

<span data-ttu-id="41062-109">本文涵蓋發行 Visual Studio 中使用的工具。</span><span class="sxs-lookup"><span data-stu-id="41062-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="41062-110">Visual Studio Code 使用者可以使用[Azure CLI](/cli/azure)命令，以將應用程式發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="41062-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="41062-111">如需詳細資訊，請參閱 <<c0> [ 使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure](/azure/app-service/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="41062-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="41062-112">在 [方案總管]  中，以滑鼠右鍵按一下專案，然後選取 [發佈]  。</span><span class="sxs-lookup"><span data-stu-id="41062-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="41062-113">確認**App Service**並**新建**中所選**挑選發行目標**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="41062-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="41062-114">選取 **建立設定檔**從**發佈**向下按鈕拖放。</span><span class="sxs-lookup"><span data-stu-id="41062-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="41062-115">輸入下表中所述的資訊**建立 App Service**對話方塊，然後選取**建立**。</span><span class="sxs-lookup"><span data-stu-id="41062-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="41062-116">項目</span><span class="sxs-lookup"><span data-stu-id="41062-116">Item</span></span>               | <span data-ttu-id="41062-117">描述</span><span class="sxs-lookup"><span data-stu-id="41062-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="41062-118">**名稱**</span><span class="sxs-lookup"><span data-stu-id="41062-118">**Name**</span></span>           | <span data-ttu-id="41062-119">應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="41062-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="41062-120">**訂用帳戶**</span><span class="sxs-lookup"><span data-stu-id="41062-120">**Subscription**</span></span>   | <span data-ttu-id="41062-121">Azure 訂用帳戶，應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="41062-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="41062-122">**資源群組**</span><span class="sxs-lookup"><span data-stu-id="41062-122">**Resource Group**</span></span> | <span data-ttu-id="41062-123">應用程式所屬的相關資源的群組。</span><span class="sxs-lookup"><span data-stu-id="41062-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="41062-124">**主控方案**</span><span class="sxs-lookup"><span data-stu-id="41062-124">**Hosting Plan**</span></span>   | <span data-ttu-id="41062-125">Web 應用程式的定價方案。</span><span class="sxs-lookup"><span data-stu-id="41062-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="41062-126">選取  **Azure SignalR 服務**中**相依性** > **新增**下拉式清單：</span><span class="sxs-lookup"><span data-stu-id="41062-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![在 [加入] 下拉式清單中顯示選取的 Azure SignalR 服務的相依性區域](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="41062-128">在  **Azure SignalR 服務**對話方塊中，選取**建立新的 Azure SignalR 服務執行個體**。</span><span class="sxs-lookup"><span data-stu-id="41062-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="41062-129">提供**名稱**，**資源群組**，並**位置**。</span><span class="sxs-lookup"><span data-stu-id="41062-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="41062-130">返回**Azure SignalR 服務**對話方塊，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="41062-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="41062-131">Visual Studio 中完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="41062-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="41062-132">建立發行設定檔包含發佈設定。</span><span class="sxs-lookup"><span data-stu-id="41062-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="41062-133">會建立*Azure Web 應用程式*提供詳細資料。</span><span class="sxs-lookup"><span data-stu-id="41062-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="41062-134">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="41062-134">Publishes the app.</span></span>
* <span data-ttu-id="41062-135">會啟動瀏覽器中，它會載入 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="41062-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="41062-136">應用程式的 URL 的格式是`{APP SERVICE NAME}.azurewebsites.net`。</span><span class="sxs-lookup"><span data-stu-id="41062-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="41062-137">例如，名為應用程式`SignalRChatApp`具有一個 URL 的`https://signalrchatapp.azurewebsites.net`。</span><span class="sxs-lookup"><span data-stu-id="41062-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="41062-138">如果 HTTP *502.2-閘道不正確*預覽.NET Core 版本為目標的應用程式部署時，就會發生錯誤，請參閱[至 Azure App Service 部署 ASP.NET Core 預覽版本](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)加以解決。</span><span class="sxs-lookup"><span data-stu-id="41062-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="41062-139">在 Azure App Service 中設定應用程式</span><span class="sxs-lookup"><span data-stu-id="41062-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="41062-140">*本節僅適用於未使用 Azure SignalR 服務應用程式。*</span><span class="sxs-lookup"><span data-stu-id="41062-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="41062-141">如果應用程式使用 Azure SignalR 服務，App Service 不需要應用程式要求路由 (ARR) 親和性與這一節所述的 Web 通訊端的組態。</span><span class="sxs-lookup"><span data-stu-id="41062-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="41062-142">Azure SignalR 服務，不是直接以應用程式，用戶端連線其 Web 通訊端。</span><span class="sxs-lookup"><span data-stu-id="41062-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="41062-143">託管的應用程式而不需 Azure SignalR 服務，啟用：</span><span class="sxs-lookup"><span data-stu-id="41062-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="41062-144">[ARR 同質性](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html)回到相同的 App Service 執行個體的路由要求的使用者。</span><span class="sxs-lookup"><span data-stu-id="41062-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="41062-145">預設值是**上**。</span><span class="sxs-lookup"><span data-stu-id="41062-145">The default setting is **On**.</span></span>
* <span data-ttu-id="41062-146">[Web 通訊端](xref:fundamentals/websockets)以允許 Web 通訊端傳輸至函式。</span><span class="sxs-lookup"><span data-stu-id="41062-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="41062-147">預設值是**關閉**。</span><span class="sxs-lookup"><span data-stu-id="41062-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="41062-148">在 Azure 入口網站中，瀏覽至 web 應用程式中**應用程式服務**。</span><span class="sxs-lookup"><span data-stu-id="41062-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="41062-149">開啟**組態** > **一般設定**。</span><span class="sxs-lookup"><span data-stu-id="41062-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="41062-150">設定**Web 通訊端**要**上**。</span><span class="sxs-lookup"><span data-stu-id="41062-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="41062-151">確認**ARR 同質性**設為**上**。</span><span class="sxs-lookup"><span data-stu-id="41062-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="41062-152">App Service 方案限制</span><span class="sxs-lookup"><span data-stu-id="41062-152">App Service Plan limits</span></span>

<span data-ttu-id="41062-153">Web 通訊端和其他傳輸數目限制為基礎的 App Service 方案選取。</span><span class="sxs-lookup"><span data-stu-id="41062-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="41062-154">如需詳細資訊，請參閱 < *Azure 雲端服務限制*並*App Service 限制*區段[Azure 訂用帳戶和服務限制、 配額和條件約束](/azure/azure-subscription-service-limits#app-service-limits)發行項。</span><span class="sxs-lookup"><span data-stu-id="41062-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41062-155">其他資源</span><span class="sxs-lookup"><span data-stu-id="41062-155">Additional resources</span></span>

* [<span data-ttu-id="41062-156">什麼是 Azure SignalR 服務？</span><span class="sxs-lookup"><span data-stu-id="41062-156">What is Azure SignalR Service?</span></span>](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="41062-157">使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="41062-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="41062-158">裝載和部署在 Azure 上的 ASP.NET Core 預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="41062-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
