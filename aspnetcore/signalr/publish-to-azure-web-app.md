---
title: 將 ASP.NET Core SignalR 應用程式發佈至 Azure App Service
author: bradygaster
description: 瞭解如何將 ASP.NET Core SignalR 應用程式發佈至 Azure App Service。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: d03a007ca883b3d0391b848e3e92c90469ee640a
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963925"
---
# <a name="publish-an-aspnet-core-opno-locsignalr-app-to-azure-app-service"></a><span data-ttu-id="f7bb6-103">將 ASP.NET Core SignalR 應用程式發佈至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f7bb6-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="f7bb6-104">依[Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="f7bb6-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="f7bb6-105">[Azure App Service](/azure/app-service/app-service-web-overview)是用來裝載 web 應用程式的[Microsoft 雲端](https://azure.microsoft.com/)運算平臺服務，包括 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="f7bb6-106">本文是指從 Visual Studio 發佈 ASP.NET Core SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="f7bb6-107">如需詳細資訊，請參閱[適用于 Azure 的SignalR 服務](https://azure.microsoft.com/services/signalr-service)。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="f7bb6-108">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="f7bb6-108">Publish the app</span></span>

<span data-ttu-id="f7bb6-109">本文說明如何使用 Visual Studio 中的工具來發行。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="f7bb6-110">Visual Studio Code 使用者可以使用[Azure CLI](/cli/azure)命令將應用程式發佈到 Azure。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="f7bb6-111">如需詳細資訊，請參閱[使用命令列工具將 ASP.NET Core 應用程式發佈到 Azure](/azure/app-service/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="f7bb6-112">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [發佈]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="f7bb6-113">確認在 [**挑選發行目標**] 對話方塊中選取了 [ **App Service** ] 和 [**建立新**的]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="f7bb6-114">從 [**發佈**] 按鈕下拉式選選取 [**建立設定檔**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="f7bb6-115">在 [**建立 App Service** ] 對話方塊中，輸入下表所述的資訊，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="f7bb6-116">項目</span><span class="sxs-lookup"><span data-stu-id="f7bb6-116">Item</span></span>               | <span data-ttu-id="f7bb6-117">描述</span><span class="sxs-lookup"><span data-stu-id="f7bb6-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="f7bb6-118">**名稱**</span><span class="sxs-lookup"><span data-stu-id="f7bb6-118">**Name**</span></span>           | <span data-ttu-id="f7bb6-119">應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="f7bb6-120">**訂用帳戶**</span><span class="sxs-lookup"><span data-stu-id="f7bb6-120">**Subscription**</span></span>   | <span data-ttu-id="f7bb6-121">應用程式使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="f7bb6-122">**資源群組**</span><span class="sxs-lookup"><span data-stu-id="f7bb6-122">**Resource Group**</span></span> | <span data-ttu-id="f7bb6-123">應用程式所屬的相關資源群組。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="f7bb6-124">**主控方案**</span><span class="sxs-lookup"><span data-stu-id="f7bb6-124">**Hosting Plan**</span></span>   | <span data-ttu-id="f7bb6-125">Web 應用程式的定價方案。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="f7bb6-126">在 [相依性 > **新增**] 下拉式清單中選取 [ **Azure SignalR 服務** **]** ：</span><span class="sxs-lookup"><span data-stu-id="f7bb6-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![[相依性] 區域顯示 Azure 的選取專案 [！OP.[新增] 下拉式清單中的 [無-LOC （SignalR）] 服務](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="f7bb6-128">在 [ **Azure SignalR 服務**] 對話方塊中，選取 [**建立新的 Azure SignalR 服務實例**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="f7bb6-129">提供 [**名稱**]、[**資源群組**] 和 [**位置**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="f7bb6-130">返回 [ **Azure SignalR 服務**] 對話方塊，然後選取 [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="f7bb6-131">Visual Studio 完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="f7bb6-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="f7bb6-132">建立包含發行設定的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="f7bb6-133">建立具有所提供詳細資料的*Azure Web 應用程式*。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="f7bb6-134">發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-134">Publishes the app.</span></span>
* <span data-ttu-id="f7bb6-135">啟動瀏覽器，它會載入 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="f7bb6-136">應用程式 URL 的格式為 `{APP SERVICE NAME}.azurewebsites.net`。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="f7bb6-137">例如，名為 `SignalRChatApp` 的應用程式具有 `https://signalrchatapp.azurewebsites.net`的 URL。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="f7bb6-138">如果部署以預覽 .NET Core 版本為目標的應用程式時，發生 HTTP *502.2-不正確的閘道*錯誤，請參閱將[ASP.NET Core preview 版本部署至 Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)加以解決。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="f7bb6-139">在 Azure App Service 中設定應用程式</span><span class="sxs-lookup"><span data-stu-id="f7bb6-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="f7bb6-140">*本節僅適用于不使用 Azure SignalR 服務的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="f7bb6-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="f7bb6-141">如果應用程式使用 Azure SignalR 服務，則 App Service 不需要設定本章節所述的應用程式要求路由（ARR）親和性和 Web 通訊端。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="f7bb6-142">用戶端會將其 Web 通訊端連線至 Azure SignalR 服務，而不是直接連接至應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="f7bb6-143">針對沒有 Azure SignalR 服務託管的應用程式，啟用：</span><span class="sxs-lookup"><span data-stu-id="f7bb6-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="f7bb6-144">[ARR 親和性](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html)，可將來自使用者的要求路由回到相同的 App Service 實例。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="f7bb6-145">預設設定為 [**開啟**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-145">The default setting is **On**.</span></span>
* <span data-ttu-id="f7bb6-146">[Web 通訊端](xref:fundamentals/websockets)，允許 Web 通訊端傳輸運作。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="f7bb6-147">預設設定為 [**關閉**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="f7bb6-148">在 Azure 入口網站中，流覽至**應用程式服務**中的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="f7bb6-149">開啟**Configuration** > **一般設定**。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="f7bb6-150">將 [ **Web 通訊端**] 設為 [**開啟**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="f7bb6-151">確認 [ **ARR 親和性**] 已設定為 [**開啟**]。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="f7bb6-152">App Service 計畫限制</span><span class="sxs-lookup"><span data-stu-id="f7bb6-152">App Service Plan limits</span></span>

<span data-ttu-id="f7bb6-153">Web 通訊端和其他傳輸會根據選取的 App Service 方案而受到限制。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="f7bb6-154">如需詳細資訊，請參閱[azure 訂用帳戶和服務限制、配額和限制](/azure/azure-subscription-service-limits#app-service-limits)一文中的*azure 雲端服務限制*和*App Service 限制*一節。</span><span class="sxs-lookup"><span data-stu-id="f7bb6-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7bb6-155">其他資源</span><span class="sxs-lookup"><span data-stu-id="f7bb6-155">Additional resources</span></span>

* <span data-ttu-id="f7bb6-156">[什麼是 Azure SignalR 服務？](/azure/azure-signalr/signalr-overview)</span><span class="sxs-lookup"><span data-stu-id="f7bb6-156">[What is Azure SignalR Service?](/azure/azure-signalr/signalr-overview)</span></span>
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="f7bb6-157">使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="f7bb6-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="f7bb6-158">在 Azure 上裝載和部署 ASP.NET Core 預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="f7bb6-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
