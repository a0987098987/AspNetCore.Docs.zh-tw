---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: 將通用的提供者的資料移轉成員資格和使用者設定檔至 ASP.NET Identity (C#) |Microsoft Docs
author: rustd
description: 本教學課程將告訴您需要移轉使用者和角色的資料，以及使用現有的應用程式的通用提供者所建立的使用者設定檔資料的步驟...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: a91bb6ac51819d7dbb8eb3c63bd36a9d830eecce
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578221"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>將通用的提供者的資料移轉成員資格和使用者設定檔至 ASP.NET Identity (C#)
====================
藉由[請參閱 Pranav Rastogi](https://github.com/rustd)， [Rick Anderson]((https://twitter.com/RickAndMSFT))， [Robert McMurray](https://github.com/rmcmurray)， [Suhas Joshi](https://github.com/suhasj)

> 本教學課程會說明需要移轉使用者和角色的資料，以及使用 ASP.NET 身分識別模型的現有應用程式的通用提供者所建立的使用者設定檔資料的步驟。 方法此處提及來移轉使用者設定檔資料可以使用 SQL 成員資格的應用程式中。


版本的 Visual Studio 2013 中，ASP.NET 團隊推出了新的 ASP.NET 身分識別系統，以及您可以深入了解該版本[此處](../../index.md)。 要從 web 應用程式移轉的發行項上[到新的身分識別系統的 SQL 成員資格](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)，這篇文章說明如何移轉現有的應用程式，請依照下列使用者和角色管理的提供者模型的步驟新的身分識別模型。 本教學課程的焦點會在移轉使用者設定檔資料，來順暢地將它連結到新的系統。 移轉使用者和角色的資訊是類似 SQL 的成員資格。 移轉設定檔資料需遵循的方法可以使用 SQL 成員資格的應用程式中。

例如，我們會開始使用 Visual Studio 2012 使用的提供者模型所建立的 web 應用程式。 我們將則加入設定檔管理的程式碼、 註冊的使用者、 新增使用者設定檔資料、 移轉資料庫結構描述，然後變更應用程式使用的身分識別系統的使用者和角色管理。 為移轉的測試，使用通用的提供者所建立的使用者應該能夠登入，而且新的使用者應該無法註冊。

> [!NOTE]
> 您可以找到完整的範例，在[ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations)。


## <a name="profile-data-migration-summary"></a>設定檔資料移轉摘要

再開始移轉，讓我們看看設定檔資料儲存在提供者模型中的體驗。 應用程式，使用者可以儲存多種方式的設定檔資料，最常見其間正在使用通用的提供者所隨附的內建的設定檔提供者。 這些步驟會包含

1. 加入具有用來儲存設定檔資料屬性的類別。
2. 新增類別來擴充 'ProfileBase'，並實作方法，以取得使用者的上述的設定檔資料。
3. 使用預設設定檔提供者中的啟用*web.config*檔案，並在步驟 #2，用於存取設定檔資訊中宣告的類別定義。

設定檔資訊會儲存為序列化的 xml 和資料庫中的 'Profiles' 資料表中的二進位資料。

移轉應用程式使用新的 ASP.NET 身分識別系統之後, 會設定檔資訊還原序列化，並儲存為使用者類別的屬性。 然後，每個屬性可以對應到使用者資料表中的資料行。 此處的優點是，屬性即可使用除了不需要序列化/還原序列化資料資訊每個使用者類別直接存取時的時間。

## <a name="getting-started"></a>快速入門

1. Visual Studio 2012 中建立新的 ASP.NET 4.5 Web Form 應用程式。 目前的範例會使用 Web Forms 範本，但您也可以使用 MVC 應用程式。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. 建立新的資料夾 '模型' 儲存設定檔資訊  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. 舉例來說，讓我們儲存日期的生日、 縣 （市）、 高度和寬度的使用者設定檔中。 高度和寬度會儲存為自訂類別，稱為 'PersonalStats' 中。 若要儲存及擷取的設定檔，我們需要可擴充 'ProfileBase' 的類別。 讓我們建立一個新的類別 'AppProfile' 來取得及儲存設定檔資訊。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. 啟用中的設定檔*web.config*檔案。 輸入用於儲存/擷取使用者資訊在步驟 #3 中建立的類別名稱。

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. 從使用者取得設定檔資料，並將其儲存的 'Account' 資料夾中新增 web forms 網頁。 以滑鼠右鍵按一下專案，並選取 [加入新項目]。 新增與主版頁面 'AddProfileData.aspx' webforms 頁面。 複製下列 'MainContent' 區段中：

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   在程式碼後置中新增下列程式碼：

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   新增命名空間下的 AppProfile 類別定義要移除的編譯錯誤。
6. 執行應用程式，並建立新的使用者具有使用者名稱 '**olduser'。** 瀏覽至 'AddProfileData' 頁面，並新增使用者設定檔資訊。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

您可以確認資料會儲存為序列化的 xml，使用 [伺服器總管] 視窗的 'Profiles' 資料表中。 在 Visual Studio 中，從 [檢視] 功能表上，選擇 [伺服器總管]。 應該針對資料庫中定義的資料連接*web.config*檔案。 按一下資料連接，顯示不同的子類別目錄。 展開以顯示不同的資料表在資料庫中，以滑鼠右鍵按一下 'Profiles' 然後選擇 [顯示資料表資料] 檢視設定檔資料表中儲存的設定檔資料的 「 資料表 」。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>移轉資料庫結構描述

若要讓現有的資料庫使用的身分識別系統，我們需要更新以支援我們加入至原始的資料庫欄位的識別資料庫中的結構描述。 這可以使用 SQL 指令碼來建立新的資料表，並複製現有的資訊。 在 [伺服器總管] 視窗中，展開 'DefaultConnection' 中顯示的資料表。 以滑鼠右鍵按一下資料表，然後選取 [新增查詢]

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

貼上的 SQL 指令碼[ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt)並加以執行。 如果重新整理 'DefaultConnection' 時，我們可以看到，會新增新的資料表。 您可以檢查以查看已移轉資訊的資料表內的資料。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>若要使用 ASP.NET 身分識別應用程式移轉

1. 安裝 ASP.NET 身分識別所需的 Nuget 套件：

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   在 管理 Nuget 套件的詳細資訊可以找到[這裡](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. 若要使用資料表中的現有資料，我們要建立模型類別將對應至資料表，並將它們連結在身分識別系統中。 身分識別合約的一部分，模型類別應實作 Identity.Core dll 中定義的介面，或可以擴充現有 Microsoft.AspNet.Identity.EntityFramework 這些介面的實作。 我們將使用現有類別的角色、 使用者登入和使用者宣告。 我們需要使用自訂的使用者，我們的範例。 以滑鼠右鍵按一下專案，並建立新的資料夾 'IdentityModels'。 加入新的 「 使用者 」 類別，如下所示：

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   請注意，'ProfileInfo' 現在是使用者類別的屬性。 因此我們可以使用使用者類別直接使用設定檔資料。

在將檔案複製**IdentityModels**並**IdentityAccount**下載來源的資料夾 ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) )。 它們有剩餘的模型類別和新使用者和角色管理使用 ASP.NET 身分識別 Api 所需的頁面。 所使用的方法是類似於 SQL 成員資格，且可以找到詳細的說明[此處](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)。

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>將設定檔資料複製到新的資料表

如先前所述，我們要還原序列化設定檔資料表中的 xml 資料，並將它儲存在 AspNetUsers 資料表的資料行。 因此剩下的就是填入必要資料，這些資料行上一個步驟中的 users 資料表中建立新的資料行。 若要這樣做，我們將使用它來填入新建立的資料行，users 資料表中一次執行的主控台應用程式。

1. 在現有的方案中建立新的主控台應用程式。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. 安裝最新版的 Entity Framework 套件。
3. 新增為主控台應用程式的參考上述所建立的 web 應用程式。 若要執行這以滑鼠右鍵按一下專案，然後 [新增參考]，然後解決方案中，按一下專案，然後按一下 [確定]。
4. 複製下列 Program.cs 類別中的程式碼。 此邏輯讀取的每個使用者設定檔資料、 將其序列化為 'ProfileInfo' 物件，並將它儲存回資料庫。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   部分使用的模型定義於 'IdentityModels' web 應用程式專案資料夾，因此您必須將包含對應的命名空間。
5. 上述程式碼適用於應用程式中的資料庫檔案\_上一個步驟中建立的 web 應用程式專案的 [資料] 資料夾。 若要參考的請使用 web 應用程式的 web.config 中的連接字串更新主控台應用程式的 app.config 檔案中的連接字串。 也提供 'AttachDbFilename' 屬性中的完整實體路徑。
6. 開啟命令提示字元並瀏覽至上面的主控台應用程式的 bin 資料夾。 執行可執行檔，並檢閱的記錄輸出，如下圖所示。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. 在 [伺服器總管] 中開啟 'AspNetUsers' 資料表，並確認保存屬性的新資料行中的資料。 它們應該更新的相對應的屬性值。

## <a name="verify-functionality"></a>驗證功能

使用會實作使用 ASP.NET 身分識別登入從舊的資料庫使用者的新加入的成員資格頁面。 使用者應該能夠使用相同的認證登入。 請嘗試其他功能，例如新增 OAuth，建立新的使用者，並變更密碼的新增角色，將使用者新增至角色，依此類推。

應該擷取和儲存在使用者資料表中舊的使用者和新的使用者設定檔資料。 應該不會再參考舊的資料表。

## <a name="conclusion"></a>結論

本文說明使用 ASP.NET 身分識別的成員資格提供者模型的移轉 web 應用程式的程序。 發行項此外概述移轉連結至身分識別系統的使用者設定檔資料。 請留下註解以下問題和移轉您的應用程式時所遇到的問題。

*感謝 Rick Anderson 和 Robert McMurray 的檢閱。*
