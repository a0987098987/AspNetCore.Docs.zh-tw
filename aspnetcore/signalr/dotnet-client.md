---
title: ASP.NET Core SignalR.NET 用戶端
author: bradygaster
description: ASP.NET Core SignalR.NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/dotnet-client
ms.openlocfilehash: a03abef53aa44f0a1016b8f72d8e3a7af2f9bed1
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978300"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="6824e-103">ASP.NET Core SignalR.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="6824e-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="6824e-104">ASP.NET Core SignalR.NET 用戶端程式庫可讓您與 SignalR 中樞從.NET 應用程式進行通訊。</span><span class="sxs-lookup"><span data-stu-id="6824e-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="6824e-105">Xamarin 已針對 Visual Studio 版本的特殊需求。</span><span class="sxs-lookup"><span data-stu-id="6824e-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="6824e-106">如需詳細資訊，請參閱[在 Xamarin 中的 SignalR 用戶端 2.1.1](https://github.com/aspnet/Announcements/issues/305)。</span><span class="sxs-lookup"><span data-stu-id="6824e-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="6824e-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6824e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6824e-108">這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6824e-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="6824e-109">安裝 SignalR.NET 用戶端套件</span><span class="sxs-lookup"><span data-stu-id="6824e-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="6824e-110">`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="6824e-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="6824e-111">若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：</span><span class="sxs-lookup"><span data-stu-id="6824e-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="6824e-112">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="6824e-112">Connect to a hub</span></span>

<span data-ttu-id="6824e-113">若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。</span><span class="sxs-lookup"><span data-stu-id="6824e-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="6824e-114">中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。</span><span class="sxs-lookup"><span data-stu-id="6824e-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="6824e-115">設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。</span><span class="sxs-lookup"><span data-stu-id="6824e-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="6824e-116">啟動與連線`StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="6824e-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="6824e-117">中斷連線的控制代碼</span><span class="sxs-lookup"><span data-stu-id="6824e-117">Handle lost connection</span></span>

<span data-ttu-id="6824e-118">使用<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件回應失去連線。</span><span class="sxs-lookup"><span data-stu-id="6824e-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="6824e-119">例如，您可能要自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="6824e-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="6824e-120">`Closed`事件要求委派，會傳回`Task`，可讓非同步程式碼執行，而不使用`async void`。</span><span class="sxs-lookup"><span data-stu-id="6824e-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="6824e-121">為了滿足中的委派簽章`Closed`執行以同步方式傳回的事件處理常式`Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="6824e-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="6824e-122">非同步支援的主要原因是，因此您可以重新連線。</span><span class="sxs-lookup"><span data-stu-id="6824e-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="6824e-123">正在開始連線是非同步動作。</span><span class="sxs-lookup"><span data-stu-id="6824e-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="6824e-124">在 `Closed`連線，會重新啟動的處理常式考慮一些隨機的延遲，避免多載在伺服器上，等待，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="6824e-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="6824e-125">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="6824e-125">Call hub methods from client</span></span>

<span data-ttu-id="6824e-126">`InvokeAsync` 集線器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="6824e-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="6824e-127">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="6824e-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="6824e-128">SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="6824e-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

> [!NOTE]
> <span data-ttu-id="6824e-129">如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="6824e-129">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="6824e-130">如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="6824e-130">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="6824e-131">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="6824e-131">Call client methods from hub</span></span>

<span data-ttu-id="6824e-132">定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。</span><span class="sxs-lookup"><span data-stu-id="6824e-132">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="6824e-133">在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="6824e-133">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="6824e-134">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="6824e-134">Error handling and logging</span></span>

<span data-ttu-id="6824e-135">處理錯誤的 try / catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="6824e-135">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="6824e-136">檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。</span><span class="sxs-lookup"><span data-stu-id="6824e-136">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="6824e-137">其他資源</span><span class="sxs-lookup"><span data-stu-id="6824e-137">Additional resources</span></span>

* [<span data-ttu-id="6824e-138">中樞</span><span class="sxs-lookup"><span data-stu-id="6824e-138">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6824e-139">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="6824e-139">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="6824e-140">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="6824e-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="6824e-141">Azure SignalR 服務的無伺服器文件</span><span class="sxs-lookup"><span data-stu-id="6824e-141">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
