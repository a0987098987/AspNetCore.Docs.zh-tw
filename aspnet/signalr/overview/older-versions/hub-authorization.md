---
uid: signalr/overview/older-versions/hub-authorization
title: "SignalR 中樞的驗證和授權 (SignalR 1.x) |Microsoft 文件"
author: pfletcher
description: "本主題描述如何限制哪些使用者或角色可以存取中樞方法。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="e649a-103">SignalR 中樞的驗證和授權 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="e649a-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="e649a-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e649a-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e649a-105">本主題描述如何限制哪些使用者或角色可以存取中樞方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="e649a-106">總覽</span><span class="sxs-lookup"><span data-stu-id="e649a-106">Overview</span></span>

<span data-ttu-id="e649a-107">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="e649a-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e649a-108">授權屬性</span><span class="sxs-lookup"><span data-stu-id="e649a-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="e649a-109">需要之所有驗證</span><span class="sxs-lookup"><span data-stu-id="e649a-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="e649a-110">自訂的授權</span><span class="sxs-lookup"><span data-stu-id="e649a-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="e649a-111">將驗證資訊傳遞給用戶端</span><span class="sxs-lookup"><span data-stu-id="e649a-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="e649a-112">.NET 用戶端的驗證選項</span><span class="sxs-lookup"><span data-stu-id="e649a-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="e649a-113">使用表單驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="e649a-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="e649a-114">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="e649a-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="e649a-115">連接標頭</span><span class="sxs-lookup"><span data-stu-id="e649a-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="e649a-116">Certificate</span><span class="sxs-lookup"><span data-stu-id="e649a-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="e649a-117">授權屬性</span><span class="sxs-lookup"><span data-stu-id="e649a-117">Authorize attribute</span></span>

<span data-ttu-id="e649a-118">提供 SignalR[授權](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者或角色有存取權的集線器或方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="e649a-119">這個屬性位於`Microsoft.AspNet.SignalR`命名空間。</span><span class="sxs-lookup"><span data-stu-id="e649a-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="e649a-120">您套用`Authorize`屬性到集線器或中樞中的特定方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="e649a-121">當您將套用`Authorize`屬性套用至 hub 類別，指定的授權需求套用至所有中樞中的方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="e649a-122">您可以將套用的授權需求的不同類型如下所示。</span><span class="sxs-lookup"><span data-stu-id="e649a-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="e649a-123">不含`Authorize`屬性集線器上的所有公用方法可用來連線至中樞的用戶端。</span><span class="sxs-lookup"><span data-stu-id="e649a-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="e649a-124">如果您已經定義名為"Admin"，web 應用程式中的角色，您可以指定只有該角色中的使用者可以存取的集線器以下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="e649a-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="e649a-125">或者，您可以指定一種方法，可供所有使用者和第二個方法，才可以使用已驗證的使用者，如下所示，中樞會包含。</span><span class="sxs-lookup"><span data-stu-id="e649a-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="e649a-126">下列範例會處理不同的授權案例：</span><span class="sxs-lookup"><span data-stu-id="e649a-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="e649a-127">`[Authorize]`– 僅限驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="e649a-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="e649a-128">`[Authorize(Roles = "Admin,Manager")]`– 僅限驗證的使用者指定的角色</span><span class="sxs-lookup"><span data-stu-id="e649a-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="e649a-129">`[Authorize(Users = "user1,user2")]`– 僅限驗證的使用者指定的使用者名稱</span><span class="sxs-lookup"><span data-stu-id="e649a-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="e649a-130">`[Authorize(RequireOutgoing=false)]`– 只有經過驗證的使用者可以叫用中樞，但從伺服器傳回給用戶端的呼叫不受限制的授權，例如，只有特定使用者可以傳送訊息，但其他所有項目可以接收訊息時。</span><span class="sxs-lookup"><span data-stu-id="e649a-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="e649a-131">RequireOutgoing 屬性只能套用至整個中樞，不是在中樞內的個人版方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="e649a-132">RequireOutgoing 未設定為 false，會從伺服器呼叫符合授權需求的使用者。</span><span class="sxs-lookup"><span data-stu-id="e649a-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="e649a-133">需要之所有驗證</span><span class="sxs-lookup"><span data-stu-id="e649a-133">Require authentication for all hubs</span></span>

<span data-ttu-id="e649a-134">您可以對所有中樞和中樞方法進行驗證，應用程式中，藉由呼叫[RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)應用程式啟動時的方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="e649a-135">當您有多個中樞，並想要強制所有的驗證需求，您可以使用這個方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="e649a-136">使用此方法，您無法指定角色、 使用者或外寄的授權。</span><span class="sxs-lookup"><span data-stu-id="e649a-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="e649a-137">您只可以指定存取中樞的方法是限制為已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="e649a-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="e649a-138">不過，您仍然可以套用 Authorize 屬性中心或方法，以指定其他需求。</span><span class="sxs-lookup"><span data-stu-id="e649a-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="e649a-139">除了基本驗證的需求，會套用您在屬性中指定的任何需求。</span><span class="sxs-lookup"><span data-stu-id="e649a-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="e649a-140">下列範例會示範已驗證的使用者限制所有中樞方法的 Global.asax 檔案。</span><span class="sxs-lookup"><span data-stu-id="e649a-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="e649a-141">如果您呼叫`RequireAuthentication()`方法 SignalR 要求已處理之後，SignalR 會擲回`InvalidOperationException`例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e649a-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="e649a-142">因為您無法將模組加入 HubPipeline，叫用管線之後，會擲回這個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e649a-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="e649a-143">上述範例示範呼叫`RequireAuthentication`方法中的`Application_Start`之前處理第一個要求一次執行此方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="e649a-144">自訂的授權</span><span class="sxs-lookup"><span data-stu-id="e649a-144">Customized authorization</span></span>

<span data-ttu-id="e649a-145">如果您需要自訂如何決定授權，您可以建立衍生自類別`AuthorizeAttribute`並覆寫[UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="e649a-146">針對每個要求，以判斷是否授權使用者完成要求，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="e649a-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="e649a-147">在覆寫方法中，您會為授權案例提供必要的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e649a-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="e649a-148">下列範例會示範如何強制執行宣告式身分識別授權。</span><span class="sxs-lookup"><span data-stu-id="e649a-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="e649a-149">將驗證資訊傳遞給用戶端</span><span class="sxs-lookup"><span data-stu-id="e649a-149">Pass authentication information to clients</span></span>

<span data-ttu-id="e649a-150">您可能需要在用戶端執行的程式碼中使用的驗證資訊。</span><span class="sxs-lookup"><span data-stu-id="e649a-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="e649a-151">在用戶端上呼叫方法時，您會傳遞所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="e649a-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="e649a-152">例如，交談的應用程式方法無法當做參數傳遞張貼郵件的人員的使用者名稱如下所示。</span><span class="sxs-lookup"><span data-stu-id="e649a-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="e649a-153">或者，您可以建立的物件來代表驗證資訊，並將該物件傳遞為參數，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e649a-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="e649a-154">您永遠不會應該傳遞給其他用戶端，一部用戶端連接識別碼，因為惡意使用者可以用它來模擬該用戶端的要求。</span><span class="sxs-lookup"><span data-stu-id="e649a-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="e649a-155">.NET 用戶端的驗證選項</span><span class="sxs-lookup"><span data-stu-id="e649a-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="e649a-156">當您有.NET 用戶端，例如主控台應用程式，與為集線器，僅限於驗證的使用者互動時，您可以傳遞在 cookie 中，連接標頭或憑證的驗證認證。</span><span class="sxs-lookup"><span data-stu-id="e649a-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="e649a-157">本節中的範例示範如何使用這些不同的方法來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="e649a-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="e649a-158">它們不是完整功能的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e649a-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="e649a-159">如需與 SignalR 的.NET 用戶端的詳細資訊，請參閱[集線器 API 指南-.NET 用戶端](../guide-to-the-api/hubs-api-guide-net-client.md)。</span><span class="sxs-lookup"><span data-stu-id="e649a-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="e649a-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="e649a-160">Cookie</span></span>

<span data-ttu-id="e649a-161">當.NET 用戶端會使用 ASP.NET 表單驗證的集線器與互動時，您必須以手動方式在連接上設定的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="e649a-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="e649a-162">新增 cookie`CookieContainer`屬性[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)物件。</span><span class="sxs-lookup"><span data-stu-id="e649a-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="e649a-163">下列範例主控台應用程式會從網頁擷取驗證 cookie 並將該 cookie 新增到連線。</span><span class="sxs-lookup"><span data-stu-id="e649a-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="e649a-164">URL`https://www.contoso.com/RemoteLogin`範例點，您需要建立的網頁中。</span><span class="sxs-lookup"><span data-stu-id="e649a-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="e649a-165">網頁會擷取已張貼的使用者名稱和密碼，並嘗試登入使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="e649a-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="e649a-166">在主控台應用程式會張貼 www.contoso.com/RemoteLogin 其無法參考包含下列程式碼後置檔案的空白頁面的認證。</span><span class="sxs-lookup"><span data-stu-id="e649a-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="e649a-167">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="e649a-167">Windows authentication</span></span>

<span data-ttu-id="e649a-168">使用 Windows 驗證時，您可以使用傳遞目前使用者的認證[DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="e649a-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="e649a-169">您可以設定連線認證 DefaultCredentials 的值。</span><span class="sxs-lookup"><span data-stu-id="e649a-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="e649a-170">連接標頭</span><span class="sxs-lookup"><span data-stu-id="e649a-170">Connection header</span></span>

<span data-ttu-id="e649a-171">如果您的應用程式不使用 cookie，您可以在連接標頭中傳遞使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="e649a-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="e649a-172">例如，您可以連接標頭中傳遞權杖。</span><span class="sxs-lookup"><span data-stu-id="e649a-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="e649a-173">然後，在中樞，您會確認使用者的權杖。</span><span class="sxs-lookup"><span data-stu-id="e649a-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="e649a-174">憑證</span><span class="sxs-lookup"><span data-stu-id="e649a-174">Certificate</span></span>

<span data-ttu-id="e649a-175">您可以傳遞用戶端憑證，以驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="e649a-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="e649a-176">建立連接時，您可以加入憑證。</span><span class="sxs-lookup"><span data-stu-id="e649a-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="e649a-177">下列範例顯示如何只將用戶端憑證新增至連線;它不會顯示完整的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="e649a-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="e649a-178">它會使用[X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)類別提供幾種不同的方式建立憑證。</span><span class="sxs-lookup"><span data-stu-id="e649a-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
