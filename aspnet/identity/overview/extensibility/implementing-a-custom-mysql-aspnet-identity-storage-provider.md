---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: 實作自訂 MySQL ASP.NET Identity 的存放裝置提供者 |Microsoft 文件
author: raquelsa
description: ASP.NET Identity 是可擴充的系統可讓您建立自己的儲存體提供者，並將之插入您的應用程式而不需要重新使用應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: d843b31e011fe520aad6cfdab0beca2d12477f12
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872734"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a><span data-ttu-id="06641-103">實作自訂 MySQL ASP.NET Identity 的存放裝置提供者</span><span class="sxs-lookup"><span data-stu-id="06641-103">Implementing a Custom MySQL ASP.NET Identity Storage Provider</span></span>
====================
<span data-ttu-id="06641-104">由[Raquel Soares De Almeida](https://github.com/raquelsa)， [Suhas Joshi](https://github.com/suhasj)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="06641-104">by [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="06641-105">ASP.NET Identity 是可擴充的系統可讓您建立自己的儲存體提供者，並將之插入您的應用程式而不需要重新使用應用程式。</span><span class="sxs-lookup"><span data-stu-id="06641-105">ASP.NET Identity is an extensible system which enables you to create your own storage provider and plug it into your application without re-working the application.</span></span> <span data-ttu-id="06641-106">本主題描述如何建立 ASP.NET Identity 的 MySQL 存放裝置提供者。</span><span class="sxs-lookup"><span data-stu-id="06641-106">This topic describes how to create a MySQL storage provider for ASP.NET Identity.</span></span> <span data-ttu-id="06641-107">如需建立自訂的存放裝置提供者的概觀，請參閱[概觀的自訂儲存提供者，ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)。</span><span class="sxs-lookup"><span data-stu-id="06641-107">For an overview of creating custom storage providers, see [Overview of Custom Storage Providers for ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span></span>
> 
> <span data-ttu-id="06641-108">若要完成本教學課程，您必須使用 Visual Studio 2013 Update 2。</span><span class="sxs-lookup"><span data-stu-id="06641-108">To complete this tutorial, you must have Visual Studio 2013 with Update 2.</span></span>
> 
> <span data-ttu-id="06641-109">本教學課程將會：</span><span class="sxs-lookup"><span data-stu-id="06641-109">This tutorial will:</span></span>
> 
> - <span data-ttu-id="06641-110">示範如何在 Azure 上建立的 MySQL 資料庫執行個體。</span><span class="sxs-lookup"><span data-stu-id="06641-110">Show how to create a MySQL database instance on Azure.</span></span>
> - <span data-ttu-id="06641-111">示範如何使用 MySQL 用戶端工具 (MySQL Workbench) 來建立資料表，並管理您在 Azure 上的遠端資料庫。</span><span class="sxs-lookup"><span data-stu-id="06641-111">Show how to use a MySQL client tool (MySQL Workbench) to create tables and manage your remote database on Azure.</span></span>
> - <span data-ttu-id="06641-112">顯示如何以取代預設 ASP.NET Identity 儲存實作我們在 MVC 應用程式專案的自訂實作。</span><span class="sxs-lookup"><span data-stu-id="06641-112">Show how to replace the default ASP.NET Identity storage implementation with our custom implementation on a MVC application project.</span></span>
> 
> <span data-ttu-id="06641-113">本教學課程原始寫入 Raquel Soares De Almeida 和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="06641-113">This tutorial was originally written by Raquel Soares De Almeida and Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).</span></span> <span data-ttu-id="06641-114">這個範例專案已更新 Suhas Joshi 2.0 身分識別。</span><span class="sxs-lookup"><span data-stu-id="06641-114">The sample project was updated for Identity 2.0 by Suhas Joshi.</span></span> <span data-ttu-id="06641-115">本主題已更新 Tom FitzMacken 2.0 身分識別。</span><span class="sxs-lookup"><span data-stu-id="06641-115">The topic was updated for Identity 2.0 by Tom FitzMacken.</span></span>


## <a name="download-completed-project"></a><span data-ttu-id="06641-116">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="06641-116">Download completed project</span></span>

<span data-ttu-id="06641-117">在本教學課程結束時，您必須在 MVC 應用程式專案與 ASP.NET Identity 使用裝載於 Azure 的 MySQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="06641-117">At the end of this tutorial, you will have an MVC application project with ASP.NET Identity working with a MySQL database hosted on Azure.</span></span>

<span data-ttu-id="06641-118">您可以下載已完成的 MySQL 存放裝置提供者在[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)。</span><span class="sxs-lookup"><span data-stu-id="06641-118">You can download the completed MySQL storage provider at [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span></span>

## <a name="the-steps-you-will-perform"></a><span data-ttu-id="06641-119">您將執行的步驟</span><span class="sxs-lookup"><span data-stu-id="06641-119">The steps you will perform</span></span>

<span data-ttu-id="06641-120">在本教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="06641-120">In this tutorial you will:</span></span>

1. <span data-ttu-id="06641-121">在 Azure 上建立 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="06641-121">Create a MySQL database on Azure</span></span>
2. <span data-ttu-id="06641-122">在 MySQL 中建立 ASP.NET Identity 資料表</span><span class="sxs-lookup"><span data-stu-id="06641-122">Create the ASP.NET Identity tables in MySQL</span></span>
3. <span data-ttu-id="06641-123">建立 MVC 應用程式，並將它設定為使用 MySQL 提供者</span><span class="sxs-lookup"><span data-stu-id="06641-123">Create an MVC application and configure it to use the MySQL provider</span></span>
4. <span data-ttu-id="06641-124">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="06641-124">Run the app</span></span>

<span data-ttu-id="06641-125">本主題並未涵蓋 ASP.NET Identity 和實作的客戶儲存體提供者時，您必須做出的決策的架構。</span><span class="sxs-lookup"><span data-stu-id="06641-125">This topic does not cover the architecture of ASP.NET Identity and the decisions you must make when implementing a customer storage provider.</span></span> <span data-ttu-id="06641-126">如需該資訊，請參閱[概觀的自訂儲存提供者，ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)。</span><span class="sxs-lookup"><span data-stu-id="06641-126">For that information, see [Overview of Custom Storage Providers for ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span></span>

## <a name="review-mysql-storage-provider-classes"></a><span data-ttu-id="06641-127">檢視 MySQL 的存放裝置提供者類別</span><span class="sxs-lookup"><span data-stu-id="06641-127">Review MySQL storage provider classes</span></span>

<span data-ttu-id="06641-128">再開始建立 MySQL 存放裝置提供者的步驟，讓我們看看組成的儲存提供者的類別。</span><span class="sxs-lookup"><span data-stu-id="06641-128">Before jumping into the steps to create the MySQL storage provider, let's look at the classes that make up the storage provider.</span></span> <span data-ttu-id="06641-129">您必須管理的資料庫作業的類別和類別，從要管理使用者和角色的應用程式呼叫。</span><span class="sxs-lookup"><span data-stu-id="06641-129">You will need classes that manage the database operations and classes that are called from the application to manage users and roles.</span></span>

### <a name="storage-classes"></a><span data-ttu-id="06641-130">儲存類別</span><span class="sxs-lookup"><span data-stu-id="06641-130">Storage classes</span></span>

- <span data-ttu-id="06641-131">[IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -包含使用者的屬性。</span><span class="sxs-lookup"><span data-stu-id="06641-131">[IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) - contains properties for the user.</span></span>
- <span data-ttu-id="06641-132">[UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -包含新增、 更新或擷取使用者的作業。</span><span class="sxs-lookup"><span data-stu-id="06641-132">[UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) - contains operations for adding, updating or retrieving users.</span></span>
- <span data-ttu-id="06641-133">[IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -包含角色的屬性。</span><span class="sxs-lookup"><span data-stu-id="06641-133">[IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) - contains properties for roles.</span></span>
- <span data-ttu-id="06641-134">[RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -包含用於加入、 刪除、 更新和擷取角色的作業。</span><span class="sxs-lookup"><span data-stu-id="06641-134">[RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) - contains operations for adding, deleting, updating and retrieving roles.</span></span>

### <a name="data-access-layer-classes"></a><span data-ttu-id="06641-135">資料存取層的類別</span><span class="sxs-lookup"><span data-stu-id="06641-135">Data access layer classes</span></span>

<span data-ttu-id="06641-136">此範例中，資料存取層的類別包含 SQL 陳述式來處理資料表;不過，在程式碼中您可能想要使用物件關聯式對應 (ORM)，Entity Framework 或 NHibernate 等。</span><span class="sxs-lookup"><span data-stu-id="06641-136">For this example, the data access layer classes contain SQL statements for working with the tables; however, in your code you might want to use object-relational mapping (ORM) such as Entity Framework or NHibernate.</span></span> <span data-ttu-id="06641-137">特別是，您的應用程式可能會遇到效能不佳 ORM 包含消極式載入和快取物件沒有。</span><span class="sxs-lookup"><span data-stu-id="06641-137">In particular, your application may experience poor performance without an ORM that includes lazy loading and object caching.</span></span> <span data-ttu-id="06641-138">如需詳細資訊，請參閱[ASP.NET Identity 2.0 沒有 Entity Framework？](https://aspnetidentity.codeplex.com/discussions/561828)</span><span class="sxs-lookup"><span data-stu-id="06641-138">For more information, see [ASP.NET Identity 2.0 without Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)</span></span>

- <span data-ttu-id="06641-139">[MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -包含的 MySQL 資料庫連接和執行資料庫作業的方法。</span><span class="sxs-lookup"><span data-stu-id="06641-139">[MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) - contains the MySQL database connection and methods for performing database operations.</span></span> <span data-ttu-id="06641-140">UserStore 和 RoleStore 會同時使用具現化這個類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="06641-140">UserStore and RoleStore are both instantiated with an instance of this class.</span></span>
- <span data-ttu-id="06641-141">[RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -包含資料庫作業的資料表，儲存角色。</span><span class="sxs-lookup"><span data-stu-id="06641-141">[RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) - contains database operations for the table that stores roles.</span></span>
- <span data-ttu-id="06641-142">[UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -包含資料庫作業的儲存使用者宣告的資料表。</span><span class="sxs-lookup"><span data-stu-id="06641-142">[UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) - contains database operations for the table that stores user claims.</span></span>
- <span data-ttu-id="06641-143">[UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -包含資料庫作業的儲存使用者登入資訊的資料表。</span><span class="sxs-lookup"><span data-stu-id="06641-143">[UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) - contains database operations for the table that stores user login information.</span></span>
- <span data-ttu-id="06641-144">[UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -包含儲存哪些角色所指派的使用者資料表的資料庫作業。</span><span class="sxs-lookup"><span data-stu-id="06641-144">[UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) - contains database operations for the table that stores which users are assigned to which roles.</span></span>
- <span data-ttu-id="06641-145">[UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -包含資料庫作業的儲存使用者的資料表。</span><span class="sxs-lookup"><span data-stu-id="06641-145">[UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) - contains database operations for the table that stores users.</span></span>

## <a name="create-a-mysql-database-instance-on-azure"></a><span data-ttu-id="06641-146">在 Azure 上建立的 MySQL 資料庫執行個體</span><span class="sxs-lookup"><span data-stu-id="06641-146">Create a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="06641-147">登入[Azure 入口網站](https://manage.windowsazure.com/)。</span><span class="sxs-lookup"><span data-stu-id="06641-147">Log in to the [Azure Portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="06641-148">按一下 **+ 新增**底部的頁面上，，然後選取**存放區**。</span><span class="sxs-lookup"><span data-stu-id="06641-148">Click **+NEW** at the bottom of the page, and then select **STORE**.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. <span data-ttu-id="06641-149">在**選擇和附加元件**精靈中，選取**ClearDB 的 MySQL 資料庫**，然後按一下右下方的對話方塊的 下一步箭號。</span><span class="sxs-lookup"><span data-stu-id="06641-149">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database** and click on the next arrow at the bottom right of the dialog.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. <span data-ttu-id="06641-150">保留預設值**免費**計劃，並變更**名稱**至**IdentityMySQLDatabase**。</span><span class="sxs-lookup"><span data-stu-id="06641-150">Keep the default **Free** plan and change the **Name** to **IdentityMySQLDatabase**.</span></span> <span data-ttu-id="06641-151">選取最接近您的區域，然後按一下 下一步箭頭。</span><span class="sxs-lookup"><span data-stu-id="06641-151">Select the region nearest you and then click the next arrow.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. <span data-ttu-id="06641-152">按一下核取記號以完成 建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="06641-152">Click the checkmark to complete the database creation.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. <span data-ttu-id="06641-153">建立資料庫之後，您可以管理從**附加元件**管理入口網站中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="06641-153">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span>   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. <span data-ttu-id="06641-154">您可以按一下來取得資料庫連接資訊**連接資訊**頁面的底部。</span><span class="sxs-lookup"><span data-stu-id="06641-154">You can get the database connection information by clicking on **CONNECTION INFO** at the bottom of the page.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. <span data-ttu-id="06641-155">依序按一下 [複製] 按鈕上複製連接字串，並將它儲存，因此您可以稍後在 MVC 應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="06641-155">Copy the connection string by clicking on the copy button and save it so you can use later in your MVC application.</span></span>   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a><span data-ttu-id="06641-156">建立 ASP.NET 識別資料表中的 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="06641-156">Create the ASP.NET Identity tables in a MySQL database</span></span>

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a><span data-ttu-id="06641-157">安裝 MySQL Workbench 工具連線和管理 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="06641-157">Install MySQL Workbench tool to connect and manage MySQL database</span></span>

1. <span data-ttu-id="06641-158">安裝**MySQL Workbench**工具[MySQL 下載頁面](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="06641-158">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="06641-159">啟動應用程式，新增上按一下  **MySQLConnections +** 按鈕即可加入新的連接。</span><span class="sxs-lookup"><span data-stu-id="06641-159">Launch the app and add click on the **MySQLConnections +** button to add a new connection.</span></span> <span data-ttu-id="06641-160">使用您複製從您稍早在本教學課程中建立 Azure 的 MySQL 資料庫的連接字串資料。</span><span class="sxs-lookup"><span data-stu-id="06641-160">Use the connection string data you copied from the Azure MySQL database you created earlier in this tutorial.</span></span>
3. <span data-ttu-id="06641-161">建立連接後，開啟 [新**查詢**] 索引標籤; 貼上從命令[MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)到查詢，才能建立資料庫資料表中執行它。</span><span class="sxs-lookup"><span data-stu-id="06641-161">After establishing the connection, open a new **Query** tab; paste the commands from [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) into the query and execute it in order to create the database tables.</span></span>
4. <span data-ttu-id="06641-162">現在，您會有所有 ASP.NET 識別必要的資料表建立 MySQL 資料庫裝載於 Azure，如下所示。</span><span class="sxs-lookup"><span data-stu-id="06641-162">You now have all the ASP.NET Identity necessary tables created on a MySQL database hosted on Azure as shown below.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a><span data-ttu-id="06641-163">從範本建立的 MVC 應用程式專案，並將它設定為使用 MySQL 提供者</span><span class="sxs-lookup"><span data-stu-id="06641-163">Create an MVC application project from template and configure it to use MySQL provider</span></span>

<span data-ttu-id="06641-164">如有需要安裝[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) Update 2。</span><span class="sxs-lookup"><span data-stu-id="06641-164">If needed, install either [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) with Update 2.</span></span>

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a><span data-ttu-id="06641-165">從 CodePlex 下載 ASP.NET.Identity.MySQL 專案</span><span class="sxs-lookup"><span data-stu-id="06641-165">Download the ASP.NET.Identity.MySQL project from CodePlex</span></span>

1. <span data-ttu-id="06641-166">瀏覽至儲存機制 URL，在[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)。</span><span class="sxs-lookup"><span data-stu-id="06641-166">Browse to the repository URL at [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span></span>
2. <span data-ttu-id="06641-167">下載的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="06641-167">Download the source code.</span></span>
3. <span data-ttu-id="06641-168">將.zip 檔案解壓縮至本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="06641-168">Extract the .zip file into a local folder.</span></span>
4. <span data-ttu-id="06641-169">開啟 AspNet.Identity.MySQL 方案並建置專案。</span><span class="sxs-lookup"><span data-stu-id="06641-169">Open the AspNet.Identity.MySQL solution and build it.</span></span>

### <a name="create-a-new-mvc-application-project-from-template"></a><span data-ttu-id="06641-170">從範本建立新的 MVC 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="06641-170">Create a new MVC application project from template</span></span>

1. <span data-ttu-id="06641-171">以滑鼠右鍵按一下**AspNet.Identity.MySQL**方案和**新增**，**新專案**</span><span class="sxs-lookup"><span data-stu-id="06641-171">Right click the **AspNet.Identity.MySQL** solution and **Add**, **New Project**</span></span>
2. <span data-ttu-id="06641-172">在**加入新的專案**對話方塊中，選取**Visual C#** 在左邊，然後**Web** ，然後選取  **ASP.NET Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="06641-172">In the **Add New Project** Dialog select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="06641-173">命名您的專案**IdentityMySQLDemo**; 然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="06641-173">Name your project **IdentityMySQLDemo**; and then click OK.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. <span data-ttu-id="06641-174">中**新增 ASP.NET 專案**] 對話方塊中，選取預設選項的 MVC 範本 (包含**個別使用者帳戶**做為驗證方法)，按一下 [**確定**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)</span><span class="sxs-lookup"><span data-stu-id="06641-174">In the **New ASP.NET Project** dialog, select the MVC template with the default options (that includes **Individual User Accounts** as authentication method) and click **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)</span></span>
4. <span data-ttu-id="06641-175">在 方案總管 IdentityMySQLDemo 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="06641-175">In Solution Explorer, right-click your IdentityMySQLDemo project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="06641-176">在 搜尋文字 方塊 對話方塊中，輸入**Identity.EntityFramework**。</span><span class="sxs-lookup"><span data-stu-id="06641-176">In the search text box dialog, type **Identity.EntityFramework**.</span></span> <span data-ttu-id="06641-177">在結果清單中選取此套件，然後按一下**解除安裝**。</span><span class="sxs-lookup"><span data-stu-id="06641-177">Select this package in the list of results and click **Uninstall**.</span></span> <span data-ttu-id="06641-178">系統會提示您解除安裝相依性套件 EntityFramework。</span><span class="sxs-lookup"><span data-stu-id="06641-178">You will be prompted to uninstall the dependency package EntityFramework.</span></span> <span data-ttu-id="06641-179">按一下 [是] 我們不會再將此封裝，在此應用程式。</span><span class="sxs-lookup"><span data-stu-id="06641-179">Click on Yes as we will no longer this package on this application.</span></span>
5. <span data-ttu-id="06641-180">以滑鼠右鍵按一下 IdentityMySQLDemo 專案中，選取**新增**，**參考、 方案、 專案;** 選取 AspNet.Identity.MySQL 專案，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="06641-180">Right click the IdentityMySQLDemo project, select **Add**, **Reference, Solution, Projects;** select the AspNet.Identity.MySQL project and click **OK**.</span></span>
6. <span data-ttu-id="06641-181">在 IdentityMySQLDemo 專案中，以取代所有參考</span><span class="sxs-lookup"><span data-stu-id="06641-181">In the IdentityMySQLDemo project, replace all references to</span></span>  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   <span data-ttu-id="06641-182">取代為</span><span class="sxs-lookup"><span data-stu-id="06641-182">with</span></span>  
     `using AspNet.Identity.MySQL;`
7. <span data-ttu-id="06641-183">集中 IdentityModels.cs， **ApplicationDbContext**衍生自**MySqlDatabase**而且包含需要連接名稱的單一參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="06641-183">In IdentityModels.cs, set **ApplicationDbContext** to derive from **MySqlDatabase** and include a contructor that take a single parameter with the connection name.</span></span>  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. <span data-ttu-id="06641-184">開啟 IdentityConfig.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="06641-184">Open the IdentityConfig.cs file.</span></span> <span data-ttu-id="06641-185">在**ApplicationUserManager.Create**方法，取代為下列程式碼，UserManager 具現化：</span><span class="sxs-lookup"><span data-stu-id="06641-185">In the **ApplicationUserManager.Create** method, replace instantiating UserManager with the following code:</span></span>  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. <span data-ttu-id="06641-186">開啟 web.config 檔案並將 DefaultConnection 字串取代此項目反白顯示的值取代為您在上一個步驟所建立的 MySQL 資料庫的連接字串：</span><span class="sxs-lookup"><span data-stu-id="06641-186">Open the web.config file and replace the DefaultConnection string with this entry replacing the highlighted values with the connection string of the MySQL database you created on previous steps:</span></span>  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a><span data-ttu-id="06641-187">執行應用程式，並連接到 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="06641-187">Run the app and connect to the MySQL DB</span></span>

1. <span data-ttu-id="06641-188">以滑鼠右鍵按一下**IdentityMySQLDemo**專案，然後選取**設定為啟始專案**</span><span class="sxs-lookup"><span data-stu-id="06641-188">Right click the **IdentityMySQLDemo** project and select **Set as Startup Project**</span></span>
2. <span data-ttu-id="06641-189">按**Ctrl + F5**建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="06641-189">Press **Ctrl + F5** to build and run the app.</span></span>
3. <span data-ttu-id="06641-190">按一下**註冊**的頁面頂端的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="06641-190">Click on **Register** tab on the top of the page.</span></span>
4. <span data-ttu-id="06641-191">輸入新的使用者名稱和密碼，然後按一下 上**註冊**。</span><span class="sxs-lookup"><span data-stu-id="06641-191">Enter a new user name and password and then click on **Register**.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. <span data-ttu-id="06641-192">新的使用者現在已註冊，且登入。</span><span class="sxs-lookup"><span data-stu-id="06641-192">The new user is now registered and logged in.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. <span data-ttu-id="06641-193">返回 MySQL Workbench 工具並檢查**IdentityMySQLDatabase**資料表的內容。</span><span class="sxs-lookup"><span data-stu-id="06641-193">Go back to the MySQL Workbench tool and inspect the **IdentityMySQLDatabase** table's contents.</span></span> <span data-ttu-id="06641-194">當您註冊新的使用者，請檢查使用者資料表的項目。</span><span class="sxs-lookup"><span data-stu-id="06641-194">Inspect the users table for the entries as you register new users.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a><span data-ttu-id="06641-195">後續步驟</span><span class="sxs-lookup"><span data-stu-id="06641-195">Next Steps</span></span>

<span data-ttu-id="06641-196">如需有關如何啟用此應用程式上的其他驗證方法的詳細資訊，請參閱[建立 ASP.NET MVC 5 應用程式與 Facebook、 Google OAuth2 和 OpenID 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="06641-196">For more information on how to enable other authentication methods on this app, refer to [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<span data-ttu-id="06641-197">若要深入了解如何使用 OAuth 整合您的資料庫，以及如何設定角色來限制使用者存取您的應用程式，請參閱[成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="06641-197">To learn how to integrate your DB with OAuth and to set up roles to limit users access to your app, see [Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>
