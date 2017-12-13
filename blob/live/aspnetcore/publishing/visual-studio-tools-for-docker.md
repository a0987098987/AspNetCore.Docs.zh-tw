---
title: "搭配 ASP.NET Core 使用 Visual Studio Tools for Docker"
description: "本文會逐步解說如何使用適用於 Windows 的 Visual Studio 2017 工具和 Docker 對 ASP.NET Core 應用程式進行容器化。"
keywords: "Docker,ASP.NET Core,Visual Studio,container,容器"
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="dd90b-104">Visual Studio Tools for Docker</span><span class="sxs-lookup"><span data-stu-id="dd90b-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="dd90b-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) (含 [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)) 支援使用 Windows 和 Linux 容器來建置、偵錯與執行 .NET Framework 及 .NET Core Web 和主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd90b-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd90b-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="dd90b-106">Prerequisites</span></span>

- <span data-ttu-id="dd90b-107">含 .NET Core 工作負載的 [Microsoft Visual Studio 2017](https://www.visualstudio.com/)</span><span class="sxs-lookup"><span data-stu-id="dd90b-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="dd90b-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="dd90b-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="dd90b-109">安裝和設定</span><span class="sxs-lookup"><span data-stu-id="dd90b-109">Installation and setup</span></span>

<span data-ttu-id="dd90b-110">安裝含 .NET Core 工作負載的 [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="dd90b-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span> <span data-ttu-id="dd90b-111">如果您已經安裝 Visual Studio，則可以[修改您的 Visual Studio 安裝](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)以新增 .NET Core 工作負載。</span><span class="sxs-lookup"><span data-stu-id="dd90b-111">If you already have Visual Studio installed, you can [modify your Visual Studio installation](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) to add the .NET Core workload.</span></span>

<span data-ttu-id="dd90b-112">若要安裝 Docker，請檢閱 [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker for Windows：安裝須知) 的資訊，並安裝 [Docker For Windows](https://docs.docker.com/docker-for-windows/install/)。</span><span class="sxs-lookup"><span data-stu-id="dd90b-112">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="dd90b-113">必要的設定是在 Docker for Windows 中安裝**[共用磁碟機](https://docs.docker.com/docker-for-windows/#shared-drives)**。</span><span class="sxs-lookup"><span data-stu-id="dd90b-113">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="dd90b-114">這對磁碟區對應及偵錯支援是必要的設定。</span><span class="sxs-lookup"><span data-stu-id="dd90b-114">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="dd90b-115">以滑鼠右鍵按一下系統匣中的 Docker 圖示，按一下 [設定]，並選取 [共用磁碟機]。</span><span class="sxs-lookup"><span data-stu-id="dd90b-115">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="dd90b-116">選取 Docker 儲存檔案所在的磁碟機，並套用變更。</span><span class="sxs-lookup"><span data-stu-id="dd90b-116">Select the drive where Docker will store your files and apply changes.</span></span>

![共用磁碟機](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="dd90b-118">建立 ASP.NET Web 應用程式並新增 Docker 支援</span><span class="sxs-lookup"><span data-stu-id="dd90b-118">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="dd90b-119">使用 Visual Studio 建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd90b-119">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="dd90b-120">載入應用程式之後，請從 [專案] 功能表選取 [新增 Docker 支援]，或在方案總管中的專案上按一下滑鼠右鍵，並選取 [新增] > [Docker 支援]。</span><span class="sxs-lookup"><span data-stu-id="dd90b-120">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="dd90b-121">[專案] 功能表</span><span class="sxs-lookup"><span data-stu-id="dd90b-121">*Project Menu*</span></span>

![[專案] 「Add Docker Support」 (新增 Docker 支援)](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="dd90b-123">[專案] 內容功能表</span><span class="sxs-lookup"><span data-stu-id="dd90b-123">*Project Context Menu*</span></span>

![以滑鼠右鍵按一下 [新增 Docker 支援]](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="dd90b-125">將 Docker 支援加入您的專案時，可以選擇 Windows 或 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="dd90b-125">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="dd90b-126">(Docker 主機必須執行相同的容器類型。</span><span class="sxs-lookup"><span data-stu-id="dd90b-126">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="dd90b-127">如果您需要變更執行中 Docker 執行個體內的容器類型，請以滑鼠右鍵按一下系統匣中的 Docker 圖示，並選擇 [切換到 Windows 容器] 或 [切換到 Linux 容器])。</span><span class="sxs-lookup"><span data-stu-id="dd90b-127">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="dd90b-128">下列檔案會新增到專案中：</span><span class="sxs-lookup"><span data-stu-id="dd90b-128">The following files are added to the project:</span></span>

- <span data-ttu-id="dd90b-129">**Dockerfile**：ASP.NET Core 應用程式的 Docker 檔案是以 [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) 映像為基礎。</span><span class="sxs-lookup"><span data-stu-id="dd90b-129">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="dd90b-130">此映像包含 ASP.NET Core NuGet 封裝，已經過預先 JIT 編譯改善啟動效能。</span><span class="sxs-lookup"><span data-stu-id="dd90b-130">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="dd90b-131">建置 .NET Core 主控台應用程式時，Dockerfile FROM 會參考最新的 [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) 映像。</span><span class="sxs-lookup"><span data-stu-id="dd90b-131">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="dd90b-132">**docker-compose.yml**︰基礎 Docker Compose 檔案，用於定義要使用 docker-compose build/run 建置及執行的映像集合。</span><span class="sxs-lookup"><span data-stu-id="dd90b-132">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="dd90b-133">**docker-compose.dev.debug.yml**︰當組態設定為偵錯時，其他具有反覆變更的 docker-compose 檔案。</span><span class="sxs-lookup"><span data-stu-id="dd90b-133">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="dd90b-134">Visual Studio 會呼叫 -f docker-compose.yml -f docker-compose.dev.debug.yml 將這些合併到一起。</span><span class="sxs-lookup"><span data-stu-id="dd90b-134">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="dd90b-135">這是 Visual Studio 開發工具使用的 compose 檔案。</span><span class="sxs-lookup"><span data-stu-id="dd90b-135">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="dd90b-136">**docker-compose.dev.release.yml**：偵錯版本定義的其他 Docker Compose 檔案。</span><span class="sxs-lookup"><span data-stu-id="dd90b-136">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="dd90b-137">它會以磁碟區掛接方式掛接偵錯工具，這樣就不會變更生產環境映像的內容。</span><span class="sxs-lookup"><span data-stu-id="dd90b-137">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="dd90b-138">*docker-compose.yml* 檔案包含執行專案時所建立之映像的名稱。</span><span class="sxs-lookup"><span data-stu-id="dd90b-138">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

<span data-ttu-id="dd90b-139">在本例中，當應用程式在 **偵錯**模式中執行，而 `user/hellodockertools:latest` 在**發行**模式中執行時，`image: user/hellodockertools${TAG}` 會產生映像 `user/hellodockertools:dev`。</span><span class="sxs-lookup"><span data-stu-id="dd90b-139">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="dd90b-140">如果您打算將映像推送至登錄，請將 `user` 變更為您的 [Docker Hub](https://hub.docker.com/) \(英文\) 使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="dd90b-140">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="dd90b-141">例如 `spboyer/hellodockertools`，或是變更為您的私用登錄 URL `privateregistry.domain.com/`，視您的設定而定。</span><span class="sxs-lookup"><span data-stu-id="dd90b-141">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="dd90b-142">偵錯</span><span class="sxs-lookup"><span data-stu-id="dd90b-142">Debugging</span></span>

<span data-ttu-id="dd90b-143">從工具列的 [偵錯] 下拉式清單中選取 [Docker]，然後使用 F5 來開始對應用程式進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="dd90b-143">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="dd90b-144">您將會取得 *microsoft/aspnetcore* 映像 (若尚未存在於快取中)</span><span class="sxs-lookup"><span data-stu-id="dd90b-144">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="dd90b-145">*ASPNETCORE_ENVIRONMENT* 在容器內已設定為 [開發]</span><span class="sxs-lookup"><span data-stu-id="dd90b-145">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="dd90b-146">PORT 80 為 EXPOSED，且對應至本機主機的動態指派連接埠。</span><span class="sxs-lookup"><span data-stu-id="dd90b-146">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="dd90b-147">連接埠是由銜接站主機決定，可以使用 docker ps 進行查詢。</span><span class="sxs-lookup"><span data-stu-id="dd90b-147">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="dd90b-148">您的應用程式會複製到容器</span><span class="sxs-lookup"><span data-stu-id="dd90b-148">Your application is copied to the container</span></span>
- <span data-ttu-id="dd90b-149">預設瀏覽器會和附加至容器的偵錯工具一起啟動，使用動態指派的連接埠。</span><span class="sxs-lookup"><span data-stu-id="dd90b-149">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="dd90b-150">產生的 Docker 映像組建是應用程式的 *dev* 映像，並以 *microsoft/aspnetcore* 映像作為基礎映像。</span><span class="sxs-lookup"><span data-stu-id="dd90b-150">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="dd90b-151">**注意︰**由於 [偵錯] 設定會使用磁碟區掛接來提供反覆體驗，因此 dev 映像將不會有任何應用程式內容。</span><span class="sxs-lookup"><span data-stu-id="dd90b-151">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="dd90b-152">若要推送映像，請使用 [發行] 設定。</span><span class="sxs-lookup"><span data-stu-id="dd90b-152">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="dd90b-153">應用程式正在使用容器執行，您可以透過執行 `docker ps` 命令來查看該容器。</span><span class="sxs-lookup"><span data-stu-id="dd90b-153">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="dd90b-154">編輯後繼續</span><span class="sxs-lookup"><span data-stu-id="dd90b-154">Edit and Continue</span></span>

<span data-ttu-id="dd90b-155">針對靜態檔案和/或 Razor 範本檔案 (*.cshtml*) 所做的變更會自動更新，而不需要編譯步驟。</span><span class="sxs-lookup"><span data-stu-id="dd90b-155">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="dd90b-156">進行變更，儲存並點選瀏覽器中的 [重新整理] 來檢視更新。</span><span class="sxs-lookup"><span data-stu-id="dd90b-156">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="dd90b-157">程式碼檔案的修改需要編譯以及重新啟動容器內的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="dd90b-157">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="dd90b-158">完成變更之後，使用 CTRL + F5 執行程序，並啟動容器內的應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd90b-158">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="dd90b-159">Docker 容器不會重建或停止，透過在命令列中使用 `docker ps`，您就能看見原始容器從 10 分鐘前便持續執行中。</span><span class="sxs-lookup"><span data-stu-id="dd90b-159">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="dd90b-160">發佈 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="dd90b-160">Publishing Docker images</span></span>

<span data-ttu-id="dd90b-161">只要完成應用程式的開發和偵錯循環，Visual Studio Tools for Docker 就會協助您建立應用程式的生產環境映像。</span><span class="sxs-lookup"><span data-stu-id="dd90b-161">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="dd90b-162">將 [偵錯] 下拉式清單變更為 [發行] 並建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd90b-162">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="dd90b-163">這項工具會產生附有 `:latest` 標記的映像，這個標記可以推送至您的私用登錄或 Docker Hub。</span><span class="sxs-lookup"><span data-stu-id="dd90b-163">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="dd90b-164">使用 `docker images` 命令可以查看映像清單。</span><span class="sxs-lookup"><span data-stu-id="dd90b-164">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="dd90b-165">使用者可能會預期生產或發行映像的大小會比 **dev** 映像更小，不過透過使用磁碟區對應，偵錯工具和應用程式實際上是從您的本機電腦執行，而不是從容器內執行。</span><span class="sxs-lookup"><span data-stu-id="dd90b-165">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="dd90b-166">**最新的**映像已封裝在執行主機電腦應用程式所需的整個應用程式程式碼中，所以此差異是應用程式程式碼的大小。</span><span class="sxs-lookup"><span data-stu-id="dd90b-166">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
