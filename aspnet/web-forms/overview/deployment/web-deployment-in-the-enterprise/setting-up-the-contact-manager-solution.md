---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: 設定連絡管理員解決方案 |Microsoft Docs
author: jrjlee
description: 本主題描述如何下載及設定連絡管理員解決方案開發人員工作站上本機執行。
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 479dbb8d2edbe9fb953ea9e1312ffb8fdbd3e2fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802127"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="95143-103">設定連絡管理員解決方案</span><span class="sxs-lookup"><span data-stu-id="95143-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="95143-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="95143-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="95143-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="95143-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="95143-106">本主題描述如何下載及設定連絡管理員解決方案開發人員工作站上本機執行。</span><span class="sxs-lookup"><span data-stu-id="95143-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="95143-107">系統需求</span><span class="sxs-lookup"><span data-stu-id="95143-107">System Requirements</span></span>

<span data-ttu-id="95143-108">若要在本機執行連絡管理員解決方案，並在執行本教學課程中所述的其他工作，您將需要在開發人員工作站上安裝此軟體：</span><span class="sxs-lookup"><span data-stu-id="95143-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="95143-109">Visual Studio 2010 Service Pack 1、 Premium 或 Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="95143-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="95143-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="95143-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="95143-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="95143-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="95143-112">IIS Web Deployment Tool (Web Deploy) 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="95143-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="95143-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="95143-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="95143-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="95143-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="95143-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="95143-115">.NET Framework 4</span></span>
- <span data-ttu-id="95143-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="95143-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="95143-117">除了 Visual Studio 2010 中，您可以下載並安裝最新版本的所有這些產品和元件，透過[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="95143-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="95143-118">下載並解壓縮方案</span><span class="sxs-lookup"><span data-stu-id="95143-118">Download and Extract the Solution</span></span>

<span data-ttu-id="95143-119">您可以從 MSDN Code Gallery 中的 Contact Manager 範例應用程式[此處](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0)。</span><span class="sxs-lookup"><span data-stu-id="95143-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="95143-120">設定和執行解決方案</span><span class="sxs-lookup"><span data-stu-id="95143-120">Configure and Run the Solution</span></span>

<span data-ttu-id="95143-121">若要設定及執行您的本機電腦上的 連絡管理員解決方案，您將需要執行下列高階步驟：</span><span class="sxs-lookup"><span data-stu-id="95143-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="95143-122">如果您還沒有，請建立本機的 ASP.NET 應用程式服務資料庫已啟用的成員資格和角色管理功能。</span><span class="sxs-lookup"><span data-stu-id="95143-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="95143-123">編輯中的連接字串*web.config*指向本機 SQL Server Express 執行個體的檔案。</span><span class="sxs-lookup"><span data-stu-id="95143-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="95143-124">從 Visual Studio 2010 中執行方案。</span><span class="sxs-lookup"><span data-stu-id="95143-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="95143-125">本節的其餘部分會提供有關如何完成這些工作的詳細指引。</span><span class="sxs-lookup"><span data-stu-id="95143-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="95143-126">**若要建立應用程式服務資料庫**</span><span class="sxs-lookup"><span data-stu-id="95143-126">**To create the application services database**</span></span>

1. <span data-ttu-id="95143-127">開啟 Visual Studio 2010 命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="95143-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="95143-128">若要這樣做，請在**開始**功能表上，指向**所有程式**，按一下  **Microsoft Visual Studio 2010**，按一下  **Visual Studio Tools**，然後按一下  **Visual Studio 命令提示字元 (2010)**。</span><span class="sxs-lookup"><span data-stu-id="95143-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="95143-129">在命令提示字元中，輸入下列命令，並按 Enter:</span><span class="sxs-lookup"><span data-stu-id="95143-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="95143-130">使用 **– C**參數來指定您的資料庫伺服器的連接字串。</span><span class="sxs-lookup"><span data-stu-id="95143-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="95143-131">使用 **–** 參數來指定應用程式服務的功能，您想要新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="95143-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="95143-132">在此情況下， **m**指出您想要新增的成員資格提供者支援並**r**指出您想要新增的角色管理員的支援。</span><span class="sxs-lookup"><span data-stu-id="95143-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="95143-133">使用 **– d**參數來指定您的應用程式服務資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="95143-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="95143-134">如果您省略此參數，此公用程式會建立資料庫的預設名稱**aspnetdb**。</span><span class="sxs-lookup"><span data-stu-id="95143-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="95143-135">當成功建立資料庫時，命令提示字元會顯示確認訊息。</span><span class="sxs-lookup"><span data-stu-id="95143-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="95143-136">如需有關 aspnet\_regsql 公用程式，請參閱 < [ASP.NET SQL Server 註冊工具 (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="95143-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="95143-137">下一個步驟是確定連絡管理員解決方案中的連接字串指向您的 SQL Server Express 的本機執行個體。</span><span class="sxs-lookup"><span data-stu-id="95143-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="95143-138">**若要更新連接字串**</span><span class="sxs-lookup"><span data-stu-id="95143-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="95143-139">Visual Studio 2010 中開啟連絡管理員解決方案。</span><span class="sxs-lookup"><span data-stu-id="95143-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="95143-140">在 **方案總管** 視窗中，展開**ContactManager.Mvc**專案，然後再連按兩下  **Web.config**節點。</span><span class="sxs-lookup"><span data-stu-id="95143-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="95143-141">ContactManager.Mvc 專案包含兩個*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="95143-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="95143-142">您要編輯專案層級檔案。</span><span class="sxs-lookup"><span data-stu-id="95143-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="95143-143">在  **connectionStrings**項目，請確認連接字串名為**ApplicationServices**指向您的本機 ASP.NET 應用程式服務資料庫。</span><span class="sxs-lookup"><span data-stu-id="95143-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="95143-144">在 **方案總管** 視窗中，展開**ContactManager.Service**專案，然後再連按兩下  **Web.config**節點。</span><span class="sxs-lookup"><span data-stu-id="95143-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="95143-145">在  **connectionStrings**項目，名為連接字串中的**ContactManagerContext**，確認**資料來源**屬性設定為本機執行個體的 SQLServer Express。</span><span class="sxs-lookup"><span data-stu-id="95143-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="95143-146">您不需要變更連接字串中的其他任何項目。</span><span class="sxs-lookup"><span data-stu-id="95143-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="95143-147">儲存所有開啟的檔案。</span><span class="sxs-lookup"><span data-stu-id="95143-147">Save all open files.</span></span>

<span data-ttu-id="95143-148">您現在應該準備好要執行本機電腦上的 連絡管理員解決方案。</span><span class="sxs-lookup"><span data-stu-id="95143-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="95143-149">如果您遵循這些步驟不需要先建立應用程式服務資料庫時，ASP.NET 會在您嘗試建立的使用者第一次建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="95143-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="95143-150">不過，手動建立資料庫可讓您更多控制您想要支援的應用程式服務功能集。</span><span class="sxs-lookup"><span data-stu-id="95143-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="95143-151">**若要執行連絡管理員解決方案**</span><span class="sxs-lookup"><span data-stu-id="95143-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="95143-152">在 Visual Studio 2010 中，按下 F5。</span><span class="sxs-lookup"><span data-stu-id="95143-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="95143-153">Internet Explorer 會啟動，並要求連絡管理員 ASP.NET MVC 3 應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="95143-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="95143-154">根據預設，應用程式會顯示**所有連絡人**頁面。</span><span class="sxs-lookup"><span data-stu-id="95143-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="95143-155">新增幾個連絡人，然後確認 應用程式可以正常運作。</span><span class="sxs-lookup"><span data-stu-id="95143-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="95143-156">瀏覽至`http://localhost:50114/Account/Register`（如果您裝載不同的連接埠上的應用程式，請調整 URL）。</span><span class="sxs-lookup"><span data-stu-id="95143-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="95143-157">新增使用者名稱、 電子郵件地址和密碼，並驗證您能夠順利註冊帳戶。</span><span class="sxs-lookup"><span data-stu-id="95143-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="95143-158">瀏覽至`http://localhost:50114/Account/LogOn`（如果您裝載不同的連接埠上的應用程式，請調整 URL）。</span><span class="sxs-lookup"><span data-stu-id="95143-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="95143-159">請確認您能夠使用您剛才建立的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="95143-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="95143-160">關閉 Internet Explorer，以停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="95143-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="95143-161">結論</span><span class="sxs-lookup"><span data-stu-id="95143-161">Conclusion</span></span>

<span data-ttu-id="95143-162">此時，您應該完全在本機電腦上執行設定連絡管理員解決方案。</span><span class="sxs-lookup"><span data-stu-id="95143-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="95143-163">當您進行本教學課程中的其他主題，您可以使用解決方案做為參考。</span><span class="sxs-lookup"><span data-stu-id="95143-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="95143-164">下一個主題中，[了解專案檔](understanding-the-project-file.md)，說明如何使用自訂的 Microsoft Build Engine (MSBuild) 專案檔內連絡管理員解決方案，來控制部署程序。</span><span class="sxs-lookup"><span data-stu-id="95143-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="95143-165">[上一頁](the-contact-manager-solution.md)
> [下一頁](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="95143-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
