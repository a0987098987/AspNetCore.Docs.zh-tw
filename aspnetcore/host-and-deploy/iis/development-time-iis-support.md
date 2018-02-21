---
title: "Visual Studio for ASP.NET Core 中的開發階段 IIS 支援"
author: shirhatti
description: "探索 Windows 伺服器上執行 IIS 後方時，偵錯 ASP.NET Core 應用程式的支援。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core 中的開發階段 IIS 支援

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本文說明[Visual Studio](https://www.visualstudio.com/vs/)支援 Windows Server 上執行後 IIS 的 ASP.NET Core 應用程式偵錯。 本主題引導啟用此功能，以及設定專案。

## <a name="prerequisites"></a>必要條件

* Visual Studio (2017/版本 15.3 或更新版本)
* ASP.NET 與網頁程式開發工作負載「或」.NET Core 跨平台開發工作負載

## <a name="enable-iis"></a>啟用 IIS

啟用 IIS。 瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。 選取 [Internet Information Services] 核取方塊。

![Windows 功能會以黑色方塊 (而非勾選記號) 顯示選取的 [Internet Information Services] 核取方塊，表示已啟用部分 IIS 功能](development-time-iis-support/_static/enable_iis.png)

如果 IIS 安裝需要重新啟動，請重新啟動系統。

## <a name="enable-development-time-iis-support"></a>啟用開發階段 IIS 支援

啟動 Visual Studio 安裝程式。 選取**支援 IIS 的開發時間**元件。 元件會列在為選擇性**摘要** 面板的**ASP.NET 及 web 開發**工作負載。 這會安裝[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)，這是執行 ASP.NET Core 應用程式所需的原生 IIS 模組。

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。 在 [Web and Cloud]\(Web 與雲端\) 區段中，選取 [ASP.NET 與網頁程式開發] 面板。 在右側的選擇性摘要面板區域中，沒有核取方塊適用於 IIS 支援開發時間。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>設定專案

建立新的啟動設定檔，以新增開發階段 IIS 支援。 在 Visual Studio 的**方案總管**中，以滑鼠右鍵按一下專案，然後選取 [屬性]。 選取 [偵錯] 索引標籤。在 [啟動] 下拉式清單中選取 [IIS]。 請確認使用正確的 URL 啟用 [啟動瀏覽器] 功能。

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。 [設定檔] 與 [啟動] 的設定皆設為 [IIS]。 使用 http://localhost/WebApplication2 位址啟用 [啟動瀏覽器] 功能。 在 [網頁伺服器設定] 區域的 [應用程式 URL] 欄位也使用了相同的位址，並啟用了 [啟用匿名驗證]。](development-time-iis-support/_static/project_properties.png)

或者，手動新增至啟動設定檔[launchSettings.json](http://json.schemastore.org/launchsettings)應用程式中的檔案：

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

如果未以系統管理員身分執行 visual Studio 可能會提示重新啟動。 若收到提示，請重新啟動 Visual Studio。

## <a name="additional-resources"></a>其他資源

* [使用 IIS 在 Windows 上裝載 ASP.NET](xref:host-and-deploy/iis/index)
* [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)
