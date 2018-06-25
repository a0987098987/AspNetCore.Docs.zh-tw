---
title: ASP.NET Core 目錄結構
author: guardrex
description: 了解已發行之 ASP.NET Core 應用程式的目錄結構。
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 06d3f097cd93ceb2a23b9f6516a9b7a1f3ca3089
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273669"
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core 目錄結構

作者：[Luke Latham](https://github.com/guardrex)

在 ASP.NET Core 中，所發佈的應用程式目錄 *publish* 會由應用程式檔案、設定檔、靜態資產、套件及執行階段 (適用於[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)) 所組成。


| 應用程式類型 | 目錄結構 |
| -------- | ------------------- |
| [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</li><li>Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</li><li>Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</li><li>wwwroot&dagger;</li><li>*\.dll 檔案</li><li>\<assembly-name>.deps.json</li><li>\<assembly-name>.dll</li><li>\<assembly-name>.pdb</li><li>\<assembly-name>.PrecompiledViews.dll</li><li>\<assembly-name>.PrecompiledViews.pdb</li><li>\<assembly-name>.runtimeconfig.json</li><li>web.config (IIS 部署)</li></ul></li></ul> |
| [自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (選擇性，除非必須用來接收 stdout 記錄檔)</li><li>refs&dagger;</li><li>Views&dagger; (MVC 應用程式；如果未預先編譯檢視)</li><li>Pages&dagger; (MVC 或 Razor 頁面應用程式；如果未預先編譯頁面)</li><li>wwwroot&dagger;</li><li>\*.dll 檔案</li><li>\<assembly-name>.deps.json</li><li>\<assembly-name>.exe</li><li>\<assembly-name>.pdb</li><li>\<assembly-name>.PrecompiledViews.dll</li><li>\<assembly-name>.PrecompiledViews.pdb</li><li>\<assembly-name>.runtimeconfig.json</li><li>web.config (IIS 部署)</li></ul></li></ul> |

&dagger;表示是目錄

*publish* 目錄代表部署的「內容根目錄路徑」(也稱為「應用程式基底路徑」)。 不論給予伺服器上所部署應用程式的 *publish* 目錄什麼名稱，其位置都會作為所裝載應用程式的伺服器實體路徑。

*wwwroot* 目錄 (如果存在) 只包含靜態資產。

使用下列兩種方法其中之一，即可為部署建立 stdout *logs* 目錄：

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

   `<MakeDir>` 元素會在所發佈的輸出中建立一個空的 [Logs] 資料夾。 此元素會使用 `PublishDir` 屬性來判斷用於建立資料夾的目標位置。 數個部署方法 (例如 Web Deploy) 在部署期間都會略過空的資料夾。 `<WriteLinesToFile>` 元素會在 [Logs] 資料夾中產生檔案，用以確保將資料夾部署至伺服器。 請注意，如果背景工作處理序沒有目標資料夾的寫入存取權，則建立資料夾時仍然可能失敗。

* 在部署中的伺服器上實體建立 *Logs* 目錄。

部署目錄會要求「讀取/執行」權限。 *Logs* 目錄會要求「讀取/寫入」權限。 其他可供寫入檔案的目錄會要求「讀取/寫入」權限。
