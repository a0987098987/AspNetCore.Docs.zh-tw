---
title: 持續整合與部署-使用 ASP.NET Core 和 Azure DevOps
author: CamSoper
description: 使用 ASP.NET Core 和 Azure 在 DevOps 中進行持續整合和部署
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: mvc, seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 5fdf52235b49119503885f92c370dc588e809ffe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655831"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="82b94-103">持續整合及部署</span><span class="sxs-lookup"><span data-stu-id="82b94-103">Continuous integration and deployment</span></span>

<span data-ttu-id="82b94-104">在上一章中，您已建立簡單摘要讀取器應用程式的本機 Git 存放庫。</span><span class="sxs-lookup"><span data-stu-id="82b94-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="82b94-105">在本章中，您會將該程式碼發佈至 GitHub 存放庫，並使用 Azure Pipelines 來建立 Azure DevOps Services 管線。</span><span class="sxs-lookup"><span data-stu-id="82b94-105">In this chapter, you'll publish that code to a GitHub repository and construct an Azure DevOps Services pipeline using Azure Pipelines.</span></span> <span data-ttu-id="82b94-106">管線會啟用應用程式的連續組建和部署。</span><span class="sxs-lookup"><span data-stu-id="82b94-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="82b94-107">任何對 GitHub 存放庫的認可都會觸發組建和部署至 Azure Web 應用程式的預備位置。</span><span class="sxs-lookup"><span data-stu-id="82b94-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="82b94-108">在本節中，您將完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="82b94-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="82b94-109">將應用程式的程式碼發佈至 GitHub</span><span class="sxs-lookup"><span data-stu-id="82b94-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="82b94-110">中斷本機 Git 部署的連線</span><span class="sxs-lookup"><span data-stu-id="82b94-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="82b94-111">建立 Azure DevOps 組織</span><span class="sxs-lookup"><span data-stu-id="82b94-111">Create an Azure DevOps organization</span></span>
* <span data-ttu-id="82b94-112">在 Azure DevOps Services 中建立 team 專案</span><span class="sxs-lookup"><span data-stu-id="82b94-112">Create a team project in Azure DevOps Services</span></span>
* <span data-ttu-id="82b94-113">建立組建定義</span><span class="sxs-lookup"><span data-stu-id="82b94-113">Create a build definition</span></span>
* <span data-ttu-id="82b94-114">建立發行管線</span><span class="sxs-lookup"><span data-stu-id="82b94-114">Create a release pipeline</span></span>
* <span data-ttu-id="82b94-115">將變更認可至 GitHub 並自動部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="82b94-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="82b94-116">檢查 Azure Pipelines 管線</span><span class="sxs-lookup"><span data-stu-id="82b94-116">Examine the Azure Pipelines pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="82b94-117">將應用程式的程式碼發佈至 GitHub</span><span class="sxs-lookup"><span data-stu-id="82b94-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="82b94-118">開啟瀏覽器視窗，然後流覽至 [`https://github.com`]。</span><span class="sxs-lookup"><span data-stu-id="82b94-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="82b94-119">按一下標頭中的 [ **+** ] 下拉式選單，然後選取 [**新增存放庫**]：</span><span class="sxs-lookup"><span data-stu-id="82b94-119">Click the **+** drop-down in the header, and select **New repository**:</span></span>

    ![GitHub 新增存放庫選項](media/cicd/github-new-repo.png)

1. <span data-ttu-id="82b94-121">在 [**擁有**者] 下拉式方塊中選取您的帳戶，然後在 [存放**庫名稱**] 文字方塊中輸入*簡單摘要讀取器*。</span><span class="sxs-lookup"><span data-stu-id="82b94-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="82b94-122">按一下 [**建立存放庫**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="82b94-123">開啟本機電腦的命令 shell。</span><span class="sxs-lookup"><span data-stu-id="82b94-123">Open your local machine's command shell.</span></span> <span data-ttu-id="82b94-124">流覽至儲存*簡單摘要讀取器*Git 儲存機制的目錄。</span><span class="sxs-lookup"><span data-stu-id="82b94-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="82b94-125">將現有的*來源*遠端重新命名為*上游*。</span><span class="sxs-lookup"><span data-stu-id="82b94-125">Rename the existing *origin* remote to *upstream*.</span></span> <span data-ttu-id="82b94-126">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="82b94-126">Execute the following command:</span></span>

    ```console
    git remote rename origin upstream
    ```

1. <span data-ttu-id="82b94-127">在 GitHub 上新增指向存放庫複本的新*原始*遠端。</span><span class="sxs-lookup"><span data-stu-id="82b94-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="82b94-128">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="82b94-128">Execute the following command:</span></span>

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. <span data-ttu-id="82b94-129">將您的本機 Git 存放庫發佈至新建立的 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="82b94-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="82b94-130">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="82b94-130">Execute the following command:</span></span>

    ```console
    git push -u origin master
    ```

1. <span data-ttu-id="82b94-131">開啟瀏覽器視窗，然後流覽至 [`https://github.com/<GitHub_username>/simple-feed-reader/`]。</span><span class="sxs-lookup"><span data-stu-id="82b94-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="82b94-132">驗證您的程式碼出現在 GitHub 存放庫中。</span><span class="sxs-lookup"><span data-stu-id="82b94-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="82b94-133">中斷本機 Git 部署的連線</span><span class="sxs-lookup"><span data-stu-id="82b94-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="82b94-134">使用下列步驟移除本機 Git 部署。</span><span class="sxs-lookup"><span data-stu-id="82b94-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="82b94-135">Azure Pipelines （Azure DevOps 服務）會取代並增強該功能。</span><span class="sxs-lookup"><span data-stu-id="82b94-135">Azure Pipelines (an Azure DevOps service) both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="82b94-136">開啟[Azure 入口網站](https://portal.azure.com/)，然後流覽至預備環境 *（mywebapp\<unique_number\>/Staging）* Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="82b94-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="82b94-137">在入口網站的搜尋方塊中輸入*預備*，可以快速地找到 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="82b94-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![預備 Web 應用程式搜尋詞彙](media/cicd/portal-search-box.png)

1. <span data-ttu-id="82b94-139">按一下 [**部署中心**]。</span><span class="sxs-lookup"><span data-stu-id="82b94-139">Click **Deployment Center**.</span></span> <span data-ttu-id="82b94-140">新的面板隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-140">A new panel appears.</span></span> <span data-ttu-id="82b94-141">按一下 **[中斷連線]** ，移除上一章中新增的本機 Git 原始檔控制設定。</span><span class="sxs-lookup"><span data-stu-id="82b94-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="82b94-142">按一下 [**是]** 按鈕，確認移除操作。</span><span class="sxs-lookup"><span data-stu-id="82b94-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="82b94-143">流覽至*mywebapp < unique_number >* App Service。</span><span class="sxs-lookup"><span data-stu-id="82b94-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="82b94-144">提醒您，入口網站的 [搜尋] 方塊可以用來快速找出 App Service。</span><span class="sxs-lookup"><span data-stu-id="82b94-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="82b94-145">按一下 [**部署中心**]。</span><span class="sxs-lookup"><span data-stu-id="82b94-145">Click **Deployment Center**.</span></span> <span data-ttu-id="82b94-146">新的面板隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-146">A new panel appears.</span></span> <span data-ttu-id="82b94-147">按一下 **[中斷連線]** ，移除上一章中新增的本機 Git 原始檔控制設定。</span><span class="sxs-lookup"><span data-stu-id="82b94-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="82b94-148">按一下 [**是]** 按鈕，確認移除操作。</span><span class="sxs-lookup"><span data-stu-id="82b94-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-an-azure-devops-organization"></a><span data-ttu-id="82b94-149">建立 Azure DevOps 組織</span><span class="sxs-lookup"><span data-stu-id="82b94-149">Create an Azure DevOps organization</span></span>

1. <span data-ttu-id="82b94-150">開啟瀏覽器，然後流覽至[Azure DevOps 組織建立 頁面](https://go.microsoft.com/fwlink/?LinkId=307137)。</span><span class="sxs-lookup"><span data-stu-id="82b94-150">Open a browser, and navigate to the [Azure DevOps organization creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="82b94-151">在 [**挑選易記名稱**] 文字方塊中輸入唯一的名稱，以形成用來存取 Azure DevOps 組織的 URL。</span><span class="sxs-lookup"><span data-stu-id="82b94-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your Azure DevOps organization.</span></span>
1. <span data-ttu-id="82b94-152">選取 [ **Git** ] 選項按鈕，因為程式碼裝載于 GitHub 存放庫中。</span><span class="sxs-lookup"><span data-stu-id="82b94-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="82b94-153">按一下 [繼續] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-153">Click the **Continue** button.</span></span> <span data-ttu-id="82b94-154">短暫等候之後，就會建立名為*myfirstproject 專案*的帳戶和 team 專案。</span><span class="sxs-lookup"><span data-stu-id="82b94-154">After a short wait, an account and a team project, named *MyFirstProject*, are created.</span></span>

    ![Azure DevOps 組織建立頁面](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="82b94-156">開啟確認電子郵件，表示 Azure DevOps 的組織和專案已準備好可供使用。</span><span class="sxs-lookup"><span data-stu-id="82b94-156">Open the confirmation email indicating that the Azure DevOps organization and project are ready for use.</span></span> <span data-ttu-id="82b94-157">按一下 [**啟動您的專案**] 按鈕：</span><span class="sxs-lookup"><span data-stu-id="82b94-157">Click the **Start your project** button:</span></span>

    ![啟動您的專案按鈕](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="82b94-159">瀏覽器會開啟 *\<account_name\>。 visualstudio.com*。</span><span class="sxs-lookup"><span data-stu-id="82b94-159">A browser opens to *\<account_name\>.visualstudio.com*.</span></span> <span data-ttu-id="82b94-160">按一下 [ *myfirstproject 專案*] 連結，開始設定專案的 DevOps 管線。</span><span class="sxs-lookup"><span data-stu-id="82b94-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-azure-pipelines-pipeline"></a><span data-ttu-id="82b94-161">設定 Azure Pipelines 管線</span><span class="sxs-lookup"><span data-stu-id="82b94-161">Configure the Azure Pipelines pipeline</span></span>

<span data-ttu-id="82b94-162">有三個不同的步驟可完成。</span><span class="sxs-lookup"><span data-stu-id="82b94-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="82b94-163">完成下列三節中的步驟，會產生可運作的 DevOps 管線。</span><span class="sxs-lookup"><span data-stu-id="82b94-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-azure-devops-access-to-the-github-repository"></a><span data-ttu-id="82b94-164">授與 Azure DevOps GitHub 存放庫的存取權</span><span class="sxs-lookup"><span data-stu-id="82b94-164">Grant Azure DevOps access to the GitHub repository</span></span>

1. <span data-ttu-id="82b94-165">展開**或從外部存放庫中建立程式碼可**折疊。</span><span class="sxs-lookup"><span data-stu-id="82b94-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="82b94-166">按一下 [**安裝組建**] 按鈕：</span><span class="sxs-lookup"><span data-stu-id="82b94-166">Click the **Setup Build** button:</span></span>

    ![設定組建按鈕](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="82b94-168">從 [**選取來源**] 區段中選取 [ **GitHub** ] 選項：</span><span class="sxs-lookup"><span data-stu-id="82b94-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![選取來源-GitHub](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="82b94-170">需要授權，Azure DevOps 才能存取您的 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="82b94-170">Authorization is required before Azure DevOps can access your GitHub repository.</span></span> <span data-ttu-id="82b94-171">在 連線**名稱** 文字方塊中，輸入 *< GitHub_username > GitHub 連接*。</span><span class="sxs-lookup"><span data-stu-id="82b94-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="82b94-172">例如：</span><span class="sxs-lookup"><span data-stu-id="82b94-172">For example:</span></span>

    ![GitHub 連接名稱](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="82b94-174">如果已在您的 GitHub 帳戶上啟用雙因素驗證，則需要個人存取權杖。</span><span class="sxs-lookup"><span data-stu-id="82b94-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="82b94-175">在此情況下，請按一下 [**使用 GitHub 個人存取權杖進行授權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="82b94-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="82b94-176">如需協助，請參閱[官方 GitHub 個人存取權杖建立指示](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)。</span><span class="sxs-lookup"><span data-stu-id="82b94-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="82b94-177">只需要許可權的存放*庫範圍。*</span><span class="sxs-lookup"><span data-stu-id="82b94-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="82b94-178">否則，請按一下 [**使用 OAuth 授權**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="82b94-179">出現提示時，請登入您的 GitHub 帳戶。</span><span class="sxs-lookup"><span data-stu-id="82b94-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="82b94-180">然後選取 [授權]，將存取權授與您的 Azure DevOps 組織。</span><span class="sxs-lookup"><span data-stu-id="82b94-180">Then select Authorize to grant access to your Azure DevOps organization.</span></span> <span data-ttu-id="82b94-181">如果成功，就會建立新的服務端點。</span><span class="sxs-lookup"><span data-stu-id="82b94-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="82b94-182">按一下 [存放**庫**] 按鈕旁的省略號按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="82b94-183">從清單中選取  *< GitHub_username >/simple-feed-reader*  存放庫。</span><span class="sxs-lookup"><span data-stu-id="82b94-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="82b94-184">按一下 [選取] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="82b94-185">從 [**手動和排程的組建**] 下拉式選單的預設分支中，選取 [*主要*] 分支。</span><span class="sxs-lookup"><span data-stu-id="82b94-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="82b94-186">按一下 [繼續] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-186">Click the **Continue** button.</span></span> <span data-ttu-id="82b94-187">[範本選取專案] 頁面隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="82b94-188">建立組建定義</span><span class="sxs-lookup"><span data-stu-id="82b94-188">Create the build definition</span></span>

1. <span data-ttu-id="82b94-189">從 [範本選擇] 頁面的 [搜尋] 方塊中，輸入*ASP.NET Core* ：</span><span class="sxs-lookup"><span data-stu-id="82b94-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![ASP.NET Core 在範本頁面上搜尋](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="82b94-191">範本搜尋結果隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-191">The template search results appear.</span></span> <span data-ttu-id="82b94-192">將滑鼠停留在 [ **ASP.NET Core** ] 範本上，然後按一下 [套用 **] 按鈕。**</span><span class="sxs-lookup"><span data-stu-id="82b94-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="82b94-193">組建**定義的 [工作] 索引**標籤隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="82b94-194">按一下 [觸發程序] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="82b94-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="82b94-195">勾選 [**啟用連續整合**] 方塊。</span><span class="sxs-lookup"><span data-stu-id="82b94-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="82b94-196">在 [**分支篩選**] 區段下，確認 [**類型**] 下拉式設定為 [*包含*]。</span><span class="sxs-lookup"><span data-stu-id="82b94-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*.</span></span> <span data-ttu-id="82b94-197">將 [**分支規格**] 下拉式節點設定為 [ *master*]。</span><span class="sxs-lookup"><span data-stu-id="82b94-197">Set the **Branch specification** drop-down to *master*.</span></span>

    ![啟用持續整合設定](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="82b94-199">這些設定會在將任何變更推送至 GitHub 存放庫的*主要*分支時觸發組建。</span><span class="sxs-lookup"><span data-stu-id="82b94-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="82b94-200">持續整合會在將[變更認可至 GitHub 並自動部署至 Azure](#commit-changes-to-github-and-automatically-deploy-to-azure)一節中進行測試。</span><span class="sxs-lookup"><span data-stu-id="82b94-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="82b94-201">按一下 [**儲存 & 佇列**] 按鈕，然後選取 [**儲存**] 選項：</span><span class="sxs-lookup"><span data-stu-id="82b94-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![[儲存] 按鈕](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="82b94-203">會出現下列強制回應對話方塊：</span><span class="sxs-lookup"><span data-stu-id="82b94-203">The following modal dialog appears:</span></span>

    ![儲存組建定義-強制回應對話方塊](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="82b94-205">使用 *\\* 的預設資料夾，然後按一下 [**儲存**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-205">Use the default folder of *\\*, and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="82b94-206">建立發行管線</span><span class="sxs-lookup"><span data-stu-id="82b94-206">Create the release pipeline</span></span>

1. <span data-ttu-id="82b94-207">按一下 team 專案的 [**發行**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="82b94-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="82b94-208">按一下 [**新增管線**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-208">Click the **New pipeline** button.</span></span>

    ![[發行] 索引標籤-新增定義按鈕](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="82b94-210">[範本選取專案] 窗格隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="82b94-211">從 [範本選擇] 頁面的 [搜尋] 方塊中，輸入*App Service* ：</span><span class="sxs-lookup"><span data-stu-id="82b94-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![發行管線範本搜尋方塊](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="82b94-213">範本搜尋結果隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-213">The template search results appear.</span></span> <span data-ttu-id="82b94-214">將滑鼠停留在 [具有位置的**Azure App Service 部署**] 範本，然後按一下 [套用 **] 按鈕。**</span><span class="sxs-lookup"><span data-stu-id="82b94-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="82b94-215">發行管線的 [**管線**] 索引標籤隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![[發行管線管線] 索引標籤](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="82b94-217">按一下 [**構件**] 方塊中的 [**新增**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="82b94-218">[**新增**成品] 面板隨即出現：</span><span class="sxs-lookup"><span data-stu-id="82b94-218">The **Add artifact** panel appears:</span></span>

    ![發行管線-新增成品面板](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="82b94-220">從 [**來源類型**] 區段中選取 [**組建**] 磚。</span><span class="sxs-lookup"><span data-stu-id="82b94-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="82b94-221">此類型允許將發行管線連結至組建定義。</span><span class="sxs-lookup"><span data-stu-id="82b94-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="82b94-222">從 [**專案**] 下拉式選單中選取 [ *myfirstproject 專案*]。</span><span class="sxs-lookup"><span data-stu-id="82b94-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="82b94-223">從 [**來源（組建定義）** ] 下拉式選單中，選取組建定義名稱*MYFIRSTPROJECT-ASP.NET Core-CI*。</span><span class="sxs-lookup"><span data-stu-id="82b94-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="82b94-224">從 [**預設版本**] 下拉式選單中選取 [*最新*]。</span><span class="sxs-lookup"><span data-stu-id="82b94-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="82b94-225">此選項會建立最新執行組建定義所產生的構件。</span><span class="sxs-lookup"><span data-stu-id="82b94-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="82b94-226">將 [**來源別名**] 文字方塊中的文字取代為*Drop*。</span><span class="sxs-lookup"><span data-stu-id="82b94-226">Replace the text in the **Source alias** textbox with *Drop*.</span></span>
1. <span data-ttu-id="82b94-227">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-227">Click the **Add** button.</span></span> <span data-ttu-id="82b94-228">[**成品**] 區段會更新以顯示變更。</span><span class="sxs-lookup"><span data-stu-id="82b94-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="82b94-229">按一下閃電圖示以啟用持續部署：</span><span class="sxs-lookup"><span data-stu-id="82b94-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![發行管線成品-閃電圖示](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="82b94-231">啟用此選項時，每次有新的組建可供使用時，就會進行部署。</span><span class="sxs-lookup"><span data-stu-id="82b94-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="82b94-232">[**持續部署觸發**程式] 面板會出現在右側。</span><span class="sxs-lookup"><span data-stu-id="82b94-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="82b94-233">按一下 [切換] 按鈕以啟用功能。</span><span class="sxs-lookup"><span data-stu-id="82b94-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="82b94-234">不需要啟用**提取要求觸發**程式。</span><span class="sxs-lookup"><span data-stu-id="82b94-234">It isn't necessary to enable the **Pull request trigger**.</span></span>
1. <span data-ttu-id="82b94-235">按一下 [**組建分支篩選器**] 區段中的 [**新增**] 下拉式按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="82b94-236">選擇**組建定義的預設分支**選項。</span><span class="sxs-lookup"><span data-stu-id="82b94-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="82b94-237">此篩選會使發行僅針對來自 GitHub 存放庫之*主要*分支的組建觸發。</span><span class="sxs-lookup"><span data-stu-id="82b94-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="82b94-238">按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-238">Click the **Save** button.</span></span> <span data-ttu-id="82b94-239">在 [產生的**儲存**模式] 對話方塊中，按一下 [**確定]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="82b94-240">按一下 [**環境 1** ] 方塊。</span><span class="sxs-lookup"><span data-stu-id="82b94-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="82b94-241">[**環境**] 面板會出現在右側。</span><span class="sxs-lookup"><span data-stu-id="82b94-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="82b94-242">將 [**環境名稱**] 文字方塊中的*環境 1*文字變更為 [*生產*]。</span><span class="sxs-lookup"><span data-stu-id="82b94-242">Change the *Environment 1* text in the **Environment name** textbox to *Production*.</span></span>

   ![發行管線-環境名稱文字方塊](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="82b94-244">在 [**生產**] 方塊中，按一下 [ **1 個階段，2**個工作] 連結：</span><span class="sxs-lookup"><span data-stu-id="82b94-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![發行管線-生產環境連結 .png](media/cicd/vsts-production-link.png)

    <span data-ttu-id="82b94-246">環境**的 [工作] 索引**標籤隨即出現。</span><span class="sxs-lookup"><span data-stu-id="82b94-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="82b94-247">按一下 [**部署 Azure App Service 至**位置] 工作。</span><span class="sxs-lookup"><span data-stu-id="82b94-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="82b94-248">其設定會出現在右邊的面板中。</span><span class="sxs-lookup"><span data-stu-id="82b94-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="82b94-249">從 [ **azure 訂**用帳戶] 下拉式選單中，選取與 App Service 相關聯的 azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="82b94-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="82b94-250">選取之後，請按一下 [**授權**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="82b94-251">從 [**應用程式類型**] 下拉式選單選取 [ *Web 應用程式*]。</span><span class="sxs-lookup"><span data-stu-id="82b94-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="82b94-252">從  **App service 名稱** 下拉式選單中選取 mywebapp/<  *unique_number/>* 。</span><span class="sxs-lookup"><span data-stu-id="82b94-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="82b94-253">從 [**資源群組**] 下拉式選單中選取 [ *AzureTutorial* ]。</span><span class="sxs-lookup"><span data-stu-id="82b94-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="82b94-254">從 **[位置**] 下拉式選單選取 [*預備*]。</span><span class="sxs-lookup"><span data-stu-id="82b94-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="82b94-255">按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="82b94-256">將滑鼠停留在預設的發行管線名稱上。</span><span class="sxs-lookup"><span data-stu-id="82b94-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="82b94-257">按一下鉛筆圖示以編輯它。</span><span class="sxs-lookup"><span data-stu-id="82b94-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="82b94-258">使用*MyFirstProject-ASP.NET Core-CD*作為名稱。</span><span class="sxs-lookup"><span data-stu-id="82b94-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![發行管線名稱](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="82b94-260">按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82b94-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="82b94-261">將變更認可至 GitHub 並自動部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="82b94-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="82b94-262">在 Visual Studio 中開啟*SimpleFeedReader* 。</span><span class="sxs-lookup"><span data-stu-id="82b94-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="82b94-263">在方案總管中，開啟*Pages\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="82b94-263">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="82b94-264">將 `<h2>Simple Feed Reader - V3</h2>` 變更為 `<h2>Simple Feed Reader - V4</h2>`。</span><span class="sxs-lookup"><span data-stu-id="82b94-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="82b94-265">按**Ctrl**+**Shift**+**B**來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="82b94-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="82b94-266">將檔案認可至 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="82b94-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="82b94-267">使用 Visual Studio 的 [ *Team Explorer* ] 索引標籤中的 [**變更**] 頁面，或使用本機電腦的命令 shell 執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="82b94-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. <span data-ttu-id="82b94-268">將*主要*分支中的變更推送至 GitHub 存放庫的*原始*遠端：</span><span class="sxs-lookup"><span data-stu-id="82b94-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="82b94-269">認可會出現在 GitHub 存放庫的*主要*分支中：</span><span class="sxs-lookup"><span data-stu-id="82b94-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![主要分支中的 GitHub 認可](media/cicd/github-commit.png)

    <span data-ttu-id="82b94-271">會觸發組建，因為組建定義的 [**觸發**程式] 索引標籤中已啟用持續整合：</span><span class="sxs-lookup"><span data-stu-id="82b94-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![啟用持續整合](media/cicd/enable-ci.png)

1. <span data-ttu-id="82b94-273">在 Azure DevOps Services 中，流覽至**Azure Pipelines** > **組建** 頁面的 已**佇列** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="82b94-273">Navigate to the **Queued** tab of the **Azure Pipelines** > **Builds** page in Azure DevOps Services.</span></span> <span data-ttu-id="82b94-274">已排入佇列的組建會顯示觸發組建的分支和認可：</span><span class="sxs-lookup"><span data-stu-id="82b94-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![已排入佇列的組建](media/cicd/build-queued.png)

1. <span data-ttu-id="82b94-276">組建成功之後，就會發生 Azure 部署。</span><span class="sxs-lookup"><span data-stu-id="82b94-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="82b94-277">在瀏覽器中流覽至應用程式。</span><span class="sxs-lookup"><span data-stu-id="82b94-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="82b94-278">請注意，[V4] 文字會出現在標題中：</span><span class="sxs-lookup"><span data-stu-id="82b94-278">Notice that the "V4" text appears in the heading:</span></span>

    ![更新的應用程式](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a><span data-ttu-id="82b94-280">檢查 Azure Pipelines 管線</span><span class="sxs-lookup"><span data-stu-id="82b94-280">Examine the Azure Pipelines pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="82b94-281">組建定義</span><span class="sxs-lookup"><span data-stu-id="82b94-281">Build definition</span></span>

<span data-ttu-id="82b94-282">建立的組建定義名稱為*MyFirstProject-ASP.NET Core-CI*。</span><span class="sxs-lookup"><span data-stu-id="82b94-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*.</span></span> <span data-ttu-id="82b94-283">完成時，組建會產生一個 *.zip*檔案，其中包含要發行的資產。</span><span class="sxs-lookup"><span data-stu-id="82b94-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="82b94-284">發行管線會將這些資產部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="82b94-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="82b94-285">組建定義的 [**工作] 索引**標籤會列出所使用的個別步驟。</span><span class="sxs-lookup"><span data-stu-id="82b94-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="82b94-286">有五個組建工作。</span><span class="sxs-lookup"><span data-stu-id="82b94-286">There are five build tasks.</span></span>

![組建定義工作](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="82b94-288">**Restore** &mdash; 會執行 `dotnet restore` 命令來還原應用程式的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="82b94-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="82b94-289">使用的預設封裝摘要是 nuget.org。</span><span class="sxs-lookup"><span data-stu-id="82b94-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="82b94-290">**組建**&mdash; 執行 `dotnet build --configuration release` 命令來編譯應用程式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="82b94-290">**Build** &mdash; Executes the `dotnet build --configuration release` command to compile the app's code.</span></span> <span data-ttu-id="82b94-291">這個 `--configuration` 選項是用來產生程式碼的優化版本，適用于部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="82b94-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="82b94-292">在組建定義的 [**變數**] 索引標籤上修改*BuildConfiguration*變數（例如，需要進行 debug 設定）。</span><span class="sxs-lookup"><span data-stu-id="82b94-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="82b94-293">**測試**&mdash; 會執行 `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` 命令，以執行應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="82b94-293">**Test** &mdash; Executes the `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` command to run the app's unit tests.</span></span> <span data-ttu-id="82b94-294">單元測試會在符合 `**/*Tests/*.csproj` C# glob 模式的任何專案內執行。</span><span class="sxs-lookup"><span data-stu-id="82b94-294">Unit tests are executed within any C# project matching the `**/*Tests/*.csproj` glob pattern.</span></span> <span data-ttu-id="82b94-295">測試結果會儲存在 [`--results-directory`] 選項所指定位置的 *.trx*檔案中。</span><span class="sxs-lookup"><span data-stu-id="82b94-295">Test results are saved in a *.trx* file at the location specified by the `--results-directory` option.</span></span> <span data-ttu-id="82b94-296">如果有任何測試失敗，則組建會失敗且不會部署。</span><span class="sxs-lookup"><span data-stu-id="82b94-296">If any tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="82b94-297">若要確認單元測試能夠正常執行，請修改*SimpleFeedReader Tests\Services\NewsServiceTests.cs*以特意中斷其中一個測試。</span><span class="sxs-lookup"><span data-stu-id="82b94-297">To verify the unit tests work, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests.</span></span> <span data-ttu-id="82b94-298">例如，將 `Returns_News_Stories_Given_Valid_Uri` 方法中的 `Assert.True(result.Count > 0);` 變更為 `Assert.False(result.Count > 0);`。</span><span class="sxs-lookup"><span data-stu-id="82b94-298">For example, change `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri` method.</span></span> <span data-ttu-id="82b94-299">認可並推送變更至 GitHub。</span><span class="sxs-lookup"><span data-stu-id="82b94-299">Commit and push the change to GitHub.</span></span> <span data-ttu-id="82b94-300">組建已觸發且失敗。</span><span class="sxs-lookup"><span data-stu-id="82b94-300">The build is triggered and fails.</span></span> <span data-ttu-id="82b94-301">組建管線狀態變更為 [**失敗**]。</span><span class="sxs-lookup"><span data-stu-id="82b94-301">The build pipeline status changes to **failed**.</span></span> <span data-ttu-id="82b94-302">再次還原變更、認可和推送。</span><span class="sxs-lookup"><span data-stu-id="82b94-302">Revert the change, commit, and push again.</span></span> <span data-ttu-id="82b94-303">組建成功。</span><span class="sxs-lookup"><span data-stu-id="82b94-303">The build succeeds.</span></span>

1. <span data-ttu-id="82b94-304">**發行**&mdash; 執行 `dotnet publish --configuration release --output <local_path_on_build_agent>` 命令，以產生包含要部署之成品的 *.zip*檔案。</span><span class="sxs-lookup"><span data-stu-id="82b94-304">**Publish** &mdash; Executes the `dotnet publish --configuration release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="82b94-305">`--output` 選項會指定 *.zip*檔案的發行位置。</span><span class="sxs-lookup"><span data-stu-id="82b94-305">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="82b94-306">該位置是藉由傳遞名為 `$(build.artifactstagingdirectory)`的[預先定義變數](/azure/devops/pipelines/build/variables)來指定。</span><span class="sxs-lookup"><span data-stu-id="82b94-306">That location is specified by passing a [predefined variable](/azure/devops/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="82b94-307">該變數會擴充至組建代理程式上的本機路徑，例如*c:\agent\_work\1\a*。</span><span class="sxs-lookup"><span data-stu-id="82b94-307">That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.</span></span>
1. <span data-ttu-id="82b94-308">**發行**成品 &mdash; 發行**發行**工作所產生的 *.zip*檔案。</span><span class="sxs-lookup"><span data-stu-id="82b94-308">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="82b94-309">此工作會接受 *.zip*檔案位置做為參數，也就是預先定義的變數 `$(build.artifactstagingdirectory)`。</span><span class="sxs-lookup"><span data-stu-id="82b94-309">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="82b94-310">*.Zip*檔案會發行為名為*drop*的資料夾。</span><span class="sxs-lookup"><span data-stu-id="82b94-310">The *.zip* file is published as a folder named *drop*.</span></span>

<span data-ttu-id="82b94-311">按一下組建定義的 [**摘要**] 連結，以使用定義來查看組建的歷程記錄：</span><span class="sxs-lookup"><span data-stu-id="82b94-311">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![顯示組建定義歷程記錄的螢幕擷取畫面](media/cicd/build-definition-summary.png)

<span data-ttu-id="82b94-313">在產生的頁面上，按一下對應至唯一組建編號的連結：</span><span class="sxs-lookup"><span data-stu-id="82b94-313">On the resulting page, click the link corresponding to the unique build number:</span></span>

![顯示組建定義摘要頁面的螢幕擷取畫面](media/cicd/build-definition-completed.png)

<span data-ttu-id="82b94-315">隨即顯示此特定組建的摘要。</span><span class="sxs-lookup"><span data-stu-id="82b94-315">A summary of this specific build is displayed.</span></span> <span data-ttu-id="82b94-316">按一下 [**成品] 索引**標籤，並注意組建所產生的*drop*資料夾已列出：</span><span class="sxs-lookup"><span data-stu-id="82b94-316">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![顯示組建定義成品-放置資料夾的螢幕擷取畫面](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="82b94-318">使用 [**下載**] 和 [**探索**] 連結來檢查已發佈的成品。</span><span class="sxs-lookup"><span data-stu-id="82b94-318">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="82b94-319">發行管線</span><span class="sxs-lookup"><span data-stu-id="82b94-319">Release pipeline</span></span>

<span data-ttu-id="82b94-320">發行管線的建立名稱是*MyFirstProject-ASP.NET Core-CD*：</span><span class="sxs-lookup"><span data-stu-id="82b94-320">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:</span></span>

![顯示發行管線總覽的螢幕擷取畫面](media/cicd/release-definition-overview.png)

<span data-ttu-id="82b94-322">發行**管線的兩**個主要元件是成品和**環境**。</span><span class="sxs-lookup"><span data-stu-id="82b94-322">The two major components of the release pipeline are the **Artifacts** and the **Environments**.</span></span> <span data-ttu-id="82b94-323">按一下 [成品 **] 區段中的方塊**，會顯示下列面板：</span><span class="sxs-lookup"><span data-stu-id="82b94-323">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![顯示發行管線成品的螢幕擷取畫面](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="82b94-325">**來源（組建定義）** 值代表此發行管線所連結的組建定義。</span><span class="sxs-lookup"><span data-stu-id="82b94-325">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="82b94-326">成功執行組建定義所產生的 *.zip*檔案會提供給*生產*環境，以部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="82b94-326">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="82b94-327">按一下 [*生產*環境] 方塊中的 [ *1 階段，2*個工作] 連結，以查看發行管線工作：</span><span class="sxs-lookup"><span data-stu-id="82b94-327">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![顯示發行管線工作的螢幕擷取畫面](media/cicd/release-definition-tasks.png)

<span data-ttu-id="82b94-329">發行管線包含兩個工作：*將 Azure App Service 部署至*位置，以及*管理 Azure App Service 位置交換*。</span><span class="sxs-lookup"><span data-stu-id="82b94-329">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*.</span></span> <span data-ttu-id="82b94-330">按一下第一個工作會顯示下列工作設定：</span><span class="sxs-lookup"><span data-stu-id="82b94-330">Clicking the first task reveals the following task configuration:</span></span>

![顯示發行管線部署工作的螢幕擷取畫面](media/cicd/release-definition-task1.png)

<span data-ttu-id="82b94-332">Azure 訂用帳戶、服務類型、web 應用程式名稱、資源群組和部署位置都會在部署工作中定義。</span><span class="sxs-lookup"><span data-stu-id="82b94-332">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="82b94-333">[**封裝或資料夾**] 文字方塊會保存要解壓縮並部署到*mywebapp\<unique_number\>* web 應用程式*預備*位置的 *.zip*檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="82b94-333">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="82b94-334">按一下 [插槽交換] 工作會顯示下列工作設定：</span><span class="sxs-lookup"><span data-stu-id="82b94-334">Clicking the slot swap task reveals the following task configuration:</span></span>

![顯示發行管線位置交換工作的螢幕擷取畫面](media/cicd/release-definition-task2.png)

<span data-ttu-id="82b94-336">提供訂用帳戶、資源群組、服務類型、web 應用程式名稱和部署位置詳細資料。</span><span class="sxs-lookup"><span data-stu-id="82b94-336">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="82b94-337">[**與生產交換**] 核取方塊已核取。</span><span class="sxs-lookup"><span data-stu-id="82b94-337">The **Swap with Production** check box is checked.</span></span> <span data-ttu-id="82b94-338">因此，部署至*預備*位置的 bits 會交換到生產環境中。</span><span class="sxs-lookup"><span data-stu-id="82b94-338">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="82b94-339">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="82b94-339">Additional reading</span></span>

* [<span data-ttu-id="82b94-340">使用 Azure Pipelines 建立您的第一個管線</span><span class="sxs-lookup"><span data-stu-id="82b94-340">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)
* [<span data-ttu-id="82b94-341">組建和 .NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="82b94-341">Build and .NET Core project</span></span>](/azure/devops/pipelines/languages/dotnet-core)
* [<span data-ttu-id="82b94-342">使用 Azure Pipelines 部署 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="82b94-342">Deploy a web app with Azure Pipelines</span></span>](/azure/devops/pipelines/targets/webapp)
