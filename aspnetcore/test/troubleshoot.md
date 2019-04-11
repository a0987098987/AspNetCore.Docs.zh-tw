---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: test/troubleshoot
ms.openlocfilehash: 3d755b2f0c509d65dea86bbe719e42935d87d546
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488737"
---
# <a name="troubleshoot-aspnet-core-projects"></a>疑難排解 ASP.NET Core 專案

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

下列連結提供疑難排解指導方針：

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC Conference （倫敦，2018年）：ASP.NET Core 應用程式中的診斷問題](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET 部落格：ASP.NET Core 的效能問題的疑難排解](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK 警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>會安裝 32 位元和 64 位元版本的.NET Core SDK

在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：

> 會安裝 32 和 64 位元版本的.NET Core sdk。 只有從安裝在 64 位元版本的範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。

時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。 這兩個版本可能會安裝的常見原因包括：

* 您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。
* 32 位元.NET Core SDK 已安裝另一個應用程式。
* 版本錯誤下載並安裝。

解除安裝 32 位元.NET Core SDK，以避免這個警告。 從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。 如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK 安裝在多個位置

在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：

> .NET Core SDK 會安裝在多個位置。 只有在已安裝的 SDK 範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。

您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\*。 通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。

解除安裝所有 32 位元.NET Core Sdk 和執行階段可避免這個警告。 從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。 如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。

### <a name="no-net-core-sdks-were-detected"></a>偵測到.NET Core Sdk

* 在 Visual Studio**新的專案**對話方塊適用於 ASP.NET Core 中，您可能會看到下列警告：

  > 偵測到任何.NET Core Sdk，請確定它們會包含在環境變數`PATH`。

* 執行時`dotnet`命令時，警告會顯示為：

  > 無法找到任何已安裝的 dotnet Sdk。

這些警告時顯示的環境變數`PATH`未指向任何電腦上的.NET Core Sdk。 若要解決此問題：

* 安裝 .NET Core SDK。 取得最新的安裝程式，從[.NET 下載](https://dotnet.microsoft.com/download)。
* 確認`PATH`環境變數指向安裝 SDK 的位置 (`C:\Program Files\dotnet\`針對 64 位元 x64 或`C:\Program Files (x86)\dotnet\`的 32 位元 x86)。 SDK 安裝程式通常設定`PATH`。 一律在相同電腦上安裝相同的位元 Sdk 和執行階段。

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>安裝.NET Core 裝載套件組合之後遺漏 SDK

安裝[.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)修改`PATH`它會安裝.NET Core 執行階段，以指向 32 位元 (x86) 版本的.NET Core (`C:\Program Files (x86)\dotnet\`)。 這會導致遺漏 Sdk 時 32 位元 (x86) 的.NET Core`dotnet`使用命令 ([偵測到.NET Core Sdk](#no-net-core-sdks-were-detected))。 若要解決此問題，請移動`C:\Program Files\dotnet\`到之前的位置`C:\Program Files (x86)\dotnet\`上`PATH`。

## <a name="obtain-data-from-an-app"></a>從應用程式取得資料

應用程式是否能夠回應要求，您可以從使用中介軟體應用程式來取得下列資料：

* 要求&ndash;方法配置、 主機、 pathbase、 路徑、 查詢字串的標頭
* 連接&ndash;遠端 IP 位址、 遠端連接埠、 本機 IP 位址、 本機連接埠、 用戶端憑證
* 身分識別&ndash;名稱、 顯示名稱
* 組態設定
* 環境變數

下列項目放[中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)開頭的程式碼`Startup.Configure`方法的要求處理管線。 中介軟體會執行以確保只在開發環境中執行的程式碼之前，會檢查環境。

若要取得環境，請使用下列其中一個方法：

* 插入`IHostingEnvironment`成`Startup.Configure`方法，並檢查本機變數的環境。 下列程式碼範例會示範這種方法。

* 將環境指派給屬性，以在`Startup`類別。 檢查環境中使用屬性 (例如`if (Environment.IsDevelopment())`)。

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
