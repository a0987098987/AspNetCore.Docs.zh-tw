---
uid: web-api/overview/security/external-authentication-services
title: 外部驗證服務與 ASP.NET Web API (C#) |Microsoft Docs
author: rmcmurray
description: 描述如何使用 ASP.NET Web API 中的外部驗證服務。
ms.author: aspnetcontent
ms.date: 06/26/2013
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: fe821384a195e69c102aad95e534d543de705a00
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822783"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>外部驗證服務與 ASP.NET Web API (C#)
====================
藉由[Robert McMurray](https://github.com/rmcmurray)

展開 visual Studio 2013 和 ASP.NET 4.5.1 的安全性選項[單一頁面應用程式](../../../single-page-application/index.md)(SPA) 及[Web API](../../index.md)服務外部驗證服務，其中包含數個整合OAuth/OpenID 和社交媒體驗證服務： Microsoft 帳戶、 Twitter、 Facebook 和 Google。

### <a name="in-this-walkthrough"></a>在本逐步解說

- [使用外部驗證服務](#USING)
- [建立範例 Web 應用程式](#SAMPLE)
- [啟用 Facebook 驗證](#FACEBOOK)
- [啟用 Google 驗證](#GOOGLE)
- [啟用 Microsoft 驗證](#MICROSOFT)
- [啟用 Twitter 驗證](#TWITTER)
- [其他資訊](#MOREINFO)

    - [結合外部驗證服務](#COMBINE)
    - [設定 IIS Express 要使用完整網域名稱](#FQDN)
    - [濆爧髍孮 Microsoft 驗證您的應用程式設定](#OBTAIN)
    - [選擇性︰ 停用本機註冊](#DISABLE)

### <a name="prerequisites"></a>必要條件

若要依照本逐步解說中的範例，您需要具備下列項目：

- Visual Studio 2013
- 至少一個下列的外部驗證服務帳戶：

    - Google 使用者帳戶
    - 開發人員使用的應用程式識別碼和祕密金鑰的其中一個下列的社交媒體驗證服務帳戶：

        - Microsoft 帳戶 ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>使用外部驗證服務

目前可供 web 開發人員說明，以降低開發的外部驗證服務的眾多時建立新的 web 應用程式的時間。 Web 使用者通常有數個現有的帳戶，以受歡迎的 web 服務和社交媒體網站，因此當 web 應用程式實作，從外部 web 服務或社交媒體網站的驗證服務，它會將儲存的開發時間，將已使建立驗證實作。 使用外部驗證服務可以節省使用者毋需建立 web 應用程式的另一個帳戶，也不需要記住另一個使用者名稱和密碼。

在過去，開發人員在過去兩個選擇： 建立自己的驗證實作，或了解如何整合到其應用程式的外部驗證服務。 在最基本的層級下, 圖說明簡單的要求流程的使用者代理程式 （web 瀏覽器） 從 web 應用程式設定為使用外部驗證服務要求資訊：

[![](external-authentication-services/_static/image2.png "按一下以展開映像")](external-authentication-services/_static/image1.png)

在上圖中，使用者代理程式 （或在此範例中的網頁瀏覽器） 會提出要求的 web 瀏覽器重新導向至外部驗證服務與 web 應用程式。 使用者代理程式會將其憑證傳送到外部驗證服務，並在服務和使用者代理程式已成功驗證，如果外部驗證服務將使用者代理程式重新導向至原始的 web 應用程式，使用某種形式的語彙基元的使用者代理程式會傳送給 web 應用程式。 Web 應用程式會使用權杖，以確認使用者代理程式已成功驗證外部驗證服務，且 web 應用程式可能會使用權杖來收集使用者代理程式的詳細資訊。 完成應用程式之後處理的使用者代理程式的資訊，web 應用程式會傳回適當的回應取決於其授權設定的使用者代理程式。

在第二個範例中，與 web 應用程式和外部的授權伺服器交涉，使用者代理程式，並在 web 應用程式執行與外部的授權伺服器，以擷取使用者的其他資訊的其他通訊代理程式：

[![](external-authentication-services/_static/image4.png "按一下以展開映像")](external-authentication-services/_static/image3.png)

Visual Studio 2013 和 ASP.NET 4.5.1 與外部驗證服務的整合更輕鬆地進行開發人員藉由提供內建的整合，如以下驗證服務：

- Facebook
- Google
- Microsoft 帳戶 （Windows Live ID 帳戶）
- Twitter

在本逐步解說的範例將示範如何使用 Visual Studio 2013 中使用隨附的新 ASP.NET Web 應用程式範本就可以將它設定每個支援的外部驗證服務。

> [!NOTE]
> 如有必要，您可能需要將您的 FQDN 新增到您的外部驗證服務的設定。 這項需求根據安全性限制，某些外部驗證服務，而這需要您的應用程式設定中的 FQDN，以符合您的用戶端會使用的 FQDN。 （這個步驟很大的每個外部驗證服務，您必須參考文件，每個外部驗證服務，以查看這是否必要，以及如何設定這些設定）。如果您要設定 IIS Express 來測試此環境中使用 FQDN，請參閱[若要使用完整網域名稱設定 IIS Express](#FQDN)稍後在本逐步解說的區段。


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>建立範例 Web 應用程式

下列步驟將引導您完成使用 ASP.NET Web 應用程式範本，建立範例應用程式，您將使用此範例應用程式的每個外部驗證服務，稍後在本逐步解說。

啟動 Visual Studio 2013 選取**新的專案**從 [開始] 頁面。 或從**檔案**功能表上，選取**新增**，然後**專案**。

[![](external-authentication-services/_static/image6.png "按一下以展開映像")](external-authentication-services/_static/image5.png)

當**新的專案**對話方塊隨即出現，請選取**已安裝****範本**展開**Visual C#**。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET Web 應用程式**。 輸入您專案的名稱，然後按一下**確定**。

[![](external-authentication-services/_static/image8.png "按一下以展開映像")](external-authentication-services/_static/image7.png)

當**新的 ASP.NET 專案**顯示，請選取**SPA**範本，然後按一下**建立的專案**。

[![](external-authentication-services/_static/image10.png "按一下以展開映像")](external-authentication-services/_static/image9.png)

Visual studio 2013 的等候建立您的專案。

[![](external-authentication-services/_static/image12.png "按一下以展開映像")](external-authentication-services/_static/image11.png)

當 Visual Studio 2013 已完成建立您的專案時，開啟*Startup.Auth.cs*檔案，它位於**應用程式\_啟動**資料夾。

[![](external-authentication-services/_static/image14.png "按一下以展開映像")](external-authentication-services/_static/image13.png)

當您第一次建立專案時，沒有任何外部驗證服務中啟用*Startup.Auth.cs*檔案; 下圖說明您的程式碼可能類似，反白顯示位置區段會啟用外部驗證服務和任何相關的設定，才能使用您的 ASP.NET 應用程式中的 Microsoft 帳戶、 Twitter、 Facebook 或 Google 驗證：

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

當您按 F5 以建置和偵錯您的 web 應用程式時，它會顯示登入畫面，您會看到尚未定義任何外部驗證服務。

[![](external-authentication-services/_static/image16.png "按一下以展開映像")](external-authentication-services/_static/image15.png)

在下列章節中，您將學習如何啟用每個外部驗證服務所提供的 Visual Studio 2013 中的 ASP.NET。

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>啟用 Facebook 驗證

使用 Facebook 驗證會要求您建立 Facebook 開發人員帳戶，而您的專案需要的應用程式識別碼和來自 Facebook 的祕密金鑰才能運作。 建立 Facebook 開發人員帳戶，並取得您的應用程式識別碼和祕密金鑰的相關資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166)。

一旦您取得您的應用程式識別碼和祕密金鑰，請使用下列步驟來啟用 web 應用程式的 Facebook 驗證：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image18.png "按一下以展開映像")](external-authentication-services/_static/image17.png)
2. 找出程式碼的反白顯示的區段：

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. 移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，然後新增 您的應用程式識別碼和祕密金鑰。 一旦您加入這些參數，您可以重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Facebook，已定義為外部驗證服務：

    [![](external-authentication-services/_static/image20.png "按一下以展開映像")](external-authentication-services/_static/image19.png)
5. 當您按一下 [ **Facebook** ] 按鈕，您的瀏覽器會被重新導向至 Facebook 登入頁面：

    [![](external-authentication-services/_static/image22.png "按一下以展開映像")](external-authentication-services/_static/image21.png)
6. 您輸入您的 Facebook 認證，然後按一下 後**登入**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要關聯您Facebook 帳戶：

    [![](external-authentication-services/_static/image24.png "按一下以展開映像")](external-authentication-services/_static/image23.png)
7. 在您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**為您的 Facebook 帳戶：

    [![](external-authentication-services/_static/image26.png "按一下以展開映像")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>啟用 Google 驗證

Google 是到目前為止最簡單的方法來啟用，因為它不需要開發人員帳戶，也不會要求您的應用程式識別碼或祕密金鑰等其他的外部驗證服務的其他資訊的外部驗證服務需要此項目。

若要啟用 Google 驗證 web 應用程式，請使用下列步驟：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image28.png "按一下以展開映像")](external-authentication-services/_static/image27.png)
2. 找出程式碼的反白顯示的區段：

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. 移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，再重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到，Google 已定義為外部驗證服務：

    [![](external-authentication-services/_static/image30.png "按一下以展開映像")](external-authentication-services/_static/image29.png)
5. 當您按一下 [ **Google** ] 按鈕，您的瀏覽器會被重新導向至 Google 登入頁面：

    [![](external-authentication-services/_static/image32.png "按一下以展開映像")](external-authentication-services/_static/image31.png)
6. 您輸入您的 Google 認證，然後按一下 後**登入**，Google 將會提示您確認您的 web 應用程式具有存取您的 Google 帳戶的權限：

    [![](external-authentication-services/_static/image34.png "按一下以展開映像")](external-authentication-services/_static/image33.png)
7. 當您按一下  **Accept**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要將與您的 Google 帳戶產生關聯：

    [![](external-authentication-services/_static/image36.png "按一下以展開映像")](external-authentication-services/_static/image35.png)
8. 在您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**為您的 Google 帳戶：

    [![](external-authentication-services/_static/image38.png "按一下以展開映像")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>啟用 Microsoft 驗證

Microsoft 驗證會要求您建立開發人員帳戶，以及它需要用戶端識別碼和用戶端祕密，才能運作。 如需建立 Microsoft 開發人員帳戶，並取得您的用戶端識別碼和用戶端祕密資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070)。

一旦您取得您的取用者金鑰和取用者祕密，請使用下列步驟來啟用 web 應用程式的 Microsoft 驗證：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image40.png "按一下以展開映像")](external-authentication-services/_static/image39.png)
2. 找出程式碼的反白顯示的區段：

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. 移除&quot; // &quot;取消註解的程式碼中，反白顯示的行，然後新增 您的用戶端識別碼和用戶端祕密的字元。 一旦您加入這些參數，您可以重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到，Microsoft 已定義為外部驗證服務：

    [![](external-authentication-services/_static/image42.png "按一下以展開映像")](external-authentication-services/_static/image41.png)
5. 當您按一下 [ **Microsoft** ] 按鈕，您的瀏覽器會被重新導向至 Microsoft 登入頁面：

    [![](external-authentication-services/_static/image44.png "按一下以展開映像")](external-authentication-services/_static/image43.png)
6. 您輸入您的 Microsoft 認證，然後按一下 後**登入**，系統會提示您確認您的 web 應用程式具有存取您的 Microsoft 帳戶的權限：

    [![](external-authentication-services/_static/image46.png "按一下以展開映像")](external-authentication-services/_static/image45.png)
7. 當您按一下  **是**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要將與您的 Microsoft 帳戶產生關聯：

    [![](external-authentication-services/_static/image48.png "按一下以展開映像")](external-authentication-services/_static/image47.png)
8. 在您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**您的 Microsoft 帳戶：

    [![](external-authentication-services/_static/image50.png "按一下以展開映像")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>啟用 Twitter 驗證

Twitter 驗證會要求您建立開發人員帳戶，以及它需要取用者金鑰和取用者祕密，才能運作。 建立 Twitter 開發人員帳戶，並取得您的取用者金鑰和取用者祕密的相關資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166)。

一旦您取得您的取用者金鑰和取用者祕密，請使用下列步驟啟用 Twitter 驗證 web 應用程式：

1. 在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：

    [![](external-authentication-services/_static/image52.png "按一下以展開映像")](external-authentication-services/_static/image51.png)
2. 找出程式碼的反白顯示的區段：

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. 移除&quot; // &quot;取消註解的程式碼中，反白顯示的行，然後新增 您的取用者金鑰和取用者密碼的字元。 一旦您加入這些參數，您可以重新編譯您的專案：

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. 當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Twitter，已定義為外部驗證服務：

    [![](external-authentication-services/_static/image54.png "按一下以展開映像")](external-authentication-services/_static/image53.png)
5. 當您按一下 [ **Twitter** ] 按鈕，您的瀏覽器會被重新導向至 Twitter 登入頁面：

    [![](external-authentication-services/_static/image56.png "按一下以展開映像")](external-authentication-services/_static/image55.png)
6. 您輸入您的 Twitter 認證，然後按一下 後**授權應用程式**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要與產生關聯您的 Twitter 帳戶：

    [![](external-authentication-services/_static/image58.png "按一下以展開映像")](external-authentication-services/_static/image57.png)
7. 您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**為您的 Twitter 帳戶：

    [![](external-authentication-services/_static/image60.png "按一下以展開映像")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>其他資訊

如需建立使用 OAuth 和 OpenID 應用程式的詳細資訊，請參閱下列 Url:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>結合外部驗證服務

更大的彈性，您可以定義多個外部驗證服務在相同的時間-這可讓您的 web 應用程式的使用者使用任何已啟用的外部驗證服務的帳戶：

[![](external-authentication-services/_static/image62.png "按一下以展開映像")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>設定 IIS Express 要使用完整網域名稱

某些外部驗證提供者不支援測試您的應用程式使用 HTTP 位址，例如`http://localhost:port/`。 若要暫時解決此問題，您可以新增靜態的完整格式網域名稱 (FQDN) 對應至主機檔案，並使用 FQDN 進行測試/偵錯的 Visual Studio 2013 中設定您的專案選項。 若要這樣做，請使用下列步驟：

- 新增靜態 FQDN 對應您的主機檔案：

  1. 在 Windows 中，開啟提升權限的命令提示字元。
  2. 輸入下列命令：

      <kbd>在 [記事本] %WinDir%\system32\drivers\etc\hosts</kbd>
  3. 主機檔案中加入的項目，如下所示：

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. 儲存並關閉您的主機檔案。

- 設定 Visual Studio 專案使用的 FQDN:

  1. 在 Visual Studio 2013 中開啟您的專案時，請按一下**專案**功能表，然後選取 專案屬性。 例如，您可以選取**WebApplication1 屬性**。
  2. 選取 [ **Web** ] 索引標籤。
  3. 輸入的 FQDN<strong>專案 Url</strong>。 例如，您會輸入<kbd> <http://www.wingtiptoys.com> </kbd>如果這是您新增至主機檔案的 FQDN 對應。

- 設定 IIS Express 以使用您的應用程式的 FQDN:

    1. 在 Windows 中，開啟提升權限的命令提示字元。
    2. 輸入下列命令，以將變更為您的 IIS Express 資料夾：

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. 輸入下列命令以將 FQDN 新增到您的應用程式：

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  何處**WebApplication1**是您的專案名稱並**bindingInformation**包含您想要用於測試的連接埠號碼和 FQDN。

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>濆爧髍孮 Microsoft 驗證您的應用程式設定

連結至 Windows Live，Microsoft 驗證應用程式是簡單的程序。 如果您未連結至 Windows Live 應用程式，您可以使用下列步驟：

1. 瀏覽至[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070)並輸入您的 Microsoft 帳戶名稱和密碼提示時，然後按一下**登入**:

    [![](external-authentication-services/_static/image64.png "按一下以展開映像")](external-authentication-services/_static/image63.png)
2. 輸入的名稱和出現提示時，您的應用程式的語言，然後按一下**我接受**:

    [![](external-authentication-services/_static/image66.png "按一下以展開映像")](external-authentication-services/_static/image65.png)
3. 上**API 設定**應用程式頁面上，輸入您的應用程式和複製的 重新導向網域**用戶端識別碼**並**用戶端祕密**為您的專案，然後按一下 **儲存**:

    [![](external-authentication-services/_static/image68.png "按一下以展開映像")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>選擇性︰ 停用本機註冊

目前的 ASP.NET 本機註冊功能不會防止自動的程式 (bot) 帳戶; 建立成員例如，藉由使用一種 bot 防護及驗證的技術，像是[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)。 基於這個原因，您應該移除的登入頁面上的本機登入表單和註冊連結。 若要這樣做，請開啟 *\_Login.cshtml*在專案中，頁面上，然後再標記為註解的線條，「 本機登入面板和 [註冊] 連結。 產生的頁面看起來應該像下列程式碼範例：

[!code-html[Main](external-authentication-services/samples/sample10.html)]

本機登入面板和 [註冊] 連結已停用，一旦您登入頁面只會顯示您已啟用外部驗證提供者：

[![](external-authentication-services/_static/image70.png "按一下以展開映像")](external-authentication-services/_static/image69.png)
