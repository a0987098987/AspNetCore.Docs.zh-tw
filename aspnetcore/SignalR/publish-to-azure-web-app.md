---
title: 發行 ASP.NET Core Azure Web 應用程式的 SignalR 應用程式
author: rachelappel
description: 發行 ASP.NET Core Azure Web 應用程式的 SignalR 應用程式
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="4ccb7-103">發行 ASP.NET Core SignalR 應用程式至 Azure Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4ccb7-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="4ccb7-104">[Azure Web 應用程式](/azure/app-service/app-service-web-overview)是[Microsoft 雲端運算](https://azure.microsoft.com/)裝載 web 應用程式，包括 ASP.NET Core 平台服務。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="4ccb7-105">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="4ccb7-105">Publish the app</span></span>

<span data-ttu-id="4ccb7-106">Visual Studio 會提供內建的工具發行至 Azure Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="4ccb7-107">Visual Studio 程式碼的使用者可以使用[Azure CLI](/cli/azure)發行至 Azure 的應用程式的命令。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="4ccb7-108">本文涵蓋發行 Visual Studio 中使用的工具。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="4ccb7-109">若要發行應用程式使用 Azure CLI，請參閱[及命令列工具，將 ASP.NET Core 應用程式發行至 Azure](xref:tutorials/publish-to-azure-webapp-using-cli)。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="4ccb7-110">中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="4ccb7-111">確認**建立新**簽入**挑選發行目標** 對話方塊中，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![選取發行目標](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="4ccb7-113">輸入中的下列資訊**建立 App Service**對話方塊並選取**建立**。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="4ccb7-114">項目</span><span class="sxs-lookup"><span data-stu-id="4ccb7-114">Item</span></span> | <span data-ttu-id="4ccb7-115">描述</span><span class="sxs-lookup"><span data-stu-id="4ccb7-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="4ccb7-116">**應用程式名稱**</span><span class="sxs-lookup"><span data-stu-id="4ccb7-116">**App name**</span></span> | <span data-ttu-id="4ccb7-117">應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-117">A unique name of the app.</span></span> |
| <span data-ttu-id="4ccb7-118">**訂用帳戶**</span><span class="sxs-lookup"><span data-stu-id="4ccb7-118">**Subscription**</span></span> | <span data-ttu-id="4ccb7-119">應用程式使用 Azure 訂閱。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="4ccb7-120">**資源群組**</span><span class="sxs-lookup"><span data-stu-id="4ccb7-120">**Resource Group**</span></span> | <span data-ttu-id="4ccb7-121">相關資源的應用程式所屬的群組。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="4ccb7-122">**裝載計劃**</span><span class="sxs-lookup"><span data-stu-id="4ccb7-122">**Hosting Plan**</span></span> | <span data-ttu-id="4ccb7-123">Web 應用程式定價方案。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-123">The pricing plan for the web app.</span></span> |

![建立應用程式服務](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="4ccb7-125">Visual Studio 會完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="4ccb7-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="4ccb7-126">建立發行設定檔包含發行設定。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="4ccb7-127">建立或使用現有*Azure Web 應用程式*提供詳細資料。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="4ccb7-128">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-128">Publishes the app.</span></span>
* <span data-ttu-id="4ccb7-129">啟動瀏覽器中，載入已發佈的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="4ccb7-130">請注意 URL 的格式，應用程式是 *{應用程式名稱}.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="4ccb7-131">例如，應用程式命名`SignalRChattR`具有一個 URL，看起來像`https://signalrchattr.azurewebsites.net`。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="4ccb7-132">如果 HTTP 502.2 錯誤發生，請參閱[部署 ASP.NET Core 預覽發行至 Azure App Service](xref:host-and-deploy/azure-apps/index)加以解決。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="4ccb7-133">設定 SignalR web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4ccb7-133">Configure SignalR web app</span></span>

<span data-ttu-id="4ccb7-134">ASP.NET Core SignalR 應用程式必須具有 Azure Web 應用程式發行[ARR 親和性](https://en.wikipedia.org/wiki/Application_Request_Routing)啟用。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="4ccb7-135">[WebSockets](xref:fundamentals/websockets)必須啟用，以允許 WebSockets 傳輸函式。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="4ccb7-136">在 Azure 入口網站中，瀏覽至**應用程式設定**web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="4ccb7-137">設定**WebSockets**至**上**，並確認**ARR 親和性**是**上**。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![在 Azure 入口網站的 azure Web 應用程式設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="4ccb7-139">WebSockets 和其他傳輸[受到為基礎的 App Service 方案](/azure/azure-subscription-service-limits#app-service-limits)。</span><span class="sxs-lookup"><span data-stu-id="4ccb7-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="4ccb7-140">相關資源</span><span class="sxs-lookup"><span data-stu-id="4ccb7-140">Related resources</span></span>

* [<span data-ttu-id="4ccb7-141">將 ASP.NET Core 應用程式發行至 Azure 中，使用命令列工具</span><span class="sxs-lookup"><span data-stu-id="4ccb7-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="4ccb7-142">將 ASP.NET Core 應用程式發行至 Azure，使用 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ccb7-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="4ccb7-143">主機和部署在 Azure 上的 ASP.NET Core 預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="4ccb7-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
