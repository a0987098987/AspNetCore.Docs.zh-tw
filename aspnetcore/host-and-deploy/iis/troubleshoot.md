---
title: 疑難排解在 IIS 上的 ASP.NET Core
author: guardrex
description: 了解如何診斷問題的網際網路資訊服務 (IIS) 的 ASP.NET Core 應用程式的部署。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e44892d2022ca1a176cee9d027e220e196c6572d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>疑難排解在 IIS 上的 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex)

本文章提供指示如何診斷 ASP.NET Core 應用程式啟動問題時裝載具有[網際網路資訊服務 (IIS)](/iis)。 本文章中的資訊適用於裝載在 IIS 上 Windows Server 和 Windows 桌面。

在 Visual Studio 中的 ASP.NET Core 專案預設值為[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)裝載在偵錯期間。 A *502.5 的處理序失敗*，發生於在本機偵錯可能會 troubleshooted 使用本主題中的建議。

其他的疑難排解主題：

[針對 Azure App Service 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/azure-apps/troubleshoot)  
雖然應用程式服務會使用[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)和 IIS 主控件應用程式，請參閱應用程式服務的特定指示的專用的主題。

[處理錯誤](xref:fundamentals/error-handling)  
了解如何在本機系統上的開發期間處理 ASP.NET Core 應用程式中的錯誤。

[了解使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger)  
本主題將介紹 Visual Studio 偵錯工具的功能。

## <a name="app-startup-errors"></a>應用程式啟動錯誤

**502.5 處理程序失敗**  
工作者處理序會失敗。 無法啟動應用程式。

ASP.NET 核心模組嘗試啟動背景工作處理序，但無法啟動。 處理序啟動失敗的原因通常可由中的項目[應用程式事件記錄檔](#application-event-log)和[ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)。

*502.5 的處理序失敗*裝載或應用程式設定不正確而造成工作者處理序失敗時傳回錯誤頁面：

![瀏覽器視窗中顯示 502.5 處理序失敗的頁面](troubleshoot/_static/process-failure-page.png)

**500 內部伺服器錯誤**  
應用程式啟動，但因為錯誤而造成伺服器無法完成要求。

在啟動期間，或建立回應時，應用程式的程式碼中會發生此錯誤。 回應可能會包含任何內容，或回應可能會顯示為*500 內部伺服器錯誤*瀏覽器中。 應用程式事件記錄檔通常會指出應用程式正常啟動。 從伺服器的觀點來看，這是正確。 應用程式沒有開始，但它無法產生有效的回應。 [在命令提示字元中執行應用程式](#run-the-app-at-a-command-prompt)在伺服器上或[啟用 ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)來疑難排解問題。

**連接重設**

如果會傳送標頭之後，就會發生錯誤，則晚伺服器傳送**500 內部伺服器錯誤**發生的錯誤。 這通常發生在回應複雜物件的序列化期間發生錯誤時。 這種類型的錯誤會顯示為*連接重設*用戶端上的錯誤。 [應用程式記錄](xref:fundamentals/logging/index)可協助疑難排解這些類型的錯誤。

## <a name="default-startup-limits"></a>預設啟動限制

預設值設定 ASP.NET 核心模組*startupTimeLimit*為 120 秒。 當保留預設值，應用程式可能需要兩分鐘的時間啟動之前模組記錄處理序失敗。 如需設定此模組的資訊，請參閱[aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

## <a name="troubleshoot-app-startup-errors"></a>疑難排解應用程式啟動錯誤

### <a name="application-event-log"></a>應用程式事件記錄檔

存取應用程式事件記錄檔：

1. 開啟 [開始] 功能表中，搜尋**事件檢視器**，然後選取**事件檢視器**應用程式。
1. 在**事件檢視器**，開啟**Windows 記錄檔**節點。
1. 選取**應用程式**開啟應用程式事件記錄檔。
1. 搜尋失敗應用程式相關聯的錯誤。 錯誤上面會有值為*IIS AspNetCore 模組*或*IIS Express AspNetCore 模組*中*來源*資料行。

### <a name="run-the-app-at-a-command-prompt"></a>在命令提示字元中執行應用程式

許多的啟動錯誤不會產生應用程式事件日誌中的有用資訊。 您可以在命令提示字元中執行應用程式，在主機系統上，以找出某些錯誤原因。

**Framework 相依的部署**

如果應用程式是[framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. 在命令提示字元中，巡覽至部署資料夾並執行應用程式藉由執行應用程式的組件*dotnet.exe*。 下列命令，以取代應用程式的組件名稱\<assembly_name >: `dotnet .\<assembly_name>.dll`。
1. 主控台從應用程式，並顯示任何錯誤，輸出會寫入至主控台視窗。
1. 如果應用程式提出要求時，就會發生錯誤，，提出要求的主機和 Kestrel 位置接聽的連接埠。 使用預設主控件和 post 要求提出要求來`http://localhost:5000/`。 如果應用程式通常回應 Kestrel 端點位址，此問題會更相關的反向 proxy 設定和比較不可能是應用程式內。

**獨立的部署**

如果應用程式是[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd):

1. 在命令提示字元中，瀏覽至部署資料夾，並執行應用程式的可執行檔。 下列命令，以取代應用程式的組件名稱\<assembly_name >: `<assembly_name>.exe`。
1. 主控台從應用程式，並顯示任何錯誤，輸出會寫入至主控台視窗。
1. 如果應用程式提出要求時，就會發生錯誤，，提出要求的主機和 Kestrel 位置接聽的連接埠。 使用預設主控件和 post 要求提出要求來`http://localhost:5000/`。 如果應用程式通常回應 Kestrel 端點位址，此問題會更相關的反向 proxy 設定和比較不可能是應用程式內。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET 核心模組 stdout 記錄檔

若要啟用，並檢視 stdout 記錄檔：

1. 瀏覽至主機系統上的站台的部署資料夾。
1. 如果*記錄*資料夾不存在、 建立資料夾。 如需如何啟用 MSBuild 的指示來建立*記錄*部署中的資料夾自動執行，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。
1. 編輯*web.config*檔案。 設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑以指向*記錄檔*資料夾 (例如， `.\logs\stdout`)。 `stdout` 在路徑中會記錄檔案名稱前置詞。 時間戳記、 處理序識別碼，以及檔案延伸模組會自動加入時建立記錄檔。 使用`stdout`一般記錄檔檔案名稱前置詞，以名為*stdout_20180205184032_5412.log*。 
1. 儲存已更新*web.config*檔案。
1. 應用程式提出要求。
1. 瀏覽至*記錄*資料夾。 尋找並開啟最近 stdout 記錄檔。
1. 研究記錄的錯誤。

**重要！** 停用 stdout 記錄完成疑難排解時。

1. 編輯*web.config*檔案。
1. 設定**stdoutLogEnabled**至`false`。
1. 儲存檔案。

> [!WARNING]
> 若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="enabling-the-developer-exception-page"></a>啟用 [開發人員的例外狀況] 頁面

`ASPNETCORE_ENVIRONMENT` [環境變數可以加入至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)開發環境中執行應用程式。 只要環境不覆寫中的應用程式啟動`UseEnvironment`主控件產生器中，設定環境變數可讓[開發人員例外狀況頁面](xref:fundamentals/error-handling)出現應用程式執行時。

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

設定環境變數，如`ASPNETCORE_ENVIRONMENT`只建議用於籌畫與測試不會被公開到網際網路的伺服器上使用。 移除環境變數從*web.config*疑難排解後的檔案。 如需有關設定環境變數詳細*web.config*，請參閱[aspNetCore environmentVariables 子項目](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。

## <a name="common-startup-errors"></a>常見的啟動錯誤 

請參閱[ASP.NET Core 常見錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)。 參考主題會說明最常見的問題阻礙應用程式啟動。

## <a name="slow-or-hanging-app"></a>較慢或無回應的應用程式

當應用程式回應變慢或停止回應的要求時，取得並分析[傾印檔案](/visualstudio/debugger/using-dump-files)。 您可以使用任何下列工具取得傾印檔案：

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg:[下載適用於 Windows 的偵錯工具](https://developer.microsoft.com/windows/hardware/download-windbg)，[偵錯使用 WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>遠端偵錯

請參閱[遠端偵錯 ASP.NET Core 上遠端 IIS 電腦在 Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio 文件中。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/)提供從裝載的 IIS，包括錯誤記錄和報告功能的應用程式的遙測。 Application Insights 只可以在應用程式啟動時的應用程式記錄功能會變成可用之後，會發生的錯誤報告。 如需詳細資訊，請參閱[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)。

## <a name="additional-troubleshooting-advice"></a>其他疑難排解建議

有時正常運作的應用程式後，無法立即升級其中一個.NET Core SDK 在應用程式內的開發電腦或套件版本。 在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。 遵循下列指示，就可以修正問題：

1. 刪除*bin*和*obj*資料夾。
1. 封裝在快取的清除*%userprofile%\\.nuget\\封裝*和*%localappdata%\\Nuget\\v3 快取*。
1. 還原並重建專案。
1. 請確認先前的部署在伺服器上有之前重新部署應用程式完全刪除。

> [!TIP]
> 清除封裝快取的便利方式是執行`dotnet nuget locals all --clear`從命令提示字元。
> 
> 清除封裝快取也可藉由使用[nuget.exe](https://www.nuget.org/downloads)工具並執行命令`nuget locals all -clear`。 *nuget.exe*未在 Windows 桌面作業系統配套的安裝，且必須取自分別[NuGet 網站](https://www.nuget.org/downloads)。

## <a name="additional-resources"></a>其他資源

* [ASP.NET Core 中的錯誤處理簡介](xref:fundamentals/error-handling)
* [Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)
* [針對 Azure App Service 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/azure-apps/troubleshoot)
