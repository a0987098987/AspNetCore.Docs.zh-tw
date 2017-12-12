---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: "加入 ASP.NET Identity 至空的或現有的 Web Form 專案 |Microsoft 文件"
author: raquelsa
description: "本教學課程會示範如何將 ASP.NET Identity （新的成員資格系統適用於 ASP.NET） 加入至 ASP.NET 應用程式。 當您建立新的 Web Form 或 MVC..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: f5783287a26174ddf65bb0eae34c347831d02c47
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>加入 ASP.NET Identity 至空的或現有的 Web Form 專案
====================
由[Raquel Soares De Almeida](https://github.com/raquelsa)

> 本教學課程會示範如何加入[ASP.NET Identity](introduction-to-aspnet-identity.md) （新的成員資格系統適用於 ASP.NET） 的 ASP.NET 應用程式。
> 
> 當您建立新的 Web Form 或 MVC 專案在 Visual Studio 2013 RTM 與個別帳戶時，Visual Studio 會安裝所有必要的封裝，並為您新增所有必要的類別。 本教學課程將說明如何將 ASP.NET 識別身分支援加入至現有的 Web Form 專案或新的空白專案。 我會列出所有安裝，您需要的 NuGet 封裝和您要新增的類別。 我將介紹範例 Web Form 註冊新的使用者和登入時反白顯示所有的主要進入點 Api，管理使用者和驗證。 這個範例將使用 SQL 資料存放區為基礎的 Entity Framework 所建立的 ASP.NET Identity 的預設實作。 本教學課程中，我們將使用 LocalDB 的 SQL 資料庫。
> 
> 本教學課程中所編寫的 Raquel Soares De Almeida 和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="getting-started-aspnet-identity"></a>開始使用 ASP.NET Identity

1. 開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。
2. 按一下**新專案**從開始頁面上，或者您可以使用功能表，然後選取**檔案**，然後**新專案**。
3. 選取**Visual C# i** n 的左窗格，然後**Web** ，然後選取  **ASP.NET Web 應用程式**。 將您的專案"WebFormsIdentity"，再按一下**確定**。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. 在**新增 ASP.NET 專案**對話方塊中，選取**空**範本。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
 請注意**變更驗證**按鈕已停用，並提供此範本中沒有驗證支援。 Web Form、 MVC 和 Web API 範本可讓您選取驗證方法。 如需詳細資訊，請參閱[概觀的驗證](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。

## <a name="adding-identity-packages-to-your-app"></a>識別封裝加入您的應用程式

在 [方案總管] 中，以滑鼠右鍵按一下您的專案，然後選取**管理 NuGet 封裝**。 在 搜尋文字 方塊 對話方塊中，輸入 「*Identity.E*"。 按一下 安裝此套件。   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
請注意，此套件會安裝相依性套件： EntityFramework 和 Microsoft ASP.NET Identity Core。

## <a name="adding-web-forms-to-register-users"></a>若要註冊的使用者加入的 Web Form

1. 在**方案總管 中**，以滑鼠右鍵按一下您的專案，然後按一下**新增**，然後**Web Form**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. 在**指定項目的名稱**對話方塊中，命名新的 web form**註冊**，然後按一下  **確定**
3. 取代所產生的標記*Register.aspx*下列程式碼檔案。 程式碼變更會反白顯示。   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > 這是只的簡化的版*Register.aspx*當您建立新的 ASP.NET Web Form 專案時所建立的檔案。 上述的標記將表單欄位並註冊新的使用者 按鈕。
4. 開啟*Register.aspx.cs*檔案，然後以下列程式碼取代檔案的內容：

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. 上述程式碼為概要*Register.aspx.cs*當您建立新的 ASP.NET Web Form 專案時所建立的檔案。
    > 2. *IdentityUser*類別是預設 EntityFramework 實作*IUser*介面。 *IUser*介面是基本的 ASP.NET Identity Core 上的使用者介面。
    > 3. *UserStore*類別是使用者存放區的預設 EntityFramework 實作。 這個類別會實作 ASP.NET Identity Core 最少的介面： *IUserStore*， *IUserLoginStore*， *IUserClaimStore*和*IUserRoleStore*.
    > 4. *UserManager*類別公開使用者相關的 Api，其會自動儲存的變更*UserStore*。
    > 5. *IdentityResult*類別表示識別作業的結果。
5. 在**方案總管 中**，以滑鼠右鍵按一下您的專案，然後按一下**新增**，**加入 ASP.NET 資料夾**然後**應用程式\_資料**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. 開啟*Web.config*檔案，並加入我們將用來儲存使用者資訊之資料庫的連接字串項目。 將會建立此資料庫在執行階段由 EntityFramework 識別實體。 當您建立新的 Web Form 專案時，為您建立的其中一個相似的連接字串。 反白顯示的程式碼示範標記，您應該將新增：

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Visual Studio 2015 或更高版本，取代`(localdb)\v11.0`與`(localdb)\MSSQLLocalDB`連接字串中。
    
7. 以滑鼠右鍵按一下檔案*Register.aspx*在您的專案並選取**設定為起始頁**。 按 Ctrl + F5 以建置並執行 web 應用程式。 輸入新的使用者名稱和密碼，然後按一下 上**註冊**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET 識別進行驗證的支援，與在此範例中，您可以驗證使用者名稱和密碼的預設行為是來自身分識別核心封裝的驗證程式。 使用者的預設驗證程式 (`UserValidator`) 具有屬性`AllowOnlyAlphanumericUserNames`具有預設值設定為`true`。 密碼的預設驗證程式 (`MinimumLengthValidator`) 可確保該密碼已經至少 6 個字元。 這些驗證程式位於屬性`UserManager`，會覆寫如果您想要有自訂的驗證

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>驗證 LocalDb 識別資料庫和 Entity Framework 所產生的資料表

1. 在**檢視**功能表上，按一下 **伺服器總管**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. 展開**DefaultConnection (WebFormsIdentity)**，依序展開**資料表**，以滑鼠右鍵按一下**AspNetUsers**按一下**顯示資料表資料**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>設定 OWIN 驗證的應用程式

此時我們只新增了支援以建立使用者。 現在，我們將示範如何我們新增登入使用者的驗證。 ASP.NET Identity 表單驗證使用 Microsoft OWIN 驗證中介軟體。 OWIN 的 Cookie 驗證 cookie 和宣告型的驗證機制，可供任何架構上裝載[OWIN](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx)或 IIS。 使用這個模型，相同的驗證封裝可以用於跨多個架構，包含 ASP.NET MVC 和 Web Form。 如需有關專案 Katana 和如何執行中介軟體中主機無從驗證，請參閱[入門 Katana 專案](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx)。

## <a name="installing-authentication-packages-to-your-application"></a>驗證封裝安裝至您的應用程式

1. 在 [方案總管] 中，以滑鼠右鍵按一下您的專案，然後選取**管理 NuGet 封裝**。 在 搜尋文字 方塊 對話方塊中，輸入 「*Identity.Owin*"。 按一下 安裝此套件。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. 搜尋封裝***Microsoft.Owin.Host.SystemWeb***並安裝它。   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin**封裝包含一組管理與設定 OWIN 驗證中介軟體可供 ASP.NET Identity Core 套件 OWIN 擴充功能類別。  
    > **Microsoft.Owin.Host.SystemWeb**封裝包含 OWIN 伺服器，可讓 OWIN 應用程式，以使用 ASP.NET 要求管線在 IIS 上執行。 如需詳細資訊，請參閱[OWIN 中介軟體在 IIS 中整合管線](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md)。

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>新增 OWIN 啟動 「 和驗證組態類別

1. 在**方案總管 中**，以滑鼠右鍵按一下您的專案，按一下**新增**，然後**加入新項目**。 在 搜尋文字 方塊 對話方塊中，輸入 「*owin*"。 將類別 」*啟動*「 按一下**新增**。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. 在 Startup.cs 檔案中，將反白顯示程式碼來設定 OWIN 的 cookie 驗證，如下所示。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > 這個類別包含`OwinStartup`指定 OWIN 啟動類別的屬性。 每個 OWIN 應用程式已啟動類別讓您指定的應用程式管線元件。 請參閱[OWIN 啟動類別偵測](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)如需詳細資訊，此模型上。

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>加入註冊和登入使用者的 Web Form

1. 開啟*Register.cs*檔案，然後加入下列程式碼的註冊成功時，將登入使用者。 下面反白顯示所做的變更。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - 架構，因為 ASP.NET Identity 和 OWIN 的 Cookie 驗證是宣告式系統，需要產生應用程式開發人員[ClaimsIdentity](https://msdn.microsoft.com/en-us/library/microsoft.identitymodel.claims.claimsidentity.aspx)使用者。 身分識別的所有宣告的使用者，例如使用者所屬的角色的相關資訊。 您也可以在這個階段中加入更多的使用者宣告。
    > - 您可以使用登入使用者呼叫與 OWIN AuthenticationManager`SignIn`並傳入 ClaimsIdentity，如上所示。 這段程式碼將會在使用者登入，並產生以及 cookie。 這個呼叫是類似於[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx)供[FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx)模組。
2. 在**方案總管 中**，以滑鼠右鍵按一下專案按一下**新增**，然後**Web Form**。 Web 表單**登入**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. 取代內容*Login.aspx*以下列程式碼檔案：  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. 取代內容*Login.aspx.cs*以下列檔案：  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load`現在會檢查目前使用者的狀態，並且會根據其`Context.User.Identity.IsAuthenticated`狀態。  
    >     **在 使用者名稱顯示間隔**: Microsoft ASP.NET Identity Framework 已加入的擴充方法上[System.Security.Principal.IIdentity](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.aspx) ，可讓您取得`UserName`和`UserId`的登入的使用者。 這些擴充方法中定義`Microsoft.AspNet.Identity.Core`組件。 這些擴充方法會取代[HttpContext.User.Identity.Name](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.user.aspx) 。
    > - 登入方法：   
    >     `This`方法會取代先前`CreateUser_Click`方法，在這個範例和現在已成功建立使用者之後，在使用者的符號。   
    >  Microsoft OWIN 架構有加入擴充方法上`System.Web.HttpContext`，可讓您取得的參考`IOwinContext`。 這些擴充方法中定義`Microsoft.Owin.Host.SystemWeb`組件。 `OwinContext`類別會公開`IAuthenticationManager`屬性，代表目前要求中可取的驗證中介軟體功能。  
    >  您可以使用登入使用者`AuthenticationManager`OWIN 並呼叫從`SignIn`和傳入`ClaimsIdentity`如上所示。   
    >  因為 ASP.NET Identity 和 OWIN 的 Cookie 驗證以宣告為基礎的系統，架構都需要應用程式產生`ClaimsIdentity`使用者。   
    >  `ClaimsIdentity`有使用者，例如使用者所屬的角色的所有宣告的相關資訊。 您也可以在這個階段新增更多的使用者宣告  
    >  這段程式碼將會在使用者登入，並產生以及 cookie。 這個呼叫是類似於[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx)供[FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx)模組。
    > - `SignOut`方法：   
    >  取得參考`AuthenticationManager`OWIN 並呼叫從`SignOut`。 這是類似於[FormsAuthentication.SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx)所使用的方法[FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx)模組。
5. 按**Ctrl + F5**建置並執行 web 應用程式。 輸入新的使用者名稱和密碼，然後按一下 上**註冊**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
 注意： 此時，新的使用者建立並登入。
6. 按一下**登出** 按鈕。安裝程式會重新導向至表單中的記錄檔。
7. 輸入使用者名稱無效或密碼並按一下上**登入** 按鈕。   
 `UserManager.Find`方法會傳回 null，且錯誤訊息: 「*無效的使用者名稱或密碼*」 將會顯示。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
