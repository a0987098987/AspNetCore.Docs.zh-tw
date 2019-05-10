---
title: 發佈 ASP.NET Core SignalR 應用程式至 Azure Web 應用程式
author: bradygaster
description: 發佈 ASP.NET Core SignalR 應用程式至 Azure Web 應用程式
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 1c472711a86edae8dc6e207734aa54e48c02d47d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087689"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="9ec66-103">發佈 ASP.NET Core SignalR 應用程式，為 Azure Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9ec66-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="9ec66-104">[Azure Web 應用程式](/azure/app-service/app-service-web-overview)已[Microsoft 雲端運算](https://azure.microsoft.com/)裝載 web 應用程式，包括 ASP.NET Core 的平台服務。</span><span class="sxs-lookup"><span data-stu-id="9ec66-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="9ec66-105">這篇文章是指從 Visual Studio 發佈的 ASP.NET Core SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ec66-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="9ec66-106">請瀏覽[適用於 Azure SignalR 服務](https://azure.microsoft.com/services/signalr-service)如需有關在 Azure 上使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="9ec66-106">Visit [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="9ec66-107">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="9ec66-107">Publish the app</span></span>

<span data-ttu-id="9ec66-108">Visual Studio 會提供內建工具發行至 Azure Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ec66-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="9ec66-109">Visual Studio Code 使用者可以使用[Azure CLI](/cli/azure)命令，以將應用程式發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="9ec66-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="9ec66-110">本文涵蓋發行 Visual Studio 中使用的工具。</span><span class="sxs-lookup"><span data-stu-id="9ec66-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="9ec66-111">若要發行應用程式使用 Azure CLI，請參閱[使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure](/azure/app-service/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="9ec66-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="9ec66-112">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [發佈]。</span><span class="sxs-lookup"><span data-stu-id="9ec66-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="9ec66-113">確認**新建**簽入**挑選發行目標**對話方塊中，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="9ec66-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![挑選發佈目標](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="9ec66-115">輸入中的下列資訊**建立 App Service**對話方塊，然後選取**建立**。</span><span class="sxs-lookup"><span data-stu-id="9ec66-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="9ec66-116">項目</span><span class="sxs-lookup"><span data-stu-id="9ec66-116">Item</span></span> | <span data-ttu-id="9ec66-117">描述</span><span class="sxs-lookup"><span data-stu-id="9ec66-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="9ec66-118">**應用程式名稱**</span><span class="sxs-lookup"><span data-stu-id="9ec66-118">**App name**</span></span> | <span data-ttu-id="9ec66-119">應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="9ec66-119">A unique name of the app.</span></span> |
| <span data-ttu-id="9ec66-120">**訂用帳戶**</span><span class="sxs-lookup"><span data-stu-id="9ec66-120">**Subscription**</span></span> | <span data-ttu-id="9ec66-121">Azure 訂用帳戶，應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="9ec66-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="9ec66-122">**資源群組**</span><span class="sxs-lookup"><span data-stu-id="9ec66-122">**Resource Group**</span></span> | <span data-ttu-id="9ec66-123">應用程式所屬的相關資源的群組。</span><span class="sxs-lookup"><span data-stu-id="9ec66-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="9ec66-124">**主控方案**</span><span class="sxs-lookup"><span data-stu-id="9ec66-124">**Hosting Plan**</span></span> | <span data-ttu-id="9ec66-125">Web 應用程式的定價方案。</span><span class="sxs-lookup"><span data-stu-id="9ec66-125">The pricing plan for the web app.</span></span> |

![建立 App Service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="9ec66-127">Visual Studio 中完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="9ec66-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="9ec66-128">建立發行設定檔包含發佈設定。</span><span class="sxs-lookup"><span data-stu-id="9ec66-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="9ec66-129">建立或使用現有*Azure Web 應用程式*提供詳細資料。</span><span class="sxs-lookup"><span data-stu-id="9ec66-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="9ec66-130">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ec66-130">Publishes the app.</span></span>
* <span data-ttu-id="9ec66-131">啟動瀏覽器中，使用已發佈的 web 應用程式載入。</span><span class="sxs-lookup"><span data-stu-id="9ec66-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="9ec66-132">請注意 URL 的格式，為應用程式 *{應用程式名稱}.azurewebsites.net.net*。</span><span class="sxs-lookup"><span data-stu-id="9ec66-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="9ec66-133">例如，名為應用程式`SignalRChattR`具有一個 URL 看起來像 `https://signalrchattr.azurewebsites.net`。</span><span class="sxs-lookup"><span data-stu-id="9ec66-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="9ec66-134">如果 HTTP 502.2 錯誤發生，請參閱[至 Azure App Service 部署 ASP.NET Core 預覽版本](xref:host-and-deploy/azure-apps/index)加以解決。</span><span class="sxs-lookup"><span data-stu-id="9ec66-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="9ec66-135">SignalR web 應用程式設定</span><span class="sxs-lookup"><span data-stu-id="9ec66-135">Configure SignalR web app</span></span>

<span data-ttu-id="9ec66-136">ASP.NET Core SignalR 應用程式的已發行的 Azure Web 應用程式就必須擁有[ARR 同質性](https://en.wikipedia.org/wiki/Application_Request_Routing)啟用。</span><span class="sxs-lookup"><span data-stu-id="9ec66-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="9ec66-137">[Websocket](xref:fundamentals/websockets)應該啟用，以允許函式的 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="9ec66-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="9ec66-138">在 Azure 入口網站中，瀏覽至**應用程式設定**web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ec66-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="9ec66-139">設定**WebSockets**要**上**，並確認**ARR 同質性**是**上**。</span><span class="sxs-lookup"><span data-stu-id="9ec66-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![在 Azure 入口網站中的 azure Web 應用程式設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="9ec66-141">Websocket 和其他傳輸[會限制為基礎的 App Service 方案](/azure/azure-subscription-service-limits#app-service-limits)。</span><span class="sxs-lookup"><span data-stu-id="9ec66-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="9ec66-142">相關資源</span><span class="sxs-lookup"><span data-stu-id="9ec66-142">Related resources</span></span>

* [<span data-ttu-id="9ec66-143">使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="9ec66-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="9ec66-144">使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure</span><span class="sxs-lookup"><span data-stu-id="9ec66-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="9ec66-145">裝載和部署在 Azure 上的 ASP.NET Core 預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="9ec66-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
