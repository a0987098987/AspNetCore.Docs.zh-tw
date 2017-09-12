---
title: "裝載和部署概觀 - ASP.NET Core"
author: tdykstra
description: "如何設定裝載環境，並將 ASP.NET Core 應用程式部署到這些環境的概觀。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: df3c1f0c2768b89c3ea5dc901782170c530a542e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>ASP.NET Core 應用程式的裝載和部署概觀

以下是您將 ASP.NET Core 應用程式部署到裝載環境時執行的主要步驟：

* 將應用程式發行至裝載伺服器上的資料夾。
* 設定處理序管理員，以在要求傳入時啟動應用程式，並在其損毀或伺服器重新開機後重新啟動。
* 設定反向 Proxy，以將要求轉送至應用程式。

## <a name="publish-to-a-folder"></a>發行至資料夾 

[dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI 命令會編譯應用程式程式碼，並將執行應用程式所需的檔案複製到 *publish* 資料夾。 當您從 Visual Studio 部署時，會在檔案複製到部署目的地之前，為您自動完成 `dotnet publish` 步驟。

### <a name="folder-contents"></a>資料夾內容

*publish* 資料夾包含應用程式、其相依性和選擇性 .NET 執行階段的 *.exe* 和 *.dll* 檔案。

.NET Core 應用程式可以發行為*獨立*或 *與 Framework 相依的*應用程式。 如果應用程式是獨立應用程式，包含 .NET 執行階段的 *.dll* 檔案將包含在 *publish* 資料夾中。  如果應用程式是與 Framework 相依的應用程式，則不會包含 .NET 執行階段檔案，因為應用程式具有對電腦上已安裝之 .NET 版本的參考。 預設部署模式是與 Framework 相依。 如需詳細資訊，請參閱 [.NET Core 應用程式部署](https://docs.microsoft.com/dotnet/articles/core/deploying/index)。

除了 *.exe* 和 *.dll* 檔案之外，ASP.NET Core 應用程式的 *publish* 資料夾通常還包含組態檔、靜態資產和 MVC 檢視。  如需詳細資訊，請參閱[目錄結構](xref:hosting/directory-structure)。

## <a name="set-up-a-process-manager"></a>設定處理序管理員

ASP.NET Core 應用程式是一種主控台應用程式，必須在伺服器開機和損毀後重新啟動時啟動此應用程式。 若要自動啟動和重新啟動，您需要有處理序管理員。 ASP.NET Core 最常見的處理序管理員是 Linux 上的 [Nginx](xref:publishing/linuxproduction) 和 [Apache](xref:publishing/apache-proxy)，以及 Windows 上的 [IIS](xref:publishing/iis) 和 [Windows 服務](xref:hosting/windows-service)。

## <a name="set-up-a-reverse-proxy"></a>設定反向 Proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器，您可以使用 [Nginx](xref:publishing/linuxproduction)、[Apache](xref:publishing/apache-proxy) 或 [IIS](xref:publishing/iis) 作為反向 Proxy 伺服器。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器，而且將公開至網際網路，您必須使用 [Nginx](xref:publishing/linuxproduction)、 [Apache](xref:publishing/apache-proxy) 或 [IIS](xref:publishing/iis) 作為反向 Proxy 伺服器。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。 使用反向 Proxy 的主要原因是安全性。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)。

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>使用 Visual Studio 和 MSBuild 來自動化部署

除了將輸出從 `dotnet publish` 複製到伺服器之外，部署通常還需要額外的工作。 例如，您可以將額外檔案包含在 *publish* 資料夾，也可以從中排除檔案。 Visual Studio 會將 MSBuild 用於 Web 部署，而且您可以自訂 MSBuild，以便在部署期間執行許多其他工作。 如需詳細資訊，請參閱[在 Visual Studio 中發行設定檔](xref:publishing/web-publishing-vs)和[使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com/) 書籍。

您可以使用[發行 Web 功能](xref:tutorials/publish-to-azure-webapp-using-vs)或使用[內建的 Git 支援](xref:publishing/azure-continuous-deployment)，直接從 Visual Studio 部署至 Azure App Service。 Visual Studio Team Services 支援[持續部署至 Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)。

## <a name="additional-resources"></a>其他資源

如需使用 Docker 作為裝載環境的詳細資訊，請參閱[在 Docker 中裝載 ASP.NET Core 應用程式](xref:publishing/docker)。
