---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: 實作自訂的 MySQL ASP.NET Identity 儲存體提供者 |Microsoft Docs
author: raquelsa
description: ASP.NET 身分識別是一種可延伸的系統可讓您建立自己的儲存體提供者，並插入您的應用程式中而不需要重新使用的應用...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 358b1a3b7277f21c63a1d395f2a5fce79bbe0d56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834235"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>實作自訂的 MySQL ASP.NET Identity 儲存體提供者
====================
藉由[Raquel Soares De Almeida](https://github.com/raquelsa)， [Suhas Joshi](https://github.com/suhasj)， [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET 身分識別是可延伸系統，可讓您建立自己的儲存體提供者，並插入您的應用程式中而不需要重新使用應用程式。 本主題描述如何建立 ASP.NET Identity 的 MySQL 儲存體提供者。 如需建立自訂的儲存體提供者的概觀，請參閱 <<c0> [ 概觀的自訂儲存體提供者的 ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)。
> 
> 若要完成本教學課程中，您必須使用 Visual Studio 2013 Update 2。
> 
> 本教學課程將會：
> 
> - 示範如何在 Azure 上建立的 MySQL 資料庫執行個體。
> - 示範如何使用 MySQL 用戶端工具 (MySQL Workbench) 來建立資料表及管理您在 Azure 上的遠端資料庫。
> - 示範如何使用我們在 MVC 應用程式專案的自訂實作取代預設的 ASP.NET Identity 儲存區實作。
> 
> 本教學課程中原先編寫 Raquel Soares De Almeida 和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。 範例專案已更新 Suhas Joshi 2.0 身分識別。 本主題已更新 Tom FitzMacken 2.0 身分識別。


## <a name="download-completed-project"></a>下載已完成專案

在本教學課程結束時，您必須 MVC 應用程式專案以使用 MySQL 資料庫裝載於 Azure 上的 ASP.NET 身分識別。

您可以下載已完成的 MySQL 儲存體提供者，在[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)。

## <a name="the-steps-you-will-perform"></a>您將執行的步驟

在本教學課程中，您將會：

1. 在 Azure 上建立 MySQL 資料庫
2. 在 MySQL 中建立 ASP.NET 身分識別的資料表
3. 建立 MVC 應用程式，並將它設定為使用 MySQL 提供者
4. 執行應用程式

本主題並未涵蓋 ASP.NET 身分識別與實作的客戶儲存體提供者時，您必須做出的決策的架構。 如需該資訊，請參閱[概觀的自訂儲存體提供者的 ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)。

## <a name="review-mysql-storage-provider-classes"></a>檢閱 MySQL 儲存體提供者類別

再開始建立 MySQL 儲存體提供者的步驟，讓我們看看可構成的儲存提供者類別。 您必須管理的資料庫作業的類別和應用程式以管理使用者和角色從呼叫的類別。

### <a name="storage-classes"></a>儲存類別

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -包含使用者的屬性。
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -包含新增、 更新或擷取使用者的作業。
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -包含角色的屬性。
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -包含新增、 刪除、 更新及擷取角色的作業。

### <a name="data-access-layer-classes"></a>資料存取層的類別

此範例中，資料存取層的類別包含 SQL 陳述式，使用資料表;不過，在您的程式碼中您可能想要使用物件關聯式對應 (ORM)，例如 Entity Framework 或 NHibernate。 特別是，您的應用程式可能會遇到不佳的效能，而不需要 ORM，其中包含消極式載入和快取的物件。 如需詳細資訊，請參閱[而不需要 Entity Framework 的 ASP.NET 身分識別 2.0 嗎？](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -包含 MySQL 資料庫連線和執行資料庫作業的方法。 UserStore 和 RoleStore 同時具現化這個類別的執行個體。
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -包含儲存角色之資料表的資料庫作業。
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -包含可儲存使用者宣告的資料表的資料庫作業。
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -包含可儲存使用者登入資訊的資料表的資料庫作業。
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -包含儲存哪些使用者指派給哪個角色之資料表的資料庫作業。
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -包含可儲存使用者的資料表的資料庫作業。

## <a name="create-a-mysql-database-instance-on-azure"></a>在 Azure 上建立的 MySQL 資料庫執行個體

1. 登入 [Azure 入口網站](https://manage.windowsazure.com/)。
2. 按一下  **+ 新增**底部的頁面上，，然後選取**存放區**。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. 在 **選擇和附加元件**精靈中，選取**ClearDB MySQL 資料庫**，然後按一下右下方的對話方塊 的下一步 箭號。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. 保留預設值**空閒**計劃，並將變更**名稱**來**IdentityMySQLDatabase**。 選取最接近的區域，然後按一下 下一步箭頭。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. 按一下核取記號以完成建立資料庫。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. 您的資料庫建立之後，您可以管理從**附加元件**管理入口網站中的索引標籤。   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. 您可以按一下來取得資料庫連接資訊**連接資訊**在頁面底部。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. 藉由按一下複製按鈕複製 連接字串，並將它儲存，因此您可以使用 MVC 應用程式中的更新版本。   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>在 MySQL 資料庫中建立 ASP.NET 身分識別的資料表

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>安裝 MySQL Workbench 來連線及管理 MySQL 資料庫的工具

1. 安裝**MySQL Workbench**工具[MySQL 下載頁面](http://dev.mysql.com/downloads/windows/installer/)
2. 啟動應用程式，並將按一下 新增上**MySQLConnections +** 按鈕以新增新的連接。 使用您從您稍早在本教學課程中建立的 Azure MySQL 資料庫複製的連接字串資料。
3. 建立連接後，開啟 新**查詢**索引標籤上，貼上從命令[MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)到查詢，才能建立資料庫資料表中執行它。
4. 現在，您會有所有 ASP.NET 身分識別必要的資料表建立 MySQL 資料庫裝載於 Azure，如下所示。  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>從範本建立 MVC 應用程式專案，並將它設定為使用 MySQL 提供者

如有需要安裝[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) Update 2。

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>從 CodePlex 下載 ASP.NET.Identity.MySQL 專案

1. 瀏覽至存放庫 URL [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)。
2. 下載的原始程式碼。
3. 將.zip 檔案解壓縮到本機資料夾。
4. 開啟 AspNet.Identity.MySQL 方案並建置專案。

### <a name="create-a-new-mvc-application-project-from-template"></a>從範本建立新的 MVC 應用程式專案

1. 以滑鼠右鍵按一下**AspNet.Identity.MySQL**解決方案並**新增**，**新專案**
2. 在 **加入新的專案**對話方塊中，選取**Visual C#** 左邊，然後**Web** ，然後選取**ASP.NET Web 應用程式**。 命名您的專案**IdentityMySQLDemo**; 然後按一下 [確定]。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. 中**新增 ASP.NET 專案**] 對話方塊中，選取 MVC 範本使用的預設選項 (包含**個別使用者帳戶**做為驗證方法)，按一下 [**確定**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. 在 [方案總管] 中，以滑鼠右鍵按一下您 IdentityMySQLDemo 的專案，然後選取**管理 NuGet 套件**。 在 搜尋文字 方塊 對話方塊中，輸入**Identity.EntityFramework**。 在結果清單中選取 此套件，然後按一下**解除安裝**。 系統會提示您解除安裝相依性套件 EntityFramework。 按一下 [是]，我們將不再於此應用程式的這個封裝。
5. 以滑鼠右鍵按一下 IdentityMySQLDemo 專案中，選取**新增**，**參考、 方案、 專案;** 選取 AspNet.Identity.MySQL 專案，然後按一下 **[確定]**。
6. 在 IdentityMySQLDemo 專案中，取代所有參考  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   取代為  
     `using AspNet.Identity.MySQL;`
7. 在 IdentityModels.cs，設定**ApplicationDbContext**衍生自**MySqlDatabase** ，而且包含需要連接名稱的單一參數的建構函式。  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. 開啟 IdentityConfig.cs 檔案。 在  **ApplicationUserManager.Create**方法中，取代為下列程式碼，UserManager 具現化：  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. 開啟 web.config 檔案，並取代此項目反白顯示的值替換為您在上一個步驟建立的 MySQL 資料庫的連接字串中的 DefaultConnection 字串：  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>執行應用程式，並連接到 MySQL 資料庫

1. 以滑鼠右鍵按一下**IdentityMySQLDemo**專案，然後選取**設定為啟始專案**
2. 按下**Ctrl + F5**以建置並執行應用程式。
3. 按一下 **註冊**頁面頂端的索引標籤。
4. 輸入新的使用者名稱和密碼，然後按一下**註冊**。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. 新的使用者立即註冊並登入。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. 返回至 MySQL Workbench 工具並檢查**IdentityMySQLDatabase**資料表的內容。 當您註冊新的使用者，請檢查使用者資料表的項目。  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>後續步驟

如需有關如何啟用此應用程式上的其他驗證方法的詳細資訊，請參閱[建立 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth2 和 OpenID 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。

若要了解如何使用 OAuth 整合您的資料庫，以及設定角色來限制使用者存取您的應用程式，請參閱[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。
