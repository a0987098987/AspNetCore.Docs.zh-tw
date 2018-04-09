---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: 單一登入 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件
author: MikeWasson
description: Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 82f2f99154d94074b03d580a0f491053d6f53bde
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>單一登入 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


有許多的安全性問題，思考當您正在開發雲端應用程式，但這一系列我們將焦點放在其中一個： 單一登入。 問題的人的常常詢問這是: 「 我要主要建置應用程式的 我的公司中; 員工如何裝載在雲端中的這些應用程式並讓它們使用相同的安全性模型，我員工知道和使用內部部署環境中，當它們執行的應用程式，依然會裝載在防火牆內嗎？ 」 其中一種方式，我們會啟用這種情況下會呼叫 Azure Active Directory (Azure AD)。 Azure AD 可讓您使用企業的特定業務 (LOB) 應用程式透過網際網路，而且可讓您可讓商務夥伴以及使用這些應用程式。

## <a name="introduction-to-azure-ad"></a>Azure AD 簡介

[Azure AD](https://docs.microsoft.com/azure/active-directory/)提供[Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx)雲端中。 主要功能包括下列各項：

- 它會與內部部署 Active Directory 整合。
- 它可讓單一登入與您的應用程式。
- 它支援開放標準，例如[SAML](http://en.wikipedia.org/wiki/SAML_2.0)， [Ws-fed](http://en.wikipedia.org/wiki/WS-Federation)，和[OAuth 2.0](http://oauth.net/2/)。
- 它支援企業[Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx)。

假設您有內部部署 Windows Server Active Directory 環境您用來啟用登入內部網路應用程式的員工：

![](single-sign-on/_static/image1.png)

哪些 Azure AD 可讓您執行此作業是在雲端上建立目錄。 這是可用的功能且容易設定。

它可以是完全獨立於您的內部部署 Active Directory。您可以將您想在其中，並進行驗證，在網際網路應用程式中的任何人。

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

您可以將它整合與您內部部署或 AD。

![AD 和 WAAD 整合](single-sign-on/_static/image3.png)

現在所有可以驗證內部的員工可以也進行驗證，透過網際網路 – 您不必開啟防火牆，或部署資料中心中的任何新伺服器。 您可以繼續利用所有現有 Active Directory 環境，您知道使用今天功能讓您內部應用程式單一登。

一旦您所做這個 AD 與 Azure AD 之間的連線，您也可以啟用您的 web 應用程式和行動裝置來驗證您的員工在雲端中，而且您可以啟用協力廠商應用程式，例如 Office 365、 SalesForce.com、 或 Google 應用程式，以接受您員工的認證。 如果您使用 Office 365，您已經設定好 Azure AD 與因為 Office 365 會使用 Azure AD 進行驗證和授權。

![第 3 個合作對象應用程式](single-sign-on/_static/image4.png)

這種方法的優點是的每當您的組織新增或刪除使用者，或使用者變更密碼，您使用內部部署環境中目前使用的相同程序。 所有的內部 AD 變更會自動傳播至雲端環境。

如果您的公司是使用，或移至 Office 365 好消息您必須設定自動，因為 Office 365 會使用 Azure AD 進行驗證的 Azure AD。 因此您可以輕鬆地使用您自己的應用程式中的 Office 365 會使用的相同驗證。

## <a name="set-up-an-azure-ad-tenant"></a>設定 Azure AD 租用戶

呼叫 Azure AD 目錄的 Azure AD[租用戶](https://technet.microsoft.com/library/jj573650.aspx)，並設定租用戶會很簡單。 我們會告訴您如何它為了在 Azure 管理入口網站中說明的概念，但當然和其他入口網站的函式一樣您也可以執行它所使用的指令碼或管理應用程式開發介面。

在管理入口網站中按一下 [Active Directory] 索引標籤。

![WAAD 在入口網站](single-sign-on/_static/image5.png)

您會自動為您的 Azure 帳戶擁有一個 Azure AD 租用戶，您可以按一下**新增**建立額外的目錄 頁面底部的按鈕。 您可能需要一個用於測試環境，一個用於生產環境中，例如。 請仔細考慮有關您命名新的目錄。 如果您使用您的目錄名稱，然後您可以使用您在一次一位使用者的名稱，可能會造成混淆。

![新增目錄](single-sign-on/_static/image6.png)

在入口網站已建立、 刪除和管理此環境中的使用者的完整支援。 例如，若要加入使用者前往**使用者**索引標籤上，按一下 [**新增使用者**] 按鈕。

![新增使用者 按鈕](single-sign-on/_static/image7.png)

![加入使用者 對話方塊](single-sign-on/_static/image8.png)

您可以只在這個目錄中，建立新使用者存在，或您可以為此目錄中或暫存器中的使用者或其他 Azure AD 目錄的使用者註冊 Microsoft 帳戶，此目錄中的使用者身分。 （在實際的目錄中，預設網域是 ContosoTest.onmicrosoft.com。您也可以使用自己選擇，例如 contoso.com 的網域。）

![使用者類型](single-sign-on/_static/image9.png)

![加入使用者 對話方塊](single-sign-on/_static/image10.png)

您可以指派使用者至角色。

![使用者設定檔](single-sign-on/_static/image11.png)

並在帳戶建立的暫時密碼。

![暫時密碼](single-sign-on/_static/image12.png)

這種方式建立的使用者可以立即登入您的 web 應用程式使用此雲端目錄。

什麼是最適合用於企業單一登入，不過，是**目錄整合** 索引標籤：

![目錄整合 索引標籤](single-sign-on/_static/image13.png)

如果您啟用目錄整合和[下載工具](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)，您可以同步處理現有的內部部署 Active Directory，您已經使用您組織內部與這個雲端目錄。 然後所有儲存在您的目錄中的使用者會顯示此雲端目錄中。 雲端應用程式現在可以進行驗證所有員工使用其現有的 Active Directory 認證。 這是完全免費 – 同步作業工具和 Azure AD 本身。

此工具是一個精靈，為易於使用，您可以看到這些螢幕擷取畫面。 這些不是完整的指示，只顯示基本程序的範例。 如需詳細 how-要執行的 it 的資訊，請參閱中的連結[資源](#resources)章節的結尾 」 一節。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image14.png)

按一下**下一步**，然後輸入您的 Azure Active Directory 認證。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image15.png)

按一下**下一步**，然後輸入您內部部署 AD 認證。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image16.png)

按一下**下一步**，表示如果您想要在雲端中儲存的 AD 密碼雜湊。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image17.png)

您可以在雲端中儲存密碼雜湊是單向的雜湊。實際的密碼永遠不會儲存在 Azure AD 中。 如果您決定對雜湊儲存在雲端中，您必須使用[Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS)。 另外還有[時，要考量的其他因素選擇是否使用 ADFS](https://technet.microsoft.com/library/jj573653.aspx)。 ADFS 選項需要一些額外的設定步驟。

如果您選擇將雜湊儲存在雲端中，您完成時，工具將會啟動同步處理目錄，當您按一下**下一步**。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image18.png)

並在幾分鐘內完成。

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image19.png)

您只能有一個在組織中，在 Windows 2003 或更新版本的網域控制站上執行此。 也不需要重新開機。 當您完成時，所有使用者都在雲端，而且您可以執行單一登入從任何 web 或行動應用程式，使用 SAML、 OAuth、 或 Ws-fed。

有時候我們取得要求這是有關安全-沒有 Microsoft 用於它自己的敏感的商業資料？ 就的是我們所執行。 例如，如果您移至內部 Microsoft SharePoint 站台[ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/)，系統提示您取得登入。

![Office 365 登入](single-sign-on/_static/image20.png)

Microsoft 已啟用 ADFS，因此當您輸入 Microsoft ID，您取得重新導向至 ADFS 登入頁面。

![ADFS 登入](single-sign-on/_static/image21.png)

一旦您輸入儲存在內部 Microsoft AD 帳戶的認證，您可以存取此內部應用程式。

![MS SharePoint 網站](single-sign-on/_static/image22.png)

我們會使用 AD 登入伺服器，主要是因為我們已經有 Azure AD 變成可用，但在登入程序都會通過 Azure AD 目錄中的雲端之前，先設定 ADFS。 我們會讓我們重要的文件、 原始檔控制、 效能管理檔案、 銷售報表和雲端中的多，並且使用此完全相同的解決方案來保護其安全。

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>建立 ASP.NET 應用程式使用 Azure AD 進行單一登入

Visual Studio 可讓您輕鬆建立使用 Azure AD 進行單一登入，應用程式，您可以看到從幾個螢幕擷取畫面。

當您建立新 ASP.NET 應用程式、 MVC 或 Web Form 的預設驗證方法是 ASP.NET Identity。 若要變更的 Azure AD，您按一下**變更驗證** 按鈕。

![變更驗證](single-sign-on/_static/image23.png)

選取組織帳戶，輸入您的網域名稱，然後選取 單一登入。

![設定驗證對話方塊](single-sign-on/_static/image24.png)

您也可以讓應用程式讀取或讀取/寫入目錄資料的權限。 如果您這樣做，可以使用[Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx)查閱使用者的電話號碼，找出其是否在辦公室，上次登入等。

這就是您必須執行的 Visual Studio 會要求認證的 Azure AD 租用戶系統管理員，並接著設定您的專案和 Azure AD 租用戶的新應用程式。

當您執行專案時，您會看到，登入頁面，以及您可以使用認證登入使用者的 Azure AD 目錄中。

![組織帳戶登入](single-sign-on/_static/image25.png)

![登入](single-sign-on/_static/image26.png)

當您將應用程式部署至 Azure 時，您必須先執行所有已選取**啟用組織驗證**核取方塊，並再次 Visual Studio 會負責為您的所有設定。

![發行網站](single-sign-on/_static/image27.png)

這些螢幕擷取畫面來自示範如何建置使用 Azure AD 驗證的應用程式的完整逐步教學課程：[開發 ASP.NET 應用程式與 Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)。

## <a name="summary"></a>總結

在這一章您已看到，Azure Active Directory、 Visual Studio 和 ASP.NET，讓您輕鬆設定單一登入貴組織的使用者的網際網路應用程式中。 您的使用者可以在網際網路應用程式中使用它們用來登入您的內部網路中使用 Active Directory 的相同認證登入。

[下一章](data-storage-options.md)會查看可用的雲端應用程式的資料儲存體選項。

<a id="resources"></a>
## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源：

- [Azure Active Directory 文件](https://docs.microsoft.com/azure/active-directory/)。 如需 windowsazure.com 網站上的 Azure AD 文件入口網站頁面。 如需逐步教學課程，請參閱**開發**> 一節。
- [Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)。 如需有關在 Azure 中的多因素驗證的文件入口網站頁面。
- [組織帳戶的驗證選項](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions)。 在 Visual Studio 2013 的 [新增專案] 對話方塊中的 Azure AD 驗證選項的說明。
- [Microsoft Patterns and Practices-同盟身分識別模式](https://msdn.microsoft.com/library/dn589790.aspx)。
- [如何： 安裝 Azure Active Directory 同步作業工具](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)。
- [Active Directory Federation Services 2.0 內容地圖](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)。 ADFS 2.0 的相關文件的連結。
- [在 Windows Azure AD 應用程式中的角色型和 ACL 型授權](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1)。 範例應用程式。
- [Azure Active Directory Graph API 部落格](https://blogs.msdn.com/b/aadgraphteam/)。
- [存取控制在 BYOD 和混合式身分識別基礎結構中的目錄整合](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=)。 技術 Ed 2014 工作階段視訊 Gayana Bagdasaryan。

> [!div class="step-by-step"]
> [上一頁](web-development-best-practices.md)
> [下一頁](data-storage-options.md)
