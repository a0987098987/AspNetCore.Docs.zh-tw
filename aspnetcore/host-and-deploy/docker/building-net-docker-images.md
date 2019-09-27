---
title: ASP.NET Core 的 Docker 映像
author: rick-anderson
description: 了解如何使用 Docker 登錄中已發佈的 .NET Core Docker 映像。 提取映像並建置自己的映像。
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: 12665fb2e7a9c75747f5c83129a617aea6adfbbf
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317699"
---
# <a name="docker-images-for-aspnet-core"></a><span data-ttu-id="26641-104">ASP.NET Core 的 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="26641-104">Docker images for ASP.NET Core</span></span>

<span data-ttu-id="26641-105">本教學課程示範如何在 Docker 容器中執行 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-105">This tutorial shows how to run an ASP.NET Core app in Docker containers.</span></span>

<span data-ttu-id="26641-106">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="26641-106">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="26641-107">了解 Microsoft.NET Core Docker 映像</span><span class="sxs-lookup"><span data-stu-id="26641-107">Learn about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="26641-108">下載 ASP.NET Core 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-108">Download an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="26641-109">在本機執行範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-109">Run the sample app locally</span></span>
> * <span data-ttu-id="26641-110">在 Linux 容器中執行範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-110">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="26641-111">在 Windows 容器中執行範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-111">Run the sample app in Windows containers</span></span>
> * <span data-ttu-id="26641-112">手動建置並部署</span><span class="sxs-lookup"><span data-stu-id="26641-112">Build and deploy manually</span></span>

## <a name="aspnet-core-docker-images"></a><span data-ttu-id="26641-113">ASP.NET Core Docker 映像</span><span class="sxs-lookup"><span data-stu-id="26641-113">ASP.NET Core Docker images</span></span>

<span data-ttu-id="26641-114">針對本教學課程，您可以下載 ASP.NET Core 範例應用程式，並在 Docker 容器中執行它。</span><span class="sxs-lookup"><span data-stu-id="26641-114">For this tutorial, you download an ASP.NET Core sample app and run it in Docker containers.</span></span> <span data-ttu-id="26641-115">該範例可與 Linux 或 Windows 容器搭配使用。</span><span class="sxs-lookup"><span data-stu-id="26641-115">The sample works with both Linux and Windows containers.</span></span>

<span data-ttu-id="26641-116">範例 Dockerfile 會使用 [Docker 多階段建置功能](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) \(英文\)，在不同容器中建置並執行。</span><span class="sxs-lookup"><span data-stu-id="26641-116">The sample Dockerfile uses the [Docker multi-stage build feature](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) to build and run in different containers.</span></span> <span data-ttu-id="26641-117">建置和執行容器會從 Docker Hub 中 Microsoft 所提供的映像來建置：</span><span class="sxs-lookup"><span data-stu-id="26641-117">The build and run containers are created from images that are provided in Docker Hub by Microsoft:</span></span>

* `dotnet/core/sdk`

  <span data-ttu-id="26641-118">範例會使用此映像來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-118">The sample uses this image for building the app.</span></span> <span data-ttu-id="26641-119">此映像包含隨附命令列工具 (CLI) 的 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="26641-119">The image contains the .NET Core SDK, which includes the Command Line Tools (CLI).</span></span> <span data-ttu-id="26641-120">此映像會最佳化來進行本機開發、偵錯和單元測試。</span><span class="sxs-lookup"><span data-stu-id="26641-120">The image is optimized for local development, debugging, and unit testing.</span></span> <span data-ttu-id="26641-121">基於開發和編譯而安裝的工具可使此映像成為相對較大的映像。</span><span class="sxs-lookup"><span data-stu-id="26641-121">The tools installed for development and compilation make this a relatively large image.</span></span> 

* `dotnet/core/aspnet` 

   <span data-ttu-id="26641-122">範例會使用此映像來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-122">The sample uses this image for running the app.</span></span> <span data-ttu-id="26641-123">此映像包含 ASP.NET Core 執行階段和程式庫，並會進行最佳化，以在生產環境中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-123">The image contains the ASP.NET Core runtime and libraries and is optimized for running apps in production.</span></span> <span data-ttu-id="26641-124">專為部署和應用程式啟動速度而設計的映像相對較小，因此，已將從 Docker 登錄到 Docker 主機的網路效能最佳化。</span><span class="sxs-lookup"><span data-stu-id="26641-124">Designed for speed of deployment and app startup, the image is relatively small, so network performance from Docker Registry to Docker host is optimized.</span></span> <span data-ttu-id="26641-125">只會將執行應用程式所需的程式庫和內容複製到容器中。</span><span class="sxs-lookup"><span data-stu-id="26641-125">Only the binaries and content needed to run an app are copied to the container.</span></span> <span data-ttu-id="26641-126">內容已準備好執行，可用最短的時間從 `Docker run` 到應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="26641-126">The contents are ready to run, enabling the fastest time from `Docker run` to app startup.</span></span> <span data-ttu-id="26641-127">在 Docker 模型中，不需要動態程式碼編譯。</span><span class="sxs-lookup"><span data-stu-id="26641-127">Dynamic code compilation isn't needed in the Docker model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26641-128">必要條件</span><span class="sxs-lookup"><span data-stu-id="26641-128">Prerequisites</span></span>

* [<span data-ttu-id="26641-129">.NET Core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="26641-129">.NET Core SDK 3.0</span></span>](https://dotnet.microsoft.com/download)

* <span data-ttu-id="26641-130">Docker 用戶端 18.03 或更新版本</span><span class="sxs-lookup"><span data-stu-id="26641-130">Docker client 18.03 or later</span></span>

  * <span data-ttu-id="26641-131">Linux 散發</span><span class="sxs-lookup"><span data-stu-id="26641-131">Linux distributions</span></span>
    * [<span data-ttu-id="26641-132">CentOS</span><span class="sxs-lookup"><span data-stu-id="26641-132">CentOS</span></span>](https://docs.docker.com/install/linux/docker-ce/centos/)
    * [<span data-ttu-id="26641-133">Debian</span><span class="sxs-lookup"><span data-stu-id="26641-133">Debian</span></span>](https://docs.docker.com/install/linux/docker-ce/debian/)
    * [<span data-ttu-id="26641-134">Fedora</span><span class="sxs-lookup"><span data-stu-id="26641-134">Fedora</span></span>](https://docs.docker.com/install/linux/docker-ce/fedora/)
    * [<span data-ttu-id="26641-135">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="26641-135">Ubuntu</span></span>](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [<span data-ttu-id="26641-136">macOS</span><span class="sxs-lookup"><span data-stu-id="26641-136">macOS</span></span>](https://docs.docker.com/docker-for-mac/install/)
  * [<span data-ttu-id="26641-137">Windows</span><span class="sxs-lookup"><span data-stu-id="26641-137">Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

* [<span data-ttu-id="26641-138">Git</span><span class="sxs-lookup"><span data-stu-id="26641-138">Git</span></span>](https://git-scm.com/download)

## <a name="download-the-sample-app"></a><span data-ttu-id="26641-139">下載範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-139">Download the sample app</span></span>

* <span data-ttu-id="26641-140">藉由複製 [.NET Core Docker 存放庫](https://github.com/dotnet/dotnet-docker) \(英文\) 來下載範例：</span><span class="sxs-lookup"><span data-stu-id="26641-140">Download the sample by cloning the [.NET Core Docker repository](https://github.com/dotnet/dotnet-docker):</span></span> 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a><span data-ttu-id="26641-141">在本機執行應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-141">Run the app locally</span></span>

* <span data-ttu-id="26641-142">瀏覽至 *dotnet-docker/samples/aspnetapp/aspnetapp* 中的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="26641-142">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="26641-143">執行下列命令，在本機建置並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="26641-143">Run the following command to build and run the app locally:</span></span>

  ```dotnetcli
  dotnet run
  ```

* <span data-ttu-id="26641-144">在瀏覽器中移至 `http://localhost:5000` 以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-144">Go to `http://localhost:5000` in a browser to test the app.</span></span>

* <span data-ttu-id="26641-145">在命令提示字元中按 Ctrl + C 以停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-145">Press Ctrl+C at the command prompt to stop the app.</span></span>

## <a name="run-in-a-linux-container"></a><span data-ttu-id="26641-146">在 Linux 容器中執行</span><span class="sxs-lookup"><span data-stu-id="26641-146">Run in a Linux container</span></span>

* <span data-ttu-id="26641-147">在 Docker 用戶端中，切換至 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="26641-147">In the Docker client, switch to Linux containers.</span></span>

* <span data-ttu-id="26641-148">瀏覽至 *dotnet-docker/samples/aspnetapp* 中的 Dockerfile 資料夾。</span><span class="sxs-lookup"><span data-stu-id="26641-148">Navigate to the Dockerfile folder at *dotnet-docker/samples/aspnetapp*.</span></span>

* <span data-ttu-id="26641-149">執行下列命令，在 Docker 中建置並執行範例：</span><span class="sxs-lookup"><span data-stu-id="26641-149">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  <span data-ttu-id="26641-150">`build` 命令引數：</span><span class="sxs-lookup"><span data-stu-id="26641-150">The `build` command arguments:</span></span>
  * <span data-ttu-id="26641-151">將映像命名為 aspnetapp。</span><span class="sxs-lookup"><span data-stu-id="26641-151">Name the image aspnetapp.</span></span>
  * <span data-ttu-id="26641-152">在目前的資料夾 (結束期間) 中尋找 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="26641-152">Look for the Dockerfile in the current folder (the period at the end).</span></span>

  <span data-ttu-id="26641-153">run 命令引數：</span><span class="sxs-lookup"><span data-stu-id="26641-153">The run command arguments:</span></span>
  * <span data-ttu-id="26641-154">配置虛擬 TTY，即使未連接，還是要使其保持開啟</span><span class="sxs-lookup"><span data-stu-id="26641-154">Allocate a pseudo-TTY and keep it open even if not attached.</span></span> <span data-ttu-id="26641-155">(與 `--interactive --tty` 的效果相同)。</span><span class="sxs-lookup"><span data-stu-id="26641-155">(Same effect as `--interactive --tty`.)</span></span>
  * <span data-ttu-id="26641-156">結束時自動移除容器。</span><span class="sxs-lookup"><span data-stu-id="26641-156">Automatically remove the container when it exits.</span></span>
  * <span data-ttu-id="26641-157">將本機電腦上的連接埠 5000 對應至容器中的連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="26641-157">Map port 5000 on the local machine to port 80 in the container.</span></span>
  * <span data-ttu-id="26641-158">將容器命名為 aspnetcore_sample。</span><span class="sxs-lookup"><span data-stu-id="26641-158">Name the container aspnetcore_sample.</span></span>
  * <span data-ttu-id="26641-159">指定 aspnetapp 映像。</span><span class="sxs-lookup"><span data-stu-id="26641-159">Specify the aspnetapp image.</span></span>

* <span data-ttu-id="26641-160">在瀏覽器中移至 `http://localhost:5000` 以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-160">Go to `http://localhost:5000` in a browser to test the app.</span></span>

## <a name="run-in-a-windows-container"></a><span data-ttu-id="26641-161">在 Windows 容器中執行</span><span class="sxs-lookup"><span data-stu-id="26641-161">Run in a Windows container</span></span>

* <span data-ttu-id="26641-162">在 Docker 用戶端中，切換至 Windows 容器。</span><span class="sxs-lookup"><span data-stu-id="26641-162">In the Docker client, switch to Windows containers.</span></span>

<span data-ttu-id="26641-163">瀏覽至 `dotnet-docker/samples/aspnetapp` 中的 docker 檔案資料夾。</span><span class="sxs-lookup"><span data-stu-id="26641-163">Navigate to the docker file folder at `dotnet-docker/samples/aspnetapp`.</span></span>

* <span data-ttu-id="26641-164">執行下列命令，在 Docker 中建置並執行範例：</span><span class="sxs-lookup"><span data-stu-id="26641-164">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* <span data-ttu-id="26641-165">針對 Windows 容器，您需要容器的 IP 位址 (瀏覽至 `http://localhost:5000` 將無法運作)：</span><span class="sxs-lookup"><span data-stu-id="26641-165">For Windows containers, you need the IP address of the container (browsing to `http://localhost:5000` won't work):</span></span>
  * <span data-ttu-id="26641-166">開啟另一個命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="26641-166">Open up another command prompt.</span></span>
  * <span data-ttu-id="26641-167">執行 `docker ps` 以查看執行中的容器。</span><span class="sxs-lookup"><span data-stu-id="26641-167">Run `docker ps` to see the running containers.</span></span> <span data-ttu-id="26641-168">確認其中有 "aspnetcore_sample" 容器。</span><span class="sxs-lookup"><span data-stu-id="26641-168">Verify that the "aspnetcore_sample" container is there.</span></span>
  * <span data-ttu-id="26641-169">執行 `docker exec aspnetcore_sample ipconfig` 來顯示容器的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="26641-169">Run `docker exec aspnetcore_sample ipconfig` to display the IP address of the container.</span></span> <span data-ttu-id="26641-170">此命令的輸出看起來就像下列範例：</span><span class="sxs-lookup"><span data-stu-id="26641-170">The output from the command looks like this example:</span></span>

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* <span data-ttu-id="26641-171">複製容器 IPv4 位址 (例如 172.29.245.43) 並貼入瀏覽器網址列，以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-171">Copy the container IPv4 address (for example, 172.29.245.43) and paste into the browser address bar to test the app.</span></span>

## <a name="build-and-deploy-manually"></a><span data-ttu-id="26641-172">手動建置並部署</span><span class="sxs-lookup"><span data-stu-id="26641-172">Build and deploy manually</span></span>

<span data-ttu-id="26641-173">在某些情況下，您可能想要藉由將其複製到執行階段所需的應用程式檔案，來將應用程式部署到容器。</span><span class="sxs-lookup"><span data-stu-id="26641-173">In some scenarios, you might want to deploy an app to a container by copying to it the application files that are needed at run time.</span></span> <span data-ttu-id="26641-174">本節示範如何手動部署。</span><span class="sxs-lookup"><span data-stu-id="26641-174">This section shows how to deploy manually.</span></span>

* <span data-ttu-id="26641-175">瀏覽至 *dotnet-docker/samples/aspnetapp/aspnetapp* 中的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="26641-175">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="26641-176">執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令：</span><span class="sxs-lookup"><span data-stu-id="26641-176">Run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

  ```dotnetcli
  dotnet publish -c Release -o published
  ```

  <span data-ttu-id="26641-177">命令引數：</span><span class="sxs-lookup"><span data-stu-id="26641-177">The command arguments:</span></span>
  * <span data-ttu-id="26641-178">以發行模式建置應用程式 (預設值是偵錯模式)。</span><span class="sxs-lookup"><span data-stu-id="26641-178">Build the application in release mode (the default is debug mode).</span></span>
  * <span data-ttu-id="26641-179">在 *published* 資料夾中建立檔案。</span><span class="sxs-lookup"><span data-stu-id="26641-179">Create the files in the *published* folder.</span></span>

* <span data-ttu-id="26641-180">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="26641-180">Run the application.</span></span>

  * <span data-ttu-id="26641-181">Windows：</span><span class="sxs-lookup"><span data-stu-id="26641-181">Windows:</span></span>

    ```dotnetcli
    dotnet published\aspnetapp.dll
    ```

  * <span data-ttu-id="26641-182">Linux：</span><span class="sxs-lookup"><span data-stu-id="26641-182">Linux:</span></span>

    ```dotnetcli
    dotnet published/aspnetapp.dll
    ```

* <span data-ttu-id="26641-183">瀏覽至 `http://localhost:5000` 以查看首頁。</span><span class="sxs-lookup"><span data-stu-id="26641-183">Browse to `http://localhost:5000` to see the home page.</span></span>

<span data-ttu-id="26641-184">若要在 Docker 容器內使用手動發行的應用程式，請建立新的 Dockerfile 並使用 `docker build .` 命令來建置容器。</span><span class="sxs-lookup"><span data-stu-id="26641-184">To use the manually published application within a Docker container, create a new Dockerfile and use the `docker build .` command to build the container.</span></span>

```console
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY published/aspnetapp.dll ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

### <a name="the-dockerfile"></a><span data-ttu-id="26641-185">Dockerfile</span><span class="sxs-lookup"><span data-stu-id="26641-185">The Dockerfile</span></span>

<span data-ttu-id="26641-186">以下是您稍早執行的`docker build`命令所使用的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="26641-186">Here's the *Dockerfile* used by the `docker build` command you ran earlier.</span></span>  <span data-ttu-id="26641-187">它會以您在本節所做的相同方式，使用 `dotnet publish` 進行建置及部署。</span><span class="sxs-lookup"><span data-stu-id="26641-187">It uses `dotnet publish` the same way you did in this section to build and deploy.</span></span>  

```dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /app/aspnetapp
RUN dotnet publish -c Release -o out


FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY --from=build /app/aspnetapp/out ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

## <a name="additional-resources"></a><span data-ttu-id="26641-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="26641-188">Additional resources</span></span>

* <span data-ttu-id="26641-189">[Docker build 命令列](https://docs.docker.com/engine/reference/commandline/build) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="26641-189">[Docker build command](https://docs.docker.com/engine/reference/commandline/build)</span></span>
* <span data-ttu-id="26641-190">[Docker run 命令列](https://docs.docker.com/engine/reference/commandline/run) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="26641-190">[Docker run command](https://docs.docker.com/engine/reference/commandline/run)</span></span>
* <span data-ttu-id="26641-191">[ASP.NET Core Docker 範例](https://github.com/dotnet/dotnet-docker) \(英文\) (本教學課程中所使用的範例。)</span><span class="sxs-lookup"><span data-stu-id="26641-191">[ASP.NET Core Docker sample](https://github.com/dotnet/dotnet-docker) (The one used in this tutorial.)</span></span>
* [<span data-ttu-id="26641-192">設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器</span><span class="sxs-lookup"><span data-stu-id="26641-192">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="26641-193">使用 Visual Studio Docker 工具</span><span class="sxs-lookup"><span data-stu-id="26641-193">Working with Visual Studio Docker Tools</span></span>](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* <span data-ttu-id="26641-194">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="26641-194">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers)</span></span> 

## <a name="next-steps"></a><span data-ttu-id="26641-195">後續步驟</span><span class="sxs-lookup"><span data-stu-id="26641-195">Next steps</span></span>

<span data-ttu-id="26641-196">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="26641-196">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="26641-197">了解 Microsoft .NET Core Docker 映像</span><span class="sxs-lookup"><span data-stu-id="26641-197">Learned about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="26641-198">已下載 ASP.NET Core 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-198">Downloaded an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="26641-199">在本機執行範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-199">Run the sample app locally</span></span>
> * <span data-ttu-id="26641-200">在 Linux 容器中執行範例應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-200">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="26641-201">使用 Windows 容器執行範例</span><span class="sxs-lookup"><span data-stu-id="26641-201">Run the sample with in Windows containers</span></span>
> * <span data-ttu-id="26641-202">手動建置並部署</span><span class="sxs-lookup"><span data-stu-id="26641-202">Built and deployed manually</span></span>

<span data-ttu-id="26641-203">包含範例應用程式的 Git 存放庫也會包含文件。</span><span class="sxs-lookup"><span data-stu-id="26641-203">The Git repository that contains the sample app also includes documentation.</span></span> <span data-ttu-id="26641-204">如需存放庫中可用資源的概觀，請參閱[讀我檔案](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="26641-204">For an overview of the resources available in the repository, see [the README file](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span></span> <span data-ttu-id="26641-205">特別是了解如何實作 HTTPS：</span><span class="sxs-lookup"><span data-stu-id="26641-205">In particular, learn how to implement HTTPS:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="26641-206">透過 HTTPS 使用 Docker 開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="26641-206">Developing ASP.NET Core Applications with Docker over HTTPS</span></span>](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)
