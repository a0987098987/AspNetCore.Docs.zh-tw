---
title: 在 Windows 服務上裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153525"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="735ff-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="735ff-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="735ff-104">作者：[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="735ff-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="735ff-105">在不使用 IIS 的情況下，若要在 Windows 上裝載 ASP.NET Core 應用程式，建議的方法是在 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)中執行該應用程式。</span><span class="sxs-lookup"><span data-stu-id="735ff-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="735ff-106">以 Windows 服務的形式裝載時，應用程式可以在重新開機和當機後自動啟動，而無須人為介入。</span><span class="sxs-lookup"><span data-stu-id="735ff-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="735ff-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="735ff-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="735ff-108">如需有關如何執行範例應用程式的指示，請參閱範例的 *README.md* 檔案。</span><span class="sxs-lookup"><span data-stu-id="735ff-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="735ff-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="735ff-109">Prerequisites</span></span>

* <span data-ttu-id="735ff-110">應用程式必須在 .NET Framework 執行階段上執行。</span><span class="sxs-lookup"><span data-stu-id="735ff-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="735ff-111">在 *.csproj* 檔案中，為 [TargetFramework](/nuget/schema/target-frameworks) 和 [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) 指定適當的值。</span><span class="sxs-lookup"><span data-stu-id="735ff-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="735ff-112">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="735ff-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="735ff-113">在 Visual Studio 中建立專案時，請使用 [ASP.NET Core 應用程式 (.NET Framework)] 範本。</span><span class="sxs-lookup"><span data-stu-id="735ff-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="735ff-114">如果應用程式會從網際網路 (而不僅僅從內部網路) 接收要求，它就必須使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 網頁伺服器 (以前稱為 [WebListener](xref:fundamentals/servers/weblistener)；適用於 ASP.NET Core 1.x 應用程式)，而不是使用 [Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="735ff-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="735ff-115">針對邊緣部署，建議使用 IIS 作為與 Kestrel 搭配運作的反向 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="735ff-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="735ff-116">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="735ff-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="735ff-117">開始使用</span><span class="sxs-lookup"><span data-stu-id="735ff-117">Get started</span></span>

<span data-ttu-id="735ff-118">本節說明設定現有 ASP.NET Core 專案以在服務中執行所需的最基本變更。</span><span class="sxs-lookup"><span data-stu-id="735ff-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="735ff-119">安裝 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="735ff-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="735ff-120">在 `Program.Main` 中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="735ff-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="735ff-121">呼叫 `host.RunAsService`，而不要呼叫 `host.Run`。</span><span class="sxs-lookup"><span data-stu-id="735ff-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="735ff-122">如果程式碼會呼叫 `UseContentRoot`，請使用發佈位置路徑，而不要使用 `Directory.GetCurrentDirectory()`。</span><span class="sxs-lookup"><span data-stu-id="735ff-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="735ff-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="735ff-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="735ff-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="735ff-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="735ff-125">將應用程式發佈至資料夾。</span><span class="sxs-lookup"><span data-stu-id="735ff-125">Publish the app to a folder.</span></span> <span data-ttu-id="735ff-126">使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) 或會發佈至資料夾的 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)。</span><span class="sxs-lookup"><span data-stu-id="735ff-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="735ff-127">建立並啟動服務來進行測試。</span><span class="sxs-lookup"><span data-stu-id="735ff-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="735ff-128">使用系統管理權限來開啟命令殼層，以使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具來建立並啟動服務。</span><span class="sxs-lookup"><span data-stu-id="735ff-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="735ff-129">如果服務是命名為 MyService、發佈至 `c:\svc`，然後命名為 AspNetCoreService，則命令為：</span><span class="sxs-lookup"><span data-stu-id="735ff-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="735ff-130">`binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="735ff-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![主控台視窗建立和啟動範例](windows-service/_static/create-start.png)

   <span data-ttu-id="735ff-132">當這些命令完成時，請瀏覽至以主控台應用程式形式執行時的相同路徑 (預設為 `http://localhost:5000`)：</span><span class="sxs-lookup"><span data-stu-id="735ff-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![在服務中執行](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="735ff-134">提供一個在服務外執行的方式</span><span class="sxs-lookup"><span data-stu-id="735ff-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="735ff-135">在服務外執行時較容易進行測試和偵錯，因此習慣上只有在特定情況下，才會新增呼叫 `RunAsService` 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="735ff-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="735ff-136">例如，使用 `--console` 命令列引數或連結偵錯工具，即可讓應用程式以主控台應用程式的形式執行：</span><span class="sxs-lookup"><span data-stu-id="735ff-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="735ff-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="735ff-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="735ff-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="735ff-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="735ff-139">處理停止和啟動事件</span><span class="sxs-lookup"><span data-stu-id="735ff-139">Handle stopping and starting events</span></span>

<span data-ttu-id="735ff-140">若要處理 `OnStarting`、`OnStarted` 及 `OnStopping` 事件，請進行下列額外的變更：</span><span class="sxs-lookup"><span data-stu-id="735ff-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="735ff-141">建立衍生自 `WebHostService` 的類別：</span><span class="sxs-lookup"><span data-stu-id="735ff-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="735ff-142">為 `IWebHost` 建立一個會將自訂 `WebHostService` 傳遞給 `ServiceBase.Run`的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="735ff-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="735ff-143">在 `Program.Main` 中，呼叫新的擴充方法 `RunAsCustomService`，而不是呼叫 `RunAsService`：</span><span class="sxs-lookup"><span data-stu-id="735ff-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="735ff-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="735ff-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="735ff-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="735ff-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="735ff-146">如果自訂的 `WebHostService` 程式碼需要一個來自相依性插入的服務 (例如記錄器)，請從 `IWebHost` 的 `Services` 屬性取得該服務：</span><span class="sxs-lookup"><span data-stu-id="735ff-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="735ff-147">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="735ff-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="735ff-148">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="735ff-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="735ff-149">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="735ff-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="735ff-150">感謝</span><span class="sxs-lookup"><span data-stu-id="735ff-150">Acknowledgments</span></span>

<span data-ttu-id="735ff-151">撰寫本文時借助了下列已發佈的來源：</span><span class="sxs-lookup"><span data-stu-id="735ff-151">This article was written with the help of published sources:</span></span>

* <span data-ttu-id="735ff-152">[以 Windows 服務的形式裝載 ASP.NET Core](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="735ff-152">[Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)</span></span>
* <span data-ttu-id="735ff-153">[如何在 Windows 服務中裝載 ASP.NET Core](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="735ff-153">[How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)</span></span>
