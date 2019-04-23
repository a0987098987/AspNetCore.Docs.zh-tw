---
title: 裝載和部署 Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 774fbb6fdaab14a015db4fb39de2e1ea73a1837b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982636"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="66a26-103">裝載和部署 Blazor</span><span class="sxs-lookup"><span data-stu-id="66a26-103">Host and deploy Blazor</span></span>

<span data-ttu-id="66a26-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="66a26-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="66a26-105">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="66a26-105">Publish the app</span></span>

<span data-ttu-id="66a26-106">應用程式在發行組態中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="66a26-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="66a26-107">整合式開發環境 (IDE) 可能會自動使用內建的發佈功能處理執行 `dotnet publish` 命令，因此可能不需要以手動方式從命令提示字元執行命令，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="66a26-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="66a26-108">`dotnet publish` 會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)，並[建置](/dotnet/core/tools/dotnet-build)專案，然後才建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="66a26-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="66a26-109">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="66a26-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="66a26-110">部署會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="66a26-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="66a26-111">*publish* 資料夾中的資產會部署至網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="66a26-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="66a26-112">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="66a26-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="66a26-113">部署</span><span class="sxs-lookup"><span data-stu-id="66a26-113">Deployment</span></span>

<span data-ttu-id="66a26-114">如需部署指導方針，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="66a26-114">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
