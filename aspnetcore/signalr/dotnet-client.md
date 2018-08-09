---
title: ASP.NET Core SignalR.NET 用戶端
author: tdykstra
description: ASP.NET Core SignalR.NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655248"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="a7359-103">ASP.NET Core SignalR.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="a7359-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="a7359-104">作者：[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="a7359-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="a7359-105">ASP.NET Core SignalR.NET 用戶端可以使用 Xamarin、 WPF、 Windows Form、 主控台和.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a7359-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="a7359-106">像是[JavaScript 用戶端](xref:signalr/javascript-client)，.NET 用戶端可讓您接收和傳送和接收訊息到中樞即時。</span><span class="sxs-lookup"><span data-stu-id="a7359-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="a7359-107">Xamarin 已針對 Visual Studio 版本的特殊需求。</span><span class="sxs-lookup"><span data-stu-id="a7359-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="a7359-108">如需詳細資訊，請參閱 <<c0> [ 在 Xamarin 中的 SignalR 用戶端 2.1.1](https://github.com/aspnet/Announcements/issues/305)。</span><span class="sxs-lookup"><span data-stu-id="a7359-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="a7359-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7359-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a7359-110">這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a7359-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="a7359-111">安裝 SignalR.NET 用戶端套件</span><span class="sxs-lookup"><span data-stu-id="a7359-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="a7359-112">`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="a7359-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="a7359-113">若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：</span><span class="sxs-lookup"><span data-stu-id="a7359-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="a7359-114">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="a7359-114">Connect to a hub</span></span>

<span data-ttu-id="a7359-115">若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。</span><span class="sxs-lookup"><span data-stu-id="a7359-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="a7359-116">中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。</span><span class="sxs-lookup"><span data-stu-id="a7359-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="a7359-117">設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。</span><span class="sxs-lookup"><span data-stu-id="a7359-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="a7359-118">啟動與連線`StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="a7359-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="a7359-119">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="a7359-119">Call hub methods from client</span></span>

<span data-ttu-id="a7359-120">`InvokeAsync` 集線器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="a7359-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="a7359-121">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="a7359-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="a7359-122">SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="a7359-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="a7359-123">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="a7359-123">Call client methods from hub</span></span>

<span data-ttu-id="a7359-124">定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。</span><span class="sxs-lookup"><span data-stu-id="a7359-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="a7359-125">在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="a7359-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="a7359-126">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="a7359-126">Error handling and logging</span></span>

<span data-ttu-id="a7359-127">處理錯誤的 try / catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="a7359-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="a7359-128">檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。</span><span class="sxs-lookup"><span data-stu-id="a7359-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="a7359-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="a7359-129">Additional resources</span></span>

* [<span data-ttu-id="a7359-130">中樞</span><span class="sxs-lookup"><span data-stu-id="a7359-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a7359-131">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="a7359-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a7359-132">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="a7359-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
