---
title: ASP.NET Core 目錄結構
author: rick-anderson
description: 了解已發行之 ASP.NET Core 應用程式的目錄結構。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2020
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 3ed27db31c5fda8376ed108568ea74b657d30531
ms.sourcegitcommit: 4506a8f71ece921010ad6b7edebc8b200618f40d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/09/2020
ms.locfileid: "81002934"
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core 目錄結構

::: moniker range=">= aspnetcore-3.0"

*publish* 目錄包含 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令所產生的應用程式可部署資產。 此目錄包含：

* 應用程式檔案
* 組態檔
* 靜態資產
* Packages
* 執行階段 (僅限[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd))

| 應用程式類型 | 目錄結構 |
| -------- | ------------------- |
| [與框架相關的執行 (FDE)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>publish&dagger;<ul><li>查看&dagger;MVC 應用;如果檢視未預寫</li><li>頁面&dagger;MVC 或 Razor 頁面應用(如果頁面未預編譯)</li><li>wwwroot&dagger;</li><li>\*.dll 檔案</li><li>{組件名稱}.deps.json</li><li>{組件名稱}.dll</li><li>【程式集名稱]{.延伸* .exe 延伸在 Windows 上,macOS 或 Linux 上沒有擴展</li><li>{組件名稱}.pdb</li><li>{組件名稱}.Views.dll</li><li>{組件名稱}.Views.pdb</li><li>{組件名稱}.runtimeconfig.json</li><li>web.config (IIS 部署)</li><li>建立傾印([Linux 建立轉印實用程式](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li><li>\*.所以(Linux 共用物件庫)</li><li>\*.a(macOS 存檔)</li><li>\*.dylib(macOS 動態庫)</li></ul></li></ul> |
| [自包含部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>檢視&dagger;MVC 應用程式(如果檢視未預編譯)</li><li>頁面&dagger;MVC 或 Razor 頁面應用(如果頁面未預編譯)</li><li>wwwroot&dagger;</li><li>\*.dll 檔案</li><li>{組件名稱}.deps.json</li><li>{組件名稱}.dll</li><li>{組件名稱}.exe</li><li>{組件名稱}.pdb</li><li>{組件名稱}.Views.dll</li><li>{組件名稱}.Views.pdb</li><li>{組件名稱}.runtimeconfig.json</li><li>web.config (IIS 部署)</li></ul></li></ul> |

&dagger;表示是目錄

*publish* 目錄代表部署的「內容根目錄路徑」**(也稱為「應用程式基底路徑」**)。 不論給予伺服器上所部署應用程式的 *publish* 目錄什麼名稱，其位置都會作為所裝載應用程式的伺服器實體路徑。

*wwwroot* 目錄 (如果存在) 只包含靜態資產。

## <a name="additional-resources"></a>其他資源

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [.NET 核心應用程式部署](/dotnet/core/deploying/)
* [目標 Framework](/dotnet/standard/frameworks)
* [.NET Core RID 類別目錄](/dotnet/core/rid-catalog)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*publish* 目錄包含 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令所產生的應用程式可部署資產。 此目錄包含：

* 應用程式檔案
* 組態檔
* 靜態資產
* Packages
* 執行階段 (僅限[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd))

| 應用程式類型 | 目錄結構 |
| -------- | ------------------- |
| [與框架相關的執行 (FDE)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>publish&dagger;<ul><li>查看&dagger;MVC 應用;如果檢視未預寫</li><li>頁面&dagger;MVC 或 Razor 頁面應用(如果頁面未預編譯)</li><li>wwwroot&dagger;</li><li>\*.dll 檔案</li><li>{組件名稱}.deps.json</li><li>{組件名稱}.dll</li><li>【程式集名稱]{.延伸* .exe 延伸在 Windows 上,macOS 或 Linux 上沒有擴展</li><li>{組件名稱}.pdb</li><li>{組件名稱}.Views.dll</li><li>{組件名稱}.Views.pdb</li><li>{組件名稱}.runtimeconfig.json</li><li>web.config (IIS 部署)</li><li>建立傾印([Linux 建立轉印實用程式](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li><li>\*.所以(Linux 共用物件庫)</li><li>\*.a(macOS 存檔)</li><li>\*.dylib(macOS 動態庫)</li></ul></li></ul> |
| [自包含部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>檢視&dagger;MVC 應用程式(如果檢視未預編譯)</li><li>頁面&dagger;MVC 或 Razor 頁面應用(如果頁面未預編譯)</li><li>wwwroot&dagger;</li><li>\*.dll 檔案</li><li>{組件名稱}.deps.json</li><li>{組件名稱}.dll</li><li>{組件名稱}.exe</li><li>{組件名稱}.pdb</li><li>{組件名稱}.Views.dll</li><li>{組件名稱}.Views.pdb</li><li>{組件名稱}.runtimeconfig.json</li><li>web.config (IIS 部署)</li></ul></li></ul> |

&dagger;表示是目錄

*publish* 目錄代表部署的「內容根目錄路徑」**(也稱為「應用程式基底路徑」**)。 不論給予伺服器上所部署應用程式的 *publish* 目錄什麼名稱，其位置都會作為所裝載應用程式的伺服器實體路徑。

*wwwroot* 目錄 (如果存在) 只包含靜態資產。

建立 *Logs* 資料夾對 [ASP.NET Core 模組增強型偵錯記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)很有用。 模組不會在提供給 `<handlerSetting>` 值的路徑中自動建立資料夾，這些資料夾應該已存在於部署中，模組才能寫入偵錯記錄。

使用下列兩種方法其中之一，即可為部署建立 *Logs* 目錄：

* 將下列 `<Target>` 元素新增至專案檔：

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   `<MakeDir>` 元素會在所發佈的輸出中建立一個空的 [Logs]** 資料夾。 此元素會使用 `PublishDir` 屬性來判斷用於建立資料夾的目標位置。 數個部署方法 (例如 Web Deploy) 在部署期間都會略過空的資料夾。 `<WriteLinesToFile>` 元素會在 [Logs]** 資料夾中產生檔案，用以確保將資料夾部署至伺服器。 如果背景工作處理序沒有目標資料夾的寫入權限，則使用此方法建立資料夾時會失敗。

* 在部署中的伺服器上實體建立 *Logs* 目錄。

部署目錄會要求「讀取/執行」權限。 *Logs* 目錄會要求「讀取/寫入」權限。 其他可供寫入檔案的目錄會要求「讀取/寫入」權限。

## <a name="additional-resources"></a>其他資源

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [.NET 核心應用程式部署](/dotnet/core/deploying/)
* [目標 Framework](/dotnet/standard/frameworks)
* [.NET Core RID 類別目錄](/dotnet/core/rid-catalog)

::: moniker-end
