---
title: 將應用程式部署至 App Service-使用 ASP.NET Core 和 Azure 的 DevOps
author: CamSoper
description: 將 ASP.NET Core 應用程式部署至 Azure App Service，使用 ASP.NET Core 和 Azure 的 DevOps 的第一個步驟。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: e09d03f1d30f128b1db1588aa92b28ec3e4ae626
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892635"
---
# <a name="deploy-an-app-to-app-service"></a>將應用程式部署至 App Service

[Azure App Service](/azure/app-service/)是 Azure 的 web 裝載平台。 手動或自動化程序，可以完成將 web 應用程式部署至 Azure App Service。 本指南的本節將討論以手動方式或使用命令列中，指令碼可以觸發，或以手動方式使用 Visual Studio 觸發的部署方法。

在本節中，您將完成下列工作：

* 下載並建置範例應用程式。
* 建立 Azure App Service Web 應用程式使用 Azure Cloud Shell。
* 將範例應用程式部署至 Azure 中使用 Git 中。
* 將變更部署到使用 Visual Studio 應用程式。
* 將預備位置加入至 web 應用程式。
* 將更新部署至預備位置。
* 交換預備與生產位置。

## <a name="download-and-test-the-app"></a>下載並測試應用程式

本指南中使用應用程式已預先建置的 ASP.NET Core 應用程式中，[簡單的摘要讀取器](https://github.com/Azure-Samples/simple-feed-reader/)。 它是使用 Razor 頁面應用程式`Microsoft.SyndicationFeed.ReaderWriter`API 來擷取 RSS/Atom 摘要，並顯示在清單中的新聞項目。

歡迎您檢閱的程式碼，但請務必了解，沒有特別關於此應用程式。 這是說明之用，只是簡單的 ASP.NET Core 應用程式。

從命令殼層中，下載的程式碼、 建置專案，並執行它，如下所示。

> *注意：Linux/macOS 使用者應變更適當的路徑，例如，使用正斜線 (`/`) 而不是反斜線 (`\`)。*

1. 複製程式碼到您的本機電腦上的資料夾。

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. 變更您的工作資料夾*簡單-摘要讀取器*所建立的資料夾。

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. 還原套件，然後建置方案。

    ```console
    dotnet build
    ```

4. 執行應用程式。

    ```console
    dotnet run
    ```

    ![Dotnet run 命令已成功](./media/deploying-to-app-service/dotnet-run.png)

5. 開啟瀏覽器並瀏覽至`http://localhost:5000`。 應用程式可讓您輸入或貼上的新聞訂閱摘要的 URL，並檢視新聞項目清單。

     ![顯示內容的 RSS 摘要的應用程式](./media/deploying-to-app-service/app-in-browser.png)

6. 一旦您認為應用程式是否正常運作，藉由按下關機**Ctrl**+**C**命令殼層中。

## <a name="create-the-azure-app-service-web-app"></a>建立 Azure App Service Web 應用程式

若要部署應用程式，您必須建立 App Service [Web 應用程式](/azure/app-service/app-service-web-overview)。 在建立之後的 Web 應用程式，您將從本機電腦使用 Git 部署至它。

1. 登入[Azure Cloud Shell](https://shell.azure.com/bash)。 注意:當您第一次登入時，Cloud Shell 會提示您建立組態檔的儲存體帳戶。 接受預設值，或提供唯一的名稱。

2. 使用 Cloud Shell 中的下列步驟。

    a. 宣告一個變數來儲存您的 web 應用程式的名稱。 名稱必須是唯一可用於預設的 URL。 使用`$RANDOM`Bash 函式來建構名稱保證唯一性，並導致格式`webappname99999`。

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. 建立資源群組。 資源群組提供方法來彙總當成群組來管理 Azure 資源。

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    `az`命令會叫用[Azure CLI](/cli/azure/)。 可以在本機執行 CLI，但在 Cloud Shell 中使用它可以節省時間與組態。

    c.  在 S1 層中建立 App Service 方案。 App Service 方案是共用相同的定價層的 web 應用程式的群組。 S1 層不是免費的但它具有所需的暫存位置功能。

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. 建立使用 App Service 方案相同的資源群組中的 web 應用程式資源。

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. 設定部署認證。 這些部署認證套用至您的訂用帳戶中的所有 web 應用程式。 在 使用者名稱，不使用特殊字元。

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. 設定 web 應用程式接受從本機 Git 和顯示的部署*Git 部署 URL*。 **請注意此 URL 供稍後參考**。

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. 顯示*web 應用程式 URL*。 瀏覽至此 url，以查看空白 web 應用程式。 **請注意此 URL 供稍後參考**。

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. 在您的本機電腦上使用命令殼層，瀏覽至 web 應用程式的專案資料夾 (例如`.\simple-feed-reader\SimpleFeedReader`)。 執行下列命令來設定 Git 推送至部署的 URL:

    a. 加入本機儲存機制中的遠端 URL。

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. 推播本地*主要*分支*azure 生產*遠端的*主要*分支。

    ```console
    git push azure-prod master
    ```

    系統會提示您稍早建立的部署認證。 請注意命令殼層中的輸出。 Azure 會從遠端建置 ASP.NET Core 應用程式。

4. 在瀏覽器中，瀏覽至*Web 應用程式 URL*並記下已建置和部署應用程式。 其他變更可以認可至本機 Git 儲存機制，與`git commit`。 這些變更推送至 Azure 的前面加上`git push`命令。

## <a name="deployment-with-visual-studio"></a>使用 Visual Studio 部署

> *注意：本節僅適用於 Windows。Linux 和 macOS 使用者應在下面的步驟 2 所述的變更。儲存檔案，並認可變更與本機存放庫`git commit`。最後，將變更推送`git push`，如所示的第一個區段。*

從命令殼層已部署應用程式。 讓我們將更新部署至應用程式中使用 Visual Studio 的整合式的工具。 在幕後，Visual Studio 會完成同樣的工作命令列工具，但在 Visual Studio 的熟悉的 UI。

1. 開啟*SimpleFeedReader.sln* Visual Studio 中。
2. 在 [方案總管] 中，開啟*Pages\Index.cshtml*。 變更`<h2>Simple Feed Reader</h2>`至`<h2>Simple Feed Reader - V2</h2>`。
3. 按下**Ctrl**+**Shift**+**B**建置應用程式。
4. 在 方案總管 中，以滑鼠右鍵按一下專案，然後按一下 **發佈**。

    ![螢幕擷取畫面顯示以滑鼠右鍵按一下，發行](./media/deploying-to-app-service/publish.png)
5. Visual Studio 可以建立新的 App Service 資源，但這項更新將會發行透過現有的部署。 在 **挑選發行目標**對話方塊中，選取**App Service**從左邊的清單，然後選取**選取現有**。 按一下 [發行] 。
6. 在 [ **App Service** ] 對話方塊中，確認 Microsoft 或組織帳戶，用來建立您的 Azure 訂用帳戶會顯示在右上方。 如果不存在，請按一下下拉式清單，並將它新增。
7. 確認已選取正確的 Azure**訂用帳戶**已選取。 針對**檢視**，選取**資源群組**。 依序展開**AzureTutorial**資源群組，然後選取現有的 web 應用程式。 按一下 [確定 **Deploying Office Solutions**]。

    ![螢幕擷取畫面顯示 [發行的 App Service] 對話方塊](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio 會建置並部署至 Azure 的應用程式。 瀏覽至 web 應用程式 URL。 驗證`<h2>`項目修改已上線。

![應用程式與已變更標題](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>部署位置

部署位置支援變更的預備環境，而不會影響生產環境中執行的應用程式。 一旦品質保證小組驗證應用程式的預備的版本時，可以交換生產和預備位置。 在預備環境中的應用程式會升級到生產環境，以這種方式。 下列步驟建立預備位置、 某些變更部署到它，並交換預備與生產環境之後驗證位置。

1. 登入[Azure Cloud Shell](https://shell.azure.com/bash)，如果您尚未登入。
2. 建立預備位置。

    a. 建立具有名稱的部署位置*預備*。

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. 設定要從本機 Git 以及如何取得部署的預備位置**預備**部署 URL。 **請注意此 URL 供稍後參考**。

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c.  顯示預備位置的 URL。 瀏覽至 URL 可以看到空白的預備位置。 **請注意此 URL 供稍後參考**。

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. 在文字編輯器或 Visual Studio 中，修改*pages/Index.cshtml*再次使`<h2>`項目讀取`<h2>Simple Feed Reader - V3</h2>`並儲存檔案。

4. 檔案認可到本機 Git 存放庫，使用**變更**Visual Studio 中的頁面*Team Explorer*  索引標籤，或輸入下列使用本機電腦的命令殼層：

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. 使用本機電腦的命令殼層，將預備部署 URL 新增為 Git 遠端和推送認可的變更：

    a. 將預備環境的遠端 URL 新增至本機 Git 存放庫。

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. 推播本地*主要*分支*azure 預備環境*遠端的*主要*分支。

    ```console
    git push azure-staging master
    ```

    請等候 Azure 建置和部署應用程式。

6. 若要確認 V3，已部署至預備位置，請開啟兩個瀏覽器視窗。 在一個視窗中，瀏覽至原始的 web 應用程式 URL。 在其他視窗中，瀏覽至預備 web 應用程式 URL。 生產環境 URL 是 V2 應用程式。 預備 URL 是 V3 應用程式。

    ![比較瀏覽器視窗的螢幕擷取畫面](./media/deploying-to-app-service/ready-to-swap.png)

7. 在 Cloud Shell 中，驗證/準備總預備位置交換到生產位置。

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. 請確認交換發生的重新整理的兩個瀏覽器視窗。

    ![交換之後比較瀏覽器視窗](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>總結

在本節中，已完成下列工作：

* 下載並建置範例應用程式。
* 建立 Azure App Service Web 應用程式使用 Azure Cloud Shell。
* Azure 中使用 Git 來部署範例應用程式。
* 使用 Visual Studio 應用程式部署的變更。
* 加入 web 應用程式中的預備位置。
* 部署至預備位置的更新。
* 交換預備與生產位置。

在下一步 區段中，您將了解如何建置 DevOps 管線，其中含有 Azure 管線。

## <a name="additional-reading"></a>其他閱讀資料

* [Web Apps 概觀](/azure/app-service/app-service-web-overview)
* [建置.NET Core 和 SQL Database web 應用程式在 Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [設定 Azure App Service 部署認證](/azure/app-service/app-service-deployment-credentials)
* [設定 Azure App Service 中的預備環境](/azure/app-service/web-sites-staged-publishing)
