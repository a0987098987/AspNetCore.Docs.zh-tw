---
title: "在 Windows 服務的主機"
author: tdykstra
description: "了解如何裝載 ASP.NET Core 應用程式在 Windows 服務。"
keywords: "ASP.NET Core，Windows 服務裝載"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: a6d1acf5ab8f40b0b4d487a6f34cd83d13907852
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="52215-104">裝載在 Windows 服務的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="52215-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="52215-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="52215-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="52215-106">當您不使用 IIS 裝載的 ASP.NET Core 應用程式，在 Windows 上的建議的方式是執行[Windows 服務](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications)。</span><span class="sxs-lookup"><span data-stu-id="52215-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="52215-107">如此一來它可以自動啟動之後重新開機和損毀，而不需等待有人登入。</span><span class="sxs-lookup"><span data-stu-id="52215-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="52215-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="52215-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="52215-109">請參閱[接下來的步驟](#next-steps)> 一節，如需如何執行此程式碼的指示。</span><span class="sxs-lookup"><span data-stu-id="52215-109">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52215-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="52215-110">Prerequisites</span></span>

* <span data-ttu-id="52215-111">應用程式必須執行.NET Framework 執行階段。</span><span class="sxs-lookup"><span data-stu-id="52215-111">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="52215-112">在*.csproj*檔案中，指定適當的值[TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks)和[RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="52215-112">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="52215-113">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="52215-113">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="52215-114">使用 Visual Studio 中建立專案時**ASP.NET Core 應用程式 (.NET Framework)**範本。</span><span class="sxs-lookup"><span data-stu-id="52215-114">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="52215-115">如果應用程式將取得要求從網際網路 （不只是從內部網路），則必須使用[WebListener](xref:fundamentals/servers/weblistener) web 伺服器而非[Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="52215-115">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="52215-116">Kestrel 必須搭配 IIS 邊緣部署。</span><span class="sxs-lookup"><span data-stu-id="52215-116">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="52215-117">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="52215-117">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="52215-118">使用者入門</span><span class="sxs-lookup"><span data-stu-id="52215-118">Getting started</span></span>

<span data-ttu-id="52215-119">本節說明將現有的 ASP.NET Core 專案設定為在服務執行所需的最小變更。</span><span class="sxs-lookup"><span data-stu-id="52215-119">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="52215-120">安裝 NuGet 套件[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)。</span><span class="sxs-lookup"><span data-stu-id="52215-120">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="52215-121">在變更下列`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="52215-121">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="52215-122">呼叫`host.RunAsService`而不是`host.Run`。</span><span class="sxs-lookup"><span data-stu-id="52215-122">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="52215-123">如果您的程式碼呼叫`UseContentRoot`，到發行位置，而不是使用的路徑`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="52215-123">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="52215-124">發行應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="52215-124">Publish the application to a folder.</span></span>

  <span data-ttu-id="52215-125">使用[dotnet 發行](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish)或[Visual Studio 發行設定檔](xref:publishing/web-publishing-vs)所發行的資料夾。</span><span class="sxs-lookup"><span data-stu-id="52215-125">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="52215-126">測試建立和啟動服務。</span><span class="sxs-lookup"><span data-stu-id="52215-126">Test by creating and starting the service.</span></span>

  <span data-ttu-id="52215-127">開啟要使用系統管理員命令提示字元視窗[sc.exe](https://technet.microsoft.com/library/bb490995)命令列工具來建立及啟動服務。</span><span class="sxs-lookup"><span data-stu-id="52215-127">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="52215-128">如果您命名 MyService 服務時，就會發行您的應用程式`c:\svc`，而且應用程式本身名為 AspNetCoreService，命令看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="52215-128">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="52215-129">`binPath`值是您的應用程式可執行檔，包括可執行檔的檔案名稱本身的路徑。</span><span class="sxs-lookup"><span data-stu-id="52215-129">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![主控台視窗建立及啟動範例](windows-service/_static/create-start.png)

  <span data-ttu-id="52215-131">當命令完成時，您可以瀏覽至與當您執行為主控台應用程式相同的路徑 (根據預設， `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="52215-131">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![在服務中執行](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="52215-133">提供服務外部執行的方式</span><span class="sxs-lookup"><span data-stu-id="52215-133">Provide a way to run outside of a service</span></span>

<span data-ttu-id="52215-134">更輕鬆地測試和偵錯時您執行外部服務，所以可將呼叫的程式碼加入`host.RunAsService`只在某些情況下。</span><span class="sxs-lookup"><span data-stu-id="52215-134">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="52215-135">例如，您可以執行為主控台應用程式如果您收到`--console`命令列引數，或是如果附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="52215-135">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="52215-136">停止和啟動的事件處理</span><span class="sxs-lookup"><span data-stu-id="52215-136">Handle stopping and starting events</span></span>

<span data-ttu-id="52215-137">如果您想要處理`OnStarting`， `OnStarted`，和`OnStopping`事件，進行下列的其他變更：</span><span class="sxs-lookup"><span data-stu-id="52215-137">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="52215-138">建立衍生自 `WebHostService` 的類別。</span><span class="sxs-lookup"><span data-stu-id="52215-138">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="52215-139">建立擴充方法給`IWebHost`傳遞之任何自訂`WebHostService`至`ServiceBase.Run`。</span><span class="sxs-lookup"><span data-stu-id="52215-139">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="52215-140">在`Program.Main`變更呼叫新的擴充方法，而不是`host.RunAsService`。</span><span class="sxs-lookup"><span data-stu-id="52215-140">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="52215-141">如果您的自訂`WebHostService`的程式碼需要從相依性插入 （例如記錄器） 取得服務，您可以從中取得從`Services`屬性`IWebHost`。</span><span class="sxs-lookup"><span data-stu-id="52215-141">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="52215-142">後續步驟</span><span class="sxs-lookup"><span data-stu-id="52215-142">Next steps</span></span>

<span data-ttu-id="52215-143">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample)伴隨著此發行項是簡單的 MVC web 應用程式已經修改上述程式碼範例所示。</span><span class="sxs-lookup"><span data-stu-id="52215-143">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="52215-144">若要在服務中執行它，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="52215-144">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="52215-145">發行至*c:\svc*。</span><span class="sxs-lookup"><span data-stu-id="52215-145">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="52215-146">開啟系統管理員 視窗。</span><span class="sxs-lookup"><span data-stu-id="52215-146">Open an administrator window.</span></span>

* <span data-ttu-id="52215-147">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="52215-147">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="52215-148">在瀏覽器，移至 http://localhost:5000/ 來確認正在執行。</span><span class="sxs-lookup"><span data-stu-id="52215-148">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="52215-149">如果應用程式未如預期的服務執行時啟動啟動，讓錯誤訊息都能存取快速方法是將記錄提供者，例如[Windows 事件記錄檔提供者](xref:fundamentals/logging/index#eventlog)。</span><span class="sxs-lookup"><span data-stu-id="52215-149">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="52215-150">通知</span><span class="sxs-lookup"><span data-stu-id="52215-150">Acknowledgments</span></span>

<span data-ttu-id="52215-151">撰寫本文時的協助的已發行的來源。</span><span class="sxs-lookup"><span data-stu-id="52215-151">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="52215-152">最早和最有用的是這些內容：</span><span class="sxs-lookup"><span data-stu-id="52215-152">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="52215-153">為 Windows 服務裝載的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52215-153">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="52215-154">如何在裝載您的 ASP.NET 核心 Windows 服務中</span><span class="sxs-lookup"><span data-stu-id="52215-154">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
