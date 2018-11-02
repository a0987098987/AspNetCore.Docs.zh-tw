---
title: 管理使用者和 signalr 的群組
author: tdykstra
description: ASP.NET Core SignalR 使用者和群組管理的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 02db46f090c487a03171de244ff7ad0d5e9de0fa
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758163"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="58d00-103">管理使用者和 signalr 的群組</span><span class="sxs-lookup"><span data-stu-id="58d00-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="58d00-104">由[brennan Conroy](https://github.com/BrennanConroy)提供</span><span class="sxs-lookup"><span data-stu-id="58d00-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="58d00-105">SignalR 可讓訊息傳送至特定使用者相關聯的所有連線以及具名群組的連線。</span><span class="sxs-lookup"><span data-stu-id="58d00-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="58d00-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="58d00-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="58d00-107">SignalR 中的使用者</span><span class="sxs-lookup"><span data-stu-id="58d00-107">Users in SignalR</span></span>

<span data-ttu-id="58d00-108">SignalR 可讓您將訊息傳送至特定使用者相關聯的所有連線。</span><span class="sxs-lookup"><span data-stu-id="58d00-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="58d00-109">根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。</span><span class="sxs-lookup"><span data-stu-id="58d00-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="58d00-110">單一使用者可以有多個連線的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="58d00-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="58d00-111">比方說，使用者可以在他們的桌面，以及他們的手機上連線。</span><span class="sxs-lookup"><span data-stu-id="58d00-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="58d00-112">每個裝置都個別 SignalR 連線，但它們是所有具有相同的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="58d00-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="58d00-113">如果訊息傳送給使用者時，所有與該使用者相關聯的連接就會收到的訊息。</span><span class="sxs-lookup"><span data-stu-id="58d00-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="58d00-114">可以存取連接的使用者識別碼`Context.UserIdentifier`中樞內的屬性。</span><span class="sxs-lookup"><span data-stu-id="58d00-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="58d00-115">傳遞至的使用者識別碼，將訊息傳送至特定使用者`User`函式在您的中樞方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="58d00-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="58d00-116">使用者識別碼會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="58d00-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="58d00-117">可以藉由建立自訂的使用者識別碼`IUserIdProvider`，並註冊在`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="58d00-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="58d00-118">註冊您的自訂 SignalR 服務之前，必須呼叫 AddSignalR。</span><span class="sxs-lookup"><span data-stu-id="58d00-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="58d00-119">Signalr 的群組</span><span class="sxs-lookup"><span data-stu-id="58d00-119">Groups in SignalR</span></span>

<span data-ttu-id="58d00-120">群組是連線名稱相關聯的集合。</span><span class="sxs-lookup"><span data-stu-id="58d00-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="58d00-121">訊息可以傳送至群組中的所有連線。</span><span class="sxs-lookup"><span data-stu-id="58d00-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="58d00-122">群組是傳送給多個連線或連線，因為群組由應用程式的建議的方式。</span><span class="sxs-lookup"><span data-stu-id="58d00-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="58d00-123">連接可以是多個群組的成員。</span><span class="sxs-lookup"><span data-stu-id="58d00-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="58d00-124">這使得群組非常適合類似交談應用程式，其中顯示每個房間，為群組。</span><span class="sxs-lookup"><span data-stu-id="58d00-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="58d00-125">可以加入或移除透過群組連線`AddToGroupAsync`和`RemoveFromGroupAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="58d00-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="58d00-126">重新連線時，不會保存群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="58d00-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="58d00-127">連接必須重新建立時，重新加入群組。</span><span class="sxs-lookup"><span data-stu-id="58d00-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="58d00-128">您不可能來計算群組的成員，因為這項資訊不提供，如果應用程式調整為多部伺服器。</span><span class="sxs-lookup"><span data-stu-id="58d00-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="58d00-129">若要保護資源的存取權，使用群組時，使用[驗證和授權](xref:signalr/authn-and-authz)ASP.NET Core 中的功能。</span><span class="sxs-lookup"><span data-stu-id="58d00-129">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="58d00-130">如果您只將使用者新增至群組的認證適用於該群組時，傳送至該群組的訊息將只會移至授權的使用者。</span><span class="sxs-lookup"><span data-stu-id="58d00-130">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="58d00-131">不過，群組不是一項安全性功能。</span><span class="sxs-lookup"><span data-stu-id="58d00-131">However, groups are not a security feature.</span></span> <span data-ttu-id="58d00-132">驗證宣告有群組未這麼做，例如到期及撤銷的功能。</span><span class="sxs-lookup"><span data-stu-id="58d00-132">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="58d00-133">如果已撤銷使用者的權限存取群組，您必須以手動方式偵測，並從群組中移除。</span><span class="sxs-lookup"><span data-stu-id="58d00-133">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="58d00-134">群組名稱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="58d00-134">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="58d00-135">相關資源</span><span class="sxs-lookup"><span data-stu-id="58d00-135">Related resources</span></span>

* [<span data-ttu-id="58d00-136">開始使用</span><span class="sxs-lookup"><span data-stu-id="58d00-136">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="58d00-137">中樞</span><span class="sxs-lookup"><span data-stu-id="58d00-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="58d00-138">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="58d00-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
