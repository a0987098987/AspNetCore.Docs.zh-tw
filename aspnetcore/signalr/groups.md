---
<span data-ttu-id="58bb4-101">標題： [管理使用者和群組的 SignalR 作者： bradygaster 描述：] ASP.NET Core SignalR 使用者和群組管理的總覽。</span><span class="sxs-lookup"><span data-stu-id="58bb4-101">title: 'Manage users and groups in SignalR' author: bradygaster description: 'Overview of ASP.NET Core SignalR User and Group management.'</span></span>
<span data-ttu-id="58bb4-102">monikerRange： ' >= aspnetcore-2.1 ' ms-chap： bradyg ms. custom： mvc ms. date： 05/17/2020 no-loc：</span><span class="sxs-lookup"><span data-stu-id="58bb4-102">monikerRange: '>= aspnetcore-2.1' ms.author: bradyg ms.custom: mvc ms.date: 05/17/2020 no-loc:</span></span>
- <span data-ttu-id="58bb4-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="58bb4-103">'Blazor'</span></span>
- <span data-ttu-id="58bb4-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="58bb4-104">'Identity'</span></span>
- <span data-ttu-id="58bb4-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="58bb4-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="58bb4-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="58bb4-106">'Razor'</span></span>
- <span data-ttu-id="58bb4-107">' SignalR ' uid： signalr/groups</span><span class="sxs-lookup"><span data-stu-id="58bb4-107">'SignalR' uid: signalr/groups</span></span>

---

# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="58bb4-108">管理中的使用者和群組SignalR</span><span class="sxs-lookup"><span data-stu-id="58bb4-108">Manage users and groups in SignalR</span></span>

<span data-ttu-id="58bb4-109">依[Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="58bb4-109">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

SignalR<span data-ttu-id="58bb4-110">允許訊息傳送至與特定使用者相關聯的所有連接，以及命名的連接群組。</span><span class="sxs-lookup"><span data-stu-id="58bb4-110"> allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="58bb4-111">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="58bb4-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="58bb4-112">中的使用者SignalR</span><span class="sxs-lookup"><span data-stu-id="58bb4-112">Users in SignalR</span></span>

<span data-ttu-id="58bb4-113">中的單一使用者 SignalR 可以有多個應用程式連接。</span><span class="sxs-lookup"><span data-stu-id="58bb4-113">A single user in SignalR can have multiple connections to an app.</span></span> <span data-ttu-id="58bb4-114">例如，使用者可以連線到桌上型電腦以及電話。</span><span class="sxs-lookup"><span data-stu-id="58bb4-114">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="58bb4-115">每個裝置都有個別的 SignalR 連線，但它們全都與相同的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="58bb4-115">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="58bb4-116">如果訊息傳送給使用者，與該使用者相關聯的所有連接都會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="58bb4-116">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="58bb4-117">中樞的屬性可以存取連接的使用者識別碼 `Context.UserIdentifier` 。</span><span class="sxs-lookup"><span data-stu-id="58bb4-117">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in the hub.</span></span>

<span data-ttu-id="58bb4-118">根據預設，會 SignalR 使用 `ClaimTypes.NameIdentifier` `ClaimsPrincipal` 與連接相關聯的，做為使用者識別碼。</span><span class="sxs-lookup"><span data-stu-id="58bb4-118">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="58bb4-119">若要自訂此行為，請參閱[使用宣告來自訂身分識別處理](xref:signalr/authn-and-authz#use-claims-to-customize-identity-handling)。</span><span class="sxs-lookup"><span data-stu-id="58bb4-119">To customize this behavior, see [Use claims to customize identity handling](xref:signalr/authn-and-authz#use-claims-to-customize-identity-handling).</span></span>

<span data-ttu-id="58bb4-120">將使用者識別碼傳遞至中樞方法中的函式，以將訊息傳送給特定使用者 `User` ，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="58bb4-120">Send a message to a specific user by passing the user identifier to the `User` function in a hub method, as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="58bb4-121">使用者識別碼會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="58bb4-121">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/Hubs/ChatHub.cs?range=29-32)]

## <a name="groups-in-signalr"></a><span data-ttu-id="58bb4-122">群組于SignalR</span><span class="sxs-lookup"><span data-stu-id="58bb4-122">Groups in SignalR</span></span>

<span data-ttu-id="58bb4-123">「群組」（group）是與名稱相關聯之連接的集合。</span><span class="sxs-lookup"><span data-stu-id="58bb4-123">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="58bb4-124">訊息可以傳送至群組中的所有連接。</span><span class="sxs-lookup"><span data-stu-id="58bb4-124">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="58bb4-125">群組是傳送至連線或多個連接的建議方式，因為這些群組是由應用程式所管理。</span><span class="sxs-lookup"><span data-stu-id="58bb4-125">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="58bb4-126">連接可以是多個群組的成員。</span><span class="sxs-lookup"><span data-stu-id="58bb4-126">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="58bb4-127">群組適用于類似聊天應用程式的專案，其中每個房間可以表示為一個群組。</span><span class="sxs-lookup"><span data-stu-id="58bb4-127">Groups are ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="58bb4-128">您可以透過和方法，在群組中新增或移除連接 `AddToGroupAsync` `RemoveFromGroupAsync` 。</span><span class="sxs-lookup"><span data-stu-id="58bb4-128">Connections are added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/Hubs/ChatHub.cs?range=15-27)]

<span data-ttu-id="58bb4-129">當連線重新連接時，不會保留群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="58bb4-129">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="58bb4-130">重新建立群組時，連接需要重新加入該群組。</span><span class="sxs-lookup"><span data-stu-id="58bb4-130">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="58bb4-131">無法計算群組的成員，因為如果應用程式調整為多部伺服器，則無法使用此資訊。</span><span class="sxs-lookup"><span data-stu-id="58bb4-131">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="58bb4-132">若要在使用群組時保護對資源的存取，請使用 ASP.NET Core 中的[驗證和授權](xref:signalr/authn-and-authz)功能。</span><span class="sxs-lookup"><span data-stu-id="58bb4-132">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="58bb4-133">如果只有在認證對該群組有效時才將使用者新增至群組，則傳送至該群組的訊息只會前往已授權的使用者。</span><span class="sxs-lookup"><span data-stu-id="58bb4-133">If a user is added to a group only when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="58bb4-134">不過，群組並不是安全性功能。</span><span class="sxs-lookup"><span data-stu-id="58bb4-134">However, groups are not a security feature.</span></span> <span data-ttu-id="58bb4-135">驗證宣告具有群組不提供的功能，例如到期和撤銷。</span><span class="sxs-lookup"><span data-stu-id="58bb4-135">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="58bb4-136">如果使用者存取群組的許可權已被撤銷，應用程式必須明確地從群組中移除使用者。</span><span class="sxs-lookup"><span data-stu-id="58bb4-136">If a user's permission to access the group is revoked, the app must remove the user from the group explicitly.</span></span>

> [!NOTE]
> <span data-ttu-id="58bb4-137">組名會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="58bb4-137">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="58bb4-138">相關資源</span><span class="sxs-lookup"><span data-stu-id="58bb4-138">Related resources</span></span>

* [<span data-ttu-id="58bb4-139">開始使用</span><span class="sxs-lookup"><span data-stu-id="58bb4-139">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="58bb4-140">中樞</span><span class="sxs-lookup"><span data-stu-id="58bb4-140">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="58bb4-141">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="58bb4-141">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
