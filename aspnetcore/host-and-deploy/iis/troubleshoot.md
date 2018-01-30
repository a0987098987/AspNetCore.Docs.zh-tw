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
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="5b350-103">疑難排解在 IIS 上的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b350-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="5b350-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b350-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5b350-105">若要診斷的 IIS 部署問題：</span><span class="sxs-lookup"><span data-stu-id="5b350-105">To diagnose issues with IIS deployments:</span></span>

* <span data-ttu-id="5b350-106">研究瀏覽器輸出。</span><span class="sxs-lookup"><span data-stu-id="5b350-106">Study browser output.</span></span>
* <span data-ttu-id="5b350-107">透過**事件檢視器**，檢查系統的**應用程式**記錄檔。</span><span class="sxs-lookup"><span data-stu-id="5b350-107">Examine the system's **Application** log through **Event Viewer**.</span></span>
* <span data-ttu-id="5b350-108">啟用 `stdout` 記錄。</span><span class="sxs-lookup"><span data-stu-id="5b350-108">Enable `stdout` logging.</span></span> <span data-ttu-id="5b350-109">您可以在 *web.config* 中 `<aspNetCore>` 項目的 *stdoutLogFile* 屬性所提供的路徑上，找到 **ASP.NET Core 模組**記錄檔。部署中必須有屬性值提供之路徑上的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="5b350-109">The **ASP.NET Core Module** log is found on the path provided in the *stdoutLogFile* attribute of the `<aspNetCore>` element in *web.config*. Any folders on the path provided in the attribute value must exist in the deployment.</span></span> <span data-ttu-id="5b350-110">設定*stdoutLogEnabled*至`true`。</span><span class="sxs-lookup"><span data-stu-id="5b350-110">Set *stdoutLogEnabled* to `true`.</span></span> <span data-ttu-id="5b350-111">使用應用程式`Microsoft.NET.Sdk.Web`SDK 建立*web.config*檔案預設*stdoutLogEnabled*設`false`、 手動提供*web.config*檔案或修改檔案以啟用`stdout`記錄。</span><span class="sxs-lookup"><span data-stu-id="5b350-111">Apps that use the the `Microsoft.NET.Sdk.Web` SDK to create the *web.config* file default the *stdoutLogEnabled* setting to `false`, so manually provide the *web.config* file or modify the file in order to enable `stdout` logging.</span></span>

<span data-ttu-id="5b350-112">使用具有三個來源的資訊[常見錯誤參考主題](xref:host-and-deploy/azure-iis-errors-reference)來判斷問題。</span><span class="sxs-lookup"><span data-stu-id="5b350-112">Use information from those three sources with the [common errors reference topic](xref:host-and-deploy/azure-iis-errors-reference) to determine the problem.</span></span> <span data-ttu-id="5b350-113">請遵循所提供以解決此問題的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="5b350-113">Follow the troubleshooting advice provided to resolve the issue.</span></span>

<span data-ttu-id="5b350-114">若干常見錯誤要等到模組的 *startupTimeLimit* (預設值：120 秒) 和 *startupRetryCount* (預設值：2) 經過後，才會出現在瀏覽器、應用程式記錄檔和 ASP.NET Core 模組記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="5b350-114">Several of the common errors don't appear in the browser, Application Log, and ASP.NET Core Module Log until the module *startupTimeLimit* (default: 120 seconds) and *startupRetryCount* (default: 2) have passed.</span></span> <span data-ttu-id="5b350-115">因此，要等候整整六分鐘，才能推算出模組無法啟動應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="5b350-115">Therefore, wait a full six minutes before deducing that the module has failed to start a process for the app.</span></span>

<span data-ttu-id="5b350-116">若要判斷應用程式是否正常運作，有一個快速的方法是直接在 Kestrel 上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5b350-116">One quick way to determine if the app is working properly is to run the app directly on Kestrel.</span></span> <span data-ttu-id="5b350-117">如果應用程式已發佈做為[framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，執行`dotnet <assembly_name>.dll`的部署資料夾中，這是 IIS 應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="5b350-117">If the app was published as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), execute `dotnet <assembly_name>.dll` in the deployment folder, which is the IIS physical path to the app.</span></span> <span data-ttu-id="5b350-118">如果應用程式已發佈做為[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請執行應用程式的可執行檔，直接從命令提示字元， `<assembly_name>.exe`，部署資料夾中。</span><span class="sxs-lookup"><span data-stu-id="5b350-118">If the app was published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), run the app's executable directly from a command prompt, `<assembly_name>.exe`, in the deployment folder.</span></span> <span data-ttu-id="5b350-119">如果 Kestrel 接聽預設通訊埠 5000，應用程式應該可以在`http://localhost:5000/`。</span><span class="sxs-lookup"><span data-stu-id="5b350-119">If Kestrel is listening on default port 5000, the app should be available at `http://localhost:5000/`.</span></span> <span data-ttu-id="5b350-120">如果應用程式通常回應 Kestrel 端點位址，此問題會更相關的反向 proxy 設定和比較不可能是應用程式內。</span><span class="sxs-lookup"><span data-stu-id="5b350-120">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

<span data-ttu-id="5b350-121">若要判斷反向 proxy 是否正常運作的一種方式為樣式表、 指令碼，或從應用程式的靜態檔案中映像執行簡單的靜態檔案要求*wwwroot*使用[靜態檔案中介軟體](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="5b350-121">One way to determine if the reverse proxy is working properly is to perform a simple static file request for a stylesheet, script, or image from the app's static files in *wwwroot* using [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="5b350-122">如果應用程式可以提供靜態檔案，但是 MVC 檢視和其他端點失敗，問題是比較不可能相關的反向 proxy 設定和比較可能的應用程式內 （例如，MVC 路由或 500 內部伺服器錯誤）。</span><span class="sxs-lookup"><span data-stu-id="5b350-122">If the app can serve static files but MVC Views and other endpoints are failing, the problem is less likely related to the reverse proxy configuration and more likely within the app (for example, MVC routing or 500 Internal Server Error).</span></span>

<span data-ttu-id="5b350-123">當 Kestrel 正常啟動 IIS 後，但應用程式將不會在系統上執行本機成功執行之後時，環境變數可以暫時加入*web.config*設定`ASPNETCORE_ENVIRONMENT`至`Development`。</span><span class="sxs-lookup"><span data-stu-id="5b350-123">When Kestrel starts normally behind IIS but the app won't run on the system after successfully running locally, an environment variable can be temporarily added to *web.config* to set the `ASPNETCORE_ENVIRONMENT` to `Development`.</span></span> <span data-ttu-id="5b350-124">只要在應用程式啟動環境不覆寫時，設定環境變數可讓[開發人員例外狀況頁面](xref:fundamentals/error-handling)出現應用程式執行時。</span><span class="sxs-lookup"><span data-stu-id="5b350-124">As long as the environment isn't overridden in app startup, setting the environment variable allows the [developer exception page](xref:fundamentals/error-handling) to appear when the app is run.</span></span> <span data-ttu-id="5b350-125">設定環境變數，如`ASPNETCORE_ENVIRONMENT`以這種方式只建議用於臨時/測試伺服器，不會被公開到網際網路。</span><span class="sxs-lookup"><span data-stu-id="5b350-125">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` in this way is only recommended for staging/testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="5b350-126">請務必移除環境變數從*web.config*檔案時完成。</span><span class="sxs-lookup"><span data-stu-id="5b350-126">Be sure to remove the environment variable from the *web.config* file when finished.</span></span> <span data-ttu-id="5b350-127">如需有關設定環境變數，透過詳細*web.config*，請參閱[aspNetCore environmentVariables 子項目](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="5b350-127">For information on setting environment variables via *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

<span data-ttu-id="5b350-128">在大部分情況下，啟用應用程式記錄有助於針對應用程式或反向 Proxy 的問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="5b350-128">In most cases, enabling application logging assists in troubleshooting problems with the app or the reverse proxy.</span></span> <span data-ttu-id="5b350-129">如需詳細資訊，請參閱[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="5b350-129">See [Logging](xref:fundamentals/logging/index) for more information.</span></span>

<span data-ttu-id="5b350-130">最後一個疑難排解提示無法執行，請在升級後的應用程式與.NET Core SDK 在應用程式內的開發電腦或套件版本。</span><span class="sxs-lookup"><span data-stu-id="5b350-130">The last troubleshooting tip pertains to apps that fail to run after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="5b350-131">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="5b350-131">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="5b350-132">大部分的這些問題可以藉由修正：</span><span class="sxs-lookup"><span data-stu-id="5b350-132">Most of these issues can be fixed by:</span></span>

* <span data-ttu-id="5b350-133">刪除`bin`和`obj`專案中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5b350-133">Deleting the `bin` and `obj` folders in the project.</span></span>
* <span data-ttu-id="5b350-134">清除封裝快取在`%UserProfile%\.nuget\packages\`和`%LocalAppData%\Nuget\v3-cache`。</span><span class="sxs-lookup"><span data-stu-id="5b350-134">Clearing package caches at `%UserProfile%\.nuget\packages\` and `%LocalAppData%\Nuget\v3-cache`.</span></span>
* <span data-ttu-id="5b350-135">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="5b350-135">Restoring and rebuilding the project.</span></span>
* <span data-ttu-id="5b350-136">請確認先前的部署在伺服器上已重新部署應用程式之前，先完全刪除。</span><span class="sxs-lookup"><span data-stu-id="5b350-136">Confirming that the prior deployment on the server has been completely deleted prior to re-deploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="5b350-137">清除封裝快取的便利方式是執行`dotnet nuget locals all --clear`從命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="5b350-137">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
> 
> <span data-ttu-id="5b350-138">清除封裝快取也可藉由使用[nuget.exe](https://www.nuget.org/downloads)工具並執行命令`nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="5b350-138">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="5b350-139">*nuget.exe*未與 Windows 10 的配套的安裝，且必須另外從 NuGet 網站取得。</span><span class="sxs-lookup"><span data-stu-id="5b350-139">*nuget.exe* isn't a bundled install with Windows 10 and must be obtained separately from the NuGet website.</span></span>
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a><span data-ttu-id="5b350-140">其他資源</span><span class="sxs-lookup"><span data-stu-id="5b350-140">Additional resources</span></span>

* [<span data-ttu-id="5b350-141">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="5b350-141">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="5b350-142">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="5b350-142">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
