---
title: 管理 SignalR 中的使用者和群組
author: bradygaster
description: ASP.NET Core SignalR 使用者和群組管理的總覽。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 7e8c85dcbc46daa68988374f499f19a9d2cead47
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665862"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a><span data-ttu-id="da817-103">管理 SignalR 中的使用者和群組</span><span class="sxs-lookup"><span data-stu-id="da817-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="da817-104">依[Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="da817-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

SignalR<span data-ttu-id="da817-105"> 允許訊息傳送至與特定使用者相關聯的所有連接，以及命名的連線群組。</span><span class="sxs-lookup"><span data-stu-id="da817-105"> allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="da817-106">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="da817-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-opno-locsignalr"></a><span data-ttu-id="da817-107">SignalR 中的使用者</span><span class="sxs-lookup"><span data-stu-id="da817-107">Users in SignalR</span></span>

SignalR<span data-ttu-id="da817-108"> 可讓您將訊息傳送至與特定使用者相關聯的所有連接。</span><span class="sxs-lookup"><span data-stu-id="da817-108"> allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="da817-109">根據預設，SignalR 會使用與連接相關聯的 `ClaimsPrincipal` 中的 `ClaimTypes.NameIdentifier` 作為使用者識別碼。</span><span class="sxs-lookup"><span data-stu-id="da817-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="da817-110">單一使用者可以有多個 SignalR 應用程式的連接。</span><span class="sxs-lookup"><span data-stu-id="da817-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="da817-111">例如，使用者可以連線到桌上型電腦以及電話。</span><span class="sxs-lookup"><span data-stu-id="da817-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="da817-112">每個裝置都有個別的 SignalR 連線，但全都與相同的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="da817-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="da817-113">如果訊息傳送給使用者，與該使用者相關聯的所有連接都會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="da817-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="da817-114">您的中樞中的 `Context.UserIdentifier` 屬性可以存取連接的使用者識別碼。</span><span class="sxs-lookup"><span data-stu-id="da817-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="da817-115">將使用者識別碼傳遞至中樞方法中的 `User` 函式，以將訊息傳送給特定使用者，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="da817-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="da817-116">使用者識別碼會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="da817-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a><span data-ttu-id="da817-117">SignalR 中的群組</span><span class="sxs-lookup"><span data-stu-id="da817-117">Groups in SignalR</span></span>

<span data-ttu-id="da817-118">「群組」（group）是與名稱相關聯之連接的集合。</span><span class="sxs-lookup"><span data-stu-id="da817-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="da817-119">訊息可以傳送至群組中的所有連接。</span><span class="sxs-lookup"><span data-stu-id="da817-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="da817-120">群組是傳送至連線或多個連接的建議方式，因為這些群組是由應用程式所管理。</span><span class="sxs-lookup"><span data-stu-id="da817-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="da817-121">連接可以是多個群組的成員。</span><span class="sxs-lookup"><span data-stu-id="da817-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="da817-122">這讓群組適用于類似聊天應用程式的專案，其中每個房間可以表示為一個群組。</span><span class="sxs-lookup"><span data-stu-id="da817-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="da817-123">您可以透過 `AddToGroupAsync` 和 `RemoveFromGroupAsync` 方法，在群組中新增或移除連接。</span><span class="sxs-lookup"><span data-stu-id="da817-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="da817-124">當連線重新連接時，不會保留群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="da817-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="da817-125">重新建立群組時，連接需要重新加入該群組。</span><span class="sxs-lookup"><span data-stu-id="da817-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="da817-126">無法計算群組的成員，因為如果應用程式調整為多部伺服器，則無法使用此資訊。</span><span class="sxs-lookup"><span data-stu-id="da817-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="da817-127">若要在使用群組時保護對資源的存取，請使用 ASP.NET Core 中的[驗證和授權](xref:signalr/authn-and-authz)功能。</span><span class="sxs-lookup"><span data-stu-id="da817-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="da817-128">如果您只在認證對該群組有效時，將使用者新增至群組，則傳送至該群組的訊息只會前往已授權的使用者。</span><span class="sxs-lookup"><span data-stu-id="da817-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="da817-129">不過，群組並不是安全性功能。</span><span class="sxs-lookup"><span data-stu-id="da817-129">However, groups are not a security feature.</span></span> <span data-ttu-id="da817-130">驗證宣告具有群組不提供的功能，例如到期和撤銷。</span><span class="sxs-lookup"><span data-stu-id="da817-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="da817-131">如果已撤銷使用者存取群組的許可權，您就必須手動偵測並將其從群組中移除。</span><span class="sxs-lookup"><span data-stu-id="da817-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="da817-132">組名會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="da817-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="da817-133">相關資源</span><span class="sxs-lookup"><span data-stu-id="da817-133">Related resources</span></span>

* [<span data-ttu-id="da817-134">開始使用</span><span class="sxs-lookup"><span data-stu-id="da817-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="da817-135">中樞</span><span class="sxs-lookup"><span data-stu-id="da817-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="da817-136">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="da817-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
