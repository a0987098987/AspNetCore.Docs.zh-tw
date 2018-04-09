---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: EntityFramework MySQL 提供者 (C#) 搭配使用 MySQL 的儲存體 |Microsoft 文件'
author: maumar
description: 本教學課程會示範如何以取代 MySQL 能 EntityFramework （SQL 用戶端提供者） 與 ASP.NET 識別的預設資料儲存機制...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="48b6d-103">ASP.NET 識別： 使用 MySQL 儲存體使用 EntityFramework MySQL 提供者 (C#)</span><span class="sxs-lookup"><span data-stu-id="48b6d-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="48b6d-104">由[Maurycy Markowski](https://github.com/maumar)， [Raquel Soares De Almeida](https://github.com/raquelsa)， [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="48b6d-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="48b6d-105">本教學課程會示範如何取代預設資料儲存機制的[ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) EntityFramework （SQL 用戶端提供者） 與 MySQL 提供者使用。</span><span class="sxs-lookup"><span data-stu-id="48b6d-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="48b6d-106">下列主題將涵蓋在本教學課程：</span><span class="sxs-lookup"><span data-stu-id="48b6d-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="48b6d-107">在 Azure 上建立 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="48b6d-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="48b6d-108">建立 MVC 應用程式使用 Visual Studio 2013 MVC 範本</span><span class="sxs-lookup"><span data-stu-id="48b6d-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="48b6d-109">設定 EntityFramework 為處理 MySQL 資料庫提供者</span><span class="sxs-lookup"><span data-stu-id="48b6d-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="48b6d-110">執行應用程式來驗證結果</span><span class="sxs-lookup"><span data-stu-id="48b6d-110">Running the application to verify the results</span></span>

<span data-ttu-id="48b6d-111">在本教學課程結束時，您必須儲存 ASP.NET Identity 的 MVC 應用程式正在使用裝載於 Azure 的 MySQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="48b6d-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="48b6d-112">在 Azure 上建立的 MySQL 資料庫執行個體</span><span class="sxs-lookup"><span data-stu-id="48b6d-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="48b6d-113">登入[Azure 入口網站](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="48b6d-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="48b6d-114">按一下**新增**底部的頁面上，，然後選取**存放區**:</span><span class="sxs-lookup"><span data-stu-id="48b6d-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="48b6d-115">在**選擇和附加元件**精靈中，選取**ClearDB 的 MySQL 資料庫**，然後按一下 **下一步**框架底部的箭號：</span><span class="sxs-lookup"><span data-stu-id="48b6d-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="48b6d-116">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-116">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-117">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="48b6d-118">保留預設值**免費**，計劃變更**名稱**至**IdentityMySQLDatabase**，選取 是，最接近的區域，然後按一下**下一步**框架底部的箭號：</span><span class="sxs-lookup"><span data-stu-id="48b6d-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="48b6d-119">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-119">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-120">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="48b6d-121">按一下**購買**核取記號，完成資料庫的建立。</span><span class="sxs-lookup"><span data-stu-id="48b6d-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="48b6d-122">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-122">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-123">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="48b6d-124">建立資料庫之後，您可以管理從**附加元件**管理入口網站中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="48b6d-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="48b6d-125">若要擷取之資料庫的連接資訊，請按一下**連接資訊**底部的頁面：</span><span class="sxs-lookup"><span data-stu-id="48b6d-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="48b6d-126">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-126">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-127">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="48b6d-128">複製連接字串上的 [複製] 按鈕即可**CONNECTIONSTRING** ; 欄位，並將它儲存為 MVC 應用程式將這項資訊稍後在本教學課程：</span><span class="sxs-lookup"><span data-stu-id="48b6d-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="48b6d-129">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-129">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-130">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="48b6d-131">建立 MVC 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="48b6d-131">Creating an MVC application project</span></span>

<span data-ttu-id="48b6d-132">若要完成本教學課程的這一節中的步驟，您首先需要安裝[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="48b6d-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="48b6d-133">一旦安裝 Visual Studio 之後，請建立新的 MVC 應用程式專案中使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="48b6d-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="48b6d-134">開啟 Visual Studio 2103。</span><span class="sxs-lookup"><span data-stu-id="48b6d-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="48b6d-135">按一下**新專案**從**啟動**] 頁面上，或者您可以按一下 [**檔案**功能表，然後**新專案**:</span><span class="sxs-lookup"><span data-stu-id="48b6d-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="48b6d-136">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-136">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-137">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="48b6d-138">當**新專案**] 對話方塊隨即顯示，接著展開**Visual C#**在範本清單，然後按一下 [ **Web**，並選取**ASP.NET Web 應用程式**.</span><span class="sxs-lookup"><span data-stu-id="48b6d-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="48b6d-139">命名您的專案**IdentityMySQLDemo** ，然後按一下 **確定**:</span><span class="sxs-lookup"><span data-stu-id="48b6d-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="48b6d-140">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-140">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-141">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="48b6d-142">在**新增 ASP.NET 專案**對話方塊中，選取**MVC** templatewith 預設選項; 這將會設定**個別使用者帳戶**做為驗證方法。</span><span class="sxs-lookup"><span data-stu-id="48b6d-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="48b6d-143">按一下**確定**:</span><span class="sxs-lookup"><span data-stu-id="48b6d-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="48b6d-144">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-144">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-145">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="48b6d-146">設定要使用的 MySQL 資料庫 EntityFramework</span><span class="sxs-lookup"><span data-stu-id="48b6d-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="48b6d-147">更新您的專案的 Entity Framework 組件</span><span class="sxs-lookup"><span data-stu-id="48b6d-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="48b6d-148">從 Visual Studio 2013 範本建立的 MVC 應用程式包含的參考[EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework)封裝，但必須已包含該組件，因為其版本更新的重要效能改善。</span><span class="sxs-lookup"><span data-stu-id="48b6d-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="48b6d-149">若要在應用程式中使用這些最新的更新，請使用下列步驟。</span><span class="sxs-lookup"><span data-stu-id="48b6d-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="48b6d-150">Visual Studio 2013 中開啟 MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="48b6d-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="48b6d-151">按一下**工具**，然後按一下 **程式庫套件管理員**，然後按一下  **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="48b6d-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="48b6d-152">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-152">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-153">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="48b6d-154">**Package Manager Console**會出現在 Visual Studio 的下一節。</span><span class="sxs-lookup"><span data-stu-id="48b6d-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="48b6d-155">型別&quot;**更新套件 EntityFramework** &quot;按下 Enter:</span><span class="sxs-lookup"><span data-stu-id="48b6d-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="48b6d-156">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-156">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-157">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="48b6d-158">安裝 EntityFramework 的 MySQL 提供者</span><span class="sxs-lookup"><span data-stu-id="48b6d-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="48b6d-159">為了讓 EntityFramework 連接到 MySQL 資料庫，您要安裝 MySQL 提供者。</span><span class="sxs-lookup"><span data-stu-id="48b6d-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="48b6d-160">若要這樣做，請開啟**Package Manager Console**和型別&quot; **Install-package MySql.Data.Entity 前**&quot;，然後按 Enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="48b6d-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="48b6d-161">這是發行前版本的組件，而且在這種情況可能包含 bug。</span><span class="sxs-lookup"><span data-stu-id="48b6d-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="48b6d-162">您不應該在生產環境中使用的提供者的發行前版本。</span><span class="sxs-lookup"><span data-stu-id="48b6d-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="48b6d-163">[按一下以展開圖]。</span><span class="sxs-lookup"><span data-stu-id="48b6d-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="48b6d-164">對您的應用程式的 Web.config 檔案中的專案組態變更</span><span class="sxs-lookup"><span data-stu-id="48b6d-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="48b6d-165">在本節中，您將設定 Entity Framework 使用您剛才安裝的 MySQL 提供者，註冊 MySQL 提供者處理站，並加入您的連接字串從 Azure。</span><span class="sxs-lookup"><span data-stu-id="48b6d-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="48b6d-166">下列範例包含 MySql.Data.dll 特定組件版本。</span><span class="sxs-lookup"><span data-stu-id="48b6d-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="48b6d-167">如果組件版本變更，您必須修改適當的組態設定，以正確的版本。</span><span class="sxs-lookup"><span data-stu-id="48b6d-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="48b6d-168">Visual Studio 2013 中開啟您專案的 Web.config 檔。</span><span class="sxs-lookup"><span data-stu-id="48b6d-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="48b6d-169">找出 Entity Framework 定義的預設資料庫提供者和 factory 的下列組態設定：</span><span class="sxs-lookup"><span data-stu-id="48b6d-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="48b6d-170">取代下列程式碼，將設定為使用 MySQL 提供者的 Entity Framework 中的這些組態設定：</span><span class="sxs-lookup"><span data-stu-id="48b6d-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="48b6d-171">找出&lt;connectionStrings&gt;區段並取代為下列程式碼，將會定義在 Azure 裝載您的 MySQL 資料庫的連接字串 (請注意 providerName 值從也有所變更原始）：</span><span class="sxs-lookup"><span data-stu-id="48b6d-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="48b6d-172">加入自訂 MigrationHistory 內容</span><span class="sxs-lookup"><span data-stu-id="48b6d-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="48b6d-173">使用 entity Framework Code First **MigrationHistory**資料表追蹤的模型變更，並確保資料庫結構描述與概念結構描述之間的一致性。</span><span class="sxs-lookup"><span data-stu-id="48b6d-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="48b6d-174">不過，這個表格不適用於 MySQL 預設因為太大的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="48b6d-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="48b6d-175">若要補救這種情況下，您必須針對該資料表索引鍵的大小縮減。</span><span class="sxs-lookup"><span data-stu-id="48b6d-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="48b6d-176">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="48b6d-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="48b6d-177">此資料表的結構描述資訊中擷取**HistoryContext**，可能會修改任何其他**DbContext**。</span><span class="sxs-lookup"><span data-stu-id="48b6d-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="48b6d-178">若要這樣做，請將加入新的類別檔案命名為**MySqlHistoryContext.cs**至專案，並將其內容取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="48b6d-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="48b6d-179">接下來您將需要設定 Entity Framework 使用修改後**HistoryContext**，而不是預設值。</span><span class="sxs-lookup"><span data-stu-id="48b6d-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="48b6d-180">利用程式碼為基礎的組態功能可以完成此動作。</span><span class="sxs-lookup"><span data-stu-id="48b6d-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="48b6d-181">若要這樣做，請將加入新的類別檔案命名為**MySqlConfiguration.cs**至您的專案，並取代其內容：</span><span class="sxs-lookup"><span data-stu-id="48b6d-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="48b6d-182">建立 ApplicationDbContext 自訂 EntityFramework 初始設定式</span><span class="sxs-lookup"><span data-stu-id="48b6d-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="48b6d-183">在本教學課程顯示為精選 MySQL 提供者目前不支援 Entity Framework 移轉，因此您必須使用模型初始設定式，才能連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="48b6d-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="48b6d-184">因為本教學課程在 Azure 上使用的 MySQL 執行個體，您必須建立自訂的 Entity Framework 初始設定式。</span><span class="sxs-lookup"><span data-stu-id="48b6d-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="48b6d-185">如果您要連接到 Azure，或如果您使用內部部署裝載的資料庫上的 SQL Server 執行個體，就不需要此步驟。</span><span class="sxs-lookup"><span data-stu-id="48b6d-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="48b6d-186">若要建立 MySQL 自訂 Entity Framework 初始設定式，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="48b6d-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="48b6d-187">加入新的類別檔案命名為**MySqlInitializer.cs**很至專案，並取代為下列程式碼的內容：</span><span class="sxs-lookup"><span data-stu-id="48b6d-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="48b6d-188">開啟**IdentityModels.cs**檔案位於您專案**模型**目錄，並以下列內容取代它的內容：</span><span class="sxs-lookup"><span data-stu-id="48b6d-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="48b6d-189">執行應用程式，而且正在驗證資料庫</span><span class="sxs-lookup"><span data-stu-id="48b6d-189">Running the application and verifying the database</span></span>

<span data-ttu-id="48b6d-190">當您完成前面幾節中的步驟時，您應該測試您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="48b6d-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="48b6d-191">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="48b6d-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="48b6d-192">按**Ctrl + F5**建置並執行 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="48b6d-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="48b6d-193">按一下**註冊**的頁面頂端的索引標籤：</span><span class="sxs-lookup"><span data-stu-id="48b6d-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="48b6d-194">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-194">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-195">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="48b6d-196">輸入新的使用者名稱和密碼，然後再按一下**註冊**:</span><span class="sxs-lookup"><span data-stu-id="48b6d-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="48b6d-197">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-197">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-198">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="48b6d-199">此時將 ASP.NET Identity 資料表建立的 MySQL 資料庫，並註冊並登入應用程式使用者：</span><span class="sxs-lookup"><span data-stu-id="48b6d-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="48b6d-200">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-200">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-201">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="48b6d-202">安裝 MySQL Workbench 工具來確認資料</span><span class="sxs-lookup"><span data-stu-id="48b6d-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="48b6d-203">安裝**MySQL Workbench**工具[MySQL 下載頁面](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="48b6d-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="48b6d-204">在安裝精靈：**特徵選取**索引標籤上，選取**MySQL Workbench**下**應用程式**> 一節。</span><span class="sxs-lookup"><span data-stu-id="48b6d-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="48b6d-205">啟動應用程式，並加入新的連接使用連接字串中的資料在本教學課程的供您建立 Azure 的 MySQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="48b6d-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="48b6d-206">建立連接後，檢查**ASP.NET Identity**資料表上建立**IdentityMySQLDatabase。**</span><span class="sxs-lookup"><span data-stu-id="48b6d-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="48b6d-207">您會看到所有 ASP.NET 識別所都需資料表會建立在下列影像所示：</span><span class="sxs-lookup"><span data-stu-id="48b6d-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="48b6d-208">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-208">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-209">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="48b6d-210">檢查**aspnetusers**資料表執行個體若要檢查的項目，當您註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="48b6d-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="48b6d-211">[按一下以展開圖。</span><span class="sxs-lookup"><span data-stu-id="48b6d-211">[Click the following image to expand it.</span></span> <span data-ttu-id="48b6d-212">]</span><span class="sxs-lookup"><span data-stu-id="48b6d-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
