---
title: ASP.NET Core 目錄結構
author: guardrex
description: 了解已發行之 ASP.NET Core 應用程式的目錄結構。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ee0bebb8b5c688f8471d6420d1641b87ac271f6c
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284561"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="40ef5-103">ASP.NET Core 目錄結構</span><span class="sxs-lookup"><span data-stu-id="40ef5-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="40ef5-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="40ef5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="40ef5-105">*publish* 目錄包含 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令所產生的應用程式可部署資產。</span><span class="sxs-lookup"><span data-stu-id="40ef5-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="40ef5-106">此目錄包含：</span><span class="sxs-lookup"><span data-stu-id="40ef5-106">The directory contains:</span></span>

* <span data-ttu-id="40ef5-107">應用程式檔案</span><span class="sxs-lookup"><span data-stu-id="40ef5-107">Application files</span></span>
* <span data-ttu-id="40ef5-108">組態檔</span><span class="sxs-lookup"><span data-stu-id="40ef5-108">Configuration files</span></span>
* <span data-ttu-id="40ef5-109">靜態資產</span><span class="sxs-lookup"><span data-stu-id="40ef5-109">Static assets</span></span>
* <span data-ttu-id="40ef5-110">封裝</span><span class="sxs-lookup"><span data-stu-id="40ef5-110">Packages</span></span>
* <span data-ttu-id="40ef5-111">執行階段 (僅限[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd))</span><span class="sxs-lookup"><span data-stu-id="40ef5-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="40ef5-112">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="40ef5-112">App Type</span></span> | <span data-ttu-id="40ef5-113">目錄結構</span><span class="sxs-lookup"><span data-stu-id="40ef5-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="40ef5-114">架構相依部署</span><span class="sxs-lookup"><span data-stu-id="40ef5-114">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="40ef5-115">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="40ef5-115">publish&dagger;</span></span><ul><li><span data-ttu-id="40ef5-116">Logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</span><span class="sxs-lookup"><span data-stu-id="40ef5-116">Logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="40ef5-117">Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</span><span class="sxs-lookup"><span data-stu-id="40ef5-117">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="40ef5-118">Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</span><span class="sxs-lookup"><span data-stu-id="40ef5-118">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="40ef5-119">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="40ef5-119">wwwroot&dagger;</span></span></li><li><span data-ttu-id="40ef5-120">\*\.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="40ef5-120">\*\.dll files</span></span></li><li><span data-ttu-id="40ef5-121">{組件名稱}.deps.json</span><span class="sxs-lookup"><span data-stu-id="40ef5-121">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="40ef5-122">{組件名稱}.dll</span><span class="sxs-lookup"><span data-stu-id="40ef5-122">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="40ef5-123">{組件名稱}.pdb</span><span class="sxs-lookup"><span data-stu-id="40ef5-123">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="40ef5-124">{組件名稱}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="40ef5-124">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="40ef5-125">{組件名稱}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="40ef5-125">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="40ef5-126">{組件名稱}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="40ef5-126">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="40ef5-127">web.config (IIS 部署)</span><span class="sxs-lookup"><span data-stu-id="40ef5-127">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="40ef5-128">自封式部署</span><span class="sxs-lookup"><span data-stu-id="40ef5-128">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="40ef5-129">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="40ef5-129">publish&dagger;</span></span><ul><li><span data-ttu-id="40ef5-130">Logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</span><span class="sxs-lookup"><span data-stu-id="40ef5-130">Logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="40ef5-131">Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</span><span class="sxs-lookup"><span data-stu-id="40ef5-131">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="40ef5-132">Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</span><span class="sxs-lookup"><span data-stu-id="40ef5-132">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="40ef5-133">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="40ef5-133">wwwroot&dagger;</span></span></li><li><span data-ttu-id="40ef5-134">\*.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="40ef5-134">\*.dll files</span></span></li><li><span data-ttu-id="40ef5-135">{組件名稱}.deps.json</span><span class="sxs-lookup"><span data-stu-id="40ef5-135">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="40ef5-136">{組件名稱}.dll</span><span class="sxs-lookup"><span data-stu-id="40ef5-136">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="40ef5-137">{組件名稱}.exe</span><span class="sxs-lookup"><span data-stu-id="40ef5-137">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="40ef5-138">{組件名稱}.pdb</span><span class="sxs-lookup"><span data-stu-id="40ef5-138">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="40ef5-139">{組件名稱}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="40ef5-139">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="40ef5-140">{組件名稱}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="40ef5-140">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="40ef5-141">{組件名稱}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="40ef5-141">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="40ef5-142">web.config (IIS 部署)</span><span class="sxs-lookup"><span data-stu-id="40ef5-142">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="40ef5-143">&dagger;表示是目錄</span><span class="sxs-lookup"><span data-stu-id="40ef5-143">&dagger;Indicates a directory</span></span>

<span data-ttu-id="40ef5-144">*publish* 目錄代表部署的「內容根目錄路徑」(也稱為「應用程式基底路徑」)。</span><span class="sxs-lookup"><span data-stu-id="40ef5-144">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="40ef5-145">不論給予伺服器上所部署應用程式的 *publish* 目錄什麼名稱，其位置都會作為所裝載應用程式的伺服器實體路徑。</span><span class="sxs-lookup"><span data-stu-id="40ef5-145">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="40ef5-146">*wwwroot* 目錄 (如果存在) 只包含靜態資產。</span><span class="sxs-lookup"><span data-stu-id="40ef5-146">The *wwwroot* directory, if present, only contains static assets.</span></span>

<span data-ttu-id="40ef5-147">使用下列兩種方法其中之一，即可為部署建立 stdout *Logs* 目錄：</span><span class="sxs-lookup"><span data-stu-id="40ef5-147">The stdout *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="40ef5-148">將下列 `<Target>` 元素新增至專案檔：</span><span class="sxs-lookup"><span data-stu-id="40ef5-148">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="40ef5-149">`<MakeDir>` 元素會在所發佈的輸出中建立一個空的 [Logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="40ef5-149">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="40ef5-150">此元素會使用 `PublishDir` 屬性來判斷用於建立資料夾的目標位置。</span><span class="sxs-lookup"><span data-stu-id="40ef5-150">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="40ef5-151">數個部署方法 (例如 Web Deploy) 在部署期間都會略過空的資料夾。</span><span class="sxs-lookup"><span data-stu-id="40ef5-151">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="40ef5-152">`<WriteLinesToFile>` 元素會在 [Logs] 資料夾中產生檔案，用以確保將資料夾部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="40ef5-152">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="40ef5-153">如果背景工作處理序沒有目標資料夾的寫入權限，則使用此方法建立資料夾時會失敗。</span><span class="sxs-lookup"><span data-stu-id="40ef5-153">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="40ef5-154">在部署中的伺服器上實體建立 *Logs* 目錄。</span><span class="sxs-lookup"><span data-stu-id="40ef5-154">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="40ef5-155">部署目錄會要求「讀取/執行」權限。</span><span class="sxs-lookup"><span data-stu-id="40ef5-155">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="40ef5-156">*Logs* 目錄會要求「讀取/寫入」權限。</span><span class="sxs-lookup"><span data-stu-id="40ef5-156">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="40ef5-157">其他可供寫入檔案的目錄會要求「讀取/寫入」權限。</span><span class="sxs-lookup"><span data-stu-id="40ef5-157">Additional directories where files are written require Read/Write permissions.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40ef5-158">其他資源</span><span class="sxs-lookup"><span data-stu-id="40ef5-158">Additional resources</span></span>

* [<span data-ttu-id="40ef5-159">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="40ef5-159">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="40ef5-160">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="40ef5-160">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="40ef5-161">目標架構</span><span class="sxs-lookup"><span data-stu-id="40ef5-161">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="40ef5-162">.NET Core RID 類別目錄</span><span class="sxs-lookup"><span data-stu-id="40ef5-162">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
