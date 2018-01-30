---
title: "疑難排解在 IIS 上的 ASP.NET Core"
author: guardrex
description: "了解如何診斷問題的 IIS 的 ASP.NET Core 應用程式的部署。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: c68070a9cba5667504d1ad4927b02b73f83e6573
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>疑難排解在 IIS 上的 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex)

若要診斷的 IIS 部署問題：

* 研究瀏覽器輸出。
* 透過**事件檢視器**，檢查系統的**應用程式**記錄檔。
* 啟用 `stdout` 記錄。 您可以在 *web.config* 中 `<aspNetCore>` 項目的 *stdoutLogFile* 屬性所提供的路徑上，找到 **ASP.NET Core 模組**記錄檔。部署中必須有屬性值提供之路徑上的所有資料夾。 設定*stdoutLogEnabled*至`true`。 使用應用程式`Microsoft.NET.Sdk.Web`SDK 建立*web.config*檔案預設*stdoutLogEnabled*設`false`、 手動提供*web.config*檔案或修改檔案以啟用`stdout`記錄。

使用具有三個來源的資訊[常見錯誤參考主題](xref:host-and-deploy/azure-iis-errors-reference)來判斷問題。 請遵循所提供以解決此問題的疑難排解建議。

若干常見錯誤要等到模組的 *startupTimeLimit* (預設值：120 秒) 和 *startupRetryCount* (預設值：2) 經過後，才會出現在瀏覽器、應用程式記錄檔和 ASP.NET Core 模組記錄檔中。 因此，要等候整整六分鐘，才能推算出模組無法啟動應用程式的程序。

若要判斷應用程式是否正常運作，有一個快速的方法是直接在 Kestrel 上執行應用程式。 如果應用程式已發佈做為[framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，執行`dotnet <assembly_name>.dll`的部署資料夾中，這是 IIS 應用程式的實體路徑。 如果應用程式已發佈做為[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請執行應用程式的可執行檔，直接從命令提示字元， `<assembly_name>.exe`，部署資料夾中。 如果 Kestrel 接聽預設通訊埠 5000，應用程式應該可以在`http://localhost:5000/`。 如果應用程式通常回應 Kestrel 端點位址，此問題會更相關的反向 proxy 設定和比較不可能是應用程式內。

若要判斷反向 proxy 是否正常運作的一種方式為樣式表、 指令碼，或從應用程式的靜態檔案中映像執行簡單的靜態檔案要求*wwwroot*使用[靜態檔案中介軟體](xref:fundamentals/static-files). 如果應用程式可以提供靜態檔案，但是 MVC 檢視和其他端點失敗，問題是比較不可能相關的反向 proxy 設定和比較可能的應用程式內 （例如，MVC 路由或 500 內部伺服器錯誤）。

當 Kestrel 正常啟動 IIS 後，但應用程式將不會在系統上執行本機成功執行之後時，環境變數可以暫時加入*web.config*設定`ASPNETCORE_ENVIRONMENT`至`Development`。 只要在應用程式啟動環境不覆寫時，設定環境變數可讓[開發人員例外狀況頁面](xref:fundamentals/error-handling)出現應用程式執行時。 設定環境變數，如`ASPNETCORE_ENVIRONMENT`以這種方式只建議用於臨時/測試伺服器，不會被公開到網際網路。 請務必移除環境變數從*web.config*檔案時完成。 如需有關設定環境變數，透過詳細*web.config*，請參閱[aspNetCore environmentVariables 子項目](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。

在大部分情況下，啟用應用程式記錄有助於針對應用程式或反向 Proxy 的問題進行疑難排解。 如需詳細資訊，請參閱[記錄](xref:fundamentals/logging/index)。

最後一個疑難排解提示無法執行，請在升級後的應用程式與.NET Core SDK 在應用程式內的開發電腦或套件版本。 在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。 大部分的這些問題可以藉由修正：

* 刪除`bin`和`obj`專案中的資料夾。
* 清除封裝快取在`%UserProfile%\.nuget\packages\`和`%LocalAppData%\Nuget\v3-cache`。
* 還原並重建專案。
* 請確認先前的部署在伺服器上已重新部署應用程式之前，先完全刪除。

> [!TIP]
> 清除封裝快取的便利方式是執行`dotnet nuget locals all --clear`從命令提示字元。
> 
> 清除封裝快取也可藉由使用[nuget.exe](https://www.nuget.org/downloads)工具並執行命令`nuget locals all -clear`。 *nuget.exe*未與 Windows 10 的配套的安裝，且必須另外從 NuGet 網站取得。
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>其他資源

* [Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)
