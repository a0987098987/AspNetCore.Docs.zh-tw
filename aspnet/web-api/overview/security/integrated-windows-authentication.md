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
<a name="integrated-windows-authentication"></a>整合式的 Windows 驗證
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

整合式的 Windows 驗證可讓使用者登入他們的 Windows 認證，使用 Kerberos 或 NTLM。 用戶端會將認證傳送授權標頭中。 Windows 驗證是最適合用於內部網路環境。 如需詳細資訊，請參閱 < [Windows 驗證](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)。

| 優點 | 缺點 |
| --- | --- |
| -內建 IIS。 -不會在要求中傳送的使用者認證。 -如果用戶端電腦所屬網域 （例如，內部網路應用程式），使用者就不需要輸入認證。 | -不建議用於網際網路應用程式。 -需要 Kerberos 或 NTLM 的用戶端中的支援。 用戶端必須位於 Active Directory 網域。 |

> [!NOTE]
> 如果您的應用程式裝載在 Azure 上，而您有內部部署 Active Directory 網域，請考慮建立您的內部部署 AD 與 Azure Active Directory 同盟。 如此一來，使用者可以登入他們的內部部署認證，但由 Azure AD 執行驗證。 如需詳細資訊，請參閱 < [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md)。


若要建立使用整合式 Windows 驗證的應用程式，選取 [MVC 4 專案精靈] 中的 「 內部網路應用程式 」 範本。 這個專案範本會置於 Web.config 檔案中的下列設定：

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

用戶端，在整合式 Windows 驗證可搭配任何支援的瀏覽器[交涉](http://www.ietf.org/rfc/rfc4559.txt)驗證配置，其中包含大部分主要瀏覽器。 .NET 用戶端應用程式，如**HttpClient**類別支援 Windows 驗證：

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows 驗證是容易遭受跨網站偽造要求 (CSRF) 攻擊。 請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。
