---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: bce09a500160f0bf13926786d277f8b1e88c1bf8
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894253"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="eaaa1-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eaaa1-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="eaaa1-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="eaaa1-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="eaaa1-105">ASP.NET Core 應用程式可以裝載在 Windows 上，不需要使用 IIS 作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="eaaa1-106">以 Windows 服務的形式裝載時，應用程式可以在重新開機和當機後自動啟動，而無須人為介入。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="eaaa1-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eaaa1-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="eaaa1-108">開始使用</span><span class="sxs-lookup"><span data-stu-id="eaaa1-108">Get started</span></span>

<span data-ttu-id="eaaa1-109">至少需要變更下列內容，才能設定現有的 ASP.NET Core 專案在服務中執行：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="eaaa1-110">在專案檔中：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-110">In the project file:</span></span>

   1. <span data-ttu-id="eaaa1-111">確認有執行階段識別碼，或將它新增至包含目標 Framework 的 **\<PropertyGroup>**：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="eaaa1-112">新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="eaaa1-113">在 `Program.Main` 中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="eaaa1-114">呼叫 [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)，而非 `host.Run`。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="eaaa1-115">呼叫 [UseContentRoot](xref:fundamentals/host/web-host#content-root) 並使用應用程式發佈位置的路徑，不要使用 `Directory.GetCurrentDirectory()`。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="eaaa1-116">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-116">Publish the app.</span></span> <span data-ttu-id="eaaa1-117">使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) 或 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="eaaa1-118">若要從命令列發佈範例應用程式，請在專案資料夾的主控台視窗中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="eaaa1-119">使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具建立服務。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="eaaa1-120">`binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="eaaa1-121">**等號和路徑開頭的引號字元之間需要有間距。**</span><span class="sxs-lookup"><span data-stu-id="eaaa1-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="eaaa1-122">對於在專案資料夾中發行的服務，請使用 *publish* 資料夾的路徑來建立服務。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="eaaa1-123">在下列範例中，此服務為：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="eaaa1-124">具名 **MyService**。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-124">Named **MyService**.</span></span>
   * <span data-ttu-id="eaaa1-125">已發行至 *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="eaaa1-126">以名為 *AspNetCoreService.exe* 的應用程式可執行檔表示。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="eaaa1-127">以系統管理權限開啟命令殼層，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="eaaa1-128">確定 `binPath=` 引數與其值之間具有間距。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="eaaa1-129">若要從不同的資料夾發行並啟動服務：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="eaaa1-130">在 `dotnet publish` 命令上使用 [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) 選項。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="eaaa1-131">使用 `sc.exe` 命令搭配輸出資料夾路徑來建立服務。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="eaaa1-132">在提供給 `binPath` 的路徑中包含服務的可執行檔名稱。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="eaaa1-133">以 `sc start <SERVICE_NAME>` 命令啟動服務。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="eaaa1-134">請使用下列命令啟動範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="eaaa1-135">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="eaaa1-136">`sc query <SERVICE_NAME>` 命令可以用來檢查服務的狀態以判斷其狀態：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="eaaa1-137">使用下列命令來檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="eaaa1-138">當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="eaaa1-139">範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="eaaa1-140">以 `sc stop <SERVICE_NAME>` 命令停止服務。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="eaaa1-141">下列命令會停止範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="eaaa1-142">在停止服務的短暫延遲之後，請以 `sc delete <SERVICE_NAME>` 命令解除安裝服務。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="eaaa1-143">檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="eaaa1-144">當範例應用程式服務處於 `STOPPED` 狀態時，請使用下列命令來解除安裝範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="eaaa1-145">提供一個在服務外執行的方式</span><span class="sxs-lookup"><span data-stu-id="eaaa1-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="eaaa1-146">在服務外執行時較容易進行測試和偵錯，因此習慣上只有在特定情況下，才會新增呼叫 `RunAsService` 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="eaaa1-147">例如，使用 `--console` 命令列引數或連結偵錯工具，即可讓應用程式以主控台應用程式的形式執行：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="eaaa1-148">因為 ASP.NET Core 組態需要命令列引數成對的名稱和數值，所以會先移除 `--console` 參數，再將引數傳遞至 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="eaaa1-149">處理停止和啟動事件</span><span class="sxs-lookup"><span data-stu-id="eaaa1-149">Handle stopping and starting events</span></span>

<span data-ttu-id="eaaa1-150">請執行下列額外變更，以處理 [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) 和 [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) 事件：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-150">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="eaaa1-151">建立衍生自 [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) 的類別：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-151">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="eaaa1-152">為將自訂的 `WebHostService` 傳遞給 [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) 的 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 建立擴充方法：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-152">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="eaaa1-153">在 `Program.Main` 中，呼叫新的擴充方法 `RunAsCustomService`，而不是呼叫 [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-153">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="eaaa1-154">如果自訂的 `WebHostService` 程式碼需要一個來自相依性插入的服務 (例如記錄器)，請從 [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) 屬性取得該服務：</span><span class="sxs-lookup"><span data-stu-id="eaaa1-154">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="eaaa1-155">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="eaaa1-155">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="eaaa1-156">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-156">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="eaaa1-157">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-157">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="eaaa1-158">Kestrel 端點組態</span><span class="sxs-lookup"><span data-stu-id="eaaa1-158">Kestrel endpoint configuration</span></span>

<span data-ttu-id="eaaa1-159">如需 Kestrel 端點組態，包括 HTTPS 組態和 SNI 支援的資訊，請參閱 [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="eaaa1-159">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
