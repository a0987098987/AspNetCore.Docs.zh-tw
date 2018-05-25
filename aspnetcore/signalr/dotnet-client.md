---
title: ASP.NET Core SignalR.NET 用戶端
author: rachelappel
description: ASP.NET Core SignalR 的.NET 用戶端的相關資訊
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/18/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/dotnet-client
ms.openlocfilehash: 412d2362575789f1fb4792940df6d3dd24dbdd5a
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2018
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="4772e-103">ASP.NET Core SignalR.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="4772e-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="4772e-104">作者：[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="4772e-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="4772e-105">ASP.NET Core SignalR 的.NET 用戶端可以使用 Xamarin、 WPF、 Windows Form、 主控台與.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4772e-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="4772e-106">像[JavaScript 用戶端](xref:signalr/javascript-client)，.NET 用戶端可讓您接收和傳送和接收訊息至中樞即時。</span><span class="sxs-lookup"><span data-stu-id="4772e-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="4772e-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4772e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4772e-108">本文章中的程式碼範例是使用 ASP.NET Core SignalR 的.NET 用戶端的 WPF 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4772e-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="setup-client"></a><span data-ttu-id="4772e-109">安裝用戶端</span><span class="sxs-lookup"><span data-stu-id="4772e-109">Setup client</span></span>

<span data-ttu-id="4772e-110">`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="4772e-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="4772e-111">若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：</span><span class="sxs-lookup"><span data-stu-id="4772e-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="4772e-112">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="4772e-112">Connect to a hub</span></span>

<span data-ttu-id="4772e-113">若要建立連接時，建立`HubConnectionBuilder`呼叫`Build`。</span><span class="sxs-lookup"><span data-stu-id="4772e-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="4772e-114">建立連接時可以設定中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭和其他選項。</span><span class="sxs-lookup"><span data-stu-id="4772e-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="4772e-115">設定任何必要的選項插入任何`HubConnectionBuilder`方法到`Build`。</span><span class="sxs-lookup"><span data-stu-id="4772e-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="4772e-116">啟動與連線`StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="4772e-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="4772e-117">從用戶端呼叫 hub 方法</span><span class="sxs-lookup"><span data-stu-id="4772e-117">Call hub methods from client</span></span>

<span data-ttu-id="4772e-118">`InvokeAsync` 集線器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="4772e-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="4772e-119">通過集線器方法名稱和中樞方法中定義任何引數`InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="4772e-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="4772e-120">SignalR 是非同步，因此，使用`async`和`await`時進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="4772e-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="4772e-121">從中樞呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="4772e-121">Call client methods from hub</span></span>

<span data-ttu-id="4772e-122">定義的方法使用呼叫中樞`connection.On`之後建築物，但啟動連接之前。</span><span class="sxs-lookup"><span data-stu-id="4772e-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="4772e-123">在上述程式碼`connection.On`時伺服器端程式碼會呼叫它使用執行`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="4772e-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="4772e-124">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="4772e-124">Error handling and logging</span></span>

<span data-ttu-id="4772e-125">處理 try catch 陳述式的錯誤。</span><span class="sxs-lookup"><span data-stu-id="4772e-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="4772e-126">檢查`Exception`物件來判斷發生錯誤之後所採取的適當動作。</span><span class="sxs-lookup"><span data-stu-id="4772e-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="4772e-127">其他資源</span><span class="sxs-lookup"><span data-stu-id="4772e-127">Additional resources</span></span>

* [<span data-ttu-id="4772e-128">中樞</span><span class="sxs-lookup"><span data-stu-id="4772e-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4772e-129">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="4772e-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="4772e-130">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="4772e-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)