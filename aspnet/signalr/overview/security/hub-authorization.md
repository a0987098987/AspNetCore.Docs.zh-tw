---
uid: signalr/overview/security/hub-authorization
title: SignalR 中樞的驗證和授權 |Microsoft Docs
author: pfletcher
description: 本主題描述如何限制哪些使用者或角色可以存取中樞方法。 本主題中使用的軟體版本 Visual Studio 2013.NET 4.5 SignalR ve...
ms.author: riande
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 886373f6c0949f9dc0d3f95edf1d052c6c05490b
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910963"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="2101e-104">SignalR 中樞的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="2101e-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="2101e-105">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2101e-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2101e-106">本主題描述如何限制哪些使用者或角色可以存取中樞方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-106">This topic describes how to restrict which users or roles can access hub methods.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="2101e-107">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="2101e-107">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="2101e-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2101e-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="2101e-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2101e-109">.NET 4.5</span></span>
> - <span data-ttu-id="2101e-110">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="2101e-110">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="2101e-111">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="2101e-111">Previous versions of this topic</span></span>
>
> <span data-ttu-id="2101e-112">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="2101e-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="2101e-113">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="2101e-113">Questions and comments</span></span>
>
> <span data-ttu-id="2101e-114">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="2101e-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2101e-115">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="2101e-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="2101e-116">總覽</span><span class="sxs-lookup"><span data-stu-id="2101e-116">Overview</span></span>

<span data-ttu-id="2101e-117">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="2101e-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2101e-118">授權屬性</span><span class="sxs-lookup"><span data-stu-id="2101e-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="2101e-119">驗證所需的所有中樞</span><span class="sxs-lookup"><span data-stu-id="2101e-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="2101e-120">自訂的授權</span><span class="sxs-lookup"><span data-stu-id="2101e-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="2101e-121">將驗證資訊傳遞給用戶端</span><span class="sxs-lookup"><span data-stu-id="2101e-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="2101e-122">.NET 用戶端的驗證選項</span><span class="sxs-lookup"><span data-stu-id="2101e-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="2101e-123">使用表單驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="2101e-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="2101e-124">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="2101e-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="2101e-125">連接標頭</span><span class="sxs-lookup"><span data-stu-id="2101e-125">Connection header</span></span>](#header)
    - <span data-ttu-id="2101e-126">[[MSSQLSERVER 的通訊協定內容]](#certificate)</span><span class="sxs-lookup"><span data-stu-id="2101e-126">[Certificate](#certificate)</span></span>

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="2101e-127">授權屬性</span><span class="sxs-lookup"><span data-stu-id="2101e-127">Authorize attribute</span></span>

<span data-ttu-id="2101e-128">提供 SignalR[授權](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者或角色有中樞或方法的存取權。</span><span class="sxs-lookup"><span data-stu-id="2101e-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="2101e-129">此屬性位於`Microsoft.AspNet.SignalR`命名空間。</span><span class="sxs-lookup"><span data-stu-id="2101e-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="2101e-130">您套用`Authorize`中樞或中樞內的特定方法的屬性。</span><span class="sxs-lookup"><span data-stu-id="2101e-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="2101e-131">當您將套用`Authorize`hub 類別，指定的授權需求的屬性會套用到所有中樞中的方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="2101e-132">本主題提供的授權需求，您可以套用不同類型的範例。</span><span class="sxs-lookup"><span data-stu-id="2101e-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="2101e-133">不含`Authorize`屬性，已連接用戶端可以存取任何中樞上的公用方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="2101e-134">如果您已定義名為"Admin"，web 應用程式中的角色，您可以指定只有該角色中的使用者可以存取為下列程式碼中樞。</span><span class="sxs-lookup"><span data-stu-id="2101e-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="2101e-135">或者，您可以指定中樞都包含一個方法，可供所有使用者，並只會提供給已驗證的使用者，如下所示的第二個方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="2101e-136">下列範例會處理不同的授權案例：</span><span class="sxs-lookup"><span data-stu-id="2101e-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="2101e-137">`[Authorize]` – 只有經過驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="2101e-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="2101e-138">`[Authorize(Roles = "Admin,Manager")]` – 只有經過驗證的指定角色的使用者</span><span class="sxs-lookup"><span data-stu-id="2101e-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="2101e-139">`[Authorize(Users = "user1,user2")]` – 只有經過驗證的使用者指定的使用者名稱</span><span class="sxs-lookup"><span data-stu-id="2101e-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="2101e-140">`[Authorize(RequireOutgoing=false)]` – 只有已驗證的使用者可以叫用中樞，但從伺服器傳回給用戶端的呼叫不會受到授權，例如，只有特定使用者可以傳送訊息，但所有其他人可以接收訊息時。</span><span class="sxs-lookup"><span data-stu-id="2101e-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="2101e-141">RequireOutgoing 屬性只能套用至整個中樞，不需在中樞內的個人方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="2101e-142">當 RequireOutgoing 未設定為 false 時，只有符合授權需求的使用者會呼叫從伺服器中。</span><span class="sxs-lookup"><span data-stu-id="2101e-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="2101e-143">驗證所需的所有中樞</span><span class="sxs-lookup"><span data-stu-id="2101e-143">Require authentication for all hubs</span></span>

<span data-ttu-id="2101e-144">您可以對所有主機和主機方法要求驗證，應用程式中，藉由呼叫[RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)應用程式啟動時的方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="2101e-145">當您有多個中樞，想要針對所有人都強制執行的驗證需求，您可以使用這個方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="2101e-146">使用此方法，您無法指定角色、 使用者或外寄的授權的需求。</span><span class="sxs-lookup"><span data-stu-id="2101e-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="2101e-147">您只能指定中樞方法的存取權僅限於已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="2101e-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="2101e-148">不過，您仍然可以套用 Authorize 屬性中心或方法，來指定額外的需求。</span><span class="sxs-lookup"><span data-stu-id="2101e-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="2101e-149">您指定在屬性中的任何需求會新增至驗證的基本需求。</span><span class="sxs-lookup"><span data-stu-id="2101e-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="2101e-150">下列範例顯示已驗證的使用者限制所有中樞方法的啟動檔案。</span><span class="sxs-lookup"><span data-stu-id="2101e-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="2101e-151">如果您呼叫`RequireAuthentication()`SignalR 要求處理完之後的方法，SignalR 會擲回`InvalidOperationException`例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2101e-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="2101e-152">SignalR 擲回這個例外狀況，因為您無法將模組新增至 HubPipeline，在叫用管線之後。</span><span class="sxs-lookup"><span data-stu-id="2101e-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="2101e-153">先前的範例示範呼叫`RequireAuthentication`方法中的`Configuration`方法會執行一次，再處理第一個要求。</span><span class="sxs-lookup"><span data-stu-id="2101e-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="2101e-154">自訂的授權</span><span class="sxs-lookup"><span data-stu-id="2101e-154">Customized authorization</span></span>

<span data-ttu-id="2101e-155">如果您需要自訂如何決定授權，您可以建立衍生自類別`AuthorizeAttribute`，並覆寫[UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="2101e-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="2101e-156">針對每個要求，SignalR 叫用這個方法，以判斷是否授權使用者完成要求。</span><span class="sxs-lookup"><span data-stu-id="2101e-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="2101e-157">在覆寫方法中，您提供您授權的案例所需的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2101e-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="2101e-158">下列範例示範如何強制執行授權透過宣告式身分識別。</span><span class="sxs-lookup"><span data-stu-id="2101e-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="2101e-159">將驗證資訊傳遞給用戶端</span><span class="sxs-lookup"><span data-stu-id="2101e-159">Pass authentication information to clients</span></span>

<span data-ttu-id="2101e-160">您可能需要在用戶端執行的程式碼中使用的驗證資訊。</span><span class="sxs-lookup"><span data-stu-id="2101e-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="2101e-161">在用戶端上呼叫方法時，您會傳遞所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="2101e-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="2101e-162">比方說，聊天應用程式方法可以當做參數傳遞張貼訊息，該人員的使用者名稱，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2101e-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="2101e-163">或者，您可以建立代表驗證資訊，並將該物件傳遞做為參數的物件，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2101e-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="2101e-164">您永遠不應該傳遞一個用戶端連接識別碼其他用戶端，因為惡意使用者無法使用它來模擬該用戶端的要求。</span><span class="sxs-lookup"><span data-stu-id="2101e-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="2101e-165">.NET 用戶端的驗證選項</span><span class="sxs-lookup"><span data-stu-id="2101e-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="2101e-166">當您有.NET 用戶端，例如主控台應用程式，其與限制為已驗證的使用者中樞互動時，您可以傳遞 cookie、 連接標頭，或憑證的驗證認證。</span><span class="sxs-lookup"><span data-stu-id="2101e-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="2101e-167">在本節中的範例示範如何使用這些不同的方法來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="2101e-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="2101e-168">它們不是功能完整的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2101e-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="2101e-169">如需有關使用 SignalR 的.NET 用戶端的詳細資訊，請參閱[中樞 API 指南-.NET 用戶端](../guide-to-the-api/hubs-api-guide-net-client.md)。</span><span class="sxs-lookup"><span data-stu-id="2101e-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="2101e-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="2101e-170">Cookie</span></span>

<span data-ttu-id="2101e-171">當您的.NET 用戶端與 ASP.NET 表單驗證會使用中樞互動時，您必須手動設定連接上的 驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="2101e-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="2101e-172">您加入的 cookie`CookieContainer`上的屬性[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)物件。</span><span class="sxs-lookup"><span data-stu-id="2101e-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="2101e-173">下列範例示範從網頁擷取驗證 cookie，並將該 cookie 加入至連接的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="2101e-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="2101e-174">主控台應用程式會張貼到認證<strong>www.contoso.com/RemoteLogin</strong>它可以參考包含下列程式碼後置檔案的空白頁面。</span><span class="sxs-lookup"><span data-stu-id="2101e-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="2101e-175">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="2101e-175">Windows authentication</span></span>

<span data-ttu-id="2101e-176">使用 Windows 驗證時，您可以藉由傳遞目前使用者的認證[DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="2101e-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="2101e-177">您可以設定連線認證 DefaultCredentials 的值。</span><span class="sxs-lookup"><span data-stu-id="2101e-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="2101e-178">連接標頭</span><span class="sxs-lookup"><span data-stu-id="2101e-178">Connection header</span></span>

<span data-ttu-id="2101e-179">如果您的應用程式不使用 cookie，您可以在連接標頭中傳遞使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="2101e-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="2101e-180">比方說，您也可以在連接標頭中傳遞權杖。</span><span class="sxs-lookup"><span data-stu-id="2101e-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="2101e-181">然後，在中樞中，您會確認使用者的權杖。</span><span class="sxs-lookup"><span data-stu-id="2101e-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="2101e-182">憑證</span><span class="sxs-lookup"><span data-stu-id="2101e-182">Certificate</span></span>

<span data-ttu-id="2101e-183">您可以傳遞用戶端憑證來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="2101e-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="2101e-184">建立連線時，您可以加入憑證。</span><span class="sxs-lookup"><span data-stu-id="2101e-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="2101e-185">下列範例示範如何只將用戶端憑證新增至連線;它不會顯示完整的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="2101e-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="2101e-186">它會使用[X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)類別可提供數個不同的方式，來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="2101e-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
