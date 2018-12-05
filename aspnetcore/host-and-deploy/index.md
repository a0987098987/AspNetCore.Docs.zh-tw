---
title: 裝載及部署 ASP.NET Core
author: guardrex
description: 了解如何設定裝載環境及部署 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/index
ms.openlocfilehash: 86022c33a3c5a8b82b14ae51b98c44497f39bd16
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862443"
---
# <a name="host-and-deploy-aspnet-core"></a>裝載及部署 ASP.NET Core

一般是將 ASP.NET Core 應用程式部署到裝載環境：

* 將已發行的電子郵件部署到裝載伺服器上的資料夾。
* 設定處理序管理員，以在要求到達時啟動應用程式，並在其損毀或伺服器重新開機後重新啟動應用程式。
* 至於反向 Proxy 的設定，請對反向 Proxy 進行設定，使其將要求轉送到應用程式。

## <a name="publish-to-a-folder"></a>發行至資料夾

[dotnet publish](/dotnet/core/tools/dotnet-publish) 命令會編譯應用程式程式碼，並將執行應用程式所需的檔案複製到 *publish* 資料夾。 從 Visual Studio 部署時，`dotnet publish` 步驟會在檔案複製到部署目的地之前自動執行。

### <a name="folder-contents"></a>資料夾內容

*publish* 資料夾包含一或多個應用程式組件檔、相依性，也可能會有 .NET 執行階段。

.NET Core 應用程式可以發行為「自主式部署」或「相依於架構的部署」。 如果應用程式是自主式，包含 .NET 執行階段的組件檔會包含在 *publish* 資料夾中。 如果應用程式是與 Framework 相依的應用程式，則不會包含 .NET 執行階段檔案，因為應用程式具有對伺服器上已安裝之 .NET 版本的參考。 預設部署模式是與 Framework 相依。 如需詳細資訊，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。

除了 *.exe* 和 *.dll* 檔案之外，ASP.NET Core 應用程式的 *publish* 資料夾通常還包含組態檔、靜態資產和 MVC 檢視。 如需詳細資訊，請參閱<xref:host-and-deploy/directory-structure>。

## <a name="set-up-a-process-manager"></a>設定處理序管理員

ASP.NET Core 應用程式是一種主控台應用程式，必須在伺服器開機和損毀後重新啟動。 若要自動化啟動和重新啟動，需要有處理序管理員。 ASP.NET Core 最常見的處理序管理員是：

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows 服務](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>設定反向 Proxy

::: moniker range=">= aspnetcore-2.0"

如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，反向 Proxy 伺服器可用 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache) 或 [IIS](xref:host-and-deploy/iis/index)。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。

不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式來說，都是支援的裝載設定。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器並會向網際網路公開，則使用 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)或 [IIS](xref:host-and-deploy/iis/index) 當作反向 Proxy 伺服器。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。 使用反向 Proxy 的主要原因是安全性。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)。

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。 若沒有其他設定，應用程式可能無法存取配置 (HTTP/HTTPS) 和發出要求的遠端 IP 位址。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a>使用 Visual Studio 和 MSBuild 來自動化部署

除了從 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將輸出複製到伺服器之外，部署通常還需要額外的工作。 例如，*publish* 資料夾可能需要或排除額外的檔案。 Visual Studio 會將 MSBuild 用於 Web 部署，而且您可以自訂 MSBuild 在部署期間執行許多其他工作。 如需詳細資訊，請參閱 <xref:host-and-deploy/visual-studio-publish-profiles>和[使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com/) 書籍。

使用[發行 Web 功能](xref:tutorials/publish-to-azure-webapp-using-vs)或使用[內建的 Git 支援](xref:host-and-deploy/azure-apps/azure-continuous-deployment)，應用程式可以直接從 Visual Studio 部署至 Azure App Service。 Azure DevOps Services 支援[持續部署至 Azure App Service](/azure/devops/pipelines/targets/webapp)。 如需詳細資訊，請參閱 [ASP.NET Core 與 Azure 的 DevOps](xref:azure/devops/index)。

## <a name="publish-to-azure"></a>發佈至 Azure

請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs> 以取得有關如何使用 Visual Studio 將應用程式發佈到 Azure 的指示。 您也可以透過[命令列](/azure/app-service/app-service-web-get-started-dotnet)，將應用程式發行至 Azure。

## <a name="host-in-a-web-farm"></a>裝載於 Web 伺服陣列

如需在 Web 伺服陣列環境中裝載 ASP.NET Core 應用程式的設定資訊 (例如部署應用程式的多個執行個體以獲得調整能力)，請參閱 <xref:host-and-deploy/web-farm>。

::: moniker range=">= aspnetcore-2.2"

## <a name="perform-health-checks"></a>執行健康狀態檢查

您可以使用健康狀態檢查中介軟體，對應用程式及其相依性執行健康狀態檢查。 如需詳細資訊，請參閱<xref:host-and-deploy/health-checks>。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
