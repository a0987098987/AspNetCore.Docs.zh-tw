---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: 將通用的提供者的資料移轉成員資格和使用者設定檔，以 ASP.NET Identity (C#) |Microsoft 文件
author: rustd
description: 本教學課程中說明的步驟所需移轉使用者和角色的資料，以及使用通用的提供者的現有應用程式所建立的使用者設定檔資料...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f65f93b20543d06ea70a9009b6921e297477c99e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871567"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>將通用的提供者的資料移轉成員資格和使用者設定檔，以 ASP.NET Identity (C#)
====================
由[Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Robert McMurray](https://github.com/rmcmurray)， [Suhas Joshi](https://github.com/suhasj)

> 本教學課程描述所移轉使用者和角色的資料，以及使用通用的提供者的現有 ASP.NET 身分識別模型的應用程式所建立的使用者設定檔資料所需的步驟。 方法此處提到移轉使用者設定檔資料可以使用 SQL 成員資格的應用程式中。


使用 Visual Studio 2013 版本，ASP.NET 團隊導入新的 ASP.NET 識別系統，並閱讀更多關於該版本[這裡](../../index.md)。 要從 web 應用程式移轉的發行項上後續[SQL 成員資格，才能在新的身分識別系統](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)，本文說明如何移轉現有的應用程式，請依照下列使用者和角色管理的提供者模型的步驟加入新的身分識別模型。 本教學課程的焦點會在移轉使用者設定檔資料，來順暢地將它連結到新的系統。 移轉使用者和角色的資訊是類似 SQL 的成員資格。 遵循移轉設定檔資料的方法可以使用 SQL 成員資格的應用程式中。

例如，我們將開始使用 Visual Studio 2012 中使用的提供者模型所建立的 web 應用程式。 我們將則加入管理設定檔的程式碼、 註冊的使用者、 新增使用者的設定檔資料、 移轉資料庫結構描述，然後變更應用程式使用的識別系統的使用者和角色管理。 移轉的測試，以使用通用的提供者所建立的使用者應該能夠登入和新的使用者應該能夠註冊。

> [!NOTE]
> 您可以找到完整的範例： [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations)。


## <a name="profile-data-migration-summary"></a>設定檔資料移轉摘要

開始之前的移轉，讓我們看看設定檔資料儲存提供者模型中的體驗。 使用者可以儲存多種方式可以在應用程式的設定檔資料，其中最常見正在使用通用的提供者以及隨附的內建的設定檔提供者。 這些步驟會包含

1. 加入具有屬性用來儲存設定檔資料的類別。
2. 加入擴充 'ProfileBase' 和實作方法來取得使用者的上述的設定檔資料的類別。
3. 使用預設設定檔提供者中的啟用*web.config*檔案，並在步驟 2 以用於存取設定檔資訊中宣告的類別定義。

設定檔資訊會儲存為序列化的 xml 和資料庫中的 '的設定檔 」 資料表中的二進位資料。

在移轉之後使用新的 ASP.NET Identity 系統應用程式，會還原序列化的設定檔資訊，並將其儲存為使用者類別上的屬性中。 每個屬性接著可以對應到使用者資料表中的資料行。 此處的優點是屬性即可直接使用除了不需要序列化/還原序列化資料資訊每個使用者類別存取它的時間。

## <a name="getting-started"></a>快速入門

1. 在 Visual Studio 2012 中建立新的 ASP.NET 4.5 Web Form 應用程式。 目前的範例使用 Web Form 範本，但您也可以使用 MVC 應用程式。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. 建立新的資料夾 '模型' 儲存設定檔資訊  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. 例如，讓我們儲存日期的生日、 縣 （市）、 高度和寬度的使用者設定檔中。 高度和寬度會儲存為自訂類別，稱為 'PersonalStats' 中。 若要儲存和擷取的設定檔，我們需要可擴充 'ProfileBase' 的類別。 讓我們來建立新的類別 'AppProfile' 以取得並儲存設定檔資訊。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. 啟用設定檔中的*web.config*檔案。 輸入要用於儲存/擷取使用者資訊在步驟 3 中建立的類別名稱。

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. 取得使用者的設定檔資料，並將其儲存的 'Account' 資料夾中新增的 web form 頁面。 以滑鼠右鍵按一下專案並選取 加入新項目'。 加入新 webforms 網頁主版頁面 'AddProfileData.aspx'。 複製下列 'MainContent' 區段中：

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   在程式碼中加入下列程式碼：

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   加入命名空間下的 AppProfile 類別定義移除編譯錯誤。
6. 執行應用程式，並以使用者名稱建立新的使用者 '**olduser'。** 巡覽至 'AddProfileData' 頁面，並新增使用者設定檔資訊。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

您可以驗證的資料會儲存為序列化的 xml，使用 [伺服器總管] 視窗 '的設定檔 」 資料表中。 在 Visual Studio 中，從 [檢視] 功能表上，選擇 [伺服器總管] '。 應該是資料庫中定義的資料連線*web.config*檔案。 按一下即可在資料連線顯示不同的子類別目錄。 展開以顯示不同的資料表在資料庫中，'設定檔] 上按一下滑鼠右鍵然後選擇 [檢視設定檔資料儲存在設定檔資料表中的 「 顯示資料表資料 」 的 「 資料表 」。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>移轉資料庫結構描述

若要讓現有的資料庫搭配使用識別身分系統，我們需要更新的結構描述中識別資料庫，以支援我們加入至原始資料庫的欄位。 這可以使用 SQL 指令碼來建立新的資料表，並將複製現有的資訊。 在 [伺服器總管] 視窗中，展開 'DefaultConnection' 顯示資料表。 以滑鼠右鍵按一下資料表，然後選取 [新增查詢]

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

貼上的 SQL 指令碼，從[ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt)並執行它。 如果重新整理 'DefaultConnection' 時，我們可以看到新資料表隨即加入。 您可以檢查以查看已移轉的資訊的資料表內的資料。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>應用程式移轉至使用 ASP.NET Identity

1. 安裝所需的 ASP.NET Identity 的 Nuget 封裝：

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   在 管理 Nuget 封裝的詳細資訊可以找到[這裡](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. 若要使用資料表中的現有資料，我們需要建立模型類別將對應至資料表，並將它們連結身分識別系統中。 識別合約的一部分，模型類別應實作 Identity.Core dll 中定義的介面，或者也可以擴充現有 Microsoft.AspNet.Identity.EntityFramework 用於這些介面的實作。 我們會針對角色、 使用者登入和使用者宣告使用現有的類別。 我們需要自訂的使用者使用我們的範例。 以滑鼠右鍵按一下專案，並建立新資料夾 'IdentityModels'。 加入新的 「 使用者 」 類別，如下所示：

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   請注意，'ProfileInfo' 現在是使用者類別上的屬性。 因此，我們可以直接使用設定檔資料使用的使用者類別。

在將檔案複製**IdentityModels**和**IdentityAccount**下載來源的資料夾 ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) )。 這些有剩餘的模型類別和新使用者和角色管理使用 ASP.NET 識別應用程式開發介面所需的頁面。 可以找到詳細的說明和使用的方法是類似 SQL 的成員資格[這裡](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)。

## <a name="copying-profile-data-to-the-new-tables"></a>將設定檔資料複製到新的資料表

如先前所述，我們需要還原序列化設定檔資料表中的 xml 資料，並將它儲存在 AspNetUsers 資料表的資料行。 讓剩下的是，填入資料行的必要資料的上一個步驟中的使用者資料表中建立新的資料行。 若要這樣做，我們將使用它來填入新建立的資料行，使用者資料表中一次執行主控台應用程式。

1. 建立新的主控台應用程式中現有的方案。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. 安裝最新版的 Entity Framework 套件。
3. 加入 web 應用程式建立上方，為主控台應用程式的參考。 以這滑鼠右鍵按一下專案，然後 ' 加入參考，然後方案，然後在專案上按一下，並按一下 確定。
4. 複製 Program.cs 類別中的程式碼底下。 此邏輯讀取的每個使用者設定檔資料、 將它序列化為物件 'ProfileInfo' 並將它儲存回資料庫。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   部分使用的模型定義於 'IdentityModels' 資料夾的 web 應用程式專案中，因此您必須將包含對應的命名空間。
5. 上述程式碼適用於應用程式中的資料庫檔案\_在先前步驟中建立的 web 應用程式專案的儲存資料夾。 若要參考的請使用 web 應用程式的 web.config 中的連接字串更新主控台應用程式的 app.config 檔案中的連接字串。 也提供 'AttachDbFilename' 屬性中的完整實體路徑。
6. 開啟命令提示字元並瀏覽至上面的主控台應用程式的 bin 資料夾。 執行可執行檔，並檢閱記錄檔輸出，如下列影像所示。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. 在 [伺服器總管] 中開啟 'AspNetUsers' 資料表，並確認保存的屬性的新資料行中的資料。 他們應該更新的相對應的屬性值。

## <a name="verify-functionality"></a>驗證功能

請使用新加入的成員資格頁面從舊的資料庫使用者登入使用 ASP.NET Identity 實作。 使用者應該能夠使用相同的認證登入。 請嘗試其他功能，例如新增 OAuth、 建立新的使用者，並變更密碼，新增角色，將使用者新增至角色等等。

應擷取並儲存在使用者資料表中舊的使用者和新使用者的設定檔資料。 舊的資料表應該不會再參考。

## <a name="conclusion"></a>結論

本文說明移轉用於 ASP.NET 識別的成員資格提供者模型的 web 應用程式的程序。 此外，發行項所述移轉的使用者連接到 身分識別系統的分析資料。 請留下註解以下問題和移轉您的應用程式時遇到的問題。

*感謝您 Rick Anderson 和 Robert McMurray 檢閱發行項。*
