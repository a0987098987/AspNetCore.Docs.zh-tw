---
title: ASP.NET Core 目錄結構
author: guardrex
description: 了解已發行之 ASP.NET Core 應用程式的目錄結構。
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 8e2693397f826d0e9a36ff52aa1d1d623b31043d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960823"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="2e565-103">ASP.NET Core 目錄結構</span><span class="sxs-lookup"><span data-stu-id="2e565-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="2e565-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2e565-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2e565-105">在 ASP.NET Core 中，所發佈的應用程式目錄 *publish* 會由應用程式檔案、設定檔、靜態資產、套件及執行階段 (適用於[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)) 所組成。</span><span class="sxs-lookup"><span data-stu-id="2e565-105">In ASP.NET Core, the published application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for [self-contained deployments](/dotnet/core/deploying/#self-contained-deployments-scd)).</span></span>


| <span data-ttu-id="2e565-106">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="2e565-106">App Type</span></span> | <span data-ttu-id="2e565-107">目錄結構</span><span class="sxs-lookup"><span data-stu-id="2e565-107">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="2e565-108">架構相依部署</span><span class="sxs-lookup"><span data-stu-id="2e565-108">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="2e565-109">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="2e565-109">publish&dagger;</span></span><ul><li><span data-ttu-id="2e565-110">Logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</span><span class="sxs-lookup"><span data-stu-id="2e565-110">Logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="2e565-111">Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</span><span class="sxs-lookup"><span data-stu-id="2e565-111">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="2e565-112">Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</span><span class="sxs-lookup"><span data-stu-id="2e565-112">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="2e565-113">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="2e565-113">wwwroot&dagger;</span></span></li><li><span data-ttu-id="2e565-114">\*\.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="2e565-114">\*\.dll files</span></span></li><li><span data-ttu-id="2e565-115">\<assembly-name>.deps.json</span><span class="sxs-lookup"><span data-stu-id="2e565-115">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="2e565-116">\<assembly-name>.dll</span><span class="sxs-lookup"><span data-stu-id="2e565-116">\<assembly-name>.dll</span></span></li><li><span data-ttu-id="2e565-117">\<assembly-name>.pdb</span><span class="sxs-lookup"><span data-stu-id="2e565-117">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="2e565-118">\<assembly-name>.PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="2e565-118">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="2e565-119">\<assembly-name>.PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="2e565-119">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="2e565-120">\<assembly-name>.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="2e565-120">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="2e565-121">web.config (IIS 部署)</span><span class="sxs-lookup"><span data-stu-id="2e565-121">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="2e565-122">自封式部署</span><span class="sxs-lookup"><span data-stu-id="2e565-122">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="2e565-123">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="2e565-123">publish&dagger;</span></span><ul><li><span data-ttu-id="2e565-124">Logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</span><span class="sxs-lookup"><span data-stu-id="2e565-124">Logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="2e565-125">refs&dagger;</span><span class="sxs-lookup"><span data-stu-id="2e565-125">refs&dagger;</span></span></li><li><span data-ttu-id="2e565-126">Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</span><span class="sxs-lookup"><span data-stu-id="2e565-126">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="2e565-127">Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</span><span class="sxs-lookup"><span data-stu-id="2e565-127">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="2e565-128">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="2e565-128">wwwroot&dagger;</span></span></li><li><span data-ttu-id="2e565-129">\*.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="2e565-129">\*.dll files</span></span></li><li><span data-ttu-id="2e565-130">\<assembly-name>.deps.json</span><span class="sxs-lookup"><span data-stu-id="2e565-130">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="2e565-131">\<assembly-name>.exe</span><span class="sxs-lookup"><span data-stu-id="2e565-131">\<assembly-name>.exe</span></span></li><li><span data-ttu-id="2e565-132">\<assembly-name>.pdb</span><span class="sxs-lookup"><span data-stu-id="2e565-132">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="2e565-133">\<assembly-name>.PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="2e565-133">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="2e565-134">\<assembly-name>.PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="2e565-134">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="2e565-135">\<assembly-name>.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="2e565-135">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="2e565-136">web.config (IIS 部署)</span><span class="sxs-lookup"><span data-stu-id="2e565-136">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="2e565-137">&dagger;表示是目錄</span><span class="sxs-lookup"><span data-stu-id="2e565-137">&dagger;Indicates a directory</span></span>

<span data-ttu-id="2e565-138">*publish* 目錄代表部署的「內容根目錄路徑」(也稱為「應用程式基底路徑」)。</span><span class="sxs-lookup"><span data-stu-id="2e565-138">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="2e565-139">不論給予伺服器上所部署應用程式的 *publish* 目錄什麼名稱，其位置都會作為所裝載應用程式的伺服器實體路徑。</span><span class="sxs-lookup"><span data-stu-id="2e565-139">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="2e565-140">*wwwroot* 目錄 (如果存在) 只包含靜態資產。</span><span class="sxs-lookup"><span data-stu-id="2e565-140">The *wwwroot* directory, if present, only contains static assets.</span></span>

<span data-ttu-id="2e565-141">使用下列兩種方法其中之一，即可為部署建立 stdout *Logs* 目錄：</span><span class="sxs-lookup"><span data-stu-id="2e565-141">The stdout *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="2e565-142">將下列 `<Target>` 元素新增至專案檔：</span><span class="sxs-lookup"><span data-stu-id="2e565-142">Add the following `<Target>` element to the project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   <span data-ttu-id="2e565-143">`<MakeDir>` 元素會在所發佈的輸出中建立一個空的 [Logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2e565-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="2e565-144">此元素會使用 `PublishDir` 屬性來判斷用於建立資料夾的目標位置。</span><span class="sxs-lookup"><span data-stu-id="2e565-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="2e565-145">數個部署方法 (例如 Web Deploy) 在部署期間都會略過空的資料夾。</span><span class="sxs-lookup"><span data-stu-id="2e565-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="2e565-146">`<WriteLinesToFile>` 元素會在 [Logs] 資料夾中產生檔案，用以確保將資料夾部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="2e565-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="2e565-147">請注意，如果背景工作處理序沒有目標資料夾的寫入存取權，則建立資料夾時仍然可能失敗。</span><span class="sxs-lookup"><span data-stu-id="2e565-147">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="2e565-148">在部署中的伺服器上實體建立 *Logs* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2e565-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="2e565-149">部署目錄會要求「讀取/執行」權限。</span><span class="sxs-lookup"><span data-stu-id="2e565-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="2e565-150">*Logs* 目錄會要求「讀取/寫入」權限。</span><span class="sxs-lookup"><span data-stu-id="2e565-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="2e565-151">其他可供寫入檔案的目錄會要求「讀取/寫入」權限。</span><span class="sxs-lookup"><span data-stu-id="2e565-151">Additional directories where files are written require Read/Write permissions.</span></span>
