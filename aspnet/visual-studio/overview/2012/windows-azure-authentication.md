---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure 驗證 |Microsoft Docs
author: Rick-Anderson
description: Windows Azure Active Directory 的 Microsoft ASP.NET 工具可以輕鬆啟用 Windows Azure 網站上所裝載的 web 應用程式的驗證...
ms.author: aspnetcontent
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: e3443fb627dc8d7d5011341828556b4c13836170
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812806"
---
<a name="windows-azure-authentication"></a>Windows Azure 驗證
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET 工具針對 Windows Azure Active Directory 輕鬆地啟用上裝載的 web 應用程式的驗證[Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/)。 您可以使用 Windows Azure 驗證來驗證您的組織，從您內部部署 Active Directory 同步處理的公司帳戶或建立您自己自訂的 Windows Azure Active Directory 網域中的使用者從 Office 365 使用者。 啟用 Windows Azure 驗證設定您的應用程式，來驗證使用者使用單一[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)租用戶。
> 
> ASP.NET 的 Windows Azure 驗證工具不支援雲端服務中的 web 角色，但我們計劃在未來的版本中這麼做。 [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) 支援在 Windows Azure web 角色中。
> 
> 如需如何設定您的內部部署 Active Directory 與 Windows Azure Active Directory 租用戶之間的同步處理的詳細資訊，請參閱[來實作和管理使用 AD FS 2.0 單一登入](https://technet.microsoft.com/library/jj205462.aspx)。
> 
> Windows Azure Active Directory 是目前可供[免費預覽服務](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。


## <a name="requirements"></a>需求：

- Visual Studio 2012 或[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web 工具擴充功能適用於 Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)或[Web 工具延伸模組，適用於 Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306)或[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>使用 Visual Studio 2012 中建立 ASP.NET Web 應用程式

您可以使用 Visual Studio 2012 中建立任何 Web 應用程式，本教學課程使用 ASP.NET MVC 內部網路範本。

1. 建立新 ASP.NET MVC 4 內部網路應用程式，並接受所有預設值。 (它必須是 In **tra** net 和 not In **ter** net 專案)。  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Window Azure 驗證 （當啟用您的租用戶的全域系統管理員）

如果您沒有現有的 Windows Azure Active Directory 租用戶 （例如，透過現有的 Office 365 帳戶） 您可以建立新的租用戶註冊[新的 Windows Azure Active Directory 帳戶](http://g.microsoftonline.com/0AX00en/5)。

1. 從 [專案] 功能表中選取**啟用 Windows Azure 驗證**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. 輸入您的 Windows Azure Active Directory 租用戶 (例如，contoso.onmicrosoft.com) 的網域，然後按一下**啟用**:

![](windows-azure-authentication/_static/image3.png)

3. 在 Web 驗證對話方塊登入 Windows Azure Active Directory 租用戶系統管理員身分：  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>非系統管理員的原則來啟用 Windows Azure

如果您沒有 Windows Azure Active Directory 租用戶的全域系統管理員權限，您可以取消選取佈建應用程式的核取方塊。

![](windows-azure-authentication/_static/image6.png)

對話方塊會顯示**網域**，**應用程式主體識別碼**並**回覆 URL**所需的佈建與 Azure Active Directory 應用程式租用戶。 您必須擁有足夠的權限，來佈建應用程式的任何人提供這項資訊。 請參閱[如何實作單一登入 Windows Azure Active Directory-ASP.NET 應用程式與](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)如需有關如何使用 cmdlet 來手動建立服務主體。  
一旦應用程式已成功佈建，您可以按一下**持續與選取的設定更新 web.config**。 如果您想要繼續開發應用程式，同時等候發生，您可以按一下 [佈建**關閉]，請記住專案檔案中的設定**。 在下一次叫用 啟用 Windows Azure 驗證，並取消佈建的核取方塊，您會看到相同的設定，您可以按一下**繼續**，然後按一下 **套用這些設定，在 web.config**.

1. 稍候，正在設定 Windows Azure 驗證和使用 Windows Azure Active Directory 佈建應用程式。
2. Windows Azure 驗證啟用您的應用程式之後，按一下 **關閉：** 

    ![](windows-azure-authentication/_static/image7.png)
3. 按 f5 鍵執行應用程式。 您應該會自動取得重新導向至登入頁面。 您可以使用目錄租用戶的使用者認證來登入應用程式...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. 因為您的應用程式目前使用的自我簽署的測試憑證，所以您會收到警告之瀏覽器的不受信任的憑證授權單位所發出的憑證。

    這項警告可以放心忽略在本機開發期間按一下**繼續瀏覽此網站：** 

    ![](windows-azure-authentication/_static/image8.png)
5. 現在已成功登入使用 Windows Azure 驗證您的應用程式 ！

    ![](windows-azure-authentication/_static/image2.jpg)

啟用 Windows Azure 驗證可以進行下列變更您的應用程式：

- 反跨網站偽造要求 ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) 類別 (*應用程式\_Start\AntiXsrfConfig.cs* ) 加入至專案。
- NuGet 套件`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`加入至專案。
- 在您的應用程式中的 Windows Identity Foundation 設定會設定為接受來自 Windows Azure Active Directory 租用戶的安全性權杖。 按一下來查看所做的變更的展開的檢視影像*Web.config*檔案。  
  
     ![](windows-azure-authentication/_static/image9.png)
- 將佈建服務主體應用程式在 Windows Azure Active Directory 租用戶中。
- 會啟用 HTTPS。

## <a name="deploy-the-application-to-windows-azure"></a>部署至 Windows Azure 應用程式

如需完整指示，請參閱 < [ASP.NET Web 應用程式至 Windows Azure 網站部署](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。

若要發佈的應用程式使用 Windows Azure 驗證至 Azure 網站：

1. 以滑鼠右鍵按一下您的應用程式，然後選取**發行：** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. 從 [發佈 Web] 對話方塊下載並匯入發行設定檔的 Azure 網站。

    ![](windows-azure-authentication/_static/image4.jpg)
3. **連接**索引標籤會顯示**目的地 URL** (公用面向應用程式 URL)。 按一下 **驗證連線**來測試您的連線：

    ![](windows-azure-authentication/_static/image5.jpg)
4. 如果您已經發行到此 Azure 網站之前，請考慮檢查**移除目的地上的其他檔案**設定，以確保您的應用程式完全發行。 請注意**啟用 Windows Azure 驗證**就 slected 核取方塊。  

    ![](windows-azure-authentication/_static/image10.png)
5. 選擇性： 在**Preview**索引標籤處按一下**開始預覽**查看部署的檔案。

    ![](windows-azure-authentication/_static/image6.jpg)
6. 按一下 **發行。**

    系統會提示您啟用 Windows Azure 驗證目標主機。 按一下 **啟用**繼續：

    ![](windows-azure-authentication/_static/image11.png)
7. 輸入您的 Windows Azure Active Directory 租用戶系統管理員認證：

    ![](windows-azure-authentication/_static/image7.jpg)
8. 一旦成功發行您的應用程式，瀏覽器會開啟至已發行的網站。

    > [!NOTE]
    > 可能需要最多五分鐘的時間 （通常必須更少） 完整佈建 Windows Azure Active Directory 與目標主機啟用 Windows Azure 驗證後的應用程式。 當您第一次執行應用程式如果您收到錯誤 ACS50001： 找不到名稱 '[領域]' 的信賴憑證者合作對象，然後等候幾分鐘的時間，並嘗試再次執行應用程式。
9. 出現提示時，您的目錄中的使用者身分登入：

    ![](windows-azure-authentication/_static/image8.jpg)
10. 您現在已成功登入您的 Azure 裝載使用 Windows Azure 驗證應用程式。  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>已知問題

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>以角色為基礎的授權失敗時使用 Windows Azure 驗證 < o: p>< >< / o: p>< >

Windows Azure Authentication 目前未提供必要的角色宣告，因此可以執行以角色為基礎的授權。 已驗證的使用者角色必須以手動方式從 Windows Azure Active Directory。 < o: p>< >< 擷取 / o: p>< >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>瀏覽至應用程式與 Windows Azure 驗證錯誤會產生 「 ACS20016 登入的使用者 (live.com) 的網域不符合任何允許網域這個 STS 」 < o: p>< >< / o: p>< >

如果您已經登入 Microsoft 帳戶 （例如 hotmail.com、 live.com、 outlook.com），而且您嘗試存取的應用程式已啟用 Windows Azure 驗證，您就可能取得 400 錯誤回應，因為您的 Microsoft 帳戶的網域無法辨識 Windows Azure Active Directory。 若要登入的應用程式，登出您的 Microsoft 帳戶第一次。 < o: p>< >< / o: p>< >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>以外的其他登入啟用的 Windows Azure 驗證應用程式和 X509CertificateValidationMode 無導致憑證驗證錯誤 accounts.accesscontrol.windows.net 憑證 < o: p>< >< / o: p>< >

則不需要憑證驗證，且應該維持在停用。 簽發者憑證的指紋會進行驗證。 WSFederationAuthenticationModule < o: p>< >< / o: p>< >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>當您嘗試啟用 Windows Azure 驗證時 [Web 驗證] 對話方塊會顯示錯誤 「 ACS20016： 登入的使用者 (contoso.onmicrosoft.com) 的網域不符合此 STS 的任何允許的網域。 」< o: p>< >< / o: p>< >

當您先前已成功登入使用其他 Windows Azure Active Directory 帳戶，從相同的 Visual Studio 程序中，您會看到此錯誤。 從指定的帳戶登出或重新啟動 Visual Studio。 如果您先前已登入，並選取 「 讓我保持登中 」 的選項則您可能需要清除瀏覽器 cookie。 < o: p>< >< / o: p>< >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012： 要求不是有效的 WS-同盟通訊協定訊息 < o: p>< >< / o: p>< >

如果您已經登入一些其他 Microsoft ID 以其中一項 Azure 服務，會發生這項目。 使用私用瀏覽器視窗中，例如在 IE 中的 InPrivate 或 chrome Incognito 或清除所有 cookie。 < o: p>< >< / o: p>< >

## <a name="additional-resources"></a>其他資源

- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) -Vittorio Bertocci
- [Windows Azure 功能： 身分識別](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory： 為您的組織開發的應用程式](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory： 開發應用程式的多個組織](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [如何實作單一登入 Windows Azure Active directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [單一登入 windows Azure Active Directory： 深入了解](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx)-Vittorio Bertocci
- [使用 AD FS 2.0 實作和管理單一登入](https://technet.microsoft.com/library/jj205462.aspx)
