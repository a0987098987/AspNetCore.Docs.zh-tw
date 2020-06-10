---
title: Visual Studio 容器工具搭配 ASP.NET Core
author: spboyer
description: 了解如何使用適用於 Windows 的 Visual Studio 工具和 Docker 對 ASP.NET Core 應用程式進行容器化。
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 391cc18e499cdfaa67beeced382fd53b16a668a2
ms.sourcegitcommit: 6a71b560d897e13ad5b61d07afe4fcb57f8ef6dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84105216"
---
# <a name="visual-studio-container-tools-with-aspnet-core"></a><span data-ttu-id="c1461-103">Visual Studio 容器工具搭配 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1461-103">Visual Studio Container Tools with ASP.NET Core</span></span>

<span data-ttu-id="c1461-104">Visual Studio 2017 及更新版本支援建置、偵錯和執行以 .NET Core 為目標的容器化 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1461-104">Visual Studio 2017 and later versions support building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="c1461-105">同時支援 Windows 和 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="c1461-105">Both Windows and Linux containers are supported.</span></span>

<span data-ttu-id="c1461-106">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c1461-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1461-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="c1461-107">Prerequisites</span></span>

* [<span data-ttu-id="c1461-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="c1461-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)
* <span data-ttu-id="c1461-109">已安裝 **.NET Core 跨平台開發**工作負載的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="c1461-109">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **.NET Core cross-platform development** workload</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="c1461-110">安裝與設定</span><span class="sxs-lookup"><span data-stu-id="c1461-110">Installation and setup</span></span>

<span data-ttu-id="c1461-111">若要進行 Docker 安裝，請先檢閱 [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) 中的資訊。</span><span class="sxs-lookup"><span data-stu-id="c1461-111">For Docker installation, first review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).</span></span> <span data-ttu-id="c1461-112">接下來，安裝 [適用於 Windows 的 Docker](https://docs.docker.com/docker-for-windows/install/)。</span><span class="sxs-lookup"><span data-stu-id="c1461-112">Next, install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="c1461-113">Docker for Windows 中的 **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** (共用磁碟機) 必須設定為支援磁碟區對應和偵錯。</span><span class="sxs-lookup"><span data-stu-id="c1461-113">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="c1461-114">以滑鼠右鍵按一下系統匣的 Docker 圖示，選取 [**設定**]，然後選取 [**共用磁片磁碟機**]。</span><span class="sxs-lookup"><span data-stu-id="c1461-114">Right-click the System Tray's Docker icon, select **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="c1461-115">選取 Docker 儲存檔案的磁碟機。</span><span class="sxs-lookup"><span data-stu-id="c1461-115">Select the drive where Docker stores files.</span></span> <span data-ttu-id="c1461-116">按一下 [套用] 。</span><span class="sxs-lookup"><span data-stu-id="c1461-116">Click **Apply**.</span></span>

![為容器選取共用本機 C 磁碟機的對話方塊](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="c1461-118">未設定 [共用磁碟機]\*\*\*\* 時，Visual Studio 2017 15.6 版和更新版本會顯示提示。</span><span class="sxs-lookup"><span data-stu-id="c1461-118">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-a-project-to-a-docker-container"></a><span data-ttu-id="c1461-119">將專案新增至 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="c1461-119">Add a project to a Docker container</span></span>

<span data-ttu-id="c1461-120">若要容器化 ASP.NET Core 專案，該專案必須以 .NET Core 為目標。</span><span class="sxs-lookup"><span data-stu-id="c1461-120">To containerize an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="c1461-121">同時支援 Linux 和 Windows 容器。</span><span class="sxs-lookup"><span data-stu-id="c1461-121">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="c1461-122">將 Docker 支援新增至專案時，請選擇 Windows 或 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="c1461-122">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="c1461-123">Docker 主機必須執行相同的容器類型。</span><span class="sxs-lookup"><span data-stu-id="c1461-123">The Docker host must be running the same container type.</span></span> <span data-ttu-id="c1461-124">若要變更執行中 Docker 執行個體中的容器類型，請以滑鼠右鍵按一下系統匣的 Docker 圖示，然後選擇 [Switch to Windows containers...] (切換至 Windows 容器...)\*\*\*\* 或 [Switch to Linux containers] (切換至 Linux 容器...)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="c1461-124">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="c1461-125">新增應用程式</span><span class="sxs-lookup"><span data-stu-id="c1461-125">New app</span></span>

<span data-ttu-id="c1461-126">使用 **ASP.NET Core Web 應用程式**專案範本來建立新的應用程式時，請選取 [啟用 Docker 支援]\*\*\*\* 核取方塊：</span><span class="sxs-lookup"><span data-stu-id="c1461-126">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![啟用 Docker 支援核取方塊](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

<span data-ttu-id="c1461-128">如果目標架構是 .NET Core，則 [OS]\*\*\*\* 下拉式清單會允許選取容器類型。</span><span class="sxs-lookup"><span data-stu-id="c1461-128">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="c1461-129">現有應用程式</span><span class="sxs-lookup"><span data-stu-id="c1461-129">Existing app</span></span>

<span data-ttu-id="c1461-130">針對以 .NET Core 為目標的 ASP.NET Core 專案，有兩個選項可透過工具來新增 Docker 支援。</span><span class="sxs-lookup"><span data-stu-id="c1461-130">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="c1461-131">在 Visual Studio 中開啟專案，然後選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="c1461-131">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="c1461-132">選取 [專案]\*\*\*\* 功能表的 [Docker 支援]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="c1461-132">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="c1461-133">以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [新增]\*\*\*\* > [Docker 支援]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="c1461-133">Right-click the project in **Solution Explorer** and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="c1461-134">Visual Studio 容器工具不支援將 Docker 新增至以 .NET Framework 為目標的現有 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="c1461-134">The Visual Studio Container Tools don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span>

## <a name="dockerfile-overview"></a><span data-ttu-id="c1461-135">Dockerfile 概觀</span><span class="sxs-lookup"><span data-stu-id="c1461-135">Dockerfile overview</span></span>

<span data-ttu-id="c1461-136">*Dockerfile*，是用於建立最終 Docker 映像的配方，會新增至專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="c1461-136">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="c1461-137">請參閱[Dockerfile 參考](https://docs.docker.com/engine/reference/builder/)以瞭解其內的命令。</span><span class="sxs-lookup"><span data-stu-id="c1461-137">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="c1461-138">此特定 *Dockerfile* 使用[多階段建置](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)，包含四個不同的具名建置階段：</span><span class="sxs-lookup"><span data-stu-id="c1461-138">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) with four distinct, named build stages:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

<span data-ttu-id="c1461-139">上述的 *Dockerfile* 以 [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) 映像為基礎。</span><span class="sxs-lookup"><span data-stu-id="c1461-139">The preceding *Dockerfile* is based on the [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) image.</span></span> <span data-ttu-id="c1461-140">此基底映像包含 ASP.NET Core 執行階段與 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="c1461-140">This base image includes the ASP.NET Core runtime and NuGet packages.</span></span> <span data-ttu-id="c1461-141">套件會進行 Just-in-Time (JIT) 編譯，以改善啟動效能。</span><span class="sxs-lookup"><span data-stu-id="c1461-141">The packages are just-in-time (JIT) compiled to improve startup performance.</span></span>

<span data-ttu-id="c1461-142">核取新專案對話方塊的 [設定 HTTPS]\*\*\*\* 核取方塊時，*Dockerfile* 會提供兩個連接埠。</span><span class="sxs-lookup"><span data-stu-id="c1461-142">When the new project dialog's **Configure for HTTPS** check box is checked, the *Dockerfile* exposes two ports.</span></span> <span data-ttu-id="c1461-143">其中一個連接埠用於 HTTP 流量，另一個連接埠則用於 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="c1461-143">One port is used for HTTP traffic; the other port is used for HTTPS.</span></span> <span data-ttu-id="c1461-144">如果未選取該核取方塊，則會為 HTTP 流量提供單一連接埠 (80)。</span><span class="sxs-lookup"><span data-stu-id="c1461-144">If the check box isn't checked, a single port (80) is exposed for HTTP traffic.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

<span data-ttu-id="c1461-145">上述的 *Dockerfile* 以 [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) 映像為基礎。</span><span class="sxs-lookup"><span data-stu-id="c1461-145">The preceding *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) image.</span></span> <span data-ttu-id="c1461-146">此基底映像包含 ASP.NET Core NuGet 套件，其進行 Just-in-Time (JIT) 編譯，以改善啟動效能。</span><span class="sxs-lookup"><span data-stu-id="c1461-146">This base image includes the ASP.NET Core NuGet packages, which are just-in-time (JIT) compiled to improve startup performance.</span></span>

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a><span data-ttu-id="c1461-147">為應用程式新增容器協調器支援</span><span class="sxs-lookup"><span data-stu-id="c1461-147">Add container orchestrator support to an app</span></span>

<span data-ttu-id="c1461-148">Visual Studio 2017 版本 15.7 或更早的版本支援將 [Docker Compose](https://docs.docker.com/compose/overview/) 作為唯一的容器協調流程解決方案。</span><span class="sxs-lookup"><span data-stu-id="c1461-148">Visual Studio 2017 versions 15.7 or earlier support [Docker Compose](https://docs.docker.com/compose/overview/) as the sole container orchestration solution.</span></span> <span data-ttu-id="c1461-149">Docker Compose 成品是**透過新增**  >  **Docker 支援**來新增。</span><span class="sxs-lookup"><span data-stu-id="c1461-149">The Docker Compose artifacts are added via **Add** > **Docker Support**.</span></span>

<span data-ttu-id="c1461-150">Visual Studio 2017 版本 15.8 或更新版本只有在指示進行時，才會新增協調流程解決方案。</span><span class="sxs-lookup"><span data-stu-id="c1461-150">Visual Studio 2017 versions 15.8 or later add an orchestration solution only when instructed.</span></span> <span data-ttu-id="c1461-151">以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [新增]\*\*\*\* > [容器協調器支援]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="c1461-151">Right-click the project in **Solution Explorer** and select **Add** > **Container Orchestrator Support**.</span></span> <span data-ttu-id="c1461-152">可用的選項如下：</span><span class="sxs-lookup"><span data-stu-id="c1461-152">The following choices are available:</span></span> 

* [<span data-ttu-id="c1461-153">Docker Compose</span><span class="sxs-lookup"><span data-stu-id="c1461-153">Docker Compose</span></span>](#docker-compose)
* [<span data-ttu-id="c1461-154">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c1461-154">Service Fabric</span></span>](#service-fabric)
* [<span data-ttu-id="c1461-155">Kubernetes/Helm</span><span class="sxs-lookup"><span data-stu-id="c1461-155">Kubernetes/Helm </span></span>](https://helm.sh/)

### <a name="docker-compose"></a><span data-ttu-id="c1461-156">Docker Compose</span><span class="sxs-lookup"><span data-stu-id="c1461-156">Docker Compose</span></span>

<span data-ttu-id="c1461-157">Visual Studio 容器工具會將 *docker-compose* 專案新增至包含下列檔案的解決方案：</span><span class="sxs-lookup"><span data-stu-id="c1461-157">The Visual Studio Container Tools add a *docker-compose* project to the solution with the following files:</span></span>

* <span data-ttu-id="c1461-158">*docker-compose.dcproj*：代表專案的檔案。</span><span class="sxs-lookup"><span data-stu-id="c1461-158">*docker-compose.dcproj*: The file representing the project.</span></span> <span data-ttu-id="c1461-159">包含 `<DockerTargetOS>` 項目，指定要使用的 OS。</span><span class="sxs-lookup"><span data-stu-id="c1461-159">Includes a `<DockerTargetOS>` element specifying the OS to be used.</span></span>
* <span data-ttu-id="c1461-160">*. .dockerignore*：列出產生組建內容時要排除的檔案和目錄模式。</span><span class="sxs-lookup"><span data-stu-id="c1461-160">*.dockerignore*: Lists the file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="c1461-161">*docker-compose.dev.debug.yml. yml*：基底[Docker Compose](https://docs.docker.com/compose/overview/)檔案，用來定義以和分別建立和執行的映射集合 `docker-compose build` `docker-compose run` 。</span><span class="sxs-lookup"><span data-stu-id="c1461-161">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="c1461-162">*docker-compose.dev.debug.yml. yml*：選擇性的檔案，由 Docker Compose 讀取，並具有服務的設定覆寫。</span><span class="sxs-lookup"><span data-stu-id="c1461-162">*docker-compose.override.yml*: An optional file, read by Docker Compose, with configuration overrides for services.</span></span> <span data-ttu-id="c1461-163">Visual Studio 會執行 `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` 來合併這些檔案。</span><span class="sxs-lookup"><span data-stu-id="c1461-163">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="c1461-164">*docker-compose.yml* 檔案會參考執行專案時所建立映像的名稱：</span><span class="sxs-lookup"><span data-stu-id="c1461-164">The *docker-compose.yml* file references the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

<span data-ttu-id="c1461-165">在上述範例中，`image: hellodockertools` 會在以**偵錯**模式執行應用程式時產生 `hellodockertools:dev` 映像。</span><span class="sxs-lookup"><span data-stu-id="c1461-165">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="c1461-166">以**發行**模式執行應用程式時，會產生 `hellodockertools:latest` 映像。</span><span class="sxs-lookup"><span data-stu-id="c1461-166">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="c1461-167">如果映像會推送至登錄，會在映像名稱前加上 [Docker Hub](https://hub.docker.com/) 使用者名稱 (例如，`dockerhubusername/hellodockertools`)。</span><span class="sxs-lookup"><span data-stu-id="c1461-167">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image is pushed to the registry.</span></span> <span data-ttu-id="c1461-168">或者，根據設定將映像名稱變更為包含私人登錄 URL (例如，`privateregistry.domain.com/hellodockertools`)。</span><span class="sxs-lookup"><span data-stu-id="c1461-168">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

<span data-ttu-id="c1461-169">如果您需要基於組建設定的不同行為 (例如偵錯或發行)，請新增特定於設定的 *docker-compose* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c1461-169">If you want different behavior based on the build configuration (for example, Debug or Release), add configuration-specific *docker-compose* files.</span></span> <span data-ttu-id="c1461-170">應根據組建設定來命名檔案 (例如 *docker-compose.vs.debug.yml* 和 *docker-compose.vs.release.yml*) 並放在與 *docker-compose-override.yml* 檔案相同的位置。</span><span class="sxs-lookup"><span data-stu-id="c1461-170">The files should be named according to the build configuration (for example, *docker-compose.vs.debug.yml* and *docker-compose.vs.release.yml*) and placed in the same location as the *docker-compose-override.yml* file.</span></span> 

<span data-ttu-id="c1461-171">使用特定於設定的覆寫檔案，可以為偵錯和發行組建設定指定不同的組態設定 (例如環境變數或進入點)。</span><span class="sxs-lookup"><span data-stu-id="c1461-171">Using the configuration-specific override files, you can specify different configuration settings (such as environment variables or entry points) for Debug and Release build configurations.</span></span>

<span data-ttu-id="c1461-172">若要讓 Docker Compose 顯示在 Visual Studio 中執行的選項，Docker 專案必須是啟始專案。</span><span class="sxs-lookup"><span data-stu-id="c1461-172">For Docker Compose to display an option to run in Visual Studio, the docker project must be the startup project.</span></span>

### <a name="service-fabric"></a><span data-ttu-id="c1461-173">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c1461-173">Service Fabric</span></span>

<span data-ttu-id="c1461-174">除了基礎[必要條件](#prerequisites)之外，[Service Fabric](/azure/service-fabric/) 協調流程解決方案還需要下列必要條件：</span><span class="sxs-lookup"><span data-stu-id="c1461-174">In addition to the base [Prerequisites](#prerequisites), the [Service Fabric](/azure/service-fabric/) orchestration solution demands the following prerequisites:</span></span>

* <span data-ttu-id="c1461-175">[Microsoft Azure Service Fabric SDK](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) 版本 2.6 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c1461-175">[Microsoft Azure Service Fabric SDK](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) version 2.6 or later</span></span>
* <span data-ttu-id="c1461-176">Visual Studio 的 **Azure 開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="c1461-176">Visual Studio's **Azure Development** workload</span></span>

<span data-ttu-id="c1461-177">Service Fabric 不支援在 Windows 上的本機開發叢集中執行 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="c1461-177">Service Fabric doesn't support running Linux containers in the local development cluster on Windows.</span></span> <span data-ttu-id="c1461-178">如果專案已在使用 Linux 容器，Visual Studio 會提示您切換至 Windows 容器。</span><span class="sxs-lookup"><span data-stu-id="c1461-178">If the project is already using a Linux container, Visual Studio prompts to switch to Windows containers.</span></span>

<span data-ttu-id="c1461-179">Visual Studio 容器工具會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="c1461-179">The Visual Studio Container Tools do the following tasks:</span></span>

* <span data-ttu-id="c1461-180">將\* &lt; project_name &gt; 應用程式\* **Service Fabric 應用**程式專案加入至方案。</span><span class="sxs-lookup"><span data-stu-id="c1461-180">Adds a *&lt;project_name&gt;Application* **Service Fabric Application** project to the solution.</span></span>
* <span data-ttu-id="c1461-181">將 *Dockerfile* 與 *.dockerignore* 檔案，新增至 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="c1461-181">Adds a *Dockerfile* and a *.dockerignore* file to the ASP.NET Core project.</span></span> <span data-ttu-id="c1461-182">如果 ASP.NET Core 專案中已存在 *Dockerfile*，則會重新命名為 *Dockerfile.original*。</span><span class="sxs-lookup"><span data-stu-id="c1461-182">If a *Dockerfile* already exists in the ASP.NET Core project, it's renamed to *Dockerfile.original*.</span></span> <span data-ttu-id="c1461-183">會建立類似如下的新 *Dockerfile*：</span><span class="sxs-lookup"><span data-stu-id="c1461-183">A new *Dockerfile*, similar to the following, is created:</span></span>

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* <span data-ttu-id="c1461-184">會將 `<IsServiceFabricServiceProject>` 項目新增至 ASP.NET Core 專案的 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="c1461-184">Adds an `<IsServiceFabricServiceProject>` element to the ASP.NET Core project's *.csproj* file:</span></span>

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* <span data-ttu-id="c1461-185">會將 *PackageRoot* 資料夾新增至 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="c1461-185">Adds a *PackageRoot* folder to the ASP.NET Core project.</span></span> <span data-ttu-id="c1461-186">該資料夾包含服務資訊清單與新服務的設定。</span><span class="sxs-lookup"><span data-stu-id="c1461-186">The folder includes the service manifest and settings for the new service.</span></span>

<span data-ttu-id="c1461-187">如需詳細資訊，請參閱[將 Windows 容器中的 .NET 應用程式部署至 Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container)。</span><span class="sxs-lookup"><span data-stu-id="c1461-187">For more information, see [Deploy a .NET app in a Windows container to Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span></span>

## <a name="debug"></a><span data-ttu-id="c1461-188">偵錯</span><span class="sxs-lookup"><span data-stu-id="c1461-188">Debug</span></span>

<span data-ttu-id="c1461-189">從工具列的偵錯下拉式清單中選取 [Docker]\*\*\*\*，然後開始對應用程式進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="c1461-189">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="c1461-190">[輸出]\*\*\*\* 視窗的 **Docker** 檢視會顯示下列將採取的動作：</span><span class="sxs-lookup"><span data-stu-id="c1461-190">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="c1461-191">取得 *microsoft/dotnet* 執行階段映像的 *2.1-aspnetcore-runtime* 標籤 (若尚未存在於快取中)。</span><span class="sxs-lookup"><span data-stu-id="c1461-191">The *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* runtime image is acquired (if not already in the cache).</span></span> <span data-ttu-id="c1461-192">映像會安裝 ASP.NET Core 與 .NET Core 執行階段，以及相關聯的程式庫。</span><span class="sxs-lookup"><span data-stu-id="c1461-192">The image installs the ASP.NET Core and .NET Core runtimes and associated libraries.</span></span> <span data-ttu-id="c1461-193">在生產環境中最好是執行 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1461-193">It's optimized for running ASP.NET Core apps in production.</span></span>
* <span data-ttu-id="c1461-194">在容器內，`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="c1461-194">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="c1461-195">會提供兩個動態指派的連接埠：一個用於 HTTP，另一個用於 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="c1461-195">Two dynamically assigned ports are exposed: one for HTTP and one for HTTPS.</span></span> <span data-ttu-id="c1461-196">可使用 `docker ps` 命令，查詢指派到 localhost 的連接埠。</span><span class="sxs-lookup"><span data-stu-id="c1461-196">The port assigned to localhost can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="c1461-197">應用程式會複製至容器。</span><span class="sxs-lookup"><span data-stu-id="c1461-197">The app is copied to the container.</span></span>
* <span data-ttu-id="c1461-198">預設瀏覽器會在偵錯工具使用動態指派的連接埠附加至容器的情況下啟動。</span><span class="sxs-lookup"><span data-stu-id="c1461-198">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="c1461-199">產生的應用程式 Docker 映像，會標記為 *dev*。</span><span class="sxs-lookup"><span data-stu-id="c1461-199">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="c1461-200">此映像以 *microsoft/dotnet* 基底映像的 *2.1-aspnetcore-runtime* 標籤為基礎。</span><span class="sxs-lookup"><span data-stu-id="c1461-200">The image is based on the *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* base image.</span></span> <span data-ttu-id="c1461-201">在 [套件管理員主控台]\*\*\*\* (PMC) 視窗中，執行 `docker images` 命令。</span><span class="sxs-lookup"><span data-stu-id="c1461-201">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="c1461-202">這會顯示電腦上的映像：</span><span class="sxs-lookup"><span data-stu-id="c1461-202">The images on the machine are displayed:</span></span>

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <span data-ttu-id="c1461-203">取得 *microsoft/aspnetcore* 執行階段映像 (若尚未存在於快取中)。</span><span class="sxs-lookup"><span data-stu-id="c1461-203">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="c1461-204">在容器內，`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="c1461-204">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="c1461-205">連接埠 80 會公開並對應至本機主機的動態指派連接埠。</span><span class="sxs-lookup"><span data-stu-id="c1461-205">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="c1461-206">連接埠是由 Docker 主機所決定，並且可以使用 `docker ps` 命令進行查詢。</span><span class="sxs-lookup"><span data-stu-id="c1461-206">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="c1461-207">應用程式會複製至容器。</span><span class="sxs-lookup"><span data-stu-id="c1461-207">The app is copied to the container.</span></span>
* <span data-ttu-id="c1461-208">預設瀏覽器會在偵錯工具使用動態指派的連接埠附加至容器的情況下啟動。</span><span class="sxs-lookup"><span data-stu-id="c1461-208">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="c1461-209">產生的應用程式 Docker 映像，會標記為 *dev*。</span><span class="sxs-lookup"><span data-stu-id="c1461-209">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="c1461-210">映像以 *microsoft/aspnetcore* 基底映像為基礎。</span><span class="sxs-lookup"><span data-stu-id="c1461-210">The image is based on the *microsoft/aspnetcore* base image.</span></span> <span data-ttu-id="c1461-211">在 [套件管理員主控台]\*\*\*\* (PMC) 視窗中，執行 `docker images` 命令。</span><span class="sxs-lookup"><span data-stu-id="c1461-211">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="c1461-212">這會顯示電腦上的映像：</span><span class="sxs-lookup"><span data-stu-id="c1461-212">The images on the machine are displayed:</span></span>

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="c1461-213">因為**偵錯**組態會使用磁碟區掛接來提供重複的體驗，所以 *dev* 映像不會有應用程式內容。</span><span class="sxs-lookup"><span data-stu-id="c1461-213">The *dev* image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="c1461-214">若要推送映射，請使用**發行**設定。</span><span class="sxs-lookup"><span data-stu-id="c1461-214">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="c1461-215">在 PMC 中執行 `docker ps` 命令。</span><span class="sxs-lookup"><span data-stu-id="c1461-215">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="c1461-216">請注意是使用容器來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="c1461-216">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="c1461-217">編輯後繼續</span><span class="sxs-lookup"><span data-stu-id="c1461-217">Edit and continue</span></span>

<span data-ttu-id="c1461-218">靜態檔案和視圖的變更會 Razor 自動更新，而不需要編譯步驟。</span><span class="sxs-lookup"><span data-stu-id="c1461-218">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="c1461-219">進行變更並儲存，然後重新整理瀏覽器來檢視更新。</span><span class="sxs-lookup"><span data-stu-id="c1461-219">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="c1461-220">程式碼檔案的修改需要編譯以及重新啟動容器內的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="c1461-220">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="c1461-221">完成變更之後，請使用 `CTRL+F5` 來執行程序，並啟動容器內的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1461-221">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="c1461-222">Docker 容器不會進行重建或停止。</span><span class="sxs-lookup"><span data-stu-id="c1461-222">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="c1461-223">在 PMC 中執行 `docker ps` 命令。</span><span class="sxs-lookup"><span data-stu-id="c1461-223">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="c1461-224">請注意，原始容器在 10 分鐘前仍在執行：</span><span class="sxs-lookup"><span data-stu-id="c1461-224">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="c1461-225">發行 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="c1461-225">Publish Docker images</span></span>

<span data-ttu-id="c1461-226">當應用程式的開發和偵錯循環完畢之後，Visual Studio 容器工具就會協助建立應用程式的實際執行映像。</span><span class="sxs-lookup"><span data-stu-id="c1461-226">Once the develop and debug cycle of the app is completed, the Visual Studio Container Tools assist in creating the production image of the app.</span></span> <span data-ttu-id="c1461-227">將組態下拉式清單變更為 [發行]\*\*\*\* 並建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1461-227">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="c1461-228">工具會從 Docker Hub (若尚未在快取中) 取得編譯/發行映像。</span><span class="sxs-lookup"><span data-stu-id="c1461-228">The tooling acquires the compile/publish image from Docker Hub (if not already in the cache).</span></span> <span data-ttu-id="c1461-229">映像會使用*最新*的標籤產生，其可推送至私人登錄或 Docker Hub。</span><span class="sxs-lookup"><span data-stu-id="c1461-229">An image is produced with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="c1461-230">在 PMC 中執行 `docker images` 命令，可查看映像清單。</span><span class="sxs-lookup"><span data-stu-id="c1461-230">Run the `docker images` command in PMC to see the list of images.</span></span> <span data-ttu-id="c1461-231">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="c1461-231">Output similar to the following is displayed:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
REPOSITORY        TAG                     IMAGE ID      CREATED             SIZE
hellodockertools  latest                  e3984a64230c  About a minute ago  258MB
hellodockertools  dev                     d72ce0f1dfe7  4 minutes ago       255MB
microsoft/dotnet  2.1-sdk                 9e243db15f91  6 days ago          1.7GB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago          255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```console
REPOSITORY                  TAG     IMAGE ID      CREATED         SIZE
hellodockertools            latest  cd28f0d4abbd  12 seconds ago  349MB
hellodockertools            dev     5fafe5d1ad5b  23 minutes ago  347MB
microsoft/aspnetcore-build  2.0     7fed40fbb647  13 days ago     2.02GB
microsoft/aspnetcore        2.0     c69d39472da9  13 days ago     347MB
```

<span data-ttu-id="c1461-232">自 .NET Core 2.1 起，上述輸出中所列出的 `microsoft/aspnetcore-build` 和 `microsoft/aspnetcore` 映像，會取代為 `microsoft/dotnet` 映像。</span><span class="sxs-lookup"><span data-stu-id="c1461-232">The `microsoft/aspnetcore-build` and `microsoft/aspnetcore` images listed in the preceding output are replaced with `microsoft/dotnet` images as of .NET Core 2.1.</span></span> <span data-ttu-id="c1461-233">如需詳細資訊，請參閱 [Docker 存放庫移轉公告](https://github.com/aspnet/Announcements/issues/298)。</span><span class="sxs-lookup"><span data-stu-id="c1461-233">For more information, see [the Docker repositories migration announcement](https://github.com/aspnet/Announcements/issues/298).</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="c1461-234">`docker images`命令會傳回存放庫名稱和標籤識別為的中繼映射 *\<none>* （未列于上方）。</span><span class="sxs-lookup"><span data-stu-id="c1461-234">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="c1461-235">這些未命名映像是由[多階段建置](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile* 所產生。</span><span class="sxs-lookup"><span data-stu-id="c1461-235">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="c1461-236">它們可以改善最終映像的建置效率 &mdash; 發生變更時只會重建必要層。</span><span class="sxs-lookup"><span data-stu-id="c1461-236">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="c1461-237">當不再需要中繼映像時，請使用 [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) \(英文\) 命令予以刪除。</span><span class="sxs-lookup"><span data-stu-id="c1461-237">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="c1461-238">相較於 *dev* 映像，生產或發行映像的大小可能需要更小。</span><span class="sxs-lookup"><span data-stu-id="c1461-238">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="c1461-239">基於磁碟區對應，偵錯工具和應用程式是從本機電腦執行，而不是在容器內執行。</span><span class="sxs-lookup"><span data-stu-id="c1461-239">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="c1461-240">「最新」\*\* 映像已封裝在主機上執行應用程式所需的應用程式碼。</span><span class="sxs-lookup"><span data-stu-id="c1461-240">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="c1461-241">因此，差異是應用程式碼的大小。</span><span class="sxs-lookup"><span data-stu-id="c1461-241">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1461-242">其他資源</span><span class="sxs-lookup"><span data-stu-id="c1461-242">Additional resources</span></span>

* [<span data-ttu-id="c1461-243">使用 Visual Studio 進行容器開發</span><span class="sxs-lookup"><span data-stu-id="c1461-243">Container development with Visual Studio</span></span>](/visualstudio/containers)
* [<span data-ttu-id="c1461-244">Azure Service Fabric：準備您的開發環境</span><span class="sxs-lookup"><span data-stu-id="c1461-244">Azure Service Fabric: Prepare your development environment</span></span>](/azure/service-fabric/service-fabric-get-started)
* [<span data-ttu-id="c1461-245">將 Windows 容器中的 .NET 應用程式部署至 Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c1461-245">Deploy a .NET app in a Windows container to Azure Service Fabric</span></span>](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [<span data-ttu-id="c1461-246">對使用 Docker 進行的 Visual Studio 開發進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="c1461-246">Troubleshoot Visual Studio development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [<span data-ttu-id="c1461-247">GitHub 存放庫上的 Visual Studio 容器工具</span><span class="sxs-lookup"><span data-stu-id="c1461-247">Visual Studio Container Tools GitHub repository</span></span>](https://github.com/Microsoft/DockerTools)
* [<span data-ttu-id="c1461-248">使用 Docker 和小型容器的 GC</span><span class="sxs-lookup"><span data-stu-id="c1461-248">GC using Docker and small containers</span></span>](xref:performance/memory#sc)
