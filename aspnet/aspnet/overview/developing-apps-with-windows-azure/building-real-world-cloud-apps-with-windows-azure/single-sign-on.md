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
ms.locfileid: "30873881"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="61224-104">單一登入 （使用 Azure 建置實際的雲端應用程式）</span><span class="sxs-lookup"><span data-stu-id="61224-104">Single Sign-On (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="61224-105">由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="61224-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="61224-106">[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="61224-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="61224-107">**建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。</span><span class="sxs-lookup"><span data-stu-id="61224-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="61224-108">它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="61224-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="61224-109">E 書籍的相關資訊，請參閱[第一章](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="61224-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="61224-110">有許多的安全性問題，思考當您正在開發雲端應用程式，但這一系列我們將焦點放在其中一個： 單一登入。</span><span class="sxs-lookup"><span data-stu-id="61224-110">There are many security issues to think about when you're developing a cloud app, but for this series we'll focus on just one: single sign-on.</span></span> <span data-ttu-id="61224-111">問題的人的常常詢問這是: 「 我要主要建置應用程式的 我的公司中; 員工如何裝載在雲端中的這些應用程式並讓它們使用相同的安全性模型，我員工知道和使用內部部署環境中，當它們執行的應用程式，依然會裝載在防火牆內嗎？ 」</span><span class="sxs-lookup"><span data-stu-id="61224-111">A question people often ask is this: "I'm primarily building apps for the employees of my company; how do I host these apps in the cloud and still enable them to use the same security model that my employees know and use in the on-premises environment when they're running apps that are hosted inside the firewall?"</span></span> <span data-ttu-id="61224-112">其中一種方式，我們會啟用這種情況下會呼叫 Azure Active Directory (Azure AD)。</span><span class="sxs-lookup"><span data-stu-id="61224-112">One of the ways we enable this scenario is called Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="61224-113">Azure AD 可讓您使用企業的特定業務 (LOB) 應用程式透過網際網路，而且可讓您可讓商務夥伴以及使用這些應用程式。</span><span class="sxs-lookup"><span data-stu-id="61224-113">Azure AD enables you to make enterprise line-of-business (LOB) apps available over the Internet, and it enables you to make these apps available to business partners as well.</span></span>

## <a name="introduction-to-azure-ad"></a><span data-ttu-id="61224-114">Azure AD 簡介</span><span class="sxs-lookup"><span data-stu-id="61224-114">Introduction to Azure AD</span></span>

<span data-ttu-id="61224-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/)提供[Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx)雲端中。</span><span class="sxs-lookup"><span data-stu-id="61224-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) provides [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in the cloud.</span></span> <span data-ttu-id="61224-116">主要功能包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="61224-116">Key features include the following:</span></span>

- <span data-ttu-id="61224-117">它會與內部部署 Active Directory 整合。</span><span class="sxs-lookup"><span data-stu-id="61224-117">It integrates with on-premises Active Directory.</span></span>
- <span data-ttu-id="61224-118">它可讓單一登入與您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="61224-118">It enables single sign-on with your apps.</span></span>
- <span data-ttu-id="61224-119">它支援開放標準，例如[SAML](http://en.wikipedia.org/wiki/SAML_2.0)， [Ws-fed](http://en.wikipedia.org/wiki/WS-Federation)，和[OAuth 2.0](http://oauth.net/2/)。</span><span class="sxs-lookup"><span data-stu-id="61224-119">It supports open standards such as [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), and [OAuth 2.0](http://oauth.net/2/).</span></span>
- <span data-ttu-id="61224-120">它支援企業[Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx)。</span><span class="sxs-lookup"><span data-stu-id="61224-120">It supports Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span></span>

<span data-ttu-id="61224-121">假設您有內部部署 Windows Server Active Directory 環境您用來啟用登入內部網路應用程式的員工：</span><span class="sxs-lookup"><span data-stu-id="61224-121">Suppose you have an on-premises Windows Server Active Directory environment that you use to enable employees to sign on to Intranet apps:</span></span>

![](single-sign-on/_static/image1.png)

<span data-ttu-id="61224-122">哪些 Azure AD 可讓您執行此作業是在雲端上建立目錄。</span><span class="sxs-lookup"><span data-stu-id="61224-122">What Azure AD enables you to do is create a directory in the cloud.</span></span> <span data-ttu-id="61224-123">這是可用的功能且容易設定。</span><span class="sxs-lookup"><span data-stu-id="61224-123">It's a free feature and easy to set up.</span></span>

<span data-ttu-id="61224-124">它可以是完全獨立於您的內部部署 Active Directory。您可以將您想在其中，並進行驗證，在網際網路應用程式中的任何人。</span><span class="sxs-lookup"><span data-stu-id="61224-124">It can be entirely independent from your on-premises Active Directory; you can put anyone you want in it and authenticate them in Internet apps.</span></span>

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

<span data-ttu-id="61224-126">您可以將它整合與您內部部署或 AD。</span><span class="sxs-lookup"><span data-stu-id="61224-126">Or you can integrate it with your on-premises AD.</span></span>

![AD 和 WAAD 整合](single-sign-on/_static/image3.png)

<span data-ttu-id="61224-128">現在所有可以驗證內部的員工可以也進行驗證，透過網際網路 – 您不必開啟防火牆，或部署資料中心中的任何新伺服器。</span><span class="sxs-lookup"><span data-stu-id="61224-128">Now all the employees who can authenticate on-premises can also authenticate over the Internet – without you having to open up a firewall or deploy any new servers in your data center.</span></span> <span data-ttu-id="61224-129">您可以繼續利用所有現有 Active Directory 環境，您知道使用今天功能讓您內部應用程式單一登。</span><span class="sxs-lookup"><span data-stu-id="61224-129">You can continue to leverage all the existing Active Directory environment that you know and use today to give your internal apps single-sign on capability.</span></span>

<span data-ttu-id="61224-130">一旦您所做這個 AD 與 Azure AD 之間的連線，您也可以啟用您的 web 應用程式和行動裝置來驗證您的員工在雲端中，而且您可以啟用協力廠商應用程式，例如 Office 365、 SalesForce.com、 或 Google 應用程式，以接受您員工的認證。</span><span class="sxs-lookup"><span data-stu-id="61224-130">Once you've made this connection between AD and Azure AD, you can also enable your web apps and your mobile devices to authenticate your employees in the cloud, and you can enable third-party apps, such as Office 365, SalesForce.com, or Google apps, to accept your employees' credentials.</span></span> <span data-ttu-id="61224-131">如果您使用 Office 365，您已經設定好 Azure AD 與因為 Office 365 會使用 Azure AD 進行驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="61224-131">If you're using Office 365, you're already set up with Azure AD because Office 365 uses Azure AD for authentication and authorization.</span></span>

![第 3 個合作對象應用程式](single-sign-on/_static/image4.png)

<span data-ttu-id="61224-133">這種方法的優點是的每當您的組織新增或刪除使用者，或使用者變更密碼，您使用內部部署環境中目前使用的相同程序。</span><span class="sxs-lookup"><span data-stu-id="61224-133">The beauty of this approach is that any time your organization adds or deletes a user, or a user changes a password, you use the same process that you use today in your on-premises environment.</span></span> <span data-ttu-id="61224-134">所有的內部 AD 變更會自動傳播至雲端環境。</span><span class="sxs-lookup"><span data-stu-id="61224-134">All of your on-premises AD changes are automatically propagated to the cloud environment.</span></span>

<span data-ttu-id="61224-135">如果您的公司是使用，或移至 Office 365 好消息您必須設定自動，因為 Office 365 會使用 Azure AD 進行驗證的 Azure AD。</span><span class="sxs-lookup"><span data-stu-id="61224-135">If your company is using or moving to Office 365, the good news is that you'll have Azure AD set up automatically because Office 365 uses Azure AD for authentication.</span></span> <span data-ttu-id="61224-136">因此您可以輕鬆地使用您自己的應用程式中的 Office 365 會使用的相同驗證。</span><span class="sxs-lookup"><span data-stu-id="61224-136">So you can easily use in your own apps the same authentication that Office 365 uses.</span></span>

## <a name="set-up-an-azure-ad-tenant"></a><span data-ttu-id="61224-137">設定 Azure AD 租用戶</span><span class="sxs-lookup"><span data-stu-id="61224-137">Set up an Azure AD tenant</span></span>

<span data-ttu-id="61224-138">呼叫 Azure AD 目錄的 Azure AD[租用戶](https://technet.microsoft.com/library/jj573650.aspx)，並設定租用戶會很簡單。</span><span class="sxs-lookup"><span data-stu-id="61224-138">an Azure AD directory is called an Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), and setting up a tenant is pretty easy.</span></span> <span data-ttu-id="61224-139">我們會告訴您如何它為了在 Azure 管理入口網站中說明的概念，但當然和其他入口網站的函式一樣您也可以執行它所使用的指令碼或管理應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="61224-139">We'll show you how it's done in the Azure Management Portal in order to illustrate the concepts, but of course like the other portal functions you can also do it by using a script or management API.</span></span>

<span data-ttu-id="61224-140">在管理入口網站中按一下 [Active Directory] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="61224-140">In the management portal click the Active Directory tab.</span></span>

![WAAD 在入口網站](single-sign-on/_static/image5.png)

<span data-ttu-id="61224-142">您會自動為您的 Azure 帳戶擁有一個 Azure AD 租用戶，您可以按一下**新增**建立額外的目錄 頁面底部的按鈕。</span><span class="sxs-lookup"><span data-stu-id="61224-142">You automatically have one Azure AD tenant for your Azure account, and you can click the **Add** button at the bottom of the page to create additional directories.</span></span> <span data-ttu-id="61224-143">您可能需要一個用於測試環境，一個用於生產環境中，例如。</span><span class="sxs-lookup"><span data-stu-id="61224-143">You might want one for a test environment and one for production, for example.</span></span> <span data-ttu-id="61224-144">請仔細考慮有關您命名新的目錄。</span><span class="sxs-lookup"><span data-stu-id="61224-144">Think carefully about what you name a new directory.</span></span> <span data-ttu-id="61224-145">如果您使用您的目錄名稱，然後您可以使用您在一次一位使用者的名稱，可能會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="61224-145">If you use your name for the directory and then you use your name again for one of the users, that can be confusing.</span></span>

![新增目錄](single-sign-on/_static/image6.png)

<span data-ttu-id="61224-147">在入口網站已建立、 刪除和管理此環境中的使用者的完整支援。</span><span class="sxs-lookup"><span data-stu-id="61224-147">The portal has full support for creating, deleting, and managing users within this environment.</span></span> <span data-ttu-id="61224-148">例如，若要加入使用者前往**使用者**索引標籤上，按一下 [**新增使用者**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="61224-148">For example, to add a user go to the **Users** tab and click the **Add User** button.</span></span>

![新增使用者 按鈕](single-sign-on/_static/image7.png)

![加入使用者 對話方塊](single-sign-on/_static/image8.png)

<span data-ttu-id="61224-151">您可以只在這個目錄中，建立新使用者存在，或您可以為此目錄中或暫存器中的使用者或其他 Azure AD 目錄的使用者註冊 Microsoft 帳戶，此目錄中的使用者身分。</span><span class="sxs-lookup"><span data-stu-id="61224-151">You can create a new user who exists only in this directory, or you can register a Microsoft Account as a user in this directory, or register or a user from another Azure AD directory as a user in this directory.</span></span> <span data-ttu-id="61224-152">（在實際的目錄中，預設網域是 ContosoTest.onmicrosoft.com。您也可以使用自己選擇，例如 contoso.com 的網域。）</span><span class="sxs-lookup"><span data-stu-id="61224-152">(In a real directory, the default domain would be ContosoTest.onmicrosoft.com. You can also use a domain of your own choosing, like contoso.com.)</span></span>

![使用者類型](single-sign-on/_static/image9.png)

![加入使用者 對話方塊](single-sign-on/_static/image10.png)

<span data-ttu-id="61224-155">您可以指派使用者至角色。</span><span class="sxs-lookup"><span data-stu-id="61224-155">You can assign the user to a role.</span></span>

![使用者設定檔](single-sign-on/_static/image11.png)

<span data-ttu-id="61224-157">並在帳戶建立的暫時密碼。</span><span class="sxs-lookup"><span data-stu-id="61224-157">And the account is created with a temporary password.</span></span>

![暫時密碼](single-sign-on/_static/image12.png)

<span data-ttu-id="61224-159">這種方式建立的使用者可以立即登入您的 web 應用程式使用此雲端目錄。</span><span class="sxs-lookup"><span data-stu-id="61224-159">The users you create this way can immediately log in to your web apps using this cloud directory.</span></span>

<span data-ttu-id="61224-160">什麼是最適合用於企業單一登入，不過，是**目錄整合** 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="61224-160">What's great for enterprise single sign-on, though, is the **Directory Integration** tab:</span></span>

![目錄整合 索引標籤](single-sign-on/_static/image13.png)

<span data-ttu-id="61224-162">如果您啟用目錄整合和[下載工具](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)，您可以同步處理現有的內部部署 Active Directory，您已經使用您組織內部與這個雲端目錄。</span><span class="sxs-lookup"><span data-stu-id="61224-162">If you enable directory integration, and [download a tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), you can sync this cloud directory with your existing on-premises Active Directory that you're already using inside your organization.</span></span> <span data-ttu-id="61224-163">然後所有儲存在您的目錄中的使用者會顯示此雲端目錄中。</span><span class="sxs-lookup"><span data-stu-id="61224-163">Then all of the users stored in your directory will show up in this cloud directory.</span></span> <span data-ttu-id="61224-164">雲端應用程式現在可以進行驗證所有員工使用其現有的 Active Directory 認證。</span><span class="sxs-lookup"><span data-stu-id="61224-164">Your cloud apps can now authenticate all of your employees using their existing Active Directory credentials.</span></span> <span data-ttu-id="61224-165">這是完全免費 – 同步作業工具和 Azure AD 本身。</span><span class="sxs-lookup"><span data-stu-id="61224-165">And all this is free – both the sync tool and Azure AD itself.</span></span>

<span data-ttu-id="61224-166">此工具是一個精靈，為易於使用，您可以看到這些螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="61224-166">The tool is a wizard that is easy to use, as you can see from these screen shots.</span></span> <span data-ttu-id="61224-167">這些不是完整的指示，只顯示基本程序的範例。</span><span class="sxs-lookup"><span data-stu-id="61224-167">These are not complete instructions, just an example showing you the basic process.</span></span> <span data-ttu-id="61224-168">如需詳細 how-要執行的 it 的資訊，請參閱中的連結[資源](#resources)章節的結尾 」 一節。</span><span class="sxs-lookup"><span data-stu-id="61224-168">For more detailed how-to-do-it information, see the links in the [Resources](#resources) section at the end of the chapter.</span></span>

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image14.png)

<span data-ttu-id="61224-170">按一下**下一步**，然後輸入您的 Azure Active Directory 認證。</span><span class="sxs-lookup"><span data-stu-id="61224-170">Click **Next**, and then enter your Azure Active Directory credentials.</span></span>

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image15.png)

<span data-ttu-id="61224-172">按一下**下一步**，然後輸入您內部部署 AD 認證。</span><span class="sxs-lookup"><span data-stu-id="61224-172">Click **Next**, and then enter your on-premises AD credentials.</span></span>

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image16.png)

<span data-ttu-id="61224-174">按一下**下一步**，表示如果您想要在雲端中儲存的 AD 密碼雜湊。</span><span class="sxs-lookup"><span data-stu-id="61224-174">Click **Next**, and then indicate if you want to store a hash of your AD passwords in the cloud.</span></span>

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image17.png)

<span data-ttu-id="61224-176">您可以在雲端中儲存密碼雜湊是單向的雜湊。實際的密碼永遠不會儲存在 Azure AD 中。</span><span class="sxs-lookup"><span data-stu-id="61224-176">The password hash that you can store in the cloud is a one-way hash; actual passwords are never stored in Azure AD.</span></span> <span data-ttu-id="61224-177">如果您決定對雜湊儲存在雲端中，您必須使用[Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS)。</span><span class="sxs-lookup"><span data-stu-id="61224-177">If you decide against storing hashes in the cloud, you'll have to use [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span></span> <span data-ttu-id="61224-178">另外還有[時，要考量的其他因素選擇是否使用 ADFS](https://technet.microsoft.com/library/jj573653.aspx)。</span><span class="sxs-lookup"><span data-stu-id="61224-178">There are also [other factors to consider when choosing whether or not to use ADFS](https://technet.microsoft.com/library/jj573653.aspx).</span></span> <span data-ttu-id="61224-179">ADFS 選項需要一些額外的設定步驟。</span><span class="sxs-lookup"><span data-stu-id="61224-179">The ADFS option requires a few additional configuration steps.</span></span>

<span data-ttu-id="61224-180">如果您選擇將雜湊儲存在雲端中，您完成時，工具將會啟動同步處理目錄，當您按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="61224-180">If you choose to store hashes in the cloud, you're done, and the tool starts synchronizing directories when you click **Next**.</span></span>

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image18.png)

<span data-ttu-id="61224-182">並在幾分鐘內完成。</span><span class="sxs-lookup"><span data-stu-id="61224-182">And in a few minutes you're done.</span></span>

![WAAD 同步作業工具組態精靈](single-sign-on/_static/image19.png)

<span data-ttu-id="61224-184">您只能有一個在組織中，在 Windows 2003 或更新版本的網域控制站上執行此。</span><span class="sxs-lookup"><span data-stu-id="61224-184">You only have to run this on one domain controller in the organization, on Windows 2003 or higher.</span></span> <span data-ttu-id="61224-185">也不需要重新開機。</span><span class="sxs-lookup"><span data-stu-id="61224-185">And no need to reboot.</span></span> <span data-ttu-id="61224-186">當您完成時，所有使用者都在雲端，而且您可以執行單一登入從任何 web 或行動應用程式，使用 SAML、 OAuth、 或 Ws-fed。</span><span class="sxs-lookup"><span data-stu-id="61224-186">When you're done, all of your users are in the cloud and you can do single sign-on from any web or mobile application, using SAML, OAuth, or WS-Fed.</span></span>

<span data-ttu-id="61224-187">有時候我們取得要求這是有關安全-沒有 Microsoft 用於它自己的敏感的商業資料？</span><span class="sxs-lookup"><span data-stu-id="61224-187">Sometimes we get asked about how secure this is – does Microsoft use it for their own sensitive business data?</span></span> <span data-ttu-id="61224-188">就的是我們所執行。</span><span class="sxs-lookup"><span data-stu-id="61224-188">And the answer is yes we do.</span></span> <span data-ttu-id="61224-189">例如，如果您移至內部 Microsoft SharePoint 站台[ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/)，系統提示您取得登入。</span><span class="sxs-lookup"><span data-stu-id="61224-189">For example, if you go to the internal Microsoft SharePoint site at [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), you get prompted to log in.</span></span>

![Office 365 登入](single-sign-on/_static/image20.png)

<span data-ttu-id="61224-191">Microsoft 已啟用 ADFS，因此當您輸入 Microsoft ID，您取得重新導向至 ADFS 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="61224-191">Microsoft has enabled ADFS, so when you enter a Microsoft ID, you get redirected to an ADFS log-in page.</span></span>

![ADFS 登入](single-sign-on/_static/image21.png)

<span data-ttu-id="61224-193">一旦您輸入儲存在內部 Microsoft AD 帳戶的認證，您可以存取此內部應用程式。</span><span class="sxs-lookup"><span data-stu-id="61224-193">And once you enter credentials stored in an internal Microsoft AD account, you have access to this internal application.</span></span>

![MS SharePoint 網站](single-sign-on/_static/image22.png)

<span data-ttu-id="61224-195">我們會使用 AD 登入伺服器，主要是因為我們已經有 Azure AD 變成可用，但在登入程序都會通過 Azure AD 目錄中的雲端之前，先設定 ADFS。</span><span class="sxs-lookup"><span data-stu-id="61224-195">We're using an AD sign-in server mainly because we already had ADFS set up before Azure AD became available, but the log-in process is going through an Azure AD directory in the cloud.</span></span> <span data-ttu-id="61224-196">我們會讓我們重要的文件、 原始檔控制、 效能管理檔案、 銷售報表和雲端中的多，並且使用此完全相同的解決方案來保護其安全。</span><span class="sxs-lookup"><span data-stu-id="61224-196">We put our important documents, source control, performance management files, sales reports, and more, in the cloud and are using this exact same solution to secure them.</span></span>

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a><span data-ttu-id="61224-197">建立 ASP.NET 應用程式使用 Azure AD 進行單一登入</span><span class="sxs-lookup"><span data-stu-id="61224-197">Create an ASP.NET app that uses Azure AD for single sign-on</span></span>

<span data-ttu-id="61224-198">Visual Studio 可讓您輕鬆建立使用 Azure AD 進行單一登入，應用程式，您可以看到從幾個螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="61224-198">Visual Studio makes it really easy to create an app that uses Azure AD for single sign-on, as you can see from a few screen shots.</span></span>

<span data-ttu-id="61224-199">當您建立新 ASP.NET 應用程式、 MVC 或 Web Form 的預設驗證方法是 ASP.NET Identity。</span><span class="sxs-lookup"><span data-stu-id="61224-199">When you create a new ASP.NET application, either MVC or Web Forms, the default authentication method is ASP.NET Identity.</span></span> <span data-ttu-id="61224-200">若要變更的 Azure AD，您按一下**變更驗證** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="61224-200">To change that to Azure AD, you click a **Change Authentication** button.</span></span>

![變更驗證](single-sign-on/_static/image23.png)

<span data-ttu-id="61224-202">選取組織帳戶，輸入您的網域名稱，然後選取 單一登入。</span><span class="sxs-lookup"><span data-stu-id="61224-202">Select Organizational Accounts, enter your domain name, and then select Single Sign On.</span></span>

![設定驗證對話方塊](single-sign-on/_static/image24.png)

<span data-ttu-id="61224-204">您也可以讓應用程式讀取或讀取/寫入目錄資料的權限。</span><span class="sxs-lookup"><span data-stu-id="61224-204">You can also give the app read or read/write permission for directory data.</span></span> <span data-ttu-id="61224-205">如果您這樣做，可以使用[Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx)查閱使用者的電話號碼，找出其是否在辦公室，上次登入等。</span><span class="sxs-lookup"><span data-stu-id="61224-205">If you do that, it can use the [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) to look up users' phone number, find out if they're in the office, when they last logged on, etc.</span></span>

<span data-ttu-id="61224-206">這就是您必須執行的 Visual Studio 會要求認證的 Azure AD 租用戶系統管理員，並接著設定您的專案和 Azure AD 租用戶的新應用程式。</span><span class="sxs-lookup"><span data-stu-id="61224-206">That's all you have to do - Visual Studio asks for the credentials for an administrator for your Azure AD tenant, and then it configures both your project and your Azure AD tenant for the new application.</span></span>

<span data-ttu-id="61224-207">當您執行專案時，您會看到，登入頁面，以及您可以使用認證登入使用者的 Azure AD 目錄中。</span><span class="sxs-lookup"><span data-stu-id="61224-207">When you run the project, you'll see a sign-in page, and you can sign in with credentials of a user in your Azure AD directory.</span></span>

![組織帳戶登入](single-sign-on/_static/image25.png)

![登入](single-sign-on/_static/image26.png)

<span data-ttu-id="61224-210">當您將應用程式部署至 Azure 時，您必須先執行所有已選取**啟用組織驗證**核取方塊，並再次 Visual Studio 會負責為您的所有設定。</span><span class="sxs-lookup"><span data-stu-id="61224-210">When you deploy the app to Azure, all you have to do is select an **Enable Organizational Authentication** check box, and once again Visual Studio takes care of all the configuration for you.</span></span>

![發行網站](single-sign-on/_static/image27.png)

<span data-ttu-id="61224-212">這些螢幕擷取畫面來自示範如何建置使用 Azure AD 驗證的應用程式的完整逐步教學課程：[開發 ASP.NET 應用程式與 Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)。</span><span class="sxs-lookup"><span data-stu-id="61224-212">These screen shots come from a complete step-by-step tutorial that shows how to build an app that uses Azure AD authentication: [Developing ASP.NET Apps with Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span></span>

## <a name="summary"></a><span data-ttu-id="61224-213">總結</span><span class="sxs-lookup"><span data-stu-id="61224-213">Summary</span></span>

<span data-ttu-id="61224-214">在這一章您已看到，Azure Active Directory、 Visual Studio 和 ASP.NET，讓您輕鬆設定單一登入貴組織的使用者的網際網路應用程式中。</span><span class="sxs-lookup"><span data-stu-id="61224-214">In this chapter you saw that Azure Active Directory, Visual Studio, and ASP.NET, make it easy to set up single sign-on in Internet applications for your organization's users.</span></span> <span data-ttu-id="61224-215">您的使用者可以在網際網路應用程式中使用它們用來登入您的內部網路中使用 Active Directory 的相同認證登入。</span><span class="sxs-lookup"><span data-stu-id="61224-215">Your users can sign on in Internet apps using the same credentials they use to sign on using Active Directory in your internal network.</span></span>

<span data-ttu-id="61224-216">[下一章](data-storage-options.md)會查看可用的雲端應用程式的資料儲存體選項。</span><span class="sxs-lookup"><span data-stu-id="61224-216">The [next chapter](data-storage-options.md) looks at the data storage options available for a cloud app.</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="61224-217">資源</span><span class="sxs-lookup"><span data-stu-id="61224-217">Resources</span></span>

<span data-ttu-id="61224-218">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="61224-218">For more information, see the following resources:</span></span>

- <span data-ttu-id="61224-219">[Azure Active Directory 文件](https://docs.microsoft.com/azure/active-directory/)。</span><span class="sxs-lookup"><span data-stu-id="61224-219">[Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="61224-220">如需 windowsazure.com 網站上的 Azure AD 文件入口網站頁面。</span><span class="sxs-lookup"><span data-stu-id="61224-220">Portal page for Azure AD documentation on the windowsazure.com site.</span></span> <span data-ttu-id="61224-221">如需逐步教學課程，請參閱**開發**> 一節。</span><span class="sxs-lookup"><span data-stu-id="61224-221">For step by step tutorials, see the **Develop** section.</span></span>
- <span data-ttu-id="61224-222">[Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)。</span><span class="sxs-lookup"><span data-stu-id="61224-222">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span></span> <span data-ttu-id="61224-223">如需有關在 Azure 中的多因素驗證的文件入口網站頁面。</span><span class="sxs-lookup"><span data-stu-id="61224-223">Portal page for documentation about multi-factor authentication in Azure.</span></span>
- <span data-ttu-id="61224-224">[組織帳戶的驗證選項](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions)。</span><span class="sxs-lookup"><span data-stu-id="61224-224">[Organizational account authentication options](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span></span> <span data-ttu-id="61224-225">在 Visual Studio 2013 的 [新增專案] 對話方塊中的 Azure AD 驗證選項的說明。</span><span class="sxs-lookup"><span data-stu-id="61224-225">Explanation of the Azure AD authentication options in the Visual Studio 2013 new-project dialog.</span></span>
- <span data-ttu-id="61224-226">[Microsoft Patterns and Practices-同盟身分識別模式](https://msdn.microsoft.com/library/dn589790.aspx)。</span><span class="sxs-lookup"><span data-stu-id="61224-226">[Microsoft Patterns and Practices - Federated Identity Pattern](https://msdn.microsoft.com/library/dn589790.aspx).</span></span>
- <span data-ttu-id="61224-227">[如何： 安裝 Azure Active Directory 同步作業工具](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)。</span><span class="sxs-lookup"><span data-stu-id="61224-227">[HowTo: Install the Azure Active Directory Sync Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span></span>
- <span data-ttu-id="61224-228">[Active Directory Federation Services 2.0 內容地圖](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)。</span><span class="sxs-lookup"><span data-stu-id="61224-228">[Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span></span> <span data-ttu-id="61224-229">ADFS 2.0 的相關文件的連結。</span><span class="sxs-lookup"><span data-stu-id="61224-229">Links to documentation about ADFS 2.0.</span></span>
- <span data-ttu-id="61224-230">[在 Windows Azure AD 應用程式中的角色型和 ACL 型授權](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1)。</span><span class="sxs-lookup"><span data-stu-id="61224-230">[Role-Based and ACL-Based Authorization in a Windows Azure AD Application](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span></span> <span data-ttu-id="61224-231">範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="61224-231">Sample application.</span></span>
- <span data-ttu-id="61224-232">[Azure Active Directory Graph API 部落格](https://blogs.msdn.com/b/aadgraphteam/)。</span><span class="sxs-lookup"><span data-stu-id="61224-232">[Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).</span></span>
- <span data-ttu-id="61224-233">[存取控制在 BYOD 和混合式身分識別基礎結構中的目錄整合](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=)。</span><span class="sxs-lookup"><span data-stu-id="61224-233">[Access Control in BYOD and Directory Integration in a Hybrid Identity Infrastructure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span></span> <span data-ttu-id="61224-234">技術 Ed 2014 工作階段視訊 Gayana Bagdasaryan。</span><span class="sxs-lookup"><span data-stu-id="61224-234">Tech Ed 2014 session video by Gayana Bagdasaryan.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61224-235">[上一頁](web-development-best-practices.md)
> [下一頁](data-storage-options.md)</span><span class="sxs-lookup"><span data-stu-id="61224-235">[Previous](web-development-best-practices.md)
[Next](data-storage-options.md)</span></span>
