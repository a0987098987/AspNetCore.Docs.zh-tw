---
uid: web-api/overview/security/integrated-windows-authentication
title: 整合式 Windows 驗證 |Microsoft Docs
author: MikeWasson
description: 描述如何使用 ASP.NET Web API 中的整合式 Windows 驗證。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: f11b9fe5d98118a252c6c00dd2997b2ee9a3da7a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381599"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="7d268-103">整合式的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="7d268-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="7d268-104">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7d268-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7d268-105">整合式的 Windows 驗證可讓使用者登入他們的 Windows 認證，使用 Kerberos 或 NTLM。</span><span class="sxs-lookup"><span data-stu-id="7d268-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="7d268-106">用戶端會將認證傳送授權標頭中。</span><span class="sxs-lookup"><span data-stu-id="7d268-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="7d268-107">Windows 驗證是最適合用於內部網路環境。</span><span class="sxs-lookup"><span data-stu-id="7d268-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="7d268-108">如需詳細資訊，請參閱 < [Windows 驗證](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)。</span><span class="sxs-lookup"><span data-stu-id="7d268-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="7d268-109">優點</span><span class="sxs-lookup"><span data-stu-id="7d268-109">Advantages</span></span> | <span data-ttu-id="7d268-110">缺點</span><span class="sxs-lookup"><span data-stu-id="7d268-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="7d268-111">-內建 IIS。</span><span class="sxs-lookup"><span data-stu-id="7d268-111">- Built into IIS.</span></span> <span data-ttu-id="7d268-112">-不會在要求中傳送的使用者認證。</span><span class="sxs-lookup"><span data-stu-id="7d268-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="7d268-113">-如果用戶端電腦所屬網域 （例如，內部網路應用程式），使用者就不需要輸入認證。</span><span class="sxs-lookup"><span data-stu-id="7d268-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="7d268-114">-不建議用於網際網路應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d268-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="7d268-115">-需要 Kerberos 或 NTLM 的用戶端中的支援。</span><span class="sxs-lookup"><span data-stu-id="7d268-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="7d268-116">用戶端必須位於 Active Directory 網域。</span><span class="sxs-lookup"><span data-stu-id="7d268-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="7d268-117">如果您的應用程式裝載在 Azure 上，而您有內部部署 Active Directory 網域，請考慮建立您的內部部署 AD 與 Azure Active Directory 同盟。</span><span class="sxs-lookup"><span data-stu-id="7d268-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="7d268-118">如此一來，使用者可以登入他們的內部部署認證，但由 Azure AD 執行驗證。</span><span class="sxs-lookup"><span data-stu-id="7d268-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="7d268-119">如需詳細資訊，請參閱 < [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md)。</span><span class="sxs-lookup"><span data-stu-id="7d268-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="7d268-120">若要建立使用整合式 Windows 驗證的應用程式，選取 [MVC 4 專案精靈] 中的 「 內部網路應用程式 」 範本。</span><span class="sxs-lookup"><span data-stu-id="7d268-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="7d268-121">這個專案範本會置於 Web.config 檔案中的下列設定：</span><span class="sxs-lookup"><span data-stu-id="7d268-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="7d268-122">用戶端，在整合式 Windows 驗證可搭配任何支援的瀏覽器[交涉](http://www.ietf.org/rfc/rfc4559.txt)驗證配置，其中包含大部分主要瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7d268-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="7d268-123">.NET 用戶端應用程式，如**HttpClient**類別支援 Windows 驗證：</span><span class="sxs-lookup"><span data-stu-id="7d268-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="7d268-124">Windows 驗證是容易遭受跨網站偽造要求 (CSRF) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="7d268-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="7d268-125">請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。</span><span class="sxs-lookup"><span data-stu-id="7d268-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
