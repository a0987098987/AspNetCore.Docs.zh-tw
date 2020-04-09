---
title: 持續整合與部署 - 使用 ASP.NET 核心與 Azure 的 DevOps
author: CamSoper
description: 使用ASP.NET核心和 Azure 在 DevOps 中持續整合和部署
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: mvc, seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 5fdf52235b49119503885f92c370dc588e809ffe
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78655831"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="bc56f-103">持續整合及部署</span><span class="sxs-lookup"><span data-stu-id="bc56f-103">Continuous integration and deployment</span></span>

<span data-ttu-id="bc56f-104">在上一章中,您為簡單源閱讀器應用創建了本地 Git 儲存庫。</span><span class="sxs-lookup"><span data-stu-id="bc56f-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="bc56f-105">在本章中,您將將該代碼發佈到 GitHub 儲存庫,並使用 Azure 管道建構 Azure DevOps 服務管道。</span><span class="sxs-lookup"><span data-stu-id="bc56f-105">In this chapter, you'll publish that code to a GitHub repository and construct an Azure DevOps Services pipeline using Azure Pipelines.</span></span> <span data-ttu-id="bc56f-106">管道支援應用的連續生成和部署。</span><span class="sxs-lookup"><span data-stu-id="bc56f-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="bc56f-107">任何提交到 GitHub 儲存庫都觸發生成和部署到 Azure Web 應用的暫存槽。</span><span class="sxs-lookup"><span data-stu-id="bc56f-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="bc56f-108">在本節中,您將完成以下任務:</span><span class="sxs-lookup"><span data-stu-id="bc56f-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="bc56f-109">將應用程式的代碼發布到 GitHub</span><span class="sxs-lookup"><span data-stu-id="bc56f-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="bc56f-110">斷線開發的 Git 部署</span><span class="sxs-lookup"><span data-stu-id="bc56f-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="bc56f-111">建立 Azure DevOps 組織</span><span class="sxs-lookup"><span data-stu-id="bc56f-111">Create an Azure DevOps organization</span></span>
* <span data-ttu-id="bc56f-112">在 Azure DevOps 服務中建立團隊專案</span><span class="sxs-lookup"><span data-stu-id="bc56f-112">Create a team project in Azure DevOps Services</span></span>
* <span data-ttu-id="bc56f-113">建立組建定義</span><span class="sxs-lookup"><span data-stu-id="bc56f-113">Create a build definition</span></span>
* <span data-ttu-id="bc56f-114">建立發行管線</span><span class="sxs-lookup"><span data-stu-id="bc56f-114">Create a release pipeline</span></span>
* <span data-ttu-id="bc56f-115">將變更認可至 GitHub 並自動部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="bc56f-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="bc56f-116">檢查 Azure 管道導管</span><span class="sxs-lookup"><span data-stu-id="bc56f-116">Examine the Azure Pipelines pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="bc56f-117">將應用程式的代碼發布到 GitHub</span><span class="sxs-lookup"><span data-stu-id="bc56f-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="bc56f-118">開啟瀏覽器視窗,然後瀏覽`https://github.com`到 。</span><span class="sxs-lookup"><span data-stu-id="bc56f-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="bc56f-119">按下**+** 標題中的下拉清單,然後選擇 **"新建儲存庫**"</span><span class="sxs-lookup"><span data-stu-id="bc56f-119">Click the **+** drop-down in the header, and select **New repository**:</span></span>

    ![GitHub 新儲存函式庫選項](media/cicd/github-new-repo.png)

1. <span data-ttu-id="bc56f-121">在 **「擁有者」** 下拉清單中選擇您的帳戶,並在**儲存庫名稱**文字框中輸入*簡單來源閱讀器*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="bc56f-122">按下「**創建儲存庫**」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="bc56f-123">打開本地電腦的命令外殼。</span><span class="sxs-lookup"><span data-stu-id="bc56f-123">Open your local machine's command shell.</span></span> <span data-ttu-id="bc56f-124">導航到儲存*簡單源讀取器*Git 儲存庫的目錄。</span><span class="sxs-lookup"><span data-stu-id="bc56f-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="bc56f-125">將現有*來源*重新命名到遠端到*上游*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-125">Rename the existing *origin* remote to *upstream*.</span></span> <span data-ttu-id="bc56f-126">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bc56f-126">Execute the following command:</span></span>

    ```console
    git remote rename origin upstream
    ```

1. <span data-ttu-id="bc56f-127">在 GitHub 上添加指向儲存庫副本的新*源*遙控器。</span><span class="sxs-lookup"><span data-stu-id="bc56f-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="bc56f-128">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bc56f-128">Execute the following command:</span></span>

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. <span data-ttu-id="bc56f-129">將本地 Git 儲存庫發表到新創建的 GitHub 儲存庫。</span><span class="sxs-lookup"><span data-stu-id="bc56f-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="bc56f-130">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bc56f-130">Execute the following command:</span></span>

    ```console
    git push -u origin master
    ```

1. <span data-ttu-id="bc56f-131">開啟瀏覽器視窗,然後瀏覽`https://github.com/<GitHub_username>/simple-feed-reader/`到 。</span><span class="sxs-lookup"><span data-stu-id="bc56f-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="bc56f-132">驗證您的代碼是否顯示在 GitHub 儲存庫中。</span><span class="sxs-lookup"><span data-stu-id="bc56f-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="bc56f-133">斷線開發的 Git 部署</span><span class="sxs-lookup"><span data-stu-id="bc56f-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="bc56f-134">使用以下步驟刪除本地 Git 部署。</span><span class="sxs-lookup"><span data-stu-id="bc56f-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="bc56f-135">Azure 管道(Azure DevOps 服務)既替換並增強了該功能。</span><span class="sxs-lookup"><span data-stu-id="bc56f-135">Azure Pipelines (an Azure DevOps service) both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="bc56f-136">打開[Azure 門戶](https://portal.azure.com/),然後導航到*暫存\<(mywebapp unique_number\>/暫存)Web*應用。</span><span class="sxs-lookup"><span data-stu-id="bc56f-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="bc56f-137">透過在門戶搜尋框中輸入*暫存,* 可以快速定位 Web 應用:</span><span class="sxs-lookup"><span data-stu-id="bc56f-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![暫存 Web 應用搜尋詞](media/cicd/portal-search-box.png)

1. <span data-ttu-id="bc56f-139">點選**部署中心**。</span><span class="sxs-lookup"><span data-stu-id="bc56f-139">Click **Deployment Center**.</span></span> <span data-ttu-id="bc56f-140">將顯示一個新面板。</span><span class="sxs-lookup"><span data-stu-id="bc56f-140">A new panel appears.</span></span> <span data-ttu-id="bc56f-141">按下 **「斷開連線**」可刪除上一章中添加的本地 Git 原始程式碼管理設定。</span><span class="sxs-lookup"><span data-stu-id="bc56f-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="bc56f-142">按下「**是**」按鈕確認刪除操作。</span><span class="sxs-lookup"><span data-stu-id="bc56f-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="bc56f-143">導航到*mywebapp<unique_number>* 應用服務。</span><span class="sxs-lookup"><span data-stu-id="bc56f-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="bc56f-144">作為提醒,門戶的搜索框可用於快速查找應用服務。</span><span class="sxs-lookup"><span data-stu-id="bc56f-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="bc56f-145">點選**部署中心**。</span><span class="sxs-lookup"><span data-stu-id="bc56f-145">Click **Deployment Center**.</span></span> <span data-ttu-id="bc56f-146">將顯示一個新面板。</span><span class="sxs-lookup"><span data-stu-id="bc56f-146">A new panel appears.</span></span> <span data-ttu-id="bc56f-147">按下 **「斷開連線**」可刪除上一章中添加的本地 Git 原始程式碼管理設定。</span><span class="sxs-lookup"><span data-stu-id="bc56f-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="bc56f-148">按下「**是**」按鈕確認刪除操作。</span><span class="sxs-lookup"><span data-stu-id="bc56f-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-an-azure-devops-organization"></a><span data-ttu-id="bc56f-149">建立 Azure DevOps 組織</span><span class="sxs-lookup"><span data-stu-id="bc56f-149">Create an Azure DevOps organization</span></span>

1. <span data-ttu-id="bc56f-150">開啟瀏覽器,然後瀏覽到[Azure DevOps 組織建立頁](https://go.microsoft.com/fwlink/?LinkId=307137)。</span><span class="sxs-lookup"><span data-stu-id="bc56f-150">Open a browser, and navigate to the [Azure DevOps organization creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="bc56f-151">在 **「選取令人難忘的名稱**文字框」中鍵入唯一名稱,以形成用於訪問 Azure DevOps 組織的 URL。</span><span class="sxs-lookup"><span data-stu-id="bc56f-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your Azure DevOps organization.</span></span>
1. <span data-ttu-id="bc56f-152">選擇**Git**單選按鈕,因為代碼託管在 GitHub 儲存庫中。</span><span class="sxs-lookup"><span data-stu-id="bc56f-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="bc56f-153">按一下 [繼續]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-153">Click the **Continue** button.</span></span> <span data-ttu-id="bc56f-154">經過短暫的等待,將創建名為*MyFirstProject*的帳戶和團隊專案。</span><span class="sxs-lookup"><span data-stu-id="bc56f-154">After a short wait, an account and a team project, named *MyFirstProject*, are created.</span></span>

    ![Azure DevOps 組織建立頁](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="bc56f-156">打開確認電子郵件,指示 Azure DevOps 組織和專案已準備就緒,</span><span class="sxs-lookup"><span data-stu-id="bc56f-156">Open the confirmation email indicating that the Azure DevOps organization and project are ready for use.</span></span> <span data-ttu-id="bc56f-157">按下「**啟動項目」** 按鈕:</span><span class="sxs-lookup"><span data-stu-id="bc56f-157">Click the **Start your project** button:</span></span>

    ![啟動項目按鈕](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="bc56f-159">瀏覽器將開啟*\<到\>account_name .visualstudio.com*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-159">A browser opens to *\<account_name\>.visualstudio.com*.</span></span> <span data-ttu-id="bc56f-160">按下*MyFirstProject*連結開始配置專案的 DevOps 管道。</span><span class="sxs-lookup"><span data-stu-id="bc56f-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-azure-pipelines-pipeline"></a><span data-ttu-id="bc56f-161">設定 Azure 導管導管</span><span class="sxs-lookup"><span data-stu-id="bc56f-161">Configure the Azure Pipelines pipeline</span></span>

<span data-ttu-id="bc56f-162">有三個不同的步驟要完成。</span><span class="sxs-lookup"><span data-stu-id="bc56f-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="bc56f-163">完成以下三節中的步驟后,將生成可操作的 DevOps 管道。</span><span class="sxs-lookup"><span data-stu-id="bc56f-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-azure-devops-access-to-the-github-repository"></a><span data-ttu-id="bc56f-164">授予 Azure 開發人員對 GitHub 儲存庫的權限</span><span class="sxs-lookup"><span data-stu-id="bc56f-164">Grant Azure DevOps access to the GitHub repository</span></span>

1. <span data-ttu-id="bc56f-165">從外部儲存函式庫手風琴延伸**或產生代碼**。</span><span class="sxs-lookup"><span data-stu-id="bc56f-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="bc56f-166">點選 **「設定產生」** 按鈕:</span><span class="sxs-lookup"><span data-stu-id="bc56f-166">Click the **Setup Build** button:</span></span>

    ![設定產生按鈕](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="bc56f-168">從選擇**來源**「部份中選擇**GitHub**選項:</span><span class="sxs-lookup"><span data-stu-id="bc56f-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![選擇來源 - GitHub](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="bc56f-170">在 Azure DevOps 可以訪問 GitHub 儲存庫之前,需要授權。</span><span class="sxs-lookup"><span data-stu-id="bc56f-170">Authorization is required before Azure DevOps can access your GitHub repository.</span></span> <span data-ttu-id="bc56f-171">在**連接名稱**文字框中*輸入> GitHub 連接<GitHub_username。*</span><span class="sxs-lookup"><span data-stu-id="bc56f-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="bc56f-172">例如：</span><span class="sxs-lookup"><span data-stu-id="bc56f-172">For example:</span></span>

    ![GitHub 連線名稱](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="bc56f-174">如果在 GitHub 帳戶上啟用了雙重身份驗證,則需要個人訪問令牌。</span><span class="sxs-lookup"><span data-stu-id="bc56f-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="bc56f-175">在這種情況下,按下**使用 GitHub 個人存取權杖連結的授權**。</span><span class="sxs-lookup"><span data-stu-id="bc56f-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="bc56f-176">有關說明,請參閱[官方 GitHub 個人存取權杖建立說明](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)。</span><span class="sxs-lookup"><span data-stu-id="bc56f-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="bc56f-177">只需要許可權的*回購*範圍。</span><span class="sxs-lookup"><span data-stu-id="bc56f-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="bc56f-178">否則,按下 **「使用 OAuth 授權**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="bc56f-179">出現提示后,請登錄到 GitHub 帳戶。</span><span class="sxs-lookup"><span data-stu-id="bc56f-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="bc56f-180">然後選擇"授權"以授予對 Azure DevOps 組織的訪問許可權。</span><span class="sxs-lookup"><span data-stu-id="bc56f-180">Then select Authorize to grant access to your Azure DevOps organization.</span></span> <span data-ttu-id="bc56f-181">如果成功,將創建新的服務終結點。</span><span class="sxs-lookup"><span data-stu-id="bc56f-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="bc56f-182">按下 **「存儲庫」** 按鈕旁邊的省略號按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="bc56f-183">從清單中選擇*GitHub_username>/簡單饋送讀取體儲存庫<。*</span><span class="sxs-lookup"><span data-stu-id="bc56f-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="bc56f-184">按一下 [選取]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="bc56f-185">從 **「預設分支」中選擇主分支,用於手動和計畫生成**下拉*master*清單。</span><span class="sxs-lookup"><span data-stu-id="bc56f-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="bc56f-186">按一下 [繼續]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-186">Click the **Continue** button.</span></span> <span data-ttu-id="bc56f-187">將顯示範本選擇頁。</span><span class="sxs-lookup"><span data-stu-id="bc56f-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="bc56f-188">建立組建定義</span><span class="sxs-lookup"><span data-stu-id="bc56f-188">Create the build definition</span></span>

1. <span data-ttu-id="bc56f-189">在樣本選擇頁中,在搜尋框中輸入*ASP.NET核心*:</span><span class="sxs-lookup"><span data-stu-id="bc56f-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![ASP.NET範本頁面上的核心搜尋](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="bc56f-191">將顯示範本搜尋結果。</span><span class="sxs-lookup"><span data-stu-id="bc56f-191">The template search results appear.</span></span> <span data-ttu-id="bc56f-192">將滑鼠懸停在**ASP.NET核心**範本上,然後按下「**應用」** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="bc56f-193">將顯示生成定義的 **「任務**」選項卡。</span><span class="sxs-lookup"><span data-stu-id="bc56f-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="bc56f-194">按下「**觸發」** 選項卡。</span><span class="sxs-lookup"><span data-stu-id="bc56f-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="bc56f-195">選中"**啟用連續集成**"框。</span><span class="sxs-lookup"><span data-stu-id="bc56f-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="bc56f-196">在 **「分支篩選器」** 部分下,確認 **「類型**下拉清單」設置為 *「包括*」。。</span><span class="sxs-lookup"><span data-stu-id="bc56f-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*.</span></span> <span data-ttu-id="bc56f-197">將**分支規範**下拉清單設定為*主*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-197">Set the **Branch specification** drop-down to *master*.</span></span>

    ![開啟連續整合設定](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="bc56f-199">當將任何更改推送到 GitHub 儲存庫*的主*分支時,這些設置會導致生成觸發。</span><span class="sxs-lookup"><span data-stu-id="bc56f-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="bc56f-200">在[GitHub 的「提交」更改中測試持續整合,自動部署到 Azure](#commit-changes-to-github-and-automatically-deploy-to-azure)部分。</span><span class="sxs-lookup"><span data-stu-id="bc56f-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="bc56f-201">按下「**儲存& 佇列**」按鈕,然後選擇「**儲存**」選項:</span><span class="sxs-lookup"><span data-stu-id="bc56f-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![[儲存] 按鈕](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="bc56f-203">顯示以下模式對話框:</span><span class="sxs-lookup"><span data-stu-id="bc56f-203">The following modal dialog appears:</span></span>

    ![儲存產生定義 - 模式對話框](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="bc56f-205">使用 的預設*\\*資料夾 ,然後按下 **「儲存**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-205">Use the default folder of *\\*, and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="bc56f-206">建立發行管線</span><span class="sxs-lookup"><span data-stu-id="bc56f-206">Create the release pipeline</span></span>

1. <span data-ttu-id="bc56f-207">單擊團隊專案的 **「發佈」** 選項卡。</span><span class="sxs-lookup"><span data-stu-id="bc56f-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="bc56f-208">按下 **「新建管道**」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-208">Click the **New pipeline** button.</span></span>

    ![釋放選項卡 - 新訂按鈕](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="bc56f-210">將顯示範本選擇窗格。</span><span class="sxs-lookup"><span data-stu-id="bc56f-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="bc56f-211">在樣本選擇頁中,在搜尋框中輸入*應用服務*:</span><span class="sxs-lookup"><span data-stu-id="bc56f-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![發布導管樣本搜尋框](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="bc56f-213">將顯示範本搜尋結果。</span><span class="sxs-lookup"><span data-stu-id="bc56f-213">The template search results appear.</span></span> <span data-ttu-id="bc56f-214">使用插槽範本將滑鼠懸停在**Azure 應用服務部署**上,然後單擊「**應用」** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="bc56f-215">將顯示發佈**管道的管道**選項卡。</span><span class="sxs-lookup"><span data-stu-id="bc56f-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![釋放導管導管選項卡](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="bc56f-217">按下 **「項目**」 的 **「新增**」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="bc56f-218">將顯示 **'新增專案'** 面板:</span><span class="sxs-lookup"><span data-stu-id="bc56f-218">The **Add artifact** panel appears:</span></span>

    ![發布導管 - 新增工件面板](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="bc56f-220">從 **「源類型」** 部分選擇 **「生成**」磁貼。</span><span class="sxs-lookup"><span data-stu-id="bc56f-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="bc56f-221">此類型允許將發佈管道連結到生成定義。</span><span class="sxs-lookup"><span data-stu-id="bc56f-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="bc56f-222">從**專案**下拉清單中選擇*MyFirst 專案*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="bc56f-223">從 **「源(生成定義)** 下拉清單中選擇生成定義名稱*MyFirstProject-ASP.NET Core-CI。*</span><span class="sxs-lookup"><span data-stu-id="bc56f-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="bc56f-224">從**預設版本**下拉清單中選擇 *「最新*」。</span><span class="sxs-lookup"><span data-stu-id="bc56f-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="bc56f-225">此選項生成生成定義的最新運行生成的工件。</span><span class="sxs-lookup"><span data-stu-id="bc56f-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="bc56f-226">將**源別名**文字框中的文字取代為 *"刪除*"。</span><span class="sxs-lookup"><span data-stu-id="bc56f-226">Replace the text in the **Source alias** textbox with *Drop*.</span></span>
1. <span data-ttu-id="bc56f-227">按一下 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-227">Click the **Add** button.</span></span> <span data-ttu-id="bc56f-228">**項目「** 部分將更新以顯示更改」。</span><span class="sxs-lookup"><span data-stu-id="bc56f-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="bc56f-229">點選閃電圖示以開啟連續部署:</span><span class="sxs-lookup"><span data-stu-id="bc56f-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![釋放導管防火 - 閃電圖示](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="bc56f-231">啟用此選項后,每次新生成可用時都會進行部署。</span><span class="sxs-lookup"><span data-stu-id="bc56f-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="bc56f-232">**"連續部署觸發器**"面板顯示在右側。</span><span class="sxs-lookup"><span data-stu-id="bc56f-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="bc56f-233">按下切換按鈕以啟用該功能。</span><span class="sxs-lookup"><span data-stu-id="bc56f-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="bc56f-234">不要開啟 **「拉取」要求觸發器**。</span><span class="sxs-lookup"><span data-stu-id="bc56f-234">It isn't necessary to enable the **Pull request trigger**.</span></span>
1. <span data-ttu-id="bc56f-235">按下 **「生成分支篩選器**」部分中的「**添加**下拉清單」。</span><span class="sxs-lookup"><span data-stu-id="bc56f-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="bc56f-236">選擇**定義的預設分支**選項。</span><span class="sxs-lookup"><span data-stu-id="bc56f-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="bc56f-237">此篩選器使版本僅觸發來自 GitHub 儲存庫*的主*分支的生成。</span><span class="sxs-lookup"><span data-stu-id="bc56f-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="bc56f-238">按一下 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-238">Click the **Save** button.</span></span> <span data-ttu-id="bc56f-239">按下產生的 **「儲存**模式」對話框中的 **「確定**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="bc56f-240">按下 **「環境 1」** 框。</span><span class="sxs-lookup"><span data-stu-id="bc56f-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="bc56f-241">**右側將顯示"環境**"面板。</span><span class="sxs-lookup"><span data-stu-id="bc56f-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="bc56f-242">將**環境名稱**文字框中*的環境 1*文字更改為 *「生產*」。</span><span class="sxs-lookup"><span data-stu-id="bc56f-242">Change the *Environment 1* text in the **Environment name** textbox to *Production*.</span></span>

   ![執行導管 - 環境名稱文字框](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="bc56f-244">點選 **「生產**」 框中**1 階段、2 個工作**連結:</span><span class="sxs-lookup"><span data-stu-id="bc56f-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![啟動導管 - 生產環境連結.png](media/cicd/vsts-production-link.png)

    <span data-ttu-id="bc56f-246">將顯示環境的 **「任務**」選項卡。</span><span class="sxs-lookup"><span data-stu-id="bc56f-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="bc56f-247">按下「**將 Azure 應用服務部署到槽**」任務。</span><span class="sxs-lookup"><span data-stu-id="bc56f-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="bc56f-248">其設置顯示在右側的面板中。</span><span class="sxs-lookup"><span data-stu-id="bc56f-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="bc56f-249">從**Azure 訂閱**下拉清單中選擇與應用服務關聯的 Azure 訂閱。</span><span class="sxs-lookup"><span data-stu-id="bc56f-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="bc56f-250">選中後,按下 **「授權」** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="bc56f-251">從**套用型態**下拉清單中選擇*Web 應用*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="bc56f-252">從**應用服務名稱**下拉清單中選擇*mywebapp/<unique_number/>。*</span><span class="sxs-lookup"><span data-stu-id="bc56f-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="bc56f-253">從**資源群組**下拉清單中選擇*Azure 教程*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="bc56f-254">從 **「插槽**」 下拉清單中選擇*暫存*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="bc56f-255">按一下 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="bc56f-256">將滑鼠懸停在預設發佈管道名稱上。</span><span class="sxs-lookup"><span data-stu-id="bc56f-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="bc56f-257">按一下鉛筆圖示進行編輯。</span><span class="sxs-lookup"><span data-stu-id="bc56f-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="bc56f-258">使用*MyFirstProject-ASP.NET核心 CD*作為名稱。</span><span class="sxs-lookup"><span data-stu-id="bc56f-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![釋放導管名稱](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="bc56f-260">按一下 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc56f-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="bc56f-261">將變更認可至 GitHub 並自動部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="bc56f-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="bc56f-262">在視覺工作室打開*簡單閱讀器.sln。*</span><span class="sxs-lookup"><span data-stu-id="bc56f-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="bc56f-263">在解決方案資源管理員中,開啟*頁面\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-263">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="bc56f-264">將 `<h2>Simple Feed Reader - V3</h2>` 變更為 `<h2>Simple Feed Reader - V4</h2>`。</span><span class="sxs-lookup"><span data-stu-id="bc56f-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="bc56f-265">按**Ctrl**+**移位**+**B**可生成應用程式。</span><span class="sxs-lookup"><span data-stu-id="bc56f-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="bc56f-266">將檔提交到 GitHub 儲存庫。</span><span class="sxs-lookup"><span data-stu-id="bc56f-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="bc56f-267">使用 Visual Studio*的團隊資源管理員*選項卡中的 **「更改」** 頁,或使用本地電腦的命令外殼執行以下內容:</span><span class="sxs-lookup"><span data-stu-id="bc56f-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. <span data-ttu-id="bc56f-268">將*主*分支的更改推送到 GitHub 儲存函式庫的原*點*遠端:</span><span class="sxs-lookup"><span data-stu-id="bc56f-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="bc56f-269">提交將顯示在 GitHub*儲存函式庫的主分支*中:</span><span class="sxs-lookup"><span data-stu-id="bc56f-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![GitHub 提交主分支](media/cicd/github-commit.png)

    <span data-ttu-id="bc56f-271">產生將觸發,因為在生成定義的**觸發器**選項卡中啟用了連續整合:</span><span class="sxs-lookup"><span data-stu-id="bc56f-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![實現持續整合](media/cicd/enable-ci.png)

1. <span data-ttu-id="bc56f-273">導覽 Azure DevOps 服務中的**Azure 管道** > **產生**頁的 **「已排隊」** 選項卡。</span><span class="sxs-lookup"><span data-stu-id="bc56f-273">Navigate to the **Queued** tab of the **Azure Pipelines** > **Builds** page in Azure DevOps Services.</span></span> <span data-ttu-id="bc56f-274">重新產生的佇列的顯示觸發成的分支與提交:</span><span class="sxs-lookup"><span data-stu-id="bc56f-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![佇列產生](media/cicd/build-queued.png)

1. <span data-ttu-id="bc56f-276">生成成功後,將部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="bc56f-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="bc56f-277">在瀏覽器中導航到應用。</span><span class="sxs-lookup"><span data-stu-id="bc56f-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="bc56f-278">請注意,「V4」文字顯示在標題中:</span><span class="sxs-lookup"><span data-stu-id="bc56f-278">Notice that the "V4" text appears in the heading:</span></span>

    ![更新的應用程式](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a><span data-ttu-id="bc56f-280">檢查 Azure 管道導管</span><span class="sxs-lookup"><span data-stu-id="bc56f-280">Examine the Azure Pipelines pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="bc56f-281">組建定義</span><span class="sxs-lookup"><span data-stu-id="bc56f-281">Build definition</span></span>

<span data-ttu-id="bc56f-282">使用*核心 CI MyFirstProject-ASP.NET*創建了生成定義。</span><span class="sxs-lookup"><span data-stu-id="bc56f-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*.</span></span> <span data-ttu-id="bc56f-283">完成後,生成將生成一個 *.zip*檔,包括要發佈的資產。</span><span class="sxs-lookup"><span data-stu-id="bc56f-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="bc56f-284">發佈管道將這些資產部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="bc56f-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="bc56f-285">生成定義**任務選項**卡列出了正在使用的各個步驟。</span><span class="sxs-lookup"><span data-stu-id="bc56f-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="bc56f-286">有五個生成任務。</span><span class="sxs-lookup"><span data-stu-id="bc56f-286">There are five build tasks.</span></span>

![組建定義工作](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="bc56f-288">**Restore**還原`dotnet restore`執行命令以還原應用的 NuGet&mdash;包。</span><span class="sxs-lookup"><span data-stu-id="bc56f-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="bc56f-289">使用的預設包饋送為nuget.org。</span><span class="sxs-lookup"><span data-stu-id="bc56f-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="bc56f-290">**生成**&mdash;執行`dotnet build --configuration release`命令以編譯應用的代碼。</span><span class="sxs-lookup"><span data-stu-id="bc56f-290">**Build** &mdash; Executes the `dotnet build --configuration release` command to compile the app's code.</span></span> <span data-ttu-id="bc56f-291">此選項`--configuration`用於生成代碼的優化版本,該版本適用於部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="bc56f-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="bc56f-292">如果需要調試*配置*,則修改生成定義"**變數"** 選項卡上的 Build 配置變數。</span><span class="sxs-lookup"><span data-stu-id="bc56f-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="bc56f-293">**測試**&mdash;執行`dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>`命令 以運行應用的單元測試。</span><span class="sxs-lookup"><span data-stu-id="bc56f-293">**Test** &mdash; Executes the `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` command to run the app's unit tests.</span></span> <span data-ttu-id="bc56f-294">單元測試在匹配`**/*Tests/*.csproj`球形模式的任何 C# 專案中執行。</span><span class="sxs-lookup"><span data-stu-id="bc56f-294">Unit tests are executed within any C# project matching the `**/*Tests/*.csproj` glob pattern.</span></span> <span data-ttu-id="bc56f-295">測試結果儲存在`--results-directory`選項指定的位置的 *.trx*檔案中。</span><span class="sxs-lookup"><span data-stu-id="bc56f-295">Test results are saved in a *.trx* file at the location specified by the `--results-directory` option.</span></span> <span data-ttu-id="bc56f-296">如果任何測試失敗,則生成將失敗,並且未部署。</span><span class="sxs-lookup"><span data-stu-id="bc56f-296">If any tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bc56f-297">要驗證單元測試的工作,請修改*SimpleFeedReader.Test_Services_NewsServiceTest.cs*以故意中斷其中一個測試。</span><span class="sxs-lookup"><span data-stu-id="bc56f-297">To verify the unit tests work, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests.</span></span> <span data-ttu-id="bc56f-298">例如,在`Assert.True(result.Count > 0);`方法`Assert.False(result.Count > 0);`中更改為`Returns_News_Stories_Given_Valid_Uri`。</span><span class="sxs-lookup"><span data-stu-id="bc56f-298">For example, change `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri` method.</span></span> <span data-ttu-id="bc56f-299">提交更改並將更改推送到 GitHub。</span><span class="sxs-lookup"><span data-stu-id="bc56f-299">Commit and push the change to GitHub.</span></span> <span data-ttu-id="bc56f-300">生成將觸發並失敗。</span><span class="sxs-lookup"><span data-stu-id="bc56f-300">The build is triggered and fails.</span></span> <span data-ttu-id="bc56f-301">產生導管狀態變更為**失敗**。</span><span class="sxs-lookup"><span data-stu-id="bc56f-301">The build pipeline status changes to **failed**.</span></span> <span data-ttu-id="bc56f-302">還原更改、提交並再次推送。</span><span class="sxs-lookup"><span data-stu-id="bc56f-302">Revert the change, commit, and push again.</span></span> <span data-ttu-id="bc56f-303">生成成功。</span><span class="sxs-lookup"><span data-stu-id="bc56f-303">The build succeeds.</span></span>

1. <span data-ttu-id="bc56f-304">**執行**&mdash;執行指令`dotnet publish --configuration release --output <local_path_on_build_agent>`以產生包含要部署的專案的 *.zip*檔。</span><span class="sxs-lookup"><span data-stu-id="bc56f-304">**Publish** &mdash; Executes the `dotnet publish --configuration release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="bc56f-305">這個`--output`選項指定 *.zip*檔的發佈位置。</span><span class="sxs-lookup"><span data-stu-id="bc56f-305">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="bc56f-306">該位置是通過傳遞名為[的預定義變數](/azure/devops/pipelines/build/variables)來`$(build.artifactstagingdirectory)`指定的。</span><span class="sxs-lookup"><span data-stu-id="bc56f-306">That location is specified by passing a [predefined variable](/azure/devops/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="bc56f-307">該變數延伸到生成代理上的本地路徑,例如*c:\代理\_工作\1\a。*</span><span class="sxs-lookup"><span data-stu-id="bc56f-307">That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.</span></span>
1. <span data-ttu-id="bc56f-308">**發佈項目**&mdash;發佈**由發佈**任務生成的 *.zip*檔。</span><span class="sxs-lookup"><span data-stu-id="bc56f-308">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="bc56f-309">工作接受 *.zip*檔案位置作為參數,這是預先定義`$(build.artifactstagingdirectory)`的變數 。</span><span class="sxs-lookup"><span data-stu-id="bc56f-309">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="bc56f-310">*.zip*檔案作為名為*drop*的資料夾發佈。</span><span class="sxs-lookup"><span data-stu-id="bc56f-310">The *.zip* file is published as a folder named *drop*.</span></span>

<span data-ttu-id="bc56f-311">點選產生定義的 **「摘要」** 連結,檢視具有定義的產生歷史紀錄:</span><span class="sxs-lookup"><span data-stu-id="bc56f-311">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![顯示產生定義歷史記錄的螢幕擷取](media/cicd/build-definition-summary.png)

<span data-ttu-id="bc56f-313">在產生的頁面上,按下與唯一內部編號對應的連結:</span><span class="sxs-lookup"><span data-stu-id="bc56f-313">On the resulting page, click the link corresponding to the unique build number:</span></span>

![顯示產生定義摘要頁的螢幕擷取](media/cicd/build-definition-completed.png)

<span data-ttu-id="bc56f-315">將顯示此特定生成摘要。</span><span class="sxs-lookup"><span data-stu-id="bc56f-315">A summary of this specific build is displayed.</span></span> <span data-ttu-id="bc56f-316">按下「**項目」** 選項卡,並注意到列出生成產生的*放置*資料夾:</span><span class="sxs-lookup"><span data-stu-id="bc56f-316">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![顯示產生定義的螢幕截圖 - 放置資料夾](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="bc56f-318">使用 **「下載**和**瀏覽」** 連結檢查已發布的專案。</span><span class="sxs-lookup"><span data-stu-id="bc56f-318">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="bc56f-319">發行管線</span><span class="sxs-lookup"><span data-stu-id="bc56f-319">Release pipeline</span></span>

<span data-ttu-id="bc56f-320">建立一個發佈導管,名稱*MyFirstProject-ASP.NET核心 CD*:</span><span class="sxs-lookup"><span data-stu-id="bc56f-320">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:</span></span>

![顯示宣告導管概述的螢幕擷圖](media/cicd/release-definition-overview.png)

<span data-ttu-id="bc56f-322">執行導管的兩個主要元件是**工件\*\*\*\*與環境**。</span><span class="sxs-lookup"><span data-stu-id="bc56f-322">The two major components of the release pipeline are the **Artifacts** and the **Environments**.</span></span> <span data-ttu-id="bc56f-323">按下 **「工件」** 部分中的框將顯示以下面板:</span><span class="sxs-lookup"><span data-stu-id="bc56f-323">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![顯示釋放導管的螢幕擷圖](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="bc56f-325">**源(生成定義)** 值表示此發佈管道連結到的生成定義。</span><span class="sxs-lookup"><span data-stu-id="bc56f-325">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="bc56f-326">成功執行生成定義生成的 *.zip*檔案將提供給*生產*環境以部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="bc56f-326">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="bc56f-327">按下 *「生產*環境」 方塊中 *, 2 個工作*連結以檢視發佈導管工作:</span><span class="sxs-lookup"><span data-stu-id="bc56f-327">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![顯示宣告導管工作的螢幕擷取](media/cicd/release-definition-tasks.png)

<span data-ttu-id="bc56f-329">執行導管由兩個工作組成:*將 Azure 應用服務部署到插槽\*\*和管理 Azure 應用服務 - 插槽交換*。</span><span class="sxs-lookup"><span data-stu-id="bc56f-329">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*.</span></span> <span data-ttu-id="bc56f-330">按一個工作會顯示以下任務設定:</span><span class="sxs-lookup"><span data-stu-id="bc56f-330">Clicking the first task reveals the following task configuration:</span></span>

![顯示宣告導管部署工作的螢幕擷取](media/cicd/release-definition-task1.png)

<span data-ttu-id="bc56f-332">Azure 訂閱、服務類型、Web 應用名稱、資源組和部署槽在部署任務中定義。</span><span class="sxs-lookup"><span data-stu-id="bc56f-332">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="bc56f-333">**包或資料夾**文字框保存要提取和部署到*\<\> mywebapp unique_number Web*應用*的暫存*槽的 *.zip*檔路徑。</span><span class="sxs-lookup"><span data-stu-id="bc56f-333">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="bc56f-334">點選插槽交換工作會顯示以下任務設定:</span><span class="sxs-lookup"><span data-stu-id="bc56f-334">Clicking the slot swap task reveals the following task configuration:</span></span>

![顯示宣告導管插槽交換工作的螢幕截圖](media/cicd/release-definition-task2.png)

<span data-ttu-id="bc56f-336">提供了訂閱、資源組、服務類型、Web 應用名稱和部署槽詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="bc56f-336">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="bc56f-337">選中"**與生產交換**「複選框。</span><span class="sxs-lookup"><span data-stu-id="bc56f-337">The **Swap with Production** check box is checked.</span></span> <span data-ttu-id="bc56f-338">因此,部署到*暫存*槽的位將交換到生產環境中。</span><span class="sxs-lookup"><span data-stu-id="bc56f-338">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="bc56f-339">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="bc56f-339">Additional reading</span></span>

* [<span data-ttu-id="bc56f-340">使用 Azure Pipelines 建立您的第一個管線</span><span class="sxs-lookup"><span data-stu-id="bc56f-340">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)
* [<span data-ttu-id="bc56f-341">產生與 .NET 核心項目</span><span class="sxs-lookup"><span data-stu-id="bc56f-341">Build and .NET Core project</span></span>](/azure/devops/pipelines/languages/dotnet-core)
* [<span data-ttu-id="bc56f-342">使用 Azure 管道部署 Web 應用</span><span class="sxs-lookup"><span data-stu-id="bc56f-342">Deploy a web app with Azure Pipelines</span></span>](/azure/devops/pipelines/targets/webapp)
