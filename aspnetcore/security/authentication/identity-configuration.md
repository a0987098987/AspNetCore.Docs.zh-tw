---
title: "設定 ASP.NET Core 身分識別"
author: AdrienTorris
description: "了解 ASP.NET Core 識別預設值，並設定要使用自訂值的各種識別屬性。"
keywords: "ASP.NET Core，身分識別驗證安全性"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2017
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
