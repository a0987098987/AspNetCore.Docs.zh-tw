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
# <a name="host-aspnet-core-in-a-windows-service"></a>在 Windows 服務上裝載 ASP.NET Core

作者：[Tom Dykstra](https://github.com/tdykstra)

在不使用 IIS 的情況下，若要在 Windows 上裝載 ASP.NET Core 應用程式，建議的方法是在 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)中執行該應用程式。 以 Windows 服務的形式裝載時，應用程式可以在重新開機和當機後自動啟動，而無須人為介入。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。 如需有關如何執行範例應用程式的指示，請參閱範例的 *README.md* 檔案。

## <a name="prerequisites"></a>必要條件

* 應用程式必須在 .NET Framework 執行階段上執行。 在 *.csproj* 檔案中，為 [TargetFramework](/nuget/schema/target-frameworks) 和 [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) 指定適當的值。 以下為範例：

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  在 Visual Studio 中建立專案時，請使用 [ASP.NET Core 應用程式 (.NET Framework)] 範本。

* 如果應用程式會從網際網路 (而不僅僅從內部網路) 接收要求，它就必須使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 網頁伺服器 (以前稱為 [WebListener](xref:fundamentals/servers/weblistener)；適用於 ASP.NET Core 1.x 應用程式)，而不是使用 [Kestrel](xref:fundamentals/servers/kestrel)。 針對邊緣部署，建議使用 IIS 作為與 Kestrel 搭配運作的反向 Proxy 伺服器。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。

## <a name="get-started"></a>開始使用

本節說明設定現有 ASP.NET Core 專案以在服務中執行所需的最基本變更。

1. 安裝 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) NuGet 套件。

2. 在 `Program.Main` 中進行下列變更：

   * 呼叫 `host.RunAsService`，而不要呼叫 `host.Run`。

   * 如果程式碼會呼叫 `UseContentRoot`，請使用發佈位置路徑，而不要使用 `Directory.GetCurrentDirectory()`。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. 將應用程式發佈至資料夾。 使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) 或會發佈至資料夾的 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)。

4. 建立並啟動服務來進行測試。

   使用系統管理權限來開啟命令殼層，以使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具來建立並啟動服務。 如果服務是命名為 MyService、發佈至 `c:\svc`，然後命名為 AspNetCoreService，則命令為：

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   `binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。

   ![主控台視窗建立和啟動範例](windows-service/_static/create-start.png)

   當這些命令完成時，請瀏覽至以主控台應用程式形式執行時的相同路徑 (預設為 `http://localhost:5000`)：

   ![在服務中執行](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>提供一個在服務外執行的方式

在服務外執行時較容易進行測試和偵錯，因此習慣上只有在特定情況下，才會新增呼叫 `RunAsService` 的程式碼。 例如，使用 `--console` 命令列引數或連結偵錯工具，即可讓應用程式以主控台應用程式的形式執行：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>處理停止和啟動事件

若要處理 `OnStarting`、`OnStarted` 及 `OnStopping` 事件，請進行下列額外的變更：

1. 建立衍生自 `WebHostService` 的類別：

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. 為 `IWebHost` 建立一個會將自訂 `WebHostService` 傳遞給 `ServiceBase.Run`的擴充方法：

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. 在 `Program.Main` 中，呼叫新的擴充方法 `RunAsCustomService`，而不是呼叫 `RunAsService`：

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

如果自訂的 `WebHostService` 程式碼需要一個來自相依性插入的服務 (例如記錄器)，請從 `IWebHost` 的 `Services` 屬性取得該服務：

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

## <a name="acknowledgments"></a>感謝

撰寫本文時借助了下列已發佈的來源：

* [以 Windows 服務的形式裝載 ASP.NET Core](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) \(英文\)
* [如何在 Windows 服務中裝載 ASP.NET Core](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) \(英文\)
