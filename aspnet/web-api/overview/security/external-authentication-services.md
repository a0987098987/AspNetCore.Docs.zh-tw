---
uid: web-api/overview/security/external-authentication-services
title: "外部驗證服務與 ASP.NET Web 應用程式開發介面 (C#) |Microsoft 文件"
author: rmcmurray
description: "描述如何使用 ASP.NET Web API 中的外部驗證服務。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 744396cb0c95d1887f259b1e2e890bd06ef7d049
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>外部驗證服務與 ASP.NET Web 應用程式開發介面 (C#)
====================
由[Robert McMurray](https://github.com/rmcmurray)

展開 visual Studio 2013 和 ASP.NET 4.5.1 的安全性選項[單一頁面應用程式](../../../single-page-application/index.md)(SPA) 和[Web API](../../index.md)與外部驗證服務，包括數個整合服務OAuth/OpenID 和社交媒體驗證服務： Microsoft 帳戶、 Twitter、 Facebook 和 Google。

### <a name="in-this-walkthrough"></a>在本逐步解說

- [使用外部驗證服務](#USING)
- [建立 Web 應用程式範例](#SAMPLE)
- [啟用 Facebook 驗證](#FACEBOOK)
- [啟用 Google 驗證](#GOOGLE)
- [啟用 Microsoft 驗證](#MICROSOFT)
- [啟用 Twitter 驗證](#TWITTER)
- [其他資訊](#MOREINFO)

    - [結合外部驗證服務](#COMBINE)
    - [設定 IIS Express 要使用完整網域名稱](#FQDN)
    - [如何取得 Microsoft 驗證的應用程式設定](#OBTAIN)
    - [選擇性： 停用本機註冊](#DISABLE)

### <a name="prerequisites"></a>必要條件

若要依照本逐步解說中的範例，您需要具備以下項目：

- Visual Studio 2013
- 至少一個下列外部驗證服務帳戶：

    - Google 使用者帳戶
    - 使用應用程式識別碼和祕密金鑰，如以下的社交媒體驗證服務的其中一個開發人員帳戶：

        - Microsoft 帳戶 ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>使用外部驗證服務

若要減少程式開發的 web 開發人員說明目前可用的外部驗證服務的眾多時建立新的 web 應用程式的時間。 Web 使用者時通常會有數個現有的帳戶以常用的 web 服務和社交媒體網站，因此從外部 web 服務或網站社交媒體服務驗證 web 應用程式實作，它會將儲存的開發時間，將已使建立驗證實作。 使用外部驗證服務會儲存使用者不必建立另一個帳戶用於您的 web 應用程式，而且也不需要記住另一個使用者名稱和密碼。

在過去，開發人員有兩個選擇： 建立自己的驗證實作，或了解如何將外部驗證服務整合至其應用程式。 在最基本的層級下, 圖說明簡單的要求流程使用者代理程式 （web 瀏覽器） 設定為使用外部驗證服務的 web 應用程式要求資訊：

[![](external-authentication-services/_static/image2.png "按一下以展開映像")](external-authentication-services/_static/image1.png)

在上圖中，使用者代理程式 （或在此範例中的網頁瀏覽器） 會提出要求的 web 應用程式，重新導向至外部驗證服務的網頁瀏覽器。 使用者代理程式會將憑證傳送至外部驗證服務，並已成功驗證的使用者代理程式，如果外部驗證服務重新導向使用者代理程式至原始的 web 應用程式，使用某種形式的語彙基元的使用者代理程式會將傳送至 web 應用程式。 Web 應用程式會使用語彙基元確認使用者代理程式已成功驗證外部驗證服務，而且 web 應用程式可能會使用權杖來收集使用者代理程式的詳細資訊。 完成應用程式正在處理使用者代理程式的資訊，web 應用程式會傳回適當的回應取決於其授權設定的使用者代理程式。

在第二個範例中，使用者代理程式與 web 應用程式和外部授權伺服器交涉和 web 應用程式會執行其他與外部授權伺服器通訊來擷取有關使用者的其他資訊代理程式：

[![](external-authentication-services/_static/image4.png "按一下以展開映像")](external-authentication-services/_static/image3.png)

Visual Studio 2013 和 ASP.NET 4.5.1 與外部驗證服務的整合更輕鬆地進行開發人員藉由提供內建整合，如以下驗證服務：

- Facebook
- Google
- Microsoft 帳戶 （Windows Live ID 帳戶）
- Twitter

在本逐步解說的範例將示範如何使用 Visual Studio 2013 中使用新的 ASP.NET Web 應用程式範本所隨附設定每個支援的外部驗證服務。

> [!NOTE]
> 如有必要，您可能需要將您的 FQDN 新增到您的外部驗證服務的設定。 這項需求根據需要應用程式設定中的 FQDN，以符合您的用戶端會使用 FQDN 某些外部驗證服務的安全性限制。 （這個步驟會有極大不同，為每個外部驗證服務，您必須參閱說明文件以查看是否需要為每個外部驗證服務，以及如何設定這些設定）。如果您要設定 IIS Express 來測試此環境中使用 FQDN，請參閱[使用完整網域名稱設定 IIS Express](#FQDN)稍後在本逐步解說 > 一節。


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>建立範例 Web 應用程式

下列步驟將引導您完成範例應用程式使用 ASP.NET Web 應用程式範本建立，並將此範例應用程式對每個外部驗證服務，稍後在本逐步解說。

啟動 Visual Studio 2013 選取**新專案**從 [開始] 頁面。 或從**檔案**功能表上，選取**新增**然後**專案**。

[![](external-authentication-services/_static/image6.png "按一下以展開映像")](external-authentication-services/_static/image5.png)

當**新專案**對話方塊隨即出現，請選取**已安裝****範本**展開**Visual C#**。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET Web 應用程式**。 輸入您的專案名稱，然後按一下**確定**。

[![](external-authentication-services/_static/image8.png "按一下以展開映像")](external-authentication-services/_static/image7.png)

當**新增 ASP.NET 專案**顯示，請選取**SPA**範本，然後按一下**建立專案**。

[![](external-authentication-services/_static/image10.png "按一下以展開映像")](external-authentication-services/_static/image9.png)

等候與 Visual Studio 2013 中建立您的專案。

[![](external-authentication-services/_static/image12.png "按一下以展開映像")](external-authentication-services/_static/image11.png)

Visual Studio 2013 已完成建立您的專案，開啟*Startup.Auth.cs*檔案位於**應用程式\_啟動**資料夾。

[![](external-authentication-services/_static/image14.png "按一下以展開映像")](external-authentication-services/_static/image13.png)

當您第一次建立專案時，沒有任何外部驗證服務中啟用*Startup.Auth.cs*檔案; 以下將說明您的程式碼可能類似、 反白顯示的位置區段會啟用外部驗證服務，以及任何相關的設定，才能搭配 ASP.NET 應用程式使用 Microsoft 帳戶、 Twitter、 Facebook 或 Google 驗證：

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

當您按 F5 以建置並偵錯 web 應用程式時，它會顯示，您會看到已定義任何外部驗證服務的登入畫面。

[![](external-authentication-services/_static/image16.png "按一下以展開映像")](external-authentication-services/_static/image15.png)

在下列章節中，您將學習如何啟用每個外部驗證服務所提供的 Visual Studio 2013 中的 ASP.NET。

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>啟用 Facebook 驗證

使用 Facebook 驗證會要求您建立 Facebook 開發人員帳戶，且您的專案需要應用程式識別碼和來自 Facebook 的祕密金鑰才能運作。 如需建立 Facebook 開發人員帳戶，並取得您的應用程式識別碼和祕密金鑰資訊，請參閱[https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)。

一旦您取得您的應用程式識別碼和祕密金鑰，請使用下列步驟啟用 Facebook 驗證 web 應用程式：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image18.png "按一下以展開映像")](external-authentication-services/_static/image17.png)
2. 找出反白顯示程式碼區段：

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. 移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，然後加入 您的應用程式識別碼和祕密金鑰。 一旦您加入這些參數，您可以重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Facebook 具有已定義為外部驗證服務：

    [![](external-authentication-services/_static/image20.png "按一下以展開映像")](external-authentication-services/_static/image19.png)
5. 當您按一下**Facebook**  按鈕，您的瀏覽器會被重新導向至 Facebook 登入頁面：

    [![](external-authentication-services/_static/image22.png "按一下以展開映像")](external-authentication-services/_static/image21.png)
6. 您輸入您的 Facebook 認證，然後按一下之後**登入**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名稱**您想要關聯您Facebook 帳戶：

    [![](external-authentication-services/_static/image24.png "按一下以展開映像")](external-authentication-services/_static/image23.png)
7. 您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**Facebook 帳戶：

    [![](external-authentication-services/_static/image26.png "按一下以展開映像")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>啟用 Google 驗證

Google 是到目前為止最簡單的啟用，因為它不需要開發人員帳戶，也不會要求您應用程式識別碼或密碼金鑰為其他外部驗證服務的其他資訊的外部驗證服務也許。

若要啟用 Google 驗證 web 應用程式，請使用下列步驟：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image28.png "按一下以展開映像")](external-authentication-services/_static/image27.png)
2. 找出反白顯示程式碼區段：

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. 移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，再重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Google 具有已定義為外部驗證服務：

    [![](external-authentication-services/_static/image30.png "按一下以展開映像")](external-authentication-services/_static/image29.png)
5. 當您按一下**Google**  按鈕，您的瀏覽器會被重新導向至 Google 登入頁面：

    [![](external-authentication-services/_static/image32.png "按一下以展開映像")](external-authentication-services/_static/image31.png)
6. 您輸入您的 Google 認證，然後按一下之後**登入**，Google 將會提示您確認您的 web 應用程式具有存取您的 Google 帳戶的權限：

    [![](external-authentication-services/_static/image34.png "按一下以展開映像")](external-authentication-services/_static/image33.png)
7. 當您按一下**接受**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名**您想要與您的 Google 帳戶產生關聯：

    [![](external-authentication-services/_static/image36.png "按一下以展開映像")](external-authentication-services/_static/image35.png)
8. 您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**Google 帳戶：

    [![](external-authentication-services/_static/image38.png "按一下以展開映像")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>啟用 Microsoft 驗證

Microsoft 驗證需要您建立開發人員帳戶，而且需要用戶端識別碼和用戶端密碼才能運作。 如需建立 Microsoft 開發人員帳戶，並取得您的用戶端識別碼和用戶端密碼資訊，請參閱[https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)。

一旦您取得您的取用者索引鍵和取用者密碼，請使用下列步驟啟用 web 應用程式的 Microsoft 驗證：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image40.png "按一下以展開映像")](external-authentication-services/_static/image39.png)
2. 找出反白顯示程式碼區段：

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. 移除&quot; // &quot;反白顯示的行程式碼，請取消註解，然後加入 您的用戶端識別碼和用戶端密碼的字元。 一旦您加入這些參數，您可以重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Microsoft 具有已定義為外部驗證服務：

    [![](external-authentication-services/_static/image42.png "按一下以展開映像")](external-authentication-services/_static/image41.png)
5. 當您按一下**Microsoft**  按鈕，您的瀏覽器會被重新導向至 Microsoft 登入頁面：

    [![](external-authentication-services/_static/image44.png "按一下以展開映像")](external-authentication-services/_static/image43.png)
6. 您輸入您的 Microsoft 認證，然後按一下之後**登入**，會提示您確認您的 web 應用程式具有存取您的 Microsoft 帳戶的權限：

    [![](external-authentication-services/_static/image46.png "按一下以展開映像")](external-authentication-services/_static/image45.png)
7. 當您按一下**是**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名**您想要與您的 Microsoft 帳戶產生關聯：

    [![](external-authentication-services/_static/image48.png "按一下以展開映像")](external-authentication-services/_static/image47.png)
8. 您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**您的 Microsoft 帳戶：

    [![](external-authentication-services/_static/image50.png "按一下以展開映像")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>啟用 Twitter 驗證

Twitter 驗證需要您建立開發人員帳戶，而且需要取用者索引鍵和取用者秘密才能運作。 如需建立 Twitter 開發人員帳戶，並取得您的取用者索引鍵和取用者秘密資訊，請參閱[https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)。

一旦您取得您的取用者索引鍵和取用者密碼，請使用下列步驟啟用 Twitter 驗證 web 應用程式：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image52.png "按一下以展開映像")](external-authentication-services/_static/image51.png)
2. 找出反白顯示程式碼區段：

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. 移除&quot; // &quot;反白顯示的行程式碼，請取消註解，然後加入 您的取用者索引鍵和取用者密碼的字元。 一旦您加入這些參數，您可以重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Twitter 具有已定義為外部驗證服務：

    [![](external-authentication-services/_static/image54.png "按一下以展開映像")](external-authentication-services/_static/image53.png)
5. 當您按一下**Twitter**  按鈕，您的瀏覽器會被重新導向至 Twitter 登入頁面：

    [![](external-authentication-services/_static/image56.png "按一下以展開映像")](external-authentication-services/_static/image55.png)
6. 您輸入您的 Twitter 認證，然後按一下之後**授權應用程式**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名稱**您想要與建立關聯您的 Twitter 帳戶：

    [![](external-authentication-services/_static/image58.png "按一下以展開映像")](external-authentication-services/_static/image57.png)
7. 您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**Twitter 帳戶：

    [![](external-authentication-services/_static/image60.png "按一下以展開映像")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>其他資訊

如需有關建立使用 OAuth 和 OpenID 應用程式的詳細資訊，請參閱下列 Url:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>結合外部驗證服務

較大的彈性，您可以同時定義多個外部驗證服務-這可讓您的 web 應用程式的使用者，以使用從任何已啟用的外部驗證服務的帳戶：

[![](external-authentication-services/_static/image62.png "按一下以展開映像")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>設定 IIS Express 要使用完整網域名稱

某些外部驗證提供者不支援測試您的應用程式使用的 HTTP 地址如下`http://localhost:port/`。 若要暫時解決此問題，您可以新增靜態的完整格式網域名稱 (FQDN) 對應至主機檔案，並使用 FQDN 進行測試/偵錯 Visual Studio 2013 中設定您的專案選項。 若要這樣做，請使用下列步驟：

- 新增靜態 FQDN，對應的主機檔案：

    1. 在視窗中開啟提升權限的命令提示字元。
    2. 輸入下列命令：

        <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
    3. 將類似下列的項目加入至主機檔案：

        <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
    4. 儲存並關閉主機檔案。
- 設定 Visual Studio 專案以使用 FQDN:

    1. 在 Visual Studio 2013 中開啟您的專案時，請按一下**專案**功能表，然後選取您的專案屬性。 例如，您可能會選取**WebApplication1 屬性**。
    2. 選取**Web**  索引標籤。
    3. 輸入的 FQDN**專案 Url**。 例如，您會輸入<kbd>http://www.wingtiptoys.com</kbd>如果這是您加入至主機檔案的 FQDN 對應。
- 設定 IIS Express 應用程式使用 FQDN:

    1. 在視窗中開啟提升權限的命令提示字元。
    2. 輸入下列命令以變更您的 IIS Express 資料夾：

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. 輸入下列命令以將 FQDN 新增到您的應用程式：

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

 其中**WebApplication1**是您的專案名稱和**bindingInformation**包含您想要用於測試的通訊埠編號和 FQDN。

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>如何取得 Microsoft 驗證的應用程式設定

連結至 Windows Live Microsoft 驗證的應用程式是簡單的程序。 如果您未連結至 Windows Live 應用程式，您可以使用下列步驟：

1. 瀏覽至[https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)並輸入您的 Microsoft 帳戶名稱和密碼提示時，然後按一下**登入**:

    [![](external-authentication-services/_static/image64.png "按一下以展開映像")](external-authentication-services/_static/image63.png)
2. 輸入的名稱和出現提示時，您的應用程式的語言，然後按一下**我接受**:

    [![](external-authentication-services/_static/image66.png "按一下以展開映像")](external-authentication-services/_static/image65.png)
3. 上**API 設定**應用程式頁面上，輸入您的應用程式和複製的重新導向網域**用戶端識別碼**和**用戶端密碼**專案，然後按一下按一下**儲存**:

    [![](external-authentication-services/_static/image68.png "按一下以展開映像")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>選擇性： 停用本機註冊

目前 ASP.NET 本機註冊功能不會防止自動的程式 (bot) 建立成員的帳戶。例如，藉由使用 bot 防止和驗證技術，例如[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)。 因為這個緣故，您應該移除登入頁面上的本機登入表單和註冊連結。 若要這樣做，請開啟 *\_Login.cshtml*在專案中，頁面上，然後再標記為註解的線本機登入面板和註冊連結。 結果頁面應看起來像下列程式碼範例：

[!code-html[Main](external-authentication-services/samples/sample10.html)]

本機登入面板和註冊連結已停用，一旦您登入頁面只會顯示您已啟用外部驗證提供者：

[![](external-authentication-services/_static/image70.png "按一下以展開映像")](external-authentication-services/_static/image69.png)
