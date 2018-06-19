---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: 部署特定的組建 |Microsoft 文件
author: jrjlee
description: 本主題描述如何部署 web 封裝和資料庫指令碼從特定的前一個組建至新的目的地，像是預備或生產環境的流程圖...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 271d084b3c69016df5be28ada032973bf7fd5a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880033"
---
<a name="deploying-a-specific-build"></a><span data-ttu-id="16aa3-103">部署特定的組建</span><span class="sxs-lookup"><span data-stu-id="16aa3-103">Deploying a Specific Build</span></span>
====================
<span data-ttu-id="16aa3-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="16aa3-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="16aa3-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="16aa3-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="16aa3-106">本主題描述如何部署 web 封裝和資料庫指令碼至新的目的地，像是預備或生產環境特定的前一個組建。</span><span class="sxs-lookup"><span data-stu-id="16aa3-106">This topic describes how to deploy web packages and database scripts from a specific previous build to a new destination, like a staging or production environment.</span></span>


<span data-ttu-id="16aa3-107">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="16aa3-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="16aa3-108">這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，兩個專案檔案中的組建和部署程序控制由&#x2014;其中一個包含組建指示適用於每個目的地環境中和包含特定環境的建置和部署設定。</span><span class="sxs-lookup"><span data-stu-id="16aa3-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="16aa3-109">在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。</span><span class="sxs-lookup"><span data-stu-id="16aa3-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="16aa3-110">工作概觀</span><span class="sxs-lookup"><span data-stu-id="16aa3-110">Task Overview</span></span>

<span data-ttu-id="16aa3-111">到目前為止，在此教學課程的集合中的主題有著重於如何建置、 封裝和部署 web 應用程式和資料庫做為單一步驟的一部分，或自動化程序。</span><span class="sxs-lookup"><span data-stu-id="16aa3-111">Until now, the topics in this tutorial set have focused on how to build, package, and deploy web applications and databases as part of a single-step or automated process.</span></span> <span data-ttu-id="16aa3-112">不過，某些常見的情況下，您要從組建置放資料夾中的清單中選取您所部署的資源。</span><span class="sxs-lookup"><span data-stu-id="16aa3-112">However, in some common scenarios, you'll want to select the resources that you deploy from a list of builds in a drop folder.</span></span> <span data-ttu-id="16aa3-113">換句話說，最新的組建可能不是您想要部署的組建。</span><span class="sxs-lookup"><span data-stu-id="16aa3-113">In other words, the latest build may not be the build you want to deploy.</span></span>

<span data-ttu-id="16aa3-114">請考慮在先前的主題所述的持續整合 (CI) 案例[建立組建定義，支援部署](creating-a-build-definition-that-supports-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="16aa3-114">Consider the continuous integration (CI) scenario described in the previous topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="16aa3-115">您已建立組建定義 Team Foundation Server (TFS) 2010年中。</span><span class="sxs-lookup"><span data-stu-id="16aa3-115">You've created a build definition in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="16aa3-116">每當開發人員會將程式碼簽入 TFS，Team Build 會建置您的程式碼、 建立 web 封裝和資料庫的指令碼做為建置流程的一部分、 執行任何單元測試，和將您的資源部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="16aa3-116">Every time a developer checks code into TFS, Team Build will build your code, create web packages and database scripts as part of the build process, run any unit tests, and deploy your resources to a test environment.</span></span> <span data-ttu-id="16aa3-117">建立組建定義時所設定的保留原則，根據 TFS 將會保留特定數目的前一個組建。</span><span class="sxs-lookup"><span data-stu-id="16aa3-117">Depending on the retention policy you configured when you created the build definition, TFS will retain a certain number of previous builds.</span></span>

![](deploying-a-specific-build/_static/image1.png)

<span data-ttu-id="16aa3-118">現在，假設您已執行驗證和驗證對下列其中一種測試建置在測試環境中，您準備好應用程式部署至預備環境。</span><span class="sxs-lookup"><span data-stu-id="16aa3-118">Now, suppose you've performed verification and validation testing against one of these builds in your test environment, and you're ready to deploy your application to a staging environment.</span></span> <span data-ttu-id="16aa3-119">在此同時，開發人員可能已簽入新的程式碼。</span><span class="sxs-lookup"><span data-stu-id="16aa3-119">In the meantime, developers may have checked in new code.</span></span> <span data-ttu-id="16aa3-120">您不想重建方案，並將部署到預備環境中，您不想要將最新組建部署到預備環境。</span><span class="sxs-lookup"><span data-stu-id="16aa3-120">You don't want to rebuild the solution and deploy to the staging environment, and you don't want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="16aa3-121">相反地，您會想要部署您已驗證，並在測試伺服器上驗證特定組建。</span><span class="sxs-lookup"><span data-stu-id="16aa3-121">Instead, you want to deploy the specific build that you've verified and validated on the test servers.</span></span>

<span data-ttu-id="16aa3-122">若要達成此目的，您需要告知 Microsoft Build Engine (MSBuild) 尋找的網頁套件和特定的組建產生資料庫指令碼的位置。</span><span class="sxs-lookup"><span data-stu-id="16aa3-122">To accomplish this, you need to tell the Microsoft Build Engine (MSBuild) where to find the web packages and database scripts that a specific build generated.</span></span>

## <a name="overriding-the-outputroot-property"></a><span data-ttu-id="16aa3-123">覆寫 OutputRoot 屬性</span><span class="sxs-lookup"><span data-stu-id="16aa3-123">Overriding the OutputRoot Property</span></span>

<span data-ttu-id="16aa3-124">在[範例方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、 *Publish.proj*檔案宣告名為屬性**OutputRoot**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-124">In the [sample solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), the *Publish.proj* file declares a property named **OutputRoot**.</span></span> <span data-ttu-id="16aa3-125">顧名思義，這是根資料夾，其中包含建置流程所產生的所有項目。</span><span class="sxs-lookup"><span data-stu-id="16aa3-125">As the name suggests, this is the root folder that contains everything that the build process generates.</span></span> <span data-ttu-id="16aa3-126">在*Publish.proj*檔案，您所見， **OutputRoot**屬性參考到所有部署資源的根位置。</span><span class="sxs-lookup"><span data-stu-id="16aa3-126">In the *Publish.proj* file, you can see that the **OutputRoot** property refers to the root location for all deployment resources.</span></span>

> [!NOTE]
> <span data-ttu-id="16aa3-127">**OutputRoot**是常用的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="16aa3-127">**OutputRoot** is a commonly used property name.</span></span> <span data-ttu-id="16aa3-128">Visual C# 和 Visual Basic 專案檔也會宣告此屬性，將所有組建輸出的根位置。</span><span class="sxs-lookup"><span data-stu-id="16aa3-128">Visual C# and Visual Basic project files also declare this property to store the root location for all build outputs.</span></span>


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


<span data-ttu-id="16aa3-129">如果您想要部署 web 封裝和資料庫從不同位置的指令碼專案檔&#x2014;喜歡的前一個 TFS 組建輸出&#x2014;您只需要覆寫**OutputRoot**屬性。</span><span class="sxs-lookup"><span data-stu-id="16aa3-129">If you want your project file to deploy web packages and database scripts from a different location&#x2014;like the outputs of a previous TFS build&#x2014;you simply need to override the **OutputRoot** property.</span></span> <span data-ttu-id="16aa3-130">您應該設定的相關組建資料夾的屬性值 Team Build 的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="16aa3-130">You should set the property value to the relevant build folder on the Team Build server.</span></span> <span data-ttu-id="16aa3-131">如果您從命令列執行 MSBuild，您可以指定的值**OutputRoot**做為命令列引數：</span><span class="sxs-lookup"><span data-stu-id="16aa3-131">If you were running MSBuild from the command line, you could specify a value for **OutputRoot** as a command-line argument:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


<span data-ttu-id="16aa3-132">在實務上，不過，您也想要略過**建置**目標&#x2014;沒有點中建置您的方案，如果您不打算使用組建輸出。</span><span class="sxs-lookup"><span data-stu-id="16aa3-132">In practice, however, you'd also want to skip the **Build** target&#x2014;there's no point in building your solution if you don't plan to use the build outputs.</span></span> <span data-ttu-id="16aa3-133">您可以藉由指定您想要從命令列執行的目標來這麼做：</span><span class="sxs-lookup"><span data-stu-id="16aa3-133">You could do this by specifying the targets you want to execute from the command line:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


<span data-ttu-id="16aa3-134">不過，在大部分情況下，您需要建置您的部署邏輯到 TFS 組建定義。</span><span class="sxs-lookup"><span data-stu-id="16aa3-134">However, in most cases, you'll want to build your deployment logic into a TFS build definition.</span></span> <span data-ttu-id="16aa3-135">這可讓使用者與**組建排入佇列**觸發部署從任何 Visual Studio 安裝，以連線至 TFS 伺服器的權限。</span><span class="sxs-lookup"><span data-stu-id="16aa3-135">This enables users with the **Queue builds** permission to trigger the deployment from any Visual Studio installation with a connection to the TFS server.</span></span>

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a><span data-ttu-id="16aa3-136">建立組建定義，以便部署特定組建</span><span class="sxs-lookup"><span data-stu-id="16aa3-136">Creating a Build Definition to Deploy Specific Builds</span></span>

<span data-ttu-id="16aa3-137">下一個程序描述如何建立組建定義，可讓使用者使用單一命令的預備環境部署觸發程序。</span><span class="sxs-lookup"><span data-stu-id="16aa3-137">The next procedure describes how to create a build definition that enables users to trigger deployments to a staging environment with a single command.</span></span>

<span data-ttu-id="16aa3-138">在此情況下，您不想要實際建置任何組建定義&#x2014;您只想要自訂的專案檔中執行部署邏輯。</span><span class="sxs-lookup"><span data-stu-id="16aa3-138">In this case, you don't want the build definition to actually build anything&#x2014;you just want it to execute the deployment logic in your custom project file.</span></span> <span data-ttu-id="16aa3-139">*Publish.proj*檔案包含條件式邏輯，會略過**建置**目標如果檔案在 Team Build 中執行。</span><span class="sxs-lookup"><span data-stu-id="16aa3-139">The *Publish.proj* file includes conditional logic that skips the **Build** target if the file is running in Team Build.</span></span> <span data-ttu-id="16aa3-140">它會透過評估內建**BuildingInTeamBuild**屬性，會自動設為**true**如果您在 Team Build 中執行您的專案檔。</span><span class="sxs-lookup"><span data-stu-id="16aa3-140">It does this by evaluating the built-in **BuildingInTeamBuild** property, which is automatically set to **true** if you run your project file in Team Build.</span></span> <span data-ttu-id="16aa3-141">如此一來，您可以略過建置程序，並執行現有的組建部署至專案檔即可。</span><span class="sxs-lookup"><span data-stu-id="16aa3-141">As a result, you can skip the build process and simply run the project file to deploy an existing build.</span></span>

<span data-ttu-id="16aa3-142">**若要建立組建定義來手動觸發部署**</span><span class="sxs-lookup"><span data-stu-id="16aa3-142">**To create a build definition to trigger deployment manually**</span></span>

1. <span data-ttu-id="16aa3-143">在 Visual Studio 2010 中，在**Team Explorer**視窗中，展開您的 team 專案節點，以滑鼠右鍵按一下**建置**，然後按一下 **新增組建定義**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-143">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](deploying-a-specific-build/_static/image2.png)
2. <span data-ttu-id="16aa3-144">在**一般**索引標籤上，為指定的組建定義名稱 (例如， **DeployToStaging**) 和選擇性描述。</span><span class="sxs-lookup"><span data-stu-id="16aa3-144">On the **General** tab, give the build definition a name (for example, **DeployToStaging**) and an optional description.</span></span>
3. <span data-ttu-id="16aa3-145">在**觸發程序**索引標籤上，選取**手動-簽入不會觸發新組建**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-145">On the **Trigger** tab, select **Manual – Check-ins do not trigger a new build**.</span></span>
4. <span data-ttu-id="16aa3-146">在**組建預設值**索引標籤的**複製組建輸出複製至下列置放資料夾**方塊中，輸入 drop 資料夾的通用命名慣例 (UNC) 路徑 (例如，  **\\TFSBUILD\Drops**)。</span><span class="sxs-lookup"><span data-stu-id="16aa3-146">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](deploying-a-specific-build/_static/image3.png)
5. <span data-ttu-id="16aa3-147">在**程序**索引標籤的**建置流程檔**下拉式清單中，保留**DefaultTemplate.xaml**選取。</span><span class="sxs-lookup"><span data-stu-id="16aa3-147">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="16aa3-148">這是加入到所有新的 team 專案的預設建置流程範本。</span><span class="sxs-lookup"><span data-stu-id="16aa3-148">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="16aa3-149">在**建置流程參數**資料表中，按一下 [在**要建置的項目**資料列，然後再按一下**省略**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="16aa3-149">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](deploying-a-specific-build/_static/image4.png)
7. <span data-ttu-id="16aa3-150">在**要建置的項目**對話方塊中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-150">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="16aa3-151">在**類型的項目**下拉式清單中選取**MSBuild 專案檔**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-151">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
9. <span data-ttu-id="16aa3-152">檔案位置的自訂專案與您控制的部署程序，選取檔案，然後按一下瀏覽**確定**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-152">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](deploying-a-specific-build/_static/image5.png)
10. <span data-ttu-id="16aa3-153">在**要建置的項目**對話方塊中，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-153">In the **Items to Build** dialog box, click **OK**.</span></span>
11. <span data-ttu-id="16aa3-154">在**建置流程參數**資料表中，展開 **進階**> 一節。</span><span class="sxs-lookup"><span data-stu-id="16aa3-154">In the **Build process parameters** table, expand the **Advanced** section.</span></span>
12. <span data-ttu-id="16aa3-155">在**MSBuild 引數**資料列，指定您的環境特定專案檔的位置並將您的組建資料夾位置的預留位置：</span><span class="sxs-lookup"><span data-stu-id="16aa3-155">In the **MSBuild Arguments** row, specify the location of your environment-specific project file and add a placeholder for the location of your build folder:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="16aa3-156">您必須覆寫**OutputRoot**值每次組建排入佇列。</span><span class="sxs-lookup"><span data-stu-id="16aa3-156">You'll need to override the **OutputRoot** value every time you queue a build.</span></span> <span data-ttu-id="16aa3-157">這點會在下一個程序中說明。</span><span class="sxs-lookup"><span data-stu-id="16aa3-157">This is covered in the next procedure.</span></span>
13. <span data-ttu-id="16aa3-158">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="16aa3-158">Click **Save**.</span></span>

<span data-ttu-id="16aa3-159">當您觸發組建時，您需要更新**OutputRoot**屬性以指向您要部署的組建。</span><span class="sxs-lookup"><span data-stu-id="16aa3-159">When you trigger a build, you need to update the **OutputRoot** property to point to the build you want to deploy.</span></span>

<span data-ttu-id="16aa3-160">**若要部署的組建定義從特定的組建**</span><span class="sxs-lookup"><span data-stu-id="16aa3-160">**To deploy a specific build from a build definition**</span></span>

1. <span data-ttu-id="16aa3-161">在**Team Explorer**視窗中，以滑鼠右鍵按一下組建定義，然後**佇列新組建**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-161">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](deploying-a-specific-build/_static/image7.png)
2. <span data-ttu-id="16aa3-162">在**佇列組建**對話方塊**參數**索引標籤上，依序展開**進階**> 一節。</span><span class="sxs-lookup"><span data-stu-id="16aa3-162">In the **Queue Build** dialog box, on the **Parameters** tab, expand the **Advanced** section.</span></span>
3. <span data-ttu-id="16aa3-163">在**MSBuild 引數**資料列中，以取代值**OutputRoot**屬性與您的組建資料夾位置。</span><span class="sxs-lookup"><span data-stu-id="16aa3-163">In the **MSBuild Arguments** row, replace the value of the **OutputRoot** property with the location of your build folder.</span></span> <span data-ttu-id="16aa3-164">例如: </span><span class="sxs-lookup"><span data-stu-id="16aa3-164">For example:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="16aa3-165">請務必包含結尾斜線結尾的組建資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="16aa3-165">Be sure to include a trailing slash at the end of the path to your build folder.</span></span>
4. <span data-ttu-id="16aa3-166">按一下**佇列**。</span><span class="sxs-lookup"><span data-stu-id="16aa3-166">Click **Queue**.</span></span>

<span data-ttu-id="16aa3-167">當組建排入佇列，專案檔會將資料庫指令碼部署 web 封裝從組建置放資料夾中指定**OutputRoot**屬性。</span><span class="sxs-lookup"><span data-stu-id="16aa3-167">When you queue the build, the project file will deploy the database scripts and web packages from the build drop folder you specified in the **OutputRoot** property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="16aa3-168">結論</span><span class="sxs-lookup"><span data-stu-id="16aa3-168">Conclusion</span></span>

<span data-ttu-id="16aa3-169">本主題描述如何發行部署資源，例如 web 封裝和資料庫的指令碼，從上一個特定建置使用分割專案檔的部署模型。</span><span class="sxs-lookup"><span data-stu-id="16aa3-169">This topic described how to publish deployment resources, like web packages and database scripts, from a specific previous build using the split project file deployment model.</span></span> <span data-ttu-id="16aa3-170">它說明如何覆寫**OutputRoot**屬性以及如何部署邏輯併入 TFS 組建定義。</span><span class="sxs-lookup"><span data-stu-id="16aa3-170">It explained how to override the **OutputRoot** property and how to incorporate the deployment logic into a TFS build definition.</span></span>

## <a name="further-reading"></a><span data-ttu-id="16aa3-171">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="16aa3-171">Further Reading</span></span>

<span data-ttu-id="16aa3-172">如需有關如何建立組建定義的詳細資訊，請參閱[建立基本組建定義](https://msdn.microsoft.com/library/ms181716.aspx)和[定義建置流程](https://msdn.microsoft.com/library/ms181715.aspx)。</span><span class="sxs-lookup"><span data-stu-id="16aa3-172">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="16aa3-173">如需詳細指引佇列組建的詳細資訊，請參閱[組建排入佇列](https://msdn.microsoft.com/library/ms181722.aspx)。</span><span class="sxs-lookup"><span data-stu-id="16aa3-173">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16aa3-174">[上一頁](creating-a-build-definition-that-supports-deployment.md)
> [下一頁](configuring-permissions-for-team-build-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="16aa3-174">[Previous](creating-a-build-definition-that-supports-deployment.md)
[Next](configuring-permissions-for-team-build-deployment.md)</span></span>
