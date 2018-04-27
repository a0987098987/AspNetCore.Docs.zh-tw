---
title: ASP.NET Core 目錄結構
author: guardrex
description: 深入了解已發行的 ASP.NET Core 應用程式的目錄結構。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ac9b777bcc7f4a8634161fc1347a4d0fdc3b4784
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/19/2018
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="a106d-103">ASP.NET Core 目錄結構</span><span class="sxs-lookup"><span data-stu-id="a106d-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="a106d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a106d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a106d-105">在 ASP.NET Core，已發行的應用程式目錄，*發行*，組成應用程式檔案、 組態檔、 靜態資產、 封裝和執行階段 (如[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="a106d-105">In ASP.NET Core, the published application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for [self-contained deployments](/dotnet/core/deploying/#self-contained-deployments-scd)).</span></span>


| <span data-ttu-id="a106d-106">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="a106d-106">App Type</span></span> | <span data-ttu-id="a106d-107">目錄結構</span><span class="sxs-lookup"><span data-stu-id="a106d-107">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="a106d-108">Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="a106d-108">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="a106d-109">發行&dagger;</span><span class="sxs-lookup"><span data-stu-id="a106d-109">publish&dagger;</span></span><ul><li><span data-ttu-id="a106d-110">記錄檔&dagger;（除非需要接收 stdout 記錄檔的選擇性）</span><span class="sxs-lookup"><span data-stu-id="a106d-110">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="a106d-111">檢視&dagger;（MVC 應用程式; 如果不先行編譯的檢視）</span><span class="sxs-lookup"><span data-stu-id="a106d-111">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="a106d-112">頁面&dagger;（MVC 或 Razor 網頁應用程式; 如果不先行編譯的頁面）</span><span class="sxs-lookup"><span data-stu-id="a106d-112">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="a106d-113">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="a106d-113">wwwroot&dagger;</span></span></li><li><span data-ttu-id="a106d-114">\*\.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="a106d-114">\*\.dll files</span></span></li><li><span data-ttu-id="a106d-115">\<組件名稱 >。 deps.json</span><span class="sxs-lookup"><span data-stu-id="a106d-115">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="a106d-116">\<組件名稱 >.dll</span><span class="sxs-lookup"><span data-stu-id="a106d-116">\<assembly-name>.dll</span></span></li><li><span data-ttu-id="a106d-117">\<組件名稱 >.pdb</span><span class="sxs-lookup"><span data-stu-id="a106d-117">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="a106d-118">\<組件名稱 >。PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="a106d-118">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="a106d-119">\<組件名稱 >。PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="a106d-119">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="a106d-120">\<組件名稱 >。 runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="a106d-120">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="a106d-121">web.config （IIS 部署）</span><span class="sxs-lookup"><span data-stu-id="a106d-121">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="a106d-122">獨立的部署</span><span class="sxs-lookup"><span data-stu-id="a106d-122">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="a106d-123">發行&dagger;</span><span class="sxs-lookup"><span data-stu-id="a106d-123">publish&dagger;</span></span><ul><li><span data-ttu-id="a106d-124">記錄檔&dagger;（除非需要接收 stdout 記錄檔的選擇性）</span><span class="sxs-lookup"><span data-stu-id="a106d-124">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="a106d-125">refs&dagger;</span><span class="sxs-lookup"><span data-stu-id="a106d-125">refs&dagger;</span></span></li><li><span data-ttu-id="a106d-126">檢視&dagger;（MVC 應用程式; 如果不先行編譯的檢視）</span><span class="sxs-lookup"><span data-stu-id="a106d-126">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="a106d-127">頁面&dagger;（MVC 或 Razor 網頁應用程式; 如果不先行編譯的頁面）</span><span class="sxs-lookup"><span data-stu-id="a106d-127">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="a106d-128">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="a106d-128">wwwroot&dagger;</span></span></li><li><span data-ttu-id="a106d-129">\*.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="a106d-129">\*.dll files</span></span></li><li><span data-ttu-id="a106d-130">\<組件名稱 >。 deps.json</span><span class="sxs-lookup"><span data-stu-id="a106d-130">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="a106d-131">\<組件名稱 >.exe</span><span class="sxs-lookup"><span data-stu-id="a106d-131">\<assembly-name>.exe</span></span></li><li><span data-ttu-id="a106d-132">\<組件名稱 >.pdb</span><span class="sxs-lookup"><span data-stu-id="a106d-132">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="a106d-133">\<組件名稱 >。PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="a106d-133">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="a106d-134">\<組件名稱 >。PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="a106d-134">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="a106d-135">\<組件名稱 >。 runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="a106d-135">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="a106d-136">web.config （IIS 部署）</span><span class="sxs-lookup"><span data-stu-id="a106d-136">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="a106d-137">&dagger;表示目錄</span><span class="sxs-lookup"><span data-stu-id="a106d-137">&dagger;Indicates a directory</span></span>

<span data-ttu-id="a106d-138">*發行*目錄代表*內容的根路徑*，也稱為*應用程式基底路徑*的部署。</span><span class="sxs-lookup"><span data-stu-id="a106d-138">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="a106d-139">若要指定任何名稱*發行*目錄中的伺服器上部署的應用程式，其位置做為裝載應用程式伺服器的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="a106d-139">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="a106d-140">*Wwwroot*目錄中，如果有的話，只包含靜態資產。</span><span class="sxs-lookup"><span data-stu-id="a106d-140">The *wwwroot* directory, if present, only contains static assets.</span></span>

<span data-ttu-id="a106d-141">Stdout*記錄*部署使用其中一種下列兩種方法可以建立目錄：</span><span class="sxs-lookup"><span data-stu-id="a106d-141">The stdout *logs* directory can be created the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="a106d-142">加入下列`<Target>`項目加入專案檔：</span><span class="sxs-lookup"><span data-stu-id="a106d-142">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="a106d-143">`<MakeDir>`項目會建立空*記錄*中發行的輸出資料夾。</span><span class="sxs-lookup"><span data-stu-id="a106d-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="a106d-144">項目使用`PublishDir`屬性來判斷建立此資料夾的目標位置。</span><span class="sxs-lookup"><span data-stu-id="a106d-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="a106d-145">數種部署方法，例如 Web Deploy，請在部署期間，略過空資料夾。</span><span class="sxs-lookup"><span data-stu-id="a106d-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="a106d-146">`<WriteLinesToFile>`項目會產生的檔案中*記錄*資料夾中，以確保部署到伺服器的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a106d-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="a106d-147">請注意是否工作者處理序不具有寫入存取權的目標資料夾可能仍然無法建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="a106d-147">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="a106d-148">實際建立*記錄*目錄以部署在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="a106d-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="a106d-149">部署目錄中需要讀取/執行權限。</span><span class="sxs-lookup"><span data-stu-id="a106d-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="a106d-150">*記錄*目錄需要讀取/寫入權限。</span><span class="sxs-lookup"><span data-stu-id="a106d-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="a106d-151">寫入檔案的其他目錄需要讀取/寫入權限。</span><span class="sxs-lookup"><span data-stu-id="a106d-151">Additional directories where files are written require Read/Write permissions.</span></span>
