---
title: "設定身分識別"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a><span data-ttu-id="488f7-102">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="488f7-102">Configure Identity</span></span>

<span data-ttu-id="488f7-103">ASP.NET Core 識別有一些您可以覆寫輕鬆地在您的應用程式啟動類別中的預設行為。</span><span class="sxs-lookup"><span data-stu-id="488f7-103">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="488f7-104">密碼原則</span><span class="sxs-lookup"><span data-stu-id="488f7-104">Passwords policy</span></span>

<span data-ttu-id="488f7-105">根據預設，身分識別會需要密碼包含大寫字元、 小寫字元，以及數字。</span><span class="sxs-lookup"><span data-stu-id="488f7-105">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="488f7-106">另外還有一些其他的限制。</span><span class="sxs-lookup"><span data-stu-id="488f7-106">There are also some other restrictions.</span></span> <span data-ttu-id="488f7-107">如果您想要簡化密碼限制，您可以您的應用程式的啟動類別中。</span><span class="sxs-lookup"><span data-stu-id="488f7-107">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

<span data-ttu-id="488f7-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span><span class="sxs-lookup"><span data-stu-id="488f7-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="488f7-109">應用程式的 cookie 設定</span><span class="sxs-lookup"><span data-stu-id="488f7-109">Application's cookie settings</span></span>

<span data-ttu-id="488f7-110">類似的密碼原則的應用程式的 cookie 的所有設定都可以都變更啟動類別中。</span><span class="sxs-lookup"><span data-stu-id="488f7-110">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

<span data-ttu-id="488f7-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span><span class="sxs-lookup"><span data-stu-id="488f7-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span></span>

## <a name="users-lockout"></a><span data-ttu-id="488f7-112">使用者的鎖定</span><span class="sxs-lookup"><span data-stu-id="488f7-112">User's lockout</span></span>

<span data-ttu-id="488f7-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span><span class="sxs-lookup"><span data-stu-id="488f7-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span></span>
