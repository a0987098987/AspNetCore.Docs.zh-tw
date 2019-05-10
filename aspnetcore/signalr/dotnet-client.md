---
title: ASP.NET Core SignalR.NET 用戶端
author: bradygaster
description: ASP.NET Core SignalR.NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: b59af0f9c84a008f778709970dba2273abdfcd4f
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087721"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR.NET 用戶端

ASP.NET Core SignalR.NET 用戶端程式庫可讓您與 SignalR 中樞從.NET 應用程式進行通訊。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。

## <a name="install-the-signalr-net-client-package"></a>安裝 SignalR.NET 用戶端套件

`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。 若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。 中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。 設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。 啟動與連線`StartAsync`。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>中斷連線的控制代碼

使用<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件回應失去連線。 例如，您可能要自動重新連線。

`Closed`事件要求委派，會傳回`Task`，可讓非同步程式碼執行，而不使用`async void`。 為了滿足中的委派簽章`Closed`執行以同步方式傳回的事件處理常式`Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

非同步支援的主要原因是，因此您可以重新連線。 正在開始連線是非同步動作。

在 `Closed`連線，會重新啟動的處理常式考慮一些隨機的延遲，避免多載在伺服器上，等待，如下列範例所示：

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

`InvokeAsync` 集線器上呼叫方法。 將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。 SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync`方法會傳回`Task`伺服器方法傳回時，其完成。 傳回值，如果有的話，可作為結果`Task`。 在伺服器上的方法所擲回任何例外狀況會產生錯誤`Task`。 使用`await`等待完成要求伺服器方法的語法和`try...catch`語法來處理錯誤。

`SendAsync`方法會傳回`Task`其完成，當訊息已傳送到伺服器。 未提供任何傳回的值，因為這`Task`不等候，直到伺服器在方法完成。 用戶端傳送訊息時擲回任何例外狀況會產生錯誤`Task`。 使用`await`和`try...catch`語法來處理將錯誤傳送。

> [!NOTE]
> 如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

處理錯誤的 try / catch 陳述式。 檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [Azure SignalR 服務的無伺服器文件](/azure/azure-signalr/signalr-concept-serverless-development-config)
