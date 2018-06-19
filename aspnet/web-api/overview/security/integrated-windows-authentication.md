---
uid: web-api/overview/security/integrated-windows-authentication
title: 整合式 Windows 驗證 |Microsoft 文件
author: MikeWasson
description: 描述如何使用 ASP.NET Web API 中的整合式 Windows 驗證。
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
ms.locfileid: "26508157"
---
<a name="integrated-windows-authentication"></a>整合式的 Windows 驗證
====================
由[Mike Wasson](https://github.com/MikeWasson)

整合式的 Windows 驗證可讓使用者登入他們的 Windows 認證，使用 Kerberos 或 NTLM。 用戶端傳送授權標頭中的認證。 Windows 驗證是最適合內部網路環境。 如需詳細資訊，請參閱[Windows 驗證](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)。

| 優點 | 缺點 |
| --- | --- |
| -內建 IIS。 -不會在要求中傳送的使用者認證。 -如果用戶端電腦隸屬於網域 （例如，內部網路應用程式），使用者就不需要輸入認證。 | -不建議用於網際網路應用程式。 -需要 Kerberos 或 ntlm 驗證支援的用戶端中。 用戶端必須是 Active Directory 網域中。 |

> [!NOTE]
> 如果您的應用程式裝載於 Azure，且您必須在內部部署 Active Directory 網域，請考慮將加入在內部部署 AD 與 Azure Active Directory 同盟。 這樣一來，使用者可以登入其在內部部署認證，但由 Azure AD 執行驗證。 如需詳細資訊，請參閱[Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md)。


若要建立使用整合式 Windows 驗證的應用程式，選取 [MVC 4 專案精靈] 中的 「 內部網路應用程式 」 範本。 這個專案範本會將 Web.config 檔案中的下列設定：

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

在用戶端，整合式 Windows 驗證可搭配任何支援的瀏覽器[交涉](http://www.ietf.org/rfc/rfc4559.txt)驗證配置，其中包括大部分主要的瀏覽器。 .NET 用戶端應用程式， **HttpClient**類別支援 Windows 驗證：

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows 驗證是很容易遭受跨網站要求偽造 (CSRF) 攻擊。 請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。
