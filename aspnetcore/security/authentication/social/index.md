---
title: "使用 Facebook、Google 和其他外部提供者啟用驗證"
author: rick-anderson
description: 
keywords: "ASP.NET Core, 驗證, 社交, 驗證提供者, Google, Facebook, Twitter, Microsoft 帳戶"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: b561dcee5435dfc34cfa0b9b15babf75ca8f3508
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a>使用 Facebook、Google 和其他外部提供者啟用驗證

<a name=security-authentication-social-logins></a>

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何建立 ASP.NET Core 2.x 應用程式，讓使用者使用 OAuth 2.0 與來自外部驗證提供者的認證進行登入。

下列各節涵蓋 [Facebook](facebook-logins.md)、[Twitter](twitter-logins.md)、[Google](google-logins.md) 和 [Microsoft](microsoft-logins.md) 的提供者。 您可透過 [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) 和 [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) 這類協力廠商套件，取得其他提供者。

![Facebook、Twitter、Google+ 和 Windows 的社交媒體圖示](index/_static/social.png)

若使用者可以利用其現有的認證登入，一方面對使用者來說十分方便，另一方面則可將管理登入程序的許多複雜工作轉移給協力廠商。 如需社交登入如何帶動流量和客戶轉換的範例，請參閱 [Facebook](https://www.facebook.com/unsupportedbrowser) 和 [Twitter](https://dev.twitter.com/resources/case-studies) 的案例研究。

注意：此處提供的套件摘錄了大量 OAuth 驗證流程的複雜內容，但在您進行疑難排解時，可能有必要了解詳細資料。 可用的資源很多；例如，您可以參閱 [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) (OAuth 2 簡介) 或 [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/) (了解 OAuth 2)。 您也可以查看[適用於提供者套件的 ASP.NET Core 原始程式碼](https://github.com/aspnet/Security/tree/dev/src)，來解決部分問題。

## <a name="create-a-new-aspnet-core-project"></a>建立新的 ASP.NET Core 專案

* 在 Visual Studio 2017 中，您可以從 [開始] 頁面來建立新的專案，或透過 [檔案] > [新增] > [專案] 來進行。

* 在 [Visual C#] > [.NET Core] 類別中，選取可用的 [ASP.NET Core Web 應用程式] 範本：

![[新增專案] 對話](index/_static/new-project.png)

* 點選 [Web 應用程式]，並驗證已將 [驗證] 設定為 [個別使用者帳戶]：

![[新增 Web 應用程式] 對話方塊](index/_static/select-project.png)

注意：本教學課程適用於 ASP.NET Core 2.0 SDK 版本；您可以在精靈的頂端選取此版本。

## <a name="require-ssl"></a>需要 SSL

OAuth 2.0 要求使用 SSL 以透過 HTTPS 通訊協定進行驗證。

注意：如果您是使用適用於 ASP.NET Core 2.x 的 [Web 應用程式] 或 [Web API] 專案範本來建立專案，且專案精靈的 [變更驗證] 對話方塊已如上所示選取 [個別使用者帳戶] 選項的話，則系統會自動將該專案設為啟用 SSL，並使用 https URL 來啟動。

* 了解如何遵循[設定 HTTPS 進行 ASP.NET Core 開發](xref:security/https)主題中的步驟以手動啟用 SSL。

* 然後，遵循[強制執行 ASP.NET Core 應用程式中的 SSL](xref:security/enforcing-ssl) 主題的下列步驟，要求網站使用 SSL。

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>使用 SecretManager 來儲存登入提供者指派的權杖

社交登入提供者會在註冊程序期間指派「應用程式識別碼」和「應用程式密碼」權杖 (確切的命名因提供者而異)。

這些值都是應用程式用來存取其 API 的有效「使用者名稱」和「密碼」，並構成可連結至應用程式組態的「密碼」(透過**密碼管理員**來完成，而不是直接儲存在組態檔中或將其硬式編碼)。

請遵循[在 ASP.NET Core 開發期間安全儲存應用程式密碼](xref:security/app-secrets)主題中的步驟，儲存下列每個登入提供者所指派的權杖。

## <a name="setup-login-providers-required-by-your-application"></a>設定應用程式所需的登入提供者

若要將應用程式設定為使用相應的提供者，請使用下列主題：

* [Facebook](facebook-logins.md) 指示
* [Twitter](twitter-logins.md) 指示
* [Google](google-logins.md) 指示
* [Microsoft](microsoft-logins.md) 指示
* [其他提供者](other-logins.md)指示

## <a name="optionally-set-password"></a>選擇性地設定密碼

當您使用外部登入提供者註冊時，您並未向應用程式註冊密碼。 這樣可以減輕您建立網站密碼與記住密碼的壓力，但這也會讓您依賴外部登入提供者。 如果無法使用外部登入提供者，您就無法登入網站。

若要建立密碼，並使用您在外部提供者登入程序期間所設的電子郵件進行登入：

* 點選右上角的 **Hello <email alias>** 連結，巡覽至 [管理] 檢視。

![Web 應用程式的 [管理] 檢視](index/_static/pass1a.png)

* 點選 [建立]

![[設定密碼] 頁面](index/_static/pass2a.png)

* 設定有效的密碼，以便使用此密碼與電子郵件進行登入。

## <a name="next-steps"></a>後續步驟

* 本文介紹了外部驗證，並說明將外部登入新增至 ASP.NET Core 應用程式所需的必要條件。

* 請參考提供者的特定頁面，以設定應用程式所需的提供者登入項目。
