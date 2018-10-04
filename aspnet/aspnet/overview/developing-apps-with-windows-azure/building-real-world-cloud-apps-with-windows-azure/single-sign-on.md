---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: 單一登入 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 22ef4c2908783e513bfb6fb63364e71378cb8719
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578428"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>單一登入 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson]((https://twitter.com/RickAndMSFT))， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


有許多的安全性問題，思考當您正在開發雲端應用程式，但這一系列中，我們將著重於其中一個： 單一登入。 問題經常有人問這是: 「 我主要建置應用程式的 我的公司; 員工如何裝載在雲端中的這些應用程式並讓它們使用相同的安全性模型員工知道，並使用內部部署環境中，當他們執行的應用程式，依然會裝載在防火牆內嗎？ 」 其中一種我們實現此案例會呼叫 Azure Active Directory (Azure AD)。 Azure AD 可讓您提供企業特定業務 (LOB) 應用程式，透過網際網路，並可讓您可讓商務夥伴以及使用這些應用程式。

## <a name="introduction-to-azure-ad"></a>Azure AD 簡介

[Azure AD](https://docs.microsoft.com/azure/active-directory/)提供[Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx)在雲端中。 主要功能包括下列各項：

- 它與內部部署 Active Directory 整合。
- 它可讓單一登入您的應用程式。
- 它支援開放標準，例如[SAML](http://en.wikipedia.org/wiki/SAML_2.0)， [Ws-fed](http://en.wikipedia.org/wiki/WS-Federation)，並[OAuth 2.0](http://oauth.net/2/)。
- 它支援企業[Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx)。

假設您有內部部署 Windows Server Active Directory 環境，您使用以提高員工的登入內部網路應用程式：

![](single-sign-on/_static/image1.png)

哪些 Azure AD 可讓您執行是在雲端中建立的目錄。 它是免費功能且容易設定。

它可以是完全獨立於您的內部部署 Active Directory;您可以輸入任何您想要在其中，並在網際網路應用程式中進行驗證。

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

您可以將它整合與您內部部署或 AD。

![AD 和 WAAD 整合](single-sign-on/_static/image3.png)

現在所有可以驗證內部的員工也可以驗證透過網際網路 – 而不需開啟防火牆，或部署您的資料中心中的任何新伺服器。 您可以繼續利用所有的現有 Active Directory 環境，您知道並且立即使用功能讓您的內部應用程式單一登。

一旦進行此 AD 與 Azure AD 之間的連線，您也可以啟用您的 web 應用程式和行動裝置來驗證您的員工，在雲端中，而且您可以啟用第三方應用程式，例如 SalesForce.com、 Office 365、windows 或 Google 應用程式，以接受您員工的認證。 如果您使用 Office 365，您已經設定好與 Azure AD 因為 Office 365 會使用 Azure AD 進行驗證和授權。

![第 3 方應用程式](single-sign-on/_static/image4.png)

這種方法的優點是的每當您的組織新增或刪除使用者，或在使用者變更密碼，您使用相同的程序，您立即使用您的內部部署環境中。 所有您在內部部署 AD 的變更會自動傳播到雲端環境。

如果是使用您的公司，或移至 Office 365，好消息，您必須設定自動，因為 Office 365 使用 Azure AD 進行驗證的 Azure AD。 因此您可以輕鬆地使用您自己的應用程式中與 Office 365 會使用相同的驗證。

## <a name="set-up-an-azure-ad-tenant"></a>設定 Azure AD 租用戶

呼叫 Azure AD 目錄的 Azure AD[租用戶](https://technet.microsoft.com/library/jj573650.aspx)，並設定租用戶並不難。 我們將告訴您如何完成在 Azure 管理入口網站中以說明概念，但當然其他入口網站的函式類似您也可以執行它所使用的指令碼或管理 API。

在管理入口網站中，按一下 [Active Directory] 索引標籤。

![在入口網站中的 WAAD](single-sign-on/_static/image5.png)

您會自動為您的 Azure 帳戶，擁有一個 Azure AD 租用戶，您可以按一下**新增**底部的頁面，即可建立其他目錄的按鈕。 您可能想一個用於測試環境，一個用於生產環境，例如。 請仔細思考您命名新的目錄。 如果您使用您目錄的名稱，然後您可以使用您在一次的其中一個使用者名稱，很容易混淆。

![新增目錄](single-sign-on/_static/image6.png)

在入口網站有完整支援建立、 刪除和管理此環境中的使用者。 例如，若要將使用者前往**使用者**索引標籤，然後按一下**加入使用者** 按鈕。

![新增使用者 按鈕](single-sign-on/_static/image7.png)

![新增使用者 對話方塊](single-sign-on/_static/image8.png)

您可以建立存在的新使用者只有在此目錄中，或您可以為此目錄中或暫存器中的使用者或從另一個 Azure AD 目錄的使用者註冊 Microsoft 帳戶，為此目錄中的使用者。 （在實際的目錄中，預設網域會是 ContosoTest.onmicrosoft.com。 您也可以使用自己選擇，例如 contoso.com 的網域。）

![使用者類型](single-sign-on/_static/image9.png)

![新增使用者 對話方塊](single-sign-on/_static/image10.png)

您可以將使用者指派給角色。

![使用者設定檔](single-sign-on/_static/image11.png)

並使用臨時密碼建立帳戶。

![暫時密碼](single-sign-on/_static/image12.png)

這種方式建立的使用者可以立即登入您的 web 應用程式，使用這個雲端目錄。

什麼是適用於企業單一登入，不過是**目錄整合** 索引標籤：

![目錄整合 索引標籤](single-sign-on/_static/image13.png)

如果您啟用目錄整合，並[下載工具](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)，您可以將您現有的內部部署 Active Directory，您已經使用組織內部使用這個雲端目錄同步處理。 然後所有儲存在您的目錄中的使用者會顯示在這個雲端目錄中。 您的雲端應用程式現在可以驗證所有員工使用其現有的 Active Directory 認證。 這是完全免費-在同步作業工具和 Azure AD 本身。

此工具是一個精靈，是易於使用，您可以看到從這些螢幕擷取畫面。 這些不是完整的指示，只是其中會顯示基本程序的範例。 如需詳細作法-要-執行-it 的資訊，請參閱中的連結[資源](#resources)本章的最後一節。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image14.png)

按一下 [**下一步]**，然後輸入您的 Azure Active Directory 認證。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image15.png)

按一下 [**下一步]**，然後輸入您內部部署 AD 認證。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image16.png)

按一下 [**下一步]**，並指定是否您想要將 AD 密碼的雜湊儲存在雲端中。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image17.png)

您可以在雲端中儲存密碼雜湊是單向的雜湊;實際的密碼永遠不會儲存在 Azure AD 中。 如果您決定對儲存在雲端中的雜湊，您必須使用[Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS)。 另外還有[時，要考量的其他因素選擇是否要使用 ADFS](https://technet.microsoft.com/library/jj573653.aspx)。 ADFS 選項只需要一些額外的設定步驟。

如果您選擇將雜湊儲存在雲端中，您完成時，工具就會開始同步處理目錄，當您按一下 [**下一步]**。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image18.png)

在幾分鐘內便大功告成。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image19.png)

您只需要一個網域控制站，在組織中，在 Windows 2003 或更高版本上執行這個。 而且不需要重新開機。 當您完成時所有的使用者是在雲端中，您可以執行單一登入從任何 web 或行動應用程式，使用 SAML、 OAuth 或 Ws-fed。

有時候我們有人問這是有關安全，那會 Microsoft 使用它自己的商業機密資料嗎？ 答案是我們所執行。 例如，如果您移至內部 Microsoft SharePoint 站台[ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/)，提示您登入。

![Office 365 登入](single-sign-on/_static/image20.png)

Microsoft 已啟用 ADFS，因此當您輸入 Microsoft ID，您會被重新導向至 ADFS 登入頁面。

![ADFS 登入](single-sign-on/_static/image21.png)

一旦您輸入認證儲存在內部 Microsoft AD 帳戶，您可以存取此內部的應用程式。

![MS SharePoint 網站](single-sign-on/_static/image22.png)

我們主要是因為我們早已有 ADFS 設定 Azure AD 會變成可用，但登入程序會透過在雲端中 Azure AD 目錄才能使用 AD 登入伺服器。 我們將我們重要的文件、 原始檔控制、 效能管理檔案、 銷售報表，和在雲端中的多，並將此完全相同的解決方案來保護其安全。

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>建立 ASP.NET 應用程式使用 Azure AD 進行單一登入

Visual Studio 可讓您更輕鬆地建立使用 Azure AD 進行單一登入，應用程式，您可以看到從幾個螢幕擷取畫面。

當您建立的新 ASP.NET 應用程式，無論是 MVC 還是 Web Form 的預設驗證方法是 ASP.NET 身分識別。 若要將其變更為 Azure AD 中，按一下**變更驗證** 按鈕。

![變更驗證](single-sign-on/_static/image23.png)

選取 組織帳戶，請輸入您的網域名稱，然後選取單一登入。

![設定驗證對話方塊](single-sign-on/_static/image24.png)

您也可以讓應用程式讀取或讀取/寫入目錄資料的權限。 如果您這麼做時，它可以使用[Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx)以查閱使用者的電話號碼，請了解其是否在辦公室，當它們一次登入 on 等等。

這是您只需要-Visual Studio 會要求認證的 Azure AD 租用戶系統管理員，然後它會設定您的專案和新的應用程式的 Azure AD 租用戶。

當您執行專案時，您會看到一個登入頁面，以及您可以使用登入使用者的認證在您的 Azure AD 目錄中。

![組織帳戶登入](single-sign-on/_static/image25.png)

![登入](single-sign-on/_static/image26.png)

當您將應用程式部署至 Azure 時，您只需要是選取**啟用組織驗證**核取方塊，並再次 Visual Studio 會負責為您的所有設定。

![發佈 Web](single-sign-on/_static/image27.png)

這些螢幕擷取畫面是來自完整的逐步教學課程，示範如何建置使用 Azure AD 驗證的應用程式：[開發 ASP.NET 應用程式與 Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)。

## <a name="summary"></a>總結

在這一章中您所見，Azure Active Directory、 Visual Studio 及 ASP.NET，讓您輕鬆設定 單一登入您的組織使用者的網際網路應用程式中。 使用他們用來登入您的內部網路中使用 Active Directory 的相同認證的網際網路應用程式中，您的使用者可以登入。

[下一步 一章](data-storage-options.md)探討適用於雲端應用程式的資料儲存體選項。

<a id="resources"></a>
## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源：

- [Azure Active Directory 文件](https://docs.microsoft.com/azure/active-directory/)。 Windowsazure.com 網站上的 Azure AD 文件的入口網站頁面。 逐步解說教學課程中，請參閱**開發**一節。
- [Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)。 如需有關在 Azure 中的多因素驗證的文件的入口網站頁面。
- [組織帳戶驗證選項](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions)。 在 Visual Studio 2013 的 [新增專案] 對話方塊中的 Azure AD 驗證選項的說明。
- [Microsoft Patterns and Practices-同盟身分識別 」 模式](https://msdn.microsoft.com/library/dn589790.aspx)。
- [如何： 安裝 Azure Active Directory Sync 工具](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)。
- [Active Directory Federation Services 2.0 內容地圖](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)。 ADFS 2.0 的相關文件的連結。
- [在 Windows Azure AD 應用程式中的角色型和 ACL 型授權](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1)。 範例應用程式。
- [Azure Active Directory 圖形 API 部落格](https://blogs.msdn.com/b/aadgraphteam/)。
- [存取控制在 BYOD 和混合式身分識別基礎結構中的目錄整合](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=)。 Tech Ed 2014 工作階段視訊 Gayana Bagdasaryan。

> [!div class="step-by-step"]
> [上一頁](web-development-best-practices.md)
> [下一頁](data-storage-options.md)
