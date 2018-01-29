---
title: "在 Windows 服務的主機"
author: tdykstra
description: "了解如何裝載 ASP.NET Core 應用程式在 Windows 服務。"
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 03/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4bf64f54dd9c6d2a1706bd405b0d6d75d7573a7a
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>裝載在 Windows 服務的 ASP.NET Core 應用程式

由[Tom Dykstra](https://github.com/tdykstra)

裝載在 Windows 上的 ASP.NET Core 應用程式，而不使用 IIS 執行它的建議的方式[Windows 服務](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications)。 如此一來它可以自動啟動之後重新開機和損毀，而不需等待有人登入。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))。 請參閱[接下來的步驟](#next-steps)> 一節，如需如何執行此程式碼的指示。

## <a name="prerequisites"></a>必要條件

* 應用程式必須執行.NET Framework 執行階段。  在*.csproj*檔案中，指定適當的值[TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks)和[RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog)。 以下為範例：

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  使用 Visual Studio 中建立專案時**ASP.NET Core 應用程式 (.NET Framework)**範本。

* 如果應用程式接收要求，從網際網路 （不只是從內部網路），則必須使用[WebListener](xref:fundamentals/servers/weblistener) web 伺服器而非[Kestrel](xref:fundamentals/servers/kestrel)。  Kestrel 必須搭配 IIS 邊緣部署。  如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。

## <a name="getting-started"></a>使用者入門

本節說明將現有的 ASP.NET Core 專案設定為在服務執行所需的最小變更。

* 安裝 NuGet 套件[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)。

* 在變更下列`Program.Main`:
  
  * 呼叫`host.RunAsService`而不是`host.Run`。
  
  * 如果程式碼會呼叫`UseContentRoot`，到發行位置，而不是使用的路徑`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* 發行應用程式的資料夾。

  使用[dotnet 發行](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish)或[Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)所發行的資料夾。

* 測試建立和啟動服務。

  開啟要使用系統管理員命令提示字元視窗[sc.exe](https://technet.microsoft.com/library/bb490995)命令列工具來建立及啟動服務。  
  
  如果服務為 MyService，應用程式發行到`c:\svc`，而且應用程式本身名為 AspNetCoreService，命令看起來像這樣：

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  `binPath`值是應用程式的可執行檔，包括可執行檔的檔案名稱本身的路徑。

  ![主控台視窗建立及啟動範例](windows-service/_static/create-start.png)

  當命令完成時，瀏覽至與相同的路徑做為主控台應用程式執行時 (根據預設， `http://localhost:5000`)

  ![在服務中執行](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>提供服務外部執行的方式

更輕鬆地測試和偵錯時執行外部服務，所以可將呼叫的程式碼加入`host.RunAsService`只在某些情況下。  例如，應用程式可以執行為主控台應用程式與`--console`命令列引數，或是如果附加偵錯工具。

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>停止和啟動的事件處理

若要處理`OnStarting`， `OnStarted`，和`OnStopping`事件，進行下列的其他變更：

* 建立衍生自 `WebHostService` 的類別。

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* 建立擴充方法給`IWebHost`傳遞之任何自訂`WebHostService`至`ServiceBase.Run`。

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* 在`Program.Main`變更呼叫新的擴充方法，而不是`host.RunAsService`。

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

如果自訂`WebHostService`的程式碼需要從相依性插入 （例如記錄器） 取得服務，請將它從`Services`屬性`IWebHost`。

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>後續步驟

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)伴隨著此發行項是簡單的 MVC web 應用程式已經修改上述程式碼範例所示。  若要在服務中執行它，請執行下列步驟：

* 發行至*c:\svc*。

* 開啟系統管理員 視窗。

* 輸入下列命令：

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * 在瀏覽器，移至 http://localhost:5000/ 來確認正在執行。

如果應用程式未如預期的服務執行時啟動啟動，讓錯誤訊息都能存取快速方法是將記錄提供者，例如[Windows 事件記錄檔提供者](xref:fundamentals/logging/index#eventlog)。

## <a name="acknowledgments"></a>通知

撰寫本文時的協助的已發行的來源。 最早和最有用的是這些內容：

* [為 Windows 服務裝載的 ASP.NET Core](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [如何在裝載您的 ASP.NET 核心 Windows 服務中](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
