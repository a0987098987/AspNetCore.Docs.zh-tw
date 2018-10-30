---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 7f19db0a1d12b904daff989bc969daf8d2302bfa
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325779"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="a0008-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0008-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="a0008-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a0008-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a0008-105">ASP.NET Core 應用程式可以裝載在 Windows 上，不需要使用 IIS 作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)。</span><span class="sxs-lookup"><span data-stu-id="a0008-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="a0008-106">當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="a0008-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="a0008-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0008-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="a0008-108">將專案轉換成 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="a0008-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="a0008-109">至少需要變更下列內容，才能設定現有的 ASP.NET Core 專案作為服務執行：</span><span class="sxs-lookup"><span data-stu-id="a0008-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="a0008-110">在專案檔中：</span><span class="sxs-lookup"><span data-stu-id="a0008-110">In the project file:</span></span>

   * <span data-ttu-id="a0008-111">確認有 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)，或將其新增至包含目標 Framework 的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="a0008-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      <span data-ttu-id="a0008-112">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="a0008-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="a0008-113">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="a0008-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="a0008-114">使用屬性名稱 `<RuntimeIdentifiers>` (複數)。</span><span class="sxs-lookup"><span data-stu-id="a0008-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="a0008-115">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="a0008-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="a0008-116">新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="a0008-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="a0008-117">在 `Program.Main` 中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="a0008-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="a0008-118">呼叫 [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)，而非 `host.Run`。</span><span class="sxs-lookup"><span data-stu-id="a0008-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="a0008-119">呼叫 [UseContentRoot](xref:fundamentals/host/web-host#content-root) 並使用應用程式發佈位置的路徑，不要使用 `Directory.GetCurrentDirectory()`。</span><span class="sxs-lookup"><span data-stu-id="a0008-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="a0008-120">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0008-120">Publish the app.</span></span> <span data-ttu-id="a0008-121">使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) 或 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)。</span><span class="sxs-lookup"><span data-stu-id="a0008-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="a0008-122">使用 Visual Studio 時，請選取 [FolderProfile]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a0008-122">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="a0008-123">若要使用命令列介面 (CLI) 工具發行範例應用程式，請從專案資料夾的命令提示字元執行 [dotnet publish](/dotnet/core/tools/dotnet-publish)命令。</span><span class="sxs-lookup"><span data-stu-id="a0008-123">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="a0008-124">必須在 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 中指定 RID 專案檔的屬性。</span><span class="sxs-lookup"><span data-stu-id="a0008-124">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="a0008-125">在下列範例中，應用程式在 `win7-x64` 執行階段發行設定中發行：</span><span class="sxs-lookup"><span data-stu-id="a0008-125">In the following example, the app is published in Release configuration for the `win7-x64` runtime:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. <span data-ttu-id="a0008-126">使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具建立服務。</span><span class="sxs-lookup"><span data-stu-id="a0008-126">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="a0008-127">`binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="a0008-127">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="a0008-128">**等號和路徑開頭的引號字元之間需要有間距。**</span><span class="sxs-lookup"><span data-stu-id="a0008-128">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="a0008-129">對於在專案資料夾中發行的服務，請使用 *publish* 資料夾的路徑來建立服務。</span><span class="sxs-lookup"><span data-stu-id="a0008-129">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="a0008-130">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="a0008-130">In the following example:</span></span>

   * <span data-ttu-id="a0008-131">專案位於 *c:\\my_services\\AspNetCoreService* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0008-131">The project resides in the *c:\\my_services\\AspNetCoreService* folder.</span></span>
   * <span data-ttu-id="a0008-132">專案是以 `Release` 設定所發行。</span><span class="sxs-lookup"><span data-stu-id="a0008-132">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="a0008-133">目標 Framework Moniker (TFM) 是 `netcoreapp2.1`。</span><span class="sxs-lookup"><span data-stu-id="a0008-133">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="a0008-134">執行階段識別碼 (RID) 是 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="a0008-134">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="a0008-135">應用程式可執行檔的名稱是 *AspNetCoreService.exe*。</span><span class="sxs-lookup"><span data-stu-id="a0008-135">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="a0008-136">服務的名稱是 **MyService**。</span><span class="sxs-lookup"><span data-stu-id="a0008-136">The service is named **MyService**.</span></span>

   <span data-ttu-id="a0008-137">範例：</span><span class="sxs-lookup"><span data-stu-id="a0008-137">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="a0008-138">確定 `binPath=` 引數與其值之間具有間距。</span><span class="sxs-lookup"><span data-stu-id="a0008-138">Make sure the space is present between the `binPath=` argument and its value.</span></span>

   <span data-ttu-id="a0008-139">若要從不同的資料夾發行並啟動服務：</span><span class="sxs-lookup"><span data-stu-id="a0008-139">To publish and start the service from a different folder:</span></span>

      * <span data-ttu-id="a0008-140">在 `dotnet publish` 命令上使用 [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) 選項。</span><span class="sxs-lookup"><span data-stu-id="a0008-140">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="a0008-141">若使用 Visual Studio，選取 [發行]\*\*\*\* 按鈕之前，請先選取 [FolderProfile]\*\*\*\* 發行屬性頁面中的 [目標位置]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a0008-141">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
      * <span data-ttu-id="a0008-142">使用 `sc.exe` 命令搭配輸出資料夾路徑來建立服務。</span><span class="sxs-lookup"><span data-stu-id="a0008-142">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="a0008-143">在提供給 `binPath` 的路徑中包含服務的可執行檔名稱。</span><span class="sxs-lookup"><span data-stu-id="a0008-143">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="a0008-144">以 `sc start <SERVICE_NAME>` 命令啟動服務。</span><span class="sxs-lookup"><span data-stu-id="a0008-144">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="a0008-145">請使用下列命令啟動範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="a0008-145">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="a0008-146">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="a0008-146">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="a0008-147">若要檢查服務的狀態，請使用 `sc query <SERVICE_NAME>` 命令。</span><span class="sxs-lookup"><span data-stu-id="a0008-147">To check the status of the service, use the `sc query <SERVICE_NAME>` command.</span></span> <span data-ttu-id="a0008-148">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="a0008-148">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="a0008-149">使用下列命令來檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="a0008-149">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="a0008-150">當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。</span><span class="sxs-lookup"><span data-stu-id="a0008-150">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="a0008-151">範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0008-151">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="a0008-152">以 `sc stop <SERVICE_NAME>` 命令停止服務。</span><span class="sxs-lookup"><span data-stu-id="a0008-152">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="a0008-153">下列命令會停止範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="a0008-153">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="a0008-154">在停止服務的短暫延遲之後，請以 `sc delete <SERVICE_NAME>` 命令解除安裝服務。</span><span class="sxs-lookup"><span data-stu-id="a0008-154">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="a0008-155">檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="a0008-155">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="a0008-156">當範例應用程式服務處於 `STOPPED` 狀態時，請使用下列命令來解除安裝範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="a0008-156">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="a0008-157">在服務外執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a0008-157">Run the app outside of a service</span></span>

<span data-ttu-id="a0008-158">在服務外執行時較容易進行測試和偵錯，因此習慣上只有在特定情況下，才會新增呼叫 `RunAsService` 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a0008-158">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="a0008-159">例如，使用 `--console` 命令列引數或連結偵錯工具，即可讓應用程式以主控台應用程式的形式執行：</span><span class="sxs-lookup"><span data-stu-id="a0008-159">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="a0008-160">因為 ASP.NET Core 組態需要命令列引數成對的名稱和數值，所以會先移除 `--console` 參數，再將引數傳遞至 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)。</span><span class="sxs-lookup"><span data-stu-id="a0008-160">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="a0008-161">因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="a0008-161">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="a0008-162">處理停止和啟動事件</span><span class="sxs-lookup"><span data-stu-id="a0008-162">Handle stopping and starting events</span></span>

<span data-ttu-id="a0008-163">請執行下列額外變更，以處理 [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) 和 [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) 事件：</span><span class="sxs-lookup"><span data-stu-id="a0008-163">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="a0008-164">建立衍生自 [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) 的類別：</span><span class="sxs-lookup"><span data-stu-id="a0008-164">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="a0008-165">為將自訂的 `WebHostService` 傳遞給 [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) 的 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 建立擴充方法：</span><span class="sxs-lookup"><span data-stu-id="a0008-165">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="a0008-166">在 `Program.Main` 中，呼叫新的擴充方法 `RunAsCustomService`，而不是呼叫 [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)：</span><span class="sxs-lookup"><span data-stu-id="a0008-166">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="a0008-167">因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="a0008-167">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="a0008-168">如果自訂的 `WebHostService` 程式碼需要一個來自相依性插入的服務 (例如記錄器)，請從 [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) 屬性取得該服務：</span><span class="sxs-lookup"><span data-stu-id="a0008-168">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="a0008-169">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="a0008-169">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="a0008-170">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="a0008-170">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="a0008-171">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="a0008-171">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="a0008-172">設定 HTTPS</span><span class="sxs-lookup"><span data-stu-id="a0008-172">Configure HTTPS</span></span>

<span data-ttu-id="a0008-173">指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="a0008-173">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="a0008-174">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="a0008-174">Current directory and content root</span></span>

<span data-ttu-id="a0008-175">針對 Windows 服務呼叫 `Directory.GetCurrentDirectory()` 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0008-175">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="a0008-176">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="a0008-176">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="a0008-177">請使用下列其中一種方法，於使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 時，利用 [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) 來維護及存取服務的資產和設定檔：</span><span class="sxs-lookup"><span data-stu-id="a0008-177">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="a0008-178">使用內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="a0008-178">Use the content root path.</span></span> <span data-ttu-id="a0008-179">`IHostingEnvironment.ContentRootPath` 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="a0008-179">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="a0008-180">請使用內容根路徑並維護應用程式內容根目錄中的檔案，而不是使用 `Directory.GetCurrentDirectory()` 來建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="a0008-180">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="a0008-181">將檔案儲存在磁碟上的適當位置。</span><span class="sxs-lookup"><span data-stu-id="a0008-181">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="a0008-182">請使用 `SetBasePath` 將絕對路徑指定為包含檔案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0008-182">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a0008-183">其他資源</span><span class="sxs-lookup"><span data-stu-id="a0008-183">Additional resources</span></span>

* <span data-ttu-id="a0008-184">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="a0008-184">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
