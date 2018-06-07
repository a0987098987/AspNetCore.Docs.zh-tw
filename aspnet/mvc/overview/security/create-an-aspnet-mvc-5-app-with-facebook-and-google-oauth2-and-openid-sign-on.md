---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: 建立 MVC 5 應用程式與 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程會示範如何建置讓使用者能夠登入來自外部的驗證認證搭配使用 OAuth 2.0 的 ASP.NET MVC 5 web 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: aa4c91865f7b720846a5e8deb4281c3ca6933c8e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819093"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>建立 ASP.NET MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何建立 ASP.NET MVC 5 web 應用程式，讓使用者能夠登入使用[OAuth 2.0](http://oauth.net/2/)使用從外部驗證提供者，例如 Facebook、 Twitter、 LinkedIn、 Microsoft 或 Google 認證。 為了簡單起見，本教學課程著重於使用 Facebook 和 Google 認證。
> 
> 啟用瀏覽的網站中的這些認證提供更大的優點，因為已經有數百萬名使用者的帳戶與這些外部提供者。 這些使用者可能比較想要註冊您的站台如果它們不需要建立並記住一組新的認證。
> 
> 另請參閱[使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)。
> 
> 本教學課程也會示範如何加入分析資料的使用者，以及如何使用可將角色的成員資格應用程式開發介面。 本教學課程中所編寫的[Rick Anderson](https://blogs.msdn.com/rickAndy) (請在 Twitter 上關注我： [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="start"></a>
## <a name="getting-started"></a>快速入門

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。 Dropbox、 GitHub、 Linkedin、 Instagram、 緩衝區、 Salesforce、 資料流、 堆疊 Exchange、 Tripit、 Twitch、 Twitter、 yahoo ！ 和更多的說明，請參閱此[範例專案](https://github.com/matthewdunsdon/oauthforaspnet)。

> [!NOTE]
> 您必須安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本，才能使用 Google OAuth 2 和偵錯在本機，而 SSL 警告。


按一下**新專案**從**啟動** 頁面上，或者您可以使用功能表並選取**檔案**，然後**新專案**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>建立第一個應用程式

按一下**新專案**，然後選取**Visual C#** 在左邊，然後**Web** ，然後選取  **ASP.NET Web 應用程式**。 將您的專案"MvcAuth"，再按一下**確定**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

在**新增 ASP.NET 專案**] 對話方塊中，按一下 [ **MVC**。 如果無法驗證**個別使用者帳戶**，按一下 **變更驗證**按鈕，然後選取**個別使用者帳戶**。 藉由檢查**雲端中的主機**，應用程式將會很容易就能在 Azure 中裝載。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

如果您選取**雲端中的主機**，完成 [設定] 對話方塊。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>使用 NuGet 來更新為最新的 OWIN 中介軟體

使用 NuGet 套件管理員來更新[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)。 選取**更新**左側功能表中。 您可以按一下**全部更新** 按鈕，或者您可以搜尋只有 OWIN 封裝 （在下圖所示）：

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

在下列影像顯示只有 OWIN 封裝：

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

從封裝管理員主控台 (PMC)，您可以輸入`Update-Package`命令，將會更新所有封裝。

按**F5**或**Ctrl + F5**執行應用程式。 在下面的影像中的連接埠號碼是 1234。 當您執行應用程式時，您會看到不同的通訊埠編號。

根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示以查看**首頁**，**有關**，**連絡人**，**註冊**和**登入**連結。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>在專案中的 SSL 設定

若要連接到 Google 和 Facebook 驗證提供者，您必須將 IIS Express 設定為使用 SSL。 請務必保留登入之後，使用 SSL 以及不卸除回為 HTTP，登入 cookie 只做為密碼做為您的使用者名稱和密碼，而不使用的 SSL，您要先將它以純文字傳送，在網路上。 此外，您已記執行交握和安全通道 （這是大量構成 HTTPS 低於 HTTP） 的時間執行 MVC 管線之前，因此將重新導向回到 HTTP 您登入之後將不會使目前的要求或未來要求快得多。

1. 在**方案總管] 中**，按一下 [ **MvcAuth**專案。
2. 按 F4 鍵以顯示專案屬性。 或者，從**檢視**功能表您可以選取**屬性 視窗**。
3. 變更**啟用 SSL**為 True。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. 複製 SSL URL (這將成為`https://localhost:44300/`除非您已建立其他 SSL 專案)。
5. 在**方案總管 中**，以滑鼠右鍵按一下**MvcAuth**專案，然後選取**屬性**。
6. 選取**Web**索引標籤，然後貼到 SSL URL**專案 Url**方塊。 儲存檔案 (Ctl + S)。 您將需要此 URL，若要設定 Facebook 和 Google 驗證的應用程式。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. 新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)屬性`Home`需要的所有要求的控制站都必須使用 HTTPS。 最安全的作法是新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)應用程式的篩選。 請參閱節&quot;保護應用程式的 SSL 和授權屬性&quot;在我 tutoral[驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 如下所示主控制器的一部分。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. 按 CTRL+F5 執行應用程式。 如果您已安裝憑證，在過去，您可以略過此章節的其餘部分，並跳至[OAuth 2 建立 Google 應用程式和應用程式連接至專案](#goog)，否則請遵循指示信任的自我簽署IIS Express 產生的憑證。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. 讀取**安全性警告**對話方塊，然後按一下**是**如果您想要安裝憑證，表示 localhost。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. 即會顯示*首頁*頁面上並不沒有出現任何 SSL 的警告。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome 也接受該憑證，並會顯示 HTTPS 內容，而不發出警告。 Firefox 會使用自己的憑證存放區，因此它會顯示警告。 對於我們的應用程式可以安全地按一下**我了解風險**。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>OAuth 2 建立 Google 應用程式和應用程式連接至專案

> [!WARNING]
> 目前的 Google OAuth 指示，請參閱[設定 Google 驗證中 ASP.NET Core](/aspnet/core/security/authentication/social/google-logins)。

1. 瀏覽至[Google 開發人員主控台](https://console.developers.google.com/)。
2. 如果您尚未建立的專案之前，請選取**認證**在左側的索引標籤上，然後選取**建立**。
3. 在左側的索引標籤上，按一下**認證**。
4. 按一下**建立認證**然後**OAuth 用戶端識別碼**。 

    1. 在**建立用戶端識別碼** 對話方塊中，保留預設值**Web 應用程式**應用程式類型。
    2. 設定**授權 JavaScript**來源可您在上面使用的 SSL URL (`https://localhost:44300/`除非您已建立其他 SSL 專案)
    3. 設定**授權重新導向 URI**至：  
         `https://localhost:44300/signin-google`
5. 按一下 OAuth 同意畫面功能表項目，然後設定您的電子郵件地址和產品名稱。 當您完成表單按一下**儲存**。
6. 按一下文件庫功能表項目，請搜尋**Google + API**，請按一下它，然後按下啟用。
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   下圖顯示已啟用的應用程式開發介面。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. 從 Google Api API 管理員 中，請瀏覽**認證**索引標籤，以取得**用戶端識別碼**。 下載應用程式密碼儲存在 JSON 檔案。 複製並貼上**ClientId**和**ClientSecret**到`UseGoogleAuthentication`方法中找到*Startup.Auth.cs*檔案*App_Start*資料夾。 **ClientId**和**ClientSecret**值如下所示的範例和沒有作用。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > 安全性-永遠不會儲存敏感的資料來源上的程式碼中。 為了簡化範例上述程式碼中加入的帳戶和認證。 請參閱[ASP.NET 和 Azure App Service 部署的密碼和其他機密資料的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
8. 按**CTRL + F5**建置並執行應用程式。 按一下**登入**連結。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. 在下**使用另一個服務登入**，按一下  **Google**。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > 若您遺漏任何上述的步驟會收到 HTTP 401 錯誤。 重新檢查您上述的步驟。 若您遺漏必要的設定 (例如**產品名稱**)、 新增遺失的項目，並儲存，則可能需要幾分鐘，讓驗證才能運作。
10. 安裝程式會重新導向至 Google 站台，您將在其中輸入您的認證。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. 輸入您的認證之後，系統會提示您授與您剛才建立的 web 應用程式的權限：
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. 按一下**接受**。 您將會被重新導向至**註冊**MvcAuth 應用程式，您可以在其中註冊您的 Google 帳戶頁面。 您可以選擇變更為您的 Gmail 帳戶所使用的本機電子郵件註冊名稱，但您通常想要保留的預設電子郵件別名 （也就是一個用來驗證）。 按一下 [註冊]。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>在 Facebook 中建立應用程式和應用程式連接至專案

> [!WARNING]
> 目前的 Facebook OAuth2 驗證指示，請參閱[設定 Facebook 驗證](/aspnet/core/security/authentication/social/facebook-logins)

Facebook OAuth2 驗證，您要複製到專案的某些設定從您在 Facebook 中建立的應用程式。

1. 在瀏覽器中，瀏覽至[ https://developers.facebook.com/apps ](https://developers.facebook.com/apps)並輸入您的 Facebook 認證登入。
2. 如果您不已註冊為 Facebook 開發人員，請按一下**身為開發人員註冊**並依照指示進行註冊。
3. 在**應用程式**索引標籤上，按一下 **建立新的應用程式**。

    ![建立新的應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. 輸入**應用程式名稱**和**類別**，然後按一下 **建立應用程式**。

    <strong>應用程式命名空間</strong>屬於您的應用程式用來存取驗證的 Facebook 應用程式的 URL (例如，https\://apps.facebook.com/{App 命名空間})。 如果您未指定<strong>應用程式命名空間</strong>、<strong>應用程式識別碼</strong>將使用的 URL。 <strong>應用程式識別碼</strong>是下一個步驟中，您會看到一個長時間的系統產生數字。

    ![建立新的應用程式 對話方塊](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. 提交的一般安全性檢查。

    ![安全性檢查](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. 選取**設定**左側的功能表列![Facebook 開發人員的功能表列](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. 在**基本**頁面的 [設定] 區段選取**加入平台**來指定您要加入網站的應用程式。 ![基本設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. 選取**網站**從平台的選項。  
  
    ![平台選項](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. 請記下您**應用程式識別碼**和您**應用程式秘鑰**，讓您可以稍後在本教學課程新增到您的 MVC 應用程式。 此外，請加入您的站台 URL (`https://localhost:44300/`) 來測試 MVC 應用程式。 此外，請加入**Contact Email**。 然後，選取**儲存變更**。   

    ![基本應用程式詳細資料頁面](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > 請注意，您只能使用已註冊的電子郵件別名來進行驗證。 其他使用者及測試帳戶將無法註冊。 您可以授與其他上 Facebook 應用程式的 Facebook 帳戶存取**開發人員角色** 索引標籤。
10. 在 Visual Studio 中開啟*應用程式\_Start\Startup.Auth.cs*。
11. 複製並貼上**AppId**和**應用程式秘鑰**到`UseFacebookAuthentication`方法。 **AppId**和**應用程式秘鑰**值如下所示的範例，將無法運作。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. 按一下**儲存變更**。
13. 按**CTRL + F5**執行應用程式。


選取**登入**以顯示登入頁面。 按一下**Facebook**下**使用另一個服務登入。**

輸入您的 Facebook 認證。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

系統會提示您授與應用程式存取您的公用設定檔和 friend 清單的權限。

![Facebook 應用程式詳細資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

您現在已登入。 您現在可以註冊此帳戶與應用程式。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

當您註冊時，要將項目加入至*使用者*成員資格資料庫資料表。

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>檢查成員資格資料

在**檢視**功能表上，按一下 **伺服器總管**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

展開**DefaultConnection (MvcAuth)**，依序展開**資料表**，以滑鼠右鍵按一下**AspNetUsers**按一下**顯示資料表資料**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers 資料表資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>將設定檔資料加入至使用者類別

本節中您會將出生日期以及主城鎮使用者資料在註冊期間，在下圖所示。

![使用家用城鎮和 Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

開啟*Models\IdentityModels.cs*檔案，然後加入出生日期和從家裡城鎮屬性：

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

開啟*Models\AccountViewModels.cs*檔案與設定的出生日期和從家裡城鎮屬性中的`ExternalLoginConfirmationViewModel`。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

開啟*Controllers\AccountController.cs*檔案，然後加入程式碼中的出生日期和從家裡城鎮`ExternalLoginConfirmation`動作方法所示：

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

新增生日及家用城鎮*Views\Account\ExternalLoginConfirmation.cshtml*檔案：

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

刪除成員資格資料庫，因此您可以再次向您的應用程式註冊您的 Facebook 帳戶，並確認您可以新增新的生日及家用城鎮設定檔資訊。

從**方案總管] 中**，按一下 [**顯示所有檔案**圖示，然後以滑鼠右鍵按一下*新增\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf*按一下**刪除**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

從**工具**功能表上，按一下  **NuGet 封裝管理員**，然後按一下  **Package Manager Console** (PMC)。 PMC 中輸入下列命令。

1. Enable-migrations
2. 新增移轉初始化
3. 更新資料庫

執行應用程式，並使用 FaceBook 和 Google 登入並註冊某些使用者。

## <a name="examine-the-membership-data"></a>檢查成員資格資料

在**檢視**功能表上，按一下 **伺服器總管**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

以滑鼠右鍵按一下**AspNetUsers**按一下**顯示資料表資料**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown`和`BirthDate`欄位如下所示。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>登出您的應用程式，並與另一個帳戶中的記錄

如果您登入您的應用程式與 Facebook、，然後登出並嘗試登入一次使用不同的 Facebook 帳戶 （使用相同的瀏覽器），您會立即登入使用先前用過的 Facebook 帳戶。 若要使用其他帳戶，您要瀏覽至 Facebook，並在 Facebook 登出。 相同的規則適用於任何其他第 3 個合作對象驗證提供者。 或者，您可以使用不同的瀏覽器登入另一個帳戶。

## <a name="next-steps"></a>後續步驟

請參閱[簡介的 Yahoo 和 LinkedIn OAuth 安全性提供者設定 owin](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)由 Jerrie Pelser Yahoo 和 LinkedIn 的指示。 請參閱 Jerrie 的 ASP.NET MVC 5，以取得啟用社交登入按鈕的社交登入按鈕看起來很。

請遵循我教學課程[驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)，這會繼續本教學課程，並顯示下列：

1. 如何將您的應用程式部署至 Azure。
2. 如何保護您的角色的應用程式。
3. 如何保護您的應用程式[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)和[授權](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)篩選器。
4. 如何使用應用程式開發介面的成員資格加入使用者和角色。

請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。 您甚至可以要求並票選加入 ASP.NET 的新功能。 例如，您可以在此投票的工具[建立及管理使用者和角色。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

ASP.NET 外部驗證服務的運作方式很好的說明，請參閱 Robert McMurray[外部驗證服務](https://asp.net/web-api/overview/security/external-authentication-services)。 Robert 的發行項也會啟用 Microsoft 和 Twitter 驗證的詳細資料。 Tom Dykstra 的絕佳[EF/MVC 教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)示範如何使用 Entity Framework。
