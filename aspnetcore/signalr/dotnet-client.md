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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="8e628-103">ASP.NET Core SignalR.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="8e628-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="8e628-104">ASP.NET Core SignalR.NET 用戶端程式庫可讓您與 SignalR 中樞從.NET 應用程式進行通訊。</span><span class="sxs-lookup"><span data-stu-id="8e628-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="8e628-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e628-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8e628-106">這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e628-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="8e628-107">安裝 SignalR.NET 用戶端套件</span><span class="sxs-lookup"><span data-stu-id="8e628-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="8e628-108">`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="8e628-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="8e628-109">若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：</span><span class="sxs-lookup"><span data-stu-id="8e628-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="8e628-110">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="8e628-110">Connect to a hub</span></span>

<span data-ttu-id="8e628-111">若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。</span><span class="sxs-lookup"><span data-stu-id="8e628-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="8e628-112">中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。</span><span class="sxs-lookup"><span data-stu-id="8e628-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="8e628-113">設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。</span><span class="sxs-lookup"><span data-stu-id="8e628-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="8e628-114">啟動與連線`StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="8e628-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="8e628-115">中斷連線的控制代碼</span><span class="sxs-lookup"><span data-stu-id="8e628-115">Handle lost connection</span></span>

<span data-ttu-id="8e628-116">使用<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件回應失去連線。</span><span class="sxs-lookup"><span data-stu-id="8e628-116">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="8e628-117">例如，您可能要自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="8e628-117">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="8e628-118">`Closed`事件要求委派，會傳回`Task`，可讓非同步程式碼執行，而不使用`async void`。</span><span class="sxs-lookup"><span data-stu-id="8e628-118">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="8e628-119">為了滿足中的委派簽章`Closed`執行以同步方式傳回的事件處理常式`Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="8e628-119">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="8e628-120">非同步支援的主要原因是，因此您可以重新連線。</span><span class="sxs-lookup"><span data-stu-id="8e628-120">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="8e628-121">正在開始連線是非同步動作。</span><span class="sxs-lookup"><span data-stu-id="8e628-121">Starting a connection is an async action.</span></span>

<span data-ttu-id="8e628-122">在 `Closed`連線，會重新啟動的處理常式考慮一些隨機的延遲，避免多載在伺服器上，等待，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8e628-122">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="8e628-123">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="8e628-123">Call hub methods from client</span></span>

<span data-ttu-id="8e628-124">`InvokeAsync` 集線器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="8e628-124">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="8e628-125">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="8e628-125">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="8e628-126">SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="8e628-126">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="8e628-127">`InvokeAsync`方法會傳回`Task`伺服器方法傳回時，其完成。</span><span class="sxs-lookup"><span data-stu-id="8e628-127">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="8e628-128">傳回值，如果有的話，可作為結果`Task`。</span><span class="sxs-lookup"><span data-stu-id="8e628-128">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="8e628-129">在伺服器上的方法所擲回任何例外狀況會產生錯誤`Task`。</span><span class="sxs-lookup"><span data-stu-id="8e628-129">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="8e628-130">使用`await`等待完成要求伺服器方法的語法和`try...catch`語法來處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e628-130">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="8e628-131">`SendAsync`方法會傳回`Task`其完成，當訊息已傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="8e628-131">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="8e628-132">未提供任何傳回的值，因為這`Task`不等候，直到伺服器在方法完成。</span><span class="sxs-lookup"><span data-stu-id="8e628-132">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="8e628-133">用戶端傳送訊息時擲回任何例外狀況會產生錯誤`Task`。</span><span class="sxs-lookup"><span data-stu-id="8e628-133">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="8e628-134">使用`await`和`try...catch`語法來處理將錯誤傳送。</span><span class="sxs-lookup"><span data-stu-id="8e628-134">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="8e628-135">如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="8e628-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="8e628-136">如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="8e628-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="8e628-137">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="8e628-137">Call client methods from hub</span></span>

<span data-ttu-id="8e628-138">定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。</span><span class="sxs-lookup"><span data-stu-id="8e628-138">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="8e628-139">在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="8e628-139">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="8e628-140">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="8e628-140">Error handling and logging</span></span>

<span data-ttu-id="8e628-141">處理錯誤的 try / catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="8e628-141">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="8e628-142">檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。</span><span class="sxs-lookup"><span data-stu-id="8e628-142">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="8e628-143">其他資源</span><span class="sxs-lookup"><span data-stu-id="8e628-143">Additional resources</span></span>

* [<span data-ttu-id="8e628-144">中樞</span><span class="sxs-lookup"><span data-stu-id="8e628-144">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8e628-145">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="8e628-145">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="8e628-146">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="8e628-146">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="8e628-147">Azure SignalR 服務的無伺服器文件</span><span class="sxs-lookup"><span data-stu-id="8e628-147">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
