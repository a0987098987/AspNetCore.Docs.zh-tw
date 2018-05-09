---
title: ASP.NET Core 目錄結構
author: guardrex
description: 深入了解已發行的 ASP.NET Core 應用程式的目錄結構。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core 目錄結構

作者：[Luke Latham](https://github.com/guardrex)

在 ASP.NET Core，已發行的應用程式目錄，*發行*，組成應用程式檔案、 組態檔、 靜態資產、 封裝和執行階段 (如[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd))。


| 應用程式類型 | 目錄結構 |
| -------- | ------------------- |
| [Framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>發行&dagger;<ul><li>記錄檔&dagger;（除非需要接收 stdout 記錄檔的選擇性）</li><li>檢視&dagger;（MVC 應用程式; 如果不先行編譯的檢視）</li><li>頁面&dagger;（MVC 或 Razor 網頁應用程式; 如果不先行編譯的頁面）</li><li>wwwroot&dagger;</li><li>*\.dll 檔案</li><li>\<組件名稱 >。 deps.json</li><li>\<組件名稱 >.dll</li><li>\<組件名稱 >.pdb</li><li>\<組件名稱 >。PrecompiledViews.dll</li><li>\<組件名稱 >。PrecompiledViews.pdb</li><li>\<組件名稱 >。 runtimeconfig.json</li><li>web.config （IIS 部署）</li></ul></li></ul> |
| [獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>發行&dagger;<ul><li>記錄檔&dagger;（除非需要接收 stdout 記錄檔的選擇性）</li><li>refs&dagger;</li><li>檢視&dagger;（MVC 應用程式; 如果不先行編譯的檢視）</li><li>頁面&dagger;（MVC 或 Razor 網頁應用程式; 如果不先行編譯的頁面）</li><li>wwwroot&dagger;</li><li>\*.dll 檔案</li><li>\<組件名稱 >。 deps.json</li><li>\<組件名稱 >.exe</li><li>\<組件名稱 >.pdb</li><li>\<組件名稱 >。PrecompiledViews.dll</li><li>\<組件名稱 >。PrecompiledViews.pdb</li><li>\<組件名稱 >。 runtimeconfig.json</li><li>web.config （IIS 部署）</li></ul></li></ul> |

&dagger;表示目錄

*發行*目錄代表*內容的根路徑*，也稱為*應用程式基底路徑*的部署。 若要指定任何名稱*發行*目錄中的伺服器上部署的應用程式，其位置做為裝載應用程式伺服器的實體路徑。

*Wwwroot*目錄中，如果有的話，只包含靜態資產。

Stdout*記錄*可以使用下列兩種方法的其中一個部署建立目錄：

* 加入下列`<Target>`項目加入專案檔：

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

   `<MakeDir>`項目會建立空*記錄*中發行的輸出資料夾。 項目使用`PublishDir`屬性來判斷建立此資料夾的目標位置。 數種部署方法，例如 Web Deploy，請在部署期間，略過空資料夾。 `<WriteLinesToFile>`項目會產生的檔案中*記錄*資料夾中，以確保部署到伺服器的資料夾。 請注意是否工作者處理序不具有寫入存取權的目標資料夾可能仍然無法建立資料夾。

* 實際建立*記錄*目錄以部署在伺服器上。

部署目錄中需要讀取/執行權限。 *記錄*目錄需要讀取/寫入權限。 寫入檔案的其他目錄需要讀取/寫入權限。
