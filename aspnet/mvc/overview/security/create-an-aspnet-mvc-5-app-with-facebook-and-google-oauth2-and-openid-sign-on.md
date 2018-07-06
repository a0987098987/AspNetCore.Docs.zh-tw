---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: 建立 MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何建置 ASP.NET MVC 5 web 應用程式，可讓使用者能夠登入使用 OAuth 2.0 搭配來自外部的驗證認證...
ms.author: aspnetcontent
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 6af4990f726bfcd0c45eb6991418661f9b8ccbf6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824702"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>建立 ASP.NET MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何使用建置 ASP.NET MVC 5 web 應用程式，可讓使用者能夠登入[OAuth 2.0](http://oauth.net/2/)使用來自外部驗證提供者，例如 Facebook、 Twitter、 LinkedIn、 Microsoft 或 Google 的認證。 為了簡單起見，本教學課程著重於使用來自 Facebook 和 Google 的認證。
> 
> 啟用您的網站中的這些認證提供極大的好處，因為數百萬位使用者已將這些外部提供者的帳戶。 這些使用者可能會更有必要，如果它們不需要建立並記住一組新的認證，登入您的網站。
> 
> 另請參閱[使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)。
> 
> 本教學課程也會示範如何新增使用者設定檔資料，以及如何使用成員資格 API 來新增角色。 本教學課程以寫入[Rick Anderson](https://blogs.msdn.com/rickAndy) (請在 Twitter 上關注我： [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="start"></a>
## <a name="getting-started"></a>快速入門

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。 如需使用 Dropbox、 GitHub、 Linkedin、 Instagram、 緩衝區、 Salesforce、 資料流、 Stack Exchange、 Tripit、 Twitch、 Twitter、 yahoo ！ 和更多說明，請參閱此[範例專案](https://github.com/matthewdunsdon/oauthforaspnet)。

> [!NOTE]
> 您必須安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本，若要使用 Google OAuth 2 並在本機偵錯，而不需要 SSL 警告。


按一下 **新的專案**從**開始**頁面上，或者您可以使用功能表，然後選取**檔案**，然後**新專案**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>建立第一個應用程式

按一下 **新的專案**，然後選取**Visual C#** 左邊，然後**Web** ，然後選取**ASP.NET Web 應用程式**。 命名您的專案 「 MvcAuth"，然後按一下**確定**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

在 [**新的 ASP.NET 專案**] 對話方塊中，按一下**MVC**。 如果無法驗證**個別使用者帳戶**，按一下**變更驗證**按鈕，然後選取**個別使用者帳戶**。 藉由檢查**雲端中的主機**，應用程式將會很容易就能在 Azure 中裝載。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

如果您選取**雲端中的主機**，完成 [設定] 對話方塊。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>使用 NuGet 來更新至最新的 OWIN 中介軟體

使用 NuGet 套件管理員來更新[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)。 選取 **更新**左側功能表中。 您可以按一下**全部更新** 按鈕，或者您可以搜尋只 OWIN 套件 （在下圖所示）：

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

下圖中會顯示只有 OWIN 封裝：

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

從套件管理員主控台 (PMC)，您可以輸入`Update-Package`命令，將會更新所有封裝。

按下**F5**或是**Ctrl + F5**執行應用程式。 在下面的影像中的連接埠號碼是 1234。 當您執行應用程式時，您會看到不同的連接埠號碼。

根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示，以查看**首頁**，**有關**，**連絡**，**註冊**並**登入**連結。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>設定專案中的 SSL

若要連接到 Google 和 Facebook 等的驗證提供者，您必須將 IIS Express 設定為使用 SSL。 務必要保留登入之後，使用 SSL 以及不卸除回為 HTTP，您的登入 cookie 只做為密碼做為您的使用者名稱和密碼，且未使用的 SSL，您要先將它以純文字傳送，透過網路。 此外，您等於已經跨執行交握及安全通道 （這是大多數有何 HTTPS 比 HTTP 更慢） 的時間執行 MVC 管線之前，因此重新導向回 HTTP 之後您的登入並不會造成未來的目前要求要求快得多。

1. 在 **方案總管**，按一下**MvcAuth**專案。
2. 按 F4 鍵以顯示專案屬性。 或者，從**檢視**您可以選取的功能表**屬性 視窗**。
3. 變更**啟用 SSL**設為 True。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. 複製 SSL URL (這將成為`https://localhost:44300/`除非您已建立 SSL 的其他專案)。
5. 在 **方案總管**，以滑鼠右鍵按一下**MvcAuth**專案，然後選取**屬性**。
6. 選取 [ **Web**索引標籤，然後貼到 SSL URL**專案 Url** ] 方塊中。 儲存檔案 (Ctl + S)。 您將需要此 URL，以設定 Facebook 和 Google 驗證的應用程式。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. 新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)屬性設定為`Home`控制器要求所有要求都必須使用 HTTPS。 更安全的方法是新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)應用程式的篩選條件。 請參閱章節&quot;保護的應用程式使用 SSL 和授權屬性&quot;在我 tutoral[使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 主控制器的部分如下所示。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. 按 CTRL+F5 執行應用程式。 如果您已安裝憑證，在過去，您可以略過本節的其餘部分，並跳至[建立 Google app for OAuth 2 和應用程式連接到專案](#goog)，否則請遵循指示來信任的自我簽署IIS Express 產生的憑證。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. 讀取**安全性警告**對話方塊，然後按一下**是**如果您想要安裝憑證，代表 localhost。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE 顯示*首頁*頁面上，並有沒有出現 SSL 警告。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome 也接受憑證，並會顯示 HTTPS 內容，而不發出警告。 Firefox 會使用自己的憑證存放區，因此它會顯示警告。 我們的應用程式對於您可以放心地按一下**我了解風險**。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>建立 Google app for OAuth 2 和應用程式連線至專案

> [!WARNING]
> 目前的 Google OAuth 指示，請參閱[ASP.NET Core 中的 設定 Google 驗證](/aspnet/core/security/authentication/social/google-logins)。

1. 瀏覽至[Google 開發人員主控台](https://console.developers.google.com/)。
2. 如果您尚未建立的專案之前，請選取**認證**左側的索引標籤，然後選取**建立**。
3. 在左側索引標籤中，按一下**認證**。
4. 按一下 **建立認證**再**OAuth 用戶端識別碼**。 

    1. 在 [**建立用戶端識別碼**] 對話方塊中，保留預設值**Web 應用程式**應用程式類型。
    2. 設定**授權的 JavaScript**來源，則您在上面使用的 SSL URL (`https://localhost:44300/`除非您已建立 SSL 的其他專案)
    3. 設定**授權重新導向 URI**來：  
         `https://localhost:44300/signin-google`
5. 按一下 OAuth 同意畫面功能表項目，然後設定您的電子郵件地址和產品名稱。 當您完成表單按一下**儲存**。
6. 按一下 [程式庫] 功能表項目、 搜尋**Google + API**、 對它按一下，然後按下啟用。
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   下圖顯示已啟用的 Api。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. 從 [Google Api API 管理員] 中，瀏覽**認證**索引標籤，以取得**用戶端識別碼**。 若要對應用程式祕密儲存 JSON 檔案的下載。 複製並貼上**ClientId**並**ClientSecret**成`UseGoogleAuthentication`方法中找到*Startup.Auth.cs*檔案*App_Start*資料夾。 **ClientId**並**ClientSecret**如下所示的值是範例，而且無法運作。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > 安全性-機密資料絕不儲存在原始程式碼中。 上述保持簡單的範例程式碼中加入的帳戶和認證。 請參閱[將密碼和其他機密資料部署到 ASP.NET 和 Azure App Service 最佳作法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
8. 按下**CTRL + F5**以建置並執行應用程式。 按一下 **登入**連結。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. 底下**另一個服務用來登入**，按一下**Google**。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > 若您遺漏任何上述步驟，您會將 HTTP 401 錯誤。 重新檢查您上述的步驟。 如果您遺漏必要的設定 (例如**產品名稱**)、 新增遺失的項目，並儲存，可能需要幾分鐘，讓驗證才能運作。
10. 您將會重新導向至 Google 網站，您會在此輸入您的認證。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. 您輸入認證之後，系統會提示您授與您剛才建立的 web 應用程式的權限：
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. 按一下 **接受**。 您現在會重新導向回到**註冊**MvcAuth 應用程式，您可以在此註冊您的 Google 帳戶頁面。 您可以選擇變更用於 Gmail 帳戶、 本機電子郵件註冊名稱，但您通常想要保留預設電子郵件別名 （也就是一個您用來驗證）。 按一下 [註冊]。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>在 Facebook 中建立應用程式和應用程式連線至專案

> [!WARNING]
> 目前 Facebook OAuth2 驗證的指示，請參閱[設定 Facebook 驗證](/aspnet/core/security/authentication/social/facebook-logins)

Facebook OAuth2 驗證，您需要將複製到您的專案部分設定從您在 Facebook 中建立的應用程式。

1. 在瀏覽器中瀏覽至[ https://developers.facebook.com/apps ](https://developers.facebook.com/apps)並輸入您的 Facebook 認證登入。
2. 如果您未註冊為 Facebook 開發人員，按一下**註冊為開發人員**並依照指示進行註冊。
3. 在 **應用程式**索引標籤上，按一下**建立新的應用程式**。

    ![建立新的應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. 請輸入**應用程式名稱**並**類別目錄**，然後按一下 **建立的應用程式**。

    <strong>應用程式命名空間</strong>屬於您的應用程式將用來存取驗證的 Facebook 應用程式的 URL (例如，https\://apps.facebook.com/{App 命名空間})。 如果您未指定<strong>應用程式命名空間</strong>，則<strong>應用程式識別碼</strong>將用於 URL。 <strong>應用程式識別碼</strong>是長時間的系統產生的數字，您會看到的下一個步驟。

    ![建立新的應用程式 對話方塊](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. 送出標準的安全性檢查。

    ![安全性檢查](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. 選取 **設定**左側的功能表列![Facebook 開發人員的功能表列](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. 在上**基本**選取頁面的 [設定] 區段**新增平台**來指定您要加入網站應用程式。 ![基本設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. 選取 **網站**從平台的選擇。  
  
    ![平台選擇](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. 請記下的您**應用程式識別碼**和您**應用程式祕密**，讓您可以新增到您的 MVC 應用程式的這兩個稍後在本教學課程。 此外，新增您的網站 URL (`https://localhost:44300/`) 來測試您的 MVC 應用程式。 此外，新增**Contact Email**。 然後，選取**儲存變更**。   

    ![基本應用程式詳細資料頁面](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > 請注意，您將只能使用已註冊的電子郵件別名來進行驗證。 其他使用者與測試帳戶將無法註冊。 您可以授與其他 Facebook 帳戶的存取權的 Facebook 應用程式**開發人員角色** 索引標籤。
10. 在 Visual Studio 中開啟*應用程式\_Start\Startup.Auth.cs*。
11. 複製並貼上**AppId**並**應用程式祕密**成`UseFacebookAuthentication`方法。 **AppId**並**應用程式祕密**如下所示的值是範例，將無法運作。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. 按一下 **儲存變更**。
13. 按下**CTRL + F5**執行應用程式。


選取 **登入**以顯示登入頁面。 按一下  **Facebook**下方**使用其他服務進行登入。**

輸入您的 Facebook 認證。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

系統會提示您授與應用程式存取您的公用設定檔及朋友清單的權限。

![Facebook 應用程式詳細資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

您現在已登入。 您現在可以註冊此帳戶與應用程式。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

當您註冊時，要將項目加入至*使用者*的成員資格資料庫的資料表。

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>檢查成員資格資料

在 **檢視**功能表上，按一下**伺服器總管**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

依序展開**DefaultConnection (MvcAuth)**，展開**資料表**，以滑鼠右鍵按一下**AspNetUsers**然後按一下**顯示資料表資料**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers 資料表資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>將設定檔資料新增至 User 類別

在本節中您將新增出生日期和主要城鎮的使用者資料在註冊期間，如下圖所示。

![使用家用 town 和 Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

開啟*Models\IdentityModels.cs*檔案，並新增出生日期和從家裡 town 屬性：

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

開啟*Models\AccountViewModels.cs*檔案與設定的 birth 中的日期和從家裡 town 屬性`ExternalLoginConfirmationViewModel`。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

開啟*Controllers\AccountController.cs*檔案，並新增程式碼中的出生日期和從家裡 town`ExternalLoginConfirmation`動作方法所示：

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

新增出生日期及首頁的城鎮*Views\Account\ExternalLoginConfirmation.cshtml*檔案：

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

因此您可以再次向您的應用程式註冊您的 Facebook 帳戶，並確認您可以加入新的出生日期和主要城鎮的設定檔資訊，請刪除成員資格資料庫。

從**方案總管**，按一下**顯示所有檔案**圖示，然後以滑鼠右鍵按一下*新增\_Data\aspnet-MvcAuth-&lt;時間戳記&gt;.mdf* ，按一下 **刪除**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

從**工具**功能表上，按一下**NuGet 封裝管理員**，然後按一下**Package Manager Console** (PMC)。 在 PMC 中，輸入下列命令。

1. 啟用移轉
2. 新增移轉 Init
3. 更新資料庫

執行應用程式，並使用 FaceBook 和 Google 登入並註冊某些使用者。

## <a name="examine-the-membership-data"></a>檢查成員資格資料

在 **檢視**功能表上，按一下**伺服器總管**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

以滑鼠右鍵按一下**AspNetUsers**然後按一下**顯示資料表資料**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown`和`BirthDate`如下所示的欄位。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>登出您的應用程式，並使用另一個帳戶中的記錄

如果您登入您的應用程式使用 Facebook、，然後登出並嘗試登入一次使用不同的 Facebook 帳戶 （使用相同的瀏覽器），您會立即登入使用先前用過的 Facebook 帳戶。 若要使用另一個帳戶，您需要瀏覽至 Facebook，並在 Facebook 登出。 相同的規則適用於任何其他第 3 方驗證提供者。 或者，您可以使用不同的瀏覽器登入另一個帳戶。

## <a name="next-steps"></a>後續步驟

請參閱[簡介 Yahoo 和 LinkedIn OAuth 安全性提供者設定 owin](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)由 Jerrie Pelser Yahoo 與 LinkedIn 的指示。 請參閱 Jerrie 的美化取得啟用社交登入按鈕的 ASP.NET MVC 5 的社交登入按鈕。

請遵循我教學課程[使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)，它會繼續本教學課程中，並顯示下列：

1. 如何將您的應用程式部署至 Azure。
2. 如何保護您的應用程式角色。
3. 如何保護您的應用程式[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)並[授權](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)篩選器。
4. 如何使用成員資格 API 來新增使用者和角色。

您喜歡本教學課程中的方式，和我們可以改善，歡迎留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。 您甚至可以要求並票選新功能新增至 ASP.NET。 例如，您可以在此投票的工具[建立和管理使用者和角色。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

ASP.NET 外部驗證服務的運作方式的合理的解釋，請參閱 Robert McMurray[外部驗證服務](https://asp.net/web-api/overview/security/external-authentication-services)。 Robert 的文章也會啟用 Microsoft 和 Twitter 驗證的詳細資料。 Tom Dykstra 的傑出[EF/MVC 教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)示範如何使用 Entity Framework。
