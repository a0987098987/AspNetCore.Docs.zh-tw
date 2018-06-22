---
title: 發行 ASP.NET Core Azure Web 應用程式的 SignalR 應用程式
author: rachelappel
description: 發行 ASP.NET Core Azure Web 應用程式的 SignalR 應用程式
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271914"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="a5945-103">發行 ASP.NET Core SignalR 應用程式至 Azure Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a5945-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="a5945-104">[Azure Web 應用程式](/azure/app-service/app-service-web-overview)是[Microsoft 雲端運算](https://azure.microsoft.com/)裝載 web 應用程式，包括 ASP.NET Core 平台服務。</span><span class="sxs-lookup"><span data-stu-id="a5945-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="a5945-105">這篇文章是指從 Visual Studio 發行 ASP.NET Core SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a5945-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="a5945-106">請瀏覽[Azure SignalR 服務](https://azure.microsoft.com/en-gb/services/signalr-service?)如需有關在 Azure 上使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="a5945-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="a5945-107">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="a5945-107">Publish the app</span></span>

<span data-ttu-id="a5945-108">Visual Studio 會提供內建的工具發行至 Azure Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a5945-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="a5945-109">Visual Studio 程式碼的使用者可以使用[Azure CLI](/cli/azure)發行至 Azure 的應用程式的命令。</span><span class="sxs-lookup"><span data-stu-id="a5945-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="a5945-110">本文涵蓋發行 Visual Studio 中使用的工具。</span><span class="sxs-lookup"><span data-stu-id="a5945-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="a5945-111">若要發行應用程式使用 Azure CLI，請參閱[及命令列工具，將 ASP.NET Core 應用程式發行至 Azure](xref:tutorials/publish-to-azure-webapp-using-cli)。</span><span class="sxs-lookup"><span data-stu-id="a5945-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="a5945-112">中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="a5945-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="a5945-113">確認**建立新**簽入**挑選發行目標** 對話方塊中，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="a5945-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![選取發行目標](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="a5945-115">輸入中的下列資訊**建立 App Service**對話方塊並選取**建立**。</span><span class="sxs-lookup"><span data-stu-id="a5945-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="a5945-116">項目</span><span class="sxs-lookup"><span data-stu-id="a5945-116">Item</span></span> | <span data-ttu-id="a5945-117">描述</span><span class="sxs-lookup"><span data-stu-id="a5945-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="a5945-118">**應用程式名稱**</span><span class="sxs-lookup"><span data-stu-id="a5945-118">**App name**</span></span> | <span data-ttu-id="a5945-119">應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="a5945-119">A unique name of the app.</span></span> |
| <span data-ttu-id="a5945-120">**訂用帳戶**</span><span class="sxs-lookup"><span data-stu-id="a5945-120">**Subscription**</span></span> | <span data-ttu-id="a5945-121">應用程式使用 Azure 訂閱。</span><span class="sxs-lookup"><span data-stu-id="a5945-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="a5945-122">**資源群組**</span><span class="sxs-lookup"><span data-stu-id="a5945-122">**Resource Group**</span></span> | <span data-ttu-id="a5945-123">相關資源的應用程式所屬的群組。</span><span class="sxs-lookup"><span data-stu-id="a5945-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="a5945-124">**裝載計劃**</span><span class="sxs-lookup"><span data-stu-id="a5945-124">**Hosting Plan**</span></span> | <span data-ttu-id="a5945-125">Web 應用程式定價方案。</span><span class="sxs-lookup"><span data-stu-id="a5945-125">The pricing plan for the web app.</span></span> |

![建立應用程式服務](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="a5945-127">Visual Studio 會完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="a5945-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="a5945-128">建立發行設定檔包含發行設定。</span><span class="sxs-lookup"><span data-stu-id="a5945-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="a5945-129">建立或使用現有*Azure Web 應用程式*提供詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a5945-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="a5945-130">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a5945-130">Publishes the app.</span></span>
* <span data-ttu-id="a5945-131">啟動瀏覽器中，載入已發佈的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a5945-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="a5945-132">請注意 URL 的格式，應用程式是 *{應用程式名稱}.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="a5945-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="a5945-133">例如，應用程式命名`SignalRChattR`具有一個 URL，看起來像`https://signalrchattr.azurewebsites.net`。</span><span class="sxs-lookup"><span data-stu-id="a5945-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="a5945-134">如果 HTTP 502.2 錯誤發生，請參閱[部署 ASP.NET Core 預覽發行至 Azure App Service](xref:host-and-deploy/azure-apps/index)加以解決。</span><span class="sxs-lookup"><span data-stu-id="a5945-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="a5945-135">設定 SignalR web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a5945-135">Configure SignalR web app</span></span>

<span data-ttu-id="a5945-136">ASP.NET Core SignalR 應用程式必須具有 Azure Web 應用程式發行[ARR 親和性](https://en.wikipedia.org/wiki/Application_Request_Routing)啟用。</span><span class="sxs-lookup"><span data-stu-id="a5945-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="a5945-137">[WebSockets](xref:fundamentals/websockets)必須啟用，以允許 WebSockets 傳輸函式。</span><span class="sxs-lookup"><span data-stu-id="a5945-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="a5945-138">在 Azure 入口網站中，瀏覽至**應用程式設定**web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a5945-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="a5945-139">設定**WebSockets**至**上**，並確認**ARR 親和性**是**上**。</span><span class="sxs-lookup"><span data-stu-id="a5945-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![在 Azure 入口網站的 azure Web 應用程式設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="a5945-141">WebSockets 和其他傳輸[受到為基礎的 App Service 方案](/azure/azure-subscription-service-limits#app-service-limits)。</span><span class="sxs-lookup"><span data-stu-id="a5945-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="a5945-142">相關資源</span><span class="sxs-lookup"><span data-stu-id="a5945-142">Related resources</span></span>

* [<span data-ttu-id="a5945-143">將 ASP.NET Core 應用程式發行至 Azure 中，使用命令列工具</span><span class="sxs-lookup"><span data-stu-id="a5945-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="a5945-144">將 ASP.NET Core 應用程式發行至 Azure，使用 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5945-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="a5945-145">主機和部署在 Azure 上的 ASP.NET Core 預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="a5945-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
