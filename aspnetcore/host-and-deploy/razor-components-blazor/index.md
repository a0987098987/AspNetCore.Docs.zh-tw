---
title: 裝載和部署 Razor 元件和 Blazor
author: guardrex
description: 探索如何裝載和部署 Razor 元件和 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070305"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a><span data-ttu-id="52c9f-103">裝載和部署 Razor 元件和 Blazor</span><span class="sxs-lookup"><span data-stu-id="52c9f-103">Host and deploy Razor Components and Blazor</span></span>

<span data-ttu-id="52c9f-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="52c9f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="52c9f-105">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="52c9f-105">Publish the app</span></span>

<span data-ttu-id="52c9f-106">應用程式在發行組態中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="52c9f-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="52c9f-107">整合式開發環境 (IDE) 可能會自動使用內建的發佈功能處理執行 `dotnet publish` 命令，因此可能不需要以手動方式從命令提示字元執行命令，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="52c9f-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

`dotnet publish` <span data-ttu-id="52c9f-108">會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)，並[建置](/dotnet/core/tools/dotnet-build)專案，然後才建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="52c9f-108">triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="52c9f-109">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="52c9f-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="52c9f-110">部署會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="52c9f-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="52c9f-111">*publish* 資料夾中的資產會部署至網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="52c9f-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="52c9f-112">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="52c9f-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="52c9f-113">部署</span><span class="sxs-lookup"><span data-stu-id="52c9f-113">Deployment</span></span>

<span data-ttu-id="52c9f-114">如需部署指導方針，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="52c9f-114">For deployment guidance, see the following topics:</span></span>

* [<span data-ttu-id="52c9f-115">用戶端 Blazor</span><span class="sxs-lookup"><span data-stu-id="52c9f-115">Client-side Blazor</span></span>](xref:host-and-deploy/razor-components-blazor/blazor)
* [<span data-ttu-id="52c9f-116">伺服器端 ASP.NET Core Razor 元件</span><span class="sxs-lookup"><span data-stu-id="52c9f-116">Server-side ASP.NET Core Razor Components</span></span>](xref:host-and-deploy/razor-components-blazor/razor-components)
