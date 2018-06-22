---
title: ASP.NET Core SignalR.NET 用戶端
author: rachelappel
description: ASP.NET Core SignalR 的.NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273291"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR.NET 用戶端

作者：[Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR 的.NET 用戶端可以使用 Xamarin、 WPF、 Windows Form、 主控台與.NET Core 應用程式。 像[JavaScript 用戶端](xref:signalr/javascript-client)，.NET 用戶端可讓您接收和傳送和接收訊息至中樞即時。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

本文章中的程式碼範例是使用 ASP.NET Core SignalR 的.NET 用戶端的 WPF 應用程式。

## <a name="install-the-signalr-net-client-package"></a>SignalR 的.NET 用戶端封裝安裝

`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。 若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立連接時，建立`HubConnectionBuilder`呼叫`Build`。 建立連接時可以設定中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭和其他選項。 設定任何必要的選項插入任何`HubConnectionBuilder`方法到`Build`。 啟動與連線`StartAsync`。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫 hub 方法

`InvokeAsync` 集線器上呼叫方法。 通過集線器方法名稱和中樞方法中定義任何引數`InvokeAsync`。 SignalR 是非同步，因此，使用`async`和`await`時進行呼叫。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>從中樞呼叫用戶端方法

定義的方法使用呼叫中樞`connection.On`之後建築物，但啟動連接之前。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

在上述程式碼`connection.On`時伺服器端程式碼會呼叫它使用執行`SendAsync`方法。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

處理 try catch 陳述式的錯誤。 檢查`Exception`物件來判斷發生錯誤之後所採取的適當動作。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)