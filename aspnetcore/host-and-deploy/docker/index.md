---
title: 在 Docker 容器中裝載 ASP.NET Core
author: rick-anderson
description: 探索資源連結以了解如何在 Docker 容器中裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: cb5f774db5fab46a57f8ca4bbbca148f20f371ba
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664077"
---
# <a name="host-aspnet-core-in-docker-containers"></a>在 Docker 容器中裝載 ASP.NET Core

下列文章可用來了解如何在 Docker 中裝載 ASP.NET Core 應用程式：

[容器和 Docker 簡介](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
了解容器化是一種軟體開發方法，在此方法中，應用程式或服務、其相依性及其組態會封裝在一起，成為一個容器映像。 映像可進行測試並部署到主機。

[什麼是 Docker？](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
探索 Docker 開放原始碼專案，將應用程式自動化部署為可攜式且可自足的容器，在雲端或內部部署上執行。

[Docker 術語](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
了解 Docker 技術的詞彙和定義。

[Docker 容器、影像和登錄](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
了解 Docker 容器映像儲存在映像登錄的方式，取得跨環境部署的一致性。

<xref:host-and-deploy/docker/building-net-docker-images> 了解如何建置和 Docker 化 ASP.NET Core 應用程式。 瀏覽 Microsoft 維護的 Docker 映像，並檢查使用案例。

[Visual Studio 容器工具](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
探索 Visual Studio 如何在 Docker for Windows 上支援建置、偵錯和執行以 .NET Framework 或 .NET Core 為目標的 ASP.NET Core 應用程式。 同時支援 Windows 和 Linux 容器。

[發佈到 Azure Container Registry](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
了解如何使用 Visual Studio Container Tools 延伸模組，透過PowerShell 將 ASP.NET Core 應用程式部署到 Azure 上的 Docker 主機。

[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)  
Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。 透過 Proxy 傳遞要求通常會遮住原始要求的相關資訊，例如配置和用戶端 IP。 可能必須以手動方式將一些關於要求的資訊轉送至應用程式。
