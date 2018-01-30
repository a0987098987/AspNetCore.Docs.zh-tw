---
title: "在 Windows 服務的主機"
author: tdykstra
description: "了解如何裝載 ASP.NET Core 應用程式在 Windows 服務。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="47b5e-103">裝載在 Windows 服務的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="47b5e-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="47b5e-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="47b5e-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="47b5e-105">裝載在 Windows 上的 ASP.NET Core 應用程式，而不使用 IIS 執行它的建議的方式[Windows 服務](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications)。</span><span class="sxs-lookup"><span data-stu-id="47b5e-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="47b5e-106">如此一來它可以自動啟動之後重新開機和損毀，而不需等待有人登入。</span><span class="sxs-lookup"><span data-stu-id="47b5e-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="47b5e-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="47b5e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="47b5e-108">請參閱[接下來的步驟](#next-steps)> 一節，如需如何執行此程式碼的指示。</span><span class="sxs-lookup"><span data-stu-id="47b5e-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47b5e-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="47b5e-109">Prerequisites</span></span>

* <span data-ttu-id="47b5e-110">應用程式必須執行.NET Framework 執行階段。</span><span class="sxs-lookup"><span data-stu-id="47b5e-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="47b5e-111">在*.csproj*檔案中，指定適當的值[TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks)和[RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="47b5e-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="47b5e-112">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="47b5e-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="47b5e-113">使用 Visual Studio 中建立專案時**ASP.NET Core 應用程式 (.NET Framework)**範本。</span><span class="sxs-lookup"><span data-stu-id="47b5e-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="47b5e-114">如果應用程式接收要求，從網際網路 （不只是從內部網路），則必須使用[WebListener](xref:fundamentals/servers/weblistener) web 伺服器而非[Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="47b5e-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="47b5e-115">Kestrel 必須搭配 IIS 邊緣部署。</span><span class="sxs-lookup"><span data-stu-id="47b5e-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="47b5e-116">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="47b5e-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="47b5e-117">使用者入門</span><span class="sxs-lookup"><span data-stu-id="47b5e-117">Getting started</span></span>

<span data-ttu-id="47b5e-118">本節說明將現有的 ASP.NET Core 專案設定為在服務執行所需的最小變更。</span><span class="sxs-lookup"><span data-stu-id="47b5e-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="47b5e-119">安裝 NuGet 套件[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)。</span><span class="sxs-lookup"><span data-stu-id="47b5e-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="47b5e-120">在變更下列`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="47b5e-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="47b5e-121">呼叫`host.RunAsService`而不是`host.Run`。</span><span class="sxs-lookup"><span data-stu-id="47b5e-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="47b5e-122">如果程式碼會呼叫`UseContentRoot`，到發行位置，而不是使用的路徑`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="47b5e-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="47b5e-123">發行應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b5e-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="47b5e-124">使用[dotnet 發行](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish)或[Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)所發行的資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b5e-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="47b5e-125">測試建立和啟動服務。</span><span class="sxs-lookup"><span data-stu-id="47b5e-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="47b5e-126">開啟要使用系統管理員命令提示字元視窗[sc.exe](https://technet.microsoft.com/library/bb490995)命令列工具來建立及啟動服務。</span><span class="sxs-lookup"><span data-stu-id="47b5e-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="47b5e-127">如果服務為 MyService，應用程式發行到`c:\svc`，而且應用程式本身名為 AspNetCoreService，命令看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="47b5e-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="47b5e-128">`binPath`值是應用程式的可執行檔，包括可執行檔的檔案名稱本身的路徑。</span><span class="sxs-lookup"><span data-stu-id="47b5e-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![主控台視窗建立及啟動範例](windows-service/_static/create-start.png)

  <span data-ttu-id="47b5e-130">當命令完成時，瀏覽至與相同的路徑做為主控台應用程式執行時 (根據預設， `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="47b5e-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![在服務中執行](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="47b5e-132">提供服務外部執行的方式</span><span class="sxs-lookup"><span data-stu-id="47b5e-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="47b5e-133">更輕鬆地測試和偵錯時執行外部服務，所以可將呼叫的程式碼加入`host.RunAsService`只在某些情況下。</span><span class="sxs-lookup"><span data-stu-id="47b5e-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="47b5e-134">例如，應用程式可以執行為主控台應用程式與`--console`命令列引數，或是如果附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="47b5e-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="47b5e-135">停止和啟動的事件處理</span><span class="sxs-lookup"><span data-stu-id="47b5e-135">Handle stopping and starting events</span></span>

<span data-ttu-id="47b5e-136">若要處理`OnStarting`， `OnStarted`，和`OnStopping`事件，進行下列的其他變更：</span><span class="sxs-lookup"><span data-stu-id="47b5e-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="47b5e-137">建立衍生自 `WebHostService` 的類別。</span><span class="sxs-lookup"><span data-stu-id="47b5e-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="47b5e-138">建立擴充方法給`IWebHost`傳遞之任何自訂`WebHostService`至`ServiceBase.Run`。</span><span class="sxs-lookup"><span data-stu-id="47b5e-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="47b5e-139">在`Program.Main`變更呼叫新的擴充方法，而不是`host.RunAsService`。</span><span class="sxs-lookup"><span data-stu-id="47b5e-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="47b5e-140">如果自訂`WebHostService`的程式碼需要從相依性插入 （例如記錄器） 取得服務，請將它從`Services`屬性`IWebHost`。</span><span class="sxs-lookup"><span data-stu-id="47b5e-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="47b5e-141">後續步驟</span><span class="sxs-lookup"><span data-stu-id="47b5e-141">Next steps</span></span>

<span data-ttu-id="47b5e-142">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)伴隨著此發行項是簡單的 MVC web 應用程式已經修改上述程式碼範例所示。</span><span class="sxs-lookup"><span data-stu-id="47b5e-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="47b5e-143">若要在服務中執行它，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="47b5e-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="47b5e-144">發行至*c:\svc*。</span><span class="sxs-lookup"><span data-stu-id="47b5e-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="47b5e-145">開啟系統管理員 視窗。</span><span class="sxs-lookup"><span data-stu-id="47b5e-145">Open an administrator window.</span></span>

* <span data-ttu-id="47b5e-146">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="47b5e-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="47b5e-147">在瀏覽器，移至 http://localhost:5000/ 來確認正在執行。</span><span class="sxs-lookup"><span data-stu-id="47b5e-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="47b5e-148">如果應用程式未如預期的服務執行時啟動啟動，讓錯誤訊息都能存取快速方法是將記錄提供者，例如[Windows 事件記錄檔提供者](xref:fundamentals/logging/index#eventlog)。</span><span class="sxs-lookup"><span data-stu-id="47b5e-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="47b5e-149">通知</span><span class="sxs-lookup"><span data-stu-id="47b5e-149">Acknowledgments</span></span>

<span data-ttu-id="47b5e-150">撰寫本文時的協助的已發行的來源。</span><span class="sxs-lookup"><span data-stu-id="47b5e-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="47b5e-151">最早和最有用的是這些內容：</span><span class="sxs-lookup"><span data-stu-id="47b5e-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="47b5e-152">為 Windows 服務裝載的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47b5e-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="47b5e-153">如何在裝載您的 ASP.NET 核心 Windows 服務中</span><span class="sxs-lookup"><span data-stu-id="47b5e-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
