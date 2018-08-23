---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: 登入使用外部網站，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 這篇文章說明如何登入您使用 Facebook、 Google、 Twitter、 Yahoo 和其他站台的 ASP.NET Web Pages (Razor) 網站 — 也就是如何支援...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a74b13e9d1ddb5bc02f4ea5184108de5e014ead0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831746"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>使用 ASP.NET Web Pages (Razor) 網站中的外部網站登入
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何登入您使用 Facebook、 Google、 Twitter、 Yahoo 和其他站台的 ASP.NET Web Pages (Razor) 網站 — 也就是如何在您的站台支援 OAuth 和 OpenID。
> 
> 您將學到什麼：
> 
> - 如何啟用從其他站台的登入，當您使用 WebMatrix 入門網站範本。
> 
> 這是發行項中導入的 ASP.NET 功能：
> 
> - `OAuthWebSecurity`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

ASP.NET Web 網頁包含支援[OAuth](http://oauth.net/)並[OpenID](http://openid.net/)提供者。 使用這些提供者，您可以讓使用者登入您的網站使用其利用 Facebook、 Twitter、 Microsoft 和 Google 的現有認證。 比方說，若要登入的 Facebook 帳戶，使用者就可以選擇 [Facebook] 圖示，將它們重新導向至 Facebook 登入頁面，讓他們輸入其使用者資訊。 它們可以再將其帳戶在您的網站上 Facebook 登入關聯。 網頁的成員資格功能相關的增強功能是使用者可以將產生關聯 （包括從社交網路網站的登入） 的多個登入與您的網站上的單一帳戶。

下圖顯示登入頁面**入門網站**範本，使用者可以在其中選擇 Facebook、 Twitter、 Google 或 Microsoft 的圖示，以啟用登入的外部帳戶：

![外部提供者](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

您可以啟用 OAuth 和 OpenID 成員資格中的程式碼的幾行取消註解**入門網站**範本。 您的方法和屬性用來處理與 OAuth 和 OpenID 提供者位於`WebMatrix.Security.OAuthWebSecurity`類別。 **入門網站**範本包含完整的成員資格的基礎結構，您需要讓使用者登入您的網站使用本機認證或來自另一個站台完成登入頁面、 成員資格資料庫中，與所有的程式碼.

本節提供如何讓使用者登入從外部站台的站台為基礎的範例**入門網站**範本。 建立入門網站之後, 您這樣做，（詳細資料）：

- 使用 OAuth 提供者 （Facebook、 Twitter 和 Microsoft） 的網站，您可以建立應用程式外部站台上。 這可讓您將需叫用這些站台的登入功能的應用程式金鑰。
- 使用 OpenID 提供者 (Google) 的網站，您不必建立應用程式。 針對所有的這些網站，您必須有帳戶才能登入，並建立開發人員應用程式。

    > [!NOTE]
    > Microsoft 應用程式只接受即時工作網站 URL，因此您無法使用本機的網站 URL，測試登入。
- 以指定適當的驗證提供者，並提交至您想要使用的站台的登入，請編輯您的網站中的幾個檔案。

本文提供不同的指示，來執行下列工作：

- [啟用 Google 登入](#To_enable_Google_logins)
- [啟用 Facebook 登入](#To_enable_Facebook_logins)
- [啟用 Twitter 登入](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>啟用 Google 登入

1. 建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。
2. 開啟 *\_AppStart.cshtml*頁面上，並取消註解下列程式碼行。 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>測試 Google 登入

1. 執行*default.cshtml*網站頁面，然後選擇**登入** 按鈕。
2. 上*登入*頁面上，於**使用其他服務進行登入**區段中，選擇  **Google**或是**Yahoo**送出按鈕。 此範例會使用 Google 登入。 

    網頁上將要求重新導向至 Google 登入頁面。

    ![Google 登入](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. 輸入現有的 Google 帳戶認證。
4. 如果 Google 會詢問您是否要允許*Localhost*若要使用來自帳戶的資訊，請按一下 **允許**。

    程式碼會驗證使用者，使用 Google 的權杖，然後回到此頁面，在您的網站。 這個頁面可讓您在網站上，現有的帳戶相關聯其 Google 登入的使用者，或他們可以註冊新的帳戶建立關聯外部登入與您站台上。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. 選擇**產生關聯** 按鈕。 在瀏覽器會返回您的應用程式首頁。

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>啟用 Facebook 登入

1. 移至[Facebook 開發人員網站](https://developers.facebook.com/apps)（登入如果您還沒登入）。
2. 選擇**建立新的應用程式**按鈕，然後依照提示來命名，並建立新的應用程式。
3. 一節**選取您的應用程式將會如何整合 Facebook**，選擇**網站**一節。
4. 填寫**站台 URL**欄位與您網站的 URL (例如`http://www.example.com`)。 **網域**欄位是選擇性的; 您可以使用此選項來提供整個網域的驗證 (例如*example.com*)。 

    > [!NOTE]
    > 如果您正在 URL 與您本機電腦上的站台也`http://localhost:12345`（其中數字是本機連接埠號碼），您可以新增此值，以**站台 URL**欄位來測試您的網站。 不過，任何時候您本機站台的變更的通訊埠編號，您必須更新**站台 URL**應用程式的欄位。
5. 選擇**儲存變更** 按鈕。
6. 選擇**應用程式**同樣地，索引標籤，然後檢視 應用程式的 開始 頁面。
7. 複製**應用程式識別碼**並**應用程式祕密**應用程式的值並貼到暫存的文字檔。 網站程式碼中，您將 Facebook 提供者來傳遞這些值。
8. 結束 Facebook 開發人員網站。

現在您對變更兩個頁面在您的網站，讓使用者能夠登入使用其 Facebook 帳戶的網站。

1. 建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。
2. 開啟 *\_AppStart.cshtml*頁面和程式碼取消註解的 Facebook OAuth 提供者。 取消註解的程式碼區塊看起來如下所示： 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. 複製**應用程式識別碼**Facebook 應用程式的值介於`appId`參數 （以引號）。
4. 複製**應用程式祕密**從 Facebook 應用程式，做為值`appSecret`參數值。
5. 儲存並關閉檔案。

### <a name="testing-facebook-login"></a>測試 Facebook 登入

1. 執行站台*default.cshtml*頁面上，然後選擇**登入** 按鈕。
2. 在 *登入*頁面上，於**另一個服務用來登入**區段中，選擇**Facebook**圖示。 

    網頁上將要求重新導向至 Facebook 登入頁面。

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. 登入的 Facebook 帳戶。 

    程式碼來驗證您使用 Facebook 權杖，然後返回頁面，您可以讓您的 Facebook 登入關聯站台的登入。 您的使用者名稱或電子郵件地址填入**電子郵件**欄位在表單上。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. 選擇**產生關聯** 按鈕。 

    瀏覽器會返回首頁並登入。

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>啟用 Twitter 登入

1. 瀏覽至[Twitter 開發人員網站](https://dev.twitter.com/)。
2. 選擇**建立應用程式**連結，然後再登入網站。
3. 在上**建立應用程式**表單中填寫**名稱**並**描述**欄位。
4. 在 **網站**欄位中，輸入您網站的 URL (例如`http://www.example.com`)。 

    > [!NOTE]
    > 如果您要測試您的網站，在本機 (使用之類的 URL `http://localhost:12345`)，Twitter 可能不會接受 URL。 不過，您可以使用本機回送 IP 位址 (例如`http://127.0.0.1:12345`)。 這可簡化測試您的應用程式在本機的程序。 不過，每次您的本機網站的連接埠號碼變更時，您將需要更新**網站**應用程式的欄位。
5. 在 **回呼 URL**欄位中，輸入您要讓使用者之後若要返回登入 Twitter 的網站中的頁面的 URL。 比方說，若要將使用者傳送至 （這會辨識其登入的狀態） 的入門網站的 [首頁] 頁面中，輸入相同 URL 即可中輸入**網站**欄位。
6. 接受條款，然後選擇**建立 Twitter 應用程式** 按鈕。
7. 在 **我的應用程式**登陸頁面上，選擇您所建立的應用程式。
8. 在 [**詳細資料**索引標籤上，捲動到底部，然後選擇**建立我的存取權杖**] 按鈕。
9. 上**詳細資料**索引標籤上，複製**取用者索引鍵**並**取用者祕密**應用程式的值並貼到暫存的文字檔。 您要傳遞至 Twitter 提供者的這些值，在您的網站程式碼。
10. 結束 Twitter 網站。

現在您對變更兩個頁面在您的網站，讓使用者能夠登入使用 Twitter 帳戶的網站。

1. 建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。
2. 開啟 *\_AppStart.cshtml*頁面及 Twitter OAuth 提供者，程式碼取消註解。 取消註解的程式碼區塊看起來像這樣： 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. 複製**取用者索引鍵**從 Twitter 應用程式，做為值的值`consumerKey`參數 （以引號）。
4. 複製**取用者祕密**從 Twitter 應用程式，做為值的值`consumerSecret`參數。
5. 儲存並關閉檔案。

### <a name="testing-twitter-login"></a>測試 Twitter 登入

1. 執行*default.cshtml*網站頁面，然後選擇**登入** 按鈕。
2. 在 *登入*頁面上，於**另一個服務用來登入**區段中，選擇**Twitter**圖示。 

    網頁的要求重新導向至您所建立的應用程式的 Twitter 登入頁面。

    ![oauth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. 登入 Twitter 帳戶。
4. 程式碼來驗證使用者使用 Twitter 語彙基元，然後傳回您頁面，您可以將與您網站的帳戶登入。 您的姓名或電子郵件地址填入**電子郵件**欄位在表單上。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. 選擇**產生關聯** 按鈕。 

    瀏覽器會返回首頁並登入。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


- [自訂全網站行為](https://go.microsoft.com/fwlink/?LinkId=202906)
- [加入 ASP.NET Web Pages 網站中的安全性及成員資格](https://go.microsoft.com/fwlink/?LinkID=202904)
