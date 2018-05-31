---
title: ASP.NET Core 目錄結構
author: guardrex
description: 了解已發行之 ASP.NET Core 應用程式的目錄結構。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838432"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="5bf42-103">ASP.NET Core 目錄結構</span><span class="sxs-lookup"><span data-stu-id="5bf42-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="5bf42-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5bf42-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5bf42-105">在 ASP.NET Core 中，所發佈的應用程式目錄 *publish* 會由應用程式檔案、設定檔、靜態資產、套件及執行階段 (適用於[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)) 所組成。</span><span class="sxs-lookup"><span data-stu-id="5bf42-105">In ASP.NET Core, the published application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for [self-contained deployments](/dotnet/core/deploying/#self-contained-deployments-scd)).</span></span>


| <span data-ttu-id="5bf42-106">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="5bf42-106">App Type</span></span> | <span data-ttu-id="5bf42-107">目錄結構</span><span class="sxs-lookup"><span data-stu-id="5bf42-107">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="5bf42-108">架構相依部署</span><span class="sxs-lookup"><span data-stu-id="5bf42-108">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="5bf42-109">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="5bf42-109">publish&dagger;</span></span><ul><li><span data-ttu-id="5bf42-110">logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</span><span class="sxs-lookup"><span data-stu-id="5bf42-110">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="5bf42-111">Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</span><span class="sxs-lookup"><span data-stu-id="5bf42-111">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="5bf42-112">Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</span><span class="sxs-lookup"><span data-stu-id="5bf42-112">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="5bf42-113">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="5bf42-113">wwwroot&dagger;</span></span></li><li><span data-ttu-id="5bf42-114">\*\.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="5bf42-114">\*\.dll files</span></span></li><li><span data-ttu-id="5bf42-115">\<assembly-name>.deps.json</span><span class="sxs-lookup"><span data-stu-id="5bf42-115">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="5bf42-116">\<assembly-name>.dll</span><span class="sxs-lookup"><span data-stu-id="5bf42-116">\<assembly-name>.dll</span></span></li><li><span data-ttu-id="5bf42-117">\<assembly-name>.pdb</span><span class="sxs-lookup"><span data-stu-id="5bf42-117">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="5bf42-118">\<assembly-name>.PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="5bf42-118">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="5bf42-119">\<assembly-name>.PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="5bf42-119">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="5bf42-120">\<assembly-name>.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="5bf42-120">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="5bf42-121">web.config (IIS 部署)</span><span class="sxs-lookup"><span data-stu-id="5bf42-121">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="5bf42-122">自封式部署</span><span class="sxs-lookup"><span data-stu-id="5bf42-122">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="5bf42-123">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="5bf42-123">publish&dagger;</span></span><ul><li><span data-ttu-id="5bf42-124">logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</span><span class="sxs-lookup"><span data-stu-id="5bf42-124">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="5bf42-125">refs&dagger;</span><span class="sxs-lookup"><span data-stu-id="5bf42-125">refs&dagger;</span></span></li><li><span data-ttu-id="5bf42-126">Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</span><span class="sxs-lookup"><span data-stu-id="5bf42-126">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="5bf42-127">Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</span><span class="sxs-lookup"><span data-stu-id="5bf42-127">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="5bf42-128">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="5bf42-128">wwwroot&dagger;</span></span></li><li><span data-ttu-id="5bf42-129">\*.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="5bf42-129">\*.dll files</span></span></li><li><span data-ttu-id="5bf42-130">\<assembly-name>.deps.json</span><span class="sxs-lookup"><span data-stu-id="5bf42-130">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="5bf42-131">\<assembly-name>.exe</span><span class="sxs-lookup"><span data-stu-id="5bf42-131">\<assembly-name>.exe</span></span></li><li><span data-ttu-id="5bf42-132">\<assembly-name>.pdb</span><span class="sxs-lookup"><span data-stu-id="5bf42-132">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="5bf42-133">\<assembly-name>.PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="5bf42-133">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="5bf42-134">\<assembly-name>.PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="5bf42-134">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="5bf42-135">\<assembly-name>.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="5bf42-135">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="5bf42-136">web.config (IIS 部署)</span><span class="sxs-lookup"><span data-stu-id="5bf42-136">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="5bf42-137">&dagger;表示是目錄</span><span class="sxs-lookup"><span data-stu-id="5bf42-137">&dagger;Indicates a directory</span></span>

<span data-ttu-id="5bf42-138">*publish* 目錄代表部署的「內容根目錄路徑」(也稱為「應用程式基底路徑」)。</span><span class="sxs-lookup"><span data-stu-id="5bf42-138">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="5bf42-139">不論給予伺服器上所部署應用程式的 *publish* 目錄什麼名稱，其位置都會作為所裝載應用程式的伺服器實體路徑。</span><span class="sxs-lookup"><span data-stu-id="5bf42-139">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="5bf42-140">*wwwroot* 目錄 (如果存在) 只包含靜態資產。</span><span class="sxs-lookup"><span data-stu-id="5bf42-140">The *wwwroot* directory, if present, only contains static assets.</span></span>

<span data-ttu-id="5bf42-141">使用下列兩種方法其中之一，即可為部署建立 stdout *logs* 目錄：</span><span class="sxs-lookup"><span data-stu-id="5bf42-141">The stdout *logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="5bf42-142">將下列 `<Target>` 元素新增至專案檔：</span><span class="sxs-lookup"><span data-stu-id="5bf42-142">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="5bf42-143">`<MakeDir>` 元素會在所發佈的輸出中建立一個空的 [Logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5bf42-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="5bf42-144">此元素會使用 `PublishDir` 屬性來判斷用於建立資料夾的目標位置。</span><span class="sxs-lookup"><span data-stu-id="5bf42-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="5bf42-145">數個部署方法 (例如 Web Deploy) 在部署期間都會略過空的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5bf42-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="5bf42-146">`<WriteLinesToFile>` 元素會在 [Logs] 資料夾中產生檔案，用以確保將資料夾部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="5bf42-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="5bf42-147">請注意，如果背景工作處理序沒有目標資料夾的寫入存取權，則建立資料夾時仍然可能失敗。</span><span class="sxs-lookup"><span data-stu-id="5bf42-147">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="5bf42-148">在部署中的伺服器上實體建立 *Logs* 目錄。</span><span class="sxs-lookup"><span data-stu-id="5bf42-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="5bf42-149">部署目錄會要求「讀取/執行」權限。</span><span class="sxs-lookup"><span data-stu-id="5bf42-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="5bf42-150">*Logs* 目錄會要求「讀取/寫入」權限。</span><span class="sxs-lookup"><span data-stu-id="5bf42-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="5bf42-151">其他可供寫入檔案的目錄會要求「讀取/寫入」權限。</span><span class="sxs-lookup"><span data-stu-id="5bf42-151">Additional directories where files are written require Read/Write permissions.</span></span>
