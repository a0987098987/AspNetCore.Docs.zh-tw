---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: "MVC 建議教學課程和文章 |Microsoft 文件"
author: Rick-Anderson
description: "此頁面包含 ASP.NET MVC 的教學課程和建議的順序，遵循這些連結。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: 538eff2b2b2fdab5b0be879f0a5dfa5c9403b089
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="mvc-recommended-tutorials-and-articles"></a>MVC 建議教學課程和文章
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

<a id="pwd"></a>
## <a name="getting-started"></a>快速入門

- [開始使用 ASP.NET MVC 5](introduction/getting-started.md)此 11 一部分數列不是很好的起點。
- [Pluralsight 的 ASP.NET MVC 5 基礎](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals)（視訊課程）
- [ASP.NET MVC 簡介](https://www.microsoftvirtualacademy.com/training-courses/introduction-to-asp-net-mvc)Jon Galloway 和 Christopher Harrison
- [ASP.NET MVC 5 應用程式的生命週期](lifecycle-of-an-aspnet-mvc-5-application.md)圖表上的 ASP.NET MVC 5 應用程式生命週期的 PDF 文件。

<a id="con"></a>
## <a name="working-with-data"></a>使用資料

- [開始使用 EF 6 Code First 使用 MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) EF 深入探討 Tom Dykstra 深獲肯定 winning 數列。

<a id="wj"></a>
## <a name="security"></a>安全性

- [驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署到 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)這個常用的教學課程將告訴您如何建立簡單的應用程式並加入成員資格和角色。
- [建立 ASP.NET MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何建立 ASP.NET MVC 5 web 應用程式，讓使用者能夠登入來自外部的驗證認證搭配使用 OAuth 2.0提供者，例如 Facebook、 Twitter、 LinkedIn、 Microsoft 或 Google。
- [建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)先識別數列中包含的程式碼[重新傳送確認連結](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend)。
- [使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)上識別序列的第二個。
- [將密碼和其他敏感性資料部署到 ASP.NET 和 Azure App Service 的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)`isPersistent`和安全性 cookie，程式碼，以要求使用者擁有的已驗證的電子郵件帳戶，他們可以登入，才能 SignInManager 如何檢查 2FA 需求，以及其他更多。
- [帳戶確認和密碼復原與 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)提供詳細資料中找不到識別[建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)例如如何讓使用者重設忘記的密碼。

<a id="da"></a>
## <a name="azure"></a>Azure

- [在 Azure 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)簡單而簡短的教學課程，以部署至 Azure。
- [驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署到 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>效能和偵錯

- [使用 Glimpse 分析與偵錯 ASP.NET MVC 應用程式](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
