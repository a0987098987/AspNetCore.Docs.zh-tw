---
uid: mvc/overview/deployment/docker
title: 將 ASP.NET MVC 應用程式遷移到 Windows 容器
description: 了解如何擷取現有的 ASP.NET MVC 應用程式並在 Windows Docker 容器中執行
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 12/14/2018
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: ef184f4256c20e2a66de8fd2d4f8e67f07d9a086
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425155"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="b254e-104">將 ASP.NET MVC 應用程式遷移到 Windows 容器</span><span class="sxs-lookup"><span data-stu-id="b254e-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="b254e-105">在 Windows 容器中執行現有的 .NET Framework 架構應用程式，不需要對您的應用程式進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="b254e-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="b254e-106">若要在 Windows 容器中執行您的應用程式，您可以建立包含您應用程式的 Docker 映像，然後啟動該容器。</span><span class="sxs-lookup"><span data-stu-id="b254e-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="b254e-107">本主題說明如何才能擷取現有 [ASP.NET MVC 應用程式 (ASP.NET MVC application)](http://www.asp.net/mvc) 並部署到 Windows 容器中。</span><span class="sxs-lookup"><span data-stu-id="b254e-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="b254e-108">您可以從現有的 ASP.NET MVC 應用程式開始，然後使用 Visual Studio 建置已發行的資產。</span><span class="sxs-lookup"><span data-stu-id="b254e-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="b254e-109">您可以使用 Docker，建立其中包含並會執行您應用程式的映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="b254e-110">接著瀏覽到 Windows 容器中正在執行的網站，並確認應用程式可以正常運作。</span><span class="sxs-lookup"><span data-stu-id="b254e-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="b254e-111">本文假設您具備 Docker 的基本知識。</span><span class="sxs-lookup"><span data-stu-id="b254e-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="b254e-112">您可以閱讀 [Docker 概觀](https://docs.docker.com/engine/understanding-docker/)，以了解 Docker。</span><span class="sxs-lookup"><span data-stu-id="b254e-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="b254e-113">您將在容器中執行的應用程式是會隨機回答問題的簡單網站。</span><span class="sxs-lookup"><span data-stu-id="b254e-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="b254e-114">此應用程式是沒有驗證或資料庫儲存體的基本 MVC 應用程式，讓您著重於將 Web 層移至容器。</span><span class="sxs-lookup"><span data-stu-id="b254e-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="b254e-115">未來的主題將說明如何移動和管理容器化應用程式中的永續性儲存體。</span><span class="sxs-lookup"><span data-stu-id="b254e-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="b254e-116">移動您的應用程式包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b254e-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="b254e-117">建立發行工作以建置映像資產</span><span class="sxs-lookup"><span data-stu-id="b254e-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="b254e-118">建立用以執行應用程式的 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="b254e-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="b254e-119">啟動執行映像的 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="b254e-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="b254e-120">使用瀏覽器確認應用程式</span><span class="sxs-lookup"><span data-stu-id="b254e-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="b254e-121">[完成的應用程式](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator)在 GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="b254e-121">The [finished application](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b254e-122">必要條件</span><span class="sxs-lookup"><span data-stu-id="b254e-122">Prerequisites</span></span>

<span data-ttu-id="b254e-123">開發用的電腦必須具備下列軟體：</span><span class="sxs-lookup"><span data-stu-id="b254e-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="b254e-124">[Windows 10 年度更新版](https://www.microsoft.com/software-download/windows10/)（或更新版本） 或[Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) （或更新版本）</span><span class="sxs-lookup"><span data-stu-id="b254e-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="b254e-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - 穩定版 1.13.0 或 1.12 Beta 26 (或更新版本)</span><span class="sxs-lookup"><span data-stu-id="b254e-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="b254e-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b254e-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="b254e-127">如果您在使用 Windows Server 2016，請遵循[容器主機部署 - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment) 的指示。</span><span class="sxs-lookup"><span data-stu-id="b254e-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="b254e-128">安裝並啟動 Docker，以滑鼠右鍵按一下系統匣圖示，然後選取後**切換至 Windows 容器**。</span><span class="sxs-lookup"><span data-stu-id="b254e-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="b254e-129">如此才能根據 Windows 執行 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="b254e-130">此命令需要幾秒鐘的時間執行：</span><span class="sxs-lookup"><span data-stu-id="b254e-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="b254e-131">![Windows 容器][windows-container]</span><span class="sxs-lookup"><span data-stu-id="b254e-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="b254e-132">發行指令碼</span><span class="sxs-lookup"><span data-stu-id="b254e-132">Publish script</span></span>

<span data-ttu-id="b254e-133">將您必須載入 Docker 映像的所有資產收集在某一位置。</span><span class="sxs-lookup"><span data-stu-id="b254e-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="b254e-134">您可以使用 Visual Studio 的 [發行] 命令來建立應用程式的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="b254e-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="b254e-135">此設定檔會將所有資產放在一個樹狀目錄中，您稍後會在本教學課程將此目錄複製到目標映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="b254e-136">**發行步驟**</span><span class="sxs-lookup"><span data-stu-id="b254e-136">**Publish Steps**</span></span>

1. <span data-ttu-id="b254e-137">以滑鼠右鍵按一下 Visual Studio 中的 Web 專案，然後選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="b254e-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="b254e-138">按一下 [自訂設定檔] 按鈕，然後選取 [檔案系統] 作為方法。</span><span class="sxs-lookup"><span data-stu-id="b254e-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="b254e-139">選擇目錄。</span><span class="sxs-lookup"><span data-stu-id="b254e-139">Choose the directory.</span></span> <span data-ttu-id="b254e-140">依照慣例，下載的範例會使用 `bin\Release\PublishOutput`。</span><span class="sxs-lookup"><span data-stu-id="b254e-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="b254e-141">![發行連線][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="b254e-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="b254e-142">開啟 [設定] 索引標籤的 [檔案發行選項] 區段。選取 [發行期間預先編譯]。</span><span class="sxs-lookup"><span data-stu-id="b254e-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="b254e-143">此最佳化表示您將要編譯 Docker 容器中的檢視，而正在複製預先編譯的檢視。</span><span class="sxs-lookup"><span data-stu-id="b254e-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="b254e-144">![發行設定][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="b254e-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="b254e-145">按一下 [發行]，Visual Studio 會將所有需要的資產複製到目的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b254e-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="b254e-146">建立映像</span><span class="sxs-lookup"><span data-stu-id="b254e-146">Build the image</span></span>

<span data-ttu-id="b254e-147">建立新的檔案，名為*Dockerfile*來定義您的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-147">Create a new file named *Dockerfile* to define your Docker image.</span></span> <span data-ttu-id="b254e-148">*Dockerfile*包含指示，來建置最終映像，並包含任何基礎映像名稱、 所需的元件、 您想要執行的應用程式和其他組態映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-148">*Dockerfile* contains instructions to build the final image and includes any base image names, required components, the app you want to run, and other configuration images.</span></span> <span data-ttu-id="b254e-149">*Dockerfile*是輸入`docker build`建立映像的命令。</span><span class="sxs-lookup"><span data-stu-id="b254e-149">*Dockerfile* is the input to the `docker build` command that creates the image.</span></span>

<span data-ttu-id="b254e-150">針對此練習中，您將建置基礎的映像`microsoft/aspnet`映像位於[Docker Hub](https://hub.docker.com/r/microsoft/aspnet/)。</span><span class="sxs-lookup"><span data-stu-id="b254e-150">For this exercise, you will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="b254e-151">基礎映像 `microsoft/aspnet` 是 Windows Server 映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="b254e-152">它包含 Windows Server Core、 IIS 和 ASP.NET 4.7.2。</span><span class="sxs-lookup"><span data-stu-id="b254e-152">It contains Windows Server Core, IIS, and ASP.NET 4.7.2.</span></span> <span data-ttu-id="b254e-153">當您在容器中執行此映像時，其將會自動啟動 IIS 和已安裝的網站。</span><span class="sxs-lookup"><span data-stu-id="b254e-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="b254e-154">建立映像的 Dockerfile 看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="b254e-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="b254e-155">此 Dockerfile 中沒有 `ENTRYPOINT` 命令。</span><span class="sxs-lookup"><span data-stu-id="b254e-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="b254e-156">您不需要此命令。</span><span class="sxs-lookup"><span data-stu-id="b254e-156">You don't need one.</span></span> <span data-ttu-id="b254e-157">執行 Windows Server 與 IIS，IIS 處理序時，進入點，會設定為 aspnet 的基底映像中啟動。</span><span class="sxs-lookup"><span data-stu-id="b254e-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="b254e-158">執行 Docker 建置命令，建立會執行 ASP.NET 應用程式的映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="b254e-159">若要這樣做，請在您的專案目錄中開啟 PowerShell 視窗並的方案目錄中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b254e-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="b254e-160">此命令會建立使用您的 Dockerfile 中的指示新的映像命名 (-t 標記) 為 mvcrandomanswers 映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="b254e-161">這可能包括從 [Docker Hub](http://hub.docker.com) 提取基礎映像，然後將您的應用程式加入至該映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="b254e-162">該命令完成之後，您可以執行 `docker images` 命令來了解新映像的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="b254e-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="b254e-163">您電腦上的映像識別碼會有所不同。</span><span class="sxs-lookup"><span data-stu-id="b254e-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="b254e-164">現在，讓我們來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b254e-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="b254e-165">啟動容器</span><span class="sxs-lookup"><span data-stu-id="b254e-165">Start a container</span></span>

<span data-ttu-id="b254e-166">執行下列 `docker run` 命令來啟動容器：</span><span class="sxs-lookup"><span data-stu-id="b254e-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="b254e-167">`-d` 引數會指示 Docker 在離線模式中啟動映像。</span><span class="sxs-lookup"><span data-stu-id="b254e-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="b254e-168">這表示 Docker 映像可在與目前殼層中斷連線的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="b254e-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="b254e-169">在許多 docker 範例中，您可能會看到對應的容器和主機的連接埠-p。</span><span class="sxs-lookup"><span data-stu-id="b254e-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="b254e-170">預設的 aspnet 映像已經設定要在連接埠 80 上接聽，並將它公開的容器。</span><span class="sxs-lookup"><span data-stu-id="b254e-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="b254e-171">`--name randomanswers` 提供執行中容器的名稱。</span><span class="sxs-lookup"><span data-stu-id="b254e-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="b254e-172">您可以使用此名稱，而不是大多數命令中的容器識別碼。</span><span class="sxs-lookup"><span data-stu-id="b254e-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="b254e-173">`mvcrandomanswers` 是要啟動之映像的名稱。</span><span class="sxs-lookup"><span data-stu-id="b254e-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="b254e-174">在瀏覽器中確認</span><span class="sxs-lookup"><span data-stu-id="b254e-174">Verify in the browser</span></span>

<span data-ttu-id="b254e-175">一旦容器啟動時，連接到執行的容器使用`http://localhost`中所示的範例。</span><span class="sxs-lookup"><span data-stu-id="b254e-175">Once the container starts, connect to the running container using `http://localhost` in the example shown.</span></span> <span data-ttu-id="b254e-176">在您的瀏覽器中輸入該 URL，您應該會看到執行中的網站。</span><span class="sxs-lookup"><span data-stu-id="b254e-176">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="b254e-177">某些 VPN 或 Proxy 軟體可能會阻止您瀏覽至您的網站。</span><span class="sxs-lookup"><span data-stu-id="b254e-177">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="b254e-178">您可以暫時停用，以確保您的容器可以正常運作。</span><span class="sxs-lookup"><span data-stu-id="b254e-178">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="b254e-179">GitHub 上的範例目錄包含 [PowerShell 指令碼](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1)，可為您執行這些命令。</span><span class="sxs-lookup"><span data-stu-id="b254e-179">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="b254e-180">開啟 PowerShell 視窗，將目錄變更為您的方案目錄，然後輸入：</span><span class="sxs-lookup"><span data-stu-id="b254e-180">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="b254e-181">上述命令會建置該映像、 映像清單會顯示您的電腦，並啟動容器。</span><span class="sxs-lookup"><span data-stu-id="b254e-181">The command above builds the image, displays the list of images on your machine, and starts a container.</span></span>

<span data-ttu-id="b254e-182">若要停止容器，請發出 `docker stop` 命令：</span><span class="sxs-lookup"><span data-stu-id="b254e-182">To stop your container, issue a `docker stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="b254e-183">若要移除容器，請發出 `docker rm` 命令：</span><span class="sxs-lookup"><span data-stu-id="b254e-183">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "切換至 Windows 容器"
[publish-connection]: media/aspnetmvc/PublishConnection.png "發行至檔案系統"
[publish-settings]: media/aspnetmvc/PublishSettings.png "發行設定"
