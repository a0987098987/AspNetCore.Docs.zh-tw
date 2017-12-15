---
title: "在 ASP.NET Core 上的識別簡介"
author: rick-anderson
description: "使用身分識別與 ASP.NET Core 應用程式"
keywords: "ASP.NET Core，身分識別授權安全性"
ms.author: riande
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 7daf0267a6dc659afbd188ce87e35ca40816a31d
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a>在 ASP.NET Core 上的識別簡介

由[Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，Jon Galloway [Erik Reitan](https://github.com/Erikre)，和[Steve Smith](https://ardalis.com/)

ASP.NET Core 身分識別是可讓您登入功能加入您的應用程式的成員資格系統。 使用者可以建立帳戶和登入的使用者名稱和密碼，或者也可以使用例如 Facebook、 Google、 Microsoft 帳戶、 Twitter 或其他外部登入提供者。

您可以設定要用來儲存使用者名稱、 密碼及分析資料的 SQL Server 資料庫的 ASP.NET 核心身分識別。 或者，您可以使用您自己的持續性存放區，例如 Azure 資料表儲存體。 本文件包含適用於 Visual Studio 以及使用 CLI 的指示。

## <a name="overview-of-identity"></a>身分識別的概觀

本主題中，您將了解如何使用 ASP.NET Core 身分加入的功能，以註冊、 登入，並登出使用者。 如需使用 ASP.NET Core 識別建立應用程式的詳細指示，請參閱本文結尾處的後續步驟 > 一節。

1.  建立 ASP.NET Core Web 應用程式專案與個別使用者帳戶。

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)
    在 Visual Studio 中，選取**檔案** -> **新增** -> **專案**。 選取**ASP.NET Web 應用程式**從**新專案** 對話方塊。 選取 ASP.NET Core **Web Application(Model-View-Controller)**適用於 ASP.NET Core 與 2.x**個別使用者帳戶**做為驗證方法。

    注意： 您必須選取**個別使用者帳戶**。
 
    ![[新增專案] 對話](identity/_static/01-mvc_2.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)
    如果使用.NET 核心 CLI，請建立新的專案使用``dotnet new mvc --auth Individual``。 此命令會建立新的專案與 Visual Studio 建立的相同身分識別範本程式碼。
 
    建立的專案包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`封裝，它會保存身分資料和 SQL Server 使用的結構描述[Entity Framework Core](https://docs.microsoft.com/ef/)。
    
    ---
 
2.  設定身分識別服務，並將新增中的介軟體中`Startup`。

    識別服務會加入至應用程式中`ConfigureServices`方法中的`Startup`類別：

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    這些服務會提供給應用程式透過[相依性插入](xref:fundamentals/dependency-injection)。
    
    識別已啟用應用程式藉由呼叫`UseAuthentication`中`Configure`方法。 `UseAuthentication`加入驗證[中介軟體](xref:fundamentals/middleware)要求管線。
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    這些服務會提供給應用程式透過[相依性插入](xref:fundamentals/dependency-injection)。
    
    識別已啟用應用程式藉由呼叫`UseIdentity`中`Configure`方法。 `UseIdentity`新增 cookie 基本驗證[中介軟體](xref:fundamentals/middleware)要求管線。
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    如需處理程序的應用程式啟動的詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。

3.  建立使用者。

    啟動應用程式，然後按一下**註冊**連結。

    如果這是您要執行此動作的第一次，您可能需要執行移轉。 應用程式會提示您**套用移轉**:
    
    ![適用於移轉 Web 網頁](identity/_static/apply-migrations.png)
    
    或者，您可以測試沒有持續性資料庫的應用程式使用 ASP.NET Core 身分識別，使用記憶體中資料庫。 若要使用記憶體中資料庫時，將``Microsoft.EntityFrameworkCore.InMemory``封裝到您的應用程式，並修改您的應用程式呼叫``AddDbContext``中``ConfigureServices``，如下所示：

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    當使用者按一下**註冊**連結，``Register``上叫用動作``AccountController``。 ``Register``動作會建立使用者藉由呼叫`CreateAsync`上`_userManager`物件 (提供給``AccountController``的相依性插入):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    如果已成功建立使用者，使用者會登入的呼叫所``_signInManager.SignInAsync``。

    **注意：**看到[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)的步驟，以避免立即註冊登入。
 
4.  登入。
 
    使用者可以登入，依序按一下**登入**頂端的站台連結或它們可能瀏覽至登入頁面當他們嘗試存取需要的授權站台的一部分。 當使用者提交表單的登入頁面上， ``AccountController`` ``Login``動作呼叫。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    ``Login``動作呼叫``PasswordSignInAsync``上``_signInManager``物件 (提供給``AccountController``的相依性插入)。
 
    基底``Controller``類別會公開``User``從控制器方法可以存取的屬性。 比方說，您可以列舉`User.Claims`和進行授權決策。 如需詳細資訊，請參閱[授權](xref:security/authorization/index)。
 
5.  登出。
 
    按一下**登出**連結呼叫`LogOut`動作。
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    上述程式碼中的呼叫上述`_signInManager.SignOutAsync`方法。 `SignOutAsync`方法會清除儲存在 cookie 中的使用者的宣告。
 
6.  組態設定。

    識別有一些您可以在您的應用程式啟動類別中覆寫的預設行為。 您不需要設定``IdentityOptions``如果您使用的預設行為。

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    如需如何設定身分識別的詳細資訊，請參閱[設定身分識別](xref:security/authentication/identity-configuration)。
    
    您也可以設定資料類型的主索引鍵，請參閱[設定識別主索引鍵資料類型](xref:security/authentication/identity-primary-key-configuration)。
 
7.  檢視的資料庫。

    如果您的應用程式使用 SQL Server 資料庫 （預設值在 Windows 上，以及適用於 Visual Studio 使用者），您可以檢視資料庫建立的應用程式。 您可以使用**SQL Server Management Studio**。 或者，從 Visual Studio 中，選取**檢視** -> **SQL Server 物件總管**。 連接到**(localdb) \MSSQLLocalDB**。 資料庫名稱符合 **aspnet-<*的專案名稱*>-<*日期字串*> * * 隨即出現。

    ![AspNetUsers 資料庫資料表上的內容功能表](identity/_static/04-db.png)
    
    展開的資料庫及其**資料表**，然後以滑鼠右鍵按一下**dbo。AspNetUsers**資料表，然後選取**檢視資料**。

## <a name="identity-components"></a>身分識別的元件

識別系統的主要參考組件是`Microsoft.AspNetCore.Identity`。 此套件包含一組核心介面 ASP.NET Core 身分識別，而且會包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。

可用於 ASP.NET Core 應用程式的身分識別系統，會需要這些相依性：

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-包含必要的型別，以搭配 Entity Framework Core 的身分識別。

* `Microsoft.EntityFrameworkCore.SqlServer`實體架構的核心是 Microsoft 的建議的資料存取技術，例如 SQL Server 關聯式資料庫。 為了測試，您可以使用`Microsoft.EntityFrameworkCore.InMemory`。

* `Microsoft.AspNetCore.Authentication.Cookies`中介軟體，可讓應用程式使用 cookie 型驗證。

## <a name="migrating-to-aspnet-core-identity"></a>移轉至 ASP.NET Core 身分識別

針對其他資訊和指引移轉您現有的身分識別存放區，請參閱[移轉的驗證和身分識別](xref:migration/identity)。

## <a name="next-steps"></a>後續步驟

* [移轉驗證和身分識別](xref:migration/identity)
* [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [使用 SMS 的雙因素驗證](xref:security/authentication/2fa)
* [使用 Facebook、Google 和其他外部提供者啟用驗證](xref:security/authentication/social/index)
