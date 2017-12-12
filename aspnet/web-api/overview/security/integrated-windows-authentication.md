---
uid: web-api/overview/security/integrated-windows-authentication
title: "整合式 Windows 驗證 |Microsoft 文件"
author: MikeWasson
description: "描述如何使用 ASP.NET Web API 中的整合式 Windows 驗證。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="114c9-103">整合式的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="114c9-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="114c9-104">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="114c9-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="114c9-105">整合式的 Windows 驗證可讓使用者登入他們的 Windows 認證，使用 Kerberos 或 NTLM。</span><span class="sxs-lookup"><span data-stu-id="114c9-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="114c9-106">用戶端傳送授權標頭中的認證。</span><span class="sxs-lookup"><span data-stu-id="114c9-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="114c9-107">Windows 驗證是最適合內部網路環境。</span><span class="sxs-lookup"><span data-stu-id="114c9-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="114c9-108">如需詳細資訊，請參閱[Windows 驗證](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)。</span><span class="sxs-lookup"><span data-stu-id="114c9-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="114c9-109">優點</span><span class="sxs-lookup"><span data-stu-id="114c9-109">Advantages</span></span> | <span data-ttu-id="114c9-110">缺點</span><span class="sxs-lookup"><span data-stu-id="114c9-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="114c9-111">-內建 IIS。</span><span class="sxs-lookup"><span data-stu-id="114c9-111">- Built into IIS.</span></span> <span data-ttu-id="114c9-112">-不會在要求中傳送的使用者認證。</span><span class="sxs-lookup"><span data-stu-id="114c9-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="114c9-113">-如果用戶端電腦隸屬於網域 （例如，內部網路應用程式），使用者就不需要輸入認證。</span><span class="sxs-lookup"><span data-stu-id="114c9-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="114c9-114">-不建議用於網際網路應用程式。</span><span class="sxs-lookup"><span data-stu-id="114c9-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="114c9-115">-需要 Kerberos 或 ntlm 驗證支援的用戶端中。</span><span class="sxs-lookup"><span data-stu-id="114c9-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="114c9-116">用戶端必須是 Active Directory 網域中。</span><span class="sxs-lookup"><span data-stu-id="114c9-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="114c9-117">如果您的應用程式裝載於 Azure，且您必須在內部部署 Active Directory 網域，請考慮將加入在內部部署 AD 與 Azure Active Directory 同盟。</span><span class="sxs-lookup"><span data-stu-id="114c9-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="114c9-118">這樣一來，使用者可以登入其在內部部署認證，但由 Azure AD 執行驗證。</span><span class="sxs-lookup"><span data-stu-id="114c9-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="114c9-119">如需詳細資訊，請參閱[Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md)。</span><span class="sxs-lookup"><span data-stu-id="114c9-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="114c9-120">若要建立使用整合式 Windows 驗證的應用程式，選取 [MVC 4 專案精靈] 中的 「 內部網路應用程式 」 範本。</span><span class="sxs-lookup"><span data-stu-id="114c9-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="114c9-121">這個專案範本會將 Web.config 檔案中的下列設定：</span><span class="sxs-lookup"><span data-stu-id="114c9-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="114c9-122">在用戶端，整合式 Windows 驗證可搭配任何支援的瀏覽器[交涉](http://www.ietf.org/rfc/rfc4559.txt)驗證配置，其中包括大部分主要的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="114c9-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="114c9-123">.NET 用戶端應用程式， **HttpClient**類別支援 Windows 驗證：</span><span class="sxs-lookup"><span data-stu-id="114c9-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="114c9-124">Windows 驗證是很容易遭受跨網站要求偽造 (CSRF) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="114c9-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="114c9-125">請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。</span><span class="sxs-lookup"><span data-stu-id="114c9-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
