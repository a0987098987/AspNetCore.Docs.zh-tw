---
title: "設定身分識別"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a>設定身分識別

ASP.NET Core 識別有一些您可以覆寫輕鬆地在您的應用程式啟動類別中的預設行為。

## <a name="passwords-policy"></a>密碼原則

根據預設，身分識別會需要密碼包含大寫字元、 小寫字元，以及數字。 另外還有一些其他的限制。 如果您想要簡化密碼限制，您可以您的應用程式的啟動類別中。

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>應用程式的 cookie 設定

類似的密碼原則的應用程式的 cookie 的所有設定都可以都變更啟動類別中。

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>使用者的鎖定

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
