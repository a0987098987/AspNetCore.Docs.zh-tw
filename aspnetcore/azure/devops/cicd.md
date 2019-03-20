---
title: 持續整合和部署-使用 ASP.NET Core 和 Azure 的 DevOps
author: CamSoper
description: 持續整合及 DevOps 中的部署使用 ASP.NET Core 和 Azure
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 676620b5dd151c9cd009d7cb278ed2c2b122c83f
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264876"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="af8fd-103">持續整合和部署</span><span class="sxs-lookup"><span data-stu-id="af8fd-103">Continuous integration and deployment</span></span>

<span data-ttu-id="af8fd-104">在上一章中，您會建立簡單的摘要讀取程式應用程式的本機 Git 存放庫。</span><span class="sxs-lookup"><span data-stu-id="af8fd-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="af8fd-105">在本章中，您會將該程式碼發行至 GitHub 存放庫，並建構 Azure DevOps 服務管線，使用 Azure 的管線。</span><span class="sxs-lookup"><span data-stu-id="af8fd-105">In this chapter, you'll publish that code to a GitHub repository and construct an Azure DevOps Services pipeline using Azure Pipelines.</span></span> <span data-ttu-id="af8fd-106">持續建置與部署應用程式，可讓管線。</span><span class="sxs-lookup"><span data-stu-id="af8fd-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="af8fd-107">建置和部署至 Azure Web 應用程式的預備位置，就會觸發任何認可的 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="af8fd-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="af8fd-108">在本節中，您將完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="af8fd-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="af8fd-109">將應用程式的程式碼發行至 GitHub</span><span class="sxs-lookup"><span data-stu-id="af8fd-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="af8fd-110">中斷連線的本機 Git 部署</span><span class="sxs-lookup"><span data-stu-id="af8fd-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="af8fd-111">建立 Azure DevOps 的組織</span><span class="sxs-lookup"><span data-stu-id="af8fd-111">Create an Azure DevOps organization</span></span>
* <span data-ttu-id="af8fd-112">Azure DevOps 服務中建立 team 專案</span><span class="sxs-lookup"><span data-stu-id="af8fd-112">Create a team project in Azure DevOps Services</span></span>
* <span data-ttu-id="af8fd-113">建立組建定義</span><span class="sxs-lookup"><span data-stu-id="af8fd-113">Create a build definition</span></span>
* <span data-ttu-id="af8fd-114">建立發行管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-114">Create a release pipeline</span></span>
* <span data-ttu-id="af8fd-115">GitHub 中認可變更，並自動部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="af8fd-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="af8fd-116">檢查 Azure 管線的管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-116">Examine the Azure Pipelines pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="af8fd-117">將應用程式的程式碼發行至 GitHub</span><span class="sxs-lookup"><span data-stu-id="af8fd-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="af8fd-118">開啟瀏覽器視窗，並瀏覽至`https://github.com`。</span><span class="sxs-lookup"><span data-stu-id="af8fd-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="af8fd-119">按一下  **+** 下拉式清單中的標頭，然後選取**新的存放庫**:</span><span class="sxs-lookup"><span data-stu-id="af8fd-119">Click the **+** drop-down in the header, and select **New repository**:</span></span>

    ![GitHub 新的存放庫選項](media/cicd/github-new-repo.png)

1. <span data-ttu-id="af8fd-121">選取您的帳戶**擁有者**下拉式清單中，然後輸入*簡單-摘要讀取器*中**存放庫名稱**文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="af8fd-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="af8fd-122">按一下 [**建立存放庫**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="af8fd-123">開啟 [本機電腦的命令殼層]。</span><span class="sxs-lookup"><span data-stu-id="af8fd-123">Open your local machine's command shell.</span></span> <span data-ttu-id="af8fd-124">瀏覽至在其中的目錄*簡單-摘要讀取器*儲存 Git 儲存機制。</span><span class="sxs-lookup"><span data-stu-id="af8fd-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="af8fd-125">重新命名現有*原點*遠端*上游*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-125">Rename the existing *origin* remote to *upstream*.</span></span> <span data-ttu-id="af8fd-126">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="af8fd-126">Execute the following command:</span></span>

    ```console
    git remote rename origin upstream
    ```

1. <span data-ttu-id="af8fd-127">加入新*原點*遠端指向您的 GitHub 上的存放庫複本。</span><span class="sxs-lookup"><span data-stu-id="af8fd-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="af8fd-128">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="af8fd-128">Execute the following command:</span></span>

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. <span data-ttu-id="af8fd-129">將本機 Git 儲存機制發行至新建立的 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="af8fd-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="af8fd-130">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="af8fd-130">Execute the following command:</span></span>

    ```console
    git push -u origin master
    ```

1. <span data-ttu-id="af8fd-131">開啟瀏覽器視窗，並瀏覽至`https://github.com/<GitHub_username>/simple-feed-reader/`。</span><span class="sxs-lookup"><span data-stu-id="af8fd-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="af8fd-132">驗證您的程式碼，會出現在 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="af8fd-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="af8fd-133">中斷連線的本機 Git 部署</span><span class="sxs-lookup"><span data-stu-id="af8fd-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="af8fd-134">移除本機 Git 部署進行下列步驟。</span><span class="sxs-lookup"><span data-stu-id="af8fd-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="af8fd-135">Azure 的管線 （Azure DevOps 服務） 會取代並增強了該功能。</span><span class="sxs-lookup"><span data-stu-id="af8fd-135">Azure Pipelines (an Azure DevOps service) both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="af8fd-136">開啟[Azure 入口網站](https://portal.azure.com/)，然後瀏覽至*接移 (mywebapp\<unique_number\>/預備)* Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="af8fd-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="af8fd-137">Web 應用程式可以藉由輸入快速找到*預備*入口網站的 [搜尋] 方塊中：</span><span class="sxs-lookup"><span data-stu-id="af8fd-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![預備 Web 應用程式搜尋字詞](media/cicd/portal-search-box.png)

1. <span data-ttu-id="af8fd-139">按一下 **部署中心**。</span><span class="sxs-lookup"><span data-stu-id="af8fd-139">Click **Deployment Center**.</span></span> <span data-ttu-id="af8fd-140">此時會出現一個新的面板。</span><span class="sxs-lookup"><span data-stu-id="af8fd-140">A new panel appears.</span></span> <span data-ttu-id="af8fd-141">按一下 **中斷連線**移除在前一章中已加入本機 Git 原始檔控制組態。</span><span class="sxs-lookup"><span data-stu-id="af8fd-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="af8fd-142">按一下 [確認移除操作**是**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="af8fd-143">瀏覽至*mywebapp < unique_number >* App Service。</span><span class="sxs-lookup"><span data-stu-id="af8fd-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="af8fd-144">提醒您，入口網站的 [搜尋] 方塊可用來快速找出應用程式服務。</span><span class="sxs-lookup"><span data-stu-id="af8fd-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="af8fd-145">按一下 **部署中心**。</span><span class="sxs-lookup"><span data-stu-id="af8fd-145">Click **Deployment Center**.</span></span> <span data-ttu-id="af8fd-146">此時會出現一個新的面板。</span><span class="sxs-lookup"><span data-stu-id="af8fd-146">A new panel appears.</span></span> <span data-ttu-id="af8fd-147">按一下 **中斷連線**移除在前一章中已加入本機 Git 原始檔控制組態。</span><span class="sxs-lookup"><span data-stu-id="af8fd-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="af8fd-148">按一下 [確認移除操作**是**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-an-azure-devops-organization"></a><span data-ttu-id="af8fd-149">建立 Azure DevOps 的組織</span><span class="sxs-lookup"><span data-stu-id="af8fd-149">Create an Azure DevOps organization</span></span>

1. <span data-ttu-id="af8fd-150">開啟瀏覽器，並瀏覽至[Azure DevOps 的組織建立頁面](https://go.microsoft.com/fwlink/?LinkId=307137)。</span><span class="sxs-lookup"><span data-stu-id="af8fd-150">Open a browser, and navigate to the [Azure DevOps organization creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="af8fd-151">輸入唯一名稱**挑選一個易記的名稱**文字方塊來形成來存取組織的 Azure DevOps 的 URL。</span><span class="sxs-lookup"><span data-stu-id="af8fd-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your Azure DevOps organization.</span></span>
1. <span data-ttu-id="af8fd-152">選取  **Git**選項按鈕，因為程式碼裝載於 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="af8fd-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="af8fd-153">按一下 [繼續] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-153">Click the **Continue** button.</span></span> <span data-ttu-id="af8fd-154">在之後短暫的等候、 帳戶以及 team 專案，名為*MyFirstProject*，所建立。</span><span class="sxs-lookup"><span data-stu-id="af8fd-154">After a short wait, an account and a team project, named *MyFirstProject*, are created.</span></span>

    ![Azure DevOps 的組織建立頁面](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="af8fd-156">開啟 確認電子郵件，指出 Azure DevOps 的組織和專案可供使用。</span><span class="sxs-lookup"><span data-stu-id="af8fd-156">Open the confirmation email indicating that the Azure DevOps organization and project are ready for use.</span></span> <span data-ttu-id="af8fd-157">按一下 **啟動您的專案**按鈕：</span><span class="sxs-lookup"><span data-stu-id="af8fd-157">Click the **Start your project** button:</span></span>

    ![啟動您的專案 按鈕](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="af8fd-159">瀏覽器中開啟 *\<account_name\>。 visualstudio.com*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-159">A browser opens to *\<account_name\>.visualstudio.com*.</span></span> <span data-ttu-id="af8fd-160">按一下  *MyFirstProject*連結，即可開始設定專案的 DevOps 管線。</span><span class="sxs-lookup"><span data-stu-id="af8fd-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-azure-pipelines-pipeline"></a><span data-ttu-id="af8fd-161">設定 Azure 管線的管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-161">Configure the Azure Pipelines pipeline</span></span>

<span data-ttu-id="af8fd-162">有三個不同的步驟，才能完成。</span><span class="sxs-lookup"><span data-stu-id="af8fd-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="af8fd-163">完成操作的 DevOps 管線中的下列三個區段結果中的步驟。</span><span class="sxs-lookup"><span data-stu-id="af8fd-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-azure-devops-access-to-the-github-repository"></a><span data-ttu-id="af8fd-164">授與 Azure DevOps 的 GitHub 存放庫的存取權</span><span class="sxs-lookup"><span data-stu-id="af8fd-164">Grant Azure DevOps access to the GitHub repository</span></span>

1. <span data-ttu-id="af8fd-165">依序展開**建置程式碼，從外部存放庫或**accordion。</span><span class="sxs-lookup"><span data-stu-id="af8fd-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="af8fd-166">按一下 **安裝程式建置**按鈕：</span><span class="sxs-lookup"><span data-stu-id="af8fd-166">Click the **Setup Build** button:</span></span>

    ![設定組建按鈕](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="af8fd-168">選取  **GitHub**選項**選取來源**區段：</span><span class="sxs-lookup"><span data-stu-id="af8fd-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![選取來源-GitHub](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="af8fd-170">Azure DevOps 才能存取您的 GitHub 存放庫需要授權。</span><span class="sxs-lookup"><span data-stu-id="af8fd-170">Authorization is required before Azure DevOps can access your GitHub repository.</span></span> <span data-ttu-id="af8fd-171">請輸入 *< GitHub_username > GitHub 連線*中**連線名稱**文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="af8fd-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="af8fd-172">例如: </span><span class="sxs-lookup"><span data-stu-id="af8fd-172">For example:</span></span>

    ![GitHub 連接名稱](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="af8fd-174">如果您的 GitHub 帳戶上啟用雙因素驗證，個人存取權杖是必要的。</span><span class="sxs-lookup"><span data-stu-id="af8fd-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="af8fd-175">在此情況下，按一下**使用 GitHub 個人存取權杖的授權**連結。</span><span class="sxs-lookup"><span data-stu-id="af8fd-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="af8fd-176">請參閱[官方 GitHub 個人存取權杖建立指示](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)取得協助。</span><span class="sxs-lookup"><span data-stu-id="af8fd-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="af8fd-177">只有*存放庫*需要的權限的範圍。</span><span class="sxs-lookup"><span data-stu-id="af8fd-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="af8fd-178">否則，請按一下**使用 OAuth 授權** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="af8fd-179">出現提示時，登入您的 GitHub 帳戶。</span><span class="sxs-lookup"><span data-stu-id="af8fd-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="af8fd-180">然後選取 [授權] 可授與您 Azure DevOps 的組織的存取權。</span><span class="sxs-lookup"><span data-stu-id="af8fd-180">Then select Authorize to grant access to your Azure DevOps organization.</span></span> <span data-ttu-id="af8fd-181">如果成功，則會建立新的服務端點。</span><span class="sxs-lookup"><span data-stu-id="af8fd-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="af8fd-182">按一下省略符號按鈕旁**存放庫** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="af8fd-183">選取  *< GitHub_username > 簡單-摘要讀取器 /* 從清單中的存放庫。</span><span class="sxs-lookup"><span data-stu-id="af8fd-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="af8fd-184">按一下 [**選取**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="af8fd-185">選取 *主要*從分支**手動和排程組建的預設分支**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="af8fd-186">按一下 [繼續] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-186">Click the **Continue** button.</span></span> <span data-ttu-id="af8fd-187">範本的 [選取] 頁面隨即出現。</span><span class="sxs-lookup"><span data-stu-id="af8fd-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="af8fd-188">建立組建定義</span><span class="sxs-lookup"><span data-stu-id="af8fd-188">Create the build definition</span></span>

1. <span data-ttu-id="af8fd-189">從 [範本選擇] 頁面中，輸入*ASP.NET Core*在搜尋方塊中：</span><span class="sxs-lookup"><span data-stu-id="af8fd-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![在 [範本] 頁面上的 ASP.NET Core 搜尋](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="af8fd-191">範本搜尋結果會出現。</span><span class="sxs-lookup"><span data-stu-id="af8fd-191">The template search results appear.</span></span> <span data-ttu-id="af8fd-192">將滑鼠停留**ASP.NET Core**範本，然後按**套用** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="af8fd-193">**任務**的組建定義的索引標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="af8fd-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="af8fd-194">按一下 [觸發程序]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="af8fd-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="af8fd-195">請檢查**啟用持續整合** 方塊中。</span><span class="sxs-lookup"><span data-stu-id="af8fd-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="af8fd-196">底下**分支篩選器**區段中，確認**型別**下拉式清單設定為*Include*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*.</span></span> <span data-ttu-id="af8fd-197">設定**分支規格**下拉式清單可*主要*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-197">Set the **Branch specification** drop-down to *master*.</span></span>

    ![啟用持續整合設定](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="af8fd-199">這些設定會使要若要推送的任何變更時觸發的組建*主要*GitHub 儲存機制分支。</span><span class="sxs-lookup"><span data-stu-id="af8fd-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="af8fd-200">持續整合測試中[認可變更到 GitHub，並自動部署至 Azure](#commit-changes-to-github-and-automatically-deploy-to-azure)一節。</span><span class="sxs-lookup"><span data-stu-id="af8fd-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="af8fd-201">按一下 **儲存與佇列**按鈕，然後選取**儲存**選項：</span><span class="sxs-lookup"><span data-stu-id="af8fd-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![[儲存] 按鈕](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="af8fd-203">下列的強制回應對話方塊隨即出現：</span><span class="sxs-lookup"><span data-stu-id="af8fd-203">The following modal dialog appears:</span></span>

    ![儲存組建定義為強制回應對話方塊](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="af8fd-205">使用的預設資料夾 *\\* ，然後按一下**儲存** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-205">Use the default folder of *\\*, and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="af8fd-206">建立發行管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-206">Create the release pipeline</span></span>

1. <span data-ttu-id="af8fd-207">按一下 [**版本**] 索引標籤，您的 team 專案。</span><span class="sxs-lookup"><span data-stu-id="af8fd-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="af8fd-208">按一下 [**新的管線**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-208">Click the **New pipeline** button.</span></span>

    ![Releases 索引標籤-新增定義 按鈕](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="af8fd-210">範本的 [選取] 窗格隨即出現。</span><span class="sxs-lookup"><span data-stu-id="af8fd-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="af8fd-211">從 [範本選擇] 頁面中，輸入*App Service*在搜尋方塊中：</span><span class="sxs-lookup"><span data-stu-id="af8fd-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![發行管線範本搜尋 方塊](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="af8fd-213">範本搜尋結果會出現。</span><span class="sxs-lookup"><span data-stu-id="af8fd-213">The template search results appear.</span></span> <span data-ttu-id="af8fd-214">將滑鼠停留**Azure App Service 部署位置**範本，然後按**套用** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="af8fd-215">**管線**發行管線的索引標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="af8fd-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![發行管線管線 索引標籤](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="af8fd-217">按一下 [**新增**按鈕**成品**] 方塊中。</span><span class="sxs-lookup"><span data-stu-id="af8fd-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="af8fd-218">**新增構件**面板出現時：</span><span class="sxs-lookup"><span data-stu-id="af8fd-218">The **Add artifact** panel appears:</span></span>

    ![發行管線-加入成品面板](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="af8fd-220">選取 **建置**圖格**來源類型**一節。</span><span class="sxs-lookup"><span data-stu-id="af8fd-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="af8fd-221">這個型別允許在發行管線對組建定義的連結。</span><span class="sxs-lookup"><span data-stu-id="af8fd-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="af8fd-222">選取  *MyFirstProject*從**專案**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="af8fd-223">選取組建定義名稱*MyFirstProject ASP.NET Core CI*，從**來源 （組建定義）** 下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="af8fd-224">選取 *最新*從**預設版本**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="af8fd-225">此選項會建立組建定義的最新的執行所產生的成品。</span><span class="sxs-lookup"><span data-stu-id="af8fd-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="af8fd-226">中的文字替換**來源別名**具有 textbox*卸除*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-226">Replace the text in the **Source alias** textbox with *Drop*.</span></span>
1. <span data-ttu-id="af8fd-227">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-227">Click the **Add** button.</span></span> <span data-ttu-id="af8fd-228">**成品**區段以顯示變更的更新。</span><span class="sxs-lookup"><span data-stu-id="af8fd-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="af8fd-229">按一下 啟用持續部署的閃電圖示：</span><span class="sxs-lookup"><span data-stu-id="af8fd-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![發行管線成品-閃電圖示](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="af8fd-231">啟用此選項，部署就會發生每個新的組建時的時間。</span><span class="sxs-lookup"><span data-stu-id="af8fd-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="af8fd-232">A**持續部署觸發程序**面板出現在右邊。</span><span class="sxs-lookup"><span data-stu-id="af8fd-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="af8fd-233">按一下切換按鈕，即可啟用此功能。</span><span class="sxs-lookup"><span data-stu-id="af8fd-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="af8fd-234">不需要啟用**提取要求觸發程序**。</span><span class="sxs-lookup"><span data-stu-id="af8fd-234">It isn't necessary to enable the **Pull request trigger**.</span></span>
1. <span data-ttu-id="af8fd-235">按一下 **新增**下拉式清單中**組建分支篩選**一節。</span><span class="sxs-lookup"><span data-stu-id="af8fd-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="af8fd-236">選擇**組建定義預設分支**選項。</span><span class="sxs-lookup"><span data-stu-id="af8fd-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="af8fd-237">此篩選條件會導致只會針對從 GitHub 存放庫的組建觸發發行*主要*分支。</span><span class="sxs-lookup"><span data-stu-id="af8fd-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="af8fd-238">按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-238">Click the **Save** button.</span></span> <span data-ttu-id="af8fd-239">按一下  **確定**按鈕，在產生**儲存**強制回應對話方塊。</span><span class="sxs-lookup"><span data-stu-id="af8fd-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="af8fd-240">按一下 [**環境 1** ] 方塊中。</span><span class="sxs-lookup"><span data-stu-id="af8fd-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="af8fd-241">**環境**面板出現在右邊。</span><span class="sxs-lookup"><span data-stu-id="af8fd-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="af8fd-242">變更*環境 1*中的文字**環境名稱**文字方塊*生產*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-242">Change the *Environment 1* text in the **Environment name** textbox to *Production*.</span></span>

   ![發行管線-環境名稱 文字方塊中](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="af8fd-244">按一下 **階段 1、 2 個工作**連結**生產**方塊：</span><span class="sxs-lookup"><span data-stu-id="af8fd-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![發行管線-生產環境 link.png](media/cicd/vsts-production-link.png)

    <span data-ttu-id="af8fd-246">**任務**環境索引標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="af8fd-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="af8fd-247">按一下 **部署 Azure App Service，插槽以**工作。</span><span class="sxs-lookup"><span data-stu-id="af8fd-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="af8fd-248">它的設定會出現在右邊面板。</span><span class="sxs-lookup"><span data-stu-id="af8fd-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="af8fd-249">選取 Azure 訂用帳戶，並從應用程式服務相關聯**Azure 訂用帳戶**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="af8fd-250">選取之後，按一下**授權** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="af8fd-251">選取  *Web 應用程式*從**應用程式類型**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="af8fd-252">選取  *mywebapp / < unique_number / >* 從**App service 名稱**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="af8fd-253">選取  *AzureTutorial*從**資源群組**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="af8fd-254">選取 *預備*從**插槽**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="af8fd-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="af8fd-255">按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="af8fd-256">暫留在預設的發行管線名稱。</span><span class="sxs-lookup"><span data-stu-id="af8fd-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="af8fd-257">按一下鉛筆圖示以編輯它。</span><span class="sxs-lookup"><span data-stu-id="af8fd-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="af8fd-258">使用*MyFirstProject ASP.NET Core CD*做為名稱。</span><span class="sxs-lookup"><span data-stu-id="af8fd-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![發行管線名稱](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="af8fd-260">按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af8fd-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="af8fd-261">GitHub 中認可變更，並自動部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="af8fd-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="af8fd-262">開啟*SimpleFeedReader.sln* Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="af8fd-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="af8fd-263">在 [方案總管] 中，開啟*Pages\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-263">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="af8fd-264">變更`<h2>Simple Feed Reader - V3</h2>`至`<h2>Simple Feed Reader - V4</h2>`。</span><span class="sxs-lookup"><span data-stu-id="af8fd-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="af8fd-265">按下**Ctrl**+**Shift**+**B**建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="af8fd-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="af8fd-266">檔案認可到 GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="af8fd-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="af8fd-267">使用任何一種**變更**Visual Studio 中的頁面*Team Explorer*索引標籤，或執行下列命令，使用本機電腦的命令殼層：</span><span class="sxs-lookup"><span data-stu-id="af8fd-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. <span data-ttu-id="af8fd-268">推送變更*主要*分支*原點*遠端的 GitHub 存放庫：</span><span class="sxs-lookup"><span data-stu-id="af8fd-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="af8fd-269">認可會出現在 GitHub 儲存機制*主要*分支：</span><span class="sxs-lookup"><span data-stu-id="af8fd-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![主要分支中的 GitHub 認可](media/cicd/github-commit.png)

    <span data-ttu-id="af8fd-271">觸發組建時，由於在組建定義中啟用持續整合**觸發程序** 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="af8fd-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![啟用持續整合](media/cicd/enable-ci.png)

1. <span data-ttu-id="af8fd-273">瀏覽至**已排入佇列**索引標籤**Azure 管線** > **建置**Azure DevOps 服務中的頁面。</span><span class="sxs-lookup"><span data-stu-id="af8fd-273">Navigate to the **Queued** tab of the **Azure Pipelines** > **Builds** page in Azure DevOps Services.</span></span> <span data-ttu-id="af8fd-274">已排入佇列的組建會顯示新分支和認可觸發組建：</span><span class="sxs-lookup"><span data-stu-id="af8fd-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![已排入佇列的組建](media/cicd/build-queued.png)

1. <span data-ttu-id="af8fd-276">一旦建置成功時，它會部署至 Azure，就會發生。</span><span class="sxs-lookup"><span data-stu-id="af8fd-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="af8fd-277">瀏覽至瀏覽器中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="af8fd-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="af8fd-278">請注意，"V4"的文字會出現在標題中：</span><span class="sxs-lookup"><span data-stu-id="af8fd-278">Notice that the "V4" text appears in the heading:</span></span>

    ![更新應用程式](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a><span data-ttu-id="af8fd-280">檢查 Azure 管線的管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-280">Examine the Azure Pipelines pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="af8fd-281">組建定義</span><span class="sxs-lookup"><span data-stu-id="af8fd-281">Build definition</span></span>

<span data-ttu-id="af8fd-282">建立組建定義名稱*MyFirstProject ASP.NET Core CI*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*.</span></span> <span data-ttu-id="af8fd-283">完成時，組建會產生 *.zip*包含要發佈的資產檔案。</span><span class="sxs-lookup"><span data-stu-id="af8fd-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="af8fd-284">發行管線部署到 Azure 的資產。</span><span class="sxs-lookup"><span data-stu-id="af8fd-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="af8fd-285">組建定義**任務**索引標籤會列出所使用的個別步驟。</span><span class="sxs-lookup"><span data-stu-id="af8fd-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="af8fd-286">有五個建置工作。</span><span class="sxs-lookup"><span data-stu-id="af8fd-286">There are five build tasks.</span></span>

![建置定義工作](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="af8fd-288">**還原**&mdash;執行`dotnet restore`還原應用程式的 NuGet 套件 命令。</span><span class="sxs-lookup"><span data-stu-id="af8fd-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="af8fd-289">預設封裝使用的摘要是 nuget.org。</span><span class="sxs-lookup"><span data-stu-id="af8fd-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="af8fd-290">**建置**&mdash;執行`dotnet build --configuration release`命令以編譯應用程式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="af8fd-290">**Build** &mdash; Executes the `dotnet build --configuration release` command to compile the app's code.</span></span> <span data-ttu-id="af8fd-291">這`--configuration`選項用來產生程式碼，適合用來部署至生產環境的最佳化的版本。</span><span class="sxs-lookup"><span data-stu-id="af8fd-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="af8fd-292">修改*BuildConfiguration*在組建定義的變數**變數**索引標籤上，如果您需要偵錯組態，例如。</span><span class="sxs-lookup"><span data-stu-id="af8fd-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="af8fd-293">**測試**&mdash;執行`dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>`命令來執行應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="af8fd-293">**Test** &mdash; Executes the `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` command to run the app's unit tests.</span></span> <span data-ttu-id="af8fd-294">執行單元測試內任何 C# 專案符合`**/*Tests/*.csproj`glob 模式。</span><span class="sxs-lookup"><span data-stu-id="af8fd-294">Unit tests are executed within any C# project matching the `**/*Tests/*.csproj` glob pattern.</span></span> <span data-ttu-id="af8fd-295">測試結果會儲存在 *.trx*所指定的位置在檔案`--results-directory`選項。</span><span class="sxs-lookup"><span data-stu-id="af8fd-295">Test results are saved in a *.trx* file at the location specified by the `--results-directory` option.</span></span> <span data-ttu-id="af8fd-296">如果任何測試失敗，組建會失敗，並不會部署。</span><span class="sxs-lookup"><span data-stu-id="af8fd-296">If any tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="af8fd-297">若要確認單元測試工作，修改*SimpleFeedReader.Tests\Services\NewsServiceTests.cs*特意不中斷其中一個測試。</span><span class="sxs-lookup"><span data-stu-id="af8fd-297">To verify the unit tests work, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests.</span></span> <span data-ttu-id="af8fd-298">例如，變更`Assert.True(result.Count > 0);`要`Assert.False(result.Count > 0);`在`Returns_News_Stories_Given_Valid_Uri`方法。</span><span class="sxs-lookup"><span data-stu-id="af8fd-298">For example, change `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri` method.</span></span> <span data-ttu-id="af8fd-299">認可並推送變更至 GitHub。</span><span class="sxs-lookup"><span data-stu-id="af8fd-299">Commit and push the change to GitHub.</span></span> <span data-ttu-id="af8fd-300">組建會觸發，並會失敗。</span><span class="sxs-lookup"><span data-stu-id="af8fd-300">The build is triggered and fails.</span></span> <span data-ttu-id="af8fd-301">建置管線狀態會變成**失敗**。</span><span class="sxs-lookup"><span data-stu-id="af8fd-301">The build pipeline status changes to **failed**.</span></span> <span data-ttu-id="af8fd-302">一次還原的變更、 認可並推送。</span><span class="sxs-lookup"><span data-stu-id="af8fd-302">Revert the change, commit, and push again.</span></span> <span data-ttu-id="af8fd-303">建置成功。</span><span class="sxs-lookup"><span data-stu-id="af8fd-303">The build succeeds.</span></span>

1. <span data-ttu-id="af8fd-304">**發佈**&mdash;執行`dotnet publish --configuration release --output <local_path_on_build_agent>`命令，以產生 *.zip*與要部署之成品的檔案。</span><span class="sxs-lookup"><span data-stu-id="af8fd-304">**Publish** &mdash; Executes the `dotnet publish --configuration release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="af8fd-305">`--output`選項指定的發行位置 *.zip*檔案。</span><span class="sxs-lookup"><span data-stu-id="af8fd-305">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="af8fd-306">位置由傳遞[預先定義的變數](/azure/devops/pipelines/build/variables)名為`$(build.artifactstagingdirectory)`。</span><span class="sxs-lookup"><span data-stu-id="af8fd-306">That location is specified by passing a [predefined variable](/azure/devops/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="af8fd-307">該變數會展開為本機路徑，例如*c:\agent\_work\1\a*，組建代理程式上。</span><span class="sxs-lookup"><span data-stu-id="af8fd-307">That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.</span></span>
1. <span data-ttu-id="af8fd-308">**發行成品** &mdash; Publishes *.zip*所產生檔案**發行**工作。</span><span class="sxs-lookup"><span data-stu-id="af8fd-308">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="af8fd-309">此工作會接受 *.zip*檔案位置做為參數，也就是預先定義的變數`$(build.artifactstagingdirectory)`。</span><span class="sxs-lookup"><span data-stu-id="af8fd-309">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="af8fd-310">*.Zip*檔案會發行名為的資料夾*卸除*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-310">The *.zip* file is published as a folder named *drop*.</span></span>

<span data-ttu-id="af8fd-311">按一下 組建定義**摘要**連結以檢視組建定義的歷程記錄：</span><span class="sxs-lookup"><span data-stu-id="af8fd-311">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![螢幕擷取畫面顯示組建定義歷程記錄](media/cicd/build-definition-summary.png)

<span data-ttu-id="af8fd-313">在產生的頁面上，按一下對應至唯一的組建編號的連結：</span><span class="sxs-lookup"><span data-stu-id="af8fd-313">On the resulting page, click the link corresponding to the unique build number:</span></span>

![螢幕擷取畫面顯示組建定義摘要頁面](media/cicd/build-definition-completed.png)

<span data-ttu-id="af8fd-315">這個特定組建的摘要會顯示。</span><span class="sxs-lookup"><span data-stu-id="af8fd-315">A summary of this specific build is displayed.</span></span> <span data-ttu-id="af8fd-316">按一下 **成品**索引標籤，並注意*卸除*組建所產生的資料夾會列出：</span><span class="sxs-lookup"><span data-stu-id="af8fd-316">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![螢幕擷取畫面顯示組建定義成品置放資料夾](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="af8fd-318">使用**下載**並**瀏覽**檢查已發行的成品的連結。</span><span class="sxs-lookup"><span data-stu-id="af8fd-318">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="af8fd-319">發行管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-319">Release pipeline</span></span>

<span data-ttu-id="af8fd-320">發行管線建立同名*MyFirstProject ASP.NET Core CD*:</span><span class="sxs-lookup"><span data-stu-id="af8fd-320">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:</span></span>

![螢幕擷取畫面顯示發行管線的概觀](media/cicd/release-definition-overview.png)

<span data-ttu-id="af8fd-322">發行管線的兩個主要元件如下**成品**並**環境**。</span><span class="sxs-lookup"><span data-stu-id="af8fd-322">The two major components of the release pipeline are the **Artifacts** and the **Environments**.</span></span> <span data-ttu-id="af8fd-323">按一下  方塊中的**成品**區段會顯示下列窗格：</span><span class="sxs-lookup"><span data-stu-id="af8fd-323">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![螢幕擷取畫面顯示發行管線成品](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="af8fd-325">**來源 （組建定義）** 值代表此發行管線所連結的組建定義。</span><span class="sxs-lookup"><span data-stu-id="af8fd-325">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="af8fd-326">*.Zip*成功執行的組建定義時所產生的檔案提供給*生產*環境以部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="af8fd-326">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="af8fd-327">按一下 *階段 1、 2 個工作*連結*生產*環境方塊，以檢視發行管線工作：</span><span class="sxs-lookup"><span data-stu-id="af8fd-327">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![螢幕擷取畫面顯示發行管線工作](media/cicd/release-definition-tasks.png)

<span data-ttu-id="af8fd-329">發行管線包含兩個工作：*部署 Azure App Service，插槽*並*管理 Azure App Service-Slot Swap*。</span><span class="sxs-lookup"><span data-stu-id="af8fd-329">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*.</span></span> <span data-ttu-id="af8fd-330">按一下第一項工作會顯示下列工作組態：</span><span class="sxs-lookup"><span data-stu-id="af8fd-330">Clicking the first task reveals the following task configuration:</span></span>

![螢幕擷取畫面顯示發行管線部署工作](media/cicd/release-definition-task1.png)

<span data-ttu-id="af8fd-332">在 [部署] 工作中定義的 Azure 訂用帳戶、 服務類型、 web 應用程式名稱、 資源群組和部署位置。</span><span class="sxs-lookup"><span data-stu-id="af8fd-332">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="af8fd-333">**封裝或資料夾**文字方塊中會保留 *.zip*擷取並部署到的檔案路徑*預備*插槽*mywebapp\<唯一（_n)\>*  web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="af8fd-333">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="af8fd-334">按一下位置交換工作會顯示下列工作組態：</span><span class="sxs-lookup"><span data-stu-id="af8fd-334">Clicking the slot swap task reveals the following task configuration:</span></span>

![螢幕擷取畫面顯示發行管線位置交換工作](media/cicd/release-definition-task2.png)

<span data-ttu-id="af8fd-336">提供訂用帳戶、 資源群組、 服務類型、 web 應用程式名稱，以及部署位置的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="af8fd-336">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="af8fd-337">**與生產環境交換** 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="af8fd-337">The **Swap with Production** check box is checked.</span></span> <span data-ttu-id="af8fd-338">因此，將位元部署到*預備*位置交換到生產環境。</span><span class="sxs-lookup"><span data-stu-id="af8fd-338">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="af8fd-339">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="af8fd-339">Additional reading</span></span>

* [<span data-ttu-id="af8fd-340">使用 Azure Pipelines 建立您的第一個管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-340">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)
* [<span data-ttu-id="af8fd-341">組建和.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="af8fd-341">Build and .NET Core project</span></span>](/azure/devops/pipelines/languages/dotnet-core)
* [<span data-ttu-id="af8fd-342">部署 web 應用程式與 Azure 的管線</span><span class="sxs-lookup"><span data-stu-id="af8fd-342">Deploy a web app with Azure Pipelines</span></span>](/azure/devops/pipelines/targets/webapp)
