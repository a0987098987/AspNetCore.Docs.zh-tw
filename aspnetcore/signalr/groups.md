---
title: 管理使用者和 signalr 的群組
author: bradygaster
description: ASP.NET Core SignalR 使用者和群組管理的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667748"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="b232b-103">管理使用者和 signalr 的群組</span><span class="sxs-lookup"><span data-stu-id="b232b-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="b232b-104">由[brennan Conroy](https://github.com/BrennanConroy)提供</span><span class="sxs-lookup"><span data-stu-id="b232b-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="b232b-105">SignalR 可讓訊息傳送至特定使用者相關聯的所有連線以及具名群組的連線。</span><span class="sxs-lookup"><span data-stu-id="b232b-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="b232b-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b232b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="b232b-107">SignalR 中的使用者</span><span class="sxs-lookup"><span data-stu-id="b232b-107">Users in SignalR</span></span>

<span data-ttu-id="b232b-108">SignalR 可讓您將訊息傳送至特定使用者相關聯的所有連線。</span><span class="sxs-lookup"><span data-stu-id="b232b-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="b232b-109">根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。</span><span class="sxs-lookup"><span data-stu-id="b232b-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="b232b-110">單一使用者可以有多個連線的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b232b-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="b232b-111">比方說，使用者可以在他們的桌面，以及他們的手機上連線。</span><span class="sxs-lookup"><span data-stu-id="b232b-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="b232b-112">每個裝置都個別 SignalR 連線，但它們是所有具有相同的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="b232b-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="b232b-113">如果訊息傳送給使用者時，所有與該使用者相關聯的連接就會收到的訊息。</span><span class="sxs-lookup"><span data-stu-id="b232b-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="b232b-114">可以存取連接的使用者識別碼`Context.UserIdentifier`中樞內的屬性。</span><span class="sxs-lookup"><span data-stu-id="b232b-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="b232b-115">傳遞至的使用者識別碼，將訊息傳送至特定使用者`User`函式在您的中樞方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b232b-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="b232b-116">使用者識別碼會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b232b-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a><span data-ttu-id="b232b-117">Signalr 的群組</span><span class="sxs-lookup"><span data-stu-id="b232b-117">Groups in SignalR</span></span>

<span data-ttu-id="b232b-118">群組是連線名稱相關聯的集合。</span><span class="sxs-lookup"><span data-stu-id="b232b-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="b232b-119">訊息可以傳送至群組中的所有連線。</span><span class="sxs-lookup"><span data-stu-id="b232b-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="b232b-120">群組是傳送給多個連線或連線，因為群組由應用程式的建議的方式。</span><span class="sxs-lookup"><span data-stu-id="b232b-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="b232b-121">連接可以是多個群組的成員。</span><span class="sxs-lookup"><span data-stu-id="b232b-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="b232b-122">這使得群組非常適合類似交談應用程式，其中顯示每個房間，為群組。</span><span class="sxs-lookup"><span data-stu-id="b232b-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="b232b-123">可以加入或移除透過群組連線`AddToGroupAsync`和`RemoveFromGroupAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="b232b-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="b232b-124">重新連線時，不會保存群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="b232b-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="b232b-125">連接必須重新建立時，重新加入群組。</span><span class="sxs-lookup"><span data-stu-id="b232b-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="b232b-126">您不可能來計算群組的成員，因為這項資訊不提供，如果應用程式調整為多部伺服器。</span><span class="sxs-lookup"><span data-stu-id="b232b-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="b232b-127">若要保護資源的存取權，使用群組時，使用[驗證和授權](xref:signalr/authn-and-authz)ASP.NET Core 中的功能。</span><span class="sxs-lookup"><span data-stu-id="b232b-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="b232b-128">如果您只將使用者新增至群組的認證適用於該群組時，傳送至該群組的訊息將只會移至授權的使用者。</span><span class="sxs-lookup"><span data-stu-id="b232b-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="b232b-129">不過，群組不是一項安全性功能。</span><span class="sxs-lookup"><span data-stu-id="b232b-129">However, groups are not a security feature.</span></span> <span data-ttu-id="b232b-130">驗證宣告有群組未這麼做，例如到期及撤銷的功能。</span><span class="sxs-lookup"><span data-stu-id="b232b-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="b232b-131">如果已撤銷使用者的權限存取群組，您必須以手動方式偵測，並從群組中移除。</span><span class="sxs-lookup"><span data-stu-id="b232b-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="b232b-132">群組名稱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b232b-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="b232b-133">相關資源</span><span class="sxs-lookup"><span data-stu-id="b232b-133">Related resources</span></span>

* [<span data-ttu-id="b232b-134">開始使用</span><span class="sxs-lookup"><span data-stu-id="b232b-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="b232b-135">中樞</span><span class="sxs-lookup"><span data-stu-id="b232b-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b232b-136">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="b232b-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
