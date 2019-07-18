---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: b434af2dd046045836d2f6f7f7b7b2d57699bedc
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308286"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="23cfa-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="23cfa-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="23cfa-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23cfa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="23cfa-105">下列連結提供疑難排解指引:</span><span class="sxs-lookup"><span data-stu-id="23cfa-105">The following links provide troubleshooting guidance:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="23cfa-106">NDC 會議 (倫敦, 2018):診斷 ASP.NET Core 應用程式中的問題</span><span class="sxs-lookup"><span data-stu-id="23cfa-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="23cfa-107">ASP.NET Blog:疑難排解 ASP.NET Core 效能問題</span><span class="sxs-lookup"><span data-stu-id="23cfa-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="23cfa-108">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="23cfa-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="23cfa-109">已安裝32位和64位版本的 .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="23cfa-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="23cfa-110">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="23cfa-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="23cfa-111">已安裝32位和64位版本的 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="23cfa-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="23cfa-112">只會顯示安裝在\\「C: Program Files\\dotnet\\sdk\\」之64位版本的範本。</span><span class="sxs-lookup"><span data-stu-id="23cfa-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="23cfa-113">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="23cfa-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="23cfa-114">可能會安裝這兩個版本的常見原因包括:</span><span class="sxs-lookup"><span data-stu-id="23cfa-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="23cfa-115">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="23cfa-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="23cfa-116">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="23cfa-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="23cfa-117">下載並安裝了錯誤的版本。</span><span class="sxs-lookup"><span data-stu-id="23cfa-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="23cfa-118">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="23cfa-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="23cfa-119">從 [**控制台** > ] [**程式和功能** > ] 卸載**或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="23cfa-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="23cfa-120">如果您瞭解為什麼會發生警告和其影響, 您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="23cfa-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="23cfa-121">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="23cfa-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="23cfa-122">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="23cfa-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="23cfa-123">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="23cfa-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="23cfa-124">只會顯示安裝在\\「C: Program Files\\dotnet\\sdk\\」的 sdk 範本。</span><span class="sxs-lookup"><span data-stu-id="23cfa-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="23cfa-125">您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\* 。</span><span class="sxs-lookup"><span data-stu-id="23cfa-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="23cfa-126">通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="23cfa-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="23cfa-127">卸載所有32位 .NET Core Sdk 和執行時間, 以避免出現此警告。</span><span class="sxs-lookup"><span data-stu-id="23cfa-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="23cfa-128">從 [**控制台** > ] [**程式和功能** > ] 卸載**或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="23cfa-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="23cfa-129">如果您瞭解為什麼會發生警告和其影響, 您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="23cfa-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="23cfa-130">偵測到.NET Core Sdk</span><span class="sxs-lookup"><span data-stu-id="23cfa-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="23cfa-131">在 ASP.NET Core 的 [Visual Studio**新增專案**] 對話方塊中, 您可能會看到下列警告:</span><span class="sxs-lookup"><span data-stu-id="23cfa-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="23cfa-132">未偵測到任何 .NET Core Sdk, 請確認其包含在環境變數`PATH`中。</span><span class="sxs-lookup"><span data-stu-id="23cfa-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="23cfa-133">執行`dotnet`命令時, 警告會顯示為:</span><span class="sxs-lookup"><span data-stu-id="23cfa-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="23cfa-134">找不到任何已安裝的 dotnet Sdk。</span><span class="sxs-lookup"><span data-stu-id="23cfa-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="23cfa-135">當環境變數`PATH`未指向電腦上的任何 .net Core sdk 時, 就會出現這些警告。</span><span class="sxs-lookup"><span data-stu-id="23cfa-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="23cfa-136">若要解決此問題:</span><span class="sxs-lookup"><span data-stu-id="23cfa-136">To resolve this problem:</span></span>

* <span data-ttu-id="23cfa-137">安裝 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="23cfa-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="23cfa-138">從[.Net 下載](https://dotnet.microsoft.com/download)取得最新的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="23cfa-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="23cfa-139">確認環境變數指向安裝 SDK 的位置 (`C:\Program Files\dotnet\`適用于64位/x64 或`C:\Program Files (x86)\dotnet\` 32 位/x86)。 `PATH`</span><span class="sxs-lookup"><span data-stu-id="23cfa-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="23cfa-140">SDK 安裝程式通常會設定`PATH`。</span><span class="sxs-lookup"><span data-stu-id="23cfa-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="23cfa-141">一律在同一部電腦上安裝相同的位 Sdk 和執行時間。</span><span class="sxs-lookup"><span data-stu-id="23cfa-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="23cfa-142">安裝 .NET Core 裝載套件組合之後遺失 SDK</span><span class="sxs-lookup"><span data-stu-id="23cfa-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="23cfa-143">安裝 .net core[裝載](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)套件組合會`PATH`在安裝 .net core 執行時間時修改, 以指向 .net core 的32位 (x86) 版本 (`C:\Program Files (x86)\dotnet\`)。</span><span class="sxs-lookup"><span data-stu-id="23cfa-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="23cfa-144">當使用32位 (x86) .net core `dotnet`命令 (未偵測[到 .net core sdk](#no-net-core-sdks-were-detected)) 時, 這可能會導致缺少 sdk。</span><span class="sxs-lookup"><span data-stu-id="23cfa-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="23cfa-145">若要解決這個問題, `C:\Program Files\dotnet\`請移至上`C:\Program Files (x86)\dotnet\` `PATH`之前的位置。</span><span class="sxs-lookup"><span data-stu-id="23cfa-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="23cfa-146">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="23cfa-146">Obtain data from an app</span></span>

<span data-ttu-id="23cfa-147">如果應用程式能夠回應要求, 您可以使用中介軟體從應用程式取得下列資料:</span><span class="sxs-lookup"><span data-stu-id="23cfa-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="23cfa-148">Request &ndash;方法、配置、主機、pathbase、路徑、查詢字串、標頭</span><span class="sxs-lookup"><span data-stu-id="23cfa-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="23cfa-149">連接&ndash;遠端 IP 位址、遠端埠、本機 IP 位址、本機埠、用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="23cfa-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="23cfa-150">識別&ndash;名稱、顯示名稱</span><span class="sxs-lookup"><span data-stu-id="23cfa-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="23cfa-151">組態設定</span><span class="sxs-lookup"><span data-stu-id="23cfa-151">Configuration settings</span></span>
* <span data-ttu-id="23cfa-152">環境變數</span><span class="sxs-lookup"><span data-stu-id="23cfa-152">Environment variables</span></span>

<span data-ttu-id="23cfa-153">將下列[中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)程式碼放在`Startup.Configure`方法的要求處理管線的開頭。</span><span class="sxs-lookup"><span data-stu-id="23cfa-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="23cfa-154">在執行中介軟體之前, 會檢查環境, 以確保程式碼只會在開發環境中執行。</span><span class="sxs-lookup"><span data-stu-id="23cfa-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="23cfa-155">若要取得環境, 請使用下列其中一種方法:</span><span class="sxs-lookup"><span data-stu-id="23cfa-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="23cfa-156">將插入`Startup.Configure`方法中,並使用本機變數檢查環境。`IHostingEnvironment`</span><span class="sxs-lookup"><span data-stu-id="23cfa-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="23cfa-157">下列範例程式碼示範這種方法。</span><span class="sxs-lookup"><span data-stu-id="23cfa-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="23cfa-158">將環境指派給`Startup`類別中的屬性。</span><span class="sxs-lookup"><span data-stu-id="23cfa-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="23cfa-159">使用屬性檢查環境 (例如, `if (Environment.IsDevelopment())`)。</span><span class="sxs-lookup"><span data-stu-id="23cfa-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
