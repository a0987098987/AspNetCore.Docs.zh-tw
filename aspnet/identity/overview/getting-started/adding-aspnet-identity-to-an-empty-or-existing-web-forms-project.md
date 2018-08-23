---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: 新增 ASP.NET Identity 新增至空的或現有的 Web Form 專案 |Microsoft Docs
author: raquelsa
description: 本教學課程會示範如何將 ASP.NET Identity （新成員資格系統針對 ASP.NET） 新增至 ASP.NET 應用程式。 當您建立新的 Web Form 或 MVC...
ms.author: riande
ms.date: 10/23/2013
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 229d6fef5aa9c2384b6d92ec3e3ed7316b69afe0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831187"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>將 ASP.NET Identity 新增至空的或現有的 Web Form 專案
====================
藉由[Raquel Soares De Almeida](https://github.com/raquelsa)

> 本教學課程會示範如何新增[ASP.NET Identity](introduction-to-aspnet-identity.md) （新成員資格系統針對 ASP.NET） 的 ASP.NET 應用程式。
> 
> 當您建立新的 Web Form 或 MVC 專案在 Visual Studio 2013 RTM 使用個別的帳戶時，Visual Studio 會安裝所有必要的套件，並為您新增所有必要的類別。 本教學課程將說明如何將 ASP.NET 身分識別支援新增至您現有的 Web Form 專案或新的空白專案。 我將概述所有的 NuGet 套件，必須先安裝和您要新增的類別。 我將介紹範例 Web Form，註冊新的使用者與登入，同時醒目提示 使用者管理與驗證的所有主要進入點 Api。 此範例會建立在 Entity Framework 上的 SQL 資料儲存體使用 ASP.NET Identity 的預設實作。 本教學課程中，我們將使用 LocalDB，SQL database。
> 
> 本教學課程中所編寫的 Raquel Soares De Almeida 和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="getting-started-aspnet-identity"></a>開始使用 ASP.NET 身分識別

1. 開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。
2. 按一下 **新的專案**從 開始 頁面上，或者您可以使用功能表並選取**檔案**，然後**新專案**。
3. 選取  **Visual C# i** n 左窗格中，然後**Web** ，然後選取**ASP.NET Web 應用程式**。 命名您的專案 「 WebFormsIdentity"，然後按一下**確定**。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. 在 **新的 ASP.NET 專案**對話方塊中，選取**空白**範本。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   請注意**變更驗證**按鈕已停用，並驗證不提供任何支援此範本中。 Web Form、 MVC 和 Web API 範本可讓您選取驗證方法。 如需詳細資訊，請參閱 <<c0> [ 驗證的概觀](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。

## <a name="adding-identity-packages-to-your-app"></a>將身分識別的套件新增至您的應用程式

在 [方案總管] 中，以滑鼠右鍵按一下您的專案，然後選取**管理 NuGet 套件**。 在 搜尋文字 方塊 對話方塊中，輸入 「*Identity.E*"。 按一下 安裝此套件。   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
請注意，此套件會安裝相依性套件： EntityFramework 和 Microsoft Asp.net 身分識別。

## <a name="adding-web-forms-to-register-users"></a>新增註冊使用者的 Web Form

1. 在**方案總管**，以滑鼠右鍵按一下您的專案，然後按一下**新增**，然後**Web Form**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. 在 **指定項目的名稱** 對話方塊中，名稱為新的 web form**註冊**，然後按一下  **確定**
3. 取代所產生的標記*Register.aspx*下列程式碼的檔案。 程式碼變更已醒目提示。   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > 這是只是一個簡化的版的*Register.aspx*當您建立新的 ASP.NET Web Form 專案建立的檔案。 上述標記會將表單欄位和按鈕，以註冊新的使用者。
4. 開啟*Register.aspx.cs*檔案，並以下列程式碼取代檔案的內容：

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. 上述程式碼是一個簡化的版的*Register.aspx.cs*當您建立新的 ASP.NET Web Form 專案建立的檔案。
    > 2. *IdentityUser*類別是預設 EntityFramework 實作*IUser*介面。 *IUser*介面是在 Asp.net 身分識別使用者的基本介面。
    > 3. *UserStore*類別是使用者存放區的預設 EntityFramework 實作。 這個類別會實作 ASP.NET 身分識別 Core 的最少的介面： *IUserStore*， *IUserLoginStore*， *IUserClaimStore*並*IUserRoleStore*.
    > 4. *UserManager*類別會公開使用者相關的應用程式開發介面會自動將變更儲存到*UserStore*。
    > 5. *IdentityResult*類別表示識別作業的結果。
5. 在 **方案總管**，以滑鼠右鍵按一下您的專案，然後按一下**新增**，**加入 ASP.NET 資料夾**，然後**應用程式\_資料**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. 開啟*Web.config*檔案，並加入我們將用來儲存使用者資訊的資料庫的連接字串項目。 資料庫會建立在執行階段由 EntityFramework 為身分識別的實體。 連接字串是類似當您建立新的 Web Form 專案，為您建立的其中一項。 反白顯示的程式碼顯示標記，您應該加入：

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Visual Studio 2015 或更新版本中，取代`(localdb)\v11.0`與`(localdb)\MSSQLLocalDB`連接字串中。
    
7. 以滑鼠右鍵按一下檔案*Register.aspx*在您的專案，然後選取**設定為起始頁**。 按 Ctrl + F5 以建置並執行 web 應用程式。 輸入新的使用者名稱和密碼，然後按一下**註冊**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET 身分識別已驗證的支援，並在此範例中，您可以確認使用者名稱和密碼的預設行為是來自身分識別核心封裝的驗證程式。 使用者的預設驗證程式 (`UserValidator`) 都具有屬性`AllowOnlyAlphanumericUserNames`具有預設值設定為`true`。 密碼的預設驗證程式 (`MinimumLengthValidator`) 可確保該密碼必須至少 6 個字元。 這些驗證器位於屬性`UserManager`，可以覆寫如果您想要有自訂的驗證，

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>正在驗證的 LocalDb 識別資料庫和 Entity Framework 所產生的資料表

1. 在 **檢視**功能表上，按一下**伺服器總管**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. 依序展開**DefaultConnection (WebFormsIdentity)**，展開**資料表**，以滑鼠右鍵按一下**AspNetUsers**然後按一下**顯示資料表資料**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>設定 OWIN 驗證的應用程式

此時我們還只支援建立使用者。 現在，我們要示範我們要如何新增登入使用者的驗證。 ASP.NET 身分識別使用表單驗證的 Microsoft OWIN 驗證中介軟體。 OWIN 的 Cookie 驗證 cookie，並宣告型的驗證機制，可供裝載於任何架構[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)或 IIS。 此模型中，使用相同的驗證封裝可用於多個架構，包括 ASP.NET MVC 和 Web Form。 如需有關專案 Katana，以及如何執行主機無從驗證，請參閱在的中介軟體[Getting Started with Katana 專案](https://msdn.microsoft.com/magazine/dn451439.aspx)。

## <a name="installing-authentication-packages-to-your-application"></a>驗證封裝安裝至您的應用程式

1. 在 [方案總管] 中，以滑鼠右鍵按一下您的專案，然後選取**管理 NuGet 套件**。 在 搜尋文字 方塊 對話方塊中，輸入 「*Identity.Owin*"。 按一下 安裝此套件。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. 搜尋封裝***Microsoft.Owin.Host.SystemWeb***並安裝它。   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin**套件包含一組來管理和設定 OWIN 驗證中介軟體可供 Asp.net 身分識別封裝的 OWIN 擴充功能類別。  
    > **Microsoft.Owin.Host.SystemWeb**套件包含 OWIN 伺服器，可讓 OWIN 型應用程式使用 ASP.NET 要求管線在 IIS 上執行。 如需詳細資訊，請參閱[OWIN 中介軟體，在 IIS 整合式管線](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md)。

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>新增 OWIN 啟動和驗證組態類別

1. 在**方案總管**，以滑鼠右鍵按一下您的專案，按一下**新增**，然後**加入新項目**。 在 搜尋文字 方塊 對話方塊中，輸入 「*owin*"。 將類別命名為"*啟始*」，按一下 **新增**。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. 在 Startup.cs 檔案中新增醒目提示的程式碼來設定 OWIN 的 cookie 驗證，如下所示。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > 這個類別包含`OwinStartup`指定 OWIN 啟動類別的屬性。 每個 OWIN 應用程式有啟動類別，您可在其中指定應用程式管線的元件。 請參閱[OWIN 啟動類別偵測](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)如需詳細資訊，此模型上。

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>新增註冊和登入使用者的 Web Form

1. 開啟*Register.cs*檔案，並新增下列程式碼註冊成功時，將登入使用者。 下面反白顯示所做的變更。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - 由於 ASP.NET 身分識別和 OWIN Cookie 驗證是宣告式系統，架構會要求應用程式開發人員產生[ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx)使用者。 ClaimsIdentity 有的使用者，例如使用者屬於哪些角色的所有宣告的相關資訊。 您也可以在這個階段中新增更多的使用者宣告。
    > - 您可以使用登入使用者從 OWIN 及呼叫 AuthenticationManager`SignIn`並傳入 ClaimsIdentity，如上所示。 此程式碼將會登入使用者，並產生以及 cookie。 這個呼叫是類似[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx)供[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)模組。
2. 在 [**方案總管] 中**，以滑鼠右鍵按一下您的專案按一下**新增**，，然後**Web Form**。 Web 表單命名**登入**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. 內容取代*Login.aspx*為下列程式碼的檔案：  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. 內容取代*Login.aspx.cs*以下列檔案：  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load`現在會檢查目前使用者的狀態，並且會根據其`Context.User.Identity.IsAuthenticated`狀態。  
    >     **顯示已登入的使用者名稱**: Microsoft ASP.NET 身分識別架構上加入擴充方法[System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) ，可讓您取得`UserName`和`UserId`的登入的使用者。 這些擴充方法定義中`Microsoft.AspNet.Identity.Core`組件。 這些擴充方法會取代[HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) 。
    > - 登入方法：   
    >     `This` 方法會取代先前`CreateUser_Click`在此範例，並立即登入的使用者已成功建立使用者之後的方法。   
    >  Microsoft OWIN 架構上加入擴充方法`System.Web.HttpContext`，可讓您取得的參考`IOwinContext`。 這些擴充方法定義中`Microsoft.Owin.Host.SystemWeb`組件。 `OwinContext`類別會公開`IAuthenticationManager`屬性，表示目前要求上可用的驗證中介功能。  
    >  您可以使用登入的使用者`AuthenticationManager`OWIN 和呼叫`SignIn`並傳入`ClaimsIdentity`如上所示。   
    >  ASP.NET 身分識別和 OWIN 的 Cookie 驗證是宣告為基礎的系統，因為架構需要應用程式來產生`ClaimsIdentity`使用者。   
    >  `ClaimsIdentity`已針對使用者，例如使用者屬於何種角色的所有宣告的相關資訊。 您也可以在這個階段新增更多的使用者宣告  
    >  此程式碼將會登入使用者，並產生以及 cookie。 這個呼叫是類似[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx)供[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)模組。
    > - `SignOut` 方法：   
    >  取得參考`AuthenticationManager`OWIN 和呼叫`SignOut`。 這相當於[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)所使用的方法[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)模組。
5. 按下**Ctrl + F5**建置及執行 web 應用程式。 輸入新的使用者名稱和密碼，然後按一下**註冊**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   注意： 此時，新的使用者建立並登入。
6. 按一下 [**登出**] 按鈕。您將會重新導向至登入表單。
7. 輸入使用者名稱無效或密碼並按一下**登入** 按鈕。   
   `UserManager.Find`方法會傳回 null，且錯誤訊息: 「*無效的使用者名稱或密碼*」 將會顯示。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
