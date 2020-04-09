---
title: ASP.NET核心 3.0 中的新增功能
author: rick-anderson
description: 瞭解 ASP.NET 酷 3.0 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.0
ms.openlocfilehash: 4886673a9b16b8be8d9a0b0d5c7002a91760544e
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80976972"
---
# <a name="whats-new-in-aspnet-core-30"></a>ASP.NET核心 3.0 中的新增功能

本文重點介紹了 ASP.NET 酷 3.0 中最重要的更改,並包含指向相關文檔的連結。

## Blazor

Blazor是ASP.NET核心中用於使用 .NET 建構式互動式客戶端 Web UI 的新框架:

* 使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。
* 共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。
* 將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。

Blazor框架支援的機制:

* 可重用的 UI 元件(Razor 元件)
* 用戶端路由
* 元件佈局
* 支援相依項
* 表單和驗證
* 使用 Razor 函式庫建構元件庫
* JavaScript Interop

如需詳細資訊，請參閱 <xref:blazor/index>。

### <a name="opno-locblazor-server"></a>Blazor伺服器

Blazor將元件呈現邏輯與 UI 更新的應用方式分離。 Blazor伺服器支援在 ASP.NET核心應用中在伺服器上託管 Razor 元件。 UI 更新SignalR通過 連接處理。 BlazorASP.NET核心 3.0 中支援伺服器。

### <a name="opno-locblazor-webassembly-preview"></a>Blazor網路組裝(預覽)

Blazor應用也可以使用基於 Web 大會的 .NET 運行時直接在瀏覽器中運行。 BlazorWeb組裝處於預覽狀態,ASP.NET酷睿 3.0 中*不支援*Web 組裝。 BlazorWeb組裝將在將來發佈的ASP.NET核心版中得到支援。

### <a name="razor-components"></a>剃刀元件

Blazor應用程式是從元件構建的。 元件是用戶介面 (UI) 的自包含塊,如頁面、對話框或窗體。 元件是定義 UI 呈現邏輯和用戶端事件處理程式的正常 .NET 類。 無需 JAVAScript 即可創建豐富的互動式 Web 應用。

中的Blazor元件通常使用 Razor 語法創作,這是 HTML 和 C# 的自然混合。 剃刀元件類似於剃刀頁面和 MVC 視圖,因為它們都使用 Razor。 與基於請求-回應模型的頁面和視圖不同,元件專門用於處理 UI 組合。

## <a name="grpc"></a>gRPC

[gRPC](https://grpc.io/):

* 是一個流行的高性能 RPC(遠端過程調用)框架。
* 為 API 開發提供了一種有意見的合同優先方法。
* 使用現代技術,例如:

  * 用於傳輸的 HTTP/2。
  * 協定緩衝區作為介面描述語言。
  * 二進位序列化格式。
* 提供如下功能:

  * 驗證
  * 雙向流和流控制。
  * 取消和超時。

ASP.NET核心 3.0 中的 gRPC 功能包括:

* [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)&ndash;用於託管 gRPC 服務ASP.NET核心框架。 ASP.NET酷睿上的 gRPC 與標準ASP.NET核心功能(如日誌記錄、依賴項注入 (DI)、身份驗證和授權)集成。
* [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; gRPC 客戶端,用於 .NET Core,`HttpClient`該用戶端基於熟悉的 。
* [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC`HttpClientFactory`用戶端整合。

如需詳細資訊，請參閱 <xref:grpc/index>。

## SignalR

有關移轉說明,請參閱[更新SignalR程式碼](xref:migration/22-to-30#signalr)。 SignalR現在用於`System.Text.Json`序列化/去序列化 JSON 消息。 有關還原基於`Newtonsoft.Json`序列化器的說明,請參閱[切換到 Newtonsoft.Json。](xref:migration/22-to-30#switch-to-newtonsoftjson)

在 JavaScript 和SignalR.NET 用戶端中,添加了自動重新連接的支援。 默認情況下,客戶端嘗試立即重新連接,並在必要時在 2、10 和 30 秒後重試。 如果用戶端成功重新連接,它將收到一個新的連接 ID。 自動重新連線是選擇加入的:

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

可以通過傳遞基於毫秒的持續時間陣列來指定重新連接間隔:

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

可以傳入自定義實現以完全控制重新連接間隔。

如果重新連線在最後一次重新連接間隔後失敗:

* 客戶端認為連接處於離線狀態。
* 用戶端停止嘗試重新連接。

在重新連接嘗試期間,更新應用 UI 以通知使用者正在嘗試重新連接。

為了在連接中斷時提供 UISignalR回饋, 用戶端 API 已展開,以包括以下事件處理程式:

* `onreconnecting`:讓開發人員有機會禁用 UI 或讓使用者知道應用處於脫機狀態。
* `onreconnected`:使開發人員有機會在重新建立連接后更新 UI。

以下代碼用於`onreconnecting`在嘗試連接時更新 UI:

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

以下代碼用於`onreconnected`在連線時更新 UI:

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

SignalR當中心方法需要授權時,3.0 及更高版本向授權處理程式提供自定義資源。 資源是的`HubInvocationContext`實例。 包含`HubInvocationContext`:

* `HubCallerContext`
* 要調用的集線器方法的名稱。
* 對中心方法的參數。

請考慮以下聊天室應用示例,該應用允許通過 Azure 活動目錄登錄多個組織。 擁有 Microsoft 帳戶的任何人都可以登錄聊天,但只有擁有組織的成員可以禁止使用者或查看使用者的聊天歷史記錄。 該應用程式可能會限制特定使用者的某些功能。

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

在前面的代碼中,`DomainRestrictedRequirement`用作自`IAuthorizationRequirement`訂 。 由於`HubInvocationContext`資源參數正在傳入,因此內部邏輯可以:

* 檢查調用集線器的上下文。
* 決定允許使用者執行單個中心方法。

單個中心方法可以使用代碼在運行時檢查的策略名稱進行標記。 當客戶端嘗試呼叫單個中心方法時`DomainRestrictedRequirement`, 處理程式將運行和控制對這些方法的訪問。 依`DomainRestrictedRequirement`控制項存取方式:

* 所有登入使用者可以呼叫`SendMessage`該方法 。
* 只有使用`@jabbr.net`電子郵件地址登錄的使用者才能查看使用者的歷史。
* 只能`bob42@jabbr.net`禁止用戶離開聊天室。

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

建立`DomainRestricted`策略可能涉及:

* 在*Startup.cs,* 添加新政策。
* 將自定義`DomainRestrictedRequirement`要求作為參數提供。
* `DomainRestricted`註冊授權中間件。

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

SignalR集線器使用[連接端點路由](xref:fundamentals/routing)。 SignalR中心連線以前顯示式完成:

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

在以前的版本中,開發人員需要將控制器、Razor 頁面和集線器連接到不同位置。 明確連線會導致一系列幾乎相同的路由段:

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

SignalR3.0 集線器可通過端點路由進行路由。 使用終結點路由時,通常可以在`UseRouting`中 配置所有路由:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

ASP.NET核心 3.0SignalR新增了:

用戶端到伺服器流。 使用用戶端到伺服器流式處理時,伺服器端方法可以採用或`IAsyncEnumerable<T>``ChannelReader<T>`的實例。 在以下 C# 範例中,Hub`UploadStream`上的方法將從客戶端接收字串流:

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

.NET 客戶端應用可以`IAsyncEnumerable<T>``ChannelReader<T>`將 或`stream`實例作為`UploadStream`上面 的 Hub 方法的參數傳遞。

循環`for`完成並離開本地函數後,將傳送流完成:

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

JavaScript 用戶端應用SignalR`Subject`使用 (或[RxJS 主題](https://rxjs.dev/api/index/class/Subject)`stream`) 進行`UploadStream`上述中心方法的參數。

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

JavaScript 程式碼可以`subject.next`使用 方法 處理字串捕獲並準備發送到伺服器。

```javascript
subject.next("example");
subject.complete();
```

使用前兩個代碼段等代碼,可以創建即時流式處理體驗。

## <a name="new-json-serialization"></a>新的 JSON 序列化

ASP.NET Core 3.0<xref:System.Text.Json>現在預設用於 JSON 序列化:

* 非同步讀取和寫入 JSON。
* 針對 UTF-8 文本進行了優化。
* 效能通常高於`Newtonsoft.Json`。

要將Json.NET新增到ASP.NET核心 3.0,請參閱[新增 Newtonsoft.基於 Json 的 JSON 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。

## <a name="new-razor-directives"></a>新的剃刀指令

以下清單包含新的 Razor 指令:

* [`@attribute`](xref:mvc/views/razor#attribute)&ndash;該`@attribute`指令將給定的屬性應用於生成的頁面或視圖的類。 例如： `@attribute [Authorize]` 。
* [`@implements`](xref:mvc/views/razor#implements)&ndash;該`@implements`指令為生成的類實現介面。 例如： `@implements IDisposable` 。

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a>識別伺服器4支援對 Web API 與 SA 的驗證與授權

ASP.NET Core 3.0 使用 Web API 授權支援,在單頁應用 (SPA) 中提供身份驗證。 ASP.NET用於驗證和存儲使用者的核心標識與[標識Server4](https://identityserver.io/)相結合,用於實現開放ID連接。

標識伺服器4 是一個 OpenID 連接和 OAuth 2.0 框架,用於ASP.NET核心 3.0。 它支援以下安全功能:

* 為服務身份驗證 (AaaS)
* 跨多個應用程式類型的單一登入 /關閉 (SSO)
* API 的存取控制
* 聯合閘道

有關詳細資訊,請參閱[識別Server4 文件](http://docs.identityserver.io/en/latest/index.html)或[SA 的身份驗證和授權](xref:security/authentication/identity/spa)。

## <a name="certificate-and-kerberos-authentication"></a>憑證與 Kerberos 驗證

憑證認證要求:

* 配置伺服器以接受證書。
* 在`Startup.Configure`中 添加身份驗證中間件。
* 在`Startup.ConfigureServices`中 添加證書身份驗證服務。

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

憑證認證選項包括:

* 接受自簽名證書。
* 檢查證書吊銷。
* 檢查提供的證書中具有正確的使用標誌。

默認用戶主體是從證書屬性構造的。 用戶主體包含一個事件,該事件允許補充或替換主體。 如需詳細資訊，請參閱 <xref:security/authentication/certauth>。

[Windows 身份驗證](/windows-server/security/windows-authentication/windows-authentication-overview)已擴展到 Linux 和 macOS。 在以前的版本中,Windows 身份驗證僅限於[IIS](xref:host-and-deploy/iis/index)和[Hsys](xref:fundamentals/servers/httpsys)。 在ASP.NET核心3.0中[,Kestrel](xref:fundamentals/servers/kestrel)能夠在Windows、Linux和macOS上為Windows網域加入的主機使用協商[、Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview)和[NTLM。](/windows-server/security/kerberos/ntlm-overview) Kestrel 對這些身份驗證方案的支援由[Microsoft.AspNetCore.身份驗證.協商 NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)包提供。 與其他身份驗證服務一樣,在寬配置身份驗證應用,然後配置服務:

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

主機要求:

* Windows 主機必須將[服務主體名稱](/windows/win32/ad/service-principal-names)(SPN) 添加到託管應用的使用者帳戶。
* Linux 和macOS電腦必須加入域。
  * 必須為 Web 進程創建 SPN。
  * 必須在主機上產生與設定[Keytab 檔案](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)。

如需詳細資訊，請參閱 <xref:security/authentication/windowsauth>。

## <a name="template-changes"></a>樣本變更

Web UI 樣本(Razor 頁面、帶控制器和檢視的 MVC)已刪除以下內容:

* Cookie 同意 UI 不再包括在內。 要在 ASP.NET酷睿 3.0 範本生成的應用中啟用<xref:security/gdpr>Cookie 同意 功能,請參閱。
* 文本和相關靜態資產現在被引用為本地檔案,而不是使用CDN。 有關詳細資訊,請參閱[文本和相關靜態資產現在被引用為本地檔,而不是基於當前環境(aspnet/AspNetCore.Docs #14350)使用CDN。](https://github.com/dotnet/AspNetCore.Docs/issues/14350)

角度範本更新為使用角 8。

默認情況下,Razor 類庫 (RCL) 範本預設為 Razor 元件開發。 Visual Studio 中的新範本選項為頁面和檢視提供範本支援。 從命令 shell 中的樣本建立 RCL`--support-pages-and-views`時`dotnet new razorclasslib --support-pages-and-views`,傳遞選項 ( 。

## <a name="generic-host"></a>一般主機

ASP.NET核心 3.0 樣本<xref:fundamentals/host/generic-host>使用 。 以前使用<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>的版本。 使用 .NET 核心通用<xref:Microsoft.Extensions.Hosting.HostBuilder>主機 ( ) 可更好地將 ASP.NET 核心應用與其他非 Web 特定的伺服器方案整合。 有關詳細資訊,請參閱[主機產生器取代 WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder)。

### <a name="host-configuration"></a>主機組態

在 ASP.NET 酷 3.0 發佈之前,`ASPNETCORE_`已載入預 固定的環境變數,用於 Web 主機的主機配置。 在 3.0`AddEnvironmentVariables`中,用於載入預固定`DOTNET_``CreateDefaultBuilder`的具有 主機配置的環境變數。

### <a name="changes-to-startup-constructor-injection"></a>對啟動建構函式注入的變更

泛型主機僅支援建構`Startup`函數注入的以下類型:

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

所有服務仍可以直接作為參數注入到方法。 `Startup.Configure` 有關詳細資訊,請參閱[通用主機限制啟動構造函數注入(aspnet/公告#353)](https://github.com/aspnet/Announcements/issues/353)。

## <a name="kestrel"></a>Kestrel

* 已更新 Kestrel 配置,以便遷移到通用主機。 在 3.0 中,Kestrel`ConfigureWebHostDefaults`配置在 提供的 Web 主機生成器上。
* 連接配配器已從 Kestrel 中刪除,並替換為連接中間件,這與 ASP.NET核心管道中的 HTTP 中間件類似,但對於較低級別的連接。
* Kestrel 傳輸層已作為 公共介面`Connections.Abstractions`在 中 公開。
* 通過將尾隨標頭移動到新集合,解決了標頭和尾部之間的歧義。
* 同步 I/O API(如`HttpRequest.Body.Read`)是導致應用崩潰的常見線程不足源。 在 3.0`AllowSynchronousIO`中,默認情況下處於禁用狀態。

如需詳細資訊，請參閱 <xref:migration/22-to-30#kestrel>。

## <a name="http2-enabled-by-default"></a>預設的功能啟用 HTTP/2

默認情況下,HTTP/2 在 KEStrel 中為 HTTPS 終結點啟用。 當作業系統支援時,將啟用對 IIS 或 HTTP.sys 的 HTTP/2 支援。

## <a name="eventcounters-on-request"></a>應要求的事件計數器

主主事件來源`Microsoft.AspNetCore.Hosting`傳入要求相關的以下新<xref:System.Diagnostics.Tracing.EventCounter>型態:

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a>端點路由

終結點路由,允許框架(例如,MVC)很好地與中間件配合使用,得到了增強:

* 中間件和端點的順序在`Startup.Configure`的請求處理管道中可配置。
* 端點和中間件與其他基於核心的技術(如運行狀況檢查)很好地組成了ASP.NET。
* 端點可以在中間件和 MVC 中實現策略,如 CORS 或授權。
* 篩選器和屬性可以放置在控制器中的方法上。

如需詳細資訊，請參閱 <xref:fundamentals/routing#routing-basics>。

## <a name="health-checks"></a>健康情況檢查

運行狀況檢查使用與通用主機的終結點路由。 在`Startup.Configure`中`MapHealthChecks`,使用終結點網址 或相對路徑呼叫終結點產生器:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

執行狀況檢查終結點可以:

* 指定一個或多個允許的主機/埠。
* 需要授權。
* 需要 CORS。

如需詳細資訊，請參閱下列文章：

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a>HTTPContext 上的導管

現在可以讀取請求正文並使用<xref:System.IO.Pipelines>API 寫入回應正文。 此 <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> `HttpRequest.BodyReader`屬性提供可用於<xref:System.IO.Pipelines.PipeReader>讀取要求正文的 。 此 <!-- <xref:Microsoft.AspNetCore.Http.> --> `HttpResponse.BodyWriter`屬性提供可用於<xref:System.IO.Pipelines.PipeWriter>寫入回應正文的 。 `HttpRequest.BodyReader`是`HttpRequest.Body`流的類比。 `HttpResponse.BodyWriter`是`HttpResponse.Body`流的類比。

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a>改進 IIS 中的錯誤報告

在IIS中託管ASP.NET核心應用時啟動錯誤現在會產生更豐富的診斷數據。 這些錯誤將報告給 Windows 事件日誌,在適用的情況下具有堆疊跟蹤。 此外,所有警告、錯誤和未處理的異常將記錄到 Windows 事件日誌。

## <a name="worker-service-and-worker-sdk"></a>協助服務與輔助角色 SDK

.NET Core 3.0 引入了新的輔助服務應用範本。 此範本為在 .NET Core 中編寫長時間運行的服務提供了一個起點。

如需詳細資訊，請參閱

* [.NET 核心工作人員作為 Windows 服務](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a>轉寄的標頭 中間件改進

在以前版本的ASP.NET核心中,在<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>部署到<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>Azure Linux或IIS以外的任何反向代理後面時,調用和調用都存在問題。 早期版本的修復程序記錄在[Linux 和非 IIS 反向代理的方案](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies)中。

此方案在ASP.NET核心3.0中修復。 當環境變數設定為`ASPNETCORE_FORWARDEDHEADERS_ENABLED``true`時,主機啟用[轉寄的標頭中間件](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options)。 `ASPNETCORE_FORWARDEDHEADERS_ENABLED`設置為`true`我們的容器映射。

## <a name="performance-improvements"></a>效能改善

ASP.NET酷睿 3.0 包含許多改進,可降低記憶體使用並提高輸送量:

* 使用內置依賴項注入容器進行作用域服務時,記憶體使用量減少。
* 減少整個框架的分配,包括中間件方案和路由。
* 減少 WebSocket 連接的記憶體使用量。
* HTTPS 連接的記憶體縮減和輸送量改進。
* 新的優化和完全異步的 JSON 序列化器。
* 減少記憶體使用和表單分析的輸送量改進。

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a>ASP.NET核心 3.0 僅在 .NET Core 3.0 上執行

從 ASP.NET核心 3.0 起,.NET 框架不再是受支持的目標框架。 目標 .NET 框架的專案可以使用[.NET Core 2.1 LTS 版本](https://dotnet.microsoft.com/download/dotnet-core/2.1)以完全支援的方式繼續。 大多數ASP.NET核心 2.1.x 相關包將無限期地支援,超過 .NET Core 2.1 的三年 LTS 期限。

有關移轉資訊,請參閱[將代碼從 .NET 框架移植到 .NET 核心](/dotnet/core/porting/)。

## <a name="use-the-aspnet-core-shared-framework"></a>使用ASP.NET核心共用框架

ASP.NET Core 3.0 共用框架包含在[Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)中`<PackageReference />`,不再需要專案檔中 的顯式元素。 在專案檔中使用 SDK`Microsoft.NET.Sdk.Web`時, 將自動參考共用框架:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a>從ASP.NET核心共用框架中移除的程式集

從 ASP.NET Core 3.0 共用框架中刪除的最值得注意的程式集是:

* [牛頓軟.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET)。 要將Json.NET新增到ASP.NET核心 3.0,請參閱[新增 Newtonsoft.基於 Json 的 JSON 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。 ASP.NET核心3.0介紹`System.Text.Json`閱讀和寫作JSON。 有關詳細資訊,請參閱本文件中[的新 JSON 序列化](#new-json-serialization)。
* [Entity Framework Core](/ef/core/)

有關從共用框架中刪除的程式集的完整清單,請參閱從[Microsoft 中刪除的程式集。](https://github.com/dotnet/AspNetCore/issues/3755) 有關此更改的動機的詳細資訊,請參閱[3.0 中對 Microsoft.AspNetCore.App 的中斷更改](https://github.com/aspnet/Announcements/issues/325),並[首先查看 ASP.NET酷 3.0 中即將出現的變化](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)。

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
 