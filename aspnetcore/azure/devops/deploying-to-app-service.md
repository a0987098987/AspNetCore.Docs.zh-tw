---
title: 套用應用程式到應用程式服務 - 使用 ASP.NET 核心和 Azure 進行 DevOps
author: CamSoper
description: 將ASP.NET核心應用部署到Azure應用服務,這是使用ASP.NET核心和Azure的DevOps的第一步。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78657742"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="7add7-103">套用應用到應用服務</span><span class="sxs-lookup"><span data-stu-id="7add7-103">Deploy an app to App Service</span></span>

<span data-ttu-id="7add7-104">[Azure 應用服務](/azure/app-service/)是 Azure 的 Web 託管平臺。</span><span class="sxs-lookup"><span data-stu-id="7add7-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="7add7-105">將 Web 應用部署到 Azure 應用服務可以手動完成,也可以由自動化過程完成。</span><span class="sxs-lookup"><span data-stu-id="7add7-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="7add7-106">本指南的這一部分討論了可以使用命令行手動或腳本觸發的部署方法,或者使用 Visual Studio 手動觸發。</span><span class="sxs-lookup"><span data-stu-id="7add7-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="7add7-107">在本節中,您將完成以下任務:</span><span class="sxs-lookup"><span data-stu-id="7add7-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="7add7-108">下載並生成示例應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-108">Download and build the sample app.</span></span>
* <span data-ttu-id="7add7-109">使用 Azure 雲外殼創建 Azure 應用服務 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="7add7-110">使用 Git 將示例應用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="7add7-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="7add7-111">使用 Visual Studio 將更改部署到應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="7add7-112">向 Web 應用添加暫存槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="7add7-113">將更新部署到暫存槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="7add7-114">交換預備與生產位置。</span><span class="sxs-lookup"><span data-stu-id="7add7-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="7add7-115">下載並測試應用程式</span><span class="sxs-lookup"><span data-stu-id="7add7-115">Download and test the app</span></span>

<span data-ttu-id="7add7-116">這個指南中使用的應用程式是預先建構 ASP.NET 核心應用程式,[簡單的飼料閱讀器](https://github.com/Azure-Samples/simple-feed-reader/)。</span><span class="sxs-lookup"><span data-stu-id="7add7-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="7add7-117">它是 Razor Pages 應用`Microsoft.SyndicationFeed.ReaderWriter`, 它使用 API 檢索 RSS/Atom 源並在清單中顯示新聞專案。</span><span class="sxs-lookup"><span data-stu-id="7add7-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="7add7-118">隨意查看代碼,但請務必瞭解此應用程式沒有什麼特別之處。</span><span class="sxs-lookup"><span data-stu-id="7add7-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="7add7-119">它只是一個簡單的ASP.NET核心應用程式,用於說明目的。</span><span class="sxs-lookup"><span data-stu-id="7add7-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="7add7-120">從命令外殼下載代碼、生成專案並按如下方式運行它。</span><span class="sxs-lookup"><span data-stu-id="7add7-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="7add7-121">*注意:Linux/macOS 用戶應該對路徑進行適當的更改,例如,使用正向斜杠`/`() 而不是`\`反斜杠 ()。*</span><span class="sxs-lookup"><span data-stu-id="7add7-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="7add7-122">將代碼複製到本地電腦上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7add7-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="7add7-123">將工作資料夾更改為已創建*的簡單源閱讀器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7add7-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="7add7-124">還原包,並生成解決方案。</span><span class="sxs-lookup"><span data-stu-id="7add7-124">Restore the packages, and build the solution.</span></span>

    ```dotnetcli
    dotnet build
    ```

4. <span data-ttu-id="7add7-125">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7add7-125">Run the app.</span></span>

    ```dotnetcli
    dotnet run
    ```

    ![點網執行命令成功](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="7add7-127">開啟瀏覽器並瀏覽至 `http://localhost:5000` 。</span><span class="sxs-lookup"><span data-stu-id="7add7-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="7add7-128">該應用程式允許您鍵入或貼上聯合源網址並查看新聞專案清單。</span><span class="sxs-lookup"><span data-stu-id="7add7-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![顯示 RSS 原始內容的應用程式](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="7add7-130">一旦您滿意應用程式工作正常,通過按命令 shell 中的**Ctrl**+**C**將其關閉。</span><span class="sxs-lookup"><span data-stu-id="7add7-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="7add7-131">建立 Azure 應用服務 Web 應用</span><span class="sxs-lookup"><span data-stu-id="7add7-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="7add7-132">要部署應用程式,您需要建立套用服務[Web 應用](/azure/app-service/app-service-web-overview)。</span><span class="sxs-lookup"><span data-stu-id="7add7-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="7add7-133">建立 Web 應用程式後,您將使用 Git 從本地電腦部署到它。</span><span class="sxs-lookup"><span data-stu-id="7add7-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="7add7-134">登入 [Azure Cloud Shell](https://shell.azure.com/bash)。</span><span class="sxs-lookup"><span data-stu-id="7add7-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="7add7-135">注意:首次登錄時,雲外殼會提示為配置檔創建存儲帳戶。</span><span class="sxs-lookup"><span data-stu-id="7add7-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="7add7-136">接受預設值或提供唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="7add7-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="7add7-137">在以下步驟中,請使用雲外殼。</span><span class="sxs-lookup"><span data-stu-id="7add7-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="7add7-138">a.</span><span class="sxs-lookup"><span data-stu-id="7add7-138">a.</span></span> <span data-ttu-id="7add7-139">聲明一個變數來存儲 Web 應用的名稱。</span><span class="sxs-lookup"><span data-stu-id="7add7-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="7add7-140">名稱必須是唯一才能在預設 URL 中使用。</span><span class="sxs-lookup"><span data-stu-id="7add7-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="7add7-141">使用`$RANDOM`Bash 函數建構名稱可確保唯一`webappname99999`性和格式 的結果。</span><span class="sxs-lookup"><span data-stu-id="7add7-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="7add7-142">b.</span><span class="sxs-lookup"><span data-stu-id="7add7-142">b.</span></span> <span data-ttu-id="7add7-143">建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="7add7-143">Create a resource group.</span></span> <span data-ttu-id="7add7-144">資源組提供了一種聚合要作為組管理的 Azure 資源的方法。</span><span class="sxs-lookup"><span data-stu-id="7add7-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="7add7-145">這個`az`指令呼叫[Azure CLI](/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="7add7-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="7add7-146">CLI 可以在本地運行,但在雲外殼中使用它可以節省時間和配置。</span><span class="sxs-lookup"><span data-stu-id="7add7-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="7add7-147">c.</span><span class="sxs-lookup"><span data-stu-id="7add7-147">c.</span></span> <span data-ttu-id="7add7-148">在 S1 層中創建應用服務計畫。</span><span class="sxs-lookup"><span data-stu-id="7add7-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="7add7-149">應用服務計劃是共用相同定價層的 Web 應用的分組。</span><span class="sxs-lookup"><span data-stu-id="7add7-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="7add7-150">S1 層不是免費的,但它是暫存槽功能所必需的。</span><span class="sxs-lookup"><span data-stu-id="7add7-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="7add7-151">d.</span><span class="sxs-lookup"><span data-stu-id="7add7-151">d.</span></span> <span data-ttu-id="7add7-152">使用同一資源組中的應用服務計劃創建 Web 應用資源。</span><span class="sxs-lookup"><span data-stu-id="7add7-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="7add7-153">e.</span><span class="sxs-lookup"><span data-stu-id="7add7-153">e.</span></span> <span data-ttu-id="7add7-154">設置部署憑據。</span><span class="sxs-lookup"><span data-stu-id="7add7-154">Set the deployment credentials.</span></span> <span data-ttu-id="7add7-155">這些部署憑據適用於訂閱中的所有 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="7add7-156">不要在使用者名中使用特殊字元。</span><span class="sxs-lookup"><span data-stu-id="7add7-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="7add7-157">f.</span><span class="sxs-lookup"><span data-stu-id="7add7-157">f.</span></span> <span data-ttu-id="7add7-158">將 Web 應用設定為接受來自本地 Git 部署並顯示*Git 部署 URL。*</span><span class="sxs-lookup"><span data-stu-id="7add7-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="7add7-159">**請注意此網址 回報**。</span><span class="sxs-lookup"><span data-stu-id="7add7-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="7add7-160">g.</span><span class="sxs-lookup"><span data-stu-id="7add7-160">g.</span></span> <span data-ttu-id="7add7-161">顯示*Web 應用程式網址*。</span><span class="sxs-lookup"><span data-stu-id="7add7-161">Display the *web app URL*.</span></span> <span data-ttu-id="7add7-162">瀏覽到此 URL 以檢視空白 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="7add7-163">**請注意此網址 回報**。</span><span class="sxs-lookup"><span data-stu-id="7add7-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="7add7-164">在本地電腦上使用命令外殼,導航到 Web 應用的專案資料夾(`.\simple-feed-reader\SimpleFeedReader`例如。</span><span class="sxs-lookup"><span data-stu-id="7add7-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="7add7-165">執行以下指令以設定 Git 以推送到部署網址:</span><span class="sxs-lookup"><span data-stu-id="7add7-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="7add7-166">a.</span><span class="sxs-lookup"><span data-stu-id="7add7-166">a.</span></span> <span data-ttu-id="7add7-167">將遠端 URL 新增到本地儲存庫。</span><span class="sxs-lookup"><span data-stu-id="7add7-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="7add7-168">b.</span><span class="sxs-lookup"><span data-stu-id="7add7-168">b.</span></span> <span data-ttu-id="7add7-169">將本地*主*分支推送到*azure prod* *遠端的主分支*。</span><span class="sxs-lookup"><span data-stu-id="7add7-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="7add7-170">系統會提示您獲得之前創建的部署憑據。</span><span class="sxs-lookup"><span data-stu-id="7add7-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="7add7-171">觀察命令 shell 中的輸出。</span><span class="sxs-lookup"><span data-stu-id="7add7-171">Observe the output in the command shell.</span></span> <span data-ttu-id="7add7-172">Azure 遠端生成ASP.NET核心應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="7add7-173">在瀏覽器中,導航到*Web 應用 URL*並注意已生成和部署應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="7add7-174">可以使用將其他更改提交到本地 Git`git commit`儲存庫。</span><span class="sxs-lookup"><span data-stu-id="7add7-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="7add7-175">這些更改使用前面的`git push`命令推送到 Azure。</span><span class="sxs-lookup"><span data-stu-id="7add7-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="7add7-176">使用視覺化工作室進行部署</span><span class="sxs-lookup"><span data-stu-id="7add7-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="7add7-177">*注意:此部分僅適用於 Windows。Linux 和macOS用戶應該進行下面的步驟2中描述的更改。保存檔,並將更改提交到本地存儲庫。 `git commit`最後,使用`git push`推送更改,如第一部分。*</span><span class="sxs-lookup"><span data-stu-id="7add7-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="7add7-178">應用已從命令外殼部署。</span><span class="sxs-lookup"><span data-stu-id="7add7-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="7add7-179">讓我們使用 Visual Studio 的整合工具將更新部署到應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="7add7-180">在幕後,Visual Studio 完成與命令列工具相同的任務,但在 Visual Studio 熟悉的 UI 中完成。</span><span class="sxs-lookup"><span data-stu-id="7add7-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="7add7-181">在視覺工作室打開*簡單閱讀器.sln。*</span><span class="sxs-lookup"><span data-stu-id="7add7-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="7add7-182">在解決方案資源管理員中,開啟*頁面\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="7add7-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="7add7-183">將 `<h2>Simple Feed Reader</h2>` 變更為 `<h2>Simple Feed Reader - V2</h2>`。</span><span class="sxs-lookup"><span data-stu-id="7add7-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="7add7-184">按**Ctrl**+**移位**+**B**可生成應用程式。</span><span class="sxs-lookup"><span data-stu-id="7add7-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="7add7-185">在解決方案資源管理器中,右鍵單擊專案並按一下「**發布**」。</span><span class="sxs-lookup"><span data-stu-id="7add7-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![顯示右鍵按一下、發佈功能的螢幕擷取](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="7add7-187">Visual Studio 可以創建新的應用服務資源,但此更新將通過現有部署發佈。</span><span class="sxs-lookup"><span data-stu-id="7add7-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="7add7-188">在「**選擇發佈目標**」對話框中,從左側清單中選擇**應用服務**,然後選擇 **「選擇現有**」。</span><span class="sxs-lookup"><span data-stu-id="7add7-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="7add7-189">按一下 **[發行]**。</span><span class="sxs-lookup"><span data-stu-id="7add7-189">Click **Publish**.</span></span>
6. <span data-ttu-id="7add7-190">在 **「應用服務」** 對話框中,確認用於創建 Azure 訂閱的 Microsoft 或組織帳戶顯示在右上角。</span><span class="sxs-lookup"><span data-stu-id="7add7-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="7add7-191">如果不是,請按下下拉清單並添加它。</span><span class="sxs-lookup"><span data-stu-id="7add7-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="7add7-192">確認選擇了正確的 Azure**訂閱**。</span><span class="sxs-lookup"><span data-stu-id="7add7-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="7add7-193">對於**檢視**,請選擇 **「資源群組**」 。。</span><span class="sxs-lookup"><span data-stu-id="7add7-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="7add7-194">展開**Azure 教程**資源組,然後選擇現有的 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="7add7-195">按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="7add7-195">Click **OK**.</span></span>

    ![顯示發佈應用服務對話框的螢幕截圖](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="7add7-197">可視化工作室生成應用並將其部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="7add7-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="7add7-198">瀏覽到 Web 應用 URL。</span><span class="sxs-lookup"><span data-stu-id="7add7-198">Browse to the web app URL.</span></span> <span data-ttu-id="7add7-199">驗證`<h2>`元素修改是否為即時。</span><span class="sxs-lookup"><span data-stu-id="7add7-199">Validate that the `<h2>` element modification is live.</span></span>

![具有已變更標題的應用程式](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="7add7-201">部署位置</span><span class="sxs-lookup"><span data-stu-id="7add7-201">Deployment slots</span></span>

<span data-ttu-id="7add7-202">部署槽支援暫存更改,而不會影響在生產中運行的應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="7add7-203">一旦品質保證團隊驗證了應用的暫存版本,就可以交換生產和暫存槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="7add7-204">以這種方式將暫存中的應用程式提升為生產。</span><span class="sxs-lookup"><span data-stu-id="7add7-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="7add7-205">以下步驟創建暫存槽,部署一些更改,並在驗證后將暫存槽與生產交換。</span><span class="sxs-lookup"><span data-stu-id="7add7-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="7add7-206">登錄到 Azure[雲外殼](https://shell.azure.com/bash)(如果尚未登錄)。</span><span class="sxs-lookup"><span data-stu-id="7add7-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="7add7-207">創建暫存槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-207">Create the staging slot.</span></span>

    <span data-ttu-id="7add7-208">a.</span><span class="sxs-lookup"><span data-stu-id="7add7-208">a.</span></span> <span data-ttu-id="7add7-209">創建名稱*暫存*的部署槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="7add7-210">b.</span><span class="sxs-lookup"><span data-stu-id="7add7-210">b.</span></span> <span data-ttu-id="7add7-211">將暫存槽配置為使用本地 Git 的部署,並獲取**暫存**部署 URL。</span><span class="sxs-lookup"><span data-stu-id="7add7-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="7add7-212">**請注意此網址 回報**。</span><span class="sxs-lookup"><span data-stu-id="7add7-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="7add7-213">c.</span><span class="sxs-lookup"><span data-stu-id="7add7-213">c.</span></span> <span data-ttu-id="7add7-214">顯示暫存槽的 URL。</span><span class="sxs-lookup"><span data-stu-id="7add7-214">Display the staging slot's URL.</span></span> <span data-ttu-id="7add7-215">瀏覽到 URL 以查看空暫存槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="7add7-216">**請注意此網址 回報**。</span><span class="sxs-lookup"><span data-stu-id="7add7-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="7add7-217">在文字編輯器或 Visual Studio 中,再次修改*Pages/Index.cshtml,* 以便`<h2>`元素讀取`<h2>Simple Feed Reader - V3</h2>`並保存檔。</span><span class="sxs-lookup"><span data-stu-id="7add7-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="7add7-218">使用 Visual Studio*的團隊資源管理員*選項卡中的 **「更改」** 頁,或使用本地電腦的命令外殼輸入以下內容,將檔案提交到本地 Git 儲存函式庫:</span><span class="sxs-lookup"><span data-stu-id="7add7-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. <span data-ttu-id="7add7-219">使用本地電腦的命令外殼,將暫存部署網址 加入為 Git 遙控器,然後推送提交的變更:</span><span class="sxs-lookup"><span data-stu-id="7add7-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="7add7-220">a.</span><span class="sxs-lookup"><span data-stu-id="7add7-220">a.</span></span> <span data-ttu-id="7add7-221">將用於暫存的遠端 URL 添加到本地 Git 儲存庫。</span><span class="sxs-lookup"><span data-stu-id="7add7-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="7add7-222">b.</span><span class="sxs-lookup"><span data-stu-id="7add7-222">b.</span></span> <span data-ttu-id="7add7-223">將本地*主*分支推送到*Azure 暫存\*\*遠端的主分支*。</span><span class="sxs-lookup"><span data-stu-id="7add7-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="7add7-224">在 Azure 生成和部署應用時等待。</span><span class="sxs-lookup"><span data-stu-id="7add7-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="7add7-225">要驗證 V3 已部署到暫存槽,請打開兩個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="7add7-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="7add7-226">在一個視窗中,導航到原始 Web 應用 URL。</span><span class="sxs-lookup"><span data-stu-id="7add7-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="7add7-227">在其他視窗中,導航到暫存 Web 應用 URL。</span><span class="sxs-lookup"><span data-stu-id="7add7-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="7add7-228">生產 URL 為應用程式的 V2 提供服務。</span><span class="sxs-lookup"><span data-stu-id="7add7-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="7add7-229">暫存 URL 為應用程式的 V3 提供服務。</span><span class="sxs-lookup"><span data-stu-id="7add7-229">The staging URL serves V3 of the app.</span></span>

    ![比較瀏覽器視窗的螢幕擷取](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="7add7-231">在雲外殼中,將已驗證/預熱的暫存槽交換到生產中。</span><span class="sxs-lookup"><span data-stu-id="7add7-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="7add7-232">通過刷新兩個瀏覽器窗口來驗證交換是否發生。</span><span class="sxs-lookup"><span data-stu-id="7add7-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![比較交換瀏覽器視窗](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="7add7-234">摘要</span><span class="sxs-lookup"><span data-stu-id="7add7-234">Summary</span></span>

<span data-ttu-id="7add7-235">在本節中,已完成以下任務:</span><span class="sxs-lookup"><span data-stu-id="7add7-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="7add7-236">下載並構建了示例應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="7add7-237">使用 Azure 雲外殼創建 Azure 應用服務 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="7add7-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="7add7-238">使用 Git 將示例應用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="7add7-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="7add7-239">使用 Visual Studio 對應用部署了更改。</span><span class="sxs-lookup"><span data-stu-id="7add7-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="7add7-240">向 Web 應用添加了暫存槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="7add7-241">已部署到暫存槽的更新。</span><span class="sxs-lookup"><span data-stu-id="7add7-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="7add7-242">已交換暫存和生產插槽。</span><span class="sxs-lookup"><span data-stu-id="7add7-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="7add7-243">在下一節中,您將學習如何使用 Azure 管道構建 DevOps 管道。</span><span class="sxs-lookup"><span data-stu-id="7add7-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="7add7-244">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="7add7-244">Additional reading</span></span>

* [<span data-ttu-id="7add7-245">Web Apps 概觀</span><span class="sxs-lookup"><span data-stu-id="7add7-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="7add7-246">在 Azure App Service 中建置 .NET Core 和 SQL Database Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7add7-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="7add7-247">設定 Azure App Service 的部署認證</span><span class="sxs-lookup"><span data-stu-id="7add7-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="7add7-248">在 Azure App Service 中設定預備環境</span><span class="sxs-lookup"><span data-stu-id="7add7-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
