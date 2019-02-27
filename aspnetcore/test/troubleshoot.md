---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: c8b34f51fd329eb9a7c34f7be93bd7f2aa054283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899280"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="3c006-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="3c006-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="3c006-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3c006-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3c006-105">下列連結提供疑難排解指導方針：</span><span class="sxs-lookup"><span data-stu-id="3c006-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="3c006-106">NDC Conference （倫敦，2018年）：ASP.NET Core 應用程式中的診斷問題</span><span class="sxs-lookup"><span data-stu-id="3c006-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="3c006-107">ASP.NET 部落格：ASP.NET Core 的效能問題的疑難排解</span><span class="sxs-lookup"><span data-stu-id="3c006-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="3c006-108">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="3c006-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="3c006-109">會安裝 32 位元和 64 位元版本的.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="3c006-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="3c006-110">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="3c006-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="3c006-111">會安裝 32 和 64 位元版本的.NET Core sdk。</span><span class="sxs-lookup"><span data-stu-id="3c006-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="3c006-112">只有從安裝在 64 位元版本的範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="3c006-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="3c006-114">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="3c006-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="3c006-115">這兩個版本可能會安裝的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="3c006-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="3c006-116">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="3c006-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="3c006-117">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c006-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="3c006-118">版本錯誤下載並安裝。</span><span class="sxs-lookup"><span data-stu-id="3c006-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="3c006-119">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="3c006-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="3c006-120">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="3c006-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="3c006-121">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="3c006-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="3c006-122">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="3c006-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="3c006-123">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="3c006-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="3c006-124">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="3c006-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="3c006-125">只有在已安裝的 SDK 範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="3c006-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="3c006-127">您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\*。</span><span class="sxs-lookup"><span data-stu-id="3c006-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="3c006-128">通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="3c006-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="3c006-129">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="3c006-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="3c006-130">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="3c006-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="3c006-131">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="3c006-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="3c006-132">偵測到.NET Core Sdk</span><span class="sxs-lookup"><span data-stu-id="3c006-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="3c006-133">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="3c006-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="3c006-134">偵測到.NET Core Sdk，請確定它們包含在環境變數 'PATH'。</span><span class="sxs-lookup"><span data-stu-id="3c006-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="3c006-136">時，會出現這個警告的環境變數`PATH`未指向任何電腦上的.NET Core Sdk (例如`C:\Program Files\dotnet\`和`C:\Program Files (x86)\dotnet\`)。</span><span class="sxs-lookup"><span data-stu-id="3c006-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine (for example, `C:\Program Files\dotnet\` and `C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="3c006-137">若要解決此問題：</span><span class="sxs-lookup"><span data-stu-id="3c006-137">To resolve this problem:</span></span>

* <span data-ttu-id="3c006-138">安裝或檢查已安裝.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="3c006-138">Install or verify the .NET Core SDK is installed.</span></span> <span data-ttu-id="3c006-139">取得最新的安裝程式，從[.NET 下載](https://dotnet.microsoft.com/download)。</span><span class="sxs-lookup"><span data-stu-id="3c006-139">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span> 
* <span data-ttu-id="3c006-140">確認`PATH`環境變數指向安裝 SDK 的位置。</span><span class="sxs-lookup"><span data-stu-id="3c006-140">Verify that the `PATH` environment variable points to the location where the SDK is installed.</span></span> <span data-ttu-id="3c006-141">安裝程式通常設定`PATH`。</span><span class="sxs-lookup"><span data-stu-id="3c006-141">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="3c006-142">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="3c006-142">Obtain data from an app</span></span>

<span data-ttu-id="3c006-143">應用程式是否能夠回應要求，您可以從使用中介軟體應用程式來取得下列資料：</span><span class="sxs-lookup"><span data-stu-id="3c006-143">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="3c006-144">要求&ndash;方法配置、 主機、 pathbase、 路徑、 查詢字串的標頭</span><span class="sxs-lookup"><span data-stu-id="3c006-144">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="3c006-145">連接&ndash;遠端 IP 位址、 遠端連接埠、 本機 IP 位址、 本機連接埠、 用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="3c006-145">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="3c006-146">身分識別&ndash;名稱、 顯示名稱</span><span class="sxs-lookup"><span data-stu-id="3c006-146">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="3c006-147">組態設定</span><span class="sxs-lookup"><span data-stu-id="3c006-147">Configuration settings</span></span>
* <span data-ttu-id="3c006-148">環境變數</span><span class="sxs-lookup"><span data-stu-id="3c006-148">Environment variables</span></span>

<span data-ttu-id="3c006-149">下列項目放[中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)開頭的程式碼`Startup.Configure`方法的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="3c006-149">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="3c006-150">中介軟體會執行以確保只在開發環境中執行的程式碼之前，會檢查環境。</span><span class="sxs-lookup"><span data-stu-id="3c006-150">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="3c006-151">若要取得環境，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="3c006-151">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="3c006-152">插入`IHostingEnvironment`成`Startup.Configure`方法，並檢查本機變數的環境。</span><span class="sxs-lookup"><span data-stu-id="3c006-152">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="3c006-153">下列程式碼範例會示範這種方法。</span><span class="sxs-lookup"><span data-stu-id="3c006-153">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="3c006-154">將環境指派給屬性，以在`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="3c006-154">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="3c006-155">檢查環境中使用屬性 (例如`if (Environment.IsDevelopment())`)。</span><span class="sxs-lookup"><span data-stu-id="3c006-155">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
