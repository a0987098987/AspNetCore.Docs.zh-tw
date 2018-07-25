---
title: 搭配 ASP.NET Core 使用 Visual Studio Tools for Docker
author: spboyer
description: 了解如何使用適用於 Windows 的 Visual Studio 2017 工具和 Docker 對 ASP.NET Core 應用程式進行容器化。
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146880"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="67d1b-103">搭配 ASP.NET Core 使用 Visual Studio Tools for Docker</span><span class="sxs-lookup"><span data-stu-id="67d1b-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="67d1b-104">[Visual Studio 2017](https://www.visualstudio.com/) 支援建置、偵錯和執行以 .NET Core 為目標的容器化 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="67d1b-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="67d1b-105">同時支援 Windows 和 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="67d1b-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67d1b-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="67d1b-106">Prerequisites</span></span>

* <span data-ttu-id="67d1b-107">已安裝「.NET Core 跨平台開發」工作負載的 [Visual Studio 2017](https://www.visualstudio.com/)</span><span class="sxs-lookup"><span data-stu-id="67d1b-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="67d1b-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="67d1b-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="67d1b-109">安裝和設定</span><span class="sxs-lookup"><span data-stu-id="67d1b-109">Installation and setup</span></span>

<span data-ttu-id="67d1b-110">若要安裝 Docker，請檢閱 [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker for Windows：安裝須知) 的資訊，並安裝 [Docker For Windows](https://docs.docker.com/docker-for-windows/install/)。</span><span class="sxs-lookup"><span data-stu-id="67d1b-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="67d1b-111">Docker for Windows 中的 **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** (共用磁碟機) 必須設定為支援磁碟區對應和偵錯。</span><span class="sxs-lookup"><span data-stu-id="67d1b-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="67d1b-112">以滑鼠右鍵按一下系統匣的 Docker 圖示，並選取 [設定]，然後選取 [共用磁碟機]。</span><span class="sxs-lookup"><span data-stu-id="67d1b-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="67d1b-113">選取 Docker 儲存檔案的磁碟機。</span><span class="sxs-lookup"><span data-stu-id="67d1b-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="67d1b-114">選取 [套用]。</span><span class="sxs-lookup"><span data-stu-id="67d1b-114">Select **Apply**.</span></span>

![共用磁碟機](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="67d1b-116">未設定 [共用磁碟機] 時，Visual Studio 2017 15.6 版和更新版本會顯示提示。</span><span class="sxs-lookup"><span data-stu-id="67d1b-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="67d1b-117">將 Docker 支援新增至應用程式</span><span class="sxs-lookup"><span data-stu-id="67d1b-117">Add Docker support to an app</span></span>

<span data-ttu-id="67d1b-118">若要將 Docker 支援加入 ASP.NET Core 專案，該專案必須以 .NET Core 為目標。</span><span class="sxs-lookup"><span data-stu-id="67d1b-118">To add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="67d1b-119">同時支援 Linux 和 Windows 容器。</span><span class="sxs-lookup"><span data-stu-id="67d1b-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="67d1b-120">將 Docker 支援新增至專案時，請選擇 Windows 或 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="67d1b-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="67d1b-121">Docker 主機必須執行相同的容器類型。</span><span class="sxs-lookup"><span data-stu-id="67d1b-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="67d1b-122">若要變更執行中 Docker 執行個體中的容器類型，請以滑鼠右鍵按一下系統匣的 Docker 圖示，然後選擇 [Switch to Windows containers...] (切換至 Windows 容器...) 或 [Switch to Linux containers] (切換至 Linux 容器...)。</span><span class="sxs-lookup"><span data-stu-id="67d1b-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="67d1b-123">新增應用程式</span><span class="sxs-lookup"><span data-stu-id="67d1b-123">New app</span></span>

<span data-ttu-id="67d1b-124">使用 **ASP.NET Core Web 應用程式**專案範本來建立新的應用程式時，請選取 [啟用 Docker 支援] 核取方塊：</span><span class="sxs-lookup"><span data-stu-id="67d1b-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![啟用 Docker 支援核取方塊](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

<span data-ttu-id="67d1b-126">如果目標架構是 .NET Core，則 [OS] 下拉式清單會允許選取容器類型。</span><span class="sxs-lookup"><span data-stu-id="67d1b-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="67d1b-127">現有應用程式</span><span class="sxs-lookup"><span data-stu-id="67d1b-127">Existing app</span></span>

<span data-ttu-id="67d1b-128">Visual Studio Tools for Docker 不支援將 Docker 新增至以 .NET Framework 為目標的現有 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="67d1b-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="67d1b-129">針對以 .NET Core 為目標的 ASP.NET Core 專案，有兩個選項可透過工具來新增 Docker 支援。</span><span class="sxs-lookup"><span data-stu-id="67d1b-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="67d1b-130">在 Visual Studio 中開啟專案，然後選擇下列其中一個選項：</span><span class="sxs-lookup"><span data-stu-id="67d1b-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="67d1b-131">選取 [專案] 功能表的 [Docker 支援]。</span><span class="sxs-lookup"><span data-stu-id="67d1b-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="67d1b-132">以滑鼠右鍵按一下方案總管中的專案，然後選取 [新增] > [Docker 支援]。</span><span class="sxs-lookup"><span data-stu-id="67d1b-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="67d1b-133">Docker 資產概觀</span><span class="sxs-lookup"><span data-stu-id="67d1b-133">Docker assets overview</span></span>

<span data-ttu-id="67d1b-134">Visual Studio Tools for Docker 會將 *docker-compose* 專案新增至包含下列檔案的方案：</span><span class="sxs-lookup"><span data-stu-id="67d1b-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution containing the following files:</span></span>

* <span data-ttu-id="67d1b-135">*.dockerignore*：包含要在產生組建內容時排除的檔案和目錄模式清單。</span><span class="sxs-lookup"><span data-stu-id="67d1b-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="67d1b-136">*docker-compose.yml*︰基礎 [Docker Compose](https://docs.docker.com/compose/overview/) 檔案，用於定義要分別使用 `docker-compose build` 和 `docker-compose run` 建置並執行的映像集合。</span><span class="sxs-lookup"><span data-stu-id="67d1b-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="67d1b-137">*docker-compose.override.yml*：透過 Docker Compose 讀取的選擇性檔案，內含服務的組態覆寫。</span><span class="sxs-lookup"><span data-stu-id="67d1b-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="67d1b-138">Visual Studio 會執行 `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` 來合併這些檔案。</span><span class="sxs-lookup"><span data-stu-id="67d1b-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="67d1b-139">*Dockerfile*，是用於建立最終 Docker 映像的配方，會新增至專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="67d1b-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="67d1b-140">請參閱 [Dockerfile 參考](https://docs.docker.com/engine/reference/builder/)，以了解其內的命令。</span><span class="sxs-lookup"><span data-stu-id="67d1b-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="67d1b-141">這個特定 *Dockerfile* 使用[多階段建置](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)，內含四個不同的具名建置階段：</span><span class="sxs-lookup"><span data-stu-id="67d1b-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="67d1b-142">*Dockerfile* 是根據 [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) 映像。</span><span class="sxs-lookup"><span data-stu-id="67d1b-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="67d1b-143">此基底映像包含 ASP.NET Core NuGet 套件，已經過預先 JIT 編譯改善啟動效能。</span><span class="sxs-lookup"><span data-stu-id="67d1b-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="67d1b-144">*docker-compose.yml* 檔案包含執行專案時所建立映像的名稱：</span><span class="sxs-lookup"><span data-stu-id="67d1b-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="67d1b-145">在上述範例中，`image: hellodockertools` 會在以**偵錯**模式執行應用程式時產生 `hellodockertools:dev` 映像。</span><span class="sxs-lookup"><span data-stu-id="67d1b-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="67d1b-146">以**發行**模式執行應用程式時，會產生 `hellodockertools:latest` 映像。</span><span class="sxs-lookup"><span data-stu-id="67d1b-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="67d1b-147">如果映像會被推送至登錄，請在映像名稱前加上 [Docker Hub](https://hub.docker.com/) \(英文\) 使用者名稱 (例如，`dockerhubusername/hellodockertools`)。</span><span class="sxs-lookup"><span data-stu-id="67d1b-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="67d1b-148">或者，根據設定將映像名稱變更為包含私人登錄 URL (例如，`privateregistry.domain.com/hellodockertools`)。</span><span class="sxs-lookup"><span data-stu-id="67d1b-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="67d1b-149">偵錯</span><span class="sxs-lookup"><span data-stu-id="67d1b-149">Debug</span></span>

<span data-ttu-id="67d1b-150">從工具列的偵錯下拉式清單中選取 [Docker]，然後開始對應用程式進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="67d1b-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="67d1b-151">[輸出] 視窗的 **Docker** 檢視會顯示下列將採取的動作：</span><span class="sxs-lookup"><span data-stu-id="67d1b-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="67d1b-152">取得 *microsoft/aspnetcore* 執行階段映像 (若尚未存在於快取中)。</span><span class="sxs-lookup"><span data-stu-id="67d1b-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="67d1b-153">取得 *microsoft/aspnetcore-build* 編譯/發行映像 (若尚未存在於快取中)。</span><span class="sxs-lookup"><span data-stu-id="67d1b-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="67d1b-154">在容器內，*ASPNETCORE_ENVIRONMENT* 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="67d1b-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="67d1b-155">連接埠 80 會公開並對應至本機主機的動態指派連接埠。</span><span class="sxs-lookup"><span data-stu-id="67d1b-155">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="67d1b-156">連接埠是由 Docker 主機所決定，並且可以使用 `docker ps` 命令進行查詢。</span><span class="sxs-lookup"><span data-stu-id="67d1b-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="67d1b-157">應用程式會複製至容器。</span><span class="sxs-lookup"><span data-stu-id="67d1b-157">The app is copied to the container.</span></span>
* <span data-ttu-id="67d1b-158">預設瀏覽器會在偵錯工具使用動態指派的連接埠附加至容器的情況下啟動。</span><span class="sxs-lookup"><span data-stu-id="67d1b-158">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="67d1b-159">產生的 Docker 映像是應用程式的 *dev* 映像，並以 *microsoft/aspnetcore* 映像作為基底映像。</span><span class="sxs-lookup"><span data-stu-id="67d1b-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="67d1b-160">在 [套件管理員主控台] (PMC) 視窗中，執行 `docker images` 命令。</span><span class="sxs-lookup"><span data-stu-id="67d1b-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="67d1b-161">這會顯示電腦上的映像：</span><span class="sxs-lookup"><span data-stu-id="67d1b-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="67d1b-162">因為**偵錯**設定會使用磁碟區掛接來提供反覆體驗，所以 dev 映像不會有應用程式內容。</span><span class="sxs-lookup"><span data-stu-id="67d1b-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="67d1b-163">若要推送映像，請使用 [發行] 組態。</span><span class="sxs-lookup"><span data-stu-id="67d1b-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="67d1b-164">在 PMC 中執行 `docker ps` 命令。</span><span class="sxs-lookup"><span data-stu-id="67d1b-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="67d1b-165">請注意是使用容器來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="67d1b-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="67d1b-166">編輯後繼續</span><span class="sxs-lookup"><span data-stu-id="67d1b-166">Edit and continue</span></span>

<span data-ttu-id="67d1b-167">針對靜態檔案和 Razor 檢視所做的變更會自動更新，而不需要編譯步驟。</span><span class="sxs-lookup"><span data-stu-id="67d1b-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="67d1b-168">進行變更並儲存，然後重新整理瀏覽器來檢視更新。</span><span class="sxs-lookup"><span data-stu-id="67d1b-168">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="67d1b-169">程式碼檔案的修改需要編譯以及重新啟動容器內的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="67d1b-169">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="67d1b-170">完成變更之後，請使用 `CTRL+F5` 來執行程序，並啟動容器內的應用程式。</span><span class="sxs-lookup"><span data-stu-id="67d1b-170">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="67d1b-171">Docker 容器不會進行重建或停止。</span><span class="sxs-lookup"><span data-stu-id="67d1b-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="67d1b-172">在 PMC 中執行 `docker ps` 命令。</span><span class="sxs-lookup"><span data-stu-id="67d1b-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="67d1b-173">請注意，原始容器在 10 分鐘前仍在執行：</span><span class="sxs-lookup"><span data-stu-id="67d1b-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="67d1b-174">發行 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="67d1b-174">Publish Docker images</span></span>

<span data-ttu-id="67d1b-175">當應用程式的開發和偵錯循環完畢之後，Visual Studio Tools for Docker 就會協助建立應用程式的實際執行映像。</span><span class="sxs-lookup"><span data-stu-id="67d1b-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="67d1b-176">將組態下拉式清單變更為 [發行] 並建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="67d1b-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="67d1b-177">這項工具會產生附有「最新」標記的映像，此映像可被推送至私人登錄或 Docker Hub。</span><span class="sxs-lookup"><span data-stu-id="67d1b-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="67d1b-178">在 PMC 中執行 `docker images` 命令，以查看映像清單：</span><span class="sxs-lookup"><span data-stu-id="67d1b-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="67d1b-179">`docker images` 命令會傳回存放庫名稱和標記識別為 \<無> (上面未列出) 的中繼映像。</span><span class="sxs-lookup"><span data-stu-id="67d1b-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="67d1b-180">這些未命名映像是由[多階段建置](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile* 所產生。</span><span class="sxs-lookup"><span data-stu-id="67d1b-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="67d1b-181">它們可以改善最終映像的建置效率 &mdash; 發生變更時只會重建必要層。</span><span class="sxs-lookup"><span data-stu-id="67d1b-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="67d1b-182">當不再需要中繼映像時，請使用 [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) \(英文\) 命令予以刪除。</span><span class="sxs-lookup"><span data-stu-id="67d1b-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="67d1b-183">相較於 *dev* 映像，生產或發行映像的大小可能需要更小。</span><span class="sxs-lookup"><span data-stu-id="67d1b-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="67d1b-184">基於磁碟區對應，偵錯工具和應用程式是從本機電腦執行，而不是在容器內執行。</span><span class="sxs-lookup"><span data-stu-id="67d1b-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="67d1b-185">「最新」映像已封裝在主機上執行應用程式所需的應用程式碼。</span><span class="sxs-lookup"><span data-stu-id="67d1b-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="67d1b-186">因此，差異是應用程式碼的大小。</span><span class="sxs-lookup"><span data-stu-id="67d1b-186">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="67d1b-187">其他資源</span><span class="sxs-lookup"><span data-stu-id="67d1b-187">Additional resources</span></span>

* [<span data-ttu-id="67d1b-188">使用 Docker 針對 Visual Studio 2017 開發進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="67d1b-188">Troubleshoot Visual Studio 2017 development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [<span data-ttu-id="67d1b-189">Visual Studio Tools for Docker GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="67d1b-189">Visual Studio Tools for Docker GitHub repository</span></span>](https://github.com/Microsoft/DockerTools)
