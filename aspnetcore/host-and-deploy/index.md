---
title: "裝載及部署 ASP.NET Core"
author: tdykstra
description: "了解如何設定裝載環境及部署 ASP.NET Core 應用程式。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: baa77eba837ff8b86ad543a74ebeee51ace4c25d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="host-and-deploy-aspnet-core"></a>裝載及部署 ASP.NET Core

一般是將 ASP.NET Core 應用程式部署到裝載環境：

* 將應用程式發行至裝載伺服器上的資料夾。
* 設定處理序管理員，以在要求到達時啟動應用程式，並在其損毀或伺服器重新開機後重新啟動應用程式。
* 設定反向 Proxy，以將要求轉送至應用程式。

## <a name="publish-to-a-folder"></a>發行至資料夾 

[dotnet publish](/dotnet/articles/core/tools/dotnet-publish) CLI 命令會編譯應用程式程式碼，並將執行應用程式所需的檔案複製到 *publish* 資料夾。 從 Visual Studio 部署時，[dotnet publish](/dotnet/core/tools/dotnet-publish) 步驟會在檔案複製到部署目的地之前自動完成。

### <a name="folder-contents"></a>資料夾內容

*publish* 資料夾包含應用程式、其相依性和選擇性 .NET 執行階段的 *.exe* 和 *.dll* 檔案。

.NET Core 應用程式可以發行為「獨立式」或「與 Framework 相依」的應用程式。 如果應用程式是獨立應用程式，包含 .NET 執行階段的 *.dll* 檔案將包含在 *publish* 資料夾中。 如果應用程式是與 Framework 相依的應用程式，則不會包含 .NET 執行階段檔案，因為應用程式具有對伺服器上已安裝之 .NET 版本的參考。 預設部署模式是與 Framework 相依。 如需詳細資訊，請參閱 [.NET Core 應用程式部署](/dotnet/articles/core/deploying/index)。

除了 *.exe* 和 *.dll* 檔案之外，ASP.NET Core 應用程式的 *publish* 資料夾通常還包含組態檔、靜態資產和 MVC 檢視。 如需詳細資訊，請參閱[目錄結構](xref:host-and-deploy/directory-structure)。

## <a name="set-up-a-process-manager"></a>設定處理序管理員

ASP.NET Core 應用程式是一種主控台應用程式，必須在伺服器開機和損毀後重新啟動。 若要自動化啟動和重新啟動，需要有處理序管理員。 ASP.NET Core 最常見的處理序管理員是：

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows 服務](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>設定反向 Proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器，反向 Proxy 伺服器可用 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache) 或 [IIS](xref:host-and-deploy/iis/index)。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器且將公開至網際網路，反向 Proxy 伺服器則用 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)或 [IIS](xref:host-and-deploy/iis/index)。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。 使用反向 Proxy 的主要原因是安全性。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)。

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>使用 Visual Studio 和 MSBuild 來自動化部署

除了從 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將輸出複製到伺服器之外，部署通常還需要額外的工作。 例如，*publish* 資料夾可能需要或排除額外的檔案。 Visual Studio 會將 MSBuild 用於 Web 部署，而且您可以自訂 MSBuild 在部署期間執行許多其他工作。 如需詳細資訊，請參閱[在 Visual Studio 中發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)和[使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com/) 書籍。

使用[發行 Web 功能](xref:tutorials/publish-to-azure-webapp-using-vs)或使用[內建的 Git 支援](xref:host-and-deploy/azure-apps/azure-continuous-deployment)，應用程式可以直接從 Visual Studio 部署至 Azure App Service。 Visual Studio Team Services 支援[持續部署至 Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts)。

## <a name="publishing-to-azure"></a>發行至 Azure

如需使用 Visual Studio 將應用程式發行到 Azure 的指示，請參閱[使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。 您也可以透過[命令列](xref:tutorials/publish-to-azure-webapp-using-cli)，將應用程式發行至 Azure。

## <a name="additional-resources"></a>其他資源

如需使用 Docker 作為裝載環境的資訊，請參閱[在 Docker 中裝載 ASP.NET Core 應用程式](xref:host-and-deploy/docker/index)。
