---
title: 3\.0 ASP.NET Core 的新功能
author: rick-anderson
description: 深入瞭解 ASP.NET Core 3.0 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: aspnetcore-3.0
ms.openlocfilehash: 8c53d8a9fa222ca40f26dc713ec3b70ddde76539
ms.sourcegitcommit: eb2fe5ad2e82fab86ca952463af8d017ba659b25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2019
ms.locfileid: "73416116"
---
# <a name="whats-new-in-aspnet-core-30"></a>3\.0 ASP.NET Core 的新功能

本文將重點放在 ASP.NET Core 3.0 中最重要的變更，並提供相關檔的連結。

## <a name="blazor"></a>Blazor

Blazor 是 ASP.NET Core 中的新架構，可讓您使用 .NET 建立互動式用戶端 web UI：

* 使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。
* 共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。
* 將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。

Blazor framework 支援的案例：

* 可重複使用的 UI 元件（Razor 元件）
* 用戶端路由
* 元件版面配置
* 支援相依性插入
* 表單和驗證
* 使用 Razor 類別庫建立元件程式庫
* JavaScript Interop

如需詳細資訊，請參閱<xref:blazor/index>。

### <a name="blazor-server"></a>Blazor 伺服器

Blazor 會將元件轉譯邏輯與套用 UI 更新的方式分隔。 Blazor 伺服器提供在 ASP.NET Core 應用程式中將 Razor 元件裝載在伺服器上的支援。 UI 更新透過 SignalR 連線處理。 ASP.NET Core 3.0 支援 Blazor 伺服器。

### <a name="blazor-webassembly-preview"></a>Blazor WebAssembly （預覽）

Blazor apps 也可以直接在瀏覽器中使用以 WebAssembly 為基礎的 .NET 執行時間來執行。 Blazor WebAssembly 處於預覽狀態，ASP.NET Core 3.0*不*支援。 ASP.NET Core 的未來版本將會支援 Blazor WebAssembly。

### <a name="razor-components"></a>Razor 元件

Blazor 應用程式是從元件建立而成。 元件是獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。 元件是定義 UI 呈現邏輯和用戶端事件處理常式的一般 .NET 類別。 您可以建立豐富的互動式 web 應用程式，而不需要 JavaScript。

Blazor 中的元件通常是使用 Razor 語法（這是 HTML 和C#的自然 blend）來撰寫。 Razor 元件與 Razor Pages 和 MVC 的觀點相似，因為它們都使用 Razor。 不同于以要求-回應模型為基礎的頁面和視圖，元件是專門用來處理 UI 組合。

## <a name="grpc"></a>gRPC

[gRPC](https://grpc.io/)：

* 是一種熱門、高效能的 RPC （遠端程序呼叫）架構。
* 提供固定合約優先的 API 開發方法。
* 使用現代化技術，例如：

  * 用於傳輸的 HTTP/2。
  * 通訊協定緩衝區，做為介面描述語言。
  * 二進位序列化格式。
* 提供下列功能：

  * 驗證
  * 雙向串流和流量控制。
  * 取消和超時。

ASP.NET Core 3.0 中的 gRPC 功能包括：

* [Grpc. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; 用來裝載 Grpc 服務的 ASP.NET Core 架構。 ASP.NET Core 上的 gRPC 與標準 ASP.NET Core 功能整合，例如記錄、相依性插入（DI）、驗證和授權。
* [Grpc .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)&ndash; Grpc 用戶端，適用于以熟悉的 `HttpClient`為基礎的 .net Core。
* [Grpc .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; Grpc 與 `HttpClientFactory`的用戶端整合。

如需詳細資訊，請參閱<xref:grpc/index>。

## <a name="signalr"></a>SignalR

如需遷移指示，請參閱[更新 SignalR 程式碼](xref:migration/22-to-30#signalr)。 SignalR 現在會使用 `System.Text.Json` 來序列化/還原序列化 JSON 訊息。 如需還原以 `Newtonsoft.Json`為基礎之序列化程式的指示，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)

在適用于 SignalR 的 JavaScript 和 .NET 用戶端中，已加入自動重新連接的支援。 根據預設，用戶端會嘗試立即重新連線，並在2、10和30秒後重試（如有必要）。 如果用戶端成功重新連接，則會收到新的連線識別碼。 自動重新連線是加入宣告的：

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

藉由傳遞以毫秒為依據的持續時間陣列，可以指定重新連接間隔：

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

您可以傳入自訂的執行，以取得重新連接間隔的完整控制。

如果在上一次重新連線間隔之後失敗重新連接：

* 用戶端會將連接視為離線。
* 用戶端停止嘗試重新連線。

在重新連線嘗試期間，更新應用程式 UI，以通知使用者正在嘗試重新連接。

若要在連接中斷時提供 UI 意見反應，SignalR 用戶端 API 已擴充為包含下列事件處理常式：

* `onreconnecting`：讓開發人員有機會停用 UI，或讓使用者知道應用程式已離線。
* `onreconnected`：讓開發人員有機會在連接重新建立後更新 UI。

下列程式碼會在嘗試連接時，使用 `onreconnecting` 來更新 UI：

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

下列程式碼會使用 `onreconnected` 來更新連接上的 UI：

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

當中樞方法需要授權時，SignalR 3.0 和更新版本會將自訂資源提供給授權處理常式。 資源是 `HubInvocationContext` 的實例。 `HubInvocationContext` 包括：

* `HubCallerContext`
* 所叫用之中樞方法的名稱。
* 中樞方法的引數。

請考慮下列聊天室應用程式範例，讓多個組織能夠透過 Azure Active Directory 進行登入。 具有 Microsoft 帳戶的任何人都可以登入交談，但只有擁有組織的成員可以禁止使用者或觀看使用者的聊天記錄。 應用程式可能會限制特定使用者的特定功能。

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

在上述程式碼中，`DomainRestrictedRequirement` 做為自訂 `IAuthorizationRequirement`。 因為傳入 `HubInvocationContext` 資源參數，所以內部邏輯可以：

* 檢查正在呼叫中樞的內容。
* 請決定是否要讓使用者執行個別的中樞方法。

您可以使用程式碼在執行時間檢查的原則名稱來裝飾個別的中樞方法。 當用戶端嘗試呼叫個別的中樞方法時，`DomainRestrictedRequirement` 處理常式會執行並控制方法的存取。 根據 `DomainRestrictedRequirement` 控制存取的方式：

* 所有登入的使用者都可以呼叫 `SendMessage` 方法。
* 只有使用 `@jabbr.net` 電子郵件地址登入的使用者，才能夠查看使用者的歷程記錄。
* 只有 `bob42@jabbr.net` 可以禁止來自聊天室的使用者。

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

建立 `DomainRestricted` 原則可能牽涉到：

* 在*Startup.cs*中，新增原則。
* 提供自訂 `DomainRestrictedRequirement` 需求做為參數。
* 向授權中介軟體註冊 `DomainRestricted`。

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

SignalR 中樞會使用[端點路由](xref:fundamentals/routing)。 SignalR 中樞連線先前已明確完成：

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

在舊版中，開發人員需要在各種地方連接控制器、Razor 頁面和中樞。 明確連接會產生一系列幾乎相同的路由區段：

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

SignalR 3.0 中樞可以透過端點路由來路由傳送。 透過端點路由，通常可以在 `UseRouting`中設定所有路由：

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

已新增 ASP.NET Core 3.0 SignalR：

用戶端對伺服器串流。 使用用戶端對伺服器串流，伺服器端方法可以接受 `IAsyncEnumerable<T>` 或 `ChannelReader<T>`的實例。 在下列C#範例中，中樞上的 `UploadStream` 方法會接收來自用戶端的字串資料流程：

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

.NET 用戶端應用程式可以將 `IAsyncEnumerable<T>` 或 `ChannelReader<T>` 實例當做上述 `UploadStream` 中樞方法的 `stream` 引數傳遞。

在 `for` 迴圈完成且區域函式結束後，就會傳送資料流程完成：

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

JavaScript 用戶端應用程式會針對上述 `UploadStream` 中樞方法的 `stream` 引數使用 SignalR `Subject` （或[RxJS 主體](https://rxjs.dev/api/index/class/Subject)）。

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

JavaScript 程式碼可以使用 `subject.next` 方法來處理已捕捉並準備傳送至伺服器的字串。

```javascript
subject.next("example");
subject.complete();
```

使用上述兩個程式碼片段這類程式碼，可以建立即時串流體驗。

## <a name="new-json-serialization"></a>新的 JSON 序列化

ASP.NET Core 3.0 現在會使用 <xref:System.Text.Json> JSON 序列化的預設值：

* 非同步讀取和寫入 JSON。
* 已針對 UTF-8 文字進行優化。
* 通常比 `Newtonsoft.Json`更高的效能。

若要將 Json.NET 新增至 ASP.NET Core 3.0，請參閱[新增 Newtonsoft 以 json 為基礎的 json 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。

## <a name="new-razor-directives"></a>新的 Razor 指示詞

下列清單包含新的 Razor 指示詞：

* [@attribute](xref:mvc/views/razor#attribute) &ndash; `@attribute` 指示詞會將指定的屬性套用至所產生頁面或視圖的類別。 例如，`@attribute [Authorize]`。
* [@implements](xref:mvc/views/razor#implements) &ndash; `@implements` 指示詞會為所產生的類別執行介面。 例如，`@implements IDisposable`。

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a>IdentityServer4 支援 web Api 和 Spa 的驗證和授權

ASP.NET Core 3.0 使用 Web API 授權的支援，在單一頁面應用程式（Spa）中提供驗證。 用於驗證和儲存使用者的 ASP.NET Core 身分識別會與[IdentityServer4](https://identityserver.io/)結合，以執行 Open ID Connect。

IdentityServer4 是適用于 ASP.NET Core 3.0 的 OpenID Connect 和 OAuth 2.0 架構。 它會啟用下列安全性功能：

* 驗證即服務（AaaS）
* 多個應用程式類型的單一登入/關閉（SSO）
* Api 的存取控制
* 同盟閘道

如需詳細資訊，請參閱[IdentityServer4 檔](http://docs.identityserver.io/en/latest/index.html)或[spa 的驗證和授權](xref:security/authentication/identity/spa)。

## <a name="certificate-and-kerberos-authentication"></a>憑證和 Kerberos 驗證

憑證驗證需要：

* 正在設定伺服器以接受憑證。
* 在 `Startup.Configure`中新增驗證中介軟體。
* 在 `Startup.ConfigureServices`中新增憑證驗證服務。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

憑證驗證的選項包括下列功能：

* 接受自我簽署憑證。
* 檢查憑證是否已撤銷。
* 檢查好處憑證中是否有正確的使用方式旗標。

預設的使用者主體會從憑證屬性來建立。 使用者主體包含的事件可讓您補充或取代主體。 如需詳細資訊，請參閱<xref:security/authentication/certauth>。

[Windows 驗證](/windows-server/security/windows-authentication/windows-authentication-overview)已擴充到 Linux 和 macOS。 在先前的版本中，Windows 驗證僅限於[IIS](xref:host-and-deploy/iis/index)和[HttpSys](xref:fundamentals/servers/httpsys)。 在 ASP.NET Core 3.0 中， [Kestrel](xref:fundamentals/servers/kestrel)可以在 windows、Linux 和 macOS 上針對已加入網域的 windows 主機使用 Negotiate、 [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview)和[NTLM](/windows-server/security/kerberos/ntlm-overview)。 這些驗證配置的 Kestrel 支援是由 AspNetCore 所提供。 [Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)套件。 如同其他驗證服務，請將驗證應用程式設定為 [寬]，然後設定服務：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

主機需求：

* Windows 主機必須將[服務主體名稱](/windows/win32/ad/service-principal-names)（spn）新增至裝載應用程式的使用者帳戶。
* Linux 和 macOS 機器必須加入網域。
  * 必須為 web 進程建立 Spn。
  * 必須在主機電腦上產生和設定[Keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)檔案。

如需詳細資訊，請參閱<xref:security/authentication/windowsauth>。

## <a name="template-changes"></a>範本變更

Web UI 範本（Razor Pages、具有控制器和 views 的 MVC）已移除下列各項：

* Cookie 同意 UI 已不再包含在內。 若要在 ASP.NET Core 3.0 範本產生的應用程式中啟用 cookie 同意功能，請參閱 <xref:security/gdpr>。
* 腳本和相關的靜態資產現在會當做本機檔案來參考，而不是使用 Cdn。 如需詳細資訊，請參閱[腳本和相關靜態資產現在會當做本機檔案參考，而不是根據目前的環境使用 cdn （aspnet/AspNetCore #14350）](https://github.com/aspnet/AspNetCore.Docs/issues/14350)。

「角度」範本已更新為使用「角度8」。

根據預設，Razor 類別庫（RCL）範本預設為 Razor 元件開發。 Visual Studio 中的新範本選項會提供頁面和視圖的範本支援。 從命令 shell 中的範本建立 RCL 時，請傳遞 `--support-pages-and-views` 選項（`dotnet new razorclasslib --support-pages-and-views`）。

## <a name="generic-host"></a>一般主機

ASP.NET Core 3.0 範本會使用 <xref:fundamentals/host/generic-host>。 先前使用的版本 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。 使用 .NET Core 泛型主機（<xref:Microsoft.Extensions.Hosting.HostBuilder>）可讓 ASP.NET Core 應用程式與其他不是 web 特定的伺服器案例更緊密整合。 如需詳細資訊，請參閱[HostBuilder 取代 WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder)。

### <a name="host-configuration"></a>主機組態

在 ASP.NET Core 3.0 發行之前，會針對 Web 主機的主機設定載入前面加上 `ASPNETCORE_` 的環境變數。 在3.0 中，`AddEnvironmentVariables` 是用來載入前面加上 `DOTNET_` 的環境變數，以使用 `CreateDefaultBuilder`進行主機設定。

### <a name="changes-to-startup-constructor-injection"></a>啟動函式插入的變更

泛型主機僅支援下列類型的 `Startup` 的程式表示式插入：

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

所有服務仍然可以直接插入 `Startup.Configure` 方法的引數。 如需詳細資訊，請參閱[泛型主機限制啟動函數插入（aspnet/公告 #353）](https://github.com/aspnet/Announcements/issues/353)。

## <a name="kestrel"></a>Kestrel

* Kestrel 設定已更新，可供遷移至一般主機。 在3.0 中，Kestrel 是在 `ConfigureWebHostDefaults`所提供的 web 主機產生器上設定。
* 連接介面卡已從 Kestrel 中移除，並以連線中介軟體取代，類似于 ASP.NET Core 管線中的 HTTP 中介軟體，但較低層級的連接。
* Kestrel 傳輸層已公開為 `Connections.Abstractions` 中的公用介面。
* 標頭和尾端之間的多義性已藉由將尾端標頭移至新集合來解決。
* 同步 IO Api （例如 `HttpRequest.Body.Read`）是執行緒耗盡的常見來源，因而導致應用程式當機。 在3.0 中，預設會停用 `AllowSynchronousIO`。

如需詳細資訊，請參閱<xref:migration/22-to-30#kestrel>。

## <a name="http2-enabled-by-default"></a>預設啟用 HTTP/2

在 HTTPS 端點的 Kestrel 中，預設會啟用 HTTP/2。 受作業系統支援時，會啟用 IIS 或 HTTP.sys 的 HTTP/2 支援。

## <a name="eventcounters-on-request"></a>要求 EventCounters

主控 EventSource （`Microsoft.AspNetCore.Hosting`）會發出下列與連入要求相關的新 <xref:System.Diagnostics.Tracing.EventCounter> 類型：

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a>端點路由

端點路由可讓架構（例如 MVC）適用于中介軟體，已增強：

* 中介軟體和端點的順序可在 `Startup.Configure`的要求處理管線中設定。
* 端點和中介軟體會與其他以 ASP.NET Core 為基礎的技術（例如健康狀態檢查）妥善地撰寫。
* 端點可以在中介軟體和 MVC 中執行原則，例如 CORS 或授權。
* 篩選器和屬性可以放在控制器中的方法上。

如需詳細資訊，請參閱<xref:fundamentals/routing#routing-basics>。

## <a name="health-checks"></a>健康情況檢查

健全狀況檢查會搭配泛型主機使用端點路由。 在 `Startup.Configure` 中，使用端點 URL 或相對路徑，在端點產生器上呼叫 `MapHealthChecks`：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

健康情況檢查端點可以：

* 指定一或多個允許的主機/埠。
* 需要授權。
* 需要 CORS。

如需詳細資訊，請參閱下列文章：

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a>HttpCoNtext 上的管道

現在可以使用 <xref:System.IO.Pipelines> API 來讀取要求本文並寫入回應主體。 必須提供 <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> `HttpRequest.BodyReader` 屬性提供可用於讀取要求本文的 <xref:System.IO.Pipelines.PipeReader>。 必須提供 <!-- <xref:Microsoft.AspNetCore.Http.> --> `HttpResponse.BodyWriter` 屬性提供可用於寫入回應主體的 <xref:System.IO.Pipelines.PipeWriter>。 `HttpRequest.BodyReader` 是 `HttpRequest.Body` 串流的類比。 `HttpResponse.BodyWriter` 是 `HttpResponse.Body` 串流的類比。

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a>改善 IIS 中的錯誤報表

在 IIS 中裝載 ASP.NET Core 應用程式時，啟動錯誤現在會產生更豐富的診斷資料。 這些錯誤會在適用的情況下向 Windows 事件記錄檔回報堆疊追蹤。 此外，所有的警告、錯誤和未處理的例外狀況都會記錄到 Windows 事件記錄檔中。

## <a name="worker-service-and-worker-sdk"></a>背景工作服務和背景工作角色 SDK

.NET Core 3.0 引進了新的背景工作服務應用程式範本。 此範本提供在 .NET Core 中撰寫長時間執行服務的起點。

如需詳細資訊，請參閱:

* [.NET Core 背景工作角色做為 Windows 服務](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a>轉送的標頭中介軟體改善

在舊版的 ASP.NET Core 中，當部署到 Azure Linux 或 IIS 以外的任何反向 proxy 後方時，呼叫 <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> 和 <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> 會有問題。 先前版本的修正已記載于[轉送 Linux 和非 IIS 反向 proxy 的配置](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies)中。

此案例已在 ASP.NET Core 3.0 中修正。 當 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`時，主機會啟用[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options)。 在我們的容器映射中，`ASPNETCORE_FORWARDEDHEADERS_ENABLED` 設定為 `true`。

## <a name="performance-improvements"></a>效能改善

ASP.NET Core 3.0 包含許多增強功能，可減少記憶體使用量並改善輸送量：

* 針對範圍服務使用內建的相依性插入容器時，減少記憶體使用量。
* 減少整個架構的配置，包括中介軟體案例和路由。
* 減少 WebSocket 連接的記憶體使用量。
* HTTPS 連線的記憶體減少和輸送量改善。
* 新的優化和完全非同步 JSON 序列化程式。
* 減少在表單剖析中的記憶體使用量和輸送量改善。

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a>ASP.NET Core 3.0 只會在 .NET Core 3.0 上執行

從 ASP.NET Core 3.0，.NET Framework 不再是支援的目標架構。 以 .NET Framework 為目標的專案可以使用[.Net Core 2.1 LTS 版本](https://www.microsoft.com/net/download/dotnet-core/2.1)，以完全支援的方式繼續進行。 大部分的 ASP.NET Core 2.1. x 相關套件會無限期地受到支援，超過 .NET Core 2.1 的三年 LTS 期限。

如需遷移資訊，請參閱[將您的程式碼從 .NET Framework 移植到 .Net Core](/dotnet/core/porting/)。

## <a name="use-the-aspnet-core-shared-framework"></a>使用 ASP.NET Core 共用架構

[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中所包含的 ASP.NET Core 3.0 共用架構，不再需要專案檔中明確的 `<PackageReference />` 元素。 在專案檔中使用 `Microsoft.NET.Sdk.Web` SDK 時，會自動參考共用架構：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a>從 ASP.NET Core 共用架構移除的元件

從 ASP.NET Core 3.0 共用架構中移除的最顯著元件如下：

* [Newtonsoft. Json](https://www.nuget.org/packages/Newtonsoft.Json/) （Json.NET）。 若要將 Json.NET 新增至 ASP.NET Core 3.0，請參閱[新增 Newtonsoft 以 json 為基礎的 json 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。 ASP.NET Core 3.0 引進讀取和寫入 JSON 的 `System.Text.Json`。 如需詳細資訊，請參閱本檔中的[新 JSON 序列化](#new-json-serialization)。
* [Entity Framework Core](/ef/core/)

如需從共用架構中移除之元件的完整清單，請參閱[從 3.0 AspNetCore 中移除的元件](https://github.com/aspnet/AspNetCore/issues/3755)。 如需這項變更動機的詳細資訊，請參閱[3.0 中 AspNetCore 應用程式的重大變更](https://github.com/aspnet/Announcements/issues/325)，以及[第一次介紹 ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)中的變更。

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
