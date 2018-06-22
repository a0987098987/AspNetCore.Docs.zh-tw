---
title: 管理 SignalR 中使用者和群組
author: rachelappel
description: ASP.NET Core SignalR 使用者和群組管理的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272077"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="b2edf-103">管理 SignalR 中使用者和群組</span><span class="sxs-lookup"><span data-stu-id="b2edf-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="b2edf-104">由[Brennan 瑜吉](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="b2edf-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="b2edf-105">SignalR 允許訊息傳送至所有與特定使用者相關聯的連接以及名為群組的連線。</span><span class="sxs-lookup"><span data-stu-id="b2edf-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="b2edf-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b2edf-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="b2edf-107">SignalR 的使用者</span><span class="sxs-lookup"><span data-stu-id="b2edf-107">Users in SignalR</span></span>

<span data-ttu-id="b2edf-108">SignalR 可讓您將訊息傳送至所有與特定使用者相關聯的連接。</span><span class="sxs-lookup"><span data-stu-id="b2edf-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="b2edf-109">根據預設使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`的使用者識別碼與連接相關聯。</span><span class="sxs-lookup"><span data-stu-id="b2edf-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="b2edf-110">單一使用者可以有多個連線的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2edf-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="b2edf-111">例如，使用者無法連接其桌面，以及他們的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="b2edf-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="b2edf-112">每個裝置都有個別的 SignalR 連線，但所有具有相同的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="b2edf-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="b2edf-113">如果訊息傳送給使用者時，所有與該使用者相關聯的連接將會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="b2edf-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="b2edf-114">傳送訊息至特定的使用者中，藉由傳遞使用者識別項`User`中樞方法的函式，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b2edf-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="b2edf-115">使用者識別碼會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b2edf-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="b2edf-116">可以藉由建立自訂的使用者識別碼`IUserIdProvider`，並註冊在`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="b2edf-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="b2edf-117">在註冊自訂的 SignalR 服務之前，必須先呼叫 AddSignalR。</span><span class="sxs-lookup"><span data-stu-id="b2edf-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="b2edf-118">SignalR 中的群組</span><span class="sxs-lookup"><span data-stu-id="b2edf-118">Groups in SignalR</span></span>

<span data-ttu-id="b2edf-119">群組是連線名稱相關聯的集合。</span><span class="sxs-lookup"><span data-stu-id="b2edf-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="b2edf-120">訊息可以傳送到群組中的所有連線。</span><span class="sxs-lookup"><span data-stu-id="b2edf-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="b2edf-121">群組是傳送至多個連線或連線，因為群組由應用程式管理的建議的方式。</span><span class="sxs-lookup"><span data-stu-id="b2edf-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="b2edf-122">多個群組的成員時，可以在連線。</span><span class="sxs-lookup"><span data-stu-id="b2edf-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="b2edf-123">這使得群組非常適合類似交談應用程式，其中每個房間可以表示為群組。</span><span class="sxs-lookup"><span data-stu-id="b2edf-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="b2edf-124">可以加入或移除群組透過連線`AddToGroupAsync`和`RemoveFromGroupAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="b2edf-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="b2edf-125">重新連線時，並不會保留群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="b2edf-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="b2edf-126">連線需要重新建立時，重新加入群組。</span><span class="sxs-lookup"><span data-stu-id="b2edf-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="b2edf-127">您不可能來計算群組的成員，因為這項資訊不是使用應用程式會調整為多部伺服器。</span><span class="sxs-lookup"><span data-stu-id="b2edf-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="b2edf-128">群組名稱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b2edf-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="b2edf-129">相關資源</span><span class="sxs-lookup"><span data-stu-id="b2edf-129">Related resources</span></span>

* [<span data-ttu-id="b2edf-130">開始使用</span><span class="sxs-lookup"><span data-stu-id="b2edf-130">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="b2edf-131">中樞</span><span class="sxs-lookup"><span data-stu-id="b2edf-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b2edf-132">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="b2edf-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
