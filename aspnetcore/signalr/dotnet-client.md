---
title: ASP.NET Core SignalR.NET 用戶端
author: tdykstra
description: ASP.NET Core SignalR.NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ce5be911e67831cbf6c09e24744111e73ffdbe63
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095030"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR.NET 用戶端

作者：[Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR.NET 用戶端可以使用 Xamarin、 WPF、 Windows Form、 主控台和.NET Core 應用程式。 像是[JavaScript 用戶端](xref:signalr/javascript-client)，.NET 用戶端可讓您接收和傳送和接收訊息到中樞即時。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。

## <a name="install-the-signalr-net-client-package"></a>安裝 SignalR.NET 用戶端套件

`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。 若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。 中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。 設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。 啟動與連線`StartAsync`。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

`InvokeAsync` 集線器上呼叫方法。 將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。 SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

處理錯誤的 try / catch 陳述式。 檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
