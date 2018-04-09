---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure Authentication |Microsoft 文件
author: Rick-Anderson
description: Windows Azure Active Directory 的 Microsoft ASP.NET 工具可簡化針對裝載 Windows Azure 網站上的 web 應用程式啟用驗證...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 09cb37ceb0132958a48f5f3a5d52dc46c6f0a78d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="windows-azure-authentication"></a>Windows Azure 驗證
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET 工具的 Windows Azure Active Directory 可簡化針對上主控的 web 應用程式啟用驗證[Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/)。 您可以使用 Windows Azure 驗證來驗證 Office 365 使用者，從您的組織，從您在內部部署 Active Directory 同步處理的公司帳戶或建立您自己自訂的 Windows Azure Active Directory 網域中的使用者。 啟用 Windows Azure 驗證設定您的應用程式驗證使用者使用單一[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)租用戶。
> 
> ASP.NET Windows Azure 驗證工具不支援的雲端服務中的 web 角色，但我們計劃在未來的版本中這麼做。 [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) 適用於 Windows Azure web 角色。
> 
> 如需有關如何設定您在內部部署 Active Directory 與 Windows Azure Active Directory 租用戶之間同步處理的詳細資訊，請參閱[實作和管理使用 AD FS 2.0 單一登入](https://technet.microsoft.com/library/jj205462.aspx)。
> 
> Windows Azure Active Directory 是目前可供[免費預覽服務](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。


## <a name="requirements"></a>需求：

- Visual Studio 2012 or [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web Tools Extensions for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)或[Web Tools Extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306)或[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>使用 Visual Studio 2012 中建立 ASP.NET Web 應用程式

您可以使用 Visual Studio 2012 中建立任何 Web 應用程式，本教學課程使用 ASP.NET MVC 內部網路範本。

1. 建立新 ASP.NET MVC 4 內部網路應用程式，並接受所有預設值。 (它必須是 In**周遊**網路而不在**輸入**net 專案)。  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>啟用 Window Azure 驗證 （當您的租用戶的全域系統管理員）

如果您沒有現有的 Windows Azure Active Directory 租用戶 （例如，透過現有的 Office 365 帳戶） 您可以建立新的租用戶註冊[新的 Windows Azure Active Directory 帳戶](http://g.microsoftonline.com/0AX00en/5)。

1. 從 [專案] 功能表選取**啟用 Windows Azure Authentication**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Windows Azure Active Directory 租用戶 (例如 contoso.onmicrosoft.com) 輸入的網域，然後按一下**啟用**:

![](windows-azure-authentication/_static/image3.png)

3. 在 Web 驗證對話方塊登入 Windows Azure Active Directory 租用戶系統管理員身分：  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>非系統管理員的租用戶啟用 Windows Azure

如果您沒有 Windows Azure Active Directory 租用戶全域管理員權限，您可以取消勾選的核取方塊佈建應用程式。

![](windows-azure-authentication/_static/image6.png)

對話方塊會顯示**網域**，**應用程式主體識別碼**和**回覆 URL**所需的佈建與 Azure Active Directory 應用程式租用戶。 您需要將此資訊提供給擁有足夠的權限，來佈建應用程式的任何人。 請參閱[如何實作單一登入 Windows Azure Active Directory-ASP.NET 應用程式與](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)如需有關如何使用指令程式來手動建立服務主體的詳細資訊。  
一旦應用程式已成功佈建，您可以按一下**繼續與所選擇的設定更新 web.config**。 如果您想要繼續開發應用程式，等候要發生的情況，您可以按一下 [佈建**關閉]，請記住專案檔中的設定**。 下次您叫用 啟用 Windows Azure 驗證，並取消佈建的核取方塊，您會看到相同的設定，然後您可以按一下**繼續**，然後按 **套用這些設定，在 web.config 中**.

1. 您的應用程式設定成使用 Windows Azure 驗證及使用 Windows Azure Active Directory 佈建稍候。
2. 一旦您的應用程式已啟用 Windows Azure 驗證，按一下 **關閉：** 

    ![](windows-azure-authentication/_static/image7.png)
3. 按 F5 執行您的應用程式。 您應該會自動取得重新導向至登入頁面。 您可以使用目錄租用戶的使用者認證來登入應用程式...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. 您的應用程式目前使用的自我簽署的測試憑證，因為您會收到警告從憑證不受信任的憑證授權單位所發出的瀏覽器。

    這個警告可以放心忽略在本機開發期間按一下**繼續瀏覽此網站：** 

    ![](windows-azure-authentication/_static/image8.png)
5. 您現在已成功登入使用 Windows Azure Authentication 應用程式 ！

    ![](windows-azure-authentication/_static/image2.jpg)

驗證啟用 Windows Azure 應用程式進行下列變更：

- 反跨網站要求偽造 ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) 類別 (*應用程式\_Start\AntiXsrfConfig.cs* ) 加入至專案。
- NuGet 封裝`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`加入至專案。
- 設定 Windows Identity Foundation 應用程式中的將設定為接受來自您的 Windows Azure Active Directory 租用戶的安全性權杖。 按一下以查看變更的展開的檢視下面的影像*Web.config*檔案。  
  
     ![](windows-azure-authentication/_static/image9.png)
- 將佈建服務主體中您的 Windows Azure Active Directory 租用戶應用程式。
- 啟用 HTTPS。

## <a name="deploy-the-application-to-windows-azure"></a>部署至 Windows Azure 應用程式

如需完整指示，請參閱[ASP.NET Web 應用程式至 Windows Azure 網站部署](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。

若要發行使用 Windows Azure 驗證至 Azure 網站的應用程式：

1. 您的應用程式上按一下滑鼠右鍵，然後選取**發行：** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. 從 [發行 Web] 對話方塊下載並匯入發行設定檔的 Azure 網站。

    ![](windows-azure-authentication/_static/image4.jpg)
3. **連接**索引標籤上顯示**目的地 URL** （公用對您應用程式的 URL)。 按一下**驗證連線**來測試您的連線：

    ![](windows-azure-authentication/_static/image5.jpg)
4. 如果您已經發行這個 Azure Web 站台之前，請考慮檢查**移除目的端的其他檔案**完全發行設定，以確保您的應用程式。 請注意**啟用 Windows Azure Authentication**就 slected 核取方塊。  

    ![](windows-azure-authentication/_static/image10.png)
5. 選擇性： 在**預覽**索引標籤處按一下**啟動預覽**來查看部署的檔案。

    ![](windows-azure-authentication/_static/image6.jpg)
6. 按一下**發行。**

    系統會提示您啟用 Windows Azure 驗證目標主機。 按一下**啟用**繼續：

    ![](windows-azure-authentication/_static/image11.png)
7. 輸入您的 Windows Azure Active Directory 租用戶系統管理員認證：

    ![](windows-azure-authentication/_static/image7.jpg)
8. 一旦已成功發行您的應用程式，瀏覽器會開啟至已發行的網站。

    > [!NOTE]
    > 可能需要最多五分鐘的時間 （通常是必須更少） 完全佈建的 Windows Azure Active Directory 後啟用 Windows Azure 驗證目標主機的應用程式。 當您第一次執行應用程式如果您收到錯誤 ACS50001： 找不到信賴憑證者的合作對象名稱 [領域]，然後稍候幾分鐘，再嘗試執行的應用程式。
9. 出現提示時，您的目錄中的使用者身分登入：

    ![](windows-azure-authentication/_static/image8.jpg)
10. 您現在已成功登入您的 Azure 裝載使用 Windows Azure Authentication 應用程式。  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>已知問題

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>以角色為基礎的授權失敗時使用 Windows Azure Authentication < o: p>< >< / o: p>< >

Windows Azure 驗證目前未提供必要的角色宣告，因此可以執行以角色為基礎的授權。 已驗證使用者的角色必須從 Windows Azure Active Directory。 < o: p>< >< 手動擷取 / o: p>< >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>瀏覽至 Windows Azure 驗證會產生錯誤的應用程式 [ACS20016 登入的使用者 (live.com) 的網域不符合任何允許網域的 STS] < o: p>< >< / o: p>< >

如果您已經登入 Microsoft 帳戶 （例如 hotmail.com、 live.com、 outlook.com），而且您嘗試存取已啟用 Windows Azure Authentication 應用程式可能會出現 400 錯誤回應因為您的 Microsoft 帳戶的網域無法辨識由 Windows Azure Active Directory。 若要記錄至應用程式時，登出您的 Microsoft 帳戶第一次。 < o: p>< >< / o: p>< >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>無結果中 < o: p>< >< accounts.accesscontrol.windows.net 憑證的憑證驗證錯誤以外登入啟用的 Windows Azure 驗證的應用程式和 X509CertificateValidationMode / o: p>< >

不需要憑證驗證，而且應該會保持停用。 簽發者憑證的指紋驗證 WSFederationAuthenticationModule。 < o: p>< >< / o: p>< >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>當您嘗試啟用 Windows Azure 驗證時 Web 驗證對話方塊中會顯示錯誤 「 ACS20016： 登入的使用者 (contoso.onmicrosoft.com) 的網域不符合此 STS 的任何允許的網域。 」< o: p>< >< / o: p>< >

您先前已成功登入使用其他 Windows Azure Active Directory 帳戶，從相同的 Visual Studio 處理序內時，您可能會看到這個錯誤。 從指定的帳戶登出或重新啟動 Visual Studio。 如果您先前已登入並選取 「 讓我保持登中 」 的選項則您可能必須清除瀏覽器 cookie。 < o: p>< >< / o: p>< >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012： 要求不是有效的 WS-同盟通訊協定訊息 < o: p>< >< / o: p>< >

如果您已登入其中一項 Azure 服務的一些其他 Microsoft ID，這可能會發生。 使用私用瀏覽器視窗，例如在 IE 中的 InPrivate 或 Incognito 在 Chrome 中的，或清除所有 cookie。 <o:p></o:p>

## <a name="additional-resources"></a>其他資源

- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure 功能： 身分識別](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory： 為您的組織開發應用程式](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory： 多個組織中開發應用程式](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [如何實作單一登入與 Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [單一登入 windows Azure Active Directory： 深入了解](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx)– Vittorio Bertocci
- [實作和管理使用 AD FS 2.0 單一登入](https://technet.microsoft.com/library/jj205462.aspx)
