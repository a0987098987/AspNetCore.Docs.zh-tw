---
title: 將應用程式部署至 App Service-使用 ASP.NET Core 和 Azure 的 DevOps
author: CamSoper
description: 將 ASP.NET Core 應用程式部署至 Azure App Service，使用 ASP.NET Core 和 Azure 的 DevOps 的第一個步驟。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080443"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="5f8e0-103">將應用程式部署至 App Service</span><span class="sxs-lookup"><span data-stu-id="5f8e0-103">Deploy an app to App Service</span></span>

<span data-ttu-id="5f8e0-104">[Azure App Service](/azure/app-service/)是 Azure 的 web 裝載平台。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="5f8e0-105">手動或自動化程序，可以完成將 web 應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="5f8e0-106">本指南的本節將討論以手動方式或使用命令列中，指令碼可以觸發，或以手動方式使用 Visual Studio 觸發的部署方法。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="5f8e0-107">在本節中，您將完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="5f8e0-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="5f8e0-108">下載並建置範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-108">Download and build the sample app.</span></span>
* <span data-ttu-id="5f8e0-109">建立 Azure App Service Web 應用程式使用 Azure Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="5f8e0-110">將範例應用程式部署至 Azure 中使用 Git 中。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="5f8e0-111">將變更部署到使用 Visual Studio 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="5f8e0-112">將預備位置加入至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="5f8e0-113">將更新部署至預備位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="5f8e0-114">交換預備與生產位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="5f8e0-115">下載並測試應用程式</span><span class="sxs-lookup"><span data-stu-id="5f8e0-115">Download and test the app</span></span>

<span data-ttu-id="5f8e0-116">本指南中使用應用程式已預先建置的 ASP.NET Core 應用程式中，[簡單的摘要讀取器](https://github.com/Azure-Samples/simple-feed-reader/)。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="5f8e0-117">它是使用 Razor 頁面應用程式`Microsoft.SyndicationFeed.ReaderWriter`API 來擷取 RSS/Atom 摘要，並顯示在清單中的新聞項目。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="5f8e0-118">歡迎您檢閱的程式碼，但請務必了解，沒有特別關於此應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="5f8e0-119">這是說明之用，只是簡單的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="5f8e0-120">從命令殼層中，下載的程式碼、 建置專案，並執行它，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="5f8e0-121">*注意：Linux/macOS 使用者應該對路徑進行適當的變更，例如，使用正斜線（`/`），而不是反`\`斜杠（）。*</span><span class="sxs-lookup"><span data-stu-id="5f8e0-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="5f8e0-122">複製程式碼到您的本機電腦上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="5f8e0-123">變更您的工作資料夾*簡單-摘要讀取器*所建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="5f8e0-124">還原套件，然後建置方案。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-124">Restore the packages, and build the solution.</span></span>

    ```dotnetcli
    dotnet build
    ```

4. <span data-ttu-id="5f8e0-125">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-125">Run the app.</span></span>

    ```dotnetcli
    dotnet run
    ```

    ![Dotnet run 命令已成功](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="5f8e0-127">開啟瀏覽器並瀏覽至`http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="5f8e0-128">應用程式可讓您輸入或貼上的新聞訂閱摘要的 URL，並檢視新聞項目清單。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![顯示內容的 RSS 摘要的應用程式](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="5f8e0-130">一旦您認為應用程式是否正常運作，藉由按下關機**Ctrl**+**C**命令殼層中。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="5f8e0-131">建立 Azure App Service Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="5f8e0-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="5f8e0-132">若要部署應用程式，您必須建立 App Service [Web 應用程式](/azure/app-service/app-service-web-overview)。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="5f8e0-133">在建立之後的 Web 應用程式，您將從本機電腦使用 Git 部署至它。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="5f8e0-134">登入[Azure Cloud Shell](https://shell.azure.com/bash)。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="5f8e0-135">注意:當您第一次登入時，Cloud Shell 會提示您建立設定檔的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="5f8e0-136">接受預設值，或提供唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="5f8e0-137">使用 Cloud Shell 中的下列步驟。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="5f8e0-138">a.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-138">a.</span></span> <span data-ttu-id="5f8e0-139">宣告一個變數來儲存您的 web 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="5f8e0-140">名稱必須是唯一可用於預設的 URL。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="5f8e0-141">使用`$RANDOM`Bash 函式來建構名稱保證唯一性，並導致格式`webappname99999`。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="5f8e0-142">b.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-142">b.</span></span> <span data-ttu-id="5f8e0-143">建立資源群組。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-143">Create a resource group.</span></span> <span data-ttu-id="5f8e0-144">資源群組提供方法來彙總當成群組來管理 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="5f8e0-145">`az`命令會叫用[Azure CLI](/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="5f8e0-146">可以在本機執行 CLI，但在 Cloud Shell 中使用它可以節省時間與組態。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="5f8e0-147">c.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-147">c.</span></span> <span data-ttu-id="5f8e0-148">在 S1 層中建立 App Service 方案。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="5f8e0-149">App Service 方案是共用相同的定價層的 web 應用程式的群組。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="5f8e0-150">S1 層不是免費的但它具有所需的暫存位置功能。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="5f8e0-151">d.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-151">d.</span></span> <span data-ttu-id="5f8e0-152">建立使用 App Service 方案相同的資源群組中的 web 應用程式資源。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="5f8e0-153">e.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-153">e.</span></span> <span data-ttu-id="5f8e0-154">設定部署認證。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-154">Set the deployment credentials.</span></span> <span data-ttu-id="5f8e0-155">這些部署認證套用至您的訂用帳戶中的所有 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="5f8e0-156">在 使用者名稱，不使用特殊字元。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="5f8e0-157">f.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-157">f.</span></span> <span data-ttu-id="5f8e0-158">設定 web 應用程式接受從本機 Git 和顯示的部署*Git 部署 URL*。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="5f8e0-159">**請注意此 URL 供稍後參考**。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="5f8e0-160">g.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-160">g.</span></span> <span data-ttu-id="5f8e0-161">顯示*web 應用程式 URL*。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-161">Display the *web app URL*.</span></span> <span data-ttu-id="5f8e0-162">瀏覽至此 url，以查看空白 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="5f8e0-163">**請注意此 URL 供稍後參考**。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="5f8e0-164">在您的本機電腦上使用命令殼層，瀏覽至 web 應用程式的專案資料夾 (例如`.\simple-feed-reader\SimpleFeedReader`)。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="5f8e0-165">執行下列命令來設定 Git 推送至部署的 URL:</span><span class="sxs-lookup"><span data-stu-id="5f8e0-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="5f8e0-166">a.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-166">a.</span></span> <span data-ttu-id="5f8e0-167">加入本機儲存機制中的遠端 URL。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="5f8e0-168">b.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-168">b.</span></span> <span data-ttu-id="5f8e0-169">推播本地*主要*分支*azure 生產*遠端的*主要*分支。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="5f8e0-170">系統會提示您稍早建立的部署認證。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="5f8e0-171">請注意命令殼層中的輸出。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-171">Observe the output in the command shell.</span></span> <span data-ttu-id="5f8e0-172">Azure 會從遠端建置 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="5f8e0-173">在瀏覽器中，瀏覽至*Web 應用程式 URL*並記下已建置和部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="5f8e0-174">其他變更可以認可至本機 Git 儲存機制，與`git commit`。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="5f8e0-175">這些變更推送至 Azure 的前面加上`git push`命令。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="5f8e0-176">使用 Visual Studio 部署</span><span class="sxs-lookup"><span data-stu-id="5f8e0-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="5f8e0-177">*注意：本節僅適用于 Windows。Linux 和 macOS 使用者應在下面的步驟 2 所述的變更。儲存檔案，並認可變更與本機存放庫`git commit`。最後，將變更推送`git push`，如所示的第一個區段。*</span><span class="sxs-lookup"><span data-stu-id="5f8e0-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="5f8e0-178">從命令殼層已部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="5f8e0-179">讓我們將更新部署至應用程式中使用 Visual Studio 的整合式的工具。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="5f8e0-180">在幕後，Visual Studio 會完成同樣的工作命令列工具，但在 Visual Studio 的熟悉的 UI。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="5f8e0-181">開啟*SimpleFeedReader.sln* Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="5f8e0-182">在 [方案總管] 中，開啟*Pages\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="5f8e0-183">變更`<h2>Simple Feed Reader</h2>`至`<h2>Simple Feed Reader - V2</h2>`。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="5f8e0-184">按下**Ctrl**+**Shift**+**B**建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="5f8e0-185">在 方案總管 中，以滑鼠右鍵按一下專案，然後按一下 **發佈**。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![螢幕擷取畫面顯示以滑鼠右鍵按一下，發行](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="5f8e0-187">Visual Studio 可以建立新的 App Service 資源，但這項更新將會發行透過現有的部署。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="5f8e0-188">在 **挑選發行目標**對話方塊中，選取**App Service**從左邊的清單，然後選取**選取現有**。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="5f8e0-189">按一下 [發行]。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-189">Click **Publish**.</span></span>
6. <span data-ttu-id="5f8e0-190">在 [ **App Service** ] 對話方塊中，確認 Microsoft 或組織帳戶，用來建立您的 Azure 訂用帳戶會顯示在右上方。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="5f8e0-191">如果不存在，請按一下下拉式清單，並將它新增。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="5f8e0-192">確認已選取正確的 Azure**訂用帳戶**已選取。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="5f8e0-193">針對**檢視**，選取**資源群組**。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="5f8e0-194">依序展開**AzureTutorial**資源群組，然後選取現有的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="5f8e0-195">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-195">Click **OK**.</span></span>

    ![螢幕擷取畫面顯示 [發行的 App Service] 對話方塊](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="5f8e0-197">Visual Studio 會建置並部署至 Azure 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="5f8e0-198">瀏覽至 web 應用程式 URL。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-198">Browse to the web app URL.</span></span> <span data-ttu-id="5f8e0-199">驗證`<h2>`項目修改已上線。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-199">Validate that the `<h2>` element modification is live.</span></span>

![應用程式與已變更標題](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="5f8e0-201">部署位置</span><span class="sxs-lookup"><span data-stu-id="5f8e0-201">Deployment slots</span></span>

<span data-ttu-id="5f8e0-202">部署位置支援變更的預備環境，而不會影響生產環境中執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="5f8e0-203">一旦品質保證小組驗證應用程式的預備的版本時，可以交換生產和預備位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="5f8e0-204">在預備環境中的應用程式會升級到生產環境，以這種方式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="5f8e0-205">下列步驟建立預備位置、 某些變更部署到它，並交換預備與生產環境之後驗證位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="5f8e0-206">登入[Azure Cloud Shell](https://shell.azure.com/bash)，如果您尚未登入。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="5f8e0-207">建立預備位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-207">Create the staging slot.</span></span>

    <span data-ttu-id="5f8e0-208">a.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-208">a.</span></span> <span data-ttu-id="5f8e0-209">建立具有名稱的部署位置*預備*。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="5f8e0-210">b.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-210">b.</span></span> <span data-ttu-id="5f8e0-211">設定要從本機 Git 以及如何取得部署的預備位置**預備**部署 URL。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="5f8e0-212">**請注意此 URL 供稍後參考**。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="5f8e0-213">c.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-213">c.</span></span> <span data-ttu-id="5f8e0-214">顯示預備位置的 URL。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-214">Display the staging slot's URL.</span></span> <span data-ttu-id="5f8e0-215">瀏覽至 URL 可以看到空白的預備位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="5f8e0-216">**請注意此 URL 供稍後參考**。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="5f8e0-217">在文字編輯器或 Visual Studio 中，修改*pages/Index.cshtml*再次使`<h2>`項目讀取`<h2>Simple Feed Reader - V3</h2>`並儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="5f8e0-218">檔案認可到本機 Git 存放庫，使用**變更**Visual Studio 中的頁面*Team Explorer*  索引標籤，或輸入下列使用本機電腦的命令殼層：</span><span class="sxs-lookup"><span data-stu-id="5f8e0-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. <span data-ttu-id="5f8e0-219">使用本機電腦的命令殼層，將預備部署 URL 新增為 Git 遠端和推送認可的變更：</span><span class="sxs-lookup"><span data-stu-id="5f8e0-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="5f8e0-220">a.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-220">a.</span></span> <span data-ttu-id="5f8e0-221">將預備環境的遠端 URL 新增至本機 Git 存放庫。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="5f8e0-222">b.</span><span class="sxs-lookup"><span data-stu-id="5f8e0-222">b.</span></span> <span data-ttu-id="5f8e0-223">推播本地*主要*分支*azure 預備環境*遠端的*主要*分支。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="5f8e0-224">請等候 Azure 建置和部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="5f8e0-225">若要確認 V3，已部署至預備位置，請開啟兩個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="5f8e0-226">在一個視窗中，瀏覽至原始的 web 應用程式 URL。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="5f8e0-227">在其他視窗中，瀏覽至預備 web 應用程式 URL。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="5f8e0-228">生產環境 URL 是 V2 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="5f8e0-229">預備 URL 是 V3 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-229">The staging URL serves V3 of the app.</span></span>

    ![比較瀏覽器視窗的螢幕擷取畫面](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="5f8e0-231">在 Cloud Shell 中，驗證/準備總預備位置交換到生產位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="5f8e0-232">請確認交換發生的重新整理的兩個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![交換之後比較瀏覽器視窗](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="5f8e0-234">總結</span><span class="sxs-lookup"><span data-stu-id="5f8e0-234">Summary</span></span>

<span data-ttu-id="5f8e0-235">在本節中，已完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="5f8e0-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="5f8e0-236">下載並建置範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="5f8e0-237">建立 Azure App Service Web 應用程式使用 Azure Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="5f8e0-238">Azure 中使用 Git 來部署範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="5f8e0-239">使用 Visual Studio 應用程式部署的變更。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="5f8e0-240">加入 web 應用程式中的預備位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="5f8e0-241">部署至預備位置的更新。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="5f8e0-242">交換預備與生產位置。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="5f8e0-243">在下一步 區段中，您將了解如何建置 DevOps 管線，其中含有 Azure 管線。</span><span class="sxs-lookup"><span data-stu-id="5f8e0-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="5f8e0-244">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="5f8e0-244">Additional reading</span></span>

* [<span data-ttu-id="5f8e0-245">Web Apps 概觀</span><span class="sxs-lookup"><span data-stu-id="5f8e0-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="5f8e0-246">建置.NET Core 和 SQL Database web 應用程式在 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5f8e0-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="5f8e0-247">設定 Azure App Service 部署認證</span><span class="sxs-lookup"><span data-stu-id="5f8e0-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="5f8e0-248">設定 Azure App Service 中的預備環境</span><span class="sxs-lookup"><span data-stu-id="5f8e0-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
