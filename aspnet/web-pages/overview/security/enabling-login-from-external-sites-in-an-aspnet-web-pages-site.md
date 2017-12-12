---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: "使用 ASP.NET Web 中的外部網站登入頁面 (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "這篇文章說明如何登入使用 Facebook、 Google、 Twitter、 Yahoo 和其他站台的 ASP.NET Web Pages (Razor) 網站 — 也就是如何支援..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>使用外部站台的 ASP.NET Web Pages (Razor) 網站中登入
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何登入使用 Facebook、 Google、 Twitter、 Yahoo 和其他站台的 ASP.NET Web Pages (Razor) 網站 — 也就是如何在您的站台支援 OAuth 和 OpenID。
> 
> 您將學習：
> 
> - 如何啟用登入從其他站台，當您使用 WebMatrix 入門網站範本。
> 
> 這是發行項中所引進的 ASP.NET 功能：
> 
> - `OAuthWebSecurity`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

ASP.NET Web 網頁包含支援[OAuth](http://oauth.net/)和[OpenID](http://openid.net/)提供者。 使用這些提供者，您可以讓使用者登入您的網站使用其現有的認證從 Facebook、 Twitter、 Microsoft 與 Google。 例如，使用 Facebook 帳戶進行登入，使用者就可以選擇一個 Facebook 圖示，重新導向至在其中輸入他們的使用者資訊的 Facebook 登入頁面。 他們可將其網站上的帳戶產生關聯 Facebook 登入。 網頁的成員資格功能相關的增強功能是，使用者可將多個登入 （包括從社交網路網站的登入） 與您的網站上的單一帳戶。

下圖顯示登入頁面**入門網站**範本，使用者可以選擇 Facebook、 Twitter、 Google 或 Microsoft 的圖示，以啟用外部的帳戶登入：

![外部提供者](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

您可以啟用 OAuth 和 OpenID 成員資格中的程式碼的幾行取消註解**入門網站**範本。 您的方法和屬性用來處理 OAuth 和 OpenID 提供者位於`WebMatrix.Security.OAuthWebSecurity`類別。 **入門網站**範本包含完整的成員資格基礎結構，您需要讓使用者登入您的網站使用本機認證或來自另一個站台完成與登入頁面、 成員資格資料庫和所有的程式碼.

本節提供如何讓使用者登入從外部站台至站台為基礎的範例**入門網站**範本。 建立入門網站之後, 您這樣做，（詳細資料）：

- 針對使用 OAuth 提供者 （Facebook、 Twitter 和 Microsoft） 的網站，您可以建立應用程式外部站台上。 這讓您將需要以叫用這些站台的登入功能的應用程式金鑰。
- 使用 OpenID 提供者 (Google) 的網站，您沒有建立應用程式。 針對所有這些站台，您必須有帳戶才能登入，並建立開發人員應用程式。

    > [!NOTE]
    > Microsoft 應用程式只接受即時工作網站 URL，因此您無法使用本機網站 URL，測試登入。
- 若要指定適當的驗證提供者，以及提交的登入您想要使用的站台，請編輯網站中的幾個檔案。

本文提供不同的指示，進行下列工作：

- [啟用 Google 登入](#To_enable_Google_logins)
- [啟用 Facebook 登入](#To_enable_Facebook_logins)
- [啟用 Twitter 登入](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>啟用 Google 登入

1. 建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。
2. 開啟 *\_AppStart.cshtml*頁面上，並取消註解下列程式碼行。 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>測試 Google 登入

1. 執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。
2. 在*登入*頁面上，於**使用另一個服務登入**區段中，選擇  **Google**或**Yahoo**送出按鈕。 這個範例會使用 Google 登入。 

    網頁上將要求重新導向至 Google 登入頁面。

    ![Google 登入](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. 現有的 Google 帳戶輸入認證。
4. 如果 Google 會詢問您是否要允許*Localhost*使用來自帳戶的資訊，請按一下**允許**。

    程式碼來驗證使用者使用 Google 語彙基元，並會返回此頁面，在您的網站。 此頁面可讓使用者將其 Google 登入與現有的帳戶，在您的網站產生關聯，或他們可以註冊新的帳戶產生關聯的外部登入您的站台上。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. 選擇**關聯** 按鈕。 瀏覽器會返回應用程式的首頁。

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>啟用 Facebook 登入

1. 移至[Facebook 開發人員網站](https://developers.facebook.com/apps)（登入如果您未登入）。
2. 選擇**建立新的應用程式**按鈕，然後再遵循提示來命名和建立新的應用程式。
3. 一節中**選取與 Facebook 整合您的應用程式將會如何**，選擇**網站**> 一節。
4. 填寫**網站 URL**欄位與您的網站 URL (例如， `http://www.example.com`)。 **網域**欄位是選擇性的; 您可以使用此提供驗證整個網域 (例如*example.com*)。 

    > [!NOTE]
    > 如果您正在執行網站的 url 在本機電腦上要`http://localhost:12345`（其中數目是本機連接埠號碼），您可以將此值以**網站 URL**欄位以供測試您的網站。 不過，任何時候您本機站台的變更的通訊埠編號，您將需要更新**網站 URL**應用程式的欄位。
5. 選擇**儲存變更** 按鈕。
6. 選擇**應用程式**同樣地，索引標籤，然後再檢視應用程式的 [開始] 頁面。
7. 複製**應用程式識別碼**和**應用程式秘鑰**應用程式的值並貼到暫時的文字檔。 在網站上的程式碼中，您將會 Facebook 提供者來傳遞這些值。
8. 結束 Facebook 開發人員網站。

現在您變更兩個頁面中您的網站，讓使用者將能夠登入使用其 Facebook 帳戶的站台。

1. 建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。
2. 開啟 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Facebook OAuth 提供者。 取消註解的程式碼區塊看起來如下： 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. 複製**應用程式識別碼**從 Facebook 應用程式做為值的值`appId`（引號） 內的參數。
4. 複製**應用程式秘鑰**從 Facebook 應用程式做為值`appSecret`參數值。
5. 儲存並關閉檔案。

### <a name="testing-facebook-login"></a>測試 Facebook 登入

1. 執行站台的*default.cshtml*頁面上，然後選擇 [**登入**] 按鈕。
2. 在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Facebook**圖示。 

    網頁上將要求重新導向至 Facebook 登入頁面。

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. 登入 Facebook 帳戶。 

    程式碼來驗證您使用 Facebook 語彙基元，然後傳回頁面您可以在其中將與您的網站登入您的 Facebook 登入。 使用者名稱或電子郵件地址填入**電子郵件**欄位在表單上。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. 選擇**關聯** 按鈕。 

    瀏覽器會返回 [首頁] 頁面，並在登入。

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>啟用 Twitter 登入

1. 瀏覽至[Twitter 開發人員網站](https://dev.twitter.com/)。
2. 選擇**建立應用程式**連結，然後再登入網站。
3. 在**建立應用程式**表單中，填寫**名稱**和**描述**欄位。
4. 在**網站**欄位中，輸入您網站的 URL (例如， `http://www.example.com`)。 

    > [!NOTE]
    > 如果您要測試您的網站，在本機 (使用的 URL，例如`http://localhost:12345`)，Twitter 可能無法接受 URL。 不過，您可以使用本機回送 IP 位址 (例如`http://127.0.0.1:12345`)。 這樣可簡化測試您的應用程式在本機的程序。 不過，每次變更您的本機網站的通訊埠編號，您必須先更新**網站**應用程式的欄位。
5. 在**回呼 URL**欄位中，輸入您的網站，您要讓使用者在後返回記錄到 Twitter 中頁面的 URL。 例如，要傳送給使用者 （這會將其登入的狀態） 的入門網站的首頁，輸入您在輸入的相同 URL**網站**欄位。
6. 接受條款，然後選擇 [**建立應用程式 Twitter** ] 按鈕。
7. 在**我的應用程式**登陸頁面上，選擇您所建立的應用程式。
8. 在**詳細資料**索引標籤上，捲動到底部，然後選擇**建立我的存取權杖** 按鈕。
9. 上**詳細資料**索引標籤上，複製**取用者索引鍵**和**取用者秘密**應用程式的值並貼到暫時的文字檔。 在網站上的程式碼中，您將 Twitter 提供者來傳遞這些值。
10. 結束 Twitter 站台。

現在您變更兩個頁面中您的網站，讓使用者能夠登入使用 Twitter 帳戶的網站。

1. 建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。
2. 開啟 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Twitter OAuth 提供者。 取消註解的程式碼區塊看起來像這樣： 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. 複製**取用者索引鍵**從 Twitter 應用程式做為值的值`consumerKey`（引號） 內的參數。
4. 複製**取用者秘密**從 Twitter 應用程式做為值的值`consumerSecret`參數。
5. 儲存並關閉檔案。

### <a name="testing-twitter-login"></a>測試 Twitter 登入

1. 執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。
2. 在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Twitter**圖示。 

    網頁的要求重新導向至您所建立的應用程式的 Twitter 登入頁面。

    ![oauth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. 登入 Twitter 帳戶。
4. 程式碼來驗證使用者使用 Twitter 語彙基元，然後傳回您的頁面您可在關聯與您網站的帳戶登入。 您的名稱或電子郵件地址填入**電子郵件**欄位在表單上。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. 選擇**關聯** 按鈕。 

    瀏覽器會返回 [首頁] 頁面，並在登入。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


- [自訂全站台的行為](https://go.microsoft.com/fwlink/?LinkId=202906)
- [加入 ASP.NET Web Pages 站台中的安全性和成員資格](https://go.microsoft.com/fwlink/?LinkID=202904)
