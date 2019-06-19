---
title: 裝載及部署 ASP.NET Core Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 8a5ac5c58e7ceab07e55da8b61ebb01f7ac984bc
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153198"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="7b6f4-103">裝載及部署 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="7b6f4-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="7b6f4-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7b6f4-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="7b6f4-105">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="7b6f4-105">Publish the app</span></span>

<span data-ttu-id="7b6f4-106">應用程式會在發行組態中發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7b6f4-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7b6f4-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7b6f4-108">從巡覽列中選取 [建置]   > [發佈 {應用程式}]  。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="7b6f4-109">選取「發佈目標」  。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-109">Select the *publish target*.</span></span> <span data-ttu-id="7b6f4-110">若要在本機發佈，請選取 [資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="7b6f4-111">接受 [選擇資料夾]  欄位中的預設位置，或指定不同的位置。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="7b6f4-112">選取 [發行]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-112">Select the **Publish** button.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="7b6f4-113">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7b6f4-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="7b6f4-114">使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：</span><span class="sxs-lookup"><span data-stu-id="7b6f4-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="7b6f4-115">發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="7b6f4-116">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="7b6f4-117">Blazor 用戶端應用程式會發佈至 /bin/Release/{目標 FRAMEWORK}/publish/{組件名稱}/dist  資料夾。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="7b6f4-118">Blazor 伺服器端應用程式會發佈至 */bin/Release/{目標 FRAMEWORK}/publish* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="7b6f4-119">該資料夾中的資產均會部署至 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="7b6f4-120">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="7b6f4-121">部署</span><span class="sxs-lookup"><span data-stu-id="7b6f4-121">Deployment</span></span>

<span data-ttu-id="7b6f4-122">如需部署指導方針，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="7b6f4-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="7b6f4-123">使用 Azure Storage 的 Blazor 無伺服器裝載</span><span class="sxs-lookup"><span data-stu-id="7b6f4-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="7b6f4-124">Blazor 用戶端應用程式可從 [Azure Storage](https://azure.microsoft.com/services/storage/) 直接從儲存體容器以靜態內容方式提供。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="7b6f4-125">如需詳細資訊，請參閱[裝載及部署 ASP.NET Core Blazor 用戶端 (獨立部署)：Azure 儲存體](xref:host-and-deploy/blazor/client-side#azure-storage)。</span><span class="sxs-lookup"><span data-stu-id="7b6f4-125">For more information, see [Host and deploy ASP.NET Core Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
