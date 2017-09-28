---
title: "Visual Studio for ASP.NET Core 中的開發階段 IIS 支援"
author: shirhatti
description: "探索在 Windows Server 上的 IIS 背後執行時，ASP.NET Core 應用程式的偵錯支援。"
keywords: "ASP.NET Core, Internet Information Services, IIS, Windows Server, ASP.NET Core Module, 偵錯"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core 中的開發階段 IIS 支援

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本文說明在 Windows Server 上的 IIS 背後執行時，ASP.NET Core 應用程式的 [Visual Studio](https://www.visualstudio.com/vs/) 偵錯支援。 本主題會逐步引導您啟用這項功能，以及設定專案。

## <a name="prerequisites"></a>必要條件

* Visual Studio (2017/版本 15.3 或更新版本)
* ASP.NET 與網頁程式開發工作負載「或」.NET Core 跨平台開發工作負載

## <a name="enable-iis"></a>啟用 IIS

在系統上啟用 IIS。 瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] \(畫面左側)。 選取 [Internet Information Services] 核取方塊。

![Windows 功能會以黑色方塊 (而非勾選記號) 顯示選取的 [Internet Information Services] 核取方塊，表示已啟用部分 IIS 功能](development-time-iis-support/_static/enable_iis.png)

若您的 IIS 安裝需要重新開機，請重新啟動系統。

## <a name="enable-development-time-iis-support"></a>啟用開發階段 IIS 支援

安裝 IIS 後，請啟動 Visual Studio 安裝程式來修改現有的 Visual Studio 安裝。 在安裝程式中，選取 [開發階段 IIS 支援] 元件。 該元件會在 [ASP.NET 與網頁程式開發] 工作負載的 [摘要] 面板中，列為選擇性元件。 這會安裝 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module)，這是執行 ASP.NET Core 應用程式的必要原生 IIS 模組。

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。 在 [Web and Cloud]\(Web 與雲端\) 區段中，選取 [ASP.NET 與網頁程式開發] 面板。 在右側 [摘要] 面板的 [選擇性] 區域中，有 [開發階段 IIS 支援] 的核取方塊。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>設定專案

建立新的啟動設定檔，以新增開發階段 IIS 支援。 在 Visual Studio 的**方案總管**中，以滑鼠右鍵按一下專案，然後選取 [屬性]。 選取 [偵錯] 索引標籤。在 [啟動] 下拉式清單中選取 [IIS]。 請確認使用正確的 URL 啟用 [啟動瀏覽器] 功能。

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。 [設定檔] 與 [啟動] 的設定皆設為 [IIS]。 使用 http://localhost/WebApplication2 位址啟用 [啟動瀏覽器] 功能。 在 [網頁伺服器設定] 區域的 [應用程式 URL] 欄位也使用了相同的位址，並啟用了 [啟用匿名驗證]。](development-time-iis-support/_static/project_properties.png)

或者，您也可以手動將啟動設定檔新增至應用程式中的 [launchSettings.json](http://json.schemastore.org/launchsettings) 檔案：

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

若您不是以系統管理員的身分執行，系統可能會提示您重新啟動 Visual Studio。 若收到提示，請重新啟動 Visual Studio。

恭喜您！ 您已成功為專案設定開發階段 IIS 支援。 

## <a name="additional-resources"></a>其他資源

* [使用 IIS 在 Windows 上裝載 ASP.NET](xref:publishing/iis)
* [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)
