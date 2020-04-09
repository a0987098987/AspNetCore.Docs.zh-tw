---
title: ASP.NET核心項目進行故障排除和除錯
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: 345967f08cf99ef5f18d0c9bcd59ab29c74454f1
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79511505"
---
# <a name="troubleshoot-and-debug-aspnet-core-projects"></a>ASP.NET核心項目進行故障排除和除錯

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

以下連結提供故障排除指南:

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC 會議(2018 年 倫敦):診斷ASP.NET核心應用中的問題](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET博客:解決核心性能問題的ASP.NET](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET 核心 SDK 警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>安裝了 .NET Core SDK 的 32 位元和 64 位元版本

在「ASP.NET核心 **」的「新項目**」對話框中,您可能會看到以下警告:

> 安裝了 .NET Core SDK 的 32 位元和 64 位元版本。 僅\\顯示安裝在「C:程式\\檔\\點\\網 」 上的 64 位元版本的範本。

安裝[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)的 32 位元 (x86) 和 64 位元 (x64) 版本時,將顯示此警告。 可能安裝兩個版本的常見原因包括:

* 您最初使用 32 位元電腦下載了 .NET Core SDK 安裝程式,但隨後將其複製到整個電腦上,並將其安裝在 64 位元電腦上。
* 32 位元 .NET 核心 SDK 由另一個應用程式安裝。
* 下載並安裝了錯誤的版本。

卸載 32 位元 .NET 核心 SDK 以防止此警告。 從**控制面板** > **程式與功能** > 卸載**或變更程式**。 如果您瞭解警告發生的原因及其影響,則可以忽略該警告。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET 核心 SDK 安裝在多個位置

在「ASP.NET核心 **」的「新項目**」對話框中,您可能會看到以下警告:

> .NET 核心 SDK 安裝在多個位置。 僅\\顯示安裝在「C:程式檔\\點\\網\\」 上的 SDK 的樣本。

當您在*\\C:\\程式檔案\\點網\\sdk*之外的目錄中至少安裝一個 .NET Core SDK 時,您將看到此消息。 通常,當 .NET Core SDK 已使用複製/粘貼而不是 MSI 安裝程式部署在電腦上時,就會發生這種情況。

卸載所有 32 位元 .NET 核心 SDK 和運行時,以防止出現此警告。 從**控制面板** > **程式與功能** > 卸載**或變更程式**。 如果您瞭解警告發生的原因及其影響,則可以忽略該警告。

### <a name="no-net-core-sdks-were-detected"></a>未偵測到 .NET 核心 SDK

* 在「ASP.NET核心的視覺化工作室**新項目**」對話框中,您可能會看到以下警告:

  > 未檢測到 .NET 核心 SDK,確保它們包含在`PATH`環境變數 中。

* 執行命令`dotnet`時,警告顯示為:

  > 無法找到任何已安裝的點網 SDK。

當環境變數`PATH`不指向電腦上的任何 .NET 核心 SDK 時,將顯示這些警告。 若要解決此問題：

* 安裝 .NET Core SDK。 從[.NET 下載](https://dotnet.microsoft.com/download)獲取最新的安裝程式。
* 驗證`PATH`環境變數是否指向安裝 SDK`C:\Program Files\dotnet\`的位置( 對於 64 位`C:\Program Files (x86)\dotnet\`元/x64 或 32 位元/x86)。 SDK 安裝程式通常`PATH`設定 。 始終在同一台電腦上安裝相同的位點 SDK 和運行時。

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>安裝 .NET 核心託管捆綁包後缺少 SDK

安裝[.NET 核心託管捆綁套件](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)將`PATH`修改安裝 .NET Core 執行時以指向 .NET Core () 的`C:\Program Files (x86)\dotnet\`32 位元 (x86) 版本 ( ) 當使用 32 位元 (x86)`dotnet`.NET Core 命令(檢測到 No[.NET 核心 SDK)](#no-net-core-sdks-were-detected)時,這可能會導致缺少 SDK。 要解決此問題,轉到`C:\Program Files\dotnet\``C:\Program Files (x86)\dotnet\`之前`PATH`在上的位置。

## <a name="obtain-data-from-an-app"></a>從應用程式取得資料

如果應用能夠回應請求,則可以使用中間件從應用獲取以下資料:

* 要求&ndash;方法、方案、主機、路徑庫、路徑、查詢字串、標頭
* 連線&ndash;遠端 IP 位址、遠端連接埠、本地 IP 位址、本地端埠、用戶端憑證
* 識別&ndash;名稱,顯示名稱
* 組態設定
* 環境變數

將以下[中間件](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)代碼`Startup.Configure`放在方法的請求處理管道的開頭。 在運行中間件之前檢查環境,以確保代碼僅在開發環境中執行。

要取得環境,請使用以下任一方法:

* 將`IHostingEnvironment`注入`Startup.Configure`方法,並與局部變數檢查環境。 以下示例代碼演示了此方法。

* 將環境分配給類中的`Startup`屬性。 使用 屬性檢查環境(例如`if (Environment.IsDevelopment())`,

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

## <a name="debug-aspnet-core-apps"></a>除錯ASP.NET核心應用

以下連結提供有關調試 ASP.NET核心應用的資訊。

* [Linux 上呼試 ASP 核心](https://devblogs.microsoft.com/premier-developer/debugging-asp-core-on-linux-with-visual-studio-2017/)
* [在 SSH 上 Unix 上調試 .NET 內核](https://devblogs.microsoft.com/devops/debugging-net-core-on-unix-over-ssh/)
* [快速入門：使用 Visual Studio 偵錯工具來偵錯 ASP.NET](/visualstudio/debugger/quickstart-debug-aspnet)
* 有關詳細資訊,請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/2960)。
