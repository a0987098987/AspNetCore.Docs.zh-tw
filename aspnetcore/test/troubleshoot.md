---
title: 疑難排解和調試 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: 042e1c84a7226cb4bf56bcc0ff4e576b67e68e1b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655299"
---
# <a name="troubleshoot-and-debug-aspnet-core-projects"></a>疑難排解和調試 ASP.NET Core 專案

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

下列連結提供疑難排解指引：

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC 會議（倫敦，2018）：診斷 ASP.NET Core 應用程式中的問題](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET Blog：對 ASP.NET Core 效能問題進行疑難排解](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET Core SDK 警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>已安裝32位和64位版本的 .NET Core SDK

在 ASP.NET Core 的 [**新增專案**] 對話方塊中，您可能會看到下列警告：

> 已安裝32位和64位版本的 .NET Core SDK。 只會顯示安裝在 ' C：\\Program Files\\dotnet\\sdk\\' 的64位版本範本。

當安裝32位（x86）和64位（x64）版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)時，就會出現這個警告。 可能會安裝這兩個版本的常見原因包括：

* 您一開始是使用32位電腦下載 .NET Core SDK 安裝程式，然後將它複製到64位電腦並加以安裝。
* 32位 .NET Core SDK 是由另一個應用程式所安裝。
* 下載並安裝了錯誤的版本。

卸載32位 .NET Core SDK 以避免出現此警告。 從 [**控制台**] 中卸載 > [**程式和功能**] > **卸載或變更程式**。 如果您瞭解為什麼會發生警告和其影響，您可以忽略此警告。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK 安裝在多個位置

在 ASP.NET Core 的 [**新增專案**] 對話方塊中，您可能會看到下列警告：

> .NET Core SDK 安裝在多個位置。 只會顯示安裝在 ' C：\\Program Files\\dotnet\\sdk\\' 的 Sdk 範本。

當您至少有一個 .NET Core SDK 安裝在*C：\\Program Files\\dotnet\\SDK\\* 以外的目錄中時，您會看到此訊息。 當 .NET Core SDK 已使用複製/貼上而非 MSI 安裝程式部署到電腦上時，通常就會發生這種情況。

卸載所有32位 .NET Core Sdk 和執行時間，以避免出現此警告。 從 [**控制台**] 中卸載 > [**程式和功能**] > **卸載或變更程式**。 如果您瞭解為什麼會發生警告和其影響，您可以忽略此警告。

### <a name="no-net-core-sdks-were-detected"></a>未偵測到任何 .NET Core Sdk

* 在 ASP.NET Core 的 [Visual Studio**新增專案**] 對話方塊中，您可能會看到下列警告：

  > 未偵測到任何 .NET Core Sdk，請確認其包含在環境變數 `PATH`中。

* 執行 `dotnet` 命令時，警告會顯示為：

  > 找不到任何已安裝的 dotnet Sdk。

當環境變數 `PATH` 未指向電腦上的任何 .NET Core Sdk 時，就會出現這些警告。 若要解決此問題：

* 安裝 .NET Core SDK。 從[.Net 下載](https://dotnet.microsoft.com/download)取得最新的安裝程式。
* 確認 `PATH` 環境變數指向安裝 SDK 的位置（適用于64位/x64 的`C:\Program Files\dotnet\` 或32位/x86 的 `C:\Program Files (x86)\dotnet\`）。 SDK 安裝程式通常會設定 `PATH`。 一律在同一部電腦上安裝相同的位 Sdk 和執行時間。

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>安裝 .NET Core 裝載套件組合之後遺失 SDK

安裝 .net core[裝載](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)套件組合時，會在安裝 .net core 執行時間以指向 .net core 的32位（x86）版本（`C:\Program Files (x86)\dotnet\`）時，修改 `PATH`。 當使用32位（x86） .NET Core `dotnet` 命令時（[未偵測到 .Net Core sdk](#no-net-core-sdks-were-detected)），這可能會導致缺少 sdk。 若要解決此問題，請將 `C:\Program Files\dotnet\` 移至 `PATH`上 `C:\Program Files (x86)\dotnet\` 之前的位置。

## <a name="obtain-data-from-an-app"></a>從應用程式取得資料

如果應用程式能夠回應要求，您可以使用中介軟體從應用程式取得下列資料：

* 要求 &ndash; 方法、配置、主機、pathbase、路徑、查詢字串、標頭
* 連接 &ndash; 遠端 IP 位址、遠端埠、本機 IP 位址、本機埠、用戶端憑證
* 身分識別 &ndash; 名稱、顯示名稱
* 組態設定
* 環境變數

將下列[中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)程式碼放在 `Startup.Configure` 方法的要求處理管線的開頭。 在執行中介軟體之前，會檢查環境，以確保程式碼只會在開發環境中執行。

若要取得環境，請使用下列其中一種方法：

* 將 `IHostingEnvironment` 插入 `Startup.Configure` 方法，並使用本機變數檢查環境。 下列範例程式碼示範這種方法。

* 將環境指派給 `Startup` 類別中的屬性。 使用屬性檢查環境（例如，`if (Environment.IsDevelopment())`）。

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

## <a name="debug-aspnet-core-apps"></a>ASP.NET Core 應用程式的 Debug

下列連結提供有關 ASP.NET Core 應用程式的偵錯工具的資訊。

* [在 Linux 上對 ASP Core 進行調試](https://devblogs.microsoft.com/premier-developer/debugging-asp-core-on-linux-with-visual-studio-2017/)
* [透過 SSH 在 Unix 上對 .NET Core 進行調試](https://devblogs.microsoft.com/devops/debugging-net-core-on-unix-over-ssh/)
* [快速入門：使用 Visual Studio 偵錯工具的 Debug ASP.NET](/visualstudio/debugger/quickstart-debug-aspnet)
* 如需更多的偵錯工具資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/2960)。
