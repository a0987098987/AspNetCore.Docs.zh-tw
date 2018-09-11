---
title: ASP.NET Core SignalR.NET 用戶端
author: tdykstra
description: ASP.NET Core SignalR.NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373315"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="5fe0f-103">ASP.NET Core SignalR.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="5fe0f-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="5fe0f-104">作者：[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="5fe0f-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="5fe0f-105">ASP.NET Core SignalR.NET 用戶端程式庫可讓您與 SignalR 中樞從.NET 應用程式進行通訊。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="5fe0f-106">Xamarin 已針對 Visual Studio 版本的特殊需求。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="5fe0f-107">如需詳細資訊，請參閱 <<c0> [ 在 Xamarin 中的 SignalR 用戶端 2.1.1](https://github.com/aspnet/Announcements/issues/305)。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="5fe0f-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5fe0f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5fe0f-109">這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="5fe0f-110">安裝 SignalR.NET 用戶端套件</span><span class="sxs-lookup"><span data-stu-id="5fe0f-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="5fe0f-111">`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="5fe0f-112">若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：</span><span class="sxs-lookup"><span data-stu-id="5fe0f-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="5fe0f-113">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="5fe0f-113">Connect to a hub</span></span>

<span data-ttu-id="5fe0f-114">若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="5fe0f-115">中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="5fe0f-116">設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="5fe0f-117">啟動與連線`StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="5fe0f-118">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="5fe0f-118">Call hub methods from client</span></span>

<span data-ttu-id="5fe0f-119">`InvokeAsync` 集線器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="5fe0f-120">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="5fe0f-121">SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="5fe0f-122">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="5fe0f-122">Call client methods from hub</span></span>

<span data-ttu-id="5fe0f-123">定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="5fe0f-124">在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="5fe0f-125">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="5fe0f-125">Error handling and logging</span></span>

<span data-ttu-id="5fe0f-126">處理錯誤的 try / catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="5fe0f-127">檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。</span><span class="sxs-lookup"><span data-stu-id="5fe0f-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="5fe0f-128">其他資源</span><span class="sxs-lookup"><span data-stu-id="5fe0f-128">Additional resources</span></span>

* [<span data-ttu-id="5fe0f-129">中樞</span><span class="sxs-lookup"><span data-stu-id="5fe0f-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="5fe0f-130">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="5fe0f-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="5fe0f-131">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="5fe0f-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
