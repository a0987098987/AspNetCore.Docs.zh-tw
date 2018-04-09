---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: 將資料庫角色成員資格部署到測試環境 |Microsoft 文件
author: jrjlee
description: 本主題描述如何將使用者帳戶新增至資料庫角色做為方案部署到測試環境的一部分。 當您部署方案，包含...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 4f635153213b0695d7d4b64d09adefaf8ee8e892
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a><span data-ttu-id="cff02-104">將資料庫角色成員資格部署到測試環境</span><span class="sxs-lookup"><span data-stu-id="cff02-104">Deploying Database Role Memberships to Test Environments</span></span>
====================
<span data-ttu-id="cff02-105">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="cff02-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="cff02-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="cff02-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="cff02-107">本主題描述如何將使用者帳戶新增至資料庫角色做為方案部署到測試環境的一部分。</span><span class="sxs-lookup"><span data-stu-id="cff02-107">This topic describes how to add user accounts to database roles as part of a solution deployment to a test environment.</span></span>
> 
> <span data-ttu-id="cff02-108">當您部署包含要預備或生產環境的資料庫專案的方案時，您通常不想自動加入至資料庫角色的使用者帳戶的開發人員。</span><span class="sxs-lookup"><span data-stu-id="cff02-108">When you deploy a solution containing a database project to a staging or production environment, you typically don't want the developer to automate the addition of user accounts to database roles.</span></span> <span data-ttu-id="cff02-109">在大部分情況下，開發人員不知道要新增至哪一個資料庫角色、 哪些使用者帳戶，這些需求可能會隨時變更。</span><span class="sxs-lookup"><span data-stu-id="cff02-109">In most cases, the developer won't know which user accounts need to be added to which database roles, and these requirements could change at any time.</span></span> <span data-ttu-id="cff02-110">不過，當您部署方案，包含資料庫專案開發或測試環境，這種情況通常是而不同：</span><span class="sxs-lookup"><span data-stu-id="cff02-110">However, when you deploy a solution containing a database project to a development or test environment, the situation is usually rather different:</span></span>
> 
> - <span data-ttu-id="cff02-111">開發人員通常重新部署上定期執行，解決方案通常數次一天。</span><span class="sxs-lookup"><span data-stu-id="cff02-111">The developer typically re-deploys the solution on a regular basis, often several times a day.</span></span>
> - <span data-ttu-id="cff02-112">通常每個部署，這表示必須建立資料庫使用者，而且每次部署之後加入至角色上重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="cff02-112">The database is typically re-created on every deployment, which means that database users must be created and added to roles after every deployment.</span></span>
> - <span data-ttu-id="cff02-113">開發人員通常有目標開發或測試環境的完整控制權。</span><span class="sxs-lookup"><span data-stu-id="cff02-113">The developer typically has full control over the target development or test environment.</span></span>
> 
> <span data-ttu-id="cff02-114">在此案例中，通常很有幫助自動建立資料庫使用者，並將資料庫角色成員資格指派做為部署程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="cff02-114">In this scenario, it's often beneficial to automatically create database users and assign database role memberships as part of the deployment process.</span></span>
> 
> <span data-ttu-id="cff02-115">關鍵因素是，這項作業必須是有條件根據目標環境。</span><span class="sxs-lookup"><span data-stu-id="cff02-115">The key factor is that this operation needs to be conditional based on the target environment.</span></span> <span data-ttu-id="cff02-116">如果您要部署至預備或生產環境中，您會想要跳過此作業。</span><span class="sxs-lookup"><span data-stu-id="cff02-116">If you're deploying to a staging or a production environment, you want to skip the operation.</span></span> <span data-ttu-id="cff02-117">如果您要部署的開發人員或測試環境，您會想要部署不需要其他介入的角色成員資格。</span><span class="sxs-lookup"><span data-stu-id="cff02-117">If you're deploying to a developer or test environment, you want to deploy role memberships without further intervention.</span></span> <span data-ttu-id="cff02-118">本主題說明您可以使用來解決這個問題的其中一個方法。</span><span class="sxs-lookup"><span data-stu-id="cff02-118">This topic describes one approach you can use to address this challenge.</span></span>


<span data-ttu-id="cff02-119">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="cff02-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="cff02-120">這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="cff02-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="cff02-121">在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。</span><span class="sxs-lookup"><span data-stu-id="cff02-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="cff02-122">工作概觀</span><span class="sxs-lookup"><span data-stu-id="cff02-122">Task Overview</span></span>

<span data-ttu-id="cff02-123">本主題假設：</span><span class="sxs-lookup"><span data-stu-id="cff02-123">This topic assumes that:</span></span>

- <span data-ttu-id="cff02-124">中所述，使用方案部署的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。</span><span class="sxs-lookup"><span data-stu-id="cff02-124">You use the split project file approach to solution deployment, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span>
- <span data-ttu-id="cff02-125">中所述，從專案檔來部署資料庫專案中，呼叫 VSDBCMD[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="cff02-125">You call VSDBCMD from the project file to deploy your database project, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

<span data-ttu-id="cff02-126">若要建立資料庫使用者，並指派角色成員資格，您將資料庫專案部署到測試環境時，您將需要：</span><span class="sxs-lookup"><span data-stu-id="cff02-126">To create database users and assign role memberships when you deploy a database project to a test environment, you'll need to:</span></span>

- <span data-ttu-id="cff02-127">建立會進行必要的資料庫變更的 Transact 結構化查詢語言 (TRANSACT-SQL) 指令碼。</span><span class="sxs-lookup"><span data-stu-id="cff02-127">Create a Transact Structured Query Language (Transact-SQL) script that makes the necessary database changes.</span></span>
- <span data-ttu-id="cff02-128">建立會使用 sqlcmd.exe 公用程式執行的 SQL 指令碼的 Microsoft Build Engine (MSBuild) 目標。</span><span class="sxs-lookup"><span data-stu-id="cff02-128">Create a Microsoft Build Engine (MSBuild) target that uses the sqlcmd.exe utility to run the SQL script.</span></span>
- <span data-ttu-id="cff02-129">設定您的專案檔案，來叫用的目標，當您要將方案部署至測試環境。</span><span class="sxs-lookup"><span data-stu-id="cff02-129">Configure your project files to invoke the target when you're deploying your solution to a test environment.</span></span>

<span data-ttu-id="cff02-130">本主題將說明如何執行這種程序。</span><span class="sxs-lookup"><span data-stu-id="cff02-130">This topic will show you how to perform each of these procedures.</span></span>

## <a name="scripting-the-database-role-memberships"></a><span data-ttu-id="cff02-131">指令碼的資料庫角色成員資格</span><span class="sxs-lookup"><span data-stu-id="cff02-131">Scripting the Database Role Memberships</span></span>

<span data-ttu-id="cff02-132">您可以在許多不同的方式，來建立 TRANSACT-SQL 指令碼，而且在您選擇的任何位置。</span><span class="sxs-lookup"><span data-stu-id="cff02-132">You can create a Transact-SQL script in a lot of different ways, and in any location you choose.</span></span> <span data-ttu-id="cff02-133">最簡單的方法是在 Visual Studio 2010 中建立您的方案內的指令碼。</span><span class="sxs-lookup"><span data-stu-id="cff02-133">The easiest approach is to create the script within your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="cff02-134">**若要建立 SQL 指令碼**</span><span class="sxs-lookup"><span data-stu-id="cff02-134">**To create a SQL script**</span></span>

1. <span data-ttu-id="cff02-135">在**方案總管 中**視窗中，展開資料庫專案節點。</span><span class="sxs-lookup"><span data-stu-id="cff02-135">In the **Solution Explorer** window, expand your database project node.</span></span>
2. <span data-ttu-id="cff02-136">以滑鼠右鍵按一下**指令碼**資料夾，指向**新增**，然後按一下 **新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="cff02-136">Right-click the **Scripts** folder, point to **Add**, and then click **New Folder**.</span></span>
3. <span data-ttu-id="cff02-137">型別**測試**做為資料夾名稱，然後按 Enter。</span><span class="sxs-lookup"><span data-stu-id="cff02-137">Type **Test** as the folder name, and then press Enter.</span></span>
4. <span data-ttu-id="cff02-138">以滑鼠右鍵按一下**測試**資料夾，指向**新增**，然後按一下 **指令碼**。</span><span class="sxs-lookup"><span data-stu-id="cff02-138">Right-click the **Test** folder, point to **Add**, and then click **Script**.</span></span>
5. <span data-ttu-id="cff02-139">在**加入新項目**對話方塊方塊中，為您的指令碼提供有意義的名稱 (例如， **AddRoleMemberships.sql**)，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="cff02-139">In the **Add New Item** dialog box, give your script a meaningful name (for example, **AddRoleMemberships.sql**), and then click **Add**.</span></span>

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. <span data-ttu-id="cff02-140">在*AddRoleMemberships.sql*檔案中，將 TRANSACT-SQL 陳述式加入的：</span><span class="sxs-lookup"><span data-stu-id="cff02-140">In the *AddRoleMemberships.sql* file, add Transact-SQL statements that:</span></span>

    1. <span data-ttu-id="cff02-141">建立資料庫使用者，將會存取您的資料庫之 SQL Server 登入。</span><span class="sxs-lookup"><span data-stu-id="cff02-141">Create a database user for the SQL Server login that will access your database.</span></span>
    2. <span data-ttu-id="cff02-142">將資料庫使用者加入至任何必要的資料庫角色。</span><span class="sxs-lookup"><span data-stu-id="cff02-142">Add the database user to any required database roles.</span></span>
7. <span data-ttu-id="cff02-143">檔案應與以下相似：</span><span class="sxs-lookup"><span data-stu-id="cff02-143">The file should resemble this:</span></span>

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. <span data-ttu-id="cff02-144">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="cff02-144">Save the file.</span></span>

## <a name="executing-the-script-on-the-target-database"></a><span data-ttu-id="cff02-145">在目標資料庫上執行指令碼</span><span class="sxs-lookup"><span data-stu-id="cff02-145">Executing the Script on the Target Database</span></span>

<span data-ttu-id="cff02-146">在理想情況下，您會執行任何必要的 TRANSACT-SQL 指令碼做為部署後指令碼的一部分，當您部署資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="cff02-146">Ideally, you'd run any required Transact-SQL scripts as part of a post-deployment script when you deploy your database project.</span></span> <span data-ttu-id="cff02-147">不過，部署後指令碼不允許您執行條件式方案組態或組建屬性上的邏輯。</span><span class="sxs-lookup"><span data-stu-id="cff02-147">However, post-deployment scripts don't allow you to execute logic conditionally based on solution configurations or build properties.</span></span> <span data-ttu-id="cff02-148">替代方法是直接從 MSBuild 專案檔中，執行您的 SQL 指令碼，藉由建立**目標**執行 sqlcmd.exe 命令的項目。</span><span class="sxs-lookup"><span data-stu-id="cff02-148">The alternative is to run your SQL scripts directly from the MSBuild project file, by creating a **Target** element that executes a sqlcmd.exe command.</span></span> <span data-ttu-id="cff02-149">您可以使用此命令在目標資料庫上執行您的指令碼：</span><span class="sxs-lookup"><span data-stu-id="cff02-149">You can use this command to run your script on the target database:</span></span>


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> <span data-ttu-id="cff02-150">如需有關 sqlcmd 命令列選項的詳細資訊，請參閱[sqlcmd 公用程式](https://msdn.microsoft.com/library/ms162773.aspx)。</span><span class="sxs-lookup"><span data-stu-id="cff02-150">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>


<span data-ttu-id="cff02-151">此命令嵌入 MSBuild 目標之前，您需要考慮在哪些情況下您想要執行的指令碼：</span><span class="sxs-lookup"><span data-stu-id="cff02-151">Before you embed this command in an MSBuild target, you need to consider under what conditions you want the script to run:</span></span>

- <span data-ttu-id="cff02-152">目標資料庫必須存在才能變更其角色成員資格。</span><span class="sxs-lookup"><span data-stu-id="cff02-152">The target database must exist before you change its role memberships.</span></span> <span data-ttu-id="cff02-153">因此，您需要執行此指令碼*之後*資料庫部署。</span><span class="sxs-lookup"><span data-stu-id="cff02-153">As such, you need to run this script *after* the database deployment.</span></span>
- <span data-ttu-id="cff02-154">您必須包含一個條件，以便在測試環境，才會執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="cff02-154">You need to include a condition so that the script is only executed for test environments.</span></span>
- <span data-ttu-id="cff02-155">如果您執行 「 假設 」 部署&#x2014;換句話說，如果您正在產生部署指令碼，但實際上不會執行這些&#x2014;您不應該執行的 SQL 指令碼。</span><span class="sxs-lookup"><span data-stu-id="cff02-155">If you're running a "what if" deployment&#x2014;in other words, if you're generating deployment scripts but not actually running them&#x2014;you shouldn't run the SQL script.</span></span>

<span data-ttu-id="cff02-156">如果您使用分割專案檔案描述的方法中[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，所示連絡人管理員範例方案，您可以將組建指示分割 SQL 指令碼如下：</span><span class="sxs-lookup"><span data-stu-id="cff02-156">If you're using the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), as demonstrated by the Contact Manager sample solution, you can split the build instructions for your SQL script like this:</span></span>

- <span data-ttu-id="cff02-157">任何所需的環境特有的屬性，以及決定是否要將權限，部署的屬性應該放在特定環境的專案檔 (例如， *Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="cff02-157">Any required environment-specific properties, together with the property that determines whether to deploy permissions, should go in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>
- <span data-ttu-id="cff02-158">MSBuild 目標本身，以及目的地環境之間不會變更任何屬性都應在通用專案檔 (例如， *Publish.proj*)。</span><span class="sxs-lookup"><span data-stu-id="cff02-158">The MSBuild target itself, together with any properties that will not change between destination environments, should go in the universal project file (for example, *Publish.proj*).</span></span>

<span data-ttu-id="cff02-159">在特定環境的專案檔中，您需要定義的資料庫伺服器名稱、 目標資料庫名稱，以及讓使用者可以指定是否要部署角色成員資格的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="cff02-159">In the environment-specific project file, you need to define the database server name, the target database name, and a Boolean property that lets the user specify whether to deploy role memberships.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


<span data-ttu-id="cff02-160">在通用專案檔中，您需要提供 sqlcmd 可執行檔的位置以及您想要執行的 SQL 指令碼的位置。</span><span class="sxs-lookup"><span data-stu-id="cff02-160">In the universal project file, you need to provide the location of the sqlcmd executable and the location of the SQL script you want to run.</span></span> <span data-ttu-id="cff02-161">這些屬性會維持不變，不論目的環境。</span><span class="sxs-lookup"><span data-stu-id="cff02-161">These properties will remain the same regardless of the destination environment.</span></span> <span data-ttu-id="cff02-162">您也需要建立 MSBuild 目標執行 sqlcmd 命令。</span><span class="sxs-lookup"><span data-stu-id="cff02-162">You also need to create an MSBuild target to execute the sqlcmd command.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


<span data-ttu-id="cff02-163">請注意 sqlcmd 可執行檔的位置加入為靜態屬性，這可能是其他目標很有用。</span><span class="sxs-lookup"><span data-stu-id="cff02-163">Notice that you add the location of the sqlcmd executable as a static property, as this could be useful to other targets.</span></span> <span data-ttu-id="cff02-164">相反地，您定義 SQL 指令碼的位置和 sqlcmd 命令的語法為動態屬性內目標，因為它們不會需要執行目標之前。</span><span class="sxs-lookup"><span data-stu-id="cff02-164">In contrast, you define the location of your SQL script and the syntax of the sqlcmd command as dynamic properties within the target, as they will not be required before the target is executed.</span></span> <span data-ttu-id="cff02-165">在此情況下， **DeployTestDBPermissions**如果符合這些條件，則只會執行目標：</span><span class="sxs-lookup"><span data-stu-id="cff02-165">In this case, the **DeployTestDBPermissions** target will only be executed if these conditions are met:</span></span>

- <span data-ttu-id="cff02-166">**DeployTestDBRoleMemberships**屬性設定為**true**。</span><span class="sxs-lookup"><span data-stu-id="cff02-166">The **DeployTestDBRoleMemberships** property is set to **true**.</span></span>
- <span data-ttu-id="cff02-167">使用者尚未指定**WhatIf = true**旗標。</span><span class="sxs-lookup"><span data-stu-id="cff02-167">The user hasn't specified a **WhatIf=true** flag.</span></span>

<span data-ttu-id="cff02-168">最後，別忘了要叫用的目標。</span><span class="sxs-lookup"><span data-stu-id="cff02-168">Finally, don't forget to invoke the target.</span></span> <span data-ttu-id="cff02-169">在*Publish.proj*檔案中，您可以將目標加入至預設值的相依性清單**FullPublish**目標。</span><span class="sxs-lookup"><span data-stu-id="cff02-169">In the *Publish.proj* file, you can do this by adding the target to the dependency list for the default **FullPublish** target.</span></span> <span data-ttu-id="cff02-170">您必須確保**DeployTestDBPermissions**目標不會執行，直到**PublishDbPackages**在執行目標。</span><span class="sxs-lookup"><span data-stu-id="cff02-170">You need to ensure that the **DeployTestDBPermissions** target is not executed until the **PublishDbPackages** target has been executed.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a><span data-ttu-id="cff02-171">結論</span><span class="sxs-lookup"><span data-stu-id="cff02-171">Conclusion</span></span>

<span data-ttu-id="cff02-172">本主題描述可以在其中您加入資料庫使用者和角色成員資格做為部署後動作當您部署資料庫專案的一種方法。</span><span class="sxs-lookup"><span data-stu-id="cff02-172">This topic described one way in which you can add database users and role memberships as a post-deployment action when you deploy a database project.</span></span> <span data-ttu-id="cff02-173">當您定期重新建立資料庫，以在測試環境中，但它應該通常避免您將資料庫部署到預備環境或生產環境時，這是通常很有用。</span><span class="sxs-lookup"><span data-stu-id="cff02-173">This is typically useful when you regularly re-create a database in a test environment, but it should usually be avoided when you deploy databases to staging or production environments.</span></span> <span data-ttu-id="cff02-174">因此，您應該確定您使用的是必要的條件式邏輯，使資料庫使用者和角色成員資格，才會建立適合這樣做的時機。</span><span class="sxs-lookup"><span data-stu-id="cff02-174">As such, you should ensure that you use the necessary conditional logic so that database users and role memberships are only created when it's appropriate to do so.</span></span>

## <a name="further-reading"></a><span data-ttu-id="cff02-175">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="cff02-175">Further Reading</span></span>

<span data-ttu-id="cff02-176">如需有關使用 VSDBCMD 部署資料庫專案的詳細資訊，請參閱[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="cff02-176">For more information on using VSDBCMD to deploy database projects, see [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="cff02-177">如需自訂不同的目標環境的資料庫部署的指引，請參閱[自訂資料庫部署多個環境](customizing-database-deployments-for-multiple-environments.md)。</span><span class="sxs-lookup"><span data-stu-id="cff02-177">For guidance on customizing database deployments for different target environments, see [Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="cff02-178">如需使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="cff02-178">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="cff02-179">如需有關 sqlcmd 命令列選項的詳細資訊，請參閱[sqlcmd 公用程式](https://msdn.microsoft.com/library/ms162773.aspx)。</span><span class="sxs-lookup"><span data-stu-id="cff02-179">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cff02-180">[上一頁](customizing-database-deployments-for-multiple-environments.md)
> [下一頁](deploying-membership-databases-to-enterprise-environments.md)</span><span class="sxs-lookup"><span data-stu-id="cff02-180">[Previous](customizing-database-deployments-for-multiple-environments.md)
[Next](deploying-membership-databases-to-enterprise-environments.md)</span></span>
