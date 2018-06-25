---
title: ASP.NET Core 模組
author: rick-anderson
description: 了解 ASP.NET Core 模組如何讓 Kestrel Web 伺服器將 IIS 或 IIS Express 作為反向 Proxy 伺服器使用。
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 319ab80f20f3917dae921f43566e368b51c29f0b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272331"
---
# <a name="aspnet-core-module"></a>ASP.NET Core 模組

作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl) 和 [Chris Ross](https://github.com/Tratcher) 

ASP.NET Core 模組可讓 ASP.NET Core 應用程式在反向 Proxy 組態中的 IIS 後方 執行。 IIS 提供進階的 Web 應用程式安全性和管理能力功能。

支援的 Windows 版本：

* Windows 7 或更新版本
* Windows Server 2008 R2 或更新版本

ASP.NET Core 模組只適用於 Kestrel。 該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不相容。

## <a name="aspnet-core-module-description"></a>ASP.NET Core 模組說明

ASP.NET Core 模組是一種原生 IIS 模組，可連到 IIS 管線，將 Web 要求重新導向至後端的 ASP.NET Core 應用程式。 許多如 Windows 驗證等原生模組仍在使用中。 若要深入了解搭配此模組的使用中 IIS 模組，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)。

因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。 此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。 此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。

下圖說明 IIS、ASP.NET Core 模組和 ASP.NET Core 應用程式之間的關聯性：

![ASP.NET Core 模組](aspnet-core-module/_static/ancm.png)

要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。 驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。 此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，這並不是通訊埠 80/443。

此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。 將會執行額外檢查，不是源自模組的要求都會遭到拒絕。 此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。

Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。 中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。 應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。

ASP.NET Core 模組具有一些其他功能。 此模組可以：

* 設定背景工作處理序的環境變數。
* 將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。
* 轉送 Windows 驗證權杖。

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>如何安裝和使用 ASP.NET Core 模組

如需如何安裝和使用 ASP.NET Core 模組的詳細指示，請參閱 [Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)。 如需設定模組的詳細資訊，請參閱 [ASP.NET Core 模組的組態參考](xref:host-and-deploy/aspnet-core-module)。

## <a name="additional-resources"></a>其他資源

* [ Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)
* [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET Core 模組 GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/AspNetCoreModule)
